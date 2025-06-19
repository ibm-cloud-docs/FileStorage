---

copyright:
  years: 2018, 2025
lastupdated: "2025-06-19"

keywords: File Storage for Classic, NFS, cPanel, backups

subcollection: FileStorage

---
{{site.data.keyword.attribute-definition-list}}

# Configuring {{site.data.keyword.filestorage_short}} for backup with cPanel
{: #cPanelBackups}

You can use these instructions to configure your backups to be stored in {{site.data.keyword.filestorage_full}} by cPanel. The assumption is that root or sudo SSH and full WebHost Manager (WHM) access are available.
{: shortdesc}

While you can store a backup directly to a remote file system, cPanel and WHM do **not** support this configuration. For more information, see the [cPanel documentation for backup](https://docs.cpanel.net/knowledge-base/backup/how-to-run-backups-on-locally-mounted-remoted-file-systems/){: external}.
{: attention}

1. Connect to the host through SSH.
2. Make sure that a mount point target exists.

   By default, the cPanel system saves backup files locally, to the `/backup` directory. In this document, the assumption is that the `/backup` folder exists and contains backups, and `/backup2` can be used as the new mount point.
   {: note}

3. Configure your {{site.data.keyword.filestorage_short}} as described in one of the following topics: 
    - [Accessing {{site.data.keyword.filestorage_short}} on Red Hat Enterprise Linux&reg;](/docs/FileStorage?topic=FileStorage-mountingLinux), 
    - [Mounting {{site.data.keyword.filestorage_short}} in CentOS](/docs/FileStorage?topic=FileStorage-mountingCentOS),
    - [Mounting {{site.data.keyword.filestorage_short}} in Ubuntu](/docs/FileStorage?topic=FileStorage-mountingUbuntu). 

1. Mount the volume to `/backup2` and configure it in file system table (`/etc/fstab`) to enable mounting on start.

   By default, NFS downgrades any files that were created with the root permissions to the nobody user. To allow root clients to retain the root permissions on the NFS share, `no_root_squash` needs to be added to `/etc/exports`.
   {: tip}

1. **Optional**. Copy existing backups to the new storage. You can use `rsync` for example.
   ```sh
   rsync -azv /backup/* /backup2/
   ```
   {: pre}

    This command compresses and transmits your data, and preserves as much as possible, except for hard links. It also provides information about what files are being transferred, plus a brief summary at the end.
    {: tip}

1. Log in to WebHost Manager, and go to the backup configuration through **Home** > **Backup** > **Backup Configuration**.

1. Edit the configuration to save the backups in the new mount point.
    - Change the default backup directory by entering the absolute path to the new location in place of the `/backup/` directory.
    - Select **Enable to mount a backup drive**. This setting causes the configuration process to check the `/etc/fstab` file for a backup mount (`/backup2`).

      If a mount exists with the same name as the staging directory, the backup configuration process mounts the drive and backs up the information there. After the backup process finishes, it dismounts the drive.
      {: note}

1. Apply the changes by clicking **Save Configuration**.

1. **Optional**. As dictated by your particular use case and business needs, remove the old storage from the server and cancel from the account.
