---

copyright:
  years: 2018, 2023
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, Plesk, backups

subcollection: FileStorage

---
{{site.data.keyword.attribute-definition-list}}

# Configuring {{site.data.keyword.filestorage_short}} for backup with Plesk
{: #PleskBackup}

You can use these instructions to configure {{site.data.keyword.filestorage_full}} for your backups in Plesk. The assumption is that root or sudo SSH and full admin level Plesk access are available. This example is based on a CentOS7 host.
{: shortdesc}

For more information, see [Plesk's Documentation for backing up and restoration](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){: external}.
{: tip}

1. Connect to the host through SSH.
2. Ensure that a mount point target exists.

   Plesk has two options for storing backups. One is the internal Plesk storage, which is storage on your Plesk server. The other is external FTP storage, which is storage on some external server in the web or your local network. Commonly on Plesk boxes, internal backups are stored in `/var/lib/psa/dumps` and use `/tmp` as a temporary directory. In this example, the temporary directory is kept local, but the `dumps` directory is moved to the {{site.data.keyword.filestorage_short}} target (`/backup/psa/dumps`). No FTP user credentials are required.
   {: note}

3. Configure your {{site.data.keyword.filestorage_short}} as described in [Accessing {{site.data.keyword.filestorage_short}} on Red Hat Enterprise Linux&reg;](/docs/FileStorage?topic=FileStorage-mountingLinux) and [Mounting NFS {{site.data.keyword.filestorage_short}} in CentOS](/docs/FileStorage?topic=FileStorage-mountingCentOS). Mount the volume to `/backup` and configure it in the file system table (`/etc/fstab`) to enable mounting on start.

   By default, NFS downgrades any files that were created with the root permissions to the nobody user. To allow root clients to retain the root permissions on the NFS share, `no_root_squash` needs to be added to `/etc/exports`.
   {: tip}

4. **Optional**. Copy existing backups to the new storage. Use `rsync` for example:
   ```zsh
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {: pre}

   This command compresses and transmits your data, and it preserves as much as possible, except hard links. It also provides information about what files are being transferred plus a brief summary at the end.
   {: tip}

5. Edit `/etc/psa/psa.conf` to point the `DUMP_D` value at the new target.
    - It appears as: `DUMP_D /backup/psa/dumps`.
6. **Optional**. As dictated by your particular use case and business needs, remove the old storage from the server and cancel it from the account.
