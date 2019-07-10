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

# SLCLI-Befehle für {{site.data.keyword.filestorage_short}}
{: #SLCLIcommands}

Sie können die SLCLI verwenden, um Aktionen auszuführen, die normalerweise über die [{{site.data.keyword.cloud}}-Konsole](https://{DomainName}/classic){: external} ausgeführt werden. So können Sie über die SLCLI beispielsweise neue Datenträger, Snapshotbereiche und Replikationen bestellen, Berechtigungen aktualisieren, Datenträger stornieren usw.

Weitere Informationen zur Installation und Verwendung der SLCLI finden Sie unter [Python-API-Client](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.
{:tip}

## SLCLI-Befehle im Zusammenhang mit dem Zugriff
* [{{site.data.keyword.filestorage_short}} verwalten](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage)  
  ```
  slcli file access-authorize
  slcli file access-list
  slcli file access-revoke
  ```

## SLCLI-Befehle im Zusammenhang mit der Replikation

* [SLCLI-Befehle im Zusammenhang mit der Replikation](/docs/infrastructure/FileStorage?topic=FileStorage-replication#clicommands)
  ```
  slcli file replica-failback
  slcli file replica-failover
  slcli file replica-locations
  slcli file replica-order
  slcli file replica-partners
  ```

## SLCLI-Befehle im Zusammenhang mit Snapshots

* [Snapshots bestellen](/docs/infrastructure/FileStorage?topic=FileStorage-ordering-snapshots)
  ```
  slcli file snapshot-order
  ```

* [Snapshots verwalten](/docs/infrastructure/FileStorage?topic=FileStorage-managingSnapshots)
  ```
  slcli file snapshot-cancel
  slcli file snapshot-create
  slcli file snapshot-delete
  slcli file snapshot-disable
  slcli file snapshot-enable
  slcli file snapshot-list
  slcli file snapshot-restore
  ```

## SLCLI-Befehle im Zusammenhang mit Datenträgern

* [{{site.data.keyword.filestorage_short}}-Datenträger bestellen](/docs/infrastructure/FileStorage?topic=FileStorage-orderingSLCLI)
* [Duplikat eines Datenträgers erstellen](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume)
  ```
  slcli file volume-duplicate
  ```
* [IOPS anpassen](/docs/infrastructure/FileStorage?topic=FileStorage-adjustingIOPS#adjustingsteps)
  ```
  slcli file volume-modify
  ```
* [Kapazität erweitern](/docs/infrastructure/FileStorage?topic=FileStorage-expandCapacity#resizingsteps)
  ```
  slcli file volume-modify
  ```
* [{{site.data.keyword.filestorage_short}} verwalten](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage)
  ```
  slcli file volume-cancel
  slcli file volume-count
  slcli file volume-detail
  slcli file volume-duplicate
  slcli file volume-list
  slcli file volume-order
  ```
* [Speichergrenzwerte verwalten](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits)
  ```
  slcli file volume-count
  ```
