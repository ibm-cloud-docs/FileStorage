---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Montaggio di NFS/{{site.data.keyword.filestorage_short}} in CentOS

Il processo per montare la nostra {{site.data.keyword.filestorage_full}} Endurance/Performance in CentOS 7 è praticamente uguale al processo per il [montaggio dell'{{site.data.keyword.filestorage_short}} su RHEL 6](https://console.stage1.bluemix.net/docs/infrastructure/FileStorage/accessing-file-storage-linux.html), ma, poiché il montaggio è NFS, possiamo specificare delle opzioni aggiuntive utilizzando la riga *Options=* nel file di montaggio. Nel seguente esempio, stiamo impostando NFS per il montaggio a /data/www. Nota: il punto di montaggio NFS dell'istanza {{site.data.keyword.filestorage_short}} può essere ottenuto dalla pagina di elenco {{site.data.keyword.filestorage_short}} oppure richiamando la API `SoftLayer_Network_Storage::getNetworkMountAddress()`.

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

Possiamo ora abilitare il montaggio e verificare che ne venga eseguito il montaggio correttamente.

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs4 (rw,relatime,vers=4.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
