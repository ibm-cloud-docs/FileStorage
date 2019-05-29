---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, locations, data centers

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 新位置及特性
{: #news}

{{site.data.keyword.cloud}} 將引進新版的 {{site.data.keyword.filestorage_full}}！新的儲存空間會提供在精選資料中心內，並且由更高 IOPS 層次的快閃記憶體儲存空間支援，且具有靜態資料的磁碟層次加密。在精選資料中心訂購的所有儲存空間，都會自動使用新版本的 {{site.data.keyword.filestorage_short}} 建立。

新磁區的 NFS 裝載點已變更。如需詳細資料，請參閱[加強型 {{site.data.keyword.filestorage_short}} 磁區的新裝載點](#new-mount-point-for-enhanced-file-storage-volumes)區段。
{:important}

## 新位置
{: #new-locations}

下列地區和資料中心已提供新的 {{site.data.keyword.filestorage_short}}，後續將有更多資料中心加入此陣容！

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
        SYD05<br /><br /><br /><br /><br /><br /><br /><br /><br />
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

## 新特性及功能
{: #features}

- [靜態資料的提供者管理加密](/docs/infrastructure/FileStorage?topic=FileStorage-encryption)。<br/> 所有 {{site.data.keyword.filestorage_short}} 磁區都會自動佈建為已加密，不需額外付費。
- 每 GB 10 IOPS 層級選項。<br/> 「耐久性」類型 {{site.data.keyword.filestorage_short}} 已新增層級，可支援最嚴苛的工作負載。
- 全快閃記憶體支援的儲存空間。<br/> {{site.data.keyword.filestorage_short}} 已佈建每 GB 2 IOPS 或以上的「耐久性」或「效能」選項，並由全快閃記憶體儲存空間支援。
- Snapshot 及抄寫支援。
- 針對計劃使用期間不到一整個月的儲存空間，新增了「按小時計費」選項。
- 已佈建「效能」類型的 {{site.data.keyword.filestorage_short}} 最多有 48,000 IOPS。
- 可以調整 IOPS 速率，以改善季節性負載變更的效能。請在[這裡](/docs/infrastructure/FileStorage?topic=FileStorage-adjustingIOPS)深入閱讀此特性。
- 使用 [{{site.data.keyword.filestorage_short}} 磁區複製特性](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume)，以建立您資料的複製品。
- 可以立即擴充儲存空間（以 GB 為增量單位，最多可到 12 TB），而不需要建立重複項目，或手動將資料移動至較大的磁區。請在[這裡](/docs/infrastructure/FileStorage?topic=FileStorage-expandCapacity)深入閱讀此特性。

## 加強型 {{site.data.keyword.filestorage_short}} 磁區的新裝載點

這些資料中心內佈建的所有加強型 {{site.data.keyword.filestorage_short}} 磁區都具有與未加密磁區不同的裝載點。為了確保兩個儲存空間磁區都是使用正確的裝載點，您可以在主控台的**磁區詳細資料**頁面中檢視裝載點資訊。您也可以透過 API 呼叫來存取正確的裝載點：`SoftLayer_Network_Storage::getNetworkMountAddress()`。

若要能夠存取所有新增特性，請在透過 API 下訂單時，選取 `Storage-as-a-Service Package 759`。如需透過 API 來訂購 {{site.data.keyword.filestorage_short}} 的相關資訊，請參閱 [order_file_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/file/#SoftLayer.managers.file.FileStorageManager.order_file_volume){: external}。
{:important}

請在這裡再次確認，以查看其他資料中心何時升級以及針對 {{site.data.keyword.filestorage_short}} 新增的特性及功能。
{:tip}
