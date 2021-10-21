---

copyright:
  years: 2014, 2020
lastupdated: "2020-04-17"

keywords: File Storage, NSF, SLCLI, API

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}
{:shortdesc: .shortdesc}

# SLCLI commands for {{site.data.keyword.filestorage_short}}
{: #SLCLIcommands}

You can use the SLCLI to perform operations that are normally handled through the [{{site.data.keyword.cloud}} console](https://{DomainName}/classic/storage/file){: external}. For example, from the SLCLI you can place orders for new volumes, snapshot space and replication, update authorizations, and cancel volumes.
{: shortdesc}

For more information about how to install and use the SLCLI, see [Python API Client](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.
{: tip}

## Access-related SLCLI commands
* [Managing {{site.data.keyword.filestorage_short}}](/docs/FileStorage?topic=FileStorage-managingstorage)  
   ```python
   slcli file access-authorize
   slcli file access-list
   slcli file access-revoke
   ```

## Replication-related SLCLI commands

* [Replication-related SLCLI commands](/docs/FileStorage?topic=FileStorage-replication)
   ```python
   slcli file replica-failback
   slcli file replica-failover
   slcli file replica-locations
   slcli file replica-order
   slcli file replica-partners
   ```

## Snapshots-related SLCLI commands

* [Ordering Snapshots](/docs/FileStorage?topic=FileStorage-ordering-snapshots)
   ```python
   slcli file snapshot-order
   ```

* [Managing Snapshots](/docs/FileStorage?topic=FileStorage-managingSnapshots)
   ```python
   slcli file snapshot-cancel
   slcli file snapshot-create
   slcli file snapshot-delete
   slcli file snapshot-disable
   slcli file snapshot-enable
   slcli file snapshot-list
   slcli file snapshot-restore
   ```

## Volume-related SLCLI commands

* [Ordering a {{site.data.keyword.filestorage_short}} volume](/docs/FileStorage?topic=FileStorage-orderingFileStorage#orderingthroughCLI)
   ```python
   slcli file volume-order
   ```
* [Creating an independent duplicate volume](/docs/FileStorage?topic=FileStorage-duplicatevolume)
   ```python
   slcli file volume-duplicate
   ```
* [Creating and managing a dependent duplicate volume](/docs/FileStorage?topic=FileStorage-dependentduplicate)
   ```python
   slcli file  volume-duplicate --dependent-duplicate TRUE <independent-vol-id>|
   slcli file  volume-refresh <dependent-vol-id> <independent-snapshot-id>
   slcli file  volume-convert <dependent-vol-id>
   ```

* [Adjusting the IOPS](/docs/FileStorage?topic=FileStorage-adjustingIOPS)
   ```python
   slcli file volume-modify
   ```
* [Expanding the capacity](/docs/FileStorage?topic=FileStorage-expandCapacity)
   ```python
   slcli file volume-modify
   ```
* [Managing {{site.data.keyword.filestorage_short}}](/docs/FileStorage?topic=FileStorage-managingstorage)
   ```python
   slcli file volume-cancel
   slcli file volume-count
   slcli file volume-detail
   slcli file volume-duplicate
   slcli file volume-list
   slcli file volume-order
   ```
* [Managing storage limits](/docs/FileStorage?topic=FileStorage-managinglimits)
   ```python
   slcli file volume-limit
   slcli file volume-count
   ```
