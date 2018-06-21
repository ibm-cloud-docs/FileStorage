---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Gerenciando capturas instantâneas

## Como criar um planejamento de captura instantânea?

É possível decidir com que frequência e quando você deseja criar uma referência de momento de seu volume de armazenamento criando planejamentos de captura instantânea. É possível ter um máximo de 50 capturas
instantâneas por volume de armazenamento. Os planejamentos são gerenciados por meio da guia **Armazenamento** > **{{site.data.keyword.filestorage_short}}** do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

Para poder configurar seu planejamento inicial, deve-se primeiramente comprar um espaço de captura instantânea, caso você não tenha comprado durante o fornecimento inicial do volume de armazenamento.

### Incluir um planejamento de captura instantânea

Os planejamentos de capturas instantâneas podem ser configurados em intervalos, como por hora,
diários e semanais, cada um com um ciclo de retenção diferente. Há um máximo de 50 capturas instantâneas planejadas, podendo ser uma combinação de planejamentos por hora, diários e semanais, além de capturas instantâneas manuais por volume de armazenamento.

1. Clique em seu volume de armazenamento, clique na caixa suspensa **Ações** e clique em **Planejar captura instantânea**.
2. Na caixa de diálogo **Novo planejamento de captura instantânea**, há três frequências de captura instantânea diferentes para seleção. É possível usar qualquer combinação das três para criar um planejamento de captura instantânea abrangente.
   - Hora em Hora
      - Especifique o minuto de cada hora em que uma captura instantânea deve ser tirada; o padrão é o minuto atual.
      - Especifique o número de capturas instantâneas por hora a serem retidas antes de
descartar a mais antiga.
   - Diário
      - Especifique a hora e o minuto em que uma captura instantânea deve ser tirada; o padrão é a hora e o minuto atuais.
      - Selecione o número de capturas instantâneas por hora a serem retidas antes de
descartar a mais antiga.
   - Semanal
      - Especifique o dia da semana, a hora e o minuto em que uma captura instantânea deve ser tirada; o padrão é o dia, a hora e o minuto atuais.
      - Selecione o número de capturas instantâneas semanais a serem retidas antes de descartar a
mais antiga.
3. Clique em **Salvar** e crie outro planejamento com uma frequência diferente. </br> **Nota**: se o número total de Capturas instantâneas planejadas for mais de 50, você receberá uma mensagem de aviso e não conseguirá salvar o planejamento.

Uma lista das capturas instantâneas será exibida conforme forem tiradas na seção **Capturas instantâneas** da página **Detalhe**.

## Como tirar uma captura instantânea manual?

Capturas instantâneas manuais podem ser obtidas em vários pontos durante um upgrade ou
manutenção do aplicativo. Também é possível tirar capturas instantâneas em múltiplos servidores que tenham sido desativados provisoriamente no nível do aplicativo.

Há um máximo de 50 capturas instantâneas manuais por volume de armazenamento.

1. Clique em seu volume de armazenamento.
2. Clique na caixa suspensa Ações.
3. Clique em **Tomar captura instantânea manual**.

A captura instantânea é tirada imediatamente e exibida na seção Capturas instantâneas da página **Detalhe**. Seu planejamento aparece como Manual.

## Como ver uma lista de Capturas instantâneas com espaço usado e Funções de gerenciamento?

Uma lista de capturas instantâneas retidas e de espaço usado pode ser vista na página **Detalhe** (**Armazenamento** > **{{site.data.keyword.filestorage_short}}**). As funções de gerenciamento (editar planejamentos e incluir mais espaço) são conduzidas na página **Detalhe** usando o menu suspenso **Ações** ou links na várias seções na página.

## Como visualizar uma lista de Capturas instantâneas retidas?

As capturas instantâneas retidas são baseadas no número que você inseriu no campo **Manter o último** ao configurar seus planejamentos. É possível visualizar as capturas instantâneas que foram tomadas sob a seção **Captura instantânea**. As capturas instantâneas são listadas por planejamento.

## Como ver quanto espaço de Captura instantânea foi usado?

O gráfico de pizza na parte superior da página **Detalhes** exibe quanto espaço foi usado e quanto espaço resta. Você receberá notificações quando atingir os limites de espaço de 75 por cento, 90 por cento e 95 por cento.

## Como mudar a quantia de espaço de captura instantânea para meu volume?

Talvez seja necessário incluir espaço de captura instantânea em um volume que não tinha nenhum anteriormente ou que pode requerer mais espaço de captura instantânea. É possível incluir entre 5 GB e 4.000 GB, dependendo de suas necessidades.
**Nota**: o espaço de captura instantânea pode ser somente aumentado, não reduzido. É possível selecionar uma quantia menor de espaço até determinar a quantia de espaço que você realmente
precisa. Lembre-se, as capturas instantâneas automatizadas e manuais compartilham o mesmo espaço.

O espaço de captura instantânea é mudado por meio de **Armazenamento** > **{{site.data.keyword.filestorage_short}}**.

1. Clique nos volumes de armazenamento, clique na caixa suspensa **Ações** e clique em **Incluir mais espaço de captura instantânea**.
2. Selecione em um intervalo de tamanhos no prompt. Os tamanhos geralmente variam de 0 até o tamanho
de seu volume.
3. Clique em **Continuar** para provisionar o espaço adicional.
4. Insira qualquer Código promocional que você tiver e clique em **Recalcular**. **Encargos para esta ordem** e **Revisão de ordem** mostram valores padrão.
5. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal…**
e clique em **Fazer pedido**. Seu espaço de captura instantânea adicional será provisionado em alguns minutos.

## Como receber notificações quando estou próximo de meu limite de espaço de captura instantânea e as capturas instantâneas são excluídas?

São enviadas notificações por meio de chamados de suporte do Suporte para o Usuário principal em sua conta ao atingir três limites de espaço diferentes - 75 por cento, 90 por cento e 95 por cento.

- Capacidade de 75 por cento: é enviado um aviso de que a utilização de espaço de captura instantânea excedeu 75 por cento. Se você seguir o aviso e incluir espaço manualmente ou excluir capturas instantâneas retidas e desnecessárias, a ação será anotada e o chamado será encerrado. No caso de nenhuma ação, deve-se reconhecer manualmente o chamado e, em seguida, ele é encerrado.
- Capacidade de 90 por cento: é enviado um segundo aviso quando a utilização de espaço de captura instantânea excede 90 por cento. Assim como ao atingir a capacidade de 75 por cento, se você executar as ações necessárias para diminuir o espaço usado, a ação será observada e o chamado será encerrado. No caso de nenhuma ação, deve-se reconhecer manualmente o chamado e, em seguida, ele é encerrado.
- Capacidade de 95 por cento: um aviso final é enviado. Se nenhuma ação for executada
para trazer seu espaço abaixo do limite, uma notificação será gerada e uma exclusão automática ocorrerá para que
capturas instantâneas futuras possam ser criadas. As capturas instantâneas planejadas são excluídas, iniciando com a mais antiga, até que o uso caia abaixo de 95 por cento e continuarão sendo excluídas cada vez que a utilização exceder 95 por cento até cair abaixo do limite. Se o espaço for aumentado manualmente ou capturas instantâneas forem excluídas, o aviso será reconfigurado e emitido novamente se o limite for excedido mais uma vez. Se nenhuma ação for executada, este será o único aviso que será recebido.

## Como excluir um planejamento de Captura instantânea?

Os planejamentos de captura instantânea podem ser cancelados por meio de **Armazenamento** > **{{site.data.keyword.filestorage_short}}**.

1. No quadro **Planejamentos de captura instantânea** na página **Detalhes**, clique no planejamento a ser excluído.
2. Próximo ao planejamento a ser excluído, clique na caixa de seleção e depois em **Salvar**.<br/>
**Cuidado**: se você estiver usando o recurso de replicação, certifique-se de que o planejamento que está sendo excluído não seja o planejamento usado pela replicação. Clique
[aqui](replication.html) para obter mais informações sobre como excluir um
planejamento de replicação.

## Como excluir uma Captura instantânea?

As capturas instantâneas que não são mais necessárias podem ser removidas manualmente para liberar
espaço para capturas instantâneas futuras. A exclusão é feita por meio de **Armazenamento** > **{{site.data.keyword.filestorage_short}}**.

1. Clique no volume de armazenamento e role para baixo para a seção **Captura instantânea** para ver uma lista de capturas instantâneas existentes.
2. Clique na lista suspensa **Ações** próxima a uma captura instantânea específica e clique em **Excluir** para excluí-la. Isso não afetará nenhuma captura instantânea futura ou passada no mesmo planejamento, já que não há dependência entre as capturas instantâneas.

As capturas instantâneas manuais não excluídas da maneira descrita aqui são excluídas automaticamente (primeiramente as mais antigas) ao atingir as limitações de espaço.

## Como restaurar meu Volume de armazenamento para um momento específico usando uma Captura instantânea?

Você pode precisar tomar seu volume de armazenamento de volta para um momento específico devido a um erro do usuário ou distorção de dados.

1. Desmonte e separe seu volume de armazenamento do host.
   - Clique [aqui](accessing-file-storage-linux.html) para obter instruções do {{site.data.keyword.filestorage_short}} no Linux.
2. Clique em **Armazenamento**, **{{site.data.keyword.filestorage_short}}**, no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
3. Role para baixo e clique no seu volume a ser restaurado. A seção **Capturas instantâneas** da página **Detalhe** exibe uma lista de todas as capturas instantâneas salvas junto a seu tamanho e data de criação.
4. Clique em **Ações** da captura instantânea a ser usada e clique em **Restaurar**. 

   **Nota**: executar uma restauração resultará em uma perda de dados que foram criados ou modificados desde que a captura instantânea que está sendo usada foi tomada. Quando concluída, seu volume de armazenamento será retornado ao mesmo estado em que estava quando a captura instantânea foi tomada. Um prompt aparecerá para notificá-lo sobre isso.
5. Clique em **Sim** para iniciar a restauração. Você receberá uma mensagem na parte superior da página informando que o volume foi restaurado usando a captura instantânea selecionada. Além disso, um ícone aparecerá próximo ao seu volume no {{site.data.keyword.filestorage_short}} indicando que uma transação ativa está em andamento. Passar o mouse sobre o ícone produz um diálogo indicando a transação. O ícone desaparece quando a transação está concluída.
6. Monte e reconecte seu volume de armazenamento ao host.
   - Clique [aqui](accessing-file-storage-linux.html) para obter instruções do {{site.data.keyword.filestorage_short}} no Linux.
   
**Nota**: a restauração de um volume resultará na exclusão de todas as capturas instantâneas anteriores à captura instantânea restaurada.
