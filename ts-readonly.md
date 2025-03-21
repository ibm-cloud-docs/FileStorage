---

copyright:
  years: 2024, 2025
lastupdated: "2025-03-21"

keywords: file storage for classic, read-only storage volume, offline file share

subcollection: FileStorage

---

{{site.data.keyword.attribute-definition-list}}

# Troubleshooting {{site.data.keyword.filestorage_short}}
{: #classic-file-troubleshooting}

When you create or manage {{site.data.keyword.filestorage_short}}, you might encounter issues. Often, you can recover by following a few steps. Issues, symptoms, likely causes, and resolutions are described in the following sections.
{: shortdesc}

## My storage appears offline or read-only.
{: #fs_snapshot_ts_restore_fail}
{: troubleshoot}

After a brief disconnect, the File storage volume appears offline or read-only.
{: tsSymptoms}

In a few scenarios, a host (bare-metal or VM) might lose connection to the storage briefly and as a result, it considers it to be a read-only volume to avoid data corruption. Most of the time the loss of connectivity is network-related but the status of the storage remains read only from the host's perspective even when the network connection is restored. This issue can be observed with virtual drives of VMs on a network-attached VMware&reg; datastore (NFS protocol).
{: tsCauses}

To resolve, confirm that the network path between the Storage and the Host is clear, and that no maintenance or outage is in progress. Then, unmount and mount the storage volume. If the volume is still read only, restart the host.
{: tsResolve}

For mounting instructions, see the following topics.
- [Mounting {{site.data.keyword.filestorage_short}} in CentOS](/docs/FileStorage?topic=FileStorage-mountingCentOS)
- [Mounting {{site.data.keyword.filestorage_short}} on ESXi hosts](/docs/FileStorage?topic=FileStorage-architectureguide)
- [Mounting {{site.data.keyword.filestorage_short}} on Red Hat Linux&reg;](/docs/FileStorage?topic=FileStorage-mountingLinux)
- [Mounting {{site.data.keyword.filestorage_short}} on Ubuntu](/docs/FileStorage?topic=FileStorage-mountingUbuntu)

To prevent this situation from recurring, the customer might consider the following actions:
- Adding guest OS tunings. For more information, see [NetApp's recommendations for guest OS tunings for a VMware&reg; vSphere deployment](https://kb.netapp.com/data-mgmt/OTV/VSC_Kbs/What_are_the_guest_OS_tunings_needed_for_a_VMware_vSphere_deployment){: external}.
- Reconfiguring Host systems that use NFSv4.1 for NFSv3 for increased resilience during maintenance operations.
- Discontinuing session trunking on host systems that run VMware&reg; ESXi. Session trunking is not supported and is known to cause disruptions.
 
For more information, see [Why did my host lose connection to my NFS datastore](/docs/vmwaresolutions?topic=vmwaresolutions-trbl_esxi_firewall_config_nfs){: external} troubleshooting topic.
