---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
 
# Migrazione di {{site.data.keyword.filestorage_short}} a {{site.data.keyword.filestorage_short}} avanzato

{{site.data.keyword.filestorage_full}} è ora disponibile in data center selezionati. Per visualizzare l'elenco dei data center di cui è stato eseguito l'upgrade e delle funzioni disponibili come ad esempio le frequenze dell'IOPS regolabile e i volumi espandibili, fai clic [qui](new-ibm-block-and-file-storage-location-and-features.html). Per ulteriori informazioni sull'archiviazione crittografata gestita dal provider, leggi l'articolo sulla [crittografia dei dati inattivi di {{site.data.keyword.filestorage_short}}](block-file-storage-encryption-rest.html). 

Il percorso di migrazione preferito consiste nello stabilire una connessione a entrambi i LUN simultaneamente e trasferire i dati direttamente da un LUN all'altro. Le specifiche dipendono dal tuo sistema operativo e dalla previsione di possibili modifiche dei dati durante l'operazione di copia.  

Si presuppone che tu già abbia il tuo LUN non crittografato collegato al tuo host. In caso contrario, attieniti alle indicazioni che meglio rispondono al sistema operativo che stai eseguendo: 

- [Montaggio dell'{{site.data.keyword.filestorage_short}} su Linux](accessing-file-storage-linux.html)
- [Montaggio di NFS/{{site.data.keyword.filestorage_short}} in CentOS](mounting-nsf-file-storage.html)
- [Montaggio dell'{{site.data.keyword.filestorage_short}} su CoreOS](mounting-storage-coreos.html)

**NOTA:** tutti i volumi {{site.data.keyword.filestorage_short}} avanzati hanno un punto di montaggio diverso rispetto ai volumi non crittografati. Per assicurarti che stai usando il punto di montaggio corretto per entrambi i tuoi volumi {{site.data.keyword.filestorage_short}} crittografato e non crittografato, puoi visualizzare le informazioni sul punto di montaggio nella pagina **Volume Details** nell'IU. Puoi inoltre accedere al punto di montaggio corrente tramite una chiamata API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.


## Crea un nuovo {{site.data.keyword.filestorage_short}}

**IMPORTANTE**: quando inserisci un ordine con l'API, specifica il pacchetto "Storage as a Service" per assicurarti di stare richiamando le funzioni avanzate con la tua nuova archiviazione.

Le seguenti istruzioni sono per ordinare un volume/condivisione file avanzato tramite l'IU. Il tuo nuovo volume dovrebbe essere della stessa dimensione o maggiore del volume originale per facilitare la migrazione.

### Ordina un nuovo volume di archiviazione Endurance

1. Dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, fai clic su **Storage** > **{{site.data.keyword.filestorage_short}}** OPPURE, dal catalogo di {{site.data.keyword.BluSoftlayer_full}}, fai clic su **Infrastructure** > **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Fai clic su **Order {{site.data.keyword.filestorage_short}}** nell'angolo in alto a destra.  
3. Seleziona **Endurance** dall'elenco **Select Storage Type**.
4. Fai clic su **Location** e seleziona il tuo data center.
   - Assicurati che la nuova archiviazione sarà aggiunta nella stessa ubicazione dell'originale. 
5. Seleziona la tua opzione di fatturazione. Puoi scegliere tra fatturazione mensile o oraria. 
6. Fai clic su **Endurance** e seleziona il livello IOPS.
6. Seleziona **Usable Storage Size** dall'elenco. Il tuo nuovo volume dovrebbe essere della stessa dimensione o maggiore del volume originale. 
7. Scegli la dimensione dello spazio per le istantanee (**Snapshot Space Size**) (oltre al tuo spazio utilizzabile) dall'elenco a discesa.
8. Fai clic su **Continue**. Ti vengono mostrati gli addebiti mensili e a base proporzionale con una possibilità finale di riesaminare i dettagli dell'ordine. Fai clic su **Previous** se vuoi modificare il tuo ordine.
9. Fai clic sulla casella di spunta **I have read the Master Service Agreement** e fai clic su **Place Order**
 
### Ordina un volume di archiviazione Performance crittografato

1. Dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, fai clic su **Storage**, **{{site.data.keyword.filestorage_short}}** OPPURE, dal catalogo di {{site.data.keyword.BluSoftlayer_full}}, fai clic su **Infrastructure** >** Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Fai clic su **Order {{site.data.keyword.filestorage_short}}** nell'angolo in alto a destra.  
3. Seleziona **Performance** dall'elenco Select Storage Type.
4. Fai clic su **Location** e seleziona il tuo data center.
    -  Assicurati che la nuova archiviazione sarà aggiunta nella stessa ubicazione dell'originale. 
5. Seleziona le tue opzioni di fatturazione. Puoi scegliere tra fatturazione oraria e mensile. 
6. Selezionare il pulsante di opzione accanto alla dimensione di archiviazione (**Storage Size**) appropriata.
6. Immetti l'IOPS nel campo **Specify IOPS**.
7. Fai clic su **Continue**. Ti vengono mostrati gli addebiti mensili e a base proporzionale con una possibilità finale di riesaminare i dettagli dell'ordine. Fai clic su **Previous** se vuoi modificare il tuo ordine.
8. Fai clic sulla casella di spunta **I have read the Master Service Agreement** e fai clic su **Place Order**. 

Il provisioning dell'archiviazione verrà eseguito in meno di un minuto e sarà visibile sulla pagina {{site.data.keyword.filestorage_short}} del {{site.data.keyword.slportal}}.

 
## Connetti il nuovo {{site.data.keyword.filestorage_short}} all'host

Gli host "autorizzati" sono host a cui sono stati concessi i diritti di accesso a un volume. Senza l'autorizzazione host, non potrai accedere all'archiviazione dal tuo sistema né utilizzarla. 

1. Fai clic sul nome del nuovo volume. 
2. Scorri alla sezione **Authorized Hosts** della pagina.
3. Fai clic sul link **Authorize Host** sul lato destro della pagina. Seleziona gli host che possono accedere al volume.

Dopo che ne è stata eseguita l'autorizzazione, connetti il volume al tuo host. 

 
## Istantanee e replica

Hai delle istantanee e una replica stabilite per il tuo volume originale? In caso affermativo, dovrai configurare lo spazio di replica e istantanea e creare delle pianificazioni delle istantanee per il nuovo volume crittografato con le stesse impostazioni del volume originale.  

**Nota**: se del tuo data center di destinazione non è stato eseguito l'upgrade per la crittografia, non sarai in grado di stabilire la replica per il nuovo volume finché non sarà stato eseguito l'upgrade del data center.

 
## Migra i tuoi dati

Il tuo host dovrebbe essere connesso sia al volume di {{site.data.keyword.filestorage_short}} originale che a quello crittografato. In caso negativo:

- Assicurati di esserti attenuto alla procedura indicata in questa documentazione e di aver fatto riferimento ai documenti correttamente. 
- Apri un ticket di supporto per ulteriore assistenza nella connessione dei due volumi al tuo host. 

### Considerazioni sui dati

A questo punto, considera quale tipo di dati hai sul tuo volume {{site.data.keyword.filestorage_short}} originale e qual è il modo migliore per copiarli nel tuo volume crittografato. Se hai dei backup, del contenuto statico ed elementi di cui non sono previste variazioni durante la copia, non ci sono considerazioni di particolare importanza.

Se stai eseguendo un database o una macchina virtuale sulla tua {{site.data.keyword.filestorage_short}}, assicurati che i dati nel volume originale non vengano modificati durante la copia in modo che non si verifichi alcun danneggiamento. Se hai qualche preoccupazione relativa alla larghezza di banda, devi eseguire la migrazione nei periodi non di punta. Se hai bisogno di assistenza con queste considerazioni, apri un ticket di supporto. 

### Microsoft Windows

Per copiare dati dal tuo volume di {{site.data.keyword.filestorage_short}} originale al tuo volume crittografato, formatta la nuova archiviazione e copiaci i file utilizzando Esplora risorse di Windows.

### Linux

Puoi considerare l'utilizzo di `rsync` per copiare i dati. Questo è un comando di esempio:

```
[root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage
```

Ti consigliamo di usare il comando di esempio con l'indicatore `--dry-run` una volta per assicurarti che l'allineamento dei percorsi sia corretto. Se questo processo viene interrotto, sarebbe opportuno che eliminassi l'ultimo file di destinazione di cui era in corso la copia per assicurarti che venga copiato dall'inizio alla nuova ubicazione. 

Dopo che questo comando è stato completato senza l'indicatore `--dry-run`, i tuoi dati dovrebbero essere stati copiati sul volume {{site.data.keyword.filestorage_short}} crittografato. Devi scorrere verso l'alto ed eseguire nuovamente il comando per assicurarti che non sia sfuggito niente. Sarebbe anche opportuno riesaminare manualmente entrambe le ubicazioni per cercare eventuali elementi che potrebbero essere sfuggiti.

Una volta completata la migrazione, sarai in grado di trasferire la produzione al volume crittografato e scollegare ed eliminare il tuo volume originale dalla tua configurazione.  

**Nota**: l'eliminazione rimuoverà anche le eventuali istantanee o repliche sul sito di destinazione che era associato al volume originale.
