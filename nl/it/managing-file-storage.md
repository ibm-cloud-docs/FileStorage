---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-25"

keywords: File Storage, file storage, NFS, authorizing hosts, rewoke access, grant access, view authorizations

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}


# Gestione di {{site.data.keyword.filestorage_short}}
{: #managingstorage}

Puoi gestire i tuoi volumi {{site.data.keyword.filestorage_full}} tramite la console {{site.data.keyword.cloud}}.

## Autorizzazione degli host ad accedere a {{site.data.keyword.filestorage_short}}

Gli host “autorizzati” sono gli host a cui era stato concesso l'accesso a uno specifico volume. Senza l'autorizzazione host, non puoi accedere all'archiviazione dal tuo sistema o utilizzarla. L'autorizzazione di un host ad accedere al tuo volume genera il nome utente e la password.

Puoi autorizzare e connettere gli host che si trovano nello stesso data center della tua archiviazione. Puoi avere più account ma non puoi autorizzare un host da un account ad accedere alla tua archiviazione su un altro account.
{:important}

1. Vai alla [console {{site.data.keyword.cloud}}](https://{DomainName}/){: external}. Dal menu, seleziona **Classic Infrastructure**.
2. Fai clic su **Storage** > **{{site.data.keyword.filestorage_short}}** e fai clic sul tuo nome volume (**Volume Name**).
3. Scorri alla sezione **Authorized Hosts** della pagina.
4. Fai clic su **Authorize Host** sulla destra.
5. Filtra l'elenco di host disponibili selezionando il tipo di dispositivo, la sottorete o l'indirizzo IP.

   Quando l'elenco viene filtrato, le sottoreti visualizzate sono sottoreti sottoscritte nello stesso data center del volume di archiviazione.
   {:note}
6. Seleziona uno o più host dall'elenco e fai clic su **Save**.

In alternativa, puoi utilizzare il seguente comando nella SLCLI.
```
# slcli file access-authorize --help
Usage: slcli file access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of one hardware server to authorize.
  -v, --virtual-id TEXT     The ID of one virtual server to authorize.
  -i, --ip-address-id TEXT  The ID of one IP address to authorize.
  -p, --ip-address TEXT     An IP address to authorize.
  -s, --subnet-id TEXT      An ID of one subnet to authorize.
  --help                    Show this message and exit.
```

## Visualizzazione dell'elenco di host che sono autorizzati ad accedere a un volume {{site.data.keyword.filestorage_short}}

1. Vai alla [console {{site.data.keyword.cloud}}](https://{DomainName}/){: external}. Dal menu, seleziona **Classic Infrastructure**.
2. Fai clic su **Storage** > **{{site.data.keyword.filestorage_short}}** e fai clic sul tuo nome volume (**Volume Name**).
3. Scorri verso il basso la pagina alla sezione **Authorized Hosts**.

Qui puoi vedere un elenco degli host attualmente autorizzati ad accedere al volume.

In alternativa, puoi utilizzare il seguente comando nella SLCLI.
```
# slcli file access-list --help
Usage: slcli file access-list [OPTIONS] VOLUME_ID

Options:
 --sortby TEXT   Column to sort by
 --columns TEXT  Columns to display. Options: id, name, type,
                 private_ip_address, source_subnet, host_iqn, username,
                 password, allowed_host_id
 -h, --help      Show this message and exit.
```


## Visualizzazione dei volumi {{site.data.keyword.filestorage_short}} a cui è autorizzato un host

Puoi visualizzare i volumi a cui ha accesso un host, comprese le informazioni necessarie per stabilire una connessione, il nome volume, il tipo di archiviazione, l'indirizzo di destinazione, la capacità e l'ubicazione.

1. Vai alla [console {{site.data.keyword.cloud}}](https://{DomainName}/){: external}
2. Dal menu, seleziona **Classic Infrastructure**.
3. Fai clic su **Devices** > **Device List**.
4. Fai clic sul dispositivo appropriato.
5. Seleziona la scheda Storage.

Ti viene presentato un elenco dei volumi di archiviazione a cui questo specifico host ha accesso, tutti raggruppati in base al tipo di archiviazione (blocco, file, altro). Dai rispettivi menu **Action**, puoi autorizzare ulteriore archiviazione oppure rimuovere l'accesso.


## Montaggio e smontaggio di {{site.data.keyword.filestorage_short}}

Puoi utilizzare le informazioni sul punto di montaggio fornite nella vista **Volume Details** per montare {{site.data.keyword.filestorage_short}} da un host. Vedi [Accesso a {{site.data.keyword.filestorage_short}} su Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)


## Revoca dell'accesso di un host a {{site.data.keyword.filestorage_short}}

Se vuoi arrestare l'accesso da uno specifico host a uno specifico volume di archiviazione, puoi revocare l'accesso. Quando l'accesso viene revocato, la connessione host viene eliminata dal volume. Il sistema operativo e le applicazioni non possono più comunicare con il volume.

Per evitare problemi sul lato host, smonta il volume di archiviazione dal tuo sistema operativo prima di revocare l'accesso per evitare unità mancanti o il danneggiamento dei dati.
{:important}

Puoi revocare l'accesso dall'una o dall'altra archiviazione dalle viste Device List o Storage.

### Revoca dell'accesso da Device List

1. Vai alla [console {{site.data.keyword.cloud}}](https://{DomainName}/){: external}.
2. Dal menu, seleziona **Classic Infrastructure**.
3. Fai clic su **Devices** > **Device List**.
2. Fai doppio clic sul dispositivo appropriato.
3. Seleziona la scheda **Storage**.
4. Ti viene presentato un elenco dei volumi di archiviazione a cui questo specifico host ha accesso, tutti raggruppati in base al tipo di archiviazione (blocco, file, altro). Seleziona il menu **Action** rispettivo accanto al volume dal quale vuoi revocare l'accesso e fai clic su **Revoke Access**.
5. Conferma se vuoi revocare l'accesso per un volume, poiché l'azione non può essere annullata. Fai clic su **Yes** per revocare l'accesso del volume oppure su **No** per annullare l'azione.

Se vuoi disconnettere più volumi da uno specifico host, devi ripetere l'azione di revoca dell'accesso per ciascun volume.
{:tip}


### Revoca dell'accesso dalla vista Storage

1. Vai alla [console {{site.data.keyword.cloud}}](https://{DomainName}/){: external}.
2. Dal menu, seleziona **Classic Infrastructure**.
3. Fai clic su **Storage** > **{{site.data.keyword.filestorage_short}}** e seleziona il **Volume** da cui vuoi revocare l'accesso.
4. Scorri alla sezione **Authorized Hosts** della pagina.
5. Fai clic su **Actions** accanto all'host di cui deve essere revocato l'accesso e seleziona **Revoke Access**.
6. Conferma se vuoi revocare l'accesso per un volume, poiché l'azione non può essere annullata. Fai clic su **Yes** per revocare l'accesso del volume oppure su **No** per annullare l'azione.

Se vuoi disconnettere più host da uno specifico volume, devi ripetere l'azione di revoca dell'accesso per ciascun host.
{:tip}

### Revoca dell'accesso tramite la SLCLI.
Puoi utilizzare il seguente comando nella SLCLI.
```
# slcli file access-revoke --help
Usage: slcli file access-revoke [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of one hardware server to revoke authorization.
  -v, --virtual-id TEXT     The ID of one virtual server to revoke authorization.
  -i, --ip-address-id TEXT  The ID of one IP address to revoke authorization.
  -p, --ip-address TEXT     An IP address to revoke authorization.
  -s, --subnet-id TEXT      An ID of one subnet to revoke authorization.
  --help                    Show this message and exit.
```

## Annullamento di un volume di archiviazione

Se non hai più bisogno di uno specifico volume, puoi annullare tale archiviazione. Per annullare un volume di archiviazione, devi prima revocare l'accesso da qualsiasi host.

1. Vai alla [console {{site.data.keyword.cloud}}](https://{DomainName}/){: external}. Dal menu, seleziona **Classic Infrastructure**.
2. Fai clic su **Storage** > **{{site.data.keyword.filestorage_short}}**.
3. Fai clic su **Actions** per il volume da annullare e seleziona **Cancel {{site.data.keyword.filestorage_short}}**.
4. Conferma se vuoi annullare il volume immediatamente oppure alla data di anniversario dell'esecuzione del provisioning del volume.

   Se selezioni l'opzione di annullare il volume alla sua data di anniversario, puoi invalidare la richiesta di annullamento prima della sua data di anniversario.
   {:tip}
5. Fai clic su **Continue** o su **Close**.
6. Fai clic sulla casella di spunta di riconoscimento e fai clic su **Confirm**.

In alternativa, puoi utilizzare il seguente comando nella SLCLI.
```
# slcli file volume-cancel --help
Usage: slcli file volume-cancel [OPTIONS] VOLUME_ID

Options:
  --reason TEXT  An optional reason for cancellation
  --immediate    Cancels the file storage volume immediately instead of on the
                 billing anniversary
  -h, --help     Show this message and exit.
```

Una volta che è stato annullato il volume, esiste un periodo di attesa per il recupero di 24 ore. Puoi ancora visualizzare il volume nella console durante queste 24 ore. Quando il periodo di recupero scade, i dati vengono eliminati permanentemente e il volume viene rimosso dalla console. Tuttavia, la fatturazione per il volume viene interrotta immediatamente. Per ulteriori informazioni, vedi le [Domande frequenti](/docs/infrastructure/FileStorage?topic=FileStorage-file-storage-faqs).
{:note}

 Le repliche attive possono bloccare il recupero del volume di archiviazione. Assicurati che il volume non sia più montato, che le autorizzazioni host siano state revocate e che la replica sia stata annullata prima di tentare di annullare il volume originale.
