---

copyright:
  years: 2014, 2023
lastupdated: "2023-01-18"

keywords: File Storage, file storage, NFS, provisioning, setup, configuration, mounting storage

subcollection: FileStorage

content-type: tutorial
services:
account-plan: paid
completion-time: 2h
---
{{site.data.keyword.attribute-definition-list}}
{: ui-linked}


# Getting started with {{site.data.keyword.filestorage_short}}
{: #getting-started}
{: toc-content-type="tutorial"}
{: toc-services=""}
{: toc-completion-time="2h"}

{{site.data.keyword.filestorage_full}} is persistent, fast, and flexible network-attached, NFS-based {{site.data.keyword.filestorage_short}}. In this network-attached storage (NAS) environment, you have total control over your file shares function and performance. {{site.data.keyword.filestorage_short}} shares can be connected to up to 64 authorized devices over routed TCP/IP connections for resiliency.
{: shortdesc}

For more information about using {{site.data.keyword.filestorage_short}} with the {{site.data.keyword.containerlong}}, see [Storing data on classic IBM Cloud File Storage](/docs/containers?topic=containers-file_storage).
{: tip}

## Before you begin
{: #prereqs}

{{site.data.keyword.filestorage_short}} volumes can be provisioned from 20 GB to 12 TB with two options:
- Provision **Endurance** tiers that feature pre-defined performance levels and other features like snapshots and replication.
- Build a high-powered **Performance** environment with allocated input/output operations per second (IOPS).

For more information about the {{site.data.keyword.filestorage_short}} offering, see [What is IBM Cloud File Storage?](https://www.ibm.com/cloud/file-storage){: external}.

## Provisioning considerations
{: #provconsiderations}
{: step}

### Block size
{: #blocksize}

The IOPS value for both Endurance and Performance is based on a 16-KB block size with a 50/50 read and write, 50/50 random and sequential workload. A 16-KB block is the equivalent of one write to the volume.
{: important}

The block size that is used by your application directly impacts the storage performance. If the block size that is used by your application is smaller than 16 KB, the IOPS limit is realized before the throughput limit. Conversely, if the block size that is used by your application is larger than 16 KB, the throughput limit is realized before to the IOPS limit.

| Block Size (KB) | IOPS | Throughput (MB/s) |
|-----------------|------|-------------------|
| 4 | 1,000 | 4 |
| 8 | 1,000 | 8 |
| 16 | 1,000 | 16 |
| 32 | 500 | 16 |
| 64 | 250 | 16 |
| 128 | 128 | 16 |
| 512 | 32 | 16 |
| 1024 | 16 | 16 |
{: caption="Table 1 shows examples of how block size and IOPS affect the throughput. Average IO size x IOPS = Throughput in MB/s." caption-side="top"}

### Authorized hosts
{: #numberofhosts}

Another factor to consider is the number of hosts that are using your volume. When only a single host that is accessing the volume, it can be difficult to realize the maximum IOPS available, especially at extreme IOPS counts (10,000s). If your workload requires high throughput, it would be best to configure at least a couple servers to access your volume to avoid a single-server bottleneck.

### Network connection
{: #networkconnection}

The speed of your Ethernet connection must be faster than the expected maximum throughput from your volume. Generally, don't expect to saturate your Ethernet connection beyond 70% of the available bandwidth. For example, if you have 6,000 IOPS and are using a 16-KB block size, the volume can handle approximately 94-MBps throughput. If you have a 1-Gbps Ethernet connection to your volume, it becomes a bottleneck when your servers attempt to use the maximum available throughput. It's because 70 percent of the theoretical limit of a 1-Gbps Ethernet connection (125 MB per second) would allow for 88 MB per second only.

To achieve maximum IOPS, adequate network resources need to be in place. Other considerations include private network usage outside of storage and host side and application-specific tunings, and other settings).

Storage traffic is to be isolated from other traffic types, and not be directed through firewalls and routers. Keeping the storage traffic in a dedicated VLAN also helps preventing MTU mismatch when Jumbo frames are enabled. For more information, see [Enabling Jumbo Frames](/docs/FileStorage?topic=FileStorage-jumboframes).

Storage traffic is included in the total network usage of Public Virtual Servers. For more information about the limits that might be imposed by the service, see the [Virtual Server documentation](/docs/virtual-servers?topic=virtual-servers-about-public-virtual-servers).

### NFS version
{: #whichnfs}

Both NFS v3 and NFS v4.1 are supported in the {{site.data.keyword.cloud}} environment. However, the preferred version is NFS v3 because it's more resilient when network events occur.

When {{site.data.keyword.filestorage_short}} is used in a VMware&reg; deployment, NFSv4.1 might be the better choice for your implementation. For more information about the different features of each version and what is supported by VMware&reg;, see [NFS Protocols and ESXi](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.storage.doc/GUID-8A929FE4-1207-4CC5-A086-7016D73C328F.html){: external}.

## Submitting your Order
{: #submitFileStorOrder}
{: step}

When you're ready to submit your order, you can place it through the [Console](/docs/FileStorage?topic=FileStorage-orderingFileStorage#orderingFileStorageUI), the [SLCLI](/docs/FileStorage?topic=FileStorage-orderingFileStorage#orderingthroughCLI), or the [IBMCLOUD CLI](/docs/cli?topic=cli-sl-file-storage-service#sl_file_volume_order). For more information about provisioning File Storage for VMware&reg; deployments, see the [architecture guide](/docs/FileStorage?topic=FileStorage-architectureguide).

## Connecting and configuring your new storage
{: #mountingstorage}
{: step}

When your provisioning request is complete, authorize your hosts to access the new storage and configure your connection. Depending on your host's operating system, follow the appropriate link.
- [Mounting {{site.data.keyword.filestorage_short}} on Red Hat Linux&reg;](/docs/FileStorage?topic=FileStorage-mountingLinux)
- [Mounting {{site.data.keyword.filestorage_short}} in CentOS](/docs/FileStorage?topic=FileStorage-mountingCentOS)
- [Mounting {{site.data.keyword.filestorage_short}} on Ubuntu](/docs/FileStorage?topic=FileStorage-mountingUbuntu)
- [Mounting {{site.data.keyword.filestorage_short}} as a VMware&reg; datastore on ESXi hosts](/docs/FileStorage?topic=FileStorage-architectureguide)
- [Configuring {{site.data.keyword.filestorage_short}} for backup with cPanel](/docs/FileStorage?topic=FileStorage-cPanelBackups)
- [Configuring {{site.data.keyword.filestorage_short}} for backup with Plesk](/docs/FileStorage?topic=FileStorage-PleskBackup)

## Managing your new Storage
{: #manFileStor}
{: step}

Through the console or the CLI, you can manage various aspects of your {{site.data.keyword.filestorage_short}} such as host authorizations and cancellations. For more information, see [Managing {{site.data.keyword.filestorage_short}}](/docs/FileStorage?topic=FileStorage-managingstorage).
