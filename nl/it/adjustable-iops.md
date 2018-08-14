---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-24"

---
{:new_window: target="_blank"}

# Regolazione dell'IOPS

Con questa nuova funzione, gli utenti dell'archiviazione {{site.data.keyword.blockstoragefull}} possono regolare l'IOPS del loro {{site.data.keyword.blockstorageshort}} esistente immediatamente. Non hanno bisogno di creare un duplicato o copiare manualmente i dati nella nuova archiviazione. Gli utenti non riscontrano alcun tipo di interruzione o mancanza di accesso all'archiviazione mentre viene eseguita la regolazione. 

La fatturazione per l'archiviazione viene aggiornata per aggiungere la differenza calcolata proporzionalmente del nuovo prezzo al ciclo di fatturazione corrente. La nuova quantità completa viene fatturata nel ciclo di fatturazione successivo.


## Vantaggi dell'IOPS regolabile

- Gestione dei costi – alcuni dei nostri clienti potrebbero avere bisogno di un IOPS elevato solo durante i tempi di utilizzo di picco. Ad esempio, un grosso negozio al dettaglio ha un utilizzo di picco durante i periodi festivi e potrebbe avere bisogno di un IOPS sulla sua archiviazione più elevato rispetto a quello necessario nel bel mezzo dell'estate. Questa funzione consente loro di gestire i loro costi e pagare un IOPS più elevato solo quando ne hanno bisogno.

## Limitazioni

Questa funzione è disponibile solo in [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html). 

I clienti non possono passare da Endurance a Performance e viceversa quando regolano il loro IOPS. Gli utenti possono specificare un nuovo livello IOPS o un livello IOPS per la loro archiviazione sulla base dei seguenti criteri e delle seguenti limitazioni.

- Se il volume originale è un livello Endurance 0,25, il livello IOPS non può essere aggiornato.
- Se il volume originale è Performance con meno di 0,30 IOPS/GB, le opzioni disponibili includono solo le combinazioni di dimensione e IOPS che danno come risultato meno di 0,30 IOPS/GB. 
- Se il volume originale è Performance con più di, o uguale a, 0,30 IOPS/GB, le opzioni disponibili includono solo le combinazioni di dimensione e IOPS che danno come risultato più di, o uguale a, 0,30 IOPS/GB. 

## Effetto della regolazione dell'IOPS sulla replica

Se per il volume è implementata la replica, quest'ultima viene aggiornata automaticamente in modo che corrisponda alla selezione IOPS di quello primario. 

## Regolazione dell'IOPS sulla tua archiviazione

1. Vai al tuo elenco di {{site.data.keyword.blockstorageshort}}
    - Dal portale clienti, fai clic su **Storage** > **{{site.data.keyword.filestorage_short}}** OPPURE
    - Dal catalogo di {{site.data.keyword.BluSoftlayer_full}}, fai clic su **Infrastruttura** > **Archiviazione** > **{{site.data.keyword.filestorage_short}}**. 
2. Seleziona il volume dall'elenco e fai clic su **Actions** > **Modify Volume**
3. In **Storage IOPS Options**, effettua una nuova selezione:
    - Endurance (IOPS a livelli): seleziona un livello IOPS superiore a 0,25 IOPS/GB della tua archiviazione. Puoi aumentare il livello IOPS in qualsiasi momento. Tuttavia, la diminuzione è disponibile solo una volta al mese.
    - Performance (IOPS allocato): specifica la nuova opzione IOPS per la tua archiviazione immettendo un valore compreso nell'intervallo 100 - 48.000 IOPS. (Assicurati di esaminare gli eventuali limiti specifici richiesti in base alla dimensione nel modulo dell'ordine).
4. Riesamina la tua selezione e la nuova determinazione del prezzo.
5. Fai clic sulla casella di spunta **I have read the Master Service Agreement...** e fai clic su **Place Order**.
6. La tua nuova allocazione di archiviazione sarà disponibile in pochi minuti.
