---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-10"

keywords: File Storage, NSF, SLCLI, API

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}

# {{site.data.keyword.filestorage_short}} の SLCLI コマンド
{: #SLCLIcommands}

通常 [{{site.data.keyword.cloud}} コンソール](https://{DomainName}/classic){: external}で処理する操作を、SLCLI を使用して実行することができます。例えば、新規ボリュームやスナップショット・スペースおよびレプリケーションの注文、許可の更新、ボリュームのキャンセルなどを、SLCLI で行うことができます。

SLCLI のインストール方法と使用法について詳しくは、[Python API Client](https://softlayer-python.readthedocs.io/en/latest/cli/){: external} を参照してください。
{:tip}

## アクセス関連の SLCLI コマンド
* [{{site.data.keyword.filestorage_short}} の管理](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage)  
  ```
  slcli file access-authorize
  slcli file access-list
  slcli file access-revoke
  ```

## レプリケーション関連の SLCLI コマンド

* [レプリケーション関連の SLCLI コマンド](/docs/infrastructure/FileStorage?topic=FileStorage-replication#clicommands)
  ```
  slcli file replica-failback
  slcli file replica-failover
  slcli file replica-locations
  slcli file replica-order
  slcli file replica-partners
  ```

## スナップショット関連の SLCLI コマンド

* [スナップショットの注文](/docs/infrastructure/FileStorage?topic=FileStorage-ordering-snapshots)
  ```
  slcli file snapshot-order
  ```

* [スナップショットの管理](/docs/infrastructure/FileStorage?topic=FileStorage-managingSnapshots)
  ```
  slcli file snapshot-cancel
  slcli file snapshot-create
  slcli file snapshot-delete
  slcli file snapshot-disable
  slcli file snapshot-enable
  slcli file snapshot-list
  slcli file snapshot-restore
  ```

## ボリューム関連の SLCLI コマンド

* [{{site.data.keyword.filestorage_short}} ボリュームの注文](/docs/infrastructure/FileStorage?topic=FileStorage-orderingSLCLI)
* [複製ボリュームの作成](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume)
  ```
  slcli file volume-duplicate
  ```
* [IOPS の調整](/docs/infrastructure/FileStorage?topic=FileStorage-adjustingIOPS#adjustingsteps)
  ```
  slcli file volume-modify
  ```
* [容量の拡張](/docs/infrastructure/FileStorage?topic=FileStorage-expandCapacity#resizingsteps)
  ```
  slcli file volume-modify
  ```
* [{{site.data.keyword.filestorage_short}} の管理](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage)
  ```
  slcli file volume-cancel
  slcli file volume-count
  slcli file volume-detail
  slcli file volume-duplicate
  slcli file volume-list
  slcli file volume-order
  ```
* [ストレージ制限の管理](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits)
  ```
  slcli file volume-count
  ```
