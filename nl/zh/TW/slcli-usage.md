---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}

# {{site.data.keyword.filestorage_short}} 的 SL CLI 指令
{: #SLCLIcommands}

您可以使用 SL CLI 來採取如下動作：訂購新磁區、Snapshot 空間及抄寫、更新授權、取消磁區等等，正常情況下，這些動作是透過 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){:new_window} 來處理。

若要進一步瞭解如何安裝及使用 SL CLI，請參閱 [Python API 用戶端 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}。
{:tip}

## 存取相關的 SL CLI 指令
* [管理 {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage)  
  ```
  slcli file access-authorize
  slcli file access-list
  slcli file access-revoke
  ```

## 抄寫相關的 SL CLI 指令

* [抄寫相關的 SL CLI 指令](/docs/infrastructure/FileStorage?topic=FileStorage-replication#clicommands)
  ```
  slcli file replica-failback
  slcli file replica-failover
  slcli file replica-locations
  slcli file replica-order
  slcli file replica-partners
  ```

## Snapshot 相關的 SL CLI 指令

* [訂購 Snapshot](/docs/infrastructure/FileStorage?topic=FileStorage-ordering-snapshots)
  ```
  slcli file snapshot-order
  ```

* [管理 Snapshot](/docs/infrastructure/FileStorage?topic=FileStorage-managingSnapshots)
  ```
  slcli file snapshot-cancel
  slcli file snapshot-create
  slcli file snapshot-delete
  slcli file snapshot-disable
  slcli file snapshot-enable
  slcli file snapshot-list
  slcli file snapshot-restore
  ```

## 磁區相關的 SL CLI 指令

* [訂購 {{site.data.keyword.filestorage_short}} 磁區](/docs/infrastructure/FileStorage?topic=FileStorage-orderingSLCLI)
* [建立重複磁區](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume)
  ```
  slcli file volume-duplicate
  ```
* [調整 IOPS](/docs/infrastructure/FileStorage?topic=FileStorage-adjustingIOPS#steps)
  ```
  slcli file volume-modify
  ```
* [擴充容量](/docs/infrastructure/FileStorage?topic=FileStorage-expandCapacity#steps)
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
* [管理儲存空間限制](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits)  
  ```
  slcli file volume-count
  ```
