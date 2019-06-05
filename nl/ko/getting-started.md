---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-02"

keywords: File Storage, file storage, NFS, provisioning, setup, configuration, mounting storage

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
 {:shortdesc: .shortdesc}


# 시작하기 튜토리얼
{: #getting-started}

{{site.data.keyword.filestorage_full}}는 지속적이고 빠르며 유연한 네트워크 연결, NFS 기반 {{site.data.keyword.filestorage_short}}입니다. 이 NAS(Network-Attached Storage) 환경에서 사용자는 파일 공유 기능과 성능을 전체적으로 제어할 수 있습니다. {{site.data.keyword.filestorage_short}} 공유는 복원성을 위해 라우트된 TCP/IP 연결 상에서 최대 64개의 권한 부여된 디바이스에 연결될 수 있습니다.
{:shortdesc}

## 시작하기 전에
{: #prereqs}

{{site.data.keyword.filestorage_short}} 볼륨은 두 개의 옵션으로 20GB에서 12TB까지 프로비저닝될 수 있습니다. <br/>
- 사전 정의된 성능 레벨 및 기타 기능(스냅샷 및 복제 등)을 제공하는 **Endurance** 계층으로 프로비저닝합니다.
- 할당된 IOPS(Input/Output Operations Per Second)의 고성능 **Performance** 환경을 빌드합니다.

{{site.data.keyword.filestorage_short}} 오퍼링에 대한 자세한 정보는 [{{site.data.keyword.filestorage_short}} 정보](/docs/infrastructure/FileStorage?topic=FileStorage-about)를 참조하십시오.

## 프로비저닝 고려사항

### 블록 크기

Endurance 및 Performance의 IOPS 값은 50/50 읽기/쓰기 50/50 랜덤/순차 워크로드의 16KB 블록 크기를 기반으로 합니다. 16KB 블록은 볼륨에 한 번 쓰기와 동등합니다.
{:important}

애플리케이션에서 사용하는 블록 크기는 스토리지 성능에 직접 영향을 줍니다. 애플리케이션에서 사용하는 블록 크기가 16KB보다 작은 경우에는 처리량 한계보다 IOPS 한계에 먼저 도달합니다. 반대로 애플리케이션에서 사용하는 블록 크기가 16KB보다 큰 경우에는 IOPS 한계보다 처리량 한계에 먼저 도달합니다.

|블록 크기(KB) |IOPS |처리량(MB/초) |
|-----|-----|-----|
|4 |1,000 |16 |
|8 |1,000 |16 |
|16 |1,000 |16 |
|32 |500 |16 |
|64 |250 |16 |
|128 |128 |16 |
|512 |32 |16 |
|1024 |16 |16 |
{: caption="표 1에는 블록 크기 및 IOPS가 처리량에 미치는 영향에 대한 예가 표시되어 있습니다.<br/평균 IO 크기 x IOPS = 처리량(MB/초)" caption-side="top"}

### 권한 부여된 호스트

고려할 또 다른 요인은 볼륨을 사용하는 호스트의 수입니다. 볼륨에 액세스하는 호스트가 하나뿐인 경우에는 사용 가능한 최대 IOPS를 실현하기 어려울 수 있습니다(특히 IOPS 개수가 큰 경우(10,000개 단위)에는 더욱). 워크로드에서 높은 처리량을 요구하는 경우에는 단일 서버 병목 현상을 피할 수 있도록 하기 위해 2 - 3개의 서버는 볼륨에 액세스하도록 구성하는 것이 좋습니다.

### 네트워크 연결

이더넷 연결 속도는 볼륨의 예상 최대 처리량보다 빨라야 합니다. 일반적으로는 이더넷 연결이 사용 가능한 대역폭의 70%를 초과하여 사용하지 않아야 합니다. 예를 들어, IOPS가 6,000이며 16KB 블록 크기를 사용하는 경우 볼륨은 약 94MBps의 처리량을 처리할 수 있습니다. LUN에 대해 1Gbps의 이더넷 연결을 보유하고 있는 경우 서버가 사용 가능한 최대 처리량을 사용하려고 시도하면 병목 현상이 발생합니다. 이는 1Gbps 이더넷 연결(초당 125MB)에 대한 70퍼센트 이론적 한계가 초당 88MB까지만 허용하기 때문입니다.

최대 IOPS를 달성하려면 적절한 네트워크 리소스가 제 위치에 있어야 합니다. 그 외에도 스토리지 외부의 사설 네트워크 사용량과 호스트 측 및 애플리케이션 특정 튜닝(IP 스택 또는 [큐 깊이](/docs/infrastructure/FileStorage?topic=FileStorage-hostqueuesettings) 및 기타 설정)도 고려해야 합니다.

스토리지 트래픽은 다른 트래픽 유형과 격리되어야 하며 방화벽 및 라우터를 통과하지 않도록 해야 합니다. 스토리지 트래픽을 전용 VLAN에 격리하는 것 또한 Jumbo 프레임이 사용으로 설정된 경우 MTU 불일치가 발생하지 않도록 하는 데 도움을 줍니다. 자세한 정보는 [IBM Cloud에서의 Jumbo 프레임](/docs/FileStorage?topic=FileStorage-jumboframes)을 참조하십시오.

스토리지 트래픽은 공용 Virtual Server의 총 네트워크 사용에 포함됩니다. 서비스에서 부과할 수 있는 한계에 관한 자세한 정보는 [Virtual Server 문서](/docs/vsi?topic=virtual-servers-about-public-virtual-servers)를 참조하십시오.

### NFS 버전

NFS v3 및 NFS v4.1 모두는 {{site.data.keyword.cloud}} 환경에서 지원됩니다. 그러나 NFS v4.1은 Stateful 프로토콜이며(NFS v3과 같은 Stateless가 아님) 네트워크 이벤트 중에 프로토콜 문제가 발생할 수 있으므로 NFS v3이 선호됩니다. NFS v4.1은 모든 오퍼레이션을 중지한 후에 잠금 교정을 수행해야 합니다. 상대적으로 많이 사용 중인 NFS 파일 서버에서는 대기 시간 증가로 인해 장애가 발생할 수 있습니다. 또한 NFS v4.1 다중 경로 및 트렁크의 부족으로 인해 NFS 오퍼레이션 복구가 확장될 수도 있습니다.

## 주문 제출

주문을 제출할 준비가 되면 [콘솔](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole)이나 [SLCLI](/docs/infrastructure/FileStorage?topic=FileStorage-orderingSLCLI)를 통해 주문을 제출하십시오. VMware에서의 File Storage 프로비저닝의 경우에는 [여기](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)를 클릭하십시오.

## 새 스토리지 연결
{: #mountingstorage}

프로비저닝 요청이 완료되면 새 스토리지에 액세스할 수 있도록 호스트에 권한을 부여하고 연결을 구성하십시오. 호스트의 운영 체제에 따라 적절한 링크를 사용하십시오.
- [Linux에서 {{site.data.keyword.filestorage_short}} 액세스](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [CentOS의 {{site.data.keyword.filestorage_short}} 마운트](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [CoreOS의 {{site.data.keyword.filestorage_short}} 마운트](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)
- [cPanel로 백업을 위한 {{site.data.keyword.filestorage_short}} 구성](/docs/infrastructure/FileStorage?topic=FileStorage-cPanelBackups)
- [Plesk로 백업을 위한 {{site.data.keyword.filestorage_short}} 구성](/docs/infrastructure/FileStorage?topic=FileStorage-PleskBackup)
- [ESXi 호스트에 {{site.data.keyword.filestorage_short}} 볼륨 마운트](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)

## 새 스토리지 관리

포털 또는 SLCLI를 통해 {{site.data.keyword.filestorage_short}}의 다양한 측면(예: 호스트 권한 부여 및 취소)을 관리할 수 있습니다. 자세한 정보는 [{{site.data.keyword.filestorage_short}} 관리](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage)를 참조하십시오.
