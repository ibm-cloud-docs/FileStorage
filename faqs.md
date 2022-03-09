---

copyright:
  years: 2014, 2022
lastupdated: "2022-03-07"

keywords: File Storage, encryption, security, provisioning, limitations, NFS

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:faq: data-hd-content-type='faq'}
{:support: data-reuse='support'}

# FAQs
{: #file-storage-faqs}

## How can I tell which of my {{site.data.keyword.filestorage_short}} volumes are encrypted?
{: faq}
{: #volumeencrypt}
{: support}

Look at your list of {{site.data.keyword.filestorage_short}} in the customer portal. You can see a lock icon next to the volume name for the volumes that are encrypted.

## If I purchased non-encrypted {{site.data.keyword.filestorage_short}} in a data center that was upgraded for encryption, can I encrypt my {{site.data.keyword.filestorage_short}}?
{: faq}
{: #encryptupgrade}
{: support}

{{site.data.keyword.filestorage_short}} that was provisioned before a data center upgrade can't be encrypted. New {{site.data.keyword.filestorage_short}} that was provisioned in upgraded data centers is automatically encrypted. It's automatic, not a provisioning setting that can be selected or left out. Data on non-encrypted storage can be encrypted by creating a new volume, then copying the data to the new encrypted volume with host-based migration. For more information, see [Migrating {{site.data.keyword.filestorage_short}}](/docs/FileStorage?topic=FileStorage-migratestorage).

## How do I know whether I'm provisioning {{site.data.keyword.filestorage_short}} in an upgraded data center?
{: faq}
{: #upgradedcenter}
{: support}

In the {{site.data.keyword.filestorage_short}} order form, all upgraded data centers are denoted with an asterisk (`*`). During the ordering process, you're given an indication that you're provisioning storage with encryption. When the storage is provisioned, you can see an icon in the storage list that shows that volume as encrypted.

All encrypted volumes and file shares are provisioned in upgraded data centers only. You can find a full list of upgraded data centers and available features [here](/docs/FileStorage?topic=FileStorage-selectDC).

## Why can {{site.data.keyword.filestorage_short}} with an Endurance 10 IOPS tier be provisioned in some data centers and not in others?
{: faq}
{: #orderendurance}
{: support}

The {{site.data.keyword.filestorage_short}} Endurance type 10 IOPS/GB tier is available in most data centers, and new data centers are going to be added soon. You can find a full list of upgraded data centers and available features [here](/docs/FileStorage?topic=FileStorage-selectDC).

## How can I find the correct mount point for my {{site.data.keyword.filestorage_short}}?
{: #mountpoint}
{: faq}
{: support}

All encrypted {{site.data.keyword.filestorage_short}} volumes that are provisioned in the enhanced data centers have a different mount point than non-encrypted volumes. To ensure that you're using the correct mount point, view the mount point information in the **Volume Details** page in the UI. You can also access the correct mount point through an API call: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

## How many volumes can I provision?
{: faq}
{: #provision}
{: support}

By default, you can provision a combined total of 750 Block and {{site.data.keyword.filestorage_short}} volumes. To increase your limit, contact your sales representative. For more information, see [Managing storage limits](/docs/FileStorage?topic=FileStorage-managinglimits).

## How many instances can share the use of a provisioned {{site.data.keyword.filestorage_short}} volume?
{: faq}
{: #authlimit}
{: support}

The default limit for number of authorizations per file volume is 64. To increase this limit, contact your sales representative.

## How many {{site.data.keyword.filestorage_short}} volumes can be attached to a single host?
{: faq}
{: #hostlimit}
{: support}

That depends on what the host operating system can handle, it’s not something that {{site.data.keyword.cloud}} limits. Refer to your OS documentation for limits on the number of file shares that can be mounted.

## How many files/directories are allowed for specific file volume sizes? What is the maximum number of inodes allowed per volume size?
{: #maxfilevolume}
{: faq}
{: support}

The number of files a volume can contain is determined by how many inodes it has. An inode is a data structure that contains information about files. Volumes have both private and public inodes. Public inodes are used for files that are visible to the customer and private inodes are used for files that are used internally by the storage system. The maximum possible number of files setting is 2 billion. However, this maximum value can only be configured with volumes of 7.8 TB or larger.
The maximum number of inodes that can be configured on a volume is calculated by taking the total allocated volume size in KB and dividing it by 4. Any volume of 7.8 TB or larger reaches the maximum limit at 2,040,109,451 inodes.

| Volume Size | Inodes |
|-----|-----|
| 20 GB | 5,242,880 |
| 40 GB | 10,485,760 |
| 80 GB | 20,971,520 |
| 100 GB | 26,214,400  |
| 500 GB | 131,072,000 |
| 1 TB | 268,435,456 |
| 4 TB | 1,073,741,824 |
| 8 TB | 2,040,109,451 |
| 12 TB| 2,040,109,451 |
{: row-headers}
{: class="comparison-table"}
{: caption="Table comparison" caption-side="top"}
{: summary="Table 1 shows the maximum number of inodes that are allowed based on the volume size. Volume sizes are in the left column. The numbers of inodes (files and directories) are on the right."}

## I ordered a {{site.data.keyword.filestorage_short}} volume in the wrong data center. Is it possible to move or migrate it to another data center?
{: faq}
{: #movedatacenter}

You need to order new {{site.data.keyword.filestorage_short}} in the right data center, and then cancel the {{site.data.keyword.filestorage_short}} device you ordered in the incorrect location. When the volume is canceled, there's a 24-hour reclaim wait period. You can still see the volume in the console during those 24 hours. Billing for the volume stops immediately. When the reclaim-period expires, the data is destroyed and the volume is removed from the console, too.

## Measuring IOPS
{: faq}
{: #iopsmeasure}
{: support}

IOPS is measured based on a load profile of 16-KB blocks with random 50 percent reads and 50 percent writes. Workloads that differ from this profile might experience poor performance.

## What happens when I use a smaller block size for measuring performance?
{: faq}
{: #smallblock}
{: support}

Maximum IOPS can be obtained even if you use smaller block sizes. However, the throughput is less in this case. For example, a volume with 6000 IOPS has the following throughput at various block sizes:

- 16 KB * 6000 IOPS == ~93.75 MB/sec
- 8 KB * 6000 IOPS == ~46.88 MB/sec
- 4 KB * 6000 IOPS == ~23.44 MB/sec


## Is the allocated IOPS enforced by instance or by volume?
{: #iopslimit}
{: faq}
{: support}

IOPS is enforced at the volume level. Said differently, two hosts connected to a volume with 6000 IOPS share that 6000 IOPS.

## Does the volume need to be pre-warmed to achieve expected throughput?
{: faq}
{: #prewarm}
{: support}

There's no need for pre-warming. You can observe the specified throughput immediately upon provisioning the volume.

## Can more throughput be achieved if a faster Ethernet connection is used?
{: faq}
{: #ethernet}
{: support}

Throughput limits are set at a per-volume level. That limit cannot be increased by using a faster Ethernet connection. However, with a slower Ethernet connection, your bandwidth can be a potential bottleneck.

## Do firewalls and security groups impact performance?
{: #isolatedstoragetraffic}
{: faq}
{: support}

It's best to run storage traffic on a VLAN, which bypasses the firewall. Running storage traffic through software firewalls increases latency and adversely affects storage performance.

## How do I route {{site.data.keyword.filestorage_short}} traffic to its own VLAN interface and bypass a firewall?
{: faq}
{: #howtoisolatedstorage}

To enact this good practice, complete the following steps.
1. Provision a VLAN in the same data center as the host and the {{site.data.keyword.filestorage_short}} device.
2. Provision a secondary private subnet to the new VLAN.
3. Trunk the new VLAN to the private interface of the host.  
   This action momentarily disrupts the network traffic on the host while the VLAN is being trunked to the host.
   {: note}
   
4. Create a new network interface.
   * On the Linux&reg; host, create a 802.11q interface. Choose one of the unused secondary IP address from the newly trunked VLAN and assign that IP address, subnet mask, and gateway to a new 802.11q interface.
   * In VMware, create a new VMkernel network interface (vmk) and assign the unused secondary IP address, subnet mask, and gateway IP from the newly trunked VLAN to the new vmk interface.
5. Add a new persistent static route on the host to the target NFS subnet.
6. Authorize the new IP to access the storage.
7. For mounting instructions, depending on your host's operating system, follow the appropriate link.
   - [Accessing {{site.data.keyword.filestorage_short}} on Linux&reg;](/docs/FileStorage?topic=FileStorage-mountingLinux)
   - [Mounting {{site.data.keyword.filestorage_short}} in CentOS](/docs/FileStorage?topic=FileStorage-mountingCentOS)
   - [Mounting {{site.data.keyword.filestorage_short}} Volume on ESXi hosts](/docs/FileStorage?topic=FileStorage-architectureguide)

## What performance latency can be expected from the {{site.data.keyword.filestorage_short}}?   
{: faq}
{: #latency}
{: support}

Target latency within the storage is less than one ms. The storage is connected to compute instances on a shared network, so the exact performance latency depends on the network traffic during the operation.

## What happens to the data when {{site.data.keyword.filestorage_short}} Volumes are deleted?
{: faq}
{: #deleted}
{: support}

{{site.data.keyword.filestorage_full}} presents file shares to customers on physical storage that is wiped before any reuse.

When you delete a {{site.data.keyword.filestorage_short}} volume, that data immediately becomes inaccessible. All pointers to the data on the physical disk are removed. If you later create a new volume in the same or another account, a new set of pointers is assigned. The account can't access any data that was on the physical storage because those pointers are deleted. When new data is written to the disk, any inaccessible data from the deleted volume is overwritten.

IBM guarantees that data deleted cannot be accessed and that deleted data is eventually overwritten and eradicated. Further, when you delete a storage volume, the share must be overwritten before the storage is made available again, either to you or to another customer.

When IBM decommissions a physical drive, the drive is destroyed before disposal. The decommissioned drives are unusable and any data on them is inaccessible.

Customers with special requirements for compliance such as NIST 800-88 Guidelines for Media Sanitization can perform the data sanitization procedure before they delete their storage.

## I cannot cancel a {{site.data.keyword.filestorage_short}} volume because the Cancel action in the Cloud console is unavailable or grayed out. What’s happening?
{: faq}
{: #cancelstorage}

The cancellation process for this storage device is in progress so the Cancel action is no longer available. The volume remains visible for at least 24 hours until it’s reclaimed, with an hourglass or clock icon next to the device name to indicate that it’s in a waiting period. The minimum 24-hour waiting period gives you a chance to void the cancel request if needed.

## Which NFS versions are supported?
{: faq}
{: #nfs}
{: support}

Both NFS v3 and NFS v4.1 are supported in the {{site.data.keyword.cloud}} environment.

The preferred version is NFS v3 because it's a stateless protocol and more resilient when network events occur.

NFS v3 natively supports `no_root_squash` that allows root clients to retain root permissions on the NFS share. You can enable this feature in NFS v4.1, by editing the domain information and running the `rpcidmapd` or a similar service. For more information, see [Implementing no_root_squash for NFS](/docs/FileStorage?topic=FileStorage-mountingLinux#norootsquash).

When it comes to vSphere Solutions, NFS v3 supports more features than v4.1. Such features include Storage DRS and Site Recovery Manager.

## Can VAAI and HW acceleration be enabled in our VMware deployments?
{: #isVAAIsupported}
{: faq}
{: support}

No. Currently, vStorage for API Array Integration and Hardware acceleration are not supported.

## What happens to the drives that are decommissioned from the cloud data center?
{: faq}
{: #decommission}
{: support}

When drives are decommissioned, IBM destroys them before they are disposed of. The drives become unusable. Any data that was written to that drive becomes inaccessible.

## What is the difference between Controlled Failover and Immediate Failover?
{: faq}
{: #failover}
{: support}

Controlled Failover does one last sync before it breaks the mirror process. The Immediate Failover immediately breaks the mirror and activates the replica volume.

## My storage appears offline or read-only. Why did it happen and how do I fix it?
{: #StorageOffline}
{: faq}
{: support}

There are a couple of scenarios where a host (bare metal or VM) loses connection to the storage however briefly and as a result, the host considers that storage read-only to avoid data corruption. Most of the time the loss of connectivity is network-related but the status of the storage remains read-only from the host's perspective even when the network connection is restored.

This issue can be observed with virtual drives of VMs on a network-attached datastore (NFS protocol). To resolve, confirm that the network path between the Storage and the Host is clear, and that there's no maintenance or outage currently in progress. Then, unmount and mount the storage volume. If the volume is still read-only, restart the host.

For mounting instructions, see the following topics.
- [Mounting {{site.data.keyword.filestorage_short}} in CentOS](/docs/FileStorage?topic=FileStorage-mountingCentOS)
- [Mounting {{site.data.keyword.filestorage_short}} on ESXi hosts](/docs/FileStorage?topic=FileStorage-architectureguide)
- [Mounting {{site.data.keyword.filestorage_short}} on Red Hat Linux&reg;](/docs/FileStorage?topic=FileStorage-mountingLinux)
- [Mounting {{site.data.keyword.filestorage_short}} on Ubuntu](/docs/FileStorage?topic=FileStorage-mountingUbuntu)

To prevent this situation from recurring, the customer might consider the following:
- Increasing disk timeout values. For more information, see [VMware KB - Increasing the disk timeout values for a Linux&reg; 2.6 virtual machine](https://kb.vmware.com/s/article/1009465){: external}.
- Adding guest OS tunings. For more information, see [NetApp's recommendations for guest OS tunings for a VMware vSphere deployment](https://kb.netapp.com/Advice_and_Troubleshooting/Data_Storage_Software/Virtual_Storage_Console_for_VMware_vSphere/What_are_the_guest_OS_tunings_needed_for_a_VMware_vSphere_deployment%3F){: external}.
- Reconfiguring Host systems that use NFSv4.1 for NFSv3 for increased resilience during maintenance operations.
- Discontinuing session trunking on host systems that run VMware ESXi. Session trunking is not supported and is known to cause disruptions.

## I expanded the volume size of my {{site.data.keyword.filestorage_short}} by using the Cloud console, but the size on my server is still the same. How do I fix it?
{: faq}
{: #expandsize}

To see the expanded volume size, mount and remount your existing {{site.data.keyword.filestorage_short}} disk on your server. In VMware, rescan storage to refresh the datastore and show the new volume size.

## How do I re-connect storage after a chassis swap?
{: faq}
{: #chassis-swap}

Complete the following tasks to connect storage after a swap.
1. Remove the authorization (revoke access) from the storage devices, and then authorize the host again.
2. Discover the storage devices again, with the new credentials that were gained from the re-authorization.

For more information, see [Managing {{site.data.keyword.filestorage_short}}](/docs/FileStorage?topic=FileStorage-managingstorage).

## How do I disconnect my storage device from a host?
{: faq}
{: #disconnect}

Complete the following steps to disconnect a volume from a host.
1. Unmount the device.
2. Revoke access for the host from the storage device in the Cloud console.
3. Remove auto mounts from NFS connections.

## How do endurance and performance storage differ?
{: faq}
{: #tier-options}

Endurance and Performance are provisioning options that you can select for storage devices. In short, Endurance IOPS tiers offer predefined performance levels whereas you can fine-tune those levels with the Performance tier. The same devices are used but delivered with different options. For more information, see [File Storage Features](https://www.ibm.com/cloud/file-storage/details){: external}.

## Can I connect {{site.data.keyword.filestorage_short}} to Windows?
{: faq}
{: #connect-Windows}

No. You cannot connect {{site.data.keyword.filestorage_full}} on Microsoft Windows. NFS is not supported by {{site.data.keyword.cloud}} in a Windows environment.

## Can I mount a single storage device to multiple hosts within {{site.data.keyword.cloud_notm}}?
{: faq}
{: #multiple-hosts}

Yes, you can use this setup because NFS is a file-aware protocol.

## Can I increase inodes for my NFS volume?
{: faq}
{: #inodes}

Typically, when volumes are provisioned, they are allotted the maximum inode count for the size that you ordered. Maximum inode count grows automatically as the volume grows. If the inodes counts does not increase after you expanded a volume, submit a [support case](https://cloud.ibm.com/unifiedsupport/cases/add){: external}.

## I am unable to upgrade storage. What can affect the ability to upgrade or expand storage?
{: faq}
{: #expand-fail}

The following situations can affect the ability to upgrade or expand storage.
- If the original volume is the Endurance 0.25 tier, then the IOPS tier can’t be updated.
- Older storage types can't be upgraded. Ensure that the storage was ordered in an upgraded Data Center that allows for [Expandable storage](/docs/FileStorage?topic=FileStorage-expandCapacity).
- The permissions that you have in the Cloud console can be a factor. For more information, see the topics within [User roles and permissions](/docs/account?topic=account-userroles){: external}.

## Are {{site.data.keyword.filestorage_short}} volumes thin or thick provisioned?
{: faq}
{: #thin}

All Block and {{site.data.keyword.filestorage_short}} services are thin-provisioned. This method is not modifiable.

## My billing ID has changed, what does this mean?
{: #staasV2migration}
{: faq}

You might notice that your Storage volumes are now billed as "Endurance Storage Service” or "Performance Storage Service" instead of "Enterprise Storage", and you have new options in the console, such as the ability to adjust IOPS or increase capacity. {{site.data.keyword.cloud}} strives to continously improve storage capabilities. As hardware gets upgraded in the datacenters, storage volumes that reside in those datacenters are also upgraded to utilize all enhanced features. The price you pay for your Storage volume does not change with this upgrade.

## How durable is {{site.data.keyword.filestorage_short}}?
{: #stordurability}
{: faq}

When you store your data in {{site.data.keyword.filestorage_short}}, it's durable, highly available, and encrypted. The durability target for a single Availability zone is 99.999999999% (11 9's). For more information, see [Availability and Durability of {{site.data.keyword.filestorage_short}}](/docs/FileStorage?topic=FileStorage-storageavailability).

## What's the average uptime for {{site.data.keyword.filestorage_short}}?
{: #storavailability}
{: faq}

When you store your data in {{site.data.keyword.filestorage_short}}, it's durable, highly available, and encrypted. File Storage is built upon best-in-class, proven, enterprise-grade hardware and software to ensure high availability and uptime. The ensure that the availability target of 99.999% (five 9's) is met, the data is stored redundantly across multiple physical disks on HA paired nodes. Each storage node has multiple paths to its own Solid-State Drives and its partner node's SSDs as well. This protects against path failure, and also controller failure because the node can still access its partner's disks seamlessly. For more information, see [Availability and Durability of {{site.data.keyword.filestorage_short}}](/docs/FileStorage?topic=FileStorage-storageavailability).
