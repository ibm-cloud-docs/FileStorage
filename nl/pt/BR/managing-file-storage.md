---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Gerenciando o {{site.data.keyword.filestorage_short}}

É possível gerenciar seus volumes do {{site.data.keyword.filestorage_full}} por meio do
{{site.data.keyword.slportal}}. Este artigo fornece instruções para as tarefas mais comuns.

## Como autorizar hosts para acessar o {{site.data.keyword.filestorage_short}}

Os hosts “Autorizados” são hosts que receberam direitos de acesso para um volume específico. Sem autorização do host, você não será capaz de acessar ou usar o armazenamento de seu sistema. Autorizar um host a acessar seu volume gera o Nome do usuário, Senha. 

**Nota**: é possível autorizar e conectar somente hosts que residem no mesmo data center que o seu armazenamento. Se você tiver múltiplas contas, não será possível autorizar um host de uma conta para acessar seu armazenamento em outra. 

1. Clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** e clique em seu **Nome do volume**.
2. Role para a seção **Hosts autorizados** da página.
3. Clique no link **Autorizar host** no lado direito da página. Selecione os hosts que podem acessar esse volume específico.

 

## Como visualizar a lista de hosts autorizados para um volume do {{site.data.keyword.filestorage_short}}

Use as etapas a seguir para visualizar a lista de hosts autorizados para um volume.

1. Clique em **Armazenamento > {{site.data.keyword.filestorage_short}}** e clique em seu **Nome do volume**.
2. Role para baixo até a parte inferior da página na seção **Hosts autorizados**.

Aqui você verá a lista de hosts que estão atualmente autorizados a acessar o volume.


## Como visualizar os volumes do {{site.data.keyword.filestorage_short}} para os quais um host está autorizado

É possível visualizar os volumes para os quais um host tem acesso, incluindo as informações necessárias para fazer uma conexão, Nome do volume, Tipo de armazenamento, Endereço de destino, capacidade e local:

1. Clique em **Dispositivos** > **Lista de dispositivos** do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} e clique no dispositivo apropriado.
2. Selecione a guia Armazenamento.

Em seguida, será apresentada uma lista de volumes de armazenamento à qual esse host específico tem acesso, todos agrupados por tipo de armazenamento (bloco, arquivo, outro). Nos respectivos menus **Ação**, é possível autorizar armazenamento adicional ou remover o acesso.

 

## Como montar e desmontar o {{site.data.keyword.filestorage_short}}

É possível usar as informações de ponto de montagem fornecidas na visualização **Detalhes do volume** para montar o {{site.data.keyword.filestorage_short}} de um host.

Consulte o artigo a seguir com detalhes para montar e desmontar o {{site.data.keyword.filestorage_short}} de um host: [Acessando o {{site.data.keyword.filestorage_short}} no Linux](accessing-file-storage-linux.html)

 

## Como revogar o acesso de um host para o {{site.data.keyword.filestorage_short}}

Se você deseja parar o acesso de um host para um volume de armazenamento específico, é possível revogar o acesso. Após revogar o acesso, a conexão de host será eliminada do volume e nem o sistema operacional nem os aplicativos poderão se comunicar com o volume. 

**Nota:** para evitar problemas do lado do host, desmonte o volume de armazenamento de seu sistema operacional antes de revogar o acesso para evitar a ausência de unidades ou a distorção de dados.

É possível revogar o acesso do Armazenamento da Lista de dispositivos ou das visualizações de Armazenamento.

### Como revogar o acesso da Lista de Dispositivos:

1. Clique em **Dispositivos** > **Lista de dispositivos** do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} e clique duas vezes no dispositivo apropriado.
2. Selecione a guia **Armazenamento**.
3. Em seguida, será apresentada uma lista de volumes de armazenamento à qual esse host específico tem acesso, todos agrupados por tipo de armazenamento (bloco, arquivo, outro). Selecione o respectivo menu **Ação** próximo ao volume do qual você deseja revogar o acesso e clique em **Revogar acesso**.
4. Será perguntado se você deseja revogar o acesso para um volume porque a ação não pode ser desfeita. Clique em **Sim** para revogar acesso ao volume ou em **Não** para cancelar a ação.

**Nota:** se você desejar desconectar múltiplos volumes de um host específico, precisará repetir a ação Revogar acesso para cada volume.

 

### Como revogar o acesso da visualização de armazenamento:
1. Clique em **Armazenamento, {{site.data.keyword.filestorage_short}}** e selecione o **Volume** do qual você deseja revogar o acesso.
2. Role para baixo até a seção **Hosts autorizados** da página.
3. Clique na seta suspensa **Ações** ao lado do host cujo acesso deve ser revogado e selecione **Revogar acesso**.
4. Será perguntado se você deseja revogar o acesso para um volume porque a ação não pode ser desfeita. Clique em **Sim** para revogar acesso ao volume ou em **Não** para cancelar a ação.

**Nota:** se você desejar desconectar múltiplos hosts de um volume específico, será necessário repetir a ação Revogar acesso para cada host.

 

## Como cancelar um volume de armazenamento

Se você não precisar mais de um volume específico, será possível cancelá-lo. Para cancelar um volume de armazenamento, é necessário primeiro revogar o acesso de quaisquer hosts.

1. Clique em **Armazenamento**>**{{site.data.keyword.filestorage_short}}**.
2. Clique na seta suspensa **Ações** para o volume ser cancelado e selecione **Cancelar o {{site.data.keyword.filestorage_short}}**.
3. Será solicitado que você confirme se deseja cancelar o volume imediatamente ou na data de aniversário de quando o volume foi provisionado. Clique em **Continuar** ou **Fechar**. 
**Nota**: se você selecionar a opção para cancelar o volume em sua data de aniversário, será possível anular a solicitação de cancelamento antes de sua data de aniversário.
4. Clique na caixa de seleção de confirmação e clique em **Confirmar**.

 

## Como ver os detalhes de um volume fornecido do {{site.data.keyword.filestorage_short}}

É possível visualizar um resumo das informações chave para o volume de armazenamento selecionado incluindo os recursos de captura instantânea e replicação adicionais que foram incluídos no armazenamento.

1. Clique em **Armazenamento**>**{{site.data.keyword.filestorage_short}}**.
2. Clique no **Nome do volume** apropriado na lista.
