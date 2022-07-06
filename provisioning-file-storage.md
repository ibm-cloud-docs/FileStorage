---

copyright:
  years: 2014, 2022
lastupdated: "2022-02-15"

keywords: File Storage, NFS, provisioning, ordering, duplicate, cloning, replication

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
{: uilinked}


# Ordering {{site.data.keyword.filestorage_short}}
{: #orderingFileStorage}

You can provision {{site.data.keyword.filestorage_short}} and fine-tune to meet your capacity and IOPS needs. Get the most out of your storage with two options for specifying performance.
{: shortdesc}

- You can choose from Endurance IOPs tiers that feature pre-defined performance levels to fit workloads that don't have well-defined performance requirements.
- You can fine-tune your storage to meet specific performance requirements by specifying the total number of IOPS with Performance.

## Ordering {{site.data.keyword.filestorage_short}} in the UI
{: #orderingFileStorageUI}
{: ui}

1. Log in to the [{{site.data.keyword.cloud}} catalog](https://{DomainName}/catalog){: external} and click **Storage**. Then, select {{site.data.keyword.filestorage_short}}. Click **Create**.
2. Select your deployment location (region, location, zone).
   - Ensure that the new Storage is added in the same location as the compute host or hosts that you have.
3. Billing. If you selected a data center with improved capabilities (marked with an asterisk), you can choose between Monthly or Hourly Billing.
   - With **hourly** billing, the number of hours that the file volume existed on the account is calculated at the time the volume is deleted or at the end of the billing cycle. Which ever comes first. Hourly billing is a good choice for storage that is used for a few days or less than a full month. Hourly billing is only available for storage that is provisioned in these [select data centers](/docs/FileStorage?topic=FileStorage-selectDC).
   - With **monthly** billing, the calculation for the price is pro-rated from the date of creation to the end of the billing cycle and billed immediately. There's no refund if a file volume is deleted before the end of the billing cycle. Monthly billing is a good choice for storage that is used in production workloads that use data that needs to be stored and accessed for long periods of time (month or longer).

    Monthly billing type is used by default for storage that is provisioned in data centers that are **not** updated with improved capabilities.
    {: note}

4. Enter your storage size in the **Size** field.
5. Select the size of the Snapshot space from the list.

   This space is in addition to your usable space. For snapshot space considerations and recommendation, read [Ordering Snapshots](/docs/BlockStorage?topic=BlockStorage-orderingsnapshots).
   {: tip}

6. Select your IOPS profile. You can choose between the predefined values of **Endurance (Tiers)** or enter your custom IOPS value for **Performance**.
    - **0.25 IOPS per GB** is designed for workloads with low I/O intensity. These workloads are typically characterized by having a large percentage of data inactive at a time. Example applications include storing mailboxes or departmental level file shares.
    - **2 IOPS per GB** is designed for most general-purpose usage. Example applications include hosting small databases that are backing web applications or virtual machine disk images for a hypervisor.
    - **4 IOPS per GB** is designed for higher-intensity workloads. These workloads are typically characterized by having a high percentage of data active at a time. Example applications include transactional and other performance-sensitive databases.
    - **10 IOPS per GB** is designed for the most demanding workloads such as those created by NoSQL databases, and data processing for Analytics. This tier is available in [select data centers](/docs/FileStorage?topic=FileStorage-selectDC) for storage that is provisioned up to 4 TB.
7. On the right, review your order summary, and apply your Promo Code if you have one.

   Discounts are applied when the order is processed.
   {: note}

8. After you reviewed the terms and conditions, check the I** have read and agree to the...** box.
9. Click **Create**. Your new storage allocation is available in a few minutes.

By default, you can provision a combined total of 750 {{site.data.keyword.filestorage_short}} volumes. To increase the number of your volumes, contact your sales representative. Read about increasing limits [here](/docs/FileStorage?topic=FileStorage-managinglimits). For more information about the limit on simultaneous authorizations, see the [FAQs](/docs/FileStorage?topic=FileStorage-file-storage-faqs#authlimit).
{: tip}

## Ordering {{site.data.keyword.filestorage_short}} through the SLCLI
{: #orderingthroughCLI}
{: cli}

You can use the SLCLI to place orders for products that are normally ordered through the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.

Each order must have an associated location (data center). When you order {{site.data.keyword.filestorage_short}}, make sure that it is provisioned in the same location as your compute instances.
{: important}

For more information about how to install and use the SLCLI, see [Python CLI Client](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.
{: tip}

Use the `slcli file volume-order` command to provision the file share volume.

```python
# slcli file volume-order --help
Usage: slcli file volume-order [OPTIONS]

  Order a file storage volume.

Options:
  --storage-type [performance|endurance]
                                  Type of file storage volume  [required]
  --size INTEGER                  Size of file storage volume in GB
                                  [required]
  --iops INTEGER                  Performance Storage IOPs, between 100 and
                                  6000 in multiples of 100  [required for
                                  storage-type performance]
  --tier [0.25|2|4|10]            Endurance Storage Tier (IOP per GB)
                                  [required for storage-type endurance]
  --location TEXT                 Datacenter short name (e.g.: dal09)
                                  [required]
  --snapshot-size INTEGER         Optional parameter for ordering snapshot
                                  space along with endurance file storage;
                                  specifies the size (in GB) of snapshot space
                                  to order
  --service-offering [storage_as_a_service|enterprise|performance]
                                  The service offering package to use for
                                  placing the order [optional, default is
                                  'storage_as_a_service']
  --billing [hourly|monthly]      Optional parameter for Billing rate (default
                                  to monthly)
  -h, --help                      Show this message and exit.
```

### Example order
{: #exampleorder}
{: cli}

The following example shows how to order a 10-GB {{site.data.keyword.filestorage_short}} volume with 100 IOPS per GB.

```python
# slcli file volume-order --storage-type performance --size 20 --location dal10 --iops 100
Order #32076317 placed successfully!
> Storage as a Service
> File Storage
> 20 GBs
> 100 IOPS
```

For more information about ordering through the IBM Cloud CLI, see [Working with the File Storage service (ibmcloud sl file)](https://cloud.ibm.com/docs/cli?topic=cli-sl-file-storage-service#sl_file_volume_order){: external}.
{: tip}

By default, you can provision a combined total of 750 {{site.data.keyword.filestorage_short}} volumes. To increase the number of your volumes, contact your sales representative. Read about increasing limits [here](/docs/FileStorage?topic=FileStorage-managinglimits). For more information about the limit on simultaneous authorizations, see the [FAQs](/docs/FileStorage?topic=FileStorage-file-storage-faqs#authlimit).
{: important}


## Ordering {{site.data.keyword.filestorage_short}} by using the API
{: #orderingthroughAPI}
{: api}

The method `order_file_volume` (storage_type, location, size, iops=None, tier_level=None, snapshot_size=None, service_offering='storage_as_a_service', hourly_billing_flag=False) places an order for a file volume.

Parameters:
- storage_type – ‘performance’ or ‘endurance’
- location – Datacenter in which to order iSCSI volume
- size – Size of the desired volume, in GB
- iops – Number of IOPs for a “Performance” order
- tier_level – Tier level to use for an “Endurance” order
- snapshot_size – The size of optional snapshot space, if snapshot space should also be ordered (None if not ordered)
- service_offering – Requested offering package to use in the order (‘storage_as_a_service’, ‘enterprise’, or ‘performance’)
- hourly_billing_flag – Billing type, monthly (False) or hourly (True), default to monthly.

To be able to access all the new features, order `Storage-as-a-Service Package 759`.
{: tip}

For more information about ordering {{site.data.keyword.filestorage_short}} through the API, see [order_file_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/file/#SoftLayer.managers.file.FileStorageManager.order_file_volume){: external}.

By default, you can provision a combined total of 750 {{site.data.keyword.filestorage_short}} volumes. To increase the number of your volumes, contact your sales representative. Read about increasing limits [here](/docs/FileStorage?topic=FileStorage-managinglimits). For more information about the limit on simultaneous authorizations, see the [FAQs](/docs/FileStorage?topic=FileStorage-file-storage-faqs#authlimit).
{: important}

## Connecting your new storage
{: #mountingvolumesPortal}

When your provisioning request is complete, authorize your hosts to access the new storage and configure your connection. Depending on your host's operating system, follow the appropriate link.

- [Mounting {{site.data.keyword.filestorage_short}} on Red Hat Linux&reg;](/docs/FileStorage?topic=FileStorage-mountingLinux)
- [Mounting {{site.data.keyword.filestorage_short}} in CentOS](/docs/FileStorage?topic=FileStorage-mountingCentOS)
- [Mounting {{site.data.keyword.filestorage_short}} on Ubuntu](/docs/FileStorage?topic=FileStorage-mountingUbuntu)
- [Mounting {{site.data.keyword.filestorage_short}} as a datastore on ESXi hosts](/docs/FileStorage?topic=FileStorage-architectureguide)
- [Configuring {{site.data.keyword.filestorage_short}} for backup with cPanel](/docs/FileStorage?topic=FileStorage-cPanelBackups)
- [Configuring {{site.data.keyword.filestorage_short}} for backup with Plesk](/docs/FileStorage?topic=FileStorage-PleskBackup)

## Disaster recovery considerations
{: #DRconsider}

To avoid data-loss and to ensure business continuity, consider replicating your servers and storage in another data center. Replication keeps your data in sync in two different locations based on your snapshot schedule. For more information, see [Replicating data](/docs/FileStorage?topic=FileStorage-replication).

If you want to clone your volume and use it independently from the original volume, see [Creating and managing independent duplicate volumes](/docs/FileStorage?topic=FileStorage-duplicatevolume).

If you want to clone your volume and have the ability to refresh the duplicate on demand, see [Creating and managing dependent duplicate volumes](/docs/FileStorage?topic=FileStorage-dependentduplicate).

## Identifying {{site.data.keyword.filestorage_short}} volumes on the invoice
{: #findstorageinvoice}

All file shares appear on your invoice as a line item. Endurance volumes appear as “Endurance Storage Service” and Performance volumes appear as "Performance Storage Service". The rate varies based on your storage level. You can expand on Endurance or Performance to see that it's {{site.data.keyword.filestorage_short}}.
