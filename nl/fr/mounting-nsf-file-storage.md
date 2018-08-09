---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-29"

---
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}

# Montage de NFS/{{site.data.keyword.filestorage_short}} dans CentOS

Le processus de montage de {{site.data.keyword.filestorage_full}} dans CentOS 7 est similaire au processus de [Montage de {{site.data.keyword.filestorage_short}} sur RHEL 6](accessing-file-storage-linux.html), mais étant donné que le montage fait appel à NFS, il est possible d'indiquer certaines options supplémentaires à l'aide de la ligne `Options=` dans le fichier de montage. Dans l'exemple suivant, le système NFS est défini pour un montage dans `/data/www`.  

>**Remarque** - le point de montage NFS de l'instance {{site.data.keyword.filestorage_short}} peut être obtenu sur la page de la liste de {{site.data.keyword.filestorage_short}} ou via un appel API : `SoftLayer_Network_Storage::getNetworkMountAddress()`.

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

A présent, activez le montage et vérifiez que celui-ci est correct. 

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs4 (rw,relatime,vers=4.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
{:codeblock}
