---

copyright:
  years: 2014, 2019
lastupdated: "2019-01-07"

---
{:tip: .tip}
{:note: .note}
{:important: .important}

# 제공자 관리 저장 시 암호화

{{site.data.keyword.BluSoftlayer_full}}에서는 보안에 대한 필요성을 심각하게 받아들이며 데이터의 보안을 유지하기 위한 데이터 암호화의 중요성을 이해하고 있습니다. 제공자 관리 암호화를 사용하면 Endurance또는 Performance 옵션이 프로비저닝된 {{site.data.keyword.filestorage_full}}가 추가 비용 없이 성능에 영향을 주지 않고 기본적으로 암호화됩니다.

제공자 관리 저장 암호화 기능은 다음의 산업 표준 프로토콜을 사용합니다.

* 산업 표준 AES-256 암호화
* 키는 산업 표준 KMIP(Key Management Interoperability Protocol)를 사용하여 사내에서 관리됨
* 스토리지는 다음 표준에 대해 유효성 검증됩니다.
    - FIPS(Federal Information Processing Standard) Publication 140-2,
    - FISMA(Federal Information Security Management Act),
    - HIPAA(Health Insurance Portability and Accountability Act),
    - PCI(Payment Card Industry),
    - Basel II,
    - California Security Breach Information Act(SB 1386) 및
    - EU Data Protection Directive (95/46/EC) 준수.

## 스냅샷 또는 복제된 스토리지의 보안  

암호화된 파일 스토리지의 모든 스냅샷 및 복제본도 기본적으로 암호화됩니다. 이 기능은 볼륨 기반으로 끄기가 불가능합니다.

## 암호화로 스토리지 프로비저닝

특정 데이터 센터에서는 제공자 관리 저장 암호화 기능이 사용 가능합니다. 이러한 데이터 센터에서 주문된 모든 스토리지는 저장 데이터의 암호화로 자동 프로비저닝됩니다. [여기](new-ibm-block-and-file-storage-location-and-features.html)를 클릭하면 {{site.data.keyword.filestorage_short}} 암호화가 사용 가능한 데이터 센터의 현재 목록을 볼 수 있습니다.

{{site.data.keyword.filestorage_short}}를 주문할 때는 별표(`*`)가 표시된 데이터 센터를 선택하십시오. LUN/볼륨 이름 필드 오른쪽에서 볼륨이 암호화되었음을 나타내는 잠금 아이콘을 볼 수 있습니다. 그림 1을 참조하십시오.

![LUN이 암호화되었음을 표시하는 잠금 아이콘](/images/encryptedstorage.png)
<caption>그림 1. 볼륨이 암호화되었음을 표시하는 잠금 아이콘의 예</caption>

데이터 센터 업그레이드 이전에 프로비저닝된 암호화되지 않은 스토리지는 자동으로 암호화되지 **않습니다**. 업그레이드된 데이터 센터에 암호화되지 않은 스토리지를 보유하고 있으며 이를 암호화하려는 경우에는 새 볼륨을 작성하고 데이터를 이동해야 합니다. 자세한 정보는 [업그레이드된 데이터 센터에서 파일 스토리지 마이그레이션](migrate-file-storage-encrypted-file-storage.html)을 참조하십시오.
{:important}
