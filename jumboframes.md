---

copyright:
  years: 2014, 2025
lastupdated: "2025-06-23"

keywords: File Storage for Classic, NSF, networking, jumbo frames

subcollection: FileStorage

---
{{site.data.keyword.attribute-definition-list}}

# Enabling Jumbo Frames
{: #jumboframes}

A jumbo frame is an Ethernet frame with a payload greater than the standard maximum transmission unit (MTU) of 1,500 bytes. Jumbo frames are used on local area networks that support at least 1 Gbps and can be as large as 9,000 bytes.
{: shortdesc}

Jumbo frames need to be configured the same on the entire network path: source device > switch > router > switch > destination device. If the entire chain isn't set the same, it defaults to the lowest setting along the chain. {{site.data.keyword.cloud}} has network devices set to 9,000 currently. For best performance, all customer devices need to be set to the same 9,000 value.
{: important}

## Enabling Jumbo Frames in Linux
{: #enablejumbo}

1. Edit the network configuration file for eth0 interface.
   - CentOS, RHEL, Fedora users edit `/etc/sysconfig/network-scripts/ifcfg-eth0`.
     ```sh
     # vi /etc/sysconfig/network-scripts/ifcfg-eth0
     ```
     {: pre}

   - Debian and Ubuntu users edit `/etc/network/interfaces`.

2. Append the following configuration directive, which specifies the size of the frame in bytes.
   - CentOS, RHEL, Fedora
     ```sh
     MTU 9000
     ```
     {: pre}

   - Debian and Ubuntu
     ```sh
     MTU=9000
     ```
     {: pre}

3. Close and save the file. Restart the Interface eth0.
   ```sh
   # /etc/init.d/networking restart
   ```
   {: pre}

   This action causes a brief loss of network connectivity.
   {: attention}

## Enabling Jumbo Frames in VMware vSphere
{: #enablejumbovmware}

For more information, see [VMware vSphere 8.0 - Enabling Jumbo Frames](https://techdocs.broadcom.com/us/en/vmware-cis/vsphere/vsphere/8-0/vsphere-networking-8-0/managing-network-resources/enabling-jumbo-frames.html){: external}.

## Related information
{: #related-info}

For more information about MTU settings, see the following topics.
- [Configuring virtual server settings for improved network performance](/docs/virtual-servers?topic=virtual-servers-configuring-network-performance){: external}.
- [Tuning performance in Red Hat OpenShift on IBM Cloud](/openshift?topic=openshift-kernel&interface=ui#calico-mtu){: external}.
- [Tuning performance in Kubernetes Service](/docs/containers?topic=containers-kernel#calico-mtu){: external}.
