---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Configurazione di {{site.data.keyword.filestorage_short}} per il backup con Plesk

Puoi utilizzare queste istruzioni per configurare {{site.data.keyword.filestorage_full}} per i tuoi backup in Plesk. Il presupposto è che siano disponibili SSH root o sudo e un accesso a Plesk a livello di amministrazione completo. Questo esempio è basato su un host CentOS7.

Per ulteriori informazioni, vedi la [documentazione di Plesk relativa al backup e al ripristino ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window}.
{:tip}

1. Connettiti all'host tramite SSH.
2. Assicurati che esista una destinazione di punto di montaggio. <br />

   Plesk ha due opzioni per l'archiviazione dei backup. Una è l'archiviazione Plesk interna, che è l'archiviazione sul tuo server Plesk. L'altra è l'archiviazione FTP esterna , che è l'archiviazione su qualche server esterno nel web o nella tua rete locale. Di norma, sui box Plesk, i backup interni sono archiviati in `/var/lib/psa/dumps` e utilizzano `/tmp` come directory temporanea. In questo esempio, la directory temporanea viene conservata in locale ma la directory `dumps` viene spostata alla destinazione {{site.data.keyword.filestorage_short}} (`/backup/psa/dumps`). Non sono necessarie credenziali utente FTP.
   {:note}
3. Configura il tuo {{site.data.keyword.filestorage_short}} come descritto in [Accesso a {{site.data.keyword.filestorage_short}} su Red Hat Enterprise Linux](accessing-file-storage-linux.html) e [Montaggio di NFS/{{site.data.keyword.filestorage_short}} in CentOS](mounting-nsf-file-storage.html) oppure [Montaggio di NFS/{{site.data.keyword.filestorage_short}} su CoreOS](mounting-storage-coreos.html). Monta il volume in `/backup` e configuralo nella tabella del file system (`/etc/fstab`) per abilitare il montaggio all'avvio. <br />

   Per impostazione predefinita, NFS esegue il downgrade dei file che erano stati creati con le autorizzazioni root per l'utente nobody. Per consentire ai client root di conservare le autorizzazioni root nella condivisione NFS, devi aggiungere `no_root_squash` a `/etc/exports`.
   {:tip}
4. **Facoltativo**. Copia i backup esistenti nella nuova archiviazione. Usa `rsync`; ad esempio:
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {:pre}

   Questo comando comprime e trasmette i tuoi dati e preserva tutto quanto è possibile, fatta eccezione per i collegamenti reali. Fornisce inoltre informazioni su quali file sono in fase di trasferimento, nonché un breve riepilogo finale.
   {:tip}
5. Modifica `/etc/psa/psa.conf` in modo che punti al valore `DUMP_D` sulla nuova destinazione.
    - Si presenta come: `DUMP_D /backup/psa/dumps`.
6. **Facoltativo**. Come dettato dal tuo specifico caso d'uso e dalle tue esigenze aziendali, rimuovi la vecchia archiviazione dal server ed annullala dall'account.
