---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-23"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# 조정 가능한 IOPS

이 새로운 기능을 사용하여 {{site.data.keyword.filestorage_full}} 스토리지 사용자는 복제를 작성하거나 수동으로 데이터를 새 스토리지로 마이그레이션하지 않고도 언제든지 기존 {{site.data.keyword.filestorage_short}}의 IOPS를 조정할 수 있습니다. 조정이 진행 중인 동안에는 스토리지에 대한 어떤 종류의 액세스 중단이나 차단도 사용자에게 발생하지 않습니다.  

스토리지에 대한 청구는 새 가격의 비례 배분된 차이가 현재 청구 주기에 추가되도록 업데이트된 후에 차기 청구 주기에서 전체 신규 용량이 청구됩니다. 

이 기능은 [엄선된 데이터 센터](new-ibm-block-and-file-storage-location-and-features.html)에서만 사용 가능합니다.  

## 조정 가능한 IOPS를 활용하는 이유는 무엇입니까?

- 비용 관리 – 일부 클라이언트가 최대 사용 시간 중에 단지 높은 IOPS만 요구할 수 있습니다. 예를 들어, 대형 소매점에서 휴일에 최대 사용량이 발생할 수 있으며 한여름의 경우보다 자체 스토리지에 더 높은 IOPS가 필요할 수 있습니다. 이 기능을 이용하면 자체 비용을 관리하면서 실제로 필요할 때만 더 높은 IOPS 비용을 지불할 수 있습니다. 

## 제한사항이 있습니까?

이 기능은 고급 기능이 있는 [데이터 센터](new-ibm-block-and-file-storage-location-and-features.html)에서 프로비저닝된 스토리지에만 사용 가능합니다. 

클라이언트는 자체 IOPS를 조정할 때 Endurance과 Performance 간에 전환할 수 없습니다. 사용자는 다음의 기준/제한사항을 기반으로 자체 스토리지에 대해 새 IOPS 계층 또는 IOPS 레벨을 지정할 수 있습니다.  

- 원래 볼륨이 Endurance 0.25 계층인 경우에는 IOPS 계층을 업데이트할 수 없습니다. 
- 원래 볼륨이 < 0.30 IOPS/GB의 Performance인 경우, 사용 가능한 옵션에는 결과가 < 0.30 IOPS/GB인 크기와 iops 조합만 포함되어야 합니다.  
- 원래 볼륨이 >= 0.30 IOPS/GB의 Performance인 경우, 사용 가능한 옵션에는 결과가 >= 0.30 IOPS/GB인 크기와 iops 조합만 포함되어야 합니다. 크기(원래 볼륨 이상)

## IOPS 조정으로 복제에 어떤 영향이 있습니까? 

볼륨에 복제가 준비된 경우, 복제본은 기본의 IOPS 선택사항과 일치하도록 자동으로 업데이트됩니다.  

## 내 스토리지에서 IOPS를 조정하는 방법은 무엇입니까? 

1. {{site.data.keyword.slportal}}에서 **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하거나 {{site.data.keyword.BluSoftlayer_full}} 카탈로그에서 **인프라** > **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하십시오. 
2. 목록에서 볼륨을 선택하고 **조치** > **볼륨 수정**을 클릭하십시오. 
3. **스토리지 IOPS 옵션** 아래에서 새로 선택하십시오. 
    - Endurance(계층 IOPS): 스토리지의 0.25 IOPS/GB보다 큰 IOPS 계층을 선택하십시오. 언제든지 IOPS 계층을 늘릴 수 있습니다. 그러나 줄이기는 한 달에 한 번만 가능합니다. 
    - Performance(할당된 IOPS): 100 - 48,000 IOPS 범위의 값을 입력하여 스토리지에 대한 새 IOPS 옵션을 지정하십시오. (주문 양식에서 크기별로 필요한 특정 경계를 반드시 살펴보십시오.) 
4. 선택사항과 새 가격 책정을 검토하십시오. 
5. **마스터 서비스 계약을 읽었습니다...** 선택란을 클릭하고 **주문하기**를 클릭하십시오. 
6. 새 스토리지 할당이 수 분 후에 사용 가능해야 합니다. 
