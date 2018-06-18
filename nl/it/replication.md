---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-22"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Gestione della replica

La replica usa una delle tue pianificazioni delle istantanee per copiare automaticamente le istantanee su un volume di destinazione in un data center remoto. Le copie possono essere ripristinate nel sito remoto nel caso si verifichi un evento catastrofico o un danneggiamento dei dati. 

Con le repliche, puoi: 

- Eseguire il ripristino da malfunzionamenti del sito e altre situazioni critiche in modo rapido eseguendo il failover al volume di destinazione,
- Eseguire il failover a uno specifico punto temporale nella copia di ripristino di emergenza (DR, disaster recovery).

Prima di poter eseguire la replica, devi creare una pianificazione delle istantanee. Quando esegui il failover, stai passando dal tuo volume di archiviazione nel tuo data center primario al volume di destinazione nel tuo data center remoto. Ad esempio, il tuo data center primario si trova a Londra e il tuo data center secondario si trova ad Amsterdam. Se si verifica un evento di malfunzionamento, eseguirai il failover ad Amsterdam, stabilendo una connessione al volume che ora è quello primario da un'istanza di calcolo in Amsterdam. Dopo che il tuo volume a Londra è stato riparato, verrà acquisita un'istantanea del volume che si trova ad Amsterdam per eseguire il fallback a Londra e al volume che ora è nuovamente quello primario da un'istanza di elaborazione a Londra. 


## Come determino il data center remoto per il mio volume di archiviazione replicato? 

I data center in tutto il mondo di {{site.data.keyword.BluSoftlayer_full}} sono accoppiati in combinazioni di primario e secondario.
Vedi la Tabella 1 per l'elenco completo della disponibilità dei data center e delle destinazioni di replica. 


<table style="width: 80.0%;">
	<caption style="text-align: left;"><p>Tabella 1 - Questa tabella mostra l'elenco completo dei data center con funzionalità avanzate in ogni regione. Ogni regione è una colonna separata. Alcune città, come Dallas, San Jose, Washington DC, Amsterdam, Francoforte, Londra e Sydney hanno più data center. </p>
		<p>&#42; I data center nella regione US 1 NON dispongono dell'archiviazione avanzata. Gli host nei data center con le funzionalità di archiviazione avanzata <strong>non possono</strong> avviare la replica con destinazioni della replica nei data center US 1.</p>
</caption>
	<thead>
		<tr>
			<th>US 1 &#42;</th>
			<th>US 2</th>
			<th>America latina</th>
			<th>Canada</th>
			<th>Europa</th>
			<th>Asia-Pacifico </th>
			<th>Australia</t>
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
				<br />
				<br />
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
				<br /><br />
			</td>
			<td>MEX01<br />
				SAO01<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
			</td>
			<td>TOR01<br />
				MON01<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
			</td>
			<td>
				AMS01<br />
				AMS03<br />
				FRA02<br />
				FRA04<br />
				FRA05<br />
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
				<br />
				<br />
			</td>
			<td>
				SYD01<br />
				SYD04<br />
				MEL01<br />
				<br /><br /><br /><br /><br /><br /><br /><br />
			</td>
		</tr>
	</tbody>
</table>


## Come creo un replica iniziale?

Le repliche iniziano da una pianificazione delle istantanee. Prima di poter eseguire la replica, devi già avere configurato lo spazio per le istantanee e una pianificazione delle istantanee per il volume di origine. Riceverai dei prompt che ti informeranno delle esigenze di spazio da acquistare o di una pianificazione da configurare, se provi a configurare una replica e manca uno di questi elementi. Le repliche sono gestite in **Storage** > **{{site.data.keyword.filestorage_short}}** nel [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} .

1. Fai clic sul tuo volume di archiviazione.
2. Fai clic sulla scheda **Replica** e fai clic sul link **Purchase a replication**.
3. Seleziona una pianificazione delle istantanee esistente che vuoi venga seguita dalle tue repliche. L'elenco contiene tutte le pianificazioni delle istantanee attive.
  **Nota:** puoi selezionare solo una singola pianificazione, anche se hai una combinazione di orarie, giornaliere e settimanali.  Tutte le istantanee acquisite a partire dal ciclo di replica precedente verranno replicate indipendentemente dalla pianificazione di origine.
4. Fai clic sulla freccia a discesa **Location** e seleziona il data center che sarà il tuo sito di ripristino di emergenza (RD, disaster recovery).
5. Fai clic su **Continue**.
6. Immetti un codice promozionale (**Promo Code**), se ne hai uno, e fai clic su **Recalculate**. Gli altri campi nella finestra di dialogo prenderanno i valori predefiniti.
7. Fai clic sulla casella di spunta **I have read the Master Service Agreement…** e fai clic su **Place Order**.


## Come modifico una replica esistente? 

Puoi modificare la tua pianificazione replica e modificare il tuo spazio di replica dalla scheda **Primary** o da quella **Replica** in **Storage** > **{{site.data.keyword.filestorage_short}}** dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.


## Come modifico una pianificazione della replica?

Stai effettivamente modificando una pianificazione delle istantanee perché la tua pianificazione replica è basata su una pianificazione delle istantanee esistente. Per modificare la pianificazione della replica, ad esempio da Hourly a Weekly, devi annullare la pianificazione replica e configurarne una nuova.

La modifica della pianificazione può essere eseguita nella scheda **Primary** o **Replica**.

1. Fai clic sul menu **Actions** dalla scheda **Primary** o da quella **Replica**.
2. Seleziona **Edit Snapshot Schedule**.
3. Controlla nel frame Snapshot in Schedule per determinare quale pianificazione stai usando per la replica. Apporta le modifiche alla pianificazione utilizzata per la replica. Ad esempio, se la tua pianificazione replica è **Daily**, puoi modificare l'ora del giorno in cui deve essere eseguita la replica. 
4. Fai clic su **Save**.


## Come modifico lo spazio di replica? 

Il tuo spazio per le istantanee primario e il tuo spazio di replica devono essere uguali. Se modifichi lo spazio nella scheda **Primary** o in quella **Replica**, verrà automaticamente aggiunto dello spazio sia al data center di origine sia a quello di destinazione. Tieni presente che un aumento dello spazio per le istantanee attiverà un immediato aggiornamento della replica.

1. Fai clic sul menu **Actions** dalla scheda **Primary** o da quella **Replica**.
2. Seleziona **Add More Snapshot Space**.
3. Seleziona la dimensione di archiviazione e fai clic su **Continue**. 
4. Immetti un codice promozionale (**Promo Code**), se ne hai uno, e fai clic su **Recalculate**. Gli altri campi nella finestra di dialogo prenderanno i valori predefiniti.
5. Fai clic sulla casella di spunta **I have read the Master Service Agreement…** e fai clic su **Place Order**.


## Come vedo i miei volumi di replica nell'elenco volumi? 

Puoi visualizzare i tuoi volumi di replica dalla pagina {{site.data.keyword.filestorage_short}} in **Storage** > **{{site.data.keyword.filestorage_short}}**. Il nome del volume (Volume Name) ha il nome del volume primario seguito da REP. Il tipo (Type) è Endurance(Performance) – Replica. L'indirizzo di destinazione (Target Address) è Non disponibile (N/A) perché il volume di replica non è montato nel data center di replica e lo stato (Status) è inattivo (Inactive).



## Come visualizzo i dettagli di un volume replicato nel data center di replica? 

Puoi visualizzare i dettagli del volume di replica nella scheda **Replica** in **Storage** > **{{site.data.keyword.filestorage_short}}**. Un'altra opzione consiste nel selezionare il volume di replica dalla pagina **{{site.data.keyword.filestorage_short}}** e nel fare clic sulla scheda **Replica**.



## Come specifico le autorizzazioni host prima dell'esecuzione del failover al data center secondario? 

Gli host autorizzati e i volumi si devono trovare nello stesso data center. Non puoi avere un volume di replica a Londra e l'host ad Amsterdam; devono trovarsi entrambi a Londra oppure ad Amsterdam. 

1. Fai clic sul tuo volume di origine o di destinazione nella pagina **{{site.data.keyword.filestorage_short}}**. 
2. Fai clic sulla scheda **Replica**. 
3. Scorri fino al frame **Authorize Hosts** e fai clic su **Authorize Hosts** alla destra.
4. Evidenzia l'host che deve essere autorizzato per le repliche. Per selezionare più host, utilizza il tasto CTRL e fai clic sugli host applicabili. 
5. Fai clic su **Submit**. Se non hai alcun host, la finestra di dialogo ti offrirà l'opzione di acquistare delle risorse di elaborazione nello stesso data center; in alternativa, puoi fare clic su **Close**.


## Come aumento il mio spazio per le istantanee nel mio data center di replica quando aumento lo spazio nel mio data center primario? 

Le dimensioni dei tuoi volumi devono essere le stesse per i volumi di archiviazione primario e di replica; l'uno non può essere più grande dell'altro. Quando aumenti il tuo spazio per le istantanee per il tuo volume primario, lo spazio di replica viene aumentato automaticamente. Tieni presente che un aumento dello spazio per le istantanee attiverà un immediato aggiornamento della replica. L'aumento per entrambi i volumi verrà visualizzato come voci di riga nella tua fattura e sarà a base proporzionale come necessario.

Fai clic [qui](snapshots.html) per informazioni su come aumentare il tuo spazio per le istantanee.

## Come avvio un failback da un volume alla sua replica? 

Nel caso di un evento di errore, puoi avviare un **Failover** al tuo volume di destinazione. Il volume di destinazione diventa attivo. L'ultima istantanea replicata correttamente viene attivata e il volume diventa disponibile per il montaggio. Tutti i dati scritti nel volume di origine a partire dal ciclo di replica precedente verranno distrutti. Tieni presente che, una volta avviato un failover, la relazione di replica si inverte. Il tuo volume di destinazione è ora il tuo volume di origine e il tuo precedente volume di origine diventa la tua destinazione, come indicato dal **REP** che segue il tuo nome LUN originale.

I failover vengono avviati in **Storage** > **{{site.data.keyword.filestorage_short}}** dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

**Prima di procedere con questo processo, si raccomanda di disconnettere il volume. In caso contrario, si verificherà un danneggiamento e una perdita di dati.**

1. Fai clic sulla tua LUN attiva ("origine"). 
2. Fai clic sulla scheda Replica e su **Actions** nell'angolo superiore destro.
3. Seleziona **Failover**.
   Riceverai un messaggio nella parte superiore che indica che il failover è in corso. Inoltre, compare un'icona accanto al tuo volume in **{{site.data.keyword.filestorage_short}}** che indica che è in corso una transazione attiva. Se passi il puntatore del mouse sull'icona, verrà visualizzata una finestra di dialogo che indica la transazione. Una volta completata la transazione, l'icona scompare. Durante il processo di failover, le azioni correlate alla configurazione sono di sola lettura. Non puoi modificare le pianificazioni delle istantanee o modificare lo spazio per le istantanee e così via. L'evento viene registrato nella cronologia replica.
4. Quando il tuo volume di destinazione è operativo, il nome LUN del tuo volume di origine originale viene aggiornato per includere il "REP" e il suo stato viene visualizzato come Inactive.
4. Fai clic sul link **View All {{site.data.keyword.filestorage_short}}** nell'angolo superiore destro. 
5. Fai clic sulla tua LUN attiva (in precedenza il tuo volume di destinazione). Questo volume è ora nello stato **Active**.
6. Monta o collega il tuo volume di archiviazione all'host.


## Come avvio un failback da un volume alla sua replica? 

Dopo che il tuo volume di origine originale sarà stato riparato, l'azione **failback** ti consentirà di avviare un failback controllato al tuo volume di origine originale. In un failback controllato,

- il volume di origine operativo viene portato offline;
- viene acquisita un'istantanea;
- il ciclo di replica viene completato;
- l'istantanea di dati appena acquisita viene attivata;
- il volume di origine viene quindi reso attivo per il montaggio. 

Tieni presente che, una volta avviato un failback, la relazione di replica si inverte nuovamente. Il tuo volume di origine viene ripristinato come tuo volume di origine e il tuo volume di destinazione diventa nuovamente il volume di destinazione, come indicato dal **REP** che segue il nome LUN.

I failback vengono avviati in **Storage** > **{{site.data.keyword.filestorage_short}}** dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}..

1. Fai clic sulla tua LUN Endurance attiva ("destinazione"). 
2. Fai clic sulla scheda **Replica** e fai clic su **Actions** nell'angolo superiore destro. 
3. Seleziona **failback**.
   Riceverai un messaggio nella parte superiore della pagina che indica che il failover è in corso. Inoltre, compare un'icona accanto al tuo volume che indica che è in corso una transazione attiva. Se passi il puntatore del mouse sull'icona, verrà visualizzata una finestra di dialogo che indica la transazione. Una volta completata la transazione, l'icona scompare. <br /> Durante il processo di failback, le azioni correlate alla configurazione sono di sola lettura. Non puoi modificare le pianificazioni delle istantanee, modificare lo spazio per le istantanee e così via. L'evento viene registrato nella cronologia replica.
5. Quando il tuo volume di origine è operativo, il tuo volume di destinazione diventerà Inactive.
4. Fai clic su **View All {{site.data.keyword.filestorage_short}}** nell'angolo superiore destro. 
5. Fai clic sulla tua LUN attiva ("origine"). Questo volume è ora in uno stato **Active**. 
6. Monta o collega il tuo volume di archiviazione all'host. Fai clic [qui](provisioning-file-storage.html) per le istruzioni.


## Come vedo la mia cronologia replica? 

La cronologia replica viene visualizzata in **Audit Log** nella scheda **Account** in **Manage**. Entrambi i volumi primario e di replica visualizzano una cronologia replica identica, che include 

- il tipo per la replica (failover o failback);
- quando è stata avviata;
- l'istantanea utilizzata per la replica;
- la dimensione della replica;
- quando è stata completata


## Come annullo una replica esistente? 

L'annullamento può essere eseguito immediatamente a alla data dell'anniversario, che causa la terminazione della fatturazione. La replica può essere annullata sia dalla scheda **Primary** che da quella **Replica**. 

1. Fai clic sul volume dalla pagina **{{site.data.keyword.filestorage_short}}**. 
2. Fai clic su **Actions** nella scheda **Primary** o in quella **Replica**. 
3. Seleziona **Cancel Replica**.
4. Seleziona quando annullare. **Immediately** oppure **Anniversary Date**, e fai clic su **Continue**. 
5. Fai clic sulla casella di spunta **I acknowledge that due to cancellation, data loss may occur** e fai clic su **Cancel Replica**. 


## Come annullo la replica quando il volume primario viene annullato? 

Quando un volume primario viene annullato, la pianificazione replica e il volume nel data center di replica vengono eliminati. Le repliche vengono annullate dalla pagina **{{site.data.keyword.filestorage_short}}**. 

 1. Evidenzia il tuo volume nella pagina **{{site.data.keyword.filestorage_short}}**.
 2. Fai clic su **Actions** e seleziona **Cancel for {{site.data.keyword.filestorage_short}}**. 
 3. Seleziona quando annullare il volume, **Immediately** oppure **Anniversary Date**, e fai clic su **Continue**.
 4. Fai clic sulla casella di spunta **I acknowledge that due to cancellation, data loss may occur** e fai clic su **Cancel**.
