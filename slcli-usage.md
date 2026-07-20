---

copyright:
  years: 2014, 2026
lastupdated: "2026-07-14"

keywords: File Storage for Classic, NSF, SLCLI, API

subcollection: FileStorage

---
{{site.data.keyword.attribute-definition-list}}

#  Managing {{site.data.keyword.filestorage_short}} with SLCLI commands
{: #SLCLIcommands}

Use the command-line interface (SLCLI) to manage file storage volumes, place orders, update authorizations, and cancel volumes in the {{site.data.keyword.cloud_notm}}.
{: shortdesc}

For more information about how to install and use the SL CLI, see [Python API Client](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.
{: tip}

## Access-related SLCLI commands
{: #slcliaccess}

* [Managing {{site.data.keyword.filestorage_short}}](/docs/FileStorage?topic=FileStorage-managingstorage)
   ```sh
   slcli file access-authorize
   slcli file access-list
   slcli file access-revoke
   ```
   {: codeblock}

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
   {: codeblock}

## Snapshots-related SLCLI commands
{: #slclisnapshot}

* [Ordering Snapshots](/docs/FileStorage?topic=FileStorage-ordering-snapshots)
   ```sh
   slcli file snapshot-order
   ```
   {: codeblock}

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
   {: codeblock}

## Volume-related SLCLI commands
{: #slclivolume}

* [Ordering a {{site.data.keyword.filestorage_short}} volume](/docs/FileStorage?topic=FileStorage-orderingFileStorage&interface=cli#orderingthroughCLI)
   ```sh
   slcli file volume-order
   ```
   {: codeblock}

* [Adjusting the IOPS](/docs/FileStorage?topic=FileStorage-adjustingIOPS)
   ```sh
   slcli file volume-modify VOLUME_ID
   ```
   {: codeblock}

* [Expanding the capacity](/docs/FileStorage?topic=FileStorage-expandCapacity)
   ```sh
   slcli file volume-modify VOLUME_ID
   ```
   {: codeblock}

* [Managing {{site.data.keyword.filestorage_short}}](/docs/FileStorage?topic=FileStorage-managingstorage)
   ```sh
   slcli file volume-cancel VOLUME_ID
   slcli file volume-count
   slcli file volume-detail VOLUME_ID
   slcli file volume-duplicate ORIGIN_VOLUME_ID
   slcli file volume-list
   slcli file volume-order
   ```
   {: codeblock}

* [Creating and managing duplicate volumes](/docs/FileStorage?topic=FileStorage-duplicatevolume)
   ```sh
   slcli file volume-duplicate ORIGIN_VOLUME_ID
   slcli file volume-duplicate ORIGIN_VOLUME_ID --dependent-duplicate TRUE
   slcli file volume-refresh DUPLICATE_VOLUME_ID ORIGIN_VOLUME_SNAPSHOT_ID
   slcli file volume-convert DUPLICATE_VOLUME_ID
   slcli
   file duplicate-convert-status DUPLICATE_VOLUME_ID
   ```
   {: codeblock}

* [Managing storage limits](/docs/FileStorage?topic=FileStorage-managinglimits)
   ```sh
   slcli file volume-limit
   slcli file volume-count
   ```
   {: codeblock}
