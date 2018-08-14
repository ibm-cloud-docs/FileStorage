---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-20"

---
{:new_window: target="_blank"}

# {{site.data.keyword.filestorage_short}}의 새 위치와 기능

{{site.data.keyword.BluSoftlayer_full}}에서는 {{site.data.keyword.filestorage_full}}의 새 버전을 도입합니다. 새 스토리지는 특정 데이터 센터에서 사용 가능하며, 저장 데이터의 디스크 레벨 암호화를 사용하는 상위 IOPS 레벨에서 플래시 스토리지의 지원을 받습니다. 특정 데이터 센터에서 주문된 모든 스토리지는 새 버전의 {{site.data.keyword.filestorage_short}}로 자동으로 작성됩니다.

**참고:** 새 볼륨의 NFS 마운트 지점이 변경되었습니다. 세부사항은 **개선된 {{site.data.keyword.filestorage_short}} 볼륨의 새 마운트 지점**을 참조하십시오.

새 {{site.data.keyword.filestorage_short}}는 다음의 지역/데이터 센터에서 사용 가능하며, 추가적인 데이터 센터도 나중에 사용할 수 있습니다.

<table role="presentation">
	<tr>
		<td><strong>미국 2</strong></td>
		<td><strong>유럽연합(EU)</strong></td>
		<td><strong>호주</strong></td>
		<td><strong>캐나다</strong></td>
		<td><strong>라틴 아메리카</strong></td>
		<td><strong>아시아 태평양</strong></td>
	</tr>
	<tr>
		<td><p>SJC03<br />
			SJC04<br />
			DC04<br />
			WDC06<br />
			WDC07<br />
			DAL09<br />
			DAL10<br />
			DAL12<br />
			DAL13<br /><br /><br /></p>
		</td>
		<td><p>LON02<br />
			LON04<br />
			LON06<br />
			FRA02<br />
			FRA04<br />
			FRA05<br />
			AMS01<br />
			AMS03<br />
			OSLO1<br />
			PAR01<br />
			MIL01<br /></p>
		</td>
		<td><p>SYD01<br />
			SYD04<br />
			MEL01<br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
		</td>
		<td><p>TOR01<br />
			MON01<br /><br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
		</td>
		<td><p>MEX01<br />
			SAO01<br /><br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
		</td>
		<td><p>TOK02<br />
			HKG02<br />
			SEO01<br />
			SNG01<br />
			CHE01<br /><br /><br /><br /><br /><br /><br /></p>
		</td>
	</tr>
</table>

*표 1에는 데이터 센터 가용성이 표시되어 있습니다. 각 지역에는 고유 열이 있습니다. 일부 도시(예: 댈러스, 산호세, 워싱턴 DC, 암스테르담, 프랑크푸르트, 런던 및 시드니)에는 여러 데이터 센터가 있습니다. *

새 스토리지에는 다음과 같은 특성과 기능이 있습니다.

- [저장 데이터에 대한 제공자 관리 암호화](block-file-storage-encryption-rest.html). <br/> 모든 {{site.data.keyword.filestorage_short}} 볼륨은 추가 비용 없이 암호화됨으로 자동 프로비저닝됩니다.
- 10IOPS/GB 계층 옵션. <br/> 가장 요구가 많은 워크로드를 지원하기 위해 새 계층이 Endurance 유형 {{site.data.keyword.filestorage_short}}에 추가되었습니다.
- 모든 플래시 지원 스토리지. <br/> 2IOPS/GB 이상에서 Endurance 또는 Performance 옵션으로 프로비저닝된 {{site.data.keyword.filestorage_short}}는 모든 플래시 스토리지에서 지원됩니다.
- 스냅샷 및 복제 지원.
- 전체 한달 미만의 사용이 계획된 스토리지에 대해 시간별 청구 옵션이 추가되었습니다.
- Performance 유형으로 프로비저닝된 {{site.data.keyword.filestorage_short}}에 대한 최대 48,000IOPS.
- 계절별 로드 변경의 성능 개선을 위해 IOPS 비율을 조정할 수 있습니다. [여기서](adjustable-iops.html) 이 기능에 대해 자세히 읽어 보십시오.
- [{{site.data.keyword.filestorage_short}} 볼륨 중복 기능](how-to-create-duplicate-volume.html)으로 데이터의 복제본을 작성합니다.
- 데이터를 더 큰 볼륨으로 수동으로 이동하거나 복제를 작성하지 않고도 스토리지를 GB 증분으로 최대 12TB까지 즉시 확장할 수 있습니다. [여기서](expandable_file_storage.html) 이 기능에 대해 자세히 읽어 보십시오.

## 개선된 {{site.data.keyword.filestorage_short}} 볼륨의 새 마운트 지점

이러한 데이터 센터에서 프로비저닝된 모든 개선된 {{site.data.keyword.filestorage_short}} 볼륨에는 암호화되지 않은 볼륨과는 다른 마운트 지점이 있습니다. 두 스토리지 볼륨에 올바른 마운트 지점을 사용 중임을 확인하기 위해 UI의 **볼륨 세부사항** 페이지에서 마운트 지점 정보를 볼 수 있습니다. 또한 API 호출 `SoftLayer_Network_Storage::getNetworkMountAddress()`를 통해 올바른 마운트 지점에 액세스할 수 있습니다.

{{site.data.keyword.filestorage_short}}용으로 추가 중인 새 기능에 대해 그리고 추가 데이터 센터가 업그레이드된 시기를 보려면 다시 여기를 확인하십시오.
