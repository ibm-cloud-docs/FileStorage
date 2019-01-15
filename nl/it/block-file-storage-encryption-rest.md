---

copyright:
  years: 2014, 2019
lastupdated: "2019-01-07"

---
{:tip: .tip}
{:note: .note}
{:important: .important}

# Crittografia dei dati inattivi gestita dal provider

{{site.data.keyword.BluSoftlayer_full}} prende sul serio la sicurezza e comprende l'importanza di poter crittografare i dati per tenerli al sicuro. Con la crittografia gestita dal provider, {{site.data.keyword.filestorage_full}} di cui è stato eseguito il provisioning con le opzioni Endurance o Performance, viene crittografato per impostazione predefinita senza costi aggiuntivi e ripercussioni sulle prestazioni.

La funzione di crittografia dei dati inattivi gestita dal provider utilizza i seguenti protocolli standard del settore:

* Crittografia AES-256 standard del settore
* Le chiavi sono gestite internamente con il KMIP (Key Management Interoperability Protocol) standard del settore
* L'archivio è convalidato con i seguenti standard:
    - FIPS (Federal Information Processing Standard) Publication 140-2,
    - FISMA (Federal Information Security Management Act),
    - HIPAA (Health Insurance Portability and Accountability Act),
    - PCI (Payment Card Industry),
    - Basel II,
    - California Security Breach Information Act (SB 1386) e
    - Direttiva sulla protezione dei dati dell'Unione europea (95/46/EC).

## Protezione delle tue istantanee o della tua archiviazione replicata  

Per impostazione predefinita, vengono crittografate anche tutte le istantanee e le repliche dell'archiviazione file crittografata. Questa funzione non può essere disattivata in base ai singoli volumi.

## Provisioning di archiviazione con la crittografia

La funzione di crittografia dei dati inattivi gestita dal provider è disponibile in data center selezionati. Tutta l'archiviazione ordinata in questi data center è automaticamente dotata della crittografia per i dati inattivi. Fai clic [qui](new-ibm-block-and-file-storage-location-and-features.html) per visualizzare l'elenco corrente di data center dove è disponibile la crittografia di {{site.data.keyword.filestorage_short}}.

Quando ordini {{site.data.keyword.filestorage_short}}, seleziona un data center indicato con un asterisco (`*`). Puoi vedere un'icona di blocco a destra del campo LUN/Volume Name che indica che il volume è crittografato. Vedi la Figura 1.

![L'icona di blocco indica che il LUN è crittografato](/images/encryptedstorage.png)
<caption>Figura 1. Esempio di icona di blocco che indica che il volume è crittografato.</caption>

Tutta l'archiviazione non crittografata di cui era stato eseguito il provisioning prima dell'upgrade a un data center **non** viene crittografata automaticamente. Se hai dell'archiviazione non crittografata in un data center di cui è stato eseguito l'upgrade e vuoi che venga crittografata, devi creare un nuovo volume e spostare i tuoi dati. Per ulteriori informazioni, vedi [Migrazione dell'archiviazione file nei data center di cui è stato eseguito l'upgrade](migrate-file-storage-encrypted-file-storage.html)
{:important}
