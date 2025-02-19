---

copyright:
  years: 2014, 2025
lastupdated: "2025-02-18"

keywords: File Storage for Classic, NFS, provisioning, setup, configuration, mounting storage

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

## Before you begin
{: #prereqs}

{{site.data.keyword.filestorage_short}} volumes can be provisioned from 20 GB to 12 TB with two options:
- Provision **Endurance** tiers that feature pre-defined performance levels and other features like snapshots and replication.
- Build a high-powered **Performance** environment with allocated input/output operations per second (IOPS).

For more information about the {{site.data.keyword.filestorage_short}} offering, see [What is IBM Cloud File Storage](https://www.ibm.com/products/file-storage){: external}.

## Provisioning considerations
{: #provconsiderations}
{: step}

### IO size
{: #blocksize}

The IOPS value for both Endurance and Performance is based on a 16-KB block size with a 50-50 read and write, 50-50 random and sequential workload. A 16-KB block is the equivalent of one write to the volume.
{: important}

The IO size that is used by your application directly impacts the storage performance. If the IO size that is used by your application is smaller than 16 KB, the IOPS limit is realized before the throughput limit. Conversely, if the block size that is used by your application is larger than 16 KB, the throughput limit is realized before to the IOPS limit.

| IO Size (KB) | IOPS | Throughput (MB/s) |
|-----------------|------|-------------------|
| 4 | 1,000 | 4 |
| 8 | 1,000 | 8 |
| 16 | 1,000 | 16 |
| 32 | 500 | 16 |
| 64 | 250 | 16 |
| 128 | 128 | 16 |
| 512 | 32 | 16 |
| 1024 | 16 | 16 |
{: caption="The table shows examples of how IO size and IOPS affect the throughput. Average IO size x IOPS = Throughput in MB/s." caption-side="bottom"}

### Authorized hosts
{: #numberofhosts}

Another factor to consider is the number of hosts that are using your volume. When only a single host that is accessing the volume, it can be difficult to realize the maximum IOPS available, especially at extreme IOPS counts (10,000s). 

The maximum IOPS for a file storage share is 48,000 IOPS. If your workload requires such high throughput, it would be best to configure at least a couple servers to access your volume to avoid a single-server bottleneck.

You can authorize up to 64 servers to access the file share. This limit includes all subnet, host, and IP authorizations combined. For more information about increasing this limit, see the [FAQs](/docs/FileStorage?topic=FileStorage-file-storage-faqs#authlimit).

### Network connection
{: #networkconnection}

The speed of your Ethernet connection must be faster than the expected maximum throughput from your volume. Generally, don't expect to saturate your Ethernet connection beyond 70% of the available bandwidth. For example, if you have 6,000 IOPS and are using a 16-KB block size, the volume can handle approximately 94-MBps throughput. If you have a 1-Gbps Ethernet connection to your volume, it becomes a bottleneck when your servers attempt to use the maximum available throughput. It's because 70% of the theoretical limit of a 1-Gbps Ethernet connection (125 MB per second) would allow for 88 MB per second only.

To achieve maximum IOPS, adequate network resources need to be in place. Other considerations include private network usage outside of storage and host side and application-specific tunings, and other settings.

Storage traffic is to be isolated from other traffic types, and it is not to be directed through firewalls and routers. Keeping the storage traffic in a dedicated VLAN also helps prevent MTU mismatch when Jumbo frames are enabled. For more information, see [Enabling Jumbo Frames](/docs/FileStorage?topic=FileStorage-jumboframes).

Storage traffic is included in the total network usage of Public Virtual Servers. For more information about the limits that might be imposed by the service, see the [Virtual Server Documentation](/docs/virtual-servers?topic=virtual-servers-about-public-virtual-servers).

### NFS version
{: #whichnfs}

Both NFSv3 and NFSv4.1 are supported in the {{site.data.keyword.cloud}} environment. Network File System (NFS) is a networking protocol for distributed file sharing. It allows remote hosts to mount file systems over a network and interact with those file systems as if they are mounted locally.

Use the **NFSv3** protocol when possible. NFSv3 supports safe asynchronous writes and is more robust at error handling than the previous NFSv2. It supports 64-bit file sizes and offsets, allowing clients to access more than 2 GB of file data. NFSv3 natively supports `no_root_squash` that allows root clients to retain root permissions on the NFS share.

When {{site.data.keyword.filestorage_short}} is used in a VMware&reg; deployment, **NFSv4.1** might be the better choice for your implementation. For more information about the different features of each version and what is supported by VMware&reg;, see [Best Practices For Running NFS with VMware vSphere](https://www.vmware.com/docs/vmw-best-practices-running-nfs-vmware-vsphere){: external}.

## Submitting your order
{: #submitFileStorOrder}
{: step}

When you're ready to submit your order, you can place it in the [Console](/docs/FileStorage?topic=FileStorage-orderingFileStorage&interface=ui#orderingFileStorageUI), from the [CLI](/docs/FileStorage?topic=FileStorage-orderingFileStorage&interface=cli#orderingthroughCLI), with the [API](/docs/FileStorage?topic=FileStorage-orderingFileStorage&interface=api#orderingthroughAPI) or with [Terraform](/docs/FileStorage?topic=FileStorage-orderingFileStorage&interface=terraform#orderingthroughTerraform). For more information about provisioning File Storage for VMware&reg; deployments, see the [architecture guide](/docs/FileStorage?topic=FileStorage-architectureguide).

By default, you can provision a combined total of 700 Block and {{site.data.keyword.filestorage_short}} volumes. For more information, see [Managing storage limits](/docs/FileStorage?topic=FileStorage-managinglimits).
{: note}

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

Mounting {{site.data.keyword.filestorage_short}} shares on Windows OS is not supported.

## Managing your storage
{: #manFileStor}
{: step}

In the console, from the CLI, with the API or Terraform, you can manage various aspects of your {{site.data.keyword.filestorage_short}} such as host authorizations and cancellations. For more information, see [Managing {{site.data.keyword.filestorage_short}}](/docs/FileStorage?topic=FileStorage-managingstorage).

You can keep your data in sync in two different locations by using replication. Replication uses one of your snapshot schedules to automatically copy snapshots to a destination volume in a remote [data center](/docs/overview?topic=overview-locations#data-centers). The copies can be recovered in the remote site if a catastrophic event occurs or your data becomes corrupted. For more information, see [Replication and Disaster Recovery – Replicating Data](/docs/FileStorage?topic=FileStorage-replication&interface=ui).
