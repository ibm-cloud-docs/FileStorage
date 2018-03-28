---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# NFS/{{site.data.keyword.filestorage_short}} in CentOS anhängen

Der Prozess zum Anhängen von Endurance/Performance {{site.data.keyword.filestorage_full}} in CentOS 7 stimmt im Wesentlichen mit dem Prozess zum [Anhängen von {{site.data.keyword.filestorage_short}} unter RHEL 6](https://console.stage1.bluemix.net/docs/infrastructure/FileStorage/accessing-file-storage-linux.html) überein. Da das Anhängen (Mount) jedoch mit NFS erfolgt, können einige weitere Optionen in der Zeile *Options=* in der Mountdatei angegeben werden. Im folgenden Beispiel wird NFS zum Anhängen an /data/www festgelegt. Beachten Sie, dass der NFS-Mountpunkt der {{site.data.keyword.filestorage_short}}-Instanz über die {{site.data.keyword.filestorage_short}}-Listenseite oder durch einen Aufruf der API `SoftLayer_Network_Storage::getNetworkMountAddress()` abgerufen werden kann.

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
