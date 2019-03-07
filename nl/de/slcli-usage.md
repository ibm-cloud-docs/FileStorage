---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}

# SLCLI-Befehle für {{site.data.keyword.filestorage_short}}
{: #SLCLIcommands}

Sie können die SLCLI verwenden, um Aktionen, wie z. B. das Bestellen neuer Datenträger, eines Snapshotbereichs oder einer Replikation, das Aktualisieren von Berechtigungen, das Stornieren von Datenträgern usw., auszuführen, die normalerweise über das [{{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){:new_window} vorgenommen werden. 

Weitere Informationen zur Installation und Verwendung der SLCLI finden Sie unter [Python-API-Client ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}.
{:tip}

## SLCLI-Befehle im Zusammenhang mit der Zugriffssteuerung
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
