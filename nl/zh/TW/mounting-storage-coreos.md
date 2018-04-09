---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# 在 CoreOS 上裝載 {{site.data.keyword.filestorage_short}}

CoreOS 是功能強大的 Linux 發行套件，建置目的是要使各種基礎架構上的大型可調式部署變得容易管理。根據 Chrome OS 建置，CoreOS 會維護輕量型主機系統，並將 Docker 容器用於所有應用程式。

## 裝載可攜式儲存空間

所有次要裝載檔都會放入 */etc/systemd/system* 目錄中，因為系統層次裝載位在 CoreOS 的唯讀目錄中。您將建立 MOUNTPOINT.mount 檔案。.mount 檔案的 Where 區段必須符合其檔名。如果裝載點未直接在 / 下，您必須使用下列語法來命名檔案：path-to-mount.mount。如您在下列範例中所見，我們要將可攜式儲存空間磁碟裝載至 `/mnt/www`，因此將檔案命名為 `mnt-www.mount`。

您必須使用 fdisk 或 parted 來建立分割區，並確定您建立的檔案系統符合 `.mount` 檔案中所列的檔案系統，否則將無法啟動服務。


```
[Unit]
Description = Mount for Portable Storage

[Mount]
What=/dev/xvdc1
Where=/mnt/www
Type=ext4

[Install]
WantedBy = multi-user.target
```

CoreOS 使用 systemd，因此，若要讓裝載點在重新開機之後仍然存在，您必須啟用 `*.mount` 檔案。如果您使用 `--now` 旗標，則會立即裝載分割區，並設為在開機時啟動。

`$ systemctl enable --now mnt-www.mount`

## 裝載 NFS/{{site.data.keyword.filestorage_short}}

裝載「耐久性」/「效能」{{site.data.keyword.filestorage_short}} 的處理程序極為相同，但因為裝載是 NFS，所以我們可以在裝載檔中使用 Options= 這一行來指定一些其他選項。在下列範例中，我們會設定在 `/data/www` 裝載 NFS。附註：您可以從 {{site.data.keyword.filestorage_short}} 清單頁面或透過 API 呼叫 SoftLayer_Network_Storage::getNetworkMountAddress()，來取得 {{site.data.keyword.filestorage_short}} 實例的 NFS 裝載點。

```
$ cat data-www.mount
[Unit]
Description = Mount for Container Storage

[Mount]
What=<nfs_mount_point>
Where=/data/www
Type=nfs
Options=vers=4,sec=sys,noauto

[Install]
WantedBy = multi-user.target
```

我們現在可以啟用裝載，並確認它已適當地裝載。

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs4 (rw,relatime,vers=4.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
 
## 裝載 NAS/Cifs

在 CoreOS 中，原本並不支援裝載 cifs 共用，但有相當簡單的暫行解決方法，可讓主機系統裝載 NAS 共用。您將使用容器來建置 mount.cfis 模組，然後將它複製到 CoreOS 系統
 
在 CoreOS 系統上，執行下列指令來下載並放入 Fedora 容器中：<br/>
`docker run -t -i -v /tmp:/host_tmp fedora /bin/bash`
 
您位於容器之後，請執行下列指令來建置 cifs 公用程式
```
dnf groupinstall -y "Development Tools" "Development Libraries"
dnf install -y tar
dnf install -y bzip2
curl https://ftp.samba.org/pub/linux-cifs/cifs-utils/cifs-utils-6.4.tar.bz2 | bunzip2 -c - | tar -xvf -
cd cifs-utils-6.4/
./configure && make
cp mount.cifs /host_tmp/
```
 
現在，已將 mount.cifs 檔案複製到主機，因此可以發出 `exit` 指令或按 **ctrl+d** 來結束 Docker 容器。當您回到 CoreOS 系統時，即可使用下列指令來裝載 CIFS 共用：<br/>
`/tmp/mount.cifs //nasXXX.service.softlayer.com/USERNAME -o username=USERNAME,password=PASSWORD /path/to/mount`
 
## 裝載 ISCSI

這目前在 CoreOS 中並不支援，但未來版本會予以支援 - https://github.com/coreos/bugs/issues/634
