---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-06"

keywords: File Storage, NSF, mounting File Storage, mounting storage on Linux,

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Montaje de {{site.data.keyword.filestorage_short}} en Linux
{: #mountingLinux}

En primer lugar, asegúrese de que el host que va a acceder al volumen de {{site.data.keyword.filestorage_full}} está autorizado mediante el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}.

1. Desde la página de listado de {{site.data.keyword.filestorage_short}}, pulse el enlace **Acciones** que está asociado a la nueva compartición y pulse **Autorizar host**.
2. Seleccione el host o hosts en la lista y pulse **Enviar**. Esta acción autoriza al host a acceder a la compartición.

De manera alternativa, puede autorizar los hosts mediante SLCLI.
```
# slcli file access-authorize --help
Uso: slcli file access-authorize [OPCIONES] ID_VOLUMEN

Opciones:
  -h, --hardware-id TEXTO    El id de un SoftLayer_Hardware que se va a autorizar
  -v, --virtual-id TEXTO     El id de un SoftLayer_Virtual_Guest que se va a autorizar
  -i, --ip-address-id TEXTO  El id de una SoftLayer_Network_Subnet_IpAddress
                            que se va a autorizar
  --ip-address TEXTO         Una dirección IP que se va a autorizar
  -s, --subnet-id TEXTO      El id de una SoftLayer_Network_Subnet_IpAddress
                            que se va a autorizar
  --help                    Mostrar este mensaje y salir.
```

## Montaje de la compartición de {{site.data.keyword.filestorage_short}}

Utilice estas instrucciones para conectar una instancia de cálculo de {{site.data.keyword.cloud}} basada en Linux a una compartición NFS (sistema de archivos de red). El ejemplo se basa en Red Hat Enterprise Linux 6. Los pasos pueden ajustarse para otras distribuciones Linux de acuerdo con la documentación del proveedor del sistema operativo.

El punto de montaje de la instancia de almacenamiento de archivos se puede obtener desde la página de listado de {{site.data.keyword.filestorage_short}} o mediante una llamada a API - `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}

1. Instale las herramientas necesarias.
   ```
   # yum -y install nfs-utils nfs-utils-lib
   ```
   {:pre}

2. Monte la compartición remota.
   ```
   # mount -t "nfs version" -o "options" <mount_point> /mnt
   ```

   Ejemplo
   ```
   # mount -t nfs4 -o hard,intr
   nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt
   ```

3. Compruebe que el montaje se haya realizado correctamente.
   ```
   # df -h
   Filesystem Size Used Avail Use% Mounted on
   /dev/xvda2 25G 1.4G 22G 6% /
   tmpfs 1.9G 0 1.9G 0% /dev/shm
   /dev/xvda1 97M 51M 42M 55%
   ```

4. Vaya al punto de montaje, y los archivos de lectura/escritura.
   ```
   # touch /mnt/test
   # ls -la /mnt
   total 12
   drwxr-xr-x 2 nobody nobody 4096 Sep 8 15:52 .
   dr-xr-xr-x. 22 root root 4096 Sep 8 14:30 ..
   -rw-r--r-- 1 nobody nobody 0 Sep 8 15:52 test
   ```

   Los archivos creados por el usuario root tendrán la propiedad de `nobody:nobody`. Para mostrar la propiedad correctamente, `idmapd.conf` tiene que actualizarse con los valores de dominio correctos. Consulte la sección [Cómo implementar no_root_squash para NFS](#norootsquash).
   {:tip}

5. Monte la compartición remota al iniciar. Para completar la configuración, edite la tabla de sistemas de archivos (`/etc/fstab`) para añadir la compartición remota a la lista de entradas que se montan automáticamente al inicio:

   ```
   (hostname):/(username) /mnt "nfs version" "options" 0 0
   ```

   Ejemplo

   ```
   nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt nfs4 defaults,hard,intr 0 0
   ```

6. Verifique que el archivo de configuración no tiene errores.

   ```
   # mount -fav
   ```
   {:pre}

   Si el mandato finaliza sin errores, la configuración se ha completado.

   Si utiliza NFS 4.1, añada `sec=sys` al mandato de montaje para impedir problemas de propiedad de archivos.
   {:tip}

   Si su sistema operativo host es CentOS, puede configurar más opciones. Para obtener más información, consulte [Montaje de {{site.data.keyword.filestorage_short}} en CentOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS).


## Implementación de `no_root_squash` para NFS (opcional)
{: #norootsquash}

La configuración de `no_root_squash` permite a los clientes root retener los permisos de root en la compartición de NFS.
- Para NFSv3, los clientes no tienen que hacer nada: `no_root_squash` funciona.
- Para NFSv4, debe configurar el dominio nfsv4 en: `slnfsv4.com` e iniciar `rpcidmapd` o un servicio similar utilizado por el sistema operativo.

Ejemplo

1. Desde el host, establezca el dominio en `/etc/idmapd.conf`.

   ```
   #vi /etc/idmapd.conf
   [General]
   #Verbosity = 0
   #Debería establecerse lo siguiente en el nombre de dominio NFSv4 local
   #El valor predeterminado es el nombre de dominio DNS del host.
   Domain = slnfsv4.com
   [Mapping]
   Nobody-User = nobody
   Nobody-Group = nobody
   ```

2. Ejecute `nfsidmap -c`.
3. Inicie `rpcidmapd`.
   ```
   # /etc/init.d/rpcidmapd start
   Starting RPC idmapd: [ OK ]
   ```
## Desmontaje del sistema de archivos

Para desmontar todos los sistemas de archivos montados actualmente en el host, ejecute el mandato `umount` con el nombre del disco o el nombre del punto de montaje.

```
umount /dev/sdb
```
{:pre}

```
umount /mnt
```
{:pre}
