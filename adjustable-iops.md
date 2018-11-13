---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-13"

---
{:new_window: target="_blank"}

# Adjusting IOPS

With this new feature, {{site.data.keyword.filestorage_full}} storage users can adjust the IOPS of their existing {{site.data.keyword.filestorage_short}} immediately. They don't need to create a duplicate or manually copy data to new storage. Users don't experience any kind of outage or lack of access to the storage while the adjustment is taking place.

Billing for the storage is updated to add the pro-rated difference of the new price to the current billing cycle. The full new amount is billed in the next billing cycle.


## Advantages of adjustable IOPS

- Cost management – Some of our clients might need high IOPS just during peak usage times. For example, a large retail store has peak usage during the holidays and might need higher IOPS on their storage then than in the middle of the summer. This feature allows them to manage their costs and pay for higher IOPs only when they need it.

## Limitations

This feature is only available in [select data centers](new-ibm-block-and-file-storage-location-and-features.html).

Clients can’t switch between Endurance and Performance when they adjust their IOPS. Users can specify a new IOPS tier or IOPS level for their storage based on the following criteria/restrictions.

- If original volume is Endurance 0.25 tier, IOPS tier can’t be updated.
- If original volume is Performance with less than or equal to 0.30 IOPS/GB, options available include only the size and IOPS combinations that result in less than or equal to 0.30 IOPS/GB.
- If original volume is Performance with more than 0.30 IOPS/GB, options available include only the size and IOPS combinations that result in more than 0.30 IOPS/GB.

## Effect of IOPS adjustment on replication

If the volume has replication in place, the replica is automatically updated to match the IOPS selection of the primary.

## Adjusting the IOPS on your Storage

1. Go to your list of {{site.data.keyword.filestorage_short}}
    - From the customer portal, click **Storage** > **{{site.data.keyword.filestorage_short}}** OR
    - From the {{site.data.keyword.BluSoftlayer_full}} console, click **Infrastructure** > **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Select the volume from the list and click **Actions** > **Modify Volume**
3. Under **Storage IOPS Options**, make a new selection:
    - For Endurance (Tiered IOPS), select an IOPS Tier greater than 0.25 IOPS/GB of your storage. You can increase the IOPS tier at any time. However, decreasing is available only once a month.
    - For Performance (Allocated IOPS), specify new IOPS option for your storage by entering a value in the range 100 - 48,000 IOPS. (Be sure to look at any specific boundaries that are required by size in the order form.)
4. Review your selection and the new pricing.
5. Click the **I have read the Master Service Agreement...** check box, and click **Place Order**.
6. Your new storage allocation is going to be available in a few minutes.
