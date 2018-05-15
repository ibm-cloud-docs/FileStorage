---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-24"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.filestorage_short}} - 자주 묻는 질문

## IOPS를 어떻게 측정합니까?

IOPS는 랜덤 50% 읽기와 50% 쓰기인 16KB 블록의 로드 프로파일을 기반으로 측정됩니다. 이 프로파일과 다른 워크로드에는 성능 저하가 발생할 수 있습니다. 

## 성능 측정 시에 보다 작은 블록 크기를 사용하면 어떻게 됩니까? 

보다 작은 블록 크기를 사용해도 여전히 최대 IOPS를 얻을 수 있지만 처리량은 떨어집니다. 예를 들어, 6000 IOPS의 볼륨에 대한 처리량은 다양한 블록 크기에서 다음과 같습니다. 

- 16KB * 6000 IOPS == ~93.75MB/sec
- 8KB * 6000 IOPS == ~46.88MB/sec
- 4KB * 6000 IOPS == ~23.44MB/sec


## 예상 처리량을 얻기 위해 볼륨의 사전 워밍이 필요합니까?

사전 워밍이 필요하지 않습니다. 볼륨 프로비저닝 시에 즉각적으로 지정된 처리량을 관찰합니다. 

## 암호화된 내 {{site.data.keyword.filestorage_short}} LUN/볼륨을 어떻게 알 수 있습니까? 

고객 포털에서 {{site.data.keyword.filestorage_short}}의 목록을 볼 때 암호화된 대상의 LUN/볼륨 이름 오른쪽에 잠금 아이콘이 나타납니다. 

## 암호화되지 않은 {{site.data.keyword.filestorage_short}}가 암호화를 위해 업그레이드된 데이터 센터에서 프로비저닝된 경우, 내 {{site.data.keyword.filestorage_short}}를 암호화할 수 있습니까? 

데이터 센터 업그레이드 이전에 프로비저닝된 {{site.data.keyword.filestorage_short}}는 암호화될 수 없습니다. 업그레이드된 데이터 센터에서 프로비저닝된 새 {{site.data.keyword.filestorage_short}}는 자동으로 암호화됩니다. 암호화 설정을 선택할 필요가 없으며 이는 자동입니다. 업그레이드된 데이터 센터의 암호화되지 않은 스토리지의 데이터는 새 파일 볼륨을 작성한 후에 새 암호화된 볼륨 또는 호스트 기반 마이그레이션의 볼륨에 데이터를 복사하여 암호화될 수 있습니다. 마이그레이션을 수행하는 방법에 대한 지시사항은 [이 기사](/docs/infrastructure/FileStorage/migrate-file-storage-encrypted-file-storage.html)를 참조하십시오. 

## 업그레이드된 데이터 센터에서 내가 {{site.data.keyword.filestorage_short}}을 프로비저닝 중임을 어떻게 알 수 있습니까? 

{{site.data.keyword.filestorage_short}}를 프로비저닝할 때 업그레이드된 모든 데이터 센터는 주문 양식에서 별표(`*`) 및 암호화로 스토리지를 프로비저닝함의 표시로 표기됩니다. 일단 스토리지가 프로비저닝되면 해당 볼륨 또는 암호화된 볼륨을 표시하는 스토리지 목록의 아이콘이 나타납니다. 모든 암호화된 볼륨 및 볼륨은 업그레이드된 데이터 센터에서만 프로비저닝됩니다. 사용 가능한 기능 및 업그레이드된 데이터 센터의 전체 목록은 [여기서](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html) 찾을 수 있습니다. 

## 일부 데이터 센터에서는 Endurance 10 IOPS 계층의 {{site.data.keyword.filestorage_short}}를 프로비저닝할 수 있지만 기타 데이터 센터에서는 불가능한 이유는 무엇입니까? 

{{site.data.keyword.filestorage_short}} Endurance 유형 10 IOPS/GB 계층은 엄선된 데이터 센터에서만 사용 가능합니다. 새 데이터 센터는 곧 추가될 예정입니다. 사용 가능한 기능 및 업그레이드된 데이터 센터의 전체 목록은 [여기서](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html) 찾을 수 있습니다. 

## 내 {{site.data.keyword.filestorage_short}}의 올바른 마운트 지점을 찾는 방법은 무엇입니까? 

이러한 데이터 센터에서 프로비저닝된 모든 암호화된 {{site.data.keyword.filestorage_short}} 볼륨에는 암호화되지 않은 볼륨과는 다른 마운트 지점이 있습니다. 암호화 및 암호화되지 않은 {{site.data.keyword.filestorage_short}} 볼륨 모두에 대해 올바른 마운트 지점을 사용 중임을 확인하기 위해 UI의 **볼륨 세부사항** 페이지에서 마운트 지점 정보를 볼 수 있음은 물론, API 호출 `SoftLayer_Network_Storage::getNetworkMountAddress()`를 통해 올바른 마운트 지점에 액세스할 수도 있습니다. 

## 파일 볼륨 크기당 허용되는 파일 공유의 수는 얼마입니까? 볼륨 크기당 허용된 최대 파일 공유의 수는 얼마입니까? 
다음은 볼륨 크기를 기반으로 하는 최대 inode 또는 파일 공유의 수입니다. 

<table>
        <tbody>
          <tr>
            <th>볼륨 크기</th>
            <th>Inode/파일</th>
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

기본적으로, 250개 블록 및 파일 스토리지 볼륨의 통합 총계를 프로비저닝할 수 있습니다. 볼륨을 늘리려면 영업 담당자에게 문의하십시오. 

## 프로비저닝된 {{site.data.keyword.filestorage_short}} 볼륨의 사용을 공유할 수 있는 인스턴스의 수는 얼마입니까?

파일 볼륨당 권한 부여의 수에 대한 기본 한계는 64개입니다. 한계를 늘리려면 영업 담당자에게 문의하십시오. 

## Endurance 또는 Performance {{site.data.keyword.filestorage_short}}를 프로비저닝할 때 인스턴스별 또는 볼륨별로 적용되는 할당된 IOPS가 있습니까? 

IOPS는 볼륨 레벨에서 적용됩니다. 달리 말하면, 6000 IOPS의 볼륨에 연결된 두 개의 호스트는 해당 6000 IOPS를 공유합니다. 

## 보다 빠른 이더넷 연결을 사용하면 추가 처리량을 얻을 수 있습니까? 

처리량 한계가 볼륨/LUN당 레벨에서 설정되므로, 보다 빠른 이더넷 연결을 사용해도 해당 설정 한계는 늘어나지 않습니다. 그러나 보다 느린 이더넷 연결을 사용하면 대역폭에서 잠재적으로 병목 현상이 발생할 수 있습니다. 

## 방화벽/보안 그룹이 성능에 영향을 줍니까? 

최선의 방법으로서 방화벽을 우회하는 vlan에서 스토리지 트래픽을 실행하도록 권장합니다. 소프트웨어 방화벽을 통해 스토리지 트래픽을 실행하면 대기 시간이 증가되며 스토리지 성능에 악영향을 줍니다. 

## 내 {{site.data.keyword.filestorage_short}}에서 기대할 수 있는 성능 대기 시간은 얼마입니까?    

스토리지 내의 대상 대기 시간은 <1ms입니다. 스토리지가 공유 네트워크의 컴퓨팅 인스턴스에 연결되므로, 정확한 성능 대기 시간은 제공된 시간 범위 내의 네트워크 트래픽에 따라 다릅니다. 

## {{site.data.keyword.filestorage_short}} 볼륨이 삭제되면 내 데이터는 어떻게 됩니까? 

스토리지가 삭제되면 해당 볼륨의 데이터에 대한 포인터가 제거되므로 데이터에 대한 액세스가 완전히 차단됩니다. 물리적 스토리지가 다른 계정으로 다시 프로비저닝된 경우에는 새 포인터 세트가 지정됩니다. 새 계정이 물리적 스토리지에 있는 데이터에 액세스할 수 있는 방법은 없으며, 새 포인터 세트는 모두 0을 표시합니다. 데이터가 볼륨/LUN에 쓰여질 때 아직 존재하는 액세스 불가능한 데이터는 겹쳐쓰여집니다.  

## 클라우드 데이터 센터에서 폐기된 드라이브는 어떻게 됩니까? 

드라이브가 폐기될 때 IBM은 이를 더 이상 사용할 수 없도록 이를 처분하기 전에 파기합니다. 해당 드라이브에 쓰여진 데이터는 더 이상 액세스할 수 없습니다. 

