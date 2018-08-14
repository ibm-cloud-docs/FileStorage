---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-20"

---
{:new_window: target="_blank"}

# Nuove ubicazioni e funzioni dell'{{site.data.keyword.filestorage_short}}

{{site.data.keyword.BluSoftlayer_full}} sta introducendo una nuova versione dell'{{site.data.keyword.filestorage_full}}! La nuova archiviazione è disponibile in data center selezionati ed è supportata da una archiviazione flash a livelli IOPS più elevati con la crittografia a livello di disco per i dati non attivi. Di tutta l'archiviazione ordinata in data center selezionati, viene automaticamente creata con la nuova versione di {{site.data.keyword.filestorage_short}}.

**Nota:** il punto di montaggio NFS per i nuovi volumi è cambiato. Per i dettagli, consulta la sezione **Nuovo punto di montaggio per i volumi di {{site.data.keyword.filestorage_short}} avanzato**.

La nuova {{site.data.keyword.filestorage_short}} è disponibile nelle seguenti regioni/nei seguenti data center e a breve verrà aggiunta la disponibilità di ulteriori data center.

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
		<td><p>SJC03<br />
			SJC04<br />
			DC04<br />
			WDC06<br />
			WDC07<br />
			DAL09<br />
			DAL10<br />
			DAL12<br />
			DAL13<br /><br /><br /></p>
		</td>
		<td><p>LON02<br />
			LON04<br />
			LON06<br />
			FRA02<br />
			FRA04<br />
			FRA05<br />
			AMS01<br />
			AMS03<br />
			OSLO1<br />
			PAR01<br />
			MIL01<br /></p>
		</td>
		<td><p>SYD01<br />
			SYD04<br />
			MEL01<br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
		</td>
		<td><p>TOR01<br />
			MON01<br /><br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
		</td>
		<td><p>MEX01<br />
			SAO01<br /><br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
		</td>
		<td><p>TOK02<br />
			HKG02<br />
			SEO01<br />
			SNG01<br />
			CHE01<br /><br /><br /><br /><br /><br /><br /></p>
		</td>
	</tr>
</table>

*La tabella 1 mostra la nostra disponibilità di data center. Ogni regione ha la propria colonna. Alcune città, come Dallas, San Jose, Washington DC, Amsterdam, Francoforte, Londra e Sydney hanno più data center.*

La nuova archiviazione ha le seguenti funzioni e capacità:

- [Crittografia gestita dal provider per i dati inattivi](block-file-storage-encryption-rest.html). <br/> Viene eseguito automaticamente il provisioning di tutti i volumi {{site.data.keyword.filestorage_short}} come crittografati senza costi aggiuntivi.
- Opzione livello 10 IOPS per GB. <br/> Un nuovo livello è stato aggiunto a {{site.data.keyword.filestorage_short}} di tipo Endurance per supportare i carichi di lavoro più esigenti.
- Archiviazione con supporto all-flash. <br/> {{site.data.keyword.filestorage_short}} di cui viene eseguito il provisioning con le opzioni Endurance o Performance a 2 IOPS per GB o superiore viene eseguito il backup dall'archiviazione con supporto all-flash.
- Supporto di istantanea e replica.
- L'opzione di fatturazione oraria aggiunta per l'archiviazione di cui è pianificato l'utilizzo per meno di un intero mese.
- Fino a 48.000 IOPS per {{site.data.keyword.filestorage_short}} di cui viene eseguito il provisioning con il tipo Performance.
- I tassi di IOPS sono regolabili per migliorare le prestazioni di cambiamenti del carico stagionali. Leggi ulteriori informazioni su questa funzione [qui](adjustable-iops.html).
- Crea un clone dei tuoi dati con la [funzione di duplicazione del volume di {{site.data.keyword.filestorage_short}}](how-to-create-duplicate-volume.html).
- L'archiviazione è espandibile in incrementi di GB fino a 12 TB immediatamente, senza dover creare un duplicato o eseguire manualmente lo spostamento dei dati a un volume più grande. Leggi ulteriori informazioni su questa funzione [qui](expandable_file_storage.html).

## Nuovo punto di montaggio per i volumi di {{site.data.keyword.filestorage_short}} avanzato

Tutti i volumi di {{site.data.keyword.filestorage_short}} avanzato di cui viene eseguito il provisioning in questi data center hanno un punto di montaggio diverso rispetto ai volumi non crittografati. Per assicurarti che stai usando il punto di montaggio corretto per entrambi i tuoi volumi, puoi visualizzare le informazioni sul punto di montaggio nella pagina **Volume Details** nell'IU. Puoi inoltre accedere al punto di montaggio corrente tramite una chiamata API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Ritorna qui a controllare quando viene eseguito l'upgrade di ulteriori data center e per vedere le nuove funzioni e funzionalità che vengono aggiunte per l'{{site.data.keyword.filestorage_short}}.
