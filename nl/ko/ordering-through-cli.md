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

# SLCLI를 통해 {{site.data.keyword.filestorage_short}} 주문
{: #orderingSLCLI}

일반적으로 [{{site.data.keyword.slportal}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/){:new_window}을 통해 주문하는 제품을 주문하는 데 SLCLI를 사용할 수 있습니다. SL API에서, 주문은 여러 주문 컨테이너로 구성됩니다. 주문 CLI는 하나의 주문 컨테이너와만 작동합니다.

SLCLI 설치 및 사용 방법에 관한 자세한 정보는 [Python API 클라이언트 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}를 참조하십시오.
{:tip}

## 사용 가능한 {{site.data.keyword.filestorage_short}} 오퍼 검색

주문할 때 검색하는 첫 번째 컴포넌트가 패키지입니다. 패키지는 {{site.data.keyword.BluSoftlayer_full}}에서 주문하는 데 사용할 수 있는 다양한 최상위 레벨 제품으로 구분됩니다. 몇 가지 예제 패키지는 VSI의 경우 CLOUD_SERVER, Bare Metal Server의 경우 BARE_METAL_SERVER, {{site.data.keyword.filestorage_short}} 및 {{site.data.keyword.blockstorageshort}}의 경우 STORAGE_AS_A_SERVICE_STAAS입니다.

패키지에서 일부 항목은 카테고리로 분할됩니다. 일부 패키지에는 편의를 위한 사전 설정 세트가 있으며 다른 패키지에서는 항목을 개별적으로 지정해야 합니다. 패키지의 카테고리가 필요한 경우, 패키지를 주문하려면 해당 카테고리의 항목을 선택해야 합니다. 카테고리에 따라 카테고리에 있는 일부 항목은 상호 배타적일 수 있습니다.

각 주문에는 위치(데이터 센터)가 연관되어 있어야 합니다. {{site.data.keyword.filestorage_short}}를 주문할 때 컴퓨팅 인스턴스와 동일한 위치에서 프로비저닝되는지 확인하십시오.
{:important}

`slcli order package-list` 명령을 사용하여 주문할 패키지를 찾을 수 있습니다. `–keyword` 옵션은 단순 검색 및 필터링을 수행하도록 제공됩니다. 이 옵션을 사용하면 필요한 패키지를 찾기가 쉬워집니다. `Storage-as-a-Service Package 759`를 검색하십시오.

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

`slcli file volume-order` 명령을 사용할 수도 있습니다.

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

API를 통해 {{site.data.keyword.filestorage_short}}를 주문하는 데 관한 자세한 정보는 [order_file_volume ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://softlayer-python.readthedocs.io/en/latest/api/managers/file.html#SoftLayer.managers.file.FileStorageManager.order_file_volume){:new_window}을 참조하십시오.
새 기능에 모두 액세스할 수 있으려면 `Storage-as-a-Service Package 759`를 주문하십시오.
{:tip}


## 주문하기

다음 예에서는 GB당 100 IOPS를 포함한 10GB {{site.data.keyword.filestorage_short}} 볼륨을 주문하는 방법을 보여줍니다.

```
# slcli file volume-order --storage-type performance --size 20 --location dal10 --iops 100
Order #32076317 placed successfully!
> Storage as a Service
> File Storage
> 20 GBs
> 100 IOPS
```

기본적으로 사용자는 총 250개의 {{site.data.keyword.blockstorageshort}} 및 {{site.data.keyword.filestorage_short}} 볼륨을 프로비저닝할 수 있습니다. 볼륨 수를 늘리려면 영업 담당자에게 문의하십시오. 한계를 늘리는 데 관한 자세한 정보는 [스토리지 한계 관리](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits)를 참조하십시오.
{:important}

## 호스트에 새 스토리지에 액세스하는 권한 부여

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

API를 통해 {{site.data.keyword.filestorage_short}}에 액세스하는 권한을 호스트에 부여하는 데 대한 자세한 정보는 [authorize_host_to_volume ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://softlayer-python.readthedocs.io/en/latest/api/managers/file.html#SoftLayer.managers.file.FileStorageManager.authorize_host_to_volume){:new_window}을 참조하십시오.
{:tip}

동시 권한 부여 한계에 관한 자세한 정보는 [FAQ](/docs/infrastructure/FileStorage?topic=FileStorage-faqs)를 참조하십시오.
{:important}

## 새 스토리지 연결
{: #mountingvolumesCLI}

호스트의 운영 체제에 따라 적절한 링크를 사용하십시오.
- [Linux의 {{site.data.keyword.filestorage_short}} 마운트](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [CentOS의 {{site.data.keyword.filestorage_short}} 마운트](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [Container Linux에 {{site.data.keyword.filestorage_short}} 마운트](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)
- [cPanel로 백업을 위한 {{site.data.keyword.filestorage_short}} 구성](/docs/infrastructure/FileStorage?topic=FileStorage-cPanelBackups)
- [Plesk로 백업을 위한 {{site.data.keyword.filestorage_short}} 구성](/docs/infrastructure/FileStorage?topic=FileStorage-PleskBackup)
- [ESXi 호스트에 {{site.data.keyword.filestorage_short}} 볼륨 마운트](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)
