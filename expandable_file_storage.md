---

copyright:
  years: 2014, 2018
lastupdated: "2018-1-12"

---
{:new_window: target="_blank"}

# Expanding File Share Capacity

With this new feature, current users of {{site.data.keyword.filestorage_full}} are able to expand the size of their {{site.data.keyword.filestorage_short}} in GB increments up to 12 TB immediately. They don't need to create a duplicate or manually migrate data to a larger volume. There’s no outage or lack of access to the storage while the resize is taking place. 

Billing for the volume is automatically updated to add the pro-rated difference of the new price to the current billing cycle. Then, the full new amount is billed in the next billing cycle.

This feature is only available in [select data centers](new-ibm-block-and-file-storage-location-and-features.html).

## Advantages of Expandable Storage

- **Cost management** – You might know that there’s potential for growth of your data, but you need a smaller amount of storage to start. The ability to expand, allows customers to save on costs of storage at the start and then grow to accommodate their needs.  

- **Growing Storage needs** - Customers who experience rapid growth beyond need a way to quickly and easily increase the size of their storage to manage that growth.

## Effects of expanding storage capacity on Replication

Expand action on the primary storage results in automatic resizing of the replica.

## Limitations

This feature is only available for storage that is provisioned in [data centers](new-ibm-block-and-file-storage-location-and-features.html) with enhanced capabilities. Encrypted storage that is provisioned in these data centers can be increased up to 12 TB.

Existing size limitations for {{site.data.keyword.filestorage_short}} that was provisioned with Endurance still apply (up to 4 TB for 10 IOPS tier and up to 12 TB for all other tiers).

## Resizing storage

1. From the {{site.data.keyword.slportal}}, click **Storage** > **{{site.data.keyword.filestorage_short}}** OR from {{site.data.keyword.BluSoftlayer_full}} catalog click **Infrastructure** > **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Select the volume from the list and click **Actions** > **Modify Volume**
3. Enter the new storage size in GB.
4. Review your selection and the new pricing.
5. Click **I have read the Master Service Agreement...**, and click **Place Order**.
6. Your new storage allocation is available in a few minutes.
