---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, Plesk, backups

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 使用 Plesk 配置 {{site.data.keyword.filestorage_short}} 以進行備份
{: #PleskBackup}

您可以使用這些指示，以在 Plesk 中配置 {{site.data.keyword.filestorage_full}} 作為您的備份。我們假設可以使用 root 或 sudo SSH 及完整管理層次 Plesk 存取權。此範例以 CentOS7 主機為基礎。

如需相關資訊，請參閱 [Plesk 的備份及還原文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window}。
{:tip}

1. 透過 SSH 連接至主機。
2. 確定裝載點目標已存在。<br />

   Plesk 有兩個用來儲存備份的選項。一個是內部 Plesk 儲存空間，即您 Plesk 伺服器上的儲存空間。另一個是外部 FTP 儲存空間，即 Web 或本端網路的某部外部伺服器上的儲存空間。通常，在 Plesk 機器上，內部備份儲存在 `/var/lib/psa/dumps` 中，並使用 `/tmp` 作為暫存目錄。在此範例中，暫存目錄會保留在本端，但 `dumps` 目錄會移至 {{site.data.keyword.filestorage_short}} 目標 (`/backup/psa/dumps`)。不需要任何 FTP 使用者認證。
   {:note}
3. 配置 {{site.data.keyword.filestorage_short}}，如[在 Red Hat Enterprise Linux 上存取 {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux) 及[在 CentOS 中裝載 NFS/{{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS) 或[在 CoreOS 上裝載 NFS/{{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS) 中所述。將磁區裝載至 `/backup`，並在檔案系統表格 (`/etc/fstab`) 中進行配置，以啟用啟動時裝載。<br />

   依預設，NFS 會將使用 root 使用者許可權建立的任何檔案降級成 nobody 使用者。若要讓 root 用戶端保留對 NFS 共用的 root 使用者許可權，需要將 `no_root_squash` 新增至 `/etc/exports`。
{:tip}
4. **選用**。將現有備份複製到新的儲存空間。請使用 `rsync`，例如：
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {:pre}

   這個指令會壓縮並傳輸您的資料，並儘可能保留越多內容（但固定鏈結除外）。此外，還會提供要傳送哪些檔案的相關資訊，並在尾端附上簡短摘要。
   {:tip}
5. 編輯 `/etc/psa/psa.conf`，以將 `DUMP_D` 值指向新目標。
    - 它會顯示為：`DUMP_D /backup/psa/dumps`。
6. **選用**。根據您的特定使用案例和商業需要，移除伺服器中的舊儲存空間，並從帳戶中予以取消。
