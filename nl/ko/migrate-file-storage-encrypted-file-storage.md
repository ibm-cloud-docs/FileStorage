---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-31"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 개선된 {{site.data.keyword.filestorage_short}}로 {{site.data.keyword.filestorage_short}} 마이그레이션

개선된 {{site.data.keyword.filestorage_full}}는 특정 데이터 센터에서만 사용 가능합니다. 조정 가능한 IOPS 비율 및 확장 가능한 볼륨과 같은 사용 가능한 기능 및 업그레이드된 데이터 센터의 목록을 보려면 [여기](new-ibm-block-and-file-storage-location-and-features.html)를 클릭하십시오. 제공자 관리 암호화된 스토리지에 대한 자세한 정보는 [{{site.data.keyword.filestorage_short}} 저장 암호화](block-file-storage-encryption-rest.html)를 참조하십시오. 

선호되는 마이그레이션 경로는 두 볼륨을 동시에 연결하고 하나의 LUN에서 다른 LUN으로 직접 데이터를 전송하는 것입니다. 세부사항은 사용자의 운영 체제와 복사 오퍼레이션 중에 데이터 변경이 예상되는지 여부에 따라 다릅니다.

사용자가 암호화되지 않은 LUN을 이미 호스트에 연결했다고 가정합니다. 그렇지 않은 경우에는 이 태스크를 완료하는 데 있어서 사용자의 운영 체제에 가장 적합한 지시사항을 따르십시오.

- [Linux의 {{site.data.keyword.filestorage_short}} 마운트](accessing-file-storage-linux.html)
- [CentOS의 NFS/{{site.data.keyword.filestorage_short}} 마운트](mounting-nsf-file-storage.html)
- [CoreOS의 {{site.data.keyword.filestorage_short}} 마운트](mounting-storage-coreos.html)

모든 개선된 {{site.data.keyword.filestorage_short}} 볼륨에는 암호화되지 않은 볼륨과는 다른 마운트 지점이 있습니다. 암호화된, 또는 암호화되지 않은 {{site.data.keyword.filestorage_short}} 볼륨에 대해 올바른 마운트 지점을 사용 중인지 확인하려는 경우에는 {{site.data.keyword.slportal}}의 **볼륨 세부사항** 페이지에서 마운트 지점 정보를 볼 수 있습니다. 또한 API 호출 `SoftLayer_Network_Storage::getNetworkMountAddress()`를 통해 올바른 마운트 지점에 액세스할 수 있습니다.
{:tip}


## 새 {{site.data.keyword.filestorage_short}} 작성

API에서 주문하는 경우에는 새 스토리지의 업데이트된 기능을 받을 수 있도록 "SaaS(Storage as a Service)" 패키지를 지정하십시오.
{:important}

{{site.data.keyword.slportal}}/{{site.data.keyword.BluSoftlayer_full}} 카탈로그를 통해 개선된 볼륨/파일 공유를 주문하는 데 대한 지시사항은 다음과 같습니다. 마이그레이션을 용이하게 하려면 새 볼륨의 크기가 원래 볼륨 크기 이상이어야 합니다.

### 새 Endurance 스토리지 볼륨 주문

1. [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}에서 **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하거나 {{site.data.keyword.BluSoftlayer_full}} 카탈로그에서 **인프라** > **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하십시오.
2. **{{site.data.keyword.filestorage_short}} 주문**을 클릭하십시오.
3. **스토리지 유형 선택** 목록에서 **Endurance**를 선택하십시오.
4. **위치**를 클릭하고 데이터 센터를 선택하십시오.
   - 새 스토리지가 원래 스토리지와 동일한 위치에 추가되는지 확인하십시오.
5. 청구 옵션을 선택하십시오. 월별 또는 시간별 청구 중에서 선택할 수 있습니다.
6. **Endurance**를 클릭하고 IOPS 계층을 선택하십시오.
6. 목록에서 **사용 가능한 스토리지 크기**를 선택하십시오. 새 볼륨의 크기는 원래 볼륨 크기 이상이어야 합니다.
7. 목록에서 **스냅샷 영역 크기**(사용 가능한 영역에 추가하여)를 선택하십시오.
8. **계속**을 클릭하십시오. 월별 및 비례 배분된 비용이 표시되며, 주문 세부사항을 검토할 마지막 기회가 제공됩니다. 주문을 변경하려면 **이전**을 클릭하십시오.
9. **마스터 서비스 계약을 읽었습니다** 선택란을 클릭하고 **주문하기**를 클릭하십시오.

### 암호화된 Performance 스토리지 볼륨 주문

1. [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}에서 **스토리지**, **{{site.data.keyword.filestorage_short}}**를 클릭하거나 {{site.data.keyword.BluSoftlayer_full}} 카탈로그에서 **인프라** >** 스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하십시오.
2. **{{site.data.keyword.filestorage_short}} 주문**을 클릭하십시오.
3. **스토리지 유형 선택** 목록에서 **Performance**를 선택하십시오.
4. **위치**를 클릭하고 데이터 센터를 선택하십시오.
    -  새 스토리지가 원래 스토리지와 동일한 위치에 추가되는지 확인하십시오.
5. 청구 옵션을 선택하십시오. 월별 또는 시간별 청구 중에서 선택할 수 있습니다.
6. 적합한 **스토리지 크기** 옆의 단일 선택 단추를 선택하십시오.
6. **IOPS 지정** 필드에서 IOPS를 입력하십시오.
7. **계속**을 클릭하십시오. 월별 및 비례 배분된 비용이 표시되며, 주문 세부사항을 검토할 마지막 기회가 제공됩니다. 주문을 변경하려면 **이전**을 클릭하십시오.
8. **마스터 서비스 계약을 읽었습니다** 선택란을 클릭하고 **주문하기**를 클릭하십시오.

스토리지가 1분 내에 프로비저닝되며 {{site.data.keyword.slportal}}의 {{site.data.keyword.filestorage_short}} 페이지에 표시됩니다.


## 새 {{site.data.keyword.filestorage_short}}에 대해 호스트에 권한 부여

"권한 부여된" 호스트는 볼륨에 대한 액세스 권한이 부여된 호스트입니다. 호스트 권한 부여를 수행하지 않으면 시스템에서 스토리지에 액세스하거나 이를 사용할 수 없습니다.

1. 새 볼륨의 이름을 클릭하십시오.
2. **권한 부여된 호스트** 섹션으로 스크롤하십시오.
3. 오른쪽에 있는 **호스트 권한 부여** 링크를 클릭하십시오. 볼륨에 액세스할 수 있는 호스트를 선택하십시오.

호스트에 권한이 부여되면 볼륨을 호스트에 연결하십시오.


## 스냅샷 및 복제 설정

원래 볼륨에 대해 스냅샷 및 복제가 설정된 경우에는 새 볼륨에 대해서도 이를 설정해야 합니다. 복제, 스냅샷 영역을 구성하고 원래 볼륨과 동일한 설정으로 스냅샷 스케줄을 작성하십시오.

대상 데이터 센터에서 암호화를 보유하지 않은 경우에는 해당 데이터 센터가 업그레이드될 때까지 새 볼륨에 대한 복제를 설정할 수 없습니다.
{:important}


## 데이터 마이그레이션

1. 원본 및 새 {{site.data.keyword.filestorage_short}} 볼륨에 연결하십시오.
  - 두 파일 공유를 호스트에 연결하는 데 지원이 필요한 경우에는 지원 티켓을 여십시오.

2. 원래 {{site.data.keyword.filestorage_short}} 볼륨에 있는 데이터의 유형과 이를 새 파일 공유에 복사하는 가장 좋은 방법을 고려하십시오.
  - 백업, 정적 컨텐츠, 복사 중에 변경될 것으로 예상되지 않는 항목이 있는 경우에는 주된 고려사항이 없습니다.
  - {{site.data.keyword.filestorage_short}}에서 데이터베이스 또는 가상 머신을 실행 중인 경우에는 데이터 손상이 발생하지 않도록 하기 위해 데이터가 변경되지 않도록 하십시오. 대역폭이 중요한 경우에는 최대 활동 시간을 피해 마이그레이션을 수행하십시오. 이러한 고려사항에 대해 지원이 필요하면 지원 티켓을 여십시오.

3. 데이터를 복사하십시오.
   - **Microsoft Windows**
     - 원래 {{site.data.keyword.filestorage_short}} LUN에서 새 LUN으로 데이터를 복사하려면 새 스토리지를 포맷하고 Windows 탐색기를 사용하여 파일을 복사하십시오.
   - **Linux**
     - `rsync`를 사용하여 데이터를 복사할 수 있습니다.
       ```
[root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage
       ```

   앞서 언급된 명령을 `--dry-run` 플래그와 함께 사용하여 경로가 올바른지 확인하는 것이 좋습니다. 이 프로세스가 인터럽트되는 경우에는 복사 중이던 마지막 대상 파일을 삭제하여 해당 파일이 확실히 처음부터 새 위치로 복사되도록 할 수 있습니다.

      이 명령이 `--dry-run` 플래그 없이 완료된 경우에는 데이터가 새 {{site.data.keyword.filestorage_short}} 볼륨으로 복사된 것입니다. 명령을 다시 실행하여 누락된 항목이 없는지 확인하십시오. 두 위치를 수동으로 검토하여 누락된 항목이 없는지 살펴볼 수도 있습니다.

      마이그레이션이 완료되면 프로덕션을 새 LUN으로 이동할 수 있습니다. 그 후에는 구성에서 원래 볼륨을 분리하고 삭제할 수 있습니다. 이 삭제 작업은 대상 사이트에 있는, 원래 볼륨과 연관된 모든 스냅샷 또는 복제본 또한 제거합니다.
