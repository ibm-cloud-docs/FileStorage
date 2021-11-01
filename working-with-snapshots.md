---

copyright:
  years: 2014, 2021
lastupdated: "2021-05-03"

keywords: File Storage, file storage, NFS, snapshots, snapshot schedule, manual snapshot, snapshot space, snapshot quota

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

# Managing Snapshots
{: #managingSnapshots}

Snapshots are a feature of {{site.data.keyword.blockstoragefull}}. A snapshot represents a volume's contents at a particular point in time. With snapshots, you can protect your data with no performance impact and minimal consumption of space. Learn more about how to manage snapshots by reading the following instructions.
{: shortdesc}

## Adding a Snapshot schedule in the UI
{: #addscheduleUI}
{: ui}

You decide how often and when you want to create a point-in-time reference of your storage volume with Snapshot schedules. You can have a maximum of 50 snapshots per storage volume. Schedules are managed through the **Storage** > **{{site.data.keyword.filestorage_short}}** tab of the [{{site.data.keyword.cloud}} console](https://{DomainName}/){: external}.

Before you can set up your initial schedule, you must first purchase snapshot space if you didn't purchase it during the initial provisioning of the storage volume.
{: important}

Snapshots schedules can be set up for hourly, daily, and weekly intervals, each with a distinct retention cycle. The maximum limit of snapshots is 50  per storage volume, which can be a mix of hourly, daily, and weekly schedules, and manual snapshots.

1. Click your storage volume, click **Actions**, and click **Schedule Snapshot**.
2. In the New Schedule Snapshot window, you can select from three different snapshot frequencies. Use any combination of the three to create a comprehensive snapshot schedule.
   - Hourly
      - Specify the minute each hour that a snapshot is to be taken. The default is the current minute.
      - Specify the number of hourly snapshots to be retained before the oldest is discarded.
   - Daily
      - Specify the hour and minute that a snapshot is to be taken. The default is the current hour and minute.
      - Select the number of hourly snapshots to be retained before the oldest is discarded.
   - Weekly
      - Specify the day of the week, hour, and minute that a snapshot is to be taken. The default is the current day, hour, and minute.
      - Select the number of weekly snapshots to be retained before the oldest is discarded.
3. Click **Save**. If the total number of scheduled snapshots is over 50, you receive a warning message and can't save.

The list of the snapshots is displayed as they're taken in the **Snapshots** section of the **Detail** page.

## Adding a Snapshot schedule from the SLCLI
{: #addscheduleCLI}
{: cli}

You decide how often and when you want to create a point-in-time reference of your storage volume with Snapshot schedules. You can have a maximum of 50 snapshots per storage volume. Schedules are managed through the **Storage** > **{{site.data.keyword.filestorage_short}}** tab of the [{{site.data.keyword.cloud}} console](https://{DomainName}/){: external}.

Before you can set up your initial schedule, you must first purchase snapshot space if you didn't purchase it during the initial provisioning of the storage volume.
{: important}

To set up a snapshot schedule, use the following command

```python
# slcli file snapshot-enable --help
Usage: slcli file snapshot-enable [OPTIONS] VOLUME_ID

  Enables snapshots for a given volume on the specified schedule

Options:
  --schedule-type TEXT    Snapshot schedule [INTERVAL|HOURLY|DAILY|WEEKLY]
                          [required]
  --retention-count TEXT  Number of snapshots to retain  [required]
  --minute INTEGER        Minute of the day when snapshots should be taken
  --hour INTEGER          Hour of the day when snapshots should be taken
  --day-of-week TEXT      Day of the week when snapshots should be taken
  -h, --help              Show this message and exit.

```

You can see the list of your snapshot schedules from the SLCLI with the following command.
```python
# slcli file snapshot-schedule-list --help
Usage: slcli file snapshot-schedule-list [OPTIONS] VOLUME_ID

  Lists snapshot schedules for a given volume

Options:
  -h, --help  Show this message and exit.
```

## Taking a manual Snapshot in the UI
{: #takemanualsnapshotUI}
{: ui}

Manual snapshots can be taken at various points during an application upgrade or maintenance. You can also take snapshots across multiple servers that were temporarily deactivated at the application level.

The maximum limit of manual snapshots per storage volume is 50.

1. Click your storage volume.
2. Click **Actions**.
3. Click **Take Manual Snapshot**.

The snapshot is taken and displayed in the **Snapshots** section of the **Detail** page. Its schedule appears Manual.

## Taking a manual Snapshot from the SLCLI
{: #takemanualsnapshotCLI}
{: cli}

You can use the following command to create a snapshot from the SLCLI.
```python
# slcli file snapshot-create --help
Usage: slcli file snapshot-create [OPTIONS] VOLUME_ID

Options:
  -n, --notes TEXT  Notes to set on the new snapshot
  -h, --help        Show this message and exit.
```

## Listing all Snapshots with Space Used Information and Management functions in the UI
{: #listsnapshotUI}
{: ui}

A list of retained snapshots and space that is used can be seen on the **Snapshots** page of the selected volume. Management functions (editing schedules and adding more space) are conducted by using the **Actions** menu or links in the various sections on the page. The Snapshot page displays how much capacity the volume has and how much of it is used. You receive notifications when you reach space thresholds – 75 percent, 90 percent, and 95 percent.

Notifications are sent through the support tickets to the Master User on your account when you reach three different space thresholds – 75 percent, 90 percent, and 95 percent.

- At **75 percent capacity**, a warning is sent that snapshot space usage exceeded 75 percent. If you heed the warning and manually add space or delete retained and unnecessary snapshots, the action is noted and the ticket is closed. If you do nothing, you must manually acknowledge the ticket and then it is closed.
- At **90 percent capacity**, a second warning is sent when snapshot space usage exceeded 90 percent. Like with reaching 75 percent capacity, if you take the necessary actions to decrease the space that is used, the action is noted and the ticket is closed. If you do nothing, you must manually acknowledge the ticket and then it is closed.
- At **95 percent capacity**, a final warning is sent. If no action is taken to bring your space usage under the threshold, a notification is generated and automatic deletion occurs so that future snapshots can be created. Scheduled snapshots are deleted, starting with the oldest, until usage drops under 95 percent. Snapshots continue to be deleted each time usage exceeds 95 percent until it drops under the threshold. If the space is manually increased or snapshots are deleted, the warning is reset and reissued if the threshold is exceeded again. If no actions are taken, this notification is the only warning that you receive.

## Listing all Snapshots with Space Used Information and Management functions from the SLCLI
{: #listsnapshotCLI}
{: cli}

You can accomplish this task from the SLCLI by using the following command.
```python
# slcli file snapshot-list --help
Usage: slcli file snapshot-list [OPTIONS] VOLUME_ID

Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: id, name, created, size_bytes
  -h, --help      Show this message and exit.
```

Notifications are sent through the support tickets to the Master User on your account when you reach three different space thresholds – 75 percent, 90 percent, and 95 percent.

- At **75 percent capacity**, a warning is sent that snapshot space usage exceeded 75 percent. If you heed the warning and manually add space or delete retained and unnecessary snapshots, the action is noted and the ticket is closed. If you do nothing, you must manually acknowledge the ticket and then it is closed.
- At **90 percent capacity**, a second warning is sent when snapshot space usage exceeded 90 percent. Like with reaching 75 percent capacity, if you take the necessary actions to decrease the space that is used, the action is noted and the ticket is closed. If you do nothing, you must manually acknowledge the ticket and then it is closed.
- At **95 percent capacity**, a final warning is sent. If no action is taken to bring your space usage under the threshold, a notification is generated and automatic deletion occurs so that future snapshots can be created. Scheduled snapshots are deleted, starting with the oldest, until usage drops under 95 percent. Snapshots continue to be deleted each time usage exceeds 95 percent until it drops under the threshold. If the space is manually increased or snapshots are deleted, the warning is reset and reissued if the threshold is exceeded again. If no actions are taken, this notification is the only warning that you receive.

## Increasing the amount of Snapshot space for a volume in the UI
{: #changesnapshotspaceUI}
{: ui}

You might need to add snapshot space to a volume that didn't previously have any or might require extra snapshot space.

Snapshot space can be increased. It can't be reduced. You can select a smaller amount of space until you determine how much space you need. Remember, automated, and manual snapshots share the space.
{: important}

Snapshot space is increased through **Storage** > **{{site.data.keyword.filestorage_short}}**.

1. Click your storage volume, click **Actions**, and click **Change Snapshot Space**.
2. Select from a range of sizes from the prompt. Sizes typically range from 0 to the size of your volume.
3. Click **Continue**.
4. Enter any Promo Code that you have, and click **Recalculate**. The Charges for this order and Order Review fields are completed by default.
5. Read the service agreement, and if you agree with the terms click check box, and click **Place Order**. Your additional snapshot space is provisioned in a few minutes.

## Deleting a snapshot schedule in the UI
{: #cancelnapshotscheduleUI}
{: ui}

Snapshot schedules can be deleted through **Storage** > **{{site.data.keyword.filestorage_short}}**.

1. Click on the volume ID to display its related information.
2. Click Snapshots.
3. Click the schedule to be deleted in the **Snapshot Schedules** frame.
4. Click the check box next to the schedule to be deleted and click **Save**.

If you're using the replication feature, be sure that the schedule that you're deleting isn't the schedule that is used by replication. For more information about deleting a replication schedule, see [here](/docs/FileStorage?topic=FileStorage-replication).
{: important}

## Deleting a snapshot schedule from the SLCLI
{: #cancelnapshotscheduleCLI}
{: cli}

You can accomplish this task by using the following command.
```python
# slcli file snapshot-disable --help
Usage: slcli file snapshot-disable [OPTIONS] VOLUME_ID

  Disables snapshots on the specified schedule for a given volume

Options:
  --schedule-type TEXT  Snapshot schedule [INTERVAL|HOURLY|DAILY|WEEKLY]
                        [required]
  -h, --help            Show this message and exit.
```

If you're using the replication feature, be sure that the schedule that you're deleting isn't the schedule that is used by replication. For more information about deleting a replication schedule, see [here](/docs/FileStorage?topic=FileStorage-replication).
{: important}


## Deleting a snapshot in the UI
{: #deletesnapshotUI}
{: ui}

Snapshots that are no longer needed can be manually removed to free up space for future snapshots. Deletion is done through **Storage** > **{{site.data.keyword.filestorage_short}}**.

1. Click your storage volume and click **Snapshot** to see the list of existing snapshots.
2. Click **Actions** next to a particular snapshot and click **Delete** to delete the snapshot. This deletion doesn't affect any future or past snapshots on the same schedule as there's no dependency between snapshots.

Manual snapshots that aren't deleted in the portal manually, are automatically deleted when you reach space limitations. The oldest snapshot is deleted first.
{: note}

## Deleting a snapshot from the SLCLI
{: #deletesnapshotCLI}
{: cli}

Snapshots that are no longer needed can be manually removed to free up space for future snapshots. You can delete a snapshot from the SLCLI by using the following command.

```python
# slcli file snapshot-delete --help
Usage: slcli file snapshot-delete [OPTIONS] SNAPSHOT_ID

Options:
  -h, --help  Show this message and exit.
```

Manual snapshots that aren't deleted in the portal manually, are automatically deleted when you reach space limitations. The oldest snapshot is deleted first.
{: note}

## Restoring storage volume to a specific point-in-time by using a snapshot in the UI
{: #restorefromsnapshotUI}
{: ui}

You might need to take your storage volume back to a specific point-in-time because of user-error or data corruption.

1. Unmount and detach your storage volume from the host.

   For more information about mounting and unmounting storage, see [connecting your new storage](/docs/FileStorage?topic=FileStorage-mountingLinux).
   {: tip}

2. Go to the [{{site.data.keyword.cloud}} console](https://{DomainName}/){: external}. From the menu, select **Classic Infrastructure**.
3. Click **Storage**, **{{site.data.keyword.filestorage_short}}**.
4. Scroll on the list, and click your volume to be restored. The **Snapshots** page displays the list of all saved snapshots along with their size and creation date.
5. Click **Actions** next to the snapshot to be used and click **Restore**.

   Completing the restore results in the loss of the data that was created or modified after the snapshot was taken. This data loss occurs because your storage volume returns to the same state it was in of the time of the snapshot.
   {: note}

6. Click **Yes** to start the restore. The restore is going to take a while, and your file share is locked during the restore.

   When you return to the file share list, note that a clock icon appeared next to your volume that indicates that an active transaction is in progress. Hovering over the icon produces a window that shows the transaction. The icon disappears when the transaction is complete.
   {: note}

7. Mount and reattach your storage volume to the host.
   - For more information about mounting and unmounting storage, see [connecting your new storage](/docs/FileStorage?topic=FileStorage-mountingLinux).

   Restoring a volume results in deleting all snapshots that were taken after the snapshot that was used for the restore.
   {: important}

## Restoring storage volume to a specific point-in-time by using a snapshot from the SLCLI
{: #restorefromsnapshotCLI}
{: cli}

You might need to take your storage volume back to a specific point-in-time because of user-error or data corruption. First, unmount your volume.

For more information about mounting and unmounting storage, see [connecting your new storage](/docs/FileStorage?topic=FileStorage-mountingLinux).
{: tip}

Then, you can restore the volume with a snapshot from the SLCLI by using the following command.
```python
# slcli file snapshot-restore --help
Usage: slcli file snapshot-restore [OPTIONS] VOLUME_ID

Options:
  -s, --snapshot-id TEXT  The id of the snapshot which will be used to restore
                          the block volume
  -h, --help              Show this message and exit.
```  

Lastly, mount and reattach your storage volume to the host.

Restoring a volume results in deleting all snapshots that were taken after the snapshot that was used for the restore.
{: important}
