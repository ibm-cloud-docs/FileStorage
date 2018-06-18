---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Introduzione a {{site.data.keyword.filestorage_short}}

{{site.data.keyword.filestorage_full}} è {{site.data.keyword.filestorage_short}} persistente, veloce, flessibile, basato su NFS e collegato alla rete. In questo ambiente NAS (Network Attached Storage), hai il controllo totale sulle prestazioni e la funzione di condivisioni dei tuoi file. Le condivisioni di {{site.data.keyword.filestorage_short}} possono essere connesse a un massimo di 64 dispositivi autorizzati su connessioni TCP/IP instradate per la resilienza.

{{site.data.keyword.filestorage_short}} offre i livelli migliori del settore di durabilità e disponibilità con un insieme di funzioni senza paragone. {{site.data.keyword.filestorage_short}} è sviluppato utilizzando gli standard del settore e le prassi ottimali e progettato per proteggere l'integrità dei dati. {{site.data.keyword.filestorage_short}} mantiene la disponibilità durante gli eventi di manutenzione e i malfunzionamenti non pianificati, fornendo al tempo stesso una baseline di prestazioni congruente.

Avvaliti delle seguenti funzioni principali di {{site.data.keyword.filestorage_short}}:

- **Baseline di prestazioni congruente**
   - Fornita tramite l'allocazione di IOPS (input/output operations per second) a livello di protocollo ai singoli volumi
- **{{site.data.keyword.filestorage_short}}**
   - Disponibile per le condivisioni NFS basate su file
- **Altamente durevole e resiliente**
   - Protegge l'integrità dei dati e mantiene la disponibilità durante gli eventi di manutenzione e i malfunzionamenti non pianificati senza che occorra creare o gestire un array ridondante a livello di sistema operativo di array di dischi indipendenti (RAID)
- **Crittografia dei dati inattivi** [(Disponibile in data center selezionati).](new-ibm-block-and-file-storage-location-and-features.html)
   - Crittografia gestita dal provider per i dati inattivi senza costi aggiuntivi 
- **Archiviazione con supporto All-Flash** [(Disponibile in data center selezionati).](new-ibm-block-and-file-storage-location-and-features.html)
   - Archiviazione All-Flash per i volumi di cui viene eseguito il provisioning con Endurance o Performance a 2 IOPS/GB o superiore
- **Istantanee (quando ne viene eseguito il provisioning con Endurance o Performance in [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html))**.
   - Acquisisce istantanee di dati con punto temporale senza causare interruzioni del servizio
- **Replica** (quando ne viene eseguito il provisioning con Endurance o Performance in [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html)).
   - Copia automaticamente le istantanee su un data center {{site.data.keyword.BluSoftlayer_full}} partner
- **Connettività altamente disponibile**
   - Utilizza le connessioni di rete ridonanti per massimizzare la disponibilità - connessioni TCP instradate {{site.data.keyword.filestorage_short}} su base NFS
- **Accesso simultaneo**
   - Consente a più host di accedere (fino a 64) simultaneamente ai volumi di file
- **Database in cluster**
   - Supporta dei casi d'uso avanzati, quali i database in cluster

## Fatturazione oraria/mensile

Puoi selezionare la fatturazione mensile o oraria per un volume di file. Il tipo di fatturazione selezionato per una LUN si applicherà al suo spazio per le istantanee e alle sue repliche. Ad esempio, se esegui il provisioning di una LUN con fatturazione oraria, eventuali addebiti di istantanee o replica verranno fatturati in modo orario. Se esegui il provisioning di una LUN con fatturazione mensile, eventuali addebiti di istantanee o replica verranno fatturati in modo mensile. 

Con la **fatturazione oraria**, il numero di ore per cui il volume di file è esistito nell'account viene calcolato quando il volume viene eliminato oppure alla fine del ciclo di fatturazione, a seconda di quale di queste condizioni si verifichi per prima. La fatturazione oraria è una buona scelta per l'archiviazione utilizzata per qualche giorno o per meno di un mese completo. La fatturazione oraria è disponibile solo per l'archiviazione di cui viene eseguito il provisioning in [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html). 

Con la **fatturazione mensile**, il calcolo per il prezzo è a base proporzionale dalla data di creazione alla fine del ciclo di fatturazione e viene fatturato immediatamente. Non è previsto alcun rimborso se il volume di file viene eliminato prima della fine del ciclo di fatturazione. La fatturazione mensile è una buona scelta per l'archiviazione utilizzata nei carichi di lavoro di produzione che usano dati che devono essere archiviati e a cui bisogna accedere per lunghi periodi di tempo (un mese o più).

 
### Prestazioni:
<table>
 <tbody>
  <tr>
   <th>Prezzo mensile</th>
   <td>$0.10/GB + $0.07/IOPS</td>
  </tr>
  <tr>
   <th>Prezzo orario</th>
   <td>$0.0001/GB + $0.0002/IOPS</td>
  </tr>
  </tbody>
</table>
 
### Endurance:
<table>
 <tbody>
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
  </tbody>
</table>

 

## Provisioning

È possibile eseguire il provisioning di volumi {{site.data.keyword.filestorage_short}} da 20 GB a 12 TB con due opzioni per il provisioning: <br/>
- Esegui il provisioning con **livelli Endurance** che offrono livelli di prestazioni predefiniti e funzioni quali le istantanee e la replica.
- Crea un ambiente **Performance** molto potente con IOPS (input/output operations per second) allocato.

 
### Livelli Endurance

Quando fai un ordine con Endurance, scegli da molteplici livelli di prestazioni per supportare diverse esigenze applicative.

Endurance è disponibile in tre livelli di prestazioni IOPS per supportare diverse esigenze applicative.

- **0,25 IOPS per GB** è progettato per carichi di lavori con bassa intensità di I/O. Questi carichi di lavoro sono di norma caratterizzati dall'avere un'ampia percentuale di dati inattivi in un dato momento. Delle applicazioni di esempio includono le caselle di posta di archiviazione o le condizioni di file a livello di reparto.
- **2 IOPS per GB** è progettato per un utilizzo per finalità più generiche. Delle applicazioni di esempio includono le attività di host di piccoli database a supporto di applicazioni web o le immagini disco di macchina virtuale per un hypervisor.
- **4 IOPS per GB** è progettato per i carichi di lavoro a maggiore intensità. Questi carichi di lavoro sono di norma caratterizzati dall'avere un'elevata percentuale di dati attivi in un dato momento. Delle applicazioni di esempio includono i database transazionali e altri database sensibili alle prestazioni.
- **10 IOPS per GB** è progettato per i carichi di lavoro più esigenti quali quelli creati dai database NoSQL e l'elaborazione di dati per l'analisi.  Questo livello è disponibile per l'archiviazione di cui viene eseguito il provisioning fino a 4 TB di dimensione in [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html).

Sono disponibili fino a 48.000 IOPS con un volume Endurance di 12 TB.


La scelta del livello giusto di {{site.data.keyword.filestorage_short}} Endurance per il tuo carico di lavoro è un fattore chiave, ma è ugualmente importante utilizzare la dimensione blocco, la velocità di connessione Ethernet e il numero di host necessari per ottenere le prestazioni massime. Se qualcuna di queste parti non si allinea con le altre, le ripercussioni sulla velocità effettiva risultante potrebbero essere di notevole entità
 
### Performance

Performance è una classe di {{site.data.keyword.filestorage_full}} progettata per supportare applicazioni a elevato I/O con requisiti di prestazioni chiari che mal si adattano in un livello Endurance. Delle prestazioni prevedibili si raggiungono tramite l'allocazione di IOPS a livello di protocollo ai singoli volumi. È possibile eseguire il provisioning di un IOPS che va da 100 a 6.000 con delle dimensioni dell'archiviazione che vanno da 20 GB a 12 TB. 

A Performance per {{site.data.keyword.filestorage_short}} si accede, e viene montato, tramite una connessione NFS (Network File System). {{site.data.keyword.filestorage_short}} viene di norma utilizzato quando al volume accederanno più macchine simultaneamente. Dei volumi Performance congruenti possono essere ordinati in base alle dimensioni e all'IOPS nella Tabella 1 e possono essere utilizzati con i sistemi operativi Linux.

<table cellpadding="1" cellspacing="1" style="width: 99%;">
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <tbody>
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
            <td>6.000 o 20.000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
          <tr>
            <td>2.000-3.000</td>
            <td>200</td>
            <td>6.000 o 40.000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
          <tr>
            <td>4.000-7.000</td>
            <td>300</td>
            <td>6.000 o 48.000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
          <tr>
            <td>8.000-9.000</td>
            <td>500</td>
            <td>6.000 o 48.000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
          <tr>
            <td>10.000-12.000</td>
            <td>1.000</td>
            <td>6.000 o 48.000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
        </tbody>
</table>

<sup>![footnote](/images/numberone.png)</sup>  Limite di IOPS oltre 6000 disponibile in data center selezionati.


I volumi Performance sono progettati per offrire prestazioni congruentemente prossime al livello IOPS di cui viene eseguito il provisioning. La congruenza semplifica l'impostazione della dimensione e dello scaling degli ambienti applicativi a uno specifico livello di prestazioni. Inoltre, dato l'intervallo di dimensioni di volume e di conteggi IOPS, diventa possibile ottimizzare un ambiente creando un volume con un rapporto ideale prezzo/prestazioni.

La misurazione di IOPS, sia per Endurance che per Performance, è basata su una dimensione in blocchi da 16 KB con una combinazione 50/50 di letture e scritture. Per ottenere il massimo IOPS su un volume, devono essere implementate risorse di rete adeguate. Altre considerazioni includono l'utilizzo della rete privata esternamente al lato archiviazione e host e le regolazioni specifiche per le applicazioni (stack di IP, profondità di coda e così via). 

## Suggerimenti per il provisioning di IOPS per {{site.data.keyword.filestorage_short}}

IOPS sia per Endurance che per Performance è basato su una dimensione in blocchi da 16 KB con un carico di lavoro casuale al 50% e con lettura/scrittura al 50/50. Un blocco di approssimativamente 16 KB è l'equivalente di una scrittura sul volume.

La dimensione del blocco utilizzata dalla tua applicazione avrà una ripercussione diretta sulla prestazioni dell'archiviazione.  Se la dimensione del blocco utilizzata dalla tua applicazione è inferiore a 16 KB, il limite IOPS verrà realizzato prima del limite di velocità effettiva. Viceversa, se la dimensione del blocco utilizzata dalla tua applicazione è superiore a 16KB, il limite di velocità effettiva verrà realizzato prima del limite di IOPS. 

La modifica della dimensione del blocco influenzerà le prestazioni nel seguente modo:

<table cellpadding="1" cellspacing="1" style="width: 99%;">
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <tbody>
          <tr>
            <th>Dimensione blocco (KB)</th>
            <th>IOPS</th>
            <th>Velocità effettiva (MB/s)</th>
          </tr>
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

Scegliere il {{site.data.keyword.blockstorageshort}} giusto per il tuo carico di lavoro è importante e ugualmente importante è come evitare i colli di bottiglia. La velocità della tua connessione Ethernet deve essere superiore a quella effettiva massima prevista dal tuo volume. Come regola generale, non devi prevedere di saturare la connessione Ethernet oltre il 70% della larghezza di banda disponibile. Ad esempio, se hai 6.000 IOPS e stai utilizzando una dimensione del blocco di 16KB, il volume ha una capacità di circa 94 MB al secondo. Se hai una connessione Ethernet da 1 Gbps alla tua LUN, diventerà un collo di bottiglia quando i tuoi server proveranno a utilizzare la velocità effettiva massima disponibile perché il 70% del limite teorico di una connessione Ethernet da 1 Gbps (125 MB al secondo) consentirebbe solo 88 MB al secondo.


Un altro fattore da considerare è il numero di host che sta utilizzando il tuo volume. Se c'è un singolo host che sta accedendo al volume, potrebbe essere difficile realizzare l'IOPS massimo disponibile, soprattutto a conteggi IOPS estremi (nell'ordine delle decine di migliaia). Se il tuo carico di lavoro richiede una velocità effettiva elevata, sarebbe meglio configurare almeno due o tre server che accedono al tuo volume per evitare un collo di bottiglia di un singolo server.


Per raggiungere l'IOPS massimo, è necessario che siano implementate delle risorse di rete adeguate. Altre considerazioni includono l'utilizzo della rete privata esternamente al lato archiviazione e host e le regolazioni specifiche per le applicazioni (stack di IP, profondità di coda e così via).

Nell'ambiente {{site.data.keyword.BluSoftlayer_full}} sono supportati sia NFS v3 che NFS v4.1. Consigliamo tuttavia l'utilizzo di NFS v3. NFS v4.1 è un protocollo con stato (non senza stato come NFSv3) e, pertanto, durante gli eventi di rete potrebbero verificarsi dei problemi di protocollo. NFS v4.1 deve disattivare tutte le operazioni e quindi eseguire un recupero del blocco. Su un server di file NFS relativamente occupato, l'aumentata latenza può causare interruzioni del servizio. La mancanza di multipercorso/trunking NFS v4.1 può anche estendere il ripristino di uno stato normale delle operazioni NFS.
