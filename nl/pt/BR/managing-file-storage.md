---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-10"

keywords: File Storage, file storage, NFS, authorizing hosts, rewoke access, grant access, view authorizations

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}


# Gerenciando o {{site.data.keyword.filestorage_short}}
{: #managingstorage}

É possível gerenciar seus volumes do {{site.data.keyword.filestorage_full}} por meio do console do {{site.data.keyword.cloud}}.

## Autorizando hosts a acessar o  {{site.data.keyword.filestorage_short}}

Hosts "Autorizados" são aqueles que receberam acesso a um volume específico. Sem a autorização do host, não é possível acessar ou usar o armazenamento de seu sistema. Autorizar um host a acessar seu volume gera o nome do usuário e a senha.

É possível autorizar e conectar hosts localizados no mesmo data center que seu armazenamento. É possível ter diversas contas, mas não é possível autorizar que um host de uma conta acesse seu armazenamento em outra conta.
{:important}

1. Acesse o [console do {{site.data.keyword.cloud}}](https://{DomainName}/){: external}. No menu, selecione **Infraestrutura clássica**.
2. Clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** e clique em seu **Nome do volume**.
3. Role para a seção **Hosts autorizados** da página.
4. Clique em  ** Autorizar host **  à direita. Selecione os hosts que podem acessar esse volume específico.

Como alternativa, é possível usar o comando a seguir no SLCLI.
```
# slcli file access-authorize --help
Usage: slcli file access-authorize [OPTIONS] VOLUME_ID

Opções:
  -h, --hardware-id TEXT    O ID de um servidor de hardware a ser autorizado.
  -v, --virtual-id TEXT     O ID de um servidor virtual a ser autorizado.
  -i, --ip-address-id TEXT  O ID de um endereço IP a ser autorizado.
  -p, --ip-address TEXT     Um endereço IP a ser autorizado.
  -s, --subnet-id TEXT      Um ID de uma sub-rede a ser autorizada.
  --help                    Mostrar essa mensagem e sair.
```

## Visualizando a lista de hosts que estão autorizados a acessar um volume do {{site.data.keyword.filestorage_short}}

1. Acesse o [console do {{site.data.keyword.cloud}}](https://{DomainName}/){: external}. No menu, selecione **Infraestrutura clássica**.
2. Clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** e clique em seu **Nome do volume**.
3. Role para baixo na página para a seção **Hosts autorizados**.

Ela exibe a lista de hosts que estão atualmente autorizados a acessar o volume.

Como alternativa, é possível usar o comando a seguir no SLCLI.
```
# slcli file access-list --help
Usage: slcli file access-list [OPTIONS] VOLUME_ID

Opções: --sortby TEXT Coluna para classificação --columns TEXT Colunas para exibição. Options: id, name, type,
                 private_ip_address, source_subnet, host_iqn, username,
                 password, allowed_host_id
 -h, --help      Show this message and exit.
```


## Visualizando os volumes do {{site.data.keyword.filestorage_short}} para os quais um host está autorizado

É possível visualizar os volumes aos quais um host tem acesso, incluindo as informações necessárias para fazer uma conexão - Nome do volume, Tipo de armazenamento, Endereço de destino, capacidade e local.

1. Acesse o [console do {{site.data.keyword.cloud}}](https://{DomainName}/){: external}
2. No menu, selecione **Infraestrutura clássica**.
3. Clique em **Dispositivos** > **Lista de dispositivos**.
4. Clique no dispositivo apropriado.
5. Selecione a guia Armazenamento.

Você é apresentado a uma lista de volumes de armazenamento aos quais esse host específico tem acesso, todos agrupados por tipo de armazenamento (bloco, arquivo, outro). Nos respectivos menus **Ação**, é possível autorizar mais armazenamento ou remover o acesso.


## Montando e desmontando o {{site.data.keyword.filestorage_short}}

É possível usar as informações do ponto de montagem fornecidas na visualização **Detalhes do volume** para montar o {{site.data.keyword.filestorage_short}} por meio de um host. Consulte  [ Acessando o  {{site.data.keyword.filestorage_short}}  no Linux ](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)


## Revogando o acesso de um host ao  {{site.data.keyword.filestorage_short}}

Se você deseja parar o acesso de um host a um volume de armazenamento específico, é possível revogar o acesso. Quando o acesso é revogado, a conexão de host é eliminada do volume. O sistema operacional e os aplicativos não podem mais se comunicar com o volume.

Para evitar problemas do lado do host, desmonte o volume de armazenamento de seu sistema operacional antes de revogar o acesso para evitar unidades ausentes ou distorção de dados.
{:important}

É possível revogar o acesso do Armazenamento da Lista de dispositivos ou das visualizações de Armazenamento.

### Revogando o acesso a partir da Lista de Dis

1. Acesse o [console do {{site.data.keyword.cloud}}](https://{DomainName}/){: external}.
2. No menu, selecione **Infraestrutura clássica**.
3. Clique em **Dispositivos** > **Lista de dispositivos**.
2. Clique duas vezes no dispositivo apropriado.
3. Selecione a guia **Armazenamento**.
4. É apresentada uma lista de volumes de armazenamento aos quais esse host específico tem acesso, todos agrupados por tipo de armazenamento (bloco, arquivo, outro). Selecione o respectivo menu **Ação** próximo ao volume do qual você deseja revogar o acesso e clique em **Revogar acesso**.
5. Confirme se você deseja revogar o acesso para um volume porque a ação não pode ser desfeita. Clique em **Sim** para revogar o acesso ao volume ou em **Não** para cancelar a ação.

Se você desejar desconectar vários volumes de um host específico, será necessário repetir a ação Revogar acesso para cada volume.
{:tip}


### Revogando o acesso por meio da Visualização de armazenamento

1. Acesse o [console do {{site.data.keyword.cloud}}](https://{DomainName}/){: external}.
2. No menu, selecione **Infraestrutura clássica**.
3. Clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** e selecione o **Volume** do qual deseja revogar o acesso.
4. Role para a seção **Hosts autorizados** da página.
5. Clique em **Ações** ao lado do host cujo acesso deve ser revogado e selecione **Revogar acesso**.
6. Confirme se você deseja revogar o acesso para um volume porque a ação não pode ser desfeita. Clique em **Sim** para revogar o acesso ao volume ou em **Não** para cancelar a ação.

Se você desejar desconectar vários hosts de um volume específico, será necessário repetir a ação Revogar acesso para cada host.
{:tip}

### Revogando o acesso por meio do SLCLI.
É possível usar o comando a seguir no SLCLI.
```
# slcli file access-revoke --help
Usage: slcli file access-revoke [OPTIONS] VOLUME_ID

Opções:
  -h, --hardware-id TEXT    O ID de um servidor de hardware para revogar a autorização.
  -v, --virtual-id TEXT     O ID de um servidor virtual para revogar a autorização.
  -i, --ip-address-id TEXT  O ID de um endereço IP para revogar a autorização.
  -p, --ip-address TEXT     Um endereço IP para revogar a autorização.
  -s, --subnet-id TEXT      Um ID de uma sub-rede para revogar a autorização.
  --help                    Mostrar essa mensagem e sair.
```

## Cancelando um volume de armazenamento

Se você não precisar mais de um volume específico, será possível cancelar esse armazenamento. Para cancelar um volume de armazenamento, é necessário revogar o acesso de qualquer host primeiro.

1. Acesse o [console do {{site.data.keyword.cloud}}](https://{DomainName}/){: external}. No menu, selecione **Infraestrutura clássica**.
2. Clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}**.
3. Clique em **Ações** para o volume a ser cancelado e selecione **Cancelar{{site.data.keyword.filestorage_short}}**.
4. Confirme se deseja cancelar o volume imediatamente ou na data de aniversário anual de quando o volume foi provisionado.

   Se você selecionar a opção para cancelar o volume na data de aniversário dele, será possível anular a solicitação de cancelamento antes da data de aniversário.
   {:tip}
5. Clique em **Continuar** ou **Fechar**.
6. Clique na caixa de seleção de confirmação e depois em **Confirmar**.

Como alternativa, é possível usar o comando a seguir no SLCLI.
```
# slcli file volume-cancel --help
Usage: slcli file volume-cancel [OPTIONS] VOLUME_ID

Options:
  --reason TEXT  An optional reason for cancellation
  --immediate    Cancels the file storage volume immediately instead of on the
                 billing anniversary
  -h, --help     Show this message and exit.
```
