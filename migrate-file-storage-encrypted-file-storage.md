---

copyright:
  years: 2014, 2024
lastupdated: "2024-02-15"

keywords: File Storage, NFS, upgrade, migrate to new

subcollection: FileStorage

---
{{site.data.keyword.attribute-definition-list}}

# Migrating {{site.data.keyword.filestorage_short}} to enhanced {{site.data.keyword.filestorage_short}}
{: #migratestorage}

Enhanced {{site.data.keyword.filestorage_full}} is now available in all [data centers](/docs/overview?topic=overview-locations#data-centers). The preferred migration path is to provision an enhanced {{site.data.keyword.filestorage_short}} volume, then connect to both volumes simultaneously and transfer data directly from one volume to another. The specifics depend on your operating system and whether the data is expected to change during the copy operation.
{: shortdesc}

All new {{site.data.keyword.filestorage_short}} volumes that are provisioned in these data centers have a different mount point than nonencrypted volumes. To ensure you're using the correct mount point for both storage volumes, you can view the mount point information in the {{site.data.keyword.filestorage_short}} Details page in the UI. You can also access the correct mount point through an API call: `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{: tip}

You don't need to follow this process if your Storage received an upgrade to the Storage-as-a-Service package as part of the ongoing hardware refresh process.
{: tip}

## Creating a {{site.data.keyword.filestorage_short}}
{: #createencryptedvol}

You can order an enhanced volume through the {{site.data.keyword.cloud}} Console UI, through the CLI or the API. Your new volume must be of the same size or greater than the original file share to facilitate the migration. For more information about provisioning a file share, see [Ordering {{site.data.keyword.filestorage_short}}](/docs/FileStorage?topic=FileStorage-orderingFileStorage).

When you place an order with API, specify the "Storage-as-a-Service" package to ensure you're getting the updated features with your new storage.
{: important}

When you order a file share, your new storage is available to mount in a few minutes. You can view it in the Resource List and in the {{site.data.keyword.filestorage_short}} list.

## Authorizing host to the new {{site.data.keyword.filestorage_short}}
{: #authtonewvol}

"Authorized" hosts are hosts that were given access to a volume. Without host authorization, you can't access or use the storage from your system.

You can create the authorization in the [UI](/docs/FileStorage?topic=FileStorage-managingstorage&interface=ui#authhostUI), from the [CLI](/docs/FileStorage?topic=FileStorage-managingstorage&interface=cli#authhostCLI), with the API, or with [Terraform](/docs/FileStorage?topic=FileStorage-managingstorage&interface=terraform#authhostterraform).

When the host is authorized, connect the volume to your host.

## Migrating your data
{: #copydataacross}

1. Connect to both your original and new {{site.data.keyword.filestorage_short}} volumes.
   - [Mounting {{site.data.keyword.filestorage_short}} in CentOS](/docs/FileStorage?topic=FileStorage-mountingCentOS)
   - [Mounting {{site.data.keyword.filestorage_short}} on Red Hat Linux&reg;](/docs/FileStorage?topic=FileStorage-mountingLinux)
   - [Mounting {{site.data.keyword.filestorage_short}} on Ubuntu](/docs/FileStorage?topic=FileStorage-mountingUbuntu)

    If you need assistance with connecting the two file shares to your host, open a support case.
    {: tip}

2. Consider the type of data that you have on your original {{site.data.keyword.filestorage_short}} volume and how best to copy it to your new file share.
   - If you have backups, static content, and things that aren't expected to change during the copy, you don't need to worry.
   - If you're running a database or a virtual machine on your {{site.data.keyword.filestorage_short}}, make sure that the data isn't altered during the copy to avoid data corruption.
   - If you have any bandwidth concerns, do the migration during off peak times.
   - If you need assistance with these considerations, open a support ticket.

3. Copy your data across. For example, you can use `rsync` to copy over the data.
   ```sh
   [root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage
   ```

   It's advisable to use the previous command with the `--dry-run` flag first to make sure that the paths line up correctly. If this process is interrupted, you can delete the last destination file that was being copied to make sure that it is copied to the new location from the beginning.
   {: tip}

   When this command completes without the `--dry-run` flag, your data is copied to the new {{site.data.keyword.filestorage_short}} volume. Run the command again to make sure that nothing was missed. You can also manually review both locations to look for anything that might be missing.

   For more information about `rsync`, see the [`rsync` man page](https://download.samba.org/pub/rsync/rsync.html){: external}.
   {: note}

4. When your migration is complete, you can move production to the new volume. Then, you can detach and delete your original volume from your configuration. The deletion also removes any snapshot or replica on the target site that was associated with the original volume.

## Setting up Snapshots and Replication
{: #setupnewreplica}

If snapshots and replication were established for your original volume, then you need to set up them up for the new volume. Configure replication, snapshot space and create snapshot schedules with the same settings as the original volume.

If your target data center does not have encryption, you can't establish replication for the new volume until that data center is upgraded.
{: important}
