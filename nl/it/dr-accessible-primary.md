---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-18"

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

# Ripristino di emergenza - Failover con un volume primario accessibile
{: #dr-accessible}

Se si verifica un errore catastrofico o un'emergenza sul sito primario e l'archiviazione primaria è ancora accessibile, i clienti possono eseguire le seguenti azioni per accedere rapidamente ai loro dati sul sito secondario.

Prima di avviare il failover, assicurati che tutta l'autorizzazione di host sia implementata.

Gli host autorizzati e i volumi si devono trovare nello stesso data center. Ad esempio, non puoi avere un volume di replica a Londra e l'host ad Amsterdam. Devono trovarsi entrambi a Londra oppure devono trovarsi entrambi ad Amsterdam.
{:note}

1. Accedi alla [console {{site.data.keyword.cloud}}](https://{DomainName}/catalog){: external} e fai clic sull'icona **menu** in alto a sinistra. Seleziona **Infrastruttura classica**.
2. Fai clic sul tuo volume di origine o di destinazione dalla pagina **{{site.data.keyword.filestorage_short}}**.
3. Fai clic su **Replica**.
4. Scorri giù al frame **Autorizza host** e fai clic su **Authorize host** sulla destra.
5. Evidenzia l'host che deve essere autorizzato per le repliche. Per selezionare più host, utilizza il tasto CTRL e fai clic sugli host applicabili.
6. Fai clic su **Submit**. Se non hai alcun host, ti viene richiesto di acquistare delle risorse di calcolo nello stesso data center.

## Avvio di un failover da un volume alla sua replica

Se si verifica un evento di errore, puoi avviare un **failover** al tuo volume di destinazione. Il volume di destinazione diventa attivo. L'ultima istantanea replicata correttamente viene attivata e il volume viene reso disponibile per il montaggio. Tutti i dati che erano stati scritti nel volume di origine a partire dal ciclo di replica precedente vanno perduti. Quando viene avviato un failover, la relazione di replica si inverte. Il tuo volume di destinazione diventa il tuo volume di origine e il tuo precedente volume di origine diventa la tua destinazione, come indicato dal nome LUN (**LUN Name**) seguito da **REP**.

I failover vengono avviati in **Storage**, **{{site.data.keyword.filestorage_short}}** nella [console {{site.data.keyword.cloud}}](https://{DomainName}/classic){: external}.

Prima di procedere con questa procedura, disconnetti il volume. In caso contrario, si verifica un danneggiamento e una perdita di dati.
{:important}

1. Fai clic sul volume attivo (“origine”).
2. In alto a destra, fai clic su **Replica** e fai clic su **Actions**.
3. Seleziona **Failover**.

   Aspettati un messaggio che indica che il failover è in corso. Inoltre, compare un'icona accanto al tuo volume in **{{site.data.keyword.filestorage_short}}** che indica che è in corso una transazione attiva. Se passi il puntatore del mouse sull'icona, viene visualizzata una finestra che mostra la transazione. Una volta completata la transazione, l'icona scompare. Durante il processo di failover, le azioni correlate alla configurazione sono di sola lettura. Non puoi modificare le pianificazioni delle istantanee o modificare lo spazio per le istantanee. L'evento viene registrato nella cronologia replica.<br/> Quando il tuo volume di destinazione è attivo, ricevi un altro messaggio. Il nome LUN (LUN Name) del tuo volume di origine originale viene aggiornato in modo da terminare con "REP" e il suo stato (Status) diventa inattivo (Inactive).
   {:note}
4. Fai clic su **View All ({{site.data.keyword.filestorage_short}})**.
5. Fai clic sul volume attivo (precedentemente il volume di destinazione). Questo volume ora ha uno stato attivo (**Active**).
6. Monta o collega il tuo volume di archiviazione all'host. Per ulteriori informazioni, vedi [Connessione alla tua nuova archiviazione](/docs/infrastructure/FileStorage?topic=FileStorage-getting-started#mountingstorage).


## Avvio di un failback da un volume alla sua replica

Dopo che il tuo volume di origine originale è stato riparato, puoi avviare un Failback controllato al tuo volume di origine originale. In un Failback controllato.

- il volume di origine operativo viene portato offline.
- Viene acquisita un'istantanea.
- Il ciclo di replica viene completato.
- L'istantanea di dati appena acquisita viene attivata.
- Il volume di origine diventa quindi attivo per il montaggio.

Quando viene avviato un Failback, la relazione di replica si inverte nuovamente. Il tuo volume di origine viene ripristinato come tuo volume di origine e il tuo volume di destinazione è nuovamente il volume di destinazione, come indicato dal nome LUN (**LUN Name**) seguito da **REP**.

I failback vengono avviati in **Storage**, **{{site.data.keyword.filestorage_short}}** nella [console {{site.data.keyword.cloud}}](https://{DomainName}/classic){: external}.

1. Fai clic sul volume attivo ("destinazione").
2. In alto a destra, fai clic su **Replica** e fai clic su **Actions**.
3. Seleziona **Failback**.

   Aspettati un messaggio che mostra che il failover è in corso. Inoltre, compare un'icona accanto al tuo volume in **{{site.data.keyword.filestorage_short}}** che indica che è in corso una transazione attiva. Se passi il puntatore del mouse sull'icona, viene visualizzata una finestra che mostra la transazione. Una volta completata la transazione, l'icona scompare. Durante il processo di Failback, le azioni correlate alla configurazione sono di sola lettura. Non puoi modificare le pianificazioni delle istantanee o modificare lo spazio per le istantanee. L'evento viene registrato nella cronologia replica.
   {:note}
4. In alto a destra, fai clic sul link **View All {{site.data.keyword.filestorage_short}}**.
5. Fai clic sul tuo volume attivo ("origine").
6. Monta o collega il tuo volume di archiviazione all'host. Per ulteriori informazioni, vedi [Connessione alla tua nuova archiviazione](/docs/infrastructure/FileStorage?topic=FileStorage-getting-started#mountingstorage).
