---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 將 {{site.data.keyword.filestorage_short}} 移轉至加強型 {{site.data.keyword.filestorage_short}}

目前精選資料中心內已提供加強型 {{site.data.keyword.filestorage_full}}。若要查看已升級資料中心及可用特性（例如可調整的 IOPS 速率及可擴充的磁區）的清單，請按一下[這裡](new-ibm-block-and-file-storage-location-and-features.html)。如需提供者管理的加密相關資訊，請參閱 [{{site.data.keyword.filestorage_short}}靜態加密](block-file-storage-encryption-rest.html)。

偏好的移轉路徑是同時連接至兩個磁區，並將資料從某個 LUN 直接傳送至另一個 LUN。細節取決於作業系統，以及是否預期在複製作業期間變更資料。

我們假設您已將未加密 LUN 連接至主機。如果沒有，請遵循最適合您作業系統的指示來完成此作業。

- [在 Linux 上裝載 {{site.data.keyword.filestorage_short}}](accessing-file-storage-linux.html)
- [在 CentOS 中裝載 {{site.data.keyword.filestorage_short}}](mounting-nsf-file-storage.html)
- [在 CoreOS 上裝載 {{site.data.keyword.filestorage_short}}](mounting-storage-coreos.html)

這些資料中心內佈建的所有加強型 {{site.data.keyword.filestorage_short}} 磁區都具有與未加密磁區不同的裝載點。為了確保兩個儲存空間磁區都是使用正確的裝載點，您可以在主控台的**磁區詳細資料**頁面中檢視裝載點資訊。您也可以透過 API 呼叫來存取正確的裝載點：`SoftLayer_Network_Storage::getNetworkMountAddress()`。
{:tip}


## 建立 {{site.data.keyword.filestorage_short}}

使用 API 下訂單時，請指定「儲存空間即服務」套件，以確保使用新的儲存空間來取得已更新的特性。
{:important}

您可以透過 {{site.data.keyword.BluSoftlayer_full}} 型錄及 {{site.data.keyword.slportal}} 訂購加強的 LUN。您的新磁區大小必須等於或大於原始檔案共用，以方便進行移轉。

- [訂購具有預先定義 IOPS 層級（耐久性）的 {{site.data.keyword.filestorage_short}}](provisioning-file-storage.html#ordering-file-storage-with-pre-defined-iops-tiers-endurance-)
- [訂購具有自訂 IOPS（效能）的 {{site.data.keyword.filestorage_short}}](provisioning-file-storage.html#ordering-file-storage-with-custom-iops-performance-)

在幾分鐘之後，您的新儲存空間就可以裝載。您可以在「資源清單」和 {{site.data.keyword.blockstorageshort}} 清單中檢視它。


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
  - 如果您有備份、靜態內容，以及在複製期間預期不會變更的事物，別擔心。
  - 如果您正在 {{site.data.keyword.filestorage_short}} 上執行資料庫或虛擬機器，請確定在複製期間未變更資料，以避免資料毀損。
  - 如果您有任何頻寬疑慮，請在離峰時間執行移轉。
  - 如果您需要關於這些考量的協助，請開立支援問題單。

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
