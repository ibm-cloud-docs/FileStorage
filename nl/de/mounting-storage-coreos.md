---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-08"

keywords: File Storage, file storage, NFS, mounting volume in Container Linux, CoreOS

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}


# {{site.data.keyword.filestorage_short}} unter Container Linux anhängen
{: #mountingCoreOS}

Container Linux von CoreOS ist ein Open-Source-Betriebssystem mit schlankerem Betriebssystem, das auf dem Linux-Kernel basiert. Es ist für die Bereitstellung der Infrastruktur für Clusterimplementierungen konzipiert. Als Betriebssystem stellt Container Linux eine minimale Funktionalität bereit, die für die Implementierung von Anwendungen innerhalb von Softwarecontainern erforderlich ist, sowie integrierte Mechanismen für die Serviceerkennung und die gemeinsame Konfiguration von Konfigurationen. Weitere Informationen finden Sie in [Speicher anhängen](https://coreos.com/os/docs/latest/mounting-storage.html).

Alle sekundären Mountdateien werden in das Verzeichnis `/etc/systemd/system` gestellt, da sich die Mounts auf Systemebene in einem Verzeichnis befinden, das in CoreOS schreibgeschützt ist. Als Erstes müssen Sie die Datei `MOUNTPOINT.mount` erstellen. Der **Where**-Abschnitt in der Mountdatei (`.mount`) muss mit dem Dateinamen übereinstimmen. Wenn der Mountpunkt nicht direkt an `/` anschließt, müssen Sie die Datei mithilfe der Syntax `pfad-zum-mount.mount` angeben. Wenn Sie beispielsweise das portierbare Speicherlaufwerk an `/mnt/www` anhängen wollen, geben Sie der Datei den Namen `mnt-www.mount`.

Sie können `fdisk` oder `parted` verwenden, um die Partition zu erstellen. Stellen Sie sicher, dass das erstellte Dateisystem mit dem in der Datei `.mount` aufgelisteten Dateisystem übereinstimmt, da andernfalls das Starten des Service fehlschlägt. Da das Anhängen (Mount) mit NFS erfolgt, können Sie weitere Optionen in der Zeile `Options=` in der Mountdatei angeben.

Im folgenden Beispiel wird für NFS festgelegt, dass das Anhängen an `/data/www` erfolgt. Der NFS-Mountpunkt der {{site.data.keyword.filestorage_short}}-Instanz kann auf der {{site.data.keyword.filestorage_short}}-Listenseite oder über den API-Aufruf `SoftLayer_Network_Storage::getNetworkMountAddress()` abgerufen werden.
{:tip}

```
$ cat data-www.mount
[Unit]
Description = Mount for Container Storage

[Mount]
What=<NFS-Mountpunkt>
Where=/data/www
Type=nfs
Options=vers=3,sec=sys,noauto

[Install]
WantedBy = multi-user.target
```
{:codeblock}

Aktivieren Sie als Nächstes die Mountoperation und prüfen Sie, ob das Anhängen ordnungsgemäß erfolgt.

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<NFS-Mountpunkt> on /data/www type nfs3 (rw,relatime,vers=3.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
{:codeblock}

Weitere Informationen finden Sie in der Dokumentation für [`systemd mount`](https://www.freedesktop.org/software/systemd/man/systemd.mount.html).
