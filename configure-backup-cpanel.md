---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-11"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
 
# Configuring {{site.data.keyword.filestorage_short}} for Backup with cPanel

In this article, we aim to provide instructions for configuring your backups to be stored in {{site.data.keyword.filestorage_full}} by cPanel. The assumption is that root or sudo SSH and full WebHost Manager (WHM) access are available. This example is based on a **CentOS 7** host.

**Note**: You can find cPanel's documentation for Configuring Backup Directory [here](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){:new_window}.

1. Connect to the host through SSH.

2. Ensure that a mount point target exists. <br />
   **Note**: By default, the cPanel system saves backup files locally, to the `/backup` directory. For the purposes of this document, we assume that `/backup` exists and contains backups, so we’ll use `/backup2` as the new mount point.
   
3. Configure your {{site.data.keyword.filestorage_short}} as described in [Accessing {{site.data.keyword.filestorage_short}} on Red Hat Enterprise Linux](accessing-file-storage-linux.html) and [Mounting NFS/{{site.data.keyword.filestorage_short}} in CentOS](mounting-nsf-file-storage.html). Make sure you mount it to `/backup2` and configure it in `/etc/fstab` to enable mounting on boot. <br />
   **Note**: By default NFS will downgrade any files created with the root permissions to the nobody user. To allow root clients to retain root permissions on the NFS share `no_root_squash`should be added to `/etc/exports`. <br />
   **Note**: There are instructions available for [Mounting NFS/{{site.data.keyword.filestorage_short}} on CoreOS](mounting-storage-coreos.html) as well. <br />

4. **Optional**: Copy existing backups to the new storage. Use `rsync` for example:
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}
    
    **Note**: This command transmits your data compressed while preserving as much as possible, except for hard links. It also provides information about what files are being transferred plus a brief summary at the end.
    
5.  Log in to WebHost Manager and navigate to the backup configuration via **Home** > **Backup** > **Backup Configuration**.

6.  Edit the configuration to save the backups in the new mount point. 
    - Change the default backup directory by entering the absolute path to the new location in place of the /backup/ directory. 
    - Select **Enable to mount a backup drive**. This setting causes the Backup Configuration process to check the `/etc/fstab` file for a backup mount (`/backup2`). <br /> **Note**: If a mount exists with the same name as the staging directory, the Backup Configuration process mounts the drive and backs up the information to the mount. After the backup process finishes, it dismounts the drive. 

7. Apply the changes by clicking **Save Configuration** at the bottom of the **Backup Configuration** interface.

8. **Optional**: As dictated by your particular use case and business needs, remove the old storage from the server and cancel from the account.
