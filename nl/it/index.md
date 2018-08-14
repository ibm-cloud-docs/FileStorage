---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-29"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Introduzione a {{site.data.keyword.filestorage_short}}

{{site.data.keyword.filestorage_full}} è {{site.data.keyword.filestorage_short}} persistente, veloce, flessibile, basato su NFS e collegato alla rete. In questo ambiente NAS (Network Attached Storage), hai il controllo totale sulle prestazioni e la funzione di condivisioni dei tuoi file. Le condivisioni di {{site.data.keyword.filestorage_short}} possono essere connesse a un massimo di 64 dispositivi autorizzati su connessioni TCP/IP instradate per la resilienza.

{{site.data.keyword.filestorage_short}} offre i livelli migliori del settore di durabilità e disponibilità con un insieme di funzioni senza paragone. {{site.data.keyword.filestorage_short}} è sviluppato utilizzando gli standard del settore e le prassi ottimali e progettato per proteggere l'integrità dei dati. {{site.data.keyword.filestorage_short}} mantiene la disponibilità durante gli eventi di manutenzione e i malfunzionamenti non pianificati e fornisce una baseline di prestazioni congruente.

Avvaliti delle seguenti funzioni principali di {{site.data.keyword.filestorage_short}}:

- **Baseline di prestazioni congruente**
   - Fornita tramite l'allocazione di IOPS (input/output operations per second) a livello di protocollo ai singoli volumi.
- **{{site.data.keyword.filestorage_short}}**
   - Disponibile per le condivisioni NFS basate su file.
- **Altamente durevole e resiliente**
   - Protegge l'integrità dei dati e mantiene la disponibilità durante gli eventi di manutenzione e i malfunzionamenti non pianificati senza che occorra creare e gestire un array ridondante a livello di sistema operativo di array di dischi indipendenti (RAID)
- **Crittografia dei dati inattivi** [(Disponibile in data center selezionati).](new-ibm-block-and-file-storage-location-and-features.html)
   - La crittografia gestita dal provider per i dati inattivi viene fornita senza costi aggiuntivi
- **Archiviazione con supporto All-Flash** [(Disponibile in data center selezionati).](new-ibm-block-and-file-storage-location-and-features.html)
   - È possibile eseguire il provisioning dell'archiviazione All-Flash a 2 IOPS/GB o a livelli più elevati.
- **Istantanee** [(disponibile in data center selezionati).](new-ibm-block-and-file-storage-location-and-features.html).
   - Acquisisce istantanee di dati con punto temporale senza causare interruzioni del servizio.
- **Replica**  [(disponibile in data center selezionati).](new-ibm-block-and-file-storage-location-and-features.html)
   - Disponibile quando viene eseguito il provisioning dell'archiviazione in [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html).
   - Copia automaticamente le istantanee in un data center {{site.data.keyword.BluSoftlayer_full}} partner.
- **Connettività altamente disponibile**
   - Utilizza le connessioni di rete ridonanti per massimizzare la disponibilità.
   - Connessioni TCP/IP instradate a {{site.data.keyword.filestorage_short}} basato su NFS.
- **Accesso simultaneo**
   - Consente a più host di accedere (fino a 64) simultaneamente ai volumi di file.
- **Database in cluster**
   - Supporta dei casi d'uso avanzati, quali i database in cluster.

## Fatturazione

Puoi selezionare la fatturazione mensile o oraria per un volume di file. Il tipo di fatturazione selezionato per un LUN si applica al suo spazio per le istantanee e alle sue repliche. Ad esempio, se esegui il provisioning di un LUN con fatturazione oraria, eventuali addebiti di istantanee o replica vengono fatturati in modo orario. Se esegui il provisioning di un LUN con fatturazione mensile, eventuali addebiti di istantanee o replica vengono fatturati in modo mensile. 

Con la **fatturazione oraria**, il numero di ore per cui il volume di file è esistito nell'account viene calcolato quando il LUN viene eliminato oppure alla fine del ciclo di fatturazione, a seconda di quale di queste condizioni si verifichi per prima. La fatturazione oraria è una buona scelta per l'archiviazione utilizzata per qualche giorno o per meno di un mese completo. La fatturazione oraria è disponibile solo per l'archiviazione di cui viene eseguito il provisioning in [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html). 

Con la **fatturazione mensile**, il calcolo per il prezzo è a base proporzionale dalla data di creazione alla fine del ciclo di fatturazione e viene fatturato immediatamente. Non è previsto alcun rimborso se un volume viene eliminato prima della fine del ciclo di fatturazione.  La fatturazione mensile è una buona scelta per l'archiviazione utilizzata nei carichi di lavoro di produzione che usano dati che devono essere archiviati e a cui bisogna accedere per lunghi periodi di tempo (un mese o più). 

 
**Performance**
<table>
  <caption>La tabella 1 mostra i prezzi per l'archiviazione Performance con fatturazione mensile e oraria.</caption>
  <tr>
   <th>Prezzo mensile</th>
   <td>$0,10/GB + $0,07/IOP</td>
  </tr>
  <tr>
   <th>Prezzo orario</th>
   <td>$0,0001/GB + $0,0002/IOP</td>
  </tr>
</table>
 
**Endurance**
<table>
  <caption>La tabella 2 mostra i prezzi per l'archiviazione Endurance per ogni livello con le opzioni di fatturazione mensile e oraria.</caption>
  <tr>
   <th>Livello IOPS</th>
   <th>0,25 IOPS/GB</th>
   <th>2 IOPS/GB</th>
   <th>4 IOPS/GB</th>
   <th>10 IOPS/GB</th>
  </tr>
  <tr>
   <th>Prezzo mensile</th>
   <td>$0,10/GB</td>
   <td>$0,20/GB</td>
   <td>$0,35/GB</td>
   <td>$0,58/GB</td>
  </tr>
  <tr>
   <th>Prezzo orario</th>
   <td>0,0002/GB</td>
   <td>$0,0003/GB</td>
   <td>$0,0005/GB</td>
   <td>$0,0009/GB</td>
  </tr>
</table>

 

## Provisioning

È possibile eseguire il provisioning di volumi {{site.data.keyword.filestorage_short}} da 20 GB a 12 TB con due opzioni:<br/>
- Esegui il provisioning di livelli **Endurance** che offrono livelli di prestazioni predefiniti e funzioni quali le istantanee e la replica.
- Crea un ambiente **Performance** molto potente con IOPS (input/output operations per second) allocato. 

 
### Provisioning con i livelli Endurance

{{site.data.keyword.filestorage_short}} Endurance è disponibile in quattro livelli di prestazioni IOPS per supportare diverse esigenze applicative.<br />

- **0,25 IOPS per GB** è progettato per carichi di lavori con bassa intensità di I/O. Questi carichi di lavoro sono di norma caratterizzati dall'avere un'ampia percentuale di dati inattivi in un dato momento. Delle applicazioni di esempio includono le caselle di posta di archiviazione o le condizioni di file a livello di reparto.

- **2 IOPS per GB** è progettato per un utilizzo per finalità più generiche. Delle applicazioni di esempio includono le attività di host di piccoli database a supporto di applicazioni web o le immagini disco di VM per un hypervisor.

- **4 IOPS per GB** è progettato per i carichi di lavoro a maggiore intensità. Questi carichi di lavoro sono di norma caratterizzati dall'avere un'elevata percentuale di dati attivi in un dato momento. Delle applicazioni di esempio includono i database transazionali e altri database sensibili alle prestazioni.

- **10 IOPS per GB** è progettato per i carichi di lavoro più esigenti quali quelli creati dai database NoSQL e l'elaborazione di dati per l'analisi. Questo livello è disponibile solo per l'archiviazione di cui viene eseguito il provisioning fino a 4 TB in [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html).

Sono disponibili fino a 48.000 IOPS con un volume Endurance di 12 TB.
 
La scelta del livello Endurance corretto per il tuo carico di lavoro è un fattore chiave. È ugualmente importante utilizzare la dimensione blocco, la velocità di connessione Ethernet e il numero di host necessari corretti per ottenere le prestazioni massime. Se qualcuna di queste parti non si allinea con le altre, le ripercussioni sulla velocità effettiva risultante potrebbero essere di notevole entità.
 
### Provisioning con Performance

Performance è una classe di {{site.data.keyword.filestorage_short}} progettata per supportare applicazioni a elevato I/O con requisiti di prestazioni chiari che mal si adattano in un livello Endurance. Delle prestazioni prevedibili si raggiungono tramite l'allocazione di IOPS a livello di protocollo ai singoli volumi. È possibile eseguire il provisioning di diversi tassi di IOPS (da 100 a 48.000) con delle dimensioni dell'archiviazione che vanno da 20 GB a 12 TB.

A Performance per {{site.data.keyword.filestorage_short}} si accede, e viene montato, tramite una connessione NFS (Network File System). {{site.data.keyword.filestorage_short}} viene di norma utilizzato quando al volume accedono più server simultaneamente. Dei volumi Performance congruenti possono essere ordinati in base alle dimensioni e all'IOPS nella Tabella 1 e possono essere utilizzati con i sistemi operativi Linux.

<table cellpadding="1" cellspacing="1" style="width: 99%;">
 <caption>La tabella 3 mostra le combinazioni di dimensione e iOPS per l'archiviazione Performance. <br/><sup><img src="/images/numberone.png" alt="Nota a piè di pagina" /></sup> Un limite IOPS superiore a 6000 è disponibile in data center selezionati.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
          <tr>
            <th>Dimensione (GB)</th>
            <th>Numero minimo di IOPS</th>
            <th>Numero massimo di IOPS</th>
          </tr>
          <tr>
            <td>20</td>
            <td>100</td>
            <td>1.000</td>
          </tr>
          <tr>
            <td>40</td>
            <td>100</td>
            <td>2.000</td>
          </tr>
          <tr>
            <td>80</td>
            <td>100</td>
            <td>4.000</td>
          </tr>
          <tr>
            <td>100</td>
            <td>100</td>
            <td>6.000</td>
          </tr>
          <tr>
            <td>250</td>
            <td>100</td>
            <td>6.000</td>
          </tr>
          <tr>
            <td>500</td>
            <td>100</td>
            <td>6.000 o 10.000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
          <tr>
            <td>1.000</td>
            <td>100</td>
            <td>6,000 o 20,000<sup><img src="/images/numberone.png" alt="Nota a piè di pagina" /></sup></td>
          </tr>
          <tr>
            <td>2.000-3.000</td>
            <td>200</td>
            <td>6,000 o 40,000<sup><img src="/images/numberone.png" alt="Nota a piè di pagina" /></sup></td>
          </tr>
          <tr>
            <td>4.000-7.000</td>
            <td>300</td>
            <td>6,000 o 48,000<sup><img src="/images/numberone.png" alt="Nota a piè di pagina" /></sup></td>
          </tr>
          <tr>
            <td>8.000-9.000</td>
            <td>500</td>
            <td>6,000 o 48,000<sup><img src="/images/numberone.png" alt="Nota a piè di pagina" /></sup></td>
          </tr>
          <tr>
            <td>10.000-12.000</td>
            <td>1.000</td>
            <td>6,000 o 48,000<sup><img src="/images/numberone.png" alt="Nota a piè di pagina" /></sup></td>
          </tr>
</table>


I volumi Performance sono progettati per operare in modo congruentemente prossimo al livello IOPS di cui viene eseguito il provisioning. La congruenza semplifica l'impostazione della dimensione e del ridimensionamento degli ambienti applicativi a uno specifico livello di prestazioni. È inoltre possibile ottimizzare un ambiente creando un volume con un rapporto ideale prezzo/prestazioni.

### Considerazioni sul provisioning

**Dimensione blocco**

IOPS sia per Endurance che per Performance è basato su una dimensione di blocco di 16 KB con un carico di lavoro casuale al 50 percento e con lettura/scrittura al 50/50. Un blocco di 16 KB è l'equivalente di una scrittura sul volume.

La dimensione del blocco utilizzata dalla tua applicazione influisce direttamente sulle prestazioni dell'archiviazione. Se la dimensione del blocco utilizzata dalla tua applicazione è inferiore a 16 KB, il limite IOPS si realizza prima del limite di velocità effettiva. Viceversa, se la dimensione del blocco utilizzata dalla tua applicazione è superiore a 16 KB, il limite di velocità effettiva si realizza prima del limite di IOPS.

<table>
  <caption>La tabella 4 mostra degli esempi di come la dimensione del blocco e IOPS influiscono sulla velocità effettiva.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <thead>
          <tr>
            <th>Dimensione blocco (KB)</th>
            <th>IOPS</th>
            <th>Velocità effettiva (MB/s)</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>4 (tipica per Linux)</td>
            <td>1.000</td>
            <td>4</td>
          </tr>
          <tr>
            <td>8 (tipica per Oracle)</td>
            <td>1.000</td>
            <td>8</td>
          </tr>
          <tr>
            <td>16</td>
            <td>1.000</td>
            <td>16</td>
          </tr>
          <tr>
            <td>32 (tipica per SQLServer)</td>
            <td>500</td>
            <td>16</td>
          </tr>          
          <tr>
            <td>64</td>
            <td>250</td>
            <td>16</td>
          </tr>
          <tr>
            <td>128</td>
            <td>128</td>
            <td>16</td>
          </tr>
          <tr>
            <td>512</td>
            <td>32</td>
            <td>16</td>
          </tr>
        </tbody>
</table>

**Host autorizzati**

Un altro fattore da considerare è il numero di host che sta utilizzando il tuo volume. Se c'è un singolo host che sta accedendo al volume, potrebbe essere difficile realizzare l'IOPS massimo disponibile, soprattutto a conteggi IOPS estremi (nell'ordine delle decine di migliaia). Se il tuo carico di lavoro richiede una velocità effettiva elevata, sarebbe meglio configurare almeno un paio di server che accedono al tuo volume per evitare un collo di bottiglia di un singolo server.

**Connessione di rete**

La velocità della tua connessione Ethernet deve essere superiore a quella effettiva massima prevista dal tuo volume. In generale, non prevedere di saturare la tua connessione Ethernet oltre il 70% della larghezza di banda disponibile. Ad esempio, se hai 6.000 IOPS e stai utilizzando una dimensione del blocco di 16 KB, il volume può gestire una velocità effettiva di circa 94 MBps. Se hai una connessione Ethernet da 1 Gbps al tuo LUN, diventa un collo di bottiglia quando i tuoi server provano a utilizzare la velocità effettiva massima disponibile. Ciò è dovuto al fatto che il 70 percento del limite teorico di una connessione Ethernet da 1 Gbps (125 MB al secondo) consentirebbe solo 88 MB al secondo.

Per raggiungere l'IOPS massimo, è necessario che siano implementate delle risorse di rete adeguate. Altre considerazioni includono l'utilizzo della rete privata esternamente al lato archiviazione e host e le regolazioni specifiche per le applicazioni (stack di IP o profondità di coda e altre impostazioni).

**Versione NFS**

Nell'ambiente {{site.data.keyword.BluSoftlayer_full}} sono supportati sia NFS v3 che NFS v4.1. Tuttavia, NFS v3 è preferito perché NFS v4.1 è un protocollo con stato (non senza stato come NFSv3) e durante gli eventi di rete potrebbero verificarsi dei problemi di protocollo. NFS v4.1 deve disattivare tutte le operazioni e quindi completare un recupero del blocco. Su un server di file NFS relativamente occupato, l'aumentata latenza può causare interruzioni del servizio. La mancanza di multipercorso/trunking NFS v4.1 può anche estendere il ripristino di uno stato normale delle operazioni NFS.

## Inoltro del tuo ordine

Quando sei pronto a inoltrare il tuo ordine, attieniti alle istruzioni riportate [qui](provisioning-file-storage.html). Per il File Storage Provisioning con VMware, fai clic [qui](architecture-guide-file-storage-vmware.html)

## Connessione della tua nuova archiviazione

Dopo che la tua richiesta di provisioning è stata completata, autorizza i tuoi host ad accedere alla nuova archiviazione e configura la tua connessione. A seconda del sistema operativo del tuo host, segui il link appropriato.
- [Accesso a {{site.data.keyword.filestorage_short}} su Linux](accessing-file-storage-linux.html)
- [Montaggio di NFS/File Storage in CentOS](mounting-nsf-file-storage.html)
- [Montaggio dell'{{site.data.keyword.filestorage_short}} su CoreOS](mounting-storage-coreos.html)
- [Configurazione di {{site.data.keyword.filestorage_short}} per il backup con cPanel](configure-backup-cpanel.html)
- [Configurazione di {{site.data.keyword.filestorage_short}} per il backup con Plesk](configure-backup-plesk.html)
- [Montaggio del volume {{site.data.keyword.filestorage_short}} sugli host ESXi](architecture-guide-file-storage-vmware.html)
