---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
 
# Migrating {{site.data.keyword.filestorage_short}} to enhanced {{site.data.keyword.filestorage_short}}

Enhanced {{site.data.keyword.filestorage_full}} is now available in select data centers. To see the list of upgraded data centers and available features such as adjustable IOPS rates and expandable volumes click [here](new-ibm-block-and-file-storage-location-and-features.html). For more information on provider-managed encrypted storage, read the [{{site.data.keyword.filestorage_short}} Encryption-At-Rest](block-file-storage-encryption-rest.html) article.

The preferred migration path is to connect to both LUNs simultaneously and transfer data directly from one LUN to another. The specifics depend on your operating system and whether the data is expected to change during the copy operation. 

There's an assumption that you already have your non-encrypted LUN attached to your host. If not, follow the directions that fit your operating system the best to accomplish this task:

- [Mounting {{site.data.keyword.filestorage_short}} on Linux](accessing-file-storage-linux.html)
- [Mounting NFS/{{site.data.keyword.filestorage_short}} in CentOS](mounting-nsf-file-storage.html)
- [Mounting {{site.data.keyword.filestorage_short}} on CoreOS](mounting-storage-coreos.html)

**NOTE:** All enhanced {{site.data.keyword.filestorage_short}} volumes have a different mount point than non-encrypted volumes. To ensure you're using the correct mount point for both your encrypted and non-encrypted {{site.data.keyword.filestorage_short}} volumes you can view the mount point information in the **Volume Details** page in the UI. You can also access the correct mount point through an API call:  `SoftLayer_Network_Storage::getNetworkMountAddress()`.


## Create a new {{site.data.keyword.filestorage_short}}

**IMPORTANT**: When placing an order with API, specify the "Storage as a Service" package to ensure you're getting the updated features with your new storage.

The following instructions are for ordering an enhanced volume/fileshare through the UI. Your new volume should be the same size or larger than the original volume to facilitate the migration.

### Order a new Endurance Storage volume

1. From the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, click **Storage** > **{{site.data.keyword.filestorage_short}}** OR from {{site.data.keyword.BluSoftlayer_full}} catalog click **Infrastructure** > **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Click **Order {{site.data.keyword.filestorage_short}}** in the upper-right corner. 
3. Select **Endurance** from the **Select Storage Type** list.
4. Click **Location** and select your data center.
   - Ensure that the new Storage will be added in the same location as the original.
5. Select your billing option. You can choose between monthly or hourly billing.
6. Click **Endurance** and select the IOPS tier.
6. Select the **Usable Storage Size** from the list. Your new volume should be the same size or larger than the original volume.
7. Choose the **Snapshot Space Size** (in addition to your usable space) from the drop-down list.
8. Click** Continue**. You're shown the monthly and prorated charges with a final chance to review order details. Click **Previous** if you want to change your order.
9. Click the **I have read the Master Service Agreement** check box and click **Place Order**
 
### Order an Encrypted Performance Storage Volume

1. From the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, click **Storage**, **{{site.data.keyword.filestorage_short}}** OR from {{site.data.keyword.BluSoftlayer_full}} catalog click **Infrastructure** >** Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Click **Order {{site.data.keyword.filestorage_short}}** in the upper-right corner. 
3. Select **Performance** from the Select Storage Type list.
4. Click **Location** and select your data center.
    -  Ensure that the new Storage will be added in the same location as the original.
5. Select your billing options. You can choose between hourly and monthly billing.
6. Select the radio button next to the appropriate **Storage Size**.
6. Enter the IOPS in the **Specify IOPS** field.
7. Click **Continue**. You're shown the monthly and prorated charges with a final chance to review order details. Click **Previous** if you want to change your order.
8. Click the **I have read the Master Service Agreement** check box and click **Place Order**.

Storage will be provisioned in less than a minute and will be visible on the {{site.data.keyword.filestorage_short}} page of the {{site.data.keyword.slportal}}.

 
## Connect new {{site.data.keyword.filestorage_short}} to host

"Authorized" hosts are hosts that have been given access rights to a volume. Without host authorization, you won't be able to access or use the storage from your system.

1. Click the name of your new volume.
2. Scroll to the **Authorized Hosts** section of the page.
3. Click **Authorize Host** link on the right side of the page. Select the hosts that can access the volume.

When authorized, connect the volume to your host.

 
## Snapshots and Replication

Do you have snapshots and replication established for your original volume? If yes, you'll need to set up replication, snapshot space and create snapshot schedules for the new encrypted volume with the same settings as the original volume. 

**Note**: If your target data center hasn't been upgraded for encryption, you won' be able to establish replication for the new volume until that data center is upgraded.

 
## Migrate your data

Your host should be connected to both your original and encrypted {{site.data.keyword.filestorage_short}} volumes. If not:

- Make sure that you followed the steps in this document and referenced documents correctly.
- Open a support ticket for further assistance in connecting the two volumes to your host.

### Data considerations

At this point, consider what type of data you have on your original {{site.data.keyword.filestorage_short}} volume and how best to copy it to your encrypted volume. If you have backups, static content, and things that aren't expected to change during the copy, there aren't any major considerations.

If you're running a database or a virtual machine on your {{site.data.keyword.filestorage_short}}, make sure that the data on the original volume isn't altered during copy so that no corruption occurs. If you have any bandwidth concerns, you should perform the migration during off-peak times. If you need assistance with these considerations, open a support ticket.

### Microsoft Windows

To copy data from your original {{site.data.keyword.filestorage_short}} volume to your encrypted volume, format the new storage and copy the files over using Windows Explorer.

### Linux

You may consider using `rsync` to copy over the data. Here is an example command:

```
[root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage
```

It's recommended that you use the example command with the `--dry-run` flag once to make sure the paths line up correctly. If this process is interrupted, you may want to delete the last destination file that was being copied to make sure that it's copied to the new location from the beginning.

Once this command completes without the `--dry-run` flag, your data should be copied to the encrypted {{site.data.keyword.filestorage_short}} volume. You should scroll up and run the command again to make sure nothing was missed. You may also want to manually review both locations to look for anything that might be missing.

When your migration is complete, you'll be able to move production to the encrypted volume and detach and delete your original volume from your configuration. 

**Note**: the deletion will also remove any snapshot or replica on the target site that was associated with the original volume.
