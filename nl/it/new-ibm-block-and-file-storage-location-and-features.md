---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, locations, data centers

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Nuove ubicazioni e funzioni
{: #news}

{{site.data.keyword.cloud}} sta introducendo una nuova versione di {{site.data.keyword.filestorage_full}}! La nuova archiviazione è disponibile in data center selezionati ed è supportata da una archiviazione flash a livelli IOPS più elevati con la crittografia a livello di disco per i dati non attivi. Di tutta l'archiviazione ordinata in data center selezionati, viene automaticamente creata con la nuova versione di {{site.data.keyword.filestorage_short}}.

Il punto di montaggio NFS per i nuovi volumi è cambiato. Per i dettagli, consulta la sezione [Nuovo punto di montaggio per i volumi di {{site.data.keyword.filestorage_short}} avanzato](#new-mount-point-for-enhanced-file-storage-volumes).
{:important}

## Nuove ubicazioni
{: #new-locations}

La nuova {{site.data.keyword.filestorage_short}} è disponibile nelle seguenti regioni e nei seguenti data center e a breve verrà aggiunta la disponibilità di ulteriori data center.

<table role="presentation">
  <tr>
    <td><strong>US 2</strong></td>
    <td><strong>UE</strong></td>
    <td><strong>Australia</strong></td>
    <td><strong>Canada</strong></td>
    <td><strong>America Latina</strong></td>
    <td><strong>Asia Pacifico</strong></td>
  </tr>
  <tr>
    <td>DAL09<br />
	DAL10<br />
	DAL12<br />
	DAL13<br />
	SJC03<br />
        SJC04<br />
	WDC04<br />
	WDC06<br />
	WDC07<br />
	<br /><br /><br />
    </td>
    <td>AMS01<br />
        AMS03<br />
	FRA02<br />
	FRA04<br />
	FRA05<br />
	LON02<br />
	LON04<br />
	LON05<br />
	LON06<br />
	MIL01<br />
	OSLO1<br />
	PAR01<br />
    </td>
    <td>MEL01<br />
        SYD01<br />
        SYD04<br />
        SYD05<br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>MON01<br />
        TOR01<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>MEX01<br />
        SAO01<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>CHE01<br />
        HKG02<br />
	SEO01<br />
	SNG01<br />
        TOK02<br />
	TOK04<br />
	TOK05<br />
	<br /><br /><br /><br /><br />
    </td>
  </tr>
</table>

*La tabella 1 mostra la nostra disponibilità di data center. Ogni regione ha la propria colonna. Alcune città, come Dallas, San Jose, Washington DC, Amsterdam, Francoforte, Londra e Sydney, hanno più data center.*

## Nuove funzioni e funzionalità
{: #features}

- [Crittografia gestita dal provider per i dati inattivi](/docs/infrastructure/FileStorage?topic=FileStorage-encryption). <br/> Viene eseguito automaticamente il provisioning di tutti i volumi {{site.data.keyword.filestorage_short}} come crittografati senza costi aggiuntivi.
- Opzione livello 10 IOPS per GB. <br/> Un nuovo livello è stato aggiunto a {{site.data.keyword.filestorage_short}} di tipo Endurance per supportare i carichi di lavoro più esigenti.
- Archiviazione con supporto all-flash. <br/> {{site.data.keyword.filestorage_short}} di cui viene eseguito il provisioning con le opzioni Endurance o Performance a 2 IOPS per GB o superiore viene eseguito il backup dall'archiviazione con supporto all-flash.
- Supporto di istantanea e replica.
- L'opzione di fatturazione oraria aggiunta per l'archiviazione di cui è pianificato l'utilizzo per meno di un intero mese.
- Fino a 48.000 IOPS per {{site.data.keyword.filestorage_short}} di cui viene eseguito il provisioning con il tipo Performance.
- I tassi di IOPS sono regolabili per migliorare le prestazioni di cambiamenti del carico stagionali. Leggi ulteriori informazioni su questa funzione [qui](/docs/infrastructure/FileStorage?topic=FileStorage-adjustingIOPS).
- Crea un clone dei tuoi dati con la [funzione di duplicazione del volume di {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume).
- L'archiviazione è espandibile in incrementi di GB fino a 12 TB immediatamente, senza dover creare un duplicato o eseguire manualmente lo spostamento dei dati a un volume più grande. Leggi ulteriori informazioni su questa funzione [qui](/docs/infrastructure/FileStorage?topic=FileStorage-expandCapacity).

## Nuovo punto di montaggio per i volumi di {{site.data.keyword.filestorage_short}} avanzato

Tutti i volumi di {{site.data.keyword.filestorage_short}} avanzato di cui viene eseguito il provisioning in questi data center hanno un punto di montaggio diverso rispetto ai volumi non crittografati. Per assicurarti che stai usando il punto di montaggio corretto per entrambi i volumi di archiviazione, puoi visualizzare le informazioni sul punto di montaggio nella pagina **Volume Details** nella console. Puoi inoltre accedere al punto di montaggio corrente tramite una chiamata API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Per poter accedere a tutte le nuove funzioni, seleziona `Storage-as-a-Service Package 759` quando effettui il tuo ordine tramite l'API. Per ulteriori informazioni sull'ordinazione di {{site.data.keyword.filestorage_short}} tramite l'API, vedi [order_file_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/file/#SoftLayer.managers.file.FileStorageManager.order_file_volume){: external}.
{:important}

Ritorna qui a controllare quando viene eseguito l'upgrade di ulteriori data center e per vedere le nuove funzioni e funzionalità che vengono aggiunte per {{site.data.keyword.filestorage_short}}.
{:tip}
