---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-10"

keywords: File Storage, mounting file storage, Linux, CentOS, NFS

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}


# 在 CentOS 中安装 {{site.data.keyword.filestorage_short}}
{: #mountingCentOS}

要在 CentOS 7 中安装 {{site.data.keyword.filestorage_full}}，必须先通过 [{{site.data.keyword.cloud}} 控制台](https://{DomainName}/classic){: external}或 SLCLI 来授权主机。然后再安装 NFS 实用程序，如[在 Linux 上安装 {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux) 中所述。

对于 CentOS，可以通过使用安装文件中的 `Options=` 行来指定其他一些选项。在以下示例中，NFS 设置为安装在 `/data/www` 上。

{{site.data.keyword.filestorage_short}} 实例的 NFS 安装点可以从 {{site.data.keyword.filestorage_short}} 列表页面中或通过以下 API 调用来获取：`SoftLayer_Network_Storage::getNetworkMountAddress()`。
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

现在，请启用安装，并检查它是否正确安装。

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs4 (rw,relatime,vers=4.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
{:codeblock}
