---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-18"

---
{:new_window: target="_blank"}
{:faq: data-hd-content-type='faq'}

# 常見問題

## 如何分辨哪些 {{site.data.keyword.filestorage_short}} 磁區已加密？
{: faq}

請查看客戶入口網站中的 {{site.data.keyword.filestorage_short}} 清單。您會看到已加密磁區的 LUN/磁區名稱右側有一個鎖定圖示。

## 如果我在已升級進行加密的資料中心內購買了未加密的 {{site.data.keyword.filestorage_short}}，則是否可以加密我的 {{site.data.keyword.filestorage_short}}？
{: faq}

在資料中心升級之前所佈建的 {{site.data.keyword.filestorage_short}} 無法加密。在已升級的資料中心內佈建的新 {{site.data.keyword.filestorage_short}} 則會自動加密。由於是自動，因此沒有任何加密設定可供選擇。未加密儲存空間上的資料加密，可以藉由建立新磁區，然後透過主機型移轉將資料複製到新的已加密磁區來進行。如需相關資訊，請參閱[移轉檔案儲存空間](/docs/infrastructure/FileStorage/migrate-file-storage-encrypted-file-storage.html)。

## 如何知道是否在已升級資料中心內佈建 {{site.data.keyword.filestorage_short}}？
{: faq}

在 {{site.data.keyword.filestorage_short}} 訂單表格中，所有已升級的資料中心都會註記星號 (`*`)。在訂購程序期間，會提供您指示，指出您正在佈建加密的儲存空間。佈建儲存空間時，您可以看到儲存空間清單中有一個圖示，顯示該磁區已加密。 

所有加密磁區及檔案共用都只會佈建在已升級的資料中心內。您可以在[這裡](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html)找到完整的已升級資料中心及可用特性清單。

## 為什麼可以在某些資料中心內佈建具有「耐久性 10 IOPS」層級的 {{site.data.keyword.filestorage_short}}，但不能在其他資料中心內進行？
{: faq}

「{{site.data.keyword.filestorage_short}} 耐久性類型 10 IOPS/GB」層級僅適用於精選資料中心，並且很快會新增資料中心。您可以在[這裡](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html)找到完整的已升級資料中心及可用特性清單。

## 如何找到 {{site.data.keyword.filestorage_short}} 的正確裝載點？
{: faq}

加強型資料中心內佈建的所有加密 {{site.data.keyword.filestorage_short}} 磁區都具有與未加密磁區不同的裝載點。為確保您使用正確的裝載點，請檢視使用者介面中**磁區詳細資料**頁面上的裝載點資訊。您也可以透過 API 呼叫來存取正確的裝載點：`SoftLayer_Network_Storage::getNetworkMountAddress()`。

## 可以佈建多少個磁區？
{: faq}

依預設，您可以佈建總計 250 個區塊及檔案儲存空間磁區。若要增加限制，請與業務代表聯絡。如需相關資訊，請參閱[管理儲存空間限制](managing-storage-limits.html)。

## 有多少實例可以共用已佈建的 {{site.data.keyword.filestorage_short}} 磁區？
{: faq}

每個檔案磁區的預設授權數目限制是 64。若要增加此限制，請與業務代表聯絡。

## 可以將多少個 {{site.data.keyword.filestorage_short}} 磁區連接至單一主機？
{: faq}

這取決於主機作業系統可處理的項目，而不是由 {{site.data.keyword.BluSoftlayer_full}} 所限制。如需可裝載的檔案共用數目限制，請參閱 OS 文件。

## 每個檔案磁區大小容許多少個檔案共用？每個磁區大小容許的檔案共用上限為何？
{: faq}

<table>
  <caption>表 1 顯示根據磁區大小而容許的 Inode 數目上限。磁區大小在左直欄中。Inode/檔案共用數目在右側。</caption>
  <thead>
    <tr>
      <th>磁區大小</th>
      <th>Inode/檔案共用</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>20 GB - 39 GB</td>
      <td>622,484</td>
    </tr>
    <tr>
      <td>40 GB - 79 GB</td>
      <td>1,245,084</td>
    </tr>          
    <tr>
      <td>80 GB - 99 GB</td>
      <td>2,490,263</td>
    </tr>          
    <tr>
      <td>100 GB - 249 GB</td>
      <td>3,112,863</td>
    </tr>          
    <tr>
      <td>250 GB - 499 GB</td>
      <td>7,782,300</td>
    </tr>          
    <tr>
      <td>500 GB - 999 GB</td>
      <td>15,564,695</td>
    </tr>
    <tr>
      <td>1 TB</td>
      <td>31,876,593</td>
    </tr>
    <tr>
      <td>2 TB</td>
      <td>63,753,186</td>
    </tr>
    <tr>
      <td>3 TB</td>
      <td>95,629,970</td>
    </tr>
    <tr>
      <td>4 TB - 12 TB</td>
      <td>127,506,359</td>
    </tr>
   </tbody>
</table>

## 測量 IOPS
{: faq}

IOPS 的測量基礎是具有隨機百分之 50 讀取及百分之 50 寫入之 16 KB 區塊的負載設定檔。與此設定檔不同的工作負載可能會經歷效能降低。

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

傳輸量限制是依每個磁區/LUN 的層次而設定，因此，使用速度更快的乙太網路連線並不會提高該項已設定的限制。不過，乙太網路連線較慢時，您的頻寬可能是潛在瓶頸。

## 防火牆/安全群組是否會影響效能？
{: faq}

最好是在 VLAN 上執行儲存空間資料流量，這樣會略過防火牆。透過軟體防火牆執行儲存空間資料流量，會增加延遲，而且會對儲存空間效能造成不利的影響。

## {{site.data.keyword.filestorage_short}} 的預期效能延遲為何？   
{: faq}

儲存空間內的目標延遲為 < 1 毫秒。儲存空間會連接至共用網路上的運算實例，因此，確切的效能延遲取決於該作業期間的網路資料流量。

## 刪除 {{site.data.keyword.filestorage_short}} 磁區時，資料會發生什麼情況？
{: faq}

{{site.data.keyword.filestorage_full}} 會向客戶呈現在任何重複使用之前抹除的實體儲存空間上的檔案共用。具有特殊規範需求的客戶（例如 NIST 800-88 媒體資料安全清除準則）必須先執行資料安全清除程序，再刪除其儲存空間。

## 從雲端資料中心解除任務的磁碟機會發生什麼情況？
{: faq}

磁碟機解除任務時，IBM 會先破壞它們再進行處理。磁碟機會變成無法使用。已寫入該磁碟機的任何資料都會變成無法存取。
