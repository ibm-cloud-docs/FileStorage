---

copyright:
  years: 2014, 2025
lastupdated: "2025-11-03"

keywords: File Storage for Classic, NFS, provisioning, ordering, order file share, provision file volume, duplicate, cloning, replication

subcollection: FileStorage

---
{{site.data.keyword.attribute-definition-list}}
{: uilinked}


# Ordering {{site.data.keyword.filestorage_short}}
{: #orderingFileStorage}

You can order {{site.data.keyword.filestorage_short}} volumes in the console, from the CLI, with the API, or Terraform. You can provision {{site.data.keyword.filestorage_short}} and fine-tune your file shares to meet your capacity and IOPS needs. Get the most out of your storage with two options for specifying IOPS: Endurance and Performance. You can provision file shares with capacity up to 12 TB and maximum IOPS of 48,000.
{: shortdesc}

- You can choose from Endurance IOPS tiers that feature pre-defined performance levels to fit workloads that don't have well-defined performance requirements.
   - **0.25 IOPS per GB** is designed for workloads with low I/O intensity. These workloads are typically characterized by having a large percentage of data inactive at a time. Example applications include storing mailboxes or departmental-level file shares. The CLI and API responses show this tier as `LOW_INTENSITY_TIER`.
   - **2 IOPS per GB** is designed for most general-purpose usage. Example applications include hosting small databases that are backing web applications or virtual machine disk images for a hypervisor. The CLI and API responses show this tier as `READHEAVY_TIER`.
   - **4 IOPS per GB** is designed for higher-intensity workloads. These workloads are typically characterized by having a high percentage of data active at a time. Example applications include transactional and other performance-sensitive databases. The CLI and API responses show this tier as `WRITEHEAVY_TIER`.
   - **10 IOPS per GB** is designed for the most demanding workloads such as those created by NoSQL databases, and data processing for Analytics. This tier is available for storage that is provisioned up to 4 TB.The CLI and API responses show this tier as `10_IOPS_PER_GB`.
- You can fine-tune your storage to meet specific performance requirements by specifying the total number of IOPS with Performance. The available **custom** IOPS range depends on the volume capacity. The following table shows the available IOPS ranges based on volume size.

   | Volume size (GB) | IOPS range |
   |-------------|-----------------|
   | 10 - 39     | 100 - 1,000 |
   | 40 - 79     | 100 - 2,000 |
   | 80 - 99     | 100 - 4,000 |
   | 100 - 499   | 100 - 6,000 |
   | 500 - 999   | 100 - 10,000|
   | 1,000 - 1,999 | 100 - 20,000|
   | 2,000 - 2,999 | 200 - 40,000|
   | 3,000 - 3,999 | 200 - 48,000|
   | 4,000 - 7,999 | 300 - 48,000|
   | 8,000 - 9,999 | 500 - 48,000 |
   | 10,000 - 12,000 | 1,000 - 48,000 |
   {: caption="Available IOPS based on volume size" caption-side="bottom"}

By default, you can provision a combined total of 700 {{site.data.keyword.filestorage_short}} volumes. To increase the number of your volumes, contact Support. For more information, see [Managing storage limits](/docs/FileStorage?topic=FileStorage-managinglimits). For more information about the limit on simultaneous authorizations, see the [FAQs](/docs/FileStorage?topic=FileStorage-file-storage-faqs#authlimit).
{: important}   

## Ordering {{site.data.keyword.filestorage_short}} in the console
{: #orderingFileStorageUI}
{: ui}

1. Log in to the [{{site.data.keyword.cloud}} catalog](https://{DomainName}/catalog){: external} and click **Storage**. Then, select {{site.data.keyword.filestorage_short}}. Click **Create**.
2. Select your deployment location (region, location, zone).
   - Make sure that the new file share is added in the same location as the Compute host or hosts that you have.
3. Billing. You can choose between Monthly or Hourly Billing.
   - With **hourly** billing, the number of hours that the file volume existed on the account is calculated at the time the volume is deleted or at the end of the billing cycle. Which ever comes first. Hourly billing is a good choice for storage that is used for a few days or less than a full month.
   - With **monthly** billing, the calculation for the price is pro-rated from the date of creation to the end of the billing cycle and billed immediately. If a file volume is deleted before the end of the billing cycle, the difference is not refunded. Monthly billing is a good choice for storage that is used in production workloads that use data that needs to be stored and accessed for long periods of time (month or longer).
4. Enter your storage size in the **Size** field.
5. Select the size of the Snapshot space from the list.

   This space is in addition to your usable space. For snapshot space considerations and recommendations, read [Ordering Snapshots](/docs/BlockStorage?topic=BlockStorage-orderingsnapshots).
   {: tip}

6. Select your IOPS profile. You can choose between the predefined values of **Endurance (Tiers)** or enter your custom IOPS value for **Performance**.
7. In the side panel, review your order summary, and apply your Promo Code if you have one.

   Discounts are applied when the order is processed.
   {: note}

8. Review your order, and read the service agreement. If you agree with the terms, check the box.
9. Click **Create**. Your new storage allocation is available in a few minutes.

## Ordering {{site.data.keyword.filestorage_short}} from the CLI
{: #orderingthroughCLI}
{: cli}

Before you begin, decide on the CLI client that you want to use.

* You can either install the [IBM Cloud CLI](/docs/cli){: external} and install the SL plug-in with `ibmcloud plugin install sl`. For more information, see [Extending IBM Cloud CLI with plug-ins](/docs/cli?topic=cli-plug-ins).
* Or, you can install the [SLCLI](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.

Each order must have an associated location (data center). When you order {{site.data.keyword.filestorage_short}}, make sure that it is provisioned in the same location as your Compute instances.
{: important}

### Provisioning from the IBMCLOUD CLI
{: #orderingthroughICCLI}

Use the `ibmcloud sl file volume-order` command to order a file volume. The following example provisions a 500-GB file share in the DAL13 data center with a tiered performance profile (4 IOPS per GB) and 500 GB snapshot space.

```sh
$ ibmcloud sl file volume-order --storage-type endurance --size 500 --tier 4 -d dal13 --snapshot-size 500
This action will incur charges on your account. Continue?> y
OK
Order 110526870 was placed.
 > Storage as a Service
 > File Storage
 > 500 GBs
 > 4 IOPS per GB
 > 500 GB (Snapshot Space)

You may run 'ibmcloud sl file volume-list --order 110526870' to find this file volume after it is ready.
```
{: codeblock}

For more information about all of the parameters that are available for this command, see [ibmcloud sl file volume-order](/docs/cli?topic=cli-sl-file-storage-service#sl_file_volume_order).

### Provisioning from the SLCLI
{: #orderingthroughSLCLI}

Use the `slcli file volume-order` command to provision the file share volume. The following example shows how to order a 10 GB {{site.data.keyword.filestorage_short}} volume with 100 IOPS per GB.

```sh
$ slcli file volume-order --storage-type performance --size 20 --location dal10 --iops 100
Order #32076317 placed successfully!
> Storage as a Service
> File Storage
> 20 GBs
> 100 IOPS
```
{: codeblock}

## Ordering {{site.data.keyword.filestorage_short}} with the API
{: #orderingthroughAPI}
{: api}

The method `order_file_volume` (storage_type, location, size, iops=None, tier_level=None, snapshot_size=None, service_offering='storage_as_a_service', hourly_billing_flag=False) places an order for a file share.

You must specify the following parameters for a successful order.
- `storage_type` – "performance" or "endurance".
- `location` – Data center in which to order the share.
- `size` – Size of the new volume, in GB.
- `iops` – Number of IOPS for a “Performance” order.
- `tier_level` – Tier level to use for an “Endurance” order.
- `snapshot_size` – The size of optional snapshot space, if snapshot space is also to be ordered. (None if not ordered.)
- `service_offering` – Requested offering package to use in the order ("storage_as_a_service").
- `hourly_billing_flag` – Billing type, monthly (False) or hourly (True), default to monthly.

To be able to access all the new features, order `Storage-as-a-Service Package 759`.
{: tip}

For more information about ordering {{site.data.keyword.filestorage_short}} through the API, see [order_file_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/SoftLayer.managers.FileStorageManager/#SoftLayer.managers.FileStorageManager.order_file_volume){: external}.

## Ordering {{site.data.keyword.filestorage_short}} with Terraform
{: #orderingthroughTerraform}
{: terraform}

To use Terraform, download the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).
{: requirement}

### Provision Endurance {{site.data.keyword.filestorage_short}} with Terraform
{: #order-endurance-terraform}

You can use the following example to create a 20 GB block storage volume with 10 GB snapshot capacity and 0.25 IOPS/GB performance tier in the DAL09 data center. The example also defines weekly snapshots to be created at 1:02 PM every Sunday.

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
    minute          = 2
    hour            = 13
    day_of_week     = "SUNDAY"
    enable          = true
  }
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_storage_block](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/storage_block){: external}.

### Provision Performance {{site.data.keyword.filestorage_short}} with Terraform
{: #order-performance-terraform}

You can use the following example to create a 20 GB block storage volume with custom 100 IOPS performance level.

```terraform
resource "ibm_storage_file" "fs_performance" {
  type       = "Performance"
  datacenter = "dal09"
  capacity   = 20
  iops       = 100

  # Optional fields
  allowed_virtual_guest_ids = ["28961689"]
  allowed_subnets           = ["10.146.139.64/26"]
  allowed_ip_addresses      = ["10.146.139.84"]
  hourly_billing            = true
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_storage_file](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/storage_file){: external}.

## Connecting your new storage
{: #mountingvolumesPortal}

When your provisioning request is complete, authorize your hosts to access the new storage and configure your connection. Depending on your host's operating system, follow the appropriate link.

- [Mounting {{site.data.keyword.filestorage_short}} on Red Hat Linux&reg;](/docs/FileStorage?topic=FileStorage-mountingLinux)
- [Mounting {{site.data.keyword.filestorage_short}} on Ubuntu](/docs/FileStorage?topic=FileStorage-mountingUbuntu)
- [Mounting {{site.data.keyword.filestorage_short}} as a VMware&reg; datastore on ESXi hosts](/docs/FileStorage?topic=FileStorage-architectureguide)
- [Configuring {{site.data.keyword.filestorage_short}} for backup with cPanel](/docs/FileStorage?topic=FileStorage-cPanelBackups)
- [Configuring {{site.data.keyword.filestorage_short}} for backup with Plesk](/docs/FileStorage?topic=FileStorage-PleskBackup)

## Disaster recovery considerations
{: #DRconsider}

To avoid data-loss and to ensure business continuity, consider replicating your servers and storage in another data center. Replication keeps your data in sync in two different locations based on your snapshot schedule. For more information, see [Replicating data](/docs/FileStorage?topic=FileStorage-replication).

If you want to clone your volume, see [Creating and managing independent duplicate volumes](/docs/FileStorage?topic=FileStorage-duplicatevolume). Duplicate volumes can be managed independently and refreshed with data from the original volume on demand.

## Identifying {{site.data.keyword.filestorage_short}} volumes on the invoice
{: #findstorageinvoice}

All file shares appear on your invoice as a line item. Endurance volumes appear as _Endurance Storage Service_ and Performance volumes appear as _Performance Storage Service_. The rate varies based on your storage level. You can expand on Endurance or Performance to see that it's {{site.data.keyword.filestorage_short}}.
