---

copyright:
  years: 2014, 2022
lastupdated: "2022-03-04"

keywords: File Storage, file storage, NFS, durability, availability, HA, high-availability, data loss, data integrity, 

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:shortdesc: .shortdesc}

# Availability and Durability of {{site.data.keyword.filestorage_short}}
{: #storageavailability}

When you store your data in {{site.data.keyword.filestorage_full}}, it's durable, highly available and encrypted.
{: shortdesc}

## Durability
{: #stordurability}

Think of durability as a measurement of how healthy and resilient your data is. Durability in {{site.data.keyword.filestorage_short}} means that your data is stored consistent and intact without the signs of data decay, influence of drive failures, or any other form of corruption. 99.999999999% (11 nines) durability means that if you store 10 million files, then you expect to lose one file of your data every ten thousand years.

When people hear the word "durability", most of them think of hardware failures of storage, compute and network components that could cause data loss. In {{site.data.keyword.filestorage_short}}, your data is protected against multiple drive failures and numerous type of disk errors that otherwise might negatively affect data durability and data integrity.

Other than physical failure, a common source of data loss is accidental deletion or modifications of files by end users. {{site.data.keyword.filestorage_short}} is only accessible to authorized hosts within your network. You control who can access it. Another measure to protect against accidental deletion and modification of files by end users, is to set up periodic Snapshots. If a user accidentally modifies or deletes crucial data from a volume, the data can be easily and quickly restored from a snapshot copy. For more information about this feature, see [Snapshots](/docs/FileStorage?topic=FileStorage-snapshot).

The 11 nines durability target applies to a single Availability Zone. To protect against natural disasters that could destroy an entire Availability Zone, consider storing your most important data in multiple locations. For more information about this topic, see [Replicating Data](/docs/FileStorage?topic=FileStorage-replicatio).

## High Availability
{: #storavailability}

{{site.data.keyword.filestorage_short}} is built upon best-in-class, proven, enterprise-grade hardware and software to ensure high availability and uptime. Your data is stored on HA paired nodes. Each storage node has many paths to its own Solid State Drives and its partner node's SSDs as well. This protects against path failure and if there's a controller failure, the node can still access its partner's disks for continued productivity. Redundant network ports and paths protect against network failures all the way up the stack across the cloud connections.

| Storage type | Use Case | Durability | Availability |
|--------------|----------|------------|--------------|
| Endurance -  0.25 IOPS per GB tier  |  It's designed for workloads with low I/O intensity. These workloads are typically characterized by having a large percentage of data inactive at a time. Example applications include storing mailboxes or departmental level file shares. | 99.999999999% (11 9's) | 99.999%       |
| Endurance - 2 IOPS per GB tier | It's designed for most general-purpose usage. Example applications include hosting small databases that are backing web applications or virtual machine disk images for a hypervisor.| 99.999999999% (11 9's) | 99.999% |
| Endurance - 4 IOPS per GB tier| It's designed for higher-intensity workloads. These workloads are typically characterized by having a high percentage of data active at a time. Example applications include transactional and other performance-sensitive databases. | 99.999999999% (11 9's) | 99.999% |
| Endurance - 10 IOPS per GB tier| It's designed for the most demanding workloads such as those created by NoSQL databases, and data processing for Analytics. | 99.999999999% (11 9's) | 99.999% |
| Performance - a high-powered environment with custom IOPS | It's designed to manage rapid data changes with well-defined performance requirements. | 99.999999999% (11 9's) | 99.999% |
{: caption="Table 1. Storage durability and availability chart." caption-side="top"}

## Encryption
{: #storencryption}

{{site.data.keyword.cloud}} provides full-disk encryption without compromising storage application performance. For more information about encrypion, see [Securing your data in File Storage)](/docs/FileStorage?topic=FileStorage-mng-data).
