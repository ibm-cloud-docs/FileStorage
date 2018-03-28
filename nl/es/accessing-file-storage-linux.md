---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Acceso a {{site.data.keyword.filestorage_short}} en Linux

Antes de empezar, asegúrese de que el host que accede al volumen de {{site.data.keyword.filestorage_full}} se haya autorizado a través del [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}:

1. Desde la página de listado de {{site.data.keyword.filestorage_short}}, pulse **Acciones** asociadas a la compartición recién suministrada y pulse **Autorizar host**.
2. Seleccione el host(s) deseado de la lista y pulse **Enviar**; esta acción autoriza al host(s) el acceso a la compartición.

## Montaje de la compartición de {{site.data.keyword.filestorage_short}}

A continuación se describen los pasos necesarios para conectar una instancia de cálculo de {{site.data.keyword.BluSoftlayer_full}} basada en Linux a una compartición de Network File System (NFS). El ejemplo se basa en Red Hat Enterprise Linux 6. Los pasos pueden ajustarse para otras distribuciones Linux de acuerdo con la documentación del proveedor del sistema operativo.

**Nota:** el punto de montaje de la instancia de almacenamiento de archivos se puede obtener en la página de listado de {{site.data.keyword.filestorage_short}} o mediante una llamada a API  - SoftLayer_Network_Storage::getNetworkMountAddress().

1. Instale los paquetes/herramientas necesarios.

    `[root@TEST-RHEL6]# yum -y install nfs-utils nfs-utils-lib
    `
2. Monte la compartición remota
    `[root@TEST-RHEL6]# mount -t "nfs version" -o "options" <mount_point> /mnt`
    
    A continuación se muestra un ejemplo de montaje de compartición remota en una instancia de almacenamiento.
    
    `[root@TEST-RHEL6 mnt]# mount -t nfs4 -o hard,intr`
    
    `nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt`
 
3. Compruebe que el montaje se haya realizado correctamente.

    `[root@TEST-RHEL6]# df -h`
    
    `Filesystem Size Used Avail Use% Mounted on`
    
    `/dev/xvda2 25G 1.4G 22G 6% /`
    
    `tmpfs 1.9G 0 1.9G 0% /dev/shm`
    
    `/dev/xvda1 97M 51M 42M 55%`
    
4. Vaya al punto de montaje y los archivos de lectura/escritura.

    `[root@TEST-RHEL6]# touch /mnt/test`
    
    `[root@TEST-RHEL6]# ls -la /mnt`
    
    `total 12`
    
    `drwxr-xr-x 2 nobody nobody 4096 Sep 8 15:52 .`
    
    `dr-xr-xr-x. 22 root root 4096 Sep 8 14:30 ..`
    
    `-rw-r--r-- 1 nobody nobody 0 Sep 8 15:52 test`

    **Nota:** los archivos creados por raíz tendrán la propiedad de nobody:nobody. Para mostrar la propiedad correctamente, idmapd.conf tiene que actualizarse con los valores de dominio correctos. Consulte “Cómo implementar no_root_squash para NFS” a continuación.
    
5. Monte la compartición remota al arrancar. Para completar la configuración, edite la tabla de sistemas de archivos /etc/fstab para añadir la compartición remota a la lista de entradas que se montarán automáticamente al inicio:

    `(hostname):/(username) /mnt "nfs version" "options" 0 0`
    
    Utilizando la instancia desde el ejemplo de montaje de compartición remota, la entrada sería:
    
    `nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt nfs4 defaults,hard,intr 0 0`
    
6.  Compruebe que no haya errores con el archivo de configuración.

    `[root@TEST-RHEL6 /]# mount -fav`
    
    Si el mandato finaliza sin errores, la configuración se ha completado.

**Nota:** si utiliza NFS 4.1, añada 'sec=sys' al mandato de montaje para impedir problemas de propiedad de archivos.

 
## Cómo implementar no_root_squash para NFS (opcional)

La configuración de no_root_squash permite a los clientes root retener los permisos de root en la compartición de NFS. Para NFSv3, los clientes no tienen que hacer nada, no_root_squash debería funcionar.
Para NFSv4, tendrá que configurar el dominio nfsv4 en: slnfsv4.com e iniciar rpcidmapd, o un servicio similar en función de la versión de sistema operativo.

Aquí tiene un ejemplo:

1. Desde el host, establezca el dominio en /etc/idmapd.conf.

    `#vi /etc/idmapd.conf`
    
    `[General]`
    
    `#Verbosity = 0`
    
    `#Debería establecerse lo siguiente en el nombre de dominio NFSv4 local`
    
    `#El valor predeterminado es el nombre de dominio DNS del host.`
    
    `Domain = slnfsv4.com`
    
    `[Mapping]`
    
    `Nobody-User = nobody`
    
    `Nobody-Group = nobody`
    
2. Ejecute nfsidmap -c.
3. Inicie rpcidmapd.

   ` # /etc/init.d/rpcidmapd start`
   
   ` Starting RPC idmapd: [ OK ]`
