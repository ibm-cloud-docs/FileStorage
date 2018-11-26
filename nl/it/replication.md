---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-31"

---

{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}


# Replica dei dati

La replica usa una delle tue pianificazioni delle istantanee per copiare automaticamente le istantanee su un volume di destinazione in un data center remoto. Le copie possono essere ripristinate nel sito remoto nel caso si verifichi un evento catastrofico o un danneggiamento dei dati.

Con le repliche, puoi eseguire il ripristino rapidamente per via di errori del sito e di emergenze. In caso di emergenza, puoi eseguire il failover al volume di destinazione e accedere ai tuoi dati da un preciso momento nella copia DR. Per ulteriori informazioni, consulta [Duplicazione dei volumi di replica per il ripristino d'emergenza](disaster-recovery.html). 

La replica mantiene i tuoi dati sincronizzati in due diverse ubicazioni. Se vuoi soltanto clonare il tuo volume e utilizzarlo indipendentemente dal volume originale, consulta [Creazione di un volume di file duplicato](how-to-create-duplicate-volume.html).
{:tip}

Prima di poter eseguire la replica, devi creare una pianificazione delle istantanee. Quando esegui il failover, stai passando dal tuo volume di archiviazione nel tuo data center primario al volume di destinazione nel tuo data center remoto. Ad esempio, il tuo data center primario si trova a Londra e il tuo data center secondario si trova ad Amsterdam. Se si verifica un evento di malfunzionamento, eseguirai il failover ad Amsterdam, stabilendo una connessione al volume che ora è quello primario da un'istanza di calcolo in Amsterdam. Dopo che il tuo volume a Londra è stato riparato, verrà acquisita un'istantanea del volume che si trova ad Amsterdam per eseguire il fallback a Londra e al volume che ora è nuovamente quello primario da un'istanza di elaborazione a Londra.


## Determinazione del data center remoto per il volume di archiviazione replicato

I data center in tutto il mondo di {{site.data.keyword.BluSoftlayer_full}} sono accoppiati in combinazioni di primario e secondario.
Vedi la Tabella 1 per l'elenco completo della disponibilità dei data center e delle destinazioni di replica.

<table>
  <caption style="text-align: left;"><p>Tabella 1 - Questa tabella mostra l'elenco completo dei data center con funzionalità avanzate in ogni regione. Ogni regione è una colonna separata. Alcune città, come Dallas, San Jose, Washington DC, Amsterdam, Francoforte, Londra e Sydney hanno più data center.</p>
  <p>&#42; I data center nella regione US 1 NON dispongono dell'archiviazione avanzata. Gli host nei data center con le funzionalità di archiviazione avanzata <strong>non possono</strong> avviare la replica con destinazioni della replica nei data center US 1.</p>
  </caption>
  <thead>
    <tr>
      <th>US 1 &#42;</th>
      <th>US 2</th>
      <th>America latina</th>
      <th>Canada</th>
      <th>Europa</th>
      <th>Asia-Pacifico</th>
      <th>Australia</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>DAL01<br />
          DAL05<br />
	  DAL06<br />
	  HOU02<br />
	  SJC01<br />
	  WDC01<br />
	  <br /><br /><br /><br /><br /><br />
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
	  <br /><br /><br />
      </td>
      <td>MEX01<br />
	  SAO01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
      <td>TOR01<br />
          MON01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
      <td>AMS01<br />
	  AMS03<br />
	  FRA02<br />
	  FRA04<br />
	  FRA05<br />
	  LON02<br />
	  LON04<br />
	  LON05<br />
	  LON06<br />
	  OSL01<br />
	  PAR01<br />
	  MIL01<br />
      </td>
      <td>HKG02<br />
          TOK02<br />
	  TOK04<br />
	  TOK05<br />
	  SNG01<br />
	  SEO01<br />
          CHE01<br />
	  <br /><br /><br /><br /><br />
      </td>
      <td>SYD01<br />
          SYD04<br />
	  MEL01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
    </tr>
  </tbody>
</table>

## Creazione della replica iniziale

Le repliche funzionano in base a una pianificazione delle istantanee. Prima di poter eseguire la replica, devi già disporre dello spazio per le istantanee e di una pianificazione delle istantanee per il volume di origine. Se provi a configurare una replica e non disponi di uno di questi due elementi, ti verrà richiesto di acquistare ulteriore spazio o di configurare una pianificazione. Le repliche sono gestite in **Storage** > **{{site.data.keyword.filestorage_short}}** nel [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} .

1. Fai clic sul tuo volume di archiviazione.
2. Fai clic su **Replica** e fai clic su **Purchase a replication**.
3. Seleziona la pianificazione delle istantanee esistente che vuoi venga seguita dalla tua replica. L'elenco contiene tutte le pianificazioni delle istantanee attive. <br />

   Puoi selezionare solo una singola pianificazione, anche se hai una combinazione di orarie, giornaliere e settimanali. Tutte le istantanee che erano state acquisite a partire dal ciclo di replica precedente vengono replicate indipendentemente dalla pianificazione che ha dato loro origine.<br />Se non disponi di istantanee configurate, ti viene richiesto di procedere a farlo prima di poter ordinare la replica. Per ulteriori informazioni, vedi [Gestione delle istantanee](snapshots.html).
{:tip}
3. Fai clic su **Location** e seleziona il data center che è il tuo sito di ripristino di emergenza (DR, disaster recovery).
4. Fai clic su **Continue**.
5. Immetti un codice promozionale (**Promo Code**), se ne hai uno, e fai clic su **Recalculate**. Gli altri campi nella finestra vengono completati per impostazione predefinita.
6. Fai clic sulla casella di spunta **I have read the Master Service Agreement…** e fai clic su **Place Order**.


## Modifica di una replica esistente

Puoi modificare la tua pianificazione della replica e modificare il tuo spazio di replica dalla scheda **Primary** o da quella **Replica** in **Storage** > **{{site.data.keyword.filestorage_short}}** dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.


## Modifica della pianificazione della replica

La pianificazione della replica è basata su una pianificazione delle istantanee esistente. Per modificare la pianificazione della replica, ad esempio da Hourly a Weekly, devi annullare la pianificazione della replica e configurarne una nuova.

La modifica della pianificazione può essere effettuata nella scheda Primary o in quella Replica.

1. Fai clic su **Actions** nella scheda **Primary** o in quella **Replica**.
2. Seleziona **Edit Snapshot Schedule**.
3. Guarda nel frame **Snapshot** sotto **Schedule** per determinare quale pianificazione stai usando per la replica. Modifica la pianificazione che desideri. Ad esempio, se la tua pianificazione della replica è **Daily**, puoi modificare l'ora del giorno in cui deve essere eseguita la replica.
4. Fai clic su **Save**.


## Modifica dello spazio di replica

Il tuo spazio per le istantanee primario e il tuo spazio di replica devono essere uguali. Se modifichi lo spazio nella scheda **Primary** o in quella **Replica**, viene automaticamente aggiunto dello spazio sia al data center di origine sia a quello di destinazione. L'aumento dello spazio per le istantanee attiva anche un aggiornamento della replica immediato.

1. Fai clic su **Actions** nella scheda **Primary** o in quella **Replica**.
2. Seleziona **Add More Snapshot Space**.
3. Seleziona la dimensione di archiviazione dall'elenco e fai clic su **Continue**.
4. Immetti un codice promozionale (**Promo Code**), se ne hai uno, e fai clic su **Recalculate**. Gli altri campi nella finestra di dialogo vengono completati per impostazione predefinita.
5. Fai clic sulla casella di spunta **I have read the Master Service Agreement…** e fai clic su **Place Order**.


## Visualizzazione dei volumi di replica nell'elenco volumi

Puoi visualizzare i tuoi volumi di replica nella pagina {{site.data.keyword.filestorage_short}} in **Storage** > **{{site.data.keyword.filestorage_short}}**. Il nome del volume mostra il nome del volume primario seguito da REP. Il tipo (**Type**) è Endurance oppure Performance – Replica. L'indirizzo di destinazione (**Target Address**) non è disponibile (N/A) perché il volume di replica non è montato nel data center di replica e lo stato (**Status**) è inattivo (Inactive).



## Visualizzazione dei dettagli di un volume replicato nel data center di replica

Puoi visualizzare i dettagli del volume di replica nella scheda **Replica** in **Storage** > **{{site.data.keyword.filestorage_short}}**. Un'altra opzione consiste nel selezionare il volume di replica dalla pagina **{{site.data.keyword.filestorage_short}}** e nel fare clic sulla scheda **Replica**.


## Specifica delle autorizzazioni host prima del failover del server al data center secondario

Gli host autorizzati e i volumi si devono trovare nello stesso data center. Non puoi avere un volume di replica a Londra e l'host ad Amsterdam. Devono trovarsi entrambi a Londra oppure devono trovarsi entrambi ad Amsterdam.

1. Fai clic sul tuo volume di origine o di destinazione dalla pagina **{{site.data.keyword.filestorage_short}}**.
2. Fai clic su **Replica**.
3. Scorri giù al frame **Autorizza host** e fai clic su **Authorize host** sulla destra.
4. Evidenzia l'host che deve essere autorizzato per le repliche. Per selezionare più host, utilizza il tasto CTRL e fai clic sugli host applicabili.
5. Fai clic su **Submit**. Se non hai alcun host, ti viene richiesto di acquistare delle risorse di calcolo nello stesso data center.


## Aumento dello spazio per le istantanee nel data center di replica quando lo spazio per le istantanee viene aumentato nel data center primario

Le dimensioni dei tuoi volumi devono essere le stesse per i volumi di archiviazione primario e di replica. L'uno non può essere più grande dell'altro. Quando aumenti il tuo spazio per le istantanee per il tuo volume primario, lo spazio di replica viene aumentato automaticamente. L'aumento dello spazio per le istantanee attiva un aggiornamento della replica immediato. L'aumento per entrambi i volumi viene visualizzato come voci di riga nella tua fattura ed è a base proporzionale come necessario.

Per ulteriori informazioni sull'aumento dello spazio dell'istantanea, consulta [Istantanee](snapshots.html).


## Avvio di un failover da un volume alla sua replica

Se si verifica un evento di errore, puoi avviare un **failover** al tuo volume di destinazione. Il volume di destinazione diventa attivo. L'ultima istantanea replicata correttamente viene attivata e il volume viene reso disponibile per il montaggio. Tutti i dati che erano stati scritti nel volume di origine a partire dal ciclo di replica precedente vanno perduti. Quando viene avviato un failover, la relazione di replica si inverte. Il tuo volume di destinazione diventa il tuo volume di origine e il tuo precedente volume di origine diventa la tua destinazione, come indicato dal nome LUN (**LUN Name**) seguito da **REP**.

I failover vengono avviati in **Storage**, **{{site.data.keyword.filestorage_short}}** nel [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

Prima di procedere con questa procedura, disconnetti il volume. In caso contrario, si verifica un danneggiamento e una perdita di dati.
{:important}

1. Fai clic sul volume attivo (“origine”).
2. In alto a destra, fai clic su **Replica** e fai clic su **Actions**.
3. Seleziona **Failover**.

   Aspettati un messaggio che indica che il failover è in corso. Inoltre, compare un'icona accanto al tuo volume in **{{site.data.keyword.filestorage_short}}** che indica che è in corso una transazione attiva. Se passi il puntatore del mouse sull'icona, viene visualizzata una finestra che mostra la transazione. Una volta completata la transazione, l'icona scompare. Durante il processo di failover, le azioni correlate alla configurazione sono di sola lettura. Non puoi modificare le pianificazioni delle istantanee o modificare lo spazio per le istantanee. L'evento viene registrato nella cronologia replica.<br/> Quando il tuo volume di destinazione è attivo, ricevi un altro messaggio. Il nome LUN (LUN Name) del tuo volume di origine originale viene aggiornato in modo da terminare con "REP" e il suo stato (Status) diventa inattivo (Inactive).
   {:note}
4. Fai clic su **View All ({{site.data.keyword.filestorage_short}})**.
5. Fai clic sul volume attivo (precedentemente il volume di destinazione). Questo volume ora ha uno stato attivo (**Active**).
6. Monta o collega il tuo volume di archiviazione all'host. Fai clic [qui](provisioning-file-storage.html) per le istruzioni.


## Avvio di un failback da un volume alla sua replica

Dopo che il tuo volume di origine originale è stato riparato, puoi avviare un Failback controllato al tuo volume di origine originale. In un Failback controllato.

- il volume di origine operativo viene portato offline.
- Viene acquisita un'istantanea.
- Il ciclo di replica viene completato.
- L'istantanea di dati appena acquisita viene attivata.
- Il volume di origine diventa quindi attivo per il montaggio.

Quando viene avviato un Failback, la relazione di replica si inverte nuovamente. Il tuo volume di origine viene ripristinato come tuo volume di origine e il tuo volume di destinazione è nuovamente il volume di destinazione, come indicato dal nome LUN (**LUN Name**) seguito da **REP**.

I failback vengono avviati in **Storage**, **{{site.data.keyword.filestorage_short}}** nel [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Fai clic sul volume attivo ("destinazione").
2. In alto a destra, fai clic su **Replica** e fai clic su **Actions**.
3. Seleziona **Failback**.

   Aspettati un messaggio che mostra che il failover è in corso. Inoltre, compare un'icona accanto al tuo volume in **{{site.data.keyword.filestorage_short}}** che indica che è in corso una transazione attiva. Se passi il puntatore del mouse sull'icona, viene visualizzata una finestra che mostra la transazione. Una volta completata la transazione, l'icona scompare. Durante il processo di Failback, le azioni correlate alla configurazione sono di sola lettura. Non puoi modificare le pianificazioni delle istantanee o modificare lo spazio per le istantanee. L'evento viene registrato nella cronologia replica.
   {:note}
4. In alto a destra, fai clic sul link **View All {{site.data.keyword.filestorage_short}}**.
5. Fai clic sul tuo volume attivo ("origine").
6. Monta o collega il tuo volume di archiviazione all'host. Fai clic [qui](provisioning-file-storage.html) per le istruzioni.


## Visualizzazione della cronologia replica

La cronologia replica viene visualizzata in **Audit Log** nella scheda **Account** in **Manage**. Entrambi i volumi primario e di replica visualizzano una cronologia replica identica, che include

- Il tipo per la replica (failover o failback),
- Quando è stata avviata;
- L'istantanea che è stata utilizzata per la replica.
- La dimensione della replica.
- Quando è stata completata.


## Creazione di un duplicato di una replica

Puoi creare un duplicato di un {{site.data.keyword.BluSoftlayer_full}} {{site.data.keyword.filestorage_full}} esistente. Il volume duplicato eredita per impostazione predefinita le opzioni di capacità e prestazioni del LUN/volume originale e ha una copia dei dati che arriva fino al punto temporale di un'istantanea.

I duplicati possono essere creati sia dai volumi primari che da quelli di replica. Il nuovo duplicato viene creato nello stesso data center del volume originale. Se crei un duplicato da un volume di replica, il nuovo volume viene creato nello stesso data center del volume di replica.

I volumi duplicati solo accessibili da un host per la lettura/scrittura non appena viene seguito il provisioning dell'archiviazione. Tuttavia, le istantanee e le repliche sono consentite solo dopo il completamento della copia dei dati dall'originale al duplicato.

Per ulteriori informazioni, vedi [Creazione di un volume di file duplicato](how-to-create-duplicate-volume.html)


## Annullamento di una replica esistente

Puoi annullare la replica immediatamente o alla data dell'anniversario, che causa la terminazione della fatturazione. La replica può essere annullata sia dalla scheda **Primary** che da quella **Replica**.

1. Fai clic sul volume dalla pagina **{{site.data.keyword.filestorage_short}}**.
2. Fai clic su **Actions** nella scheda **Primary** o in quella **Replica**.
3. Seleziona **Cancel Replica**.
4. Seleziona quando annullare. Scegli **Immediately** oppure **Anniversary Date** e fai clic su **Continue**.
5. Fai clic su **I acknowledge that due to cancellation, data loss may occur** e fai clic su **Cancel Replica**.


## Annullamento della replica quando il volume primario viene annullato

Quando un volume primario viene annullato, la pianificazione della replica e il volume nel data center di replica vengono eliminati. Le repliche vengono annullate dalla pagina **{{site.data.keyword.filestorage_short}}**.

 1. Evidenzia il tuo volume nella pagina **{{site.data.keyword.filestorage_short}}**.
 2. Fai clic su **Actions** e seleziona **Cancel for {{site.data.keyword.filestorage_short}}**.
 3. Seleziona quando annullare il volume. Scegli **Immediately** oppure **Anniversary Date** e fai clic su **Continue**.
 4. Fai clic su **I acknowledge that due to cancellation, data loss may occur** e fai clic su **Cancel**.
