---

copyright:
  years: 2018, 2021
lastupdated: "2021-06-10"

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
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Disaster Recovery - Fail over with an accessible primary volume
{: #dr-accessible}

If a catastrophic failure or disaster occurs on the primary site and the primary storage is still accessible, customers can perform the following actions to quickly access their data on the secondary site.

Before you start the failover, make sure that all host-authorization is in place.
{: important}

Authorized hosts and volumes must be in the same data center. For example, you can't have a replica volume in London and the host in Amsterdam. Both must be in London or both must be in Amsterdam.
{: note}

## Authorizing the host in the UI
{: #authreplicahostUI}
{: ui}

You can authorize a host to access the {{site.data.keyword.filestorage_full}} volume through the [{{site.data.keyword.cloud}} console](https://{DomainName}/cloud-storage/file){: external}.

1. Log in to the [{{site.data.keyword.cloud}} console](https://{DomainName}/){: external} and click the **menu** icon on the upper left. Select **Classic Infrastructure**.
2. Locate the source or destination volume in the **{{site.data.keyword.filestorage_short}}** list. Its replica volume is listed under the source volume in the inactive status.
3. Click the replica name and on the next screen, click **Actions**. From the menu, select **Authorize Hosts**.
4. Select a host type and then choose a host from the dropdown that is available for the volume. Filter the available host list by the device type, subnet, or IP address.

   When the list is filtered by subnet, the subnets that are displayed are subscribed subnets in the same data center as the storage volume.
   {: note}
5. Highlight the host that is to be authorized for replications. To select multiple hosts, use the CTRL-key and click the applicable hosts.
6. Click **Save**. If you have no hosts, you are prompted to purchase compute resources in the same data center.

## Authorizing the host from the SLCLI
{: #authreplicahostCLI}
{: cli}

To authorize the hosts in the replica datacenter, use the following command.
```
# slcli file access-authorize --help
Usage: slcli file access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of one hardware server to authorize.
  -v, --virtual-id TEXT     The ID of one virtual server to authorize.
  -i, --ip-address-id TEXT  The ID of one IP address to authorize.
  -p, --ip-address TEXT     An IP address to authorize.
  -s, --subnet-id TEXT      An ID of one subnet to authorize.
  --help                    Show this message and exit.
```

## Starting a failover from a volume to its replica
{: #failovertoreplica}

If a failure event is imminent, you can start an **Immediate failover** or a "Controlled Failover" to your destination, or target, volume.

When you choose an Immediate Failover, the last successfully replicated snapshot is activated, and the volume is made available for mounting. The target volume becomes active in less time compared to a Controlled Failover. However, any data that was written to the source volume since the previous replication cycle is lost.

A Controlled Failover is the best choice when you want to test the failover functionality or when it’s more important to continue operations at the replica location with the most recent data. In a Controlled Failover, a new snapshot is taken and copied over to the replica location. After the data is successfully copied over, the volume is made available for mounting.

When a failover is started, the replication relationship is flipped. Your target volume becomes your source volume, and your former source volume becomes your target as indicated by the **Volume Name** followed by **REP**.

Failovers are started under **Storage**, **{{site.data.keyword.filestorage_short}}** in the [{{site.data.keyword.cloud}} console](https://{DomainName}/classic/storage/file){: external}.

Before you proceed with these steps, disconnect the volume. Failure to do so, results in corruption and data loss.
{: important}

## Fail over to replica in the UI
{: #failovertoreplicaUI}
{: ui}

1. Click your active volume (“source”).
2. In the upper right, click **Actions**.
3. Select **Controlled Failover** or **Immediate Failover**.

   Expect a message that states that the failover is in progress. Additionally, an icon appears next to your volume on the **{{site.data.keyword.filestorage_short}}** that indicates that an active transaction is occurring. Hovering over the icon produces a window that shows the transaction. The icon disappears when the transaction is complete. During the failover process, configuration-related actions are read-only. You can't edit any snapshot schedule or change snapshot space. The event is logged in replication history.<br/> When your target volume is live, you get another message. Your original source volume's Status becomes Inactive.
   {: note}
4. Click **View All ({{site.data.keyword.filestorage_short}})**.
5. Click your active volume (formerly your target volume). This volume now has an **Active** status.
6. Mount and attach your storage volume to the host. For more information, see [connecting your new storage](/docs/FileStorage?topic=FileStorage-getting-started#mountingstorage).

## Fail over to replica from the SLCLI
{: #failovertoreplicaCLI}
{: cli}

To fail over a file volume to a specific replicant volume, use the following command.
  ```
  # slcli file replica-failover --help
  Usage: slcli file replica-failover [OPTIONS] VOLUME_ID

  Options:
  --replicant-id TEXT  ID of the replicant volume
  --immediate          Failover to replicant immediately.
  -h, --help           Show this message and exit.
  ```

## Starting a failback from a volume to its replica
{: #failbackfromreplica}

When your original source volume is repaired, you can start a controlled Failback to your original source volume. In a controlled Failback,

- The acting source volume is taken offline,
- A snapshot is taken,
- The replication cycle is completed,
- The just-taken data snapshot is activated,
- And the source volume becomes active for mounting.

When a Failback is started, the replication relationship is flipped again. Your source volume is restored as your source volume, and your target volume is the target volume again as indicated by the **Volume Name** followed by **REP**.

## Fail back in the UI
{: #failbackfromreplicaUI}
{: ui}

Failbacks are started under **Storage**, **{{site.data.keyword.filestorage_short}}** in the [{{site.data.keyword.cloud}} console](https://{DomainName}/classic/storage/file){: external}.

1. Click your active volume ("target").
2. In the upper right, click **Replica** and click **Actions**.
3. Select **Controlled Failback**.

   Expect a message that shows the failover is in progress. Additionally, an icon appears next to your volume on the **{{site.data.keyword.filestorage_short}}** that indicates that an active transaction is occurring. Hovering over the icon produces a window that shows the transaction. The icon disappears when the transaction is complete. During the Failback process, configuration-related actions are read-only. You can't edit any snapshot schedule or change snapshot space. The event is logged in replication history.
   {: note}
4. In the upper right, click **View All {{site.data.keyword.filestorage_short}}**.
5. Click your active volume ("source"). This volume now has an **Inactive** status.
6. Mount and attach your storage volume to the host. For more information, see [connecting your new storage](/docs/FileStorage?topic=FileStorage-getting-started#mountingstorage).

## Fail back from the SLCLI
{: #failbackfromreplicaCLI}
{: cli}

To fail back a file volume from a specific replicant volume.
  ```
  # slcli file replica-failback --help
  Usage: slcli file replica-failback [OPTIONS] VOLUME_ID

  Options:
  --replicant-id TEXT  ID of the replicant volume
  -h, --help           Show this message and exit.
  ```
