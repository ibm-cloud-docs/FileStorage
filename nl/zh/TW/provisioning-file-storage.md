---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-07"

---
{:new_window: target="_blank"}

# 訂購 {{site.data.keyword.filestorage_short}}

您可以佈建 {{site.data.keyword.filestorage_short}}，並微調以符合您的容量及 IOPS 需求。使用兩個指定效能的選項，來善加利用儲存空間。

- 您可以從「耐久性」IOPS 層級進行選擇，而這些層級具備預先定義的效能層次特性，可放入未妥善定義效能需求的工作負載。 
- 您可以指定具有「效能」的 IOPS 總數，來細部調整儲存空間以符合極特定的效能需求。

## 使用預先定義的 IOPS 層級（耐久性）訂購 {{site.data.keyword.filestorage_short}}

1. 從 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 中，按一下**儲存空間** > **{{site.data.keyword.filestorage_short}}**，或者，從 {{site.data.keyword.BluSoftlayer_full}} 型錄中，按一下**基礎架構** > **儲存空間** > **{{site.data.keyword.filestorage_short}}**。
2. 在右上方，按一下**訂購 {{site.data.keyword.filestorage_short}}**。
3. 選取您的部署**位置**（資料中心）。
   - 確定將新的「儲存空間」新增至與您的運算主機相同的位置。
4. 計費。如果您已選取具有改良功能的資料中心（以星號標示），則可以選擇「按月計費」或「按小時計費」。 
     1. 使用**按小時**計費，會在刪除 LUN 或計費週期結束時，計算檔案磁區存在於帳戶上的小時數。看何者為先。如果儲存空間使用期間為期只有幾天或不到一整個月，則按小時計費是一個不錯的選擇。按小時計費只適用於這些[精選資料中心](new-ibm-block-and-file-storage-location-and-features.html)內所佈建的儲存空間。 
     2. 使用**按月**計費，價格是從建立日期到計費週期結束按比例計算，並立即計費。如果在計費週期結束之前刪除檔案磁區，則不會退款。如果儲存空間用於正式作業工作負載，而正式作業工作負載使用需要長期（一個月或更久）儲存及存取的資料，則按月計費是一個不錯的選擇。
        >**附註**：依預設，如果儲存空間佈建在**未**使用改良功能更新的資料中心內，則會使用按月計費類型。
5. 在**新的儲存空間大小**欄位中，輸入儲存空間大小。
6. 在**儲存空間 IOPS 選項**區段中，選取**耐久性（分層 IOPS）**。
7. 選取您應用程式所需的 IOPS 層級。
    - **每 GB 0.25 IOPS** 是為了低 I/O 強度的工作負載而設計。這些工作負載的特點通常一次會有大比例的非作用中資料。應用程式範例包括儲存信箱或部門層次檔案共用。
    - **每 GB 2 IOPS** 是為了大多數通用用途而設計。應用程式範例包括管理小型資料庫，其支持 Web 應用程式或是 Hypervisor 的虛擬機器磁碟映像檔。
    - **每 GB 4 IOPS** 是為了較高強度工作負載而設計。這些工作負載的特點通常是一次會有大比例的作用中資料。應用程式範例包括交易式資料庫及其他效能相關的資料庫。
    - **每 GB 10 IOPS** 是為了最嚴苛的工作負載（例如 NoSQL 資料庫所建立的工作負載）以及進行分析用的資料處理而設計。此層級適用於[精選資料中心](new-ibm-block-and-file-storage-location-and-features.html)內所佈建且最多為 4 TB 的儲存空間。
8. 按一下**指定 Snapshot 空間大小**，然後從清單中選取 Snapshot 大小。除了可用空間之外，還有此空間。如需 Snapshot 空間考量及建議，請閱讀[訂購 Snapshot](ordering-snapshots.html)。
9. 選取**條款**的勾選框，然後按一下**下訂單**。
10. 在幾分鐘之後，就可以使用您的新儲存空間配置。

>**附註**：依預設，您可以佈建總計 250 個 {{site.data.keyword.blockstorageshort}} 磁區。若要增加磁區數目，請與業務代表聯絡。請在[這裡](managing-storage-limits.html)閱讀提高限制的相關資訊。<br/><br/>如需同時授權的限制，請參閱[常見問題](File-Storage-FAQ.html)。

## 使用自訂 IOPS（效能）訂購 {{site.data.keyword.filestorage_short}}

1. 從 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 中，按一下**儲存空間**、**{{site.data.keyword.filestorage_short}}**，或者，從 {{site.data.keyword.BluSoftlayer_full}} 型錄中，按一下**基礎架構** >** 儲存空間** > **{{site.data.keyword.filestorage_short}}**。
2. 在右上方，按一下**訂購 {{site.data.keyword.filestorage_short}}**。
3. 按一下**位置**，然後選取資料中心。
   - 確定將新的「儲存空間」新增至與您的運算主機相同的位置。
4. 計費。如果您已選取具有改良功能的資料中心（以星號標示），則可以選擇「按月計費」或「按小時計費」。 
     1. 使用**按小時**計費，會在刪除 LUN 或計費週期結束時，計算檔案磁區存在於帳戶上的小時數。看何者為先。如果儲存空間使用期間為期只有幾天或不到一整個月，則按小時計費是一個不錯的選擇。按小時計費只適用於這些[精選資料中心](new-ibm-block-and-file-storage-location-and-features.html)內所佈建的儲存空間。 
     2. 使用**按月**計費，價格是從建立日期到計費週期結束按比例計算，並立即計費。如果在計費週期結束之前刪除檔案磁區，則不會退款。如果儲存空間用於正式作業工作負載，而正式作業工作負載使用需要長期（一個月或更久）儲存及存取的資料，則按月計費是一個不錯的選擇。
        >**附註**：依預設，如果儲存空間佈建在**未**使用改良功能更新的資料中心內，則會使用按月計費類型。
5. 在**新的儲存空間大小**欄位中，輸入儲存空間大小。
6. 在**儲存空間 IOPS 選項**區段中，選取**效能（已配置的 IOPS）**。
7. 在**效能（已配置的 IOPS）**欄位中，輸入 IOPS。
8. 按一下**指定 Snapshot 空間大小**，然後從清單中選取 Snapshot 大小。除了可用空間之外，還有此空間。如需 Snapshot 空間考量及建議，請閱讀[訂購 Snapshot](ordering-snapshots.html)。
9. 選取**條款**的勾選框，然後按一下**下訂單**。
10. 在幾分鐘之後，就可以使用您的新儲存空間配置。

>**附註**：依預設，您可以佈建總計 250 個 {{site.data.keyword.blockstorageshort}} 磁區。若要增加磁區數目，請與業務代表聯絡。請在[這裡](managing-storage-limits.html)閱讀提高限制的相關資訊。<br/><br/>如需同時授權的限制，請參閱[常見問題](File-Storage-FAQ.html)。


## 連接新的儲存空間

當您的佈建要求完成時，請授權主機存取新的儲存空間，並配置連線。請根據主機的作業系統，遵循適當的鏈結。
- [存取 Linux 上的 {{site.data.keyword.filestorage_short}}](accessing-file-storage-linux.html)
- [在 CentOS 中裝載 NFS/{{site.data.keyword.filestorage_short}}](mounting-nsf-file-storage.html)
- [在 CoreOS 上裝載 {{site.data.keyword.filestorage_short}}](mounting-storage-coreos.html)
- [使用 cPanel 配置 {{site.data.keyword.filestorage_short}} 以進行備份](configure-backup-cpanel.html)
- [使用 Plesk 配置 {{site.data.keyword.filestorage_short}} 以進行備份](configure-backup-plesk.html)
- [在 ESXi 主機上裝載 {{site.data.keyword.filestorage_short}} 磁區](architecture-guide-file-storage-vmware.html)


## 識別發票上的 {{site.data.keyword.filestorage_short}} 磁區

所有檔案共用在發票上都會顯示為明細行項目。「耐久性磁區」會顯示為「耐久性儲存空間服務」，而「效能」磁區會顯示為「效能儲存空間服務」。根據您的儲存空間層次，速率會不同。您可以在「耐久性」或「效能」上展開，即可看到它是 {{site.data.keyword.filestorage_short}}。
