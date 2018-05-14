---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
 
# Migrazione di {{site.data.keyword.filestorage_short}} a {{site.data.keyword.filestorage_short}} crittografato

Il {{site.data.keyword.filestorage_full}} crittografato per Endurance o Performance è stato avviato in data center selezionati. Qui di seguito troverai le informazioni su come migrare il tuo {{site.data.keyword.filestorage_short}} da non crittografato a crittografato. Per ulteriori informazioni sull'archiviazione crittografata gestita dal provider, leggi l'articolo sulla [crittografia dei dati inattivi di{{site.data.keyword.filestorage_short}}](block-file-storage-encryption-rest.html). Per visualizzare un elenco dei data center di cui è stato eseguito l'upgrade e delle funzioni disponibili, fai clic [qui](new-ibm-block-and-file-storage-location-and-features).

Il percorso di migrazione preferito consiste nello stabilire una connessione a entrambi i volumi simultaneamente e trasferire i file direttamente da un volume di file all'altro. Le specifiche dipenderanno dal tuo sistema operativo e dalla previsione di possibili modifiche dei dati durante l'operazione di copia.

Per tua comodità, sono stati indicati gli scenari più comuni. Si presuppone che tu già abbia il tuo volume di file non crittografato collegato al tuo host. In caso contrario, per svolgere tale compito, attieniti alle indicazioni qui di seguito che meglio rispondono al sistema operativo che stai eseguendo. 

**NOTA:** tutti i volumi {{site.data.keyword.filestorage_short}} crittografati hanno un punto di montaggio diverso rispetto ai volumi non crittografati. Per assicurarti che stai usando il punto di montaggio corretto per entrambi i volumi {{site.data.keyword.filestorage_short}} crittografati e non crittografati, puoi visualizzare le informazioni sul punto di montaggio nella pagina **Volume Details** nell'IU, nonché accedere al punto di montaggio corretto tramite una chiamata API: SoftLayer_Network_Storage::getNetworkMountAddress().

[Accesso a {{site.data.keyword.filestorage_short}} su Linux](accessing-file-storage-linux.html)

## Crea un volume di file crittografato

Attieniti alla seguente procedura per creare un volume, di dimensione pari o superiori, che sia crittografato per facilitare il processo di migrazione.

### Ordina un volume di archiviazione Endurance crittografato

1. Fai clic su **Storage** > **{{site.data.keyword.filestorage_short}}** dalla home page di [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} OPPURE fai clic su **Infrastructure** > **Storage** > **{{site.data.keyword.filestorage_short}}** nel catalogo di {{site.data.keyword.BluSoftlayer_full}}.

2. Fai clic sul link **Order {{site.data.keyword.filestorage_short}}** nella pagina {{site.data.keyword.filestorage_short}}.

3. Seleziona **Endurance**.

4. Seleziona il data center dove si trova il tuo volume originale. Nota: la crittografia è disponibile solo nei data center con un asterisco.

5. Immetti il **livello IOPS** desiderato.

6. Seleziona la quantità desiderata di spazio di archiviazione in GB. Per TB, 1 TB equivale a 1.000 GB e 12 TB equivalgono a 12.000 GB.

7. Immetti la quantità desiderata di spazio di archiviazione in GB per le istantanee.

8. Seleziona il **sistema operativo VMware** dall'elenco a discesa.

9. Invia l'ordine.
 
### Ordina un volume di archiviazione Performance crittografato

1. Fai clic su **Storage** > **{{site.data.keyword.filestorage_short}}** dalla home page di [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} OPPURE fai clic su **Infrastructure** > **Storage** > **{{site.data.keyword.filestorage_short}}** nel catalogo di {{site.data.keyword.BluSoftlayer_full}}.

2. Fai clic su **Order {{site.data.keyword.filestorage_short}}**.

3. Seleziona **Performance**.

4. Seleziona il data center dove si trova il tuo volume originale. Nota: la crittografia è disponibile solo nei data center con un asterisco (`*`).

5. Seleziona la quantità desiderata di spazio di archiviazione in GB della stessa dimensione del volume originale o più grande.

6. Immetti la quantità desiderata di IOPS in intervalli di 100.

7. Seleziona il sistema operativo VMware dall'elenco a discesa.

8. Invia l'ordine.

Il provisioning dell'archiviazione verrà eseguito in meno di un minuto e sarà visibile sulla pagina {{site.data.keyword.filestorage_short}} del Portale del cliente.

 
## Connetti un nuovo volume all'host

Gli host "autorizzati" sono host a cui sono stati concessi i diritti di accesso a un volume. Senza l'autorizzazione host, non potrai accedere all'archiviazione dal tuo sistema né utilizzarla.

1. Fai clic sul tuo nome volume (**Volume Name**) crittografato.

2. Scorri alla sezione **Authorized Hosts** della pagina.

3. Fai clic sul link **Authorize Host** sul lato destro della pagina. Seleziona gli host che possono accedere al volume.

Dopo che ne è stata eseguita l'autorizzazione, connetti il volume al tuo host.

 
## Istantanee e replica

Hai delle istantanee e una replica stabilite per il tuo volume originale? In caso affermativo, dovrai configurare lo spazio di replica e istantanea e creare delle pianificazioni delle istantanee per il nuovo volume crittografato con le stesse impostazioni del volume originale. 

Nota: se del tuo data center di destinazione non è stato eseguito l'upgrade per la crittografia, non sarai in grado di stabilire la replica per il nuovo volume finché non sarà stato eseguito l'upgrade del data center.

 
## Migra i tuoi dati

Il tuo host dovrebbe essere connesso sia al volume di {{site.data.keyword.filestorage_short}} originale che a quello crittografato. In caso negativo:

• Assicurati di esserti attenuto alla procedura sopra indicata e di aver fatto riferimento ai documenti correttamente.

• Apri un ticket di supporto per ulteriore assistenza nella connessione dei due volumi al tuo host.

### Considerazioni sui dati

A questo punto, considera quale tipo di dati hai sul tuo volume {{site.data.keyword.filestorage_short}} originale e qual è il modo migliore per copiarli nel tuo volume crittografato. Se hai dei backup, del contenuto statico ed elementi di cui non sono previste variazioni durante la copia, non ci sono considerazioni di particolare importanza.

Se stai eseguendo un database o una macchina virtuale sulla tua {{site.data.keyword.filestorage_short}}, assicurati che i dati nel volume originale non vengano modificati durante la copia in modo che non si verifichi alcun danneggiamento. Se ha qualche preoccupazione relativa alla larghezza di banda, devi eseguire la migrazione nei periodi non di punta. Se hai bisogno di assistenza con queste considerazioni, non esitare ad aprire un ticket di supporto.

### Microsoft Windows

Per copiare dati dal tuo volume di {{site.data.keyword.filestorage_short}} originale al tuo volume crittografato, formatta la nuova archiviazione e copiaci i file utilizzando Esplora risorse di Windows.

### Linux

Puoi considerare l'utilizzo di rsync per copiare i dati. Il seguente è un comando di esempio

`[root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage` 

Ti consigliamo di usare il comando sopra indicato con l'indicatore `--dry-run` una volta per assicurarti che l'allineamento dei percorsi sia corretto. Se questo processo viene interrotto, sarebbe opportuno che eliminassi l'ultimo file di destinazione di cui era in corso la copia per assicurarti che venga copiato dall'inizio alla nuova ubicazione.

Dopo che questo comando è stato completato senza l'indicatore `--dry-run`, i tuoi dati dovrebbero essere stati copiati sul volume {{site.data.keyword.filestorage_short}} crittografato. Devi scorrere verso l'alto ed eseguire nuovamente il comando per assicurarti che non sia sfuggito niente. Sarebbe anche opportuno riesaminare manualmente entrambe le ubicazioni per cercare eventuali elementi che potrebbero essere sfuggiti.

Una volta completata la migrazione, sarai in grado di trasferire la produzione al volume crittografato e scollegare ed eliminare il tuo volume originale dalla tua configurazione. Nota: l'eliminazione rimuoverà anche le eventuali istantanee o repliche sul sito di destinazione che era associato al volume originale.
