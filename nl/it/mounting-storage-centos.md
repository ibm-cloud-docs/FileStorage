---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-22"

keywords: File Storage, mounting file storage, Linux, CentOS, NFS

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}


# Montaggio di {{site.data.keyword.filestorage_short}} in CentOS
{: #mountingCentOS}

Per montare {{site.data.keyword.filestorage_full}} in CentOS 7, devi autorizzare l'host tramite [{{site.data.keyword.slportal}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/){:new_window} o la SLCLI. Quindi, installa i programmi di utilità NFS come descritto in [Montaggio di {{site.data.keyword.filestorage_short}} su Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux).

Per CentOS, puoi specificare ulteriori opzioni utilizzando la riga `Options=` nel file di montaggio. Nel seguente esempio, NFS è impostato per il montaggio a `/data/www`.

Il punto di montaggio dell'istanza di {{site.data.keyword.filestorage_short}} può essere ottenuto dalla pagina di elenco {{site.data.keyword.filestorage_short}} oppure tramite una chiamata API -`SoftLayer_Network_Storage::getNetworkMountAddress()`.
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
