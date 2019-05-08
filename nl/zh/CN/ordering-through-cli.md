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

# 通过 SLCLI 订购 {{site.data.keyword.filestorage_short}}
{: #orderingSLCLI}

可以使用 SLCLI 为通常通过 [{{site.data.keyword.slportal}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/){:new_window} 订购的产品下订单。在 SL API 中，订单可由多个订单容器组成。订单 CLI 仅使用一个订单容器。

有关如何安装和使用 SLCLI 的更多信息，请参阅 [Python API 客户机 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://softlayer-python.readthedocs.io/en/latest/cli/){:new_window}。
{:tip}

## 搜索可用的 {{site.data.keyword.filestorage_short}} 产品

下订单时要查找的第一个组件是包。在可供在 {{site.data.keyword.BluSoftlayer_full}} 中进行订购的不同顶级产品之间拆分包。一些示例包为 CLOUD_SERVER（用于 VSI）、BARE_METAL_SERVER（用于裸机服务器）和 STORAGE_AS_A_SERVICE_STAAS（用于 {{site.data.keyword.blockstorageshort}} 和 {{site.data.keyword.filestorage_short}}）。

利用包，可将某些项细分到类别。为方便起见，某些包有预设项，而其他包需要单独指定项。如果包的类别是必需的，那么必须选择该类别的项来订购包。根据类别，类别中的某些项可能互斥。

每个订单必须具有关联的位置（数据中心）。在订购 {{site.data.keyword.filestorage_short}} 时，确保在与计算实例相同的位置中进行供应。
{:important}

您可以使用 `slcli order package-list` 命令以查找想要订购的包。提供了 `–keyword` 选项以用于执行简单搜索和过滤。通过此选项，可以更轻松地查找所需包。请查找 `Storage-as-a-Service Package 759`。

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

此外，还可以使用 `slcli file volume-order` 命令。

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

有关通过 API 订购 {{site.data.keyword.filestorage_short}} 的更多信息，请参阅 [order_file_volume ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://softlayer-python.readthedocs.io/en/latest/api/managers/file/#SoftLayer.managers.file.FileStorageManager.order_file_volume){:new_window}。
要能够访问所有新功能，请订购 `Storage-as-a-Service Package 759`。
{:tip}


## 下订单

以下示例显示了如何订购一个 10 GB 的 {{site.data.keyword.filestorage_short}} 卷（100 IOPS/GB）。

```
# slcli file volume-order --storage-type performance --size 20 --location dal10 --iops 100
Order #32076317 placed successfully!
> Storage as a Service
> File Storage
> 20 GBs
> 100 IOPS
```

缺省情况下，总共可以供应 250 个 {{site.data.keyword.blockstorageshort}} 和 {{site.data.keyword.filestorage_short}} 卷。要增加卷的数量，请联系销售代表。有关提高限制的更多信息，请参阅[管理存储限制](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits)。
{:important}

## 授权主机访问新存储器

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

有关通过 API 授权主机访问 {{site.data.keyword.filestorage_short}} 的更多信息，请参阅 [authorize_host_to_volume ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://softlayer-python.readthedocs.io/en/latest/api/managers/file/#SoftLayer.managers.file.FileStorageManager.authorize_host_to_volume){:new_window}。
{:tip}

有关同时授权限制的更多信息，请参阅[常见问题](/docs/infrastructure/FileStorage?topic=FileStorage-faqs)。
{:important}

## 连接新存储器
{: #mountingvolumesCLI}

根据主机的操作系统，访问相应的链接。
- [在 Linux 上安装 {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [在 CentOS 中安装 {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [在 Container Linux 上安装 {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)
- [使用 cPanel 配置 {{site.data.keyword.filestorage_short}} 进行备份](/docs/infrastructure/FileStorage?topic=FileStorage-cPanelBackups)
- [使用 Plesk 配置 {{site.data.keyword.filestorage_short}} 进行备份](/docs/infrastructure/FileStorage?topic=FileStorage-PleskBackup)
- [在 ESXi 主机上安装 {{site.data.keyword.filestorage_short}} 卷](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)
