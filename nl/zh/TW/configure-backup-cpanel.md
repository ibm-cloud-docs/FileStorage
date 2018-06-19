---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-24"

---
{:new_window: target="_blank"}
{:pre: .pre}
 
# 使用 cPanel 配置 {{site.data.keyword.filestorage_short}} 以進行備份

在本文章中，我們的目標是提供指示來透過 cPanel 將您的備份配置為儲存在 {{site.data.keyword.filestorage_full}} 中。我們假設可以使用 root 或 sudo SSH 及完整 WebHost Manager (WHM) 存取權。此範例以 **CentOS 7** 主機為基礎。

**附註**：您可以在[這裡](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){:new_window}找到關於配置備份目錄的 cPanel 文件。

1. 透過 SSH 連接至主機。

2. 確定裝載點目標已存在。<br />
   **附註**：依預設，cPanel 系統會在本端將備份檔儲存至 `/backup` 目錄。在本文件中，我們假設 `/backup` 資料夾存在且包含備份，因此我們將使用 `/backup2` 作為新的裝載點。
   
3. 配置 {{site.data.keyword.filestorage_short}}，如[在 Red Hat Enterprise Linux 上存取 {{site.data.keyword.filestorage_short}}](accessing-file-storage-linux.html) 及[在 CentOS 中裝載 NFS/{{site.data.keyword.filestorage_short}}](mounting-nsf-file-storage.html) 中所述。將磁區裝載至 `/backup2` 並在檔案系統表格 `/etc/fstab` 中進行配置，以啟用開機時裝載。<br />
   **附註**：依預設，NFS 會將使用 root 使用者許可權建立的任何檔案降級成 nobody 使用者。若要容許 root 用戶端保留 NFS 共用上的 root 使用者許可權，則需要將 `no_root_squash` 新增至 `/etc/exports`。<br />
   **附註**：關於[在 CoreOS 上裝載 NFS/{{site.data.keyword.filestorage_short}}](mounting-storage-coreos.html) 也有可用的指示。<br />

4. **選用**：將現有備份複製到新的儲存空間。請使用 `rsync`，例如：
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}
    
    **附註**：此指令會傳輸壓縮的資料，同時儘可能保留最多內容（但硬式鏈結除外）。此外，還會提供要傳送哪些檔案的相關資訊，並在尾端附上簡短摘要。
    
5. 登入 WebHost Manager，然後透過**首頁** > **備份** > **備份配置**導覽至備份配置。

6. 編輯配置，以將備份儲存在新的裝載點中。 
    - 輸入新位置的絕對路徑來取代 `/backup/` 目錄，以變更預設備份目錄。 
    - 選取**啟用以裝載備份磁碟機**。此設定會導致配置處理程序檢查 `/etc/fstab` 檔案來尋找備份裝載 (`/backup2`)。<br /> **附註**：如果存在裝載的名稱與暫置目錄相同，則備份配置處理程序會裝載磁碟機，並將資訊備份至該處。在備份處理程序完成之後，它會卸載磁碟機。 

7. 套用變更，方法是按一下**備份配置**介面底端的**儲存配置**。

8. **選用**：根據您的特定使用案例和商業需要，從伺服器中移除舊的儲存空間並從帳戶中取消。
