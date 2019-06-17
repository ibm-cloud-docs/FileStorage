---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-10"

keywords: File Storage, file storage, NFS, upgrade, migrate to new

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.filestorage_short}}から拡張{{site.data.keyword.filestorage_short}}へのマイグレーション
{: #migratestorage}

現在、拡張 {{site.data.keyword.filestorage_full}} は一部のデータ・センターでのみ提供されています。 アップグレードされたデータ・センターと、調整可能な IOPS レートや拡張可能なボリュームなどの使用可能な機能のリストを確認するには、[ここ](/docs/infrastructure/FileStorage?topic=FileStorage-news)をクリックしてください。 プロバイダー管理の暗号化について詳しくは、[{{site.data.keyword.filestorage_short}} 保存データの暗号化](/docs/infrastructure/FileStorage?topic=FileStorage-encryption)を参照してください。

お勧めするマイグレーション・パスは、両方のボリュームに同時に接続して LUN 間で直接データを転送する方法です。 具体的な手順は、オペレーティング・システムと、コピー操作中にデータ変更が行われるかどうかによって異なります。

ホストに非暗号化 LUN が既に接続されていることを想定しています。 接続されていない場合は、以下の説明のうち、ご使用のオペレーティング・システムに最も該当する説明に従って、このタスクを実行してください。

- [Linux への{{site.data.keyword.filestorage_short}}のマウント](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [CentOS への{{site.data.keyword.filestorage_short}}のマウント](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [CoreOS への{{site.data.keyword.filestorage_short}}のマウント](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)

これらのデータ・センターでプロビジョンされる拡張{{site.data.keyword.filestorage_short}}・ボリュームはすべて、非暗号化ボリュームとは異なるマウント・ポイントになります。 両方のストレージ・ボリュームに正しいマウント・ポイントを使用するために、コンソールの**「ボリュームの詳細 (Volume Details)」**ページでマウント・ポイント情報を確認することができます。 API 呼び出し `SoftLayer_Network_Storage::getNetworkMountAddress()` を使用して正しいマウント・ポイントを取得することもできます。
{:tip}


## {{site.data.keyword.filestorage_short}} の作成

API を使用して注文する場合は、「Storage as a Service」パッケージを指定して、更新済みの機能を新規ストレージと一緒に取得してください。
{:important}

拡張された LUN は {{site.data.keyword.cloud}} カタログと  を通じて注文できます。簡単にマイグレーションできるようにするには、新規ボリュームのサイズを元のファイル共有のサイズ以上にする必要があります。

- [事前定義済み IOPS ティアによる {{site.data.keyword.filestorage_short}} の注文 (エンデュランス)](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole#endurance)
- [カスタム IOPS による {{site.data.keyword.filestorage_short}} の注文 (パフォーマンス)](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole#performance)

新しいストレージのマウントが数分で可能になります。 そのストレージは、「リソース・リスト」と {{site.data.keyword.blockstorageshort}} リストで確認できます。


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
  - バックアップ、静的コンテンツ、およびコピー中に変化が予想されないものがある場合、心配する必要はありません。
  - {{site.data.keyword.filestorage_short}} 上でデータベースまたは仮想マシンを実行している場合は、データ破壊を避けるため、コピー中にデータが変更されないようにしてください。
  - 帯域幅に懸念がある場合は、非ピーク時にマイグレーションを行ってください。
  - これらの考慮事項に関して支援が必要な場合は、サポート・チケットを開いてください。

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
