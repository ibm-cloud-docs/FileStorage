---

copyright:
  years: 2014, 2018
lastupdated: "2018-12-11"

---
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 在 Container Linux 上安装 {{site.data.keyword.filestorage_short}}

CoreOS 的 Container Linux 是基于 Linux 内核的开放式源代码轻量级操作系统。它的设计目的是为了给集群部署提供基础架构。作为操作系统，Container Linux 提供了在软件容器中部署应用程序所需的最少功能，以及用于服务发现和配置共享的内置机制。有关更多信息，请参阅 [Mounting storage ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://coreos.com/os/docs/latest/mounting-storage.html)

## 安装可移植存储器

所有辅助安装文件都会放入 `/etc/systemd/system` 目录中，因为系统级别安装位于只读目录中。首先，必须创建 `MOUNTPOINT.mount` 文件。`.mount` 文件中的 **Where** 部分必须与文件名相匹配。如果安装点不是直接位于 `/` 下，那么您必须使用语法 `path-to-mount.mount` 对该文件命名。例如，如果要将可移植存储驱动器安装到 `/mnt/www`，请将文件命名为 `mnt-www.mount`。

您可以使用 `fdisk` 或 `parted` 创建分区。请确保您创建的文件系统与 `.mount` 文件中列出的文件系统相匹配，否则服务将无法启动。


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
{:codeblock}


此操作系统使用 `systemd`，因此为了使安装点在重新启动后继续有效，必须启用 `*.mount` 文件。如果使用 `--now` 标志，那么将立即安装分区，并将其设置为在引导时启动。

```
systemctl enable --now mnt-www.mount
```
{:pre}

有关更多信息，请参阅 [`systemd mount` 文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.freedesktop.org/software/systemd/man/systemd.mount.html)

## 安装 {{site.data.keyword.filestorage_short}}

安装 {{site.data.keyword.filestorage_short}} 的过程相同。因为安装的是 NFS，所以可以在安装文件中使用 `Options=` 行来指定更多选项。

在示例中，NFS 设置为安装在 `/data/www` 上。可以在 {{site.data.keyword.filestorage_short}} 列表页面中或通过以下 API 调用来获取 {{site.data.keyword.filestorage_short}} 实例的 NFS 安装点：`SoftLayer_Network_Storage::getNetworkMountAddress()`。
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
