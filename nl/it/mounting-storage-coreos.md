---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-29"

---
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}

# Montaggio dell'{{site.data.keyword.filestorage_short}} su CoreOS

CoreOS è una potente distribuzione Linux sviluppata per rendere semplici da gestire delle distribuzioni di grandi dimensioni e scalabili su un'infrastruttura variegata. Basato su una build di Chrome OS, CoreOS mantiene un sistema host leggero e utilizza i contenitori Docker per tutte le applicazioni.

## Montaggio di archiviazione portatile

Tutti i file di montaggio secondari vanno nella directory `/etc/systemd/system` poiché i montaggi a livello di sistema si trovano n una directory che è di sola lettura in CoreOS. Per prima cosa, devi creare un file `MOUNTPOINT.mount`. La sezione **Where** del file `.mount` deve corrispondere al nome file. Se il punto di montaggio non è direttamente a partire da `/` devi denominare il file utilizzando la sintassi: `percorso-al-montaggio.mount`. Ad esempio, se vuoi montare l'unità di archiviazione portatile a `/mnt/www`, denomina il file `mnt-www.mount`.

Puoi utilizzare `fdisk` o `parted` per creare la partizione e assicurarti che il file da te creato corrisponda a quello elencato nel file system `.mount`, altrimenti l'avvio del servizio non riesce.


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
{:codeblock}


CoreOS utilizza `systemd` e quindi, per fare in modo che il punto di montaggio persista nel caso si verificasse un riavvio, devi abilitare il file `*.mount`. Se utilizzi l'indicatore `--now`, la partizione viene montata immediatamente e impostata per l'avvio al riavvio del computer.

```
$ systemctl enable --now mnt-www.mount
```
{:pre}

## Montaggio di NFS/{{site.data.keyword.filestorage_short}}

Il processo per il montaggio di {{site.data.keyword.filestorage_short}} è lo stesso. Poiché il montaggio è NFS, puoi specificare ulteriori opzioni utilizzando la riga `Options=` nel file di montaggio. 

Nell'esempio, NFS è impostato per il montaggio a `/data/www`. Il punto di montaggio dell'istanza di {{site.data.keyword.filestorage_short}} può essere ottenuto dalla pagina di elenco {{site.data.keyword.filestorage_short}} oppure tramite una chiamata API `SoftLayer_Network_Storage::getNetworkMountAddress()`.

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
 
## Montaggio di NAS/CIFS

Il montaggio di una condivisione CIFS non è supportato in modo nativo in CoreOS ma c'è una soluzione temporanea facile per consentire al sistema di montare le condivisioni NAS. Puoi utilizzare un contenitore per creare il modulo `mount.cfis` e copiarlo quindi nel sistema CoreOS
 
Sul sistema CoreOS, esegui quanto segue per il download e il rilascio in un contenitore Fedora.

```
docker run -t -i -v /tmp:/host_tmp fedora /bin/bash
```
{:pre}
 
Una volta che ti trovi nel contenitore, immetti quanto segue per creare il programma di utilità CIFS.

```
dnf groupinstall -y "Development Tools" "Development Libraries"
dnf install -y tar
dnf install -y bzip2
curl https://ftp.samba.org/pub/linux-cifs/cifs-utils/cifs-utils-6.4.tar.bz2 | bunzip2 -c - | tar -xvf -
cd cifs-utils-6.4/
./configure && make
cp mount.cifs /host_tmp/
```
{:codeblock}
 
Ora che il file `mount.cifs` è stato copiato nell'host, puoi uscire dal contenitore docker immettendo il comando `exit` o premendo **ctrl+d**. Quando torni al sistema CoreOS, puoi montare la condivisione CIFS con il seguente comando: 
```
/tmp/mount.cifs //nasXXX.service.softlayer.com/USERNAME -o username=USERNAME,password=PASSWORD /path/to/mount
```
{:pre}
 
## Montaggio di ISCSI

Ciò non è attualmente supportato in CoreOS ma è previsto per una futura release - https://github.com/coreos/bugs/issues/634
