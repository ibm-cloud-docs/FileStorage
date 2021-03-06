---

copyright:
  years: 2018, 2021
lastupdated: "2021-02-15"

keywords: File Storage, file storage, NFS, disaster recovery, duplicate volume, replica volume, failover, failback,

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
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Disaster Recovery - Fail over from an inaccessible Primary volume
{: #dr-inaccessible}

If a catastrophic failure or disaster causes an outage on the primary site, customers can perform the following actions to quickly access their data on the secondary site. When the primary volume is inaccessible, you can force a failover to the remote replica.
{:shortdesc}

This action breaks the replication relationship and cannot be undone without manual intervention from the support team.
{:important}

## Fail over to the replica volume from the SLCLI
{: #DRFailoverCLI}
{: cli}

Use the following command to fail a file volume over to a specific replicant volume.
```
  # slcli file disaster-recovery-failover --help
  Usage: slcli file disaster-recovery-failover [OPTIONS] VOLUME_ID

  Options:
  --replicant-id TEXT  ID of the replicant volume
  -h, --help           Show this message and exit.
```

## Fail over to the replica volume with the API
{: #DRFailoverAPI}
{: api}

### REST API
{: #drrestaapi}
* URL: `https://USERNAME:APIKEY@api.softlayer.com/rest/v3/SoftLayer_Network_Storage/primaryvolumeId/disasterRecoveryFailoverToReplicant`
* Request body
  ```
  {
    "parameters": [replicavolumeid]
  }
  ```

### SOAP API
{: #drsoapapi}
* URL: `https://api.softlayer.com/soap/v3/SoftLayer_Network_Storage`
* Request body
  ```
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

## Fail back to the original primary site
{: #DRFailback}

If you want to return production to the original primary site, create a [support case](https://cloud.ibm.com/unifiedsupport/supportcenter){: external}. For more information about opening a support case or case severities and response times, see [Viewing support cases](/docs/get-support?topic=get-support-managing-support-cases) or [Escalating support cases](/docs/get-support?topic=get-support-escalation).
