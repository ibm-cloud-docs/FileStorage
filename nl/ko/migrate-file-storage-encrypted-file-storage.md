---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
 
# 암호화된 {{site.data.keyword.filestorage_short}}로 {{site.data.keyword.filestorage_short}} 마이그레이션

Endurance 또는 Performance의 암호화된 {{site.data.keyword.filestorage_full}}가 엄선된 데이터 센터에서 출시되었습니다. 아래에는 암호화되지 않은 {{site.data.keyword.filestorage_short}}에서 암호화된 File Storage로 마이그레이션하는 방법에 대한 정보가 있습니다. 제공자 관리 암호화된 스토리지에 대한 자세한 정보를 보려면 [{{site.data.keyword.filestorage_short}} 저장 암호화](block-file-storage-encryption-rest.html) 기사를 읽으십시오. 사용 가능한 기능 및 업그레이드된 데이터 센터의 목록을 보려면 [여기](new-ibm-block-and-file-storage-location-and-features)를 클릭하십시오. 

선호하는 마이그레이션 경로는 두 볼륨을 동시에 연결하고 하나의 파일 볼륨에서 다른 파일 볼륨으로 직접 데이터를 전송하는 것입니다. 세부 사항은 사용자의 운영 체제와 복사 오퍼레이션 중에 데이터 변경이 예상되는지 여부에 따라 다릅니다. 

보다 일반적인 시나리오가 사용자의 편의를 위해 개략적으로 설명되어 있습니다. 사용자가 암호화되지 않은 파일 볼륨을 이미 호스트에 연결했다고 가정합니다. 그렇지 않은 경우, 이 태스크를 완료하려면 실행 중인 운영 체제에 가장 적합한 아래의 지시사항을 따르십시오.  

**참고:** 암호화된 모든 {{site.data.keyword.filestorage_short}} 볼륨에는 암호화되지 않은 볼륨과는 다른 마운트 지점이 있습니다. 암호화 및 암호화되지 않은 {{site.data.keyword.filestorage_short}} 볼륨 모두에 대해 올바른 마운트 지점을 사용 중임을 확인하기 위해 UI의 **볼륨 세부사항** 페이지에서 마운트 지점 정보를 볼 수 있음은 물론, API 호출 SoftLayer_Network_Storage::getNetworkMountAddress()를 통해 올바른 마운트 지점에 액세스할 수도 있습니다. 

[Linux의 {{site.data.keyword.filestorage_short}} 액세스](accessing-file-storage-linux.html)

## 암호화된 파일 볼륨 작성

다음 단계를 사용하여 마이그레이션 프로세스의 원활한 진행을 위해 암호화된 동일 크기 이상의 볼륨을 작성할 수 있습니다. 

### 암호화된 Endurance 스토리지 볼륨 주문

1. [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 홈 페이지에서 **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하거나 {{site.data.keyword.BluSoftlayer_full}} 카탈로그에서 **인프라** > **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하십시오. 

2. {{site.data.keyword.filestorage_short}} 페이지에서 **{{site.data.keyword.filestorage_short}} 주문** 링크를 클릭하십시오. 

3. **Endurance**를 선택하십시오. 

4. 원래 볼륨이 있는 데이터 센터를 선택하십시오. 참고로, 암호화는 별표가 있는 데이터 센터에서만 사용 가능합니다. 

5. 원하는 **IOPS 계층**을 입력하십시오. 

6. 원하는 양의 스토리지 영역(GB)을 선택하십시오. TB의 경우, 1TB는 1,000GB이며 12TB는 12,000GB입니다. 

7. 스냅샷에 대해 원하는 양의 스토리지 영역(GB)을 입력하십시오. 

8. 드롭 다운 목록에서 **VMware OS**를 선택하십시오. 

9. 주문을 제출하십시오. 
 
### 암호화된 Performance 스토리지 볼륨 주문

1. [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 홈 페이지에서 **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하거나 {{site.data.keyword.BluSoftlayer_full}} 카탈로그에서 **인프라** > **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하십시오. 

2. **{{site.data.keyword.filestorage_short}} 주문**을 클릭하십시오. 

3. **성능**을 선택하십시오. 

4. 원래 볼륨이 있는 데이터 센터를 선택하십시오. 참고로, 암호화는 별표(`*`)가 있는 데이터 센터에서만 사용 가능합니다. 

5. 원래 볼륨 크기 이상의 크기로 원하는 스토리지 영역의 양(GB)을 선택하십시오. 

6. 원하는 양의 IOPS를 100 간격으로 입력하십시오. 

7. 드롭 다운 목록에서 VMware OS를 선택하십시오. 

8. 주문을 제출하십시오. 

스토리지는 1분 내에 프로비저닝되며 고객 포털의 {{site.data.keyword.filestorage_short}} 페이지에 나타납니다. 

 
## 새 볼륨을 호스트에 연결

“권한 부여된” 호스트는 볼륨에 대한 액세스 권한이 제공된 호스트입니다. 호스트 권한이 없으면 사용자가 시스템에서 스토리지에 액세스하거나 이를 사용할 수 없습니다. 

1. 암호화된 **볼륨 이름**을 클릭하십시오. 

2. 페이지의 **권한 부여된 호스트** 섹션으로 화면을 이동하십시오. 

3. 페이지 오른쪽에서 **호스트 권한 부여** 링크를 클릭하십시오. 볼륨에 액세스할 수 있는 호스트를 선택하십시오. 

일단 권한이 부여되면 볼륨을 호스트에 연결하십시오. 

 
## 스냅샷 및 복제

원래 볼륨에 대해 스냅샷 및 복제가 설정되어 있습니까? 설정된 경우에는 복제, 스냅샷 영역을 설정하고 원래 볼륨과 동일한 설정으로 새 암호화된 볼륨에 대한 스냅샷 스케줄을 작성해야 합니다.  

참고로, 대상 데이터 센터가 암호화를 위해 업그레이드되지 않은 경우에는 데이터 센터가 업그레이드될 때까지 새 볼륨에 대한 복제를 설정할 수 없습니다. 

 
## 데이터 마이그레이션

호스트를 원래 및 암호화된 {{site.data.keyword.filestorage_short}} 볼륨 모두에 연결해야 합니다. 그렇지 않은 경우에는 다음을 수행하십시오. 

• 위의 단계와 참조된 문서를 제대로 따랐는지 확인하십시오. 

• 두 볼륨을 호스트에 연결할 때 추가 지원을 받으려면 지원 티켓을 여십시오. 

### 데이터 고려사항

이 시점에서, 원래 {{site.data.keyword.filestorage_short}} 볼륨에 있는 데이터의 유형 및 이를 암호화된 볼륨에 복사하는 최상의 방법을 고려하십시오. 백업, 정적 컨텐츠가 있으며 복사 중에 변경이 예상되지 않은 항목이 있는 경우에는 주요 고려사항이 없습니다. 

{{site.data.keyword.filestorage_short}}에서 데이터베이스 또는 가상 머신를 실행 중인 경우에는 손상이 발생하지 않도록 원래 볼륨의 데이터가 변경되지 않았는지 확인하십시오. 대역폭이 중요한 경우에는 최대 활동 시간 이외에 마이그레이션을 수행해야 합니다. 이러한 고려사항에 대해 지원이 필요하면 즉시 지원 티켓을 여십시오. 

### Microsoft Windows

원래 {{site.data.keyword.filestorage_short}} 볼륨에서 암호화된 볼륨으로 데이터를 복사하려면 새 스토리지를 포맷하고 Windows Explorer를 사용하여 파일을 복사하십시오. 

### Linux

rsync를 사용하여 데이터를 복사하고자 할 수 있습니다. 명령의 예는 다음과 같습니다. 

`[root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage` 

`--dry-run` 플래그와 함께 위의 명령을 사용하여 경로 라인업이 올바른지 확인하도록 권장합니다. 이 프로세스가 인터럽트되는 경우에는 반드시 시작부터 새 위치로 복사될 수 있도록 복사되는 마지막 대상 파일을 삭제하고자 할 수 있습니다. 

일단 `--dry-run` 플래그 없이 이 명령이 완료되면 데이터를 암호화된 {{site.data.keyword.filestorage_short}} 볼륨으로 복사해야 합니다. 화면을 위로 이동한 후에 명령을 다시 실행하여 누락된 항목이 없는지 확인해야 합니다. 두 위치를 모두 수동으로 검토하여 누락된 항목을 찾고자 할 수도 있습니다. 

마이그레이션이 완료되면 프로덕션을 암호화된 볼륨으로 이동하고 구성에서 원래 볼륨을 분리 및 삭제할 수 있습니다. 참고로, 삭제가 실행되면 원래 볼륨과 연관된 대상 사이트의 스냅샷 또는 복제본도 제거됩니다. 
