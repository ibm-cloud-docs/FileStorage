---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 在 Linux 上裝載 {{site.data.keyword.filestorage_short}}

首先，請確定要存取 {{site.data.keyword.filestorage_full}} 磁區的主機已透過 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){:new_window} 獲得授權。

1. 從 {{site.data.keyword.filestorage_short}} 清單頁面，按一下與新共用相關聯的**動作**鏈結，然後按一下**授權主機**。
2. 從清單中選取一台或多台主機，然後按一下**提交**。此動作會授權主機存取共用。

## 裝載 {{site.data.keyword.filestorage_short}} 共用

請使用下列指示，將 Linux 型「{{site.data.keyword.BluSoftlayer_full}} 運算」實例連接至「網路檔案系統 (NFS)」共用。此範例是以 Red Hat Enterprise Linux 6 為基礎。針對其他 Linux 發行套件，可以根據作業系統 (OS) 供應商文件來調整這些步驟。

您可以從 {{site.data.keyword.filestorage_short}} 清單頁面或透過 API 呼叫 `SoftLayer_Network_Storage::getNetworkMountAddress()`，來取得檔案儲存空間實例的裝載點。
{:tip}

1. 安裝必要的工具。
   ```
   # yum -y install nfs-utils nfs-utils-lib
   ```
   {:pre}

2. 裝載遠端共用。
   ```
   # mount -t "nfs version" -o "options" <mount_point> /mnt
   ```

   範例
   ```
   # mount -t nfs4 -o hard,intr
   nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt
   ```

3. 驗證已成功裝載。
   ```
   # df -h
   Filesystem Size Used Avail Use% Mounted on
   /dev/xvda2 25G 1.4G 22G 6% /
   tmpfs 1.9G 0 1.9G 0% /dev/shm
   /dev/xvda1 97M 51M 42M 55%
   ```

4. 移至裝載點，並讀寫檔案。
   ```
   # touch /mnt/test
   # ls -la /mnt
   total 12
   drwxr-xr-x 2 nobody nobody 4096 Sep 8 15:52 .
   dr-xr-xr-x. 22 root root 4096 Sep 8 14:30 ..
   -rw-r--r-- 1 nobody nobody 0 Sep 8 15:52 test
   ```

   root 所建立的檔案具有 `nobody:nobody` 所有權。若要正確顯示所有權，必須使用正確的網域設定來更新 `idmapd.conf`。請參閱[如何實作 NFS 的 no_root_squash](#implementing-no_root_squash-for-nfs-optional-) 小節。
   {:tip}

5. 啟動時裝載遠端共用。若要完成設定，請編輯檔案系統表格 (`/etc/fstab`)，將遠端共用新增至啟動時將自動裝載的項目清單：

   ```
   (hostname):/(username) /mnt "nfs version" "options" 0 0
   ```

   範例

   ```
   nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt nfs4 defaults,hard,intr 0 0
   ```

6. 驗證配置檔未發生錯誤。

   ```
   # mount -fav
   ```
   {:pre}

   如果指令完成且沒有任何錯誤，則設定已完成。

   如果您使用 NFS 4.1，請將 `sec=sys` 新增至 mount 指令，以防止發生檔案所有權問題。
   {:tip}


## 實作 NFS 的 `no_root_squash`（選用）

配置 `no_root_squash` 可讓 root 用戶端保留對 NFS 共用的 root 許可權。
- 若為 NFSv3，用戶端不需要執行任何動作；`no_root_squash` 即會運作。
- 若為 NFSv4，您需要將 nfsv4 網域設為：`slnfsv4.com`，並啟動 `rpcidmapd` 或 OS 所使用的類似服務。

範例

1. 從主機中，在 `/etc/idmapd.conf` 中設定網域設定。

   ```
   #vi /etc/idmapd.conf
   [General]
   #Verbosity = 0
   #The following should be set to the local NFSv4 domain name
   #The default is the host's DNS domain name.
   Domain = slnfsv4.com
   [Mapping]
   Nobody-User = nobody
   Nobody-Group = nobody
   ```

2. 執行 `nfsidmap -c`。
3. 啟動 `rpcidmapd`。
   ```
   # /etc/init.d/rpcidmapd start
   Starting RPC idmapd: [ OK ]
   ```
