---

copyright:
  years: 2022, 2023
lastupdated: "2023-01-18"

keywords: File Storage, use of a File Storage volume, NFS

subcollection: FileStorage

---
{{site.data.keyword.attribute-definition-list}}

# Best practices for {{site.data.keyword.filestorage_short}}
{: #best-practices-classic}

Follow our best practices to maximize the performance of your storage, and avoid application downtime.
{: shortdesc}

## Best practice 1 - Clear the path
{: #bestpractice1}

To achieve maximum IOPS, adequate network resources need to be in place. 

* **Run storage traffic on a dedicate VLAN.** Running storage traffic through software firewalls increases latency and adversely affects storage performance. It's best to run storage traffic on a VLAN, which bypasses the firewall. For more information, see [routing storage traffic to its own VLAN interface](/docs/FileStorage?topic=FileStorage-file-storage-faqs#howtoisolatedstorage).

* **Avoid routing your storage traffic to a gateway device** whenever possible. When storage traffic is routed to a gateway device, it can add latency to storage traffic, or it can cause storage traffic disruption if the firewall in the gateway device is misconfigured. The storage disruption is especially true when maintenance such as a restart is required on a single (not clustered) gateway device. If a storage traffic must be routed through a gateway device, ensure that the gateway device has an at-least 10-Gbps interface, or the gateway device might become a network bottleneck.

* **Use the subnet address** instead of individual IP addresses on firewalls for NFS traffic. NFS hostname serves 6 IP addresses, and these 6 IP addresses can change.

* **Use a faster NIC.** Some limits are set at the LUN level and a faster interface doesn't increase that limit. However, with a slower Ethernet connection, your bandwidth can be a potential hindrance to achieving best performance levels.

* **Choose a higher bandwidth.** The speed of your Ethernet connection must be faster than the expected maximum throughput from your volume. Generally, don't expect to saturate your Ethernet connection beyond 70% of the available bandwidth.

     For example, if you have 6,000 IOPS and are using a 16-KB block size, the volume can handle approximately 94-MBps throughput. However, when you have a 1-Gbps Ethernet connection to your LUN, it becomes a bottleneck when your servers attempt to use the maximum available throughput. It's because 70 percent of the theoretical limit of a 1-Gbps Ethernet connection (125 MB per second) would allow for 88 MB per second only.
     {: note}

## Best practice 2 - Optimize the host and applications
{: #bestpractice3}

* **Use the right I/O scheduler**. I/O schedulers help to optimize disk access requests. They traditionally do achieve optimization by merging I/O requests. By grouping requests at similar sections of disk, the drive doesn't need to "seek" as often, improving the overall response time for disk operations. On modern Linux implementations, several I/O scheduler options are available. Each of these options has their own unique method of scheduling disk access requests.

    - **Deadline** is the default I/O scheduler on Red Hat 7.9, and usually it does not need to be changed to a different I/O scheduler. It's latency-oriented scheduler and it works by creating a separate read queue and separate a write queue. Each I/O request has a timestamp that is associated with it to be used by the kernel for an expiration time. While this scheduler also attempts to service the queues based on the most efficient ordering possible, the expiration time acts as a "deadline" for each I/O request. When an I/O request reaches its deadline, it is pushed to the highest priority.

    - **No Operation (NOOP)** is a basic scheduler that passes down the I/O that comes to it. This scheduler places all I/O requests into a FIFO (First in, First Out) queue. It's a useful tool for checking whether complex I/O scheduling decisions of other schedulers are causing I/O performance regressions. This scheduler is recommended for setups with devices that do I/O scheduling themselves, such as intelligent storage or in multipathing environments. If you choose a more complicated scheduler on the host, the scheduler of the host and the scheduler of the storage device can compete with each other and decrease performance. The storage device can usually determine best how to schedule I/O. For more information about how to check and configure the I/O scheduler, see Red Hat's [How to use the NOOP or None IO Schedulers](https://access.redhat.com/solutions/109223){: external}.

    - **Completely fair queuing (CFQ)** uses both elevators and request merging, and it's a bit more complex than the NOOP or deadline schedulers. It's the standard scheduler for many Linux distributions. It groups simultaneous requests that are made by operations into a series of per-process pools before it allocates time slices to use the disc for every queue.

   If your work load is dominated by interactive applications, the users might complain of sluggish performance of databases with many I/O operations. In such environments, read operations happen significantly more often than write operations, and applications are more likely to be waiting to read data. You can check the default IO scheduler settings and try different schedulers to ensure optimization for your specific workload.

* **[Enable Jumbo Frames](/docs/FileStorage?topic=FileStorage-jumboframes) and configure them to be the same on the entire network path** from source device <-> switch <-> router <-> switch <-> target device. If the entire chain isn't set the same, it defaults to the lowest setting along the chain. {{site.data.keyword.cloud}} has network devices set to 9,000 currently. For best performance, all customer devices need to be set to the same 9,000 value.

   Setting MTU to 9000 on your hosts has the following benefits:
    - Data can be transmitted in fewer frames.
    - Data can be transmitted in faster because fewer packets require fewer bytes of format information that is stored in the packet header. 
    - Throughput is increased by reducing the number of CPU cycles and instructions for packet processing.
    - Jumbo frames provide less opportunity for packets to arrive out of order or to be lost, resulting in fewer retransmissions. Fewer retransmissions mean less time that is spent in TCP recovery. The result is greater throughput.

* **Use NFSv3** protocol when possible. Network File System (NFS) is a networking protocol for distributed file sharing. It allows remote hosts to mount file systems over a network and interact with those file systems as if they are mounted locally. NFSv3 supports safe asynchronous writes and is more robust at error handling than the previous NFSv2. It supports 64-bit file sizes and offsets, allowing clients to access more than 2 GB of file data. NFSv3 natively supports `no_root_squash` that allows root clients to retain root permissions on the NFS share. **NFSv4.1** is also supported. When {{site.data.keyword.filestorage_short}} is used in a VMware&reg; deployment, NFSv4.1 might be the better choice for your implementation. For more information about the different features of each version and what is supported by VMware&reg;, see [NFS Protocols and ESXi](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.storage.doc/GUID-8A929FE4-1207-4CC5-A086-7016D73C328F.html){: external}.

* **VMware&reg; specific best practice for teaming** - If you plan to use teaming to increase the availability of your network access to the storage array, you must turn off port security on the switch for the two ports on which the virtual IP address is shared. The purpose of this port security setting is to prevent spoofing of IP addresses. Thus, many network administrators enable this setting. However, if you don't change it, the port security setting prevents failover of the virtual IP from one switch port to another. Then, teaming cannot fail over from one path to another. For most LAN switches, the port security is enabled on a port level and thus can be set on or off for each port.
