---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# VMware의 {{site.data.keyword.filestorage_short}}에 대한 아키텍처 안내서

{{site.data.keyword.BluSoftlayer_full}}의 vSphere 5.5 및 vSphere 6.0 환경에서 {{site.data.keyword.filestorage_full}}를 주문하고 구성하는 단계는 다음과 같습니다. VMWare 호스트에 대한 연결이 8개 넘게 필요한 경우에는 NFS {{site.data.keyword.filestorage_short}}를 사용하는 방법이 최선입니다. 

## {{site.data.keyword.filestorage_short}} 개요

{{site.data.keyword.filestorage_short}}는 예측 가능한 성능 레벨이 필요한 높은 I/O 애플리케이션을 지원하도록 설계되었습니다. 예측 가능한 성능은 개별 볼륨에 대한 프로토콜 레벨 IOPS(Input/Output Operations Per Second)의 할당을 통해 얻을 수 있습니다. 

{{site.data.keyword.filestorage_short}} 오퍼링은 NFS 연결을 통해 액세스되고 마운트됩니다. VMware 배치에서는 공유 스토리지로서 최대 64 ESXi 호스트까지 단일 볼륨을 마운트할 수 있습니다. 또는 다수의 볼륨을 마운트하여 vSphere 스토리지 DYNAMIC 리소스 스케줄링을 활용하기 위한 스토리지 클러스터를 작성할 수도 있습니다. 

Endurance 및 Performance {{site.data.keyword.filestorage_short}}의 가격 책정 및 구성 옵션은 제공된 IOPS에 대해 예약된 영역의 조합을 기반으로 비용이 청구됩니다. 

### 1. {{site.data.keyword.filestorage_short}} 고려사항

{{site.data.keyword.filestorage_short}} 주문 시에 사용자는 다음의 정보와 고려사항을 유념하고자 합니다. 

- 일단 {{site.data.keyword.filestorage_short}} 볼륨이 프로비저닝되면 스토리지 크기, IOPS 및 운영 체제를 변경할 수 없습니다. 영역의 용량, IOPS 개수 또는 운영 체제를 변경하려면 새 볼륨을 프로비저닝해야 합니다. 이전 볼륨에 저장된 데이터는 VMware 스토리지 vMotion을 사용하여 새 볼륨으로 마이그레이션되어야 합니다. 
- 크기를 결정할 때는 필요한 워크로드와 처리량의 크기를 고려하십시오. 관리자가 용량과 성능을 별도로 선택할 수 있도록 허용하는 Performance 서비스와는 달리, 용량(IOPS/GB)에 비례하여 선형적으로 성능을 스케일링하는 Endurance 서비스에서는 크기 문제가 중요합니다. Performance 서비스에서는 처리량 요구사항 문제가 중요합니다. <br/> **참고**: 처리량 계산은 IOPS x 16KB입니다. IOPS는 50/50 읽기/쓰기 혼합의 16KB 블록 크기를 기반으로 측정됩니다. <br/> **참고**: 블록 크기를 늘리면 처리량은 증가되지만 IOPS는 감소됩니다. 예를 들어, 32KB 블록으로 블록 크기를 두 배로 늘리면 최대 처리량은 그대로 유지되지만 IOPS는 반으로 감소됩니다. 
- NFS는 다수의 추가적인 파일 제어 오퍼레이션(일부 예: lookup, getattr 및 readdir)을 활용합니다. 읽기/쓰기 오퍼레이션에 추가하여 이러한 오퍼레이션은 IOPS로 계수될 수 있으며, 이는 오퍼레이션 유형 및 NFS 버전에 따라 다양할 수 있습니다. 
- 기술적으로는 다수의 볼륨을 함께 스트립하여 더 높은 IOPS와 더 많은 처리량을 얻을 수 있지만, VMware에서는 성능 저하를 막기 위해 볼륨당 단일 VMFS(Virtual Machine File System) 데이터 저장소를 권장합니다. 
- {{site.data.keyword.filestorage_short}} 볼륨은 권한 부여된 디바이스, 서브넷 또는 IP 주소에 노출됩니다. 
- 스냅샷 및 복제 서비스는 기본적으로 Endurance {{site.data.keyword.filestorage_short}} 볼륨에서만 사용 가능합니다. Performance {{site.data.keyword.filestorage_short}}에는 이러한 기능이 없습니다. 
- 경로 장애 복구 중에 스토리지 단절을 피하기 위해 {{site.data.keyword.IBM}}에서는 적합한 제한시간 값을 설정하는 VMWare 도구를 설치하도록 권장합니다. 값을 변경할 필요는 없으며, 기본 설정으로도 VMWare 호스트의 연결이 끊기지 않을 만큼 충분합니다. 
- NFS v3 및 NFS v4.1 모두는 {{site.data.keyword.BluSoftlayer_full}} 환경에서 지원됩니다. 그러나 {{site.data.keyword.IBM}}에서는 NFS v3을 사용하도록 권장합니다. NFS v4.1이 Stateless 프로토콜이므로(NFSv3과 같은 Stateless가 아님), 프로토콜 문제가 네트워크 이벤트 중에 발생할 수 있습니다. NFS v4.1은 오퍼레이션을 거부한 후에 잠금 교정을 수행해야 합니다. 이러한 오퍼레이션이 실행되는 동안 장애가 발생할 수 있습니다. 

####  NFS 프로토콜 VMware 기능 지원 매트릭스. 
<table>
 <tbody>
  <tr>
   <th>vSphere 기능</th>
   <th>NFS 버전 3</th>
   <th>NFS 버전 4.1</th>
  </tr>
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
*소스: [VMWare - NFS 프로토콜 및 ESXi](https://docs.vmware.com/en/VMware-vSphere/6.0/com.vmware.vsphere.storage.doc/GUID-8A929FE4-1207-4CC5-A086-7016D73C328F.html){:new_window}*





### 2. Endurance {{site.data.keyword.filestorage_short}} 스냅샷

Endurance {{site.data.keyword.filestorage_short}}를 사용하여 관리자는 각각의 스토리지 볼륨에 대해 자동으로 스냅샷 사본을 작성하고 삭제하는 스냅샷 스케줄을 설정할 수 있습니다. 또한 관리자는 자동 스냅샷에 대해 추가로 스냅샷 스케줄(시간별, 일별, 주별)을 작성하고 비즈니스 연속성 및 재해 복구(BCDR) 시나리오에 대한 임시 스냅샷을 수동으로 작성할 수도 있습니다. 자동 경보는 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}을 통해 보유 중인 스냅샷과 이용된 영역의 볼륨 소유자에게 전달됩니다. 

스냅샷을 사용하려면 “스냅샷 영역”이 필요하다는 점을 유념하십시오. 영역은 조치 드롭 다운 단추를 클릭하고 **스냅샷 영역 추가**를 선택하여 **볼륨 세부사항** 페이지를 통해 초기 프로비저닝 후에 또는 초기 볼륨 주문 시에 확보될 수 있습니다. 

참고로, VMware 환경은 스냅샷을 인지하지 않음을 유념하십시오. Endurance {{site.data.keyword.filestorage_short}} 스냅샷 기능을 VMware 스냅샷과 혼동하지 않아야 합니다. Endurance {{site.data.keyword.filestorage_short}} 스냅샷 기능을 사용한 복구는 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}에서 처리되어야 합니다. Endurance {{site.data.keyword.filestorage_short}} 볼륨을 복원하려면 프로세스 중에 데이터 손상을 방지하기 위해 Endurance {{site.data.keyword.filestorage_short}}에 상주하는 모든 VM의 전원을 끄고 ESXi 호스트의 볼륨을 임시로 마운트 해제해야 합니다. 

스냅샷 구성 방법에 대한 세부사항은 [스냅샷](snapshots.html) 기사를 참조하십시오. 


### 3. 파일 저장소 복제

복제는 스냅샷 스케줄 중 하나를 사용하여 스냅샷을 원격 데이터 센터의 대상 볼륨으로 자동으로 복사합니다. 사본은 데이터 손상이나 대형 참사가 발생할 때 원격 사이트에서 복구될 수 있습니다. 


복제본을 사용하여 다음을 수행할 수 있습니다. 

- 대상 볼륨으로 장애 복구하여 사이트 장애와 기타 장애에서 빠르게 복구합니다. 
- DR 사본의 특정 시점으로 장애 복구합니다. 

복제하려면 우선 스냅샷 스케줄을 작성해야 합니다. 장애 복구 시에 사용자는 기본 데이터 센터의 스토리지 볼륨에서 원격 데이터 센터의 대상 볼륨으로 “스위치를 전환”합니다. 예를 들어, 기본 데이터 센터는 런던이고 보조 데이터 센터는 암스테르담입니다. 장애가 발생하면 암스테르담으로 장애 복구가 실행되며, 암스테르담의 vSphere 클러스터 인스턴스의 현재 기본 볼륨으로 연결됩니다. 


런던의 볼륨이 복구된 후에는 런던으로 장애 복구하기 위해 암스테르담 볼륨을 가져오고 런던의 컴퓨팅 인스턴스에서 기본 볼륨을 재차 가져옵니다. 볼륨이 기본 데이터 센터로 장애 복구되기 전에 원격 사이트에서 이의 사용을 중지해야 합니다. 신규 또는 변경된 정보의 스냅샷이 작성되며, 프로덕션 사이트 ESXi 호스트에 다시 마운트될 수 있기 전에 기본 데이터 센터로 복제됩니다. 


복제 구성 방법에 대한 세부사항은 [복제](replication.html) 정보 페이지를 참조하십시오. 

**참고**: 손상되거나 해킹되거나 감염되었는지 여부와 무관하게 올바르지 않은 데이터는 스냅샷 스케줄 및 스냅샷 보유에 따라 복제됩니다. 최소 복제 창을 사용하면 보다 나은 복구 지점 목표가 제공됩니다. 또한 이에 따라 올바르지 않은 데이터의 복제에 대해 반응하는 시간도 단축될 수 있습니다. 





## {{site.data.keyword.filestorage_short}} 주문

VMware ESXi 5 환경에 맞는 {{site.data.keyword.filestorage_short}}를 주문하고 구성할 수 있습니다. 다음 정보를 고급 단일 사이트 VMware 참조 아키텍처와 결합하여 사용하면 VMware 환경에서 이러한 스토리지 옵션 중 하나를 설정할 수 있습니다. 


{{site.data.keyword.filestorage_short}}는 **스토리지** > **{{site.data.keyword.filestorage_short}}**를 통해 {{site.data.keyword.filestorage_short}} 페이지에 액세스하여 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}를 통해 주문이 가능합니다. 


### 1. {{site.data.keyword.filestorage_short}} 주문

다음 단계를 사용하여 {{site.data.keyword.filestorage_short}}를 주문할 수 있습니다. 
1. [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 홈 페이지에서 **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하십시오. 
2. **{{site.data.keyword.filestorage_short}}** 페이지에서 **{{site.data.keyword.filestorage_short}} 주문** 링크를 클릭하십시오. 
3. 스토리지 유형 선택 드롭 다운 목록에서 **Endurance**/**Performance**를 선택하십시오. 
4. 위치를 선택하십시오. 개선된 기능의 데이터 센터는 `*`로 표기됩니다. 이전에 주문된 ESXi 호스트와 동일한 위치에 새 스토리지가 추가되는지 확인하십시오. 
5. 청구 방법을 선택하십시오. 월별 및 시간별 청구 옵션을 사용할 수 있습니다. 
6. 원하는 양의 스토리지 영역(GB)을 선택하십시오. TB의 경우, 1TB는 1,000GB이며 12TB는 12,000GB입니다. 
7. 원하는 양의 IOPS를 100 간격으로 입력하거나 IOPS 계층을 선택하십시오. 
8. 스냅샷 영역의 크기를 지정하십시오. 
9. **계속**을 클릭하십시오. 
10. 프로모션 코드를 입력하고(있는 경우) **재계산**을 클릭하십시오. 
11. 주문을 검토하십시오.  
12. **마스터 서비스 계약을 읽었으며 해당 이용 약관에 동의합니다** 선택란을 선택하십시오. 
13. **주문하기**를 클릭하여 주문을 제출하거나 **취소**를 클릭하여 주문을 제출하지 않고 양식을 닫으십시오. 

스토리지는 1분 내에 프로비저닝되며 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}의 **{{site.data.keyword.filestorage_short}}** 페이지에 나타납니다. 



### 2. 호스트에 {{site.data.keyword.filestorage_short}} 사용 권한 부여

일단 볼륨이 프로비저닝되면, 스토리지에 대한 액세스 권한이 볼륨을 사용할 {{site.data.keyword.BluBareMetServers_full}} 또는 {{site.data.keyword.BluVirtServers_full}}에 부여되어야 합니다. 다음 단계를 사용하여 볼륨에 권한을 부여할 수 있습니다. 

1. **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하십시오. 
2. **Endurance** 또는 **Performance 볼륨 조치** 메뉴에서 **호스트 액세스**를 선택하십시오. 
3. **서브넷** 단일 선택 단추를 선택하십시오. 
4. ESXi 호스트의 vmkernel 포트에 지정된 사용 가능한 서브넷의 목록에서 선택하고 **제출**을 클릭하십시오. <br/> **참고**: 표시된 서브넷은 스토리지 볼륨과 동일한 데이터 센터에 등록된 서브넷입니다. 


서브넷에 권한이 부여된 후에는 볼륨 마운트 시에 활용할 Endurance 또는 Performance 스토리지 서버의 호스트 이름을 기록하십시오. 이 정보는 특정 볼륨을 클릭하여 {{site.data.keyword.filestorage_short}} 세부사항 페이지에서 찾을 수 있습니다. 


##  VMware 가상 머신 호스트 구성

### 1. 전제조건

VMware 구성 프로세스를 시작하기 전에, 다음 전제조건이 충족되는지 확인하십시오. 

- VMware ESXi의 {{site.data.keyword.BluBareMetServers}}가 적절한 스토리지 구성과 ESXi 로그인 신임 정보로 프로비저닝되었습니다. 
- {{site.data.keyword.BluBareMetServers}}와 동일한 데이터 센터의 {{site.data.keyword.BluSoftlayer_full}} Windows 실제 또는 {{site.data.keyword.virtualmachinesshort}}. 로그인 신임 정보 및 {{site.data.keyword.BluSoftlayer_full}} Windows VM의 공인 IP 주소가 포함됩니다. 
- 인터넷 액세스가 가능하며 웹 브라우저 소프트웨어와 RDP(Remote Desktop Protocol) 클라이언트가 설치된 컴퓨터. 


### 2. VMware 호스트 구성 단계.

가상 호스트를 구성하려면 다음 단계를 완료하십시오. 

1. 인터넷에 연결된 컴퓨터에서 RDP 클라이언트를 실행하고 vSphere vCenter가 설치된 동일한 데이터 센터에서 프로비저닝된 {{site.data.keyword.BluVirtServers_full}}에 대한 RDP 세션을 설정하십시오. 
2. {{site.data.keyword.BluVirtServers_short}}에서 웹 브라우저를 실행하고 vSphere 웹 클라이언트를 통해 VMware vCenter에 연결하십시오. 
3. **홈** 화면에서 **호스트 및 클러스터**를 선택하십시오. 
왼쪽의 패널을 펼치고 이 배치에 사용될 **VMware ESXi 서버**를 선택하십시오. 
4. vSphere 호스트에서 NFS 클라이언트를 구성할 수 있도록 NFS 클라이언트의 방화벽 포트가 모든 호스트에서 열려 있는지 확인하십시오. 이는 vSphere의 보다 최신 릴리스에서는 자동으로 열립니다. 포트가 열려 있는지 확인하려면 VMware® vCenter™의 **ESXi 호스트 관리** 탭으로 이동하고 **설정**을 선택한 후에 **보안 프로파일**을 선택하십시오. **방화벽** 섹션에서 **편집**을 클릭하고 **NFS 클라이언트**로 화면을 하향 이동하십시오. 
5. **IP 주소 또는 IP 주소 목록에서 연결 허용**이 제공되는지 확인하십시오. <br/>
   ![연결 허용](/images/1_4.png)
6. **ESXi 호스트 관리** 탭으로 이동하여 Jumbo 프레임을 구성하고 **관리**를 선택한 후에 **네트워킹**을 선택하십시오. 
7. **VMkernel 어댑터**를 선택하고 **vSwitch**를 강조표시한 후에 **편집**을 클릭하십시오. (연필 아이콘)
8. **NIC 설정**을 선택하고 NIC MTU가 9000으로 설정되었는지 확인하십시오. 
   - 필요하면 MTU를 9000으로 설정하고 **확인**을 클릭하십시오. 
9. 원하면 Jumbo 프레임 설정을 다음과 같이 유효성 검증할 수 있습니다. 
   - Windows에서: ping -f -l 8972 a.b.c.d
   - Unix에서: ping -s 8972 a.b.c.d
   여기서 a.b.c.d는 명령에서 인접 {{site.data.keyword.BluVirtServers_short}} 인터페이스입니다.
   출력은 다음과 유사하게 나타납니다. 
   ```ping a.b.c.d (a.b.c.d) 8972(9000) bytes of data.
   8980 bytes from a.b.c.d: icmp_seq=1 ttl=128 time=3.36 ms
   ```

VMware 및 Jumbo 프레임에 대한 자세한 정보는 [여기](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1003712){:new_window}에서 찾을 수 있습니다. 


### 3. 가상 스위치에 업링크 어댑터 추가

1. **ESXi 호스트 관리** 탭으로 이동하여 새 업링크 어댑터를 구성하고 **관리**를 선택한 후에 **네트워킹**을 선택하십시오. 
2. **실제 어댑터** 탭을 선택하십시오. 
3. **호스트 네트워킹 추가**를 클릭하십시오. (더하기 부호의 공 아이콘)
4. 연결 유형을 **실제 네트워크 어댑터**로 선택하고 **다음**을 클릭하십시오. 
5. 기존 **vSwitch**를 선택하고 **다음**을 클릭하십시오. 
6. **미사용 어댑터**를 선택하고 **어댑터 추가**를 클릭하십시오. (더하기 부호)
7. 기타 "연결된" 어댑터를 클릭하고 **확인**을 클릭하십시오. <br/>
   ![스위치에 실제 어댑터 추가](/images/2_3.png)
8. **다음**을 클릭하고 **완료**를 클릭하십시오. 
9. 다시 **가상 스위치** 탭으로 이동하고 **가상 스위치** 표제 아래에서 상단의 **설정 편집** 아이콘을 선택하십시오. (연필 아이콘)
10. 왼쪽의 장애 복구 항목과 **vSwitch 팀**을 선택하십시오.
**로드 밸런싱** 옵션이 **원래 가상 포트를 기반으로 라우트**로 설정되었는지 확인하고 **확인**을 클릭하십시오. 


### 4. ESXi 정적 라우팅 구성(선택사항)

이 아키텍처 안내서의 네트워크 구성에서는 최소 개수의 포트 그룹을 사용합니다. NFS 스토리지의 VMkernal 포트 그룹을 설정했으면 추가 단계를 수행해야 합니다. 기본적으로, ESXi는 NFS 볼륨과 동일한 서브넷에 있는 vmkernel 포트를 사용하여 Endurance/Performance 스토리지에 NFS 볼륨을 마운트합니다. NFS 볼륨을 마운트하기 위해 계층 3 라우팅을 사용 중이므로, ESXi에서 NFS 볼륨을 마운트하도록 구성된 vmkernel 포트를 사용하도록 강제해야 합니다. 이를 수행하려면 Endurance/Performance 스토리지 어레이에 대한 정적 라우트를 작성해야 합니다. 

1. 정적 라우트를 구성하려면 Performance 또는 Endurance을 활용 중인 각 ESXi 호스트로 SSH를 실행하고 다음 명령을 실행하십시오. 참고로, 결과 IP 주소 Ping 명령(첫 번째 명령)을 가져와서 이를 아래에 표시된 esxcli 네트워크 명령과 함께 사용해야 합니다. 
   - `ping <hostname of the endurance/performance array>`

      **참고:** NFS 스토리지 DNS 호스트 이름은 다중 IP 주소가 지정된 FZ(Forwarding Zone)입니다. 이러한 IP 주소는 정적이며 해당 특정 DNS 호스트 이름에 속합니다. 해당 IP 주소를 사용하여 특정 볼륨에 액세스할 수 있습니다.
   - `esxcli network ip route ipv4 add –gateway GATEWAYIP –network <result of ping command>/32`

2. 참고로, 정적 라우트는 ESXi 5.0 이전에는 재부팅 간에 지속성이 없습니다. 추가된 정적 라우트의 지속성을 보장하려면 이러한 명령을 /etc/rc.local.d/ 디렉토리에 있는 각 호스트의 local.sh 파일에 추가해야 합니다. 이를 수행하려면 Visual Editor를 사용하여 local.sh 파일을 열고 exit 0 행 위에서 실행되도록 위의 명령을 추가해야 합니다. 
   - 다음 단계에서 볼륨을 마운트하는 데 사용될 수 있으므로 IP 주소를 기록하십시오. 
   - ESXi 호스트에 마운트할 각각의 NFS 볼륨마다 이 프로세스를 수행해야 합니다. 
   - VMware KB 기사에 대한 링크는 다음과 같습니다. [ESXi 호스트에서 VMkernel 포트에 대한 정적 라우트 구성](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2001426){:new_window}.


##  ESXi 호스트에 {{site.data.keyword.filestorage_short}} 볼륨 마운트

1. 웹 페이지의 상단에서 **vCenter로 이동** 아이콘을 클릭한 후에 **호스트 및 클러스터**를 클릭하십시오. 
2. **관련 오브젝트** 탭 아래에서 **데이터 저장소**를 클릭하십시오. **새 데이터 저장소 작성** 아이콘을 클릭하십시오. 
3. **새 데이터 저장소** 화면에서 데이터 저장소의 위치(ESXi 호스트)를 선택하고 **다음**을 클릭하십시오. 
4. **유형** 화면에서 **NFS** 단일 선택 단추를 선택하고 **다음**을 클릭하십시오. 
5. **이름 및 구성** 화면에서 데이터 저장소를 호출할 이름을 입력하십시오. 추가로 NFS 서버의 호스트 이름을 입력하십시오. NFS 서버에 대해 FQDN을 사용하면 기반 서버에 대한 최상의 트래픽 분배가 이루어집니다. IP 주소도 유효하긴 하지만 덜 자주 사용되며 특정 인스턴스에서만 사용됩니다. /foldername 양식으로 폴더 이름을 입력하십시오. 
6. **호스트 접근성** 화면에서 NFS 데이터 저장소가 마운트될 모든 호스트를 선택하고 **다음**을 클릭하십시오. 
7. 다음 화면에서 입력을 검토하고 **완료**를 클릭하십시오. 
8. 추가 {{site.data.keyword.filestorage_short}} 볼륨에 대해 반복 실행하십시오. 

**참고**: {{site.data.keyword.BluSoftlayer_full}}에서는 FQDN 이름을 사용하여 데이터 저장소에 연결하도록 권장합니다. 직접 IP 주소를 사용하면 FQDN을 사용하여 제공된 로드 밸런스 메커니즘이 무시될 수 있습니다. FQDN 대신에 IP 주소를 사용하려면 다음 명령을 실행하여 IP 주소를 가져오십시오. 

  - `ping <hostname of the endurance/performance array>`
  - ESXi 호스트에서: 
    ```
    ~ # vmkping nfsdal0902a-fz.service.softlayer.com
    PING nfsdal0902a-fz.service.softlayer.com (10.2.125.80): 56 data bytes
    64 bytes from 10.2.125.80: icmp_seq=0 ttl=253 time=0.187 ms
    10.2.125.80 is the IP address associated with the FQDN
    ```

## ESXi 스토리지 I/O 제어 설정 사용(선택사항)

SIOC(Storage I/O Control)는 Enterprise Plus 라이센스를 활용하는 고객이 사용할 수 있는 기능입니다. 해당 환경에서 사용될 때 SIOC는 단일 VM의 디바이스 큐 길이를 변경합니다. 디바이스 큐 길이가 변경되면 모든 VM의 스토리지 어레이 큐가 스토리지 큐의 동일 공유 및 제한으로 축소됩니다. SIOC는 리소스가 제한되고 스토리지 I/O 대기 시간이 정의된 임계값을 초과할 때만 개입합니다. 


SIOC에서 스토리지 디바이스의 정체된 또는 제한된 시점을 판별하려면 임계값 정의가 필요합니다. 정체 임계값 대기 시간은 각각의 스토리지 유형마다 다르며, 기본 선택 값은 최대 처리량의 90% 입니다. 최대 처리량 값의 백분율은 데이터 저장소가 예상 최대 처리량의 해당 백분율을 사용할 때의 예상 대기 시간 임계값을 표시합니다. 


참고로, VMDK 또는 데이터 저장소에 대한 SIOC 구성이 올바르지 않으면 성능에 상당한 악영향을 줄 수 있음을 유념하십시오. 



### 1. 데이터 저장소의 SIOC(Storage I/O Control)

다음 단계를 사용하여 Endurance 및 Performance 스토리지의 권장 값으로 SIOC를 사용할 수 있습니다. 

1. vSphere 웹 클라이언트 네비게이터의 데이터 저장소를 찾아보십시오. 
2. **관리** 탭을 클릭하십시오. 
3. **설정**을 클릭하고 **일반**을 클릭하십시오. 
4. **데이터 저장소 기능**에 대해 **편집**을 클릭하십시오. 
5. **SIOC(Storage I/O Control) 사용** 선택란을 선택하십시오. <br/>
   ![NSFDataStore](/images/3_0.png)
6. **확인**을 클릭하십시오. 

**참고**: 이 설정은 호스트가 아닌 데이터 저장소에 특정합니다. 


### 2. {{site.data.keyword.BluVirtServers_short}}의 SIOC(Storage I/O Control)

개별 VM에 대해 개별 가상 디스크를 제한하거나 SIOC의 서로 다른 공유를 이에 허용할 수도 있습니다. 디스크 제한과 서로 다른 공유 허용을 사용하면 확보된 {{site.data.keyword.filestorage_short}} 볼륨 IOPS 번호로 워크로드에 환경을 일치시키고 조정할 수 있습니다. 한계는 IOPS에 의해 설정되며 다른 가중치 또는 "공유"를 설정할 수도 있습니다. 공유가 높음(2,000 공유)으로 설정된 가상 디스크는 보통(1,000 공유)으로 설정된 디스크보다 2배의 I/O를 수신하며, 낮음(500 공유)으로 설정된 디스크보다 4배의 I/O를 수신합니다. 보통이 모든 VM의 기본값이므로, 이를 실제로 필요로 하는 VM에 대해 보통을 초과하거나 그 미만인 값은 조정이 필요합니다. 


다음 단계를 사용하여 vDisk 공유 및 한계를 변경할 수 있습니다. 

1. **VM 및 템플리트** 인벤토리에서 {{site.data.keyword.BluVirtServers_short}}를 선택하십시오. 아이콘은 웹 페이지의 왼쪽 상단에 있습니다. 
2. I/O 제어에 대해 {{site.data.keyword.BluVirtServers_short}}를 선택하십시오. 
3. **관리** 탭을 클릭하고 **설정**을 클릭하십시오. **편집**을 클릭하십시오. 
4. 하드 디스크 풀다운 화살표를 펼치십시오. 사용자 환경에 알맞게 공유 또는 한계 IOPS를 수정하십시오. 목록에서 가상 하드 디스크를 선택한 후에 공유 선택 값을 수정하여 {{site.data.keyword.BluVirtServers_short}}에 할당할 상대적 공유 양을 선택하십시오(낮음, 보통 또는 높음). **사용자 정의**를 클릭하고 사용자 정의된 공유 값을 입력할 수도 있습니다. 
5. 한계 - IOPS 열을 클릭하고 가상 머신에 할당할 스토리지 리소스의 상단 한계를 입력하십시오. 
6. **확인**을 클릭하십시오. 


   **참고**: 위의 프로세스는 SIOC가 사용되지 않아도 {{site.data.keyword.BluVirtServers_short}}에서 개별 vDisk의 리소스 이용 한계를 설정하는 데 사용됩니다. 이러한 설정은 SIOC에 의해 사용되는 경우에도 호스트가 아닌 개별 게스트에 특정합니다. 





## ESXi 호스트 측 설정

NFS 스토리지에 대해 ESXi 5.x 호스트를 설정는 데 필요한 일부 추가 설정이 있습니다. 

|매개변수 | 설정 ... |
|----------|------------|
|Net.TcpipHeapSize |	32 |
|Net.TcpipHeapMax |	vSphere 5.0/5.1의 경우 128로 설정 <br/> vSphere 5.5 이상의 경우 512로 설정 |
|NFS.MaxVolumes |	256 |
|NFS41.MaxVolumes |	256 (vSphere 6.0 이상에만 해당)|
|NFS.HeartbeatMaxFailures |	10 |
|NFS.HeartbeatFrequency |	12 |
|NFS.HeartbeatTimeout |	5 |
|NFS.MaxQueueDepth|	64 |


- 고급 구성 매개변수에 대해 ESXi 5.x 호스트에서 CLI 사용:
  다음 예제는 CLI를 사용하여 이러한 고급 구성 매개변수를 설정한 후에 이를 확인합니다. 예제에서 사용되는 esxcfg-advcfg 도구는 ESXi 5.x 호스트의 /usr/sbin 디렉토리에서 찾을 수 있습니다. 

   - ESXi 5.x CLI에서 고급 구성 매개변수 설정: 
   ```
   #esxcfg-advcfg -s 32 /Net/TcpipHeapSize
   #esxcfg-advcfg -s 128 /Net/TcpipHeapMax(For vSphere 5.0/5.1)
   #esxcfg-advcfg -s 512 /Net/TcpipHeapMax(For vSphere 5.5 and above)
   #esxcfg-advcfg -s 256 /NFS/MaxVolumes
   #esxcfg-advcfg -s 256 /NFS41/MaxVolumes (ESXi 6.0 and above)
   #esxcfg-advcfg -s 10 /NFS/HeartbeatMaxFailures
   #esxcfg-advcfg -s 12 /NFS/HeartbeatFrequency
   #esxcfg-advcfg -s 5 /NFS/HeartbeatTimeout   
   #esxcfg-advcfg -s 64 /NFS/MaxQueueDepth
   #esxcfg-advcfg -s 32 /Disk/QFullSampleSize
   #esxcfg-advcfg -s 8 /Disk/QFullThreshold
   ```

- ESXi 5.x CLI에서 고급 구성 매개변수 확인: 
   ```
   #esxcfg-advcfg -g /Net/TcpipHeapSize
   #esxcfg-advcfg -g /Net/TcpipHeapMax
   #esxcfg-advcfg -g /NFS/MaxVolumes
   #esxcfg-advcfg -g /NFS41/MaxVolumes (ESXi 6.0 and above)
   #esxcfg-advcfg -g /NFS/HeartbeatMaxFailures
   #esxcfg-advcfg -g /NFS/HeartbeatFrequency
   #esxcfg-advcfg -g /NFS/HeartbeatTimeout
   #esxcfg-advcfg -g /NFS/MaxQueueDepth
   #esxcfg-advcfg -g /Disk/QFullSampleSize
   #esxcfg-advcfg -g /Disk/QFullThreshold
   ```


## SoftLayer에서 Jumbo 프레임 사용 – Windows 및 Linux

{{site.data.keyword.BluSoftlayer_full}}에서는 충분한 속도를 스토리지에서 실현하기 위해 9,000 MTU에서 Jumbo 프레임을 사용해야 한다고 설명했습니다. 


Jumbo 프레임은 1,500 바이트의 표준 MTU(Maximum Transmission Unit)보다 큰 페이로드의 이더넷 프레임입니다. Jumbo 프레임은 최소한 1Gbps를 지원하는 근거리 통신망에서 사용되며 최대 9,000바이트 크기까지 가능합니다. 


Jumbo 프레임은 소스 디바이스 <-> 스위치 <-> 라우터 <-> 스위치 <-> 대상 디바이스의 전체 네트워크 경로에서 동일하게 구성되어야 합니다. 전체 체인이 동일하게 설정되지 않은 경우, 기본값은 체인의 최하위 설정입니다. SoftLayer의 네트워크 디바이스는 현재 9,000으로 설정되어 있습니다. 모든 고객 디바이스는 동일하게 설정되어야 합니다. 

### Windows

1. **네트워크 및 공유 센터**를 여십시오. 
2. **어댑터 설정 변경**을 클릭하십시오.
3. Jumbo 프레임을 사용할 NIC에 마우스 오른쪽 단추를 클릭하고 **특성**을 선택하십시오. 
4. **네트워킹** 탭 아래에서 네트워크 어댑터에 대한 **구성**을 클릭하십시오. 
5. **고급** 탭을 선택하십시오. 
6. **Jumbo 프레임**을 선택하고 NIC에 따라 "사용 안함" 값을 원하는 값으로 변경하십시오(예: 9kB MTU 또는 9,014 바이트). 
7. 모든 대화 상자에 대해 **확인**을 클릭하십시오. 

참고로, 변경 시에는 수 초 동안 NIC의 네트워크 연결이 끊깁니다. 또한 재부팅을 실행하여 변경사항을 적용해야 합니다. 



### LINUX

1. eth0 인터페이스에 대한 네트워크 구성 파일 편집 – 예: /etc/sysconfig/network-script/ifcfg-eth0 (CentOS / RHEL / Fedora Linux):
`# vi /etc/sysconfig/network-script/ifcfg-eth0`

2. 프레임 크기(바이트)를 지정하는 다음의 구성 지시문 추가: MTU 9000

3. 파일을 닫고 저장하십시오. 인터페이스 eth0를 다시 시작하십시오.
`# /etc/init.d/networking restart (This will cause a brief loss of network connectivity)`


Debian / Ubuntu Linux 사용자 관련 참고사항:
Debian / Ubuntu Linux 사용자는 MTU=9000을 /etc/network/interfaces 구성 파일에 추가해야 합니다. 

[여기서](https://console.bluemix.net/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){:new_window} 고급 단일 사이트 VMware 참조 아키텍처에 대해 자세히 알아보십시오. 
