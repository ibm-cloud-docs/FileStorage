---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-18"

keywords: File Storage, file storage, NFS, provisioning, ordering,

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# 콘솔을 통해 {{site.data.keyword.filestorage_short}} 주문
{: #orderingConsole}

{{site.data.keyword.filestorage_short}}를 프로비저닝하고 자신의 용량 및 IOPS 요구사항에 맞게 이를 상세 조정할 수 있습니다. 성능을 지정하기 위한 두 가지 옵션을 사용하여 스토리지를 최대한으로 활용하십시오.

- 적절히 정의된 성능 요구사항이 없는 워크로드를 처리할 수 있도록 사전 정의된 성능 레벨을 제공하는 Endurance IOPS 계층을 선택할 수 있습니다.
- Performance를 사용하여 총 IOPS 수를 지정함으로써 구체적인 성능 요구사항을 만족시키도록 스토리지를 상세 조정할 수 있습니다.

## 사전 정의된 IOPS 계층을 사용하는 {{site.data.keyword.filestorage_short}} 주문(Endurance)
{: #endurance}

1. [{{site.data.keyword.cloud}} 카탈로그](https://{DomainName}/catalog){: external}에 로그인하고 **스토리지**를 클릭하십시오. 그리고 {{site.data.keyword.filestorage_short}}를 선택하십시오. **작성**을 클릭하십시오.
2. 배치 **위치**(데이터 센터)를 선택하십시오.
   - 새 스토리지가 컴퓨팅 호스트 또는 보유한 호스트와 동일한 위치에 추가되었는지 확인하십시오.
3. 비용 청구 방식을 선택하십시오. 개선된 기능의 데이터 센터(*로 표기됨)를 선택한 경우 월별 또는 시간별 청구 중에 선택할 수 있습니다.
     1. **시간별** 청구에서 파일 볼륨이 계정에 존재한 시간은 볼륨이 삭제된 시점이나 청구 주기가 끝난 시점에 계산됩니다. 둘 중 먼저 발생하는 시점이 사용됩니다. 시간별 스토리지는 수 일간 또는 한달 이내에 사용되는 스토리지에 좋은 선택사항입니다. 시간별 청구는 [데이터 센터 선택](/docs/infrastructure/FileStorage?topic=FileStorage-selectDC)에서 프로비저닝된 스토리지에만 사용 가능합니다.
     2. **월별** 청구에서, 가격에 대한 계산은 작성 날짜로부터 청구 주기의 끝까지 비례 배분되며 즉각적으로 청구됩니다. 청구 주기가 끝나기 전에 파일 볼륨이 삭제되는 경우에는 환불이 되지 않습니다. 월별 청구는 장기간(한달 이상) 저장하고 액세스해야 하는 데이터를 사용하는 프로덕션 워크로드에서 사용되는 스토리지에 대해 좋은 선택사항입니다.

     월별 비용 청구 유형은 개선된 기능으로 업데이트되지 **않은** 데이터 센터에서 프로비저닝된 스토리지에 기본적으로 사용됩니다.
     {:note}
4. **새 스토리지 크기** 필드에 스토리지 크기를 입력하십시오.
5. **스토리지 IOPS 옵션** 섹션에서 **Endurance(계층 IOPS)**를 선택하십시오.
6. 애플리케이션에서 필요로 하는 IOPS 계층을 선택하십시오.
    - **0.25IOPS/GB**는 I/O 집약도가 낮은 워크로드용으로 설계되었습니다. 이러한 워크로드는 일반적으로 대부분의 시간 동안 비활성 상태인 데이터를 많이 보유하는 것이 특성입니다. 적용되는 예에는 메일함 또는 부서 레벨 파일 공유의 저장이 포함되어 있습니다.
    - **2IOPS/GB**는 가장 일반적인 사용 용도로 설계되었습니다. 적용 예에는 하이퍼바이저용 가상 머신 디스크 이미지, 또는 웹 애플리케이션을 지원하는 소형 데이터베이스의 호스팅 등이 있습니다.
    - **4IOPS/GB**는 집약도가 높은 워크로드용으로 설계되었습니다. 이러한 워크로드는 일반적으로 대부분의 시간 동안 활성 상태인 데이터를 많이 보유하는 것이 특성입니다. 적용되는 예에는 트랜잭션 및 기타 성능에 민감한 데이터베이스가 포함되어 있습니다.
    - **10IOPS/GB**는 가장 수요가 많은 워크로드(예: NoSQL 데이터베이스에서 생성된 워크로드) 및 분석을 위한 데이터 처리용으로 설계되었습니다. 이 계층은 [데이터 센터 선택](/docs/infrastructure/FileStorage?topic=FileStorage-selectDC)에서 최대 4TB 크기로 프로비저닝된 스토리지에 대해 사용 가능합니다.
7. **스냅샷 영역 크기 지정**을 클릭하고 목록에서 스냅샷 크기를 선택하십시오. 이 영역은 사용 가능한 영역에 추가됩니다. 스냅샷 영역 고려사항 및 권장사항을 알아보려면 [스냅샷 주문](/docs/infrastructure/FileStorage?topic=FileStorage-ordering-snapshots)을 읽으십시오.
8. 오른쪽에서 주문 요약을 검토하고 프로모션 코드를 적용하십시오(이를 보유 중인 경우).

   주문을 처리할 때 할인이 적용됩니다.
   {:note}
9. 이용 약관을 검토한 후에 **서드파티 서비스 계약서를 읽었으며 이에 동의합니다** 상자를 선택하십시오.
10. **작성**을 클릭하십시오. 몇 분 후 새 스토리지 할당이 사용 가능해집니다.

기본적으로, 250개 {{site.data.keyword.blockstorageshort}} 볼륨의 통합 총계를 프로비저닝할 수 있습니다. 볼륨 수를 늘리려면 영업 담당자에게 문의하십시오. [여기서](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits) 한계 늘리기에 대해 읽으십시오.<br/><br/>동시 권한 부여의 한계에 대한 자세한 정보는 [FAQ](/docs/infrastructure/FileStorage?topic=file-storage-faqs#how-many-instances-can-share-the-use-of-a-provisioned-file-storage-volume-)를 참조하십시오.
{:tip}

## 사용자 정의 IOPS를 사용하는 {{site.data.keyword.filestorage_short}} 주문(Performance)
{: #performance}

1. [{{site.data.keyword.cloud}} 카탈로그](https://{DomainName}/catalog){: external}에 로그인하고 **스토리지**를 클릭하십시오. 그리고 {{site.data.keyword.filestorage_short}}를 선택하십시오. **작성**을 클릭하십시오.
2. **위치**를 클릭하고 데이터 센터를 선택하십시오.
   - 새 스토리지가 컴퓨팅 호스트 또는 보유한 호스트와 동일한 위치에 추가되었는지 확인하십시오.
3. 비용 청구 방식을 선택하십시오. 개선된 기능의 데이터 센터(*로 표기됨)를 선택한 경우 월별 또는 시간별 청구 중에 선택할 수 있습니다.
     1. **시간별** 청구에서 파일 볼륨이 계정에 존재한 시간은 볼륨이 삭제된 시점이나 청구 주기가 끝난 시점에 계산됩니다. 둘 중 먼저 발생하는 시점이 사용됩니다. 시간별 스토리지는 수 일간 또는 한달 이내에 사용되는 스토리지에 좋은 선택사항입니다. 시간별 청구는 [데이터 센터 선택](/docs/infrastructure/FileStorage?topic=FileStorage-selectDC)에서 프로비저닝된 스토리지에만 사용 가능합니다.
     2. **월별** 청구에서, 가격에 대한 계산은 작성 날짜로부터 청구 주기의 끝까지 비례 배분되며 즉각적으로 청구됩니다. 청구 주기가 끝나기 전에 파일 볼륨이 삭제되는 경우에는 환불이 되지 않습니다. 월별 청구는 장기간(한달 이상) 저장하고 액세스해야 하는 데이터를 사용하는 프로덕션 워크로드에서 사용되는 스토리지에 대해 좋은 선택사항입니다.

     월별 비용 청구 유형은 개선된 기능으로 업데이트되지 **않은** 데이터 센터에서 프로비저닝된 스토리지에 기본적으로 사용됩니다.
     {:note}
4. **새 스토리지 크기** 필드에 스토리지 크기를 입력하십시오.
5. **스토리지 IOPS 옵션** 섹션에서 **Performance(할당된 IOPS)**를 선택하십시오.
6. **Performance(할당된 IOPS)** 필드에 IOPS를 입력하십시오.
7. **스냅샷 영역 크기 지정**을 클릭하고 목록에서 스냅샷 크기를 선택하십시오. 이 영역은 사용 가능한 영역에 추가됩니다. 스냅샷 영역 고려사항 및 권장사항을 알아보려면 [스냅샷 주문](/docs/infrastructure/FileStorage?topic=FileStorage-ordering-snapshots)을 읽으십시오.
8. 오른쪽에서 주문 요약을 검토하고 프로모션 코드를 적용하십시오(이를 보유 중인 경우).

   주문을 처리할 때 할인이 적용됩니다.
   {:note}
9. 이용 약관을 검토한 후에 **서드파티 서비스 계약서를 읽었으며 이에 동의합니다** 상자를 선택하십시오.
10. **작성**을 클릭하십시오. 몇 분 후 새 스토리지 할당이 사용 가능해집니다.

기본적으로, 250개 {{site.data.keyword.blockstorageshort}} 볼륨의 통합 총계를 프로비저닝할 수 있습니다. 볼륨 수를 늘리려면 영업 담당자에게 문의하십시오. [여기서](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits) 한계 늘리기에 대해 읽으십시오.<br/><br/>동시 권한 부여의 한계에 대한 자세한 정보는 [FAQ](/docs/infrastructure/FileStorage?topic=file-storage-faqs#how-many-instances-can-share-the-use-of-a-provisioned-file-storage-volume-)를 참조하십시오.
{:important}


## 새 스토리지 연결
{: #mountingvolumesPortal}

프로비저닝 요청이 완료되면 새 스토리지에 액세스할 수 있도록 호스트에 권한을 부여하고 연결을 구성하십시오. 호스트의 운영 체제에 따라 적절한 링크를 사용하십시오.
- [Linux의 {{site.data.keyword.filestorage_short}} 마운트](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [CentOS의 {{site.data.keyword.filestorage_short}} 마운트](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [CoreOS의 {{site.data.keyword.filestorage_short}} 마운트](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)
- [cPanel로 백업을 위한 {{site.data.keyword.filestorage_short}} 구성](/docs/infrastructure/FileStorage?topic=FileStorage-cPanelBackups)
- [Plesk로 백업을 위한 {{site.data.keyword.filestorage_short}} 구성](/docs/infrastructure/FileStorage?topic=FileStorage-PleskBackup)
- [ESXi 호스트에 {{site.data.keyword.filestorage_short}} 볼륨 마운트](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)

## 재해 복구 고려사항

데이터 손실을 방지하고 비즈니스 연속성을 보장하려면 다른 데이터 센터에 서버와 스토리지를 복제하십시오. 복제를 수행하면 스냅샷 스케줄에 따라 서로 다른 두 위치에 동기화된 데이터를 보관합니다. 자세한 정보는 [데이터 복제](/docs/infrastructure/FileStorage?topic=FileStorage-replication)를 참조하십시오.

볼륨을 복제한 후에 이를 원래 볼륨과 독립적으로 사용하려면 [중복 블록 볼륨 작성](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume)을 참조하십시오.

## 청구서에서 {{site.data.keyword.filestorage_short}} 볼륨 식별

모든 파일 공유는 청구서에 품목명으로 표시됩니다. Endurance 볼륨은 “Endurance 스토리지 서비스”로 표시되고 Performance 볼륨은 "Performance 스토리지 서비스"로 표시됩니다. 요금은 스토리지 레벨에 따라 달라집니다. Endurance 또는 Performance를 펼쳐 해당 볼륨이 {{site.data.keyword.filestorage_short}} 볼륨인지 알아볼 수 있습니다.
