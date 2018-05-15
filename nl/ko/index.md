---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.filestorage_short}} 시작하기

{{site.data.keyword.filestorage_full}}는 지속적이고 빠르며 유연성 있는 네트워크 연결, NFS 기반 {{site.data.keyword.filestorage_short}}입니다. NAS(Network Attached Storage) 환경에서 사용자에게는 파일 공유 기능과 성능에 대한 전체적인 제어권이 있습니다. {{site.data.keyword.filestorage_short}} 공유는 복원성을 위해 라우트된 TCP/IP 연결 상에서 최대 64개의 권한 부여된 디바이스에 연결될 수 있습니다. 

{{site.data.keyword.filestorage_short}}는 필적할 수 없는 기능 세트로 동종최상의 내구성 및 가용성 레벨을 제공하고 산업 표준과 우수 사례를 사용하여 빌드되어 있으며, 일관된 성능 기준선을 제공하면서도 유지보수 이벤트 및 무계획 장애를 통해 데이터 무결성을 보호하고 가용성을 유지하도록 설계되었습니다. 

{{site.data.keyword.filestorage_short}}의 다음 코어 기능을 활용하십시오. 

- **일관된 성능 기준선**
   - 개별 볼륨에 프로토콜 레벨 IOPS(Input/Output Operations Per Second)의 할당을 통해 제공됨
- **{{site.data.keyword.filestorage_short}}**
   - 파일 기반 NFS 공유에 사용 가능함
- **높은 내구성 및 복원성**
   - 운영 체제 레벨 RAID(Redundant Array of Independent Disk) 어레이를 작성하고 관리하지 않고도 유지보수 이벤트 및 무계획 장애를 통해 데이터 무결성을 보호하고 가용성을 유지함 
- **저장 데이터 암호화** [(엄선된 데이터 센터에서 사용 가능함)](new-ibm-block-and-file-storage-location-and-features.html)
   - 추가 비용 없이 저장 데이터에 대한 제공자 관리 암호화
- **모든 플래시 지원 스토리지** [(엄선된 데이터 센터에서 사용 가능함)](new-ibm-block-and-file-storage-location-and-features.html)
   - 2 IOPS/GB 이상의 Endurance 또는 Performance으로 프로비저닝된 볼륨의 모든 플래시 스토리지
- **스냅샷([엄선된 데이터 센터](new-ibm-block-and-file-storage-location-and-features.html)에서 Endurance 또는 Performance으로 프로비저닝된 경우)**.
   - 특정 시점 데이터 스냅샷을 비파괴적으로 캡처
- **복제**([엄선된 데이터 센터](new-ibm-block-and-file-storage-location-and-features.html)에서 Endurance 또는 Performance으로 프로비저닝된 경우).
   - 스냅샷을 파트너 {{site.data.keyword.BluSoftlayer_full}} 데이터 센터에 자동 복사
- **고가용성 연결**
   - 중복 네트워킹 연결을 사용하여 가용성 최대화 - NFS 기반 {{site.data.keyword.filestorage_short}} 라우팅된 TCP/IP 연결
- **동시 액세스**
   - 다수의 호스트(최대 64개)가 파일 볼륨에 동시 액세스할 수 있도록 허용
- **클러스터링된 데이터베이스**
   - 고급 유스 케이스(예: 클러스터링된 데이터베이스) 지원

## 시간별/월별 청구

파일 볼륨에 대해 시간별 또는 월별 청구를 선택할 수 있습니다. LUN에 대해 선택된 청구의 유형은 해당 스냅샷 영역 및 복제본에 적용됩니다. 예를 들어, 시간별 청구로 LUN을 프로비저닝하는 경우에 스냅샷 또는 복제본 비용은 시간별로 청구됩니다. 월별 청구로 LUN을 프로비저닝하는 경우에 스냅샷 또는 복제본 비용은 월별로 청구됩니다.  

**시간별 청구**에서, 파일 볼륨이 계정에 존재한 시간은 볼륨이 삭제된 시점이나 청구 주기가 끝난 시점(둘 중에서 먼저 발생하는 시점)에 계산됩니다. 시간별 스토리지는 수 일간 또는 한달 이내에 사용되는 스토리지에 좋은 선택사항입니다. 시간별 청구는 [엄선된 데이터 센터](new-ibm-block-and-file-storage-location-and-features.html)에서 프로비저닝된 스토리지에만 사용 가능합니다.  

**월별 청구**에서, 가격에 대한 계산은 작성 날짜로부터 청구 주기의 끝까지 비례 배분되며 즉각적으로 청구됩니다. 청구 주기가 끝나기 전에 파일 볼륨이 삭제된 경우, 환불은 없습니다. 월별 청구는 장기간(한달 이상) 저장되고 액세스되어야 하는 데이터를 사용하는 프로덕션 워크로드에서 사용되는 스토리지에 좋은 선택사항입니다. 

 
### 성능:
<table>
 <tbody>
  <tr>
   <th>월별 비용</th>
   <td>$0.10/GB + $0.07/IOP</td>
  </tr>
  <tr>
   <th>시간별 가격</th>
   <td>$0.0001/GB + $0.0002/IOP</td>
  </tr>
  </tbody>
</table>
 
### Endurance: 
<table>
 <tbody>
  <tr>
   <th>IOPS 계층</th>
   <th>0.25 IOPS/GB</th>
   <th>2 IOPS/GB</th>
   <th>4 IOPS/GB</th>
   <th>10 IOPS/GB</th>
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
  </tbody>
</table>

 

## 프로비저닝

{{site.data.keyword.filestorage_short}} 볼륨은 두 개의 프로비저닝 옵션으로 20GB에서 12TB까지 프로비저닝 될 수 있습니다. <br/>
- 사전 정의된 성능 레벨 및 기능(예: 스냅샷 및 복제)을 특성으로 하는 **Endurance 계층**을 제공합니다. 
- 할당된 IOPS(Input/Output Operations Per Second)의 고성능 **Performance** 환경을 빌드합니다. 

 
### Endurance 계층

Endurance의 주문 시에는 다양한 애플리케이션 요구사항을 지원할 수 있도록 여러 성능 계층에서 선택하십시오. 

Endurance은 다양한 애플리케이션 요구사항을 지원할 수 있도록 3개의 IOPS 성능 계층에서 사용 가능합니다. 

- **0.25 IOPS / GB**는 I/O 집약도가 낮은 워크로드용으로 설계되었습니다. 이러한 워크로드는 일반적으로 주어진 시간에 대부분 비활성 데이터를 보유하는 것이 특성입니다. 적용되는 예에는 메일함 또는 부서 레벨 파일 공유의 저장이 포함되어 있습니다. 
- **2 IOPS / GB**는 가장 일반적인 사용 용도로 설계되었습니다. 적용되는 예에는 하이퍼바이저용 가상 머신 디스크 이미지 또는 웹 애플리케이션을 지원하는 소형 데이터베이스의 호스팅이 포함되어 있습니다. 
- **4 IOPS / GB**는 집약도가 높은 워크로드용으로 설계되었습니다. 이러한 워크로드는 일반적으로 주어진 시간에 대부분 활성 데이터를 보유하는 것이 특성입니다. 적용되는 예에는 트랜잭션 및 기타 성능에 민감한 데이터베이스가 포함되어 있습니다. 
- **10 IOPS / GB**는 가장 수요가 많은 워크로드(예: NoSQL 데이터베이스에 의해 생성된 워크로드) 및 분석을 위한 데이터 처리용으로 설계되었습니다. 이 계층은 [엄선된 데이터 센터](new-ibm-block-and-file-storage-location-and-features.html)에서 최대 4TB 크기로 프로비저닝된 스토리지에 사용 가능합니다. 

12TB Endurance 볼륨으로 최대 48,000 IOPS까지 사용 가능합니다. 


핵심인 워크로드에 대한 올바른 Endurance {{site.data.keyword.filestorage_short}} 계층을 선택하는 동안, 최대 성능을 얻는 데 필요한 블록 크기, 이더넷 연결 속도 및 호스트 수를 사용하는 것도 이와 동일하게 중요합니다. 이러한 부분들 중 일부가 서로 잘 맞지 않으면 최종 처리량에 중대한 영향을 줄 수 있습니다. 
 
### Performance

Performance은 Endurance 계층 내에서 잘 맞지 않는 잘 파악된 성능 요구사항으로 높은 I/O 애플리케이션을 지원하도록 설계된 {{site.data.keyword.filestorage_full}}의 클래스입니다. 예측 가능한 성능은 개별 볼륨에 대한 프로토콜 레벨 IOPS의 할당을 통해 얻을 수 있습니다. 100 - 6,000 범위의 IOPS는 20GB - 12TB 범위의 스토리지 크기로 프로비저닝될 수 있습니다.  

{{site.data.keyword.filestorage_short}}의 Performance은 NFS(Network File System) 연결을 통해 액세스되고 마운트됩니다. {{site.data.keyword.filestorage_short}}는 일반적으로 여러 시스템이 볼륨을 동시에 액세스할 때 사용됩니다. 일관된 Performance 볼륨은 표 1의 크기와 IOPS에 따라 순서가 지정될 수 있으며 Linux 운영 체제에서 사용될 수 있습니다. 

<table cellpadding="1" cellspacing="1" style="width: 99%;">
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <tbody>
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
        </tbody>
</table>

<sup>![각주](/images/numberone.png)</sup>  6,000을 초과하는 IOPS는 엄선된 데이터 센터에서 사용 가능합니다. 


Performance 볼륨은 프로비저닝된 IOPS 레벨에 근접하여 일관되게 수행하도록 설계되었습니다. 일관성은 제공된 성능 레벨로 애플리케이션 환경을 크기 조정하고 스케일링하기 쉽도록 합니다. 또한 일정 범위의 볼륨 크기와 IOPS 개수가 제공된 경우에는 이상적인 가성비로 볼륨을 빌드하여 환경을 최적화할 수 있습니다. 

Endurance 및 Performance 모두의 경우, IOPS는 50/50 읽기/쓰기 혼합의 16KB 블록 크기를 기반으로 측정됩니다. 볼륨에서 최대 IOPS를 달성하려면 적절한 네트워크 리소스가 제 위치에 있어야 합니다. 기타 고려사항에는 스토리지 외부의 사설 네트워크 사용량과 호스트 측 및 애플리케이션 특정 튜닝(IP 스택, 큐 깊이 등)이 포함됩니다.  

## {{site.data.keyword.filestorage_short}}의 IOPS를 프로비저닝하기 위한 팁

Endurance 및 Performance 모두의 경우, IOPS는 50/50 읽기/쓰기 50% 랜덤 워크로드의 16KB 블록 크기를 기반으로 합니다. ~16KB 블록은 볼륨에 한 번 쓰기와 동등합니다. 

애플리케이션이 사용하는 블록 크기는 스토리지 성능에 직접 영향을 줍니다. 애플리케이션이 사용하는 블록 크기가 16KB보다 작은 경우에는 처리량 한계 이전에 IOPS 한계에 도달합니다. 이와는 반대로, 애플리케이션이 사용하는 블록 크기가 16KB보다 큰 경우에는 IOPS 한계 이전에 처리량 한계에 도달합니다. 

블록 크기 변경은 다음과 같이 성능에 영향을 줍니다. 

<table cellpadding="1" cellspacing="1" style="width: 99%;">
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <tbody>
          <tr>
            <th>블록 크기(KB)</th>
            <th>IOPS</th>
            <th>처리량(MB/초)</th>
          </tr>
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

워크로드에 적합한 {{site.data.keyword.blockstorageshort}}의 선택은 중요하며 병목 현상을 피하는 방법도 이와 동일하게 중요합니다. 이더넷 연결 속도는 볼륨의 예상 최대 처리량보다 빨라야 합니다. 일반적인 규칙으로서, 사용 가능한 대역폭의 70% 를 초과하여 이더넷 연결이 포화 상태가 되는 것은 예상하지 않아야 합니다. 예를 들어, 6,000 IOPS가 있으며 16KB 블록 크기를 사용하는 경우에는 볼륨이 초당 약 94MB일 수 있습니다. LUN에 대한 1Gbps 이더넷 연결을 보유하는 경우, 서버가 최대 사용 가능한 처리량을 이용하려고 시도하면 병목 현상이 발생합니다. 1Gbps 이더넷 연결에 대한 이론적 한계(초당 125MB)의 70% 가 오직 초당 88MB를 허용하기 때문입니다. 


고려할 또 다른 요인은 볼륨을 이용하는 호스트의 수입니다. 볼륨에 액세스 중인 단일 호스트가 있는 경우에는 사용 가능한 최대 IOPS를 실현하기가 어려울 수 있습니다(특히 최대 IOPS 개수(10,000s)에서). 워크로드에서 높은 처리량을 요구하는 경우에는 단일 서버 병목 현상을 피할 수 있도록 볼륨에 액세스하기 위한 최소한 2 - 3개의 서버를 구성하는 것이 가장 좋습니다. 


최대 IOPS를 달성하려면 적절한 네트워크 리소스가 제 위치에 있어야 합니다. 기타 고려사항에는 스토리지 외부의 사설 네트워크 사용량과 호스트 측 및 애플리케이션 특정 튜닝(IP 스택, 큐 깊이 등)이 포함됩니다. 

NFS v3 및 NFS v4.1 모두는 {{site.data.keyword.BluSoftlayer_full}} 환경에서 지원됩니다. 그러나 NFS v3을 사용하도록 권장합니다. NFS v4.1이 Stateless 프로토콜이므로(NFSv3과 같은 Stateless가 아님), 프로토콜 문제가 네트워크 이벤트 중에 발생할 수 있습니다. NFS v4.1은 오퍼레이션을 거부한 후에 잠금 교정을 수행해야 합니다. 상대적으로 많이 사용 중인 NFS 파일 서버에서는 대기 시간 증가로 인해 장애가 발생할 수 있습니다. 또한 NFS v4.1 다중 경로/트렁크의 부족으로 인해 NFS 오퍼레이션 복구가 확장될 수도 있습니다. 
