---

copyright:
  years: 2014, 2023
lastupdated: "2023-12-18"

keywords: Classic File Storage, provisioning File Storage for VMware, NFS, File Storage, vmware,

subcollection: FileStorage

content-type: tutorial
services:
account-plan: paid
completion-time: 1h

---
{{site.data.keyword.attribute-definition-list}}

# Provisioning {{site.data.keyword.filestorage_short}} for use as a VMware datastore
{: #architectureguide}
{: toc-content-type="tutorial"}
{: toc-services=""}
{: toc-completion-time="1h"}

This tutorial guides you through how to order and configure {{site.data.keyword.filestorage_full}} in a **vSphere 5.5** or **vSphere 6.0** environment at {{site.data.keyword.cloud}}. {{site.data.keyword.filestorage_short}} is designed to support high I/O applications that require predictable levels of performance. The predictable performance is achieved through the allocation of protocol-level input/output operations per second (IOPS) to individual volumes.
{: shortdesc}

If you require more than eight hosts to access your VMware&reg; datastore, then choosing NFS {{site.data.keyword.filestorage_short}} is the best practice.
{: tip}

The {{site.data.keyword.filestorage_short}} offering is accessed and mounted through an NFS connection. In a VMware&reg; deployment, a single volume can be mounted to up to 64 ESXi hosts as shared storage. You can also mount multiple volumes to create a storage cluster to use vSphere Storage Distributed Resource Scheduler (DRS).

## Ordering considerations
{: #preorderconsiderationsvmware}

Pricing and configuration options for {{site.data.keyword.filestorage_short}} are charged based on a combination of the reserved space and the offered IOPS.
When you order {{site.data.keyword.filestorage_short}}, consider the following information:

- When you decide on the size, consider the size of the workload and the throughput needed. Size matters with the Endurance service, which scales performance linearly in relation to capacity (IOPS/GB). Conversely, the Performance service allows the administrator to choose capacity and performance independently. Throughput requirements matter with Performance.

   The throughput calculation is IOPS x 16 KB. IOPS is measured based on a 16-KB block size with a 50/50 read/write mix. Increasing block size increases the throughput but decreases IOPS. For example, doubling the block size to 32-KB blocks maintains the maximum throughput but halves the IOPS.
   {: note}

- NFS uses many extra file control operations such as `lookup`, `getattr`, and `readdir`. These operations in addition to read/write operations can count as IOPS and vary by operation type and NFS version.
- {{site.data.keyword.filestorage_short}} volumes are accessible only to authorized devices, subnets, or IP addresses.
- To avoid storage disconnection during path-failover {{site.data.keyword.IBM}} recommends installing VMware&reg; tools, which set an appropriate timeout value. Don't change the value because the default setting is sufficient to ensure that your VMware&reg; host doesn't lose connectivity.
- Both NFSv3 and NFSv4.1 are supported in the {{site.data.keyword.cloud}} environment. However, the use NFSv3 is preferred. Because NFSv4.1 is a stateful protocol (not stateless like NFSv3), protocol issues can occur during network events. NFSv4.1 must quiesce all operations and then complete lock reclamation. While these operations are taking place, disruptions can occur.

For more information, see the VMware&reg; white paper on [Best practices for running
VMware&reg; vSphere on network-attached storage](https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/techpaper/vmware-nfs-bestpractices-white-paper-en.pdf){: external}.
{: tip}

### NFS Protocol VMware feature support matrix
{: #NFSfeaturesVMwaresupportmatrix}

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
{: caption="Table 1 - NFS Protocol VMware&reg; feature support matrix." caption-side="top"}
{: summary="This table has row and column headers. The row headers identify the vSphere features. The column headers identify the NSF version. To see whether a feature is enabled navigate to the row of the feature, and look at the column that is associated with the NFS version that you use."}

*Source - [VMware&reg; - NFS Protocols and ESXi](https://docs.vmware.com/en/VMware-vSphere/6.0/vsphere-60-guide-archive.zip){: external}*.


### Using Snapshots
{: #vmwaresnapshot}

{{site.data.keyword.filestorage_short}} allows administrators to set snapshot schedules that create and delete snapshot copies automatically for each storage volume. They can also create extra snapshot schedules (hourly, daily, weekly) for automatic snapshots and manually create ad hoc snapshots for business continuity and disaster recovery (BCDR) scenarios. Automatic alerts are delivered through the [{{site.data.keyword.cloud}} console to the volume owner for the retained snapshots and space used.

Snapshot space is required to use snapshots. Space can be purchased on the initial volume order or after the initial provisioning through the **Volume Details** page by clicking **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions") and selecting **Add Snapshot Space**.

It's important to note that VMware&reg; environments are not aware of snapshots. The {{site.data.keyword.filestorage_short}} snapshot capability must not be confused with VMware&reg; snapshots. Any recovery that uses the {{site.data.keyword.filestorage_short}} snapshot feature must be handled from the [{{site.data.keyword.cloud}} console](/login){: external}.

Restoring the {{site.data.keyword.filestorage_short}} volume requires powering off all the VMs on the {{site.data.keyword.filestorage_short}}. The volume needs to be temporarily unmounted from the ESXi host to avoid any data corruption during the process.

For more information, see the [snapshots](/docs/FileStorage?topic=FileStorage-snapshots) article.


### Using replication
{: #vmwarereplication}

Replication uses one of your snapshot schedules to automatically copy snapshots to a destination volume in a remote data center. The copies can be recovered in the remote site if a catastrophic event or data corruption occurs.

With replicas, you can

- Recover from site failures and other disasters quickly by failing over to the destination volume.
- Fail over to a specific point in time with the DR copy.

Replication keeps your data in sync in two different locations. If you want to clone your volume and use it independently from the original volume, see [Creating a duplicate File Volume](/docs/FileStorage?topic=FileStorage-duplicatevolume).
{: tip}

Before you can replicate, you must create a snapshot schedule.

When you fail over, you are “flipping the switch” from your storage volume in your primary data center to the destination volume in your remote data center. For example, your primary data center is in London and your secondary data center is in Amsterdam. If a failure event occurs, you’d fail over to Amsterdam. Failing over means connecting to the now-primary volume from a vSphere Cluster instance in Amsterdam. After your volume in London is repaired, a snapshot is taken of the Amsterdam volume. Then, you can fail back to London and connect to the primary volume from a Compute instance in London.

Before the volume fails back to the primary data center, it needs to stop being used at the remote site. A snapshot of any new or changed information is taken and replicated to the primary data center before it can be mounted again on the production site ESXi hosts.

For more information about configuring replicas, see [Replication](/docs/FileStorage?topic=FileStorage-replication).

Invalid data, whether corrupted, hacked, or infected replicate according to the snapshot schedule and snapshot retention. Using the smallest replication windows can provide for a better recovery point objective. However, it can also provide less time to react to the replication of invalid data.
{: note}

## Ordering {{site.data.keyword.filestorage_short}}
{: #orderauthvmware}

Follow the instructions in the [Advanced Single-Site VMware&reg; Reference Architecture](/docs/virtualization?topic=virtualization-advanced-single-site-vmware-reference-architecture){: external} to configure your VMware environment.

{{site.data.keyword.filestorage_short}} can be ordered through [The {{site.data.keyword.cloud}} catalog](/catalog){: external}, from the [CLI](/docs/cli?topic=cli-sl-file-storage-service#sl_file_volume_order), with the API or Terraform. For more information, see [Ordering {{site.data.keyword.filestorage_short}}](/docs/FileStorage?topic=FileStorage-orderingFileStorage).

## Authorizing hosts 
{: #hostauthvmware}

You can create the authorization in the [UI](/docs/FileStorage?topic=FileStorage-managingstorage&interface=ui#authhostUI), from the [CLI](/docs/FileStorage?topic=FileStorage-managingstorage&interface=cli#authhostCLI), with the API, or with [Terraform](/docs/FileStorage?topic=FileStorage-managingstorage&interface=terraform#authhostterraform).

##  Configuring the VMware virtual machine host
{: #configurevmwarehost}

Before you begin the configuration process, make sure that the following prerequisites are met:

- {{site.data.keyword.BluBareMetServers}} with VMware&reg; ESXi are provisioned with proper storage configuration and ESXi login credentials.
- {{site.data.keyword.cloud}} Windows physical or {{site.data.keyword.virtualmachinesshort}} in the same data center as the {{site.data.keyword.BluBareMetServers}}. Including Public IP address of the {{site.data.keyword.cloud}} Windows VM and login credentials.
- A computer with internet access, and with the web browser software and a Remote Desktop Protocol (RDP) client installed.

### 1. Configuring the VMware Host.
{: #configurevmwarehost1}

1. From an internet-connected computer, start an RDP client and establish an RDP session to the {{site.data.keyword.BluVirtServers_full}} that is provisioned in the same data center where vSphere vCenter is installed.
2. From the {{site.data.keyword.BluVirtServers_short}}, start a web browser and connect to VMware&reg; vCenter through the vSphere Web Client.
3. From the **HOME** screen, select **Hosts and Clusters**. Expand the side pane and select the **VMware&reg; ESXi server** that is to be used for this deployment.
4. Make sure that the firewall port for the NFS client is open on all hosts so that you can configure the NFS client on the vSphere host. (The port is automatically opened in the more recent releases of vSphere.) To check whether the port is open, go to the **ESXi host Manage** tab in VMware® vCenter™, select **Settings**, and then select **Security Profile**. In the **Firewall** section, click **Edit** and scroll down to **NFS Client**.
5. Make sure **Allow connection from any IP address or a list of IP addresses** is provided.
    ![Allow Connection.](/images/1_4.svg){: caption="Allow Connections." caption-side="bottom"}
6. Configure Jumbo Frames by going to the **ESXi host Manage** tab, select **Manage** and then **Networking**.
7. Select the **VMkernel adapters**, highlight the **vSwitch** and the click **Edit** (Pencil icon).
8. Select the **NIC setting**, and ensure that the NIC MTU is set to 9000.
9. **Optional**. Validate the jumbo frame settings.
   - Windows
     ```shell
     ping -f -l 8972 a.b.c.d
     ```
     {: pre}

   - UNIX
     ```zsh
     ping -s 8972 a.b.c.d
     ```
     {: pre}

     The value a.b.c.d is the neighboring {{site.data.keyword.BluVirtServers_short}} interface.

     Example
     ```text
     ping a.b.c.d (a.b.c.d) 8972(9000) bytes of data.
     8980 bytes from a.b.c.d: icmp_seq=1 ttl=128 time=3.36 ms
     ```

For more information about VMware&reg; and Jumbo Frames, see [here](https://kb.vmware.com/s/article/1003712){: external}.
{: tip}


### 2. Adding an uplink adapter to a virtual switch
{: #configurevmwarehost2}

1. Configure a new uplink adapter by going to the **ESXi host Manage** tab, select **Manage** and then **Networking**.
2. Select the **Physical adapters** tab.
3. Click **Add host networking** (Globe icon with a plus sign).
4. Select the connection type as **Physical Network Adapter** and click **Next**.
5. Select the existing **vSwitch** and click **Next**.
6. Select **Unused adapters** and click **Add adapters** (Plus sign).
7. Click the other "Connected" adapter and click **OK**.
    ![Add physical adapters to the switch.](/images/2_3.svg){: caption="Add the physical adapters to the switch." caption-side="bottom"}
8. Click **Next** and the **Finish**.
9. Go back to the **Virtual switches** tab and select the **Edit setting** (Pencil icon) under the **Virtual Switches** heading.
10. Then, select the vSwitch **Teaming and failover** entry.
11. Verify that the **Load-balancing** option is set to **Route based on the originating virtual port** and click **OK**.


### 3. Configuring static routing (Optional)
{: #configurevmwarehost3}

The network configuration for this architecture guide uses a minimal number of port groups. If you have a VMkernel port group for NFS storage, extra steps must be taken. By default, ESXi uses the VMkernel port that is on the same subnet as an NFS volume to mount the NFS volume. Since layer 3 routing is used to mount the NFS volume, ESXi must be forced to use the VMkernel port that was configured to mount the NFS volume. To use the correct port, a static route must be created to the storage array.

1. To configure a static route, SSH to each ESXi host that uses Performance or Endurance storage and run the following commands. Take note of the IP address that is the result of the `ping` command (first command) and use it with the `esxcli` network command.
   ```zsh
   ping <hostname of the storage array>
   ```
   {: pre}

   The NFS storage DNS hostname is a Forwarding Zone (FZ) that has multiple IP addresses assigned to it. These IP addresses are static and belong to that specific DNS hostname. Any of those IP addresses can be used to access a specific volume.
   {: note}

   ```zsh
   esxcli network ip route ipv4 add –gateway GATEWAYIP –network <result of ping command>/32
   ```
   {: pre}

2. Static routes are not persistent across restarts on ESXi 5.0 and earlier. To ensure that any added static routes remain persistent, this command needs to be added to the `local.sh` file on each host, which is located in the `/etc/rc.local.d/` directory. Open the `local.sh` file by using the visual editor, and add the second command in Step 4.1. in front of the `exit 0` line.

Make note of the IP address as it can be used for mounting the volume in the next step. This process needs to be done for each NFS volume you plan to mount to your ESXi host. For more information, see the VMware&reg; KB article, [Configuring static routes for VMkernel ports on an ESXi host](https://kb.vmware.com/s/article/2001426){: external}.
{: tip}

##  Creating the datastore
{: #mountNFSonESXI}

1. Click the **Go to vCenter** icon, and then **Hosts and Clusters**.
2. On the **Related Object** tab, click **Datastores**.
3. Click the **Create a new datastore** icon.
4. On the **New Datastore** screen, select the location of the VMware&reg; datastore and click **Next**.
5. On the **Type** screen, select **NFS**, and click **next**.
6. Then, select the NFS version. Both NFSv3 and NFSv4.1 are supported, but NFSv3 is preferred.

   Make sure that you use only one NFS version to access the datastore. Consequences of mounting one or more hosts to the same datastore by using different versions can result in data corruption.
   {: important}

7. On the **Name and configuration** screen, enter the name that you want to call the VMware datastore. Additionally, enter the hostname of the NFS server. Using the FQDN for the NFS server produces the best traffic distribution to the underlying server. IP address is also valid but is used less frequently and only in specific instances. Enter the folder name in the form of `/foldername`.
8. On the **Host accessibility** screen, select one or more hosts that you want to mount the NFS VMware&reg; datastore on and click **next**.
9. Review the inputs on the next screen and click **Finish**.
10. Repeat for any additional {{site.data.keyword.filestorage_short}} volumes.

{{site.data.keyword.cloud_notm}} recommends that FQDN names be used to connect to the VMware&reg; datastore. Using direct IP addressing might bypass the load-balancing mechanism that is provided by using FQDN.
{: important}

To use the IP address instead of the FQDN, ping the server to obtain the IP address.
```zsh
ping <hostname of the storage array>
```
{: pre}

To obtain the IP address from an ESXi host, use the following command. The resulting 10.2.125.80 is the IP with the FQDN.
```text
~ # vmkping nfsdal0902a-fz.service.softlayer.com
PING nfsdal0902a-fz.service.softlayer.com (10.2.125.80): 56 data bytes
64 bytes from 10.2.125.80: icmp_seq=0 ttl=253 time=0.187 ms
```

## Enabling ESXi Storage I/O Control (Optional)
{: #enableSIOC}

Storage I/O Control (SIOC) is a feature available for customers who use an Enterprise Plus license. When SIOC is enabled in the environment, it changes the device queue length for the single VM. The change to the device queue length reduces the storage array queue for all VMs to an equal share. SIOC engages only if resources are constrained and the storage I/O latency is over a defined threshold.

In order for SIOC to determine when a storage device is congested or constrained, it requires a defined threshold. The congestion threshold latency is different for different storage types. The default selection is to 90% of peak throughput. The percentage of peak throughput value indicates the estimated latency threshold when the VMware&reg; datastore is using that percentage of its estimated peak throughput.

Incorrectly configuring SIOC for a VMware&reg; datastore or for a VMDK can significantly impact performance.
{: important}

### Configuring Storage I/O Control for a VMware datastore
{: #configureSIOCStorage}

1. Browse to the VMware&reg; datastore in the vSphere Web Client navigator.
2. Click the **Manage** tab.
3. Click **Settings** and click **General**.
4. Click **Edit** for **Datastore Capabilities**.
5. Select the **Enable Storage I/O Control** checkbox.
    ![NSF VMware&reg; datastore.](/images/3_0.svg){: caption="Select Enable Storage I/O Control." caption-side="bottom"}
6. Click **OK**.

This setting is specific to the VMware&reg; datastore and not to the host.
{: note}

### Configuring Storage I/O Control for {{site.data.keyword.BluVirtServers_short}}
{: #configureSIOCStoragehost}

You can also limit individual virtual disks for individual VMs or grant them different shares with SIOC. By limiting the disks and granting different shares, you can match and align the environment to the workload with the acquired {{site.data.keyword.filestorage_short}} volume IOPS number. The limit is set by IOPS, and it's possible to set a different weight or "Shares." Virtual disks with shares set to High (2,000 shares) receive twice as much I/O as a disk set to Normal (1,000 shares). They receive four times as much IO as one set to Low (500 shares). Normal is the default value for all the VMs, so you need to adjust the values to High or Low for the VMs that require it.

Use the following steps to change the VDisk shares and limit.

1. Choose a {{site.data.keyword.BluVirtServers_short}} in the **VMs and Templates** inventory.
2. Select the {{site.data.keyword.BluVirtServers_short}} for I/O Control.
3. Click the **Manage** tab and click **Settings**. Click **Edit**.
4. Expand the **hard disk** arrow. Select **Modify the Shares** or **Limit - IOPS** as is appropriate for your environment. Choose a virtual hard disk from the list and modify the Shares selection to choose the relative number of shares to allocate to the {{site.data.keyword.BluVirtServers_short}} (Low, Normal, or High). You can also click **Custom** and enter a user-defined share value.
5. Click the **Limit - IOPS** column and enter the maximum storage resources to allocate to the virtual machine.
6. Click **OK**.

This process is used to set the resource consumption limits of individual vDisks in a {{site.data.keyword.BluVirtServers_short}} even when SIOC is not enabled. These settings are specific to the individual guest, and not the host, although they are used by SIOC.
{: important}

## Configuring ESXi host-side settings
{: #configureESXihost}

Other settings are required for configuring ESXi hosts for NFS storage. This table shows what each setting needs to be.

|Parameter | Set to ... |
|----------|------------|
|Net.TcpipHeapSize | 32 for vSphere 5.0 and later versions |
|Net.TcpipHeapMax |	- 1536 for vSphere 6.0 and later versions, \n -  512 for vSphere 5.5, \n - 128 for vSphere 5.0 and 5.1 |
|NFS.MaxVolumes |	256 for vSphere 5.0 and later versions |
|NFS41.MaxVolumes |	256 for vSphere 6.0 and later versions |
|NFS.HeartbeatMaxFailures |	10 for all NFS configurations |
|NFS.HeartbeatFrequency |	12 for all NFS configurations|
|NFS.HeartbeatTimeout |	5 for all NFS configurations|
|NFS.MaxQueueDepth|	64 for vSphere 5.0 and later versions |
{: caption="Table 2 - Host side settings." caption-side="top"}

### Updating advanced configuration parameters
{: #updateconfigparam}

The following examples use the ESXi CLI to set the advanced configuration parameters, and check them. The `esxcfg-advcfg` tool that is used in the examples can be found in the `/usr/sbin` directory on the ESXi hosts.

- Setting the advanced configuration parameters from the ESXi CLI.

 ```text
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

 ```text
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

For more information, see [Advanced Single-Site VMware&reg; Reference Architecture](/docs/virtualization?topic=virtualization-advanced-single-site-vmware-reference-architecture).
{: tip}
