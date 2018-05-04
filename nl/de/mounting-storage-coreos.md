---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.filestorage_short}} unter CoreOS anhängen

CoreOS ist eine leistungsfähige Linux-Distribution, die darauf ausgerichtet ist, die Verwaltung umfangreicher und skalierbarer Bereitstellungen auf verschiedenartiger Infrastruktur zu vereinfachen. Basierend auf einem Build von Chrome OS verwaltet CoreOS ein schlankes Hostsystem und verwendet Docker-Container für alle Anwendungen.

## Portierbaren Speicher anhängen

Alle sekundären Mountdateien werden in das Verzeichnis */etc/systemd/system* verlegt, da sich die Mounts der Systemebene in einem Verzeichnis befinden, das in CoreOS schreibgeschützt ist. Sie erstellen die Datei MOUNTPOINT.mount. Der Where-Abschnitt in der Mountdatei (.mount) muss mit dem Dateinamen übereinstimmen. Wenn der Mountpunkt nicht direkt verfügbar ist, müssen Sie die Datei nach der folgenden Syntax benennen: Pfad-zum-Mount.mount. Im folgenden Beispiel soll das portierbare Speicherlaufwerk an `/mnt/www` angehängt werden, sodass die Datei den Namen `mnt-www.mount` erhält.

Sie müssen 'fdisk' oder 'parted' zum Erstellen der Partition verwenden und sicherstellen, dass das Dateisystem, das Sie erstellen, dem entspricht, das in der Mountdatei (`.mount`) aufgeführt ist. Andernfalls kann der Service nicht starten.


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

CoreOS verwendet 'systemd'. Damit der Mountpunkt auch nach einem Neustart noch verfügbar ist, müssen Sie die Mountdatei (`*.mount`) aktivieren. Wenn Sie das Flag `--now` verwenden, wird die Partition sofort angehängt und auf Starten nach dem Startvorgang (Boot) eingestellt.

`$ systemctl enable --now mnt-www.mount`

## NFS/{{site.data.keyword.filestorage_short}} anhängen

Der Prozess zum Anhängen eines Endurance/Performance {{site.data.keyword.filestorage_short}}-Speichers bleibt im Wesentlichen gleich. Da das Anhängen (Mount) jedoch mit NFS erfolgt, können einige weitere Optionen in der Zeile 'Options=' in der Mountdatei angegeben werden. Im folgenden Beispiel wird NFS für den Mount an `/data/www` eingerichtet. Beachten Sie, dass der NFS-Mountpunkt der {{site.data.keyword.filestorage_short}}-Instanz über die {{site.data.keyword.filestorage_short}}-Listenseite oder durch einen Aufruf der API 'SoftLayer_Network_Storage::getNetworkMountAddress()' abgerufen werden kann.

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

Jetzt kann der Mount aktiviert und geprüft werden, ob er ordnungsgemäß angehängt ist.

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<NFS-Mountpunkt> on /data/www type nfs4 (rw,relatime,vers=4.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
 
## NAS/CIFS anhängen

Das Anhängen einer CIFS-Freigabe wird in CoreOS nicht nativ unterstützt, jedoch ist eine recht einfache Ausweichlösung verfügbar, durch die NAS-Freigaben an das Hostsystem angehängt werden können. Dazu verwenden Sie einen Container, um die Moduldatei 'mount.cfis' zu erstellen und diese anschließend auf das CoreOS-System zu kopieren.
 
Führen Sie auf dem CoreOS-System den folgenden Befehl aus, um die Moduldatei herunterzuladen und in einem Fedora-Container abzulegen:  <br/>
`docker run -t -i -v /tmp:/host_tmp fedora /bin/bash`
 
Sobald Sie sich in dem Container befinden, können Sie die folgenden Befehle ausführen, um das CIFS-Dienstprogramm zu erstellen:
```
dnf groupinstall -y "Development Tools" "Development Libraries"
dnf install -y tar
dnf install -y bzip2
curl https://ftp.samba.org/pub/linux-cifs/cifs-utils/cifs-utils-6.4.tar.bz2 | bunzip2 -c - | tar -xvf -
cd cifs-utils-6.4/
./configure && make
cp mount.cifs /host_tmp/
```
 
Jetzt, da die Datei 'mount.cifs' auf die Hostmaschine kopiert wurde, können Sie den Docker-Container verlassen, indem Sie den Befehl `exit` ausführen oder die Tastenkombination **STRG+d** drücken. Wenn Sie wieder im CoreOS-System sind, können Sie die CIFS-Freigabe mit dem folgenden Befehl anhängen: <br/>
`/tmp/mount.cifs //nasXXX.service.softlayer.com/BENUTZERNAME -o username=BENUTZERNAME,password=KENNWORT /Pfad/zum/Mount`
 
## ISCSI anhängen

Dies wird in CoreOS gegenwärtig nicht unterstützt, wird jedoch für ein zukünftiges Release vorbereitet: https://github.com/coreos/bugs/issues/634
