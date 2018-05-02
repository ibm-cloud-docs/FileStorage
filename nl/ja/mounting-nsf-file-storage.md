---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# 在 CentOS 中裝載 NFS/{{site.data.keyword.filestorage_short}}

在 CentOS 7 中裝載「耐久性」/「效能」{{site.data.keyword.filestorage_full}} 的處理程序，與[在 RHEL 6 上裝載 {{site.data.keyword.filestorage_short}}](https://console.stage1.bluemix.net/docs/infrastructure/FileStorage/accessing-file-storage-linux.html) 的處理程序極為相同，但因為裝載是 NFS，所以我們可以在裝載檔中使用 *Options=* 這一行來指定一些其他選項。在下列範例中，我們會設定在 /data/www 裝載 NFS。附註：您可以從 {{site.data.keyword.filestorage_short}} 清單頁面或透過 API 呼叫 `SoftLayer_Network_Storage::getNetworkMountAddress()`，來取得 {{site.data.keyword.filestorage_short}} 實例的 NFS 裝載點。

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
