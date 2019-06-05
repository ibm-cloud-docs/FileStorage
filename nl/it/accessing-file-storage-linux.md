---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-06"

keywords: File Storage, NSF, mounting File Storage, mounting storage on Linux,

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Montaggio di {{site.data.keyword.filestorage_short}} su Linux
{: #mountingLinux}

Per prima cosa, assicurati che l'host per accedere al volume {{site.data.keyword.filestorage_full}} sia autorizzato tramite [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}.

1. Dalla pagina di elenco {{site.data.keyword.filestorage_short}}, fai clic sul link **Actions** associato alla nuova condivisione e fai clic su **Authorize Host**.
2. Seleziona l'host o gli host dall'elenco e fai clic su **Submit**. Questa azione autorizza l'host ad accedere alla condivisione.

In alternativa, puoi autorizzare gli host tramite la SLCLI.
```
# slcli file access-authorize --help
Usage: slcli file access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to authorize
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to authorize
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to authorize
  --ip-address TEXT         An IP address to authorize
  -s, --subnet-id TEXT      The id of one SoftLayer_Network_Subnet to
                            authorize
  --help                    Show this message and exit.
```

## Montaggio della condivisione {{site.data.keyword.filestorage_short}}

Utilizza queste istruzioni per collegare un'istanza di elaborazione {{site.data.keyword.cloud}} basata su Linux a una condivisione NFS (Network file system). L'esempio è basato su Red Hat Enterprise Linux 6. La procedura può essere regolata per altre distribuzioni Linux in base alla documentazione del fornitore del sistema operativo.

Il punto di montaggio dell'istanza di archiviazione file può essere ottenuto dalla pagina di elenco {{site.data.keyword.filestorage_short}} oppure tramite una chiamata API - `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}

1. Installa gli strumenti richiesti.
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

   I file creati da root hanno una proprietà di `nobody:nobody`. Per visualizzare la proprietà correttamente, `idmapd.conf` deve essere aggiornato con le impostazioni di dominio corrette. Vedi la sezione [Come implementare no_root_squash per NFS](#norootsquash).
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

   Se il tuo SO host è CentOS, puoi configurare ulteriori opzioni. Per ulteriori informazioni, consulta [Montaggio di {{site.data.keyword.filestorage_short}} in CentOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS).


## Implementazione di `no_root_squash` per NFS (facoltativo)
{: #norootsquash}

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
## Smontaggio del file system 

Per smontare un qualsiasi file system al momento montato sul tuo host, esegui il comando `umount` con il nome del disco o il nome di un punto di montaggio.

```
umount /dev/sdb
```
{:pre}

```
umount /mnt
```
{:pre}
