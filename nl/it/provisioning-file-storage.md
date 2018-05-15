---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Ordinazione di {{site.data.keyword.filestorage_short}} 

Sulla base delle tue esigenze e delle tue preferenze, ci sono due tipi di {{site.data.keyword.filestorage_full}} di cui puoi eseguire il provisioning. Le due opzioni di volumi sono: 

- **Endurance**: esegui il provisioning con livelli Endurance che offrono livelli di prestazioni predefiniti e funzioni quali le istantanee e la replica.
- **Performance**: crea un ambiente Performance di alto livello dove puoi allocare le IOPS (Input/Output Operations Per Second) che desideri.

## Come ordinare Endurance per {{site.data.keyword.filestorage_short}}

1. Dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, fai clic su **Storage** > **{{site.data.keyword.filestorage_short}}** OPPURE, dal catalogo di {{site.data.keyword.BluSoftlayer_full}}, fai clic su **Infrastructure** > **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Fai clic su **Order {{site.data.keyword.filestorage_short}}** nell'angolo in alto a destra dello schermo. 
3. Seleziona **Endurance** dall'elenco a discesa **Select Storage Type**.
4. Fai clic sull'elenco a discesa **Location** e seleziona il tuo data center.
   - Assicurati che la nuova archiviazione sarà aggiunta nella stessa ubicazione dell'host o degli host ordinati in precedenza.
   - Se hai selezionato un data center con funzionalità migliorate (indicato da un * nell'elenco a discesa), disporrai dell'opzione di scegliere tra fatturazione mensile od oraria. 
     1. Con la fatturazione **oraria**, il calcolo del numero di ore per cui il volume di file è esistito nell'account viene eseguito quando il volume viene eliminato oppure alla fine del ciclo di fatturazione, a seconda di quale di queste condizioni si verifichi per prima.  La fatturazione oraria è una buona scelta per l'archiviazione utilizzata per qualche giorno o per meno di un mese completo. La fatturazione oraria è disponibile solo per l'archiviazione di cui viene eseguito il provisioning in questi [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html). 
     2. Con la fatturazione **mensile**, il calcolo per il prezzo è a base proporzionale dalla data di creazione alla fine del ciclo di fatturazione e viene fatturato immediatamente. Non è previsto alcun rimborso se il volume di file viene eliminato prima della fine del ciclo di fatturazione.  La fatturazione mensile è una buona scelta per l'archiviazione utilizzata nei carichi di lavoro di produzione che usano dati che devono essere archiviati e a cui bisogna accedere per lunghi periodi di tempo (un mese o più).
     **NOTA**: il tipo di fatturazione mensile viene utilizzato per impostazione predefinita per l'archiviazione di cui viene eseguito il provisioning in data center che non sono aggiornati con funzionalità migliorate.
5. Seleziona il livello di archiviazione per le tue applicazioni.
    - **0,25 IOPS per GB** è progettato per carichi di lavori con bassa intensità di I/O. Questi carichi di lavoro sono di norma caratterizzati dall'avere un'ampia percentuale di dati inattivi in un dato momento. Delle applicazioni di esempio includono le caselle di posta di archiviazione o le condizioni di file a livello di reparto.
    - **2 IOPS per GB** è progettato per un utilizzo per finalità più generiche. Delle applicazioni di esempio includono le attività di host di piccoli database a supporto di applicazioni web o le immagini disco di macchina virtuale per un hypervisor.
    - **4 IOPS per GB** è progettato per i carichi di lavoro a maggiore intensità. Questi carichi di lavoro sono di norma caratterizzati dall'avere un'elevata percentuale di dati attivi in un dato momento. Delle applicazioni di esempio includono i database transazionali e altri database sensibili alle prestazioni.
    - **10 IOPS per GB** è progettato per i carichi di lavoro più esigenti quali quelli creati dai database NoSQL e l'elaborazione di dati per l'analisi.  Questo livello è disponibile in [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html) per l'archiviazione di cui viene eseguito il provisioning fino a 4TB di dimensione.
6. Seleziona **Usable Storage Size** dall'elenco a discesa.
7. Scegli la dimensione dello spazio per le istantanee (**Snapshot Space Size**) (oltre al tuo spazio utilizzabile) dall'elenco a discesa.
8. Fai clic su **Continue**. Ti vengono mostrati gli addebiti mensili e a base proporzionale con una possibilità finale di riesaminare i dettagli dell'ordine.
9. Fai clic sulla casella di spunta **I have read the Master Service Agreement** e fai clic su **Place Order**.
10. La tua nuova allocazione di archiviazione dovrebbe essere disponibile in pochi minuti.

**Nota**: per impostazione predefinita, puoi eseguire il provisioning di un totale combinato di 250 volumi {{site.data.keyword.filestorage_short}}. Per aumentare il numero dei tuoi volumi, contatta il tuo rappresentante di vendita. Troverai ulteriori informazioni sull'aumento dei limiti [qui](managing-storage-limits.html).

Per il limite di autorizzazioni simultanee, consulta le nostre [Domande frequenti (FAQ)](File-Storage-FAQ.html)

## Come ordinare Performance per {{site.data.keyword.filestorage_short}}

1. Dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, fai clic su **Storage**, **{{site.data.keyword.filestorage_short}}** OPPURE, dal catalogo di {{site.data.keyword.BluSoftlayer_full}}, fai clic su **Infrastructure** >** Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Fai clic su **Order {{site.data.keyword.filestorage_short}}** nell'angolo in alto a destra dello schermo. 
3. Seleziona **Performance** dall'elenco a discesa Select Storage Type.
4. Fai clic sull'elenco a discesa **Location** e seleziona il tuo data center.
    -  Assicurati che la nuova archiviazione sarà aggiunta nella stessa ubicazione dell'host o degli host ordinati in precedenza.
    -  Se hai selezionato un data center con funzionalità migliorate (indicato da un * nell'elenco a discesa), disporrai dell'opzione di scegliere tra fatturazione mensile od oraria. 
       1.  Con la fatturazione **oraria**, il calcolo del numero di ore per cui il volume di file è esistito nell'account viene eseguito quando il volume viene eliminato oppure alla fine del ciclo di fatturazione, a seconda di quale di queste condizioni si verifichi per prima.  La fatturazione oraria è una buona scelta per l'archiviazione utilizzata per qualche giorno o per meno di un mese completo. La fatturazione oraria è disponibile solo per l'archiviazione di cui viene eseguito il provisioning in questi data center selezionati. 
       2. Con la fatturazione **mensile**, il calcolo per il prezzo è a base proporzionale dalla data di creazione alla fine del ciclo di fatturazione e viene fatturato immediatamente. Non è previsto alcun rimborso se il volume di file viene eliminato prima della fine del ciclo di fatturazione.  La fatturazione mensile è una buona scelta per l'archiviazione utilizzata nei carichi di lavoro di produzione che usano dati che devono essere archiviati e a cui bisogna accedere per lunghi periodi di tempo (un mese o più).
       **NOTA**: il tipo di fatturazione mensile viene utilizzato per impostazione predefinita per l'archiviazione di cui viene eseguito il provisioning in data center che non sono aggiornati con funzionalità migliorate.  
5. Selezionare il pulsante di opzione accanto alla dimensione di archiviazione (**Storage Size**) appropriata.
6. Immetti l'IOPS nel campo **Specify IOPS**.
7. Fai clic su **Continue**. Ti vengono mostrati gli addebiti mensili e a base proporzionale con una possibilità finale di riesaminare i dettagli dell'ordine. Fai clic su **Previous** se vuoi modificare il tuo ordine.
8. Fai clic sulla casella di spunta **I have read the Master Service Agreement** e fai clic su **Place Order**.
9. La tua nuova allocazione di archiviazione dovrebbe essere disponibile in pochi minuti.

**Nota**: per impostazione predefinita, puoi eseguire il provisioning di un totale combinato di 250 volumi {{site.data.keyword.filestorage_short}}. Per aumentare il numero dei tuoi volumi, contatta il tuo rappresentante di vendita. Troverai ulteriori informazioni sull'aumento dei limiti [qui](managing-storage-limits.html).

Per il limite di autorizzazioni simultanee, consulta le nostre [Domande frequenti (FAQ)](File-Storage-FAQ.html)

## Come identifico i miei volumi {{site.data.keyword.filestorage_short}} nella mia fattura

Tutti i volumi nella tua fattura si presenteranno come una voce di riga; Endurance si presenterà come “Endurance Storage Service” e Performance si presenterà come "Performance Storage service". la tariffa varierà in base al livello di archiviazione. Puoi espandere Endurance o Performance per appurare che sia un volume {{site.data.keyword.filestorage_short}}.

