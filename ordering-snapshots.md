---

copyright:
  years: 2014, 2022
lastupdated: "2022-07-21"

keywords: File Storage, file storage, NFS, snapshot, ordering snapshot, snapshot space

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:shortdesc: .shortdesc}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}


# Ordering Snapshots
{: #ordering-snapshots}

To create snapshots of your storage volume, either automated or manually, you need to purchase space to hold them. You can purchase snapshot capacity during the initial volume purchase or later by using these steps.
{: shortdesc}

## Determining how much snapshot space to order
{: #preorderdetermination}

Generically speaking, snapshot space is used by snapshots based on two key factors:
- How much your active file system changes over time,
- How long you plan to retain snapshots.

The way to calculate the amount of space that you need is **(Rate of Change)** x **(number of hours/days/weeks/months data is retained)**.

The first snapshot uses a negligible amount of space as it's just a copy of the metadata (pointers) that indicates the active file system blocks.
{: note}

A volume with numerous changes and a lengthy retention period needs more space than a volume with moderate change and a moderate retention schedule. An example for the first type is a high change rate database. An example for the second type is a VMware datastore.

If you take 12 hourly snapshots of 500 GB of actual data, and there's 1 percent of change between each snapshot, you end up with 60 GB for snapshots.

    *(5-GB Rate of Change) x (12 hourly snapshots) = (60 GB of used space)*

Conversely, if that 500 GB of actual data, with 12 hourly snapshots, saw 10 percent of change every hour, the snapshot space that is used is 600 GB.

    *(50-GB Rate of Change) x (12 hourly snapshots) = (600 GB of used space)*

So when you determine how much Snapshot space you need, consider the rate of change carefully. It's a huge influence on how much snapshot space you need. A bigger volume is more likely to change more often. However, a 500-GB volume with 5 GB of change and a 10-TB volume with 5 GB of change use the same amount of snapshot space.

Additionally, for most workloads, the larger a volume is the less space needs to be set aside initially. It's primarily due to the underlying data efficiencies, and the nature of how snapshots work in the environment.

## Ordering Snapshot space in the UI
{: #ordersnapshotUI}
{: ui}

1. Log in to the [{{site.data.keyword.cloud}} console](/login){: external} and click the menu icon ![Menu icon](../icons/icon_hamburger.svg "Menu").
2. Select **Classic Infrastructure** ![Classic icon](../icons/classic.svg "Classic").
3. Access your Storage through **Storage** > **{{site.data.keyword.filestorage_short}}**.
4. Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions"), then click **Add Snapshot Space**.
5. Select the amount of space that you need and the payment method.
6. Click **Continue**.
7. Enter any Promo Codes that you have and click **Recalculate**. The **Charges for this order** and **Order Review** have default values.

   Discounts are applied when the order is processed.
   {: note}

8. Check the **I have read the Service Description and agree to the terms therein** box and click **Place Order**. Your snapshot space is provisioned in a few minutes.

## Ordering Snapshot space from the SLCLI
{: #ordersnapshotCLI}
{: cli}

```python
# slcli file snapshot-order --help
Usage: slcli file snapshot-order [OPTIONS] VOLUME_ID

Options:
  --capacity INTEGER    Size of snapshot space to create in GB  [required]
  --tier [0.25|2|4|10]  Endurance Storage Tier (IOPS per GB) of the file
                        volume for which space is ordered [optional, and only
                        valid for endurance storage volumes]
  --upgrade             Flag to indicate that the order is an upgrade
  -h, --help            Show this message and exit.
```

For more information about the same function in the IBM Cloud CLI, see [bmcloud sl file snapshot-order](https://cloud.ibm.com/docs/cli?topic=cli-sl-file-storage-service#sl_file_snapshot_order){: external}.
{: tip}
