---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-29"

---
{:new_window: target="_blank"}
{:pre: .pre}
 
# Configurazione di {{site.data.keyword.filestorage_short}} per il backup con cPanel

Puoi utilizzare queste istruzioni per configurare i tuoi backup per l'archiviazione in {{site.data.keyword.filestorage_full}} da cPanel. Il presupposto è che siano disponibili SSH root o sudo e un accesso VHM (WebHost Manager) completo. Questo esempio è basato su un host **CentOS 7**.

>**Nota** - puoi trovare la documentazione di cPanel per configurare una directory di backup [qui](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){:new_window}.

1. Connettiti all'host tramite SSH.

2. Assicurati che esista una destinazione di punto di montaggio. <br />
   >**Nota** - per impostazione predefinita, il sistema cPanel salva i file di backup in locale, nella directory `/backup`. In questo documento, si presuppone che la cartella `/backup` esista e che contenga dei backup e che `/backup2` possa essere utilizzato come il nuovo punto di montaggio.
   
3. Configura il tuo {{site.data.keyword.filestorage_short}} come descritto in [Accesso a {{site.data.keyword.filestorage_short}} su Red Hat Enterprise Linux](accessing-file-storage-linux.html) e [Montaggio di NFS/{{site.data.keyword.filestorage_short}} in CentOS](mounting-nsf-file-storage.html)/[Montaggio di NFS/{{site.data.keyword.filestorage_short}} su CoreOS](mounting-storage-coreos.html). Monta il volume in `/backup2` e configuralo nella tabella del file system (`/etc/fstab`) per abilitare il montaggio all'avvio. <br />
   >**Nota** - per impostazione predefinita, NFS esegue il downgrade dei file che erano stati creati con le autorizzazioni root per l'utente nobody. Per consentire ai client root di conservare le autorizzazioni root nella condivisione NFS, devi aggiungere `no_root_squash` a `/etc/exports`. 

4. **Facoltativo**. Copia i backup esistenti nella nuova archiviazione. Puoi utilizzare `rsync`, ad esempio.
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}
    
    >**Nota** - questo comando comprime e trasmette i tuoi dati e preserva tutto quanto è possibile, fatta eccezione per i collegamenti reali. Fornisce inoltre informazioni su quali file sono in fase di trasferimento, nonché un breve riepilogo finale.
    
5. Esegui l'accesso a WebHost Manager e vai alla configurazione di backup tramite **Home** > **Backup** > **Backup Configuration**.

6. Modifica la configurazione per salvare i backup nel nuovo punto di montaggio. 
    - Modifica la directory di backup predefinita immettendo il percorso assoluto alla nuova ubicazione al posto della directory `/backup/`. 
    - Seleziona **Enable to mount a backup drive**. Questa impostazione fa in modo che il processo di configurazione controlli il file `/etc/fstab` per rilevare un montaggio di backup (`/backup2`). <br /> 
      >**Nota**- se esiste un montaggio con lo stesso nome della directory di staging, il processo di configurazione del backup monta l'unità e vi esegue il backup delle informazioni. Al suo termine del processo di backup, smonta l'unità. 

7. Applica le modifiche facendo clic su **Save Configuration**.

8. **Facoltativo**. Come dettato dal tuo specifico caso d'uso e dalle tue esigenze aziendali, rimuovi la vecchia archiviazione dal server e annulla dall'account.
