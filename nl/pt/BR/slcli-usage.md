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

# Comandos SLCLI para o {{site.data.keyword.filestorage_short}}
{: #SLCLIcommands}

É possível usar o SLCLI para executar ações que normalmente são manipuladas por meio do [console do {{site.data.keyword.cloud}}](https://{DomainName}/classic){: external}. Por exemplo, com o SLCLI, é possível fazer pedidos de novos volumes, de espaço de captura instantânea e de replicação, atualizar autorizações, cancelar volumes e assim por diante.

Para obter mais informações sobre como instalar e usar o SLCLI, consulte [Cliente da API do Python](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.
{:tip}

## Comandos do SLCLI relacionados ao acesso
* [Gerenciando o {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage)  
  ```
  slcli file access-authorize
  slcli file access-list
  slcli file access-revoke
  ```

## Comandos do SLCLI relacionados à replicação

* [Comandos do SLCLI relacionados à replicação](/docs/infrastructure/FileStorage?topic=FileStorage-replication#clicommands)
  ```
  slcli file replica-failback
  slcli file replica-failover
  slcli file replica-locations
  slcli file replica-order
  slcli file replica-partners
  ```

## Comandos do SLCLI relacionados a capturas instantâneas

* [Solicitando capturas instantâneas](/docs/infrastructure/FileStorage?topic=FileStorage-ordering-snapshots)
  ```
  captura instantânea do arquivo slcli-ordem
  ```

* [Gerenciando capturas instantâneas](/docs/infrastructure/FileStorage?topic=FileStorage-managingSnapshots)
  ```
  slcli file snapshot-cancel
  slcli file snapshot-create
  slcli file snapshot-delete
  slcli file snapshot-disable
  slcli file snapshot-enable
  slcli file snapshot-list
  slcli file snapshot-restore
  ```

## Comandos do SLCLI relacionados a um volume

* [ Ordenando um  {{site.data.keyword.filestorage_short}}  volume ](/docs/infrastructure/FileStorage?topic=FileStorage-orderingSLCLI)
* [ Criando um volume duplicado ](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume)
  ```
  volume de arquivo slcli-duplicar
  ```
* [ Ajustando o IOPS ](/docs/infrastructure/FileStorage?topic=FileStorage-adjustingIOPS#adjustingsteps)
  ```
  slcli file volume-modify
  ```
* [ Expandindo a Capacidade ](/docs/infrastructure/FileStorage?topic=FileStorage-expandCapacity#resizingsteps)
  ```
  slcli file volume-modify
  ```
* [Gerenciando o {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage)
  ```
  slcli file volume-cancel
  slcli file volume-count
  slcli file volume-detail
  slcli file volume-duplicate
  slcli file volume-list
  slcli file volume-order
  ```
* [ Gerenciando Limites de Armazenamento ](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits)
  ```
  slcli file volume-count
  ```
