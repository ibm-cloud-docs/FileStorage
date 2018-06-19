---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}

# Montaje de NFS/{{site.data.keyword.filestorage_short}} en CentOS

El proceso de montar nuestro {{site.data.keyword.filestorage_full}} en CentOS 7 es muy parecido al proceso para el [Montaje de {{site.data.keyword.filestorage_short}} en RHEL 6](accessing-file-storage-linux.html). Sin embargo, como el montaje es NFS, podemos especificar algunas opciones adicionales utilizando la línea *Options=* del archivo de montaje. En el ejemplo siguiente, establecemos el NFS para su montaje en `/data/www`. 

**Nota**: El punto de montaje de NFS de la instancia de {{site.data.keyword.filestorage_short}} se puede obtener desde la página de listado de {{site.data.keyword.filestorage_short}} o mediante una llamada a API -`SoftLayer_Network_Storage::getNetworkMountAddress()`.

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

Ahora podemos habilitar el montaje y comprobar que se haya montado correctamente.

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs4 (rw,relatime,vers=4.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
{:codeblock
