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

# Mandatos de CLI de SL para {{site.data.keyword.filestorage_short}}
{: #SLCLIcommands}

Puede utilizar la CLI de SL para realizar acciones tales como pedidos de nuevos volúmenes, espacio de instantáneas y réplicas, actualización de autorizaciones, cancelación de volúmenes y otras similares que normalmente se manejan a través del [{{site.data.keyword.slportal}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/){:new_window}.

Para obtener más información sobre cómo instalar y utilizar la CLI de SL, consulte [Cliente de API de Python ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}.
{:tip}

## Mandatos de CLI de SL relacionados con el acceso
* [Gestión de {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage)  
  ```
  slcli file access-authorize
  slcli file access-list
  slcli file access-revoke
  ```

## Mandatos de CLI de SL relacionados con la replicación

* [Mandatos de CLI de SL relacionados con la replicación](/docs/infrastructure/FileStorage?topic=FileStorage-replication#clicommands)
  ```
  slcli file replica-failback
  slcli file replica-failover
  slcli file replica-locations
  slcli file replica-order
  slcli file replica-partners
  ```

## Mandatos de CLI de SL relacionados con las instantáneas

* [Realizar pedidos de instantáneas](/docs/infrastructure/FileStorage?topic=FileStorage-ordering-snapshots)
  ```
  slcli file snapshot-order
  ```

* [Gestión de instantáneas](/docs/infrastructure/FileStorage?topic=FileStorage-managingSnapshots)
  ```
  slcli file snapshot-cancel
  slcli file snapshot-create
  slcli file snapshot-delete
  slcli file snapshot-disable
  slcli file snapshot-enable
  slcli file snapshot-list
  slcli file snapshot-restore
  ```

## Mandatos de CLI de SL relacionados con los volúmenes

* [Solicitud de un volumen de {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-orderingSLCLI)
* [Creación de un volumen duplicado](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume)
  ```
  slcli file volume-duplicate
  ```
* [Ajuste del IOPS](/docs/infrastructure/FileStorage?topic=FileStorage-adjustingIOPS#steps)
  ```
  slcli file volume-modify
  ```
* [Expansión de la capacidad](/docs/infrastructure/FileStorage?topic=FileStorage-expandCapacity#steps)
  ```
  slcli file volume-modify
  ```
* [Gestión de {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage)  
  ```
  slcli file volume-cancel
  slcli file volume-count
  slcli file volume-detail
  slcli file volume-duplicate
  slcli file volume-list
  slcli file volume-order
  ```
* [Gestión de los límites de almacenamiento](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits)  
  ```
  slcli file volume-count
  ```
