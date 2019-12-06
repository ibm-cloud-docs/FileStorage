---

copyright:
  years: 2014, 2019
lastupdated: "2019-12-06"

keywords: File Storage, encryption, security, provisioning, limitations, NFS

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:faq: data-hd-content-type='faq'}

# FAQs
{: #file-storage-faqs}

## How can I tell which of my {{site.data.keyword.filestorage_short}} volumes are encrypted?
{: faq}

Look at your list of {{site.data.keyword.filestorage_short}} in the customer portal. You can see a lock icon to the right of the volume name for the volumes that are encrypted.

## If I purchased non-encrypted {{site.data.keyword.filestorage_short}} in a data center that was upgraded for encryption, can I encrypt my {{site.data.keyword.filestorage_short}}?
{: faq}

{{site.data.keyword.filestorage_short}} that was provisioned before a data center upgrade can't be encrypted. New {{site.data.keyword.filestorage_short}} that was provisioned in upgraded data centers is automatically encrypted. It's automatic, not a provisioning setting that can be selected or left out. Data on non-encrypted storage can be encrypted by creating a new volume, then copying the data to the new encrypted volume with host-based migration. For more information, see [Migrating File Storage](/docs/infrastructure/FileStorage?topic=FileStorage-migratestorage).

## How do I know whether I'm provisioning {{site.data.keyword.filestorage_short}} in an upgraded data center?
{: faq}

In the {{site.data.keyword.filestorage_short}} order form, all upgraded data centers are denoted with an asterisk (`*`). During the ordering process, you're given an indication that you're provisioning storage with encryption. When the storage is provisioned, you can see an icon in the storage list that shows that volume as encrypted.

All encrypted volumes and file shares are provisioned in upgraded data centers only. You can find a full list of upgraded data centers and available features [here](/docs/infrastructure/FileStorage?topic=FileStorage-selectDC).

## Why {{site.data.keyword.filestorage_short}} with an Endurance 10 IOPS tier be provisioned in some data centers and not in others?
{: faq}

The {{site.data.keyword.filestorage_short}} Endurance type 10 IOPS/GB tier is available in select data centers only, and new data centers are going to be added soon. You can find a full list of upgraded data centers and available features [here](/docs/infrastructure/FileStorage?topic=FileStorage-selectDC).

## How can I find the correct mount point for my {{site.data.keyword.filestorage_short}}?
{: faq}

All encrypted {{site.data.keyword.filestorage_short}} volumes that are provisioned in the enhanced data centers have a different mount point than non-encrypted volumes. To ensure that you're using the correct mount point, view the mount point information in the **Volume Details** page in the UI. You can also access the correct mount point through an API call: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

## How many volumes can I provision?
{: faq}

By default, you can provision a combined total of 250 block and file storage volumes. To increase your limit, contact your sales representative. For more information, see [Managing storage limits](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits).

## How many instances can share the use of a provisioned {{site.data.keyword.filestorage_short}} volume?
{: faq}

The default limit for number of authorizations per file volume is 64. To increase this limit, contact your sales representative.

## How many {{site.data.keyword.filestorage_short}} volumes can be attached to a single host?
{: faq}

That depends on what the host operating system can handle, it’s not something that {{site.data.keyword.cloud}} limits. Refer to your OS documentation for limits on the number of file shares that can be mounted.

## How many files/directories are allowed for specific file volume sizes? What is the maximum number of inodes allowed per volume size?
{: #maxfilevolume}
{: faq}

| Volume Size | Inodes |
|-----|-----|
| 20 GB - 39 GB | 622,484 |
| 40 GB - 79 GB | 1,245,084 |
| 80 GB - 99 GB | 2,490,263 |
| 100 GB - 249 GB | 3,112,863 |
| 250 GB - 499 GB | 7,782,300 |
| 500 GB - 999 GB | 15,564,695 |
| 1 TB | 31,876,593 |
| 2 TB | 63,753,186 |
| 3 TB | 95,629,970 |
| 4 TB | 127,506,359 |
{: row-headers}
{: class="comparison-table"}
{: caption="Table comparison" caption-side="top"}
{: summary="Table 1 shows the maximum number of inodes that are allowed based on the volume size. Volume sizes are in the left column. The numbers of inodes (files and directories) are on the right."}

## Measuring IOPS
{: faq}

IOPS is measured based on a load profile of 16-KB blocks with random 50 percent reads and 50 percent writes. Workloads that differ from this profile might experience poor performance.

## What happens when I use a smaller block size for measuring performance?
{: faq}

Maximum IOPS can be obtained even if you use smaller block sizes. However, the throughput is less in this case. For example, a volume with 6000 IOPS has the following throughput at various block sizes:

- 16 KB * 6000 IOPS == ~93.75 MB/sec
- 8 KB * 6000 IOPS == ~46.88 MB/sec
- 4 KB * 6000 IOPS == ~23.44 MB/sec


## Is the allocated IOPS enforced by instance or by volume?
{: faq}

IOPS is enforced at the volume level. Said differently, two hosts connected to a volume with 6000 IOPS share that 6000 IOPS.

## Does the volume need to be pre-warmed to achieve expected throughput?
{: faq}

There's no need for pre-warming. You can observe the specified throughput immediately upon provisioning the volume.

## Can more throughput be achieved if a faster Ethernet connection is used?
{: faq}

Throughput limits are set at a per-volume level. That limit cannot be increased by using a faster Ethernet connection. However, with a slower Ethernet connection, your bandwidth can be a potential bottleneck.

## Do firewalls and security groups impact performance?
{: #isolatedstoragetraffic}
{: faq}

It's best to run storage traffic on a VLAN, which bypasses the firewall. Running storage traffic through software firewalls increases latency and adversely affects storage performance.

## What performance latency can be expected from the {{site.data.keyword.filestorage_short}}?   
{: faq}

Target latency within the storage is  less than one ms. The storage is connected to compute instances on a shared network, so the exact performance latency depends on the network traffic during the operation.

## What happens to the data when {{site.data.keyword.filestorage_short}} Volumes are deleted?
{: faq}

{{site.data.keyword.filestorage_full}} presents file shares to customers on physical storage that is wiped before any reuse. Customers with special requirements for compliance such as NIST 800-88 Guidelines for Media Sanitization need to perform the data sanitization procedure before they delete their storage.

## Which NFS versions are supported?
{: faq}

Both NFS v3 and NFS v4.1 are supported in the {{site.data.keyword.cloud}} environment.

The preferred version is NFS v3 because it's a stateless protocol and more resilient when network events occur.

NFS v3 natively supports `no_root_squash` that allows root clients to retain root permissions on the NFS share. You can enable this feature in NFS v4.1, by editing the domain information and running the `rpcidmapd` or a similar service. For more information, see [Implementing no_root_squash for NFS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux#norootsquash).

When it comes to vSphere Solutions, NFS v3 supports more features than v4.1. Such features include Storage DRS and Site Recovery Manager.

## Can VAAI and HW acceleration be enabled in our VMware deployments?
{: #isVAAIsupported}
{: faq}

No. Currently, vStorage for API Array Integration and Hardware acceleration are not supported.

## What happens to the drives that are decommissioned from the cloud data center?
{: faq}

When drives are decommissioned, IBM destroys them before they are disposed of. The drives become unusable. Any data that was written to that drive becomes inaccessible.

## What is the difference between Controlled Failover and Immediate Failover?
{: faq}

Controlled Failover does one last sync before it breaks the mirror process. The Immediate Failover immediately breaks the mirror and activates the replica volume.

## My storage appears offline or read-only. Why did it happen and how do I fix it?
{: faq}

There are a couple of scenarios where a host (bare metal or VM) loses connection to the storage however briefly and as a result, the host considers that storage read-only to avoid data corruption. Most of the time the loss of connectivity is network-related but the status of the storage remains read-only from the host's perspective even when the network connection is restored.

This issue can be observed with virtual drives of VMs on a network-attached datastore (NFS protocol). To resolve, confirm that the network path between the Storage and the Host is clear, and that there's no maintenance or outage currently in progress. Then, unmount and mount the storage volume. If the volume is still read-only, restart the host.

For mounting instructions, see the following topics.
- [Accessing {{site.data.keyword.filestorage_short}} on Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [Mounting {{site.data.keyword.filestorage_short}} in CentOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [Mounting {{site.data.keyword.filestorage_short}} on CoreOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)
- [Mounting {{site.data.keyword.filestorage_short}} Volume on ESXi hosts](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)

To prevent this situation from recurring, the customer might consider the following:
- Increasing disk timeout values. For more information, see [VMware KB - Increasing the disk timeout values for a Linux 2.6 virtual machine](https://kb.vmware.com/s/article/1009465){: external}.
- Adding guest OS tunings. For more information, see [NetApp's recommendations for guest OS tunings for a VMware vSphere deployment](https://kb.netapp.com/app/answers/answer_view/a_id/1001979){: external}.
- Reconfiguring Host systems that use NFSv4.1 for NFSv3 for increased resilience during maintenance operations.
- Discontinuing session trunking on host systems that run VMware ESXi. Session trunking is not supported and is known to cause disruptions.
