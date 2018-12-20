---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Creazione di un {{site.data.keyword.filestorage_short}} duplicato

Puoi creare un duplicato di un {{site.data.keyword.BluSoftlayer_full}} {{site.data.keyword.filestorage_full}} esistente. Il volume duplicato eredita per impostazione predefinita le opzioni di capacità e prestazioni del volume originale e ha una copia dei dati che arriva fino al punto temporale di un'istantanea.   

Poiché il duplicato è basato sui dati in un'istantanea a un punto temporale, è necessario lo spazio per le istantanee sul volume originale prima che tu possa creare un duplicato. Per ulteriori informazioni sulle istantanee e su come ordinare lo spazio per le istantanee, consulta la [documentazione delle istantanee](snapshots.html).  

I duplicati possono essere creati sia dai volumi **primari** che da quelli di **replica**. Il nuovo duplicato viene creato nello stesso data center del volume originale. Se crei un duplicato da un volume di replica, il nuovo volume viene creato nello stesso data center del volume di replica.

Se sei un utente di un account dedicato di {{site.data.keyword.containerlong}}, consulta le tue opzioni per la duplicazione di un volume nella [Documentazione {{site.data.keyword.containerlong_notm}}](/docs/containers/cs_storage_file.html#backup_restore).
{:tip}

I volumi duplicati solo accessibili da un host per la lettura/scrittura non appena viene seguito il provisioning dell'archiviazione. Tuttavia, le istantanee e le repliche sono consentite solo dopo il completamento della copia dei dati dall'originale al duplicato. Una volta completata la copia dei dati, il duplicato può essere gestito e utilizzato come un volume indipendente.

Questa funzione è disponibile nella maggior parte delle ubicazioni. Fai clic [qui](new-ibm-block-and-file-storage-location-and-features.html) per l'elenco dei data center disponibili.

Alcuni usi comuni per un volume duplicato includono i seguenti esempi.
- **Esecuzione di test del ripristino di emergenza**. Crea un duplicato del tuo volume di copia per verificare che i dati siano intatti e che possano essere utilizzati se si verifica un'emergenza, senza interrompere la replica.
- **Copia di versione finale**. Usa un volume di archiviazione come copia di versione finale da cui puoi creare più istanze per vari usi.
- **Aggiornamenti dei dati**. Crea una copia dei tuoi dati di produzione da montare al tuo ambiente non di produzione per l'esecuzione di test.
- **Ripristino da un'istantanea**. Ripristina i dati sul volume originale con file e data specifici da un'istantanea senza sovrascrivere l'intero volume originale con la funzione di ripristino di istantanea.
- **Sviluppo e test (dev/test)**. Crea fino a quattro duplicati simultanei per volta di un volume per creare i dati duplicati per attività di sviluppo e test.
- **Modifica delle dimensioni dell'archiviazione**. Crea un volume con la nuova dimensione, il nuovo tasso di IOPS o entrambe le cose senza dover spostare i tuoi dati.  

Puoi creare un volume duplicato tramite il [{{site.data.keyword.slportal}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/){:new_window} in un paio di modi.


## Creazione di un duplicato da un volume specifico nell'elenco archiviazioni

1. Vai al tuo elenco di {{site.data.keyword.filestorage_short}}
    - Dal portale clienti, fai clic su **Storage** > **{{site.data.keyword.filestorage_short}}** OPPURE
    - Dal catalogo {{site.data.keyword.BluSoftlayer_full}}, fai clic su **Infrastruttura** > **Archiviazione** > **{{site.data.keyword.filestorage_short}}**.
2. Seleziona un LUN dall'elenco e fai clic su **Actions** > **Duplicate LUN (Volume)**
3. Scegli la tua opzione di istantanea.
    - Se ordini da un volume non di replica,
      - Seleziona **Create from new snapshot** – questa azione crea un'istantanea da utilizzare per il duplicato. Utilizza questa opzione se il tuo volume non ha delle istantanee correnti o se vuoi creare un duplicato in questo momento.</br>
      - Seleziona **Create from latest snapshot** - questa azione crea un duplicato dall'istantanea più recente che esiste per questo volume.
    - Se ordini da un volume di replica, la sola opzione per l'istantanea consiste nell'utilizzare l'istantanea più recente disponibile.
4. Il tipo di archiviazione e l'ubicazione rimangono gli stessi del volume originale.
5. Fatturazione oraria o mensile – puoi scegliere di eseguire il provisioning del LUN duplicato con la fatturazione oraria o mensile. Il tipo di fatturazione per il volume originale viene automaticamente selezionato. Se vuoi scegliere un tipo di fatturazione differente per la tua archiviazione duplicata, puoi operare tale selezione qui.
5. Volendo, puoi specificare gli IOPS o il livello IOPS per il nuovo volume. La designazione degli IOPS del volume originale è impostata per impostazione predefinita. Vengono visualizzate le combinazioni di Performance e dimensioni disponibili.
    - Se il tuo volume originale è al livello Endurance 0,25 IOPS, non puoi operare una nuova selezione.
    - Se il tuo volume originale è al livello Endurance 2, 4 o 10 IOPS, puoi spostarti dovunque tra questi livelli per il nuovo volume.
6. Puoi aggiornare la dimensione del nuovo volume in modo che sia più grande dell'originale. La dimensione del volume originale è impostata per impostazione predefinita.

   {{site.data.keyword.filestorage_short}} può essere ridimensionato fino a 10 volte la dimensione originale del volume.
   {:tip}
7. Puoi aggiornare lo spazio per le istantanee per il nuovo volume per aggiungere più, meno o zero spazio per le istantanee. Lo spazio per le istantanee del volume originale viene impostato per impostazione predefinita.
8. Fai clic su **Continue** per effettuare il tuo ordine.


## Creazione di un duplicato da una specifica istantanea

1. Vai al tuo elenco di {{site.data.keyword.filestorage_short}}
2. Fai clic su un volume dall'elenco per visualizzare la pagina dei dettagli. Può essere un volume di replica o non di replica.
3. Scorri verso il basso e seleziona un'istantanea esistente dall'elenco nella pagina dei dettagli e fai clic su **Actions** > **Duplicate**.   
4. Il tipo di archiviazione (Endurance o Performance) e l'ubicazione rimangono gli stessi del volume originale.
5. Vengono visualizzate le combinazioni di Performance e dimensioni disponibili. La designazione degli IOPS del volume originale è impostata per impostazione predefinita. Puoi specificare gli IOPS o il livello IOPS per il nuovo volume.
    - Se il tuo volume originale è al livello Endurance 0,25 IOPS, non puoi operare una nuova selezione.
    - Se il tuo volume originale è al livello Endurance 2, 4 o 10 IOPS, puoi spostarti dovunque tra questi livelli per il nuovo volume.
6. Puoi aggiornare la dimensione del nuovo volume in modo che sia più grande dell'originale. La dimensione del volume originale è impostata per impostazione predefinita.

   {{site.data.keyword.filestorage_short}} può essere ridimensionato fino a 10 volte la dimensione originale del volume.
   {:tip}
7. Puoi aggiornare lo spazio per le istantanee per il nuovo volume per aggiungere più, meno o zero spazio per le istantanee. Lo spazio per le istantanee del volume originale viene impostato per impostazione predefinita.
8. Fai clic su **Continue** per effettuare il tuo ordine per il duplicato.


## Gestione del tuo volume duplicato

Mentre i dati vengono copiati dal volume originale al duplicato, vedi un stato nella pagina dei dettagli che mostra che la duplicazione è in corso. Durante questo lasso di tempo, puoi collegarti a un host e leggere/scrivere sul volume ma non puoi creare pianificazioni delle istantanee. Una volta completato il processo di duplicazione, il nuovo volume è indipendente da quello originale e può essere gestito con le istantanee e la replica come normale.
