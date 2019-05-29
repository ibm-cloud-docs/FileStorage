---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-26"

keywords: File Storage, encryption, security, provisioning, limitations, NFS

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:faq: data-hd-content-type='faq'}

# FAQ
{: #faqs}

## 암호화된 내 {{site.data.keyword.filestorage_short}} 볼륨을 어떻게 알 수 있습니까?
{: faq}

고객 포털에서 {{site.data.keyword.filestorage_short}} 목록을 보십시오. 암호화된 볼륨에 대한 볼륨 이름의 오른쪽에서 잠금 아이콘을 볼 수 있습니다.

## 암호화를 위해 업그레이드된 데이터 센터에서 암호화되지 않은 {{site.data.keyword.filestorage_short}}를 구매한 경우, 내 {{site.data.keyword.filestorage_short}}를 암호화할 수 있습니까?
{: faq}

데이터 센터 업그레이드 전에 프로비저닝된 {{site.data.keyword.filestorage_short}}를 암호화할 수 없습니다. 업그레이드된 데이터 센터에 프로비저닝된 새 {{site.data.keyword.filestorage_short}}가 자동으로 암호화됩니다. 이는 자동이며 선택하거나 무시해도 되는 프로비저닝 설정이 아닙니다. 암호화되지 않은 스토리지의 데이터는 새 파일 볼륨을 작성한 후에 호스트 기반 마이그레이션의 암호화된 새 볼륨에 데이터를 복사하여 암호화될 수 있습니다. 자세한 정보는 [File Storage 마이그레이션](/docs/infrastructure/FileStorage?topic=FileStorage-migratestorage)을 참조하십시오.

## 업그레이드된 데이터 센터에서 내가 {{site.data.keyword.filestorage_short}}를 프로비저닝 중임을 어떻게 알 수 있습니까?
{: faq}

업그레이드된 모든 데이터 센터가 {{site.data.keyword.filestorage_short}} 주문 양식에 별표(`*`)와 함께 표기됩니다. 주문 프로세스 중에 암호화를 사용하는 스토리지를 프로비저닝한다고 표시됩니다. 스토리지가 프로비저닝되면 스토리지 목록에 해당 볼륨이 암호화되었음을 나타내는 아이콘이 표시됩니다.

모든 암호화된 볼륨 및 파일 공유는 업그레이드된 데이터 센터에서만 프로비저닝됩니다. 사용 가능한 기능 및 업그레이드된 데이터 센터의 전체 목록은 [여기서](/docs/infrastructure/FileStorage?topic=FileStorage-news) 찾을 수 있습니다.

## Endurance 10IOPS 계층의 {{site.data.keyword.filestorage_short}}를 특정 데이터 센터에서만 프로비저닝할 수 있는 이유는 무엇입니까?
{: faq}

{{site.data.keyword.filestorage_short}} Endurance 유형 10IOPS/GB 계층은 특정 데이터 센터에서만 사용 가능하며, 새 데이터 센터가 곧 추가될 예정입니다. 사용 가능한 기능 및 업그레이드된 데이터 센터의 전체 목록은 [여기서](/docs/infrastructure/FileStorage?topic=FileStorage-news) 찾을 수 있습니다.

## 내 {{site.data.keyword.filestorage_short}}의 올바른 마운트 지점을 찾는 방법은 무엇입니까?
{: faq}

개선된 데이터 센터에서 프로비저닝된 모든 암호화된 {{site.data.keyword.filestorage_short}} 볼륨에는 암호화되지 않은 볼륨과는 다른 마운트 지점이 있습니다. 올바른 마운트 지점을 사용 중임을 확인하기 위해 UI의 **볼륨 세부사항** 페이지에서 마운트 지점 정보를 확인하십시오. 또한 API 호출 `SoftLayer_Network_Storage::getNetworkMountAddress()`를 통해 올바른 마운트 지점에 액세스할 수 있습니다.

## 프로비저닝할 수 있는 볼륨은 몇 개입니까?
{: faq}

기본적으로, 250개 블록 및 파일 스토리지 볼륨의 통합 총계를 프로비저닝할 수 있습니다. 한계를 늘리려면 영업 담당자에게 문의하십시오. 자세한 정보는 [스토리지 한계 관리](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits)를 참조하십시오.

## 프로비저닝된 {{site.data.keyword.filestorage_short}} 볼륨의 사용을 공유할 수 있는 인스턴스의 수는 얼마입니까?
{: faq}

파일 볼륨당 권한 부여의 수에 대한 기본 한계는 64개입니다. 이 한계를 늘리려면 영업 담당자에게 문의하십시오.

## 단일 호스트에 연결할 수 있는 {{site.data.keyword.filestorage_short}} 볼륨은 몇 개입니까?
{: faq}

호스트 운영 체제에서 처리할 수 있는 항목에 따라 달라지며 {{site.data.keyword.BluSoftlayer_full}}에서 제한하는 것은 아닙니다. 마운트할 수 있는 파일 공유 수에 대한 한계는 OS 문서를 참조하십시오.

## 파일 볼륨 크기당 허용되는 파일 공유의 수는 얼마입니까? 볼륨 크기당 허용된 최대 파일 공유의 수는 얼마입니까?
{: faq}

<table>
  <caption>표 1에서는 볼륨 크기에 따라 허용되는 최대 Inode 수를 표시합니다. 볼륨 크기는 왼쪽 열에 있습니다. Inode 및 파일 공유의 수는 오른쪽에 있습니다.</caption>
  <thead>
    <tr>
      <th>볼륨 크기</th>
      <th>Inodes 및 파일 공유</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>20GB - 39GB</td>
      <td>622,484</td>
    </tr>
    <tr>
      <td>40GB - 79GB</td>
      <td>1,245,084</td>
    </tr>          
    <tr>
      <td>80GB - 99GB</td>
      <td>2,490,263</td>
    </tr>          
    <tr>
      <td>100GB - 249GB</td>
      <td>3,112,863</td>
    </tr>          
    <tr>
      <td>250GB - 499GB</td>
      <td>7,782,300</td>
    </tr>          
    <tr>
      <td>500GB - 999GB</td>
      <td>15,564,695</td>
    </tr>
    <tr>
      <td>1TB</td>
      <td>31,876,593</td>
    </tr>
    <tr>
      <td>2TB</td>
      <td>63,753,186</td>
    </tr>
    <tr>
      <td>3TB</td>
      <td>95,629,970</td>
    </tr>
    <tr>
      <td>4TB - 12TB</td>
      <td>127,506,359</td>
    </tr>
   </tbody>
</table>

## IOPS 측정
{: faq}

IOPS는 랜덤 50퍼센트 읽기와 50퍼센트 쓰기인 16KB 블록의 로드 프로파일을 기반으로 측정됩니다. 이 프로파일과 다른 워크로드에는 성능 저하가 발생할 수 있습니다.

## 성능 측정 시에 보다 작은 블록 크기를 사용하면 어떻게 됩니까?
{: faq}

작은 블록 크기를 사용하는 경우에도 최대 IOPS를 얻을 수 있습니다. 그러나 이 경우에는 처리량이 줄어듭니다. 예를 들어, 다양한 블록 크기에서 IOPS가 6000인 볼륨의 처리량은 다음과 같습니다.

- 16KB * 6000IOPS == ~93.75MB/sec
- 8KB * 6000IOPS == ~46.88MB/sec
- 4KB * 6000IOPS == ~23.44MB/sec


## 할당된 IOPS는 인스턴스에 의해 적용됩니까, 아니면 볼륨에 의해 적용됩니까?
{: faq}

IOPS는 볼륨 레벨에서 적용됩니다. 달리 말하면, 6000IOPS의 볼륨에 연결된 두 개의 호스트는 해당 6000IOPS를 공유합니다.

## 예상 처리량을 얻기 위해 볼륨의 사전 워밍이 필요합니까?
{: faq}

사전 워밍이 필요하지 않습니다. 사용자는 볼륨이 프로비저닝되는 즉시 지정된 처리량을 관찰할 수 있습니다.

## 더 빠른 이더넷 연결이 사용되면 더 많은 처리량을 달성할 수 있습니까?
{: faq}

처리량 한계는 볼륨당 레벨에서 설정됩니다. 이 한계는 더 빠른 이더넷 연결을 사용하여 늘릴 수 없습니다. 그러나 보다 느린 이더넷 연결을 사용하면 대역폭에서 잠재적으로 병목 현상이 발생할 수 있습니다.

## 방화벽과 보안 그룹이 성능에 영향을 줍니까?
{: faq}

스토리지 트래픽은 방화벽을 우회하는 VLAN을 통해 전송하는 것이 좋습니다. 소프트웨어 방화벽을 통해 스토리지 트래픽을 전송하면 대기 시간이 늘어나며 스토리지 성능에 악영향을 줍니다.

## {{site.data.keyword.filestorage_short}}의 예상 성능 대기 시간은 얼마입니까?   
{: faq}

스토리지 내의 대상 대기 시간은 1ms 미만입니다. 스토리지는 공유 네트워크의 컴퓨팅 인스턴스에 연결되므로, 정확한 성능 대기 시간은 오퍼레이션 중의 네트워크 트래픽에 따라 달라집니다.

## {{site.data.keyword.filestorage_short}} 볼륨이 삭제되면 데이터는 어떻게 됩니까?
{: faq}

{{site.data.keyword.filestorage_full}}는 재사용 전에 삭제되는 물리적 스토리지의 고객에게 파일 공유를 표시합니다. 미디어 무결 처리에 대한 NIST 800-88 가이드라인과 같이 규제 준수에 대한 특별 요구사항이 있는 고객은 스토리지를 삭제하기 전에 데이터 무결 처리 프로시저를 수행해야 합니다.

## 지원되는 NFS 버전
{: faq}

NFS v3 및 NFS v4.1 모두는 {{site.data.keyword.cloud}} 환경에서 지원됩니다. 

NFS v3는 stateless 프로토콜이며 네트워크 이벤트가 발생할 때 복원력이 좋으므로 선호되는 버전입니다. 

NFS v3는 기본적으로 루트 클라이언트가 NFS 공유에 대해 루트 권한을 유지할 수 있게 하는 `no_root_squash`를 지원합니다. 도메인 정보를 편집하고 `rpcidmapd` 또는 유사한 서비스를 실행하여 NFS v4.1에서 이 기능을 사용할 수 있습니다. 자세한 정보는 [NFS에 대해 no_root_squash 구현](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux#norootsquash)을 참조하십시오.

vSphere 솔루션에 대해 NFS v3는 v4.1보다 많은 기능을 지원합니다. 이와 같은 기능에는 스토리지 DRS 및 사이트 복구 관리자가 포함됩니다.


## 클라우드 데이터 센터에서 폐기된 드라이브는 어떻게 됩니까?
{: faq}

드라이브가 폐기되는 경우 IBM은 이를 처분하기 전에 파기합니다. 해당 드라이브는 사용할 수 없는 상태가 됩니다. 해당 드라이브에 쓰여진 데이터는 더 이상 액세스할 수 없습니다.
