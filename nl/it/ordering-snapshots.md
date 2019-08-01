---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-24"

keywords: File Storage, file storage, NFS, snapshot, ordering snapshot, snapshot space

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Ordinazione di istantanee
{: #ordering-snapshots}

Per creare le istantanee del tuo volume di archiviazione, in modo automatizzato o manualmente, devi acquistare dello spazio per contenerle. Puoi acquistare capacità fino alla tua quantità di volume di archiviazione (durante l'acquisto di volume iniziale e successivamente attenendoti a questa procedura).

## Determinazione della quantità di spazio per le istantanee da ordinare

In linea generale, lo spazio per le istantanee viene utilizzato dalle istantanee in base a due fattori chiave:
- A quante modifiche è soggetto il tuo file system attivo nel corso del tempo.
- Per quanto tempo prevedi di conservare le istantanee.  

Il modo per calcolare la quantità di spazio necessaria è **(Frequenza di modifica)** x **(numero di ore/giorni/settimane/mesi di conservazione dei dati)**.  

La prima istantanea utilizza una quantità trascurabile di spazio poiché è solo una copia dei metadati (puntatori) che indica i blocchi di file system attivi.
{:note}

Un volume con numerose modifiche e un lungo periodo di conservazione richiede più spazio di un volume con una modifica e una pianificazione di conservazione moderate. Un esempio per il primo tipo è un database con un'elevata frequenza di modifica. Un esempio per il secondo tipo è un archivio dati VMware.

Se acquisisci 12 istantanee orarie di 500 GB di dati reali e c'è l'1 percento di modifica tra ciascuna istantanea, ti ritrovi alla fine con 60 GB per le istantanee.

*(Frequenza di modifica di 5-GB) x (12 istantanee orarie) = (60 GB di spazio utilizzato)*

Se invece per tali 500 GB di dati effettivi, con 12 istantanee orarie, si verifica il 10 percento di modifica ogni ora, lo spazio per le istantanee utilizzato è 600 GB.

*(Frequenza di modifica di 50-GB) x (12 istantanee orarie) = (600 GB di spazio utilizzato)*

Pertanto, quando determini la quantità di spazio di istantanea di cui hai bisogno, considera attentamente la frequenza di modifica. È un fattore che ha un enorme impatto sulla quantità di spazio di istantanea di cui hai bisogno. Un volume più grande è più probabile che subisca modifiche più spesso. Tuttavia, un volume di 500-GB con 5 GB di modifica e un volume di 10-TB con 5 GB di modifica utilizzano la stessa quantità di spazio per le istantanee.

Inoltre, per la maggior parte dei carichi di lavoro, più grande è un volume e minore è lo spazio da mettere da parte inizialmente. Ciò è dovuto principalmente all'efficienza dei dati sottostanti e alla natura della modalità di funzionamento delle istantanee nell'ambiente.

## Ordinazione di spazio dell'istantanea tramite la console {{site.data.keyword.cloud_notm}}

1. Accedi alla [console {{site.data.keyword.cloud}}](https://{DomainName}/){: external} e fai clic sull'icona menu nell'angolo superiore sinistro.
2. Seleziona **Infrastruttura classica**.
3. Accedi alla tua archiviazione tramite **Storage** > **{{site.data.keyword.filestorage_short}}**.
4. Fai clic su **Add Snapshot Space** nel frame Snapshots.
5. Seleziona la quantità di spazio che ti serve e il metodo di pagamento.
6. Fai clic su **Continue**.
7. Immetti l'eventuale codice promozionale (Promo Code) a tua disposizione e fai clic su **Recalculate**. Gli addebiti per quest'ordine (**Charges for this order**) e il riesame dell'ordine (**Order Review**) presentano i valori predefiniti.

   Gli sconti vengono applicati quando l'ordine viene elaborato.
   {:note}
8. Seleziona la casella **I have read the Master Service Agreement and agree to the terms therein.** e fai clic su **Place Order**. Nel giro di pochi minuti, viene eseguito il provisioning del tuo spazio di istantanea.

## Ordinazione di spazio dell'istantanea tramite la SLCLI

```
# slcli file snapshot-order --help
Usage: slcli file snapshot-order [OPTIONS] VOLUME_ID

Options:
  --capacity INTEGER    Size of snapshot space to create in GB  [required]
  --tier [0.25|2|4|10]  Endurance Storage Tier (IOPS per GB) of the file
                        volume for which space is ordered [optional, and only
                        valid for endurance storage volumes]
  --upgrade             Flag to indicate that the order is an upgrade
  -h, --help            Show this message and exit.
```
