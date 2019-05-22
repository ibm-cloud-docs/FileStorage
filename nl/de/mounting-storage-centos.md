---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-22"

keywords: File Storage, mounting file storage, Linux, CentOS, NFS

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}


# {{site.data.keyword.filestorage_short}} unter CentOS anhängen
{: #mountingCentOS}

Wenn Sie Mountoperationen für {{site.data.keyword.filestorage_full}} unter CentOS 7 durchführen möchten, müssen Sie zuerst über das [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} oder die SLCLI die entsprechenden Berechtigungen für den Host einrichten. Installieren Sie dann die NFS-Dienstprogramme wie in [{{site.data.keyword.filestorage_short}} unter Linux anhängen](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux) beschrieben. 

Für CentOS können Sie zusätzliche Optionen angeben, indem Sie die Zeile `Options=` in der Mountdatei verwenden. Im folgenden Beispiel wird für NFS festgelegt, dass das Anhängen an `/data/www` erfolgt.

Der NFS-Mountpunkt der {{site.data.keyword.filestorage_short}}-Instanz kann auf der {{site.data.keyword.filestorage_short}}-Listenseite oder über den API-Aufruf `SoftLayer_Network_Storage::getNetworkMountAddress()` abgerufen werden.
{:tip}

```
$ cat data-www.mount
[Unit]
Description = Mount for Container Storage

[Mount]
What=<NFS-Mountpunkt>
Where=/data/www
Type=nfs
Options=vers=4,sec=sys,noauto

[Install]
WantedBy = multi-user.target
```
{:codeblock}

Aktivieren Sie als Nächstes die Mountoperation und prüfen Sie, ob das Anhängen ordnungsgemäß erfolgt.

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<NFS-Mountpunkt> on /data/www type nfs4 (rw,relatime,vers=4.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
{:codeblock}
