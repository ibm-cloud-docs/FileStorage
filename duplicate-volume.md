---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-15"

keywords: File Storage, file storage, NFS, duplicate volume

subcollection: FileStorage

---
{{site.data.keyword.attribute-definition-list}}

# Creating and managing duplicate volumes
{: #duplicatevolume}

You can create a duplicate of an existing {{site.data.keyword.filestorage_full}}. The duplicate volume inherits the capacity and performance options of the original volume by default, however both attributes can be changed. The duplicate has a copy of the data up to the point-in-time of the snapshot that was used to create it. The duplicate volume can be dependent or independent from the original volume.
{: shortdesc}

If you are a Dedicated account user of {{site.data.keyword.containerlong}}, see your options for duplicating a volume in the [{{site.data.keyword.containerlong_notm}} documentation](/docs/containers?topic=containers-file_storage#file_backup_restore).
{: tip}

Because the duplicate is based on the data in a point-in-time snapshot, snapshot space is required on the original volume before you can create a duplicate. For more information about snapshots and how to order snapshot space, see the [Snapshot documentation](/docs/FileStorage?topic=FileStorage-snapshots).
{: important}

This feature is available in most [locations](/docs/FileStorage?topic=FileStorage-selectDC). As part of the data center modernization strategy for {{site.data.keyword.cloud}}, several data centers and PODs are scheduled to consolidate in late 2022 and early 2023. For more information, see [Data center consolidations](/docs/get-support?topic=get-support-dc-closure){: external}. Provisioning storage and snapshots in closing data centers is not allowed.
{: note}

## Types of duplicate volumes
{: #duplicatetype}

### Independent duplicate
{: #independent}

Independent duplicates can be created from both **primary** and **replica** volumes. The new duplicate is created in the same data center as the original volume. If you create a duplicate from a replica volume, the duplicate volume is created in the same data center as the replica.

Common uses for an independent duplicate volume:
- **Golden Copy**. Use a storage volume as golden copy that you can create multiple instances from for various uses.
- **Data refreshes**. Create a copy of your production data to mount to your nonproduction environment for testing.
- **Development and Testing (dev/test)**. Create up to four simultaneous duplicates of a volume at one time to create duplicate data for development and testing.

### Dependent duplicate
{: #dependent}

Dependent duplicate volumes are created by using a snapshot from the primary volume. Replica volumes cannot be used to create or update dependent duplicate volumes.

Common uses for a dependent duplicate volume:
- **Disaster Recovery Testing**. Create a duplicate of your replica volume to verify that the data is intact and can be used if a disaster occurs, without interrupting the replication.
- **Restore from Snapshot**. Restore data on the original volume with specific files and date from a snapshot without overwriting the entire original volume with the snapshot restore function.
- **Data refreshes**. Create a copy of your production data to mount to your nonproduction environment for testing.
- **Development and Testing (dev/test)**. Create up to four simultaneous duplicates of a volume at one time to create duplicate data for development and testing.

All duplicate volumes can be accessed by a host for read and write operations as soon as the volume is provisioned.

Dependent duplicate can be refreshed from new snapshots of the parent volume manually immediately after their creation. The dependent duplicate volume keeps the original snapshot locked so the snapshot cannot be deleted while the dependent duplicate exists.

However, snapshots and replication of independent duplicate volumes aren't allowed until the data copy from the original to the duplicate is complete and the duplicate volume is fully independent from the parent volume. Depending on the size of the data, the separation process can take several hours. When it's complete, the duplicate can be managed and used as an independent volume.

You can create a duplicate volume from the CLI and in the [{{site.data.keyword.cloud_notm}} console](/login){: external}.

## Creating a duplicate from a specific volume in the UI
{: #createdepduplicateUI}
{: ui}

1. Go to your list of {{site.data.keyword.filestorage_short}}. From the **Classic Infrastructure** ![Classic icon](../icons/classic.svg "Classic") menu, click **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Locate and click the volume name.
3. Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Duplicate Share**.
4. Select whether the duplicate is to be dependent or independent.
5. Choose your snapshot option.
    - Create a new snapshot for the most recent data.
    - Use the latest snapshot.
6. Location and IOPS Profile remain the same as the original volume.
7. You can choose to provision the duplicate volume with hourly or monthly billing method. The billing type for the original volume is automatically selected, and you can change it if you want to.
8. You can specify a different IOPS or IOPS Tier for the new volume if you want to. The IOPS designation of the original volume is set by default. Available Performance and size combinations are displayed.
    - If your original volume is 0.25 IOPS Endurance tier, you can't make a new selection.
    - If your original volume is 2, 4, or 10 IOPS Endurance tier, you can move anywhere between those tiers for the new volume.
9. You can update the size of the new volume so that it's larger than the original. The size of the original volume is set by default.

   {{site.data.keyword.filestorage_short}} can be resized to 10 times the original size of the volume.
   {: tip}

10. You can update the snapshot space for the new volume to add more, less, or no snapshot space. The snapshot space of the original volume is set by default.
11. Check the box to confirm that you read and agreed to the terms, then click **Create** to place your order.

After you click **Create**, the order confirmation window appears. When you close the window, you return to the resources list. You can go back to your list of {{site.data.keyword.filestorage_short}} shares to click on the newly provisioned duplicate. The share details section displays information such as Duplicate Type, a link to the parent share's details page and the name of the snapshot that was used to create the duplicate.

## Creating a duplicate from the SLCLI
{: #createindependentduplicateCLI}
{: cli}

The commands that are described in the topic are part of the SLCLI. For more information about how to install and use the SLCLI, see [Python API Client](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.
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

While data is being copied from the original volume to the **independent** duplicate, you can see that the status indicator on the details page shows the duplication is in progress. During this time, you can attach to a host, and read and write to the volume, but you can't create snapshot schedules or perform a refresh. When the separation process is complete, the new volume is independent from the original and can be managed with snapshots and replication as normal. The independent duplicate can be manually refreshed by using a snapshot from the parent volume after the conversion is complete.

**Dependent** duplicates do not go through the separation process and can be refreshed manually at any time. The refresh process can be initiated from the CLI or the UI. Later, if you want to convert the dependent duplicate into an independent volume, you can initiate that process by using the UI or the CLI, too.

## Updating data on the duplicate from the parent volume in the UI
{: #refreshindependentvol_ui}
{: ui}

1. Go to your list of {{site.data.keyword.filestorage_short}}. From the **Classic Infrastructure** ![Classic icon](../icons/classic.svg "Classic") menu, click **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Locate the duplicate volume and click its name to view the volume details.
3. Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Restore parent snapshot**.
4. From the list of snapshots, select the parent snapshot that holds the data you want to restore to the duplicate volume.
   Performing a restore results in the loss of any data that was created or modified since the selected snapshot was taken. During the refresh transaction, the duplicate volume is unavailable and must be remounted after the refresh is completed.
   {:  note}

5. Check the box to confirm that you want to proceed with the refresh operation.
6. Click **Yes**.

## Converting a dependent volume to an independent duplicate in the UI
{: #convertdependentvol_ui}
{: ui}

1. Go to your list of {{site.data.keyword.filestorage_short}}. From the **Classic Infrastructure** ![Classic icon](../icons/classic.svg "Classic") menu, click **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Locate the duplicate volume and click its name to view the volume details.
3. Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Convert Dependent Duplicate**.
4. Check the box to confirm that you want to proceed with the conversion.
5. Click **Yes**.

The conversion process can take some time to complete. The bigger the volume is, the longer it takes to convert it. You can view the status of the process on the volume details page under the **Duplicate conversion status** header.

## Updating data on the duplicate from the parent volume from the CLI
{: #refreshindependentvol}
{: cli}

As time passes and the primary volume changes, the duplicate volume can be updated with these changes to reflect the current state through the refresh action. The refresh involves taking a snapshot of the primary volume and then, updating the duplicate volume by using that snapshot.

Refreshes can be performed by using the following command.
```python
slcli file volume-refresh <duplicate-vol-id> <primary-snapshot-id>
```

A refresh incurs no downtime on the primary volume. However, during the refresh transaction, the duplicate volume is unavailable and must be remounted after the refresh is completed.
{: important}

## Converting a dependent volume to an independent duplicate from the CLI
{: #convertdependentvol}
{: cli}

If you want to use the dependent volume as a stand-alone volume in the future, you can convert it to a normal, independent {{site.data.keyword.filestorage_short}} volume through the SLCLI. Use the following command.

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
