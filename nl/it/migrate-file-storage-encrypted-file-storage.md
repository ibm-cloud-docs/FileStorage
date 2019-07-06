---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-18"

keywords: File Storage, file storage, NFS, upgrade, migrate to new

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Migrazione di {{site.data.keyword.filestorage_short}} a {{site.data.keyword.filestorage_short}} avanzato
{: #migratestorage}

Enhanced {{site.data.keyword.filestorage_full}} è ora disponibile nella maggior parte dei [data center](/docs/infrastructure/FileStorage?topic=FileStorage-selectDC).

Il percorso di migrazione preferito consiste nello stabilire una connessione a entrambi i volumi simultaneamente e trasferire i dati direttamente da un volume all'altro. Le specifiche dipendono dal tuo sistema operativo e dalla previsione di possibili modifiche dei dati durante l'operazione di copia.

Si presuppone che tu già abbia il tuo volume non crittografato collegato al tuo host. In caso contrario, attieniti alle indicazioni che meglio rispondono al tuo sistema operativo per eseguire questa attività.

- [Montaggio di {{site.data.keyword.filestorage_short}} su Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [Montaggio di {{site.data.keyword.filestorage_short}} in CentOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [Montaggio di {{site.data.keyword.filestorage_short}} su CoreOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)

Tutti i volumi di {{site.data.keyword.filestorage_short}} avanzato di cui viene eseguito il provisioning in questi data center hanno un punto di montaggio diverso rispetto ai volumi non crittografati. Per assicurarti che stai usando il punto di montaggio corretto per entrambi i volumi di archiviazione, puoi visualizzare le informazioni sul punto di montaggio nella pagina **Volume Details** nella console. Puoi inoltre accedere al punto di montaggio corrente tramite una chiamata API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}


## Creazione di una {{site.data.keyword.filestorage_short}}

Quando effettui un ordine con l'API, specifica il pacchetto "Storage as a Service" per assicurarti che stai ottenendo le funzioni avanzate con la tua nuova archiviazione.
{:important}

Puoi ordinare un volume migliorato tramite il catalogo {{site.data.keyword.cloud}}. Il tuo nuovo volume deve essere di dimensione pari o superiore a quella della condivisione file originale per facilitare la migrazione.

- [Ordinazione di {{site.data.keyword.filestorage_short}} con livelli IOPS predefiniti (Endurance)](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole#endurance)
- [Ordinazione di {{site.data.keyword.filestorage_short}} con IOPS personalizzato (Performance)](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole#performance)

La tua nuova archiviazione è pronta per essere montata in pochi minuti. Puoi visualizzarla nell'elenco risorse e nell'elenco {{site.data.keyword.blockstorageshort}}.


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
  - Se hai dei backup, del contenuto statico ed elementi di cui non sono previste variazioni durante la copia, non ti devi preoccupare.
  - Se stai eseguendo un database o una macchina virtuale sulla tua {{site.data.keyword.filestorage_short}}, assicurati che i dati non vengano modificati durante la copia per evitare un danneggiamento dei dati.
  - Se ha qualche preoccupazione relativa alla larghezza di banda, esegui la migrazione nei periodi non di punta.
  - Se hai bisogno di assistenza con queste considerazioni, apri un ticket di supporto.

3. Copia i tuoi dati.
   - **Microsoft Windows**
     - Per copiare i dati dal tuo volume {{site.data.keyword.filestorage_short}} originale al nuovo volume, formatta la nuova archiviazione e copia i file utilizzando Esplora risorse di Windows.
   - **Linux**
     - Puoi utilizzare `rsync` per copiare i dati.
       ```
       [root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage
       ```

   È una buona idea usare il comando precedente con l'indicatore `--dry-run` una volta per assicurarti che l'allineamento dei percorsi sia corretto. Se questo processo viene interrotto, puoi eliminare l'ultimo file di destinazione di cui era in corso la copia per assicurarti che venga copiato dall'inizio nella nuova ubicazione.

   Dopo che questo comando è stato completato senza l'indicatore `--dry-run`, i tuoi dati vengono copiati nel nuovo volume {{site.data.keyword.filestorage_short}}. Esegui nuovamente il comando per assicurarti che non sia sfuggito niente. Puoi anche riesaminare manualmente entrambe le ubicazioni per cercare eventuali elementi che potrebbero essere sfuggiti.

   Dopo che la tua migrazione è stata completata, puoi trasferire la produzione al nuovo volume. Puoi quindi scollegare ed eliminare il tuo volume originale dalla tua configurazione. L'eliminazione rimuove anche le eventuali istantanee o repliche sul sito di destinazione che era associato al volume originale.
