---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-12"

---
{:new_window: target="_blank"}

# IOPS 조정

{{site.data.keyword.filestorage_full}} 스토리지 사용자는 이 새 기능을 사용하여 기존 {{site.data.keyword.filestorage_short}}의 IOPS를 즉시 조정할 수 있습니다. 복제를 작성하거나 데이터를 새 스토리지로 수동으로 복사할 필요가 없습니다. 조정이 진행되는 동안 사용자에게는 어떤 유형의 가동 중단 또는 액세스 불가능 상황도 발생하지 않습니다.

스토리지에 대한 청구는 새 가격의 비례 배분된 차이가 현재 청구 주기에 추가되도록 업데이트됩니다. 다음 청구 주기에는 전체 새 금액이 청구됩니다.


## 조정 가능한 IOPS의 장점

- 비용 관리 – 일부 클라이언트가 최대 사용 시간 중에 단지 높은 IOPS만 요구할 수 있습니다. 예를 들어, 특정 대형 소매점에서는 겨울 연휴에 사용량이 가장 많으며, 이 기간에는 스토리지에 대해 한여름보다 더 높은 IOPS를 필요로 할 수 있습니다. 이 기능은 이러한 소매점에서 비용을 관리하면서 필요할 때 더 높은 IOPS를 위해 비용을 지불할 수 있게 해 줍니다.

## 제한사항

이 기능은 [특정 데이터 센터](new-ibm-block-and-file-storage-location-and-features.html)에서만 사용 가능합니다.

클라이언트는 자체 IOPS를 조정할 때 Endurance와 Performance 간에 전환할 수 없습니다. 사용자는 다음의 기준/제한사항을 기반으로 스토리지에 대해 새 IOPS 계층 또는 IOPS 레벨을 지정할 수 있습니다.

- 원래 볼륨이 Endurance 0.25 계층인 경우에는 IOPS 계층을 업데이트할 수 없습니다.
- 원래 볼륨이 0.30IOPS/GB 이하의 Performance인 경우, 사용 가능한 옵션에는 결과가 0.30IOPS/GB 이하인 크기 및 IOPS 조합만 포함됩니다. 
- 원래 볼륨이 0.30IOPS/GB를 초과하는 Performance인 경우, 사용 가능한 옵션에는 결과가 0.30IOPS/GB를 초과하는 크기 및 IOPS 조합만 포함됩니다. 

## IOPS 조정이 복제에 미치는 영향

볼륨에 복제가 있는 경우 복제본은 기본 볼륨의 IOPS 선택사항과 일치하도록 자동으로 업데이트됩니다.

## 스토리지의 IOPS 조정

1. {{site.data.keyword.filestorage_short}}의 목록으로 이동하십시오.
    - 고객 포털에서 **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하거나
    - {{site.data.keyword.BluSoftlayer_full}} 카탈로그에서 **인프라** > **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하십시오.
2. 목록에서 볼륨을 선택하고 **조치** > **볼륨 수정**을 클릭하십시오.
3. **스토리지 IOPS 옵션** 아래에서 새로 선택하십시오.
    - Endurance(계층 IOPS): 스토리지의 0.25IOPS/GB보다 큰 IOPS 계층을 선택하십시오. 언제든지 IOPS 계층을 늘릴 수 있습니다. 그러나 줄이기는 한 달에 한 번만 가능합니다.
    - Performance(할당된 IOPS): 100 - 48,000IOPS 범위의 값을 입력하여 스토리지에 대한 새 IOPS 옵션을 지정하십시오. (주문 양식에서 크기별로 필요한 특정 경계를 반드시 살펴보십시오.)
4. 선택사항과 새 가격을 검토하십시오.
5. **마스터 서비스 계약을 읽었습니다...** 선택란을 클릭하고 **주문하기**를 클릭하십시오.
6. 몇 분 후 새 스토리지 할당이 사용 가능해집니다.
