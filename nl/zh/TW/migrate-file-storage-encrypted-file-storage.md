---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-31"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 將 {{site.data.keyword.filestorage_short}} 移轉至加強型 {{site.data.keyword.filestorage_short}}

目前精選資料中心內已提供加強型 {{site.data.keyword.filestorage_full}}。若要查看已升級的資料中心及可用特性的清單（例如可調整的 IOPS 率及可擴充的磁區），請按一下[這裡](new-ibm-block-and-file-storage-location-and-features.html)。如需提供者管理的加密儲存空間的相關資訊，請參閱[{{site.data.keyword.filestorage_short}} 靜態加密](block-file-storage-encryption-rest.html)。

偏好的移轉路徑是同時連接至兩個磁區，並將資料從某個 LUN 直接傳送至另一個 LUN。細節取決於作業系統，以及是否預期在複製作業期間變更資料。

我們假設您已將未加密 LUN 連接至主機。如果沒有，請遵循最適合您作業系統的指示來完成此作業。

- [在 Linux 上裝載 {{site.data.keyword.filestorage_short}}](accessing-file-storage-linux.html)
- [在 CentOS 中裝載 NFS/{{site.data.keyword.filestorage_short}}](mounting-nsf-file-storage.html)
- [在 CoreOS 上裝載 {{site.data.keyword.filestorage_short}}](mounting-storage-coreos.html)

所有加強型 {{site.data.keyword.filestorage_short}} 磁區都具有與未加密磁區不同的裝載點。為確保您對已加密及未加密 {{site.data.keyword.filestorage_short}} 磁區都使用正確的裝載點，您可以在 {{site.data.keyword.slportal}} 的**磁區詳細資料**頁面中檢視裝載點資訊。您也可以透過 API 呼叫來存取正確的裝載點：`SoftLayer_Network_Storage::getNetworkMountAddress()`。
{:tip}


## 建立新的 {{site.data.keyword.filestorage_short}}

使用 API 下訂單時，請指定「儲存空間即服務」套件，以確保使用新的儲存空間來取得已更新的特性。
{:important}

下列指示適用於透過 {{site.data.keyword.slportal}}/{{site.data.keyword.BluSoftlayer_full}} 型錄訂購加強型磁區/檔案共用。您的新磁區大小必須等於或大於原始磁區，以方便進行移轉。

### 訂購新的耐久性儲存空間磁區

1. 從 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}，按一下**儲存空間** > **{{site.data.keyword.filestorage_short}}**，或者，從 {{site.data.keyword.BluSoftlayer_full}} 型錄，按一下**基礎架構** > **儲存空間** > **{{site.data.keyword.filestorage_short}}**。
2. 按一下**訂購 {{site.data.keyword.filestorage_short}}**。
3. 從**選取儲存空間類型**清單中，選取**耐久性**。
4. 按一下**位置**，然後選取資料中心。
   - 確定將新的「儲存空間」新增至與原始相同的位置。
5. 選取計費選項。您可以選擇按月計費或按小時計費。
6. 按一下**耐久性**，然後選取 IOPS 層級。
6. 從清單中選取**可用的儲存空間大小**。您的新磁區大小必須等於或大於原始磁區。
7. 從清單中選擇 **Snapshot 空間大小**（除了您可用的空間之外）。
8. 按一下**繼續**。您會看到按月及按比例分配的費用，並且會有最後一次機會可檢閱訂單詳細資料。如果您要變更訂單，請按**上一步**。
9. 按一下**我已閱讀主要服務合約**勾選框，然後按一下**下訂單**

### 訂購加密效能儲存空間磁區

1. 從 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}，按一下**儲存空間**、**{{site.data.keyword.filestorage_short}}**，或者，從 {{site.data.keyword.BluSoftlayer_full}} 型錄，按一下**基礎架構** >** 儲存空間** > **{{site.data.keyword.filestorage_short}}**。
2. 按一下**訂購 {{site.data.keyword.filestorage_short}}**。
3. 從**選取儲存空間類型**清單中，選取**效能**。
4. 按一下**位置**，然後選取資料中心。
    -  確定將新的「儲存空間」新增至與原始相同的位置。
5. 選取計費選項。您可以選擇按小時計費或按月計費。
6. 選取適當**儲存空間大小**旁的圓鈕。
6. 在**指定 IOPS** 欄位中，輸入 IOPS。
7. 按一下**繼續**。您會看到按月及按比例分配的費用，並且會有最後一次機會可檢閱訂單詳細資料。如果您要變更訂單，請按**上一步**。
8. 按一下**我已閱讀主要服務合約**勾選框，然後按一下**下訂單**。

即會在一分鐘以內佈建儲存空間，並且它會顯示在 {{site.data.keyword.slportal}} 的 {{site.data.keyword.filestorage_short}} 頁面上。


## 將主機授權給新的 {{site.data.keyword.filestorage_short}}

「授權」主機是已獲得磁區存取權的主機。如果沒有主機授權，則無法從系統中存取或使用儲存空間。

1. 按一下新磁區的名稱。
2. 捲動至**授權主機**區段。
3. 按一下右側的**授權主機**鏈結。選取可存取磁區的主機。

授權主機之後，請將磁區連接至主機。


## 設定 Snapshot 及抄寫

如果已建立原始磁區的 Snapshot 及抄寫，則您需要為新磁區設定它們。配置抄寫、Snapshot 空間，以及建立設定與原始磁區相同的 Snapshot 排程。

如果目標資料中心未加密，則除非該資料中心升級，否則您無法建立新磁區的抄寫。
{:important}


## 移轉資料

1. 同時連接至原始及新的 {{site.data.keyword.filestorage_short}} 磁區。
  - 如果您需要將兩個檔案共用連接至主機的協助，請開立支援問題單。

2. 考量您在原始 {{site.data.keyword.filestorage_short}} 磁區上具有的資料類型，以及如何最適當地將資料複製到新的檔案共用。
  - 如果您有備份、靜態內容，以及在複製期間預期不會變更的事物，則沒有任何重大疑慮。
  - 如果您正在 {{site.data.keyword.filestorage_short}} 上執行資料庫或虛擬機器，請確定在複製期間未變更資料，以避免資料毀損。如果您有任何頻寬疑慮，請在離峰時間執行移轉。如果您需要關於這些考量的協助，請開立支援問題單。

3. 複製您的資料。
   - **Microsoft Windows**
     - 若要將資料從原始 {{site.data.keyword.filestorage_short}} LUN 複製到新的 LUN，請使用「Windows 檔案總管」格式化新的儲存空間並將檔案複製到其中。
   - **Linux**
     - 您可以使用 `rsync` 來複製資料。
       ```
[root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage
```

   最好搭配使用前一個指令與 `--dry-run` 旗標一次，以確保正確地排列路徑。如果此處理程序遭到岔斷，您可以刪除最後一個正在複製的目的地檔案，以確定從頭將它複製到新位置。

   這個指令在沒有 `--dry-run` 旗標的情況下完成時，會將資料複製到新的 {{site.data.keyword.filestorage_short}} 磁區。重新執行指令，以確定未遺漏任何項目。您也可以手動檢閱這兩個位置，以尋找任何可能遺漏的項目。

      移轉完成時，您可以將正式作業移至新的 LUN。然後，您可以分離原始磁區與配置，以及從配置中刪除原始磁區。刪除作業也會移除目標網站上與原始磁區相關聯的任何 Snapshot 或抄本。
