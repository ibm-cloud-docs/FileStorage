---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# SL CLI による {{site.data.keyword.filestorage_short}} の注文

通常は [{{site.data.keyword.slportal}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://control.softlayer.com/){:new_window} で注文する製品については、SL CLI を使用して注文することができます。SL API では、1 つの注文を複数の注文コンテナーで処理します。注文 CLI は、1 つの注文コンテナーのみを処理します。

SL CLI のインストール方法と使用法について詳しくは、[Python API Client](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window} を参照してください。
{:tip}

## 入手可能な {{site.data.keyword.filestorage_short}} オファーの検索

注文の際には、最初のコンポーネントとしてパッケージを検索します。パッケージは、{{site.data.keyword.BluSoftlayer_full}} での注文時に入手できるさまざまなトップレベルの製品に分かれています。パッケージの例として、VSI の CLOUD_SERVER、ベアメタル・サーバーの BARE_METAL_SERVER、{{site.data.keyword.filestorage_short}}と {{site.data.keyword.blockstorageshort}} の STORAGE_AS_A_SERVICE_STAAS があります。

パッケージ内では、いくつかの項目がカテゴリーに細分化されています。利便性を考えて事前設定されているパッケージもあれば、項目を個別に指定する必要があるパッケージもあります。あるパッケージの 1 つのカテゴリーが必須である場合、そのパッケージを注文するにはそのカテゴリーから項目を選択する必要があります。カテゴリーによっては、カテゴリー内の一部の項目は同時に選択できない場合があります。

各注文では場所 (データ・センター) を関連付ける必要があります。{{site.data.keyword.filestorage_short}} を注文する際には、コンピュート・インスタンスと同じ場所でプロビジョンしてください。
{:important}

`slcli order package-list` コマンドを使用して、注文するパッケージを検索できます。簡単な検索とフィルター操作を実行するために、`–keyword` オプションが用意されています。このオプションを使用すると、必要なパッケージを見つけやすくなります。

```
$ slcli order package-list --help
Usage: slcli order package-list [OPTIONS]

  List packages that can be ordered via the placeOrder API.

  Example:
      # List out all packages for ordering
      slcli order package-list

  Keywords can also be used for some simple filtering functionality to help
  find a package easier.

  Example:
     # List out all packages with "server" in the name
      slcli order package-list --keyword server

Options:
  --keyword TEXT  A word (or string) used to filter package names.
  -h, --help      Show this message and exit.
```

*Storage-as-a-Service Package 759 の検索方法に関する指示が必要*

```
$ slcli order package-list --keyword "Storage"
:.....................:.....................:
:         name        :       keyName       :
:.....................:.....................:
: ???                 : ???                 :
: ???                 : ???                 :
:.....................:.....................:
```

```
$ slcli order category-list STORAGE_AS_A_SERVICE_STAAS --required
:..................................:...................:............:
:               name               :    categoryCode   : isRequired :
:..................................:...................:............:
:              Example             :        ???        :     Y      :
:              Example             :        ???        :     Y      :
:              Example             :        ???        :     Y      :
:              Example             :        ???        :     Y      :
:..................................:...................:............:
```

`item-list` コマンドを使用して、注文に関する残りの項目を選択します。通常、パッケージには選択できる項目が多数含まれているので、`–category` オプションを使用して、関心のあるカテゴリーからの項目のみ取得します。

```
$ slcli order item-list STORAGE_AS_A_SERVICE_STAAS --category ??
:..........................:..............................................:
:         keyName          :                description                   :
:..........................:..............................................:
:           ???            :                    ????                      :
:           ???            :                    ????                      :
:           ???            :                    ????                      :
:           ???            :                    ????                      :
:..........................:..............................................:
```

API で {{site.data.keyword.filestorage_short}}を注文する方法について詳しくは、[order_file_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/file.html#SoftLayer.managers.file.FileStorageManager.order_file_volume){:new_window} を参照してください。
すべての新機能にアクセスできるようにするには、`Storage-as-a-Service Package 759` を注文します。
{:tip}

## 注文の検証

注文で必須のカテゴリーが欠落しているのではないかと不安な場合は、`–verify` フラグを指定して `place` コマンドを使用できます。いずれかのカテゴリーが欠落している場合は、画面に表示されます。


```
$ slcli order place --verify blablabla
:..............................................:.................................................:......:
:                keyName                       :                   description                   : cost :
:..............................................:.................................................:......:
:                  ???                         :                 yadi yadi yada                  :  0   :
:                  ???                         :                 yadi yadi yada                  :  0   :
:                  ???                         :                 yadi yadi yada                  :  0   :
:                  ???                         :                 yadi yadi yada                  :  0   :
:..............................................:.................................................:......:
```

出力には、注文する各項目と、その項目に関連したコストが表示されます。注文が検証に合格すると、競合する項目がなく、すべての必須カテゴリーの項目が注文で指定されていることが分かります。

## 注文

次のステップでは、注文します。

```
$ slcli order place .....

このアクションを実行すると、ご使用のアカウントに課金されます。 続行しますか? [y/N]: y

API response
```

デフォルトでは、合計 250 の {{site.data.keyword.filestorage_short}} ボリュームをプロビジョンできます。ご使用のボリュームの数を増やすには、営業担当員にお問い合わせください。 限度を増やす方法について詳しくは、[ストレージ制限の管理](managing-storage-limits.html)を参照してください。
{:important}

## ホストが新しいストレージにアクセスすることの許可

TBD

API を使用して、ホストが {{site.data.keyword.filestorage_short}} にアクセスする権限を与える方法について詳しくは、[authorize_host_to_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/file.html#SoftLayer.managers.file.FileStorageManager.authorize_host_to_volume){:new_window} を参照してください。
{:tip}

同時許可の制限について詳しくは、[FAQ](faqs.html) を参照してください。
{:important}

## 新しいストレージの接続

ホストのホスト・オペレーティング・システムに応じて、次のうち該当するリンクを使用します。
- [Linux への{{site.data.keyword.filestorage_short}}のマウント](accessing-file-storage-linux.html)
- [CentOS への{{site.data.keyword.filestorage_short}}のマウント](mounting-nsf-file-storage.html)
- [CoreOS への{{site.data.keyword.filestorage_short}}のマウント](mounting-storage-coreos.html)
- [cPanel を使用してバックアップするための{{site.data.keyword.filestorage_short}}の構成](configure-backup-cpanel.html)
- [Plesk を使用してバックアップするための{{site.data.keyword.filestorage_short}}の構成](configure-backup-plesk.html)
- [ESXi ホストでの {{site.data.keyword.filestorage_short}} ボリュームのマウント](architecture-guide-file-storage-vmware.html)
