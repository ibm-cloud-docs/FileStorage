---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-11"

keywords: File Storage, file storage, NFS, replication, duplication, synchronous, replica schedule, replica space, disaster recovery

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}


# 데이터 복제
{: #replication}

복제는 스냅샷 스케줄 중 하나를 사용하여 스냅샷을 원격 데이터 센터의 대상 볼륨으로 자동으로 복사합니다. 중대한 문제가 발생하거나 데이터가 손상되는 경우에는 원격 사이트로부터 사본을 복구할 수 있습니다.

복제는 서로 다른 두 위치에 동기화된 데이터를 보관합니다. 볼륨을 복제한 후에 이를 원래 볼륨과 독립적으로 사용하려면 [중복 파일 볼륨 작성](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume)을 참조하십시오.
{:tip}

복제하려면 우선 스냅샷 스케줄을 작성해야 합니다.
{:important}


## 복제된 스토리지 볼륨의 원격 데이터 센터 결정

{{site.data.keyword.BluSoftlayer_full}}의 데이터 센터는 전세계에서 기본 및 원격의 조합으로 쌍을 이루고 있습니다.
데이터 센터 가용성 및 복제 대상의 전체 목록은 표 1을 참조하십시오.

<table>
  <caption style="text-align: left;"><p>표 1 - 이 표에서는 각 지역에 개선된 기능이 있는 데이터 센터의 전체 목록을 표시합니다. 모든 지역은 개별 열입니다. 일부 도시(예: 댈러스, 산호세, 워싱턴 DC, 암스테르담, 프랑크푸르트, 런던 및 시드니)에는 다수의 데이터 센터가 있습니다.</p>
  <p>&#42; 미국 1 지역의 데이터 센터에는 개선된 스토리지가 없습니다. 개선된 스토리지 기능이 있는 데이터 센터의 호스트는 미국 1 데이터 센터의 복제본 대상에 대해 복제를 <strong>시작할 수 없습니다</strong>.</p>
  </caption>
  <thead>
    <tr>
      <th>미국 1 &#42;</th>
      <th>미국 2</th>
      <th>라틴 아메리카</th>
      <th>캐나다</th>
      <th>유럽</th>
      <th>아시아-태평양</th>
      <th>오스트레일리아</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>DAL01<br />
DAL05<br />
	  DAL06<br />
	  HOU02<br />
	  SJC01<br />
	  WDC01<br />
	  <br /><br /><br /><br /><br /><br />
      </td>
      <td>SJC03<br />
	  SJC04<br />
	  WDC04<br />
	  WDC06<br />
	  WDC07<br />
	  DAL09<br />
	  DAL10<br />
	  DAL12<br />
	  DAL13<br />
	  <br /><br /><br />
      </td>
      <td>MEX01<br />
	  SAO01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
      <td>TOR01<br />
MON01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
      <td>AMS01<br />
	  AMS03<br />
	  FRA02<br />
	  FRA04<br />
	  FRA05<br />
	  LON02<br />
	  LON04<br />
	  LON05<br />
	  LON06<br />
	  OSL01<br />
	  PAR01<br />
	  MIL01<br />
      </td>
      <td>HKG02<br />
TOK02<br />
	  TOK04<br />
	  TOK05<br />
	  SNG01<br />
	  SEO01<br />
                                CHE01<br />
	  <br /><br /><br /><br /><br />
      </td>
      <td>SYD01<br />
SYD04<br />
          SYD05<br />
MEL01<br />
          <br /><br /><br /><br /><br /><br /><br /><br />
      </td>
    </tr>
  </tbody>
</table>

## 초기 복제본 작성

복제는 스냅샷 스케줄에 따라 이뤄집니다. 복제하려면 먼저 스냅샷 영역과 소스 볼륨에 대한 스냅샷 스케줄이 있어야 합니다. 복제를 설정하려는데 두 가지 중 하나가 없는 경우에는 추가 영역을 구매하거나 스케줄을 설정하라는 프롬프트가 표시됩니다. 복제는 [[{{site.data.keyword.slportal}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/){:new_window}의 **스토리지** > **{{site.data.keyword.filestorage_short}}**에서 관리합니다.

1. 스토리지 볼륨을 클릭하십시오.
2. **복제본**을 클릭하고 **복제 구매**를 클릭하십시오.
3. 복제가 따르도록 할 기존 스냅샷 스케줄을 선택하십시오. 목록에는 모든 활성 스냅샷 스케줄이 있습니다. <br />

   시간별, 일별, 주별이 혼합되어 있어도 스케줄은 하나만 선택할 수 있습니다. 이전 복제 주기 이후 캡처된 모든 스냅샷은 이를 발생시킨 스케줄에 관계없이 복제됩니다.<br />설정된 스냅샷이 없는 경우에는 복제를 주문하기 전에 이를 설정하라는 프롬프트가 표시됩니다. 자세한 정보는 [스냅샷 관련 작업](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots)을 참조하십시오.
   {:tip}
3. **위치**를 클릭하고 DR 사이트인 데이터 센터를 선택하십시오.
4. **계속**을 클릭하십시오.
5. **프로모션 코드**를 입력하고(있는 경우) **재계산**을 클릭하십시오. 창에 있는 다른 필드는 기본적으로 완료되어 있습니다.

   주문을 처리할 때 할인이 적용됩니다.
   {:note}
6. **마스터 서비스 계약을 읽었습니다…** 선택란을 클릭하고 **주문하기**를 클릭하십시오.


## 기존 복제 편집

[{{site.data.keyword.slportal}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/){:new_window}의 **스토리지** > **{{site.data.keyword.filestorage_short}}**에 있는 **1차** 또는 **복제본**에서 복제 스케줄을 편집하고 복제본 영역을 변경할 수 있습니다.


## 복제 스케줄 편집

복제 스케줄은 기존 스냅샷 스케줄을 기반으로 합니다. 예를 들어, 시간별에서 일별이나 주별로 또는 그 반대로 복제본 스케줄을 변경하려면 복제본 볼륨을 취소하고 새로 하나를 설정해야 합니다.

하지만 **일별** 복제가 발생할 때 시간을 변경하려면 기본 또는 복제본 탭에서 기존 스케줄을 조정할 수 있습니다.

1. **기본** 또는 **복제본** 탭에서 **조치**를 클릭하십시오.
2. **스냅샷 스케줄 편집**을 선택하십시오.
3. **스케줄**에 있는 **스냅샷** 프레임을 보고 복제에 사용 중인 스케줄을 판별하십시오. 원하는 스케줄을 변경하십시오.
4. **저장**을 클릭하십시오.


## 복제 영역 변경

기본 스냅샷 영역과 복제본 영역은 동일해야 합니다. **기본** 또는 **복제본** 탭에서 영역을 변경하면 소스 및 대상 데이터 센터 둘 다에서 영역이 자동으로 추가됩니다. 스냅샷 영역을 늘리면 즉각적 복제 업데이트 또한 트리거됩니다.

1. **기본** 또는 **복제본** 탭에서 **조치**를 클릭하십시오.
2. **스냅샷 영역 더 추가**를 선택하십시오.
3. 목록에서 스토리지 크기를 선택하고 **계속**을 클릭하십시오.
4. **프로모션 코드**(있는 경우)를 입력하고 **재계산**을 클릭하십시오. 대화 상자에 있는 다른 필드는 기본적으로 완료되어 있습니다.
5. **마스터 서비스 계약을 읽었습니다…** 선택란을 클릭하고 **주문하기**를 클릭하십시오.


## 기본 데이터 센터에서 스냅샷 영역이 늘어난 경우 복제본 데이터 센터에서 스냅샷 영역 늘리기

볼륨 크기는 기본 스토리지 볼륨과 복제본 스토리지 볼륨 간에 동일해야 합니다. 한 쪽이 다른 쪽보다 커서는 안 됩니다. 기본 볼륨의 스냅샷 영역을 늘리면 복제본 영역이 자동으로 늘어납니다. 스냅샷 영역을 늘리면 즉각적 복제 업데이트가 트리거됩니다. 두 볼륨의 증가는 청구서에 품목명으로 표시되며 필요에 따라 비례 배분됩니다.

스냅샷 영역 늘리기에 대한 자세한 정보는 [스냅샷](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots)을 참조하십시오.
## 볼륨 목록에서 복제본 볼륨 보기

**스토리지** > **{{site.data.keyword.filestorage_short}}**에 있는 {{site.data.keyword.filestorage_short}} 페이지에서 복제 볼륨을 볼 수 있습니다. 이러한 볼륨의 이름에서는 기본 볼륨 이름이 있고 그 뒤에 REP가 있습니다. **유형**은 Endurance 또는 Performance - 복제본입니다. 복제본 볼륨이 복제본 데이터 센터에 마운트되지 않았으며 **상태**가 비활성이므로 **대상 주소**는 해당사항 없음입니다.


## 복제본 데이터 센터에서 복제된 볼륨의 세부사항 보기

**스토리지** > **{{site.data.keyword.filestorage_short}}** 아래의 **복제본** 탭에서 복제본 볼륨 세부사항을 볼 수 있습니다. 다른 옵션은 **{{site.data.keyword.filestorage_short}}** 페이지에서 복제본 볼륨을 선택하고 **복제본** 탭을 클릭하는 것입니다.


## 복제 히스토리 보기

복제 히스토리는 **관리** 아래 **계정** 탭을 통해 **감사 로그**에 표시됩니다. 기본 볼륨과 복제본 볼륨 모두 동일한 복제 히스토리를 표시하며 여기에는 다음이 포함됩니다.

- 복제 유형(장애 복구 또는 장애 조치)
- 시작 시점
- 복제에 사용된 스냅샷
- 복제의 크기
- 완료 시점


## 복제본의 복제 작성

기존 {{site.data.keyword.BluSoftlayer_full}} {{site.data.keyword.filestorage_full}}의 복제를 작성할 수 있습니다. 복제 볼륨은 기본적으로 원래 스토리지 볼륨의 용량 및 성능 옵션을 상속하며 스냅샷의 특정 시점까지의 데이터 사본을 보유합니다.

복제는 기본 및 복제본 볼륨으로부터 작성될 수 있습니다. 새 복제는 원래 볼륨과 동일한 데이터 센터에 작성됩니다. 복제본 볼륨에서 복제를 작성하는 경우에는 새 볼륨이 복제본 볼륨과 동일한 데이터 센터에 작성됩니다.

복제 볼륨은 스토리지가 프로비저닝되는 순간 읽기/쓰기를 위해 호스트에 의해 액세스될 수 있습니다. 그러나 스냅샷 및 복제는 원본에서 복제본으로 데이터 복사가 완료될 때까지 허용되지 않습니다.

자세한 정보는 [복제 파일 볼륨 작성](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume)을 참조하십시오.

## 재해 발생 시 복제본을 사용하여 장애 복구

장애 복구 시에 기본 데이터 센터의 스토리지 볼륨에서 원격 데이터 센터의 대상 볼륨으로 "스위치를 전환"합니다. 예를 들어, 기본 데이터 센터는 런던이고 보조 데이터 센터는 암스테르담입니다. 장애가 발생하면 암스테르담으로의 장애 복구가 실행되어 암스테르담의 컴퓨팅 인스턴스에서 현재 기본 볼륨으로 연결됩니다. 런던의 볼륨이 복구된 후에 암스테르담 볼륨의 스냅샷을 가져와 런던으로 장애 복구하고 런던의 컴퓨팅 인스턴스의 기본 볼륨으로 다시 연결합니다.

* 1차 위치에서 문제점이 발생하지만 스토리지와 호스트는 여전히 온라인이면 [액세스 가능한 1차 볼륨으로 장애 복구](/docs/infrastructure/FileStorage?topic=FileStorage-dr-accessible)를 참조하십시오.
* 1차 위치가 작동 중지된 경우 [액세스할 수 없는 1차 볼륨으로 장애 복구](/docs/infrastructure/FileStorage?topic=FileStorage-dr-inaccessible)를 참조하십시오.

## 기존 복제 취소

복제는 즉시 취소하거나 청구가 종료되는 주기 날짜에 취소할 수 있습니다. 복제는 **기본** 또는 **복제본** 탭에서 취소 가능합니다.

1. **{{site.data.keyword.filestorage_short}}** 페이지에서 볼륨을 클릭하십시오.
2. **기본** 또는 **복제본** 탭에서 **조치**를 클릭하십시오.
3. **복제본 취소**를 선택하십시오.
4. 취소 시기를 선택하십시오. **즉시** 또는 **주기 날짜**를 선택하고 **계속**을 클릭하십시오.
5. **취소로 인해 데이터가 유실될 수 있음을 인지함**을 클릭하고 **복제본 취소**를 클릭하십시오.


## 기본 볼륨이 취소된 경우의 복제 취소

기본 볼륨이 취소되면 복제 데이터 센터의 볼륨 및 복제 스케줄이 삭제됩니다. 복제본은 **{{site.data.keyword.filestorage_short}}** 페이지에서 취소됩니다.

 1. **{{site.data.keyword.filestorage_short}}** 페이지에서 볼륨을 강조표시하십시오.
 2. **조치**를 클릭하고 **{{site.data.keyword.filestorage_short}} 취소**를 선택하십시오.
 3. 볼륨 취소 시점을 선택하십시오. **즉시** 또는 **주기 날짜**를 선택하고 **계속**을 클릭하십시오.
 4. **취소로 인해 데이터가 유실될 수 있음을 인지함**을 클릭하고 **취소**를 클릭하십시오.

## SLCLI의 복제 관련 명령
{: #clicommands}

* 특정 볼륨에 적합한 복제 데이터 센터를 나열합니다.
  ```
  # slcli file replica-locations --help
  Usage: slcli file replica-locations [OPTIONS] VOLUME_ID

  Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: ID, Long Name, Short Name
  -h, --help      Show this message and exit.
  ```

* 파일 스토리지 복제본 볼륨을 주문합니다.
  ```
  # slcli file replica-order --help
  Usage: slcli file replica-order [OPTIONS] VOLUME_ID

  Options:
  -s, --snapshot-schedule [INTERVAL|HOURLY|DAILY|WEEKLY]
                                  Snapshot schedule to use for replication,
                                  (INTERVAL | HOURLY | DAILY | WEEKLY)
                                  [required]
  -l, --location TEXT             Short name of the data center for the
                                  replicant (e.g.: dal09)  [required]
  --tier [0.25|2|4|10]            Endurance Storage Tier (IOPS per GB) of the
                                  primary volume for which a replicant is
                                  ordered [optional]
  -h, --help                      Show this message and exit.
  ```

* 파일 볼륨에 대한 기존의 복제 볼륨을 나열합니다.
  ```
  # slcli file replica-partners --help
  Usage: slcli file replica-partners [OPTIONS] VOLUME_ID

  Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: ID, Username, Account ID,
                  Capacity (GB), Hardware ID, Guest ID, Host ID
  -h, --help      Show this message and exit.
  ```

* 특정 복제 볼륨에 파일 볼륨의 장애 조치를 수행합니다.
  ```
  # slcli file replica-failover --help
  Usage: slcli file replica-failover [OPTIONS] VOLUME_ID

  Options:
  --replicant-id TEXT  ID of the replicant volume
  --immediate          Failover to replicant immediately.
  -h, --help           Show this message and exit.
  ```

* 특정 복제 볼륨에서 파일 볼륨의 장애 조치를 수행합니다.
  ```
  # slcli file replica-failback --help
  Usage: slcli file replica-failback [OPTIONS] VOLUME_ID

  Options:
  --replicant-id TEXT  ID of the replicant volume
  -h, --help           Show this message and exit.
  ```
