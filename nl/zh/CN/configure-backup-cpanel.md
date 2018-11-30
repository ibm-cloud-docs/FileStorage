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

# 使用 cPanel 配置 {{site.data.keyword.filestorage_short}} 进行备份

您可以使用这些指示信息来配置要由 cPanel 存储在 {{site.data.keyword.filestorage_full}} 中的备份。假定以 root 用户或 sudo 用户身份通过 SSH 登录到系统，并且有完整的 WebHost Manager (WHM) 访问权。此示例基于 **CentOS 7** 主机。

有关来自供应商的更多信息，请参阅 [cPanel - 配置备份目录 ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){:new_window}。
{:tip}

1. 通过 SSH 连接到主机。
2. 确保存在安装点目标。<br />

   缺省情况下，cPanel 系统会将备份文件保存到本地的 `/backup` 目录中。在本文档中，假定 `/backup` 文件夹已存在并包含备份，因此可以使用 `/backup2` 作为新的安装点。
   {:note}

3. 如[在 Red Hat Enterprise Linux 上访问 {{site.data.keyword.filestorage_short}}](accessing-file-storage-linux.html) 和[在 CentOS 中安装 NFS/{{site.data.keyword.filestorage_short}}](mounting-nsf-file-storage.html)/[在 CoreOS 中安装 NFS/{{site.data.keyword.filestorage_short}}](mounting-storage-coreos.html) 中所述，配置 {{site.data.keyword.filestorage_short}}。将卷安装到 `/backup2` 并且在文件系统表 (`/etc/fstab`) 中将其配置为启用启动时安装。<br />

   缺省情况下，NFS 会将使用 root 用户许可权创建的任何文件降级为供 nobody 用户访问。要允许 root 用户客户机保留对 NFS 共享的 root 用户许可权，需要将 `no_root_squash` 添加到 `/etc/exports`。
   {:tip}

4. **可选**。将现有备份复制到新存储器。例如，可以使用 `rsync`。
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}

    此命令会压缩并传输数据，同时尽可能地保留信息，但硬链接除外。它还提供有关正在传输什么文件的信息并在结尾提供简短摘要。
    {:tip}

5. 登录到 WebHost Manager，然后通过**主页** > **备份** > **备份配置**转至备份配置。

6. 编辑配置以将备份保存在新的安装点中。
    - 通过输入新位置的绝对路径来取代 `/backup/` 目录，从而更改缺省备份目录。
    - 选择**启用以安装备份驱动器**。此设置会使配置过程检查 `/etc/fstab` 文件中是否有备份安装 (`/backup2`)。<br />

      如果某个安装与编译打包目录同名，那么备份配置过程会安装驱动器并将信息备份到该驱动器上。备份过程完成后，会卸装该驱动器。
      {:note}
7. 通过单击**保存配置**来应用更改。
8. **可选**。根据特定用例和业务需求的指示，从服务器中除去旧存储器并从帐户中取消该存储器。
