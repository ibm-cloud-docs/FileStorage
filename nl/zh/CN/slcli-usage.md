---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, NSF, SLCLI, API

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}

# {{site.data.keyword.filestorage_short}} 的 SLCLI 命令
{: #SLCLIcommands}

您可以使用 SLCLI 来执行通常通过 [{{site.data.keyword.slportal}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/){:new_window} 处理的操作。例如，您可以使用 SLCLI 为新卷、快照空间和复制、更新授权、取消卷等下订单。

有关如何安装和使用 SLCLI 的更多信息，请参阅 [Python API 客户机 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}。
{:tip}

## 访问相关的 SLCLI 命令
* [管理 {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage)  
  ```
  slcli file access-authorize
  slcli file access-list
  slcli file access-revoke
  ```

## 复制相关的 SLCLI 命令

* [复制相关的 SLCLI 命令](/docs/infrastructure/FileStorage?topic=FileStorage-replication#clicommands)
  ```
  slcli file replica-failback
  slcli file replica-failover
  slcli file replica-locations
  slcli file replica-order
  slcli file replica-partners
  ```

## 快照相关的 SLCLI 命令

* [订购快照](/docs/infrastructure/FileStorage?topic=FileStorage-ordering-snapshots)
  ```
  slcli file snapshot-order
  ```

* [管理快照](/docs/infrastructure/FileStorage?topic=FileStorage-managingSnapshots)
  ```
  slcli file snapshot-cancel
  slcli file snapshot-create
  slcli file snapshot-delete
  slcli file snapshot-disable
  slcli file snapshot-enable
  slcli file snapshot-list
  slcli file snapshot-restore
  ```

## 卷相关的 SLCLI 命令

* [订购 {{site.data.keyword.filestorage_short}} 卷](/docs/infrastructure/FileStorage?topic=FileStorage-orderingSLCLI)
* [创建复制卷](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume)
  ```
  slcli file volume-duplicate
  ```
* [调整 IOPS](/docs/infrastructure/FileStorage?topic=FileStorage-adjustingIOPS#adjustingsteps)
  ```
  slcli file volume-modify
  ```
* [扩展容量](/docs/infrastructure/FileStorage?topic=FileStorage-expandCapacity#resizingsteps)
  ```
  slcli file volume-modify
  ```
* [管理 {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage)
  ```
  slcli file volume-cancel
  slcli file volume-count
  slcli file volume-detail
  slcli file volume-duplicate
  slcli file volume-list
  slcli file volume-order
  ```
* [管理存储限制](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits)
  ```
  slcli file volume-count
  ```
