---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Guida all'architettura per {{site.data.keyword.filestorage_short}} con VMware

Viene qui di seguito indicata la procedura per ordinare e configurare {{site.data.keyword.filestorage_full}} in un ambiente vSphere 5.5 e vSphere 6.0 a {{site.data.keyword.BluSoftlayer_full}}. Utilizzare l'{{site.data.keyword.filestorage_short}} NFS è la prassi ottimale se hai bisogno di più di 8 connessioni al tuo host VMWare.

## Panoramica di {{site.data.keyword.filestorage_short}}

La nostra {{site.data.keyword.filestorage_short}} è progettata per supportare applicazioni ad elevato I/O che richiedono dei livelli di prestazioni prevedibili. Le prestazioni prevedibili si raggiungono tramite l'allocazione di IOPS (input/output operations per second) a livello di protocollo ai singoli volumi.

L'offerta di {{site.data.keyword.filestorage_short}} è accessibile e montata tramite una connessione NFS. In una distribuzione VMware, un singolo volume può essere montato su un massimo di 64 host ESXi come archiviazione condivisa oppure puoi montare più volumi per creare un cluster di archiviazione per utilizzare la pianificazione risorse DINAMICA di vSphere Storage.

Le opzioni di prezzo e di configurazione per l'{{site.data.keyword.filestorage_short}} Endurance e Performance sono addebitate in base a una combinazione dello spazio riservato e dell'IOPS offerto.

### 1. Considerazioni su {{site.data.keyword.filestorage_short}}

Quando ordini l'{{site.data.keyword.filestorage_short}}, dovresti tener presente le seguenti informazioni e considerazioni:

- La dimensione dell'archiviazione, IOPS e sistema operativo non possono essere modificati dopo che è stato eseguito il provisioning dei volumi di {{site.data.keyword.filestorage_short}}. Eventuali modifiche che desideri apportare per la quantità di spazio, il numero di IOPS o il sistema operativo richiedono l'esecuzione del provisioning di un nuovo volume. Tutti i dati archiviati nel volume precedente dovranno essere migrati al nuovo volume, o ai nuovi volumi se più di uno, utilizzando VMware Storage vMotion.
- Quando decidi la dimensione, tieni conto della dimensione del carico di lavoro e della velocità effettiva necessaria. La dimensione è importante con il servizio Endurance che scala le prestazioni in modo lineare in relazione alla capacità (IOPS/GB), diversamente dal servizio Performance che consente all'amministratore di scegliere la capacità e le prestazioni indipendentemente. I requisiti di velocità effettiva sono importanti, con Performance. <br/> **Nota**: il calcolo della velocità effettiva è IOPS x 16 KB. Gli IOPS sono misurati in base a una dimensione in blocchi da 16 KB con una combinazione 50/50 di lettura e scrittura. <br/> **Nota**: aumentare la dimensione del blocco aumenterà la velocità effettiva ma diminuirà l'IOPS. Ad esempio, raddoppiare la dimensione di blocco a 32KB blocchi manterrà la velocità effettiva massima ma dimezzerà l'IOPS.
- NFS utilizza molte operazioni di controllo file aggiuntivi come lookup, getattr e readdir, per indicarne qualcuna. Queste operazioni, in aggiunta alle operazioni di lettura/scrittura, possono contare come IOPS e variare in base al tipo di operazione e alla versione NFS.
- Tecnicamente, è possibile eseguire uno striping di più volumi insieme per ottenere un IOPS più elevato e una maggiore velocità effettiva; tuttavia,VMware consiglia un singolo archivio dati VMFS (Virtual Machine File System) per volume per evitare una riduzione delle prestazioni.
- I volumi {{site.data.keyword.filestorage_short}} sono presentati agli indirizzi IP, alle sottoreti o ai dispositivi autorizzati.
- I servizi di istantanea e replica sono disponibili in modo nativo solo sui volumi {{site.data.keyword.filestorage_short}} Endurance. L'{{site.data.keyword.filestorage_short}} Performance non ha tali funzionalità.
- Per evitare una disconnessione dell'archiviazione durante il failover del percorso, {{site.data.keyword.IBM}} consiglia di installare gli strumenti VMWare che imposteranno un valore di timeout appropriato. Non è necessario modificare il valore; l'impostazione predefinita è sufficiente per garantire che il tuo host VMWare non perderà la connettività.
- Nell'ambiente {{site.data.keyword.BluSoftlayer_full}} sono supportati sia NFS v3 che NFS v4.1. Tuttavia, {{site.data.keyword.IBM}} consiglia di utilizzare NFS v3. Poiché NFS v4.1 è un protocollo con stato (non senza stato come NFSv3), durante gli eventi di rete potrebbero verificarsi dei problemi di protocollo. NFS v4.1 deve disattivare le operazioni e quindi eseguire un recupero del blocco. Mentre sono in corso tali operazioni, potrebbero verificarsi delle interruzioni del servizio.

####  Matrice di supporto delle funzioni VMware del protocollo NFS
<table>
 <tbody>
  <tr>
   <th>Funzioni vSphere</th>
   <th>NFS versione 3</th>
   <th>NFS versione 4.1</th>
  </tr>
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
*Origine: [VMWare - Protocolli NFS e ESXi](https://docs.vmware.com/en/VMware-vSphere/6.0/com.vmware.vsphere.storage.doc/GUID-8A929FE4-1207-4CC5-A086-7016D73C328F.html){:new_window}*





### 2. Istantanee di {{site.data.keyword.filestorage_short}} Endurance

L'{{site.data.keyword.filestorage_short}} Endurance consente agli amministratori di impostare le pianificazioni delle istantanee che creano ed eliminano copie di istantanea automaticamente per ciascun volume di archiviazione. Possono anche creare delle pianificazioni delle istantanee aggiuntive (orarie, giornaliere, settimanali) per le istantanee automatiche e creare manualmente delle istantanee ad hoc per gli scenari di continuità aziendale e ripristino di emergenza (BCDR, business continuity and disaster recovery). Gli avvisi automatici vengono consegnati tramite il [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} al proprietario del volume per le istantanee conservate e lo spazio consumato.

Tieni presente che per utilizzare le istantanee è necessario lo "spazio per le istantanee". Lo spazio può essere acquisito durante l'ordinazione di volumi iniziale e dopo il provisioning iniziale tramite la pagina **Volume Details** facendo clic sul pulsante a discesa Actions e selezionando **Add Snapshot Space**.

Importante: gli ambienti VMware non rilevano le istantanee. La funzionalità di istantanea dell'{{site.data.keyword.filestorage_short}} Endurance non deve essere confusa con le istantanee VMware. Qualsiasi ripristino utilizzando la funzione di istantanea dell'{{site.data.keyword.filestorage_short}} Endurance deve essere gestita dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}. Il ripristino del volume di {{site.data.keyword.filestorage_short}} Endurance richiederà lo spegnimento di tutte le macchine virtuali (VM, Virtual Machine) presenti nell'{{site.data.keyword.filestorage_short}} Endurance e lo smontaggio temporaneo del volume dagli host ESXi per evitare eventuali danneggiamenti di dati durante il processo.

Consulta il nostro articolo sulle [istantanee](snapshots.html) per ulteriori dettagli su come configurare le istantanee.


### 3. Replica dell'archivio file

La replica usa una delle tue pianificazioni delle istantanee per copiare automaticamente le istantanee su un volume di destinazione in un data center remoto. Le copie possono essere ripristinate nel sito remoto se si verifica un danneggiamento dei dati oppure un evento catastrofico.


Le repliche ti consentono

- di eseguire il ripristino da malfunzionamento del sito e altre situazioni critiche in modo rapido eseguendo il failover al volume di destinazione;
- di eseguire il failover a uno specifico punto temporale nella copia di ripristino di emergenza (DR, disaster recovery)

Prima di poter eseguire la replica, devi creare una pianificazione delle istantanee. Quando esegui il failover, stai passando dal tuo volume di archiviazione nel tuo data center primario al volume di destinazione nel tuo data center remoto. Ad esempio, il tuo data center primario si trova a Londra e il tuo data center secondario si trova ad Amsterdam. Se si verifica un evento di malfunzionamento, eseguirai il failover ad Amsterdam, stabilendo una connessione al volume che ora è quello primario da un'istanza di VSphere Cluster ad Amsterdam.


Dopo che il tuo volume a Londra sarà stato riparato, verrà acquisita un'istantanea del volume che si trova ad Amsterdam per eseguire il fallback a Londra e al volume che ora è nuovamente quello primario da un'istanza di elaborazione a Londra. Prima dell'esecuzione del failback del volume al data center primario, è necessario che si smetta di farne uso presso il sito remoto. Un'istantanea delle informazioni nuove o modificate viene eseguita e replicata al data center primario prima che possa essere nuovamente eseguito il montaggio sugli host ESXi del sito di produzione.


Consulta la pagina delle informazioni sulla [replica](replication.html) per ulteriori dettagli su come configurare la replica.

**Nota**: i dati non validi, non importa se danneggiati, oggetto di attacchi o infettati, verranno replicati nella pianificazione dell'istantanea e nella conservazione dell'istantanea. L'utilizzo delle finestre di replica più piccole può fornire un migliore obiettivo di punto di ripristino. Può anche fornire meno tempo per reagire alla replica di dati non validi.





## Ordinare {{site.data.keyword.filestorage_short}}

Puoi ordinare e configurare l'{{site.data.keyword.filestorage_short}} per un ambiente VMware ESXi 5. Utilizza le seguenti informazioni insieme alla Advanced Single-Site VMware Reference Architecture per configurare una di queste opzioni di archiviazione nel tuo ambiente VMware.


L'{{site.data.keyword.filestorage_short}} può essere ordinata tramite il [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} accedendo alla pagina {{site.data.keyword.filestorage_short}} tramite **Storage** > **{{site.data.keyword.filestorage_short}}**.


### 1. Ordinazione dell'{{site.data.keyword.filestorage_short}}

Utilizza la seguente procedura per ordinare {{site.data.keyword.filestorage_short}}:
1. Fai clic su **Storage** > **{{site.data.keyword.filestorage_short}}** nella home page del [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
2. Fai clic sul link **Order {{site.data.keyword.filestorage_short}}** nella pagina **{{site.data.keyword.filestorage_short}}**.
3. Seleziona **Endurance**/**Performance** dall'elenco a discesa Select Storage Type.
4. Seleziona l'ubicazione. I data center con funzionalità migliorate sono indicati con un `*`. Assicurati che la nuova archiviazione sarà aggiunta nella stessa ubicazione dell'host o degli host ESXi ordinati in precedenza.
5. Seleziona il metodo di fatturazione. Sono disponibili le opzioni di fatturazione mensile ed oraria.
6. Seleziona la quantità desiderata di spazio di archiviazione in GB. Per TB, 1 TB equivale a 1.000 GB e 12 TB equivalgono a 12.000 GB.
7. Immetti la quantità desiderata di IOPS in intervalli di 100 oppure seleziona un livello IOPS.
8. Specifica la dimensione dello spazio per le istantanee.
9. Fai clic su **Continue**.
10. Immetti un codice promozionale, se ne hai uno, e fai clic su **Recalculate**.
11. Riesamina il tuo ordine.
12. Seleziona la casella di spunta **I have read the Master Service Agreement and agree to the terms therein**.
13. Fai clic su **Place Order** per inviare l'ordine oppure su **Cancel** per chiudere il modulo senza inviare un ordine.

Il provisioning dell'archiviazione verrà eseguito in meno di un minuto e sarà visibile sulla pagina **{{site.data.keyword.filestorage_short}}** del [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.



### 2. Autorizza gli host a utilizzare l'{{site.data.keyword.filestorage_short}}

Dopo che è stato eseguito il provisioning di un volume, i {{site.data.keyword.BluBareMetServers_full}} o i {{site.data.keyword.BluVirtServers_full}} che utilizzeranno il volume devono essere autorizzati ad accedere all'archiviazione. Utilizza la seguente procedura per autorizzare il volume:

1. Fai clic su **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Seleziona **Access Host** nel menu **Endurance** o **Performance Volume Actions**.
3. Seleziona il pulsante di opzione **Subnets**
4. Scegli dall'elenco di sottoreti disponibili assegnate alle porte vmkernel sugli host ESXi e fai clic su **Submit**.<br/> **Nota**: le sottoreti visualizzate saranno sottoreti sottoscritte nello stesso data center del volume di archiviazione.


Dopo che le sottoreti sono state autorizzate, annota il nome host del server di archiviazione Endurance o Performance che desideri utilizzare quando monti il volume. Queste informazioni sono disponibili nella pagina dei dettagli dell'{{site.data.keyword.filestorage_short}} facendo clic su uno specifico volume.


##  Configurare l'host della macchina virtuale (VM, Virtual Machine) VMware Virtual Machine

### 1. Prerequisiti

Prima di iniziare il processo di configurazione di VMware, assicurati che siano soddisfatti i seguenti prerequisiti:

- Il provisioning dei {{site.data.keyword.BluBareMetServers}} con VMware ESXi viene eseguito con la corretta configurazione dell'archiviazione e le corrette credenziali di accesso ESXi.
- {{site.data.keyword.BluSoftlayer_full}} Windows fisico o {{site.data.keyword.virtualmachinesshort}} nello stesso data center dei {{site.data.keyword.BluBareMetServers}}. Devono essere inclusi l'indirizzo IP pubblico della macchina virtuale (VM, Virtual Machine) {{site.data.keyword.BluSoftlayer_full}} e le credenziali di accesso.
- Un computer con accesso a Internet e con il software di browser web e un client RDP (Remote Desktop Protocol) installati.


### 2. Procedura di configurazione dell'host VMware.

per configurare l'host virtuale, completa la seguente procedura:

1. Da un computer connesso a Internet, avvia un client RDP e stabilisci una sessione RDP al {{site.data.keyword.BluVirtServers_full}} di cui è stato eseguito il provisioning nello stesso data center dove è installato vSphere vCenter.
2. Da {{site.data.keyword.BluVirtServers_short}}, avvia un browser web e stabilisci una connessione a VMware vCenter tramite vSphere Web Client.
3. Dalla schermata **HOME**, seleziona **Hosts and Clusters**. Espandi il pannello sulla sinistra e seleziona il server ESXi VMware (**VMware ESXi server**) che deve essere utilizzato per questa distribuzione.
4. Assicurati che la porta del firewall per il client NFS sia aperta su tutti gli host per configurare il client NFS sull'host vSphere. Viene aperta automaticamente nelle release più recenti di vSphere. Per controllare se la porta è aperta, vai alla scheda **ESXi host Manage** in VMware® vCenter™, seleziona **Settings** e seleziona quindi **Security Profile**. Nella sezione **Firewall**, fai clic su **Edit** e scorri verso il basso a **NFS Client**.
5. Assicurati che venga fornito **Allow connection from any IP address or a list of IP addresses**. <br/>
   ![Consenti la connessione](/images/1_4.png)
6. Configura i frame jumbo andando alla scheda **ESXi host Manage**, selezionando **Manage** e quindi **Networking**.
7. Seleziona **VMkernel adapters**, evidenzia il **vSwitch** e fai clic su **Edit**. (Icona di una matita)
8. Seleziona **NIC setting** e assicurati che la MTU della NIC sia impostata su 9000.
   - Se necessario, imposta la MTU su 9000 e fai clic su **OK**.
9. Se vuoi, le impostazioni del frame jumbo possono essere convalidate nel seguente modo:
   - Da Windows: ping -f -l 8972 a.b.c.d
   - Da Unix: ping -s 8972 a.b.c.d
   dove a.b.c.d è l'interfaccia {{site.data.keyword.BluVirtServers_short}} adiacente con il comando:
   L'output è simile al seguente:
   ```ping a.b.c.d (a.b.c.d) 8972(9000) bytes of data.
   8980 bytes from a.b.c.d: icmp_seq=1 ttl=128 time=3.36 ms
   ```

Ulteriori informazioni su VMware e i frame jumbo sono disponibili [qui](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1003712){:new_window}.


### 3. Aggiunta di un adattatore uplink a uno switch virtuale

1. Configura un nuovo adattatore uplink andando alla scheda **ESXi host Manage**, selezionando **Manage** e quindi **Networking**.
2. Seleziona la scheda **Physical adapters**
3. Fai clic su **Add host networking**. (Icona di un globo con un segno più)
4. Seleziona il tipo di connessione come **Physical Network Adapter** e fai clic su **Next**.
5. Selezionare l'**vSwitch** esistente e fai clic su **Next**.
6. Seleziona **Unused adapters** e fai clic su **Add adapters**. (Segno più)
7. Fai clic sull'altro adattatore connesso ("Connected") e fai clic su **OK**. <br/>
   ![Aggiungi adattatori fisici allo switch](/images/2_3.png)
8. Fai clic su **Next** e quindi su **Finish**.
9. Torna alla scheda **Virtual switches** e seleziona l'icona **Edit setting** superiore sotto l'intestazione **Virtual Switches**. (Icona di una matita)
10. Seleziona la voce di vSwitch **Teaming and failover** sulla sinistra.
Verifica che l'opzione **Load balancing** sia impostata su **Route based on the originating virtual port** e fai clic su **OK**.


### 4. Configura l'instradamento statico ESXi (facoltativo)

La configurazione di rete per questa guida all'architettura utilizza un numero minimo di gruppi di porte. Se hai configurato un gruppo di porte VMkernal per l'archiviazione NFS, devono essere eseguiti dei passi aggiuntivi. Per impostazione predefinita, ESXi utilizzerà la porta vmkernel che si trova sulla stessa sottorete di un volume NFS per montare il volume NFS sull'archiviazione Endurance/Performance. Poiché stiamo utilizzando un instradamento di livello 3 per montare il volume NFS, dobbiamo forzare ESXi a utilizzare la porta vmkernel che abbiamo configurato per montare il volume NFS. Per eseguire questa operazione, dobbiamo creare un instradamento statico all'array di archiviazione Endurance/Performance.

1. Per configurare un instradamento statico, esegui SSH a ciascun host ESXi con Performance o Endurance ed esegui questi comandi. Nota: devi prendere il comando ping dell'indirizzo IP risultante (primo comando) e utilizzarlo con il comando di rete esxcli visualizzato di seguito:
   - `ping <hostname of the endurance/performance array>`

      **Nota** che il nome host DNS dell'archiviazione NFS è una FZ (Forwarding Zone) a cui sono assegnati più indirizzi IP. Questi indirizzi IP sono statici e appartengono a quello specifico nome host DNS.  Per accedere a uno specifico volume può essere utilizzato uno qualsiasi di questi indirizzi IP.
   - `esxcli network ip route ipv4 add –gateway GATEWAYIP –network <result of ping command>/32`

2. Nota che gli instradamenti statici non sono persistenti tra i riavvii su ESXi 5.0 e antecedenti. Per garantire che qualsiasi instradamento statico aggiunto sia persistente, è necessario aggiungere questi comandi al file local.sh su ciascun host, che si trova nella directory /etc/rc.local.d/. Per eseguire tale operazione, apri il file local.sh utilizzando l'editor visivo e aggiungi il comando sopra indicato da eseguire sopra la riga exit 0.
   - Annota l'indirizzo IP poiché può essere utilizzato per montare il volume nel passo successivo.
   - Questo processo deve essere eseguito per ciascun volume NFS che intendi montare al tuo host ESXi.
   - Questo è un link a un articolo della Knowledge Base di VMware: [Configuring static routes for VMkernel ports on an ESXi host](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2001426){:new_window}.


##  Monta uno o più volumi {{site.data.keyword.filestorage_short}} sugli host ESXi

1. Fai clic sull'icona **Go to vCenter** nella parte superiore della pagina web e quindi su **Hosts and Clusters**.
2. Fai clic su **Datastores** nella scheda **Related Object**. Fai clic sull'icona **Create a new datastore**.
3. Nella schermata **New Datastore**, seleziona l'ubicazione dell'archivio dati (il tuo host ESXi) e fai clic su **Next**.
4. Nella schermata **Type**, seleziona il pulsante di opzione **NFS** e fai clic su **Next**.
5. Nella schermata **Name and configuration**, immetti il nome che desideri dare all'archivio dati. Immetti inoltre il nome host del server NFS. L'utilizzo dell'FQDN per il server NFS produce la migliore distribuzione del traffico al server sottostante. Anche l'indirizzo IP è valido ma viene utilizzato meno di frequente e solo in specifiche istanze. Immetti il nome cartella nel formato /nomecartella.
6. Nella schermata **Host accessibility**, seleziona tutti gli host su cui desideri montare l'archivio dati NFS e fai clic su **next**.
7. Riesamina gli input nella schermata successiva e fai clic su **Finish**.
8. Ripeti per qualsiasi volume {{site.data.keyword.filestorage_short}} aggiuntivo.

**Nota**: {{site.data.keyword.BluSoftlayer_full}} consiglia di utilizzare i nomi FQDN per stabilire una connessione all'archivio dati. L'utilizzo dell'indirizzamento IP diretto può ignorare il meccanismo di bilanciamento del carico fornito utilizzando l'FQDN. Per utilizzare l'indirizzo IP invece dell'FQDN, esegui il seguente comando per ottenere l'indirizzo IP.

  - `ping <hostname of the endurance/performance array>`
  - Da un host ESXi:
    ```
    ~ # vmkping nfsdal0902a-fz.service.softlayer.com
    PING nfsdal0902a-fz.service.softlayer.com (10.2.125.80): 56 data bytes
    64 bytes from 10.2.125.80: icmp_seq=0 ttl=253 time=0.187 ms
    10.2.125.80 is the IP address associated with the FQDN
    ```

## Impostazioni di abilitatazione di ESXi SIOC (Storage I/O Control) (facoltativo)

SIOC (Storage I/O Control) è una funzione disponibile per i clienti che utilizzano una licenza Enterprise Plus. Quando è abilitato nell'ambiente, SIOC modificherà la lunghezza della coda del dispositivo per la singola VM. La modifica alla lunghezza della coda del dispositivo riduce la coda dell'array di archiviazione per tutte le VM a una condivisione e una limitazione della coda di archiviazione uguali. SIOC si attiva solo se le risorse sono vincolate e la latenza I/O dell'archiviazione è superiore a una soglia definita.


Per determinare quando un dispositivo di archiviazione è congestionato o vincolato, SIOC richiede una soglia definita. La latenza della soglia di congestione è diversa per i vari tipi di archiviazione; la selezione predefinita è al 90% della velocità effettiva di picco. La percentuale di valore di velocità effettiva di picco indica la soglia di latenza stimata quando l'archivio dati sta utilizzando tale percentuale della sua velocità effettiva di picco stimata.


Nota: configurare in modo non corretto SIOC per un archivio dati o per un VMDK può avere notevoli ripercussioni sulle prestazioni.



### 1. SIOC (Storage I/O Control) per un archivio dati

Attieniti alla seguente procedura per abilitare SIOC con i valori consigliati per l'archiviazione Endurance e Performance:

1. Vai all'archivio dati nel navigator vSphere Web Client.
2. Fai clic sulla scheda **Manage**.
3. Fai clic su **Settings** e fai clic su **General**.
4. Fai clic su **Edit** per **Datastore Capabilities**.
5. Seleziona la casella di spunta **Enable Storage I/O Control**.<br/>
   ![NSFDataStore](/images/3_0.png)
6. Fai clic su **OK**.

**Nota**: questa impostazione è specifica per l'archivio dati e non per l'host.


### 2. SIOC (Storage I/O Control) per {{site.data.keyword.BluVirtServers_short}}

Puoi anche limitare i singoli dischi virtuali per le singole VM oppure concedere loro condivisioni differenti con SIOC. La limitazione di dischi e la concessione di condivisioni differenti ti consente di mettere in corrispondenza e allineare l'ambiente al carico di lavoro con il numero di IOPS di volume {{site.data.keyword.filestorage_short}} acquisito. Il limite è impostato da IOPS e non è possibile impostare un peso o condivisioni ("Shares") differenti. Dischi virtuali con le condivisioni impostate a High (2.000 condivisioni) ricevono il doppio dell'I/O di un disco impostato su Normal (1.000 condivisioni) e il quadruplo di uno impostato su Low (500 condivisioni). Normal è il valore normale per tutte le VM; devi pertanto regolare i valori sopra o sotto Normal solo per le VM che effettivamente lo richiedono.


Utilizza la seguente procedura per modificare le condivisioni e il limite di vDisk:

1. Scegli un {{site.data.keyword.BluVirtServers_short}} nell'inventario **VMs and Templates**. L'icona si trova nella parte superiore sinistra della pagina web.
2. Seleziona i {{site.data.keyword.BluVirtServers_short}} per I/O Control.
3. Fai clic sulla scheda **Manage** e fai clic su **Settings**. Fai clic su **Edit**.
4. Espandi la freccia a discesa Hard disk. Modifica i valori di Shares o Limit-IOPs come appropriato per il tuo ambiente. Scegli un disco rigido virtuale dall'elenco e modifica la selezione Shares per scegliere la quantità relativa di condivisioni da allocare ai {{site.data.keyword.BluVirtServers_short}} (Low, Normal o High). Puoi anche fare clic su **Custom** e immettere un valore di condivisione definito dall'utente.
5. Fai clic sulla colonna Limit - IOPS e immetti il limite superiore di risorse di archiviazione da allocare alla macchina virtuale (VM, Virtual Machine).
6. Fai clic su **OK**


   **Nota**: il processo suindicato viene utilizzato per impostare i limiti di consumo di risorse dei singoli vDisk in un {{site.data.keyword.BluVirtServers_short}} anche quando SIOC non è abilitato. Queste impostazioni sono specifiche per il singolo guest, e non l'host, anche se sono utilizzate da SIOC.





## Impostazioni lato host ESXi

Ci sono alcune impostazioni aggiuntive necessarie per configurare gli host ESXi 5.x per l'archiviazione NFS.

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


- Utilizzo della CLI sull'host ESXi 5.x per i parametri di configurazione avanzata:
  I seguenti esempi utilizzano la CLI per impostare questi parametri di configurazione avanzata e per eseguirne quindi il controllo. Lo strumento esxcfg-advcfg utilizzato negli esempi è disponibile nella directory /usr/sbin sugli host ESXi 5.x.

   - Impostazione dei parametri di configurazione avanzata dalla CLI ESXi 5.x:
   ```
   #esxcfg-advcfg -s 32 /Net/TcpipHeapSize
   #esxcfg-advcfg -s 128 /Net/TcpipHeapMax(For vSphere 5.0/5.1)
   #esxcfg-advcfg -s 512 /Net/TcpipHeapMax(For vSphere 5.5 and above)
   #esxcfg-advcfg -s 256 /NFS/MaxVolumes
   #esxcfg-advcfg -s 256 /NFS41/MaxVolumes (ESXi 6.0 and above)
   #esxcfg-advcfg -s 10 /NFS/HeartbeatMaxFailures
   #esxcfg-advcfg -s 12 /NFS/HeartbeatFrequency
   #esxcfg-advcfg -s 5 /NFS/HeartbeatTimeout
   #esxcfg-advcfg -s 64 /NFS/MaxQueueDepth
   #esxcfg-advcfg -s 32 /Disk/QFullSampleSize
   #esxcfg-advcfg -s 8 /Disk/QFullThreshold
   ```

- Controllo dei parametri di configurazione avanzata dalla CLI ESXi 5.x:
   ```
   #esxcfg-advcfg -g /Net/TcpipHeapSize
   #esxcfg-advcfg -g /Net/TcpipHeapMax
   #esxcfg-advcfg -g /NFS/MaxVolumes
   #esxcfg-advcfg -g /NFS41/MaxVolumes (ESXi 6.0 and above)
   #esxcfg-advcfg -g /NFS/HeartbeatMaxFailures
   #esxcfg-advcfg -g /NFS/HeartbeatFrequency
   #esxcfg-advcfg -g /NFS/HeartbeatTimeout
   #esxcfg-advcfg -g /NFS/MaxQueueDepth
   #esxcfg-advcfg -g /Disk/QFullSampleSize
   #esxcfg-advcfg -g /Disk/QFullThreshold
   ```


## Abilitazione di frame jumbo in SoftLayer - Windows e Linux

{{site.data.keyword.BluSoftlayer_full}} ha dichiarato che, per realizzare appieno le velocità sull'archiviazione, è necessario abilitare i frame jumbo con MTU a 9.000.


Un frame jumbo è un frame Ethernet con un payload più grande della MTU (maximum transmission unit) standard di 1.500 byte. I frame jumbo sono utilizzati sulle LAN (local area network) che supportano almeno 1 Gbps e possono arrivare a una dimensione di 9.000 byte.


I frame jumbo devono essere configurati nello stesso modo nell'intero percorso di rete dal dispositivo di origine <-> switch <-> router <-> switch <-> dispositivo di destinazione. Se l'intera catena non è impostata nello stesso modo, verrà per impostazione predefinita utilizzata l'impostazione più bassa lungo la catena. SoftLayer ha i suoi dispositivi di rete impostati su 9.000, attualmente. Tutti i dispositivi cliente dovranno essere impostati allo stesso valore.

### Windows

1. Apri **Network and Sharing Center**.
2. Fai clic su **Change adapter settings**.
3. Fai clic con il pulsante destro del mouse sulla NIC per cui vuoi abilitare i frame jumbo e seleziona **Properties**.
4. Nella scheda **Networking**, fai clic su **Configure** per l'adattatore di rete.
5. Seleziona la scheda **Advanced**.
6. Seleziona **Jumbo Frame** e modifica il valore da disabled al valore desiderato, come MTU da 9kB o 9.014 byte, a seconda della NIC.
7. Fai clic su **OK** in tutte le finestre di dialogo.

Nota: quando effettui la modifica, la NIC perderà la connettività di rete per qualche secondo. Devi anche eseguire un riavvio per assicurarti che la modifica sia diventata effettiva.



### LINUX

1. Modifica il file di configurazione di rete per l'interfaccia eth0, ad esempio /etc/sysconfig/network-script/ifcfg-eth0 (CentOS / RHEL / Fedora Linux):
`# vi /etc/sysconfig/network-script/ifcfg-eth0`

2. Accoda la seguente direttiva di configurazione, che specifica la dimensione del frame in kbyte:
MTU 9000

3. Chiudi e salva il file. Riavvia l'interfaccia eth0:
`# /etc/init.d/networking restart (ciò causerà una breve perdita della connettività di rete)`


Una nota sull'utente Debian / Ubuntu Linux:
l'utente Debian / Ubuntu Linux deve aggiungere MTU=9000 al file di configurazione /etc/network/interfaces.

Trova ulteriori informazioni su Advanced Single-Site VMware Reference Architecture [qui](https://console.bluemix.net/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){:new_window}.
