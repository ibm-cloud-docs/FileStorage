---

copyright:
  years: 2014, 2017
lastupdated: "2017-12-11"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Trabalhando com capturas instantâneas

As capturas instantâneas são um recurso do {{site.data.keyword.filestorage_full}}. Uma captura instantânea representa o conteúdo de um volume em um determinado momento. As capturas instantâneas permitem que você proteja os seus dados sem impacto no desempenho, com consumo mínimo de espaço e são consideradas a sua primeira linha de defesa para proteção de dados. Os dados podem ser restaurados de forma fácil e rápida por meio de uma cópia de captura instantânea se um usuário acidentalmente modifica ou exclui dados cruciais de um volume com o recurso de captura instantânea.

O {{site.data.keyword.filestorage_short}} fornece duas maneiras de tomar suas capturas instantâneas, por meio de um planejamento de captura instantânea configurável que cria e exclui cópias de captura instantânea automaticamente para cada volume de armazenamento. Também é possível criar planejamentos de captura instantânea adicional, excluir cópias manualmente e gerenciar planejamentos com base em seus requisitos. A segunda maneira é tomar uma captura instantânea manual.

Uma cópia de captura instantânea é uma imagem somente leitura de um volume do {{site.data.keyword.filestorage_short}}, que captura o estado do volume em um momento. As cópias de captura instantânea são extremamente eficientes no tempo necessário para criá-las e no espaço de armazenamento. Uma cópia de captura instantânea do {{site.data.keyword.filestorage_short}} leva somente alguns segundos para ser criada, normalmente menos de 1 segundo, independentemente do tamanho do volume ou do nível de atividade no armazenamento. Após uma cópia de captura instantânea ter sido criada, as mudanças nos objetos de dados são refletidas em atualizações para a versão atual dos objetos, como se as cópias de Captura instantânea não existiam. Enquanto isso, a cópia dos dados permanece completamente estável. 

Uma cópia de Captura instantânea incorre em nenhuma sobrecarga de desempenho; os usuários podem armazenar facilmente até 50 capturas instantâneas planejadas e 50 capturas instantâneas manuais por volume do {{site.data.keyword.blockstorageshort}}, todas as quais são acessíveis como versões somente leitura e on-line dos dados.

As capturas instantâneas permitem que os usuários

- Criem pontos de recuperação point-in-time sem interrupção
- Revertam volumes para momentos prévios
- Deve-se comprar uma quantia de espaço de captura instantânea para seu volume para tomar capturas instantâneas disso. O espaço de captura instantânea pode ser incluído durante a criação de ordem de volume inicial ou após o fornecimento inicial por meio da página Detalhes do volume e clicando no botão suspenso Ações e selecionando Incluir espaço de captura instantânea. Esteja ciente de que as capturas instantâneas planejadas e manuais compartilham o espaço de captura instantânea, então peça seu espaço de acordo.

## Melhores práticas de captura instantânea
O design de captura instantânea depende do ambiente do cliente. As considerações de design a seguir ajudarão você a planejar e implementar as cópias de Captura instantânea: 
- 	Até 50 capturas instantâneas podem ser criadas por meio de planejamento e até 50 manualmente em cada volume ou LUN. 
- 	Não tire capturas instantâneas em excesso. Certifique-se de que sua frequência de captura instantânea planejada atenda suas necessidades de RTO e RPO, bem como suas necessidades de negócios de aplicativo, planejando capturas instantâneas por hora, diárias ou semanais. 
- 	A exclusão automática de captura instantânea deve ser usada para controlar o crescimento do consumo de armazenamento. <br/>
    **Nota**: o limite de Exclusão automática é fixado em 95%.
    
## Como as cópias de captura instantânea consomem espaço em disco?
As cópias de captura instantânea minimizam o consumo de disco preservando blocos individuais em vez de arquivos inteiros. As cópias de captura instantânea começam a consumir espaço extra somente quando os arquivos no sistema de arquivos ativo são mudados ou excluídos. Quando isso acontece, os blocos de arquivos originais ainda são preservados como parte de uma ou mais cópias de Captura instantânea.
No sistema de arquivos ativo, os blocos mudados são regravados para locais diferentes no disco ou removidos como blocos de arquivos ativos inteiramente. Como resultado, além do espaço em disco usado por blocos no sistema de arquivos ativo modificado, o espaço em disco usado pelos blocos originais ainda está reservado para refletir o status do sistema de arquivos ativo antes da mudança.

<table>
    <colgroup>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
    </colgroup>
    <tbody>
      <tr>
        <th colspan="3" style="border: 0.0px;text-align: center;">Uso de espaço em disco antes e depois da cópia de captura instantânea</th>
     </tr><tr>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle1.png" alt="Antes da cópia de captura instantânea"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle3.png" alt="Depois da cópia de captura instantânea"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle2.png" alt="Mudanças após a cópia de captura instantânea"></td>
     </tr><tr>
        <td style="border: 0.0px;">Antes de qualquer cópia de Captura instantânea ser criada, o espaço em disco é consumido somente pelo sistema de arquivos ativo.</td>
        <td style="border: 0.0px;">Depois que uma cópia de Captura instantânea é criada, o sistema de arquivos ativo e a cópia de Captura instantânea apontam para os mesmos blocos de disco. A cópia de Captura instantânea não usa espaço em disco extra.</td>
        <td style="border: 0.0px;">Após <i>myfile.txt</i> ser excluído do sistema de arquivos ativo, a cópia de Captura instantânea ainda inclui o arquivo e referencia seus blocos de disco. É por isso que a exclusão de dados do sistema de arquivos ativo nem sempre libera espaço em disco.</td>
      </tr>
    </tbody>
</table>



## Como comprar espaço de captura instantânea?

Para criar capturas instantâneas de seu volume de armazenamento, automatizadas ou manualmente, você precisa comprar espaço para retê-las. É possível comprar capacidade até sua quantia de volume de armazenamento (durante a compra de volume inicial ou posteriormente usando as etapas abaixo).

1. Acesse seu armazenamento por meio da guia **Armazenamento** > **{{site.data.keyword.filestorage_short}}** do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}
2. Clique no link Incluir espaço de captura instantânea no quadro Capturas instantâneas. Clique no link **Comprar espaço de captura instantânea agora** no quadro Capturas instantâneas.
3. Selecione a quantia de espaço que você precisa clicando no botão de opções próximo à quantia apropriada.
4. Clique em **Continuar**.
5. Insira qualquer Código promocional que você tiver e clique em **Recalcular**. Os Encargos para esta ordem e a Revisão de ordem terão valores padrão.
6. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal…** e clique em **Fazer pedido**. Seu espaço de captura instantânea será provisionado em alguns minutos.

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

1. Clique no planejamento a ser excluído no quadro **Planejamentos de captura instantânea** na página **Detalhes**.
2. Clique na caixa de seleção ao lado do planejamento a ser excluído e clique em **Salvar**.<br/>
Cuidado: se estiver usando o recurso de replicação, certifique-se de que o planejamento que você está excluindo não seja o planejamento usado pela replicação. Clique [aqui](replication.html) para obter informações adicionais sobre como excluir um planejamento de replicação.

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

