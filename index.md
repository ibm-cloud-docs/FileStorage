---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-27"

keywords: File Storage, Endurance, Performance, IOPS, replication, billing, file storage, NFS,

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
 {:shortdesc: .shortdesc}


# About {{site.data.keyword.filestorage_short}}
{: #about}

{{site.data.keyword.filestorage_full}} is persistent, fast, and flexible network-attached, NFS-based {{site.data.keyword.filestorage_short}}. In this network-attached storage (NAS) environment, you have total control over your file shares function and performance. {{site.data.keyword.filestorage_short}} shares can be connected to up to 64 authorized devices over routed TCP/IP connections for resiliency.
{:shortdesc}

## Features
{: #FileStorageFeatures}

{{site.data.keyword.filestorage_short}} brings best-in-class levels of durability and availability with an unmatched feature set. {{site.data.keyword.filestorage_short}} is built by using industry standards and best practices, and designed to protect the integrity of the data. {{site.data.keyword.filestorage_short}} maintains availability through maintenance events and unplanned failures, and provides a consistent performance baseline.

Take advantage of the following core features of {{site.data.keyword.filestorage_short}}:

- **Consistent performance baseline**
   - Provided through the allocation of protocol-level input/output operations per second (IOPS) to individual volumes.
- **{{site.data.keyword.filestorage_short}}**
   - Available for file-based NFS shares.
- **Highly durable and resilient**
   - Protects the integrity of the data, and maintains availability through maintenance events and unplanned failures without the need to create and manage operating system-level redundant array of independent disk (RAID) arrays
- **Data-At-Rest Encryption** [(Available in select data centers.)](/docs/infrastructure/FileStorage?topic=FileStorage-news)
   - Provider-managed encryption for data-at-rest is provided at no additional cost
- **All Flash Backed Storage** [(Available in select data centers.)](/docs/infrastructure/FileStorage?topic=FileStorage-news)
   - All flash storage for volumes can be provisioned at 2 IOPS/GB or higher levels.
- **Snapshots** [(Available in select data centers.)](/docs/infrastructure/FileStorage?topic=FileStorage-news).
   - Captures point-in-time data snapshots non-disruptively.
- **Replication**  [(Available in select data centers.)](/docs/infrastructure/FileStorage?topic=FileStorage-news)
   - Available when storage is provisioned in [select data centers](/docs/infrastructure/FileStorage?topic=FileStorage-news).
   - Automatically copies snapshots to a partner {{site.data.keyword.BluSoftlayer_full}} data center.
- **Highly available connectivity**
   - Uses redundant networking connections to maximize availability.
   - NFS-based {{site.data.keyword.filestorage_short}} routed TCP/IP connections.
- **Concurrent access**
   - Allows multiple hosts (up to 64) to simultaneously access file volumes.
- **Clustered databases**
   - Supports advanced use cases, such as clustered databases.

## Billing

You can select hourly or monthly billing for a File volume. The type of billing that is selected for a LUN applies to its snapshot space and replicas. For example, if you provision a LUN with hourly billing, any snapshots or replica fees are billed hourly. If you provision a LUN with monthly billing, any snapshots or replica fees are billed monthly.

With **hourly billing**, the number of hours that the File volume existed on the account is calculated at the time the LUN is deleted or at the end of the billing cycle, which ever comes first. Hourly billing is a good choice for storage that is used for a few days or less than a full month. Hourly billing is available for storage that is provisioned in [select data centers](/docs/infrastructure/FileStorage?topic=FileStorage-news) only.

With **monthly billing**, the calculation for the price is pro-rated from the date of creation to the end of the billing cycle and billed immediately. If a volume is deleted before the end of the billing cycle, there's no refund. Monthly billing is a good choice for storage that is used in production workloads that use data that needs to be stored and accessed for long periods of time (one month or longer).


**Performance**
<table>
  <caption>Table 1 is showing the prices for Performance Storage with monthly and hourly billing.</caption>
  <tr>
   <th>Monthly Price</th>
   <td>$0.10/GB + $0.07/IOP</td>
  </tr>
  <tr>
   <th>Hourly Price</th>
   <td>$0.0001/GB + $0.0002/IOP</td>
  </tr>
</table>

**Endurance**
<table>
  <caption>Table 2 is showing the prices for Endurance Storage for each tier with monthly and hourly billing options.</caption>
  <tr>
   <th>IOPS Tier</th>
   <th>0.25 IOPS/GB</th>
   <th>2 IOPS/GB</th>
   <th>4 IOPS/GB</th>
   <th>10 IOPS/GB</th>
  </tr>
  <tr>
   <th>Monthly Price</th>
   <td>$0.06/GB</td>
   <td>$0.15/GB</td>
   <td>$0.20/GB</td>
   <td>$0.58/GB</td>
  </tr>
  <tr>
   <th>Hourly Price</th>
   <td>$0.0001/GB</td>
   <td>$0.0002/GB</td>
   <td>$0.0003/GB</td>
   <td>$0.0009/GB</td>
  </tr>
</table>



## Provisioning

{{site.data.keyword.filestorage_short}} volumes can be provisioned from 20 GB to 12 TB with two options: <br/>
- Provision **Endurance** tiers that feature pre-defined performance levels and other features like snapshots and replication.
- Build a high-powered **Performance** environment with allocated input/output operations per second (IOPS).


### Provisioning with Endurance Tiers

Endurance {{site.data.keyword.filestorage_short}} is available in four IOPS performance tiers to support varying application needs. <br />

- **0.25 IOPS per GB** is designed for workloads with low I/O intensity. These workloads are typically characterized by having a large percentage of data inactive at any time. Example applications include storing mailboxes or departmental level file shares.

- **2 IOPS per GB** is designed for most general-purpose usage. Example applications include hosting small databases that are backing web applications or VM disk images for a hypervisor.

- **4 IOPS per GB** is designed for higher-intensity workloads. These workloads are typically characterized by having a high percentage of data active at any time. Example applications include transactional and other performance-sensitive databases.

- **10 IOPS per GB** is designed for the most demanding workloads such as those created by NoSQL databases, and data processing for Analytics. This tier is available for storage that is provisioned up to 4 TB in [select data centers](/docs/infrastructure/FileStorage?topic=FileStorage-news) only.

Up to 48,000 IOPS are available with a 12-TB Endurance volume.

Choosing the right Endurance tier for your workload is key. It's equally important to use the right block size, Ethernet connection speed, and the number of hosts necessary to achieve maximum performance. If any of these parts don't align with the other, it can have a significant impact on the resulting throughput.

### Provisioning with Performance

Performance is a class of {{site.data.keyword.filestorage_short}} that is designed to support high I/O applications with understood performance requirements that don't fit well within an Endurance tier. Predictable performance is achieved through the allocation of protocol-level IOPS to individual volumes. Various IOPS rates (100 - 48,000) can be provisioned with storage sizes that range from 20 GB to 12 TB.

Performance for {{site.data.keyword.filestorage_short}} is accessed and mounted through a Network file system (NFS)  connection. {{site.data.keyword.filestorage_short}} is typically used when the volume is accessed by multiple servers simultaneously. Consistent Performance volumes can be ordered according to the Sizes and IOPS in Table 1 and can be used with Linux operating systems.

<table cellpadding="1" cellspacing="1" style="width: 99%;">
 <caption>Table 3 is showing size and IOPS combinations for Performance storage.<br/><sup><img src="/images/numberone.png" alt="Footnote" /></sup> IOPS Limits greater than 6,000 are available in select data centers.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
          <tr>
            <th>Size (GB)</th>
            <th>Min IOPS</th>
            <th>Max IOPS</th>
          </tr>
          <tr>
            <td>20</td>
            <td>100</td>
            <td>1,000</td>
          </tr>
          <tr>
            <td>40</td>
            <td>100</td>
            <td>2,000</td>
          </tr>
          <tr>
            <td>80</td>
            <td>100</td>
            <td>4,000</td>
          </tr>
          <tr>
            <td>100</td>
            <td>100</td>
            <td>6,000</td>
          </tr>
          <tr>
            <td>250</td>
            <td>100</td>
            <td>6,000</td>
          </tr>
          <tr>
            <td>500</td>
            <td>100</td>
            <td>6,000 or 10,000 <sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
          <tr>
            <td>1,000</td>
            <td>100</td>
            <td>6,000 or 20,000 <sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
          <tr>
            <td>2,000-3,000</td>
            <td>200</td>
            <td>6,000 or 40,000 <sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
          <tr>
            <td>4,000-7,000</td>
            <td>300</td>
            <td>6,000 or 48,000 <sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
          <tr>
            <td>8,000-9,000</td>
            <td>500</td>
            <td>6,000 or 48,000 <sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
          <tr>
            <td>10,000-12,000</td>
            <td>1,000</td>
            <td>6,000 or 48,000 <sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
</table>


Performance volumes are designed to operate consistently close to the provisioned IOPS level. Consistency makes it easier to size and scale application environments with a specific level of performance. Additionally, it's possible to optimize an environment by building a volume with the ideal price-to-performance ratio.
