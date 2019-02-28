---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-28"

keywords:

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
 {:shortdesc: .shortdesc}


# Getting started tutorial
{: #gettingstarted}

{{site.data.keyword.filestorage_full}} is persistent, fast, and flexible network-attached, NFS-based {{site.data.keyword.filestorage_short}}. In this network-attached storage (NAS) environment, you have total control over your file shares function and performance. {{site.data.keyword.filestorage_short}} shares can be connected to up to 64 authorized devices over routed TCP/IP connections for resiliency.
{:shortdesc}

## Before you begin
{: #prereqs}

{{site.data.keyword.filestorage_short}} volumes can be provisioned from 20 GB to 12 TB with two options: <br/>
- Provision **Endurance** tiers that feature pre-defined performance levels and other features like snapshots and replication.
- Build a high-powered **Performance** environment with allocated input/output operations per second (IOPS).

For more information about the {{site.data.keyword.filestorage_short}} offering, see [About {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-about)

### Provisioning considerations

**Block size**

IOPS for both Endurance and Performance is based on a 16-KB block size with a 50/50 read/write 50 percent random workload. A 16-KB block is the equivalent of one write to the volume.
{:important}

The block size that is used by your application directly impacts the storage performance. If the block size that is used by your application is smaller than 16 KB, the IOPS limit is realized before the throughput limit. Conversely, if the block size that is used by your application is larger than 16 KB, the throughput limit is realized before to the IOPS limit.

<table>
  <caption>Table 4 shows examples of how block size and IOPS affect the throughput.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <thead>
          <tr>
            <th>Block Size (KB)</th>
            <th>IOPS</th>
            <th>Throughput (MB/s)</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>4 (typical for Linux)</td>
            <td>1,000</td>
            <td>4</td>
          </tr>
          <tr>
            <td>8 (typical for Oracle)</td>
            <td>1,000</td>
            <td>8</td>
          </tr>
          <tr>
            <td>16</td>
            <td>1,000</td>
            <td>16</td>
          </tr>
          <tr>
            <td>32 (typical for SQL Server)</td>
            <td>500</td>
            <td>16</td>
          </tr>          
          <tr>
            <td>64</td>
            <td>250</td>
            <td>16</td>
          </tr>
          <tr>
            <td>128</td>
            <td>128</td>
            <td>16</td>
          </tr>
          <tr>
            <td>512</td>
            <td>32</td>
            <td>16</td>
          </tr>
        </tbody>
</table>

**Authorized hosts**

Another factor to consider is the number of hosts that are using your volume. If there's a single host that is accessing the volume, it can be difficult to realize the maximum IOPS available, especially at extreme IOPS counts (10,000s). If your workload requires high throughput, it would be best to configure at least a couple servers to access your volume to avoid a single-server bottleneck.

**Network connection**

The speed of your Ethernet connection must be faster than the expected maximum throughput from your volume. Generally, don't expect to saturate your Ethernet connection beyond 70% of the available bandwidth. For example, if you have 6,000 IOPS and are using a 16-KB block size, the volume can handle approximately 94-MBps throughput. If you have a 1-Gbps Ethernet connection to your LUN, it becomes a bottleneck when your servers attempt to use the maximum available throughput. It's because 70 percent of the theoretical limit of a 1-Gbps Ethernet connection (125 MB per second) would allow for 88 MB per second only.

To achieve maximum IOPS, adequate network resources need to be in place. Other considerations include private network usage outside of storage and host side and application-specific tunings (IP stack or [queue depths](/docs/infrastructure/FileStorage?topic=FileStorage-hostqueuesettings), and other settings).

Storage traffic is included in the total network usage of Public Virtual Servers. For more information about the limits that might be imposed by the service, see the [Virtual Server documentation](/docs/vsi?topic=virtual-servers-about-public-virtual-servers).

**NFS version**

Both NFS v3 and NFS v4.1 are supported in the {{site.data.keyword.BluSoftlayer_full}} environment. However, NFS v3 is preferred because NFS v4.1 is a stateful protocol (not stateless like NFSv3) and protocol issues can occur during network events. NFS v4.1 must quiesce all operations and then complete lock reclamation. On a relatively busy NFS file server, the increased latency can cause disruptions. The lack of NFS v4.1 multipath and trunking can also extend NFS operations recovery.

## Submitting your Order

When you're ready to submit your order, you can place it through the [Console](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole) or the [SLCLI](/docs/infrastructure/FileStorage?topic=FileStorage-orderingSLCLI). For Provisioning File Storage with VMware, click [here](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)

## Connecting your new storage
{: #mountingstorage}

When your provisioning request is complete, authorize your hosts to access the new storage and configure your connection. Depending on your host's operating system, follow the appropriate link.
- [Accessing {{site.data.keyword.filestorage_short}} on Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [Mounting {{site.data.keyword.filestorage_short}} in CentOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [Mounting {{site.data.keyword.filestorage_short}} on CoreOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)
- [Configuring {{site.data.keyword.filestorage_short}} for backup with cPanel](/docs/infrastructure/FileStorage?topic=FileStorage-cPanelBackups)
- [Configuring {{site.data.keyword.filestorage_short}} for backup with Plesk](/docs/infrastructure/FileStorage?topic=FileStorage-PleskBackup)
- [Mounting {{site.data.keyword.filestorage_short}} Volume on ESXi hosts](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)

## Managing your new Storage

Through the portal or the SLCLI you can manage various aspects of your {{site.data.keyword.filestorage_short}} such as host authorizations and cancellations. For more information, see [Managing {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage).
