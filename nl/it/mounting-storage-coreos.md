---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Montaggio dell'{{site.data.keyword.filestorage_short}} su CoreOS

CoreOS è una potente distribuzione Linux sviluppata per rendere semplici da gestire delle distribuzioni di grandi dimensioni e scalabili su un'infrastruttura variegata. Basato su una build di Chrome OS, CoreOS mantiene un sistema host leggero e utilizza i contenitori Docker per tutte le applicazioni.

## Montaggio di archiviazione portatile

Tutti i file di montaggio secondari vanno nella directory */etc/systemd/system* poiché i montaggi a livello di sistema si trovano n una directory che è di sola lettura in CoreOS. Creerai un file MOUNTPOINT.mount. La sezione Where del file .mount deve corrispondere al nome file. Se il punto di montaggio non è direttamente a partire da / devi denominare il file utilizzando la seguente sintassi: percorso-al-montaggio.mount. Come puoi vedere nel seguente esempio, vogliamo montare l'unità di archiviazione portatile a `/mnt/www` e pertanto denominiamo il file `mnt-www.mount`.

Devi utilizzare fdisk o parted per creare la partizione e assicurarti che il filesystem da te creato corrisponda a quello elencato nel file `.mount`, altrimenti l'avvio del servizio non riuscirà.


```
[Unit]
Description = Mount for Portable Storage

[Mount]
What=/dev/xvdc1
Where=/mnt/www
Type=ext4

[Install]
WantedBy = multi-user.target
```

CoreOS utilizza systemd e quindi, per fare in modo che il punto di montaggio persista nel caso si verificasse un riavvio, devi abilitare il file `*.mount`. Se utilizzi l'indicatore `--now`, la partizione verrà montata immediatamente e impostata per l'avvio al riavvio del computer.

`$ systemctl enable --now mnt-www.mount`

## Montaggio di NFS/{{site.data.keyword.filestorage_short}}

Il processo per montare la nostra {{site.data.keyword.filestorage_short}} Endurance/Performance è praticamente uguale ma, poiché il montaggio è NFS, possiamo specificare delle opzioni aggiuntive utilizzando la riga Options= nel file di montaggio. Nel seguente esempio, stiamo impostando NFS per il montaggio a `/data/www`. Nota: il punto di montaggio NFS dell'istanza {{site.data.keyword.filestorage_short}} può essere ottenuto dalla pagina di elenco {{site.data.keyword.filestorage_short}} oppure richiamando la API SoftLayer_Network_Storage::getNetworkMountAddress().

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
 
## Montaggio di NAS/Cifs

Il montaggio di una condivisione cifs non è supportato in modo nativo in CoreOS ma c'è una soluzione temporanea abbastanza facile per consentire al sistema di montare le condivisioni NAS. Utilizzerai un contenitore per creare il modulo mount.cfis e copiarlo quindi nel sistema CoreOS
 
Sul sistema CoreOS, esegui quanto segue per il download e il rilascio in un contenitore Fedora:  <br/>
`docker run -t -i -v /tmp:/host_tmp fedora /bin/bash`
 
Una volta che ti trovi nel contenitore, dovresti eseguire quanto segue per creare il programma di utilità cifs
```
dnf groupinstall -y "Development Tools" "Development Libraries"
dnf install -y tar
dnf install -y bzip2
curl https://ftp.samba.org/pub/linux-cifs/cifs-utils/cifs-utils-6.4.tar.bz2 | bunzip2 -c - | tar -xvf -
cd cifs-utils-6.4/
./configure && make
cp mount.cifs /host_tmp/
```
 
Ora che il file mount.cifs è stato copiato nella macchina host, puoi uscire dal contenitore docker immettendo il comando `exit` o premendo **ctrl+d**. Quando torni al sistema CoreOS, puoi montare la condivisione CIFS con il seguente comando: <br/>
`/tmp/mount.cifs //nasXXX.service.softlayer.com/USERNAME -o username=USERNAME,password=PASSWORD /path/to/mount`
 
## Montaggio di ISCSI

Ciò non è attualmente supportato in CoreOS ma è previsto per una futura release - https://github.com/coreos/bugs/issues/634
