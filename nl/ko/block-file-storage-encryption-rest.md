---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-23"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# 데이터 보호 - 제공자 관리 저장 암호화(Encryption-At-Rest) 

{{site.data.keyword.BluSoftlayer_full}}에서는 보안에 대한 필요성을 진심으로 느끼며 데이터의 보안을 유지하기 위한 데이터 암호화 가능성의 중요성을 이해하고 있습니다. 제공자 관리 암호화를 사용하여, Endurance또는 Performance이 프로비저닝된 {{site.data.keyword.filestorage_full}}는 추가 비용 없이 성능에 영향을 주지 않고 기본적으로 암호화됩니다. 

제공자 관리 저장 암호화 기능은 다음의 산업 표준 프로토콜을 사용합니다. 

* 산업 표준 AES-256 암호화
* 키는 산업 표준 KMIP(Key Management Interoperability Protocol)를 사용하여 사내에서 관리됨 
* 스토리지는 FISMA(Federal Information Security Management Act), HIPAA(Health Insurance Portability and Accountability Act), PCI(Payment Card Industry), Basel II, California Security Breach Information Act(SB 1386) 및 EU Data Protection Directive 95/46/EC 준수를 위해 FIPS(Federal Information Processing Standard) Publication 140-2 유효성 검증을 거침 

## 스냅샷 또는 복제된 스토리지에 대한 저장 암호화  

암호화된 파일 스토리지의 모든 스냅샷 및 복제본도 기본적으로 암호화됩니다. 이 기능은 볼륨 기반으로 끄기가 불가능합니다. 

## 암호화로 스토리지 프로비저닝

제공자 관리 저장 암호화 기능은 정기적으로 추가되는 새 데이터 센터 가용성으로 엄선된 데이터 센터에서 프로비저닝된 {{site.data.keyword.filestorage_short}}에만 사용 가능합니다. 이러한 데이터 센터에서 프로비저닝된 모든 스토리지는 저장 데이터의 암호화로 자동 프로비저닝됩니다. [여기](new-ibm-block-and-file-storage-location-and-features.html)를 클릭하면 저장 데이터의 {{site.data.keyword.filestorage_short}} 암호화가 사용 가능한 데이터 센터의 현재 목록을 볼 수 있습니다. 


{{site.data.keyword.filestorage_short}}를 주문할 때는 암호화가 사용 가능함을 기술하는 메시지와 별표(`*`)가 표시된 데이터 센터를 선택하십시오. LUN/볼륨 이름 필드 오른쪽에 암호화되었음을 표시하는 잠금 아이콘이 나타납니다. 그림 1을 참조하십시오. 

![LUN이 암호화되었음을 표시하는 잠금 아이콘](/images/encryptedstorage.png)
<caption>그림 1. 볼륨이 암호화되었음을 표시하는 잠금 아이콘의 예</caption>



**참고로** 데이터 센터 업그레이드에 앞서 프로비저닝된 암호화되지 않은 스토리지는 자동으로 암호화되지 **않습니다**. 업그레이드된 데이터 센터에 암호화되지 않은 스토리지가 있는 경우에는 새 볼륨을 작성하고 데이터 마이그레이션을 수행해야 합니다. 다음 기사에서 지침을 제공할 수 있습니다. 

* [업그레이드된 데이터 센터의 파일 스토리지 마이그레이션](migrate-file-storage-encrypted-file-storage.html)
