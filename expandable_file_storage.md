---

copyright:
  years: 2014, 2017
lastupdated: "2017-12-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Expandable File Share Capacity

With this new feature, our current {{site.data.keyword.filestorage_full}} users are able to expand the size of their existing {{site.data.keyword.filestorage_short}} in GB increments up to 12 TB on the fly, without the need to create a duplicate or manually  migrate data to a larger volume.  There is no outage or lack of access to the storage while the resize is taking place. 

Billing for the volume is automatically updated to add the pro-rated difference of the new price to the current billing cycle and then billed the full new amount in the next billing cycle.

This feature is only available in [select data centers](new-ibm-block-and-file-storage-location-and-features.html). 

## Why Take Advantage of Expandable File Shares?

- **Cost management** – You may know that there is potential for growth of your data, but you need a smaller amount of storage to start. The ability to expand, allows our customers to save on costs of storage at the start and then grow to accommodate their needs.  

- **Growing Storage needs** - Customers who experience rapid growth beyond need a way to quickly and easily increase the size of their storage to manage that growth.

## How Does Resizing Storage Affect Replication?

Expand action on the primary storage will result automatic resize of the replica.

## Are There Any Limitations?

This feature is only available for storage that is provisioned in [datacenters](new-ibm-block-and-file-storage-location-and-features.html) with enhanced capabilities. Encrypted storage provisioned in these datacenters can be increased up to 12 TB. 

Existing size limitations for {{site.data.keyword.filestorage_short}} provisioned with Endurance still apply (up to 4 TB for 10 IOPS tier and up to 12 TB for all other tiers).

## How Can I Tell If My Provisioned Storage Is Expandable?

Storage provisioned with enhanced capabilites is always encryped-at-rest.  You can easily tell that your storage is eligible if it has a "lock" icon next to it in the portal UI. 

## How Can I Resize My Storage?

1. From the {{site.data.keyword.slportal}}, click **Storage** > **{{site.data.keyword.filestorage_short}}** OR from {{site.data.keyword.BluSoftlayer_full}} Catalog click **Infrastructure** > **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Select the volume from the list and click **Actions** > **Modify Volume**
3. Enter the new storage size in GB.
4. Review your selection and the new pricing.
5. Click the **I have read the Master Service Agreement...** checkbox and click **Place Order**.
6. Your new storage allocation should be available in a few minutes.

