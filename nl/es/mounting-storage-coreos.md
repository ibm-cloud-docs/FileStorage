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


# Montaje de {{site.data.keyword.filestorage_short}} en Container Linux
{: #mountingCoreOS}

Container Linux by CoreOS es un sistema operativo de código abierto y ligero basado en el kernel de Linux. Está diseñado para proporcionar infraestructura a despliegues en clúster. Como sistema operativo, Container Linux proporciona la funcionalidad mínima necesaria para desplegar aplicaciones dentro de contenedores de software, junto con mecanismos incorporados para el descubrimiento de servicios y la compartición de configuración. Para obtener más información, consulte la [Montaje de almacenamiento](https://coreos.com/os/docs/latest/mounting-storage.html)

Todos los archivos de montaje secundario van al directorio `/etc/systemd/system` ya que los montajes de nivel de sistema están en un directorio que es solo de lectura. Primero, cree un archivo `MOUNTPOINT.mount`. La sección **Where** del archivo `.mount` debe coincidir con el nombre de archivo. Si el punto de montaje no está directamente fuera de `/`, debe nombrar el archivo utilizando la sintaxis `path-to-mount.mount`. Por ejemplo, si desea montar la unidad de almacenamiento portátil en `/mnt/www`, nombre el archivo `mnt-www.mount`.

Puede utilizar `fdisk` o `parted` para crear la partición. Asegúrese de que el sistema de archivos que crea coincide con el listado en el archivo `.mount`; si no, el servicio no se inicia. Como el montaje es NFS, puede especificar más opciones utilizando la línea `Options=` del archivo de montaje.

En el siguiente ejemplo, NFS está configurado para montarse en `/data/www`. El punto de montaje de NFS de la instancia de {{site.data.keyword.filestorage_short}} se puede obtener desde la página de listado de {{site.data.keyword.filestorage_short}} o mediante una llamada a API `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}

```
$ cat data-www.mount
[Unit]
Description = Mount for Container Storage

[Mount]
What=<nfs_mount_point>
Where=/data/www
Type=nfs
Options=vers=3,sec=sys,noauto

[Install]
WantedBy = multi-user.target
```
{:codeblock}

A continuación, habilite el montaje y compruebe que se haya montado correctamente.

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs3 (rw,relatime,vers=3.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
{:codeblock}

Para obtener más información, consulte la [documentación de `systemd mount`](https://www.freedesktop.org/software/systemd/man/systemd.mount.html)
