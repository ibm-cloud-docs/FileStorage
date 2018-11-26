---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-31"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Montaggio dell'{{site.data.keyword.filestorage_short}} su Linux

Per prima cosa, assicurati che l'host per accedere al volume {{site.data.keyword.filestorage_full}} sia autorizzato tramite [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Dalla pagina di elenco {{site.data.keyword.filestorage_short}}, fai clic sulle azioni (**Actions**) associate alla nuova condivisione e fai clic su **Authorize Host**.
2. Seleziona l'host o gli host dall'elenco e fai clic su **Submit**. Questa azione autorizza l'host ad accedere alla condivisione.

## Montaggio della condivisione {{site.data.keyword.filestorage_short}}

Utilizza queste istruzioni per collegare un'istanza di elaborazione {{site.data.keyword.BluSoftlayer_full}} basata su Linux a una condivisione NFS (Network File System). L'esempio è basato su Red Hat Enterprise Linux 6. La procedura può essere regolata per altre distribuzioni Linux in base alla documentazione del fornitore del sistema operativo.

Il punto di montaggio dell'istanza di archiviazione file può essere ottenuto dalla pagina di elenco {{site.data.keyword.filestorage_short}} oppure tramite una chiamata API - `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}

1. Installa i pacchetti/strumenti richiesti.
   ```
   # yum -y install nfs-utils nfs-utils-lib
   ```
   {:pre}

2. Monta la condivisione remota.
   ```
   # mount -t "nfs version" -o "options" <mount_point> /mnt
   ```

   Esempio
   ```
   # mount -t nfs4 -o hard,intr
   nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt
   ```

3. Verifica che il montaggio sia stato eseguito correttamente.
   ```
   # df -h
   Filesystem Size Used Avail Use% Mounted on
   /dev/xvda2 25G 1.4G 22G 6% /
   tmpfs 1.9G 0 1.9G 0% /dev/shm
   /dev/xvda1 97M 51M 42M 55%
   ```

4. Vai al punto di montaggio e leggi/scrivi i file.
   ```
   # touch /mnt/test
   # ls -la /mnt
   total 12
   drwxr-xr-x 2 nobody nobody 4096 Sep 8 15:52 .
   dr-xr-xr-x. 22 root root 4096 Sep 8 14:30 ..
   -rw-r--r-- 1 nobody nobody 0 Sep 8 15:52 test
   ```

   I file creati da root hanno una proprietà di `nobody:nobody`. Per visualizzare la proprietà correttamente, `idmapd.conf` deve essere aggiornato con le impostazioni di dominio corrette. Vedi la sezione [Come implementare no_root_squash per NFS](#implementing-no_root_squash-for-nfs-optional-).
   {:tip}

5. Monta la condivisione remota all'avvio. Per completare l'impostazione, modifica la tabella dei file system (`/etc/fstab`) per aggiungere la condivisione remota all'elenco di voci che vengono automaticamente montate all'avvio:

   ```
   (hostname):/(username) /mnt "nfs version" "options" 0 0
   ```

   Esempio

   ```
   nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt nfs4 defaults,hard,intr 0 0
   ```

6. Verifica che il file di configurazione non presenti alcun errore.

   ```
   # mount -fav
   ```
   {:pre}

   Se il comando viene completato senza errori, la tua impostazione è completa.

   Se stai utilizzando NFS 4.1, aggiungi `sec=sys` al comando mount per evitare problemi di proprietà dei file.
   {:tip}


## Implementazione di `no_root_squash` per NFS (facoltativo)

La configurazione di `no_root_squash` consente ai client root di conservare le autorizzazioni root sulla condivisione NFS.
- Per NFSv3, i client non devono fare niente; `no_root_squash` funziona.
- Per NFSv4, devi impostare il dominio nfsv4 su: `slnfsv4.com` e avviare `rpcidmapd` o un servizio simile che viene utilizzato dal tuo sistema operativo.

Esempio

1. Dall'host, configura l'impostazione del dominio in `/etc/idmapd.conf`.

   ```
   #vi /etc/idmapd.conf
   [General]
   #Verbosity = 0
   #The following should be set to the local NFSv4 domain name
   #The default is the host's DNS domain name.
   Domain = slnfsv4.com
   [Mapping]
   Nobody-User = nobody
   Nobody-Group = nobody
   ```

2. Esegui `nfsidmap -c`.
3. Avvia `rpcidmapd`.
   ```
   # /etc/init.d/rpcidmapd start
   Starting RPC idmapd: [ OK ]
   ```
