---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
 
# Configuring {{site.data.keyword.filestorage_short}} for Backup with Plesk

In this article, we aim to provide instructions for configuring {{site.data.keyword.blockstoragefull}} for your backups in Plesk. The assumption is that root or sudo SSH and full admin level Plesk access are available. This example is based on a CentOS7 host.

**Note**: You can find Plesk's documentation for backing up and restoration [here](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window}.

1. Connect to the host through SSH.

2. Ensure that a mount point target exists. <br />
   **Note**: Plesk has two options for storing backups. One is the internal Plesk storage - backup storage on your Plesk server, the other is an external FTP storage - backup storage on some external server in the web or your local network. Commonly on Plesk boxes, internal backups are stored in `/var/lib/psa/dumps` and use `/tmp` as a temporary directory. In our example, we keep the temporary directory local, but move the dump directory to the STaaS target (`/backup/psa/dumps`). No FTP user credentials are required.
   
3. Configure your {{site.data.keyword.filestorage_short}} as described in [Accessing {{site.data.keyword.filestorage_short}} on Red Hat Enterprise Linux](accessing-file-storage-linux.html) and [Mounting NFS/{{site.data.keyword.filestorage_short}} in CentOS](mounting-nsf-file-storage.html). Make sure you mount it to `/backup` and configure it in `/etc/fstab` to enable mounting on boot. <br />
   **Note**: By default NFS will downgrade any files created with the root permissions to the nobody user. To allow root clients to retain root permissions on the NFS share `no_root_squash`should be added to `/etc/exports`. <br />
   **Note**: There are instructions available for [Mounting NFS/{{site.data.keyword.filestorage_short}} on CoreOS](mounting-storage-coreos.html) as well. <br />

4. **Optional**: Copy existing backups to the new storage. Use `rsync` for example:
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {: pre}
    
    **Note**: This command transmits your data compressed while preserving as much as possible, except hard links. It also provides information about what files are being transferred plus a brief summary at the end.
    
5. Edit `/etc/psa/psa.conf` to point the `DUMP_D` value at the new target. 
    - Should appear as: `DUMP_D /backup/psa/dumps`. 

6. **Optional**: As dictated by your particular use case and business needs, remove the old storage from the server and cancel from the account.

