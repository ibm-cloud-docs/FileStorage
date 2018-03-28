---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
 
# {{site.data.keyword.filestorage_short}}를 암호화된 {{site.data.keyword.filestorage_short}}로 마이그레이션

Endurance 또는 Performance에 대한 암호화된 {{site.data.keyword.filestorage_full}}가 엄선된 데이터 센터에서 시작되었습니다. 다음에서는 암호화되지 않은 볼륨에서 암호화된 볼륨으로 {{site.data.keyword.filestorage_short}}를 마이그레이션하는 방법에 대한 정보를 제공합니다. 제공자가 관리하는 암호화된 스토리지에 대한 자세한 정보는 [{{site.data.keyword.filestorage_short}} 휴면 중 암호화](block-file-storage-encryption-rest.html) 기사를 읽으십시오. 업그레이드된 데이터 센터 및 사용 가능한 기능의 목록을 보려면 [여기](new-ibm-block-and-file-storage-location-and-features)를 클릭하십시오. 

선호하는 마이그레이션 경로는 두 볼륨에 동시에 연결하고 한 파일 볼륨에서 다른 파일 볼륨으로 직접 데이터를 전송하는 것입니다. 구체적인 사항은 운영 체제와 복사 오퍼레이션 중 데이터의 예상 변경 여부에 따라 달라집니다. 

사용자 편의를 위해 추가 공통 시나리오를 간략히 설명합니다. 이때 이미 호스트에 암호화되지 않은 파일 볼륨이 연결되었다고 가정합니다. 그렇지 않은 경우 이 태스크를 달성하기 위해 실행하는 운영 체제에 가장 적합한 아래 지침을 따르십시오.  

**참고:**  암호화된 모든 {{site.data.keyword.filestorage_short}} 볼륨에는 암호화되지 않은 볼륨보다 마운트 지점이 더 많습니다. 암호화된 볼륨과 암호화되지 않은  {{site.data.keyword.filestorage_short}} 볼륨 모두에 대해 올바른 마운트 지점을 사용하는지 확인하기 위해 API 호출: SoftLayer_Network_Storage::getNetworkMountAddress()를 사용하여 올바른 마운트 지점에 액세스하고, UI를 통해 **볼륨 세부사항** 페이지에서 마운트 지점 정보를 볼 수 있습니다. 

[Linux에서 {{site.data.keyword.filestorage_short}}에 액세스](accessing-file-storage-linux.html)

## 암호화된 파일 볼륨 작성

다음 단계를 사용하여 마이그레이션 프로세스를 용이하게 하도록 암호화된 동일한 크기 또는 더 큰 크기의 볼륨을 작성하십시오. 

### 암호화된 Endurance 스토리지 볼륨 주문

1. [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 홈페이지에서 **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하거나 {{site.data.keyword.BluSoftlayer_full}} 카탈로그에서 **인프라** > **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하십시오. 

2. {{site.data.keyword.filestorage_short}} 페이지에서 **{{site.data.keyword.filestorage_short}} 주문** 링크를 클릭하십시오. 

3. **Endurance**를 선택하십시오. 

4. 원래 볼륨이 있는 데이터 센터를 선택하십시오. 별표가 있는 데이터 센터에서만 암호화가 가능합니다. 

5. 원하는 **IOPS 티어**를 입력하십시오. 

6. 원하는 스토리지 공간 크기(GB)를 선택하십시오. TB의 경우 1TB는 1,000GB와 같고, 12TB는 12,000GB와 같습니다. 

7. 스냅샷에 대해 원하는 스토리지 공간 크기(GB)를 입력하십시오. 

8. 드롭 다운 목록에서 **VMware OS**를 선택하십시오. 

9. 주문을 제출하십시오. 
 
### 암호화된 Performance 스토리지 볼륨 주문

1. [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 홈페이지에서 **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하거나 {{site.data.keyword.BluSoftlayer_full}} 카탈로그에서 **인프라** > **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하십시오. 

2. **{{site.data.keyword.filestorage_short}} 주문**을 클릭하십시오. 

3. **Performance**를 선택하십시오. 

4. 원래 볼륨이 있는 데이터 센터를 선택하십시오. 암호화는 별표(`*`)가 있는 데이터 센터에서만 사용 가능합니다. 

5. 원래 볼륨과 크기가 동일하거나 그 이상 크기의 원하는 스토리지 공간 크기(GB)를 선택하십시오. 

6. 100의 간격으로 원하는 IOPS 크기를 입력하십시오. 

7. 드롭 다운 목록에서 VMware OS를 선택하십시오. 

8. 주문을 제출하십시오. 

스토리지는 몇 분 안에 프로비저닝되며, 고객 포털의 {{site.data.keyword.filestorage_short}} 페이지에 표시됩니다. 

 
## 호스트에 새 볼륨 연결

“권한 부여”된 호스트는 볼륨에 대한 액세스 권한이 제공된 호스트입니다. 호스트에 권한을 부여하지 않으면 시스템에서 스토리지를 사용하거나 이에 액세스할 수 없습니다. 

1. 암호화된 **볼륨 이름**을 클릭하십시오. 

2. 페이지의 **권한 부여된 호스트** 섹션으로 이동하십시오. 

3. 페이지의 오른쪽에 있는 **호스트 권한 부여** 링크를 클릭하십시오. 볼륨에 액세스할 수 있는 호스트를 선택하십시오. 

권한이 부여되면 호스트에 볼륨을 연결하십시오. 

 
## 스냅샷 및 복제

원래 볼륨에 대해 설정된 스냅샷 및 복제가 있습니까? 있다면 원래 볼륨과 동일한 설정으로 새 암호화된 볼륨에 대한 복제, 스냅샷 영역을 설정하고 스냅샷 스케줄을 작성해야 합니다.  

암호화를 위해 대상 데이터 센터가 업그레이드되지 않은 경우 해당 데이터 센터를 업그레이드할 때까지 새 볼륨에 대한 복제를 설정할 수 없습니다. 

 
## 데이터 마이그레이션

호스트는 원래 볼륨 및 암호화된 {{site.data.keyword.filestorage_short}} 볼륨 모두에 연결되어야 합니다. 그렇지 않은 경우: 

• 위 단계를 수행하여 문서를 올바르게 참조했는지 확인하십시오. 

• 호스트에 두 볼륨을 연결하기 위한 추후 지원을 위해 지원 티켓을 여십시오. 

### 데이터 고려사항

이 시점에서 원래 {{site.data.keyword.filestorage_short}} 볼륨에서 보유한 데이터 유형 및 암호화된 볼륨으로 복사하는 최상의 방법을 고려하십시오. 백업, 정적 컨텐츠, 복사 중 변경이 예상되지 않는 항목이 있는 경우 주요 고려사항은 없습니다. 

{{site.data.keyword.filestorage_short}}에서 가상 머신 또는 데이터베이스를 실행하는 경우 원래 볼륨의 데이터가 복사 중 대체되지 않는지 확인하십시오. 그러면 손상되지 않습니다. 대역폭 문제가 있는 경우 최대 활동 시간 외 시간에 마이그레이션을 수행해야 합니다. 이 고려사항에 대한 도움이 필요한 경우 지원 티켓을 여는데 주저하지 마십시오. 

### Microsoft Windows

원래 {{site.data.keyword.filestorage_short}} 볼륨에서 암호화된 볼륨으로 데이터를 복사하려면 새 스토리지를 포맷하고 Windows 탐색기를 사용하여 파일을 복사하십시오. 

### Linux

데이터를 복사할 때 rsync 사용을 고려할 수 있습니다. 다음은 명령 예제입니다. 

`[root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage` 

`--dry-run` 플래그와 함께 위 명령을 한 번 사용하여 경로를 올바르게 구성했는지 확인하는 것이 좋습니다. 이 프로세스가 인터럽트되면 시작부터 새 위치로 복사되도록 보장하기 위해 복사된 마지막 대상 파일을 삭제할 수 있습니다. 

`--dry-run` 플래그 없이 이 명령을 완료하면 데이터는 암호화된 {{site.data.keyword.filestorage_short}} 볼륨으로 복사됩니다. 위로 스크롤하고 명령을 다시 실행하여 누락된 항목이 없는지 확인해야 합니다. 또한 수동으로 두 위치를 검토하여 누락된 항목이 있는지 확인할 수도 있습니다. 

마이그레이션이 완료되면 프로덕션을 암호화된 볼륨으로 이동하고 분리하고 구성에서 원래 볼륨을 삭제할 수 있습니다. 삭제 시 원래 볼륨과 연관된 대상 사이트에서 스냅샷 또는 복제본도 제거됩니다. 
