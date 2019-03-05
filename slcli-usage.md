---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, nsf, slcli, API

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}

# SLCLI commands for {{site.data.keyword.filestorage_short}}
{: #SLCLIcommands}

You can use the SLCLI to take actions that are normally handled through the [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}. For example, with SLCLI you can place orders for new volumes, snapshot space and replication, update authorizations, cancel volumes, and so on.

For more information about how to install and use the SLCLI, see [Python API Client ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}.
{:tip}

## Access-related SLCLI commands
* [Managing {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage)  
  ```
  slcli file access-authorize
  slcli file access-list
  slcli file access-revoke
  ```

## Replication-related SLCLI commands

* [Replication related SLCLI commands](/docs/infrastructure/FileStorage?topic=FileStorage-replication#clicommands)
  ```
  slcli file replica-failback
  slcli file replica-failover
  slcli file replica-locations
  slcli file replica-order
  slcli file replica-partners
  ```

## Snapshots-related SLCLI commands

* [Ordering Snapshots](/docs/infrastructure/FileStorage?topic=FileStorage-ordering-snapshots)
  ```
  slcli file snapshot-order
  ```

* [Managing Snapshots](/docs/infrastructure/FileStorage?topic=FileStorage-managingSnapshots)
  ```
  slcli file snapshot-cancel
  slcli file snapshot-create
  slcli file snapshot-delete
  slcli file snapshot-disable
  slcli file snapshot-enable
  slcli file snapshot-list
  slcli file snapshot-restore
  ```

## Volume-related SLCLI commands

* [Ordering a {{site.data.keyword.filestorage_short}} volume](/docs/infrastructure/FileStorage?topic=FileStorage-orderingSLCLI)
* [Creating a duplicate volume](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume)
  ```
  slcli file volume-duplicate
  ```
* [Adjusting the IOPS](/docs/infrastructure/FileStorage?topic=FileStorage-adjustingIOPS#adjustingsteps)
  ```
  slcli file volume-modify
  ```
* [Expanding the capacity](/docs/infrastructure/FileStorage?topic=FileStorage-expandCapacity#resizingsteps)
  ```
  slcli file volume-modify
  ```
* [Managing {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage)
  ```
  slcli file volume-cancel
  slcli file volume-count
  slcli file volume-detail
  slcli file volume-duplicate
  slcli file volume-list
  slcli file volume-order
  ```
* [Managing storage limits](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits)
  ```
  slcli file volume-count
  ```
