---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-26"

---
{:new_window: target="_blank"}


# Gerenciando o {{site.data.keyword.filestorage_short}}

É possível gerenciar seus volumes do  {{site.data.keyword.filestorage_full}}  por meio do  {{site.data.keyword.slportal}}.

## Autorizando hosts a acessar o  {{site.data.keyword.filestorage_short}}

Hosts "Autorizados" são aqueles que receberam acesso a um volume específico. Sem a autorização do host, não é possível acessar ou usar o armazenamento de seu sistema. Autorizar um host a acessar seu volume gera o nome do usuário e a senha. 

**Nota** - é possível autorizar e conectar hosts que estão localizados no mesmo data center que seu armazenamento. No entanto, se você tiver múltiplas contas, não será possível autorizar o host de uma conta a acessar seu armazenamento em outra. 

1. Clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** e clique em seu **Nome do volume**.
2. Role para a seção **Hosts autorizados** da página.
3. Clique em  ** Autorizar host **  à direita. Selecione os hosts que podem acessar esse volume específico.
 

## Visualizando a lista de hosts que estão autorizados a acessar um volume do {{site.data.keyword.filestorage_short}}

1. Clique em **Armazenamento > {{site.data.keyword.filestorage_short}}** e clique em seu **Nome do volume**.
2. Role para baixo na página para a seção **Hosts autorizados**.

Aqui é possível ver a lista de hosts que estão atualmente autorizados a acessar o volume.


## Visualizando os volumes do {{site.data.keyword.filestorage_short}} para os quais um host está autorizado

É possível visualizar os volumes aos quais um host tem acesso, incluindo as informações necessárias para fazer uma conexão - Nome do volume, Tipo de armazenamento, Endereço de destino, capacidade e local.

1. Clique em **Dispositivos** > **Lista de dispositivos** no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}
2. Clique no dispositivo apropriado.
2. Selecione a guia Armazenamento.

Você é apresentado a uma lista de volumes de armazenamento aos quais esse host específico tem acesso, todos agrupados por tipo de armazenamento (bloco, arquivo, outro). Nos respectivos menus **Ação**, é possível autorizar mais armazenamento ou remover o acesso.


## Montando e desmontando o  {{site.data.keyword.filestorage_short}}

É possível usar as informações do ponto de montagem fornecidas na visualização **Detalhes do volume** para montar o {{site.data.keyword.filestorage_short}} por meio de um host. Consulte  [ Acessando o  {{site.data.keyword.filestorage_short}}  no Linux ](accessing-file-storage-linux.html)


## Revogando o acesso de um host ao  {{site.data.keyword.filestorage_short}}

Se você deseja parar o acesso de um host a um volume de armazenamento específico, é possível revogar o acesso. Quando o acesso é revogado, a conexão de host é eliminada do volume. O sistema operacional e os aplicativos não podem mais se comunicar com o volume. 

**Nota** - Para evitar problemas do lado do host, desmonte o volume de armazenamento de seu sistema operacional antes de revogar o acesso para evitar unidades ausentes ou distorção de dados.

É possível revogar o acesso do Armazenamento da Lista de dispositivos ou das visualizações de Armazenamento.

### Revogando o acesso a partir da Lista de Dis

1. Clique em **Dispositivos** > **Lista de dispositivos** no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 
2. Clique duas vezes no dispositivo apropriado.
3. Selecione a guia **Armazenamento**.
4. É apresentada uma lista de volumes de armazenamento aos quais esse host específico tem acesso, todos agrupados por tipo de armazenamento (bloco, arquivo, outro). Selecione o respectivo menu **Ação** próximo ao volume do qual você deseja revogar o acesso e clique em **Revogar acesso**.
5. Confirme se você deseja revogar o acesso para um volume porque a ação não pode ser desfeita. Clique em **Sim** para revogar o acesso ao volume ou em **Não** para cancelar a ação.

**Nota** - Se você desejar desconectar múltiplos volumes de um host específico, será necessário repetir a ação Revogar acesso para cada volume.

 

### Revogando o acesso por meio da Visualização de armazenamento

1. Clique em **Armazenamento, {{site.data.keyword.filestorage_short}}** e selecione o **Volume** do qual você deseja revogar o acesso.
2. Role para a seção **Hosts autorizados** da página.
3. Clique em **Ações** ao lado do host cujo acesso deve ser revogado e selecione **Revogar acesso**.
4. Confirme se você deseja revogar o acesso para um volume porque a ação não pode ser desfeita. Clique em **Sim** para revogar o acesso ao volume ou em **Não** para cancelar a ação.

**Nota** - Se você desejar desconectar múltiplos hosts de um volume específico, será necessário repetir a ação Revogar acesso para cada host.
 

## Cancelando um volume de armazenamento

Se você não precisar mais de um volume específico, será possível cancelá-lo. Para cancelar um volume de armazenamento, é necessário revogar o acesso de qualquer host primeiro.

1. Clique em **Armazenamento**>**{{site.data.keyword.filestorage_short}}**.
2. Clique em **Ações** para o volume a ser cancelado e selecione **Cancelar{{site.data.keyword.filestorage_short}}**.
3. Confirme se deseja cancelar o volume imediatamente ou na data de aniversário anual de quando o volume foi provisionado.
   >**Nota** - Se você selecionar a opção para cancelar o volume em sua data de aniversário, será possível anular a solicitação de cancelamento antes de sua data de aniversário.
4. Clique em **Continuar** ou **Fechar**. 
5. Clique na caixa de seleção de confirmação e depois em **Confirmar**.
