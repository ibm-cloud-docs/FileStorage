---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Enabling Jumbo Frames in {{site.data.keyword.BluSoftlayer_notm}} for Windows and Linux
{: #jumboframes}

A jumbo frame is an Ethernet frame with a payload greater than the standard maximum transmission unit (MTU) of 1,500 bytes. Jumbo frames are used on local area networks that support at least 1 Gbps and can be as large as 9,000 bytes.

Jumbo frames need to be configured the same on the entire network path from source device <-> switch <-> router <-> switch <-> destination device. If the entire chain isn't set the same, it defaults to the lowest setting along the chain. {{site.data.keyword.BluSoftlayer_full}} has network devices set to 9,000 currently. All customer devices need to be set to the same 9,000 value.
{:important}

## Enabling Jumbo Frames in Windows

1. Open the **Network and Sharing Center**.
2. Click **Change adapter settings**.
3. Right-click the NIC for which you want to enable jumbo frames and select **Properties**.
4. Under the **Networking** tab, click **Configure** for the network adapter.
5. Select the **Advanced** tab.
6. Select **Jumbo Frame** and change the value from **disabled** to the value you want. The value, such as 9 kB or 9014 Bytes, depends on the NIC.
7. Click **OK** in all windows.

When you make the change, the NIC loses network connectivity for a few seconds. Restart the device to confirm that the change took effect.
{:tip}


## Enabling Jumbo Frames in Linux

1. Edit the network configuration file for eth0 interface.
   - CentOS, RHEL, Fedora Linux users edit `/etc/sysconfig/network-script/ifcfg-eth0`
     ```
     # vi /etc/sysconfig/network-script/ifcfg-eth0
     ```
     {: pre}

   - Debian and Ubuntu Linux users edit `/etc/network/interfaces`.

2. Append the following configuration directive, which specifies the size of the frame in bytes.
   - CentOS, RHEL, Fedora Linux
     ```
     MTU 9000
     ```
     {: pre}

   - Debian and Ubuntu Linux
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
