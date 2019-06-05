---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, NSF, SLCLI, API

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}

# Commandes SLCLI de {{site.data.keyword.filestorage_short}}
{: #SLCLIcommands}

Vous pouvez utiliser l'interface de ligne de commande SL pour effectuer des actions qui sont normalement gérées via le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}. Par exemple, avec l'interface SLCLI vous pouvez passer des commandes de nouveaux volumes, d'espace d'instantané et de réplication, mettre à jour des autorisations, annuler des volumes, etc.

Pour plus d'informations sur la manière d'installer et d'utiliser l'interface de ligne de commande SL, voir [Client API Python](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.
{:tip}

## Commandes SLCLI liées aux accès
* [Gestion de {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage)  
  ```
  slcli file access-authorize
  slcli file access-list
  slcli file access-revoke
  ```

## Commandes SLCLI liées à la réplication

* [Commandes SLCLI liées à la réplication](/docs/infrastructure/FileStorage?topic=FileStorage-replication#clicommands)
  ```
  slcli file replica-failback
  slcli file replica-failover
  slcli file replica-locations
  slcli file replica-order
  slcli file replica-partners
  ```

## Commandes SLCLI liées aux instantanés

* [Commande d'instantanés](/docs/infrastructure/FileStorage?topic=FileStorage-ordering-snapshots)
  ```
  slcli file snapshot-order
  ```

* [Gestion des instantanés](/docs/infrastructure/FileStorage?topic=FileStorage-managingSnapshots)
  ```
  slcli file snapshot-cancel
  slcli file snapshot-create
  slcli file snapshot-delete
  slcli file snapshot-disable
  slcli file snapshot-enable
  slcli file snapshot-list
  slcli file snapshot-restore
  ```

## Commandes SLCLI liées aux volumes

* [Commande d'un volume {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-orderingSLCLI)
* [Création d'un volume dupliqué](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume)
  ```
  slcli file volume-duplicate
  ```
* [Réglage des IOPS](/docs/infrastructure/FileStorage?topic=FileStorage-adjustingIOPS#adjustingsteps)
  ```
  slcli file volume-modify
  ```
* [Augmentation de la capacité](/docs/infrastructure/FileStorage?topic=FileStorage-expandCapacity#resizingsteps)
  ```
  slcli file volume-modify
  ```
* [Gestion de {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage)
  ```
  slcli file volume-cancel
  slcli file volume-count
  slcli file volume-detail
  slcli file volume-duplicate
  slcli file volume-list
  slcli file volume-order
  ```
* [Gestion des limites de stockage](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits)
  ```
  slcli file volume-count
  ```
