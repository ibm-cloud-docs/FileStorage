---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, provisioning File Storage for VMware, NFS, File Storage, vmware,

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Provisioning di {{site.data.keyword.filestorage_short}} con VMware
{: #architectureguide}

La seguente procedura può aiutarti a ordinare e configurare {{site.data.keyword.filestorage_full}} in un ambiente vSphere 5.5 e vSphere 6.0 a {{site.data.keyword.BluSoftlayer_full}}.

{{site.data.keyword.filestorage_short}} è progettato per supportare applicazioni ad elevato I/O che richiedono dei livelli di prestazioni prevedibili. Le prestazioni prevedibili si raggiungono tramite l'allocazione di IOPS (input/output operations per second) a livello di protocollo ai singoli volumi.

Se hai bisogno che più di otto host accedano al tuo archivio dati VMware, la scelta di NFS {{site.data.keyword.filestorage_short}} è la prassi ottimale.
{:tip}

L'offerta di {{site.data.keyword.filestorage_short}} è accessibile e montata tramite una connessione NFS. In una distribuzione VMware, un singolo volume può essere montato su un massimo di 64 host ESXi come archiviazione condivisa. Puoi anche montare più volumi per creare un cluster di archiviazione per utilizzare vSphere Storage DRS (Distributed Resource Scheduler).

Le opzioni di prezzo e di configurazione per {{site.data.keyword.filestorage_short}} sono addebitate in base a una combinazione dello spazio riservato e dell'IOPS offerto.

## Considerazioni sull'ordinazione

Quando ordini {{site.data.keyword.filestorage_short}}, tieni conto delle seguenti informazioni:

- Quando decidi la dimensione, tieni conto della dimensione del carico di lavoro e della velocità effettiva necessaria. La dimensione è importante con il servizio Endurance che scala le prestazioni in modo lineare in relazione alla capacità (IOPS/GB). Al contrario, il servizio Performance consente all'amministratore di scegliere la capacità e le prestazioni indipendentemente. I requisiti di velocità effettiva sono importanti, con Performance.

  Il calcolo della velocità effettiva è IOPS x 16 KB. L'IOPS viene misurato in base a una dimensione in blocchi da 16-KB con una combinazione 50/50 di lettura e scrittura.<br/>Un incremento della dimensione del blocco aumenta la velocità effettiva ma riduce l'IOPS. Ad esempio, raddoppiare la dimensione del blocco impostando dei blocchi da 32-KB mantiene la velocità effettiva massima ma dimezza l'IOPS.
  {:note}

- NFS utilizza molte ulteriori operazioni di controllo file come `lookup`, `getattr` e `readdir`. Queste operazioni, in aggiunta alle operazioni di lettura/scrittura, possono contare come IOPS e variare in base al tipo di operazione e alla versione NFS.
- I volumi {{site.data.keyword.filestorage_short}} sono presentati agli indirizzi IP, alle sottoreti o ai dispositivi autorizzati.
- Per evitare una disconnessione dell'archiviazione durante il failover del percorso, {{site.data.keyword.IBM}} consiglia di installare gli strumenti VMware che impostano un valore di timeout appropriato. Non è necessario modificare il valore; l'impostazione predefinita è sufficiente per garantire che il tuo host VMware non perda la connettività.
- Nell'ambiente {{site.data.keyword.cloud}} sono supportati sia NFSv3 che NFSv4.1. Tuttavia, {{site.data.keyword.IBM}} consiglia di utilizzare NFSv3. Poiché NFSv4.1 è un protocollo con stato (non senza stato come NFSv3), durante gli eventi di rete potrebbero verificarsi dei problemi di protocollo. NFSv4.1 deve disattivare tutte le operazioni e quindi completare un recupero del blocco. Mentre sono in corso tali operazioni, possono verificarsi delle interruzioni del servizio.

Per ulteriori informazioni, consulta il white paper di VMware [Best Practices for running
VMware vSphere on Network Attached Storage](https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/techpaper/vmware-nfs-bestpractices-white-paper-en.pdf){: external}
{:tip}

**Matrice di supporto delle funzioni VMware del protocollo NFS**
<table>
  <caption>La tabella 1 illustra le funzioni vSphere come vengono applicate a due versioni differenti di NFS.</caption>
 <thead>
  <tr>
   <th>Funzioni vSphere</th>
   <th>NFS versione 3</th>
   <th>NFS versione 4.1</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>vMotion e Storage vMotion</td>
   <td>Sì</td>
   <td>Sì</td>
  </tr>
  <tr>
   <td>Elevata disponibilità (HA, High Availability)</td>
   <td>Sì</td>
   <td>Sì</td>
  </tr>
  <tr>
   <td>Tolleranza ai guasti (FT, Fault Tolerance)</td>
   <td>Sì</td>
   <td>Sì</td>
  </tr>
  <tr>
   <td>DRS (Distributed Resource Scheduler)</td>
   <td>Sì</td>
   <td>Sì</td>
  </tr>
  <tr>
   <td>Profili host</td>
   <td>Sì</td>
   <td>Sì</td>
  </tr>
  <tr>
   <td>DRS di archiviazione</td>
   <td>Sì</td>
   <td>No</td>
  </tr>
  <tr>
   <td>SIOC (Storage I/O Control)</td>
   <td>Sì</td>
   <td>No</td>
  </tr>
  <tr>
   <td>Site Recovery Manager</td>
   <td>Sì</td>
   <td>No</td>
  </tr>
  <tr>
   <td>Volumi virtuali</td>
   <td>Sì</td>
   <td>No</td>
  </tr>
 </tbody>
</table>
*Origine - [VMware - NFS Protocols and ESXi](https://docs.vmware.com/en/VMware-vSphere/6.0/com.vmware.vsphere.storage.doc/GUID-8A929FE4-1207-4CC5-A086-7016D73C328F.html){: external}*



### Utilizzo delle istantanee

{{site.data.keyword.filestorage_short}} consente agli amministratori di impostare le pianificazioni delle istantanee che creano ed eliminano copie di istantanea automaticamente per ciascun volume di archiviazione. Possono anche creare delle pianificazioni delle istantanee aggiuntive (orarie, giornaliere, settimanali) per le istantanee automatiche e creare manualmente delle istantanee ad hoc per gli scenari di continuità aziendale e ripristino di emergenza (BCDR, business continuity and disaster recovery). Gli avvisi automatici vengono recapitati tramite il [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} al proprietario del volume per le istantanee conservate e lo spazio utilizzato.

Per utilizzare le istantanee è necessario lo spazio per le istantanee. Lo spazio può essere acquistato durante l'ordine dei volumi iniziale e dopo il provisioning iniziale tramite la pagina **Volume Details** facendo clic su **Actions** e selezionando **Add Snapshot Space**.

È importare notare che gli ambienti VMware non rilevano le istantanee. La funzionalità di istantanea di {{site.data.keyword.filestorage_short}} non deve essere confusa con le istantanee VMware. Qualsiasi ripristino che utilizza la funzione di istantanea di {{site.data.keyword.filestorage_short}} deve essere gestito dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}.

Il ripristino del volume di {{site.data.keyword.filestorage_short}} richiede lo spegnimento di tutte le macchine virtuali (VM, Virtual Machine) in {{site.data.keyword.filestorage_short}}. Il volume deve essere temporaneamente smontato dagli host ESXi per evitare eventuali danneggiamenti di dati durante il processo.

Per ulteriori informazioni, vedi l'articolo relativo alle [istantanee](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots).


### Utilizzo della replica

La replica usa una delle tue pianificazioni delle istantanee per copiare automaticamente le istantanee su un volume di destinazione in un data center remoto. Le copie possono essere ripristinate nel sito remoto nel caso si verifichi un evento catastrofico o un danneggiamento dei dati.

Con le repliche, puoi

- Eseguire il ripristino da malfunzionamenti del sito e da altre situazioni critiche in modo rapido eseguendo il failover al volume di destinazione
- Eseguire il failover a uno specifico punto temporale nella copia di ripristino di emergenza (DR, disaster recovery)

La replica mantiene i tuoi dati sincronizzati in due diverse ubicazioni. Se vuoi clonare il tuo volume e utilizzarlo indipendentemente dal volume originale, consulta [Creazione di un volume di file duplicato](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume).
{:tip}

Prima di poter eseguire la replica, devi creare una pianificazione delle istantanee.

Quando esegui il failover, stai passando dal tuo volume di archiviazione nel tuo data center primario al volume di destinazione nel tuo data center remoto. Ad esempio, il tuo data center primario si trova a Londra e il tuo data center secondario si trova ad Amsterdam. Se si verifica un evento di malfunzionamento, eseguirai il failover ad Amsterdam. Eseguire il failover significa stabilire una connessione al volume che ora è quello primario da un'istanza di VSphere Cluster ad Amsterdam. Dopo che il tuo volume a Londra è stato riparato, verrà acquisita un'istantanea del volume che si trova ad Amsterdam. Successivamente, puoi eseguire il fallback a Londra e al volume che ora è nuovamente quello primario da un'istanza di elaborazione a Londra.

Prima dell'esecuzione del failback del volume al data center primario, è necessario che si smetta di farne uso presso il sito remoto. Un'istantanea delle informazioni nuove o modificate viene eseguita e replicata al data center primario prima che possa essere nuovamente eseguito il montaggio sugli host ESXi del sito di produzione.

Per ulteriori informazioni sulla configurazione delle repliche, vedi [Replica](/docs/infrastructure/FileStorage?topic=FileStorage-replication).

I dati non validi, non importa se danneggiati, oggetto di attacchi o infettati, vengono replicati nella pianificazione dell'istantanea e nella conservazione dell'istantanea. L'utilizzo delle finestre di replica più piccole può fornire un migliore obiettivo di punto di ripristino. Tuttavia, può anche fornire meno tempo per reagire alla replica di dati non validi.
{:note}


## Ordinazione di {{site.data.keyword.filestorage_short}}

Utilizza la [Advanced Single-Site VMware Reference Architecture](https://{DomainName}/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){: external} per configurare {{site.data.keyword.filestorage_short}} con le opzioni Endurance o Performance nel tuo ambiente VMware.

{{site.data.keyword.filestorage_short}} può essere ordinato tramite il [catalogo IBM Cloud](https://{DomainName}/catalog){: external} oppure il [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}. Per ulteriori informazioni, vedi [Ordinazione di {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole)

Il provisioning dell'archiviazione viene eseguito in meno di un minuto e diventa visibile nella pagina **{{site.data.keyword.filestorage_short}}** del [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}.

Dopo che è stato eseguito il provisioning di un volume, i {{site.data.keyword.BluBareMetServers_full}} o i {{site.data.keyword.BluVirtServers_full}} che utilizzano il volume devono essere autorizzati ad accedere all'archiviazione. Utilizza la seguente procedura per autorizzare il volume.

1. Fai clic su **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Seleziona **Access Host** nel menu **Endurance** o **Performance Volume Actions**.
3. Fai clic su **Subnets**.
4. Scegli dall'elenco di sottoreti disponibili assegnate alle porte VMkernel sugli host ESXi e fai clic su **Submit**.<br/>

   Le sottoreti visualizzate sono sottoreti sottoscritte nello stesso data center del volume di archiviazione.
   {:note}

Dopo che le sottoreti sono state autorizzate, annota il nome host del server di archiviazione Endurance o Performance che desideri utilizzare quando monti il volume. Queste informazioni sono disponibili nella pagina dei dettagli di {{site.data.keyword.filestorage_short}} facendo clic su uno specifico volume.


##  Configurazione dell'host della macchina virtuale VMware

Prima di iniziare il processo di configurazione di VMware, assicurati che siano soddisfatti i seguenti prerequisiti:

- Il provisioning dei {{site.data.keyword.BluBareMetServers}} con VMware ESXi viene eseguito con la corretta configurazione dell'archiviazione e le corrette credenziali di accesso ESXi.
- {{site.data.keyword.cloud}} Windows fisico o {{site.data.keyword.virtualmachinesshort}} nello stesso data center dei {{site.data.keyword.BluBareMetServers}}. Devono essere inclusi l'indirizzo IP pubblico della macchina virtuale (VM, Virtual Machine) {{site.data.keyword.cloud}} e le credenziali di accesso.
- Un computer con accesso a internet e con il software di browser web e un client RDP (Remote Desktop Protocol) installati.


### 1. Configurazione dell'host VMware.

1. Da un computer connesso a internet, avvia un client RDP e stabilisci una sessione RDP al {{site.data.keyword.BluVirtServers_full}} di cui è stato eseguito il provisioning nello stesso data center dove è installato vSphere vCenter.
2. Da {{site.data.keyword.BluVirtServers_short}}, avvia un browser web e stabilisci una connessione a VMware vCenter tramite vSphere Web Client.
3. Dalla schermata **HOME**, seleziona **Hosts and Clusters**. Espandi il riquadro sulla sinistra e seleziona il server ESXi VMware (**VMware ESXi server**) che deve essere utilizzato per questa distribuzione.
4. Assicurati che la porta del firewall per il client NFS sia aperta su tutti gli host in modo da poter configurare il client NFS sull'host vSphere. (La porta viene aperta automaticamente nelle release più recenti di vSphere). Per controllare se la porta è aperta, vai alla scheda **ESXi host Manage** in VMware® vCenter™, seleziona **Settings** e seleziona quindi **Security Profile**. Nella sezione **Firewall**, fai clic su **Edit** e scorri verso il basso a **NFS Client**.
5. Assicurati che venga fornito **Allow connection from any IP address or a list of IP addresses**. <br/>
   ![Consenti la connessione](/images/1_4.png)
6. Configura i frame jumbo andando alla scheda **ESXi host Manage**, selezionando **Manage** e quindi **Networking**.
7. Seleziona **VMkernel adapters**, evidenzia il **vSwitch** e fai clic su **Edit** (icona di matita).
8. Seleziona **NIC setting** e assicurati che la MTU della NIC sia impostata su 9000.
9. **Facoltativo**. Convalida le impostazioni del frame jumbo.
   - Windows
     ```
     ping -f -l 8972 a.b.c.d
     ```
     {:pre}

   - UNIX
     ```
     ping -s 8972 a.b.c.d
     ```
     {:pre}

     Il valore a.b.c.d è l'interfaccia {{site.data.keyword.BluVirtServers_short}} adiacente.

     Esempio
     ```
     ping a.b.c.d (a.b.c.d) 8972(9000) bytes of data.
     8980 bytes from a.b.c.d: icmp_seq=1 ttl=128 time=3.36 ms
     ```

Per ulteriori informazioni su VMware e frame Jumbo, vedi [qui](https://kb.vmware.com/s/article/1003712){: external}.
{:tip}


### 2. Aggiunta di un adattatore uplink a uno switch virtuale

1. Configura un nuovo adattatore uplink andando alla scheda **ESXi host Manage**, selezionando **Manage** e quindi **Networking**.
2. Seleziona la scheda **Physical adapters**
3. Fai clic su **Add host networking** (icona di globo con un segno più).
4. Seleziona il tipo di connessione come **Physical Network Adapter** e fai clic su **Next**.
5. Selezionare l'**vSwitch** esistente e fai clic su **Next**.
6. Seleziona **Unused adapters** e fai clic su **Add adapters** (segno più).
7. Fai clic sull'altro adattatore connesso ("Connected") e fai clic su **OK**. <br/>
   ![Aggiungi adattatori fisici allo switch](/images/2_3.png)
8. Fai clic su **Next** e quindi su **Finish**.
9. Torna alla scheda **Virtual switches** e seleziona **Edit setting** (icona di matita) sotto l'intestazione **Virtual Switches**.
10. A sinistra, seleziona la voce **Teaming and failover** vSwitch.
11. Verifica che l'opzione **Load balancing** sia impostata su **Route based on the originating virtual port** e fai clic su **OK**.


### 3. Configurazione dell'instradamento statico (facoltativo)

La configurazione di rete per questa guida all'architettura utilizza un numero minimo di gruppi di porte. Se hai un gruppo di porte VMkernel per l'archiviazione NFS, è necessario eseguire dei passi extra. Per impostazione predefinita, ESXi utilizza la porta VMkernel che si trova sulla stessa sottorete di un volume NFS per montare il volume NFS. Poiché viene utilizzato un instradamento di livello 3 per montare il volume NFS, è necessario forzare ESXi a utilizzare la porta VMkernel che era stata configurata per montare il volume NFS. Per utilizzare la porta corretta, è necessario creare un instradamento statico all'array di archiviazione.

1. Per configurare un instradamento statico, esegui SSH a ciascun host ESXi che utilizza l'archiviazione Performance o Endurance ed esegui questi comandi. Prendi nota dell'indirizzo IP che è il risultato del comando `ping` (primo comando) e utilizzalo con il comando di rete `esxcli`.
   ```
   ping <nome host dell'array di archiviazione>
   ```
   {: pre}

   Il nome host DNS dell'archiviazione NFS è una FZ (Forwarding Zone) a cui sono assegnati più indirizzi IP. Questi indirizzi IP sono statici e appartengono a quello specifico nome host DNS. Per accedere a uno specifico volume può essere utilizzato uno qualsiasi di questi indirizzi IP.
   {:note}

   ```
   esxcli network ip route ipv4 add –gateway GATEWAYIP –network <risultato del comando ping>/32
   ```
   {: pre}

2. Gli instradamenti statici non sono persistenti tra i riavvii su ESXi 5.0 e antecedenti. Per garantire che eventuali instradamenti statici aggiunti rimangano persistenti, questo comando deve essere aggiunto al file `local.sh` file su ciascun host, che si trova nella directory `/etc/rc.local.d/`. Apri il file `local.sh` utilizzando l'editor visivo e aggiungi il secondo comando nel passo 4.1. davanti alla riga `exit 0`.

Annota l'indirizzo IP poiché può essere utilizzato per montare il volume nel passo successivo.<br/>Questo processo deve essere eseguito per ciascun volume NFS che intendi montare al tuo host ESXi.<br/>Per ulteriori informazioni, vedi l'articolo della Knowledge Base di VMware KB, [Configuring static routes for VMkernel ports on an ESXi host](https://kb.vmware.com/s/article/2001426){: external}.
{:tip}


##  Creazione e montaggio del volume {{site.data.keyword.filestorage_short}} sugli host ESXi

1. Fai clic sull'icona **Go to vCenter** e quindi su **Hosts and Clusters**.
2. Nella scheda **Related Object**, fai clic su **Datastores**.
3. Fai clic sull'icona **Create a new datastore**.
4. Nella schermata **New Datastore**, seleziona l'ubicazione dell'archivio dati VMware e fai clic su **Next**.
5. Nella schermata **Type**, seleziona **NFS** e fai clic su **next**.
6. Quindi, seleziona la versione NFS. Sono supportati sia NFSv3 che NFSv4.1 ma è consigliato NFSv3.

   Assicurati di utilizzare una sola versione NFS per accedere all'archivio dati. Conseguenze del montaggio di uno o più host allo stesso datastore utilizzando versioni differenti possono comportare la corruzione dei dati.
   {:important}

7. Nella schermata **Name and configuration**, immetti il nome che desideri dare all'archivio dati VMware . Immetti inoltre il nome host del server NFS. L'utilizzo dell'FQDN per il server NFS produce la migliore distribuzione del traffico al server sottostante. Anche l'indirizzo IP è valido ma viene utilizzato meno di frequente e solo in specifiche istanze. Immetti il nome cartella nel formato `/nomecartella`.
8. Nella schermata **Host accessibility**, seleziona uno o più host su cui desideri montare l'archivio dati NFS VMware e fai clic su **next**.
9. Riesamina gli input nella schermata successiva e fai clic su **Finish**.
10. Ripeti per qualsiasi volume {{site.data.keyword.filestorage_short}} aggiuntivo.

{{site.data.keyword.cloud}} consiglia di utilizzare i nomi FQDN per stabilire una connessione all'archivio dati VMware. L'utilizzo dell'indirizzamento IP diretto potrebbe ignorare il meccanismo di bilanciamento del carico fornito utilizzando l'FQDN.
{:important}

Per utilizzare l'indirizzo IP invece dell'FQDN, esegui semplicemente il ping del server per ottenere l'indirizzo IP.
```
ping <nome host dell'array di archiviazione>
```
{: pre}

Per ottenere l'indirizzo IP da un host ESXi, utilizza il seguente comando. Il valore risultante, 10.2.125.80, è l'IP con l'FQDN.
```
~ # vmkping nfsdal0902a-fz.service.softlayer.com
PING nfsdal0902a-fz.service.softlayer.com (10.2.125.80): 56 data bytes
64 bytes from 10.2.125.80: icmp_seq=0 ttl=253 time=0.187 ms
```


## Abilitazione di ESXi SIOC (Storage I/O Control) (facoltativo)

SIOC (Storage I/O Control) è una funzione disponibile per i clienti che utilizzano una licenza Enterprise Plus. Quando è abilitato nell'ambiente, SIOC modifica la lunghezza della coda del dispositivo per la singola VM. La modifica alla lunghezza della coda del dispositivo riduce la coda dell'array di archiviazione per tutte le VM a una condivisione uguale. SIOC si attiva solo se le risorse sono vincolate e la latenza I/O dell'archiviazione è superiore a una soglia definita.


Per determinare quando un dispositivo di archiviazione è congestionato o vincolato, SIOC richiede una soglia definita. La latenza della soglia di congestione è diversa per i vari tipi di archiviazione. la selezione predefinita è al 90% della velocità effettiva di picco. Il valore di percentuale di velocità effettiva di picco indica la soglia di latenza stimata quando l'archivio dati VMware utilizza tale percentuale della sua velocità effettiva di picco stimata.


Configurare in modo non corretto SIOC per un archivio dati VMware o per un VMDK può avere notevoli ripercussioni sulle prestazioni.
{:important}


### Configurazione di SIOC (Storage I/O Control) per un archivio dati VMware

1. Vai all'archivio dati VMware nel navigator vSphere Web Client.
2. Fai clic sulla scheda **Manage**.
3. Fai clic su **Settings** e fai clic su **General**.
4. Fai clic su **Edit** per **Datastore Capabilities**.
5. Seleziona la casella di spunta **Enable Storage I/O Control**.<br/>
   ![Archivio dati NSF VMware](/images/3_0.png)
6. Fai clic su **OK**.

Questa impostazione è specifica per l'archivio dati VMware e non per l'host.
{:note}


### Configurazione di SIOC (Storage I/O Control) per {{site.data.keyword.BluVirtServers_short}}

Puoi anche limitare i singoli dischi virtuali per le singole VM oppure concedere loro condivisioni differenti con SIOC. Limitando i dischi e concedendo condivisioni differenti, puoi mettere in corrispondenza e allineare l'ambiente al carico di lavoro con il numero di IOPS di volume {{site.data.keyword.filestorage_short}} acquisito. Il limite è impostato da IOPS e non è possibile impostare un peso o condivisioni ("Shares") differenti. Dischi virtuali con le condivisioni impostate a High (2.000 condivisioni) ricevono il doppio dell'I/O di un disco impostato su Normal (1.000 condivisioni) e il quadruplo di uno impostato su Low (500 condivisioni). Normal è il valore normale per tutte le VM; devi pertanto regolare i valori a High oppure Low per le VM che lo richiedono.

Utilizza la seguente procedura per modificare le condivisioni e il limite di VDisk.

1. Scegli un {{site.data.keyword.BluVirtServers_short}} nell'inventario **VMs and Templates**.
2. Seleziona i {{site.data.keyword.BluVirtServers_short}} per I/O Control.
3. Fai clic sulla scheda **Manage** e fai clic su **Settings**. Fai clic su **Edit**.
4. Espandi la freccia **hard disk**. Seleziona **Modify the Shares** oppure **Limit - IOPs**, come appropriato per il tuo ambiente. Scegli un disco rigido virtuale dall'elenco e modifica la selezione Shares per scegliere il numero relativo di condivisioni da allocare ai {{site.data.keyword.BluVirtServers_short}} (Low, Normal o High). Puoi anche fare clic su **Custom** e immettere un valore di condivisione definito dall'utente.
5. Fai clic sulla colonna **Limit - IOPS** e immetti il valore massimo di risorse di archiviazione da allocare alla macchina virtuale.
6. Fai clic su **OK**.


Questo processo viene utilizzato per impostare i limiti di consumo di risorse dei singoli vDisk in un {{site.data.keyword.BluVirtServers_short}} anche quando SIOC non è abilitato. Queste impostazioni sono specifiche per il singolo guest, e non l'host, anche se sono utilizzate da SIOC.
{:important}


## Configurazione delle impostazioni lato host ESXi

Sono richieste delle impostazioni aggiuntive per la configurazione degli host ESXi 5.x per l'archiviazione NFS. Questa tabella mostra quale valore deve avere ciascuna impostazione.

|Parametro | Impostato su ... |
|----------|------------|
|Net.TcpipHeapSize |	32 |
|Net.TcpipHeapMax |	Per vSphere 5.0/5.1, imposta 128 <br/> Per vSphere 5.5 o successive, imposta 512 |
|NFS.MaxVolumes |	256 |
|NFS41.MaxVolumes |	256 (solo vSphere 6.0 o successive) |
|NFS.HeartbeatMaxFailures |	10 |
|NFS.HeartbeatFrequency |	12 |
|NFS.HeartbeatTimeout |	5 |
|NFS.MaxQueueDepth|	64 |


### Aggiornamento dei parametri di configurazione avanzati sull'host ESXi 5.x utilizzando la CLI

I seguenti esempi utilizzano la CLI per impostare i parametri di configurazione avanzati e, quindi, per controllarli. Lo strumento `esxcfg-advcfg` utilizzato negli esempi è disponibile nella directory `/usr/sbin` sugli host ESXi 5.x.

   - Impostazione dei parametri di configurazione avanzata dalla CLI ESXi 5.x.

     ```
     #esxcfg-advcfg -s 32 /Net/TcpipHeapSize
     #esxcfg-advcfg -s 128 /Net/TcpipHeapMax(For vSphere 5.0/5.1)
     #esxcfg-advcfg -s 512 /Net/TcpipHeapMax(For vSphere 5.5 and higher)
     #esxcfg-advcfg -s 256 /NFS/MaxVolumes
     #esxcfg-advcfg -s 256 /NFS41/MaxVolumes (ESXi 6.0 and higher)
     #esxcfg-advcfg -s 10 /NFS/HeartbeatMaxFailures
     #esxcfg-advcfg -s 12 /NFS/HeartbeatFrequency
     #esxcfg-advcfg -s 5 /NFS/HeartbeatTimeout   
     #esxcfg-advcfg -s 64 /NFS/MaxQueueDepth
     #esxcfg-advcfg -s 32 /Disk/QFullSampleSize
     #esxcfg-advcfg -s 8 /Disk/QFullThreshold
     ```

  - Controllo dei parametri di configurazione avanzata dalla CLI ESXi 5.x.

    ```
    #esxcfg-advcfg -g /Net/TcpipHeapSize
    #esxcfg-advcfg -g /Net/TcpipHeapMax
    #esxcfg-advcfg -g /NFS/MaxVolumes
    #esxcfg-advcfg -g /NFS41/MaxVolumes (ESXi 6.0 and higher)
    #esxcfg-advcfg -g /NFS/HeartbeatMaxFailures
    #esxcfg-advcfg -g /NFS/HeartbeatFrequency
    #esxcfg-advcfg -g /NFS/HeartbeatTimeout
    #esxcfg-advcfg -g /NFS/MaxQueueDepth
    #esxcfg-advcfg -g /Disk/QFullSampleSize
    #esxcfg-advcfg -g /Disk/QFullThreshold
    ```
Trova ulteriori informazioni su Advanced Single-Site VMware Reference Architecture [qui](/docs/infrastructure/virtualization?topic=Virtualization-advanced-single-site-vmware-reference-architecture){: external}.
{:tip}
