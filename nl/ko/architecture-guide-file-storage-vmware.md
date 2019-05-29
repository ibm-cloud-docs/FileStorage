---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, provisioning File Storage for VMware, NFS, File Storage, vmware,

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# VMware에서의 {{site.data.keyword.filestorage_short}} 프로비저닝
{: #architectureguide}

다음 단계를 수행하여 {{site.data.keyword.BluSoftlayer_full}}의 vSphere 5.5 및 vSphere 6.0 환경에서 {{site.data.keyword.filestorage_full}}를 주문하고 구성할 수 있습니다.

{{site.data.keyword.filestorage_short}}는 예측 가능한 성능 레벨을 필요로 하는, I/O 양이 많은 애플리케이션을 지원하기 위해 디자인되었습니다. 예측 가능한 성능은 개별 볼륨에 대한 프로토콜 레벨 IOPS(Input/Output Operations Per Second)의 할당을 통해 얻을 수 있습니다.

8개가 넘는 호스트에서 VMWare 데이터 저장소에 액세스해야 하는 경우 NFS {{site.data.keyword.filestorage_short}}를 선택하는 것이 가장 좋습니다.
{:tip}

{{site.data.keyword.filestorage_short}} 오퍼링은 NFS 연결을 통해 액세스되고 마운트됩니다. VMware 배치에서는 하나의 볼륨을 최대 64개의 ESXi 호스트까지 공유 스토리지로서 마운트할 수 있습니다. 또는 여러 볼륨을 마운트하여 vSphere Storage DRS(Distributed Resource Scheduler)를 사용하기 위한 스토리지 클러스터를 작성할 수 있습니다.

{{site.data.keyword.filestorage_short}}의 가격 및 구성 옵션은 예약되는 영역과 제공되는 IOPS의 조합을 기반으로 비용이 청구됩니다.

## 주문 고려사항

{{site.data.keyword.filestorage_short}}를 주문할 때 다음 정보를 고려하십시오.

- 크기를 결정할 때는 필요한 워크로드와 처리량의 크기를 고려하십시오. 용량(IOPS/GB)에 비례하여 선형적으로 성능을 스케일링하는 Endurance 서비스에서는 크기 문제가 중요합니다. 이와 반대로 Performance 서비스에서는 관리자가 용량과 성능을 별도로 선택할 수 있습니다. Performance 서비스에서는 처리량 요구사항 문제가 중요합니다.

  처리량 계산식은 IOPS x 16KB입니다. IOPS는 50/50 읽기/쓰기 혼합의 16KB 블록 크기를 기반으로 측정됩니다.<br/>블록 크기를 늘리면 처리량이 늘어나지만 IOPS가 줄어듭니다. 예를 들어, 32KB 블록으로 블록 크기를 두 배로 늘리면 최대 처리량은 그대로 유지되지만 IOPS는 반으로 감소됩니다.
  {:note}

- NFS는 `lookup`, `getattr` 및 `readdir`과 같은 여러 추가 파일 제어 오퍼레이션을 사용합니다. 읽기/쓰기 오퍼레이션에 추가하여 이러한 오퍼레이션은 IOPS로 계수될 수 있으며, 이는 오퍼레이션 유형 및 NFS 버전에 따라 다양할 수 있습니다.
- {{site.data.keyword.filestorage_short}} 볼륨은 권한 부여된 디바이스, 서브넷 또는 IP 주소에 노출됩니다.
- 경로 장애 복구 중에 스토리지 연결이 끊김을 방지하기 위해 {{site.data.keyword.IBM}}에서는 적절한 제한시간 값을 설정하는 VMWare 도구를 설치하는 것을 권장합니다. 값을 변경할 필요는 없으며, 기본 설정은 VMWare 호스트의 연결이 끊어지지 않도록 하기에 충분합니다.
- NFSv3 및 NFSv4.1이 둘 다 {{site.data.keyword.cloud}} 환경에서 지원됩니다. 그러나 {{site.data.keyword.IBM}}에서는 NFSv3을 사용하도록 권장합니다. NFSv4.1이 Stateful 프로토콜이므로(NFSv3과 같이 Stateless가 아님), 네트워크 이벤트 중에 프로토콜 문제가 발생할 수 있습니다. NFSv4.1은 모든 오퍼레이션을 중지한 후에 잠금 교정을 완료해야 합니다. 이러한 오퍼레이션이 실행되는 동안 장애가 발생할 수 있습니다.

자세한 정보는 [네트워크 연결 스토리지에서
VMware vSphere 실행에 대한 우수 사례](https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/techpaper/vmware-nfs-bestpractices-white-paper-en.pdf)의 VMware 백서를 참조하십시오.{: external}
{:tip}

**NFS 프로토콜 VMware 기능 지원 표**
<table>
  <caption>표 1은 서로 다른 두 NFS 버전에 적용되는 vSphere 기능을 보여줍니다.</caption>
 <thead>
  <tr>
   <th>vSphere 기능</th>
   <th>NFS 버전 3</th>
   <th>NFS 버전 4.1</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>vMotion 및 스토리지 vMotion</td>
   <td>예</td>
   <td>예</td>
  </tr>
  <tr>
   <td>고가용성(HA)</td>
   <td>예</td>
   <td>예</td>
  </tr>
  <tr>
   <td>FT(Fault Tolerance)</td>
   <td>예</td>
   <td>예</td>
  </tr>
  <tr>
   <td>DRS(Distributed Resource Scheduler)</td>
   <td>예</td>
   <td>예</td>
  </tr>
  <tr>
   <td>호스트 프로파일</td>
   <td>예</td>
   <td>예</td>
  </tr>
  <tr>
   <td>스토리지 DRS</td>
   <td>예</td>
   <td>아니오</td>
  </tr>
  <tr>
   <td>스토리지 I/O 제어</td>
   <td>예</td>
   <td>아니오</td>
  </tr>
  <tr>
   <td>사이트 복구 관리자</td>
   <td>예</td>
   <td>아니오</td>
  </tr>
  <tr>
   <td>가상 볼륨</td>
   <td>예</td>
   <td>아니오</td>
  </tr>
 </tbody>
</table>
*소스 - [VMware - NFS 프로토콜 및 ESXi](https://docs.vmware.com/en/VMware-vSphere/6.0/com.vmware.vsphere.storage.doc/GUID-8A929FE4-1207-4CC5-A086-7016D73C328F.html){: external}*



### 스냅샷 사용

{{site.data.keyword.filestorage_short}}를 통해 관리자는 각 스토리지 볼륨에 대해 자동으로 스냅샷 사본을 작성 및 삭제하는 스냅샷 스케줄을 설정할 수 있습니다. 또한 관리자는 자동 스냅샷에 대해 추가로 스냅샷 스케줄(시간별, 일별, 주별)을 작성하고 비즈니스 연속성 및 재해 복구(BCDR) 시나리오에 대한 임시 스냅샷을 수동으로 작성할 수도 있습니다. 자동 경보는 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}을 통해 보유 중인 스냅샷과 사용된 영역의 볼륨 소유자에게 전달됩니다.

스냅샷을 사용하려면 스냅샷 영역이 필요합니다. 영역은 초기 볼륨 주문 중에 구매하거나, 초기 프로비저닝 후 **볼륨 세부사항** 페이지를 통해(**조치**를 클릭하고 **스냅샷 영역 추가**를 선택) 구매할 수 있습니다.

VMware 환경은 스냅샷을 인지하지 않음을 유념하십시오. {{site.data.keyword.filestorage_short}} 스냅샷 기능을 VMware 스냅샷과 혼동하지 않아야 합니다. {{site.data.keyword.filestorage_short}} 스냅샷 기능을 사용하는 복구는 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}에서 처리해야 합니다.

{{site.data.keyword.filestorage_short}} 볼륨을 복원하려면 {{site.data.keyword.filestorage_short}}의 모든 VM을 꺼야 합니다. 프로세스 중에 데이터 손상을 방지하기 위해 ESXi 호스트의 볼륨을 임시로 마운트 해제해야 합니다.

자세한 정보는 [스냅샷](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots) 문서를 참조하십시오.


### 복제 사용

복제는 스냅샷 스케줄 중 하나를 사용하여 스냅샷을 원격 데이터 센터의 대상 볼륨으로 자동으로 복사합니다. 사본은 데이터 손상이나 대형 참사가 발생할 때 원격 사이트에서 복구될 수 있습니다.

복제본을 사용하면 다음을 수행할 수 있습니다.

- 대상 볼륨으로 장애 복구하여 사이트 장애와 기타 장애에서 빠르게 복구합니다.
- DR 사본의 특정 시점으로 장애 복구합니다.

복제는 서로 다른 두 위치에 동기화된 데이터를 보관합니다. 볼륨을 복제한 후에 이를 원래 볼륨과 독립적으로 사용하려면 [중복 파일 볼륨 작성](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume)을 참조하십시오.
{:tip}

복제하려면 우선 스냅샷 스케줄을 작성해야 합니다.

장애 복구 시에 사용자는 기본 데이터 센터의 스토리지 볼륨에서 원격 데이터 센터의 대상 볼륨으로 “스위치를 전환”합니다. 예를 들면, 기본 데이터 센터는 런던에 있고 보조 데이터 센터는 암스테르담에 있습니다. 이 경우에는 장애가 발생하면 암스테르담으로 장애 복구됩니다. 장애 복구는 암스테르담의 vSphere 클러스터 인스턴스에서 현재 기본 볼륨으로 연결함을 의미합니다. 런던의 볼륨이 복구된 후에는 암스테르담 볼륨의 스냅샷을 가져옵니다. 그런 다음 런던으로 장애 복구하고 런던의 컴퓨팅 인스턴스의 기본 볼륨으로 다시 연결할 수 있습니다.

볼륨이 기본 데이터 센터로 장애 복구되기 전에 원격 사이트에서 이의 사용을 중지해야 합니다. 신규 또는 변경된 정보의 스냅샷이 작성되며, 프로덕션 사이트 ESXi 호스트에 다시 마운트될 수 있기 전에 기본 데이터 센터로 복제됩니다.

복제본 구성에 대한 자세한 정보는 [복제](/docs/infrastructure/FileStorage?topic=FileStorage-replication)를 참조하십시오.

손상, 해킹 또는 감염되어 올바르지 않은 데이터는 스냅샷 스케줄과 스냅샷 보유에 따라 복제됩니다. 최소 복제 창을 사용하면 보다 나은 복구 지점 목표가 제공됩니다. 하지만 이에 따라 올바르지 않은 데이터의 복제에 대해 반응하는 시간도 단축될 수 있습니다.
{:note}


## {{site.data.keyword.filestorage_short}} 주문

[고급 단일 사이트 VMware 참조 아키텍처](https://{DomainName}/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){: external}을 사용하여 VMware 환경의 Endurance 또는 Performance 옵션으로 {{site.data.keyword.filestorage_short}}를 설정할 수 있습니다.

{{site.data.keyword.filestorage_short}}는 [IBM Cloud 카탈로그](https://{DomainName}/catalog){: external} 또는 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}을 통해 주문할 수 있습니다. 자세한 정보는 [{{site.data.keyword.filestorage_short}} 주문](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole)을 참조하십시오.

스토리지가 1분 내에 프로비저닝되며 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}의 **{{site.data.keyword.filestorage_short}}** 페이지에 표시됩니다.

볼륨이 프로비저닝되고 나면 볼륨을 사용할 {{site.data.keyword.BluBareMetServers_full}} 또는 {{site.data.keyword.BluVirtServers_full}}에 스토리지 액세스 권한이 부여되어야 합니다. 다음 단계를 사용하여 볼륨에 권한을 부여할 수 있습니다.

1. **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하십시오.
2. **Endurance** 또는 **Performance 볼륨 조치** 메뉴에서 **호스트 액세스**를 선택하십시오.
3. **서브넷**을 클릭하십시오.
4. ESXi 호스트의 VMkernel 포트에 지정된 사용 가능한 서브넷의 목록에서 선택하고 **제출**을 클릭하십시오.<br/>

   표시되는 서브넷은 스토리지 볼륨과 동일한 데이터 센터의 구독된 서브넷입니다.
   {:note}

서브넷에 권한이 부여된 후에는 볼륨을 마운트할 때 사용할 Endurance 또는 Performance 스토리지 서버의 호스트 이름을 기록하십시오. 이 정보는 특정 볼륨을 클릭하여 {{site.data.keyword.filestorage_short}} 세부사항 페이지에서 찾을 수 있습니다.


##  VMware 가상 머신 호스트 구성

VMware 구성 프로세스를 시작하기 전에, 다음 전제조건을 만족하는지 확인하십시오.

- VMware ESXi의 {{site.data.keyword.BluBareMetServers}}가 적절한 스토리지 구성과 ESXi 로그인 인증 정보로 프로비저닝되었습니다.
- {{site.data.keyword.BluBareMetServers}}와 동일한 데이터 센터의 {{site.data.keyword.cloud}} Windows 실제 또는 {{site.data.keyword.virtualmachinesshort}}. 로그인 인증 정보 및 {{site.data.keyword.cloud}} Windows VM의 공인 IP 주소가 포함됩니다.
- 인터넷 액세스가 가능하며 웹 브라우저 소프트웨어와 RDP(Remote Desktop Protocol) 클라이언트가 설치된 컴퓨터가 있습니다.


### 1. VMware 호스트 구성

1. 인터넷에 연결된 컴퓨터에서 RDP 클라이언트를 시작하고 vSphere vCenter가 설치된 데이터 센터와 동일한 데이터 센터에서 프로비저닝된 {{site.data.keyword.BluVirtServers_full}}에 대한 RDP 세션을 설정하십시오.
2. {{site.data.keyword.BluVirtServers_short}}에서 웹 브라우저를 시작하고 vSphere 웹 클라이언트를 통해 VMware vCenter에 연결하십시오.
3. **홈** 화면에서 **호스트 및 클러스터**를 선택하십시오. 왼쪽의 분할창을 펼치고 이 배치에 사용될 **VMware ESXi 서버**를 선택하십시오.
4. vSphere 호스트에서 NFS 클라이언트를 구성할 수 있도록 NFS 클라이언트의 방화벽 포트가 모든 호스트에서 열려 있는지 확인하십시오. (포트는 vSphere의 보다 최신 릴리스에서는 자동으로 열립니다.) 포트가 열려 있는지 확인하려면 VMware® vCenter™의 **ESXi 호스트 관리** 탭으로 이동하여 **설정**을 선택한 후 **보안 프로파일**을 선택하십시오. **방화벽** 섹션에서 **편집**을 클릭하고 **NFS 클라이언트**로 스크롤하십시오.
5. **IP 주소 또는 IP 주소 목록에서 연결 허용**이 제공되는지 확인하십시오. <br/>
   ![연결 허용](/images/1_4.png)
6. **ESXi 호스트 관리** 탭으로 이동하여 Jumbo 프레임을 구성하고 **관리**를 선택한 후에 **네트워킹**을 선택하십시오.
7. **VMkernel 어댑터**를 선택하고 **vSwitch**를 강조표시한 후 **편집**(연필 아이콘)을 클릭하십시오.
8. **NIC 설정**을 선택하고 NIC MTU가 9000으로 설정되었는지 확인하십시오.
9. **선택사항**: Jumbo 프레임 설정을 유효성 검증하십시오.
   - Windows
     ```
     ping -f -l 8972 a.b.c.d
     ```
     {:pre}

   - UNIX
     ```
     ping -s 8972 a.b.c.d
     ```
     {:pre}

     a.b.c.d 값은 인접하는 {{site.data.keyword.BluVirtServers_short}} 인터페이스입니다.

     예
     ```
        ping a.b.c.d (a.b.c.d) 8972(9000) bytes of data.
        8980 bytes from a.b.c.d: icmp_seq=1 ttl=128 time=3.36 ms
     ```

VMware 및 Jumbo 프레임에 관한 자세한 정보는 [여기](https://kb.vmware.com/s/article/1003712){: external}을 참조하십시오.
{:tip}


### 2. 가상 스위치에 업링크 어댑터 추가

1. **ESXi 호스트 관리** 탭으로 이동하여 새 업링크 어댑터를 구성하고 **관리**를 선택한 후에 **네트워킹**을 선택하십시오.
2. **실제 어댑터** 탭을 선택하십시오.
3. **호스트 네트워킹 추가**(더하기 부호가 있는 구형 아이콘)를 클릭하십시오.
4. 연결 유형을 **실제 네트워크 어댑터**로 선택하고 **다음**을 클릭하십시오.
5. 기존 **vSwitch**를 선택하고 **다음**을 클릭하십시오.
6. **미사용 어댑터**를 선택하고 **어댑터 추가**(더하기 부호)를 클릭하십시오.
7. 기타 "연결된" 어댑터를 클릭하고 **확인**을 클릭하십시오. <br/>
   ![스위치에 실제 어댑터 추가](/images/2_3.png)
8. **다음**을 클릭하고 **완료**를 클릭하십시오.
9. **가상 스위치** 탭으로 돌아가 **가상 스위치** 표제 아래에 있는 **설정 편집**(연필 아이콘)을 선택하십시오.
10. 왼쪽의 vSwitch **팀 및 장애 복구** 항목을 선택하십시오.
11. **로드 밸런싱** 옵션이 **원래 가상 포트를 기반으로 라우트**로 설정되었는지 확인하고 **확인**을 클릭하십시오.


### 3. 정적 라우팅 구성(선택사항)

이 아키텍처 안내서의 네트워크 구성에서는 최소 개수의 포트 그룹을 사용합니다. NFS 스토리지를 위한 VMkernel 포트 그룹이 있는 경우에는 추가 단계를 수행해야 합니다. 기본적으로, ESXi는 NFS 볼륨과 동일한 서브넷에 있는 VMkernel 포트를 사용하여 NFS 볼륨을 마운트합니다. NFS 볼륨을 마운트하는 데 계층 3 라우팅이 사용되므로, ESXi는 NFS 볼륨을 마운트하기 위해 구성된 VMkernel 포트를 사용하도록 강제되어야 합니다. 올바른 포트를 사용하려면 스토리지 어레이에 대한 정적 라우트를 작성해야 합니다.

1. 정적 라우트를 구성하려면 Performance 또는 Endurance 스토리지를 사용 중인 각 ESXi 호스트에 SSH를 실행하고 다음 명령을 실행하십시오. `ping` 명령(첫 번째 명령)에 의해 출력되는 IP 주소를 기록한 후 이를 `esxcli` 네트워크 명령에 사용하십시오.
   ```
   ping <host name of the storage array>
   ```
   {: pre}

   NFS 스토리지 DNS 호스트 이름은 여러 IP 주소가 지정된 FZ(Forwarding Zone)입니다. 이러한 IP 주소는 정적이며 해당 특정 DNS 호스트 이름에 속합니다. 해당 IP 주소를 사용하여 특정 볼륨에 액세스할 수 있습니다.
   {:note}

   ```
   esxcli network ip route ipv4 add –gateway GATEWAYIP –network <result of ping command>/32
   ```
   {: pre}

2. ESXi 5.0 이전에서는 정적 라우트가 다시 시작 간에 지속되지 않습니다. 추가된 정적 라우트가 지속되도록 하려면 이 명령을 각 호스트의 `/etc/rc.local.d/` 디렉토리에 있는 `local.sh` 파일에 추가해야 합니다. Visual Editor를 사용하여 `local.sh` 파일을 열고 `exit 0` 행 앞에 4.1단계의 두 번째 명령을 추가하십시오.

다음 단계에서 볼륨을 마운트하는 데 사용될 수 있으므로 IP 주소를 기록해 두십시오.<br/>ESXi 호스트에 마운트할 각각의 NFS 볼륨마다 이 프로세스를 수행해야 합니다.<br/>자세한 정보는 VMware KB 문서인 [Configuring static routes for VMkernel ports on an ESXi host](https://kb.vmware.com/s/article/2001426){: external}를 참조하십시오.
{:tip}


##  ESXi 호스트에 {{site.data.keyword.filestorage_short}} 볼륨 작성 및 마운트

1. **vCenter로 이동** 아이콘을 클릭한 후 **호스트 및 클러스터**를 클릭하십시오.
2. **관련 오브젝트** 탭에서 **데이터 저장소**를 클릭하십시오.
3. **새 데이터 저장소 작성** 아이콘을 클릭하십시오.
4. **새 데이터 저장소** 화면에서 VMware 데이터 저장소의 위치를 선택하고 **다음**을 클릭하십시오.
5. **유형** 화면에서 **NFS**를 선택하고 **다음**을 클릭하십시오.
6. 그리고 NFS 버전을 선택하십시오. NFSv3 및 NFSv4.1이 둘 다 지원되지만, NFSv3이 선호됩니다.

   데이터 저장소에 액세스하려면 하나의 NFS 버전만 사용하십시오. 서로 다른 버전을 사용하여 동일한 데이터 저장소에 하나 이상의 호스트를 마운트하면 데이터가 손상될 수 있습니다.
   {:important}

7. **이름 및 구성** 화면에서 VMware 데이터 저장소에 지정할 이름을 입력하십시오. NFS 서버의 호스트 이름도 입력하십시오. NFS 서버에 대해 FQDN을 사용하면 기반 서버에 대한 최상의 트래픽 분배가 이루어집니다. IP 주소도 유효하지만 FQDN보다는 덜 사용되며 특정한 경우에서만 사용됩니다. `/foldername` 양식으로 폴더 이름을 입력하십시오.
8. **호스트 접근성** 화면에서 NFS VMware 데이터 저장소를 마운트할 호스트를 하나 이상 선택하고 **다음**을 클릭하십시오.
9. 다음 화면에서 입력을 검토하고 **완료**를 클릭하십시오.
10. 추가 {{site.data.keyword.filestorage_short}} 볼륨에 대해 반복 실행하십시오.

{{site.data.keyword.BluSoftlayer_full}}에서는 FQDN 이름을 사용하여 VMware 데이터 저장소에 연결하도록 권장합니다. 직접 IP 주소 지정을 사용하면 FQDN을 사용하여 제공되는 로드 밸런싱 메커니즘이 무시될 수 있습니다.
{:important}

FQDN이 아니라 IP 주소를 사용하려면 서버에 대해 ping을 실행하여 IP 주소를 얻으십시오.
```
ping <host name of the storage array>
```
{: pre}

ESXi 호스트에서 IP 주소를 얻으려면 다음 명령을 사용하십시오. 결과인 10.2.125.80이 FQDN의 IP입니다.
```
~ # vmkping nfsdal0902a-fz.service.softlayer.com
PING nfsdal0902a-fz.service.softlayer.com (10.2.125.80): 56 data bytes
64 bytes from 10.2.125.80: icmp_seq=0 ttl=253 time=0.187 ms
```


## ESXi SIOC(Storage I/O Control) 사용(선택사항)

SIOC(Storage I/O Control)는 Enterprise Plus 라이센스를 사용하는 고객이 사용할 수 있는 기능입니다. 환경에서 SIOC가 사용으로 설정된 경우 이는 단일 VM의 디바이스 큐 길이를 변경합니다. 디바이스 큐 길이가 변경되면 모든 VM의 스토리지 어레이 큐가 동일 공유로 축소됩니다. SIOC는 리소스가 제한되고 스토리지 I/O 대기 시간이 정의된 임계값을 초과할 때만 개입합니다.


SIOC에서 스토리지 디바이스의 정체된 또는 제한된 시점을 판별하려면 임계값 정의가 필요합니다. 정체 임계값 대기 시간은 스토리지 유형에 따라 다릅니다. 기본 선택사항은 최대 처리량의 90%입니다. 최대 처리량 값의 백분율은 VMware 데이터 저장소가 예상 최대 처리량의 해당 백분율을 사용할 때의 예상 대기 시간 임계값을 나타냅니다.


VMware 데이터 저장소 또는 VMDK에 대해 SIOC를 올바르게 구성하지 않으면 성능에 상당한 영향을 줄 수 있습니다.
{:important}


### VMware 데이터 저장소의 SIOC(Storage I/O Control) 구성

1. vSphere 웹 클라이언트 네비게이터에서 VMware 데이터 저장소를 찾아보십시오.
2. **관리** 탭을 클릭하십시오.
3. **설정**을 클릭하고 **일반**을 클릭하십시오.
4. **데이터 저장소 기능**에 대해 **편집**을 클릭하십시오.
5. **SIOC(Storage I/O Control) 사용** 선택란을 선택하십시오.<br/>
   ![NSF VMware 데이터 저장소](/images/3_0.png)
6. **확인**을 클릭하십시오.

이 설정은 호스트가 아닌 VMware 데이터 저장소에 특정합니다.
{:note}


### {{site.data.keyword.BluVirtServers_short}}의 SIOC(Storage I/O Control) 구성

개별 VM에 대해 개별 가상 디스크를 제한하거나 SIOC의 서로 다른 공유를 이에 허용할 수도 있습니다. 디스크를 제한하고 서로 다른 공유를 허용하면 확보된 {{site.data.keyword.filestorage_short}} 볼륨 IOPS 번호로 워크로드에 환경을 일치시키고 조정할 수 있습니다. 한계는 IOPS를 통해 설정되며 다른 가중치 또는 "공유"를 설정할 수도 있습니다. 공유가 높음(2,000 공유)으로 설정된 가상 디스크는 보통(1,000 공유)으로 설정된 디스크보다 2배의 I/O를 수신하며, 낮음(500 공유)으로 설정된 디스크보다 4배의 I/O를 수신합니다. 보통이 모든 VM의 기본값이므로, 높거나 낮은 값을 필요로 하는 VM에 대해서만 값을 조정하면 됩니다.

VDisk 공유 및 한계를 변경하려면 다음 단계를 사용하십시오.

1. **VM 및 템플리트** 인벤토리에서 {{site.data.keyword.BluVirtServers_short}}를 선택하십시오.
2. I/O 제어에 대해 {{site.data.keyword.BluVirtServers_short}}를 선택하십시오.
3. **관리** 탭을 클릭하고 **설정**을 클릭하십시오. **편집**을 클릭하십시오.
4. **하드 디스크** 화살을 펼치십시오. 자신의 환경에 따라 **공유 수정** 또는 **한계 - IOPS**를 선택하십시오. 목록에서 가상 하드 디스크를 선택하고 공유 선택 값을 수정하여 {{site.data.keyword.BluVirtServers_short}}에 할당할 상대적 공유 수를 선택하십시오(낮음, 보통 또는 높음). **사용자 정의**를 클릭하고 사용자 정의된 공유 값을 입력할 수도 있습니다.
5. **한계 - IOPS** 열을 클릭하고 가상 머신에 할당할 최대 스토리지 리소스를 입력하십시오.
6. **확인**을 클릭하십시오.


이 프로세스는 SIOC가 사용되지 않는 경우에도 {{site.data.keyword.BluVirtServers_short}}에 있는 개별 vDisk의 리소스 이용 한계를 설정하는 데 사용됩니다. 이러한 설정은 SIOC에 의해 사용되는 경우에도 호스트가 아닌 개별 게스트에 특정합니다.
{:important}


## ESXi 호스트 측 설정 구성

NFS 스토리지를 위해 ESXi 5.x 호스트를 구성하려면 추가 설정이 필요합니다. 이 표에는 각 설정이 어떻게 되어야 하는지 표시되어 있습니다.

|매개변수 |설정 |
|----------|------------|
|Net.TcpipHeapSize |	32 |
|Net.TcpipHeapMax |	vSphere 5.0/5.1의 경우 128로 설정 <br/> vSphere 5.5 이상의 경우 512로 설정 |
|NFS.MaxVolumes |	256 |
|NFS41.MaxVolumes |	256 (vSphere 6.0 이상에만 해당) |
|NFS.HeartbeatMaxFailures |	10 |
|NFS.HeartbeatFrequency |	12 |
|NFS.HeartbeatTimeout |	5 |
|NFS.MaxQueueDepth|	64 |


### CLI를 사용하여 ESXi 5.x 호스트의 고급 구성 매개변수 업데이트

다음 예는 CLI를 사용하여 고급 구성 매개변수를 설정한 후 이를 확인합니다. 예에서 사용된 `esxcfg-advcfg` 도구는 ESXi 5.x 호스트의 `/usr/sbin` 디렉토리에 있습니다.

   - ESXi 5.x CLI에서 고급 구성 매개변수를 설정합니다.

     ```
     #esxcfg-advcfg -s 32 /Net/TcpipHeapSize
     #esxcfg-advcfg -s 128 /Net/TcpipHeapMax(For vSphere 5.0/5.1)
     #esxcfg-advcfg -s 512 /Net/TcpipHeapMax(For vSphere 5.5 and higher)
     #esxcfg-advcfg -s 256 /NFS/MaxVolumes
     #esxcfg-advcfg -s 256 /NFS41/MaxVolumes (ESXi 6.0 and higher)
     #esxcfg-advcfg -s 10 /NFS/HeartbeatMaxFailures
     #esxcfg-advcfg -s 12 /NFS/HeartbeatFrequency
     #esxcfg-advcfg -s 5 /NFS/HeartbeatTimeout   
     #esxcfg-advcfg -s 64 /NFS/MaxQueueDepth
     #esxcfg-advcfg -s 32 /Disk/QFullSampleSize
     #esxcfg-advcfg -s 8 /Disk/QFullThreshold
     ```

  - ESXi 5.x CLI에서 고급 구성 매개변수를 확인합니다.

    ```
    #esxcfg-advcfg -g /Net/TcpipHeapSize
    #esxcfg-advcfg -g /Net/TcpipHeapMax
    #esxcfg-advcfg -g /NFS/MaxVolumes
    #esxcfg-advcfg -g /NFS41/MaxVolumes (ESXi 6.0 and higher)
    #esxcfg-advcfg -g /NFS/HeartbeatMaxFailures
    #esxcfg-advcfg -g /NFS/HeartbeatFrequency
    #esxcfg-advcfg -g /NFS/HeartbeatTimeout
    #esxcfg-advcfg -g /NFS/MaxQueueDepth
    #esxcfg-advcfg -g /Disk/QFullSampleSize
    #esxcfg-advcfg -g /Disk/QFullThreshold
    ```
[여기서](/docs/infrastructure/virtualization?topic=Virtualization-advanced-single-site-vmware-reference-architecture){: external} 고급 단일 사이트 VMware 참조 아키텍처에 대해 자세히 알아보십시오.
{:tip}
