---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-01"

keywords: File Storage, encryption, security, provisioning, limitations, NFS

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:faq: data-hd-content-type='faq'}

# 常見問題
{: #file-storage-faqs}

## 如何分辨哪些 {{site.data.keyword.filestorage_short}} 磁區已加密？
{: faq}

請查看客戶入口網站中的 {{site.data.keyword.filestorage_short}} 清單。您會看到已加密磁區的磁區名稱右側有一個鎖定圖示。

## 如果我在已升級進行加密的資料中心內購買了未加密的 {{site.data.keyword.filestorage_short}}，則是否可以加密我的 {{site.data.keyword.filestorage_short}}？
{: faq}

在資料中心升級之前所佈建的 {{site.data.keyword.filestorage_short}} 無法加密。佈建在已升級資料中心內的新 {{site.data.keyword.filestorage_short}} 會自動加密。這是自動作業，不是可供選取或不選取的佈建設定。未加密儲存空間上的資料加密，可以藉由建立新磁區，然後透過主機型移轉將資料複製到新的已加密磁區來進行。如需相關資訊，請參閱[移轉檔案儲存空間](/docs/infrastructure/FileStorage?topic=FileStorage-migratestorage)。

## 如何知道是否在已升級資料中心內佈建 {{site.data.keyword.filestorage_short}}？
{: faq}

在 {{site.data.keyword.filestorage_short}} 訂單表格中，所有已升級的資料中心都會註記星號 (`*`)。在訂購程序期間，會提供您指示，指出您正在佈建加密的儲存空間。佈建儲存空間時，您可以看到儲存空間清單中有一個圖示，顯示該磁區已加密。

所有加密磁區及檔案共用都只會佈建在已升級的資料中心內。您可以在[這裡](/docs/infrastructure/FileStorage?topic=FileStorage-selectDC)找到完整的已升級資料中心及可用特性清單。

## 為什麼可以在某些資料中心內佈建具有「耐久性 10 IOPS」層級的 {{site.data.keyword.filestorage_short}}，但不能在其他資料中心內進行？
{: faq}

「{{site.data.keyword.filestorage_short}} 耐久性類型 10 IOPS/GB」層級僅適用於精選資料中心，並且很快會新增資料中心。您可以在[這裡](/docs/infrastructure/FileStorage?topic=FileStorage-selectDC)找到完整的已升級資料中心及可用特性清單。

## 如何找到 {{site.data.keyword.filestorage_short}} 的正確裝載點？
{: faq}

加強型資料中心內佈建的所有加密 {{site.data.keyword.filestorage_short}} 磁區都具有與未加密磁區不同的裝載點。為確保您使用正確的裝載點，請檢視使用者介面中**磁區詳細資料**頁面上的裝載點資訊。您也可以透過 API 呼叫來存取正確的裝載點：`SoftLayer_Network_Storage::getNetworkMountAddress()`。

## 可以佈建多少個磁區？
{: faq}

依預設，您可以佈建總計 250 個區塊及檔案儲存空間磁區。若要增加限制，請與業務代表聯絡。如需相關資訊，請參閱[管理儲存空間限制](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits)。

## 有多少實例可以共用已佈建的 {{site.data.keyword.filestorage_short}} 磁區？
{: faq}

每個檔案磁區的預設授權數目限制是 64。若要增加此限制，請與業務代表聯絡。

## 可以將多少個 {{site.data.keyword.filestorage_short}} 磁區連接至單一主機？
{: faq}

這取決於主機作業系統可處理的項目，而不是由 {{site.data.keyword.cloud}} 所限制。如需可裝載的檔案共用數目限制，請參閱 OS 文件。

## 每個檔案磁區大小容許多少個檔案共用？每個磁區大小容許的檔案共用上限為何？
{: #maxfilevolume}
{: faq}

|磁區大小|Inode 和檔案共用|
|-----|-----|
|20 GB - 39 GB|622,484|
|40 GB - 79 GB|1,245,084|
|80 GB - 99 GB|2,490,263|
|100 GB - 249 GB|3,112,863|
|250 GB - 499 GB|7,782,300|
|500 GB - 999 GB|15,564,695|
|1 TB|31,876,593|
|2 TB|63,753,186|
|3 TB|95,629,970|
| 4 TB |127,506,359|
{: row-headers}
{: class="comparison-table"}
{: caption="表格比較" caption-side="top"}
{: summary="Table 1 shows the maximum number of inodes that are allowed based on the volume size. Volume sizes are in the left column. The number of inodes and file shares are on the right."}

## 測量 IOPS
{: faq}

IOPS 根據具有隨機 50% 讀取及 50% 寫入之 16 KB 區塊的載入設定檔進行測量。與此設定檔不同的工作負載可能會經歷效能降低。

## 如果我在測量效能時使用較小的區塊大小，會發生什麼情況？
{: faq}

即使您使用較小的區塊大小，也可以取得 IOPS 的最大數目。不過，在此情況下，傳輸量較少。例如，具有 6000 IOPS 的磁區具有下列各種區塊大小的傳輸量：

- 16 KB * 6000 IOPS == ~93.75 MB/秒
- 8 KB * 6000 IOPS == ~46.88 MB/秒
- 4 KB * 6000 IOPS == ~23.44 MB/秒


## 是依實例還是依磁區施行已配置的 IOPS？
{: faq}

IOPS 是在磁區層次上施行。換句話說，連接至具有 6000 IOPS 之磁區的兩台主機會共用該 6000 IOPS。

## 磁區是否需要預先暖機，才能達到預期的傳輸量？
{: faq}

不需要預先暖機。佈建磁區時，您可以立即觀察到指定的傳輸量。

## 如果使用速度更快的乙太網路連線，可以達到更多的傳輸量嗎？
{: faq}

傳輸量限制是依每個磁區層次設定。無法藉由使用更快的乙太網路來提高該限制。不過，乙太網路連線較慢時，您的頻寬可能是潛在瓶頸。

## 防火牆和安全群組是否會影響效能？
{: #isolatedstoragetraffic}
{: faq}

最好是在 VLAN 上執行儲存空間資料流量，這樣會略過防火牆。透過軟體防火牆執行儲存空間資料流量，會增加延遲，而且會對儲存空間效能造成不利的影響。

## {{site.data.keyword.filestorage_short}} 的預期效能延遲為何？   
{: faq}

儲存空間內的目標延遲為小於 1 毫秒。儲存空間會連接至共用網路上的運算實例，因此，確切的效能延遲取決於該作業期間的網路資料流量。

## 刪除 {{site.data.keyword.filestorage_short}} 磁區時，資料會發生什麼情況？
{: faq}

{{site.data.keyword.filestorage_full}} 會向客戶呈現在任何重複使用之前抹除的實體儲存空間上的檔案共用。具有特殊規範需求的客戶（例如 NIST 800-88 媒體資料安全清除準則）必須先執行資料安全清除程序，再刪除其儲存空間。

## 支援哪些 NFS 版本？
{: faq}

在 {{site.data.keyword.cloud}} 環境中，同時支援 NFS 第 3 版及 NFS 4.1 版。

偏好的版本是 NFS 第 3 版，因為它是無狀態的通訊協定，在發生網路事件時較具復原力。

NFS 第 3 版原本便支援 `no_root_squash`，可讓 root 用戶端保留對 NFS 共用的 root 許可權。您可以在 NFS 4.1 版中啟用此特性，方法是編輯網域資訊然後執行 `rpcidmapd` 或類似的服務。如需相關資訊，請參閱[為 NFS 實作 no_root_squash](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux#norootsquash)。

在談到 vSphere Solutions 時，NFS 第 3 版比 4.1 版支援更多特性。部分特性包括儲存空間 DRS 及網站回復管理程式。

## 可以在 VMware 部署中啟用 VAAI 和 HW 加速嗎？
{: #isVAAIsupported}
{: faq}

否。目前不支援 vStorage for API Array Integration 和硬體加速。

## 從雲端資料中心解除任務的磁碟機會發生什麼情況？
{: faq}

磁碟機解除任務時，IBM 會先破壞它們再進行處理。磁碟機會變成無法使用。已寫入該磁碟機的任何資料都會變成無法存取。
