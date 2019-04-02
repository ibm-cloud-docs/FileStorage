---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, SLCLI, provisioning, API

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# SLCLI による {{site.data.keyword.filestorage_short}} の注文
{: #orderingSLCLI}

通常は [{{site.data.keyword.slportal}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://control.softlayer.com/){:new_window} で注文する製品については、SLCLI を使用して注文することができます。 SL API では、1 つの注文を複数の注文コンテナーで処理します。 注文 CLI は、1 つの注文コンテナーのみを処理します。

SLCLI をインストールして使用する方法について詳しくは、[Python API クライアント![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}を参照してください。
{:tip}

## 入手可能な {{site.data.keyword.filestorage_short}} オファーの検索

注文の際には、最初のコンポーネントとしてパッケージを検索します。 パッケージは、{{site.data.keyword.BluSoftlayer_full}} での注文時に入手できるさまざまなトップレベルの製品に分かれています。 パッケージの例として、VSI の CLOUD_SERVER、ベアメタル・サーバーの BARE_METAL_SERVER、{{site.data.keyword.filestorage_short}}と {{site.data.keyword.blockstorageshort}} の STORAGE_AS_A_SERVICE_STAAS があります。

パッケージ内では、いくつかの項目がカテゴリーに細分化されています。 利便性を考えて事前設定されているパッケージもあれば、項目を個別に指定する必要があるパッケージもあります。 あるパッケージの 1 つのカテゴリーが必須である場合、そのパッケージを注文するにはそのカテゴリーから項目を選択する必要があります。 カテゴリーによっては、カテゴリー内の一部の項目は同時に選択できない場合があります。

各注文では場所 (データ・センター) を関連付ける必要があります。 {{site.data.keyword.filestorage_short}} を注文する際には、コンピュート・インスタンスと同じ場所でプロビジョンしてください。
{:important}

`slcli order package-list` コマンドを使用して、注文するパッケージを検索できます。 簡単な検索とフィルター操作を実行するために、`–keyword` オプションが用意されています。 このオプションにより、必要なパッケージの検索が容易になります。 `Storage-as-a-Service Package 759` を探します。

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

`slcli file volume-order` コマンドを使用することもできます。

```
# slcli file volume-order --help
Usage: slcli file volume-order [OPTIONS]

  Order a file storage volume.

Options:
  --storage-type [performance|endurance]
                                  Type of file storage volume  [required]
  --size INTEGER                  Size of file storage volume in GB
                                  [required]
  --iops INTEGER                  Performance Storage IOPs, between 100 and
                                  6000 in multiples of 100  [required for
                                  storage-type performance]
  --tier [0.25|2|4|10]            Endurance Storage Tier (IOP per GB)
                                  [required for storage-type endurance]
  --location TEXT                 Datacenter short name (e.g.: dal09)
                                  [required]
  --snapshot-size INTEGER         Optional parameter for ordering snapshot
                                  space along with endurance file storage;
                                  specifies the size (in GB) of snapshot space
                                  to order
  --service-offering [storage_as_a_service|enterprise|performance]
                                  The service offering package to use for
                                  placing the order [optional, default is
                                  'storage_as_a_service']
  --billing [hourly|monthly]      Optional parameter for Billing rate (default
                                  to monthly)
  -h, --help                      Show this message and exit.
```

API で {{site.data.keyword.filestorage_short}} を注文する方法について詳しくは、[order_file_volume ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://softlayer-python.readthedocs.io/en/latest/api/managers/file.html#SoftLayer.managers.file.FileStorageManager.order_file_volume){:new_window} を参照してください。
すべての新機能にアクセスできるようにするには、`Storage-as-a-Service Package 759` を注文します。
{:tip}


## 注文

次の例では、1 GB 当たり 100 IOPS の 10-GB {{site.data.keyword.filestorage_short}} ボリュームを注文する方法を示しています。

```
# slcli file volume-order --storage-type performance --size 20 --location dal10 --iops 100
Order #32076317 placed successfully!
> Storage as a Service
> File Storage
> 20 GBs
> 100 IOPS
```

デフォルトでは、{{site.data.keyword.blockstorageshort}} ボリュームと {{site.data.keyword.filestorage_short}} ボリュームを組み合わせた合計で 250 個をプロビジョンできます。 ご使用のボリュームの数を増やすには、営業担当員にお問い合わせください。 限度を増やす方法について詳しくは、[ストレージ制限の管理](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits)を参照してください。
{:important}

## ホストが新しいストレージにアクセスすることの許可

```
# slcli file access-authorize --help
Usage: slcli file access-authorize [OPTIONS] VOLUME_ID

  Authorizes hosts to access a given volume

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to authorize
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to authorize
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to authorize
  --ip-address TEXT         An IP address to authorize
  -s, --subnet-id TEXT      The id of one SoftLayer_Network_Subnet to
                            authorize
  --help                    Show this message and exit.
```

API を使用して、{{site.data.keyword.filestorage_short}} へのアクセスをホストに許可する方法について詳しくは、[authorize_host_to_volume ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://softlayer-python.readthedocs.io/en/latest/api/managers/file.html#SoftLayer.managers.file.FileStorageManager.authorize_host_to_volume){:new_window} を参照してください。
{:tip}

同時許可の制限について詳しくは、[FAQ](/docs/infrastructure/FileStorage?topic=FileStorage-faqs) を参照してください。
{:important}

## 新しいストレージの接続
{: #mountingvolumesCLI}

ホストのホスト・オペレーティング・システムに応じて、次のうち該当するリンクを使用します。
- [Linux への{{site.data.keyword.filestorage_short}}のマウント](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [CentOS への{{site.data.keyword.filestorage_short}}のマウント](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [Container Linux への {{site.data.keyword.filestorage_short}} のマウント](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)
- [cPanel を使用してバックアップするための{{site.data.keyword.filestorage_short}}の構成](/docs/infrastructure/FileStorage?topic=FileStorage-cPanelBackups)
- [Plesk を使用してバックアップするための{{site.data.keyword.filestorage_short}}の構成](/docs/infrastructure/FileStorage?topic=FileStorage-PleskBackup)
- [ESXi ホストでの {{site.data.keyword.filestorage_short}} ボリュームのマウント](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)
