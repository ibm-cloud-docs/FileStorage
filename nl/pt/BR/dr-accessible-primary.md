---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, disaster recovery, duplicate volume, replica volume, failover, failback,

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Recuperação de desastre: failover com um volume primário acessível
{: #dr-accessible}

Se uma falha catastrófica ou desastre ocorrer no site primário e o armazenamento primário ainda estiver acessível, os clientes poderão executar as ações a seguir para acessar rapidamente seus dados no site secundário.

Antes de iniciar o failover, certifique-se de que toda a autorização de host esteja estabelecida.

Os hosts e volumes autorizados devem estar no mesmo data center. Por exemplo, não é possível ter um volume de réplica em Londres e o host em Amsterdã. Ambos devem estar em Londres ou ambos devem estar em Amsterdã.
{:note}

1. Efetue login no [Console do {{site.data.keyword.cloud}}](https://{DomainName}/catalog){: external} e clique no ícone **Menu** na parte superior esquerda. Selecione **Infraestrutura clássica**.

   Como alternativa, é possível efetuar login no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}.
1. Clique em seu volume de origem ou de destino na página **{{site.data.keyword.filestorage_short}}**.
2. Clique em  ** Réplica **.
3. Role para baixo para o quadro **Autorizar hosts** e clique em **Autorizar hosts** à direita.
4. Destaque o host que deve ser autorizado para replicações. Para selecionar múltiplos hosts, use a tecla CTRL e clique nos hosts aplicáveis.
5. Clique em **Enviar**. Se você não tiver hosts, será solicitado que compre recursos de cálculo no mesmo data center.

## Iniciando um failover de um volume em sua réplica

Se ocorrer um evento de falha, será possível iniciar um **failover** em seu volume de destino. O volume de destino torna-se ativo. A última captura instantânea replicada com êxito é ativada e o volume é disponibilizado para montagem. Todos os dados que foram gravados no volume de origem desde que o ciclo de replicação anterior foi perdido. Quando um failover é iniciado, o relacionamento de replicação é invertido. O volume de destino torna-se o volume de origem e o volume de origem antigo torna-se o destino, conforme indicado pelo **Nome do LUN** seguido por **REP**.

Os failovers são iniciados em **Armazenamento**, **{{site.data.keyword.filestorage_short}}** no [[{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}.

Antes de continuar com essas etapas, desconecte o volume. Caso não o faça, isso resultará em distorção e perda de dados.
{:important}

1. Clique em seu volume ativo ("origem").
2. Na parte superior direita, clique em **Réplica** e clique em **Ações**.
3. Selecione **Failover**.

   Espere uma mensagem indicando que o failover está em andamento. Além disso, um ícone aparece próximo ao seu volume no **{{site.data.keyword.filestorage_short}}** que indica que uma transação ativa está ocorrendo. Passar o mouse sobre o ícone produz uma janela que mostra a transação. O ícone desaparece quando a transação está concluída. Durante o processo de failover, as ações relacionadas à configuração são somente leitura. Não é possível editar qualquer planejamento de captura instantânea ou mudar o espaço de captura instantânea. O evento é registrado no histórico de replicação.<br/> Quando seu volume de destino estiver ativo, você obterá outra mensagem. O nome do LUN do volume de origem original é atualizado para terminar em "REP" e seu Status se torna Inativo.
   {:note}
4. Clique em **Visualizar todos ({{site.data.keyword.filestorage_short}})**.
5. Clique no volume ativo (anteriormente seu volume de destino). Esse volume agora tem um status **Ativo**.
6. Montar e anexar seu volume de armazenamento ao host. Clique [aqui](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole) para obter instruções.


## Iniciando um failback de um volume para sua réplica

Quando seu volume de origem original é reparado, é possível iniciar um Failback controlado para ele. Em um Failback controlado,

- O volume de origem em ação é colocado off-line,
- Uma captura instantânea é tirada,
- O ciclo de replicação é concluído,
- A captura instantânea de dados apenas tomada está ativada,
- E o volume de origem torna-se ativo para montagem.

Quando um Failback é iniciado, o relacionamento de replicação é invertido novamente. Seu volume de origem é restaurado como seu volume de origem e seu volume de destino é o volume de destino novamente, conforme indicado pelo **Nome do LUN** seguido por **REP**.

Os failbacks são iniciados em **Armazenamento**, **{{site.data.keyword.filestorage_short}}** no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}.

1. Clique no volume ativo ("destino").
2. Na parte superior direita, clique em **Réplica** e clique em **Ações**.
3. Selecione  ** Failback **.

   Espere uma mensagem mostrando que o failover está em andamento. Além disso, um ícone aparece próximo ao seu volume no **{{site.data.keyword.filestorage_short}}** que indica que uma transação ativa está ocorrendo. Passar o mouse sobre o ícone produz uma janela que mostra a transação. O ícone desaparece quando a transação está concluída. Durante o processo de Failback, as ações relacionadas à configuração são somente leitura. Não é possível editar qualquer planejamento de captura instantânea ou mudar o espaço de captura instantânea. O evento é registrado no histórico de replicação.
   {:note}
4. No canto superior direito, clique em **Visualizar todo o link do {{site.data.keyword.filestorage_short}}**.
5. Clique em seu volume ativo ("origem").
6. Montar e anexar seu volume de armazenamento ao host. Clique [aqui](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole) para obter instruções.
