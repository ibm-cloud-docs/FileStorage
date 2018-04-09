---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-02"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.filestorage_short}} - 常見問題集

## 如何測量 IOPS？

IOPS 的測量基礎是具有隨機 50% 讀取及 50% 寫入之 16 KB 區塊的負載設定檔。與此設定檔不同的工作負載可能會經歷效能降低。

## 如果我在測量效能時使用較小的區塊大小，會發生什麼情況？

使用較小的區塊大小時，仍然可以取得最大 IOPS，不過，傳輸量會降低。例如，具有 6000 IOPS 的磁區會有下列各種區塊大小的傳輸量：

- 16 KB * 6000 IOPS == ~93.75 MB/秒
- 8 KB * 6000 IOPS == ~46.88 MB/秒
- 4 KB * 6000 IOPS == ~23.44 MB/秒


## 磁區是否需要預先暖機，才能達到預期的傳輸量？

不需要預先暖機。佈建磁區時，會立即觀察到指定的傳輸量。

## 如何分辨哪些 {{site.data.keyword.filestorage_short}} LUN/磁區已加密？

在客戶入口網站中檢視 {{site.data.keyword.filestorage_short}} 清單時，您會在已加密的 LUN/磁區名稱右側看到一個鎖定圖示。

## 如果我在已升級來進行加密的資料中心內，已佈建未加密的 {{site.data.keyword.filestorage_short}}，則是否可以加密 {{site.data.keyword.filestorage_short}}？

無法加密在資料中心升級之前佈建的 {{site.data.keyword.filestorage_short}}。會自動加密在已升級資料中心內佈建的新 {{site.data.keyword.filestorage_short}}；沒有任何加密設定可供選擇，它是自動的。建立新的「檔案」磁區，然後將資料複製到新的加密磁區或具有主機型移轉的磁區，即可將已升級資料中心內未加密儲存空間上的資料加密。如需如何執行移轉的指示，請參閱[本文](/docs/infrastructure/FileStorage/migrate-file-storage-encrypted-file-storage.html)。

## 如何知道是否在已升級資料中心內佈建 {{site.data.keyword.filestorage_short}}？

佈建 {{site.data.keyword.filestorage_short}} 時，將在訂單表格中以星號 (`*`) 表示所有已升級的資料中心，並指出您即將會佈建具有加密的儲存空間。佈建儲存空間之後，您會在儲存空間清單中看到一個圖示，顯示該磁區或磁區已加密。所有加密磁區及磁區都只會佈建在已升級資料中心內。您可以在[這裡](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html)找到完整的已升級資料中心及可用特性清單。

## 為什麼可以在某些資料中心內佈建具有「耐久性 10 IOPS」層級的 {{site.data.keyword.filestorage_short}}，但不能在其他資料中心內進行？

「{{site.data.keyword.filestorage_short}} 耐久性類型 10 IOPS/GB」層級僅適用於精選資料中心，並且很快會新增資料中心。您可以在[這裡](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html)找到完整的已升級資料中心及可用特性清單。

## 如何找到 {{site.data.keyword.filestorage_short}} 的正確裝載點？

這些資料中心內佈建的所有加密 {{site.data.keyword.filestorage_short}} 磁區都具有與未加密磁區不同的裝載點。為了確保加密及未加密 {{site.data.keyword.filestorage_short}} 磁區都使用正確的裝載點，您可以在使用者介面的**磁區詳細資料**頁面中檢視裝載點資訊，並且透過 API 呼叫來存取正確的裝載點：`SoftLayer_Network_Storage::getNetworkMountAddress()`。

## 每個檔案磁區大小容許多少個檔案共用？每個磁區大小容許的檔案共用上限為何？
以下是根據磁區大小容許的 Inode 或檔案共用上限：

<table>
        <tbody>
          <tr>
            <th>磁區大小</th>
            <th>Inode/檔案</th>
          </tr>
          <tr>
            <td>20 GB </td>
            <td>622,484</td>
          </tr>
          <tr>
            <td>40 GB </td>
            <td>1,245,084</td>
          </tr>          
          <tr>
            <td>80 GB</td>
            <td>2,490,263</td>
          </tr>          
          <tr>
            <td>100GB</td>
            <td>3,112,863</td>
          </tr>          
          <tr>
            <td>250 GB</td>
            <td>7,782,300</td>
          </tr>          
          <tr>
            <td>500 GB</td>
            <td>15,564,695</td>
          </tr>
          <tr>
            <td>1 TB+</td>
            <td>31,876,593</td>
          </tr>
        </tbody>
</table>

## 可以佈建多少個磁區？

依預設，您可以佈建總計 250 個區塊及檔案儲存空間磁區。請與業務代表聯絡，以增加磁區。

## 有多少實例可以共用已佈建的 {{site.data.keyword.filestorage_short}} 磁區？

每個檔案磁區的預設授權數目限制是 64。請與業務代表聯絡，以增加限制。

## 佈建「效能」或「耐久性」{{site.data.keyword.filestorage_short}} 時，是依實例還是依磁區施行已配置的 IOPS？

IOPS 是在磁區層次上施行。換句話說，連接至具有 6000 IOPS 之磁區的兩台主機會共用該 6000 IOPS。

## 如果使用速度更快的乙太網路連線，是否可以達到更高的傳輸量？

傳輸量限制是以每個磁區/LUN 的層次而設定，因此，使用速度更快的乙太網路連線並不會增加該項已設定的限制。不過，乙太網路連線較慢時，您的頻寬可能是潛在瓶頸。

## 防火牆/安全群組是否會影響效能？

我們建議的最佳作法是在略過防火牆的 VLAN 上執行儲存空間資料流量。透過軟體防火牆執行儲存空間資料流量，將會增加延遲，而且對儲存空間效能有不利的影響。

## 刪除「{{site.data.keyword.filestorage_short}} 磁區」時，資料會發生什麼情況？

刪除儲存空間時，會移除對於該磁區上之資料的所有指標，因此資料會變成完全無法存取。如果將實體儲存空間重新佈建給另一個帳戶，則會指派一組新的指標。新帳戶無法存取已在實體儲存空間上的任何資料，這組新指標全都會顯示 0。將新資料寫入至磁區/LUN 時，會改寫任何仍存在且無法存取的資料。 

## {{site.data.keyword.filestorage_short}} 的預期效能延遲為何？   

儲存空間內的目標延遲為 < 1 毫秒。我們的儲存空間會連接至共用網路上的運算實例，因此，確切的效能延遲將取決於給定時間範圍內的網路資料流量。

