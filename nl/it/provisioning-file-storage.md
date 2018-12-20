---

copyright:
  years: 2014, 2018
lastupdated: "2018-12-13"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Ordinazione di {{site.data.keyword.filestorage_short}}

Puoi eseguire il provisioning di {{site.data.keyword.filestorage_short}} ed eseguire un'ottimizzazione per soddisfare le tue esigenze di capacità e IOPS. Ottieni il massimo dalla tua archiviazione con due opzioni per specificare le prestazioni.

- Puoi scegliere dai livelli IOPS Endurance che presentano dei livelli di prestazioni predefiniti per adattarsi ai carichi di lavoro che non hanno dei requisiti di prestazioni ben definiti.
- Puoi ottimizzare l'archiviazione per soddisfare requisiti di prestazioni specifici specificando il numero totale di IOPS con Performance.

## Ordinazione di {{site.data.keyword.filestorage_short}} con livelli IOPS predefiniti (Endurance)

1. Accedi al [Catalogo IBM Cloud](https://{DomainName}/catalog/){:new_window} e fai clic su **Archiviazione**. Quindi, seleziona {{site.data.keyword.filestorage_short}}. Fai clic su **Crea**.

   In alternativa, puoi accedere al [{{site.data.keyword.slportal}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/){:new_window} } e fare clic su **Storage** > **{{site.data.keyword.filestorage_short}}**. In alto a destra, fai clic su **Order {{site.data.keyword.filestorage_short}}**.
2. Seleziona l'ubicazione (**Location**) della tua distribuzione (data center).
   - Assicurati che la nuova archiviazione venga aggiunta nella stessa ubicazione dell'host o degli host di calcolo di cui disponi.
3. Fatturazione. Se hai selezionato un data center con funzionalità migliorate (contrassegnato con un asterisco), puoi scegliere tra fatturazione mensile od oraria.
     1. Con la fatturazione **oraria**, il numero di ore per cui il volume di file è esistito nell'account viene calcolato quando il LUN viene eliminato oppure alla fine del ciclo di fatturazione. A seconda di quale di queste condizioni si verifica per prima. La fatturazione oraria è una buona scelta per l'archiviazione utilizzata per qualche giorno o per meno di un mese completo. La fatturazione oraria è disponibile solo per l'archiviazione di cui viene eseguito il provisioning in questi [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html).
     2. Con la fatturazione **mensile**, il calcolo per il prezzo è a base proporzionale dalla data di creazione alla fine del ciclo di fatturazione e viene fatturato immediatamente. Non è previsto alcun rimborso se il volume di file viene eliminato prima della fine del ciclo di fatturazione. La fatturazione mensile è una buona scelta per l'archiviazione utilizzata nei carichi di lavoro di produzione che usano dati che devono essere archiviati e a cui bisogna accedere per lunghi periodi di tempo (mese o più).

     Il tipo di fatturazione mensile viene utilizzato per impostazione predefinita per l'archiviazione di cui viene eseguito il provisioning in data center che **non** sono aggiornati con funzionalità migliorate.
     {:note}
4. Immetti la tua dimensione di archiviazione nel campo **New Storage Size**.
5. Seleziona **Endurance (tiered IOPS)** nella sezione **Storage IOPS Options**.
6. Seleziona il livello IOPS di cui ha bisogno la tua applicazione.
    - **0,25 IOPS per GB** è progettato per carichi di lavori con bassa intensità di I/O. Questi carichi di lavoro sono di norma caratterizzati dall'avere un'ampia percentuale di dati inattivi in un dato momento. Delle applicazioni di esempio includono le caselle di posta di archiviazione o le condizioni di file a livello di reparto.
    - **2 IOPS per GB** è progettato per un utilizzo per finalità più generiche. Delle applicazioni di esempio includono le attività di host di piccoli database a supporto di applicazioni web o le immagini disco di macchina virtuale per un hypervisor.
    - **4 IOPS per GB** è progettato per i carichi di lavoro a maggiore intensità. Questi carichi di lavoro sono di norma caratterizzati dall'avere un'elevata percentuale di dati attivi in un dato momento. Delle applicazioni di esempio includono i database transazionali e altri database sensibili alle prestazioni.
    - **10 IOPS per GB** è progettato per i carichi di lavoro più esigenti quali quelli creati dai database NoSQL e l'elaborazione di dati per l'analisi. Questo livello è disponibile in [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html) per l'archiviazione di cui viene eseguito il provisioning fino a 4 TB.
7. Fai clic su **Specify Snapshot Space Size** e seleziona la dimensione dell'istantanea dall'elenco. Questo spazio è in aggiunta al tuo spazio utilizzabile. Per le considerazioni e i suggerimenti relativi allo spazio per le istantanee, leggi [Ordinazione di istantanee](ordering-snapshots.html).
8. A destra, controlla il tuo riepilogo degli ordini e applica il tuo codice promozionale se ne hai uno.
9. Dopo aver controllato i termini e le condizioni, seleziona la casella **I have read and agree to the Third-Party Service Agreements**.
10. Fai clic su **Create**. La tua nuova allocazione di archiviazione è disponibile in pochi minuti.

Per impostazione predefinita, puoi eseguire il provisioning di un totale combinato di 250 volumi {{site.data.keyword.blockstorageshort}}. Per aumentare il numero dei tuoi volumi, contatta il tuo rappresentante di vendita. Troverai ulteriori informazioni sull'aumento dei limiti [qui](managing-storage-limits.html).<br/><br/>Per ulteriori informazioni sul limite di autorizzazioni simultanee, consulta le [FAQ](faqs.html#how-many-instances-can-share-the-use-of-a-provisioned-file-storage-volume-).
{:tip}

## Ordinamento di {{site.data.keyword.filestorage_short}} con IOPS personalizzato (Performance)

1. Accedi al [Catalogo IBM Cloud](https://{DomainName}/catalog/){:new_window} e fai clic su **Archiviazione**. Quindi, seleziona {{site.data.keyword.filestorage_short}}. Fai clic su **Crea**.

   In alternativa, puoi accedere al [{{site.data.keyword.slportal}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/){:new_window} } e fare clic su **Storage** > **{{site.data.keyword.filestorage_short}}**. In alto a destra, fai clic su **Order {{site.data.keyword.filestorage_short}}**.
2. Fai clic su **Location** e seleziona il tuo data center.
   - Assicurati che la nuova archiviazione venga aggiunta nella stessa ubicazione dell'host o degli host di calcolo di cui disponi.
3. Fatturazione. Se hai selezionato un data center con funzionalità migliorate (contrassegnato con un asterisco), puoi scegliere tra fatturazione mensile od oraria.
     1. Con la fatturazione **oraria**, il numero di ore per cui il volume di file è esistito nell'account viene calcolato quando il LUN viene eliminato oppure alla fine del ciclo di fatturazione. A seconda di quale di queste condizioni si verifica per prima. La fatturazione oraria è una buona scelta per l'archiviazione utilizzata per qualche giorno o per meno di un mese completo. La fatturazione oraria è disponibile solo per l'archiviazione di cui viene eseguito il provisioning in questi [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html).
     2. Con la fatturazione **mensile**, il calcolo per il prezzo è a base proporzionale dalla data di creazione alla fine del ciclo di fatturazione e viene fatturato immediatamente. Non è previsto alcun rimborso se il volume di file viene eliminato prima della fine del ciclo di fatturazione. La fatturazione mensile è una buona scelta per l'archiviazione utilizzata nei carichi di lavoro di produzione che usano dati che devono essere archiviati e a cui bisogna accedere per lunghi periodi di tempo (mese o più).

     Il tipo di fatturazione mensile viene utilizzato per impostazione predefinita per l'archiviazione di cui viene eseguito il provisioning in data center che **non** sono aggiornati con funzionalità migliorate.
     {:note}
4. Immetti la tua dimensione di archiviazione nel campo **New Storage Size**.
5. Seleziona **Performance (Allocated IOPS)** nella sezione **Storage IOPS Options**.
6. Immetti l'IOPS nel campo **Performance (Allocated IOPS)**.
7. Fai clic su **Specify Snapshot Space Size** e seleziona la dimensione dell'istantanea dall'elenco. Questo spazio è in aggiunta al tuo spazio utilizzabile. Per le considerazioni e i suggerimenti relativi allo spazio per le istantanee, leggi [Ordinazione di istantanee](ordering-snapshots.html).
8. A destra, controlla il tuo riepilogo degli ordini e applica il tuo codice promozionale se ne hai uno.
9. Dopo aver controllato i termini e le condizioni, seleziona la casella **I have read and agree to the Third-Party Service Agreements**.
10. Fai clic su **Create**. La tua nuova allocazione di archiviazione è disponibile in pochi minuti.

Per impostazione predefinita, puoi eseguire il provisioning di un totale combinato di 250 volumi {{site.data.keyword.blockstorageshort}}. Per aumentare il numero dei tuoi volumi, contatta il tuo rappresentante di vendita. Troverai ulteriori informazioni sull'aumento dei limiti [qui](managing-storage-limits.html).<br/><br/>Per ulteriori informazioni sul limite di autorizzazioni simultanee, consulta le [FAQ](faqs.html#how-many-instances-can-share-the-use-of-a-provisioned-file-storage-volume-).
{:important}


## Connessione della tua nuova archiviazione

Dopo che la tua richiesta di provisioning è stata completata, autorizza i tuoi host ad accedere alla nuova archiviazione e configura la tua connessione. A seconda del sistema operativo del tuo host, segui il link appropriato.
- [Montaggio di {{site.data.keyword.filestorage_short}} su Linux](accessing-file-storage-linux.html)
- [Montaggio di {{site.data.keyword.filestorage_short}} in CentOS](mounting-nsf-file-storage.html)
- [Montaggio di {{site.data.keyword.filestorage_short}} su CoreOS](mounting-storage-coreos.html)
- [Configurazione di {{site.data.keyword.filestorage_short}} per il backup con cPanel](configure-backup-cpanel.html)
- [Configurazione di {{site.data.keyword.filestorage_short}} per il backup con Plesk](configure-backup-plesk.html)
- [Montaggio del volume {{site.data.keyword.filestorage_short}} sugli host ESXi](architecture-guide-file-storage-vmware.html)

## Considerazioni sul ripristino di emergenza

Per evitare perdite di dati e per garantire la continuità aziendale, prendi in considerazione una replica dei tuoi server e della tua archiviazione in un altro data center. La replica mantiene i tuoi dati sincronizzati in due diverse ubicazioni in base alla tua pianificazione delle istantanee. Per ulteriori informazioni, vedi [Replica dei dati](replication.html).

Se vuoi clonare il tuo volume e usarlo indipendentemente dal volume originale, vedi [Creazione di un volume di blocco duplicato](how-to-create-duplicate-volume.html).

## Identificazione dei volumi {{site.data.keyword.filestorage_short}} nella fattura

Tutte le condivisioni file vengono visualizzate nella tua fattura come un elemento riga. I volumi Endurance si presentano come “Endurance Storage Service” e i volumi Performance si presentano come "Performance Storage Service". La tariffa varia in base al tuo livello di archiviazione. Puoi espandere Endurance o Performance per appurare che sia {{site.data.keyword.filestorage_short}}.
