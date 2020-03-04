---

copyright:
  years: 2014, 2020
lastupdated: "2020-03-04"

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


# Ordering {{site.data.keyword.filestorage_short}} through the Console
{: #orderingConsole}

You can provision {{site.data.keyword.filestorage_short}} and fine-tune to meet your capacity and IOPS needs. Get the most out of your storage with two options for specifying performance.
{:shortdesc}

- You can choose from Endurance IOPs tiers that feature pre-defined performance levels to fit workloads that don't have well-defined performance requirements.
- You can fine-tune your storage to meet specific performance requirements by specifying the total number of IOPS with Performance.

## Ordering {{site.data.keyword.filestorage_short}} with pre-defined IOPS Tiers (Endurance)
{: #endurance}

1. Log in to the [{{site.data.keyword.cloud}} catalog](https://{DomainName}/catalog){: external} and click **Storage**. Then, select {{site.data.keyword.filestorage_short}}. Click **Create**.
2. Select your deployment **Location** (data center).
   - Ensure that the new Storage is added in the same location as the compute host or hosts that you have.
3. Billing Method. If you selected a data center with improved capabilities (marked with an asterisk), you can choose between Monthly or Hourly Billing.
     1. With **hourly** billing, the number of hours that the file volume existed on the account is calculated at the time the volume is deleted or at the end of the billing cycle. Which ever comes first. Hourly billing is a good choice for storage that is used for a few days or less than a full month. Hourly billing is only available for storage that is provisioned in these [select data centers](/docs/FileStorage?topic=FileStorage-selectDC).
     2. With **monthly** billing, the calculation for the price is pro-rated from the date of creation to the end of the billing cycle and billed immediately. There's no refund if a file volume is deleted before the end of the billing cycle. Monthly billing is a good choice for storage that is used in production workloads that use data that needs to be stored and accessed for long periods of time (month or longer).

     Monthly billing type is used by default for storage that is provisioned in data centers that are **not** updated with improved capabilities.
     {:note}
4. Enter your storage size in the **Size** field.
5. Select **Endurance (tiered IOPS)**.
6. Select the IOPS tier that your application needs.
    - **0.25 IOPS per GB** is designed for workloads with low I/O intensity. These workloads are typically characterized by having a large percentage of data inactive at a time. Example applications include storing mailboxes or departmental level file shares.
    - **2 IOPS per GB** is designed for most general-purpose usage. Example applications include hosting small databases that are backing web applications or virtual machine disk images for a hypervisor.
    - **4 IOPS per GB** is designed for higher-intensity workloads. These workloads are typically characterized by having a high percentage of data active at a time. Example applications include transactional and other performance-sensitive databases.
    - **10 IOPS per GB** is designed for the most demanding workloads such as those created by NoSQL databases, and data processing for Analytics. This tier is available in [select data centers](/docs/FileStorage?topic=FileStorage-selectDC) for storage that is provisioned up to 4 TB.
7. Click in the field under **Snapshot space**, and select the snapshot size from the list. This space is in addition to your usable space. For snapshot space considerations and recommendation, read [Ordering Snapshots](/docs/FileStorage?topic=FileStorage-ordering-snapshots).
8. On the right, review your order summary, and apply your Promo Code if you have one.

   Discounts are applied when the order is processed.
   {:note}
9. After you reviewed the terms and conditions, check the I** have read and agree to the Third-Party Service Agreements** box.
10. Click **Create**. Your new storage allocation is available in a few minutes.

By default, you can provision a combined total of 250 {{site.data.keyword.blockstorageshort}} volumes. To increase the number of your volumes, contact your sales representative. Read about increasing limits [here](/docs/FileStorage?topic=FileStorage-managinglimits).<br/><br/>For more information about the limit on simultaneous authorizations, see the [FAQs](/docs/FileStorage?topic=FileStorage-file-storage-faqs#how-many-instances-can-share-the-use-of-a-provisioned-file-storage-volume-).
{:tip}

## Ordering {{site.data.keyword.filestorage_short}} with custom IOPS (Performance)
{: #performance}

1. Log in to the [{{site.data.keyword.cloud}} catalog](https://{DomainName}/catalog){: external} and click **Storage**. Then, select {{site.data.keyword.filestorage_short}}. Click **Create**.
2. Click **Location** and select your data center.
   - Ensure that the new Storage is added in the same location as the compute host or hosts that you have.
3. Billing Method. If you selected a data center with improved capabilities (marked with an asterisk), you can choose between Monthly or Hourly Billing.
     1. With **hourly** billing, the number of hours that the file volume existed on the account is calculated at the time the volume is deleted or at the end of the billing cycle. Which ever comes first. Hourly billing is a good choice for storage that is used for a few days or less than a full month. Hourly billing is only available for storage that is provisioned in these [select data centers](/docs/FileStorage?topic=FileStorage-selectDC).
     2. With **monthly** billing, the calculation for the price is pro-rated from the date of creation to the end of the billing cycle and billed immediately. There's no refund if a file volume is deleted before the end of the billing cycle. Monthly billing is a good choice for storage that is used in production workloads that use data that needs to be stored and accessed for long periods of time (month or longer).

     Monthly billing type is used by default for storage that is provisioned in data centers that are **not** updated with improved capabilities.
     {:note}
4. Enter your storage size in the **Size** field.
5. Select **Performance (Allocated IOPS)**.
6. Enter the IOPS in the **Performance (Allocated IOPS)** field.
7. Click in the field under **Snapshot space**, and select the snapshot size from the list. This space is in addition to your usable space. For snapshot space considerations and recommendation, read [Ordering Snapshots](/docs/FileStorage?topic=FileStorage-ordering-snapshots).
8. On the right, review your order summary, and apply your Promo Code if you have one.

   Discounts are applied when the order is processed.
   {:note}
9. After you reviewed the terms and conditions, check the I** have read and agree to the Third-Party Service Agreements** box.
10. Click **Create**. Your new storage allocation is available in a few minutes.

By default, you can provision a combined total of 250 {{site.data.keyword.blockstorageshort}} volumes. To increase the number of your volumes, contact your sales representative. Read about increasing limits [here](/docs/FileStorage?topic=FileStorage-managinglimits).<br/><br/>For more information about the limit on simultaneous authorizations, see the [FAQs](/docs/FileStorage?topic=FileStorage-file-storage-faqs#how-many-instances-can-share-the-use-of-a-provisioned-file-storage-volume-).
{:important}


## Connecting your new storage
{: #mountingvolumesPortal}

When your provisioning request is complete, authorize your hosts to access the new storage and configure your connection. Depending on your host's operating system, follow the appropriate link.
- [Mounting {{site.data.keyword.filestorage_short}} on Linux](/docs/FileStorage?topic=FileStorage-mountingLinux)
- [Mounting {{site.data.keyword.filestorage_short}} in CentOS](/docs/FileStorage?topic=FileStorage-mountingCentOS)
- [Mounting {{site.data.keyword.filestorage_short}} on CoreOS](/docs/FileStorage?topic=FileStorage-mountingCoreOS)
- [Configuring {{site.data.keyword.filestorage_short}} for backup with cPanel](/docs/FileStorage?topic=FileStorage-cPanelBackups)
- [Configuring {{site.data.keyword.filestorage_short}} for backup with Plesk](/docs/FileStorage?topic=FileStorage-PleskBackup)
- [Mounting {{site.data.keyword.filestorage_short}} Volume on ESXi hosts](/docs/FileStorage?topic=FileStorage-architectureguide)

## Disaster recovery considerations

To avoid data-loss and to ensure business continuity, consider replicating your servers and storage in another data center. Replication keeps your data in sync in two different locations based on your snapshot schedule. For more information, see [Replicating data](/docs/FileStorage?topic=FileStorage-replication).

If you want to clone your volume and use it independently from the original volume, see [Creating and managing independent duplicate volumes](/docs/FileStorage?topic=FileStorage-duplicatevolume).

If you want to clone your volume and have the ability to refresh the duplicate on demand, see [Creating and managing dependent duplicate volumes](/docs/FileStorage?topic=FileStorage-dependentduplicate).

## Identifying {{site.data.keyword.filestorage_short}} volumes on the invoice

All file shares appear on your invoice as a line item. Endurance volumes appear as “Endurance Storage Service” and Performance volumes appear as "Performance Storage Service". The rate varies based on your storage level. You can expand on Endurance or Performance to see that it's {{site.data.keyword.filestorage_short}}.
