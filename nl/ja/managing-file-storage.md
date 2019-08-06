---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-25"

keywords: File Storage, file storage, NFS, authorizing hosts, rewoke access, grant access, view authorizations

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}


# {{site.data.keyword.filestorage_short}} の管理
{: #managingstorage}

{{site.data.keyword.filestorage_full}} のボリュームは、{{site.data.keyword.cloud}} コンソールで管理できます。

## {{site.data.keyword.filestorage_short}} へのアクセスをホストに許可する

「許可」ホストとは、特定のボリュームに対するアクセス権を付与されたホストのことです。 ホストの許可がなければ、システムからストレージにアクセスすることも、ストレージを使用することもできません。 ホストにボリュームへのアクセスを許可すると、ユーザー名とパスワードが生成されます。

ご使用のストレージと同じデータ・センターにあるホストを許可および接続できます。 複数のアカウントを使用できますが、あるアカウントのホストから別のアカウントのストレージへのアクセスを許可することはできません。
{:important}

1. [{{site.data.keyword.cloud}} コンソール](https://{DomainName}/){: external}に移動します。 メニューから**「クラシック・インフラストラクチャー」**を選択します。
2. **「ストレージ」** > **「{{site.data.keyword.filestorage_short}}」**とクリックして、使用する**「ボリューム名」**をクリックします。
3. ページの**「許可ホスト」**セクションまでスクロールします。
4. 右にある**「ホストの許可」**をクリックします。
5. デバイス・タイプ、サブネット、または IP アドレスを選択して、使用可能なホスト・リストにフィルターを掛けます。

   サブネットに基づいてリストにフィルターを掛けた場合、表示されるサブネットは、ストレージ・ボリュームと同じデータ・センター内のサブスクライブ・サブネットです。
   {:note}
6. リストから 1 つ以上のホストを選択して、**「保存」**をクリックします。

代わりの方法として、SLCLI で以下のコマンドを使用することができます。
```
# slcli file access-authorize --help
Usage: slcli file access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of one hardware server to authorize.
  -v, --virtual-id TEXT     The ID of one virtual server to authorize.
  -i, --ip-address-id TEXT  The ID of one IP address to authorize.
  -p, --ip-address TEXT     An IP address to authorize.
  -s, --subnet-id TEXT      An ID of one subnet to authorize.
  --help                    Show this message and exit.
```

## {{site.data.keyword.filestorage_short}} ボリュームへのアクセスを許可されたホストのリストを表示する

1. [{{site.data.keyword.cloud}} コンソール](https://{DomainName}/){: external}に移動します。 メニューから**「クラシック・インフラストラクチャー」**を選択します。
2. **「ストレージ」** > **「{{site.data.keyword.filestorage_short}}」**とクリックして、使用する**「ボリューム名」**をクリックします。
3. ページを**「許可ホスト」**セクションまでスクロールダウンします。

そこには、ボリュームへのアクセスが現在許可されているホストのリストが表示されます。

代わりの方法として、SLCLI で以下のコマンドを使用することができます。
```
# slcli file access-list --help
Usage: slcli file access-list [OPTIONS] VOLUME_ID

Options:
 --sortby TEXT   Column to sort by
 --columns TEXT  Columns to display. Options: id, name, type,
                 private_ip_address, source_subnet, host_iqn, username,
                 password, allowed_host_id
 -h, --help      Show this message and exit.
```


## ホストが許可されている {{site.data.keyword.filestorage_short}} ボリュームを表示する

ホストがアクセス権を持っているボリュームを表示できます。接続の確立に必要な情報 (ボリューム名、ストレージ・タイプ、ターゲット・アドレス、容量、ロケーション) も表示されます。

1. [{{site.data.keyword.cloud}} コンソール](https://{DomainName}/){: external}に移動します
2. メニューから**「クラシック・インフラストラクチャー」**を選択します。
3. **「デバイス」** > **「デバイス・リスト」**をクリックします。
4. 目的のデバイスをクリックします。
5. 「ストレージ」タブを選択します。

この特定のホストがアクセス権を持っているストレージ・ボリュームのリストが表示されます。いずれも、ストレージ・タイプ (ブロック、ファイル、その他) ごとにグループ化されています。 それぞれの**「アクション」**メニューから、他のストレージを許可したり、アクセス権を削除したりできます。


## {{site.data.keyword.filestorage_short}} のマウントとアンマウント

**「ボリュームの詳細 (Volume Details)」**ビューに表示されるマウント・ポイント情報を使用して、ホストから{{site.data.keyword.filestorage_short}}をマウントできます。 [Linux での {{site.data.keyword.filestorage_short}} へのアクセス](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)を参照してください。


## {{site.data.keyword.filestorage_short}} に対するホストのアクセス権を取り消す

ホストから特定のストレージ・ボリュームへのアクセスを停止する場合は、アクセス権を取り消すことができます。 アクセス権が取り消されると、ホスト接続はボリュームから除去されます。 オペレーティング・システムとアプリケーションはそのボリュームと通信できなくなります。

ホスト側の問題を防止するために、アクセス権を取り消す前にオペレーティング・システムからストレージ・ボリュームをアンマウントして、ドライブの欠落やデータ破損を回避してください。
{:important}

「デバイス・リスト」ビューまたは「ストレージ」ビューの「ストレージ」からアクセス権を取り消すことができます。

### デバイス・リストからのアクセス権の取り消し

1. [{{site.data.keyword.cloud}} コンソール](https://{DomainName}/){: external}に移動します。
2. メニューから**「クラシック・インフラストラクチャー」**を選択します。
3. **「デバイス」** > **「デバイス・リスト」**をクリックします。
2. 該当するデバイスをダブルクリックします。
3. **「ストレージ」**タブを選択します。
4. この特定のホストがアクセス権を持っているストレージ・ボリュームのリストが表示されます。いずれも、ストレージ・タイプ (ブロック、ファイル、その他) ごとにグループ化されています。 アクセス権を取り消すボリュームの隣にある、それぞれの**「アクション」**メニューを選択して、**「アクセスの取り消し」**をクリックします。
5. このアクションは元に戻せないので、ボリュームのアクセス権を取り消してよいかどうかを確認します。 ボリュームへのアクセス権を取り消すには**「はい (Yes)」**を、アクションをキャンセルするには**「いいえ (No)」**をクリックします。

特定のホストから複数のボリュームを切断する場合は、各ボリュームに対して「アクセス権の取り消し」アクションを繰り返す必要があります。
{:tip}


### ストレージ・ビューからのアクセス権の取り消し

1. [{{site.data.keyword.cloud}} コンソール](https://{DomainName}/){: external}に移動します。
2. メニューから**「クラシック・インフラストラクチャー」**を選択します。
3. **「ストレージ」** > **「{{site.data.keyword.filestorage_short}}」**とクリックし、アクセス権を取り消す**「ボリューム」**を選択します。
4. ページの**「許可ホスト」**セクションまでスクロールします。
5. アクセス権を取り消すホストの隣にある**「アクション」**をクリックして、**「アクセス権の取り消し」**を選択します。
6. このアクションは元に戻せないので、ボリュームのアクセス権を取り消してよいかどうかを確認します。 ボリュームへのアクセス権を取り消すには**「はい (Yes)」**を、アクションをキャンセルするには**「いいえ (No)」**をクリックします。

特定のボリュームから複数のホストを切断する場合は、ホストごとに「アクセス権の取り消し」アクションを繰り返す必要があります。
{:tip}

### SLCLI でのアクセス権の取り消し
SLCLI で次のコマンドを使用します。
```
# slcli file access-revoke --help
Usage: slcli file access-revoke [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of one hardware server to revoke authorization.
  -v, --virtual-id TEXT     The ID of one virtual server to revoke authorization.
  -i, --ip-address-id TEXT  The ID of one IP address to revoke authorization.
  -p, --ip-address TEXT     An IP address to revoke authorization.
  -s, --subnet-id TEXT      An ID of one subnet to revoke authorization.
  --help                    Show this message and exit.
```

## ストレージ・ボリュームをキャンセルする

特定のボリュームが不要になった場合は、そのストレージをキャンセルできます。 ストレージ・ボリュームをキャンセルするには、最初にすべてのホストのアクセス権を取り消す必要があります。

1. [{{site.data.keyword.cloud}} コンソール](https://{DomainName}/){: external}に移動します。 メニューから**「クラシック・インフラストラクチャー」**を選択します。
2. **「ストレージ」** > **「{{site.data.keyword.filestorage_short}}」**をクリックします。
3. キャンセルするボリュームの**「アクション」**をクリックし、**「{{site.data.keyword.filestorage_short}}のキャンセル」**を選択します。
4. ボリュームをただちにキャンセルするか、ボリュームがプロビジョンされた日の応当日にキャンセルするかを確認します。

   応当日にボリュームをキャンセルするオプションを選択した場合は、応当日の前にキャンセル要求を無効にすることができます。
   {:tip}
5. **「次へ進む (Continue)」**または**「閉じる」**をクリックします。
6. 確認応答チェック・ボックスをクリックして、**「確認」**をクリックします。

代わりの方法として、SLCLI で以下のコマンドを使用することができます。
```
# slcli file volume-cancel --help
Usage: slcli file volume-cancel [OPTIONS] VOLUME_ID

Options:
  --reason TEXT  An optional reason for cancellation
  --immediate    Cancels the file storage volume immediately instead of on the
                 billing anniversary
  -h, --help     Show this message and exit.
```

ボリュームをキャンセルするときには、再要求の待機期間が 24 時間あります。その 24 時間の間は、コンソールにボリュームが表示されています。再要求の期間が期限切れになると、データは破棄されて、ボリュームもコンソールから除去されます。ただし、ボリュームに対する請求処理は即時に停止します。詳しくは、[FAQ](/docs/infrastructure/FileStorage?topic=FileStorage-file-storage-faqs) を参照してください。
{:note}

 アクティブなレプリカにより、ストレージ・ボリュームの再利用がブロックされることがあります。元のボリュームをキャンセルしようとする前に、ボリュームがもはやマウントされていないこと、ホスト許可が取り消されたこと、および複製がキャンセルされたことを確認してください。
