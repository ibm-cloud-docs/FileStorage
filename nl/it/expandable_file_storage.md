---

copyright:
  years: 2014, 2017
lastupdated: "2017-12-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Capacità di condivisione file espandibile

Con questa nuova funzione, i nostri attuali utenti {{site.data.keyword.filestorage_full}} possono espandere la dimensione del loro {{site.data.keyword.filestorage_short}} esistente in incrementi di GB fino a 12 TB senza causare alcuna interruzione, senza dover creare un duplicato o migrare manualmente i dati a un volume più grande.  Non si verificherà alcuna interruzione o mancanza di accesso all'archiviazione mentre viene eseguita la modifica della dimensione. 

La fatturazione per il volume viene aggiornata per aggiungere la differenza calcolata proporzionalmente del nuovo prezzo al ciclo di fatturazione corrente e l'intero nuovo ammontare verrà fatturato nel prossimo ciclo di fatturazione.

Questa funzione è disponibile solo in [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html). 

## Perché avvantaggiarsi delle condivisioni file espandibili?

- **Gestione dei costi** – potresti essere consapevole che esiste un potenziale di crescita dei tuoi dati ma, per iniziare, ti serve una quantità di archiviazione più piccola. La possibilità di espansione consente ai nostri clienti di risparmiare sui costi di archiviazione all'inizio e di crescere successivamente per soddisfare le loro esigenze.  

- **Crescenti esigenze di archiviazione** - i clienti che riscontrano poi una rapida crescita oltre le loro aspettative hanno bisogno di un modo per aumentare rapidamente e facilmente la dimensione della loro memoria per gestire tale crescita.

## In che modo la modifica della dimensione dell'archiviazione si ripercuote sulla replica?

L'azione di espansione sull'archiviazione primaria determinerà una modifica della dimensione automatica della replica.

## Ci sono delle limitazioni?

Questa funzione è disponibile solo per l'archiviazione di cui viene eseguito il provisioning nei [data center](new-ibm-block-and-file-storage-location-and-features.html) con funzionalità migliorate. L'archiviazione crittografata di cui viene eseguito il provisioning in questi data center può essere aumentata fino a 12 TB. 

Le limitazioni di dimensione esistenti per {{site.data.keyword.filestorage_short}} di cui viene eseguito il provisioning con Endurance continuano a essere valide (fino a 4 TB per il livello a 10 IOPS e fino a 12 TB per tutti gli altri livelli).

## Come posso distinguere se la mia archiviazione di cui è stato eseguito il provisioning è espandibile?

L'archiviazione di cui è stato eseguito il provisioning con le funzionalità avanzate è sempre crittografata da inattiva.  Puoi identificare facilmente se la tua archiviazione è idonea poiché, se lo è, presenta accanto ad essa un'icona di "blocco" nell'IU del portale. 

## Come posso modificare la dimensione della mia archiviazione?

1. Dal {{site.data.keyword.slportal}}, fai clic su **Storage** > **{{site.data.keyword.filestorage_short}}** OPPURE, dal catalogo di {{site.data.keyword.BluSoftlayer_full}}, fai clic su **Infrastructure** > **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Seleziona il volume dall'elenco e fai clic su **Actions** > **Modify Volume**
3. Immetti la nuova dimensione dell'archiviazione in GB.
4. Riesamina la tua selezione e la nuova determinazione del prezzo.
5. Fai clic sulla casella di spunta **I have read the Master Service Agreement...** e fai clic su **Place Order**.
6. La tua nuova allocazione di archiviazione dovrebbe essere disponibile in pochi minuti.

