---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# VMware에서 {{site.data.keyword.filestorage_short}}에 대한 아키텍처 안내서

다음은 {{site.data.keyword.BluSoftlayer_full}}의 vSphere 5.5 및 vSphere 6.0 환경에서 {{site.data.keyword.filestorage_full}}를 주문하고 구성하는 단계입니다. NFS {{site.data.keyword.filestorage_short}}의 사용은 VMware 호스트에 대해 8개가 넘는 연결이 필요한 경우 우수 사례입니다. 

## {{site.data.keyword.filestorage_short}} 개요

{{site.data.keyword.filestorage_short}}는 예측 가능한 성능 레벨을 요구하는 I/O가 높은 애플리케이션을 지원하도록 설계되었습니다. 예측 가능한 성능은 개별 볼륨에 프로토콜 레벨 초당 입/출력 오퍼레이션(IOPS)을 할당하여 얻을 수 있습니다.  

{{site.data.keyword.filestorage_short}} 오퍼링은 NFS 연결을 통해 액세스 및 마운트됩니다. VMware 배치에서 단일 볼륨은 공유 스토리지로 최대 64개 ESXi 호스트에 마운트하거나 스토리지 클러스터를 작성하여 vSphere Storage DYNAMIC 리소스 스케줄링을 활용하도록 여러 볼륨을 마운트할 수 있습니다. 

Endurance 및 Performance {{site.data.keyword.filestorage_short}}에 대한 가격 책정 및 구성 옵션은 제공된 IOPS 및 예약된 공간의 조합에 기반하여 비용이 청구됩니다. 

### 1. {{site.data.keyword.filestorage_short}} 고려사항

{{site.data.keyword.filestorage_short}} 주문 시 다음 정보 및 고려사항을 염두에 두어야 합니다. 

- 스토리지 크기, IOPS, 운영 체제는 {{site.data.keyword.filestorage_short}} 볼륨을 프로비저닝한 후에 변경할 수 없습니다. 공간 크기, IOPS 수 또는 운영 체제에 대한 변경사항을 적용하려면 새 볼륨을 프로비저닝해야 합니다. 이전 볼륨에 저장된 데이터는 VMware Storage vMotion을 사용하여 새 볼륨으로 마이그레이션해야 합니다. 
- 크기를 결정하기 전에 필요한 처리량과 워크로드의 크기를 고려하십시오. 크기는 Endurance 서비스에서 중요합니다. 여기에서는 Performance 서비스와 반대로 용량에 선형으로 비례하여 성능을 스케일링합니다(IOPS/GB). 반면 Performance 서비스에서는 관리자가 용량과 성능을 독립적으로 선택할 수 있습니다. 처리량 요구사항은 Performance에서 중요합니다. <br/> **참고**: 처리량 계산 방법은 IOPS x 16KB입니다. IOPS는 50/50 읽기/쓰기를 혼합하여 16KB 블록 크기에 기반하여 측정됩니다. <br/> **참고**: 블록 크기를 늘리면 처리량은 증가하지만 IOPS는 감소합니다. 예를 들어, 블록 크기를 32KB 블록으로 2배 늘리면 최대 처리량을 유지보수하지만 IOPS는 절반으로 감소합니다. 
- NFS는 여러 추가 파일 제어 오퍼레이션(예: 검색, getattr, readdir)을 활용하여 몇 가지 이름을 지정합니다. 읽기/쓰기 오퍼레이션 외에도 이러한 오퍼레이션은 IOPS로 계산될 수 있으며, 오퍼레이션 유형 및 NFS 버전에 따라 달라집니다. 
- 기술적으로 여러 볼륨을 함께 스트라이프하면 더 높은 IOPS와 더 많은 처리량을 얻을 수 있지만, VMware에서는 성능 저하를 방지하기 위해 볼륨당 하나의 가상 머신 파일 시스템(VMFS) 데이터 저장소를 권장합니다. 
- {{site.data.keyword.filestorage_short}} 볼륨은 권한 부여된 디바이스, 서브넷 또는 IP 주소로 노출됩니다. 
- 스냅샷 및 복제 서비스는 기본적으로 Endurance {{site.data.keyword.filestorage_short}} 볼륨에서만 사용 가능합니다. Performance {{site.data.keyword.filestorage_short}}에는 이러한 기능이 없습니다. 
- 경로 장애 복구 중 스토리지 연결이 끊어지지 않도록 하기 위해 {{site.data.keyword.IBM}}에서는 적절한 제한시간 값을 설정하는 VMware 도구를 설치하도록 권장합니다. 값을 변경하지 않아도 되며 기본 설정으로도 VMware 호스트에서 연결성을 유실하지 않도록 보장할 수 있습니다. 
- NFS v3 및 NFS v4.1 모두 {{site.data.keyword.BluSoftlayer_full}} 환경에서 지원됩니다. 하지만 {{site.data.keyword.IBM}}에서는 NFS v3을 사용하도록 권장합니다. NFS v4.1은 stateful 프로토콜(NFSv3과 같이 stateless가 아님)이므로, 네트워크 이벤트 중에 프로토콜 문제가 발생할 수 있기 때문입니다. NFS v4.1은 오퍼레이션을 중지하고 잠금 교정을 수행해야 합니다. 이러한 오퍼레이션을 수행하는 동안 작업이 중단될 수 있습니다. 

####  NFS 프로토콜 VMware 기능은 매트릭스를 지원합니다. 
<table>
 <tbody>
  <tr>
   <th>vSphere 기능</th>
   <th>NFS 버전 3</th>
   <th>NFS 버전 4.1</th>
  </tr>
  <tr>
   <td>vMotion 및 Storage vMotion</td>
   <td>예</td>
   <td>예</td>
  </tr>
  <tr>
   <td>고가용성(HA)</td>
   <td>예</td>
   <td>예</td>
  </tr>
  <tr>
   <td>결함 허용(FT)</td>
   <td>예</td>
   <td>예</td>
  </tr>
  <tr>
   <td>DRS(Distributed Resource Scheduler: 분배 리소스 스케줄러)</td>
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

Endurance {{site.data.keyword.filestorage_short}}에서는 관리자가 각 스토리지 볼륨에 대해 자동으로 스냅샷 사본을 작성하고 삭제하는 스냅샷 스케줄을 설정할 수 있습니다. 또한 자동 스냅샷에 대한 추가 스냅샷 스케줄(시간별, 일별, 주별)을 작성하고 비즈니스 연속성 및 재해 복구(BCDR) 시나리오에 대한 임시 스냅샷을 수동으로 작성할 수 있습니다. 볼륨 소유자에게 보유된 스냅샷 및 이용한 공간에 대한 자동 경보가 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}을 통해 전달됩니다. 

스냅샷을 사용하려면 “스냅샷 영역”이 필요합니다. 공간은 초기 볼륨 주문 또는 **볼륨 세부사항** 페이지에서 조치 드롭 다운 단추를 클릭하고 **스냅샷 영역 추가**를 선택하여 초기 프로비저닝 이후에 얻을 수 있습니다. 

VMware 환경은 스냅샷을 인식하지 않는다는 점이 중요합니다. Endurance {{site.data.keyword.filestorage_short}} 스냅샷 기능은 VMware 스냅샷과 혼동해서는 안 됩니다. Endurance {{site.data.keyword.filestorage_short}} 스냅샷 기능을 사용하는 복구는 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}에서 처리되어야 합니다. Endurance {{site.data.keyword.filestorage_short}} 볼륨 복원 시 Endurance {{site.data.keyword.filestorage_short}}에 상주하는 모든 VM의 전원을 꺼야 하며, 프로세스 중 데이터 손상을 방지하기 위해 ESXi 호스트에서 볼륨을 임시로 마운트 해제해야 합니다. 

스냅샷 구성 방법에 대한 자세한 정보는 [스냅샷](snapshots.html) 기사를 참조하십시오. 
 

### 3. 파일 저장소 복제

복제는 스냅샷 스케줄 중 하나를 사용하여 원격 데이터 센터의 대상 볼륨으로 스냅샷을 자동으로 복사합니다. 사본은 손상된 데이터 또는 재해 발생 시 원격 사이트에서 복구할 수 있습니다. 


복제본을 통해 다음을 수행할 수 있습니다. 

- 대상 볼륨에 대한 장애 조치를 통해 사이트 장애 및 기타 재해로부터 빠르게 복구하십시오. 
- DR 사본에서 특정 시점으로 장애 복구하십시오. 

복제하려면 먼저 스냅샷 스케줄을 작성해야 합니다. 장애 복구 시 기본 데이터 센터의 스토리지 볼륨에서 원격 데이터 센터의 대상 볼륨으로 “스위치를 전환”합니다. 예를 들어, 기본 데이터 센터는 런던이고, 보조 데이터 센터는 암스테르담입니다. 장애 발생 시 암스테르담으로 장애 복구되며, 암스테르담의 vSphere 클러스터 인스턴스에서 이제 기본 볼륨으로 연결됩니다. 


런던의 볼륨이 복구되면 런던으로 대체하기 위해 암스테르담 볼륨에 대한 스냅샷이 작성되고 런던의 컴퓨팅 인스턴스에서 다시 한 번 기본 볼륨이 됩니다. 볼륨이 기본 데이터 센터로 대체되기 전에 원격 사이트에서 사용을 중지해야 합니다. 새 정보 또는 변경된 정보의 스냅샷이 작성되고 프로덕션 사이트 ESXi 호스트에서 다시 마운트하기 전에 기본 데이터 센터로 복제됩니다. 


복제 구성 방법에 대한 자세한 정보는 [복제](replication.html) 정보 페이지를 참조하십시오. 

**참고**: 올바르지 않은 데이터(손상, 해킹 또는 감염되었을 수 있음)는 스냅샷 스케줄 및 스냅샷 보존에 따라 복제됩니다. 더 나은 복구 지점 목표를 위해 가장 작은 복제 기간을 사용할 수 있습니다. 또한 올바르지 않은 데이터의 복제에 대응할 시간이 더 줄어들 수 있습니다. 

 

 

## {{site.data.keyword.filestorage_short}} 주문

VMware ESXi 5 환경에 대해 {{site.data.keyword.filestorage_short}}를 주문 및 구성할 수 있습니다. Advanced Single-Site VMware Reference Architecture와 함께 다음 정보를 사용하여 VMware 환경에서 이러한 스토리지 옵션 중 하나를 설정하십시오. 


{{site.data.keyword.filestorage_short}}는 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}을 통해 **스토리지** > **{{site.data.keyword.filestorage_short}}**를 사용하여 {{site.data.keyword.filestorage_short}} 페이지에 액세스해서 주문할 수 있습니다. 
 

### 1. {{site.data.keyword.filestorage_short}} 주문

다음 단계를 사용하여 {{site.data.keyword.filestorage_short}}를 주문하십시오. 
1. [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 홈 페이지에서 **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하십시오. 
2. **{{site.data.keyword.filestorage_short}}** 페이지에서 **{{site.data.keyword.filestorage_short}} 주문** 링크를 클릭하십시오. 
3. 스토리지 유형 선택 드롭 다운 목록에서 **Endurance**/**Performance**를 선택하십시오. 
4. 위치를 선택하십시오. 향상된 기능의 데이터 센터는 `*`로 표시됩니다. 이전에 주문한 ESXi 호스트와 동일한 위치에 새 스토리지가 추가되는지 확인하십시오.  
5. 청구 방법을 선택하십시오. 월별 및 시간별 청구 옵션을 사용할 수 있습니다. 
6. 원하는 스토리지 공간 크기(GB)를 선택하십시오. TB의 경우 1TB는 1,000GB와 같고, 12TB는 12,000GB와 같습니다. 
7. 100의 간격으로 원하는 IOPS 크기를 입력하거나 IOPS 티어를 선택하십시오. 
8. 스냅샷 영역 크기를 지정하십시오. 
9. **계속**을 클릭하십시오. 
10. 프로모션 코드를 입력(있는 경우)하고 **다시 계산**을 클릭하십시오. 
11. 주문을 검토하십시오. 
12. **마스터 서비스 계약을 읽었으며 계약의 조건에 동의합니다.** 선택란을 선택하십시오. 
13. 주문을 제출하려면 **주문하기**를 클릭하거나 주문을 제출하지 않고 양식을 닫으려면 **취소**를 클릭하십시오. 

스토리지는 몇 분 안에 프로비저닝되며, [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}의 **{{site.data.keyword.filestorage_short}}** 페이지에 표시됩니다. 

 

### 2. 호스트에 {{site.data.keyword.filestorage_short}}를 사용할 권한 부여

볼륨이 프로비저닝되면 {{site.data.keyword.BluBareMetServers_full}} 또는 볼륨을 사용하는 {{site.data.keyword.BluVirtServers_full}}에 대해 스토리지에 액세스할 권한이 부여됩니다. 다음 단계를 사용하여 볼륨에 권한을 부여하십시오. 

1. **스토리지** > **{{site.data.keyword.filestorage_short}}**를 클릭하십시오. 
2. **Endurance** 또는 **Performance 볼륨 조치** 메뉴에서 **호스트에 액세스**를 선택하십시오. 
3. **서브넷** 단일 선택 단추를 선택하십시오. 
4. ESXi 호스트의 vmkernel 포트에 지정된 사용 가능한 서브넷의 목록에서 선택하고 **제출**을 클릭하십시오. <br/> **참고**: 표시되는 서브넷은 스토리지 볼륨과 동일한 데이터 센터의 등록된 서브넷입니다. 


서브넷에 권한을 부여한 후에 볼륨을 마운트할 때 활용하려는 Endurance 또는 Performance 스토리지 서버의 호스트 이름을 기록하십시오. 이 정보는 특정 볼륨을 클릭하여 {{site.data.keyword.filestorage_short}} 세부사항 페이지에서 찾을 수 있습니다. 

 
##  VMware 가상 머신 호스트 구성

### 1. 전제조건

VMware 구성 프로세스를 시작하기 전에 다음 전제조건이 충족되는지 확인하십시오. 

- {{site.data.keyword.BluBareMetServers}}(VMware ESXi 포함)는 적절한 스토리지 구성 및 ESXi 로그인 신임 정보를 사용하여 프로비저닝됩니다. 
- {{site.data.keyword.BluSoftlayer_full}} Windows 실제 또는 {{site.data.keyword.virtualmachinesshort}}({{site.data.keyword.BluBareMetServers}}와 동일한 데이터 센터에 있음). {{site.data.keyword.BluSoftlayer_full}} Windows VM 및 로그인 신임 정보의 공용 IP 주소를 포함합니다. 
- 인터넷에 액세스할 수 있으며, 웹 브라우저 소프트웨어 및 원격 데스크탑 프로토콜(RDP) 클라이언트가 설치된 컴퓨터. 
 

### 2. VMware 호스트 구성 단계. 

가상 호스트를 구성하려면 다음 단계를 완료하십시오. 

1. 인터넷에 연결된 컴퓨터에서 RDP 클라이언트를 실행하고 vSphere vCenter가 설치된 동일한 데이터 센터에서 프로비저닝된 {{site.data.keyword.BluVirtServers_full}}에 대한 RDP 세션을 설정하십시오. 
2. {{site.data.keyword.BluVirtServers_short}}에서 웹 브라우저를 실행하고 vSphere Web Client를 통해 VMware vCenter에 연결하십시오. 
3. **홈** 화면에서 **호스트 및 클러스터**를 선택하십시오. 왼쪽에서 패널을 확장하고 이 배치에 사용할 **VMware ESXi 서버**를 선택하십시오. 
4. vSphere 호스트에서 NFS 클라이언트를 구성하기 위해 NFS 클라이언트의 방화벽 포트가 모든 호스트에 대해 열려 있는지 확인하십시오. vSphere의 최근 릴리스에서 자동으로 열립니다. 포트가 열려 있는지 확인하려면 VMware® vCenter™의 **ESXi 호스트 관리** 탭으로 이동하고 **설정**을 선택하고 **보안 프로파일**을 선택하십시오. **방화벽** 섹션에서 **편집**을 클릭하고 아래로 스크롤하여 **NFS 클라이언트**로 이동하십시오. 
5. **IP 주소 또는 IP 주소 목록에서 연결 허용**이 제공되는지 확인하십시오. <br/>
   ![연결 허용](/images/1_4.png)
6. **ESXi 호스트 관리** 탭으로 이동하여 Jumbo Frame을 구성하고 **관리** 및 **네트워킹**을 선택하십시오. 
7. **VMkernel 어댑터**를 선택하고 **vSwitch**를 강조표시하고 **편집**을 클릭하십시오. (연필 아이콘)
8. **NIC 설정**을 선택하고 NIC MTU가 9000으로 설정되었는지 확인하십시오. 
   - 필요한 경우 MTU를 9000으로 설정하고 **확인**을 클릭하십시오. 
9. 원하는 경우 Jumbo Frame 설정은 다음과 같이 유효성을 검증할 수 있습니다. 
   - Windows의 경우: ping -f -l 8972 a.b.c.d
   - Unix의 경우: ping -s 8972 a.b.c.d
   여기서, a.b.c.d는 명령에 이웃한 {{site.data.keyword.BluVirtServers_short}} 인터페이스입니다.
   다음과 유사한 출력이 표시됩니다. 
   ```ping a.b.c.d (a.b.c.d) 8972(9000) bytes of data.
   8980 bytes from a.b.c.d: icmp_seq=1 ttl=128 time=3.36 ms
   ```

VMware 및 Jumbo Frames에 대한 자세한 정보는 [여기](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1003712){:new_window}에서 찾을 수 있습니다. 
 

### 3. 가상 스위치에 업링크 어댑터 추가

1. **ESXi 호스트 관리** 탭으로 이동하여 새 업링크 어댑터를 구성하고 **관리** 및 **네트워킹**을 선택하십시오. 
2. **실제 어댑터** 탭을 선택하십시오.  
3. **호스트 네트워킹 추가**를 클릭하십시오. (더하기 부호가 있는 지구본 아이콘)
4. 연결 유형을 **실제 네트워크 어댑터**로 선택하고 **다음**을 클릭하십시오.
5. 기존 **vSwitch**를 선택하고 **다음**을 클릭하십시오. 
6. **사용하지 않은 어댑터**를 선택하고 **어댑터 추가**를 클릭하십시오. (더하기 부호) 
7. 기타 "연결됨" 어댑터를 클릭하고 **확인**을 클릭하십시오. <br/>
   ![스위치에 실제 어댑터 추가](/images/2_3.png)
8. **다음**을 클릭하고 **완료**를 클릭하십시오. 
9. **가상 스위치** 탭으로 다시 이동하고 **가상 스위치** 표제 아래 상단 **설정 편집** 아이콘을 선택하십시오. (연필 아이콘)
10. 왼쪽에서 **vSwitch Teaming** 및 장애 복구 항목을 선택하십시오.
**로드 밸런싱** 옵션이 **시작 가상 포트에 기반한 라우트**로 설정되었는지 확인하고 **확인**을 클릭하십시오. 
 

### 4. ESXi 정적 라우팅 구성(선택사항)

이 아키텍처 안내서에서 네트워크 구성은 최소 포트 그룹 수를 사용합니다. NFS 스토리지에 대한 VMkernal 포트 그룹을 설정한 경우 추가 단계를 수행해야 합니다. 기본적으로 ESXi는 Endurance/Performance 스토리지에서 NFS 볼륨을 마운트하기 위해 NFS 볼륨과 동일한 서브넷에 있는 vmkernel 포트를 사용합니다. NFS 볼륨을 마운트하기 위해 레이어 3 라우팅을 사용하므로 ESXi에서 NFS 볼륨을 마운트하기 위해 구성된 vmkernel 포트를 사용하도록 강제 실행해야 합니다. 이를 수행하기 위해 Endurance/Performance 스토리지 어레이에 대한 정적 라우트를 작성해야 합니다. 

1. 정적 라우트를 구성하려면 Performance 또는 Endurance를 사용하는 각 ESXi 호스트에 대해 SSH를 수행하고 다음 명령을 실행하십시오. 결과 IP 주소 ping 명령(첫 번째 명령)을 수행하고 아래 표시된 esxcli network 명령과 함께 사용해야 합니다. 
   - `ping <hostname of the endurance/performance array>`
   
      **참고**: NFS 스토리지 DNS 호스트 이름은 여러 IP 주소에 지정된 FZ(Forwarding Zone)입니다. 이 IP 주소는 정적이며 특정 DNS 호스트 이름에 속합니다. 이러한 IP 주소는 특정 볼륨에 액세스하는 데 사용할 수 있습니다.
   - `esxcli network ip route ipv4 add –gateway GATEWAYIP –network <result of ping command>/32`

2. 정적 라우트는 ESXi 5.0 이하에서 재부팅 시 지속되지 않습니다. 추가된 정적 라우트가 지속적인지 보장하려면 각 호스트에서 local.sh 파일(/etc/rc.local.d/ 디렉토리에 있음)에 이 명령을 추가해야 합니다. 이를 수행하려면 Visual Editor를 사용하여 local.sh 파일을 열고 행 exit 0 위에서 실행하도록 위 명령을 추가하십시오. 
   - 다음 단계에서 볼륨 마운트에 사용할 수 있으므로 IP 주소를 기록하십시오. 
   - ESXi 호스트에 마운트하려는 각 NFS 볼륨에 대해 이 프로세스를 수행해야 합니다. 
   - 다음은 VMware KB 기사: [ESXi 호스트에서 VMkernel 포트에 대한 정적 라우트 구성](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2001426){:new_window}에 대한 링크입니다. 
 

##  ESXi 호스트에서 {{site.data.keyword.filestorage_short}} 볼륨 마운트

1. 웹 페이지 맨 위에 있는 **vCenter로 이동** 아이콘을 클릭하고 **호스트 및 클러스터**를 클릭하십시오. 
2. **관련 오브젝트** 탭 아래 **데이터 저장소**를 클릭하십시오. **새 데이터 저장소 작성** 아이콘을 클릭하십시오.  
3. **새 데이터 저장소** 화면에서 데이터 저장소 위치(ESXi 호스트)를 선택하고 **다음**을 클릭하십시오. 
4. **유형** 화면에서 **NFS** 단일 선택 단추를 선택하고 **다음**을 클릭하십시오. 
5. **이름 및 구성** 화면에서 데이터 저장소를 호출할 이름을 입력하십시오. 또한 NFS 서버에 대한 호스트 이름을 입력하십시오. NFS 서버의 FQDN을 사용하면 기본 서버에 대해 가장 효율적으로 트래픽을 분배할 수 있습니다. IP 주소도 유효하지만, 자주 사용되지 않으며 특정 인스턴스에서만 사용됩니다. /foldername 양식으로 폴더 이름을 입력하십시오. 
6. **호스트 접근성** 화면에서 NFS 데이터 저장소를 마운트하려는 모든 호스트를 선택하고 **다음**을 클릭하십시오. 
7. 다음 화면에서 입력을 검토하고 **완료**를 클릭하십시오. 
8. 추가 {{site.data.keyword.filestorage_short}} 볼륨에 대해서 반복하십시오. 

**참고**: {{site.data.keyword.BluSoftlayer_full}}에서는 데이터 저장소에 연결하는 데 FQDN 이름을 사용하도록 권장합니다. 직접 IP 주소를 사용하면 FQDN을 사용할 때 제공되는 로드 밸런싱 메커니즘을 우회할 수 있습니다. FQDN 대신, IP 주소를 사용하려면 다음 명령을 실행하여 IP 주소를 확보하십시오. 

  - `ping <hostname of the endurance/performance array>`
  - ESXi 호스트의 경우:
    ```
    ~ # vmkping nfsdal0902a-fz.service.softlayer.com
    PING nfsdal0902a-fz.service.softlayer.com (10.2.125.80): 56 data bytes
    64 bytes from 10.2.125.80: icmp_seq=0 ttl=253 time=0.187 ms
    10.2.125.80 is the IP address associated with the FQDN
    ```

## ESXi 스토리지 I/O 제어 설정 사용(선택사항)

스토리지 I/O 제어(SIOC)는 Enterprise Plus 라이센스를 이용하는 고객이 사용할 수 있는 기능입니다. SIOC가 환경에서 사용 가능하면 단일 VM에 대한 디바이스 큐 길이를 변경합니다. 디바이스 큐 길이가 변경되면 모든 VM의 스토리지 어레이 큐가 스토리지 큐와 동일한 공유 및 스로틀로 감소합니다. SIOC는 리소스가 제한되고 스토리지 I/O 대기 시간이 정의된 임계값을 초과하는 경우에만 적용됩니다.


스토리지 디바이스가 혼잡하거나 제한되는지를 SIOC에서 판별하려면 정의된 임계값이 필요합니다. 혼잡 임계값 대기 시간은 스토리지 유형마다 다릅니다. 기본 선택사항은 최대 처리량의 90%입니다. 최대 처리량 값의 백분율은 데이터 저장소가 예상되는 최대 처리량의 해당 백분율을 사용하는 경우 예상되는 대기 시간 임계값을 나타냅니다. 


VMDK 또는 데이터 저장소에 대해 SIOC를 잘못 구성하면 성능에 큰 영향을 줄 수 있다는 점을 명심해야 합니다. 

 

### 1. 데이터 저장소에 대한 스토리지 I/O 제어

다음 단계를 사용하여 Endurance 및 Performance 스토리지에 대해 권장되는 값으로 SIOC를 사용하십시오. 

1. vSphere Web Client 네비게이터에서 데이터 저장소를 찾아보십시오. 
2. **관리** 탭을 클릭하십시오. 
3. **설정**을 클릭하고 **일반**을 클릭하십시오. 
4. **데이터 저장소 기능**에 대한 **편집**을 클릭하십시오. 
5. **스토리지 I/O 제어 사용** 선택란을 선택하십시오. <br/>
   ![NSFDataStore](/images/3_0.png)
6. **확인**을 클릭하십시오. 

**참고**: 이 설정은 호스트가 아니라, 데이터 저장소에 특정합니다. 
 

### 2. {{site.data.keyword.BluVirtServers_short}}에 대한 스토리지 I/O 제어

또한 개별 VM에 대해 개별 가상 디스크를 제한하거나 SIOC로 서로 다른 공유를 부여할 수 있습니다. 다른 공유를 부여하고 디스크를 제한하면 확보한 {{site.data.keyword.filestorage_short}} 볼륨 IOPS 수와 워크로드에 대한 환경을 일치하고 맞출 수 있습니다. 한계는 IOPS에서 설정되며, 다른 가중치 또는 "공유"를 설정할 수 있습니다. 높음(2,000 공유)으로 설정된 가상 디스크 공유는 보통(1,000 공유)으로 설정된 디스크보다 2배, 낮음(500 공유)으로 설정된 디스크보다 4배 더 많은 I/O를 수신합니다. 보통은 모든 VM의 기본값이므로 실제로 필요한 VM에 대해 보통 미만 또는 보통을 초과하도록 값을 조정해야 합니다. 


다음 단계를 사용하여 vDisk 공유 및 한계를 변경하십시오. 

1. **VM 및 템플리트** 인벤토리에서 {{site.data.keyword.BluVirtServers_short}}를 선택하십시오. 아이콘은 웹 페이지의 맨 위 왼쪽에 있습니다.  
2. I/O 제어에 대해 {{site.data.keyword.BluVirtServers_short}}를 선택하십시오. 
3. **관리** 탭을 클릭하고 **설정**을 클릭하십시오. **편집**을 클릭하십시오. 
4. 하드 디스크 풀다운 화살표를 펼치십시오. 환경에 적절한 대로 공유 또는 제한-IOPS를 수정하십시오. 목록에서 가상 하드 디스크를 선택하고 공유 선택사항을 수정하여 {{site.data.keyword.BluVirtServers_short}}에 할당할 상대적인 공유 수(낮음, 보통 또는 높음)를 선택하십시오. 또한 **사용자 정의**를 클릭하고 사용자 정의 공유 값을 입력할 수 있습니다. 
5. 한계 - IOPS 열을 클릭하고 가상 머신에 할당할 스토리지 리소스의 상한을 입력하십시오. 
6. **확인**을 클릭하십시오. 
 

   **참고**: 위 프로세스는 SIOC가 사용되지 않는 경우에도 {{site.data.keyword.BluVirtServers_short}}에서 개별 vDisk의 리소스 이용 한계를 설정하는 데 사용됩니다. 이 설정은 SIOC를 사용해도 호스트가 아니라, 개별 게스트에 특정합니다. 

 

 

## ESXi 호스트 측 설정

다음은 NFS 스토리지에 대한 ESXi 5.x 호스트 설정 시 필요한 추가 설정입니다. 

|매개변수 | 설정값 ... |
|----------|------------|
|Net.TcpipHeapSize |	32 |
|Net.TcpipHeapMax |	vSphere 5.0/5.1의 경우 128 설정 <br/> vSphere 5.5 이상의 경우 512 설정 |
|NFS.MaxVolumes |	256 |
|NFS41.MaxVolumes |	256(vSphere 6.0 이상 전용) |
|NFS.HeartbeatMaxFailures |	10 |
|NFS.HeartbeatFrequency |	12 |
|NFS.HeartbeatTimeout |	5 |
|NFS.MaxQueueDepth|	64 |
 

- 고급 구성 매개변수에 대해 ESXi 5.x 호스트에서 CLI 사용:
  다음 예제에서는 CLI를 사용하여 이러한 고급 구성 매개변수를 설정하고 이를 확인합니다. 예제에 사용된 esxcfg-advcfg 도구는 ESXi 5.x 호스트의 /usr/sbindirectory에서 찾을 수 있습니다. 

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


## SoftLayer에서 Jumbo Frames 사용 - Windows 및 Linux

{{site.data.keyword.BluSoftlayer_full}}에서는 스토리지에서 속도를 완전히 실현하기 위해 9,000 MTU에서 Jumbo Frame을 사용해야 한다고 명시합니다. 


Jumbo Frame은 1,500바이트의 표준 최대 전송 단위(MTU)보다 페이로드가 더 큰 이더넷 프레임입니다. Jumbo Frame은 최소 1Gbps, 최대 9,000바이트를 지원하는 근거리 통신망에서 사용됩니다. 


전체 네트워크 경로에서 소스 디바이스 <-> 스위치 <-> 라우터 <-> 스위치 <-> 대상 디바이스로부터 Jumbo Frame을 동일하게 구성해야 합니다. 전체 체인을 동일하게 설정하지 않으면 체인과 함께 최저 설정이 기본적으로 적용됩니다. SoftLayer에서 네트워크 디바이스는 현재 9,000으로 설정됩니다. 모든 고객 디바이스는 동일하게 설정해야 합니다. 

### Windows

1. **네트워크 및 공유 센터**를 여십시오. 
2. **어댑터 설정 변경**을 클릭하십시오. 
3. Jumbo Frames를 사용할 NIC를 마우스 오른쪽 단추로 클릭하고 **특성**을 선택하십시오. 
4. **네트워킹** 탭 아래에서 네트워크 어댑터에 대한 **구성**을 클릭하십시오. 
5. **고급** 탭을 선택하십시오. 
6. **Jumbo Frame**을 선택하고 NIC에 따라 값을 사용 안함에서 원하는 값(예: 9kB MTU 또는 9,014바이트)으로 변경하십시오. 
7. 모든 대화 상자에 대해 **확인**을 클릭하십시오. 

변경 시 NIC는 몇 초 동안 네트워크 연결을 끊습니다. 또한 변경사항을 적용하려면 재부팅해야 합니다. 

 

### LINUX

1. eth0 인터페이스에 대한 네트워크 구성 파일을 편집하십시오. 예: /etc/sysconfig/network-script/ifcfg-eth0(CentOS/RHEL/Fedora Linux):
`# vi /etc/sysconfig/network-script/ifcfg-eth0`

2. 다음 구성 지시문을 추가하십시오. 그러면 프레임 크기(바이트)를 지정합니다.
MTU 9000

3. 파일을 닫고 저장하십시오. 인터페이스 eth0을 다시 시작하십시오.
`# /etc/init.d/networking restart (This will cause a brief loss of network connectivity)`


Debian/Ubuntu Linux 사용자에 대한 참고 사항:
Debian/Ubuntu Linux 사용자는 MTU=9000을 /etc/network/interfaces 구성 파일에 추가해야 합니다. 

Advanced Single-Site VMware Reference Architecture에 대해서는 [여기](https://console.bluemix.net/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){:new_window}에서 자세히 알아보십시오. 
