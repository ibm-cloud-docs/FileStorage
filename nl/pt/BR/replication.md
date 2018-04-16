---
 
copyright:
  years: 2015, 2018
lastupdated: "2018-04-16"
 
---

{:shortdesc: .shortdesc} 
{:new_window: target="_blank"}

# Trabalhando com replicação

A replicação usa um de seus planejamentos de captura instantânea para copiar automaticamente capturas instantâneas para um volume de destino em um data center remoto. As cópias podem ser recuperadas no site remoto no caso de dados corrompidos ou de um evento catastrófico.

As réplicas permitem

- Recuperar de falhas do site e outros desastres rapidamente efetuando failover para o volume de destino.
- Efetuar failover para um momento específico na cópia de DR.

Antes de replicar, deve-se criar um planejamento de captura instantânea. Ao efetuar failover, você está “invertendo o comutador” de seu volume de armazenamento no data center primário para o volume de destino no data center remoto. Por exemplo, seu data center primário é Londres e seu data center secundário é Amsterdã. No caso de um evento de falha, você efetuaria failover para Amsterdã, conectando-se ao volume agora primário de uma instância de cálculo em Amsterdã. Depois que seu volume em Londres tiver sido reparado, uma captura instantânea será tirada do volume de Amsterdã para que ocorra fallback em Londres e no volume novamente primário de uma instância de cálculo em Londres.

 
**Nota:** a menos que indicado de outra forma, as etapas são as mesmas para o {{site.data.keyword.blockstorageshort}} e o {{site.data.keyword.filestorage_full}}.

## Como determinar o data center remoto para meu volume de armazenamento replicado?

Os data centers mundiais do {{site.data.keyword.BluSoftlayer_full}} foram emparelhados em combinações de primário e remoto.
Veja a Tabela 1 para obter uma lista completa de disponibilidade de data center e destinos de replicação.
Observe que algumas cidades, como Dallas, San Jose, Washington, D. C. e Amsterdã, têm múltiplos data centers.


<table cellpadding="1" cellspacing="1">
	<caption>Tabela 1</caption>
	<tbody>
		<tr>
			<td><strong>EUA 1</strong><sup><img src="/images/numberone.png" alt="1" /></sup></td>
			<td><strong>EUA 2</strong></td>
			<td><strong>América Latina/Sul</strong></td>
			<td><strong>Canadá</strong></td>
			<td><strong>Europa</strong></td>
			<td><strong>Ásia-Pacífico</strong></td>
			<td><strong>Austrália</strong></td>
		</tr>
		<tr>
			<td>DAL01<br />
				DAL05<br />
				DAL06<br />
				HOU02<br />
				SJC01<br />
				WDC01<br />
				<br />
				<br />
				<br />
			</td>
			<td>SJC03<br />
			       SJC04<br />
			       WDC04<br />
			       WDC06<br />
			       WDC07<br />
			       DAL09<br />
				DAL10<br />
				DAL12<br />
				DAL13<br />
			</td>
			<td>MEX01<br />
				SAO01<br /><br /><br /><br /><br /><br /><br /><br />
			</td>
			<td>TOR01<br />
				MON01<br /><br /><br /><br /><br /><br /><br /><br />
			</td>
			<td>
				AMS01<br />
				AMS03<br />
				FRA02<br />
				LON02<br />
				LON04<br />
				LON06<br />
				OSL01<br />
				PAR01<br />
				MIL01<br />
			</td>
			<td>HKG02<br />
				TOK02<br />
				SNG01<br />
				SEO01<br />
                                CHE01<br />
				<br />
				<br />
				<br />
				<br />
			</td>
			<td>
				SYD01<br />
				SYD04<br />
				MEL01<br />
				<br /><br /><br /><br /><br /><br />
			</td>
		</tr>
		<tr>
			<td colspan="100%"><p><sup><img src="/images/numberone.png" alt="1" /></sup>Os data centers nessas regiões ou especificamente observados dentro de uma região    NÃO têm armazenamento criptografado.<br /><strong>Nota</strong>: os data centers com armazenamento criptografado <strong>não podem</strong> iniciar a replicação com data centers não criptografados como destinos de réplica.</p>
			</td>
		</tr>
	</tbody>
</table>
 

## Como criar uma replicação inicial?

As replicações trabalham fora de um planejamento de captura instantânea. Deve-se primeiro ter um espaço de captura instantânea e um planejamento de captura instantânea configurado para o volume de origem antes de poder replicar. Você receberá prompts que permitem que saiba o espaço que precisa ser comprado ou um planejamento configurado se tentar configurar a replicação e um ou outro não estiver no local. As replicações são gerenciadas em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Clique em seu volume de armazenamento.
2. Clique na guia **Réplica** e clique no link **Comprar uma replicação**.
   Selecione um planejamento de captura instantânea existente que você deseja que suas replicações sigam. A lista conterá todos os seus planejamentos de captura instantânea ativa.
  **Nota:** será possível selecionar somente um planejamento, mesmo se você tiver uma combinação de por hora, diário ou semanal. Todas as capturas instantâneas capturadas desde o ciclo de replicação prévio serão replicadas independentemente do planejamento que as originou.
3. Clique na seta suspensa **Local** e selecione o data center que será o seu site de DR.
4. Clique em **Continuar**.
5. Insira um **Código promocional** se você tiver um e clique em **Recalcular**. Os outros campos na caixa de diálogo serão padronizados.
6. Clique na caixa de seleção **Eu li o contrato de prestação de serviços principal…** e clique em **Colocar ordem**.
 

## Como editar uma replicação existente?

É possível editar seu planejamento de replicação e mudar o seu espaço de replicação na guia **Primário** ou **Réplica** em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

 

## Como editar um planejamento de replicação?

Você está, na realidade, mudando um planejamento de captura instantânea porque seu planejamento de replicação é baseado em um planejamento de captura instantânea existente. Para mudar o planejamento de réplica, por exemplo, de Por hora para Semanal, deve-se cancelar o planejamento de replicação e configurar um novo.

A mudança do planejamento pode ser feita na guia **Primário** ou **Réplica**.

1. Clique no menu suspenso **Ações** por meio da guia **Primário** ou **Réplica**.
2. Selecione **Editar planejamento de captura instantânea**.
3. Consulte a estrutura de Captura instantânea sob Planejamento para determinar qual planejamento você está usando para replicação. Faça as mudanças no planejamento que está sendo usado para replicação; por exemplo, se o seu planejamento de replicação é **Diário**, é possível mudar a hora do dia em que a replicação deve ocorrer.
4. Clique em **Salvar**.
 

## Como mudar o espaço de replicação?

Seu espaço de captura instantânea primário e seu espaço de réplica devem ser iguais. Se você mudar o espaço na guia **Primário** ou **Réplica**, ele incluirá espaço automaticamente nos data centers de origem e de destino. Esteja ciente de que aumentar o espaço de captura instantânea acionará uma atualização de replicação imediata.

Clique no menu suspenso **Ações** por meio da guia **Primário** ou **Réplica**.
Selecione **Incluir mais espaço de captura instantânea**.
Selecione o tamanho de armazenamento na lista e clique em **Continuar**.
Insira um **Código promocional** se você tiver um e clique em **Recalcular**. Os outros campos na caixa de diálogo serão padronizados.
Clique na caixa de seleção Eu li o contrato de prestação de serviços principal… e clique no botão Colocar ordem.
 

## Como ver meus volumes de réplica na lista de volumes?

É possível visualizar seus volumes de replicação na página {{site.data.keyword.filestorage_short}} em **Armazenamento** > **{{site.data.keyword.filestorage_short}}**. O Nome do volume terá o nome do volume primário seguido por REP. O Tipo é Endurance (Performance) – Réplica, o Endereço de destino é N/D porque o volume de réplica não está montado no data center de réplica e o Status é Inativo.

 

## Como visualizar os detalhes de um volume replicado no data center de réplica?

É possível visualizar os detalhes do volume de réplica na guia **Réplica** em **Armazenamento** > **{{site.data.keyword.filestorage_short}}**. Outra opção é selecionar o volume de réplica na página **{{site.data.keyword.filestorage_short}}** e clicar na guia **Réplica**.

 

## Como especificar autorizações de host antes de efetuar failover para o data center secundário?

Os hosts e volumes autorizados devem estar no mesmo data center. Não é possível ter um volume de réplica em Londres e o host em Amsterdã; ambos devem estar em Londres ou ambos devem estar em Amsterdã.

1. Clique em seu volume de origem ou destino na página **{{site.data.keyword.filestorage_short}}**.
2. Clique na guia **Réplica**.
3. Role para baixo até o quadro **Autorizar hosts** e clique no link **Autorizar hosts** no lado direito da tela.
4. Destaque o host que deve ser autorizado para replicações. Para selecionar múltiplos hosts, use a tecla CTRL e clique nos hosts aplicáveis.
5. Clique em **Enviar**. Se você não tiver hosts, a caixa de diálogo oferecerá a opção de comprar recursos de cálculo no mesmo data center ou será possível clicar em **Fechar**.
 

## Como aumentar meu espaço de captura instantânea em meu data center de réplica ao aumentar o espaço em meu data center primário?

Seus tamanhos de volume devem ser iguais para os volumes de armazenamento primário e de réplica; um não pode ser maior que o outro. Quando você aumenta seu espaço de captura instantânea para o volume primário, o espaço de réplica é aumentado automaticamente. Esteja ciente de que aumentar o espaço de captura instantânea acionará uma atualização de replicação imediata. O aumento para ambos os volumes será mostrado como itens de linha em sua fatura e será rateado conforme necessário.

Clique [aqui](snapshots.html) para saber como aumentar seu espaço de captura instantânea.

 

## Como iniciar um failover de um volume para a sua réplica?

No caso de um evento de falha, a ação **Failover** permite iniciar um failover para seu volume alvo ou de destino. O volume de destino ficará ativo, a última captura instantânea replicada com êxito será ativada e o volume se tornará ativo para montagem. Quaisquer dados gravados no volume de origem desde o ciclo de replicação prévio serão destruídos. Esteja ciente de que quando um failover é iniciado, o **relacionamento de replicação é invertido**. Seu volume de destino é agora o volume de origem e seu antigo volume de origem se torna o destino conforme indicado pelo **Nome do LUN** seguido por **REP**.

Os failovers são iniciados em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

**Antes de continuar com esse processo, é recomendável desconectar o volume. A falha em fazer isso terminará com distorção e/ou perda de dados.**

1. Clique em seu LUN ativo (“origem”).
2. Clique na guia Réplica e clique no link Ações no canto superior direito.
3. Selecione Failover.
   Você receberá uma mensagem na parte superior da página informando que o failover está em andamento. Além disso, um ícone aparecerá próximo ao seu volume no **{{site.data.keyword.filestorage_short}}** indicando que uma transação ativa está ocorrendo. Passar o mouse sobre o ícone produz um diálogo indicando a transação. O ícone desaparecerá quando a transação estiver completa. Durante o processo de failover, as ações relacionadas à configuração são somente leitura; não é possível editar nenhum planejamento de captura instantânea, mudar o espaço de captura instantânea e assim por diante. O evento é registrado no histórico de replicação.
   Outra mensagem permitirá que você saiba quando seu volume de destino está em tempo real. O Nome do LUN de seu volume de origem original será seguido por REP e seu Status será Inativo.
4. Clique no link **Visualizar todos os {{site.data.keyword.filestorage_short}}** no canto superior direito.
5. Clique em seu LUN ativo (anteriormente seu volume de destino). Esse volume terá agora um status **Ativo**.
6. Montar e anexar seu volume de armazenamento ao host.
 

## Como iniciar um fallback de um volume para sua réplica?

Quando o volume de origem original tiver sido reparado, a ação **fallback** permitirá iniciar um fallback controlado para seu volume de origem original. Em um fallback controlado,

- O volume de origem de atuação é colocado off-line;
- Uma captura instantânea é tomada;
- O ciclo de replicação é concluído;
- A captura instantânea de dados recém-tomada é ativada;
- E o volume de origem se torna ativo para montagem.

Esteja ciente de que quando um fallback é iniciado, o **relacionamento de replicação é novamente invertido**. Seu volume de origem é restaurado como seu volume de origem e seu volume de destino é novamente o volume de destino, conforme indicado pelo **Nome do LUN** seguido por **REP**.

Os fallbacks são iniciados em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Clique em seu LUN do Endurance ativo (“destino”).
2. Clique na guia **Réplica** e clique no link **Ações** no canto superior direito.
3. Selecione **Fallback**.
   Você receberá uma mensagem na parte superior da página informando que o failover está em andamento. Além disso, um ícone aparecerá próximo ao seu volume no **{{site.data.keyword.filestorage_short}}** indicando que uma transação ativa está ocorrendo. Passar o mouse sobre o ícone produz um diálogo indicando a transação. O ícone desaparecerá quando a transação estiver completa. Durante o processo de fallback, as ações relacionadas à configuração são somente leitura; não é possível editar nenhum planejamento de captura instantânea, mudar o espaço de captura instantânea e assim por diante. O evento é registrado no histórico de replicação.
   Outra mensagem permitirá que você saiba quando seu volume de origem está em tempo real. Seu volume de destino agora terá um status Inativo.
4. Clique no link **Visualizar todos os {{site.data.keyword.filestorage_short}}** no canto superior direito.
5. Clique em seu LUN do Endurance ativo (origem). Esse volume terá agora um status **Ativo**.
6. Montar e anexar seu volume de armazenamento ao host. Clique [aqui](provisioning-file-storage.html) para obter instruções.
 

## Como ver meu histórico de replicação?

O histórico de replicação é visualizado no **Log de auditoria** por meio da guia **Conta** em **Gerenciar**. Os volumes primário e de réplica exibirão histórico de replicação idêntico, que inclui

- Tipo para replicação (failover ou fallback)
- Quando ela foi iniciada
- Captura instantânea usada para a replicação
- Tamanho da replicação
- Quando ela foi concluída
 

## Como cancelar uma replicação existente?

O cancelamento pode ser executado imediatamente ou na data de aniversário, que faz com que o faturamento seja finalizado. A replicação pode ser cancelada nas guias **Primário** ou **Réplica**.

1. Clique no volume na página **{{site.data.keyword.filestorage_short}}**.
2. Clique na lista suspensa **Ações** na guia **Primário** ou **Réplica**.
3. Selecione **Cancelar réplica**.
4. Selecione quando cancelar - **Imediatamente** ou **Data de aniversário** e clique em **Continuar**.
5. Clique na caixa de seleção **Eu reconheço que devido ao cancelamento, perda de dados pode ocorrer** e clique em **Cancelar réplica**.
 

## Como cancelar a replicação quando o volume primário é cancelado?

Quando um volume primário é cancelado, o planejamento de replicação e o volume no data center de réplica são excluídos. As réplicas são canceladas na página **{{site.data.keyword.filestorage_short}}**.

 1. Destaque seu volume na página **{{site.data.keyword.filestorage_short}}**.
 2. Clique no menu suspenso **Ações** e selecione **Cancelar para o {{site.data.keyword.filestorage_short}}**.
 3. Selecione quando cancelar o volume - **Imediatamente** ou **Data de aniversário** e clique em **Continuar**.
 4. Clique na caixa de seleção **Eu reconheço que devido ao cancelamento, perda de dados pode ocorrer*um* e clique em **Cancelar**.
