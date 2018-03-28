---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-13"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.blockstorageshort}} 및 {{site.data.keyword.filestorage_short}}의 새 위치와 기능

{{site.data.keyword.BluSoftlayer_full}}에서 {{site.data.keyword.blockstorageshort}} 및 {{site.data.keyword.filestorage_full}}의 새 버전을 소개합니다! 새 스토리지는 엄선된 데이터 센터에서 사용 가능하며 저장 데이터에 대한 디스크 레벨의 암호화가 사용되는 더 높은 IOPS의 플래시 스토리지로 지원됩니다. 엄선된 데이터 센터에서 프로비저닝된 모든 스토리지는 {{site.data.keyword.blockstorageshort}} 및 {{site.data.keyword.filestorage_short}}의 새 버전으로 자동 프로비저닝됩니다. 

**참고:** 새 볼륨에 대한 NFS 마운트 위치가 변경되었습니다. 자세한 내용은 아래 **암호화된 {{site.data.keyword.filestorage_short}} 볼륨에 대한 새 마운트 지점**을 참조하십시오. 

현재 새 {{site.data.keyword.filestorage_short}}는 다음 지역/데이터 센터에서 사용 가능하며, 곧 추가 데이터 센터에서도 사용 가능할 예정입니다!
<table style="width:100%;">
	<caption>데이터 센터 가용성</caption>
	<tbody>
		<tr>
			<td><strong>US 2</strong></td>
			<td><strong>EU</strong></td>
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
					DAL13</p>
			</td>
			<td>
				<p>LON02<br />
				LON04<br />
				LON06<br />
				FRA02<br />
				AMS01<br />
				AMS03<br />
				OSLO1<br />
				PAR01<br />
				MIL01<br /></p>
			</td>
			<td>
				<p>SYD01<br />
				SYD04<br />
				MEL01<br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>TOR01<br />
					MON01<br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>MEX01<br />SAO01<br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
						<td>
				<p>TOK02<br />
				HKG02<br />
				SEO01<br />
				SNG01<br /><br /><br /><br /><br /><br /></p>
			</td>
			</tr>
	</tbody>
</table>


새 스토리지에는 다음과 같은 기능과 성능이 제공됩니다. 

-  [저장 데이터에 대한 제공자 관리 암호화](block-file-storage-encryption-rest.html). {{site.data.keyword.blockstorageshort}} 및 {{site.data.keyword.filestorage_short}} 모두 추가 비용 없이 자동으로 암호화된 상태로 프로비저닝됩니다. 
-  10 IOPS/GB 티어 옵션. 새 티어가 Endurance 유형 {{site.data.keyword.blockstorageshort}} 및 {{site.data.keyword.filestorage_short}}에 추가되어 요구되는 대부분의 워크로드를 지원합니다. 
-  모든 플래시 기반 스토리지. 모든 플래시 스토리지로 지원되는 2 IOPS/GB 이상에서 Endurance 또는 Performance로 프로비저닝되는 {{site.data.keyword.blockstorageshort}} 및 {{site.data.keyword.filestorage_short}}. 
-  스냅샷 및 복제는 Endurance 또는 Performance로 프로비저닝된 {{site.data.keyword.blockstorageshort}} 및 {{site.data.keyword.filestorage_short}}에서 지원됩니다. 
-  1개월 미만 사용에 플랜된 시간별 청구 옵션이 스토리지에 추가되었습니다.  
-  Performance로 프로비저닝된 {{site.data.keyword.blockstorageshort}} 및 {{site.data.keyword.filestorage_short}}에 대해 최대 48,000 IOPS. 
-  IOPS 등급은 계절별 로드 변경 시 성능을 향상시키기 위해 조정 가능합니다. [여기](adjustable-iops.html)에서 이 기능에 대해 자세히 알아보십시오. 
-  [{{site.data.keyword.filestorage_short}} 볼륨 복제 기능](how-to-create-duplicate-volume.html)을 통해 데이터의 새 복제본을 작성하십시오. 
- 스토리지는 상황에 따라 복제를 작성하거나 수동으로 데이터를 더 큰 볼륨으로 마이그레이션하지 않고도 최대 12TB까지 GB 증분 단위로 확장할 수 있습니다. [여기](expandable_file_storage.html)에서 이 기능에 대해 자세히 알아보십시오. 

## 암호화된 {{site.data.keyword.filestorage_short}} 볼륨에 대한 새 마운트 지점

이 데이터 센터에서 프로비저닝된 암호화된 모든 {{site.data.keyword.filestorage_short}} 볼륨에는 암호화되지 않은 볼륨과 다른 마운트 지점이 있습니다. 암호화된 파일 스토리지 볼륨과 암호화되지 않은 파일 스토리지 볼륨 모두에 대해 올바른 마운트 지점을 사용하는지 확인하기 위해 API 호출: `SoftLayer_Network_Storage::getNetworkMountAddress()`를 사용하여 올바른 마운트 지점에 액세스하고, UI를 통해 볼륨 세부사항 페이지에서 마운트 지점 정보를 볼 수 있습니다. 

추가 데이터 센터가 업그레이드된 경우 {{site.data.keyword.blockstorageshort}} 및 {{site.data.keyword.filestorage_short}}에 추가할 새롭게 사용 가능한 기능 및 성능을 여기에서 다시 확인하십시오. 
