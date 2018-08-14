---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-29"

---
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}

# Montaggio di NFS/{{site.data.keyword.filestorage_short}} in CentOS

Il processo per montare {{site.data.keyword.filestorage_full}} in CentOS 7 è simile al processo di [montaggio di {{site.data.keyword.filestorage_short}} su RHEL 6](accessing-file-storage-linux.html). Tuttavia, poiché il montaggio è NFS, puoi specificare delle opzioni aggiuntive utilizzando la riga `Options=` nel file di montaggio. Nel seguente esempio, NFS è impostato per il montaggio a `/data/www`.  

>**Nota** - il punto di montaggio dell'istanza di {{site.data.keyword.filestorage_short}} può essere ottenuto dalla pagina di elenco {{site.data.keyword.filestorage_short}} oppure tramite una chiamata API - `SoftLayer_Network_Storage::getNetworkMountAddress()`.

```
$ cat data-www.mount
[Unit]
Description = Mount for Container Storage

[Mount]
What=<nfs_mount_point>
Where=/data/www
Type=nfs
Options=vers=4,sec=sys,noauto

[Install]
WantedBy = multi-user.target
```
{:codeblock}

Abilita quindi il montaggio e verifica che ne venga eseguito il montaggio correttamente.

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs4 (rw,relatime,vers=4.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
{:codeblock}
