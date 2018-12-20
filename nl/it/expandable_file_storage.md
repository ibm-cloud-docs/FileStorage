---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-12"

---
{:new_window: target="_blank"}

# Espansione della capacità di condivisione file

Con questa nuova funzione, gli utenti correnti di {{site.data.keyword.filestorage_full}} possono espandere la dimensione del loro {{site.data.keyword.filestorage_short}} in incrementi di GB fino a 12 TB immediatamente. Non hanno bisogno di creare un duplicato o migrare manualmente i dati a un volume più grande. Non si verificherà alcuna interruzione o mancanza di accesso all'archiviazione mentre viene eseguita la modifica della dimensione.

La fatturazione per il volume viene aggiornata per aggiungere la differenza calcolata proporzionalmente del nuovo prezzo al ciclo di fatturazione corrente. Successivamente, la nuova quantità completa viene fatturata nel ciclo di fatturazione successivo.

Questa funzione è disponibile solo in [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html).

## Vantaggi dell'archiviazione espandibile

- **Gestione dei costi** – potresti essere consapevole che esiste un potenziale di crescita dei tuoi dati ma, per iniziare, ti serve una quantità di archiviazione più piccola. La possibilità di espansione consente ai clienti di risparmiare sui costi di archiviazione all'inizio e di crescere successivamente per soddisfare le loro esigenze.  

- **Crescenti esigenze di archiviazione** - i clienti che riscontrano poi una rapida crescita oltre le loro aspettative hanno bisogno di un modo per aumentare rapidamente e facilmente la dimensione della loro memoria per gestire tale crescita.

## Effetti dell'espansione della capacità di archiviazione sulla replica

L'azione di espansione sull'archiviazione primaria determina una modifica della dimensione automatica della replica.

## Limitazioni

Questa funzione è disponibile solo per l'archiviazione di cui viene eseguito il provisioning nei [data center](new-ibm-block-and-file-storage-location-and-features.html) con funzionalità migliorate. L'archiviazione crittografata di cui viene eseguito il provisioning in questi data center può essere aumentata fino a 12 TB.

Le limitazioni di dimensione esistenti per {{site.data.keyword.filestorage_short}} di cui era stato eseguito il provisioning con Endurance continuano a essere valide (fino a 4 TB per il livello a 10 IOPS e fino a 12 TB per tutti gli altri livelli).

## Ridimensionamento dell'archiviazione

1. Dal {{site.data.keyword.slportal}}, fai clic su **Storage** > **{{site.data.keyword.filestorage_short}}** OPPURE, dal catalogo di {{site.data.keyword.BluSoftlayer_full}}, fai clic su **Infrastruttura** > **Archiviazione** > **{{site.data.keyword.filestorage_short}}**.
2. Seleziona il volume dall'elenco e fai clic su **Actions** > **Modify Volume**
3. Immetti la nuova dimensione dell'archiviazione in GB.
4. Riesamina la tua selezione e la nuova determinazione del prezzo.
5. Fai clic su **I have read the Master Service Agreement...** e fai clic su **Place Order**.
6. La tua nuova allocazione di archiviazione è disponibile in pochi minuti.
