---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Gerenciando capturas instantâneas

## Como criar um planejamento de captura instantânea?

Os planejamentos de captura instantânea permitem decidir com que frequência e quando você deseja criar uma referência de momento de seu volume de armazenamento. É possível ter um máximo de 50 capturas instantâneas por volume de armazenamento. Os planejamentos são gerenciados por meio da guia **Armazenamento** > **{{site.data.keyword.filestorage_short}}** do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

Antes de poder configurar seu planejamento inicial, deve-se primeiro comprar o espaço de captura instantânea se você não o comprou durante o fornecimento inicial do volume de armazenamento.

### Incluir um planejamento de captura instantânea

Os planejamentos de capturas instantâneas podem ser configurados para intervalos por hora, diários ou semanais, cada um com um ciclo de retenção distinto. Há um máximo de 50 capturas instantâneas planejadas, que podem ser uma combinação de planejamentos por hora, diários e semanais, e capturas instantâneas manuais por volume de armazenamento.

1. Clique em seu volume de armazenamento, clique na caixa suspensa **Ações** e clique em **Planejar captura instantânea**.
2. No diálogo Nova captura instantânea de planejamento, há três frequências diferentes de captura instantânea dentre as quais selecionar. Use qualquer combinação das três para criar um planejamento de captura instantânea abrangente.
   - Por hora
      - Especifique o minuto de cada hora em que uma captura instantânea deve ser executada; o padrão é o minuto atual.
      - Especifique o número de capturas instantâneas por hora a serem retidas antes de descartar a mais antiga.
   - Diário
      - Especifique a hora e o minuto que uma captura instantânea deve ser executada; o padrão é a hora e o minuto atuais.
      - Selecione o número de capturas instantâneas por hora a serem retidas antes de descartar a mais antiga.
   - Semanal
      - Especifique o dia da semana, a hora e o minuto que uma captura instantânea deve ser executada; o padrão é o dia, a hora e o minuto atuais.
      - Selecione o número de capturas instantâneas semanais a serem retidas antes de descartar a mais antiga.
3. Clique em **Salvar** e crie outro planejamento com uma frequência diferente. Observe que você receberá uma mensagem de aviso e não será possível salvar se o número total de capturas instantâneas planejadas for acima de 50.

Uma lista das capturas instantâneas será exibida conforme elas forem tomadas na seção Capturas instantâneas da página Detalhes.

## Como tomar uma captura instantânea manual?

As capturas instantâneas manuais podem ser tomadas em vários pontos durante um upgrade ou manutenção do aplicativo. Elas também permitem tomar capturas instantâneas em múltiplas máquinas que foram temporariamente desativadas no nível do aplicativo.

Há um máximo de 50 capturas instantâneas manuais por volume de armazenamento.

1. Clique em seu volume de armazenamento.
2. Clique na caixa suspensa Ações.
3. Clique em **Tomar captura instantânea manual**.
A captura instantânea será tomada e será exibida na seção Capturas instantâneas da página Detalhes. Seu planejamento será Manual.

## Como ver uma lista de capturas instantâneas com espaço consumido e funções de gerenciamento?

Uma lista de capturas instantâneas retidas e o espaço consumido podem ser vistos na página Detalhes (**Armazenamento** > **{{site.data.keyword.filestorage_short}}**). As funções de gerenciamento (editar planejamentos e incluir mais espaço) são conduzidas na página **Detalhe** usando o menu suspenso **Ações** ou links na várias seções na página.

## Como visualizar uma lista de capturas instantâneas retidas?

As capturas instantâneas retidas são baseadas no número que você inseriu no campo **Manter o último** ao configurar seus planejamentos. É possível visualizar as capturas instantâneas que foram tomadas sob a seção **Captura instantânea**. As capturas instantâneas são listadas por planejamento.

## Como ver quanto espaço de captura instantânea foi usado?

O gráfico de pizza na parte superior da página **Detalhes** exibe quanto espaço foi usado e quanto espaço resta. Você receberá notificações quando começar a atingir os limites de espaço, 75%, 90% e 95%.

## Como mudar a quantia de espaço de captura instantânea para meu volume?

Você pode precisar incluir espaço de captura instantânea para um volume que não tinha nenhum anteriormente ou pode requerer espaço de captura instantânea adicional. É possível incluir entre 5 GB e 4.000 GB dependendo de suas necessidades. Nota: o espaço de captura instantânea pode somente ser aumentado e não reduzido. Você pode desejar selecionar uma quantia menor de espaço até determinar quanto espaço é realmente necessário. Lembre-se, as capturas instantâneas automatizadas e manuais compartilham o mesmo espaço.

O espaço de captura instantânea é mudado por meio de **Armazenamento** > **{{site.data.keyword.filestorage_short}}**.

1. Clique em seus volumes de armazenamento, clique na caixa suspensa **Ações** e clique em **Incluir mais espaço de captura instantânea**.
2. Selecione de um intervalo de tamanhos do prompt. Os tamanhos geralmente variam de 0 ao tamanho de seu volume.
3. Clique em **Continuar** para provisionar o espaço adicional.
4. Insira qualquer Código promocional que você tiver e clique em **Recalcular**. **Encargos para esta ordem** e **Revisão de ordem** mostram valores padrão.
5. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal…** e clique em **Fazer pedido**. O seu espaço de captura instantânea adicional será provisionado em alguns minutos.

## Como eu recebo notificações quando estou próximo a meu limite de espaço de captura instantânea e capturas instantâneas são excluídas?

As notificações são enviadas por meio de chamados de suporte para o Usuário principal em sua conta quando você atinge três limites de espaço diferentes, 75%, 90% e 95%.

- 75% de capacidade: um aviso é enviado de que a utilização de espaço de captura instantânea excedeu 75%. Se você considerar o aviso e incluir espaço manualmente ou excluir capturas instantâneas retidas e desnecessárias, a ação será anotada e o chamado será encerrado. Se você não fizer nada, o chamado deverá ser reconhecido manualmente e, em seguida, será encerrado.
- 90% de capacidade: um segundo aviso é enviado quando a utilização de espaço de captura instantânea excedeu 90%. Assim como ocorre se atingir 75% da capacidade, se você tomar as ações necessárias para diminuir o espaço usado, a ação será anotada e o chamado será encerrado. Se você não fizer nada, o chamado deverá ser reconhecido manualmente e, em seguida, será encerrado.
- 95% de capacidade: um aviso final é enviado. Se nenhuma ação for tomada para trazer seu espaço abaixo do limite, uma notificação será gerada e a exclusão automática ocorrerá para que capturas instantâneas futuras possam ser criadas. As capturas instantâneas planejadas são excluídas, iniciando com a mais antiga, até o uso cair abaixo de 95%, e continuarão a ser excluídas cada vez que a utilização exceder 95% até cair abaixo do limite. Se o espaço for aumentado manualmente ou as capturas instantâneas forem excluídas, o aviso será reconfigurado e emitido novamente se o limite for excedido novamente. Se nenhuma ação for tomada, este será o único aviso recebido.

## Como excluir um planejamento de captura instantânea?

Os planejamentos de captura instantânea podem ser cancelados por meio de **Armazenamento** > **{{site.data.keyword.filestorage_short}}**.

1. No quadro **Planejamentos de captura instantânea** na
página **Detalhes**, clique no planejamento a ser excluído.
2. Ao lado do planejamento a ser excluído, clique na caixa de seleção e depois em
**Salvar**.<br/>
**Cuidado**: se você estiver usando o recurso de replicação, certifique-se de que o
planejamento que você está excluindo não seja o planejamento que é usado pela replicação. Clique [aqui](replication.html) para obter informações adicionais sobre como excluir um planejamento de replicação.

## Como excluir uma captura instantânea?

As capturas instantâneas que não são mais necessárias podem ser removidas manualmente para liberar espaço para capturas instantâneas futuras. A exclusão é feita por meio de **Armazenamento** > **{{site.data.keyword.filestorage_short}}**.

1. Clique em seu volume de armazenamento e role para baixo até a seção Captura instantânea para ver uma lista de capturas instantâneas existentes.
2. Clique na lista suspensa Ações próxima a uma captura instantânea específica e clique em **Excluir** para excluir a captura instantânea. Isso não afetará nenhuma captura instantânea futura ou passada no mesmo planejamento, pois não há dependência entre as capturas instantâneas.

As capturas instantâneas manuais que não são excluídas da maneira descrita acima são excluídas automaticamente (mais antigas primeiro) quando você atinge as limitações de espaço.

## Como restaurar meu volume de armazenamento para um momento específico usando uma captura instantânea?

Você pode precisar tomar seu volume de armazenamento de volta para um momento específico devido a um erro do usuário ou distorção de dados.

1. Desmonte e desconecte seu volume de armazenamento do host.
   - Clique [aqui](accessing-file-storage-linux.html) para obter instruções do {{site.data.keyword.filestorage_short}} no Linux.
2. Clique em **Armazenamento**, **{{site.data.keyword.filestorage_short}}**, no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
3. Role para baixo e clique em seu volume a ser restaurado. A seção Capturas instantâneas da página Detalhe exibirá uma lista de todas as capturas instantâneas salvas junto com o seu tamanho e data de criação.
4. Clique no botão **Ações** para a captura instantânea a ser usada e clique em **Restaurar**. 

   **Nota**: executar uma restauração resultará em uma perda de dados que foram criados ou modificados desde que a captura instantânea que está sendo usada foi tomada. Depois de concluído, seu volume de armazenamento será retornado ao mesmo estado em que estava quando a captura instantânea foi tomada. Um prompt aparecerá para notificá-lo sobre isso.
5. Clique em **Sim** para iniciar a restauração. Você receberá uma mensagem na parte superior da página informando que o volume foi restaurado usando a captura instantânea selecionada. Além disso, um ícone aparecerá próximo ao seu volume no {{site.data.keyword.filestorage_short}} indicando que uma transação ativa está em andamento. Passar o mouse sobre o ícone produz um diálogo indicando a transação. O ícone desaparecerá quando a transação estiver completa.
6. Monte e reconecte o seu volume de armazenamento ao host.
   - Clique [aqui](accessing-file-storage-linux.html) para obter instruções do {{site.data.keyword.filestorage_short}} no Linux.
   
**Nota**: restaurar um volume resultará na exclusão de todas as capturas instantâneas antes da captura instantânea restaurada.
