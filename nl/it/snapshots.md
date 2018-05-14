---

copyright:
  years: 2014, 2017
lastupdated: "2017-12-11"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Gestione delle istantanee

Le istantanee sono una funzione di {{site.data.keyword.filestorage_full}}. Un'istantanea rappresenta il contenuto di un volume a uno specifico punto temporale. Le istantanee ti consentono di proteggere i tuoi dati senza ripercussioni sulle prestazioni, con un consumo minimo di spazio e sono considerate la prima linea di difesa per la protezione dei dati. Con la funzione di istantanea, se un utente modifica o elimina accidentalmente dei dati cruciali da un volume può ripristinarli in modo facile e veloce da una copia istantanea.

{{site.data.keyword.filestorage_short}} ti fornisce due modi per acquisire le tue istantanee: uno consiste in una pianificazione delle istantanee configurabile che crea ed elimina le copie di istantanea automaticamente per ciascun volume di archiviazione. Puoi anche creare delle pianificazioni delle istantanee aggiuntive, eliminare manualmente delle copie e gestire le pianificazioni in base ai tuoi requisiti. Il secondo modo consiste nell'acquisire un'istantanea manuale.

Una copia istantanea è un'immagine di sola lettura di un volume {{site.data.keyword.filestorage_short}} che acquisisce lo stato del volume a un punto temporale. Le copie istantanea sono estremamente efficienti sia per quanto riguarda il tempo necessario per crearle che per quanto riguarda lo spazio di archiviazione. La creazione di una copia istantanea {{site.data.keyword.filestorage_short}} impiega solo pochi secondi, di norma meno di 1 secondo, indipendentemente dalla dimensione del volume o dal livello di attività sull'archiviazione. Dopo che una copia istantanea è stata creata, le modifiche agli oggetti di dati sono riflesse negli aggiornamenti alla versione corrente degli oggetti, come se le copie istantanea non esistessero. Nel frattempo, la copia dei dati rimane completamente stabile. 

Una copia istantanea non comporta alcun sovraccarico delle prestazioni; gli utenti possono facilmente archiviare fino a 50 istantanee pianificate e 50 istantanee manuali per volume {{site.data.keyword.blockstorageshort}}, tutte accessibili come versioni di sola lettura e online dei dati.

Le istantanee consentono agli utenti di

- Creare, in un modo che non interrompe le operazioni, dei punti di ripristino a un punto temporale
- Ripristinare i volumi a punti temporali precedenti
- Devi acquistare dello spazio per le istantanee per il tuo volume al fine di acquisirne delle istantanee. Lo spazio per le istantanee può essere aggiunto durante l'ordinazione di volumi iniziale o dopo il provisioning iniziale tramite la pagina Volume Details, facendo clic sul pulsante a discesa Actions e selezionando Add Snapshot Space. Tieni presente che le istantanee pianificate e manuali condividono lo spazio per le istantanee; ordina quindi il tuo spazio di conseguenza.

## Prassi ottimali per le istantanee
La progettazione dell'istantanea dipende dall'ambiente del cliente. Le seguenti considerazioni sulla progettazione ti aiuteranno a pianificare e implementare copie istantanea. 
- 	Puoi creare fino a 50 istantanee tramite la pianificazione e fino a 50 manualmente su ogni volume o LUN. 
- 	Non acquisire troppe istantanee. Assicurati che la tua frequenza di istantanee pianificate soddisfi le tue esigenze di obiettivo del tempo di ripristino (RTO) e obiettivo del punto di ripristino (RPO) nonché i tuoi requisiti di business applicativi pianificando istantanee orarie, giornaliere o settimanali. 
- 	La funzione di eliminazione automatica (AutoDelete) di istantanea deve essere utilizzata per controllare la crescita del consumo dell'archiviazione. <br/>
    **Nota**: la soglia di AutoDelete è fissata al 95%.
    
## In che modo le copie di istantanee consumano spazio su disco?
Le copie istantanea riducono al minimo il consumo di disco preservando i singoli blocchi invece dei file interi. Le copie istantanea iniziano a consumare spazio extra solo quando i file nel file system attivo vengono modificati o eliminati. Quando ciò si verifica, i blocchi file originali continuano a essere conservati come parte di una o più copie istantanea.
Nel file system attivo, i blocchi modificati vengono riscritti in ubicazioni differenti sul disco oppure completamente rimossi come blocchi file attivi. Di conseguenza, oltre allo spazio su disco utilizzato dai blocchi nel file system attivo modificato, lo spazio su disco utilizzato dai blocchi originali continua a essere riservato per riflettere lo stesso del file system attivo prima della modifica.

<table>
    <colgroup>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
    </colgroup>
    <tbody>
      <tr>
        <th colspan="3" style="border: 0.0px;text-align: center;">Utilizzo dello spazio su disco prima e dopo la copia istantanea</th>
     </tr><tr>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle1.png" alt="Prima della copia istantanea"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle3.png" alt="Dopo la copia istantanea"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle2.png" alt="Modifiche dopo la copia istantanea"></td>
     </tr><tr>
        <td style="border: 0.0px;">Prima che venga creata qualsiasi copia istantanea, lo spazio su disco è consumato solo dal file system attivo.</td>
        <td style="border: 0.0px;">Dopo la creazione di una copia istantanea, il file system attivo e la copia istantanea puntano agli stessi blocchi disco. La copia istantanea non usa spazio su disco extra.</td>
        <td style="border: 0.0px;">Dopo che <i>myfile.txt</i> è stato eliminato dal file system attivo, la copia istantanea continua a includere il file e i riferimenti ai suoi blocchi disco. Per tale motivo l'eliminazione dei dati del file system attivo non sempre libera spazio su disco.</td>
      </tr>
    </tbody>
</table>



## Come acquisto spazio per le istantanee?

Per creare le istantanee del tuo volume di archiviazione, in modo automatizzato o manualmente, devi acquistare dello spazio per contenerle. Puoi acquistare capacità fino alla tua quantità di volume di archiviazione (durante l'acquisto di volume iniziale e successivamente attenendoti alla procedura indicata di seguito).

1. Accedi alla tua archiviazione tramite la scheda **Storage** > **{{site.data.keyword.filestorage_short}}** del [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}
2. Fai clic sul link Add Snapshot Space nel frame Snapshots. Fai clic sul link **Purchase snapshot space now** nel frame Snapshots.
3. Seleziona la quantità di spazio di cui hai bisogno facendo clic sul pulsante di opzione accanto alla quantità appropriata.
4. Fai clic su **Continue**.
5. Immetti l'eventuale codice promozionale (Promo Code) a tua disposizione e fai clic su **Recalculate**. Gli addebiti per questo ordine (Charges for this order) e il riesame dell'ordine (Order Review) avranno i valori predefiniti.
6. Fai clic sulla casella di spunta **I have read the Master Service Agreement…** e fai clic su **Place Order**. Nel giro di pochi minuti, verrà eseguito il provisioning del tuo spazio per le istantanee.

## Come creo una pianificazione delle istantanee?

Le pianificazioni delle istantanee ti consentono di decidere con che frequenza e quando vuoi creare un riferimento ad un punto nel tempo del tuo volume di archiviazione. Puoi avere un massimo di 50 istantanee per volume di archiviazione. Le pianificazioni sono gestite tramite la scheda **Storage** > **{{site.data.keyword.filestorage_short}}** del [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

Prima di poter configurare la tua pianificazione iniziale, devi procedere all'acquisto di spazio per le istantanee, se non lo hai fatto durante il provisioning iniziale del volume di archiviazione.

### Aggiungi una pianificazione delle istantanee

Le pianificazioni delle istantanee possono essere configurate per intervalli orari, giornalieri e settimanali, ciascuno con un distinto ciclo di conservazione. C'è un massimo di 50 istantanee pianificate, che può essere una combinazione di pianificazioni orarie, giornaliere e settimanali e di istantanee manuali per volume di archiviazione.

1. Fai clic sul tuo volume di archiviazione, fai clic sulla casella a discesa **Actions** e fai clic su **Schedule Snapshot**.
2. Nella finestra New Schedule Snapshot ci sono tre diverse frequenze di istantanea da cui scegliere. Usa qualsiasi combinazione di queste tre frequenze per creare una pianificazione delle istantanee completa.
   - Hourly (Ogni ora)
      - Specifica il minuto di ciascuna ora in cui deve essere eseguita un'istantanea; il valore predefinito è il minuto corrente.
      - Specifica il numero di istantanee orarie da conservare prima di eliminare quella meno recente.
   - Daily (Ogni giorno)
      - Specifica l'ora e il minuto in cui deve essere eseguita un'istantanea; il valore predefinito sono l'ora e il minuto correnti.
      - Seleziona il numero di istantanee orarie da conservare prima di eliminare quella meno recente.
   - Weekly (Ogni settimana)
      - Specifica il giorno della settimana, l'ora e il minuto in cui deve essere eseguita un'istantanea; il valore predefinito sono il giorno, l'ora e il minuto correnti.
      - Seleziona il numero di istantanee settimanali da conservare prima di eliminare quella meno recente.
3. Fai clic su **Save** e crea un'altra pianificazione con una frequenza differente. Nota: riceverai un messaggio di avvertenza e non potrai eseguire il salvataggio se il numero totale di istantanee pianificate è superiore a 50.

Un elenco delle istantanee verrà visualizzato man mano che vengono eseguite nella sezione Snapshots della pagina Detail.

## Come acquisisco un'istantanea manuale?

Le istantanee manuali possono essere acquisite a vari punti durante un upgrade o una manutenzione dell'applicazione. Ti consentono anche di acquisire delle istantanee su più macchine che sono state temporaneamente disattivate a livello dell'applicazione.

C'è un massimo di 50 istantanee manuali per volume di archiviazione.

1. Fai clic sul tuo volume di archiviazione.
2. Fai clic sulla casella a discesa Actions.
3. Fai clic su **Take Manual Snapshot**.
L'istantanea verrà acquisita e verrà visualizzata nella sezione Snapshots della pagina Detail. La sua pianificazione sarà manuale (Manual).

## Come vedo un elenco di istantanee con lo spazio consumato e le funzioni di gestione?

Un elenco di istantanee conservate e spazio consumato può essere visualizzato nella pagina Detail (**Storage** > **{{site.data.keyword.filestorage_short}}**). Le funzioni di gestione (modifica di pianificazioni e aggiunta di ulteriore spazio) vengono controllate nella pagina **Detail** utilizzando il menu a discesa **Actions** oppure i link nelle diverse sezioni della pagina.

## Come visualizzo un elenco di istantanee conservate?

Le istantanee conservate sono basate sul numero che hai immesso nel campo **Keep the last field** quando configuri le tue pianificazioni. Puoi visualizzare le istantanee che hai acquisito nella sezione **Snapshot**. Le istantanee sono elencate in base alla pianificazione.

## Come visualizzo quanto spazio per le istantanee è stato utilizzato?

Il grafico a torta nella parte superiore della pagina **Details** visualizza quanto spazio è stato utilizzato e quanto spazio è rimasto. Riceverai delle notifiche quando inizi a raggiungere le soglie di spazio – 75%, 90%, 95%.

## Come modifico la quantità di spazio per le istantanee per il mio volume?

Potresti aver bisogno di aggiungere dello spazio per le istantanee a un volume che in precedenza non ne aveva o che potrebbe avere bisogno di spazio per le istantanee aggiuntivo. Puoi aggiungere tra i 5 GB e i 4.000 GB, a seconda delle tue esigenze. Nota: lo spazio per le istantanee può essere solo aumentato e non ridotto. Dovresti selezionare una quantità di spazio più piccola finché non determini di quanto spazio hai effettivamente bisogno. Ricordati che le istantanee automatizzate e quelle manuali condividono lo stesso spazio.

Lo spazio per le istantanee viene modificato tramite **Storage** > **{{site.data.keyword.filestorage_short}}**.

1. Fai clic sui tuoi volumi di archiviazione, fai clic sulla casella a discesa **Actions** e fai clic su **Add More Snapshot Space**.
2. Seleziona da un intervallo di dimensioni dal prompt. Le dimensioni di norma vanno da 0 alla dimensione del tuo volume.
3. Fai clic su **Continue** per eseguire il provisioning dello spazio aggiuntivo.
4. Immetti l'eventuale codice promozionale (Promo Code) a tua disposizione e fai clic su **Recalculate**. Gli addebiti per quest'ordine (**Charges for this order**) e il riesame dell'ordine (**Order Review**) presenteranno i valori predefiniti.
5. Fai clic sulla casella di spunta **I have read the Master Service Agreement…** e fai clic su **Place Order**. Nel giro di pochi minuti, verrà eseguito il provisioning del tuo spazio per le istantanee aggiuntivo.

## Come ricevo delle notifiche quando sono prossimo al mio limite di spazio per le istantanee e quando vengono eliminate le istantanee?

Le notifiche vengono inviate tramite i ticket di supporto da Support al Master User sul tuo account quando raggiungi tre diverse soglie di spazio– 75%, 90%, 95%.

- Capacità al 75% : viene inviata un'avvertenza che l'utilizzo dello spazio per le istantanee ha superato il 75%. Se presti attenzione all'avvertenza e aggiungi dello spazio manualmente oppure elimini delle istantanee conservate e non necessarie, l'azione viene annotata e il ticket viene chiuso. Se non fai niente, devi confermare manualmente il ticket che viene quindi chiuso.
- Capacità al 90% : una seconda avvertenza viene inviata quando l'utilizzo dello spazio per le istantanee ha superato il 90%. In modo analogo al raggiungimento della capacità al 75% , se esegui le azioni necessarie per ridurre lo spazio utilizzato, l'azione viene annotata e il ticket viene chiuso. Se non fai niente, devi confermare manualmente il ticket che viene quindi chiuso.
- Capacità al 95% : viene inviata un'avvertenza finale. Se non viene eseguita alcuna azione per portare il tuo spazio al di sotto della soglia, viene generata una notifica e si verifica un'eliminazione automatica in modo che sia possibile creare delle future istantanee. Le istantanee pianificate vengono eliminate, a partire da quella meno recente, finché l'utilizzo non scende al di sotto del 95%, e continueranno a essere eliminate ogni volta che l'utilizzo supera il 95% finché non scende al di sotto della soglia. Se lo spazio viene aumentato manualmente oppure se le istantanee vengono eliminate, l'avvertenza verrà reimpostata ed emessa nuovamente se la soglia viene superata nuovamente. Se non viene eseguita alcuna azione, questa sarà la sola avvertenza che verrà ricevuta.

## Come elimino una pianificazione delle istantanee?

Le pianificazioni delle istantanee possono essere annullate tramite **Storage** > **{{site.data.keyword.filestorage_short}}**.

1. Fai clic sulla pianificazione da eliminare nel frame **Snapshot Schedules** nella pagina **Details**.
2. Fai clic nella casella di spunta accanto alla pianificazione da eliminare e fai clic su **Save**.<br/>
Attenzione: se stai utilizzando la funzione di replica, assicurati che la pianificazione che stai eliminando non sia la pianificazione utilizzata dalla replica. Fai clic [qui](replication.html) per ulteriori informazioni sull'eliminazione di una pianificazione della replica.

## Come elimino un'istantanea?

Le istantanee che non sono più necessarie possono essere rimosse manualmente per liberare spazio per le future istantanee. L'eliminazione viene eseguita tramite **Storage** > **{{site.data.keyword.filestorage_short}}**.

1. Fai clic sul tuo volume di archiviazione e scorri verso il basso alla sezione Snapshot per visualizzare un elenco delle istantanee esistenti.
2. Fai clic sull'elenco a discesa Actions accanto a una specifica istantanea e fai clic su **Delete** per eliminare l'istantanea. Ciò non avrà alcuna ripercussione sulle eventuali istantanee passate o future nella stessa pianificazione poiché le istantanee non hanno alcuna interdipendenza.

Le istantanee manuali che non sono eliminate nel modo sopra descritto sono eliminate automaticamente (prima quella meno recente) quando raggiungi le limitazioni di spazio.

## Come ripristino il mio volume di archiviazione a uno specifico punto temporale utilizzando un'istantanea?

Potresti dover riportare il tuo volume di archiviazione a uno specifico punto temporale a causa di un errore utente o di un danneggiamento dei dati.

1. Smonta e scollega il tuo volume di archiviazione dall'host.
   - Fai clic [qui ](accessing-file-storage-linux.html) per le istruzioni di {{site.data.keyword.filestorage_short}} su Linux.
2. Fai clic su **Storage**, **{{site.data.keyword.filestorage_short}}** nel [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
3. Scorri verso il basso e fai clic sul tuo volume da ripristinare. La sezione Snapshots della pagina Detail visualizzerà un elenco di tutte le istantanee salvate insieme alla loro dimensione e alla loro data di creazione.
4. Fai clic sul pulsante **Actions** per l'istantanea da utilizzare e fai clic su **Restore**. 

   **Nota**: l'esecuzione di un ripristino produrrà una perdita dei dati creati o modificati a partire da quando era stata acquisita l'istantanea che si sta utilizzando. Una volta completata l'operazione, il tuo volume di archiviazione verrà ripristinato allo stesso stato in cui si trovava quando era stata acquisita l'istantanea. Verrà visualizzato un prompt che ti informerà di tale operazione.
5. Fai clic su **Yes** per avviare il ripristino. Riceverai un messaggio nella parte superiore della pagina che indica che il volume è stato ripristinato utilizzando l'istantanea selezionata. Comparirà inoltre un'icona accanto al tuo volume in {{site.data.keyword.filestorage_short}} che indica che è in corso una transazione attiva. Se passi il puntatore del mouse sull'icona, verrà visualizzata una finestra di dialogo che indica la transazione. Una volta completata la transazione, l'icona scomparirà.
6. Monta e ricollega il tuo volume di archiviazione all'host.
   - Fai clic [qui ](accessing-file-storage-linux.html) per le istruzioni di {{site.data.keyword.filestorage_short}} su Linux.
   
**Nota**: il ripristino di un volume comporterà l'eliminazione di tutte le istantanee antecedenti all'istantanea ripristinata.

