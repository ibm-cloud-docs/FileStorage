---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-06"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Ordinazione e gestione di {{site.data.keyword.filestorage_full_notm}}

Ci sono due diversi tipi di archiviazione di cui puoi eseguire il provisioning in base alle tue esigenze e alle tue preferenze. Le due opzioni di volumi di {{site.data.keyword.filestorage_short}} sono:

- **Endurance**: esegui il provisioning con livelli Endurance che offrono livelli di prestazioni predefiniti e funzioni quali le istantanee e la replica.
- **Performance**: crea un ambiente Performance di alto livello dove puoi allocare le IOPS (Input/Output Operations Per Second) che desideri.

## Provisioning di {{site.data.keyword.filestorage_short}}

### Come ordinare Endurance per {{site.data.keyword.filestorage_short}}

1. Dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, fai clic su **Storage** > **{{site.data.keyword.filestorage_short}}** OPPURE, dal catalogo di {{site.data.keyword.BluSoftlayer_full}}, fai clic su **Infrastructure** > **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Fai clic su **Order {{site.data.keyword.filestorage_short}}** nell'angolo in alto a destra dello schermo. 
3. Seleziona **Endurance** dall'elenco a discesa **Select Storage Type**.
4. Fai clic sull'elenco a discesa **Location** e seleziona il tuo data center.
   - Assicurati che la nuova archiviazione sarà aggiunta nella stessa ubicazione dell'host o degli host ordinati in precedenza.
   - Se hai selezionato un data center con funzionalità migliorate (indicato da un * nell'elenco a discesa), disporrai dell'opzione di scegliere tra fatturazione mensile od oraria. 
     1. Con la fatturazione **oraria**, il calcolo del numero di ore per cui il volume di file è esistito nell'account viene eseguito quando il volume viene eliminato oppure alla fine del ciclo di fatturazione, a seconda di quale di queste condizioni si verifichi per prima. La fatturazione oraria è una buona scelta per l'archiviazione utilizzata per qualche giorno o per meno di un mese completo. La fatturazione oraria è disponibile solo per l'archiviazione di cui viene eseguito il provisioning in questi [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html). 
     2. Con la fatturazione **mensile**, il calcolo per il prezzo è a base proporzionale dalla data di creazione alla fine del ciclo di fatturazione e viene fatturato immediatamente. Non è previsto alcun rimborso se il volume di file viene eliminato prima della fine del ciclo di fatturazione. La fatturazione mensile è una buona scelta per l'archiviazione utilizzata nei carichi di lavoro di produzione che usano dati che devono essere archiviati e a cui bisogna accedere per lunghi periodi di tempo (un mese o più).
     **NOTA**: il tipo di fatturazione mensile viene utilizzato per impostazione predefinita per l'archiviazione di cui viene eseguito il provisioning in data center che non sono aggiornati con funzionalità migliorate.
5. Seleziona il livello di archiviazione per le tue applicazioni.
    - **0,25 IOPS per GB** è progettato per carichi di lavori con bassa intensità di I/O. Questi carichi di lavoro sono di norma caratterizzati dall'avere un'ampia percentuale di dati inattivi in un dato momento. Delle applicazioni di esempio includono le caselle di posta di archiviazione o le condizioni di file a livello di reparto.
    - **2 IOPS per GB** è progettato per un utilizzo per finalità più generiche. Delle applicazioni di esempio includono le attività di host di piccoli database a supporto di applicazioni web o le immagini disco di macchina virtuale per un hypervisor.
    - **4 IOPS per GB** è progettato per i carichi di lavoro a maggiore intensità. Questi carichi di lavoro sono di norma caratterizzati dall'avere un'elevata percentuale di dati attivi in un dato momento. Delle applicazioni di esempio includono i database transazionali e altri database sensibili alle prestazioni.
    - **10 IOPS per GB** è progettato per i carichi di lavoro più esigenti quali quelli creati dai database NoSQL e l'elaborazione di dati per l'analisi. Questo livello è disponibile in [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html) per l'archiviazione di cui viene eseguito il provisioning fino a 4TB di dimensione.
6. Seleziona **Usable Storage Size** dall'elenco a discesa.
7. Scegli la dimensione dello spazio per le istantanee (**Snapshot Space Size**) (oltre al tuo spazio utilizzabile) dall'elenco a discesa.
8. Fai clic su **Continue**.Ti vengono mostrati gli addebiti mensili e a base proporzionale con una possibilità finale di riesaminare i dettagli dell'ordine.
9. Fai clic sulla casella di spunta **I have read the Master Service Agreement** e fai clic su **Place Order**.
10. La tua nuova allocazione di archiviazione dovrebbe essere disponibile in pochi minuti.

**Nota**: per impostazione predefinita, puoi eseguire il provisioning di un totale combinato di 250 volumi {{site.data.keyword.blockstorageshort}}. Per aumentare il numero dei tuoi volumi, contatta il tuo rappresentante di vendita. Troverai ulteriori informazioni sull'aumento dei limiti [qui](managing-storage-limits.html).

Per il limite di autorizzazioni simultanee, consulta le nostre [Domande frequenti (FAQ)](File-Storage-FAQ.html)

### Come ordinare Performance per {{site.data.keyword.filestorage_short}}

1. Dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, fai clic su **Storage**, **{{site.data.keyword.filestorage_short}}** OPPURE, dal catalogo di {{site.data.keyword.BluSoftlayer_full}}, fai clic su **Infrastructure** >** Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Fai clic su **Order {{site.data.keyword.filestorage_short}}** nell'angolo in alto a destra dello schermo. 
3. Seleziona **Performance** dall'elenco a discesa Select Storage Type.
4. Fai clic sull'elenco a discesa **Location** e seleziona il tuo data center.
    -  Assicurati che la nuova archiviazione sarà aggiunta nella stessa ubicazione dell'host o degli host ordinati in precedenza.
    -  Se hai selezionato un data center con funzionalità migliorate (indicato da un * nell'elenco a discesa), disporrai dell'opzione di scegliere tra fatturazione mensile od oraria. 
       1.  Con la fatturazione **oraria**, il calcolo del numero di ore per cui il volume di file è esistito nell'account viene eseguito quando il volume viene eliminato oppure alla fine del ciclo di fatturazione, a seconda di quale di queste condizioni si verifichi per prima. La fatturazione oraria è una buona scelta per l'archiviazione utilizzata per qualche giorno o per meno di un mese completo. La fatturazione oraria è disponibile solo per l'archiviazione di cui viene eseguito il provisioning in questi data center selezionati. 
       2. Con la fatturazione **mensile**, il calcolo per il prezzo è a base proporzionale dalla data di creazione alla fine del ciclo di fatturazione e viene fatturato immediatamente. Non è previsto alcun rimborso se il volume di file viene eliminato prima della fine del ciclo di fatturazione. La fatturazione mensile è una buona scelta per l'archiviazione utilizzata nei carichi di lavoro di produzione che usano dati che devono essere archiviati e a cui bisogna accedere per lunghi periodi di tempo (un mese o più).
       **NOTA**: il tipo di fatturazione mensile viene utilizzato per impostazione predefinita per l'archiviazione di cui viene eseguito il provisioning in data center che non sono aggiornati con funzionalità migliorate.  
5. Selezionare il pulsante di opzione accanto alla dimensione di archiviazione (**Storage Size**) appropriata.
6. Immetti l'IOPS nel campo **Specify IOPS**.
7. Fai clic su **Continue**. Ti vengono mostrati gli addebiti mensili e a base proporzionale con una possibilità finale di riesaminare i dettagli dell'ordine. Fai clic su **Previous** se vuoi modificare il tuo ordine.
8. Fai clic sulla casella di spunta **I have read the Master Service Agreement** e fai clic su **Place Order**.
9. La tua nuova allocazione di archiviazione dovrebbe essere disponibile in pochi minuti.

**Nota**: per impostazione predefinita, puoi eseguire il provisioning di un totale combinato di 250 volumi {{site.data.keyword.blockstorageshort}}. Per aumentare il numero dei tuoi volumi, contatta il tuo rappresentante di vendita. Troverai ulteriori informazioni sull'aumento dei limiti [qui](managing-storage-limits.html).

Per il limite di autorizzazioni simultanee, consulta le nostre [Domande frequenti (FAQ)](File-Storage-FAQ.html)

## Gestione dell'{{site.data.keyword.filestorage_short}}

### Come autorizzare gli host ad accedere all'{{site.data.keyword.filestorage_short}}

Gli host "autorizzati" sono host a cui sono stati concessi i diritti di accesso a uno specifico volume. Senza l'autorizzazione host, non potrai accedere all'archiviazione dal tuo sistema né utilizzarla. Autorizzare un host ad accedere al tuo volume genera il nome utente e la password. 

**Nota**: puoi autorizzare e connettere solo gli host che si trovano nello stesso data center della tua archiviazione. Se hai più account, non puoi autorizzare un host da un account ad accedere alla tua archiviazione su un altro. 

1. Fai clic su **Storage** > **{{site.data.keyword.filestorage_short}}** e fai clic sul tuo nome volume (**Volume Name**).
2. Scorri alla sezione **Authorized Hosts** della pagina.
3. Fai clic sul link **Authorize Host** sul lato destro della pagina. Seleziona gli host che possono accedere allo specifico volume.

 

### Come visualizzare l'elenco di host autorizzati a un volume {{site.data.keyword.filestorage_short}}

Attieniti alla seguente procedura per visualizzare l'elenco di host autorizzati a un volume.

1. Fai clic su **Storage > {{site.data.keyword.filestorage_short}}** e fai clic sul tuo nome volume (**Volume Name**).
2. Scorri verso il basso fino al fondo della pagina, alla sezione **Authorized Hosts**.

Qui vedrai un elenco degli host attualmente autorizzati ad accedere al volume.


### Come visualizzare i volumi {{site.data.keyword.filestorage_short}} a cui è autorizzato un host

Puoi visualizzare i volumi a cui ha accesso un host, comprese le informazioni necessarie per stabilire una connessione, il nome volume, il tipo di archiviazione, l'indirizzo di destinazione, la capacità e l'ubicazione:

1. Fai clic su **Devices** > **Device List** dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} e fai clic sul dispositivo appropriato.
2. Seleziona la scheda Storage.

Ti verrà presentato un elenco dei volumi di archiviazione a cui questo specifico host ha accesso, tutti raggruppati in base al tipo di archiviazione (blocco, file, altro). Dai rispettivi menu **Action**, puoi autorizzare ulteriore archiviazione oppure rimuovere l'accesso.

 

### Come montare e smontare {{site.data.keyword.filestorage_short}}

Puoi utilizzare le informazioni sul punto di montaggio fornite nella vista **Volume Details** per montare dell'{{site.data.keyword.filestorage_short}} da un host.

Consulta il seguente articolo con i dettagli per montare e smontare {{site.data.keyword.filestorage_short}} da un host: [Accesso a {{site.data.keyword.filestorage_short}} su Linux](accessing-file-storage-linux.html)

 

### Come revocare l'accesso di un host all'{{site.data.keyword.filestorage_short}}

Se vuoi arrestare l'accesso da uno specifico host a uno specifico volume di archiviazione, puoi revocare l'accesso. Una volta revocato l'accesso, la connessione host al volume verrà interrotta e né il sistema operativo né le applicazioni saranno in grado di comunicare con il volume. 

**Nota:** per evitare problemi sul lato host, smonta il volume di archiviazione dal tuo sistema operativo prima di revocare l'accesso per evitare unità mancanti o il danneggiamento dei dati.

Puoi revocare l'accesso dall'una o dall'altra archiviazione dalle viste Device List o Storage.

#### Come revocare l'accesso da Device List:

1. Fai clic su **Devices** > **Device List** dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} e fai doppio clic sul dispositivo appropriato.
2. Seleziona la scheda **Storage**.
3. Ti verrà presentato un elenco dei volumi di archiviazione a cui questo specifico host ha accesso, tutti raggruppati in base al tipo di archiviazione (blocco, file, altro). Seleziona il menu **Action** rispettivo accanto al volume dal quale vuoi revocare l'accesso e fai clic su **Revoke Access**.
4. Ti verrà chiesto se vuoi revocare l'accesso per un volume, poiché l'azione non può essere annullata. Fai clic su **Yes** per revocare l'accesso del volume oppure su **No** per annullare l'azione.

**Nota:** se vuoi disconnettere più volumi da uno specifico host, dovrai ripetere l'azione di revoca dell'accesso per ciascun volume.

 

#### Come revocare l'accesso dalla vista Storage:
1. Fai clic su **Storage, {{site.data.keyword.filestorage_short}}** e seleziona il **Volume** da cui vuoi revocare l'accesso.
2. Scorri verso il basso alla sezione **Authorized Hosts** della pagina.
3. Fai clic sulla freccia a discesa **Actions** accanto all'host di cui deve essere revocato l'accesso e seleziona **Revoke Access**.
4. Ti verrà chiesto se vuoi revocare l'accesso per un volume, poiché l'azione non può essere annullata. Fai clic su **Yes** per revocare l'accesso del volume oppure su **No** per annullare l'azione.

**Nota:** se vuoi disconnettere più host da uno specifico volume, dovrai ripetere l'azione di revoca dell'accesso per ciascun host.

 

### Come annullare un volume di archiviazione

Se non hai più bisogno di uno specifico volume, puoi annullarlo. Per annullare un volume di archiviazione, è prima necessario revocare l'accesso da qualsiasi host.

1. Fai clic su **Storage**>**{{site.data.keyword.filestorage_short}}**.
2. Fai clic sulla freccia a discesa **Actions** per il volume da annullare e seleziona **Cancel {{site.data.keyword.filestorage_short}}**.
3. Ti verrà chiesto se confermi che vuoi annullare il volume immediatamente oppure alla data di anniversario dell'esecuzione del provisioning del volume. Fai clic su **Continue** o su **Close**.
**Nota**: se selezioni l'opzione di annullare il volume alla sua data di anniversario, puoi invalidare la richiesta di annullamento prima della sua data di anniversario.
4. Fai clic sulla casella di spunta di riconoscimento e fai clic su **Confirm**.

 

### Come visualizzare i dettagli di un volume {{site.data.keyword.filestorage_short}} di cui è stato eseguito il provisioning

Puoi visualizzare un riepilogo delle informazioni chiave per il volume di archiviazione selezionato, incluse le funzionalità di istantanea e replica supplementari che sono state aggiunte all'archiviazione.

1. Fai clic su **Storage**>**{{site.data.keyword.filestorage_short}}**.
2. Fai clic sul nome volume (**Volume Name**) appropriato dall'elenco.

### Come identifico i miei volumi {{site.data.keyword.filestorage_short}} nella mia fattura

Tutti i volumi nella tua fattura si presenteranno come una voce di riga; Endurance si presenterà come “Endurance Storage Service” e Performance si presenterà come "Performance Storage service". la tariffa varierà in base al livello di archiviazione. Puoi espandere Endurance o Performance per appurare che sia un volume {{site.data.keyword.filestorage_short}}.
