---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-11"

keywords: File Storage, file storage, NFS, replication, duplication, synchronous, replica schedule, replica space, disaster recovery

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}


# Replicating Data
{: #replication}

Replication uses one of your snapshot schedules to automatically copy snapshots to a destination volume in a remote data center. The copies can be recovered in the remote site if a catastrophic event occurs or your data becomes corrupted.

Replication keeps your data in sync in two different locations. If you want to clone your volume and use it independently from the original volume, see [Creating a duplicate File Volume](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume).
{:tip}

Before you can replicate, you must create a snapshot schedule.
{:important}


## Determining the remote data center for the replicated storage volume

{{site.data.keyword.cloud}} data centers are paired into primary and remote combinations worldwide.
See Table 1 for the complete list of data center availability and replication targets.

| US 1 | US 2 | Latin America | Canada  | Europe  | Asia-Pacific  | Australia  |
|-----|-----|-----|-----|-----|-----|-----|
| DAL01<br />DAL05<br />DAL06<br />HOU02<br />SJC01<br />WDC01 | SJC03<br />SJC04<br />WDC04<br />WDC06<br />WDC07<br />DAL09<br />DAL10<br />DAL12<br />DAL13 | MEX01<br />SAO01 | TOR01<br />MON01 | AMS01<br />AMS03<br />FRA02<br />FRA04<br />FRA05<br />LON02<br />LON04<br />LON05<br />LON06<br />OSL01<br />PAR01<br />MIL01 | HKG02<br />TOK02<br />TOK04<br />TOK05<br />SNG01<br />SEO01<br />CHE01 | SYD01<br />SYD04<br />SYD05<br />MEL01 |
{: caption="Table 1 - This table shows the complete list of data centers with enhanced capabilities in each region. Every region is a separate column. Some cities, such as Dallas, San Jose, Washington DC, Amsterdam, Frankfurt, London, and Sydney have multiple data centers. Data centers in US 1 region do NOT have enhanced storage. Hosts in data centers with enhanced storage capabilities can't start replication with replica targets in US 1 data centers." caption-side="top"}


## Creating the initial replica

Replications work based on a snapshot schedule. You must first have snapshot space and a snapshot schedule for the source volume before you can replicate. If you try to set up replication and one or the other isn't in place, you are going to be prompted to purchase more space or set up a schedule. Replications are managed under **Storage** > **{{site.data.keyword.filestorage_short}}** in the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}.

1. Click your storage volume.
2. Click **Replica** and click **Purchase a replication**.
3. Select the existing snapshot schedule that you want your replication to follow. The list contains all of your active snapshot schedules. <br />

   You can select only one schedule even if you have a mix of hourly, daily, and weekly. All snapshots that were captured since the previous replication cycle, are replicated regardless of the schedule that originated them.<br />If you don't have Snapshots that are set up, you are prompted to do so before you can order replication. For more information, see [Working with Snapshots](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots).
   {:tip}
3. Click **Location**, and select the data center that is your DR site.
4. Click **Continue**.
5. Enter in a **Promo Code** if you have one, and click **Recalculate**. The other fields in the window are completed by default.

   Discounts are applied when the order is processed.
   {:note}
6. Click the **I have read the Master Service Agreement…** check box, and click **Place Order**.


## Editing an existing replication

You can edit your replication schedule, and change your replication space from either the **Primary** or **Replica** tab under **Storage** > **{{site.data.keyword.filestorage_short}}** from the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}.


## Editing the Replication Schedule

The replication schedule is based on an existing snapshot schedule. To change the replica schedule from Hourly to Daily or Weekly or vice versa, you must cancel the replica volume and set up a new one.

However, if you want to change the time of day when your **Daily** replication occurs, you can adjust the existing schedule on the Primary or Replica tab.

1. Click **Actions** on either the **Primary** or **Replica** tab.
2. Select **Edit Snapshot Schedule**.
3. Look in the **Snapshot** frame under **Schedule** to determine which schedule you're using for replication. Change the schedule that you want.
4. Click **Save**.


## Changing the Replication space

Your primary snapshot space and your replica space must be the same. If you change the space on the **Primary** or **Replica** tab, it automatically adds space to both your source and destination data centers. Increasing snapshot space triggers an immediate replication update also.

1. Click **Actions** on either the **Primary** or **Replica** tab.
2. Select **Add More Snapshot Space**.
3. Select the storage size from the list and click **Continue**.
4. Enter in a **Promo Code** if you have one and click **Recalculate**. The other fields in the dialog box are completed by default.
5. Click the **I have read the Master Service Agreement…** check box and click **Place Order**.


## Increasing the Snapshot space in the replica data center when Snapshot space is increased in the primary data center

Your volume sizes must be the same for your primary and replica storage volumes. One can't be larger than the other. When you increase your snapshot space for your primary volume, the replica space is automatically increased. Increasing snapshot space triggers an immediate replication update. The increase to both volumes shows as line items on your invoice and is prorated as necessary.

For more information about increasing snapshot space, see [Snapshots](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots).
## Viewing the replica volumes in the Volume List

You can view your replication volumes on the {{site.data.keyword.filestorage_short}} page under **Storage** > **{{site.data.keyword.filestorage_short}}**. The volume name shows the primary volume's name followed by REP. The **Type** is Endurance or Performance – Replica. The **Target Address** is N/A because the replica volume isn't mounted at the replica data center, and the **Status** shows Inactive.


## Viewing a replicated volume's details at the replica data center

You can view the replica volume details on the **Replica** tab under **Storage** > **{{site.data.keyword.filestorage_short}}**. Another option is to select the replica volume from the **{{site.data.keyword.filestorage_short}}** page and click the **Replica** tab.


## Viewing the replication history

Replication history is viewed on the **Audit Log** on the **Account** tab under **Manage**. Both the primary and replica volumes display identical replication histories, which include

- Type for replication (failover or failback),
- When it was initiated,
- Snapshot that was used for the replication,
- Size of the replication,
- When it completed.


## Creating a duplicate of a replica

You can create a duplicate of an existing {{site.data.keyword.cloud}} {{site.data.keyword.filestorage_full}}. The duplicate volume inherits the capacity and performance options of the original storage volume by default and has a copy of the data up to the point-in-time of a snapshot.

Duplicates can be created from both primary and replica volumes. The new duplicate is created in the same data center as the original volume. If you create a duplicate from a replica volume, the new volume is created in the same data center as the replica volume.

Duplicate volumes can be accessed by a host for read/write as soon as the storage is provisioned. However, snapshots and replication aren't allowed until the data copy from the original to the duplicate is complete.

For more information, see [Creating a duplicate File Volume](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume)

## Using replicas to fail over when disaster strikes

When you fail over, you’re "flipping the switch" from your storage volume in your primary data center to the destination volume in your remote data center. For example, your primary data center is London and your secondary data center is Amsterdam. If a failure event occurs, you’d fail over to Amsterdam – connecting to the now-primary volume from a compute instance in Amsterdam. After your volume in London is repaired, a snapshot is taken of the Amsterdam volume to fail back to London and the once-again primary volume from a compute instance in London.

* If the primary location is experiencing problem but the storage and host are still online, see [Fail over with an accessible Primary volume](/docs/infrastructure/FileStorage?topic=FileStorage-dr-accessible).
* If the primary location is down, see [Fail over with an inaccessible Primary volume](/docs/infrastructure/FileStorage?topic=FileStorage-dr-inaccessible).

## Canceling an existing replication

You can cancel replication either immediately or on the anniversary date, which causes billing to end. Replication can be canceled from either the **Primary** or **Replica** tabs.

1. Click the volume from the **{{site.data.keyword.filestorage_short}}** page.
2. Click **Actions** on either the **Primary** or **Replica** tab.
3. Select **Cancel Replica**.
4. Select when to cancel. Choose **Immediately** or **Anniversary Date**, and click **Continue**.
5. Click **I acknowledge that due to cancellation, data loss may occur**, and click **Cancel Replica**.


## Canceling replication when the primary volume is canceled

When a primary volume is canceled, the replication schedule and the volume in the replica data center are deleted. Replicas are canceled from the **{{site.data.keyword.filestorage_short}}** page.

 1. Highlight your volume on the **{{site.data.keyword.filestorage_short}}** page.
 2. Click **Actions** and select **Cancel for {{site.data.keyword.filestorage_short}}**.
 3. Select when to cancel the volume. Choose **Immediately** or **Anniversary Date**, and click **Continue**.
 4. Click **I acknowledge that due to cancellation, data loss may occur**, and click **Cancel**.

## Replication related commands in SLCLI
{: #clicommands}

* List suitable replication datacenters for a specific volume.
  ```
  # slcli file replica-locations --help
  Usage: slcli file replica-locations [OPTIONS] VOLUME_ID

  Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: ID, Long Name, Short Name
  -h, --help      Show this message and exit.
  ```

* Order a file storage replica volume.
  ```
  # slcli file replica-order --help
  Usage: slcli file replica-order [OPTIONS] VOLUME_ID

  Options:
  -s, --snapshot-schedule [INTERVAL|HOURLY|DAILY|WEEKLY]
                                  Snapshot schedule to use for replication,
                                  (INTERVAL | HOURLY | DAILY | WEEKLY)
                                  [required]
  -l, --location TEXT             Short name of the data center for the
                                  replicant (e.g.: dal09)  [required]
  --tier [0.25|2|4|10]            Endurance Storage Tier (IOPS per GB) of the
                                  primary volume for which a replicant is
                                  ordered [optional]
  -h, --help                      Show this message and exit.
  ```

* List existing replicant volumes for a file volume.
  ```
  # slcli file replica-partners --help
  Usage: slcli file replica-partners [OPTIONS] VOLUME_ID

  Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: ID, Username, Account ID,
                  Capacity (GB), Hardware ID, Guest ID, Host ID
  -h, --help      Show this message and exit.
  ```

* Failover a file volume to a specific replicant volume.
  ```
  # slcli file replica-failover --help
  Usage: slcli file replica-failover [OPTIONS] VOLUME_ID

  Options:
  --replicant-id TEXT  ID of the replicant volume
  --immediate          Failover to replicant immediately.
  -h, --help           Show this message and exit.
  ```

* Failback a file volume from a specific replicant volume.
  ```
  # slcli file replica-failback --help
  Usage: slcli file replica-failback [OPTIONS] VOLUME_ID

  Options:
  --replicant-id TEXT  ID of the replicant volume
  -h, --help           Show this message and exit.
  ```
