---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-23"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# 조정 가능한 IOPS

이 새 기능을 통해 {{site.data.keyword.filestorage_full}} 스토리지 사용자는 상황에 따라 복제를 작성하거나 수동으로 데이터를 새 스토리지로 마이그레이션하지 않고도 기존 {{site.data.keyword.filestorage_short}}의 IOPS를 조정할 수 있습니다. 조정하는 동안 스토리지에 대한 액세스가 제한되거나 중단되지 않습니다.  

스토리지에 대한 청구는 현재 청구 주기에는 비례 배분된 새 가격의 차이만큼 추가하도록 업데이트된 후 다음 청구 주기에 완전히 새 금액으로 청구됩니다. 

이 기능은 [엄선된 데이터 센터](new-ibm-block-and-file-storage-location-and-features.html)에서만 사용할 수 있습니다.  

## 조정 가능한 IOPS를 활용하는 이유는 무엇입니까?

- 비용 관리 - 일부 클라이언트는 최대 사용 시간 중에만 높은 IOPS가 필요할 수 있습니다. 예를 들어, 한 대형 소매점에서 휴일이 최대 사용 시간인 경우 한 여름에 비해 스토리지에 더 높은 IOPS가 필요할 수 있습니다. 이 기능을 사용하면 실제로 필요한 경우에만 더 높은 IOPS를 지불하면서 비용을 관리할 수 있습니다. 

## 제한사항이 있습니까?

이 기능은 향상된 기능으로 [데이터 센터](new-ibm-block-and-file-storage-location-and-features.html)에서 프로비저닝된 스토리지에서만 사용 가능합니다. 

클라이언트는 IOPS를 조정할 때 Endurance와 Performance 사이에서 전환할 수 없습니다. 사용자는 다음 기준/제한사항에 따라 스토리지에 대한 새 IOPS 티어 또는 IOPS 레벨을 지정할 수 있습니다.  

- 원래 볼륨이 Endurance 0.25 티어인 경우 IOPS 티어를 업데이트할 수 없습니다. 
- 원래 볼륨이 < 0.30 IOPS/GB의 Performance인 경우 사용 가능한 옵션으로는 크기 및 IOPS 조합만 포함해야 합니다(결과적으로 < 0.30 IOPS/GB). 
- 원래 볼륨이 >= 0.30 IOPS/GB의 Performance인 경우 사용 가능한 옵션으로는 크기 및 IOPS 조합만 포함해야 합니다(결과적으로 >= 0.30 IOPS/GB). 크기(원래 볼륨 이상)

## IOPS 조정이 복제에 어떤 영향을 줍니까?

볼륨에 복제본이 있으면 복제본은 기본의 IOPS 선택사항과 일치하도록 자동으로 업데이트됩니다.  

## 내 스토리지에서는 IOPS를 어떻게 조정할 수 있습니까?

1. {{site.data.keyword.slportal}}에서 **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하거나 {{site.data.keyword.BluSoftlayer_full}} 카탈로그에서 **인프라** > **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하십시오. 
2. 목록에서 볼륨을 선택하고 **조치** > **볼륨 수정**을 클릭하십시오. 
3. **스토리지 IOPS 옵션**에서 새로 선택하십시오. 
    - Endurance(티어별 IOPS): 스토리지의 0.25 IOPS/GB보다 큰 IOPS 티어를 선택하십시오. 언제라도 IOPS 티어를 늘릴 수 있습니다. 하지만 줄이는 것은 한 달에 한 번만 사용 가능합니다. 
    - Performance(할당된 IOPS): 100 - 48,000 사이의 IOPS 값을 입력하여 스토리지에 대한 새 IOPS 옵션을 지정하십시오. (주문 양식에서 크기별로 필요한 특정 경계를 확인하십시오.) 
4. 선택사항 및 새 가격 책정을 검토하십시오. 
5. **마스터 서비스 계약을 읽었습니다...** 선택란을 클릭하고 **주문하기**를 클릭하십시오. 
6. 새 스토리지 할당은 몇 분 안에 사용 가능해야 합니다. 
