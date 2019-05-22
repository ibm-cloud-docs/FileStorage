---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, Plesk, backups

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 在 Plesk 中将 {{site.data.keyword.filestorage_short}} 配置用于备份
{: #PleskBackup}

使用以下指示信息，可以在 Plesk 中将 {{site.data.keyword.filestorage_full}} 配置用于您的备份。假定以 root 用户或 sudo 用户身份通过 SSH 登录到系统，并且有完整的管理级别 Plesk 访问权。此示例基于 CentOS7 主机。

有关更多信息，请参阅 [Plesk 的备份和复原文档](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){: external}。
{:tip}

1. 通过 SSH 连接到主机。
2. 确保存在安装点目标。<br />

   Plesk 有两个备份存储选项。一个是内部 Plesk 存储器，这是 Plesk 服务器上的存储器。另一个是外部 FTP 存储器，这是 Web 或本地网络中某个外部服务器上的存储器。通常在 Plesk 框中，内部备份存储在 `/var/lib/psa/dumps` 中，并使用
`/tmp` 作为临时目录。在此示例中，临时目录会保留为本地目录，但 `dumps` 目录会移至 {{site.data.keyword.filestorage_short}} 目标 (`/backup/psa/dumps`)。不需要 FTP 用户凭证。
   {:note}
3. 如[在 Red Hat Enterprise Linux 上访问 {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux) 和[在 CentOS 中安装 NFS/{{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS) 或[在 CoreOS 上安装 NFS/{{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS) 中所述，配置 {{site.data.keyword.filestorage_short}}。将卷安装到 `/backup` 并且在文件系统表 (`/etc/fstab`) 中将其配置为启用启动时安装。<br />

   缺省情况下，NFS 会将使用 root 用户许可权创建的任何文件降级为供 nobody 用户访问。要允许 root 用户客户机保留对 NFS 共享的 root 用户许可权，需要将 `no_root_squash` 添加到 `/etc/exports`。
   {:tip}
4. **可选**。将现有备份复制到新存储器。例如，使用 `rsync`：
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {:pre}

   此命令会压缩并传输数据，同时尽可能保留信息，但硬链接除外。它提供有关正在传输什么文件的信息，并在结尾提供简短摘要。
   {:tip}
5. 编辑 `/etc/psa/psa.conf` 以指向新目标上的 `DUMP_D` 值。
    - 它将显示为：`DUMP_D /backup/psa/dumps`。
6. **可选**。根据特定用例和业务需求的指示，从服务器中除去旧存储器并从帐户中取消该存储器。
