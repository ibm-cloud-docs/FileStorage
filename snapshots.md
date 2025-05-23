---

copyright:
  years: 2014, 2025
lastupdated: "2025-03-21"

keywords: File Storage for Classic, NFS, snapshots

subcollection: FileStorage

---
{{site.data.keyword.attribute-definition-list}}

# Snapshots
{: #snapshots}

Snapshots are a feature of {{site.data.keyword.filestorage_full}}. A snapshot represents a volume's contents at a particular point in time. With snapshots, you can protect your data with no performance impact and minimal consumption of space. Snapshots are considered your first line of defense for data protection. If a user accidentally modifies or deletes crucial data from a volume, the data can be easily and quickly restored from a snapshot copy.
{: shortdesc}

{{site.data.keyword.filestorage_short}} provides you with two ways to take your snapshots.

* First, through a configurable snapshot schedule that creates and deletes snapshot copies automatically for each storage volume. You can also create extra snapshot schedules, manually delete copies, and manage schedules based on your requirements.
* The second way is to take a manual snapshot.

A snapshot copy is a read-only image of a {{site.data.keyword.filestorage_short}} volume that captures the state of the volume at a point in time. Snapshot copies are efficient both in the time that is needed to create them and in storage space. A {{site.data.keyword.filestorage_short}} snapshot copy takes only a few seconds to create. It's typically less than 1 second, regardless of the size of the volume or the level of activity on the storage. After a snapshot copy is created, changes to data objects are reflected in updates to the current version of the objects, as if Snapshot copies didn't exist. Meanwhile, the copy of the data remains stable.

A Snapshot copy incurs no performance decrease. Users can easily store up to 50 scheduled snapshots and 50 manual snapshots per {{site.data.keyword.filestorage_short}} volume, all of which are accessible as read-only and online versions of the data.

With snapshots, you can:

- Nondisruptively create point-in-time recovery points,
- Revert volumes to previous points-in-time.

You must purchase some amount of snapshot space for your volume first so you can take snapshots of it. The snapshot space can be added during the initial order or afterward through the **Volume Details** page. Scheduled and manual snapshots share the snapshot space, so make sure you order enough Snapshot space. See the [Ordering Snapshots](/docs/FileStorage?topic=FileStorage-ordering-snapshots) article for more details and guidance.

## Snapshot best practices
{: #snapshotbestpractice}

Snapshot design depends on the customer’s environment. The following design considerations can help you to plan and implement Snapshot copies:
- Up to 50 snapshots can be created through a schedule and up to 50 manually on each volume.
- Don't take more snapshots than what you need. Make sure that your scheduled snapshot frequency meets your RTO and RPO needs and your application business requirements by scheduling hourly, daily, or weekly snapshots.
- Snapshot AutoDelete can be used to control the growth of storage consumption.

   The AutoDelete threshold is fixed at 95%.
   {: note}

Snapshots are not replacements for actual off-site Disaster Recovery replication or long-retention backup.
{: important}

## Security
{: #snapshotsecurity}

All snapshots and replicas of encrypted {{site.data.keyword.filestorage_short}} are also encrypted by default. This feature can't be turned off on a volume basis. For more information about provider-managed encryption-at-rest, see [Securing your data](/docs/FileStorage?topic=FileStorage-mng-data).

## How Snapshots affect the disk space
{: #snapshotsdiskspace}

Snapshot copies minimize disk consumption by preserving individual blocks rather than whole files. Snapshot copies use extra space only when files in the active file system are changed or deleted.

In the active file system, the changed blocks are rewritten to different locations on the disk or removed as active file blocks entirely. When files are changed or deleted, the original file blocks are preserved as part of one or more Snapshot copies. As a result, disk space that is used by the original blocks is still reserved to reflect the status of the active file system before the change. This space is reserved in addition to the disk space that is used by blocks in the modified active file system.

| Disk space usage |   |
|-----|-----|
| ![The shaded quarter of the circle represents the space that is used before a snapshot copy is taken.](images/bfbeforesnapshot.svg "Before Snapshot Copy") | Before any Snapshot copy is created, disk space is used by the active file system only. |
| ![The space that is used when a snapshot copy is taken is displayed as the same size shaded quarter of the circle.](images/bfnewsnapshot.svg "After Snapshot Copy") | After a Snapshot copy is created, the active file system and Snapshot copy point to the same disk blocks. The Snapshot copy doesn't use extra disk space. |
| ![The shaded area of the circle shows the space that is used when a file is deleted after a snapshot copy was taken. It shows that the shaded area did not change because the snapshot is still holding that block where the file used to be.](images/bfdeletedsnapshot.svg "Changes after Snapshot Copy") | After the file `myfile.txt` is deleted from the active file system, the Snapshot copy still includes the file, and references its disk blocks. Thus, deleting active file system data doesn't always free up disk space. |
{: caption="The table shows how snapshots affect the space usage on the Storage." caption-side="bottom"}

For more information about snapshot space usage, see [Managing Snapshots](/docs/FileStorage?topic=FileStorage-managingSnapshots).
