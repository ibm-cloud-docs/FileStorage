---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Ordinazione di {{site.data.keyword.filestorage_short}} tramite la CLI SL

Puoi utilizzare la CLI SL per effettuare degli ordini per prodotti che vengono normalmente ordinati tramite il [{{site.data.keyword.slportal}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/){:new_window}. Nell'API SL, un ordine può essere costituito da più contenitori di ordine. La CLI degli ordini funziona solo con un singolo contenitore di ordine.

Per ulteriori informazioni su come installare e utilizzare la CLI SL, vedi [Python API Client](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}.
{:tip}

## Ricerca di offerte di {{site.data.keyword.filestorage_short}} disponibili

Il primo componente da cercare quando effettui un ordine è un pacchetto. I pacchetti sono divisi tra i diversi prodotti di livello superiore disponibili per l'ordinazione in {{site.data.keyword.BluSoftlayer_full}}. Alcuni pacchetti di esempio sono CLOUD_SERVER per le VSI, BARE_METAL_SERVER per i server bare metal e STORAGE_AS_A_SERVICE_STAAS per {{site.data.keyword.filestorage_short}} e {{site.data.keyword.blockstorageshort}}.

All'interno di un pacchetto, alcuni elementi sono suddivisi in categorie. Alcuni pacchetti hanno delle preimpostazioni per tua comodità e altri richiedono che gli elementi vengano specificati singolarmente. Se la categoria di un pacchetto è richiesta, per ordinare il pacchetto è necessario che venga scelto un elemento da tale categoria. A seconda della categoria, alcuni elementi all'interno della categoria potrebbero escludersi a vicenda.

Ogni ordine deve avere un'ubicazione associata (data center). Quando ordini {{site.data.keyword.filestorage_short}}, assicurati che ne venga eseguito il provisioning nella stessa ubicazione delle tue istanze di calcolo.
{:important}

Puoi utilizzare il comando `slcli order package-list` per trovare il pacchetto che vuoi ordinare. Viene fornita un'opzione `–keyword` per eseguire semplici operazioni di ricerca e filtro. Questa opzione semplifica la ricerca del pacchetto di cui hai bisogno.

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

*Necessità delle istruzioni relative a come trovare il pacchetto Storage-as-a-Service 759*

```
$ slcli order package-list --keyword "Storage"
:.....................:.....................:
:         name        :       keyName       :
:.....................:.....................:
: ???                 : ???                 :
: ???                 : ???                 :
:.....................:.....................:
```

```
$ slcli order category-list STORAGE_AS_A_SERVICE_STAAS --required
:..................................:...................:............:
:               name               :    categoryCode   : isRequired :
:..................................:...................:............:
:              Example             :        ???        :     Y      :
:              Example             :        ???        :     Y      :
:              Example             :        ???        :     Y      :
:              Example             :        ???        :     Y      :
:..................................:...................:............:
```

Seleziona il resto dei tuoi elementi per l'ordine utilizzando il comando `item-list`. I pacchetti di norma hanno numerosi elementi da cui scegliere; utilizza pertanto l'opzione `–category` per richiamare gli elementi solo dalla categoria a cui sei interessato.

```
$ slcli order item-list STORAGE_AS_A_SERVICE_STAAS --category ??
:..........................:..............................................:
:         keyName          :                description                   :
:..........................:..............................................:
:           ???            :                    ????                      :
:           ???            :                    ????                      :
:           ???            :                    ????                      :
:           ???            :                    ????                      :
:..........................:..............................................:
```

Per ulteriori informazioni sull'ordinazione di {{site.data.keyword.filestorage_short}} tramite l'API, vedi [order_file_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/file.html#SoftLayer.managers.file.FileStorageManager.order_file_volume){:new_window}.
Per poter accedere a tutte le nuove funzioni, ordina `Storage-as-a-Service Package 759`.
{:tip}

## Verifica dell'ordine

Se ti capita di non essere sicuro delle categorie richieste che potrebbero mancare nel tuo ordine, puoi utilizzare il comando `place` con l'indicatore `–verify`. Se manca qualche categoria, viene visualizzata sullo schermo.


```
$ slcli order place --verify blablabla
:..............................................:.................................................:......:
:                keyName                       :                   description                   : cost :
:..............................................:.................................................:......:
:                  ???                         :                 yadi yadi yada                  :  0   :
:                  ???                         :                 yadi yadi yada                  :  0   :
:                  ???                         :                 yadi yadi yada                  :  0   :
:                  ???                         :                 yadi yadi yada                  :  0   :
:..............................................:.................................................:......:
```

L'output mostra ogni elemento ordinato, insieme al costo ad esso associato. Se l'ordine supera la verifica, significa che non ci sono elementi in conflitto e che tutte le categorie richieste hanno un elemento specificato nell'ordine.

## Effettuazione dell'ordine

Il passo successivo consiste nell'effettuare l'ordine.

```
$ slcli order place .....

This action will incur charges on your account. Continue? [y/N]: y

API response
```

Per impostazione predefinita, puoi eseguire il provisioning di un totale combinato di 250 volumi {{site.data.keyword.filestorage_short}}. Per aumentare il numero dei tuoi volumi, contatta il tuo rappresentante di vendita. Per ulteriori informazioni sull'aumento dei limiti, vedi [Gestione dei limiti di archiviazione](managing-storage-limits.html).{:important}

## Autorizzazione degli host ad accedere alla nuova archiviazione

Da definire

Per ulteriori informazioni sull'autorizzazione degli host ad accedere al {{site.data.keyword.filestorage_short}} tramite l'API, vedi [authorize_host_to_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/file.html#SoftLayer.managers.file.FileStorageManager.authorize_host_to_volume){:new_window}.
{:tip}

Per ulteriori informazioni sul limite di autorizzazioni simultanee, consulta le [FAQ](faqs.html).
{:important}

## Connessione della tua nuova archiviazione

A seconda del sistema operativo del tuo host, segui il link appropriato.
- [Montaggio di {{site.data.keyword.filestorage_short}} su Linux](accessing-file-storage-linux.html)
- [Montaggio di {{site.data.keyword.filestorage_short}} in CentOS](mounting-nsf-file-storage.html)
- [Montaggio di {{site.data.keyword.filestorage_short}} su CoreOS](mounting-storage-coreos.html)
- [Configurazione di {{site.data.keyword.filestorage_short}} per il backup con cPanel](configure-backup-cpanel.html)
- [Configurazione di {{site.data.keyword.filestorage_short}} per il backup con Plesk](configure-backup-plesk.html)
- [Montaggio del volume {{site.data.keyword.filestorage_short}} sugli host ESXi](architecture-guide-file-storage-vmware.html)
