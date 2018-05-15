---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-23"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Protezione dei tuoi dati - crittografia dei dati inattivi gestita dal provider 

{{site.data.keyword.BluSoftlayer_full}} prende sul serio la sicurezza e comprende l'importanza di poter crittografare i dati per tenerli al sicuro. Con la crittografia gestita dal provider, {{site.data.keyword.filestorage_full}} di cui è stato eseguito il provisioning sia con Endurance che con Performance viene crittografato per impostazione predefinita senza costi aggiuntivi e ripercussioni sulle prestazioni.

La funzione di crittografia dei dati inattivi gestita dal provider utilizza i seguenti protocolli standard del settore:

* Crittografia AES-256 standard del settore
* Le chiavi sono gestite internamente con il KMIP (Key Management Interoperability Protocol) standard del settore
* L'archiviazione è convalidata in base allo standard FIPS PUB (Federal Information Processing Standard Publication) 140-2 per la conformità a FISMA (Federal Information Security Management Act), HIPAA (Health Insurance Portability and Accountability Act), PCI (Payment Card Industry), Basel II, California Security Breach Information Act (SB 1386) e alla direttiva sulla protezione dei dati dell'Unione europea 95/46/EC

## Crittografia dei dati inattivi per istantanee o archiviazione replicata  

Per impostazione predefinita, vengono crittografate anche tutte le istantanee e le repliche dell'archiviazione file crittografata. Questa funzione non può essere disattivata in base ai singoli volumi.

## Provisioning di archiviazione con la crittografia

La funzione di crittografia dei dati inattivi gestita dal provider è disponibile solo per {{site.data.keyword.filestorage_short}} di cui viene eseguito il provisioning in data center selezionati, con una regolare aggiunta di nuova disponibilità di data center. Tutta l'archiviazione di cui viene eseguito il provisioning in questi data center è automaticamente dotata della crittografia per i dati inattivi. Fai clic [qui](new-ibm-block-and-file-storage-location-and-features.html) per visualizzare l'elenco corrente di data center dove è disponibile la crittografia dell'{{site.data.keyword.filestorage_short}} per i dati inattivi.


Quando ordini la tua {{site.data.keyword.filestorage_short}}, seleziona un data center indicato con un asterisco (`*`) e un messaggio che indica la disponibilità della crittografia. Vedrai un'icona di blocco a destra del campo LUN/Volume Name che indica che è crittografato. Vedi la Figura 1.

![L'icona di blocco indica che la LUN è crittografata](/images/encryptedstorage.png)
<caption>Figura 1. Esempio di icona di blocco che indica che il volume è crittografato.</caption>



**Nota**: l'archiviazione non crittografata di cui era stato eseguito il provisioning prima dell'upgrade a un data center **non** verrà crittografata automaticamente. Se hai dell'archiviazione non crittografata in un data center di cui è stato eseguito l'upgrade, dovrai creare un nuovo volume ed eseguire una migrazione dei dati. Il seguente articolo può fornire delle indicazioni.

* [Migrazione dell'archiviazione file nei data center di cui è stato eseguito l'upgrade](migrate-file-storage-encrypted-file-storage.html)
