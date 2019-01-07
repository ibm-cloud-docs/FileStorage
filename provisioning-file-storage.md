---

copyright:
  years: 2014, 2019
lastupdated: "2019-01-07"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Ordering {{site.data.keyword.filestorage_short}} through the Console

You can provision {{site.data.keyword.filestorage_short}} and fine-tune to meet your capacity and IOPS needs. Get the most out of your storage with two options for specifying performance.

- You can choose from Endurance IOPs tiers that feature pre-defined performance levels to fit workloads that don't have well-defined performance requirements.
- You can fine-tune your storage to meet specific performance requirements by specifying the total number of IOPS with Performance.

## Ordering {{site.data.keyword.filestorage_short}} with pre-defined IOPS Tiers (Endurance)

1. Log in to [The IBM Cloud catalog](https://{DomainName}/catalog/){:new_window} and click **Storage**. Then, select {{site.data.keyword.filestorage_short}}. Click **Create**.

   Alternatively, you can log in to the [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window} }, click **Storage** > **{{site.data.keyword.filestorage_short}}**. In the upper right, click **Order {{site.data.keyword.filestorage_short}}**.
2. Select your deployment **Location** (data center).
   - Ensure that the new Storage is added in the same location as the compute host or hosts that you have.
3. Billing. If you selected a data center with improved capabilities (marked with an asterisk), you can choose between Monthly or Hourly Billing.
     1. With **hourly** billing, the number of hours that the file volume existed on the account is calculated at the time the LUN is deleted or at the end of the billing cycle. Which ever comes first. Hourly billing is a good choice for storage that is used for a few days or less than a full month. Hourly billing is only available for storage that is provisioned in these [select data centers](new-ibm-block-and-file-storage-location-and-features.html).
     2. With **monthly** billing, the calculation for the price is pro-rated from the date of creation to the end of the billing cycle and billed immediately. There's no refund if a file volume is deleted before the end of the billing cycle. Monthly billing is a good choice for storage that is used in production workloads that use data that needs to be stored and accessed for long periods of time (month or longer).

     Monthly billing type is used by default for storage that is provisioned in data centers that are **not** updated with improved capabilities.
     {:note}
4. Enter your storage size in the **New Storage Size** field.
5. Select **Endurance (tiered IOPS)** in the **Storage IOPS Options** section.
6. Select the IOPS tier that your application needs.
    - **0.25 IOPS per GB** is designed for workloads with low I/O intensity. These workloads are typically characterized by having a large percentage of data inactive at a time. Example applications include storing mailboxes or departmental level file shares.
    - **2 IOPS per GB** is designed for most general-purpose usage. Example applications include hosting small databases that are backing web applications or virtual machine disk images for a hypervisor.
    - **4 IOPS per GB** is designed for higher-intensity workloads. These workloads are typically characterized by having a high percentage of data active at a time. Example applications include transactional and other performance-sensitive databases.
    - **10 IOPS per GB** is designed for the most demanding workloads such as those created by NoSQL databases, and data processing for Analytics. This tier is available in [select data centers](new-ibm-block-and-file-storage-location-and-features.html) for storage that is provisioned up to 4 TB.
7. Click **Specify Snapshot Space Size** and select the snapshot size from the list. This space is in addition to your usable space. For snapshot space considerations and recommendation, read [Ordering Snapshots](ordering-snapshots.html).
8. On the right, review your order summary, and apply your Promo Code if you have one.
9. After you reviewed the terms and conditions, check the I** have read and agree to the Third-Party Service Agreements** box.
10. Click **Create**. Your new storage allocation is available in a few minutes.

By default, you can provision a combined total of 250 {{site.data.keyword.blockstorageshort}} volumes. To increase the number of your volumes, contact your sales representative. Read about increasing limits [here](managing-storage-limits.html).<br/><br/>For more information about the limit on simultaneous authorizations, see the [FAQs](faqs.html#how-many-instances-can-share-the-use-of-a-provisioned-file-storage-volume-).
{:tip}

## Ordering {{site.data.keyword.filestorage_short}} with Custom IOPS (Performance)

1. Log in to [The IBM Cloud catalog](https://{DomainName}/catalog/){:new_window} and click **Storage**. Then, select {{site.data.keyword.filestorage_short}}. Click **Create**.

   Alternatively, you can log in to the [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window} }, click **Storage** > **{{site.data.keyword.filestorage_short}}**. In the upper right, click **Order {{site.data.keyword.filestorage_short}}**.
2. Click **Location** and select your data center.
   - Ensure that the new Storage is added in the same location as the compute host or hosts that you have.
3. Billing. If you selected a data center with improved capabilities (marked with an asterisk), you can choose between Monthly or Hourly Billing.
     1. With **hourly** billing, the number of hours that the file volume existed on the account is calculated at the time the LUN is deleted or at the end of the billing cycle. Which ever comes first. Hourly billing is a good choice for storage that is used for a few days or less than a full month. Hourly billing is only available for storage that is provisioned in these [select data centers](new-ibm-block-and-file-storage-location-and-features.html).
     2. With **monthly** billing, the calculation for the price is pro-rated from the date of creation to the end of the billing cycle and billed immediately. There's no refund if a file volume is deleted before the end of the billing cycle. Monthly billing is a good choice for storage that is used in production workloads that use data that needs to be stored and accessed for long periods of time (month or longer).

     Monthly billing type is used by default for storage that is provisioned in data centers that are **not** updated with improved capabilities.
     {:note}
4. Enter your storage size in the **New Storage Size** field.
5. Select **Performance (Allocated IOPS)** in the **Storage IOPS Options** section.
6. Enter the IOPS in the **Performance (Allocated IOPS)** field.
7. Click **Specify Snapshot Space Size** and select the snapshot size from the list. This space is in addition to your usable space. For snapshot space considerations and recommendation, read [Ordering Snapshots](ordering-snapshots.html).
8. On the right, review your order summary, and apply your Promo Code if you have one.
9. After you reviewed the terms and conditions, check the I** have read and agree to the Third-Party Service Agreements** box.
10. Click **Create**. Your new storage allocation is available in a few minutes.

By default, you can provision a combined total of 250 {{site.data.keyword.blockstorageshort}} volumes. To increase the number of your volumes, contact your sales representative. Read about increasing limits [here](managing-storage-limits.html).<br/><br/>For more information about the limit on simultaneous authorizations, see the [FAQs](faqs.html#how-many-instances-can-share-the-use-of-a-provisioned-file-storage-volume-).
{:important}


## Connecting your new storage

When your provisioning request is complete, authorize your hosts to access the new storage and configure your connection. Depending on your host's operating system, follow the appropriate link.
- [Mounting {{site.data.keyword.filestorage_short}} on Linux](accessing-file-storage-linux.html)
- [Mounting {{site.data.keyword.filestorage_short}} in CentOS](mounting-nsf-file-storage.html)
- [Mounting {{site.data.keyword.filestorage_short}} on CoreOS](mounting-storage-coreos.html)
- [Configuring {{site.data.keyword.filestorage_short}} for backup with cPanel](configure-backup-cpanel.html)
- [Configuring {{site.data.keyword.filestorage_short}} for backup with Plesk](configure-backup-plesk.html)
- [Mounting {{site.data.keyword.filestorage_short}} Volume on ESXi hosts](architecture-guide-file-storage-vmware.html)

## Disaster recovery considerations

To avoid data-loss and to ensure business continuity, consider replicating your servers and storage in another data center. Replication keeps your data in sync in two different locations based on your snapshot schedule. For more information, see [Replicating data](replication.html).

If you want to clone your volume and use it independently from the original volume, see [Creating a duplicate Block Volume](how-to-create-duplicate-volume.html).

## Identifying {{site.data.keyword.filestorage_short}} volumes on the invoice

All fileshares appear on your invoice as a line item. Endurance volumes appear as “Endurance Storage Service” and Performance volumes appear as "Performance Storage Service". The rate varies based on your storage level. You can expand on Endurance or Performance to see that it's {{site.data.keyword.filestorage_short}}.
