---

copyright:
  years: 2014, 2025
lastupdated: "2025-04-03"

keywords: File Storage for Classic, encryption, security, provisioning, limitations, NFS

subcollection: FileStorage

---
{{site.data.keyword.attribute-definition-list}}

# FAQ for {{site.data.keyword.filestorage_short}}
{: #file-storage-faqs}

## How can I tell which of my {{site.data.keyword.filestorage_short}} volumes are encrypted?
{: faq}
{: #volumeencrypt}
{: support}

Look at your list of {{site.data.keyword.filestorage_short}} in the customer portal. You can see a lock icon next to the volume name for the volumes that are encrypted.

## How can I find the correct mount point for my {{site.data.keyword.filestorage_short}}?
{: #mountpoint}
{: faq}
{: support}

All encrypted {{site.data.keyword.filestorage_short}} volumes that are provisioned in the enhanced data centers have a different mount point than nonencrypted volumes. To make sure that you're using the correct mount point, view the mount point information in the **Volume Details** page in the console. You can also access the correct mount point through an API call: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

## How many volumes can I provision?
{: faq}
{: #provision}
{: support}

By default, you can provision a combined total of 700 Block and {{site.data.keyword.filestorage_short}} volumes. To increase your limit, contact Support. For more information, see [Managing storage limits](/docs/FileStorage?topic=FileStorage-managinglimits).

## How many server instances can share the use of a provisioned {{site.data.keyword.filestorage_short}} volume?
{: faq}
{: #authlimit}
{: support}

The default limit for number of authorizations per file volume is 64. The limit includes all subnet, host, and IP authorizations combined. To increase this limit, contact Support. For more information, see [Creating support cases](/docs/account?topic=account-open-case).

## How many {{site.data.keyword.filestorage_short}} volumes can be attached to a single host?
{: faq}
{: #hostlimit}
{: support}

The number of volumes that can be attached to a single host depends on what the host operating system can handle. {{site.data.keyword.cloud}} does not impose limits on that. Refer to your OS Documentation for limits on the number of file shares that can be mounted.

## How many files and directories are allowed for specific file volume sizes? What is the maximum number of inodes allowed per volume size?
{: #maxfilevolume}
{: faq}
{: support}

The number of files a volume can contain is determined by how many inodes it has. An inode is a data structure that contains information about files. Volumes have both private and public inodes. Public inodes are used for files that are visible to the customer and private inodes are used for files that are used internally by the storage system. You can expect to have an inode for every 32 KB of volume capacity. The setting for maximum number of files is 2 billion. However, this maximum value can be configured only with volumes of 7.8 TB or larger. Any volume of 9,000 GB or larger reaches the maximum limit at 2,040,109,451 inodes.

| Volume Size | Inodes |
|------------:|-------:|
| 20 GB    | 4,980,731 |
| 40 GB    | 9,961,461 |
| 80 GB   | 19,922,935 |
| 100 GB  | 24,903,679 |
| 250 GB  | 62,259,189 |
| 500 GB | 124,518,391 |
| 1,000 GB| 249,036,795 |
| 2,000 GB| 498,073,589 |
| 3,000 GB| 747,110,397 |
| 4,000 GB| 996,147,191 |
| 8,000 GB| 1,992,294,395 |
| 12,000 GB| 2,040,109,451 |
| 16,000 GB| 2,040,109,451 |
{: row-headers}
{: caption="The table shows the maximum number of inodes that are allowed based on the volume size." caption-side="bottom"}
{: summary="Table 1 shows the maximum number of inodes that are allowed based on the volume size. Volume sizes are in the first column. The numbers of inodes (files and directories) are in the second column."}

## I ordered a {{site.data.keyword.filestorage_short}} volume in the wrong data center. Is it possible to move or migrate it to another data center?
{: faq}
{: #movedatacenter}

You need to order a new {{site.data.keyword.filestorage_short}} share in the correct data center, and then cancel the {{site.data.keyword.filestorage_short}} device that you ordered in the incorrect location. You can create a duplicate of your share, and cancel the parent share. For more information, see [Creating and managing duplicate volumes](/docs/FileStorage?topic=FileStorage-duplicatevolume).

When the share is canceled, the request is followed by a 24-hour reclaim wait period. You can still see the storage volume in the console during those 24 hours. Billing for the volume stops immediately. When the reclaim period expires, the data is destroyed and the volume is removed from the console, too.

## Measuring IOPS
{: faq}
{: #iopsmeasure}
{: support}

IOPS is measured based on a load profile of 16-KB blocks with random 50% reads and 50% writes. Workloads that differ from this profile might experience poor performance. To improve performance, you can try adjusting the host settings or [enabling Jumbo frames](/docs/FileStorage?topic=FileStorage-jumboframes).

## What happens when I use a smaller IO size for measuring performance?
{: faq}
{: #smallblock}
{: support}

Maximum IOPS can be obtained even if you use smaller IO sizes. However, the throughput is less in this case. For example, a volume with 6000 IOPS has the following throughput at various IO sizes:

- 16 KB * 6000 IOPS == ~93.75 MB/sec
- 8 KB * 6000 IOPS == ~46.88 MB/sec
- 4 KB * 6000 IOPS == ~23.44 MB/sec

## Is the allocated IOPS enforced by instance or by volume?
{: #iopslimit}
{: faq}
{: support}

IOPS is enforced at the volume level. Said differently, two hosts connected to a volume with 6000 IOPS share that 6000 IOPS.

## Does the volume need to be pre-warmed to achieve the expected throughput?
{: faq}
{: #prewarm}
{: support}

Pre-warming is not needed. You can observe the specified throughput immediately upon provisioning the volume.

## Can more throughput be achieved if a faster Ethernet connection is used?
{: faq}
{: #ethernet}
{: support}

Throughput limits are set at the volume level. That limit cannot be increased by using a faster Ethernet connection. However, with a slower Ethernet connection, your bandwidth can be a potential bottleneck.

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

4. Create a network interface.
   * On the Linux&reg; host, create a 802.11q interface. Choose one of the unused secondary IP address from the newly trunked VLAN and assign that IP address, subnet mask, and gateway to a new 802.11q interface.
   * In VMware&reg;, create a new VMkernel network interface (vmk) and assign the unused secondary IP address, subnet mask, and gateway IP from the newly trunked VLAN to the new vmk interface.
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

## What happens to the data when {{site.data.keyword.filestorage_short}} shares are deleted?
{: faq}
{: #deleted}
{: support}

{{site.data.keyword.filestorage_full}} presents file shares to customers on physical storage that is wiped before any reuse.

When you delete a {{site.data.keyword.filestorage_short}} volume, that data immediately becomes inaccessible. All pointers to the data on the physical disk are removed. If you later create a new volume in the same or another account, a new set of pointers is assigned. The account can't access any data that was on the physical storage because those pointers are deleted. When new data is written to the disk, any inaccessible data from the deleted volume is overwritten.

IBM guarantees that data deleted cannot be accessed and that deleted data is eventually overwritten and eradicated. Further, when you delete a storage volume, the share must be overwritten before the storage is made available again, either to you or to another customer.

When IBM decommissions a physical drive, the drive is destroyed before disposal. The decommissioned drives are unusable and any data on them is inaccessible.

Customers with special requirements for compliance such as NIST 800-88 Guidelines for Media Sanitization can perform the data sanitization procedure before they delete their storage.

## Why is the Cancel action unavailable in the console?
{: faq}
{: #cancelstorage}

The cancellation process for this storage device is in progress so the Cancel action is no longer available. The volume remains visible for at least 24 hours until it is reclaimed. The UI indicates that it’s inactive and the status "Cancellation pending" is displayed. The minimum 24-hour waiting period gives you a chance to void the cancellation request if needed.

## I accidentally deleted my volume, what can I do to get it back?
{: faq}
{: #accidentaldeletion}

The answer depends on how long ago you deleted the storage volume, and if you chose to delete immediately or on anniversary date. If the deletion happened in the last 24 hours or the anniversary date is still yet to come, the volume might still be waiting to be reclaimed. If the volume status is "Cancellation pending", you can contact support to void the cancellation request. It's important to act fast because when the reclaim-period expires, the data is deleted automatically and it is no longer possible to restore.

## Which NFS versions are supported?
{: faq}
{: #nfs}
{: support}

Both NFSv3 and NFSv4.1 are supported in the {{site.data.keyword.cloud}} environment. NFSv4.2 is not supported.

Use the **NFSv3** protocol when possible. NFSv3 supports safe asynchronous writes and is more robust at error handling than the previous NFSv2. It supports 64-bit file sizes and offsets, allowing clients to access more than 2 GB of file data. 

NFSv3 natively supports `no_root_squash` that allows root clients to retain root permissions on the NFS share. You can enable this feature in NFSv4.1, by editing the domain information and running the `rpcidmapd` or a similar service. For more information, see [Implementing no_root_squash for NFS](/docs/FileStorage?topic=FileStorage-mountingLinux#norootsquash).

When {{site.data.keyword.filestorage_short}} is used in a VMware&reg; deployment, NFSv4.1 might be the better choice for your implementation. For more information, see [Best Practices For Running NFS with VMware vSphere](https://www.vmware.com/docs/vmw-best-practices-running-nfs-vmware-vsphere){: external}.

## Can multiple hosts in my VMware deployment with different NFS protocols access the same file share?
{: faq}
{: #vmware-multiple-host-nfs}
{: support}

No. You can't use different NFS versions to mount the same datastore on multiple hosts. Because NFS 3 and NFS 4.1 clients don't use the same locking protocol. Accessing the same virtual disks from two incompatible clients might result in incorrect behavior and cause data corruption. For more information, see [NFS File Locking](https://techdocs.broadcom.com/us/en/vmware-cis/vsphere/vsphere/8-0/vsphere-storage-8-0/working-with-datastores-in-vsphere-storage-environment/nfs-datastore-concepts-and-operations-in-vsphere-environment/guidelines-and-requirements-for-nfs-storage-with-esxi/nfs-file-locking.html){: external}.

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

In a couple of scenarios a host (bare metal or VM) might lose connection to the storage briefly and as a result, the host considers that storage read-only to avoid data corruption. Most of the time the loss of connectivity is network-related but the status of the storage remains read-only from the host's perspective even when the network connection is restored.

This issue can be observed with virtual drives of VMs on a network-attached VMware&reg; datastore (NFS protocol). To resolve, confirm that the network path between the Storage and the Host is clear, and that no maintenance or outage is in progress. Then, unmount and mount the storage volume. If the volume is still read-only, restart the host.

For mounting instructions, see the following topics.
- [Mounting {{site.data.keyword.filestorage_short}} in CentOS](/docs/FileStorage?topic=FileStorage-mountingCentOS)
- [Mounting {{site.data.keyword.filestorage_short}} on ESXi hosts](/docs/FileStorage?topic=FileStorage-architectureguide)
- [Mounting {{site.data.keyword.filestorage_short}} on Red Hat Linux&reg;](/docs/FileStorage?topic=FileStorage-mountingLinux)
- [Mounting {{site.data.keyword.filestorage_short}} on Ubuntu](/docs/FileStorage?topic=FileStorage-mountingUbuntu)

To prevent this situation from recurring, the customer might consider the following actions:
- Adding guest OS tunings. For more information, see [NetApp's recommendations for guest OS tunings for a VMware&reg; vSphere deployment](https://kb.netapp.com/data-mgmt/OTV/VSC_Kbs/What_are_the_guest_OS_tunings_needed_for_a_VMware_vSphere_deployment){: external}.
- Reconfiguring Host systems that use NFSv4.1 for NFSv3 for increased resilience during maintenance operations.
- Discontinuing session trunking on host systems that run VMware&reg; ESXi. Session trunking is not supported and is known to cause disruptions.

## I expanded the volume size of my {{site.data.keyword.filestorage_short}} by using the Cloud console, but the size on my server is still the same. How do I fix it?
{: faq}
{: #expandsize}

To see the expanded volume size, mount and remount your existing {{site.data.keyword.filestorage_short}} disk on your server. In a VMware&reg; implementation, rescan storage to refresh the VMware&reg; datastore and show the new volume size.

## How do I reconnect storage after a chassis swap?
{: faq}
{: #chassis-swap}

Complete the following tasks to connect storage after a swap.
1. Remove the authorization (revoke access) from the storage device, and then authorize the host again.
2. Discover the storage devices again, with the new credentials that were gained from the reauthorization.

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

Endurance and Performance are provisioning options that you can select for storage devices. In short, Endurance IOPS tiers offer predefined performance levels whereas you can fine-tune those levels with the Performance tier. The same devices are used for storage but delivered with different options. For more information, see [File Storage Features](https://www.ibm.com/products/file-storage){: external}.

## Can I connect a {{site.data.keyword.filestorage_short}} share to Windows?
{: faq}
{: #connect-Windows}

No. You cannot mount {{site.data.keyword.filestorage_full}} shares on Microsoft Windows. NFS in a Windows environment is not supported by {{site.data.keyword.cloud}}.

{{site.data.keyword.filestorage_short}} shares can be mounted on Linux operating systems or as a VMware&reg; datastore on ESXi hosts. For more information about mounting {{site.data.keyword.filestorage_short}} volumes, see the following topics:
- [Mounting {{site.data.keyword.filestorage_short}} on Red Hat Linux&reg;](/docs/FileStorage?topic=FileStorage-mountingLinux)
- [Mounting {{site.data.keyword.filestorage_short}} in CentOS](/docs/FileStorage?topic=FileStorage-mountingCentOS)
- [Mounting {{site.data.keyword.filestorage_short}} on Ubuntu](/docs/FileStorage?topic=FileStorage-mountingUbuntu)
- [Mounting {{site.data.keyword.filestorage_short}} as a VMware&reg; datastore on ESXi hosts](/docs/FileStorage?topic=FileStorage-architectureguide)

## Can I mount a single storage device to multiple hosts within {{site.data.keyword.cloud_notm}}?
{: faq}
{: #multiple-hosts}

Yes, you can use this setup because NFS is a file-aware protocol.

## Can I increase inodes for my NFS volume?
{: faq}
{: #inodes}

Typically, when volumes are provisioned, they are allotted the maximum inode count for the size that you ordered. The maximum inode count grows automatically as the volume grows. If the inodes count does not increase after you expanded a volume, submit a [support case](/unifiedsupport/cases/add){: external}.

## I am unable to upgrade storage. What can affect the ability to upgrade or expand storage?
{: faq}
{: #expand-fail}

The following situations can affect the ability to upgrade or expand storage.
- The permissions that you have in the Cloud console can be a factor. For more information, see the topics within [User roles and permissions](/docs/account?topic=account-userroles){: external}.

## Does upgrading my storage affect the data that is on the volume?
{: faq}
{: #capacity-iops-change-vs-data}

No. When you choose to expand your storage volume or adjust its IOPS value, the change has no impact on your data. Nothing is overwritten or wiped during these operations. The adjustment does not cause any kind of outage or lack of access to the storage either.

## Are {{site.data.keyword.filestorage_short}} volumes thin or thick provisioned?
{: faq}
{: #thin}

All Block and {{site.data.keyword.filestorage_short}} services are thin-provisioned. This method is not modifiable.

## My billing ID changed, what does this mean?
{: #staasV2migration}
{: faq}

You might notice that your Storage volumes are now billed as "Endurance Storage Service” or "Performance Storage Service" instead of "Enterprise Storage". You might also have new options in the console, such as the ability to adjust IOPS or increase capacity. {{site.data.keyword.cloud}} strives to continuously improve storage capabilities. As hardware gets upgraded in the data centers, storage volumes that are in those data centers are also upgraded to use all enhanced features. The price that you pay for your Storage volume does not change with this upgrade.

## How durable is {{site.data.keyword.filestorage_short}}?
{: #stordurabilityfaq}
{: faq}

When you store your data in {{site.data.keyword.filestorage_short}}, it's durable, highly available, and encrypted. The durability target for a single Availability zone is 99.999999999% (11 9's). For more information, see [Availability and Durability of {{site.data.keyword.filestorage_short}}](/docs/FileStorage?topic=FileStorage-storageavailability).

## What's the average uptime for {{site.data.keyword.filestorage_short}}?
{: #storavailabilityfaq}
{: faq}

When you store your data in {{site.data.keyword.filestorage_short}}, it's durable, highly available, and encrypted. File Storage is built upon best-in-class, proven, enterprise-grade hardware and software to provide high availability and uptime. To make sure that the availability target of 99.999% (five 9's) is met, the data is stored redundantly across multiple physical disks on HA paired nodes. Each storage node has multiple paths to its own Solid-State Drives and its partner node's SSDs as well. This setup protects against path failure, and also controller failure because the node can still access its partner's disks seamlessly. For more information, see [Availability and Durability of {{site.data.keyword.filestorage_short}}](/docs/FileStorage?topic=FileStorage-storageavailability).

## Can I get storage performance metrics (IOPS or latency) from the Support team?
{: #storagemetrics}
{: faq}

{{site.data.keyword.cloud}} does not provide storage performance IOPS and latency metrics. Customers are expected to monitor their own {{site.data.keyword.filestorage_short}} devices by using their choice of third-party monitoring tools.

The following examples are utilities that you might consider to use to check performance statistics.
- [`sysstat`](https://github.com/sysstat/sysstat/blob/master/README.md){: external} - System performance tools for the Linux&reg; operating system.
- [`typeperf`](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/typeperf){: external} - Windows command that writes performance data to the command window or to a log file.
- [`esxtop`](https://community.broadcom.com/vmware-cloud-foundation/viewdocument/interpreting-esxtop-statistics-1){: external} - A command-line tool that gives administrators real-time information about resource usage in a VMware&reg; vSphere environment. It can monitor and collect data for all system resources: CPU, memory, disk, and network.

## What is the difference between a replica volume, a dependent and an independent duplicate volume?
{: #replicavsduplicate}
{: faq}

You can create a replica or a duplicate volume by using a snapshot of your volume. Replication and cloning use one of your snapshots to copy data to a destination volume. However, that is where the similarities end.

Replication keeps your data in sync in two different locations. Only one volume of the pair (primary volume or replica volume) can be active at a time. The replication process automatically copies information from the active volume to the inactive volume based on the replication schedule. For more information about replica volumes, see [Replicating data](/docs/FileStorage?topic=FileStorage-replication).

Duplication creates a copy of your volume based on a snapshot in the same availability zone as the parent volume. The duplicate volume inherits the capacity and performance options of the original volume by default and has a copy of the data up to the point-in-time of a snapshot. The duplicate volume can be dependent or independent from the original volume, and it can be manually refreshed with data from the parent volume. You can adjust the IOPS or increase the volume size of the duplicate without any effect on the parent volume.

- A dependent duplicate volume does not go through the conversion of becoming independent, and can be refreshed at any time after it is created. It locks the original snapshot so that the snapshot cannot be deleted while the dependent duplicate exists. The parent volume cannot be canceled while the dependent duplicate volume exists. If you want to cancel the parent volume, you must either cancel the dependent duplicate first or convert it to an independent duplicate.

- An independent duplicate is superior to the dependent duplicate in most regards, but it cannot be refreshed immediately after creation because of the lengthy conversion process. It can take up to several hours based on the size of the volume. For example, it might take up to a day for a 12-TB volume. However, after the separation process is complete, the data can be manually refreshed by using another snapshot of the original parent volume.

For more information about duplicates, see [Creating and managing duplicate volumes](/docs/FileStorage?topic=FileStorage-duplicatevolume).

| Feature | Replica | Dependent duplicate | Independent duplicate |
|---------|---------|---------------------|-----------------------|
| Created from a snapshot | ![Checkmark icon.](../../icons/checkmark-icon.svg) | ![Checkmark icon.](../../icons/checkmark-icon.svg) | ![Checkmark icon.](../../icons/checkmark-icon.svg) |
| Location of copied volume | Remote Availability Zone | Same Availability Zone   | Same Availability Zone |
| Supports failover  | ![Checkmark icon.](../../icons/checkmark-icon.svg) |  |  |
| Different Size and IOPS |          | ![Checkmark icon.](../../icons/checkmark-icon.svg) | ![Checkmark icon.](../../icons/checkmark-icon.svg) |
| Auto-synced with parent volume | ![Checkmark icon.](../../icons/checkmark-icon.svg) | |  |
| On-demand refresh from parent volume | | ![Checkmark icon.](../../icons/checkmark-icon.svg) [^depdup] | ![Checkmark icon.](../../icons/checkmark-icon.svg) [^indepdup] |
| Separated from parent volume | | | ![Checkmark icon.](../../icons/checkmark-icon.svg) |
{: caption="Comparison of features between different types of volume copies. " caption-side="bottom"}
{: summary="This table has row and column headers. The row headers identify the capability. The column headers identify the type of volume copy."}
{: #table1}

[^depdup]: Dependent duplicates can be refreshed immediately after creation.

[^indepdup]: Independent duplicates can be refreshed after the separation process completes.

## How long does it take to convert a dependent duplicate into an independent volume?
{: #duplicateconversion}
{: faq}

The conversion process can take some time to complete. The bigger the volume, the longer it takes to convert it. In a 12-TB volume, it might take 24 hours. You can check on the progress in the console or from the CLI.

- In the console, go to [Classic Infrastructure](/gen1/infrastructure/devices){: external}. Click **Storage** > **{{site.data.keyword.filestorage_short}}**, then locate the volume in the list. The conversion status is displayed on the Overview page.

- From the CLI, use the following command.

   ```sh
   slcli file duplicate-convert-status <dependent-vol-id>
   ```

   The output looks similar to the following example.
   ```sh
   slcli file duplicate-convert-status 370597202
   Username            Active Conversion Start Timestamp   Completed Percentage
   SL02SEVC307608_74   2022-06-13 14:59:17                 90
   ```

## How do I manage user permissions and access?
{: #access_mgmt}

The account owner, or a user with the "Manage user classic infrastructure" permission, can adjust the permissions for other users within the IBM Cloud account. If you are not the account owner, you can assign only the level of permissions or a subset of the permission that you're already assigned. In the IBM Cloud console, go to **Manage > Access (IAM) > Users**. Then, select a user's name from the list that you can manage access for, and click **Classic infrastructure**. Select **Account** permissions to allow the user to add and upgrade storage. For more information, see [Managing classic infrastructure access](/docs/account?topic=account-mngclassicinfra).

When the file storage is provisioned, the host servers need to be authorized to be able to mount the file share. Authorization can be set up in the console, from the CLI, with the API, or Terraform. For more information, see Authorizing hosts section in [Managing File Storage for Classic](/docs/FileStorage?topic=FileStorage-managingstorage).

After the host is authorized, you can mount the file share and assign owners to your new folder structure and files. In Linux, you can refine access control by using the `chown` and `chmod` commands to assign read, write, and execute permissions to individual users and groups. For more information, see [Mounting {{site.data.keyword.filestorage_short}} on Red Hat Linux&reg;](/docs/FileStorage?topic=FileStorage-mountingLinux) and [Mounting {{site.data.keyword.filestorage_short}} on Ubuntu](/docs/FileStorage?topic=FileStorage-mountingUbuntu).
