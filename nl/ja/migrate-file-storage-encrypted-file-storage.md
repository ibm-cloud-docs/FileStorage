---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.filestorage_short}}から拡張{{site.data.keyword.filestorage_short}}へのマイグレーション

現在、拡張 {{site.data.keyword.filestorage_full}} は一部のデータ・センターでのみ提供されています。 アップグレードされたデータ・センターと、調整可能な IOPS レートや拡張可能なボリュームなどの使用可能な機能のリストを確認するには、[ここ](new-ibm-block-and-file-storage-location-and-features.html)をクリックしてください。 プロバイダー管理の暗号化ストレージについて詳しくは、[保存データの{{site.data.keyword.filestorage_short}}暗号化](block-file-storage-encryption-rest.html)を参照してください。

お勧めするマイグレーション・パスは、両方のボリュームに同時に接続して LUN 間で直接データを転送する方法です。 具体的な手順は、オペレーティング・システムと、コピー操作中にデータ変更が行われるかどうかによって異なります。

ホストに非暗号化 LUN が既に接続されていることを前提としています。 接続されていない場合は、以下の説明のうち、ご使用のオペレーティング・システムに最も該当する説明に従って、このタスクを実行してください。

- [Linux への{{site.data.keyword.filestorage_short}}のマウント](accessing-file-storage-linux.html)
- [CentOS への NFS/{{site.data.keyword.filestorage_short}}のマウント](mounting-nsf-file-storage.html)
- [CoreOS への{{site.data.keyword.filestorage_short}}のマウント](mounting-storage-coreos.html)

すべての拡張{{site.data.keyword.filestorage_short}}・ボリュームは、非暗号化ボリュームとは異なるマウント・ポイントになります。 暗号化と非暗号化の両方の{{site.data.keyword.filestorage_short}}・ボリュームに正しいマウント・ポイントを使用するために、{{site.data.keyword.slportal}} の**「ボリュームの詳細 (Volume Details)」**ページでマウント・ポイント情報を確認することができます。 API 呼び出し `SoftLayer_Network_Storage::getNetworkMountAddress()` を使用して正しいマウント・ポイントを取得することもできます。
{:tip}


## 新しい{{site.data.keyword.filestorage_short}}の作成

API を使用して注文する場合は、「Storage as a Service」パッケージを指定して、更新済みの機能を新規ストレージと一緒に取得してください。
{:important}

{{site.data.keyword.slportal}}/{{site.data.keyword.BluSoftlayer_full}} カタログを使用して拡張ボリューム/ファイル共有を注文する手順を以下に示します。 簡単にマイグレーションできるようにするには、新規ボリュームのサイズを元のボリュームのサイズ以上にする必要があります。

### 新規エンデュランス・ストレージ・ボリュームの注文

1. [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}で**「ストレージ」** > **「{{site.data.keyword.filestorage_short}}」**をクリックするか、または {{site.data.keyword.BluSoftlayer_full}} カタログで**「インフラストラクチャー」** > **「ストレージ」** > **「{{site.data.keyword.filestorage_short}}」**をクリックします。
2. **「{{site.data.keyword.filestorage_short}} を注文」**をクリックします。
3. **「ストレージ・タイプの選択」**リストで**「エンデュランス」**を選択します。
4. **「ロケーション」**をクリックし、使用するデータ・センターを選択します。
   - 必ず、元のストレージと同じ場所に新しいストレージが追加されるようにしてください。
5. 請求オプションを選択します。 月単位の請求または時間単位の請求を選択できます。
6. **「エンデュランス」**をクリックし、IOPS ティアを選択します。
6. リストから**「使用可能なストレージ・サイズ (Usable Storage Size)」**を選択します。 新規ボリュームのサイズを元のボリュームのサイズ以上にする必要があります。
7. リストから**「スナップショット・スペース・サイズ (Snapshot Space Size)」**(使用可能なスペースに追加されます) を選択します。
8. **「次へ進む (Continue)」**をクリックします。 月額と日割り額が表示されます。これが注文の詳細を確認できる最後の機会です。 注文を変更する場合は、**「戻る」**をクリックします。
9. **「マスター・サービス契約を読み…」**チェック・ボックスをクリックし、**「注文する」**をクリックします。

### 暗号化パフォーマンス・ストレージ・ボリュームの注文

1. [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}で**「ストレージ」**、**「{{site.data.keyword.filestorage_short}}」**をクリックするか、または {{site.data.keyword.BluSoftlayer_full}} カタログで**「インフラストラクチャー」** > **「ストレージ」** > **「{{site.data.keyword.filestorage_short}}」**をクリックします。
2. **「{{site.data.keyword.filestorage_short}} を注文」**をクリックします。
3. **「ストレージ・タイプの選択」**リストで**「パフォーマンス」**を選択します。
4. **「ロケーション」**をクリックし、使用するデータ・センターを選択します。
    -  必ず、元のストレージと同じ場所に新しいストレージが追加されるようにしてください。
5. 請求オプションを選択します。 時間単位の請求または月単位の請求を選択できます。
6. 該当する**ストレージ・サイズ**の隣のラジオ・ボタンを選択します。
6. **「IOPS の指定」**フィールドに IOPS を入力します。
7. **「次へ進む (Continue)」**をクリックします。 月額と日割り額が表示されます。これが注文の詳細を確認できる最後の機会です。 注文を変更する場合は、**「戻る」**をクリックします。
8. **「マスター・サービス契約を読み…」**チェック・ボックスをクリックし、**「注文する」**をクリックします。

ストレージが 1 分もしないうちにプロビジョンされ、{{site.data.keyword.slportal}}の「{{site.data.keyword.filestorage_short}}」ページに表示されます。


## 新しい {{site.data.keyword.filestorage_short}} へのホストの許可

「許可」ホストとは、ボリュームに対するアクセス権を付与されたホストのことです。 ホストの許可がなければ、システムからストレージにアクセスすることも、ストレージを使用することもできません。

1. 新規ボリュームの名前をクリックします。
2. **「許可ホスト」**セクションまでスクロールします。
3. 右にある**「ホストの許可」**リンクをクリックします。 ボリュームにアクセスできるホストを選択します。

ホストが許可されたら、ボリュームをホストに接続します。


## スナップショットとレプリケーションのセットアップ

元のボリュームに関するスナップショットとレプリケーションが設定されている場合は、それらを新しいボリューム用にセットアップする必要があります。 元のボリュームと同じ設定で、レプリケーションとスナップショット・スペースを構成し、スナップショット・スケジュールを作成してください。

ターゲット・データ・センターが暗号化されていない場合は、そのデータ・センターがアップグレードされるまで、新しいボリュームのレプリケーションを設定できません。
{:important}


## データのマイグレーション

1. 元の {{site.data.keyword.filestorage_short}} ボリュームと新しいボリュームの両方に接続します。
  - 2 つのファイル共有をホストに接続する際に支援が必要な場合は、サポート・チケットをオープンしてください。

2. 元の {{site.data.keyword.filestorage_short}} ボリュームにあるデータのタイプと、そのデータを新しいファイル共有にコピーする最適な方法について検討します。
  - バックアップや静的コンテンツがあり、コピー中に変更がない場合は、特に検討事項はありません。
  - {{site.data.keyword.filestorage_short}} 上でデータベースまたは仮想マシンを実行している場合は、データ破壊を避けるため、コピー中にデータが変更されないようにしてください。 帯域幅に懸念がある場合は、非ピーク時にマイグレーションを行ってください。 これらの考慮事項に関して支援が必要な場合は、サポート・チケットを開いてください。

3. データを新しいロケーションにコピーします。
   - **Microsoft Windows**
     - 元の {{site.data.keyword.filestorage_short}} の LUN から新しい LUN にデータをコピーするには、新しいストレージをフォーマット設定してから、Windows エクスプローラーを使用してファイルをコピーします。
   - **Linux**
     - `rsync` を使用してデータをコピーできます。
       ```
       [root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage
       ```

   一度 `--dry-run` フラグを指定して前述のコマンドを使用し、パスの並びが正しいことを確認することをお勧めします。 このプロセスが中断された場合は、最後にコピー中であった宛先ファイルを削除して、その宛先ファイルが最初から新しいロケーションにコピーされるようにすることができます。

   `--dry-run` フラグなしでこのコマンドが完了すると、データは新しい {{site.data.keyword.filestorage_short}} ボリュームにコピーされます。 コマンドを再度実行して、何も欠落していないことを確認します。 両方のロケーションを手動で確認して、欠落しているものがないか探すこともできます。

   マイグレーションが完了したら、実動環境を新しい LUN に移動することができます。 その後、構成から元のボリュームを切り離して削除できます。 削除により、元のボリュームに関連付けられていたターゲット・サイト上のスナップショットやレプリカも除去されます。
