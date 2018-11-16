---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-31"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 使用 cPanel 配置 {{site.data.keyword.filestorage_short}} 以進行備份

您可以使用這些指示，透過 cPanel 配置將您的備份儲存至 {{site.data.keyword.filestorage_full}}。我們假設可以使用 root 或 sudo SSH 及完整 WebHost Manager (WHM) 存取權。此範例以 **CentOS 7** 主機為基礎。

如需供應商的相關資訊，請參閱 [cPanel - Configuring Backup Directory](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){:new_window}。
{:tip}

1. 透過 SSH 連接至主機。
2. 確定裝載點目標已存在。<br />

   依預設，cPanel 系統會在本端將備份檔儲存至 `/backup` 目錄。在本文件中，假設 `/backup` 資料夾已存在且包含備份，而且可以使用 `/backup2` 作為新的裝載點。{:note}

3. 配置 {{site.data.keyword.filestorage_short}}，如[在 Red Hat Enterprise Linux 上存取 {{site.data.keyword.filestorage_short}}](accessing-file-storage-linux.html) 及[在 CentOS 中裝載 NFS/{{site.data.keyword.filestorage_short}}](mounting-nsf-file-storage.html)/[在 CoreOS 上裝載 NFS/{{site.data.keyword.filestorage_short}}](mounting-storage-coreos.html) 中所述。將磁區裝載至 `/backup2`，並在檔案系統表格 (`/etc/fstab`) 中進行配置，以啟用啟動時裝載。<br />

   依預設，NFS 會將使用 root 使用者許可權建立的任何檔案降級成 nobody 使用者。若要容許 root 用戶端保留對 NFS 共用的 root 使用者許可權，則需要將 `no_root_squash` 新增至 `/etc/exports`。{:tip}

4. **選用**。將現有備份複製到新的儲存空間。例如，您可以使用 `rsync`。
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}

    這個指令會壓縮並傳輸您的資料，並儘可能保留越多內容（但固定鏈結除外）。此外，還會提供要傳送哪些檔案的相關資訊，並在尾端附上簡短摘要。
    {:tip}

5. 登入 WebHost Manager，然後透過**首頁** > **備份** > **備份配置**移至備份配置。

6. 編輯配置，以將備份儲存在新的裝載點中。
    - 輸入新位置的絕對路徑來取代 `/backup/` 目錄，以變更預設備份目錄。
    - 選取**啟用以裝載備份磁碟機**。此設定會導致配置處理程序檢查 `/etc/fstab` 檔案來尋找備份裝載 (`/backup2`)。<br />

      如果存在的裝載名稱與暫置目錄相同，則備份配置處理程序會裝載該磁碟機，並將資訊備份至該處。在備份處理程序完成之後，它會卸載磁碟機。{:note}
7. 按一下**儲存配置**，以套用變更。
8. **選用**。根據您的特定使用案例和商業需要，移除伺服器中的舊儲存空間，並從帳戶中予以取消。
