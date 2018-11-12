---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-31"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Migrating {{site.data.keyword.filestorage_short}} to enhanced {{site.data.keyword.filestorage_short}}

Enhanced {{site.data.keyword.filestorage_full}} is now available in select data centers. To see the list of upgraded data centers and available features such as adjustable IOPS rates and expandable volumes click [here](new-ibm-block-and-file-storage-location-and-features.html). For more information on provider-managed encrypted storage, see [{{site.data.keyword.filestorage_short}} Encryption-At-Rest](block-file-storage-encryption-rest.html).

The preferred migration path is to connect to both volumes simultaneously and transfer data directly from one LUN to another. The specifics depend on your operating system and whether the data is expected to change during the copy operation.

There's an assumption that you already have your non-encrypted LUN attached to your host. If not, follow the directions that fit your operating system the best to accomplish this task.

- [Mounting {{site.data.keyword.filestorage_short}} on Linux](accessing-file-storage-linux.html)
- [Mounting NFS/{{site.data.keyword.filestorage_short}} in CentOS](mounting-nsf-file-storage.html)
- [Mounting {{site.data.keyword.filestorage_short}} on CoreOS](mounting-storage-coreos.html)

All enhanced {{site.data.keyword.filestorage_short}} volumes have a different mount point than non-encrypted volumes. To ensure you're using the correct mount point for both your encrypted and non-encrypted {{site.data.keyword.filestorage_short}} volumes you can view the mount point information in the **Volume Details** page in the {{site.data.keyword.slportal}}. You can also access the correct mount point through an API call:  `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}


## Creating a new {{site.data.keyword.filestorage_short}}

When you place an order with API, specify the "Storage as a Service" package to ensure you're getting the updated features with your new storage.
{:important}

The following instructions are for ordering an enhanced volume/fileshare through the {{site.data.keyword.slportal}}/{{site.data.keyword.BluSoftlayer_full}} catalog. Your new volume must be the same size or larger than the original volume to facilitate the migration.

### Ordering a new Endurance Storage volume

1. From the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, click **Storage** > **{{site.data.keyword.filestorage_short}}** OR from {{site.data.keyword.BluSoftlayer_full}} catalog click **Infrastructure** > **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Click **Order {{site.data.keyword.filestorage_short}}**.
3. Select **Endurance** from the **Select Storage Type** list.
4. Click **Location** and select your data center.
   - Ensure that the new Storage is added in the same location as the original.
5. Select your billing option. You can choose between monthly or hourly billing.
6. Click **Endurance** and select the IOPS tier.
6. Select the **Usable Storage Size** from the list. Your new volume must be the same size or larger than the original volume.
7. Choose the **Snapshot Space Size** (in addition to your usable space) from the list.
8. Click **Continue**. You're shown the monthly and prorated charges with a final chance to review order details. Click **Previous** if you want to change your order.
9. Click the **I have read the Master Service Agreement** check box and click **Place Order**

### Ordering an Encrypted Performance Storage Volume

1. From the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, click **Storage**, **{{site.data.keyword.filestorage_short}}** OR from {{site.data.keyword.BluSoftlayer_full}} catalog click **Infrastructure** >** Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Click **Order {{site.data.keyword.filestorage_short}}**.
3. Select **Performance** from the **Select Storage Type** list.
4. Click **Location** and select your data center.
    -  Ensure that the new Storage is added in the same location as the original.
5. Select your billing options. You can choose between hourly and monthly billing.
6. Select the radio button next to the appropriate **Storage Size**.
6. Enter the IOPS in the **Specify IOPS** field.
7. Click **Continue**. You're shown the monthly and prorated charges with a final chance to review order details. Click **Previous** if you want to change your order.
8. Click the **I have read the Master Service Agreement** check box and click **Place Order**.

Storage is provisioned in less than a minute and is visible on the {{site.data.keyword.filestorage_short}} page of the {{site.data.keyword.slportal}}.


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
  - If you have backups, static content, and things that aren't expected to change during the copy, there aren't any major concerns.
  - If you're running a database or a virtual machine on your {{site.data.keyword.filestorage_short}}, make sure that the data isn't altered during the copy to avoid data corruption. If you have any bandwidth concerns, do the migration during off peak times. If you need assistance with these considerations, open a support ticket.

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
