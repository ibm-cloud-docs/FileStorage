---

copyright:
  years: 2014, 2024
lastupdated: "2024-07-29"

keywords: File Storage for Classic, NSF, SLCLI, API

subcollection: FileStorage

---
{{site.data.keyword.attribute-definition-list}}

# SLCLI commands for {{site.data.keyword.filestorage_short}}
{: #SLCLIcommands}

You can use the SLCLI to accomplish tasks that are normally handled through the [{{site.data.keyword.cloud}} console](/cloud-storage/file){: external}. For example, from the CLI you can place orders for new volumes, snapshot space and replication, update authorizations, and cancel volumes.
{: shortdesc}

For more information about how to install and use the SLCLI, see [Python API Client](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.
{: tip}

## Access-related SLCLI commands
{: #slcliaccess}

* [Managing {{site.data.keyword.filestorage_short}}](/docs/FileStorage?topic=FileStorage-managingstorage)
   ```sh
   slcli file access-authorize
   slcli file access-list
   slcli file access-revoke
   ```

## Replication-related SLCLI commands
{: #slclireplica}

* [Replication-related SLCLI commands](/docs/FileStorage?topic=FileStorage-replication)
   ```sh
   slcli file replica-failback
   slcli file replica-failover
   slcli file replica-locations
   slcli file replica-order
   slcli file replica-partners
   ```

## Snapshots-related SLCLI commands
{: #slclisnapshot}

* [Ordering Snapshots](/docs/FileStorage?topic=FileStorage-ordering-snapshots)
   ```sh
   slcli file snapshot-order
   ```

* [Managing Snapshots](/docs/FileStorage?topic=FileStorage-managingSnapshots)
   ```sh
   slcli file snapshot-cancel
   slcli file snapshot-create
   slcli file snapshot-delete
   slcli file snapshot-disable
   slcli file snapshot-enable
   slcli file snapshot-list
   slcli file snapshot-restore
   ```

## Volume-related SLCLI commands
{: #slclivolume}

* [Ordering a {{site.data.keyword.filestorage_short}} volume](/docs/FileStorage?topic=FileStorage-orderingFileStorage&interface=cli#orderingthroughCLI)
   ```sh
   slcli file volume-order
   ```
* [Adjusting the IOPS](/docs/FileStorage?topic=FileStorage-adjustingIOPS)
   ```sh
   slcli file volume-modify
   ```
* [Expanding the capacity](/docs/FileStorage?topic=FileStorage-expandCapacity)
   ```sh
   slcli file volume-modify
   ```
* [Managing {{site.data.keyword.filestorage_short}}](/docs/FileStorage?topic=FileStorage-managingstorage)
   ```sh
   slcli file volume-cancel
   slcli file volume-count
   slcli file volume-detail
   slcli file volume-duplicate
   slcli file volume-list
   slcli file volume-order
   ```
* [Creating and managing duplicate volumes](/docs/FileStorage?topic=FileStorage-duplicatevolume)
   ```sh
   slcli file volume-duplicate
   slcli file volume-duplicate --dependent-duplicate TRUE <independent-vol-id>|
   slcli file volume-refresh <duplicate-vol-id> <parent-vol-snapshot-id>
   slcli file volume-convert <dependent-vol-id>
   slcli file duplicate-convert-status <dependent-vol-id>
   ```

* [Managing storage limits](/docs/FileStorage?topic=FileStorage-managinglimits)
   ```sh
   slcli file volume-limit
   slcli file volume-count
   ```
