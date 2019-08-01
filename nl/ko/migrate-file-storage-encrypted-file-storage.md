---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-24"

keywords: File Storage, file storage, NFS, upgrade, migrate to new

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 개선된 {{site.data.keyword.filestorage_short}}로 {{site.data.keyword.filestorage_short}} 마이그레이션
{: #migratestorage}

고급 {{site.data.keyword.filestorage_full}}는 이제 대부분의 [데이터 센터](/docs/infrastructure/FileStorage?topic=FileStorage-selectDC)에서 사용할 수 있습니다.

선호되는 마이그레이션 경로는 두 볼륨을 동시에 연결하고 하나의 볼륨에서 다른 볼륨으로 직접 데이터를 전송하는 것입니다. 세부사항은 사용자의 운영 체제와 복사 오퍼레이션 중에 데이터 변경이 예상되는지 여부에 따라 다릅니다.

이 경우, 이미 호스트에 암호화되지 않은 볼륨을 연결했다고 가정합니다. 그렇지 않은 경우에는 이 태스크를 완료하는 데 있어서 사용자의 운영 체제에 가장 적합한 지시사항을 따르십시오.

- [Linux의 {{site.data.keyword.filestorage_short}} 마운트](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [CentOS의 {{site.data.keyword.filestorage_short}} 마운트](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [CoreOS의 {{site.data.keyword.filestorage_short}} 마운트](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)

이러한 데이터 센터에서 프로비저닝된 모든 개선된 {{site.data.keyword.filestorage_short}} 볼륨에는 암호화되지 않은 볼륨과는 다른 마운트 지점이 있습니다. 두 스토리지 볼륨에 올바른 마운트 지점을 사용 중임을 확인하기 위해 콘솔의 **볼륨 세부사항** 페이지에서 마운트 지점 정보를 볼 수 있습니다. 또한 API 호출 `SoftLayer_Network_Storage::getNetworkMountAddress()`를 통해 올바른 마운트 지점에 액세스할 수 있습니다.
{:tip}


## {{site.data.keyword.filestorage_short}} 작성

API에서 주문하는 경우에는 새 스토리지의 업데이트된 기능을 받을 수 있도록 "SaaS(Storage as a Service)" 패키지를 지정하십시오.
{:important}

{{site.data.keyword.cloud}} 카탈로그를 통해 고급 볼륨을 주문할 수 있습니다. 새 볼륨은 마이그레이션을 수행하기 위해 원본 파일 공유 크기 이상이어야 합니다.

- [사전 정의된 IOPS 티어(Endurance)가 있는 {{site.data.keyword.filestorage_short}} 주문](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole#endurance)
- [사용자 정의 IOPS(Performance)가 있는 {{site.data.keyword.filestorage_short}} 주문](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole#performance)

몇 분 내에 새 스토리지가 마운트할 수 있게 제공됩니다. 리소스 목록과 {{site.data.keyword.blockstorageshort}} 목록에서 새 스토리지를 볼 수 있습니다.


## 새 {{site.data.keyword.filestorage_short}}에 대해 호스트에 권한 부여

"권한 부여된" 호스트는 볼륨에 대한 액세스 권한이 부여된 호스트입니다. 호스트 권한 부여를 수행하지 않으면 시스템에서 스토리지에 액세스하거나 이를 사용할 수 없습니다.

1. 콘솔에서 **클래식 인프라**  > **스토리지** > **{{site.data.keyword.filestorage_short}}**로 이동하십시오.
2. 마운트할 파일 공유로 스크롤한 후 **...**(조치)를 클릭하십시오. 그런 다음 **호스트 권한 부여**를 선택하십시오.
3. 디바이스 유형, 서브넷 또는 IP 주소를 선택하여 사용 가능한 호스트 목록을 필터링하십시오.

   서브넷으로 목록을 필터링하는 경우 표시되는 서브넷은 스토리지 볼륨과 동일한 데이터 센터에서 구독하는 서브넷입니다.
   {:note}
4. 목록에서 하나 이상의 호스트를 선택하고 **저장**을 클릭하십시오.

호스트에 권한이 부여되면 볼륨을 호스트에 연결하십시오.


## 스냅샷 및 복제 설정

원래 볼륨에 대해 스냅샷 및 복제가 설정된 경우에는 새 볼륨에 대해서도 이를 설정해야 합니다. 복제, 스냅샷 영역을 구성하고 원래 볼륨과 동일한 설정으로 스냅샷 스케줄을 작성하십시오.

대상 데이터 센터에서 암호화를 보유하지 않은 경우에는 해당 데이터 센터가 업그레이드될 때까지 새 볼륨에 대한 복제를 설정할 수 없습니다.
{:important}


## 데이터 마이그레이션

1. 원본 및 새 {{site.data.keyword.filestorage_short}} 볼륨에 연결하십시오.
  - 호스트에 두 개의 파일 공유를 연결하는 데 지원이 필요한 경우 지원 티켓을 여십시오.

2. 원래 {{site.data.keyword.filestorage_short}} 볼륨에 있는 데이터의 유형과 이를 새 파일 공유에 복사하는 가장 좋은 방법을 고려하십시오.
  - 백업, 정적 컨텐츠, 기타 항목이 복사 중에 변경될 것으로 예상하지 않는 경우, 고려해야 할 사항은 없습니다.
  - {{site.data.keyword.filestorage_short}}에서 데이터베이스 또는 가상 머신을 실행 중인 경우에는 데이터 손상이 발생하지 않도록 하기 위해 데이터가 변경되지 않도록 하십시오.
  - 대역폭이 중요한 경우에는 최대 활동 시간을 피해 마이그레이션을 수행하십시오.
  - 이러한 고려사항에 대해 지원이 필요하면 지원 티켓을 여십시오.

3. 데이터를 복사하십시오.
   - **Microsoft Windows**
     - 원래 {{site.data.keyword.filestorage_short}} 볼륨에서 새 볼륨으로 데이터를 복사하려면 새 스토리지를 포맷하고 Windows 탐색기를 사용하여 파일을 복사하십시오.
   - **Linux**
     - `rsync`를 사용하여 데이터를 복사할 수 있습니다.
       ```
[root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage
       ```

   경로가 올바르게 설정되도록 `--dry-run` 플래그가 한 번만 포함된 이전 명령을 사용하는 것이 좋습니다. 이 프로세스가 인터럽트되는 경우에는 복사 중이던 마지막 대상 파일을 삭제하여 해당 파일이 확실히 처음부터 새 위치로 복사되도록 할 수 있습니다.

      이 명령이 `--dry-run` 플래그 없이 완료된 경우에는 데이터가 새 {{site.data.keyword.filestorage_short}} 볼륨으로 복사된 것입니다. 명령을 다시 실행하여 누락된 항목이 없는지 확인하십시오. 두 위치를 수동으로 검토하여 누락된 항목이 없는지 살펴볼 수도 있습니다.

   마이그레이션이 완료되면 프로덕션을 새 볼륨으로 이동할 수 있습니다. 그 후에는 구성에서 원래 볼륨을 분리하고 삭제할 수 있습니다. 이 삭제 작업은 대상 사이트에 있는, 원래 볼륨과 연관된 모든 스냅샷 또는 복제본 또한 제거합니다.
