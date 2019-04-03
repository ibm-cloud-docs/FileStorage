---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, adjusting IOPS, increase IOPS, decrease IOPS, modify IOPS

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# IOPS 조정
{: #adjustingIOPS}

{{site.data.keyword.filestorage_full}} 스토리지 사용자는 이 새 기능을 사용하여 기존 {{site.data.keyword.filestorage_short}}의 IOPS를 즉시 조정할 수 있습니다. 복제본을 작성하거나 수동으로 데이터를 새 스토리지에 복사할 필요가 없습니다. 조정 중에도 스토리지가 가동 중단되거나 액세스 불가능하지 않습니다.

스토리지에 대한 청구는 새 가격의 비례 배분된 차이가 현재 청구 주기에 추가되도록 업데이트됩니다. 다음 청구 주기에는 전체 새 금액이 청구됩니다.


## 조정 가능한 IOPS의 장점

- 비용 관리 – 일부 클라이언트에서는 최대 사용 시간 중에만 높은 IOPS가 필요할 수 있습니다. 예를 들어, 특정 대형 소매점에서는 겨울 연휴에 사용량이 가장 많으며, 이 기간에는 스토리지에 대해 한여름보다 더 높은 IOPS를 필요로 할 수 있습니다. 이 기능은 이러한 소매점에서 비용을 관리하면서 필요할 때 더 높은 IOPS를 위해 비용을 지불할 수 있게 해 줍니다.

## 제한사항
{: #limitsofadjustIOPS}

이 기능은 [특정 데이터 센터](/docs/infrastructure/FileStorage?topic=FileStorage-news)에서만 사용 가능합니다.

클라이언트는 자체 IOPS를 조정할 때 Endurance와 Performance 간에 전환할 수 없습니다. 사용자는 다음의 기준 및 제한사항을 기반으로 스토리지에 대해 새 IOPS 계층 또는 IOPS 레벨을 지정할 수 있습니다.

- 원래 볼륨이 Endurance 0.25 계층인 경우에는 IOPS 계층을 업데이트할 수 없습니다.
- 원래 볼륨이 0.30IOPS/GB 이하의 Performance인 경우, 사용 가능한 옵션에는 결과가 0.30IOPS/GB 이하인 크기 및 IOPS 조합만 포함됩니다.
- 원래 볼륨이 0.30IOPS/GB를 초과하는 Performance인 경우, 사용 가능한 옵션에는 결과가 0.30IOPS/GB를 초과하는 크기 및 IOPS 조합만 포함됩니다.

## IOPS 조정이 복제에 미치는 영향

볼륨에 복제가 있는 경우 복제본은 기본 볼륨의 IOPS 선택사항과 일치하도록 자동으로 업데이트됩니다.

## 스토리지의 IOPS 조정
{: #adjustingsteps}

1. {{site.data.keyword.filestorage_short}}의 목록으로 이동하십시오.
    - 고객 포털에서 **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하거나
    - {{site.data.keyword.BluSoftlayer_full}} 콘솔에서 **인프라** > **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하십시오.
2. 목록에서 볼륨을 선택하고 **조치** > **볼륨 수정**을 클릭하십시오.
3. **스토리지 IOPS 옵션** 아래에서 새로 선택하십시오.
    - Endurance(계층 IOPS)의 경우 스토리지의 0.25IOPS/GB보다 큰 IOPS 계층을 선택하십시오. 언제든지 IOPS 계층을 늘릴 수 있습니다. 그러나 줄이기는 한 달에 한 번만 가능합니다.
    - Performance(할당된 IOPS)의 경우 100 - 48,000IOPS 범위의 값을 입력하여 스토리지에 대한 새 IOPS 옵션을 지정하십시오. (주문 양식에서 크기별로 필요한 특정 경계를 반드시 살펴보십시오.)
4. 선택사항과 새 가격을 검토하십시오.
5. **마스터 서비스 계약을 읽었습니다...** 선택란을 클릭하고 **주문하기**를 클릭하십시오.
6. 몇 분 후 새 스토리지 할당이 사용 가능해집니다.

또는 SLCLI를 통해 IOPS를 업데이트할 수 있습니다.
```
# slcli file volume-modify --help
Usage: slcli file volume-modify [OPTIONS] VOLUME_ID

Options:
  -c, --new-size INTEGER        New Size of file volume in GB. ***If no size
                                is given, the original size of volume is
                                used.***
                                Potential Sizes: [20, 40, 80, 100,
                                250, 500, 1000, 2000, 4000, 8000, 12000]
                                Minimum: [the original size of the volume]
  -i, --new-iops INTEGER        Performance Storage IOPS, between 100 and 6000
                                in multiples of 100 [only for performance
                                volumes] ***If no IOPS value is specified, the
                                original IOPS value of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is less than 0.3, new IOPS/GB
                                must also be less than 0.3. If original
                                IOPS/GB for the volume is greater than or
                                equal to 0.3, new IOPS/GB for the volume must
                                also be greater than or equal to 0.3.]
  -t, --new-tier [0.25|2|4|10]  Endurance Storage Tier (IOPS per GB) [only for
                                endurance volumes] ***If no tier is specified,
                                the original tier of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is 0.25, new IOPS/GB for the
                                volume must also be 0.25. If original IOPS/GB
                                for the volume is greater than 0.25, new
                                IOPS/GB for the volume must also be greater
                                than 0.25.]
  -h, --help                    Show this message and exit.
```
