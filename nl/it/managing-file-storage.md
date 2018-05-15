---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Gestione dell'{{site.data.keyword.filestorage_short}}

Puoi gestire i tuoi volumi {{site.data.keyword.filestorage_full}} tramite {{site.data.keyword.slportal}}. Questo articolo fornisce le istruzioni per le attività più comuni. 

## Come autorizzare gli host ad accedere all'{{site.data.keyword.filestorage_short}}

Gli host "autorizzati" sono host a cui sono stati concessi i diritti di accesso a uno specifico volume. Senza l'autorizzazione host, non potrai accedere all'archiviazione dal tuo sistema né utilizzarla. Autorizzare un host ad accedere al tuo volume genera il nome utente e la password. 

**Nota**: puoi autorizzare e connettere solo gli host che si trovano nello stesso data center della tua archiviazione. Se hai più account, non puoi autorizzare un host da un account ad accedere alla tua archiviazione su un altro. 

1. Fai clic su **Storage** > **{{site.data.keyword.filestorage_short}}** e fai clic sul tuo nome volume (**Volume Name**).
2. Scorri alla sezione **Authorized Hosts** della pagina.
3. Fai clic sul link **Authorize Host** sul lato destro della pagina. Seleziona gli host che possono accedere allo specifico volume.

 

## Come visualizzare l'elenco di host autorizzati a un volume {{site.data.keyword.filestorage_short}}

Attieniti alla seguente procedura per visualizzare l'elenco di host autorizzati a un volume.

1. Fai clic su **Storage > {{site.data.keyword.filestorage_short}}** e fai clic sul tuo nome volume (**Volume Name**).
2. Scorri verso il basso fino al fondo della pagina, alla sezione **Authorized Hosts**.

Qui vedrai un elenco degli host attualmente autorizzati ad accedere al volume.


## Come visualizzare i volumi {{site.data.keyword.filestorage_short}} a cui è autorizzato un host

Puoi visualizzare i volumi a cui ha accesso un host, comprese le informazioni necessarie per stabilire una connessione, il nome volume, il tipo di archiviazione, l'indirizzo di destinazione, la capacità e l'ubicazione:

1. Fai clic su **Devices** > **Device List** dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} e fai clic sul dispositivo appropriato.
2. Seleziona la scheda Storage.

Ti verrà presentato un elenco dei volumi di archiviazione a cui questo specifico host ha accesso, tutti raggruppati in base al tipo di archiviazione (blocco, file, altro). Dai rispettivi menu **Action**, puoi autorizzare ulteriore archiviazione oppure rimuovere l'accesso.

 

## Come montare e smontare {{site.data.keyword.filestorage_short}}

Puoi utilizzare le informazioni sul punto di montaggio fornite nella vista **Volume Details** per montare dell'{{site.data.keyword.filestorage_short}} da un host.

Consulta il seguente articolo con i dettagli per montare e smontare {{site.data.keyword.filestorage_short}} da un host: [Accesso a {{site.data.keyword.filestorage_short}} su Linux](accessing-file-storage-linux.html)

 

## Come revocare l'accesso di un host all'{{site.data.keyword.filestorage_short}}

Se vuoi arrestare l'accesso da uno specifico host a uno specifico volume di archiviazione, puoi revocare l'accesso. Una volta revocato l'accesso, la connessione host al volume verrà interrotta e né il sistema operativo né le applicazioni saranno in grado di comunicare con il volume. 

**Nota:** per evitare problemi sul lato host, smonta il volume di archiviazione dal tuo sistema operativo prima di revocare l'accesso per evitare unità mancanti o il danneggiamento dei dati.

Puoi revocare l'accesso dall'una o dall'altra archiviazione dalle viste Device List o Storage.

### Come revocare l'accesso da Device List:

1. Fai clic su **Devices** > **Device List** dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} e fai doppio clic sul dispositivo appropriato.
2. Seleziona la scheda **Storage**.
3. Ti verrà presentato un elenco dei volumi di archiviazione a cui questo specifico host ha accesso, tutti raggruppati in base al tipo di archiviazione (blocco, file, altro). Seleziona il menu **Action** rispettivo accanto al volume dal quale vuoi revocare l'accesso e fai clic su **Revoke Access**.
4. Ti verrà chiesto se vuoi revocare l'accesso per un volume, poiché l'azione non può essere annullata. Fai clic su **Yes** per revocare l'accesso del volume oppure su **No** per annullare l'azione.

**Nota:** se vuoi disconnettere più volumi da uno specifico host, dovrai ripetere l'azione di revoca dell'accesso per ciascun volume.

 

### Come revocare l'accesso dalla vista Storage:
1. Fai clic su **Storage, {{site.data.keyword.filestorage_short}}** e seleziona il **Volume** da cui vuoi revocare l'accesso.
2. Scorri verso il basso alla sezione **Authorized Hosts** della pagina.
3. Fai clic sulla freccia a discesa **Actions** accanto all'host di cui deve essere revocato l'accesso e seleziona **Revoke Access**.
4. Ti verrà chiesto se vuoi revocare l'accesso per un volume, poiché l'azione non può essere annullata. Fai clic su **Yes** per revocare l'accesso del volume oppure su **No** per annullare l'azione.

**Nota:** se vuoi disconnettere più host da uno specifico volume, dovrai ripetere l'azione di revoca dell'accesso per ciascun host.

 

## Come annullare un volume di archiviazione

Se non hai più bisogno di uno specifico volume, puoi annullarlo. Per annullare un volume di archiviazione, è prima necessario revocare l'accesso da qualsiasi host.

1. Fai clic su **Storage**>**{{site.data.keyword.filestorage_short}}**.
2. Fai clic sulla freccia a discesa **Actions** per il volume da annullare e seleziona **Cancel {{site.data.keyword.filestorage_short}}**.
3. Ti verrà chiesto se confermi che vuoi annullare il volume immediatamente oppure alla data di anniversario dell'esecuzione del provisioning del volume. Fai clic su **Continue** o su **Close**. 
**Nota**: se selezioni l'opzione di annullare il volume alla sua data di anniversario, puoi invalidare la richiesta di annullamento prima della sua data di anniversario.
4. Fai clic sulla casella di spunta di riconoscimento e fai clic su **Confirm**.

 

## Come visualizzare i dettagli di un volume {{site.data.keyword.filestorage_short}} di cui è stato eseguito il provisioning

Puoi visualizzare un riepilogo delle informazioni chiave per il volume di archiviazione selezionato, incluse le funzionalità di istantanea e replica supplementari che sono state aggiunte all'archiviazione.

1. Fai clic su **Storage**>**{{site.data.keyword.filestorage_short}}**.
2. Fai clic sul nome volume (**Volume Name**) appropriato dall'elenco.
