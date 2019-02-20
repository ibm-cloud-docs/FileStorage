---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Migrating {{site.data.keyword.filestorage_short}} to enhanced {{site.data.keyword.filestorage_short}}
{: #migratestorage}

Enhanced {{site.data.keyword.filestorage_full}} is now available in select data centers. To see the list of upgraded data centers and available features such as adjustable IOPS rates and expandable volumes, click [here](/docs/infrastructure/FileStorage?topic=FileStorage-news). For more information about provider-managed encryption, see [{{site.data.keyword.filestorage_short}} Encryption-At-Rest](/docs/infrastructure/FileStorage?topic=FileStorage-encryption).

The preferred migration path is to connect to both volumes simultaneously and transfer data directly from one LUN to another. The specifics depend on your operating system and whether the data is expected to change during the copy operation.

The assumption is that you already have your non-encrypted LUN attached to your host. If not, follow the directions that fit your operating system the best to accomplish this task.

- [Mounting {{site.data.keyword.filestorage_short}} on Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [Mounting {{site.data.keyword.filestorage_short}} in CentOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [Mounting {{site.data.keyword.filestorage_short}} on CoreOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)

All enhanced {{site.data.keyword.filestorage_short}} volumes that are provisioned in these data centers have a different mount point than non-encrypted volumes. To ensure you're using the correct mount point for both storage volumes, you can view the mount point information in the **Volume Details** page in the console. You can also access the correct mount point through an API call: `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}


## Creating a {{site.data.keyword.filestorage_short}}

When you place an order with API, specify the "Storage as a Service" package to ensure you're getting the updated features with your new storage.
{:important}

You can order an enhanced LUN through the {{site.data.keyword.BluSoftlayer_full}} catalog and the {{site.data.keyword.slportal}}. Your new volume must be of the same size or greater than the original fileshare to facilitate the migration.

- [Ordering {{site.data.keyword.filestorage_short}} with pre-defined IOPS Tiers (Endurance)](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole#endurance)
- [Ordering {{site.data.keyword.filestorage_short}} with custom IOPS (Performance)](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole#performance)

Your new storage is available to mount in a few minutes. You can view it in the Resource List and in the {{site.data.keyword.blockstorageshort}} list.


## Authorizing host to the new {{site.data.keyword.filestorage_short}}

"Authorized" hosts are hosts that were given access to a volume. Without host authorization, you can't access or use the storage from your system.

1. Click the name of your new volume.
2. Scroll to the **Authorized Hosts** section.
3. Click **Authorize Host** link on the right. Select the hosts that can access the volume.

When the host is authorized, connect the volume to your host.


## Setting up Snapshots and Replication

If snapshots and replication were established for your original volume, then you need to set up them up for the new volume. Configure replication, snapshot space and create snapshot schedules the same settings as the original volume.

If your target data center does not have encryption, you can't establish replication for the new volume until that data center is upgraded.
{:important}


## Migrating your data

1. Connect to both your original and new {{site.data.keyword.filestorage_short}} volumes.
  - If you need assistance with connecting the two files hares to your host, open a support ticket.

2. Consider what type of data you have on your original {{site.data.keyword.filestorage_short}} volume and how best to copy it to your new file share
  - If you have backups, static content, and things that aren't expected to change during the copy, you don't have to worry.
  - If you're running a database or a virtual machine on your {{site.data.keyword.filestorage_short}}, make sure that the data isn't altered during the copy to avoid data corruption.
  - If you have any bandwidth concerns, do the migration during off peak times.
  - If you need assistance with these considerations, open a support ticket.

3. Copy your data across.
   - **Microsoft Windows**
     - To copy data from your original {{site.data.keyword.filestorage_short}} LUN to your new LUN, format the new storage, and copy the files over by using Windows Explorer.
   - **Linux**
     - You can use `rsync` to copy over the data.
       ```
       [root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage
       ```

   It's a good idea to use the previous command with the `--dry-run` flag once to make sure that the paths line up correctly. If this process is interrupted, you can delete the last destination file that was being copied to make sure that it is copied to the new location from the beginning.

   When this command completes without the `--dry-run` flag, your data is copied to the new {{site.data.keyword.filestorage_short}} volume. Run the command again to make sure that nothing was missed. You can also manually review both locations to look for anything that might be missing.

   When your migration is complete, you can move production to the new LUN. Then, you can detach and delete your original volume from your configuration. The deletion also removes any snapshot or replica on the target site that was associated with the original volume.
