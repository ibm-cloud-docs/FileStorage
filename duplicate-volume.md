---

copyright:
  years: 2017, 2022
lastupdated: "2022-08-29"

keywords: File Storage, file storage, NFS, duplicate volume

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:shortdesc: .shortdesc}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Creating and managing independent duplicate volumes
{: #duplicatevolume}

You can create a duplicate of an existing {{site.data.keyword.filestorage_full}}. The duplicate volume inherits the capacity and performance options of the original volume by default and has a copy of the data up to the point-in-time of a snapshot. The duplicate volume is completely independent from the original volume.
{: shortdesc}  

Because the duplicate is based on the data in a point-in-time snapshot, snapshot space is required on the original volume before you can create a duplicate. To learn more about snapshots and how to order snapshot space, refer to [Snapshot documentation](/docs/FileStorage?topic=FileStorage-snapshots).  
{: important}

 Independent duplicates can be created from both **primary** and **replica** volumes. The new duplicate is created in the same data center as the original volume. If you create a duplicate from a replica volume, the new volume is created in the same data center as the replica volume.

If you are a Dedicated account user of {{site.data.keyword.containerlong}}, see your options for duplicating a volume in the [{{site.data.keyword.containerlong_notm}} documentation](/docs/containers?topic=containers-file_storage#file_backup_restore).
{: tip}

Duplicate volumes can be accessed by a host for read/write as soon as the storage is provisioned. However, snapshots and replication aren't allowed until the data copy from the original to the duplicate is complete. Depending on the size of the data, the copying process can take up to several hours. When the data copy is complete, the duplicate can be managed and used as an independent volume.

This feature is available in most [locations](/docs/FileStorage?topic=FileStorage-selectDC).

Some common uses for a duplicate volume include the following examples.
- **Disaster Recovery Testing**. Create a duplicate of your replica volume to verify that the data is intact and can be used if a disaster occurs, without interrupting the replication.
- **Golden Copy**. Use a storage volume as golden copy that you can create multiple instances from for various uses.
- **Data refreshes**. Create a copy of your production data to mount to your non-production environment for testing.
- **Restore from Snapshot**. Restore data on the original volume with specific files and date from a snapshot without overwriting the entire original volume with the snapshot restore function.
- **Development and Testing (dev/test)**. Create up to four simultaneous duplicates of a volume at one time to create duplicate data for development and testing.
- **Storage Resize**. Create a volume with new size, IOPS rate or both without needing to move your data.  

You can create a duplicate volume through the [{{site.data.keyword.cloud}} console](https://{DomainName}/){: external} in a couple of ways.


## Creating a duplicate from a specific volume in the UI
{: #createdepduplicateUI}
{: ui}

1. Go to your list of {{site.data.keyword.filestorage_short}}. From the **Classic Infrastructure** ![Classic icon](../icons/classic.svg "Classic") menu, click **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Locate and click the volume name.
3. Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Duplicate Share**.
4. Choose your snapshot option.
    - Create a new snapshot for the most recent data.
    - Use the latest snapshot.
5. Location and IOPS Profile remain the same as the original volume.
6. You can choose to provision the duplicate volume with hourly or monthly billing method. The billing type for the original volume is automatically selected, and you can change it if you want to.
7. You can specify a different IOPS or IOPS Tier for the new volume if you want to. The IOPS designation of the original volume is set by default. Available Performance and size combinations are displayed.
    - If your original volume is 0.25 IOPS Endurance tier, you can't make a new selection.
    - If your original volume is 2, 4, or 10 IOPS Endurance tier, you can move anywhere between those tiers for the new volume.
8. You can update the size of the new volume so that it's larger than the original. The size of the original volume is set by default.

   {{site.data.keyword.filestorage_short}} can be resized to 10 times the original size of the volume.
   {: tip}

9. You can update the snapshot space for the new volume to add more, less, or no snapshot space. The snapshot space of the original volume is set by default.
10. Check the box to confirm that you read and agreed to the terms, then click **Create** to place your order.


## Creating a duplicate from the SLCLI
{: #createindependentduplicateCLI}
{: cli}

You can create a duplicate volume from the SLCLI by using the following command.

```python
# slcli file volume-duplicate --help
Usage: slcli file volume-duplicate [OPTIONS] ORIGIN_VOLUME_ID

Options:
  -o, --origin-snapshot-id INTEGER
                                  ID of an origin volume snapshot to use for
                                  duplcation.
  -c, --duplicate-size INTEGER    Size of duplicate file volume in GB. ***If
                                  no size is specified, the size of the origin
                                  volume will be used.***
                                  Minimum: [the size
                                  of the origin volume]
  -i, --duplicate-iops INTEGER    Performance Storage IOPS, between 100 and
                                  6000 in multiples of 100 [only used for
                                  performance volumes] ***If no IOPS value is
                                  specified, the IOPS value of the origin
                                  volume will be used.***
                                  Requirements: [If
                                  IOPS/GB for the origin volume is less than
                                  0.3, IOPS/GB for the duplicate must also be
                                  less than 0.3. If IOPS/GB for the origin
                                  volume is greater than or equal to 0.3,
                                  IOPS/GB for the duplicate must also be
                                  greater than or equal to 0.3.]
  -t, --duplicate-tier [0.25|2|4|10]
                                  Endurance Storage Tier (IOPS per GB) [only
                                  used for endurance volumes] ***If no tier is
                                  specified, the tier of the origin volume
                                  will be used.***
                                  Requirements: [If IOPS/GB
                                  for the origin volume is 0.25, IOPS/GB for
                                  the duplicate must also be 0.25. If IOPS/GB
                                  for the origin volume is greater than 0.25,
                                  IOPS/GB for the duplicate must also be
                                  greater than 0.25.]
  -s, --duplicate-snapshot-size INTEGER
                                  The size of snapshot space to order for the
                                  duplicate. ***If no snapshot space size is
                                  specified, the snapshot space size of the
                                  origin file volume will be used.***
                                  Input
                                  "0" for this parameter to order a duplicate
                                  volume with no snapshot space.
  --billing [hourly|monthly]      Optional parameter for Billing rate (default
                                  to monthly)
  -h, --help                      Show this message and exit.
```

## Managing your duplicate volume
{: #manageduplicate}

While data is being copied from the original volume to the duplicate, you can see a status on the details page that shows the duplication is in progress. Depending on the size of the data, the copying process can take up to several hours. During this time, you can attach to a host and read/write to the volume, but you can't create snapshot schedules or perform a refresh from the original volume. When the duplication process is complete, the new volume is independent from the original and can be managed with snapshots and replication as normal.

## Updating data on the independent duplicate volume
{: #refreshindependentvol}
{: cli}

As time passes and the primary volume changes, the duplicate volume can be updated with these changes to reflect the current state through the refresh action. The data on the duplicate volume can be refreshed at any time. The refresh involves taking a snapshot of the primary volume and then, updating the duplicate volume by using that snapshot. A refresh incurs no downtime on the primary volume. However, during the refresh transaction, the duplicate volume is unavailable and must be remounted after the refresh is completed.

Refreshes can be performed by using the SLCLI.
```python
slcli file volume-refresh <duplicate-vol-id> <primary-snapshot-id>
```
