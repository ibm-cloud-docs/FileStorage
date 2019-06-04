---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-11"

keywords: File Storage, file storage, NFS, replication, duplication, synchronous, replica schedule, replica space, disaster recovery

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}


# Replicando dados
{: #replication}

A replicação usa um de seus planejamentos de captura instantânea para copiar automaticamente capturas instantâneas para um volume de destino em um data center remoto. As cópias poderão ser recuperadas no site remoto se ocorrer um evento catastrófico ou se os dados forem corrompidos.

A replicação mantém seus dados em sincronização em dois locais diferentes. Se desejar clonar seu volume e usá-lo independentemente do volume original, consulte [Criando um volume de arquivo duplicado](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume).
{:tip}

Antes de replicar, deve-se criar um planejamento de captura instantânea.
{:important}


## Determinando o data center remoto para o volume de armazenamento replicado

Os data centers do {{site.data.keyword.cloud}} são emparelhados em combinações primárias e remotas no mundo todo.
Veja a Tabela 1 para a lista completa de disponibilidade de data center e destinos de replicação.

| EUA 1 | EUA 2 | América Latina | Canadá  | Europa  | Ásia-Pacífico  | Austrália  |
|-----|-----|-----|-----|-----|-----|-----|
| DAL01<br />DAL05<br />DAL06<br />HOU02<br />SJC01<br />WDC01 | SJC03<br />SJC04<br />WDC04<br />WDC06<br />WDC07<br />DAL09<br />DAL10<br />DAL12<br />DAL13 | MEX01<br />SAO01 | TOR01<br />MON01 | AMS01<br />AMS03<br />FRA02<br />FRA04<br />FRA05<br />LON02<br />LON04<br />LON05<br />LON06<br />OSL01<br />PAR01<br />MIL01 | HKG02<br />TOK02<br />TOK04<br />TOK05<br />SNG01<br />SEO01<br />CHE01 | SYD01<br />SYD04<br />SYD05<br />MEL01 |
{: caption="Tabela 1 - Esta tabela mostra a lista completa de data centers com recursos aprimorados em cada região. Cada região é uma coluna separada. Algumas cidades, como Dallas, San Jose, Washington DC, Amsterdã, Frankfurt, Londres e Sydney, têm múltiplos data centers. Os data centers na região EUA 1 NÃO têm armazenamento aprimorado. Os hosts em data centers com recursos de armazenamento aprimorados não podem iniciar a replicação com destinos de réplica em data centers EUA 1." caption-side="top"}


## Criando a réplica inicial

As replicações funcionam com base em um planejamento de captura instantânea. Deve-se primeiro ter espaço de captura instantânea e um planejamento de captura instantânea para o volume de origem antes de poder replicar. Se você tentar configurar a replicação e uma ou a outra não estiver em vigor, será solicitado que compre mais espaço ou configure um planejamento. As replicações são gerenciadas em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}.

1. Clique em seu volume de armazenamento.
2. Clique em **Réplica** e em **Comprar uma replicação**.
3. Selecione o planejamento de captura instantânea existente que você deseja que sua replicação siga. A lista contém todos os seus planejamentos de captura instantânea ativa. <br />

   É possível selecionar apenas um planejamento mesmo se você tem uma combinação de Por hora, Diário e Semanal. Todas as capturas instantâneas que foram capturadas desde o ciclo de replicação anterior são replicadas independentemente do planejamento que as originou.<br />Se você não tiver Capturas instantâneas configuradas, será solicitado que faça isso antes de poder pedir replicação. Para obter mais informações, consulte [Trabalhando com capturas instantâneas](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots).
   {:tip}
3. Clique em **Local** e selecione o data center que é seu site de DR.
4. Clique em **Continuar**.
5. Insira um **Código promocional** se você tiver um e clique em **Recalcular**. Os outros campos na janela são preenchidos por padrão.

   Os descontos são aplicados quando o pedido é processado.
   {:note}
6. Clique na caixa de seleção **Eu li o Contrato de prestação de serviços principal…** e clique em **Colocar ordem**.


## Editando uma replicação existente

É possível editar seu planejamento de replicação e mudar o espaço de replicação na guia **Primário** ou **Réplica** em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}.


## Editando o Planejamento de Replicação

O planejamento de replicação baseia-se em um planejamento de captura instantânea existente. Para mudar o planejamento de réplica de Por Hora para Diário ou Semanal ou vice-versa, deve-se cancelar o volume de réplica e configurar um novo.

No entanto, se você deseja mudar o horário do dia em que a replicação **Diária** ocorre, é possível ajustar o planejamento existente na guia Primário ou Réplica.

1. Clique em **Ações** na guia **Primário** ou **Réplica**.
2. Selecione **Editar planejamento de captura instantânea**.
3. Consulte o quadro **Captura instantânea** em **Planejamento** para determinar qual planejamento você está usando para replicação. Mude o planejamento que você deseja.
4. Clique em **Salvar**.


## Mudando o espaço de replicação

Seu espaço de captura instantânea primário e seu espaço de réplica devem ser os mesmos. Se você mudar o espaço na guia **Primário** ou **Réplica**, ele incluirá automaticamente o espaço nos data centers de origem e de destino. Aumentar o espaço de captura instantânea também aciona uma atualização de replicação imediata.

1. Clique em **Ações** na guia **Primário** ou **Réplica**.
2. Selecione **Incluir mais espaço de captura instantânea**.
3. Selecione o tamanho de armazenamento na lista e clique em **Continuar**.
4. Insira um **Código promocional** se você tiver um e clique em **Recalcular**. Os outros campos na caixa de diálogo são concluídos por padrão.
5. Clique na caixa de seleção **Eu li o contrato de prestação de serviços principal…** e clique em **Fazer pedido**.


## Aumentando o espaço de Captura instantânea no data center de réplica quando o espaço de Captura instantânea é aumentado no data center primário

Os tamanhos de volume devem ser os mesmos para os volumes de armazenamento primário e de réplica. Um não pode ser maior que o outro. Quando você aumenta seu espaço de captura instantânea para o volume primário, o espaço de réplica é aumentado automaticamente. Aumentar o espaço de captura instantânea aciona uma atualização de replicação imediata. O aumento em ambos os volumes é mostrado como itens de linha em sua fatura e é rateado conforme necessário.

Para obter mais informações sobre o aumento do espaço de captura instantânea, consulte [Capturas instantâneas](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots).
## Visualizando os volumes de réplica na Lista de volumes

É possível visualizar seus volumes de replicação na página {{site.data.keyword.filestorage_short}} em **Armazenamento** > **{{site.data.keyword.filestorage_short}}**. O nome do volume mostra o nome do volume primário seguido por REP. O **Tipo** é Endurance ou Performance - Réplica. O **Endereço de destino** é N/A porque o volume da réplica não está montado no data center da réplica e o **Status** mostra Inativo.


## Visualizando os detalhes de um volume replicado no data center de réplica

É possível visualizar os detalhes do volume de réplica na guia **Réplica** em **Armazenamento** > **{{site.data.keyword.filestorage_short}}**. Outra opção é selecionar o volume de réplica na página **{{site.data.keyword.filestorage_short}}** e clicar na guia **Réplica**.


## Visualizando o histórico de replicação

O histórico de replicação é visualizado no **Log de auditoria** na guia **Conta** em **Gerenciar**. Os dois volumes, primário e de réplica, exibem históricos de replicação idênticos, que incluem

- Tipo para replicação (failover ou failback),
- Quando ela foi iniciada
- Captura instantânea que foi usada para a replicação,
- Tamanho da replicação,
- Quando ela foi concluída.


## Criando uma duplicata de uma réplica

É possível criar uma duplicata de um {{site.data.keyword.cloud}}  {{site.data.keyword.filestorage_full}} existente. O volume duplicado herda a capacidade e as opções de desempenho do volume de armazenamento original por padrão e tem uma cópia dos dados até o momento de uma captura instantânea.

As duplicatas podem ser criadas de ambos os volumes, o primário e o de réplica. A nova duplicata é criada no mesmo data center que o volume original. Se você criar uma duplicata de um volume de réplica, o novo volume será criado no mesmo data center que o volume de réplica.

Os volumes duplicados podem ser acessados por um host para leitura/gravação assim que o armazenamento é provisionado. No entanto, capturas instantâneas e replicação não são permitidas até que a cópia de dados do original para a duplicata seja concluída.

Para obter mais informações, consulte [Criando um Volume de arquivo duplicado](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume)

## Usando réplicas para failover quando ocorre um desastre

Ao efetuar failover, você está "invertendo o comutador" do volume de armazenamento em seu data center primário para o volume de destino em seu data center remoto. Por exemplo, seu data center primário é Londres e seu data center secundário é Amsterdã. Se um evento de falha ocorresse, você efetuaria failover para Amsterdã - conectando-se ao volume agora primário de uma instância de cálculo em Amsterdã. Depois que seu volume em Londres é reparado, uma captura instantânea é tomada do volume de Amsterdã para efetuar failback para Londres e para o volume novamente primário de uma instância de cálculo em Londres.

* Se a localização primária estiver com problema, mas o armazenamento e o host ainda estiverem on-line, consulte [Failover com um volume primário acessível](/docs/infrastructure/FileStorage?topic=FileStorage-dr-accessible).
* Se a localização primária estiver inativa, consulte [Failover com um volume primário inacessível](/docs/infrastructure/FileStorage?topic=FileStorage-dr-inaccessible).

## Cancelando uma Replicação Existente

É possível cancelar a replicação imediatamente ou na data de aniversário, que faz com que o faturamento termine. A replicação pode ser cancelada nas guias **Primário** ou **Réplica**.

1. Clique no volume na página **{{site.data.keyword.filestorage_short}}**.
2. Clique em **Ações** na guia **Primário** ou **Réplica**.
3. Selecione **Cancelar réplica**.
4. Selecione quando cancelar. Escolha **Imediatamente** ou **Data de aniversário** e clique em **Continuar**.
5. Clique em **Eu reconheço que, devido ao cancelamento, a perda de dados pode ocorrer** e clique em **Cancelar réplica**.


## Cancelando a replicação quando o volume primário é cancelado

Quando um volume primário é cancelado, o planejamento de replicação e o volume no data center de réplica são excluídos. As réplicas são canceladas na página **{{site.data.keyword.filestorage_short}}**.

 1. Destaque seu volume na página **{{site.data.keyword.filestorage_short}}**.
 2. Clique em **Ações** e selecione **Cancelar para o {{site.data.keyword.filestorage_short}}**.
 3. Selecione quando cancelar o volume. Escolha **Imediatamente** ou **Data de aniversário** e clique em **Continuar**.
 4. Clique em **Eu reconheço que, devido ao cancelamento, a perda de dados pode ocorrer** e clique em **Cancelar**.

## Comandos Relacionados a Replicação no SLCLI
{: #clicommands}

* Listar data centers de replicação adequados para um volume específico.
  ```
  # slcli file replica-locations --help
  Usage: slcli file replica-locations [OPTIONS] VOLUME_ID

  Opções: --sortby TEXT Coluna para classificação --columns TEXT Colunas para exibição. Options: ID, Long Name, Short Name
  -h, --help      Show this message and exit.
  ```

* Solicite um volume de réplica de armazenamento de arquivo.
  ```
  # slcli file replica-order --help
  Usage: slcli file replica-order [OPTIONS] VOLUME_ID

  Options:
  -s, --snapshot-schedule [INTERVAL|HOURLY|DAILY|WEEKLY]
                                  Snapshot schedule to use for replication,
                                  (INTERVAL | HOURLY | DAILY | WEEKLY)
                                  [required]
  -l, --location TEXT             Short name of the data center for the
                                  replicant (e.g.: dal09)  [required]
  --tier [0.25|2|4|10]            Endurance Storage Tier (IOPS per GB) of the
                                  primary volume for which a replicant is
                                  ordered [optional]
  -h, --help                      Show this message and exit.
  ```

* Listar volumes replicantes existentes para um volume de arquivo.
  ```
  # slcli file replica-partners --help
  Usage: slcli file replica-partners [OPTIONS] VOLUME_ID

  Opções: --sortby TEXT Coluna para classificação --columns TEXT Colunas para exibição. Options: ID, Username, Account ID,
                  Capacity (GB), Hardware ID, Guest ID, Host ID
  -h, --help      Show this message and exit.
  ```

* Executar failover de um volume de arquivo em um volume replicado específico.
  ```
  # slcli file replica-failover --help
  Usage: slcli file replica-failover [OPTIONS] VOLUME_ID

  Options:
  --replicant-id TEXT  ID of the replicant volume
  --immediate          Failover to replicant immediately.
  -h, --help      Show this message and exit.
  ```

* Executar failback de um volume de arquivo por meio de um volume replicado específico.
  ```
  # slcli file replica-failback --help
  Usage: slcli file replica-failback [OPTIONS] VOLUME_ID

  Options:
  --replicant-id TEXT  ID of the replicant volume
  -h, --help           Show this message and exit.
  ```
