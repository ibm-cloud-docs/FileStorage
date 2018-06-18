---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
 
# 개선된 {{site.data.keyword.filestorage_short}}로 {{site.data.keyword.filestorage_short}} 마이그레이션

개선된 {{site.data.keyword.filestorage_full}}는 엄선된 데이터 센터에서만 사용 가능합니다. 조정 가능한 IOPS 비율 및 확장 가능한 볼륨과 같은 사용 가능한 기능 및 업그레이드된 데이터 센터의 목록을 보려면 [여기](new-ibm-block-and-file-storage-location-and-features.html)를 클릭하십시오. 제공자 관리 암호화된 스토리지에 대한 자세한 정보를 보려면 [{{site.data.keyword.filestorage_short}} 저장 암호화](block-file-storage-encryption-rest.html) 기사를 읽으십시오.

선호하는 마이그레이션 경로는 두 LUN을 동시에 연결하고 하나의 LUN에서 다른 LUN으로 직접 데이터를 전송하는 것입니다. 세부사항은 사용자의 운영 체제와 복사 오퍼레이션 중에 데이터 변경이 예상되는지 여부에 따라 다릅니다. 

사용자가 암호화되지 않은 LUN을 이미 호스트에 연결했다고 가정합니다. 그렇지 않은 경우, 이 태스크를 완료하려면 사용자의 운영 체제에 가장 적합한 지시사항을 따르십시오.

- [Linux의 {{site.data.keyword.filestorage_short}} 마운트](accessing-file-storage-linux.html)
- [CentOS의 NFS/{{site.data.keyword.filestorage_short}} 마운트](mounting-nsf-file-storage.html)
- [CoreOS의 {{site.data.keyword.filestorage_short}} 마운트](mounting-storage-coreos.html)

**참고:** 개선된 모든 {{site.data.keyword.filestorage_short}} 볼륨에는 암호화되지 않은 볼륨과는 다른 마운트 지점이 있습니다. 암호화된 {{site.data.keyword.filestorage_short}} 볼륨과 암호화되지 않은 볼륨에 올바른 마운트 지점을 사용 중임을 확인하기 위해 UI의 **볼륨 세부사항** 페이지에서 마운트 지점 정보를 볼 수 있습니다. 또한 API 호출 `SoftLayer_Network_Storage::getNetworkMountAddress()`를 통해 올바른 마운트 지점에 액세스할 수 있습니다.


## 새 {{site.data.keyword.filestorage_short}} 작성

**중요**: API를 사용하여 주문할 때 "Storage as a Service" 패키지를 지정하여 새 스토리지의 업데이트된 기능을 가져오는지 확인하십시오.

UI를 통한 개선된 볼륨/파일 공유 주문에 대한 지시사항은 다음과 같습니다. 마이그레이션을 용이하게 하려면 새 볼륨의 크기가 원래 볼륨 크기 이상이어야 합니다.

### 새 Endurance 스토리지 볼륨 주문

1. [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}에서 **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하거나 {{site.data.keyword.BluSoftlayer_full}} 카탈로그에서 **인프라** > **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하십시오.
2. 오른쪽 상단 모서리에서 **{{site.data.keyword.filestorage_short}} 주문**을 클릭하십시오. 
3. **스토리지 유형 선택** 목록에서 **Endurance**를 선택하십시오.
4. **위치**를 클릭하고 데이터 센터를 선택하십시오.
   - 새 스토리지가 원래 스토리지와 동일한 위치에 추가되는지 확인하십시오.
5. 청구 옵션을 선택하십시오. 월별 또는 시간별 청구 중에서 선택할 수 있습니다.
6. **Endurance**를 클릭하고 IOPS 계층을 선택하십시오.
6. 목록에서 **사용 가능한 스토리지 크기**를 선택하십시오. 새 볼륨의 크기가 원래 볼륨 크기 이상이어야 합니다.
7. 드롭 다운 목록에서 **스냅샷 영역 크기**(사용 가능한 영역에 추가하여)를 선택하십시오.
8. **계속**을 클릭하십시오. 월별 및 비례 배분된 비용이 표시되며, 주문 세부사항을 검토할 마지막 기회가 제공됩니다. 주문을 변경하려면 **이전**을 클릭하십시오.
9. **마스터 서비스 계약을 읽었습니다** 선택란을 클릭하고 **주문하기**를 클릭하십시오.
 
### 암호화된 Performance 스토리지 볼륨 주문

1. [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}에서 **스토리지**, **{{site.data.keyword.filestorage_short}}**를 클릭하거나 {{site.data.keyword.BluSoftlayer_full}} 카탈로그에서 **인프라** >** 스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하십시오.
2. 오른쪽 상단 모서리에서 **{{site.data.keyword.filestorage_short}} 주문**을 클릭하십시오. 
3. 스토리지 유형 선택 목록에서 **Performance**를 선택하십시오.
4. **위치**를 클릭하고 데이터 센터를 선택하십시오.
    -  새 스토리지가 원래 스토리지와 동일한 위치에 추가되는지 확인하십시오.
5. 청구 옵션을 선택하십시오. 월별 또는 시간별 청구 중에서 선택할 수 있습니다.
6. 적합한 **스토리지 크기** 옆의 단일 선택 단추를 선택하십시오.
6. **IOPS 지정** 필드에서 IOPS를 입력하십시오.
7. **계속**을 클릭하십시오. 월별 및 비례 배분된 비용이 표시되며, 주문 세부사항을 검토할 마지막 기회가 제공됩니다. 주문을 변경하려면 **이전**을 클릭하십시오.
8. **마스터 서비스 계약을 읽었습니다** 선택란을 클릭하고 **주문하기**를 클릭하십시오.

스토리지는 1분 내에 프로비저닝되며 {{site.data.keyword.slportal}}의 {{site.data.keyword.filestorage_short}} 페이지에 나타납니다.

 
## 호스트에 새 {{site.data.keyword.filestorage_short}} 연결

"권한 부여된" 호스트는 볼륨에 대한 액세스 권한이 제공된 호스트입니다. 호스트 권한이 없으면 시스템에서 스토리지에 액세스하거나 이를 사용할 수 없습니다.

1. 새 볼륨의 이름을 클릭하십시오.
2. 페이지의 **권한 부여된 호스트** 섹션으로 화면을 이동하십시오.
3. 페이지 오른쪽에서 **호스트 권한 부여** 링크를 클릭하십시오. 볼륨에 액세스할 수 있는 호스트를 선택하십시오.

권한 부여된 경우 호스트에 볼륨을 연결하십시오.

 
## 스냅샷 및 복제

원래 볼륨에 대해 스냅샷 및 복제가 설정되어 있습니까? 설정된 경우 복제, 스냅샷 영역을 설정하고 원래 볼륨과 동일한 설정으로 새 암호화된 볼륨에 대한 스냅샷 스케줄을 작성해야 합니다. 

**참고**: 대상 데이터 센터가 암호화를 위해 업그레이드되지 않은 경우 데이터 센터가 업그레이드될 때까지 새 볼륨에 대한 복제를 설정할 수 없습니다.

 
## 데이터 마이그레이션

호스트를 원래 및 암호화된 {{site.data.keyword.filestorage_short}} 볼륨 모두에 연결해야 합니다. 그렇지 않은 경우에는 다음을 수행하십시오.

- 이 문서 및 참조된 문서의 단계를 제대로 따랐는지 확인하십시오.
- 두 볼륨을 호스트에 연결할 때 추가 지원을 받으려면 지원 티켓을 여십시오.

### 데이터 고려사항

이 시점에서, 원래 {{site.data.keyword.filestorage_short}} 볼륨에 있는 데이터의 유형 및 이를 암호화된 볼륨에 복사하는 최상의 방법을 고려하십시오. 백업, 정적 컨텐츠가 있으며 복사 중에 변경이 예상되지 않은 항목이 있는 경우에는 주요 고려사항이 없습니다.

{{site.data.keyword.filestorage_short}}에서 데이터베이스 또는 가상 머신을 실행 중인 경우에는 손상이 발생하지 않도록 원래 볼륨의 데이터가 변경되지 않았는지 확인하십시오. 대역폭이 중요한 경우에는 최대 활동 시간 이외에 마이그레이션을 수행해야 합니다. 이러한 고려사항에 대해 지원이 필요하면 지원 티켓을 여십시오.

### Microsoft Windows

원래 {{site.data.keyword.filestorage_short}} 볼륨에서 암호화된 볼륨으로 데이터를 복사하려면 새 스토리지를 포맷하고 Windows Explorer를 사용하여 파일을 복사하십시오.

### Linux

`rsync`를 사용하여 데이터를 복사하고자 할 수 있습니다. 다음은 명령 예입니다.

```
[root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage
```

`--dry-run` 플래그와 함께 명령 예를 사용하여 경로 라인업이 올바른지 확인하도록 권장합니다. 이 프로세스가 인터럽트되는 경우에는 반드시 시작부터 새 위치로 복사될 수 있도록 복사되는 마지막 대상 파일을 삭제하고자 할 수 있습니다.

일단 `--dry-run` 플래그 없이 이 명령이 완료되면 데이터를 암호화된 {{site.data.keyword.filestorage_short}} 볼륨으로 복사해야 합니다. 화면을 위로 이동한 후에 명령을 다시 실행하여 누락된 항목이 없는지 확인해야 합니다. 두 위치를 모두 수동으로 검토하여 누락된 항목을 찾고자 할 수도 있습니다.

마이그레이션이 완료되면 프로덕션을 암호화된 볼륨으로 이동하고 구성에서 원래 볼륨을 분리 및 삭제할 수 있습니다. 

**참고**: 삭제가 실행되면 원래 볼륨과 연관된 대상 사이트의 스냅샷 또는 복제본도 제거됩니다.
