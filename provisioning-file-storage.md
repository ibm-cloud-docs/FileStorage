---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Ordering {{site.data.keyword.filestorage_short}} 

There are two different ways you can provision {{site.data.keyword.filestorage_full}} based on your needs and preferences. The two options are:

- **Endurance**: provision Endurance tiers featuring pre-defined performance levels and features like snapshots and replication.
- **Performance**: build a high powered Performance environment where you can allocate the desired input/output operations per second (IOPS).

## How to order Endurance for {{site.data.keyword.filestorage_short}}

1. From the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, click **Storage** > **{{site.data.keyword.filestorage_short}}** OR from {{site.data.keyword.BluSoftlayer_full}} catalog click **Infrastructure** > **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Click **Order {{site.data.keyword.filestorage_short}}** in the upper-right corner. 
3. Select **Endurance** from the **Select Storage Type** list.
4. Click **Location** and select your data center.
   - Ensure that the new Storage will be added in the same location as the previously ordered host.
   - If you selected a data center with improved capabilities (marked with an asterisk), you can choose between Monthly or Hourly Billing. 
     1. With **hourly** billing, the calculation of the number of hours the file volume existed on the account is performed at the time the volume is deleted or at the end of the billing cycle, which ever comes first. Hourly billing is a good choice for storage that is used for a few days or less than a full month. Hourly billing is only available for storage provisioned in [select data centers](new-ibm-block-and-file-storage-location-and-features.html). 
     2. With **monthly** billing, the calculation for the price is pro-rated from the date of creation to the end of the billing cycle and billed immediately. There is no refund if a file volume is deleted before the end of the billing cycle. Monthly billing is a good choice for storage used in production workloads that use data that needs to be stored and accessed for long periods of time (a month or longer).
     **NOTE**: Monthly billing type is used by default for storage provisioned in data centers that aren't updated with improved capabilities.
5. Select the storage level for your applications:
    - **0.25 IOPS per GB** is designed for workloads with low I/O intensity. These workloads are typically characterized by having a large percentage of data inactive at a given time. Example applications include storing mailboxes or departmental level file shares.
    - **2 IOPS per GB** is designed for most general-purpose usage. Example applications include hosting small databases backing web applications or virtual machine disk images for a hypervisor.
    - **4 IOPS per GB** is designed for higher-intensity workloads. These workloads are typically characterized by having a high percentage of data active at a given time. Example applications include transactional and other performance-sensitive databases.
    - **10 IOPS per GB** is designed for the most demanding workloads such as those created by NoSQL databases, and data processing for Analytics. This tier is available in [select data centers](new-ibm-block-and-file-storage-location-and-features.html) for storage provisioned up to 4 TB in size.
6. Select the **Usable Storage Size** from the list.
7. Choose the **Snapshot Space Size** (in addition to your usable space) from the list.
8. Click **Continue**. You’re shown the monthly and prorated charges with a final chance to review order details.
9. Click the **I have read the Master Service Agreement** check box and click **Place Order**.
10. Your new storage allocation should be available in a few minutes.

**Note**: By default, you can provision a combined total of 250 {{site.data.keyword.filestorage_short}} volumes. Contact your sales representative to increase the number of your volumes. Read about increasing limits [here](managing-storage-limits.html).

For the limit on simultaneous authorizations, see our [FAQs](File-Storage-FAQ.html)

## How to order Performance for {{site.data.keyword.filestorage_short}}

1. From the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, click **Storage**, **{{site.data.keyword.filestorage_short}}** OR from {{site.data.keyword.BluSoftlayer_full}} catalog click **Infrastructure** >** Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Click **Order {{site.data.keyword.filestorage_short}}** in the upper-right corner. 
3. Select **Performance** from the Select Storage Type list.
4. Click **Location** list and select your data center.
    - Ensure that the new Storage will be added in the same location as the previously ordered host.
    - If you selected a data center with improved capabilities (marked with an asterisk), you can choose between Monthly or Hourly Billing. 
       1. With **hourly** billing, the number of hours the file volume existed on the account is calculated at the time the volume is deleted or at the end of the billing cycle, which ever comes first. Hourly billing is a good choice for storage that is used for a few days or less than a full month. Hourly billing is only available for storage provisioned in these select data centers. 
       2. With **monthly** billing, the calculation for the price is pro-rated from the date of creation to the end of the billing cycle and billed immediately. There's no refund if a file volume is deleted before the end of the billing cycle. Monthly billing is a good choice for storage used in production workloads that use data that needs to be stored and accessed for long periods of time (a month or longer).
       **NOTE**: Monthly billing type is used by default for storage provisioned in data centers that aren't updated with improved capabilities.  
5. Select the appropriate **Storage Size**.
6. Enter the IOPS in the **Specify IOPS** field.
7. Click **Continue**. You're shown the monthly and prorated charges with a final chance to review order details. Click **Previous** if you want to change your order.
8. Click the **I have read the Master Service Agreement** check box and click **Place Order**.
9. Your new storage allocation should be available in a few minutes.

**Note**: By default, you can provision a combined total of 250 {{site.data.keyword.filestorage_short}} volumes. Contact your sales representative to increase the number of your volumes. Read about increasing limits [here](managing-storage-limits.html).

For the limit on simultaneous authorizations, see our [FAQs](File-Storage-FAQ.html)

## How to identify my {{site.data.keyword.filestorage_short}} volumes on my invoice

All volumes appear on your invoice as line items. Endurance will appear as “Endurance Storage Service” and Performance will appear as "Performance Storage service". The rate will vary based on your storage level. You can expand on Endurance or Performance to see that it's a {{site.data.keyword.filestorage_short}} volume.

