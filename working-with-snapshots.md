---

copyright:
  years: 2014, 2024
lastupdated: "2023-10-18"

keywords: File Storage, NFS, snapshots, snapshot schedule, manual snapshot, snapshot space, snapshot quota

subcollection: FileStorage

---
{{site.data.keyword.attribute-definition-list}}

# Managing Snapshots
{: #managingSnapshots}

Snapshots are a feature of {{site.data.keyword.blockstoragefull}}. A snapshot represents a volume's contents at a particular point in time. With snapshots, you can protect your data with no performance impact and minimal consumption of space. Learn more about how to manage snapshots by reading the following instructions.
{: shortdesc}

## Adding a Snapshot schedule in the UI
{: #addscheduleUI}
{: ui}

You can decide how often and when you want to create a point in time reference of your storage volume with Snapshot schedules. You can have a maximum of 50 snapshots per storage volume. Schedules are managed through the **Storage** > **{{site.data.keyword.filestorage_short}}** tab of the [{{site.data.keyword.cloud}} console](/login){: external}.

Before you can set up your initial schedule, you must first purchase snapshot space if you didn't purchase it during the initial provisioning of the storage volume.
{: important}

Snapshots schedules can be set up for hourly, daily, and weekly intervals, each with a distinct retention cycle. The maximum limit of snapshots is 50 per storage volume, which can be a mix of hourly, daily, and weekly schedules, and manual snapshots.

1. Click your storage volume to view its details.
1. Click the **Snapshots** in the side navigation.
2. In the Snapshot schedule panel, click **Edit +**. You can select from three different snapshot frequencies. Use any combination of the three to create a comprehensive snapshot schedule.
   - Hourly
      - Specify the minute each hour that a snapshot is to be taken. The default is the current minute.
      - Specify the number of hourly snapshots to be retained before the oldest is discarded.
   - Daily
      - Specify the hour and minute that a snapshot is to be taken. The default is the current hour and minute.
      - Specify the number of hourly snapshots to be retained before the oldest is discarded.
   - Weekly
      - Specify the day of the week, hour, and minute that a snapshot is to be taken. The default is the current day, hour, and minute.
      - Specify the number of weekly snapshots to be retained before the oldest is discarded.
3. Click **Save**. If the total number of scheduled snapshots is over 50, you receive a warning message and can't save.

The list of the snapshots is displayed as they're taken in the **Snapshots** section of the **Detail** page.

## Adding a Snapshot schedule from the CLI
{: #addscheduleCLI}
{: cli}

You can decide how often and when you want to create a point in time reference of your storage volume with Snapshot schedules. You can have a maximum of 50 snapshots per storage volume.

Before you can set up your initial schedule, you must first purchase snapshot space if you didn't purchase it during the initial provisioning of the storage volume.
{: important}

Before you begin, decide on the CLI client that you want to use.

* You can either install the [IBM Cloud CLI](/docs/cli){: external} and install the SL plug-in with `ibmcloud plugin install sl`. For more information, see [Extending IBM Cloud CLI with plug-ins](/docs/cli?topic=cli-plug-ins).
* Or, you can install the [SLCLI](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.

### Adding a schedule from the IBMCLOUD CLI
{: #addscheduleICCLI}

Use the `ibmcloud sl file snapshot-enable` command to create a snapshot schedule. The following example creates a weekly schedule to take snapshots on every Sunday at 2:00 AM. In this example, up to 5 snapshots are retained.

```sh
$ ibmcloud sl file snapshot-enable 560156918 -s WEEKLY -c 5 -m 0 --hour 2 -d 0
OK
WEEKLY snapshots have been enabled for volume 560156918.
```
{: pre}

For more information about all of the parameters that are available for this command, see [ibmcloud sl file snapshot-enable](/docs/cli?topic=cli-sl-file-storage-service#sl_file_snapshot_enable){: external}.

### Adding a schedule from the SLCLI
{: #addscheduleSLCLI}

To create a snapshot schedule, use the following command.

```sh
$ slcli file snapshot-enable --help
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

You can see the list of your snapshot schedules from the CLI with the following command.
```sh
$ slcli file snapshot-schedule-list --help
Usage: slcli file snapshot-schedule-list [OPTIONS] VOLUME_ID

  Lists snapshot schedules for a given volume

Options:
  -h, --help  Show this message and exit.
```

## Managing a Snapshot schedule with Terraform
{: #addscheduleTerraform}
{: terraform}

To set up a snapshot schedule, use the `ibm_storage_file` resource and specify information in the `snapshot_schedule` argument. The following example defines two different schedules. One schedule is for weekly snapshots that are taken on Sundays at 1:20 PM. 20 snapshots are kept before the oldest one is deleted to make space for a new one. The second schedule is for hourly snapshots.

```terraform
resource "ibm_storage_file" "fs_endurance" {
  type       = "Endurance"
  datacenter = "dal09"
  capacity   = 20
  iops       = 0.25

  # Optional fields
  allowed_virtual_guest_ids = ["28961689"]
  allowed_subnets           = ["10.146.139.64/26"]
  allowed_ip_addresses      = ["10.146.139.84"]
  snapshot_capacity         = 10
  hourly_billing            = true

  # Optional fields for snapshot
  snapshot_schedule {
    schedule_type   = "WEEKLY"
    retention_count = 20
    minute          = 20
    hour            = 13
    day_of_week     = "SUNDAY"
    enable          = true
  }
  snapshot_schedule {
    schedule_type   = "HOURLY"
    retention_count = 20
    minute          = 2
    enable          = true
  }

}
```
{: codeblock}

If you want to update the schedule, change these values and apply them to your resources. If you want to delete the schedule, remove its details from the `ibm_storage_file` resource definition, and apply your changes.

For more information about the arguments and attributes, see [ibm_storage_file](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/storage_file){: external}.

## Taking a manual Snapshot in the UI
{: #takemanualsnapshotUI}
{: ui}

Manual snapshots can be taken at various points during an application upgrade or maintenance. You can also take snapshots across multiple servers that were temporarily deactivated at the application level.

The maximum limit of manual snapshots per storage volume is 50.

1. Click your storage volume.
2. Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions").
3. Click **Take Manual Snapshot**.

The snapshot is taken and displayed in the **Snapshots** section of the **Detail** page. Its schedule appears Manual.

## Taking a manual Snapshot from the CLI
{: #takemanualsnapshotCLI}
{: cli}

### Taking a manual Snapshot from the IBMCLOUD CLI
{: #takemanualsnapshotICCLI}

Use the `ibmcloud sl file snapshot-create` command to create a snapshot of a specific file share.

```sh
ibmcloud sl file snapshot-create 12345678
```
{: pre}

For more information about all of the parameters that are available for this command, see [ibmcloud sl file snapshot-create](/docs/cli?topic=cli-sl-file-storage-service#sl_file_snapshot_create){: external}.

### Taking a manual Snapshot from the SLCLI
{: #takemanualsnapshotSLCLI}

You can use the following command to create a snapshot from the CLI.

```sh
$ slcli file snapshot-create --help
Usage: slcli file snapshot-create [OPTIONS] VOLUME_ID

Options:
  -n, --notes TEXT  Notes to set on the new snapshot
  -h, --help        Show this message and exit.
```

## Listing all Snapshots with usage information and management functions in the UI
{: #listsnapshotUI}
{: ui}

A list of retained snapshots and space that is used can be seen on the **{{site.data.keyword.filestorage_short}} Detail** page. Management functions (editing schedules and adding more space) are conducted on the **{{site.data.keyword.filestorage_short}} Detail** page by using the **Actions** menu or links in the various sections on the page. The Snapshot page displays how much capacity the volume has and how much of it is used.

You receive notifications when you reach space thresholds – 75 percent, 90 percent, and 95 percent.

- At **75 percent capacity**, a warning is sent that snapshot space usage exceeded 75 percent. To remediate this situation, you can manually add space, or delete retained unnecessary snapshots. You can reduce the number of retained snapshots in the schedule. If you reduce the snapshot data or increase the space, the warning system is reset, and no autodeletion occurs.
- At **90 percent capacity**, a second warning is sent when snapshot space usage exceeded 90 percent. Like with reaching 75 percent capacity, if you take the necessary actions to decrease the snapshot data or increase the space, the warning system is reset and no autodeletion occurs.
- At **95 percent capacity**, a final warning is sent. If no action is taken to bring your space usage under the threshold, automatic deletion starts so that future snapshots can be created. Scheduled snapshots are deleted, starting with the oldest, until usage drops under 95 percent. Snapshots continue to be deleted each time usage exceeds 95 percent until it drops under the threshold. If the space is manually increased or snapshots are manually deleted, the warning is reset, and reissued if the threshold is exceeded again. If no actions are taken, this notification is the only warning that you receive.

By default, snapshot warning notifications are enabled for every customer. However, you can choose to disable them. When this feature is disabled, all ticket generation and notifications are stopped. You can disable and enable notifications for the volume at any time from the CLI.

If snapshot space usage increases too rapidly, then you might receive one notification before autodeletion of the oldest scheduled snapshot occurs. For example, if usage jumps from 76% to 96% within 15 minutes, you receive one notification about exceeding 75% and one notification about exceeding 95%.
{: note}

## Listing all Snapshots with usage information and management functions from the CLI
{: #listsnapshotCLI}
{: cli}

### Listing all Snapshots from the IBMCLOUD CLI
{: #listsnapshotICCLI}

Use the `ibmcloud sl file snapshot-list` command to list the snapshots of a specific file share.

```sh
ibmcloud sl file snapshot-list 12345678 --sortby id
```
{: pre}

For more information about all of the parameters that are available for this command, see [ibmcloud sl file snapshot-list](/docs/cli?topic=cli-sl-file-storage-service#sl_file_snapshot_list){: external}.

### Listing all Snapshots from the SLCLI
{: #listsnapshotSLCLI}

You can accomplish this task from the CLI by using the following command.

```sh
$ slcli file snapshot-list --help
Usage: slcli file snapshot-list [OPTIONS] VOLUME_ID

Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: id, name, created, size_bytes
  -h, --help      Show this message and exit.
```

## Checking notification status from the CLI
{: #checknotificationstatusCLI}
{: cli}

Notifications are sent when you reach three different space thresholds – 75 percent, 90 percent, and 95 percent.

- At **75 percent capacity**, a warning is sent that snapshot space usage exceeded 75 percent. To remediate this situation, you can manually add space, or delete retained unnecessary snapshots. You can reduce the number of retained snapshots in the schedule. If you reduce the snapshot data or increase the space, the warning system is reset, and no autodeletion occurs.
- At **90 percent capacity**, a second warning is sent when snapshot space usage exceeded 90 percent. Like with reaching 75 percent capacity, if you take the necessary actions to decrease the snapshot data or increase the space, the warning system is reset and no autodeletion occurs.
- At **95 percent capacity**, a final warning is sent. If no action is taken to bring your space usage under the threshold, automatic deletion starts so that future snapshots can be created. Scheduled snapshots are deleted, starting with the oldest, until usage drops under 95 percent. Snapshots continue to be deleted each time usage exceeds 95 percent until it drops under the threshold. If the space is manually increased or snapshots are manually deleted, the warning is reset, and reissued if the threshold is exceeded again. If no actions are taken, this notification is the only warning that you receive.

If snapshot space usage increases too rapidly, then you might receive one notification before autodeletion of the oldest scheduled snapshot occurs. For example, if usage jumps from 76% to 96% within 15 minutes, you receive one notification about exceeding 75% and one notification about exceeding 95%. The system skips the 90%-exceeded warning.
{: note}

By default, snapshot warning notifications are enabled for every customer. However, you can choose to disable them. When this feature is disabled, all ticket generation and notifications are stopped. You can disable and enable notifications for the volume at any time.

### Checking whether notifications are enabled from the IBMCLOUD CLI
{: #checknotificationstatusICCLI}

Use the `ibmcloud sl file snapshot-get-notification-status` command to check the status of the notifications. The following example checks whether notifications are enabled for the file share `12345678`. If the response is `0`, the notifications are disabled. If the response is `1`, the notifications are enabled.

```sh
ibmcloud sl file snapshot-get-notification-status 12345678
```
{: pre}

For more information about all of the parameters that are available for this command, see [ibmcloud sl file snapshot-get-notification-status](/docs/cli?topic=cli-sl-file-storage-service#sl_file_snapshot_get_notification_status){: external}.

To change the status of the notifications, use the command `ibmcloud sl file snapshot-set-notification`. The following example disables the notifications for the file share `12345678`.

```sh
ibmcloud sl file snapshot-set-notification 12345678 --disable
```
{: pre}

For more information about all of the parameters that are available for this command, see [ibmcloud sl file snapshot-get-notification-status](/docs/cli?topic=cli-sl-file-storage-service#sl_file_snapshot_set_notification){: external}.

### Checking whether notifications are enabled from the SLCLI
{: #checknotificationstatusSLCLI}

To check whether the notifications are enabled for the storage volume, use the following command.

```sh
$ slcli file snapshot-get-notification-status
Usage: slcli file snapshot-get-notification-status [OPTIONS] VOLUME_ID
  Get snapshots space usage threshold warning flag setting for a given volume

Options:
  -h, --help  Show this message and exit.
```

To change the status of the notification setting, use the following command.
```sh
$ slcli file snapshot-set-notification VOLUME_ID
Usage: slcli file snapshot-set-notification VOLUME_ID [OPTIONS]

Options:
 --disable  Disable snapshot threshold warning notification for the storage volume
 --enable   Enable snapshot threshold warning notification for the storage volume
 -h, --help  Show this message and exit.
```

## Increasing the amount of Snapshot space for a volume in the UI
{: #changesnapshotspaceUI}
{: ui}

You might need to add snapshot space to a volume that didn't previously have any or might require extra snapshot space.

Snapshot space can be increased. It can't be reduced. You can select a smaller amount of space until you determine how much space you need.
{: important}

Snapshot space is increased through **Storage** > **{{site.data.keyword.filestorage_short}}**.

1. Click your storage volume, click **Actions**, and click **Change Snapshot Space**.
2. Select from a range of sizes from the prompt. Sizes typically range from 0 to the size of your volume.
3. Click **Continue**.
4. Enter any Promo Code that you have, and click **Recalculate**. The Charges for this order and Order Review fields are completed by default.
5. Read the service agreement, and if you agree with the terms click checkbox, and click **Place Order**. Your additional snapshot space is provisioned in a few minutes.

## Deleting a snapshot schedule in the UI
{: #cancelnapshotscheduleUI}
{: ui}

Snapshot schedules can be deleted through **Storage** > **{{site.data.keyword.filestorage_short}}**.

1. Click the volume ID to display its related information.
2. Click Snapshots.
3. Click the schedule to be deleted in the **Snapshot Schedules** frame.
4. Click the checkbox next to the schedule to be deleted and click **Save**.

If you're using the replication feature, be sure that the schedule that you're deleting isn't the schedule that is used by replication. For more information about deleting a replication schedule, see [here](/docs/FileStorage?topic=FileStorage-replication).
{: important}

## Deleting a snapshot schedule from the CLI
{: #cancelnapshotscheduleCLI}
{: cli}

If you're using the replication feature, ensure that the schedule that you're deleting isn't the schedule that is used by replication. For more information about deleting a replication schedule, see [here](/docs/FileStorage?topic=FileStorage-replication).
{: important}

### Deleting a schedule from the IBMCLOUD CLI
{: #cancelnapshotscheduleICCLI}

Use the `ibmcloud sl file snapshot-disable` command to remove a snapshot schedule. The following example disables daily snapshots of the file share `12345678`.

```sh
ibmcloud sl file snapshot-disable 12345678 -s DAILY
```
{: pre}

For more information about all of the parameters that are available for this command, see [ibmcloud sl file snapshot-disable](/docs/cli?topic=cli-sl-file-storage-service#sl_file_snapshot_disable){: external}.

### Deleting a schedule from the SLCLI
{: #cancelnapshotscheduleSLCLI}

You can accomplish this task by using the following command.
```sh
$ slcli file snapshot-disable --help
Usage: slcli file snapshot-disable [OPTIONS] VOLUME_ID

  Disables snapshots on the specified schedule for a given volume

Options:
  --schedule-type TEXT  Snapshot schedule [INTERVAL|HOURLY|DAILY|WEEKLY]
                        [required]
  -h, --help            Show this message and exit.
```

## Deleting a snapshot in the UI
{: #deletesnapshotUI}
{: ui}

Snapshots that are no longer needed can be manually removed to free up space for future snapshots. Deletion is done through **Storage** > **{{site.data.keyword.filestorage_short}}**.

1. Click your storage volume and click **Snapshot** to see the list of existing snapshots.
2. Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions") next to a particular snapshot and click **Delete** to delete the snapshot. This deletion doesn't affect any future or past snapshots on the same schedule as snapshots don't depend on each other.

Manual snapshots that aren't deleted in the portal manually, are automatically deleted when you reach space limitations. The oldest snapshot is deleted first.
{: note}

## Deleting a snapshot from the CLI
{: #deletesnapshotCLI}
{: cli}

Snapshots that are no longer needed can be manually removed to free up space for future snapshots.

Manual snapshots that aren't deleted in the portal manually, are automatically deleted when you reach space limitations. The oldest snapshot is deleted first.
{: note}

### Deleting a snapshot from the IBMCLOUD CLI
{: #deletesnapshotICCLI}

Use the `ibmcloud sl file ssnapshot-delete` command to delete a snapshot. The following example deleted the snapshot `12345678`.

```sh
ibmcloud sl file snapshot-delete 12345678
```
{: pre}

For more information about all of the parameters that are available for this command, see [ibmcloud sl file snapshot-delete](/docs/cli?topic=cli-sl-file-storage-service#sl_file_snapshot_delete){: external}.

### Deleting a snapshot from the SLCLI
{: #deletesnapshotSLCLI}

You can delete a snapshot from the CLI by using the following command.

```sh
$ slcli file snapshot-delete --help
Usage: slcli file snapshot-delete [OPTIONS] SNAPSHOT_ID

Options:
  -h, --help  Show this message and exit.
```

## Deleting a snapshot with Terraform
{: #deletesnapshotTerraform}
{: terraform}

Use the `terraform destroy` command to conveniently destroy a remote object such as a snapshot. The following example destroys the snapshot that is identified by its ID `ibm_file_share_snapshot.example.id`.

```terraform
terraform destroy --target ibm_file_share_snapshot.example.id
```
{: codeblock}

For more information, see [terraform destroy](https://developer.hashicorp.com/terraform/cli/commands/destroy){: external}.

## Restoring storage volume to a specific point in time by using a snapshot in the UI
{: #restorefromsnapshotUI}
{: ui}

You might need to take your storage volume back to a specific point in time because of user-error or data corruption.

1. Unmount and detach your storage volume from the host.

   For more information about mounting and unmounting storage, see [connecting your new storage](/docs/FileStorage?topic=FileStorage-mountingLinux).
   {: tip}

2. Go to the [{{site.data.keyword.cloud}} console](/login){: external}. From the menu, select **Classic Infrastructure** ![Classic icon](../icons/classic.svg "Classic").
3. Click **Storage**, **{{site.data.keyword.filestorage_short}}**.
4. Scroll on the list, and click your volume to be restored. The **Snapshots** page displays the list of all saved snapshots along with their size and creation date.
5. Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions") next to the snapshot to be used and click **Restore**.

   Completing the restore results in the loss of the data that was created or modified after the snapshot was taken. This data loss occurs because your storage volume returns to the same state that it was in of the time of the snapshot.
   {: note}

6. Click **Yes** to start the restore. The restore is going to take a while, and your file share is locked during the restore.

   When you return to the file share list, a clock icon appears next to your volume that indicates that an active transaction is in progress. Hovering over the icon produces a window that shows the transaction. The icon disappears when the transaction is complete.
   {: note}

7. Mount and reattach your storage volume to the host.
   - For more information about mounting and unmounting storage, see [connecting your new storage](/docs/FileStorage?topic=FileStorage-mountingLinux).

   Restoring a volume results in deleting all snapshots that were taken after the snapshot that was used for the restore.
   {: important}

## Restoring storage volume to a specific point in time by using a snapshot from the CLI
{: #restorefromsnapshotCLI}
{: cli}

You might need to take your storage volume back to a specific point in time because of user-error or data corruption. 

1. First, unmount your volume.
1. Then, you can restore the volume with a snapshot from the CLI.
1. Lastly, mount and reattach your storage volume to the host.

For more information about mounting and unmounting storage, see [connecting your new storage](/docs/FileStorage?topic=FileStorage-mountingLinux).

Restoring a volume results in deleting all snapshots that were taken after the snapshot that was used for the restore.
{: important}

### Restoring storage volume by using a snapshot from the IBMCLOUD CLI
{: #restorefromsnapshotICCLI}

Use the `ibmcloud sl file snapshot-restore` command to return your file share to a previous state. The following example restores the volume with ID 12345678 from the snapshot with ID 87654321.

```sh
ibmcloud sl file snapshot-restore 12345678 87654321
```
{: codeblock}

For more information about all of the parameters that are available for this command, see [ibmcloud sl file snapshot-restore](/docs/cli?topic=cli-sl-file-storage-service#sl_file_snapshot_restore){: external}.

### Restoring storage volume by using a snapshot from the SLCLI
{: #restorefromsnapshotSLCLI}

You can restore the volume with a snapshot from the CLI by using the following command.
```sh
$ slcli file snapshot-restore --help
Usage: slcli file snapshot-restore [OPTIONS] VOLUME_ID

Options:
  -s, --snapshot-id TEXT  The id of the snapshot which will be used to restore
                          the block volume
  -h, --help              Show this message and exit.
```
