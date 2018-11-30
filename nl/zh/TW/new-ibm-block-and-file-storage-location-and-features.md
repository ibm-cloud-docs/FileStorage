---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
 {:tip: .tip}
 {:note: .note}
 {:important: .important}

# {{site.data.keyword.filestorage_short}} 的新位置及特性

{{site.data.keyword.BluSoftlayer_full}} 將引進新版的 {{site.data.keyword.filestorage_full}}！新的儲存空間會提供在精選資料中心內，並且由更高 IOPS 層次的快閃記憶體儲存空間支援，且具有靜態資料的磁碟層次加密。在精選資料中心訂購的所有儲存空間，都會自動使用新版本的 {{site.data.keyword.filestorage_short}} 建立。

新磁區的 NFS 裝載點已變更。如需詳細資料，請參閱[加強型 {{site.data.keyword.filestorage_short}} 磁區的新裝載點](#new-mount-point-for-enhanced-file-storage-volumes)區段。
{:important}

下列地區/資料中心已提供新的 {{site.data.keyword.filestorage_short}}，其他資料中心稍後即會推出！

<table role="presentation">
  <tr>
    <td><strong>美國 2</strong></td>
    <td><strong>歐盟</strong></td>
    <td><strong>澳大利亞</strong></td>
    <td><strong>加拿大</strong></td>
    <td><strong>拉丁美洲</strong></td>
    <td><strong>亞太</strong></td>
  </tr>
  <tr>
    <td>DAL09<br />
	DAL10<br />
	DAL12<br />
	DAL13<br />
	SJC03<br />
SJC04<br />
	WDC04<br />
	WDC06<br />
	WDC07<br />
	<br /><br /><br />
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
	MIL01<br />
	OSLO1<br />
	PAR01<br />
    </td>
    <td>MEL01<br />
SYD01<br />
SYD04<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>MON01<br />
TOR01<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>MEX01<br />
SAO01<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>CHE01<br />
HKG02<br />
	SEO01<br />
	SNG01<br />
TOK02<br />
	TOK04<br />
	TOK05<br />
	<br /><br /><br /><br /><br />
    </td>
  </tr>
</table>

*表 1 顯示「資料中心可用性」。每一個地區都有自己的直欄。有些城市（例如「達拉斯」、「聖荷西」、「華盛頓特區」、「阿姆斯特丹」、「法蘭克福」、「倫敦」及「雪梨」）會有多個資料中心。*

新的儲存空間具有下列特性及功能：

- [靜態資料的提供者管理加密](block-file-storage-encryption-rest.html)。<br/> 所有 {{site.data.keyword.filestorage_short}} 磁區都會自動佈建為已加密，不需額外付費。
- 每 GB 10 IOPS 層級選項。<br/> 「耐久性」類型 {{site.data.keyword.filestorage_short}} 已新增層級，可支援最嚴苛的工作負載。
- 全快閃記憶體支援的儲存空間。<br/> {{site.data.keyword.filestorage_short}} 已佈建每 GB 2 IOPS 或以上的「耐久性」或「效能」選項，並由全快閃記憶體儲存空間支援。
- Snapshot 及抄寫支援。
- 針對計劃使用期間不到一整個月的儲存空間，新增了「按小時計費」選項。
- 已佈建「效能」類型的 {{site.data.keyword.filestorage_short}} 最多有 48,000 IOPS。
- 可以調整 IOPS 速率，以改善季節性負載變更的效能。請在[這裡](adjustable-iops.html)深入閱讀此特性。
- 使用 [{{site.data.keyword.filestorage_short}} 磁區複製特性](how-to-create-duplicate-volume.html)，以建立您資料的複製品。
- 可以立即擴充儲存空間（以 GB 為增量單位，最多可到 12 TB），而不需要建立重複項目，或手動將資料移動至較大的磁區。請在[這裡](expandable_file_storage.html)深入閱讀此特性。

## 加強型 {{site.data.keyword.filestorage_short}} 磁區的新裝載點

這些資料中心內佈建的所有加強型 {{site.data.keyword.filestorage_short}} 磁區都具有與未加密磁區不同的裝載點。為確保您對兩個儲存空間磁區都使用正確的裝載點，您可以在使用者介面的**磁區詳細資料**頁面中檢視裝載點資訊。您也可以透過 API 呼叫來存取正確的裝載點：`SoftLayer_Network_Storage::getNetworkMountAddress()`。

請在這裡再次確認，以查看其他資料中心何時升級以及針對 {{site.data.keyword.filestorage_short}} 新增的特性及功能。
{:tip}
