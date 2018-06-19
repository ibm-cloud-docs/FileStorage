---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
 
# 將 {{site.data.keyword.filestorage_short}} 移轉至加強型 {{site.data.keyword.filestorage_short}}

目前精選資料中心內已提供加強型 {{site.data.keyword.filestorage_full}}。若要查看已升級的資料中心及可用特性的清單（例如可調整的 IOPS 率及可擴充的磁區），請按一下[這裡](new-ibm-block-and-file-storage-location-and-features.html)。如需提供者管理的加密儲存空間的相關資訊，請閱讀 [{{site.data.keyword.filestorage_short}} 靜態加密](block-file-storage-encryption-rest.html)文章。

偏好的移轉路徑是同時連接至兩個 LUN，並將資料從某個 LUN 直接傳送至另一個 LUN。細節取決於作業系統，以及是否預期在複製作業期間變更資料。 

我們假設您已將未加密 LUN 連接至主機。如果沒有，請遵循最適合您的作業系統的指示來完成此作業：

- [在 Linux 上裝載 {{site.data.keyword.filestorage_short}}](accessing-file-storage-linux.html)
- [在 CentOS 中裝載 NFS/{{site.data.keyword.filestorage_short}}](mounting-nsf-file-storage.html)
- [在 CoreOS 上裝載 {{site.data.keyword.filestorage_short}}](mounting-storage-coreos.html)

**附註：**所有加強型 {{site.data.keyword.filestorage_short}} 磁區都具有與未加密磁區不同的裝載點。為確保您對已加密及未加密 {{site.data.keyword.filestorage_short}} 磁區都使用正確的裝載點，您可以在使用者介面的**磁區詳細資料**頁面中檢視裝載點資訊。您也可以透過 API 呼叫來存取正確的裝載點：`SoftLayer_Network_Storage::getNetworkMountAddress()`。


## 建立新的 {{site.data.keyword.filestorage_short}}

**重要事項**：使用 API 來下訂單時，請指定「儲存空間作為服務」套件來確定您透過新儲存空間來取得已更新的特性。

下列指示適用於透過使用者介面訂購加強型磁區/檔案共用。您的新磁區的大小應該等於或大於原始磁區，以協助進行移轉。

### 訂購新的耐久性儲存空間磁區

1. 從 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 中，按一下**儲存空間** > **{{site.data.keyword.filestorage_short}}**，或者，從 {{site.data.keyword.BluSoftlayer_full}} 型錄中，按一下**基礎架構** > **儲存空間** > **{{site.data.keyword.filestorage_short}}**。
2. 按一下右上角的**訂購{{site.data.keyword.filestorage_short}}**。 
3. 從**選取儲存空間類型**清單中，選取**耐久性**。
4. 按一下**位置**，然後選取資料中心。
   - 確定新的「儲存空間」將新增至與原始相同的位置。
5. 選取計費選項。您可以選擇每月計費或每小時計費。
6. 按一下**耐久性**，然後選取 IOPS 層級。
6. 從清單中選取**可用的儲存空間大小**。您的新磁區的大小應該等於或大於原始磁區。
7. 從下拉清單中，選擇 **Snapshot 空間大小**（除了可用的空間之外）。
8. 按一下**繼續**。您會看到每月及按比例分配的費用，並且會有最後一次機會可檢閱訂單詳細資料。如果您要變更訂單，請按**上一步**。
9. 按一下**我已閱讀主要服務合約**勾選框，然後按一下**下訂單**
 
### 訂購加密效能儲存空間磁區

1. 從 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 中，按一下**儲存空間**、**{{site.data.keyword.filestorage_short}}**，或者，從 {{site.data.keyword.BluSoftlayer_full}} 型錄中，按一下**基礎架構** >** 儲存空間** > **{{site.data.keyword.filestorage_short}}**。
2. 按一下右上角的**訂購{{site.data.keyword.filestorage_short}}**。 
3. 從「選取儲存空間類型」清單中，選取**效能**。
4. 按一下**位置**，然後選取資料中心。
    -  確定新的「儲存空間」將新增至與原始相同的位置。
5. 選取計費選項。您可以選擇每小時計費或每月計費。
6. 選取適當**儲存空間大小**旁的圓鈕。
6. 在**指定 IOPS** 欄位中，輸入 IOPS。
7. 按一下**繼續**。您會看到每月及按比例分配的費用，並且會有最後一次機會可檢閱訂單詳細資料。如果您要變更訂單，請按**上一步**。
8. 按一下**我已閱讀主要服務合約**勾選框，然後按一下**下訂單**。

在一分鐘以內即會佈建儲存空間，並且它會顯示在 {{site.data.keyword.slportal}} 的 {{site.data.keyword.filestorage_short}} 頁面上。

 
## 將新 {{site.data.keyword.filestorage_short}} 連接至主機

「授權」主機是已獲得磁區存取權的主機。如果沒有主機授權，就無法從系統中存取或使用儲存空間。

1. 按一下新磁區的名稱。
2. 捲動至頁面的**授權主機**區段。
3. 按一下頁面右側的**授權主機**鏈結。選取可存取磁區的主機。

授權之後，請將磁區連接至主機。

 
## Snapshot 及抄寫

您是否已為原始磁區建立 Snapshot 及抄寫？如果是，則需要對新的加密磁區使用與原始磁區相同的設定，設定抄寫、Snapshot 空間，以及建立 Snapshot 排程。 

**附註**：如果未升級您的目標資料中心來進行加密，則除非升級該資料中心，否則您無法建立新磁區的抄寫。

 
## 移轉資料

您的主機應該同時連接至原始及加密 {{site.data.keyword.filestorage_short}} 磁區。如果沒有，請：

- 確定您正確遵循此文件中的步驟並參閱文件。
- 開立支援問題單，以取得將兩個磁區連接至主機的進一步協助。

### 資料考量

此時，請考量您在原始 {{site.data.keyword.filestorage_short}} 磁區上具有的資料類型，以及如何最適當地將資料複製到加密磁區。如果您有備份、靜態內容，以及在複製期間預期不會變更的事物，則沒有任何主要考量。

如果您正在 {{site.data.keyword.filestorage_short}} 上執行資料庫或虛擬機器，請確定在複製期間未變更原始磁區上的資料，以期不會發生毀損。如果您有任何頻寬考量，則應該在離峰時間執行移轉。如果您需要關於這些考量的協助，請開立支援問題單。

### Microsoft Windows

若要將資料從原始 {{site.data.keyword.filestorage_short}} 磁區複製到加密磁區，請使用「Windows 檔案總管」格式化新的儲存空間並將檔案複製到其中。

### Linux

您可以考慮使用 `rsync` 來複製資料。以下是範例指令：

```
[root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage
```

建議您使用此範例指令並搭配 `--dry-run` 旗標一次，以確保正確地排列路徑。如果此程序被岔斷，建議您刪除最後一個正在複製的目的地檔案，以確定從頭將它複製到新位置。

此指令在沒有 `--dry-run` 旗標的情況下完成之後，資料應該已複製到加密 {{site.data.keyword.filestorage_short}} 磁區。您應該向上捲動並重新執行指令，確定未遺漏任何項目。您也可以手動檢閱這兩個位置，以尋找任何可能遺漏的項目。

移轉完成後，您就可以將正式作業移至加密磁區，然後分離並刪除配置中的原始磁區。 

**附註**：刪除作業也會移除目標網站上與原始磁區相關聯的任何 Snapshot 或抄本。
