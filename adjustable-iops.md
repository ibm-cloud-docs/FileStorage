---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-24"

---
{:new_window: target="_blank"}

# Adjustable IOPS

With this new feature, our {{site.data.keyword.filestorage_full}} storage users can adjust the IOPS of their existing {{site.data.keyword.filestorage_short}} immediately. There's no need to create a duplicate or manually migrate data to new storage. Users won’t experience any kind of outage or lack of access to the storage while the adjustment is taking place. 

Billing for the storage is updated to add the pro-rated difference of the new price to the current billing cycle. Then, the full new amount is billed in the next billing cycle.

This feature is only available in [select data centers](new-ibm-block-and-file-storage-location-and-features.html). 

## Why should you take advantage of adjustable IOPS?

- Cost management – Some of our clients may only need high IOPS during peak usage times. For example, a large retail store has peak usage during the holidays and may need higher IOPS on their storage then than in the middle of the summer. This feature allows them to manage their costs and pay for higher IOPs only when they really need it.

## Are there any limitations?

This feature is only available for storage that is provisioned in [data centers](new-ibm-block-and-file-storage-location-and-features.html) with enhanced capabilities.

Clients can’t switch between Endurance and Performance when they adjust their IOPS. Users can specify a new IOPS tier or IOPS level for their storage based on the following criteria/restrictions: 

- If original volume is Endurance 0.25 tier, IOPS tier can’t be updated.
- If original volume is Performance with < 0.30 IOPS/GB, options available include only the size and IOPS combinations that result in < 0.30 IOPS/GB. 
- If original volume is Performance with >= 0.30 IOPS/GB, options available include only the size and IOPS combinations that result in >= 0.30 IOPS/GB. 

## How does adjusting IOPS affect replication?

If the volume has replication in place, the replica will be automatically updated to match the IOPS selection of the primary. 

## How can I adjust the IOPS on my storage?

1. From the {{site.data.keyword.slportal}}, click **Storage** > **{{site.data.keyword.filestorage_short}}** OR from {{site.data.keyword.BluSoftlayer_full}} catalog click **Infrastructure** > **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Select the volume from the list and click **Actions** > **Modify Volume**
3. Under **Storage IOPS Options**, make a new selection:
    - Endurance (Tiered IOPS): select an IOPS Tier greater than 0.25 IOPS/GB of your storage. You can increase the IOPS tier at any time. However, decreasing is only available once a month.
    - Performance (Allocated IOPS): specify new IOPS option for your storage by entering a value in the range 100 - 48,000 IOPS. (Be sure to look at any specific boundaries that are required by size in the order form.)
4. Review your selection and the new pricing.
5. Click the **I have read the Master Service Agreement...** check box and click **Place Order**.
6. Your new storage allocation will be available in a few minutes.
