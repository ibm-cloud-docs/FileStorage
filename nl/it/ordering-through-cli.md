---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, SLCLI, provisioning, API

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Ordinazione di {{site.data.keyword.filestorage_short}} tramite la SLCLI
{: #orderingSLCLI}

Puoi utilizzare la SLCLI per effettuare degli ordini per prodotti che vengono normalmente ordinati tramite il [{{site.data.keyword.slportal}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/){:new_window}. Nell'API SL, un ordine può essere costituito da più contenitori di ordine. La CLI degli ordini funziona solo con un singolo contenitore di ordine.

Per ulteriori informazioni su come installare e utilizzare la SLCLI, vedi [Python API Client ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}.
{:tip}

## Ricerca di offerte di {{site.data.keyword.filestorage_short}} disponibili

Il primo componente da cercare quando effettui un ordine è un pacchetto. I pacchetti sono divisi tra i diversi prodotti di livello superiore disponibili per l'ordinazione in {{site.data.keyword.BluSoftlayer_full}}. Alcuni pacchetti di esempio sono CLOUD_SERVER per le VSI, BARE_METAL_SERVER per i server bare metal e STORAGE_AS_A_SERVICE_STAAS per {{site.data.keyword.filestorage_short}} e {{site.data.keyword.blockstorageshort}}.

All'interno di un pacchetto, alcuni elementi sono suddivisi in categorie. Alcuni pacchetti hanno delle preimpostazioni per tua comodità e altri richiedono che gli elementi vengano specificati singolarmente. Se la categoria di un pacchetto è richiesta, per ordinare il pacchetto è necessario che venga scelto un elemento da tale categoria. A seconda della categoria, alcuni elementi all'interno della categoria potrebbero escludersi a vicenda.

Ogni ordine deve avere un'ubicazione associata (data center). Quando ordini {{site.data.keyword.filestorage_short}}, assicurati che ne venga eseguito il provisioning nella stessa ubicazione delle tue istanze di calcolo.
{:important}

Puoi utilizzare il comando `slcli order package-list` per trovare il pacchetto che vuoi ordinare. Viene fornita un'opzione `–keyword` per eseguire semplici operazioni di ricerca e filtro. Questa opzione semplifica la ricerca del pacchetto di cui hai bisogno. Cerca `Storage-as-a-Service Package 759`.

```
$ slcli order package-list --help
Usage: slcli order package-list [OPTIONS]

  List packages that can be ordered via the placeOrder API.

  Example:
      # List out all packages for ordering
      slcli order package-list

  Keywords can also be used for some simple filtering functionality to help
  find a package easier.

  Example:
     # List out all packages with "server" in the name
      slcli order package-list --keyword server

Options:
  --keyword TEXT  A word (or string) used to filter package names.
  -h, --help      Show this message and exit.
```

Puoi anche utilizzare il comando `slcli file volume-order`.

```
# slcli file volume-order --help
Usage: slcli file volume-order [OPTIONS]

  Order a file storage volume.

Options:
  --storage-type [performance|endurance]
                                  Type of file storage volume  [required]
  --size INTEGER                  Size of file storage volume in GB
                                  [required]
  --iops INTEGER                  Performance Storage IOPs, between 100 and
                                  6000 in multiples of 100  [required for
                                  storage-type performance]
  --tier [0.25|2|4|10]            Endurance Storage Tier (IOP per GB)
                                  [required for storage-type endurance]
  --location TEXT                 Datacenter short name (e.g.: dal09)
                                  [required]
  --snapshot-size INTEGER         Optional parameter for ordering snapshot
                                  space along with endurance file storage;
                                  specifies the size (in GB) of snapshot space
                                  to order
  --service-offering [storage_as_a_service|enterprise|performance]
                                  The service offering package to use for
                                  placing the order [optional, default is
                                  'storage_as_a_service']
  --billing [hourly|monthly]      Optional parameter for Billing rate (default
                                  to monthly)
  -h, --help                      Show this message and exit.
```

Per ulteriori informazioni sull'ordinazione di {{site.data.keyword.filestorage_short}} tramite l'API, vedi [order_file_volume ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://softlayer-python.readthedocs.io/en/latest/api/managers/file.html#SoftLayer.managers.file.FileStorageManager.order_file_volume){:new_window}.
Per poter accedere a tutte le nuove funzioni, ordina `Storage-as-a-Service Package 759`.
{:tip}


## Effettuazione dell'ordine

Il seguente esempio mostra come ordinare un volume {{site.data.keyword.filestorage_short}} da 10-GB con 100 IOPS per ogni GB.

```
# slcli file volume-order --storage-type performance --size 20 --location dal10 --iops 100
Order #32076317 placed successfully!
> Storage as a Service
> File Storage
> 20 GBs
> 100 IOPS
```

Per impostazione predefinita, puoi eseguire il provisioning di un totale combinato di 250 volumi {{site.data.keyword.blockstorageshort}} e {{site.data.keyword.filestorage_short}}. Per aumentare il numero dei tuoi volumi, contatta il tuo rappresentante di vendita. Per ulteriori informazioni sull'aumento dei limiti, vedi [Gestione dei limiti di archiviazione](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits).
{:important}

## Autorizzazione degli host ad accedere alla nuova archiviazione

```
# slcli file access-authorize --help
Usage: slcli file access-authorize [OPTIONS] VOLUME_ID

  Authorizes hosts to access a given volume

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to authorize
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to authorize
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to authorize
  --ip-address TEXT         An IP address to authorize
  -s, --subnet-id TEXT      The id of one SoftLayer_Network_Subnet to
                            authorize
  --help                    Show this message and exit.
```

Per ulteriori informazioni sull'autorizzazione degli host ad accedere al {{site.data.keyword.filestorage_short}} tramite l'API, vedi [authorize_host_to_volume ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://softlayer-python.readthedocs.io/en/latest/api/managers/file.html#SoftLayer.managers.file.FileStorageManager.authorize_host_to_volume){:new_window}.
{:tip}

Per ulteriori informazioni sul limite di autorizzazioni simultanee, consulta le [FAQ](/docs/infrastructure/FileStorage?topic=FileStorage-faqs).
{:important}

## Connessione della tua nuova archiviazione
{: #mountingvolumesCLI}

A seconda del sistema operativo del tuo host, segui il link appropriato.
- [Montaggio di {{site.data.keyword.filestorage_short}} su Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [Montaggio di {{site.data.keyword.filestorage_short}} in CentOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [Montaggio di {{site.data.keyword.filestorage_short}} su Container Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)
- [Configurazione di {{site.data.keyword.filestorage_short}} per il backup con cPanel](/docs/infrastructure/FileStorage?topic=FileStorage-cPanelBackups)
- [Configurazione di {{site.data.keyword.filestorage_short}} per il backup con Plesk](/docs/infrastructure/FileStorage?topic=FileStorage-PleskBackup)
- [Montaggio del volume {{site.data.keyword.filestorage_short}} sugli host ESXi](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)
