---

copyright:
  years: 2014, 2019
lastupdated: "2019-11-22"

keywords: File Storage, provisioning File Storage for VMware, NFS, File Storage, vmware,

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:shortdesc: .shortdesc}


# Provisioning {{site.data.keyword.filestorage_short}} with VMware
{: #architectureguide}

The following steps can help you order and configure {{site.data.keyword.filestorage_full}} in a vSphere 5.5 and vSphere 6.0 environment at {{site.data.keyword.cloud}}. {{site.data.keyword.filestorage_short}} is designed to support high I/O applications that require predictable levels of performance. The predictable performance is achieved through the allocation of protocol-level input/output operations per second (IOPS) to individual volumes.
{:shortdesc}

If you require more than eight hosts to access your VMware datastore, then choosing NFS {{site.data.keyword.filestorage_short}} is the best practice.
{:tip}

The {{site.data.keyword.filestorage_short}} offering is accessed and mounted through an NFS connection. In a VMware deployment, a single volume can be mounted to up to 64 ESXi hosts as shared storage. You can also mount multiple volumes to create a storage cluster to use vSphere Storage Distributed Resource Scheduler (DRS).

Pricing and configuration options for {{site.data.keyword.filestorage_short}} are charged based on a combination of the reserved space and the offered IOPS.

## Ordering considerations

When you order {{site.data.keyword.filestorage_short}}, consider the following information:

- When you decide on the size, consider the size of the workload and throughput needed. Size matters with the Endurance service, which scales performance linearly in relation to capacity (IOPS/GB). Conversely, the Performance service allows the administrator to choose capacity and performance independently. Throughput requirements matter with Performance.

  The throughput calculation is IOPS x 16 KB. IOPS is measured based on a 16-KB block size with a 50/50 read/write mix.<br/>Increasing block size increases the throughput but decreases IOPS. For example, doubling the block size to 32-KB blocks maintains the maximum throughput but halves the IOPS.
  {:note}

- NFS uses many extra file control operations such as `lookup`, `getattr`, and `readdir`. These operations in addition to read/write operations can count as IOPS and vary by operation type and NFS version.
- {{site.data.keyword.filestorage_short}} volumes are exposed to authorized devices, subnets, or IP addresses.
- To avoid storage disconnection during path-failover {{site.data.keyword.IBM}} recommends installing VMware tools, which set an appropriate timeout value. There’s no need to change the value, the default setting is sufficient to ensure that your VMware host doesn't lose connectivity.
- Both NFSv3 and NFSv4.1 are supported in the {{site.data.keyword.cloud}} environment. However, {{site.data.keyword.IBM}} suggests that you use NFSv3. Because NFSv4.1 is a stateful protocol (not stateless like NFSv3), protocol issues can occur during network events. NFSv4.1 must quiesce all operations and then complete lock reclamation. While these operations are taking place, disruptions can occur.

For more information, see VMware's white paper on [Best Practices for running
VMware vSphere on network-attached storage](https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/techpaper/vmware-nfs-bestpractices-white-paper-en.pdf){: external}.
{:tip}

### NFS Protocol VMware feature support matrix

| vSphere Features | NFS version 3 | NFS version 4.1 |
|-----|-----|-----|
| vMotion and Storage vMotion | Yes | Yes |
| High Availability (HA) | Yes | Yes |
| Fault Tolerance (FT) | Yes | Yes |
| Distributed Resource Scheduler (DRS) | Yes< | Yes |
| Host Profiles | Yes | Yes |
| Storage DRS | Yes | No |
| Storage I/O Control | Yes | No |
| Site Recovery Manager | Yes | No |
| Virtual Volumes | Yes | No |
{: row-headers}
{: class="comparison-table"}
{: caption="Table 1 - NFS Protocol VMware feature support matrix." caption-side="top"}
{: summary="This table has row and column headers. The row headers identify the vSphere features. The column headers identify the NSF version. To see if a feature is enabled navigate to the row of the feature and look at the column that is associated with the NFS version you use."}

*Source - [VMware - NFS Protocols and ESXi](https://docs.vmware.com/en/VMware-vSphere/6.0/com.vmware.vsphere.storage.doc/GUID-8A929FE4-1207-4CC5-A086-7016D73C328F.html){: external}*.


### Using Snapshots

{{site.data.keyword.filestorage_short}} allows administrators to set snapshot schedules that create and delete snapshot copies automatically for each storage volume. They can also create extra snapshot schedules (hourly, daily, weekly) for automatic snapshots and manually create ad hoc snapshots for business continuity and disaster recovery (BCDR) scenarios. Automatic alerts are delivered through the [{{site.data.keyword.cloud}} console](https://{DomainName}/){: external}  to the volume owner for the retained snapshots and space used.

Snapshot space is required to use snapshots. Space can be purchased on the initial volume order or after the initial provisioning through the **Volume Details** page by clicking **Actions** and selecting **Add Snapshot Space**.

It's important to note that VMware environments are not aware of snapshots. The {{site.data.keyword.filestorage_short}} snapshot capability must not be confused with VMware snapshots. Any recovery that uses the {{site.data.keyword.filestorage_short}} snapshot feature must be handled from the [{{site.data.keyword.cloud}} console](https://{DomainName}/){: external}.

Restoring the {{site.data.keyword.filestorage_short}} volume requires powering off all the VMs on the {{site.data.keyword.filestorage_short}}. The volume needs to be temporarily unmounted from the ESXi hosts to avoid any data corruption during the process.

For more information, see the [snapshots](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots) article.


### Using replication

Replication uses one of your snapshot schedules to automatically copy snapshots to a destination volume in a remote data center. The copies can be recovered in the remote site if a catastrophic event or data corruption occurs.

With replicas, you can

- Recover from site failures and other disasters quickly by failing over to the destination volume,
- Fail over to a specific point in time in the DR copy.

Replication keeps your data in sync in two different locations. If you want to clone your volume and use it independently from the original volume, see [Creating a duplicate File Volume](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume).
{:tip}

Before you can replicate, you must create a snapshot schedule.

When you fail over, you are “flipping the switch” from your storage volume in your primary data center to the destination volume in your remote data center. For example, your primary data center is in London and your secondary data center is in Amsterdam. If a failure event occurs, you’d fail over to Amsterdam. Failing over means connecting to the now-primary volume from a vSphere Cluster instance in Amsterdam. After your volume in London is repaired, a snapshot is taken of the Amsterdam volume. Then, you can fail back to London and the once-again primary volume from a compute instance in London.

Before the volume fails back to the primary data center, it needs to stop being used at the remote site. A snapshot of any new or changed information is taken and replicated to the primary data center before it can be mounted again on the production site ESXi hosts.

For more information about configuring replicas, see [Replication](/docs/infrastructure/FileStorage?topic=FileStorage-replication).

Invalid data, whether corrupted, hacked, or infected replicate according to the snapshot schedule and snapshot retention. Using the smallest replication windows can provide for a better recovery point objective. However, it also can provide less time to react to the replication of invalid data.
{:note}


## Ordering {{site.data.keyword.filestorage_short}}

Use the [Advanced Single-Site VMware Reference Architecture](https://{DomainName}/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){: external} to set up {{site.data.keyword.filestorage_short}} with Endurance or Performance options in your VMware environment.

{{site.data.keyword.filestorage_short}} can be ordered through [The {{site.data.keyword.cloud}} catalog](https://{DomainName}/catalog){: external} or the CLI. For more information, see [Ordering {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole).

Storage is provisioned in less than a minute and becomes visible on the **{{site.data.keyword.filestorage_short}}** page of the [{{site.data.keyword.cloud}} console](https://{DomainName}/classic/storage/file){: external}.

When a volume is provisioned, the {{site.data.keyword.BluBareMetServers_full}} or {{site.data.keyword.BluVirtServers_full}} that are going to use the volume must be authorized to access the storage. Use the following steps to authorize the host.

1. In the console, go to **Classic Infrastructure**  > **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Scroll to the File share you want to mount, and click the ellipsis (**...**) for Actions. Then, select **Authorize Host**.
3. Click **Subnets**.
4. Choose from the list of available subnets that are assigned to the VMkernel ports on the ESXi hosts, and click **Save**.<br/>

   The subnets that are displayed are subscribed subnets in the same data center as the storage volume.
   {:note}

After the subnets are authorized, make note of the host name of the storage server. The host name can be found on the {{site.data.keyword.filestorage_short}} detail page of the volume.


##  Configuring the VMware virtual machine host

Before you begin the VMware configuration process, make sure that the following prerequisites are met:

- {{site.data.keyword.BluBareMetServers}} with VMware ESXi are provisioned with proper storage configuration and ESXi login credentials.
- {{site.data.keyword.cloud}} Windows physical or {{site.data.keyword.virtualmachinesshort}} in the same data center as the {{site.data.keyword.BluBareMetServers}}. Including Public IP address of the {{site.data.keyword.cloud}} Windows VM and login credentials.
- A computer with internet access, and with the web browser software and a Remote Desktop Protocol (RDP) client installed.


### 1. Configuring the VMware Host.

1. From an internet connected computer, start an RDP client and establish an RDP session to the {{site.data.keyword.BluVirtServers_full}} that are provisioned in the same data center where vSphere vCenter is installed.
2. From the {{site.data.keyword.BluVirtServers_short}}, start a web browser and connect to VMware vCenter through the vSphere Web Client.
3. From the **HOME** screen, select **Hosts and Clusters**. Expand the pane on the left and select the **VMware ESXi server** that is to be used for this deployment.
4. Make sure that the firewall port for the NFS client is open on all hosts so that you can configure the NFS client on the vSphere host. (The port is automatically opened in the more recent releases of vSphere.) To check whether the port is open, go to the **ESXi host Manage** tab in VMware® vCenter™, select **Settings**, and then select **Security Profile**. In the **Firewall** section, click **Edit** and scroll down to **NFS Client**.
5. Make sure **Allow connection from any IP address or a list of IP addresses** is provided. <br/>
   ![Allow Connection.](/images/1_4.png)
6. Configure Jumbo Frames by going to the **ESXi host Manage** tab, select **Manage** and then **Networking**.
7. Select **VMkernel adapters**, highlight the **vSwitch** and the click **Edit** (Pencil icon).
8. Select **NIC setting**, and ensure that the NIC MTU is set to 9000.
9. **Optional**. Validate the jumbo frame settings.
   - Windows
     ```
     ping -f -l 8972 a.b.c.d
     ```
     {:pre}

   - UNIX
     ```
     ping -s 8972 a.b.c.d
     ```
     {:pre}

     The value a.b.c.d is the neighboring {{site.data.keyword.BluVirtServers_short}} interface.

     Example
     ```
     ping a.b.c.d (a.b.c.d) 8972(9000) bytes of data.
     8980 bytes from a.b.c.d: icmp_seq=1 ttl=128 time=3.36 ms
     ```

For more information about VMware and Jumbo Frames, see [here](https://kb.vmware.com/s/article/1003712){: external}.
{:tip}


### 2. Adding an uplink adapter to a virtual switch

1. Configure a new uplink adapter by going to the **ESXi host Manage** tab, select **Manage** and then **Networking**.
2. Select the **Physical adapters** tab.
3. Click **Add host networking** (Globe icon with a plus sign).
4. Select connection type as **Physical Network Adapter** and click **Next**.
5. Select the existing **vSwitch** and click **Next**.
6. Select **Unused adapters** and click **Add adapters** (Plus sign).
7. Click the other "Connected" adapter and click **OK**. <br/>
   ![Add physical adapters to switch.](/images/2_3.png)
8. Click **Next** and the **Finish**.
9. Go back to the **Virtual switches** tab and select the **Edit setting** (Pencil icon) under the **Virtual Switches** heading.
10. On the left, select the vSwitch **Teaming and failover** entry.
11. Verify that the **Load balancing** option is set to **Route based on the originating virtual port** and click **OK**.


### 3. Configuring static routing (Optional)

The network configuration for this architecture guide uses a minimal number of port groups. If you have a VMkernel port group for NFS storage, extra steps must be taken. By default, ESXi uses the VMkernel port that is on the same subnet as an NFS volume to mount the NFS volume. Since layer 3 routing is used to mount the NFS volume, ESXi must be forced to use the VMkernel port that was configured to mount the NFS volume. To use the correct port, a static route must be created to the storage array.

1. To configure a static route, SSH to each ESXi host that uses Performance or Endurance storage and run the following commands. Take note of the IP address that is the result of the `ping` command (first command) and use it with the `esxcli` network command.
   ```
   ping <host name of the storage array>
   ```
   {: pre}

   The NFS storage DNS host name is a Forwarding Zone (FZ) that is assigned multiple IP addresses. These IP addresses are static and belong to that specific DNS host name. Any of those IP addresses can be used to access a specific volume.
   {:note}

   ```
   esxcli network ip route ipv4 add –gateway GATEWAYIP –network <result of ping command>/32
   ```
   {: pre}

2. Static routes are not persistent across restarts on ESXi 5.0 and earlier. To ensure that any added static routes remain persistent, this command needs to be added to the `local.sh` file on each host, which is located in the `/etc/rc.local.d/` directory. Open the `local.sh` file by using the visual editor, and add the second command in Step 4.1. in front of the `exit 0` line.

Make note of the IP address as it can be used for mounting the volume in the next step.<br/>This process needs to be done for each NFS volume you plan to mount to your ESXi host.<br/>For more information, see the VMware KB article, [Configuring static routes for VMkernel ports on an ESXi host](https://kb.vmware.com/s/article/2001426){: external}.
{:tip}


##  Creating and mounting {{site.data.keyword.filestorage_short}} Volume on the ESXi hosts

1. Click **Go to vCenter** icon, and then **Hosts and Clusters**.
2. On the **Related Object** tab, click **Datastores**.
3. Click the **Create a new datastore** icon.
4. On the **New Datastore** screen, select the location of the VMware datastore and click **Next**.
5. On the **Type** screen, select **NFS**, and click **next**.
6. Then, select the NFS version. Both NFSv3 and NFSv4.1 are supported, but NFSv3 is preferred.

   Make sure that you use only one NFS version to access the datastore. Consequences of mounting one or more hosts to the same datastore by using different versions can result in data corruption.
   {:important}

7. On the **Name and configuration** screen, enter the name that you want to call the VMware datastore. Additionally, enter the host name of the NFS server. Using the FQDN for the NFS server produces the best traffic distribution to the underlying server. IP address is also valid but is used less frequently and only in specific instances. Enter the folder name in the form of `/foldername`.
8. On the **Host accessibility** screen, select one or more hosts that you want to mount the NFS VMware datastore on and click **next**.
9. Review the inputs on the next screen and click **Finish**.
10. Repeat for any additional {{site.data.keyword.filestorage_short}} volumes.

It is {{site.data.keyword.cloud}}’s recommendation that FQDN names be used to connect to the VMware datastore. Using direct IP addressing might bypass the load-balancing mechanism that is provided by using FQDN.
{:important}

To use the IP address instead of the FQDN, simply ping the server to obtain the IP address.
```
ping <host name of the storage array>
```
{: pre}

To obtain the IP address from an ESXi host, use the following command. The resulting 10.2.125.80 is the IP with the FQDN.
```
~ # vmkping nfsdal0902a-fz.service.softlayer.com
PING nfsdal0902a-fz.service.softlayer.com (10.2.125.80): 56 data bytes
64 bytes from 10.2.125.80: icmp_seq=0 ttl=253 time=0.187 ms
```


## Enabling ESXi Storage I/O Control (Optional)

Storage I/O Control (SIOC) is a feature available for customers who use an Enterprise Plus license. When SIOC is enabled in the environment, it changes the device queue length for the single VM. The change to the device queue length reduces the storage array queue for all VMs to an equal share. SIOC engages only if resources are constrained and the storage I/O latency is over a defined threshold.


In order for SIOC to determine when a storage device is congested or constrained, it requires a defined threshold. The congestion threshold latency is different for different storage types. The default selection is to 90% of peak throughput. The percentage of peak throughput value indicates the estimated latency threshold when the VMware datastore is using that percentage of its estimated peak throughput.


Incorrectly configuring SIOC for a VMware datastore or for a VMDK can significantly impact performance.
{:important}


### Configuring Storage I/O Control for a VMware datastore

1. Browse to the VMware datastore in the vSphere Web Client navigator.
2. Click the **Manage** tab.
3. Click **Settings** and click **General**.
4. Click **Edit** for **Datastore Capabilities**.
5. Select the **Enable Storage I/O Control** check box.<br/>
   ![NSF VMware datastore.](/images/3_0.png)
6. Click **OK**.

This setting is specific to the VMware datastore and not to the host.
{:note}


### Configuring Storage I/O Control for {{site.data.keyword.BluVirtServers_short}}

You can also limit individual virtual disks for individual VMs or grant them different shares with SIOC. By limiting the disks and granting different shares, you can match and align the environment to the workload with the acquired {{site.data.keyword.filestorage_short}} volume IOPS number. The limit is set by IOPS, and it's possible to set a different weight or "Shares." Virtual disks with shares set to High (2,000 shares) receive twice as much I/O as a disk set to Normal (1,000 shares) and four times as much as one set to Low (500 shares). Normal is the default value for all the VMs, so you need to adjust the values to High or Low for the VMs that require it.

Use the following steps to change the VDisk shares and limit.

1. Choose a {{site.data.keyword.BluVirtServers_short}} in the **VMs and Templates** inventory.
2. Select the {{site.data.keyword.BluVirtServers_short}} for I/O Control.
3. Click the **Manage** tab and click **Settings**. Click **Edit**.
4. Expand the **hard disk** arrow. Select **Modify the Shares** or **Limit - IOPs** as is appropriate for your environment. Choose a virtual hard disk from the list and modify the Shares selection to choose the relative number of shares to allocate to the {{site.data.keyword.BluVirtServers_short}} (Low, Normal, or High). You can also click **Custom** and enter a user-defined share value.
5. Click the **Limit - IOPS** column and enter the maximum storage resources to allocate to the virtual machine.
6. Click **OK**.


This process is used to set the resource consumption limits of individual vDisks in a {{site.data.keyword.BluVirtServers_short}} even when SIOC is not enabled. These settings are specific to the individual guest, and not the host, although they are used by SIOC.
{:important}


## Configuring ESXi host side settings

Other settings are required for configuring ESXi hosts for NFS storage. This table shows what each setting needs to be.

|Parameter | Set to ... |
|----------|------------|
|Net.TcpipHeapSize | 32 for vSphere 5.0 and later versions |
|Net.TcpipHeapMax |	 1536 for vSphere 6.0 and later versions,<br/>512 for vSphere 5.5,<br/>128 for vSphere 5.0 and 5.1 |
|NFS.MaxVolumes |	256 for vSphere 5.0 and later versions |
|NFS41.MaxVolumes |	256 for vSphere 6.0 and later versions |
|NFS.HeartbeatMaxFailures |	10 for all NFS configurations |
|NFS.HeartbeatFrequency |	12 for all NFS configurations|
|NFS.HeartbeatTimeout |	5 for all NFS configurations|
|NFS.MaxQueueDepth|	64 for vSphere 5.0 and later versions |
{: caption="Table 2 - Host side settings." caption-side="top"}

### Updating advanced configuration parameters on ESXi host by using the CLI

The following examples use the CLI to set the advanced configuration parameters, and then, check them. The `esxcfg-advcfg` tool that is used in the examples can be found in the `/usr/sbin` directory on the ESXi hosts.

   - Setting the advanced configuration parameters from the ESXi CLI.

     ```
     #esxcfg-advcfg -s 32 /Net/TcpipHeapSize
     #esxcfg-advcfg -s 128 /Net/TcpipHeapMax(For vSphere 5.0/5.1)
     #esxcfg-advcfg -s 512 /Net/TcpipHeapMax(For vSphere 5.5 and higher)
     #esxcfg-advcfg -s 256 /NFS/MaxVolumes
     #esxcfg-advcfg -s 256 /NFS41/MaxVolumes (ESXi 6.0 and higher)
     #esxcfg-advcfg -s 10 /NFS/HeartbeatMaxFailures
     #esxcfg-advcfg -s 12 /NFS/HeartbeatFrequency
     #esxcfg-advcfg -s 5 /NFS/HeartbeatTimeout   
     #esxcfg-advcfg -s 64 /NFS/MaxQueueDepth
     #esxcfg-advcfg -s 32 /Disk/QFullSampleSize
     #esxcfg-advcfg -s 8 /Disk/QFullThreshold
     ```

  - Checking the advanced configuration parameters from the ESXi CLI.

    ```
    #esxcfg-advcfg -g /Net/TcpipHeapSize
    #esxcfg-advcfg -g /Net/TcpipHeapMax
    #esxcfg-advcfg -g /NFS/MaxVolumes
    #esxcfg-advcfg -g /NFS41/MaxVolumes (ESXi 6.0 and higher)
    #esxcfg-advcfg -g /NFS/HeartbeatMaxFailures
    #esxcfg-advcfg -g /NFS/HeartbeatFrequency
    #esxcfg-advcfg -g /NFS/HeartbeatTimeout
    #esxcfg-advcfg -g /NFS/MaxQueueDepth
    #esxcfg-advcfg -g /Disk/QFullSampleSize
    #esxcfg-advcfg -g /Disk/QFullThreshold
    ```
Learn more about Advanced Single-Site VMware Reference Architecture [here](/docs/infrastructure/virtualization?topic=Virtualization-advanced-single-site-vmware-reference-architecture){: external}.
{:tip}
