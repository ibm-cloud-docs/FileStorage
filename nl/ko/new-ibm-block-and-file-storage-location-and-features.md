---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-25"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.blockstorageshort}} 및 {{site.data.keyword.filestorage_short}}의 새 위치와 기능

{{site.data.keyword.BluSoftlayer_full}}에서는 {{site.data.keyword.blockstorageshort}} 및 {{site.data.keyword.filestorage_full}}의 새 버전을 도입합니다!  새 스토리지를 선택된 데이터 저장소에서 사용할 수 있으며, 저장 데이터의 디스크 레벨 암호화로 상위 IOPS 레벨에서 플래시 스토리지의 지원을 받습니다. 엄선된 데이터 센터의 모든 프로비저닝된 스토리지는 {{site.data.keyword.blockstorageshort}} 및 {{site.data.keyword.filestorage_short}}의 새 버전으로 자동으로 프로비저닝됩니다. 

**참고:** 새 볼륨의 NFS 마운트 지점이 변경되었습니다. 세부사항은 아래의 **암호화된 {{site.data.keyword.filestorage_short}} 볼륨의 새 마운트 지점**을 참조하십시오. 

새 {{site.data.keyword.filestorage_short}}는 현재 다음의 지역/데이터 센터에서 사용이 가능하며, 추가적인 데이터 센터도 곧 사용할 수 있습니다! 
<table style="width:100%;">
	<caption>데이터 센터 가용성</caption>
	<tbody>
		<tr>
			<td><strong>미국 2</strong></td>
			<td><strong>유럽연합(EU)</strong></td>
			<td><strong>오스트레일리아</strong></td>
			<td><strong>캐나다</strong></td>
			<td><strong>라틴 아메리카</strong></td>
			<td><strong>아시아 태평양</strong></td>
		</tr>
		<tr>
			<td>
				<p>SJC03<br />
				SJC04<br />
				WDC04<br />
				WDC06<br />
				WDC07<br />
				DAL09<br />
				DAL10<br />
				DAL12<br />
				DAL13<br /><br /></p>
			</td>
			<td>
				<p>LON02<br />
				LON04<br />
				LON06<br />
				FRA02<br />
				FRA04<br />
				AMS01<br />
				AMS03<br />
				OSLO1<br />
				PAR01<br />
				MIL01<br /></p>
			</td>
			<td>
				<p>SYD01<br />
				SYD04<br />
				MEL01<br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>TOR01<br />
				MON01<br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>MEX01<br />SAO01<br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
						<td>
				<p>TOK02<br />
				HKG02<br />
				SEO01<br />
				SNG01<br />
				CHE01<br /><br /><br /><br /><br /><br /></p>
			</td>
			</tr>
	</tbody>
</table>


새 스토리지에는 다음과 같은 특성과 기능이 있습니다. 

-  [저장 데이터에 대한 제공자 관리 암호화](block-file-storage-encryption-rest.html). 모든 {{site.data.keyword.blockstorageshort}} 및 {{site.data.keyword.filestorage_short}}는 추가 비용 없이 암호화로서 자동으로 프로비저닝됩니다. 
-  10 IOPS / GB 계층 옵션. 가장 요구가 많은 워크로드를 지원하기 위해 새 계층이 Endurance 유형 {{site.data.keyword.blockstorageshort}} 및 {{site.data.keyword.filestorage_short}}에 추가되었습니다. 
-  모든 플래시 지원 스토리지. 모든 플래시 스토리지의 지원으로 2 IOPS / GB 이상에서 Endurance 또는 Performance으로 프로비저닝된 {{site.data.keyword.blockstorageshort}} 및 {{site.data.keyword.filestorage_short}}. 
-  Endurance 또는 Performance으로 프로비저닝된 {{site.data.keyword.blockstorageshort}} 및 {{site.data.keyword.filestorage_short}}의 스냅샷 및 복제 지원. 
-  전체 한달 미만의 사용이 계획된 스토리지에 대해 시간별 청구 옵션이 추가되었습니다.  
-  Performance으로 프로비저닝된 {{site.data.keyword.blockstorageshort}} 및 {{site.data.keyword.filestorage_short}}에 대해 최대 48,000 IOPS. 
-  계절별 로드 변경의 경우 성능 개선을 위해 IOPS 비율을 조정할 수 있습니다. [여기서](adjustable-iops.html) 이 기능에 대해 자세히 읽어 보십시오. 
-  [{{site.data.keyword.filestorage_short}} 볼륨 중복 기능](how-to-create-duplicate-volume.html)으로 데이터의 새 복제본을 작성합니다. 
- 데이터를 보다 큰 볼륨으로 수동으로 마이그레이션하거나 복제를 작성하지 않고도 스토리지를 GB 증분으로 최대 12TB까지 언제든지 확장할 수 있습니다. [여기서](expandable_file_storage.html) 이 기능에 대해 자세히 읽어 보십시오. 

## 암호화된 {{site.data.keyword.filestorage_short}} 볼륨의 새 마운트 지점

이러한 데이터 센터에서 프로비저닝된 모든 암호화된 {{site.data.keyword.filestorage_short}} 볼륨에는 암호화되지 않은 볼륨과는 다른 마운트 지점이 있습니다. 암호화 및 암호화되지 않은 파일 스토리지 볼륨 모두에 대해 올바른 마운트 지점을 사용 중임을 확인하기 위해 UI의 볼륨 세부사항 페이지에서 마운트 지점 정보를 볼 수 있음은 물론, API 호출 `SoftLayer_Network_Storage::getNetworkMountAddress()`를 통해 올바른 마운트 지점에 액세스할 수도 있습니다. 

{{site.data.keyword.blockstorageshort}} 및 {{site.data.keyword.filestorage_short}}용으로 추가 중인 새로 사용 가능한 특성과 기능에 대해서와 추가 데이터 센터가 업그레이드된 시점을 보려면 다시 여기를 확인하십시오. 
