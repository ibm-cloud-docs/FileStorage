---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-10"

---
{:new_window: target="_blank"}

# {{site.data.keyword.filestorage_short}} 시작하기

{{site.data.keyword.filestorage_full}}는 지속적이고 빠르며 유연한 네트워크 연결, NFS 기반 {{site.data.keyword.filestorage_short}}입니다. 이 NAS(Network-Attached Storage) 환경에서 사용자는 파일 공유 기능과 성능을 전체적으로 제어할 수 있습니다. {{site.data.keyword.filestorage_short}} 공유는 복원성을 위해 라우트된 TCP/IP 연결 상에서 최대 64개의 권한 부여된 디바이스에 연결될 수 있습니다.

{{site.data.keyword.filestorage_short}}는 필적할 수 없는 기능 세트로 동종최상의 내구성 및 가용성 레벨을 제공합니다. {{site.data.keyword.filestorage_short}}는 산업 표준과 우수 사례에 따라 빌드되어 있으며 데이터 무결성을 보호하도록 디자인되었습니다. {{site.data.keyword.filestorage_short}}는 유지보수 이벤트 및 예기치 않은 장애 발생 상황에서 가용성을 유지하며, 일관된 성능 기준선을 제공합니다.

{{site.data.keyword.filestorage_short}}의 다음 코어 기능을 활용하십시오.

- **일관된 성능 기준선**
   - 개별 볼륨에 대한 프로토콜 레벨 IOPS(Input/Output Operations Per Second)의 할당을 통해 제공됩니다.
- **{{site.data.keyword.filestorage_short}}**
   - 파일 기반 NFS 공유에 대해 사용 가능합니다.
- **높은 내구성 및 복원성**
   - 운영 체제 레벨 RAID(Redundant Array of Independent Disk) 어레이를 작성하고 관리하지 않고도 유지보수 이벤트 및 예기치 않은 장애 발생 상황에서 데이터 무결성을 보호하고 가용성을 유지합니다.
- **저장 데이터 암호화** [(특정 데이터 센터에서 사용 가능함)](new-ibm-block-and-file-storage-location-and-features.html)
   - 저장 데이터에 대한 제공자 관리 암호화가 추가 비용 없이 제공됩니다.
- **모든 플래시 지원 스토리지** [(특정 데이터 센터에서 사용 가능함)](new-ibm-block-and-file-storage-location-and-features.html)
   - 볼륨에 대한 모든 플래시 스토리지를 2IOPS/GB 이상 레벨에서 프로비저닝할 수 있습니다.
- **스냅샷** [(특정 데이터 센터에서 사용 가능함)](new-ibm-block-and-file-storage-location-and-features.html).
   - 작업을 방해하지 않으면서 특정 시점의 데이터 스냅샷을 캡처합니다.
- **복제** [(특정 데이터 센터에서 사용 가능함)](new-ibm-block-and-file-storage-location-and-features.html)
   - 스토리지가 [특정 데이터 센터](new-ibm-block-and-file-storage-location-and-features.html)에서 프로비저닝되는 경우 사용 가능합니다.
   - 스냅샷을 파트너 {{site.data.keyword.BluSoftlayer_full}} 데이터 센터에 자동으로 복사합니다.
- **고가용성 연결**
   - 가용성을 최대화하기 위해 중복 네트워킹 연결을 사용합니다.
   - NFS 기반 {{site.data.keyword.filestorage_short}} 라우팅된 TCP/IP 연결입니다.
- **동시 액세스**
   - 여러 호스트(최대 64개)가 파일 볼륨에 동시에 액세스할 수 있도록 합니다.
- **클러스터링된 데이터베이스**
   - 클러스터링된 데이터베이스와 같은 고급 유스 케이스를 지원합니다.

## 비용 청구

파일 볼륨에 대해 시간별 또는 월별 청구를 선택할 수 있습니다. LUN에 대해 선택된 비용 청구 유형은 해당 스냅샷 영역 및 복제본에 적용됩니다. 예를 들어, 시간별 청구로 LUN을 프로비저닝하는 경우 스냅샷 또는 복제본 비용은 시간별로 청구됩니다. 월별 청구로 LUN을 프로비저닝하는 경우에는 스냅샷 또는 복제본 비용이 월별로 청구됩니다. 

**시간별 청구**에서 파일 볼륨이 계정에 존재한 시간은 LUN이 삭제된 시점이나 청구 주기가 끝난 시점(둘 중에서 먼저 발생하는 시점)에 계산됩니다. 시간별 스토리지는 수 일간 또는 한달 이내에 사용되는 스토리지에 좋은 선택사항입니다. 시간별 청구는 [특정 데이터 센터](new-ibm-block-and-file-storage-location-and-features.html)에서 프로비저닝된 스토리지에만 사용 가능합니다. 

**월별 청구**에서, 가격에 대한 계산은 작성 날짜로부터 청구 주기의 끝까지 비례 배분되며 즉각적으로 청구됩니다. 볼륨이 청구 주기가 끝나기 전에 삭제된 경우에는 환불이 되지 않습니다. 월별 청구는 장기간(한달 이상) 저장하고 액세스해야 하는 데이터를 사용하는 프로덕션 워크로드에서 사용되는 스토리지에 대해 좋은 선택사항입니다. 

 
**Performance**
<table>
  <caption>표 1에는 월별 및 시간별 청구 시의 Performance 스토리지 가격이 표시되어 있습니다.</caption>
  <tr>
   <th>월별 비용</th>
   <td>$0.10/GB + $0.07/IOP</td>
  </tr>
  <tr>
   <th>시간별 가격</th>
   <td>$0.0001/GB + $0.0002/IOP</td>
  </tr>
</table>
 
**Endurance**
<table>
  <caption>표 2에는 월별 및 시간별 청구 옵션을 사용할 때의 각 Endurance 스토리지 계층 가격이 표시되어 있습니다.</caption>
  <tr>
   <th>IOPS 계층</th>
   <th>0.25IOPS/GB</th>
   <th>2IOPS/GB</th>
   <th>4IOPS/GB</th>
   <th>10IOPS/GB</th>
  </tr>
  <tr>
   <th>월별 비용</th>
   <td>$0.10/GB</td>
   <td>$0.20/GB</td>
   <td>$0.35/GB</td>
   <td>$0.58/GB</td>
  </tr>
  <tr>
   <th>시간별 가격</th>
   <td>0.0002/GB</td>
   <td>$0.0003/GB</td>
   <td>$0.0005/GB</td>
   <td>$0.0009/GB</td>
  </tr>
</table>

 

## 프로비저닝

{{site.data.keyword.filestorage_short}} 볼륨은 두 개의 옵션으로 20GB에서 12TB까지 프로비저닝될 수 있습니다. <br/>
- 사전 정의된 성능 레벨 및 기타 기능(스냅샷 및 복제 등)을 제공하는 **Endurance** 계층으로 프로비저닝합니다.
- 할당된 IOPS(Input/Output Operations Per Second)의 고성능 **Performance** 환경을 빌드합니다. 

 
### Endurance 계층을 사용한 프로비저닝

Endurance {{site.data.keyword.filestorage_short}}는 다양한 애플리케이션 요구사항을 지원하기 위한 네 가지 IOPS 성능 계층으로 사용할 수 있습니다. <br />

- **0.25IOPS/GB**는 I/O 집약도가 낮은 워크로드용으로 설계되었습니다. 이러한 워크로드는 일반적으로 대부분의 시간 동안 비활성 상태인 데이터를 많이 보유하는 것이 특성입니다. 적용되는 예에는 메일함 또는 부서 레벨 파일 공유의 저장이 포함되어 있습니다.

- **2IOPS/GB**는 가장 일반적인 사용 용도로 설계되었습니다. 적용 예에는 하이퍼바이저용 VM 디스크 이미지, 또는 웹 애플리케이션을 지원하는 소형 데이터베이스의 호스팅 등이 있습니다.

- **4IOPS/GB**는 집약도가 높은 워크로드용으로 설계되었습니다. 이러한 워크로드는 일반적으로 대부분의 시간 동안 활성 상태인 데이터를 많이 보유하는 것이 특성입니다. 적용되는 예에는 트랜잭션 및 기타 성능에 민감한 데이터베이스가 포함되어 있습니다.

- **10IOPS/GB**는 가장 수요가 많은 워크로드(예: NoSQL 데이터베이스에서 생성된 워크로드) 및 분석을 위한 데이터 처리용으로 설계되었습니다. 이 계층은 [특정 데이터 센터](new-ibm-block-and-file-storage-location-and-features.html)에서 최대 4TB 크기로 프로비저닝된 스토리지에 대해서만 사용 가능합니다.

12TB Endurance 볼륨으로 최대 48,000IOPS까지 사용 가능합니다.
 
자신의 워크로드에 적합한 Endurance 계층을 선택하는 것이 중요합니다. 최대 성능을 달성하기 위해 필요한 적절한 블록 크기, 이더넷 연결 속도 및 호스트 수를 사용하는 것 또한 마찬가지로 중요합니다. 이러한 부분 중 일부가 서로 맞지 않으면 최종 처리량에 중대한 영향을 줄 수 있습니다.
 
### Performance를 사용한 프로비저닝

Performance는 Endurance 계층에는 잘 맞지 않는 잘 파악된 성능 요구사항이 있는, I/O 양이 많은 애플리케이션을 지원하기 위해 디자인된 {{site.data.keyword.filestorage_short}} 클래스입니다. 예측 가능한 성능은 개별 볼륨에 대한 프로토콜 레벨 IOPS의 할당을 통해 얻을 수 있습니다. 20GB - 12TB 범위의 스토리지 크기에 대해 100 - 48,000 범위의 IOPS 비율을 프로비저닝할 수 있습니다.

{{site.data.keyword.filestorage_short}}의 Performance는 NFS(Network File System) 연결을 통해 액세스되고 마운트됩니다. {{site.data.keyword.filestorage_short}}는 일반적으로 여러 서버가 동시에 볼륨에 액세스할 때 사용됩니다. 일관된 Performance 볼륨은 표 1의 크기와 IOPS에 따라 순서가 지정될 수 있으며 Linux 운영 체제에서 사용될 수 있습니다.

<table cellpadding="1" cellspacing="1" style="width: 99%;">
 <caption>표 3에는 Performance 스토리지의 크기 및 IOPS 조합이 표시되어 있습니다.<br/><sup><img src="/images/numberone.png" alt="각주" /></sup> 6,000보다 큰 IOPS 한계는 특정 데이터 센터에서 사용 가능합니다.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
          <tr>
            <th>크기(GB)</th>
            <th>최소 IOPS</th>
            <th>최대 IOPS</th>
          </tr>
          <tr>
            <td>20</td>
            <td>100</td>
            <td>1,000</td>
          </tr>
          <tr>
            <td>40</td>
            <td>100</td>
            <td>2,000</td>
          </tr>
          <tr>
            <td>80</td>
            <td>100</td>
            <td>4,000</td>
          </tr>
          <tr>
            <td>100</td>
            <td>100</td>
            <td>6,000</td>
          </tr>
          <tr>
            <td>250</td>
            <td>100</td>
            <td>6,000</td>
          </tr>
          <tr>
            <td>500</td>
            <td>100</td>
            <td>6,000 또는 10,000<sup><img src="/images/numberone.png" alt="각주" /></sup></td>
          </tr>
          <tr>
            <td>1,000</td>
            <td>100</td>
            <td>6,000 또는 20,000<sup><img src="/images/numberone.png" alt="각주" /></sup></td>
          </tr>
          <tr>
            <td>2,000-3,000</td>
            <td>200</td>
            <td>6,000 또는 40,000<sup><img src="/images/numberone.png" alt="각주" /></sup></td>
          </tr>
          <tr>
            <td>4,000-7,000</td>
            <td>300</td>
            <td>6,000 또는 48,000<sup><img src="/images/numberone.png" alt="각주" /></sup></td>
          </tr>
          <tr>
            <td>8,000-9,000</td>
            <td>500</td>
            <td>6,000 또는 48,000<sup><img src="/images/numberone.png" alt="각주" /></sup></td>
          </tr>
          <tr>
            <td>10,000-12,000</td>
            <td>1,000</td>
            <td>6,000 또는 48,000<sup><img src="/images/numberone.png" alt="각주" /></sup></td>
          </tr>
</table>


Performance 볼륨은 일관적으로 프로비저닝된 IOPS 레벨에 가깝게 작동하도록 디자인되었습니다. 일관성은 애플리케이션 환경을 특정 성능 레벨로 크기 조정하고 스케일링하기 쉽게 해 줍니다. 또한 이상적인 가성비로 볼륨을 빌드하여 환경을 최적화할 수 있게 해 줍니다.

### 프로비저닝 고려사항

**블록 크기**

Endurance 및 Performance의 IOPS는 50/50 읽기/쓰기 50퍼센트 랜덤 워크로드의 16KB 블록 크기를 기반으로 합니다. 16KB 블록은 볼륨에 한 번 쓰기와 동등합니다.

애플리케이션에서 사용하는 블록 크기는 스토리지 성능에 직접 영향을 줍니다. 애플리케이션에서 사용하는 블록 크기가 16KB보다 작은 경우에는 처리량 한계보다 IOPS 한계에 먼저 도달합니다. 반대로 애플리케이션에서 사용하는 블록 크기가 16KB보다 큰 경우에는 IOPS 한계보다 처리량 한계에 먼저 도달합니다.

<table>
  <caption>표 4에는 블록 크기 및 IOPS가 처리량에 미치는 영향에 대한 예가 표시되어 있습니다.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <thead>
          <tr>
            <th>블록 크기(KB)</th>
            <th>IOPS</th>
            <th>처리량(MB/초)</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>4(Linux의 경우 기본값)</td>
            <td>1,000</td>
            <td>4</td>
          </tr>
          <tr>
            <td>8(Oracle의 경우 기본값)</td>
            <td>1,000</td>
            <td>8</td>
          </tr>
          <tr>
            <td>16</td>
            <td>1,000</td>
            <td>16</td>
          </tr>
          <tr>
            <td>32(SQLServer의 경우 기본값)</td>
            <td>500</td>
            <td>16</td>
          </tr>          
          <tr>
            <td>64</td>
            <td>250</td>
            <td>16</td>
          </tr>
          <tr>
            <td>128</td>
            <td>128</td>
            <td>16</td>
          </tr>
          <tr>
            <td>512</td>
            <td>32</td>
            <td>16</td>
          </tr>
        </tbody>
</table>

**권한 부여된 호스트**

고려할 또 다른 요인은 볼륨을 사용하는 호스트의 수입니다. 볼륨에 액세스하는 호스트가 하나뿐인 경우에는 사용 가능한 최대 IOPS를 실현하기 어려울 수 있습니다(특히 IOPS 개수가 큰 경우(10,000개 단위)에는 더욱). 워크로드에서 높은 처리량을 요구하는 경우에는 단일 서버 병목 현상을 피할 수 있도록 하기 위해 2 - 3개의 서버는 볼륨에 액세스하도록 구성하는 것이 좋습니다.

**네트워크 연결**

이더넷 연결 속도는 볼륨의 예상 최대 처리량보다 빨라야 합니다. 일반적으로는 이더넷 연결이 사용 가능한 대역폭의 70%를 초과하여 사용하지 않아야 합니다. 예를 들어, IOPS가 6,000이며 16KB 블록 크기를 사용하는 경우 볼륨은 약 94MBps의 처리량을 처리할 수 있습니다. LUN에 대해 1Gbps의 이더넷 연결을 보유하고 있는 경우 서버가 사용 가능한 최대 처리량을 사용하려고 시도하면 병목 현상이 발생합니다. 이는 1Gbps 이더넷 연결(초당 125MB)에 대한 70퍼센트 이론적 한계가 초당 88MB까지만 허용하기 때문입니다.

최대 IOPS를 달성하려면 적절한 네트워크 리소스가 제 위치에 있어야 합니다. 기타 고려사항에는 스토리지 및 호스트 측 외의 사설 네트워크 사용량과 애플리케이션 고유 튜닝(IP 스택, 큐 깊이 및 기타 설정)이 포함됩니다.

스토리지 트래픽은 공용 Virtual Server의 총 네트워크 사용에 포함됩니다. 서비스에서 부과할 수 있는 한계를 이해하려면 [Virtual Server 문서](https://console.bluemix.net/docs/vsi/vsi_public.html#public-virtual-servers)를 참조하십시오. 

**NFS 버전**

NFS v3 및 NFS v4.1 모두는 {{site.data.keyword.BluSoftlayer_full}} 환경에서 지원됩니다. 그러나 NFS v4.1은 Stateful 프로토콜이며(NFS v3과 같은 Stateless가 아님) 네트워크 이벤트 중에 프로토콜 문제가 발생할 수 있으므로 NFS v3이 선호됩니다. NFS v4.1은 모든 오퍼레이션을 중지한 후에 잠금 교정을 수행해야 합니다. 상대적으로 많이 사용 중인 NFS 파일 서버에서는 대기 시간 증가로 인해 장애가 발생할 수 있습니다. 또한 NFS v4.1 다중 경로/트렁크의 부족으로 인해 NFS 오퍼레이션 복구가 확장될 수도 있습니다.

## 주문 제출

주문을 제출할 준비가 되었으면 [여기](provisioning-file-storage.html)에 있는 지시사항을 따르십시오. VMware에서의 File Storage 프로비저닝의 경우에는 [여기](architecture-guide-file-storage-vmware.html)를 클릭하십시오.

## 새 스토리지 연결

프로비저닝 요청이 완료되면 새 스토리지에 액세스할 수 있도록 호스트에 권한을 부여하고 연결을 구성하십시오. 호스트의 운영 체제에 따라 적절한 링크를 사용하십시오.
- [Linux에서 {{site.data.keyword.filestorage_short}} 액세스](accessing-file-storage-linux.html)
- [CentOS에서 NFS/File Storage 마운트](mounting-nsf-file-storage.html)
- [CoreOS의 {{site.data.keyword.filestorage_short}} 마운트](mounting-storage-coreos.html)
- [cPanel로 백업을 위한 {{site.data.keyword.filestorage_short}} 구성](configure-backup-cpanel.html)
- [Plesk로 백업을 위한 {{site.data.keyword.filestorage_short}} 구성](configure-backup-plesk.html)
- [ESXi 호스트에 {{site.data.keyword.filestorage_short}} 볼륨 마운트](architecture-guide-file-storage-vmware.html)
