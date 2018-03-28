---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.filestorage_short}} 시작하기

{{site.data.keyword.filestorage_full}}는 지속적이며 빠르고 유연한 네트워크에 연결된 NFS 기반 {{site.data.keyword.filestorage_short}}입니다. 이 네트워크 접속 스토리지(NAS) 환경에는 파일 공유 기능 및 성능을 모두 제어합니다. {{site.data.keyword.filestorage_short}} 공유는 복원성을 위해 라우트된 TCP/IP 연결을 사용하여 최대 64개의 권한 부여된 디바이스에 연결할 수 있습니다. 

{{site.data.keyword.filestorage_short}}에서는 뛰어난 기능 세트와 함께 최고의 내구성과 가용성을 제공하며, 업계 표준 및 우수 사례를 사용하여 빌드되었고, 데이터 무결성을 보호하고 유지보수 이벤트 및 플랜되지 않은 장애 발생 시 가용성을 유지보수하면서 일관된 성능 기준선을 제공하도록 디자인되었습니다. 

다음과 같은 {{site.data.keyword.filestorage_short}}의 핵심 기능을 활용해 보십시오. 

- **일관된 성능 기준선**
   - 개별 볼륨에 프로토콜 레벨 초당 입/출력 오퍼레이션(IOPS)을 할당하여 제공됨
- **{{site.data.keyword.filestorage_short}}**
   - 파일 기반 NFS 공유에 대한 가용성
- **뛰어난 내구성 및 복원성**
   - 운영 체제 레벨 RAID(Redundant Array of Independent Disk) 어레이를 작성 및 관리하지 않고도 데이터 무결성을 보호하고 유지보수 이벤트 및 플랜되지 않은 장애 발생 시 가용성을 유지함
- **저장 데이터 암호화**([엄선된 데이터 센터에서 사용 가능](new-ibm-block-and-file-storage-location-and-features.html).) 
   - 저장 데이터에 대한 제공자 관리 암호화
- **모든 플래시 기반 스토리지**[(엄선된 데이터 센터에서 사용 가능)](new-ibm-block-and-file-storage-location-and-features.html)
   - 2 IOPS/GB 이상에서 Endurance 또는 Performance로 프로비저닝된 볼륨의 모든 플래시 스토리지
- **스냅샷([엄선된 데이터 센터](new-ibm-block-and-file-storage-location-and-features.html)에서 Endurance 또는 Performance로 프로비저닝된 경우)**.
   - 비파괴적 방식으로 특정 시점 데이터 스냅샷 캡처
- **복제**([엄선된 데이터 센터](new-ibm-block-and-file-storage-location-and-features.html)에서 Endurance 또는 Performance로 프로비저닝된 경우)
   - 파트너 {{site.data.keyword.BluSoftlayer_full}} 데이터 센터에 스냅샷 자동 복사
- **고가용성의 연결**
   - 가용성을 극대화하기 위해 중복 네트워킹 연결 사용 - NFS 기반 {{site.data.keyword.filestorage_short}}의 라우트된 TCP/IP 연결
- **동시 액세스**
   - 여러 호스트(최대 64개)의 파일 볼륨에 동시에 액세스 허용
- **클러스터된 데이터베이스**
   - 클러스터된 데이터베이스와 같은 고급 유스 케이스 지원

## 시간별/월별 청구

파일 볼륨에 대해 시간별 또는 월별 청구를 선택할 수 있습니다. LUN에 대해 선택된 청구 유형은 해당 스냅샷 영역 및 복제본에 적용됩니다. 예를 들어, 시간별 청구로 LUN을 프로비저닝하는 경우 스냅샷 또는 복제본 수수료가 시간별로 청구됩니다. 월별 청구로 LUN을 프로비저닝하는 경우 스냅샷 또는 복제본 수수료가 월별로 청구됩니다.  

**시간별 청구**에서는 계정에 파일 볼륨이 존재하는 시간을 볼륨이 삭제된 시간 또는 청구 주기 종료 시간(둘 중 먼저 다가오는 시간)에 계산합니다. 시간별 청구는 며칠 또는 전체 1개월 미만 동안 사용하는 스토리지에 대해 바람직한 선택입니다. 시간별 청구는 [엄선된 데이터 센터](new-ibm-block-and-file-storage-location-and-features.html)에서 프로비저닝된 스토리지에서만 사용 가능합니다.  

**월별 청구** 시 가격의 계산은 작성 날짜부터 청구 주기 종료 시간까지 비례 배분되어 즉시 청구됩니다. 청구 주기 종료 전에 파일 볼륨이 삭제된 경우 환불되지 않습니다. 월별 청구는 장기간(1개월 이상) 저장 및 액세스해야 하는 데이터를 사용하는 프로덕션 워크로드에서 사용되는 스토리지에 대해 좋은 선택입니다. 

 
### Performance:
<table>
 <tbody>
  <tr>
   <th>월별 비용</th>
   <td>$0.10/GB + $0.07/IOP</td>
  </tr>
  <tr>
   <th>시간별 비용</th>
   <td>$0.0001/GB + $0.0002/IOP</td>
  </tr>
  </tbody>
</table>
 
### Endurance:
<table>
 <tbody>
  <tr>
   <th>IOPS 티어</th>
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
   <th>시간별 비용</th>
   <td>0.0002/GB</td>
   <td>$0.0003/GB</td>
   <td>$0.0005/GB</td>
   <td>$0.0009/GB</td>
  </tr>
  </tbody>
</table>

 

## 프로비저닝

{{site.data.keyword.filestorage_short}} 볼륨은 프로비저닝에 대한 두 가지 옵션을 통해 20GB에서 12TB까지 프로비저닝될 수 있습니다. <br/>
- **Endurance 티어**로 프로비저닝. 이 경우 사전 정의된 성능 레벨과 스냅샷 및 복제와 같은 기능을 제공합니다. 
- 할당된 초당 입/출력 오퍼레이션(IOPS)을 통해 고출력 **Performance** 환경을 빌드합니다. 

 
### Endurance 티어

Endurance로 주문하는 경우 다양한 애플리케이션 요구를 지원하기 위해 여러 성능 티어 중에서 선택하십시오. 

Endurance는 다양한 애플리케이션 요구를 지원하기 위해 3개의 IOPS 성능 티어에서 사용 가능합니다. 

- **0.25 IOPS/GB**는 I/O 집약도가 낮은 워크로드를 대상으로 디자인되었습니다. 이러한 워크로드는 보통 주어진 시점에 많은 비율의 데이터를 비활성화한다는 점이 특징입니다. 적용 예에는 메일함 또는 부서 레벨 파일 공유의 저장이 있습니다.
- **2 IOPS/GB**는 가장 일반적인 용도에 맞게 디자인되었습니다. 적용 예에는 웹 애플리케이션 또는 하이퍼바이저를 위한 가상 머신 디스크 이미지를 지원하는 소규모 데이터베이스의 호스팅이 있습니다.
- **4 IOPS/GB**는 집약도 높은 워크로드를 대상으로 디자인되었습니다. 이러한 워크로드는 보통 주어진 시점에 높은 데이터의 비율을 활성화한다는 점이 특징입니다. 적용 예에는 트랜잭션 데이터베이스 또는 그 외 성능에 민감한 데이터베이스가 있습니다.
- **10 IOPS/GB**는 NoSQL 데이터베이스에서 작성된 항목과 같이 가장 까다로운 워크로드 및 분석을 위한 데이터 처리를 위해 디자인되었습니다. 이 티어는 최대 4TB 크기로 프로비저닝된 스토리지에 대해 [엄선된 데이터 센터](new-ibm-block-and-file-storage-location-and-features.html)에서 사용 가능합니다. 

최대 48,000 IOPS가 12TB Endurance 볼륨에서 사용 가능합니다. 


워크로드에 올바른 Endurance {{site.data.keyword.filestorage_short}} 티어를 선택하는 경우 중요하지만, 블록 크기, 이더넷 연결 속도, 최대 성능을 달성하기 위해 필요한 호스트 수를 사용하는 것도 중요합니다. 이러한 부분이 다른 부분과 맞지 않으면 결과적으로 처리량에 큰 영향을 줄 수 있습니다. 
 
### Performance

Performance는 알려진 성능 요구사항이 Endurance 티어에 잘 맞지 않는 높은 I/O 애플리케이션을 지원하도록 디자인된 {{site.data.keyword.filestorage_full}} 클래스입니다. 예측 가능한 성능은 개별 볼륨에 프로토콜 레벨 IOPS를 할당하여 달성합니다. 100 - 6,000 범위의 IOPS는 20GB - 12TB 사이의 스토리지 크기로 프로비저닝될 수 있습니다.  

{{site.data.keyword.filestorage_short}}에 대한 Performance는 네트워크 파일 시스템(NFS) 연결을 통해 액세스 및 마운트됩니다. {{site.data.keyword.filestorage_short}}는 보통 여러 머신에서 동시에 볼륨에 액세스하는 경우에 사용됩니다. 일관된 Performance 볼륨은 표 1의 크기 및 IOPS에 따라 주문할 수 있으며 Linux 운영 체제에서 사용할 수 있습니다. 

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

<sup>![각주](/images/numberone.png)</sup> 6,000을 초과하는 IOPs 한계는 엄선된 데이터 센터에서 사용 가능합니다. 


Performance 볼륨은 프로비저닝된 IOPS 레벨에 가깝게 일관되게 수행되도록 디자인되었습니다. 일관성을 유지하면 주어진 성능 레벨에서 애플리케이션 환경을 스케일링하고 크기를 조정하기 쉬워집니다. 또한 주어진 볼륨 크기 및 IOPS 개수 범위에서 이상적인 가격 대 성능 비율의 볼륨을 빌드하여 환경을 최적화할 수 있습니다. 

Endurance 및 Performance 모두에 대해 IOPS는 50/50의 읽기/쓰기를 혼합한 16KB 블록 크기에 기반하여 측정합니다. 볼륨에서 최대 IOPS를 달성하려면 적절한 네트워크 리소스가 배치되어야 합니다. 기타 고려사항으로는 스토리지 외 사설 네트워크 사용량과 호스트 측 및 애플리케이션 특정 튜닝(IP 스택, 큐 깊이 등)이 포함됩니다. 

## {{site.data.keyword.filestorage_short}}에 대한 IOPS 프로비저닝 팁

Endurance 및 Performance 모두에 대해 IOPS는 50/50의 읽기/쓰기 50% 랜덤 워크로드를 사용하는 16KB 블록 크기에 기반합니다. ~16KB 블록은 볼륨에서 한 번의 쓰기와 동일합니다. 

애플리케이션에서 사용하는 블록 크기는 스토리지 성능에 직접 영향을 줍니다. 애플리케이션에서 사용하는 블록 크기가 16KB보다 작으면 처리량 한계 전에 IOP 한계에 도달합니다. 반대로, 애플리케이션에서 사용하는 블록 크기가 16KB보다 크면 IOP 한계 전에 처리량 한계에 도달합니다. 

블록 크기를 변경하면 다음과 같이 성능에 영향을 줍니다. 

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
            <td>4(일반적으로 Linux용)</td>
            <td>1,000</td>
            <td>4</td>
          </tr>
          <tr>
            <td>8(일반적으로 Oracle용)</td>
            <td>1,000</td>
            <td>8</td>
          </tr>
          <tr>
            <td>16</td>
            <td>1,000</td>
            <td>16</td>
          </tr>
          <tr>
            <td>32(일반적으로 SQLServer용)</td>
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

워크로드에 올바른 {{site.data.keyword.blockstorageshort}}를 선택하는 것이 중요하며, 병목 현상을 피하는 방법도 중요합니다. 이더넷 연결 속도는 볼륨에서 예상되는 최대 처리량보다 빨라야 합니다. 일반적으로 사용 가능한 대역폭의 70% 넘게 이더넷 연결이 포화되지 않도록 해야 합니다. 예를 들어, 6,000 IOPS를 보유하고 16KB 블록 크기를 사용하는 경우 볼륨은 초당 약 94MB를 감당할 수 있습니다. LUN에 대한 1Gbps 이더넷 연결을 보유한 경우 서버가 최대로 사용 가능한 처리량을 활용하려고 할 때 병목 현상이 발생할 수 있습니다. 초당 88MB에 대해 이론적으로 1Gbps 이더넷 연결 한계의 70%(초당 125MB)만 허용되기 때문입니다. 


또한, 볼륨을 사용 중인 호스트 수도 고려해야 합니다. 볼륨에 액세스하는 단일 호스트가 있는 경우 특히 극단적 IOPS 개수((10,000s)에서 최대 사용 가능한 IOPS를 달성하기 어려울 수 있습니다. 워크로드에 높은 처리량이 요구되는 경우 단일 서버 병목 현상을 방지하기 위해 볼륨에 액세스할 적어도 2개 또는 3개의 서버를 구성하는 것이 가장 좋습니다. 


최대 IOPS를 달성하려면 적절한 네트워크 리소스가 배치되어야 합니다. 기타 고려사항으로는 스토리지 외 사설 네트워크 사용량과 호스트 측 및 애플리케이션 특정 튜닝(IP 스택, 큐 깊이 등)이 포함됩니다.

NFS v3 및 NFS v4.1 모두 {{site.data.keyword.BluSoftlayer_full}} 환경에서 지원됩니다. 하지만 NFS v3을 사용하는 것이 좋습니다. NFS v4.1은 stateful 프로토콜(NFSv3과 같이 stateless가 아님)이므로, 네트워크 이벤트 중에 프로토콜 문제가 발생할 수 있습니다. NFS v4.1은 오퍼레이션을 중지하고 잠금 교정을 수행해야 합니다. 상대적으로 사용량이 많은 NFS 파일 서버에서 대기 시간이 늘어나면 중단될 수 있습니다. NFS v4.1 다중 경로/트렁크가 없으면 NFS 오퍼레이션 복구를 확장할 수 있습니다. 
