---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}

# 在 CentOS 中安装 NFS/{{site.data.keyword.filestorage_short}}

在 CentOS 7 中安装 {{site.data.keyword.filestorage_full}} 的过程，与[在 RHEL 6 上安装 {{site.data.keyword.filestorage_short}}](accessing-file-storage-linux.html) 的过程几乎相同。但由于安装的是 NFS，因此可以在安装文件中使用 *Options=* 行来指定其他一些选项。在以下示例中，我们将 NFS 设置为安装在 `/data/www` 上。 

**注**：可以在 {{site.data.keyword.filestorage_short}} 列表页面中或通过以下 API 调用来获取 {{site.data.keyword.filestorage_short}} 实例的 NFS 安装点：`SoftLayer_Network_Storage::getNetworkMountAddress()`。

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

现在，可以启用安装，并检查是否已正确安装。

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs4 (rw,relatime,vers=4.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
{:codeblock
