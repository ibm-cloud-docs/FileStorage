---

copyright:
  years: 2017, 2022
lastupdated: "2022-09-30"

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

# Creating and managing duplicate volumes
{: #duplicatevolume}

You can create a duplicate of an existing {{site.data.keyword.filestorage_full}}. The duplicate volume inherits the capacity and performance options of the original volume by default and has a copy of the data up to the point-in-time of a snapshot. The duplicate volume can be dependent or independent from the original volume.
{: shortdesc}

Because the duplicate is based on the data in a point-in-time snapshot, snapshot space is required on the original volume before you can create a duplicate. For more information about snapshots and how to order snapshot space, see the [Snapshot documentation](/docs/FileStorage?topic=FileStorage-snapshots).
{: important}

**Independent duplicates** can be created from both **primary** and **replica** volumes. The new duplicate is created in the same data center as the original volume. If you create a duplicate from a replica volume, the duplicate volume is created in the same data center as the replica volume.

**Dependent duplicate** volumes are created by using a snapshot from the primary volume. Replica volumes cannot be used to create or update dependent duplicate volumes.

If you are a Dedicated account user of {{site.data.keyword.containerlong}}, see your options for duplicating a volume in the [{{site.data.keyword.containerlong_notm}} documentation](/docs/containers?topic=containers-file_storage#file_backup_restore).
{: tip}

All duplicate volumes can be accessed by a host for read and write operations as soon as the volume is provisioned.

Dependent duplicate can be refreshed from new snapshots of the parent volume manually immediately after their creation. The dependent duplicate volume keeps the original snapshot locked so the snapshot cannot be deleted while the dependent duplicate exists.

However, snapshots and replication of independent duplicate volumes aren't allowed until the data copy from the original to the duplicate is complete and the duplicate volume is fully independent from the parent volume. Depending on the size of the data, the separation process can take several hours. When it's complete, the duplicate can be managed and used as an independent volume.

This feature is available in most [locations](/docs/FileStorage?topic=FileStorage-selectDC).

Some common uses for a duplicate volume include the following examples.
- **Disaster Recovery Testing**. Create a duplicate of your replica volume to verify that the data is intact and can be used if a disaster occurs, without interrupting the replication.
- **Golden Copy**. Use a storage volume as golden copy that you can create multiple instances from for various uses.
- **Data refreshes**. Create a copy of your production data to mount to your non-production environment for testing.
- **Restore from Snapshot**. Restore data on the original volume with specific files and date from a snapshot without overwriting the entire original volume with the snapshot restore function.
- **Development and Testing (dev/test)**. Create up to four simultaneous duplicates of a volume at one time to create duplicate data for development and testing.
- **Storage Resize**. Create a volume with new size, IOPS rate or both without needing to move your data.

You can create an independent duplicate volume through the [{{site.data.keyword.cloud_notm}} console](/login){: external} in a couple of ways. However, you can provision dependent duplicate volumes only from the CLI.

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

The commands that are described in the article are part of the SLCLI. For more information about how to install and use the SLCLI, see [Python API Client](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.
{: tip}

To create an **independent duplicate** {{site.data.keyword.filestorage_short}} volume, you can use the following command.

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

**Dependent duplicate** volumes can be ordered from the SLCLI, too, with the option `--dependent-duplicate TRUE`.

```python
slcli file volume-duplicate --dependent-duplicate TRUE <primary-vol-id>
```

## Managing your duplicate volume
{: #manageduplicate}

While data is being copied from the original volume to the independent duplicate, you can see a status on the details page that shows the duplication is in progress. During this time, you can attach to a host, and read and write to the volume, but you can't create snapshot schedules or perform a refresh. When the separation process is complete, the new volume is independent from the original and can be managed with snapshots and replication as normal, and can be manually refreshed by using a snapshot from the parent volume.

Dependent duplicates do not go through the separation process and can be refreshed manually at any time. The refresh process is initiated from the CLI. Later, if you want to convert the dependent duplicate into an independent volume, you can initiate that process from the CLI, too.

## Updating data on the duplicate from the parent volume
{: #refreshindependentvol}
{: cli}

As time passes and the primary volume changes, the duplicate volume can be updated with these changes to reflect the current state through the refresh action. The refresh involves taking a snapshot of the primary volume and then, updating the duplicate volume by using that snapshot.

Refreshes can be performed by using the following command.
```python
slcli file volume-refresh <duplicate-vol-id> <primary-snapshot-id>
```

A refresh incurs no downtime on the primary volume. However, during the refresh transaction, the duplicate volume is unavailable and must be remounted after the refresh is completed.
{: important}

## Converting a dependent volume to an independent duplicate
{: #convertdependentvol}
{: cli}

If you want to use the dependent volume as a stand-alone volume in the future, you can convert it to a normal, independent {{site.data.keyword.filestorage_short}} volume through the SLCLI by using the following command.

```python
slcli file volume-convert <dependent-vol-id>
```

The conversion process can take some time to complete. The bigger the volume is, the longer it takes to convert it. Use the following command to check on the progress.

```zsh
slcli file duplicate-convert-status <dependent-vol-id>
```

Example output:
```zsh
slcli file duplicate-convert-status 370597202
Username            Active Conversion Start Timestamp   Completed Percentage
SL02SEVC307608_74   2022-06-13 14:59:17                 90
```

## Canceling a storage volume with a dependent duplicate
{: #cancelvolwithdependent}

Canceling a parent volume that has active dependent volumes requires canceling the dependent duplicate volumes first.
