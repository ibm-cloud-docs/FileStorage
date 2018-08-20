---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-29"

---
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}

# NFS/{{site.data.keyword.filestorage_short}} in CentOS anhängen

Der Prozess zum Anhängen von {{site.data.keyword.filestorage_full}} in CentOS 7 ähnelt dem Prozess zum [Anhängen von {{site.data.keyword.filestorage_short}} unter RHEL 6](accessing-file-storage-linux.html). Da das Anhängen (Mount) jedoch mit NFS erfolgt, können einige weitere Optionen in der Zeile `Options=` in der Mountdatei angegeben werden. Im folgenden Beispiel wird für NFS festgelegt, dass das Anhängen an `/data/www` erfolgt. 

>**Anmerkung** - Der NFS-Mountpunkt der {{site.data.keyword.filestorage_short}}-Instanz kann auf der {{site.data.keyword.filestorage_short}}-Listenseite oder über den API-Aufruf `SoftLayer_Network_Storage::getNetworkMountAddress()` abgerufen werden.

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
