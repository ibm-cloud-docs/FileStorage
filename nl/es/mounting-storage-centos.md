---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-10"

keywords: File Storage, mounting file storage, Linux, CentOS, NFS

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}


# Montaje de {{site.data.keyword.filestorage_short}} en CentOS
{: #mountingCentOS}

Para montar {{site.data.keyword.filestorage_full}} en CentOS 7, primero debe autorizar el host mediante la [consola de {{site.data.keyword.cloud}}](https://{DomainName}/classic){: external} o mediante SLCLI. A continuación, instale los programas de utilidad de NFS como se describe en [Montaje de {{site.data.keyword.filestorage_short}} en Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux).

Para CentOS, puede especificar opciones adicionales utilizando la línea `Options=` en el archivo de montaje. En el siguiente ejemplo, NFS está configurado para montarse en `/data/www`.

El punto de montaje de NFS de la instancia de {{site.data.keyword.filestorage_short}} se puede obtener desde la página de listado de {{site.data.keyword.filestorage_short}} o mediante una llamada a API - `SoftLayer_Network_Storage::getNetworkMountAddress()`.
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

A continuación, habilite el montaje y compruebe que se haya montado correctamente.

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs4 (rw,relatime,vers=4.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
{:codeblock}
