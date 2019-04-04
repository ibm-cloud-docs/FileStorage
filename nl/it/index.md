---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-14"

keywords: File Storage, Endurance, Performance, IOPS, replication, billing, file storage, NFS,

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
 {:shortdesc: .shortdesc}


# Informazioni su {{site.data.keyword.filestorage_short}}
{: #about}

{{site.data.keyword.filestorage_full}} è {{site.data.keyword.filestorage_short}} persistente, veloce, flessibile, basato su NFS e collegato alla rete. In questo ambiente NAS (Network Attached Storage), hai il controllo totale sulle prestazioni e la funzione di condivisioni dei tuoi file. Le condivisioni di {{site.data.keyword.filestorage_short}} possono essere connesse a un massimo di 64 dispositivi autorizzati su connessioni TCP/IP instradate per la resilienza.
{:shortdesc}

## Funzioni
{: #FileStorageFeatures}

{{site.data.keyword.filestorage_short}} offre i livelli migliori del settore di durabilità e disponibilità con un insieme di funzioni senza paragone. {{site.data.keyword.filestorage_short}} è sviluppato utilizzando gli standard del settore e le prassi ottimali e progettato per proteggere l'integrità dei dati. {{site.data.keyword.filestorage_short}} mantiene la disponibilità durante gli eventi di manutenzione e i malfunzionamenti non pianificati e fornisce una baseline di prestazioni congruente.

Avvaliti delle seguenti funzioni principali di {{site.data.keyword.filestorage_short}}:

- **Baseline di prestazioni congruente**
   - Fornita tramite l'allocazione di IOPS (input/output operations per second) a livello di protocollo ai singoli volumi.
- **{{site.data.keyword.filestorage_short}}**
   - Disponibile per le condivisioni NFS basate su file.
- **Altamente durevole e resiliente**
   - Protegge l'integrità dei dati e mantiene la disponibilità durante gli eventi di manutenzione e i malfunzionamenti non pianificati senza che occorra creare e gestire un array ridondante a livello di sistema operativo di array di dischi indipendenti (RAID)
- **Crittografia dei dati inattivi** [(Disponibile in data center selezionati).](/docs/infrastructure/FileStorage?topic=FileStorage-news)
   - La crittografia gestita dal provider per i dati inattivi viene fornita senza costi aggiuntivi
- **Archiviazione con supporto All-Flash** [(Disponibile in data center selezionati).](/docs/infrastructure/FileStorage?topic=FileStorage-news)
   - È possibile eseguire il provisioning dell'archiviazione All-Flash a 2 IOPS/GB o a livelli più elevati.
- **Istantanee** [(disponibile in data center selezionati).](/docs/infrastructure/FileStorage?topic=FileStorage-news).
   - Acquisisce istantanee di dati con punto temporale senza causare interruzioni del servizio.
- **Replica**  [(disponibile in data center selezionati).](/docs/infrastructure/FileStorage?topic=FileStorage-news)
   - Disponibile quando viene eseguito il provisioning dell'archiviazione in [data center selezionati](/docs/infrastructure/FileStorage?topic=FileStorage-news).
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

Con la **fatturazione oraria**, il numero di ore per cui il volume di file è esistito nell'account viene calcolato quando il LUN viene eliminato oppure alla fine del ciclo di fatturazione, a seconda di quale di queste condizioni si verifica per prima. La fatturazione oraria è una buona scelta per l'archiviazione utilizzata per qualche giorno o per meno di un mese completo. La fatturazione oraria è disponibile solo per l'archiviazione di cui viene eseguito il provisioning in [data center selezionati](/docs/infrastructure/FileStorage?topic=FileStorage-news).

Con la **fatturazione mensile**, il calcolo per il prezzo è a base proporzionale dalla data di creazione alla fine del ciclo di fatturazione e viene fatturato immediatamente. Non è previsto alcun rimborso se un volume viene eliminato prima della fine del ciclo di fatturazione. La fatturazione mensile è una buona scelta per l'archiviazione utilizzata nei carichi di lavoro di produzione che usano dati che devono essere archiviati e a cui bisogna accedere per lunghi periodi di tempo (un mese o più).


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
   <td>$0.06/GB</td>
   <td>$0.15/GB</td>
   <td>$0,20/GB</td>
   <td>$0,58/GB</td>
  </tr>
  <tr>
   <th>Prezzo orario</th>
   <td>$0.0001/GB</td>
   <td>$0.0002/GB</td>
   <td>$0,0003/GB</td>
   <td>$0,0009/GB</td>
  </tr>
</table>



## Provisioning

È possibile eseguire il provisioning di volumi {{site.data.keyword.filestorage_short}} da 20 GB a 12 TB con due opzioni: <br/>
- Esegui il provisioning di livelli **Endurance** che offrono livelli di prestazioni predefiniti e funzioni quali le istantanee e la replica.
- Crea un ambiente **Performance** molto potente con IOPS (input/output operations per second) allocato.


### Provisioning con i livelli Endurance

{{site.data.keyword.filestorage_short}} Endurance è disponibile in quattro livelli di prestazioni IOPS per supportare diverse esigenze applicative. <br />

- **0,25 IOPS per GB** è progettato per carichi di lavori con bassa intensità di I/O. Questi carichi di lavoro sono di norma caratterizzati dall'avere un'ampia percentuale di dati inattivi in un dato momento. Delle applicazioni di esempio includono le caselle di posta di archiviazione o le condizioni di file a livello di reparto.

- **2 IOPS per GB** è progettato per un utilizzo per finalità più generiche. Delle applicazioni di esempio includono le attività di host di piccoli database a supporto di applicazioni web o le immagini disco di VM per un hypervisor.

- **4 IOPS per GB** è progettato per i carichi di lavoro a maggiore intensità. Questi carichi di lavoro sono di norma caratterizzati dall'avere un'elevata percentuale di dati attivi in un dato momento. Delle applicazioni di esempio includono i database transazionali e altri database sensibili alle prestazioni.

- **10 IOPS per GB** è progettato per i carichi di lavoro più esigenti quali quelli creati dai database NoSQL e l'elaborazione di dati per l'analisi. Questo livello è disponibile solo per l'archiviazione di cui viene eseguito il provisioning fino a 4 TB in [data center selezionati](/docs/infrastructure/FileStorage?topic=FileStorage-news).

Sono disponibili fino a 48.000 IOPS con un volume Endurance di 12-TB.

La scelta del livello Endurance corretto per il tuo carico di lavoro è un fattore chiave. È ugualmente importante utilizzare la dimensione blocco, la velocità di connessione Ethernet e il numero di host necessari corretti per ottenere le prestazioni massime. Se qualcuna di queste parti non si allinea con le altre, le ripercussioni sulla velocità effettiva risultante potrebbero essere di notevole entità.

### Provisioning con Performance

Performance è una classe di {{site.data.keyword.filestorage_short}} progettata per supportare applicazioni a elevato I/O con requisiti di prestazioni chiari che mal si adattano in un livello Endurance. Delle prestazioni prevedibili si raggiungono tramite l'allocazione di IOPS a livello di protocollo ai singoli volumi. È possibile eseguire il provisioning di diversi tassi di IOPS (da 100 a 48.000) con delle dimensioni dell'archiviazione che vanno da 20 GB a 12 TB.

A Performance per {{site.data.keyword.filestorage_short}} si accede, e viene montato, tramite una connessione NFS (Network file system). {{site.data.keyword.filestorage_short}} viene di norma utilizzato quando al volume accedono più server simultaneamente. Dei volumi Performance congruenti possono essere ordinati in base alle dimensioni e all'IOPS nella Tabella 1 e possono essere utilizzati con i sistemi operativi Linux.

<table cellpadding="1" cellspacing="1" style="width: 99%;">
 <caption>La tabella 3 mostra le combinazioni di dimensione e IOPS per l'archiviazione Performance.<br/><sup><img src="/images/numberone.png" alt="Nota a piè di pagina" /></sup> Dei limiti IOPS superiori a 6000 sono disponibili in data center selezionati.</caption>
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
            <td>6.000 o 20.000<sup><img src="/images/numberone.png" alt="Nota a piè di pagina" /></sup></td>
          </tr>
          <tr>
            <td>2.000</td>
            <td>200</td>
            <td>6.000 o 40.000<sup><img src="/images/numberone.png" alt="Nota a piè di pagina" /></sup></td>
          </tr>
          <tr>
            <td>3.000-7.000</td>
            <td>300</td>
            <td>6.000 o 48.000<sup><img src="/images/numberone.png" alt="Nota a piè di pagina" /></sup></td>
          </tr>
          <tr>
            <td>8.000-9.000</td>
            <td>500</td>
            <td>6.000 o 48.000<sup><img src="/images/numberone.png" alt="Nota a piè di pagina" /></sup></td>
          </tr>
          <tr>
            <td>10.000-12.000</td>
            <td>1.000</td>
            <td>6.000 o 48.000<sup><img src="/images/numberone.png" alt="Nota a piè di pagina" /></sup></td>
          </tr>
</table>


I volumi Performance sono progettati per operare in modo congruentemente prossimo al livello IOPS di cui viene eseguito il provisioning. La congruenza semplifica l'impostazione della dimensione e del ridimensionamento degli ambienti applicativi a uno specifico livello di prestazioni. È inoltre possibile ottimizzare un ambiente creando un volume con un rapporto ideale prezzo/prestazioni.
