---

copyright:
  years: 2018, 2019
lastupdated: "2019-12-06"

keywords: File Storage, file storage, NFS, cPanel, backups

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:shortdesc: .shortdesc}

# Configuring {{site.data.keyword.filestorage_short}} for backup with cPanel
{: #cPanelBackups}

You can use these instructions to configure your backups to be stored in {{site.data.keyword.filestorage_full}} by cPanel. The assumption is that root or sudo SSH and full WebHost Manager (WHM) access are available. This example is based on a **CentOS 7** host.
{:shortdesc}

For more information, see [cPanel Docs - Backup](https://docs.cpanel.net/knowledge-base/backup/){: external}.
{:tip}

1. Connect to the host through SSH.
2. Ensure that a mount point target exists. <br />

   By default, the cPanel system saves backup files locally, to the `/backup` directory. In this document, the assumption is that `/backup` folder exists and contains backups, and `/backup2` can be used as the new mount point.
   {:note}

3. Configure your {{site.data.keyword.filestorage_short}} as described in [Accessing {{site.data.keyword.filestorage_short}} on Red Hat Enterprise Linux&reg;](/docs/FileStorage?topic=FileStorage-mountingLinux) or [Mounting {{site.data.keyword.filestorage_short}} in CentOS](/docs/FileStorage?topic=FileStorage-mountingCentOS)/[Mounting NFS/{{site.data.keyword.filestorage_short}}. Mount the volume to `/backup2` and configure it in file system table (`/etc/fstab`) to enable mounting on start. <br />

   By default, NFS downgrades any files that were created with the root permissions to the nobody user. To allow root clients to retain the root permissions on the NFS share, `no_root_squash` needs to be added to `/etc/exports`.
   {:tip}

4. **Optional**. Copy existing backups to the new storage. You can use `rsync` for example.
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}

    This command compresses and transmits your data, and preserves as much as possible, except for hard links. It also provides information about what files are being transferred, plus a brief summary at the end.
    {:tip}

5. Log in to WebHost Manager, and go to the backup configuration through **Home** > **Backup** > **Backup Configuration**.

6. Edit the configuration to save the backups in the new mount point.
    - Change the default backup directory by entering the absolute path to the new location in place of the `/backup/` directory.
    - Select **Enable to mount a backup drive**. This setting causes the configuration process to check the `/etc/fstab` file for a backup mount (`/backup2`). <br />

      If a mount exists with the same name as the staging directory, the backup configuration process mounts the drive and backs up the information there. After the backup process finishes, it dismounts the drive.
      {:note}
7. Apply the changes by clicking **Save Configuration**.
8. **Optional**. As dictated by your particular use case and business needs, remove the old storage from the server and cancel from the account.
