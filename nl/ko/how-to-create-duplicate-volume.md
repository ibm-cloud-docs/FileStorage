---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, duplicate volume

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 중복 {{site.data.keyword.filestorage_short}} 작성
{: #duplicatevolume}

기존 {{site.data.keyword.BluSoftlayer_full}} {{site.data.keyword.filestorage_full}}의 복제를 작성할 수 있습니다. 복제 볼륨은 기본적으로 원래 볼륨의 용량 및 성능 옵션을 상속하며 스냅샷의 특정 시점까지의 데이터 사본을 보유합니다.   

복제가 특정 시점 스냅샷의 데이터를 기반으로 하므로, 복제를 작성하려면 우선 스냅샷 영역이 원래 볼륨에서 필요합니다. 스냅샷 및 스냅샷 영역 주문 방법에 대해 자세히 알아보려면 [스냅샷 문서](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots)를 참조하십시오.  

복제는 **기본** 및 **복제본** 볼륨으로부터 작성될 수 있습니다. 새 복제는 원래 볼륨과 동일한 데이터 센터에 작성됩니다. 복제본 볼륨에서 복제를 작성하는 경우에는 새 볼륨이 복제본 볼륨과 동일한 데이터 센터에 작성됩니다.

{{site.data.keyword.containerlong}}의 데디케이티드 계정 사용자인 경우에는 [{{site.data.keyword.containerlong_notm}} 문서](/docs/containers?topic=containers-backup_restore#backup_restore)에서 볼륨 복제에 대한 옵션을 참조하십시오.
{:tip}

복제 볼륨은 스토리지가 프로비저닝되는 순간 읽기/쓰기를 위해 호스트에 의해 액세스될 수 있습니다. 그러나 스냅샷 및 복제는 원본에서 복제본으로 데이터 복사가 완료될 때까지 허용되지 않습니다. 데이터 복사가 완료되면 복제본을 독립적인 볼륨으로서 관리하고 사용할 수 있습니다.

이 기능은 대부분의 위치에서 사용 가능합니다. 사용 가능한 데이터 센터의 목록을 보려면 [여기](/docs/infrastructure/FileStorage?topic=FileStorage-news)를 클릭하십시오.

다음은 중복 볼륨의 일반적인 용도를 보여주는 몇 가지 예입니다.
- **재해 복구 테스트**. 복제를 인터럽트하지 않으면서, 복제본 볼륨의 복제를 작성하여 재해가 발생하는 경우에도 데이터가 온전하며 사용 가능한지 확인합니다.
- **골든 사본**. 다양한 용도로 다중 인스턴스가 작성될 수 있는 골든 사본으로서 스토리지 볼륨을 사용합니다.
- **데이터 새로 고치기**. 테스트 용도로 비프로덕션 환경에 마운트할 프로덕션 데이터의 사본을 작성합니다.
- **스냅샷에서 복원**. 스냅샷 복원 기능으로 전체 원래 볼륨을 겹쳐쓰지 않고 스냅샷의 특정 파일 및 날짜로 원래 볼륨의 데이터를 복원합니다.
- **개발 및 테스트(개발/테스트)**. 한 번에 최대 4개의 동시 볼륨 복제를 작성하여 개발 및 테스트용 복제 데이터를 작성합니다.
- **스토리지 크기 조정**. 데이터를 이동할 필요 없이 크기 또는 IOPS 비율(또는 둘 다)이 새로운 볼륨을 작성합니다.  

몇 가지 방법으로 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}을 통해 중복 볼륨을 작성할 수 있습니다.


## 스토리지 목록의 특정 볼륨에서 복제 작성

1. {{site.data.keyword.filestorage_short}}의 목록으로 이동하십시오.
    - 고객 포털에서 **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하거나
    - {{site.data.keyword.BluSoftlayer_full}} 카탈로그에서 **인프라** > **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하십시오.
2. 목록에서 LUN을 선택하고 **조치** > **LUN(볼륨) 복제**를 클릭하십시오.
3. 스냅샷 옵션을 선택하십시오.
    - 비복제본 볼륨에서 주문하는 경우,
      - **새 스냅샷에서 작성** - 이 조치는 복제에 사용되는 스냅샷을 작성합니다. 볼륨에 현재 스냅샷이 없거나 이 작업 후 바로 복제를 작성하려는 경우 이 옵션을 사용하십시오.</br>
      - **최신 스냅샷에서 작성** - 이 조치는 이 볼륨에 대해 존재하는 최신 스냅샷에서 복제를 작성합니다.
    - 복제본 볼륨에서 주문하는 경우, 스냅샷에 대한 유일한 옵션은 사용 가능한 최신 스냅샷을 사용하는 것입니다.
4. 스토리지 유형 및 위치는 원래 볼륨과 동일하게 유지됩니다.
5. 시간별 또는 월별 청구 - 시간별 또는 월별 청구로 새 복제 LUN을 프로비저닝하도록 선택할 수 있습니다. 원래 볼륨에 대한 청구 유형은 자동으로 선택됩니다. 새 복제 스토리지에 대해 다른 청구 유형을 선택하려는 경우에는 여기서 선택할 수 있습니다.
5. 원하는 경우 새 볼륨에 대해 IOPS 또는 IOPS 계층을 지정할 수 있습니다. 원래 볼륨에 대해 지정된 IOPS는 기본값으로 설정되어 있습니다. 사용 가능한 성능 및 크기 조합이 표시됩니다.
    - 원래 볼륨이 0.25IOPS Endurance 계층인 경우에는 새로 선택할 수 없습니다.
    - 원래 볼륨이 2, 4 또는 10IOPS Endurance 계층인 경우에는 새 볼륨에 대해 해당 계층들 간에 어디로든 이동할 수 있습니다.
6. 원래 볼륨보다 크도록 새 볼륨의 크기를 업데이트할 수 있습니다. 원래 볼륨의 크기는 기본값으로 설정되어 있습니다.

   {{site.data.keyword.filestorage_short}}는 원래 볼륨 크기의 10배까지 크기가 조정될 수 있습니다.
   {:tip}
7. 새 볼륨의 스냅샷 영역을 업데이트하여 스냅샷 영역을 늘리거나, 줄이거나 제거할 수 있습니다. 기본적으로 원래 볼륨의 스냅샷 영역이 설정되어 있습니다.
8. **계속**을 클릭하여 주문을 제출하십시오.


## 특정 스냅샷에서 복제 작성

1. {{site.data.keyword.filestorage_short}}의 목록으로 이동하십시오.
2. 목록에서 볼륨을 클릭하여 세부사항 페이지를 보십시오. 복제본 또는 비복제본 볼륨일 수 있습니다.
3. 아래로 스크롤하여 세부사항 페이지의 목록에서 기존 스냅샷을 선택하고 **조치** > **복제**를 클릭하십시오.   
4. 스토리지 유형(Endurance 및 Performance) 및 위치는 원래 볼륨과 동일하게 유지됩니다.
5. 사용 가능한 성능 및 크기 조합이 표시됩니다. 원래 볼륨에 대해 지정된 IOPS는 기본값으로 설정되어 있습니다. 새 볼륨에 대해 IOPS 또는 IOPS 계층을 지정할 수 있습니다.
    - 원래 볼륨이 0.25IOPS Endurance 계층인 경우에는 새로 선택할 수 없습니다.
    - 원래 볼륨이 2, 4 또는 10IOPS Endurance 계층인 경우에는 새 볼륨에 대해 해당 계층들 간에 어디로든 이동할 수 있습니다.
6. 원래 볼륨보다 크도록 새 볼륨의 크기를 업데이트할 수 있습니다. 원래 볼륨의 크기는 기본값으로 설정되어 있습니다.

   {{site.data.keyword.filestorage_short}}는 원래 볼륨 크기의 10배까지 크기가 조정될 수 있습니다.
   {:tip}
7. 새 볼륨의 스냅샷 영역을 업데이트하여 스냅샷 영역을 늘리거나, 줄이거나 제거할 수 있습니다. 기본적으로 원래 볼륨의 스냅샷 영역이 설정되어 있습니다.
8. **계속**을 클릭하여 복제를 주문하십시오.

## SLCLI를 통해 복제 작성
```
# slcli file volume-duplicate --help
Usage: slcli file volume-duplicate [OPTIONS] ORIGIN_VOLUME_ID

Options:
  -o, --origin-snapshot-id INTEGER
                                  ID of an origin volume snapshot to use for
                                  duplcation.
  -c, --duplicate-size INTEGER    Size of duplicate file volume in GB. ***If
                                  no size is specified, the size of the origin
                                  volume will be used.***
                                  Minimum: [the size
                                  of the origin volume]
  -i, --duplicate-iops INTEGER    Performance Storage IOPS, between 100 and
                                  6000 in multiples of 100 [only used for
                                  performance volumes] ***If no IOPS value is
                                  specified, the IOPS value of the origin
                                  volume will be used.***
                                  Requirements: [If
                                  IOPS/GB for the origin volume is less than
                                  0.3, IOPS/GB for the duplicate must also be
                                  less than 0.3. If IOPS/GB for the origin
                                  volume is greater than or equal to 0.3,
                                  IOPS/GB for the duplicate must also be
                                  greater than or equal to 0.3.]
  -t, --duplicate-tier [0.25|2|4|10]
                                  Endurance Storage Tier (IOPS per GB) [only
                                  used for endurance volumes] ***If no tier is
                                  specified, the tier of the origin volume
                                  will be used.***
                                  Requirements: [If IOPS/GB
                                  for the origin volume is 0.25, IOPS/GB for
                                  the duplicate must also be 0.25. If IOPS/GB
                                  for the origin volume is greater than 0.25,
                                  IOPS/GB for the duplicate must also be
                                  greater than 0.25.]
  -s, --duplicate-snapshot-size INTEGER
                                  The size of snapshot space to order for the
                                  duplicate. ***If no snapshot space size is
                                  specified, the snapshot space size of the
                                  origin file volume will be used.***
                                  Input
                                  "0" for this parameter to order a duplicate
                                  volume with no snapshot space.
  --billing [hourly|monthly]      Optional parameter for Billing rate (default
                                  to monthly)
  -h, --help                      Show this message and exit.
```

## 복제 볼륨 관리

데이터가 원래 볼륨에서 복제 볼륨으로 복사되는 동안 세부사항 페이지에는 복제가 진행 중임을 보여주는 상태가 표시됩니다. 이 시간 중에는 호스트에 연결하여 볼륨에 대한 읽기/쓰기를 수행할 수 있지만, 스냅샷 스케줄을 작성할 수는 없습니다. 복제 프로세스가 완료되면 새 볼륨은 원래 볼륨으로부터 독립되며 스냅샷 및 복제 등으로 정상적으로 관리할 수 있습니다.
