---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Gestione dei limiti di archiviazione
{: #managinglimits}

Per impostazione predefinita, puoi eseguire il provisioning di un totale combinato di 250 volumi {{site.data.keyword.blockstorageshort}} e {{site.data.keyword.filestorage_short}} globalmente.

Se non sei sicuro di quanti volumi disponi, puoi elencarli per ogni data center utilizzando il seguente comando `slcli`.
```
# slcli file volume-count --help
Usage: slcli file volume-count [OPTIONS]

Options:
 -d, --datacenter TEXT  Datacenter shortname
 --sortby TEXT          Column to sort by
 -h, --help             Show this message and exit.
```

Puoi richiedere un aumento del limite inoltrando un ticket nel [{{site.data.keyword.slportal}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/){:new_window}. Una volta approvata la richiesta, ottieni un limite dei volumi che è impostato per uno specifico data center.  

Per richiedere un aumento del limite, apri un ticket e indirizzalo al tuo rappresentante di vendita.

Nel ticket, fornisci cortesemente le seguenti informazioni:

- **Oggetto del ticket** (Ticket Subject): richiedi di aumentare il limite di archiviazione del conteggio di volumi del data center

- **Qual è il caso d'uso per la richiesta di volumi aggiuntivi?** <br />
*Ad esempio, la tua risposta potrebbe essere qualcosa di simile a un nuovo archivio dati VMware, un nuovo ambiente di sviluppo e test, un database SQL o la registrazione.*

- **Quanti volumi di blocchi supplementari sono necessari in base al tipo, alla dimensione, all'IOPS e all'ubicazione?** <br />
*Ad esempio, la tua risposta potrebbe essere qualcosa di simile a "25x Endurance 2 TB @ 4 IOPS in DAL09" o "25x Performance 4 TB @ 2 IOPS in WDC04"".*

- **Quanti volumi di file supplementari sono necessari in base al tipo, alla dimensione, all'IOPS e all'ubicazione?** <br />
*Ad esempio, la tua risposta potrebbe essere qualcosa di simile a "25x Performance 20 GB @ 10 IOPS in DAL09" o "50x Endurance 2 TB @ 0,25 IOPS in SJC03".*

- **Fornisci una stima di quando prevedi o intendi eseguire il provisioning di tutto l'aumento di volumi richiesto.** <br />
 "*Ad esempio, la tua risposta potrebbe essere qualcosa di simile a "90 giorni".*

- **Fornisci una previsione a 90 giorni dell'utilizzo della capacità medio previsto di questi volumi.** <br />
*Ad esempio, la tua risposta potrebbe essere qualcosa di simile a "prevedo un utilizzo al 25 percento entro 30 giorni, al 50 percento entro 60 giorni e al 75 percento entro 90 giorni".*

Rispondi a tutte le domande e a tutte le istruzioni nella tua richiesta. Sono necessarie per l'elaborazione e l'approvazione.
{:important}

Verrai informato dell'aggiornamento dei tuoi limiti tramite il processo del ticket.
