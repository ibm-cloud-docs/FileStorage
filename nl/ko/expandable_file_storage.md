---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-18"

keywords: File Storage, modify volume, NFS, file storage, expand capacity

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 파일 공유 용량 확장
{: #expandCapacity}

현재 {{site.data.keyword.filestorage_full}} 사용자는 이 기능을 사용하여 {{site.data.keyword.filestorage_short}}의 크기를 GB 단위로 최대 12TB까지 즉시 확장할 수 있습니다. 복제본을 작성하거나 데이터를 더 큰 볼륨으로 수동으로 마이그레이션하지 않아도 됩니다. 크기 조정이 진행 중인 동안 스토리지에 대한 액세스는 중단되거나 차단되지 않습니다.

볼륨에 대한 비용 청구는 현재 청구 주기에 새 가격의 일할 계산된 차이를 추가하도록 자동으로 업데이트됩니다. 다음 청구 주기에는 전체 새 금액이 청구됩니다.

이 기능은 [데이터 센터 선택](/docs/infrastructure/FileStorage?topic=FileStorage-selectDC)에서만 사용 가능합니다.

## 확장 가능한 스토리지의 장점

- **비용 관리** - 데이터가 증가할 가능성이 있다는 것은 알고 있지만, 일단은 작은 양의 스토리지로 시작해야 하는 경우가 있습니다. 확장 기능은 고객이 시작 스토리지 비용을 절감하고 이후 필요에 따라 스토리지를 늘릴 수 있게 해 줍니다.  

- **스토리지 요구사항 증가** - 빠른 초과 확장이 필요한 고객은 이러한 확장을 관리하기 위해 빠르고 손쉽게 자체 스토리지의 크기를 늘리는 방법이 필요합니다.

## 스토리지 용량 확장이 복제에 미치는 영향

기본 스토리지에 대해 확장 조치를 수행하면 복제본의 크기가 자동으로 조정됩니다.

## 제한사항
{: #limitsofextension}

이 기능은 고급 기능이 있는 [데이터 센터](/docs/infrastructure/FileStorage?topic=FileStorage-selectDC)에서 프로비저닝된 스토리지에만 사용 가능합니다. 이러한 데이터 센터에서 프로비저닝된 암호화된 스토리지는 최대 12TB까지 늘릴 수 있습니다.

Endurance로 프로비저닝된 {{site.data.keyword.filestorage_short}}에 대한 기존 크기 제한사항은 계속 적용됩니다(10IOPS 계층의 경우 최대 4TB, 기타 모든 계층의 경우 최대 12TB).

## 스토리지 크기 조정
{: #resizingsteps}

1. [{{site.data.keyword.cloud}} 콘솔](https://{DomainName}/){: external}로 이동하십시오. 메뉴에서 **클래식 인프라**를 선택하십시오. **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하십시오.
2. 목록에서 볼륨을 선택하고 **조치** > **볼륨 수정**을 클릭하십시오.
3. 새 스토리지 크기(GB)를 입력하십시오.
4. 선택사항과 새 가격을 검토하십시오.
5. **마스터 서비스 계약을 읽었습니다...** 선택란을 클릭하고 **주문하기**를 클릭하십시오.
6. 몇 분 후 새 스토리지 할당이 사용 가능해집니다.

또는 SLCLI에서 다음 명령을 사용할 수 있습니다.
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
