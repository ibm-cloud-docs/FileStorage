---

copyright:
  years: 2014, 2018
lastupdated: "2018-08-21"

---
{:new_window: target="_blank"}

# FAQs

## How can I tell which of my {{site.data.keyword.filestorage_short}} volumes are encrypted?
Look at your list of {{site.data.keyword.filestorage_short}} in the customer portal. You can see a lock icon to the right of the LUN/volume name for the volumes that are encrypted.

## If I purchased non-encrypted {{site.data.keyword.filestorage_short}} in a data center that was upgraded for encryption, can I encrypt my {{site.data.keyword.filestorage_short}}?
{{site.data.keyword.filestorage_short}} that was provisioned before a data center upgrade can't be encrypted. New {{site.data.keyword.filestorage_short}} provisioned in upgraded data centers is automatically encrypted. There's no encrypt setting to choose from, it’s automatic. Data on non-encrypted storage can be encrypted by creating a new volume, then copying the data to the new encrypted volume with host-based migration. For more information, see [Migrating File Storage](/docs/infrastructure/FileStorage/migrate-file-storage-encrypted-file-storage.html).

## How do I know whether I'm provisioning {{site.data.keyword.filestorage_short}} in an upgraded data center?
In the {{site.data.keyword.filestorage_short}} order form, all upgraded data centers are denoted with an asterisk (`*`). During the ordering process, you're given an indication that you're provisioning storage with encryption. When the storage is provisioned, you can see an icon in the storage list that shows that volume as encrypted. 

All encrypted volumes and file shares are provisioned in upgraded data centers only. You can find a full list of upgraded data centers and available features [here](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html).

## Why {{site.data.keyword.filestorage_short}} with an Endurance 10 IOPS tier be provisioned in some data centers and not in others?
The {{site.data.keyword.filestorage_short}} Endurance type 10 IOPS/GB tier is available in select data centers only, and new data centers are going to be added soon. You can find a full list of upgraded data centers and available features [here](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html).

## How can I find the correct mount point for my {{site.data.keyword.filestorage_short}}?
All encrypted {{site.data.keyword.filestorage_short}} volumes that are provisioned in the enhanced data centers have a different mount point than non-encrypted volumes. To ensure that you're using the correct mount point, view the mount point information in the **Volume Details** page in the UI. You can also access the correct mount point through an API call: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

## How many volumes can I provision?
By default, you can provision a combined total of 250 block and file storage volumes. To increase your limit, contact your sales representative.

## How many instances can share the use of a provisioned {{site.data.keyword.filestorage_short}} volume?
The default limit for number of authorizations per file volume is 64. To increase this limit, contact your sales representative.

## How many {{site.data.keyword.filestorage_short}} volumes can be attached to a single host?
That depends on what the host operating system can handle, it’s not something that {{site.data.keyword.BluSoftlayer_full}} limits. Refer to your OS documentation for limits on the number of file shares that can be mounted.

## How many file shares are allowed per file volume size? What are the maximum file shares allowed per volume size?

<table>
  <caption>Table 1 shows that the maximum number of inodes allowed based on the volume size. Volume sizes are in the left. The number of inodes/file shares are on the right.</caption>
  <thead>
    <tr>
      <th>Volume Size</th>
      <th>Inodes/File shares</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>20 GB </td>
      <td>622,484</td>
    </tr>
    <tr>
      <td>40 GB </td>
      <td>1,245,084</td>
    </tr>          
    <tr>
      <td>80 GB</td>
      <td>2,490,263</td>
    </tr>          
    <tr>
      <td>100 GB</td>
      <td>3,112,863</td>
    </tr>          
    <tr>
      <td>250 GB</td>
      <td>7,782,300</td>
    </tr>          
    <tr>
      <td>500 GB</td>
      <td>15,564,695</td>
    </tr>
    <tr>
      <td>1 TB+</td>
      <td>31,876,593</td>
    </tr>
   </tbody>
</table>

## Measuring IOPS
IOPS is measured based on a load profile of 16 KB blocks with random 50 percent reads and 50 percent writes. Workloads that differ from this profile might experience poor performance.

## What happens when I use a smaller block size for measuring performance?
Maximum IOPS can be obtained even if you use smaller block sizes. However, the throughput is less in this case. For example, a volume with 6000 IOPS has the following throughput at various block sizes:

- 16 KB * 6000 IOPS == ~93.75 MB/sec
- 8 KB * 6000 IOPS == ~46.88 MB/sec
- 4 KB * 6000 IOPS == ~23.44 MB/sec


## Is the allocated IOPS enforced by instance or by volume?
IOPS is enforced at the volume level. Said differently, two hosts connected to a volume with 6000 IOPS share that 6000 IOPS.

## Does the volume need to be pre-warmed to achieve expected throughput?
There's no need for pre-warming. You can observe the specified throughput immediately upon provisioning the volume.

## Can more throughput be achieved if a faster Ethernet connection is used?
Throughput limits are set at a per-volume/LUN level so using a faster Ethernet connection doesn't increase that set limit. However, with a slower Ethernet connection, your bandwidth can be a potential bottleneck.

## Do firewalls/security groups impact performance?
It's best to run storage traffic on a VLAN, which bypasses the firewall. Running storage traffic through software firewalls increases latency and adversely affects storage performance.

## What performance latency can be expected from the {{site.data.keyword.filestorage_short}}?   
Target latency within the storage is <1ms. The storage is connected to compute instances on a shared network, so the exact performance latency depends on the network traffic during the operation.

## What happens to the data when {{site.data.keyword.filestorage_short}} Volumes are deleted?
When storage is deleted, any pointers to the data on that volume are removed thus the data becomes inaccessible. If the physical storage is reprovisioned to another account a new set of pointers are assigned. There’s no way for the new account to access any data that was on the physical storage. The new set of pointers shows all 0's. The new data overwrites any inaccessible data that existed on that physical storage.

## What happens to the drives that are decommissioned from the cloud data center?
When drives are decommissioned, IBM destroys them before they are disposed of. The drives become unusable. Any data that was written to that drive becomes inaccessible.
