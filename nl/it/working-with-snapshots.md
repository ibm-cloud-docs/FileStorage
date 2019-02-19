---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}


# Gestione delle istantanee
{: #managingSnapshots}

## Creazione di una pianificazione delle istantanee

Con le pianificazioni delle istantanee, decidi con che frequenza e quando vuoi creare un riferimento ad un punto nel tempo del tuo volume di archiviazione. Puoi avere un massimo di 50 istantanee per volume di archiviazione. Le pianificazioni sono gestite tramite la scheda **Storage** > **{{site.data.keyword.filestorage_short}}** del [{{site.data.keyword.slportal}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/){:new_window}.

Prima di poter configurare la tua pianificazione iniziale, devi procedere all'acquisto di spazio per le istantanee, se non lo hai fatto durante il provisioning iniziale del volume di archiviazione.
{:important}

### Aggiunta di una pianificazione delle istantanee
{: #addschedule}

Le pianificazioni delle istantanee possono essere configurate per intervalli orari, giornalieri e settimanali, ciascuno con un distinto ciclo di conservazione. Il limite massimo di istantanee è 50 per ogni volume di archiviazione, che può essere una combinazione di pianificazioni orarie, giornaliere e settimanali e di istantanee manuali.

1. Fai clic sul volume di archiviazione, fai clic su **Actions** e fai clic su **Schedule Snapshot**.
2. Nella finestra New Schedule Snapshot, puoi selezionare da tre diverse frequenze di istantanea. Usa qualsiasi combinazione di queste tre frequenze per creare una pianificazione delle istantanee completa.
   - Hourly (Ogni ora)
      - Specifica il minuto di ciascuna ora in cui deve essere acquisita un'istantanea. Il valore predefinito è il minuto corrente.
      - Specifica il numero di istantanee orarie da conservare prima che venga eliminata quella meno recente.
   - Daily (Ogni giorno)
      - Specifica l'ora e il minuto in cui deve essere acquisita un'istantanea. Il valore predefinito sono l'ora e il minuto correnti.
      - Seleziona il numero di istantanee orarie da conservare prima che venga eliminata quella meno recente.
   - Weekly (Ogni settimana)
      - Specifica il giorno della settimana, l'ora e il minuto in cui deve essere acquisita un'istantanea. Il valore predefinito sono il giorno, l'ora e il minuto correnti.
      - Seleziona il numero di istantanee settimanali da conservare prima che venga eliminata quella meno recente.
3. Fai clic su **Save** e crea un'altra pianificazione con una frequenza differente. Se il numero totale di istantanee pianificate è superiore a 50, ricevi un messaggio di avvertenza e non sarà possibile eseguire il salvataggio.

l'elenco delle istantanee viene visualizzato man mano che vengono eseguite nella sezione **Snapshots** della pagina **Detail**.

Puoi anche visualizzare l'elenco delle tue pianificazioni delle istantanee tramite la SLCLI con il seguente comando.
```
# slcli file snapshot-schedule-list --help
Usage: slcli file snapshot-schedule-list [OPTIONS] VOLUME_ID

  Lists snapshot schedules for a given volume

Options:
  -h, --help  Show this message and exit.
```

## Acquisizione di un'istantanea manuale

Le istantanee manuali possono essere acquisite a vari punti durante un upgrade o una manutenzione dell'applicazione. Puoi anche acquisire le istantanee su più server che erano stati temporaneamente disattivati a livello dell'applicazione.

Il limite massimo di istantanee manuali per ogni volume di archiviazione è 50.

1. Fai clic sul tuo volume di archiviazione.
2. Fai clic su **Actions**.
3. Fai clic su **Take Manual Snapshot**.
L'istantanea viene acquisita e viene visualizzata nella sezione **Snapshots** della pagina **Detail**. La sua pianificazione si presenta come manuale (Manual).

In alternativa, puoi utilizzare il seguente comando per creare un'istantanea tramite la SLCLI.
```
# slcli file snapshot-create --help
Usage: slcli file snapshot-create [OPTIONS] VOLUME_ID

Options:
  -n, --notes TEXT  Notes to set on the new snapshot
  -h, --help        Show this message and exit.
```

## Elenco di tutte le istanze con le funzioni di gestione e di informazioni sullo spazio utilizzato

Un elenco di istantanee conservate e spazio utilizzato può essere visualizzato nella pagina **Detail** (**Storage**, **{{site.data.keyword.filestorage_short}}**). Le funzioni di gestione (modifica di pianificazioni e aggiunta di ulteriore spazio) vengono controllate nella pagina Detail utilizzando il menu **Actions** oppure i link nelle diverse sezioni della pagina.

In alternativa, puoi effettuare questa attività tramite la CLI SL.
```
# slcli file snapshot-list --help
Usage: slcli file snapshot-list [OPTIONS] VOLUME_ID

Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: id, name, created, size_bytes
  -h, --help      Show this message and exit.
```

## Visualizzazione dell'elenco di istantanee conservate

Le istantanee conservate sono basate sul numero che hai immesso nel campo **Keep the last** quando configuri le tue pianificazioni. Puoi visualizzare le istantanee che sono state acquisite nella sezione **Snapshot**. Le istantanee sono elencate in base alla pianificazione.

## Visualizzazione della quantità di spazio per le istantanee utilizzata

Il grafico a torta nella pagina **Details** visualizza quanto spazio è utilizzato e quanto spazio è rimasto. Ricevi delle notifiche quando raggiungi le soglie di spazio – 75 percento, 90 percento e 95 percento.

## Modifica della quantità di spazio per le istantanee per un volume

Potresti aver bisogno di aggiungere dello spazio per le istantanee a un volume che in precedenza non ne aveva o che potrebbe avere bisogno di spazio per le istantanee aggiuntivo. Puoi aggiungere tra i 5 e i 4.000 GB, a seconda delle tue esigenze.

Lo spazio per le istantanee può essere solo aumentato. Non può essere ridotto. Puoi selezionare una quantità di spazio più piccola finché non determini di quanto spazio hai bisogno. Ricordati che le istantanee automatizzate e quelle manuali condividono lo spazio.
{:important}

Lo spazio per le istantanee viene modificato tramite **Storage** > **{{site.data.keyword.filestorage_short}}**.

1. Fai clic sui tuoi volumi di archiviazione, fai clic su **Actions** e fai clic su **Add More Snapshot Space**.
2. Seleziona da un intervallo di dimensioni dal prompt. Le dimensioni di norma vanno da 0 alla dimensione del tuo volume.
3. Fai clic su **Continue**.
4. Immetti l'eventuale codice promozionale (Promo Code) a tua disposizione e fai clic su **Recalculate**. Gli addebiti per questo ordine (Charges for this order) e il riesame dell'ordine (Order Review) sono completati per impostazione predefinita.
5. Fai clic sulla casella di spunta **I have read the Master Service Agreement…** e fai clic su **Place Order**. Nel giro di pochi minuti, viene eseguito il provisioning del tuo spazio per le istantanee aggiuntivo.

## Ricezione di notifiche quando il limite dello spazio per le istantanee viene raggiunto e le istantanee vengono eliminate

Le notifiche vengono inviate tramite i ticket di supporto al Master User sul tuo account quando raggiungi tre diverse soglie di spazio – 75 percento, 90 percento e 95 percento.

- A una **capacità al 75 percento**, viene inviata un'avvertenza che l'utilizzo dello spazio per le istantanee ha superato il 75 percento. Se presti attenzione all'avvertenza e aggiungi dello spazio manualmente oppure elimini delle istantanee conservate e non necessarie, l'azione viene annotata e il ticket viene chiuso. Se non fai niente, devi confermare manualmente il ticket che viene quindi chiuso.
- Al **90 percento di capacità**, viene inviata una seconda avvertenza quando l'utilizzo dello spazio per le istantanee ha superato il 90 percento. In modo analogo al raggiungimento della capacità al 75 percento, se esegui le azioni necessarie per ridurre lo spazio utilizzato, l'azione viene annotata e il ticket viene chiuso. Se non fai niente, devi confermare manualmente il ticket che viene quindi chiuso.
- Al **95 percento di capacità**, viene inviata un'avvertenza finale. Se non viene eseguita alcuna azione per portare il tuo utilizzo dello spazio al di sotto della soglia, viene generata una notifica e si verifica un'eliminazione automatica in modo che sia possibile creare delle future istantanee. Le istantanee pianificate vengono eliminate, a partire da quella meno recente, finché l'utilizzo non scende al di sotto del 95 percento. Le istantanee continuano a essere eliminate ogni volta che l'utilizzo supera il 95 percento finché non scende al di sotto della soglia. Se lo spazio viene aumentato manualmente oppure se le istantanee vengono eliminate, l'avvertenza viene reimpostata ed emessa nuovamente se la soglia viene superata nuovamente. Se non viene effettuata alcuna azione, questa notifica è l'unica avvertenza che ricevi.

## Eliminazione di una pianificazione delle istantanee

Le pianificazioni delle istantanee possono essere annullate tramite **Storage** > **{{site.data.keyword.filestorage_short}}**.

1. Fai clic sulla pianificazione da eliminare nel frame **Snapshot Schedules** nella pagina **Details**.
2. Fai clic sulla casella di spunta accanto alla pianificazione da eliminare e fai clic su **Save**.<br />

Se stai utilizzando la funzione di replica, assicurati che la pianificazione che stai eliminando non sia la pianificazione utilizzata dalla replica. Per ulteriori informazioni sull'eliminazione di una pianificazione della replica, vedi [qui](/docs/infrastructure/FileStorage?topic=FileStorage-replication).
{:important}

## Eliminazione di un'istantanea

Le istantanee che non sono più necessarie possono essere rimosse manualmente per liberare spazio per le future istantanee. L'eliminazione viene eseguita tramite **Storage** > **{{site.data.keyword.filestorage_short}}**.

1. Fai clic sul tuo volume di archiviazione e scorri alla sezione **Snapshot** per visualizzare l'elenco delle istantanee esistenti.
2. Fai clic su **Actions** accanto a una specifica istantanea e fai clic su **Delete** per eliminare l'istantanea. Tale eliminazione non ha alcuna ripercussione sulle eventuali istantanee passate o future nella stessa pianificazione poiché le istantanee non hanno alcuna interdipendenza.

In alternativa, puoi eliminare un'istantanea tramite la CLI SL.
```
# slcli file snapshot-delete --help
Usage: slcli file snapshot-delete [OPTIONS] SNAPSHOT_ID

Options:
  -h, --help  Show this message and exit.
```

Le istantanee manuali che non sono eliminate nel portale manualmente sono eliminate automaticamente quando raggiungi le limitazioni di spazio (prima quella meno recente).
{:note}

## Ripristino di un volume di archiviazione a uno specifico punto temporale utilizzando un'istantanea

Potresti dover riportare il tuo volume di archiviazione a uno specifico punto temporale a causa di un errore utente o di un danneggiamento dei dati.

1. Smonta e scollega il tuo volume di archiviazione dall'host.
   - Fai clic [qui](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux) per le istruzioni.
2. Fai clic su **Storage**, **{{site.data.keyword.filestorage_short}}** nel [{{site.data.keyword.slportal}} ![Icona link esterno](../../icons/launch-glyph.svg "Iconal ink esterno")](https://control.softlayer.com/){:new_window}.
3. Scorri verso il basso e fai clic sul tuo volume da ripristinare. La sezione **Snapshots** della pagina **Detail** visualizza l'elenco di tutte le istantanee salvate insieme alla loro dimensione e alla loro data di creazione.
4. Fai clic su **Actions** accanto all'istantanea da utilizzare e fai clic su **Restore**. <br/>

   Il completamento del ripristino comporta la perdita dei dati che erano stati creati o modificati dopo l'esecuzione dell'istantanea. Questa perdita di dati si verifica perché il tuo volume di archiviazione torna allo stesso stato in cui si trovava al momento dell'istantanea.
   {:note}
5. Fai clic su **Yes** per avviare il ripristino.

   Aspettati un messaggio nella pagina che indica che è in corso il ripristino del volume utilizzando l'istantanea selezionata. Inoltre, compare un'icona accanto al tuo volume in {{site.data.keyword.filestorage_short}} che indica che è in corso una transazione attiva. Se passi il puntatore del mouse sull'icona, viene visualizzata una finestra che mostra la transazione. Una volta completata la transazione, l'icona scompare.
   {:note}
6. Monta e ricollega il tuo volume di archiviazione all'host.
  - Fai clic [qui](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux) per le istruzioni.

In alternativa, puoi ripristinare il volume con un'istantanea tramite la SLCLI.
```
# slcli file snapshot-restore --help
Usage: slcli file snapshot-restore [OPTIONS] VOLUME_ID

Options:
  -s, --snapshot-id TEXT  The id of the snapshot which will be used to restore
                          the block volume
  -h, --help              Show this message and exit.
```  

Il ripristino di un volume comporta l'eliminazione di tutte le istantanee che erano state acquisite dopo che l'istantanea era stata utilizzata per il ripristino.
{:important}
