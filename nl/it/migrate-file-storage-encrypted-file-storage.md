---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-31"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Migrazione di {{site.data.keyword.filestorage_short}} a {{site.data.keyword.filestorage_short}} avanzato

{{site.data.keyword.filestorage_full}} è ora disponibile in data center selezionati. Per visualizzare l'elenco dei data center di cui è stato eseguito l'upgrade e delle funzioni disponibili come ad esempio le frequenze dell'IOPS regolabile e i volumi espandibili, fai clic [qui](new-ibm-block-and-file-storage-location-and-features.html). Per ulteriori informazioni sull'archiviazione crittografata gestita dal provider, consulta [Crittografia dei dati inattivi di {{site.data.keyword.filestorage_short}}](block-file-storage-encryption-rest.html).

Il percorso di migrazione preferito consiste nello stabilire una connessione a entrambi i volumi simultaneamente e trasferire i dati direttamente da un LUN all'altro. Le specifiche dipendono dal tuo sistema operativo e dalla previsione di possibili modifiche dei dati durante l'operazione di copia.

Si presuppone che tu già abbia il tuo LUN non crittografato collegato al tuo host. In caso contrario, attieniti alle indicazioni che meglio rispondono al tuo sistema operativo per eseguire questa attività.

- [Montaggio dell'{{site.data.keyword.filestorage_short}} su Linux](accessing-file-storage-linux.html)
- [Montaggio di NFS/{{site.data.keyword.filestorage_short}} in CentOS](mounting-nsf-file-storage.html)
- [Montaggio dell'{{site.data.keyword.filestorage_short}} su CoreOS](mounting-storage-coreos.html)

Tutti i volumi {{site.data.keyword.filestorage_short}} avanzati hanno un punto di montaggio diverso rispetto ai volumi non crittografati. Per assicurarti che stai usando il punto di montaggio corretto per entrambi i tuoi volumi {{site.data.keyword.filestorage_short}} crittografato e non crittografato, puoi visualizzare le informazioni sul punto di montaggio nella pagina **Volume Details** nel {{site.data.keyword.slportal}}. Puoi inoltre accedere al punto di montaggio corrente tramite una chiamata API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}


## Creazione di un nuovo {{site.data.keyword.filestorage_short}}

Quando effettui un ordine con l'API, specifica il pacchetto "Storage as a Service" per assicurarti che stai ottenendo le funzioni avanzate con la tua nuova archiviazione.
{:important}

Le seguenti istruzioni sono per ordinare un volume/una condivisione file avanzati tramite il catalogo /{{site.data.keyword.BluSoftlayer_full}} del {{site.data.keyword.slportal}}. Il tuo nuovo volume deve essere di dimensione pari o superiore a quella del volume originale per facilitare la migrazione.

### Ordinazione di un nuovo volume di archiviazione Endurance

1. Dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, fai clic su **Storage** > **{{site.data.keyword.filestorage_short}}** OPPURE, dal catalogo di {{site.data.keyword.BluSoftlayer_full}}, fai clic su **Infrastructure** > **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Fai clic su **Order{{site.data.keyword.filestorage_short}}**.
3. Seleziona **Endurance** dall'elenco **Select Storage Type**.
4. Fai clic su **Location** e seleziona il tuo data center.
   - Assicurati che la nuova archiviazione venga aggiunta nella stessa ubicazione dell'originale.
5. Seleziona la tua opzione di fatturazione. Puoi scegliere tra fatturazione mensile o oraria.
6. Fai clic su **Endurance** e seleziona il livello IOPS.
6. Seleziona **Usable Storage Size** dall'elenco. Il tuo nuovo volume deve essere di dimensione pari o superiore a quella del volume originale.
7. Scegli la dimensione dello spazio per le istantanee (**Snapshot Space Size**) (oltre al tuo spazio utilizzabile) dall'elenco.
8. Fai clic su **Continue**. Ti vengono mostrati gli addebiti mensili e a base proporzionale con una possibilità finale di riesaminare i dettagli dell'ordine. Fai clic su **Previous** se vuoi modificare il tuo ordine.
9. Fai clic sulla casella di spunta **I have read the Master Service Agreement** e fai clic su **Place Order**

### Ordinazione di un volume di archiviazione Performance crittografato

1. Dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, fai clic su **Storage**, **{{site.data.keyword.filestorage_short}}** OPPURE, dal catalogo di {{site.data.keyword.BluSoftlayer_full}}, fai clic su **Infrastructure** >** Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Fai clic su **Order{{site.data.keyword.filestorage_short}}**.
3. Seleziona **Performance** dall'elenco **Select Storage Type**.
4. Fai clic su **Location** e seleziona il tuo data center.
    -  Assicurati che la nuova archiviazione venga aggiunta nella stessa ubicazione dell'originale.
5. Seleziona le tue opzioni di fatturazione. Puoi scegliere tra fatturazione oraria e mensile.
6. Selezionare il pulsante di opzione accanto alla dimensione di archiviazione (**Storage Size**) appropriata.
6. Immetti l'IOPS nel campo **Specify IOPS**.
7. Fai clic su **Continue**. Ti vengono mostrati gli addebiti mensili e a base proporzionale con una possibilità finale di riesaminare i dettagli dell'ordine. Fai clic su **Previous** se vuoi modificare il tuo ordine.
8. Fai clic sulla casella di spunta **I have read the Master Service Agreement** e fai clic su **Place Order**.

Il provisioning dell'archiviazione viene eseguito in meno di un minuto ed è visibile sulla pagina {{site.data.keyword.filestorage_short}} del {{site.data.keyword.slportal}}.


## Autorizzazione dell'host al nuovo {{site.data.keyword.filestorage_short}}

Gli host "autorizzati" sono host a cui è stato concesso l'accesso a un volume. Senza l'autorizzazione host, non puoi accedere all'archiviazione dal tuo sistema o utilizzarla.

1. Fai clic sul nome del nuovo volume.
2. Scorri alla sezione **Authorized Hosts**.
3. Fai clic sul link **Authorize Host** sulla destra. Seleziona gli host che possono accedere al volume.

Quando l'host è autorizzato, collega il volume al tuo host.


## Configurazione di istantanee e replica

Se le istantanee e la replica erano state stabilite per il tuo volume originale, devi configurarle per il nuovo volume. Configura lo spazio di replica e delle istantanee e crea le pianificazioni delle istantanee con le stesse impostazioni del volume originale.

Se il tuo data center di destinazione non ha la crittografia, non puoi stabilire la replica per il nuovo volume finché non viene eseguito l'upgrade di tale data center.
{:important}


## Migrazione dei tuoi dati

1. Stabilisci una connessione a entrambi i tuoi volumi {{site.data.keyword.filestorage_short}}, quello originale e quello nuovo.
  - Se si ha bisogno di assistenza per collegare le due condivisioni file al tuo host, apri un ticket di supporto.

2. Considera quale tipo di dati hai sul tuo volume {{site.data.keyword.filestorage_short}} originale e qual è il modo migliore per copiarli nella tua nuova condivisione file.
  - Se hai dei backup, del contenuto statico ed elementi di cui non sono previste variazioni durante la copia, non ci sono preoccupazioni di particolare importanza.
  - Se stai eseguendo un database o una macchina virtuale sulla tua {{site.data.keyword.filestorage_short}}, assicurati che i dati non vengano modificati durante la copia per evitare un danneggiamento dei dati. Se ha qualche preoccupazione relativa alla larghezza di banda, esegui la migrazione nei periodi non di punta. Se hai bisogno di assistenza con queste considerazioni, apri un ticket di supporto.

3. Copia i tuoi dati.
   - **Microsoft Windows**
     - Per copiare i dati dal tuo LUN {{site.data.keyword.filestorage_short}} originale al nuovo LUN, formatta la nuova archiviazione e copia i file utilizzando Esplora risorse di Windows.
   - **Linux**
     - Puoi utilizzare `rsync` per copiare i dati.
       ```
       [root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage
       ```

   È una buona idea usare il comando precedente con l'indicatore `--dry-run` una volta per assicurarti che l'allineamento dei percorsi sia corretto. Se questo processo viene interrotto, puoi eliminare l'ultimo file di destinazione di cui era in corso la copia per assicurarti che venga copiato dall'inizio nella nuova ubicazione.

   Dopo che questo comando è stato completato senza l'indicatore `--dry-run`, i tuoi dati vengono copiati nel nuovo volume {{site.data.keyword.filestorage_short}}. Esegui nuovamente il comando per assicurarti che non sia sfuggito niente. Puoi anche riesaminare manualmente entrambe le ubicazioni per cercare eventuali elementi che potrebbero essere sfuggiti.

   Dopo che la tua migrazione è stata completata, puoi trasferire la produzione al nuovo LUN. Puoi quindi scollegare ed eliminare il tuo volume originale dalla tua configurazione. L'eliminazione rimuove anche le eventuali istantanee o repliche sul sito di destinazione che era associato al volume originale.
