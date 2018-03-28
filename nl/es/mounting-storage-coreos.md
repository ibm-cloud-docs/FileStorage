---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Montaje de {{site.data.keyword.filestorage_short}} en CoreOS

CoreOS es una potente distribución de Linux creada para facilitar la gestión de despliegues grandes y escalables en varias infraestructuras. Basado en una compilación del sistema operativo Chrome, CoreOS mantiene un sistema de host ligero y utiliza contenedores Docker para todas las aplicaciones.

## Montaje de almacenamiento portátil

Todos los archivos de montaje secundario van al directorio */etc/systemd/system* ya que los montajes de nivel de sistema están en un directorio que es solo de lectura en CoreOS. Creará un archivo MOUNTPOINT.mount. La sección Where del archivo .mount debe coincidir con el nombre de archivo. Si el punto de montaje no está desactivado directamente / tendrá que nombrar el archivo utilizando la siguiente sintaxis: path-to-mount.mount. Como se puede ver en el siguiente ejemplo, queremos montar la unidad de almacenamiento portátil en `/mnt/www`, por lo que nombramos el archivo `mnt-www.mount`.

Debe utilizar un disco completo o particionado para crear la partición y asegurarse de que el sistema de archivos que crea coincide con el listado en el archivo `.mount`, si no el servicio no podrá iniciarse.


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

CoreOS utiliza systemd, de manera que para que el punto de montaje sobreviva a un rearranque deberá habilitar el archivo `*.mount`. Si utiliza el distintivo `--now`, la partición se montará inmediatamente y se iniciará al arrancar.

`$ systemctl enable --now mnt-www.mount`

## Montaje de NFS/{{site.data.keyword.filestorage_short}}

El proceso de montar nuestro {{site.data.keyword.filestorage_short}} de Resistencia/Rendimiento es muy similar, pero como el montaje es NFS, podemos especificar algunas opciones adicionales utilizando la línea Options= del archivo de montaje. En el ejemplo siguiente, establecemos el NFS para su montaje en `/data/www`. Tenga en cuenta que el punto de montaje de NFS de la instancia de {{site.data.keyword.filestorage_short}} se puede obtener desde la página de listado de {{site.data.keyword.filestorage_short}} o mediante una llamada a API -SoftLayer_Network_Storage::getNetworkMountAddress().

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

Ahora podemos habilitar el montaje y comprobar que se haya montado correctamente.

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs4 (rw,relatime,vers=4.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
 
## Montaje de NAS/Cifs

Montar una compartición de cifs no está soportado de forma nativa en CoreOS, pero existe un método alternativo bastante sencillo para permitir que el sistema de host monte comparticiones de NAS. Utilizará un contenedor para crear el módulo mount.cfis y, a continuación, lo copiará en el sistema CoreOS
 
En el sistema CoreOS, ejecute el siguiente mandato para descargarlo y soltarlo en un contenedor Fedora:  <br/>
`docker run -t -i -v /tmp:/host_tmp fedora /bin/bash`
 
Cuando esté en el contenedor, ejecute el siguiente mandato para crear el programa de utilidad cifs
```
dnf groupinstall -y "Development Tools" "Development Libraries"
dnf install -y tar
dnf install -y bzip2
curl https://ftp.samba.org/pub/linux-cifs/cifs-utils/cifs-utils-6.4.tar.bz2 | bunzip2 -c - | tar -xvf -
cd cifs-utils-6.4/
./configure && make
cp mount.cifs /host_tmp/
```
 
Ahora que el archivo mount.cifs se ha copiado en la máquina host, puede salir del contenedor docker emitiendo el mandato `exit` o pulsando **ctrl+d**. Una vez de vuelta en el sistema CoreOS, puede montar la compartición de CIFS con el siguiente mandato: <br/>
`/tmp/mount.cifs //nasXXX.service.softlayer.com/USERNAME -o username=USERNAME,password=PASSWORD /path/to/mount`
 
## Montaje de ISCSI

Esta opción no está soportada en CoreOS en estos momentos, pero se prevé para versiones futuras - https://github.com/coreos/bugs/issues/634
