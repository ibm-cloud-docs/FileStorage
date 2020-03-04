---

copyright:
  years: 2018, 2019
lastupdated: "2019-11-11"

keywords: File Storage, modify volume, NFS, file storage, expand capacity

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:shortdesc: .shortdesc}

# Expanding File Share Capacity
{: #expandCapacity}

With this feature, current users of {{site.data.keyword.filestorage_full}} are able to expand the size of their {{site.data.keyword.filestorage_short}} in GB increments up to 12 TB immediately. They don't need to create a duplicate or manually migrate data to a larger volume. There’s no outage or lack of access to the storage while the resize is taking place.
{:shortdesc}

Billing for the volume is automatically updated to add the pro-rated difference of the new price to the current billing cycle. Then, the full new amount is billed in the next billing cycle.

This feature is only available in [select data centers](/docs/FileStorage?topic=FileStorage-selectDC).

## Advantages of Expandable Storage

- **Cost management** – You might know that there’s potential for growth of your data, but you need a smaller amount of storage to start. The ability to expand, allows customers to save on costs of storage at the start and then grow to accommodate their needs.  

- **Growing Storage needs** - Customers who experience rapid growth beyond need a way to quickly and easily increase the size of their storage to manage that growth.

## Effects of expanding storage capacity on Replication

Expand action on the primary storage results in automatic resizing of the replica.

## Limitations
{: #limitsofextension}

This feature is only available for storage that is provisioned in [data centers](/docs/FileStorage?topic=FileStorage-selectDC) with enhanced capabilities. Encrypted storage that is provisioned in these data centers can be increased up to 12 TB.

Existing size limitations for {{site.data.keyword.filestorage_short}} that was provisioned with Endurance still apply (up to 4 TB for 10 IOPS tier and up to 12 TB for all other tiers).

## Resizing storage
{: #resizingsteps}

1. Go to the [{{site.data.keyword.cloud}} console](https://{DomainName}/){: external}. From the menu, select **Classic Infrastructure**. Click **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Select the volume from the list and click the ellipsis (**...**) > **Modify Share**.
3. Enter the new storage size in GB.
4. Review your selection and the new pricing.
5. Click **I have read the Master Service Agreement...**, and click **Place Order**.
6. Your new storage allocation is available in a few minutes.

For the OS to recognize the extra storage space, unmount and mount the modified volume again.
{:tip}

Alternatively, you can use the following command in SLCLI.
```
# slcli file volume-modify --help
Usage: slcli file volume-modify [OPTIONS] VOLUME_ID

Options:
  -c, --new-size INTEGER        New Size of file volume in GB. ***If no size
                                is given, the original size of volume is
                                used.***
                                Potential Sizes: [20, 40, 80, 100,
                                250, 500, 1000, 2000, 4000, 8000, 12000]
                                Minimum: [the original size of the volume]
  -i, --new-iops INTEGER        Performance Storage IOPS, between 100 and 6000
                                in multiples of 100 [only for performance
                                volumes] ***If no IOPS value is specified, the
                                original IOPS value of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is less than 0.3, new IOPS/GB
                                must also be less than 0.3. If original
                                IOPS/GB for the volume is greater than or
                                equal to 0.3, new IOPS/GB for the volume must
                                also be greater than or equal to 0.3.]
  -t, --new-tier [0.25|2|4|10]  Endurance Storage Tier (IOPS per GB) [only for
                                endurance volumes] ***If no tier is specified,
                                the original tier of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is 0.25, new IOPS/GB for the
                                volume must also be 0.25. If original IOPS/GB
                                for the volume is greater than 0.25, new
                                IOPS/GB for the volume must also be greater
                                than 0.25.]
  -h, --help                    Show this message and exit.
```
