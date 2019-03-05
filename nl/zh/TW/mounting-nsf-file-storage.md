---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}


# 在 CentOS 中裝載 {{site.data.keyword.filestorage_short}}
{: #mountingCentOS}

在 CentOS 7 中裝載 {{site.data.keyword.filestorage_full}} 的處理程序，與[在 RHEL 6 上裝載 {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux) 的處理程序類似。不過，因為裝載是 NFS，所以您可以在裝載檔中使用 `Options=` 這一行來指定一些其他選項。在下列範例中，NFS 設為裝載於 `/data/www`。

您可以從 {{site.data.keyword.filestorage_short}} 清單頁面或透過 API 呼叫 `SoftLayer_Network_Storage::getNetworkMountAddress()`，來取得 {{site.data.keyword.filestorage_short}} 實例的 NFS 裝載點。
{:tip}

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
{:codeblock}

接下來，啟用裝載，並確認它已適當地裝載。

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs4 (rw,relatime,vers=4.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
{:codeblock}
