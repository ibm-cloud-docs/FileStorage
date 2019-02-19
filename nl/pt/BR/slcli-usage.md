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

# Comandos da CLI SL para  {{site.data.keyword.filestorage_short}}
{: #SLCLIcommands}

É possível usar a CLI do SL para executar ações, como fazer pedidos para novos volumes, espaço de captura instantânea e replicação, atualizar autorizações, cancelar volumes e assim por diante, que são normalmente manipulados por meio do [{{site.data.keyword.slportal}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/){:new_window}.

Para obter mais informações sobre como instalar e usar a CLI do SL, consulte [Cliente da API da Python ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}.
{:tip}

## Acessar comandos da CLI SL relacionados
* [Gerenciando o {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage)  
  ```
  slcli file access-authorize
  slcli file access-list
  slcli file access-revoke
  ```

## Comandos da CLI do SL relacionados a replicação

* [ Comandos da CLI do SL relacionados a replicação ](/docs/infrastructure/FileStorage?topic=FileStorage-replication#clicommands)
  ```
  slcli file replica-failback
  slcli file replica-failover
  slcli file replica-locations
  slcli file replica-order
  slcli file replica-partners
  ```

## Comandos da CLI do SL Relacionados ao Snapshots

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

## Comandos da CLI SL Relacionados ao Volume

* [ Ordenando um  {{site.data.keyword.filestorage_short}}  volume ](/docs/infrastructure/FileStorage?topic=FileStorage-orderingSLCLI)
* [ Criando um volume duplicado ](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume)
  ```
  volume de arquivo slcli-duplicar
  ```
* [ Ajustando o IOPS ](/docs/infrastructure/FileStorage?topic=FileStorage-adjustingIOPS#steps)
  ```
  slcli file volume-modify
  ```
* [ Expandindo a Capacidade ](/docs/infrastructure/FileStorage?topic=FileStorage-expandCapacity#steps)
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
