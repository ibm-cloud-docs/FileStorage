---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-02"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.filestorage_short}} - FAQ

## IOPS는 어떻게 측정됩니까?

IOPS는 16KB 블록의 로드 프로파일에 기반하여 랜덤 50% 읽기 및 50% 쓰기로 측정됩니다. 이 프로파일과 다른 워크로드인 경우 성능이 더 낮아질 수 있습니다. 

## 성능 측정 시 더 작은 블록 크기를 사용하면 어떻게 됩니까?

최대 IOPS는 더 작은 블록 크기를 사용할 때도 얻을 수 있지만, 처리량이 낮아집니다. 예를 들어, 6000 IOPS의 볼륨인 경우 다음은 다양한 블록 크기에서의 처리량입니다. 

- 16 KB * 6000 IOPS == ~93.75 MB/sec
- 8 KB * 6000 IOPS == ~46.88 MB/sec
- 4 KB * 6000 IOPS == ~23.44 MB/sec


## 예상 처리량을 얻기 위해 볼륨을 사전 워밍해야 합니까?

사전 워밍은 필요하지 않습니다. 볼륨을 프로비저닝하면 즉각적으로 지정된 처리량을 관찰할 수 있습니다. 

## 내 {{site.data.keyword.filestorage_short}} LUN/볼륨이 암호화되었는지 어떻게 알 수 있습니까?

고객 포털에서 {{site.data.keyword.filestorage_short}}의 목록을 볼 때 암호화된 항목의 LUN/볼륨 이름 오른쪽에 잠금 아이콘이 표시됩니다. 

## 암호화를 위해 업그레이드되는 데이터 센터에서 암호화되지 않은 프로비저닝된 {{site.data.keyword.filestorage_short}}가 있는 경우 내 {{site.data.keyword.filestorage_short}}를 암호화할 수 있습니까?

데이터 센터 업그레이드 전에 프로비저닝된 {{site.data.keyword.filestorage_short}}는 암호화할 수 없습니다. 업그레이드된 데이터 센터에서 프로비저닝된 새 {{site.data.keyword.filestorage_short}}는 자동으로 암호화됩니다. 이때 선택할 수 있는 암호화 설정은 없습니다. 이는 자동입니다. 업그레이드된 데이터 센터에서 암호화되지 안은 스토리지의 데이터는 새 파일 볼륨을 작성한 후 호스트 기반 마이그레이션이 적용된 볼륨 또는 새로 암호화된 볼륨에 데이터를 복사하여 암호화할 수 있습니다. 마이그레이션 수행 방법에 대한 지시사항은 [이 기사](/docs/infrastructure/FileStorage/migrate-file-storage-encrypted-file-storage.html)를 참조하십시오. 

## 업그레이드된 데이터 센터에서 {{site.data.keyword.filestorage_short}}의 프로비저닝 여부를 어떻게 확인합니까?

{{site.data.keyword.filestorage_short}} 프로비저닝 시 업그레이드된 모든 데이터 센터는 주문 양식에서 별표(`*`)와 암호화로 스토리지가 프로비저닝됨을 알리는 표시로 나타납니다. 스토리지가 프로비저닝되면 스토리지 목록에 암호화된 상태로 볼륨 또는 해당 볼륨을 표시하는 아이콘이 나타납니다. 암호화된 볼륨 및 볼륨 모두 업그레이드된 데이터 센터에서만 프로비저닝됩니다. 업그레이드된 데이터 센터 및 사용 가능한 기능의 전체 목록은 [여기](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html)에서 찾을 수 있습니다. 

## 일부 데이터 센터에서는 Endurance 10 IOPS 티어로 {{site.data.keyword.filestorage_short}}를 프로비저닝할 수 있지만, 다른 데이터 센터에서는 프로비저닝할 수 없는 이유는 무엇입니까?

{{site.data.keyword.filestorage_short}} Endurance 유형 10 IOPS/GB 티어는 엄선된 데이터 센터와 향후 추가되는 새 데이터 센터에서만 사용 가능합니다. 업그레이드된 데이터 센터 및 사용 가능한 기능의 전체 목록은 [여기](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html)에서 찾을 수 있습니다. 

## 내 {{site.data.keyword.filestorage_short}}에 대한 올바른 마운트 지점은 어떻게 찾을 수 있습니까?

이 데이터 센터에서 프로비저닝된 암호화된 모든 {{site.data.keyword.filestorage_short}} 볼륨에는 암호화되지 않은 볼륨과 다른 마운트 지점이 있습니다. 암호화된 볼륨과 암호화되지 않은  {{site.data.keyword.filestorage_short}} 볼륨 모두에 대해 올바른 마운트 지점을 사용하는지 확인하기 위해 API 호출: `SoftLayer_Network_Storage::getNetworkMountAddress()`를 사용하여 올바른 마운트 지점에 액세스하고, UI를 통해 **볼륨 세부사항** 페이지에서 마운트 지점 정보를 볼 수 있습니다. 

## 파일 볼륨 크기당 허용되는 파일 공유는 얼마입니까? 볼륨 크기당 허용되는 최대 파일 공유는 무엇입니까?
다음은 볼륨 크기에 기반하여 허용되는 최대 inodes 또는 파일 공유입니다. 

<table>
        <tbody>
          <tr>
            <th>볼륨 크기</th>
            <th>Inodes/파일</th>
          </tr>
          <tr>
            <td>20GB </td>
            <td>622,484</td>
          </tr>
          <tr>
            <td>40GB </td>
            <td>1,245,084</td>
          </tr>          
          <tr>
            <td>80GB</td>
            <td>2,490,263</td>
          </tr>          
          <tr>
            <td>100GB</td>
            <td>3,112,863</td>
          </tr>          
          <tr>
            <td>250GB</td>
            <td>7,782,300</td>
          </tr>          
          <tr>
            <td>500GB</td>
            <td>15,564,695</td>
          </tr>
          <tr>
            <td>1TB+</td>
            <td>31,876,593</td>
          </tr>
        </tbody>
</table>

## 프로비저닝할 수 있는 볼륨은 몇 개입니까?

기본적으로 총 결합된 250개의 블록 및 파일 스토리지 볼륨을 프로비저닝할 수 있습니다. 볼륨을 늘리려면 영업 담당자에게 문의하십시오. 

## 프로비저닝된 {{site.data.keyword.filestorage_short}} 볼륨의 사용을 몇 개의 인스턴스에서 공유할 수 있습니까?

파일 볼륨당 권한 수의 기본 한계는 64입니다. 한계를 늘리려면 영업 담당자에게 문의하십시오. 

## Performance 또는 Endurance {{site.data.keyword.filestorage_short}} 프로비저닝 시 할당된 IOPS는 인스턴스로 적용됩니까, 아니면 볼륨으로 적용됩니까?

IOPS는 볼륨 레벨로 적용됩니다. 즉, 6000 IOPS의 볼륨에 연결된 두 호스트는 해당 6000 IOPS를 공유합니다. 

## 더 빠른 이더넷 연결을 사용하는 경우 더 높은 처리량을 얻을 수 있습니까?

처리량 한계는 더 빠른 이더넷 연결을 사용해도 설정된 해당 한계를 늘리지 않으므로 사전 볼륨/LUN 레벨에서 설정됩니다. 하지만 더 느린 이더넷 연결을 사용하면 대역폭에서 병목 현상이 발생할 수 있습니다. 

## 방화벽/보안 그룹이 성능에 영향을 줍니까?

우수 사례로 방화벽을 우회하는 vlan에서 스토리지 트래픽을 실행하는 것이 좋습니다. 소프트웨어 방화벽을 통해 스토리지 트래픽을 실행하면 대기 시간이 늘고 스토리지 성능을 떨어뜨릴 수 있습니다. 

## {{site.data.keyword.filestorage_short}} 볼륨이 삭제되면 내 데이터는 어떻게 됩니까?

스토리지가 삭제되면 해당 볼륨에서 데이터에 대한 포인터가 제거되므로 데이터에 완전히 액세스할 수 없습니다. 물리적 스토리지가 다른 계정으로 다시 프로비저닝되면 새 포인터 세트가 지정됩니다. 새 계정이 물리적 스토리지에 있던 데이터에 액세스할 수 있는 방법은 없으며, 포인터의 새 세트는 모두 0을 표시합니다. 새 데이터는 볼륨/LUN에 작성되며, 여전히 존재하는 액세스할 수 없는 데이터는 겹쳐씁니다.  

## 내 {{site.data.keyword.filestorage_short}}에서 예상할 수 있는 성능 대기 시간은 어느 정도입니까?   

스토리지 내 대상 대기 시간은 <1ms입니다. 자사의 스토리지는 공유 네트워크의 컴퓨팅 인스턴스에 연결되기 때문에 정확한 성능 대기 시간은 지정된 시간 범위 내의 네트워크 트래픽에 따라 달라집니다. 

