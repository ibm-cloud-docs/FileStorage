---

copyright:
  years: 2014, 2019
lastupdated: "2020-05-19"

keywords: File Storage, file storage, NFS,

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:shortdesc: .shortdesc}
{:support: data-reuse='support'}
{:help: data-hd-content-type='help'}

# Managing storage limits
{: #managinglimits}
{: help}
{: support}

By default, you can provision a combined total of 250 {{site.data.keyword.blockstorageshort}} and {{site.data.keyword.filestorage_short}} volumes globally.By following this process you can increase the number of volumes you can provision.

For more information about increasing your storage volume capacity beyond 12 TB, see [Expanding Block Storage Capacity](/docs/FileStorage?topic=FileStorage-expandingcapacity#increasecapacity).

## Confirming your current limit and provisioning count.
{: #confirmfilelimits}

If you're unsure how many volumes you have, you can confirm the numbers by using multiple methods.

### SLCLI

You can list the number of your volumes by using the [volume-limits](https://softlayer-python.readthedocs.io/en/latest/cli/file/#file-volume-limits){: external} command in `slcli` (version 5.8.5 or higher).
```
# slcli file volume-limits
```

Example output:
```
[{'datacenterName': 'global', 'maximumAvailableCount': 250, 'provisioned Count':117}]
:............:.......................:..................:
: Datacenter : maximumAvailableCount : ProvisionedCount :
:............:.......................:..................:
:   global   :           250         :         117      :
:............:.......................:..................:
```

### IBM Cloud CLI

The volume-limits command is also available in the `sl` plug-in for IBM Cloud CLI (v1.0 or higher).

```
# ibmcloud sl file volume-limits
Datacenter   MaximumAvailableCount   ProvisionedCount
global       300                     99
```

### REST API call

To directly get this information from the API, use the following method: [`SoftLayer_Network_Storage/getVolumeCountLimits`](https://sldn.softlayer.com/reference/services/SoftLayer_Network_Storage/getVolumeCountLimits/){: external}.

```
curl -u $SL_USER:$SL_APIKEY 'https://api.softlayer.com/rest/v3.1/SoftLayer_Network_Storage/getVolumeCountLimits.json'
[{"datacenterName":"global","maximumAvailableCount":300,"provisionedCount":99}]
```

The API call shows the combined number of {{site.data.keyword.blockstorageshort}} and {{site.data.keyword.filestorage_short}}.
{:tip}

## Requesting limit increase
{: #increasefilelimits}

You can request a limit increase by submitting a support case in the [portal](https://cloud.ibm.com/unifiedsupport/cases/add){: external}. When the request is approved, you get a volume limit that is set for a specific data center.
{:shortdesc}

To request a limit increase, open a support case and direct it to your sales representative.

In the ticket, provide the following information:

- **Ticket Subject**: Request to Increase Data Center Volume Count Storage Limit

- **What is the use case for the additional volumes request?** <br />
*For example, your answer might be something similar to a new VMware datastore, a new development and testing (dev/test) environment, an SQL database, or logging.*

- **How many extra Block volumes are needed by type, size, IOPS, and location?** <br />
*For example, your answer might be something similar to "25x Endurance 2 TB @ 4 IOPS in DAL09" or "25x Performance 4 TB @ 2 IOPS in WDC04".*

- **How many extra File volumes are needed by type, size, IOPS, and location?** <br />
*For example, your answer might be something similar to "25x Performance 20 GB @ 10 IOPS in DAL09" or "50x Endurance 2 TB @ 0.25 IOPS in SJC03".*

- **Provide an estimate of when you expect or plan to provision all of the requested volume increase.** <br />
 *For example, your answer might be something similar to "90 days".*

- **Provide a 90-day forecast of expected average capacity usage of these volumes.** <br />
*For example, your answer might be something similar to "expect 25 percent to be used in 30 days, 50 percent to be used in 60 days and 75 percent to be used in 90 days".*

Respond to all questions and statements in your request. They are required for processing and approval.
{:important}

You're going to be notified of the update to your limits through the ticket process.
