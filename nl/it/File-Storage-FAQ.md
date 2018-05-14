---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-02"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.filestorage_short}} - Domande frequenti (FAQ)

## Come vengono misurati gli IOPS?

Gli IOPS vengono misurati in base a un profilo di caricamento di blocchi da 16 KB con 50% di letture e 50% di scritture casuali. I carichi di lavoro che differiscono da questo profilo potrebbero riscontrare delle prestazioni inferiori.

## Cosa succede se uso una dimensione blocco più piccola quando misuro le prestazioni?

Quando si utilizzano delle dimensioni blocco più piccole, è comunque possibile ottenere l'IOPS massimo; tuttavia, la velocità effettiva sarà inferiore. Ad esempio, un volume con 6000 IOPS avrà la seguente velocità effettiva alle varie dimensioni blocco:

- 16 KB * 6000 IOPS == ~93,75 MB/sec
- 8 KB * 6000 IOPS == ~46,88 MB/sec
- 4 KB * 6000 IOPS == ~23,44 MB/sec


## Occorre preriscaldare il volume per ottenere la velocità effettiva prevista?

Non è necessario eseguire un preriscaldamento. Osserverai la velocità effettiva specificata non appena verrà eseguito il provisioning del volume.

## Come faccio a distinguere quali dei miei volumi/LUN {{site.data.keyword.filestorage_short}} è crittografato?

Quando visualizzi il tuo elenco di {{site.data.keyword.filestorage_short}} nel Portale del cliente, vedrai un icona di lucchetto a destra dei nomi di LUN/volume, nel caso in cui siano crittografati.

## Se ho un {{site.data.keyword.filestorage_short}} non crittografato di cui è stato eseguito il provisioning in un data center di cui non è stato eseguito l'upgrade per la crittografia, posso crittografare il mio {{site.data.keyword.filestorage_short}}?

Non è possibile crittografare un {{site.data.keyword.filestorage_short}} di cui è stato eseguito il provisioning prima di un upgrade del data center. Il nuovo {{site.data.keyword.filestorage_short}} di cui è stato eseguito il provisioning in data center di cui è stato eseguito l'upgrade viene crittografato automaticamente; non c'è alcuna impostazione di crittografia da cui scegliere, l'operazione è automatica. I dati su un'archiviazione non crittografata in un data center di cui è stato eseguito l'upgrade possono essere crittografati creando un nuovo volume di file e copiando quindi i dati nel nuovo volume o nella nuova condivisione di file crittografati con la migrazione basata sull'host. Consulta [questo articolo](/docs/infrastructure/FileStorage/migrate-file-storage-encrypted-file-storage.html) per le istruzioni su come eseguire la migrazione.

## Come faccio a sapere se sto eseguendo il provisioning di {{site.data.keyword.filestorage_short}} in un data center di cui è stato eseguito l'upgrade?

Quando esegui il provisioning di {{site.data.keyword.filestorage_short}}, tutti i data center di cui è stato eseguito l'upgrade saranno segnalati da un asterisco (`*`) nel modulo dell'ordine e da un'indicazione che ti avvisa che eseguirai il provisioning dell'archiviazione con la crittografia. Una volta eseguito il provisioning dell'archiviazione, vedrai un'icona nell'elenco archiviazioni che mostra il volume o la LUN come crittografati. Il provisioning di tutte le condivisioni file e di tutti i volumi crittografati viene eseguito solo nei data center di cui è stato eseguito l'upgrade. Puoi trovare un elenco completo dei data center di cui è stato eseguito l'upgrade e delle funzioni disponibili [qui](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html).

## Perché posso eseguire il provisioning di {{site.data.keyword.filestorage_short}} con un livello Endurance 10 IOPS in alcuni data center e non in altri?

Il livello 10 IOPS/GB del tipo Endurance di {{site.data.keyword.filestorage_short}} è disponibile solo in data center selezionati; a tale selezione verranno a breve aggiunti dei nuovi data center. Puoi trovare un elenco completo dei data center di cui è stato eseguito l'upgrade e delle funzioni disponibili [qui](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html).

## Come posso trovare il punto di montaggio corretto per il mio {{site.data.keyword.filestorage_short}}?

Tutti i volumi {{site.data.keyword.filestorage_short}} crittografati di cui viene eseguito il provisioning in questi data center hanno un punto di montaggio diverso rispetto ai volumi non crittografati. Per assicurarti che stai usando il punto di montaggio corretto per entrambi i volumi {{site.data.keyword.filestorage_short}} crittografati e non crittografati, puoi visualizzare le informazioni sul punto di montaggio nella pagina **Volume Details** nell'IU, nonché accedere al punto di montaggio corretto tramite una chiamata API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

## Quante condivisione file sono consentite in base alla dimensione del volume di file? Quali sono le dimensioni file massime consentite in base alla dimensione del volume?
È qui di seguito indicato il numero massimo di inode o condivisione file consentito sulla base della dimensione del volume:

<table>
        <tbody>
          <tr>
            <th>Dimensione del volume</th>
            <th>Inode/file</th>
          </tr>
          <tr>
            <td>20 GB </td>
            <td>622.484</td>
          </tr>
          <tr>
            <td>40 GB </td>
            <td>1.245.084</td>
          </tr>          
          <tr>
            <td>80 GB</td>
            <td>2.490.263</td>
          </tr>          
          <tr>
            <td>100GB</td>
            <td>3.112.863</td>
          </tr>          
          <tr>
            <td>250 GB</td>
            <td>7.782.300</td>
          </tr>          
          <tr>
            <td>500 GB</td>
            <td>15.564.695</td>
          </tr>
          <tr>
            <td>1 TB+</td>
            <td>31.876.593</td>
          </tr>
        </tbody>
</table>

## Di quanti volumi posso eseguire il provisioning?

Per impostazione predefinita, puoi eseguire il provisioning di un totale combinato di 250 volumi di archiviazione blocchi e file. Per aumentare i tuoi volumi, ti invitiamo a contattare il tuo rappresentante di vendita.

## Quante istanze possono condividere l'uso di un volume {{site.data.keyword.filestorage_short}} di cui è stato eseguito il provisioning?

Il limite predefinito per il numero di autorizzazioni per volume di file è 64. Per aumentare il limite ti invitiamo a contattare il tuo rappresentante di vendita.

## Quando si esegue il provisioning di {{site.data.keyword.filestorage_short}}, Performance o Endurance, l'IOPS allocato viene implementato in base all'istanza o in base al volume?

Gli IOPS vengono implementati a livello dei volumi. In altre parole, due host connessi a un volume con 6000 IOPS condividono questi 6000 IOPS.

## Sarò in grado di raggiungere una velocità effettiva maggiore se uso una connessione Ethernet più rapida?

I limiti di velocità effettiva sono impostati su un livello per volume/LUN; pertanto, l'utilizzo di una connessione Ethernet più veloce non aumenterà tale limite impostato. Tuttavia, con una connessione Ethernet più lenta, la tua larghezza di banda può essere un potenziale collo di bottiglia.

## I firewall/gruppi di sicurezza hanno ripercussioni sulle prestazioni?

Come prassi ottimale, consigliamo di eseguire il traffico di archiviazione su una VLAN che ignora il firewall. L'esecuzione del traffico di archiviazione tramite i firewall software aumenterà la latenza e avrà un impatto negativo sulle prestazioni dell'archiviazione.

## Cosa succede ai miei dati quando i volumi {{site.data.keyword.filestorage_short}} vengono eliminati?

Quando l'archiviazione viene eliminata, gli eventuali puntatori ai dati su tale volume vengono rimossi e, pertanto, i dati diventano completamente inaccessibili. Se viene eseguito nuovamente il provisioning dell'archiviazione fisica a un altro account, viene assegnato un nuovo set di puntatori. Non vi è alcuna possibilità che il nuovo account acceda ad eventuali dati che potrebbero essere stati sull'archiviazione fisica; il nuovo set di puntatori mostra tutti 0. Quando vengono scritti i nuovi dati nel volume/LUN, tutti i dati non accessibili che ancora esistono vengono sovrascritti. 

## Che latenza delle prestazioni mi posso aspettare dal mio {{site.data.keyword.filestorage_short}}?   

La latenza di destinazione nell'archiviazione è di <1ms. La nostra archiviazione è connessa alle istanze di elaborazione su una rete condivisa e, pertanto, la latenza delle prestazioni esatta dipenderà dal traffico di rete entro uno specifico periodo di tempo.

