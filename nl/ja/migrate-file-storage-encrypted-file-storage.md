---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
 
# {{site.data.keyword.filestorage_short}}から暗号化{{site.data.keyword.filestorage_short}}へのマイグレーション

エンデュランスまたはパフォーマンスの暗号化 {{site.data.keyword.filestorage_full}} が、一部のデータ・センターで提供開始されました。 ここでは、{{site.data.keyword.filestorage_short}}を非暗号化から暗号化にマイグレーションする方法について説明します。 プロバイダー管理の暗号化ストレージについて詳しくは、[保存データの{{site.data.keyword.filestorage_short}}暗号化](block-file-storage-encryption-rest.html)の記事を参照してください。 アップグレードされたデータ・センターと使用可能な機能のリストを確認するには、[ここ](new-ibm-block-and-file-storage-location-and-features)をクリックしてください。

お勧めするマイグレーション・パスは、両方のボリュームに同時に接続してファイル・ボリューム間で直接データを転送する方法です。 具体的な手順は、オペレーティング・システムと、コピー操作中にデータ変更が行われるかどうかによって異なります。

ご参考までに、一般的なシナリオで大まかに説明します。 ホストに非暗号化ファイル・ボリュームが既に接続されていることを前提としています。 そうでない場合は、以下の説明のうち、実行しているオペレーティング・システムに最も該当する説明に従って、このタスクを実行してください。 

**注:**  すべての暗号化{{site.data.keyword.filestorage_short}}・ボリュームは、非暗号化ボリュームとは異なるマウント・ポイントになります。  暗号化と非暗号化の両方の{{site.data.keyword.filestorage_short}}・ボリュームに正しいマウント・ポイントを使用するには、UI の**「ボリュームの詳細 (Volume Details)」**ページでマウント・ポイント情報を参照するか、API 呼び出し SoftLayer_Network_Storage::getNetworkMountAddress() を使用して正しいマウント・ポイントを取得してください。

[Linux での{{site.data.keyword.filestorage_short}}へのアクセス](accessing-file-storage-linux.html)

## 暗号化ファイル・ボリュームの作成

マイグレーション・プロセスを円滑に進めるために、以下の手順を使用して、同じサイズ以上の暗号化ボリュームを作成します。

### 暗号化エンデュランス・ストレージ・ボリュームの注文

1. [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}のホーム・ページから**「ストレージ」** > **「{{site.data.keyword.filestorage_short}}」**をクリックするか、{{site.data.keyword.BluSoftlayer_full}} カタログで**「インフラストラクチャー」** > **「ストレージ」** > **「{{site.data.keyword.filestorage_short}}」**をクリックします。

2. 「{{site.data.keyword.filestorage_short}}」ページで**「{{site.data.keyword.filestorage_short}}を注文」**リンクをクリックします。

3. **「エンデュランス」**を選択します。

4. 元のボリュームが置かれているデータ・センターを選択します。 暗号化はアスタリスクが付いたデータ・センターでしか使用できないので注意してください。

5. 必要な **IOPS ティア**を入力します。

6. 必要なストレージ・スペース量を GB 単位で選択します。 TB の場合、1 TB は 1,000 GB、12 TB は 12,000 GB に相当します。

7. スナップショット用の必要なストレージ・スペース量を GB 単位で入力します。

8. ドロップダウン・リストから**「VMware OS」**を選択します。

9. 注文を送信します。
 
### 暗号化パフォーマンス・ストレージ・ボリュームの注文

1. [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}のホーム・ページから**「ストレージ」** > **「{{site.data.keyword.filestorage_short}}」**をクリックするか、{{site.data.keyword.BluSoftlayer_full}} カタログで**「インフラストラクチャー」** > **「ストレージ」** > **「{{site.data.keyword.filestorage_short}}」**をクリックします。

2. **「{{site.data.keyword.filestorage_short}}を注文」**をクリックします。

3. **「パフォーマンス」**を選択します。

4. 元のボリュームが置かれているデータ・センターを選択します。 暗号化はアスタリスク (`*`) が付いたデータ・センターでしか使用できないので注意してください。

5. 必要なストレージ・スペース量 (元のボリュームのサイズ以上) を GB 単位で選択します。

6. 必要な IOPS 量を 100 単位で入力します。

7. ドロップダウン・リストから VMware OS を選択します。

8. 注文を送信します。

ストレージが 1 分もしないうちにプロビジョンされ、カスタマー・ポータルの「{{site.data.keyword.filestorage_short}}」ページに表示されます。

 
## ホストへの新規ボリュームの接続

「許可」ホストとは、ボリュームに対するアクセス権を付与されたホストのことです。 ホストの許可がなければ、システムからストレージにアクセスすることも、ストレージを使用することもできません。

1. 暗号化**ボリュームの名前**をクリックします。

2. ページの**「許可ホスト」**セクションまでスクロールします。

3. ページの右側にある**「ホストの許可」**リンクをクリックします。 ボリュームにアクセスできるホストを選択します。

許可されたら、ボリュームをホストに接続します。

 
## スナップショットとレプリケーション

元のボリュームにスナップショットとレプリケーションを設定していましたか? そうであれば、元のボリュームと同じ設定で新しい暗号化ボリュームのレプリケーションとスナップショット・スペースをセットアップし、スナップショット・スケジュールを作成する必要があります。 

暗号化できるようにターゲット・データ・センターがアップグレードされていない場合は、そのデータ・センターがアップグレードされるまで、新規ボリュームのレプリケーションを設定できません。

 
## データのマイグレーション

元の{{site.data.keyword.filestorage_short}}・ボリュームと暗号化{{site.data.keyword.filestorage_short}}・ボリュームの両方にホストが接続されているはずです。 そうでない場合は、以下のようにします。

• 上記の手順に従ったこと、資料を正しく参照したことを確認してください。

• 2 つのボリュームをホストに接続するための支援がさらに必要な場合は、サポート・チケットを開いてください。

### データに関する考慮事項

ここで、元の{{site.data.keyword.filestorage_short}}・ボリュームにあるデータのタイプと、そのデータを暗号化ボリュームにコピーする最適な方法について検討しましょう。 バックアップや静的コンテンツがあり、コピー中に変更がない場合は、特に考慮事項はありません。

{{site.data.keyword.filestorage_short}}上でデータベースまたは仮想マシンを実行している場合は、破損が発生しないように、コピー中は元のボリューム上のデータが変更されないようにしてください。 帯域幅に懸念がある場合は、非ピーク時にマイグレーションを行う必要があります。 これらの考慮事項に関して支援が必要な場合は、遠慮なくサポート・チケットを開いてください。

### Microsoft Windows

元の{{site.data.keyword.filestorage_short}}・ボリュームから暗号化ボリュームにデータをコピーするには、新しいストレージをフォーマットし、Windows エクスプローラーを使用してファイルをコピーしてください。

### Linux

rsync を使用してデータをコピーすることを検討していることでしょう。 以下にコマンドの例を示します。

`[root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage` 

一度 `--dry-run` フラグを指定して上記のコマンドを使用して、パスの並びが正しいことを確認することをお勧めします。 このプロセスが中断された場合は、最後にコピー中であった宛先ファイルを削除して、その宛先ファイルが最初から新規ロケーションにコピーされるようにする必要があります。

`--dry-run` フラグなしでこのコマンドが完了したら、データは暗号化{{site.data.keyword.filestorage_short}}・ボリュームにコピーされています。 スクロールアップしてコマンドを再度実行して、何も欠落していないことを確認する必要があります。 両方のロケーションを手動で確認して、欠落しているものがないか探すことをお勧めします。

マイグレーションが完了したら、実動環境を暗号化ボリュームに移し、構成から元のボリュームを切り離して削除することができます。 削除により、元のボリュームに関連付けられていたターゲット・サイト上のスナップショットやレプリカも除去されることに注意してください。