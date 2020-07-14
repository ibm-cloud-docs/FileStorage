---

copyright:
  years: 2014, 2019
lastupdated: "2020-07-15"

keywords: File Storage, file storage, NFS, provisioning, setup, configuration, mounting storage

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:shortdesc: .shortdesc}
{:ui-linked}


# Getting started with {{site.data.keyword.filestorage_short}}
{: #getting-started}

{{site.data.keyword.filestorage_full}} is persistent, fast, and flexible network-attached, NFS-based {{site.data.keyword.filestorage_short}}. In this network-attached storage (NAS) environment, you have total control over your file shares function and performance. {{site.data.keyword.filestorage_short}} shares can be connected to up to 64 authorized devices over routed TCP/IP connections for resiliency.
{:shortdesc}

## Before you begin
{: #prereqs}

{{site.data.keyword.filestorage_short}} volumes can be provisioned from 20 GB to 12 TB with two options: <br/>
- Provision **Endurance** tiers that feature pre-defined performance levels and other features like snapshots and replication.
- Build a high-powered **Performance** environment with allocated input/output operations per second (IOPS).

For more information about the {{site.data.keyword.filestorage_short}} offering, see [Learn about {{site.data.keyword.filestorage_short}}](/docs/FileStorage?topic=FileStorage-about).

## Provisioning considerations

### Block size

The IOPS value for both Endurance and Performance is based on a 16-KB block size with a 50/50 read/write 50/50 random/sequential workload. A 16-KB block is the equivalent of one write to the volume.
{:important}

The block size that is used by your application directly impacts the storage performance. If the block size that is used by your application is smaller than 16 KB, the IOPS limit is realized before the throughput limit. Conversely, if the block size that is used by your application is larger than 16 KB, the throughput limit is realized before to the IOPS limit.

| Block Size (KB) | IOPS | Throughput (MB/s) |
|-----|-----|-----|
| 4 | 1,000 | 16 |
| 8 | 1,000 | 16 |
| 16 | 1,000 | 16 |
| 32 | 500 | 16 |
| 64 | 250 | 16 |
| 128 | 128 | 16 |
| 512 | 32 | 16 |
| 1024 | 16 | 16 |
{: caption="Table 1 shows examples of how block size and IOPS affect the throughput.<br/>Average IO size x IOPS = Throughput in MB/s." caption-side="top"}

### Authorized hosts

Another factor to consider is the number of hosts that are using your volume. If there's a single host that is accessing the volume, it can be difficult to realize the maximum IOPS available, especially at extreme IOPS counts (10,000s). If your workload requires high throughput, it would be best to configure at least a couple servers to access your volume to avoid a single-server bottleneck.

### Network connection

The speed of your Ethernet connection must be faster than the expected maximum throughput from your volume. Generally, don't expect to saturate your Ethernet connection beyond 70% of the available bandwidth. For example, if you have 6,000 IOPS and are using a 16-KB block size, the volume can handle approximately 94-MBps throughput. If you have a 1-Gbps Ethernet connection to your volume, it becomes a bottleneck when your servers attempt to use the maximum available throughput. It's because 70 percent of the theoretical limit of a 1-Gbps Ethernet connection (125 MB per second) would allow for 88 MB per second only.

To achieve maximum IOPS, adequate network resources need to be in place. Other considerations include private network usage outside of storage and host side and application-specific tunings (IP stack or [queue depths](/docs/FileStorage?topic=FileStorage-hostqueuesettings), and other settings).

Storage traffic should be isolated from other traffic types, and not be directed through firewalls and routers. Keeping the storage traffic in a dedicated VLAN also helps preventing MTU mismatch when Jumbo frames are enabled. For more information, see [Jumbo Frames in IBM Cloud](/docs/FileStorage?topic=FileStorage-jumboframes).

Storage traffic is included in the total network usage of Public Virtual Servers. For more information about the limits that might be imposed by the service, see the [Virtual Server documentation](/docs/virtual-servers?topic=virtual-servers-about-public-virtual-servers).

### NFS version

Both NFS v3 and NFS v4.1 are supported in the {{site.data.keyword.cloud}} environment. However, NFS v3 is preferred because NFS v4.1 is a stateful protocol (not stateless like NFSv3) and protocol issues can occur during network events. NFS v4.1 must quiesce all operations and then complete lock reclamation. On a relatively busy NFS file server, the increased latency can cause disruptions.

## Submitting your Order

When you're ready to submit your order, you can place it through the [Console](/docs/FileStorage?topic=FileStorage-orderingConsole), the [SLCLI](/docs/FileStorage?topic=FileStorage-orderingSLCLI), or the IBMCLOUD CLI. For more information about provisioning File Storage for VMware deployments, see the [architecture guide](/docs/FileStorage?topic=FileStorage-architectureguide).

## Connecting and configuring your new storage
{: #mountingstorage}

When your provisioning request is complete, authorize your hosts to access the new storage and configure your connection. Depending on your host's operating system, follow the appropriate link.
- [Mounting {{site.data.keyword.filestorage_short}} in CentOS](/docs/FileStorage?topic=FileStorage-mountingCentOS)
- [Mounting {{site.data.keyword.filestorage_short}} on Container Linux](/docs/FileStorage?topic=FileStorage-mountingCoreOS)
- [Mounting {{site.data.keyword.filestorage_short}} on Red Hat Linux](/docs/FileStorage?topic=FileStorage-mountingLinux)
- [Mounting {{site.data.keyword.filestorage_short}} on Ubuntu](/docs/FileStorage?topic=FileStorage-mountingUbuntu)
- [Configuring {{site.data.keyword.filestorage_short}} for backup with cPanel](/docs/FileStorage?topic=FileStorage-cPanelBackups)
- [Configuring {{site.data.keyword.filestorage_short}} for backup with Plesk](/docs/FileStorage?topic=FileStorage-PleskBackup)
- [Mounting {{site.data.keyword.filestorage_short}} Volume on ESXi hosts](/docs/FileStorage?topic=FileStorage-architectureguide)

## Managing your new Storage

Through the portal or the SLCLI, you can manage various aspects of your {{site.data.keyword.filestorage_short}} such as host authorizations and cancellations. For more information, see [Managing {{site.data.keyword.filestorage_short}}](/docs/FileStorage?topic=FileStorage-managingstorage).
