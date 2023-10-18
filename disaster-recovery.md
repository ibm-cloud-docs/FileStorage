---

copyright:
  years: 2018, 2023
lastupdated: "2023-10-18"

keywords: File Storage, NFS, disaster recovery, duplicate volume, replica volume, failover, failback,

subcollection: FileStorage

---
{{site.data.keyword.attribute-definition-list}}

# Disaster Recovery - Failover from an inaccessible primary volume
{: #dr-inaccessible}

If a catastrophic failure or disaster causes an outage on the primary site, customers can perform the following actions to quickly access their data on the secondary site. When the primary volume is inaccessible, you can force a failover to the remote replica. Before you start the failover, make sure that all host authorization is in place.
{: shortdesc}

Authorized hosts and volumes must be in the same data center. For example, you can't have a replica volume in London and the host in Amsterdam. Both must be in London or both must be in Amsterdam.
{: note}

You can create the authorization in the [UI](/docs/FileStorage?topic=FileStorage-managingstorage&interface=ui#authhostUI), from the [CLI](/docs/FileStorage?topic=FileStorage-managingstorage&interface=cli#authhostCLI), with the API, or with [Terraform](/docs/FileStorage?topic=FileStorage-managingstorage&interface=terraform#authhostterraform).

This action breaks the replication relationship and restoring the connection between the primary and the replica location can be time-consuming.
{: important}

## Failing over to the replica volume in the UI
{: #DRFailoverUI}
{: ui}

1. Go to your list of {{site.data.keyword.filestorage_short}}. From the **Classic Infrastructure** ![Classic icon](../icons/classic.svg "Classic") menu, click **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Locate and click the volume name.
3. Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Failover**.
4. When the primary location is disabled, the option of Disaster Recovery Failover becomes active.
5. Click **Yes** to proceed.

## Failing over to the replica volume from the CLI
{: #DRFailoverCLI}
{: cli}

Before you begin, decide on the CLI client that you want to use.

* You can either install the [IBM Cloud CLI](/docs/cli){: external} and install the SL plug-in with `ibmcloud plugin install sl`. For more information, see [Extending IBM Cloud CLI with plug-ins](/docs/cli?topic=cli-plug-ins).
* Or, you can install the [SLCLI](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.

### Initiating a failover from the IBMCLOUDCLI
{: #DRFailoverICCLI}

You can use the `ibmcloud sl file replica-failover` command to fail over operations from the source file share to the replica file share. The following example initiates a failover from the source share `560156918` to the replica share `560382016`.

```sh
$ ibmcloud sl file file disaster-recovery-failover 560156918 560382016
OK
Failover of volume 560156918 to replica 560382016 is now in progress.
```
{: codeblock}

### Initiating a failover from the SLCLI
{: #DRFailoverICCLI}

Use the following command to fail a file volume over to a specific replicant volume.
```python
# slcli file disaster-recovery-failover --help
sage: slcli file disaster-recovery-failover [OPTIONS] VOLUME_ID

Options:
--replicant-id TEXT  ID of the replicant volume
-h, --help           Show this message and exit.
```

## Failing over to the replica volume with the API
{: #DRFailoverAPI}
{: api}

### REST API
{: #drrestaapi}

* URL - `https://USERNAME:APIKEY@api.softlayer.com/rest/v3/SoftLayer_Network_Storage/primaryvolumeId/disasterRecoveryFailoverToReplicant`
* Request body
 ```python
  {
    "parameters": [replicavolumeid]
  }
 ```

### SOAP API
{: #drsoapapi}

* URL - `https://api.softlayer.com/soap/v3/SoftLayer_Network_Storage`
* Request body
  ```python
   <?xml version="1.0" encoding="UTF-8"?>
   <SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://api.service.softlayer.com/soap/v3.1/">
    <SOAP-ENV:Header>
     <ns1:authenticate>
     <username>USERNAME</username>
     <apiKey>APIKEY</apiKey>
    </ns1:authenticate>
    <ns2:SoftLayer_Network_StorageInitParameters>
     <id>primary Volume Id</id>
    </ns2:SoftLayer_Network_StorageInitParameters>
    </SOAP-ENV:Header>
    <SOAP-ENV:Body>
     <ns1:disasterRecoveryFailoverToReplicant>
      <replicantId xsi:type="int">replica Volume ID</replicantId>
     </ns1:disasterRecoveryFailoverToReplicant>
    </SOAP-ENV:Body>
   </SOAP-ENV:Envelope>
   ```

During the Disaster Recovery Failover, the system is forced to fail over to the replica site and the replication relationship is severed. To be able to fail back to the original site after the site is restored to normal operations, the system must reestablish the replication bond. This action can take a considerable amount of time. During the Failback process, configuration-related actions are read-only. You can't edit any snapshot schedule or change snapshot space. The event is logged in the replication history.

If you need further assistance, create a [support case](/unifiedsupport/supportcenter){: external}.

## Failing back to the original primary site in the UI
{: #DRFailbackUI}
{: ui}

After a disaster event, {{site.data.keyword.cloud}} begins remediation work to return the impacted locations to normal operations. When the site is restored, you can initiate a Failback to the original site by clicking **Storage**, **{{site.data.keyword.filestorage_short}}** in the [{{site.data.keyword.cloud}} console](/cloud-storage/file){: external}.

1. Click your active volume ("target").
2. In the upper right, click **Replica** and click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions").
3. Select **Failback**. When the primary location is marked disabled, the option of Disaster Recovery Failback becomes active.

   During the Disaster Recovery Failover, the system is forced to fail over to the replica site and the replication relationship is severed. To be able to fail back to the original site after the site is restored to normal operations, the system must reestablish the replication bond. This action can take a considerable amount of time. Expect a message that shows that the failover is in progress. Additionally, an icon appears next to your volume on the **{{site.data.keyword.filestorage_short}}** that indicates that an active transaction is occurring. Hovering over the icon produces a window that shows the transaction. The icon disappears when the transaction is complete. During the Failback process, configuration-related actions are read-only. You can't edit any snapshot schedule or change snapshot space. The event is logged in replication history.
   {: note}

4. In the upper right, click **View All {{site.data.keyword.filestorage_short}}**.
5. Click your replica volume ("source"). This volume now has an **Inactive** status.
6. Mount and attach your storage volume to the host. For more information, see [connecting your storage](/docs/FileStorage?topic=FileStorage-getting-started#mountingstorage).

If you need further assistance, create a [support case](/unifiedsupport/supportcenter){: external}.

## Failing back from the CLI
{: #DRFailbackCLI}
{: cli}

### Initiating a failback from the IBMCLOUDCLI
{: #DRFailbackICCLI}

You can use the `ibmcloud sl file replica-failback` command to fail back operations from the replica file share to the original source file share. The following example initiates a failback to the original source share `560156918`.

```sh
$ ibmcloud sl file replica-failback 560156918
OK
Failback of volume 560156918 is now in progress.
```
{: codeblock}

For more information about all of the parameters that are available for this command, see [ibmcloud sl file replica-failover](/docs/cli?topic=cli-sl-file-storage-service#sl_file_replica_failover){: external}.

### Initiating a failback from the SLCLI
{: #DRFailbackSLCLI}

To fail back a file volume from a specific replicant volume, use the following command.
```python
# slcli file replica-failback --help
Usage: slcli file replica-failback [OPTIONS] VOLUME_ID

Options:
 --replicant-id TEXT  ID of the replicant volume
 -h, --help           Show this message and exit.
```

During the Disaster Recovery Failover, the system is forced to fail over to the replica site and the replication relationship is severed. To be able to fail back to the original site after the site is restored to normal operations, the system must reestablish the replication bond. This action can take a considerable amount of time. During the Failback process, configuration-related actions are read-only. You can't edit any snapshot schedule or change snapshot space. The event is logged in the replication history.

When the original volume is active, you can mount and attach it to the host. For more information, see [connecting your storage](/docs/FileStorage?topic=FileStorage-getting-started#mountingstorage).

If you need further assistance, create a [support case](/unifiedsupport/supportcenter){: external}.
