---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-08"

keywords: File Storage, file storage, NFS, mounting volume in Container Linux, CoreOS

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}


# 在 Container Linux 上裝載 {{site.data.keyword.filestorage_short}}
{: #mountingCoreOS}

Container Linux by CoreOS 是以 Linux Kernel 為基礎的開放程式碼輕量型作業系統。其設計旨在為叢集部署提供基礎架構。作為作業系統，Container Linux 提供在軟體容器內部署應用程式所需的最基本功能，並搭配內建的機制來進行服務探索和配置共用。如需相關資訊，請參閱 [Mounting storage](https://coreos.com/os/docs/latest/mounting-storage.html)。

所有次要裝載檔都會放在 `/etc/systemd/system` 目錄中，因為系統層次裝載是位在唯讀的目錄中。首先，您必須建立 `MOUNTPOINT.mount` 檔案。`.mount` 檔案的 **Where** 區段必須符合其檔名。如果裝載點未直接在 `/` 下，您必須使用 `path-to-mount.mount` 語法來命名檔案。例如，如果您要將可攜式儲存空間磁碟裝載至 `/mnt/www`，請將檔案命名為 `mnt-www.mount`。

您可以使用 `fdisk` 或 `parted` 來建立分割區。請確定您建立的檔案系統符合 `.mount` 檔案中所列出的檔案系統，否則無法啟動服務。因為裝載是 NFS，所以您可以在裝載檔中使用 `Options=` 這一行來指定其他選項。

在下列範例中，NFS 設為裝載於 `/data/www`。您可以從 {{site.data.keyword.filestorage_short}} 清單頁面或透過 API 呼叫 `SoftLayer_Network_Storage::getNetworkMountAddress()`，來取得 {{site.data.keyword.filestorage_short}} 實例的 NFS 裝載點。
{:tip}

```
$ cat data-www.mount
[Unit]
Description = Mount for Container Storage

[Mount]
What=<nfs_mount_point>
Where=/data/www
Type=nfs
Options=vers=3,sec=sys,noauto

[Install]
WantedBy = multi-user.target
```
{:codeblock}

接下來，啟用裝載，並確認它已適當地裝載。

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs3 (rw,relatime,vers=3.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
{:codeblock}

如需相關資訊，請參閱 [`systemd mount` 文件](https://www.freedesktop.org/software/systemd/man/systemd.mount.html)。
