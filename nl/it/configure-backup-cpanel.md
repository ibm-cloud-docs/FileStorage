---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, cPanel, backups

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Configurazione di {{site.data.keyword.filestorage_short}} per il backup con cPanel
{: #cPanelBackups}

Puoi utilizzare queste istruzioni per configurare i tuoi backup per l'archiviazione in {{site.data.keyword.filestorage_full}} da cPanel. Il presupposto è che siano disponibili SSH root o sudo e un accesso VHM (WebHost Manager) completo. Questo esempio è basato su un host **CentOS 7**.

Per ulteriori informazioni, vedi [cPanel - Configuring backup directory](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){: external}.
{:tip}

1. Connettiti all'host tramite SSH.
2. Assicurati che esista una destinazione di punto di montaggio. <br />

   Per impostazione predefinita, il sistema cPanel salva i file di backup in locale, nella directory `/backup`. In questo documento, si presuppone che la cartella `/backup` esista e che contenga dei backup e che `/backup2` possa essere utilizzato come il nuovo punto di montaggio.
   {:note}

3. Configura il tuo {{site.data.keyword.filestorage_short}} come descritto in [Accesso a {{site.data.keyword.filestorage_short}} su Red Hat Enterprise Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux) e [Montaggio di {{site.data.keyword.filestorage_short}} in CentOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)/[Montaggio di NFS/{{site.data.keyword.filestorage_short}} su CoreOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS). Monta il volume in `/backup2` e configuralo nella tabella del file system (`/etc/fstab`) per abilitare il montaggio all'avvio. <br />

   Per impostazione predefinita, NFS esegue il downgrade dei file che erano stati creati con le autorizzazioni root per l'utente nobody. Per consentire ai client root di conservare le autorizzazioni root nella condivisione NFS, devi aggiungere `no_root_squash` a `/etc/exports`.
   {:tip}

4. **Facoltativo**. Copia i backup esistenti nella nuova archiviazione. Puoi utilizzare `rsync`, ad esempio.
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}

    Questo comando comprime e trasmette i tuoi dati e preserva tutto quanto è possibile, fatta eccezione per i collegamenti reali. Fornisce inoltre informazioni su quali file sono in fase di trasferimento, nonché un breve riepilogo finale.
    {:tip}

5. Accedi a WebHost Manager e vai alla configurazione di backup tramite **Home** > **Backup** > **Backup Configuration**.

6. Modifica la configurazione per salvare i backup nel nuovo punto di montaggio.
    - Modifica la directory di backup predefinita immettendo il percorso assoluto alla nuova ubicazione al posto della directory `/backup/`.
    - Seleziona **Enable to mount a backup drive**. Questa impostazione fa in modo che il processo di configurazione controlli il file `/etc/fstab` per rilevare un montaggio di backup (`/backup2`). <br />

      Se esiste un montaggio con lo stesso nome della directory di staging, il processo di configurazione del backup monta l'unità ed esegue il backup delle informazioni. Al suo termine del processo di backup, smonta l'unità.
      {:note}
7. Applica le modifiche facendo clic su **Save Configuration**.
8. **Facoltativo**. Come dettato dal tuo specifico caso d'uso e dalle tue esigenze aziendali, rimuovi la vecchia archiviazione dal server e annulla dall'account.
