---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-18"

keywords: File Storage, modify volume, NFS, file storage, expand capacity

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Espansione della capacità di condivisione file
{: #expandCapacity}

Con questa nuova funzione, gli utenti correnti di {{site.data.keyword.filestorage_full}} possono espandere la dimensione del loro {{site.data.keyword.filestorage_short}} in incrementi di GB fino a 12 TB immediatamente. Non hanno bisogno di creare un duplicato o migrare manualmente i dati a un volume più grande. Non si verificherà alcuna interruzione o mancanza di accesso all'archiviazione mentre viene eseguita la modifica della dimensione.

La fatturazione per il volume viene aggiornata per aggiungere la differenza calcolata proporzionalmente del nuovo prezzo al ciclo di fatturazione corrente. Successivamente, la nuova quantità completa viene fatturata nel ciclo di fatturazione successivo.

Questa funzione è disponibile solo in [data center selezionati](/docs/infrastructure/FileStorage?topic=FileStorage-selectDC).

## Vantaggi dell'archiviazione espandibile

- **Gestione dei costi** – potresti essere consapevole che esiste un potenziale di crescita dei tuoi dati ma, per iniziare, ti serve una quantità di archiviazione più piccola. La possibilità di espansione consente ai clienti di risparmiare sui costi di archiviazione all'inizio e di crescere successivamente per soddisfare le loro esigenze.  

- **Crescenti esigenze di archiviazione** - i clienti che riscontrano poi una rapida crescita oltre le loro aspettative hanno bisogno di un modo per aumentare rapidamente e facilmente la dimensione della loro memoria per gestire tale crescita.

## Effetti dell'espansione della capacità di archiviazione sulla replica

L'azione di espansione sull'archiviazione primaria determina una modifica della dimensione automatica della replica.

## Limitazioni
{: #limitsofextension}

Questa funzione è disponibile solo per l'archiviazione di cui viene eseguito il provisioning nei [data center](/docs/infrastructure/FileStorage?topic=FileStorage-selectDC) con funzionalità migliorate. L'archiviazione crittografata di cui viene eseguito il provisioning in questi data center può essere aumentata fino a 12 TB.

Le limitazioni di dimensione esistenti per {{site.data.keyword.filestorage_short}} di cui era stato eseguito il provisioning con Endurance continuano a essere valide (fino a 4 TB per il livello a 10 IOPS e fino a 12 TB per tutti gli altri livelli).

## Ridimensionamento dell'archiviazione
{: #resizingsteps}

1. Vai alla [console {{site.data.keyword.cloud}}](https://{DomainName}/){: external}. Dal menu, seleziona **Classic Infrastructure**. Fai clic su **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Seleziona il volume dall'elenco e fai clic su **Actions** > **Modify Volume**
3. Immetti la nuova dimensione dell'archiviazione in GB.
4. Riesamina la tua selezione e la nuova determinazione del prezzo.
5. Fai clic su **I have read the Master Service Agreement...** e fai clic su **Place Order**.
6. La tua nuova allocazione di archiviazione è disponibile in pochi minuti.

In alternativa, puoi utilizzare il seguente comando nella SLCLI.
```
# slcli file volume-modify --help
Usage: slcli file volume-modify [OPTIONS] VOLUME_ID

Options:
  -c, --new-size INTEGER        New Size of file volume in GB. ***If no size
                                is given, the original size of volume is
                                used.***
                                Potential Sizes: [20, 40, 80, 100,
                                250, 500, 1000, 2000, 4000, 8000, 12000]
                                Minimum: [the original size of the volume]
  -i, --new-iops INTEGER        Performance Storage IOPS, between 100 and 6000
                                in multiples of 100 [only for performance
                                volumes] ***If no IOPS value is specified, the
                                original IOPS value of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is less than 0.3, new IOPS/GB
                                must also be less than 0.3. If original
                                IOPS/GB for the volume is greater than or
                                equal to 0.3, new IOPS/GB for the volume must
                                also be greater than or equal to 0.3.]
  -t, --new-tier [0.25|2|4|10]  Endurance Storage Tier (IOPS per GB) [only for
                                endurance volumes] ***If no tier is specified,
                                the original tier of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is 0.25, new IOPS/GB for the
                                volume must also be 0.25. If original IOPS/GB
                                for the volume is greater than 0.25, new
                                IOPS/GB for the volume must also be greater
                                than 0.25.]
  -h, --help      Show this message and exit.
```
