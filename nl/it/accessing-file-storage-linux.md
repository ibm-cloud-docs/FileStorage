---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Accesso a {{site.data.keyword.filestorage_short}} su Linux

Prima di iniziare, assicurati che l'host che accede al volume {{site.data.keyword.filestorage_full}} sia stato autorizzato tramite il [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}:

1. Dalla pagina di elenco {{site.data.keyword.filestorage_short}}, fai clic sulle azioni (**Actions**) associata alla condivisione di cui è stato appena eseguito il provisioning e fai clic su **Authorize Host**.
2. Seleziona l'host o gli host desiderati dall'elenco e fai clic su **Submit**; questo autorizza l'host o gli host ad accedere alla condivisione.

## Montaggio della condivisione {{site.data.keyword.filestorage_short}}

Viene qui di seguito indicata la procedura necessaria per connettere un'istanza di elaborazione {{site.data.keyword.BluSoftlayer_full}} a una condivisione NFS (Network File System). L'esempio è basato su Red Hat Enterprise Linux 6. La procedura può essere regolata per altre distribuzioni Linux in base alla documentazione del fornitore del sistema operativo.

**Nota:** il punto di montaggio dell'istanza di archiviazione file può essere ottenuto dalla pagina di elenco {{site.data.keyword.filestorage_short}} oppure tramite una chiamata alla API - SoftLayer_Network_Storage::getNetworkMountAddress().

1. Installa i pacchetti/strumenti richiesti.

    `[root@TEST-RHEL6]# yum -y install nfs-utils nfs-utils-lib
    `
2. Monta la condivisione remota
    `[root@TEST-RHEL6]# mount -t "nfs version" -o "options" <mount_point> /mnt`
    
    Questo è un esempio di montaggio della condivisione remota a un'istanza di archiviazione.
    
    `[root@TEST-RHEL6 mnt]# mount -t nfs4 -o hard,intr`
    
    `nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt`
 
3. Verifica che il montaggio sia stato eseguito correttamente.

    `[root@TEST-RHEL6]# df -h`
    
    `Filesystem Size Used Avail Use% Mounted on`
    
    `/dev/xvda2 25G 1.4G 22G 6% /`
    
    `tmpfs 1.9G 0 1.9G 0% /dev/shm`
    
    `/dev/xvda1 97M 51M 42M 55%`
    
4. Accedi al punto di montaggio e leggi/scrivi i file.

    `[root@TEST-RHEL6]# touch /mnt/test`
    
    `[root@TEST-RHEL6]# ls -la /mnt`
    
    `total 12`
    
    `drwxr-xr-x 2 nobody nobody 4096 Sep 8 15:52 .`
    
    `dr-xr-xr-x. 22 root root 4096 Sep 8 14:30 ..`
    
    `-rw-r--r-- 1 nobody nobody 0 Sep 8 15:52 test`

    **Nota:** i file creati da root avranno una proprietà di nobody:nobody. Per visualizzare la proprietà correttamente, idmapd.conf deve essere aggiornato con le impostazioni di dominio corrette. Consulta “Come implementare no_root_squash per NFS” qui di seguito.
    
5. Monta la condivisione remota all'avvio. Per completare l'impostazione, modifica la tabella dei file system /etc/fstab per aggiungere la condivisione remota all'elenco di voci che verranno automaticamente montate all'avvio:

    `(hostname):/(username) /mnt "nfs version" "options" 0 0`
    
    Utilizzando l'istanza dall'esempio di montaggio della condivisione remota, la voce sarà:
    
    `nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt nfs4 defaults,hard,intr 0 0`
    
6.  Verifica che non ci sono errori con il file di configurazione.

    `[root@TEST-RHEL6 /]# mount -fav`
    
    Se il comando viene completato senza errori, la tua impostazione è completa.

**Nota:** se stai utilizzando NFS 4.1, aggiungi 'sec=sys' al comando mount per evitare problemi di proprietà dei file.

 
## Come implementare no_root_squash per NFS (facoltativo)

La configurazione di no_root_squash consente ai client root di conservare le autorizzazioni root sulla condivisione NFS. Per NFSv3, i client non dovranno fare niente; no_root_squash dovrebbe semplicemente funzionare.
Per NFSv4, dovrai impostare il dominio nfsv4 su: slnfsv4.com e avviare rpcidmapd o un servizio simile, a seconda della versione del sistema operativo.

Ecco un esempio:

1. Dall'host, imposta l'impostazione del dominio in /etc/idmapd.conf.

    `#vi /etc/idmapd.conf`
    
    `[General]`
    
    `#Verbosity = 0`
    
    `#The following should be set to the local NFSv4 domain name`
    
    `#The default is the host's DNS domain name.`
    
    `Domain = slnfsv4.com`
    
    `[Mapping]`
    
    `Nobody-User = nobody`
    
    `Nobody-Group = nobody`
    
2. Esegui nfsidmap -c.
3. Avvia rpcidmapd.

   ` # /etc/init.d/rpcidmapd start`
   
   ` Starting RPC idmapd: [ OK ]`
