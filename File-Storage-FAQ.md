---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-02"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.filestorage_full_notm}} - Frequently Asked Questions

## How are IOPS measured?

IOPS are measured based on a load profile of 16 KB blocks with random 50% reads and 50% writes. Workloads that differ from this profile may experience lower performance.

## What happens if I use a smaller block size when measuring performance?

Maximum IOPS can still be obtained when using smaller block sizes, however throughput will be lower. For example; a volume with 6000 IOPS would have the following throughput at various block sizes:

- 16 KB * 6000 IOPS == ~93.75 MB/sec
- 8 KB * 6000 IOPS == ~46.88 MB/sec
- 4 KB * 6000 IOPS == ~23.44 MB/sec


## Does the volume need to be pre-warmed to achieve expected throughput?

There is no need for pre-warming. You will observe specified throughput immediately upon provisioning the volume.

## How can I tell which of my {{site.data.keyword.filestorage_short}} LUNs/volumes are encrypted?

When viewing your list of {{site.data.keyword.filestorage_short}} in the customer portal, you will see a lock icon to the right of LUN/volume name for those that are encrypted.

## If I have non-encrypted {{site.data.keyword.filestorage_short}} provisioned in a data center that has been upgraded for encryption, can I encrypt my {{site.data.keyword.filestorage_short}}?

{{site.data.keyword.filestorage_short}} that is provisioned prior to a data center upgrade cannot be encrypted. New {{site.data.keyword.filestorage_short}} provisioned in upgraded data centers is automatically encrypted ; there is no encrypt setting to choose from, it’s automatic.. Data on non-encrypted storage in an upgraded data center can be encrypted by creating a new File volume, then copying the data to the new encrypted volume or volume with host-based migration. See [this article](/docs/infrastructure/FileStorage/migrate-file-storage-encrypted-file-storage.html) for instructions on how to perform the migration.

## How do I know if I am provisioning {{site.data.keyword.filestorage_short}} in an upgraded data center?

When provisioning {{site.data.keyword.filestorage_short}}, all upgraded data centers will be denoted with an asterisk (`*`) in the order form and an indication that you will be provisioning storage with encryption. Once the storage is provisioned, you will see an icon in the storage list that shows that volume or volume as encrypted. All encrypted volumes and volumes are provisioned in upgraded data centers only. You can find a full list of upgraded data centers and available features [here](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html).

## Why can I provision {{site.data.keyword.filestorage_short}} with an Endurance 10 IOPS tier in some data centers and not in others?

The {{site.data.keyword.filestorage_short}} Endurance type 10 IOPS/GB tier is only available in select data centers, with new data centers being added soon.  You can find a full list of upgraded data centers and available features [here](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html).

## How can I find the correct mount point for my {{site.data.keyword.filestorage_short}}?

All encrypted {{site.data.keyword.filestorage_short}} volumes provisioned in these data centers have a different mount point than non-encrypted volumes. To ensure you are using the correct mount point for both your encrypted and non-encrypted {{site.data.keyword.filestorage_short}} volumes you can view the mount point information in the **Volume Details** page in the UI as well as access the correct mountpoint via an API call:  `SoftLayer_Network_Storage::getNetworkMountAddress()`.

## How many file shares are allowed per file volume size? What are the maximum file shares allowed per volume size?
Following are the maximum inodes or file shares allowed based on volume size:

<table>
        <tbody>
          <tr>
            <th>Volume Size</th>
            <th>Inodes/Files</th>
          </tr>
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
            <td>100GB</td>
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

## How many volumes can I provision?

By default, you can provision a combined total of 250 block and file storage volumes.  Please contact your sales representative to increase your volumes.

## How many instances can share the use of a provisioned {{site.data.keyword.filestorage_short}} volume?

The default limit for number of authorizations per file volume is 64. Please contact your sales representative to increase the limit.

## When provisioning Performance or Endurance {{site.data.keyword.filestorage_short}}, are the allocated IOPS enforced by instance or by volume?

IOPS are enforced at the volume level. Said differently, two hosts connected to a volume with 6000 IOPS share that 6000 IOPS.

## Will I be able to achieve more throughput if I used a faster Ethernet connection?

Throughput limits are set at a per-volume/LUN level so using a faster Ethernet connection will not increase that set limit. However, with a slower Ethernet connection, your bandwidth can be a potential bottleneck.

## Will firewalls/security groups impact performance?

We recommend running storage traffic on a vlan which bypasses the firewall as a best practice. Running storage traffic through software firewalls will increase latency and adversely affect storage performance.

## What happens to my data when {{site.data.keyword.filestorage_short}} Volumes are deleted?

When storage is deleted any pointers to the data on that volume are removed thus the data becomes completely inaccessible. If the physical storage is re-provisioned to another account a new set of pointers are assigned. There is no way for the new account to access any data that may have been on the physical storage, the new set of pointers shows all 0's. When new data is written to the volume/LUN, any inaccessible data that still exists gets overwritten. 
