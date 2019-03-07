---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}


# Montaggio di {{site.data.keyword.filestorage_short}} su Container Linux
{: #mountingCoreOS}

Container Linux di CoreOS è un sistema operativo leggero ed open source basato sul kernel Linux. È progettato per fornire l'infrastruttura alle distribuzioni organizzate in cluster. Come sistema operativo, Container Linux fornisce la funzionalità minima richiesta per la distribuzione di applicazioni all'interno di contenitori software, insieme a meccanismi integrati per il rilevamento dei servizi e la condivisione della configurazione. Per ulteriori informazioni, vedi [Mounting storage ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://coreos.com/os/docs/latest/mounting-storage.html)

## Montaggio di archiviazione portatile

Tutti i file di montaggio secondari vanno nella directory `/etc/systemd/system` poiché i montaggi a livello di sistema si trovano n una directory che è di sola lettura. Per prima cosa, devi creare un file `MOUNTPOINT.mount`. La sezione **Where** del file `.mount` deve corrispondere al nome file. Se il punto di montaggio non è direttamente a partire da `/` devi denominare il file utilizzando la sintassi: `percorso-al-montaggio.mount`. Ad esempio, se vuoi montare l'unità di archiviazione portatile a `/mnt/www`, denomina il file `mnt-www.mount`.

Puoi utilizzare `fdisk` o `parted` per creare la partizione. Assicurati che il file da te creato corrisponda a quello elencato nel file system `.mount`, altrimenti l'avvio del servizio non riesce.


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


Questo sistema operativo utilizza `systemd` e quindi, per fare in modo che il punto di montaggio persista nel caso si verificasse un riavvio, devi abilitare il file `*.mount`. Se utilizzi l'indicatore `--now`, la partizione viene montata immediatamente e impostata per l'avvio al riavvio del computer.

```
systemctl enable --now mnt-www.mount
```
{:pre}

Per ulteriori informazioni, vedi la [documentazione di `systemd mount` ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.freedesktop.org/software/systemd/man/systemd.mount.html)

## Montaggio di {{site.data.keyword.filestorage_short}}

Il processo per il montaggio di {{site.data.keyword.filestorage_short}} è lo stesso. Poiché il montaggio è NFS, puoi specificare ulteriori opzioni utilizzando la riga `Options=` nel file di montaggio.

Nell'esempio, NFS è impostato per il montaggio a `/data/www`. Il punto di montaggio dell'istanza di {{site.data.keyword.filestorage_short}} può essere ottenuto dalla pagina di elenco {{site.data.keyword.filestorage_short}} oppure tramite una chiamata API `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}

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
