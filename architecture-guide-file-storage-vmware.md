---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-27"

---
{:pre: .pre}
{:new_window: target="_blank"}

# Provisioning {{site.data.keyword.filestorage_short}} with VMware

The following steps can help you order and configure {{site.data.keyword.filestorage_full}} in a vSphere 5.5 and vSphere 6.0 environment at {{site.data.keyword.BluSoftlayer_full}}. If you require more than eight connections to your VMware host, then choosing NFS {{site.data.keyword.filestorage_short}} is the best practice.

The {{site.data.keyword.filestorage_short}} is designed to support high I/O applications that require predictable levels of performance. The predictable performance is achieved through the allocation of protocol-level input/output operations per second (IOPS) to individual volumes.

The {{site.data.keyword.filestorage_short}} offering is accessed and mounted through an NFS connection. In a VMware deployment, a single volume can be mounted to up to 64 ESXi hosts as shared storage. You can also mount multiple volumes to create a storage cluster to use vSphere Storage Distributed Resource Scheduler (DRS).

Pricing and configuration options for Endurance and Performance {{site.data.keyword.filestorage_short}} are charged based on a combination of the reserved space and the offered IOPS.

## Ordering Considerations

When you order {{site.data.keyword.filestorage_short}}, consider the following information:

- When you decide on the size, consider the size of the workload and throughput needed. Size matters with the Endurance service, which scales performance linearly in relation to capacity (IOPS/GB). Conversely, the Performance service allows the administrator to choose capacity and performance independently. Throughput requirements matter with Performance.
  >**Note** - The throughput calculation is IOPS x 16 KB. IOPS is measured based on a 16 KB block size with a 50/50 read/write mix.<br/>Increasing block size increases the throughput but decreases IOPS. For example, doubling the block size to 32 KB blocks maintains the maximum throughput but halves the IOPS.
- NFS uses many extra file control operations such as `lookup`, `getattr`, and `readdir`. These operations in addition to read/write operations can count as IOPS and vary by operation type and NFS version.
- {{site.data.keyword.filestorage_short}} volumes are exposed to authorized devices, subnets, or IP addresses.
- To avoid storage disconnection during path failover {{site.data.keyword.IBM}} recommends installing VMware tools, which set an appropriate timeout value. There’s no need to change the value, the default setting is sufficient to ensure that your VMware host doesn't lose connectivity.
- Both NFSv3 and NFSv4.1 are supported in the {{site.data.keyword.BluSoftlayer_full}} environment. However, {{site.data.keyword.IBM}} suggests that you use NFSv3. Because NFSv4.1 is a stateful protocol (not stateless like NFSv3), protocol issues can occur during network events. NFSv4.1 must quiesce all operations and then complete lock reclamation. While these operations are taking place, disruptions can occur.

For more information, see VMware's Whitepaper on [Best Practices for running
VMware vSphere on Network Attached Storage](https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/techpaper/vmware-nfs-bestpractices-white-paper-en.pdf){:new_window}

**NFS Protocol VMware feature support matrix**
<table>
  <caption>Table 1 shows the vSphere features as they apply to the two different versions of NFS.</caption>
 <thead>
  <tr>
   <th>vSphere Features</th>
   <th>NFS version 3</th>
   <th>NFS version 4.1</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>vMotion and Storage vMotion</td>
   <td>Yes</td>
   <td>Yes</td>
  </tr>
  <tr>
   <td>High Availability (HA)</td>
   <td>Yes</td>
   <td>Yes</td>
  </tr>
  <tr>
   <td>Fault Tolerance (FT)</td>
   <td>Yes</td>
   <td>Yes</td>
  </tr>
  <tr>
   <td>Distributed Resource Scheduler (DRS)</td>
   <td>Yes</td>
   <td>Yes</td>
  </tr>
  <tr>
   <td>Host Profiles</td>
   <td>Yes</td>
   <td>Yes</td>
  </tr>
  <tr>
   <td>Storage DRS</td>
   <td>Yes</td>
   <td>No</td>
  </tr>
  <tr>
   <td>Storage I/O Control</td>
   <td>Yes</td>
   <td>No</td>
  </tr>
  <tr>
   <td>Site Recovery Manager</td>
   <td>Yes</td>
   <td>No</td>
  </tr>
  <tr>
   <td>Virtual Volumes</td>
   <td>Yes</td>
   <td>No</td>
  </tr>
 </tbody>
</table>
*Source - [VMware - NFS Protocols and ESXi](https://docs.vmware.com/en/VMware-vSphere/6.0/com.vmware.vsphere.storage.doc/GUID-8A929FE4-1207-4CC5-A086-7016D73C328F.html){:new_window}*



### Using Snapshots

{{site.data.keyword.filestorage_short}} allows administrators to set snapshot schedules that create and delete snapshot copies automatically for each storage volume. They can also create extra snapshot schedules (hourly, daily, weekly) for automatic snapshots and manually create ad hoc snapshots for business continuity and disaster recovery (BCDR) scenarios. Automatic alerts are delivered through the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} to the volume owner for the retained snapshots and space used.

Snapshot space is required to use snapshots. Space can be purchased on the initial volume order or after the initial provisioning through the **Volume Details** page by clicking **Actions** and selecting **Add Snapshot Space**.

It's important to note that VMware environments are not aware of snapshots. The Endurance {{site.data.keyword.filestorage_short}} snapshot capability must not be confused with VMware snapshots. Any recovery that uses the {{site.data.keyword.filestorage_short}} snapshot feature must be handled from the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

Restoring the {{site.data.keyword.filestorage_short}} volume requires powering off all the VMs on the {{site.data.keyword.filestorage_short}}. The volume needs to be temporarily unmounted from the ESXi hosts to avoid any data corruption during the process.

For more details about configuring Snapshots, see the [snapshots](snapshots.html) article.


### Using Replication

Replication uses one of your snapshot schedules to automatically copy snapshots to a destination volume in a remote data center. The copies can be recovered in the remote site if a catastrophic event or data corruption occurs.

With replicas, you can

- Recover from site failures and other disasters quickly by failing over to the destination volume
- Fail over to a specific point in time in the DR copy

Before you can replicate, you must create a snapshot schedule.

When you fail over, you are “flipping the switch” from your storage volume in your primary data center to the destination volume in your remote data center. For example, your primary data center is in London and your secondary data center is in Amsterdam. If a failure event occurs, you’d fail over to Amsterdam. This means connecting to the now-primary volume from a vSphere Cluster instance in Amsterdam. After your volume in London is repaired, a snapshot is taken of the Amsterdam volume. Then, you can fail back to London and the once-again primary volume from a compute instance in London.

Before the volume fails back to the primary data center, it needs to stop being used at the remote site. A snapshot of any new or changed information is taken and replicated to the primary data center before it can be mounted again on the production site ESXi hosts.

For more information about configuring replicas, see [Replication](replication.html).

>**Note** - Invalid data, whether corrupted, hacked, or infected replicate according to the snapshot schedule and snapshot retention. Using the smallest replication windows can provide for a better recovery point objective. However, it also can provide less time to react to the replication of invalid data.


## Ordering {{site.data.keyword.filestorage_short}}

You can order and configure {{site.data.keyword.filestorage_short}} for a VMware ESXi 5 environment. Use the following information along with the [Advanced Single-Site VMware Reference Architecture](https://console.bluemix.net/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){:new_window} to set up one of these storage options in your VMware environment.

{{site.data.keyword.filestorage_short}} can be ordered through the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} by accessing the {{site.data.keyword.filestorage_short}} page through **Storage** > **{{site.data.keyword.filestorage_short}}**.


### 1. Ordering

Use the following steps to order {{site.data.keyword.filestorage_short}}:
1. Click **Storage** > **{{site.data.keyword.filestorage_short}}** on the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} home page.
2. Click **Order {{site.data.keyword.filestorage_short}}** on the **{{site.data.keyword.filestorage_short}}** page.
3. Select the **Endurance**/**Performance** from the **Select Storage Type** list.
4. Select location. Data centers with improved capabilities are denoted with an asterisk. Ensure that the new Storage is added in the same location as the previously ordered ESXi host.
5. Select Billing method. Monthly and hourly billing options are available.
6. Select the amount of storage space in GBs. For TB, 1 TB equals 1,000 GB, and 12 TB equals 12,000 GB.
7. Enter the amount of IOPS in intervals of 100 or select an IOPS Tier.
8. Specify the size of Snapshot Space.
9. Click **Continue**.
10. Enter a promo code if you have one, and click **Recalculate**.
11. Review your order.
12. Check the **I have read the Master Service Agreement and agree to the terms therein** check box.
13. Click **Place Order** to submit the order, or **Cancel** to close the form without submitting an order.

Storage is provisioned in less than a minute and becomes visible on the **{{site.data.keyword.filestorage_short}}** page of the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.



### 2. Authorizing Hosts to use {{site.data.keyword.filestorage_short}}

When a volume is provisioned, the {{site.data.keyword.BluBareMetServers_full}} or {{site.data.keyword.BluVirtServers_full}} that is going to use the volume must be authorized to access the storage. Use the following steps to authorize the volume:

1. Click **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Select **Access Host** on the **Endurance** or **Performance Volume Actions** menu.
3. Click **Subnets**.
4. Choose from the list of available subnets that are assigned to the VMkernel ports on the ESXi hosts, and click **Submit**.<br/>
    >**Note** - The subnets that are displayed are subscribed subnets in the same data center as the storage volume.


After the subnets are authorized, make note of the host name of the Endurance or Performance storage server you want to use when you mount the volume. This information can be found on the {{site.data.keyword.filestorage_short}} detail page by clicking a specific volume.


##  Configuring the VMware Virtual Machine Host

### Meeting the Prerequisites

Before you begin the VMware configuration process, make sure that the following prerequisites are met:

- {{site.data.keyword.BluBareMetServers}} with VMware ESXi are provisioned with proper storage configuration and ESXi login credentials.
- {{site.data.keyword.BluSoftlayer_full}} Windows physical or {{site.data.keyword.virtualmachinesshort}} in the same data center as the {{site.data.keyword.BluBareMetServers}}. Including Public IP address of the {{site.data.keyword.BluSoftlayer_full}} Windows VM and login credentials.
- A computer with internet access, and with the web browser software and a Remote Desktop Protocol (RDP) client installed.


### 1. Configuring the VMware Host.

1. From an internet connected computer, start an RDP client and establish an RDP session to the {{site.data.keyword.BluVirtServers_full}} that is provisioned in the same data center where vSphere vCenter is installed.
2. From the {{site.data.keyword.BluVirtServers_short}}, start a web browser and connect to VMware vCenter through the vSphere Web Client.
3. From the **HOME** screen, select **Hosts and Clusters**. Expand the pane on the left and select the **VMware ESXi server** that is to be used for this deployment.
4. Make sure that the firewall port for the NFS client is open on all hosts so that you can configure the NFS client on the vSphere host. This is automatically opened in the more recent releases of vSphere. To check whether the port is open, go to the **ESXi host Manage** tab in VMware® vCenter™, select **Settings**, and then select **Security Profile**. In the **Firewall** section, click **Edit** and scroll down to **NFS Client**.
5. Make sure **Allow connection from any IP address or a list of IP addresses** is provided. <br/>
   ![Allow Connection](/images/1_4.png)
6. Configure Jumbo Frames by going to the **ESXi host Manage** tab, select **Manage** and then **Networking**.
7. Select **VMkernel adapters**, highlight the **vSwitch** and the click **Edit** (Pencil icon).
8. Select **NIC setting**, and ensure that the NIC MTU is set to 9000.
9. **Optional**. Validate the jumbo frame settings.
   - From Windows: `ping -f -l 8972 a.b.c.d`
   - From UNIX: `ping -s 8972 a.b.c.d`
     Where a.b.c.d is neighboring {{site.data.keyword.BluVirtServers_short}} interface.
     Example
     ```
     ping a.b.c.d (a.b.c.d) 8972(9000) bytes of data.
     8980 bytes from a.b.c.d: icmp_seq=1 ttl=128 time=3.36 ms
     ```

For more information about VMware and Jumbo Frames, click [here](https://kb.vmware.com/s/article/1003712){:new_window}.


### 2. Adding an uplink Adapter to a virtual switch

1. Configure a new uplink adapter by going to the **ESXi host Manage** tab, select **Manage** and then **Networking**.
2. Select the **Physical adapters** tab
3. Click **Add host networking** (Globe icon with a plus sign).
4. Select connection type as **Physical Network Adapter** and click **Next**.
5. Select the existing **vSwitch** and click **Next**.
6. Select **Unused adapters** and click **Add adapters** (Plus sign).
7. Click the other "Connected" adapter and click **OK**. <br/>
   ![Add physical adapters to switch](/images/2_3.png)
8. Click **Next** and the **Finish**.
9. Go back to the **Virtual switches** tab and select the **Edit setting** (Pencil icon) under the **Virtual Switches** heading.
10. On the left, select the vSwitch **Teaming and failover** entry.
11. Verify that the **Load balancing** option is set to **Route based on the originating virtual port** and click **OK**.


### 3. Configuring ESXi static routing (Optional)

The network configuration for this architecture guide uses a minimal number of port groups. If you have a VMkernel port group for NFS storage, extra steps must be taken. By default, ESXi uses the VMkernel port that is on the same subnet as an NFS volume to mount the NFS volume on Endurance/Performance storage. Since layer 3 routing is used to mount the NFS volume, ESXi must be forced to use the VMkernel port that was configured to mount the NFS volume. To do this, a static route must be created to Endurance/Performance storage array.

1. To configure a static route, SSH to each ESXi host that uses Performance or Endurance storage and run the following commands. Take note of the IP address that is the result of the `ping` command (first command) and use it with the `esxcli` network command.
   ```
   ping <host name of the endurance/performance array>
   ```
   {: pre}

   >**Note** - the NFS storage DNS host name is a Forwarding Zone (FZ) that is assigned multiple IP addresses. These IP addresses are static and belong to that specific DNS host name. Any of those IP addresses can be used to access a specific volume.

   ```
   esxcli network ip route ipv4 add –gateway GATEWAYIP –network <result of ping command>/32
   ```
   {: pre}

2. Static routes are not persistent across reboots on ESXi 5.0 and earlier. To ensure that any added static routes remain persistent, this command needs to be added to the `local.sh` file on each host, which is located in the `/etc/rc.local.d/` directory. Open the `local.sh` file by using the visual editor, and add the second command in Step 4.1. in front of the `exit 0` line.

>**Notes**<br/>- Make note of the IP address as it can be used for mounting the volume in the next step.<br/>This process needs to be done for each NFS volume you plan to mount to your ESXi host.<br/>For more information, see the VMware KB article, [Configuring static routes for VMkernel ports on an ESXi host](https://kb.vmware.com/s/article/2001426){:new_window}.


##  Creating and mounting {{site.data.keyword.filestorage_short}} Volume on the ESXi hosts

1. Click **Go to vCenter** icon, and then **Hosts and Clusters**.
2. On the **Related Object** tab, click **Datastores**.
3. Click the **Create a new datastore** icon.
4. On the **New Datastore** screen, select the location of the WMware datastore (your ESXi host) and click **Next**.
5. On the **Type** screen, select **NFS**, and click **next**.
6. Then, select the NFS version. Both NFSv3 and NFSv4.1 are supported, but NFSv3 is prefered. Make sure you use only one NFS version to access a given datastore. Consequences of mounting one or more hosts to the same datastore by using different versions can result in data corruption.
7. On the **Name and configuration** screen, enter the name that you want to call the WMware datastore. Additionally, enter the host name of the NFS server. Using the FQDN for the NFS server produces the best traffic distribution to the underlying server. IP address is also valid but is used less frequently and only in specific instances. Enter the folder name in the form of `/foldername`.
8. On the **Host accessibility** screen, select one or more hosts that you want to mount the NFS WMware datastore on and click **next**.
9. Review the inputs on the next screen and click **Finish**.
10. Repeat for any additional {{site.data.keyword.filestorage_short}} volumes.

>**Note** - It is {{site.data.keyword.BluSoftlayer_full}}’s recommendation that FQDN names be used to connect to the WMware datastore. Using direct IP addressing might bypass the load-balancing mechanism that is provided by using FQDN.

To use the IP address instead of the FQDN simply ping the server to obtain the IP address.
```
ping <host name of the endurance/performance array>
```
{: pre}

To obtain the IP address from an ESXi host, use the following command. The resulting 10.2.125.80 is the IP with the FQDN.
```
~ # vmkping nfsdal0902a-fz.service.softlayer.com
PING nfsdal0902a-fz.service.softlayer.com (10.2.125.80): 56 data bytes
64 bytes from 10.2.125.80: icmp_seq=0 ttl=253 time=0.187 ms
```


## Enabling ESXi Storage I/O Control (Optional)

Storage I/O Control (SIOC) is a feature available for customers who use an Enterprise Plus license. When SIOC is enabled in the environment, it changes the device queue length for the single VM. The change to the device queue length reduces the storage array queue for all VMs to an equal share. SIOC engages only if resources are constrained and the storage I/O latency is above a defined threshold.


In order for SIOC to determine when a storage device is congested or constrained, it requires a defined threshold. The congestion threshold latency is different for different storage types. The default selection is to 90% of peak throughput. The percentage of peak throughput value indicates the estimated latency threshold when the WMware datastore is using that percentage of its estimated peak throughput.


>**Note** - Incorrectly configuring SIOC for a WMware datastore or for a VMDK can significantly impact performance.


### Configuring Storage I/O Control For A WMware Datastore

Use the following steps to enable SIOC with recommended values for Endurance and Performance Storage:

1. Browse to the WMware datastore in the vSphere Web Client navigator.
2. Click the **Manage** tab.
3. Click **Settings** and click **General**.
4. Click **Edit** for **Datastore Capabilities**.
5. Select the **Enable Storage I/O Control** check box.<br/>
   ![NSF WMware Datastore](/images/3_0.png)
6. Click **OK**.

**Note**: This setting is specific to the WMware datastore and not to the host.


### Configuring Storage I/O Control For {{site.data.keyword.BluVirtServers_short}}

You can also limit individual virtual disks for individual VMs or grant them different shares with SIOC. The limiting of disks and granting different shares allows you to match and align the environment to the workload with the acquired {{site.data.keyword.filestorage_short}} volume IOPS number. The limit is set by IOPS and it is possible to set a different weight or "Shares." Virtual disks with shares set to High (2,000 shares) receive twice as much I/O as a disk set to Normal (1,000 shares) and four times as much as one set to Low (500 shares). Normal is the default value for all the VMs, so you need to adjust the values above or under Normal for the VMs that require it.

Use the following steps to change the VDisk shares and limit.

1. Choose a {{site.data.keyword.BluVirtServers_short}} in the **VMs and Templates** inventory.
2. Select the {{site.data.keyword.BluVirtServers_short}} for I/O Control.
3. Click the **Manage** tab and click **Settings**. Click **Edit**.
4. Expand the **hard disk** arrow. Select **Modify the Shares** or **Limit - IOPs** as is appropriate for your environment. Choose a virtual hard disk from the list and modify the Shares selection to choose the relative number of shares to allocate to the {{site.data.keyword.BluVirtServers_short}} (Low, Normal, or High). You can also click **Custom** and enter a user-defined share value.
5. Click the **Limit - IOPS** column and enter the maximum storage resources to allocate to the virtual machine.
6. Click **OK**


> **Note**: This process is used to set the resource consumption limits of individual vDisks in a {{site.data.keyword.BluVirtServers_short}} even when SIOC is not enabled. These settings are specific to the individual guest, and not the host, although they are used by SIOC.


## Configuring ESXi Host Side Settings

Extra settings are required for configuring ESXi 5.x hosts for NFS storage. This table shows what each setting needs to be.

|Parameter | Set to ... |
|----------|------------|
|Net.TcpipHeapSize |	32 |
|Net.TcpipHeapMax |	For vSphere 5.0/5.1 set 128 <br/> For vSphere 5.5 or higher set 512 |
|NFS.MaxVolumes |	256 |
|NFS41.MaxVolumes |	256 (vSphere 6.0 or later only) |
|NFS.HeartbeatMaxFailures |	10 |
|NFS.HeartbeatFrequency |	12 |
|NFS.HeartbeatTimeout |	5 |
|NFS.MaxQueueDepth|	64 |


### Updating advanced configuration parameters on ESXi 5.x host by using the CLI

The following examples use the CLI to set the advanced configuration parameters, and then, check them. The `esxcfg-advcfg` tool that is used in the examples can be found in the `/usr/sbin` directory on the ESXi 5.x hosts.

   - Setting the advanced configuration parameters from the ESXi 5.x CLI.

     ```
     #esxcfg-advcfg -s 32 /Net/TcpipHeapSize
     #esxcfg-advcfg -s 128 /Net/TcpipHeapMax(For vSphere 5.0/5.1)
     #esxcfg-advcfg -s 512 /Net/TcpipHeapMax(For vSphere 5.5 and above)
     #esxcfg-advcfg -s 256 /NFS/MaxVolumes
     #esxcfg-advcfg -s 256 /NFS41/MaxVolumes (ESXi 6.0 and above)
     #esxcfg-advcfg -s 10 /NFS/HeartbeatMaxFailures
     #esxcfg-advcfg -s 12 /NFS/HeartbeatFrequency
     #esxcfg-advcfg -s 5 /NFS/HeartbeatTimeout   
     #esxcfg-advcfg -s 64 /NFS/MaxQueueDepth
     #esxcfg-advcfg -s 32 /Disk/QFullSampleSize
     #esxcfg-advcfg -s 8 /Disk/QFullThreshold
     ```

  - Checking the advanced configuration parameters from the ESXi 5.x CLI.

    ```
    #esxcfg-advcfg -g /Net/TcpipHeapSize
    #esxcfg-advcfg -g /Net/TcpipHeapMax
    #esxcfg-advcfg -g /NFS/MaxVolumes
    #esxcfg-advcfg -g /NFS41/MaxVolumes (ESXi 6.0 and above)
    #esxcfg-advcfg -g /NFS/HeartbeatMaxFailures
    #esxcfg-advcfg -g /NFS/HeartbeatFrequency
    #esxcfg-advcfg -g /NFS/HeartbeatTimeout
    #esxcfg-advcfg -g /NFS/MaxQueueDepth
    #esxcfg-advcfg -g /Disk/QFullSampleSize
    #esxcfg-advcfg -g /Disk/QFullThreshold
    ```

## Enabling Jumbo Frames in {{site.data.keyword.BluSoftlayer_notm}} for Windows and Linux

A jumbo frame is an Ethernet frame with a payload greater than the standard maximum transmission unit (MTU) of 1,500 bytes. Jumbo frames are used on local area networks that support at least 1 Gbps and can be as large as 9,000 bytes.

Jumbo frames need to be configured the same on the entire network path from source device <-> switch <-> router <-> switch <-> destination device. If the entire chain isn't set the same, it defaults to the lowest setting along the chain. {{site.data.keyword.BluSoftlayer_full}} has network devices set to 9,000 currently. All customer devices need to be set to the same 9,000 value.

### Enabling Jumbo Frames in Windows

1. Open the **Network and Sharing Center**.
2. Click **Change adapter settings**.
3. Right-click the NIC for which you want to enable jumbo frames and select **Properties**.
4. Under the **Networking** tab, click **Configure** for the network adapter.
5. Select the **Advanced** tab.
6. Select **Jumbo Frame** and change the value from **disabled** to the value you want. The value, such as 9 kB or 9014 Bytes, depends on the NIC.
7. Click **OK** in all windows.

>**Note** - When you make the change, the NIC loses network connectivity for a few seconds. Restart the device to confirm that the change took effect.


### Enabling Jumbo Frames in Linux

1. Edit the network configuration file for eth0 interface.
   - CentOS/RHEL/Fedora Linux users edit `/etc/sysconfig/network-script/ifcfg-eth0`
     ```
     # vi /etc/sysconfig/network-script/ifcfg-eth0
     ```
     {: pre}

   - Debian/Ubuntu Linux users edit `/etc/network/interfaces`.

2. Append the following configuration directive, which specifies the size of the frame in bytes.
   - CentOS/RHEL/Fedora Linux
     ```
     MTU 9000
     ```
     {: pre}

   - Debian/Ubuntu Linux
     ```
     MTU=9000
     ```
     {: pre}

3. Close and save the file. Restart the Interface eth0.
   ```
   # /etc/init.d/networking restart
   ```
   {: pre}

   This action causes a brief loss of network connectivity.

Learn more about Advanced Single-Site VMware Reference Architecture [here](https://console.bluemix.net/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){:new_window}.
