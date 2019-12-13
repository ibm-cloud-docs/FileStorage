---

copyright:
  years: 2014, 2019
lastupdated: "2019-12-13"

keywords: File Storage, file storage, NSF, networking, jumbo frames

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


# Enabling Jumbo Frames in {{site.data.keyword.BluSoftlayer_notm}}
{: #jumboframes}

A jumbo frame is an Ethernet frame with a payload greater than the standard maximum transmission unit (MTU) of 1,500 bytes. Jumbo frames are used on local area networks that support at least 1 Gbps and can be as large as 9,000 bytes.
{:shortdesc}

Jumbo frames need to be configured the same on the entire network path from source device <-> switch <-> router <-> switch <-> destination device. If the entire chain isn't set the same, it defaults to the lowest setting along the chain. {{site.data.keyword.cloud}} has network devices set to 9,000 currently. All customer devices need to be set to the same 9,000 value.
{:important}

## Enabling Jumbo Frames

1. Edit the network configuration file for eth0 interface.
   - CentOS, RHEL, Fedora users edit `/etc/sysconfig/network-script/ifcfg-eth0`.
     ```
     # vi /etc/sysconfig/network-script/ifcfg-eth0
     ```
     {: pre}

   - Debian and Ubuntu users edit `/etc/network/interfaces`.

2. Append the following configuration directive, which specifies the size of the frame in bytes.
   - CentOS, RHEL, Fedora
     ```
     MTU 9000
     ```
     {: pre}

   - Debian and Ubuntu
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
   {:important}
