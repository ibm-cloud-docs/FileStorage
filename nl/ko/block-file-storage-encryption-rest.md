---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-08"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# 데이터 보안 - 제공자가 관리하는 휴면 중 암호화 

{{site.data.keyword.BluSoftlayer_full}}에서는 보안의 필요성을 충분히 인지하고 있으며, 데이터를 안전하게 지키기 위해 데이터 암호화 기능의 중요성을 이해하고 있습니다. 제공자가 관리하는 암호화를 통해 Endurance 또는 Performance로 프로비저닝된 {{site.data.keyword.filestorage_full}}는 추가 비용 없이, 성능에 영향을 주지 않고 기본적으로 암호화됩니다. 

제공자가 관리하는 휴면 중 암호화 기능은 다음과 같은 업계 표준 프로토콜을 사용합니다. 

* 업계 표준 AES-256 암호화
* 키는 업계 표준 KMIP(Key Management Improbability Protocol)를 통해 사내에서 관리됨
* 스토리지는 FISMA(Federal Information Security Management Act)에서 검증된 FIPS(Federal Information Processing Standard) 공개 140-2, HIPAA(Health Insurance Portability and Accountability Act), PCI(Payment Card Industry), Basel II, 캘리포니아 보안 침해법(SB 1386), EU 데이터 보호 명령 95/46/EC를 준수함

## 스냅샷 또는 복제된 스토리지에 대한 휴면 중 암호화  

암호화된 파일 스토리지의 모든 스냅샷 및 복제본도 기본적으로 암호화됩니다. 이 기능은 볼륨 기반으로 끌 수 없습니다. 

## 암호화를 통해 스토리지 프로비저닝

제공자가 관리하는 휴면 중 암호화 기능은 정기적으로 추가될 새 데이터 센터 가용성을 통해 엄선된 데이터 센터에서 프로비저닝되는 {{site.data.keyword.filestorage_short}}에서만 사용 가능합니다. 해당 데이터 센터에서 프로비저닝되는 모든 스토리지는 저장 데이터에 대한 암호화를 사용하여 프로비저닝됩니다. 저장 데이터에 대해 {{site.data.keyword.filestorage_short}} 암호화를 사용할 수 있는 현재 데이터 센터 목록을 보려면 [여기](new-ibm-block-and-file-storage-location-and-features.html)를 클릭하십시오. 


{{site.data.keyword.filestorage_short}} 주문 시 별표(`*`) 및 암호화가 사용 가능함을 알리는 메시지로 표시된 데이터 센터를 선택하십시오. LUN/볼륨 이름 필드 오른쪽에 암호화되었음을 알리는 잠금 아이콘이 표시됩니다. 그림 1을 참조하십시오. 

![잠금 아이콘은 LUN이 암호화되어 있음을 나타냅니다.](/images/encryptedstorage.png)
<caption>그림 1. 볼륨이 암호화되었음을 나타내는 잠금 아이콘 예.</caption>



**참고** 데이터 센터 업그레이드 전에 프로비저닝된 암호화되지 않은 스토리지는 자동으로 암호화되지 **않습니다**. 업그레이드된 데이터 센터에 암호화되지 않은 스토리지가 있으면 새 볼륨을 작성하고 데이터 마이그레이션을 수행해야 합니다. 다음 문서에서 안내를 제공할 수 있습니다. 

* [업그레이드된 데이터 센터에서 파일 스토리지 마이그레이션](migrate-file-storage-encrypted-file-storage.html)
