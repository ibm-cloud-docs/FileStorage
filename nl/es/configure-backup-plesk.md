---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-24"

---
{:new_window: target="_blank"}
{:pre: .pre}
 
# Configuración de {{site.data.keyword.filestorage_short}} para la copia de seguridad con Plesk

En este artículo proporcionamos instrucciones para configurar {{site.data.keyword.blockstoragefull}} para las copias de seguridad en Plesk. Suponemos que está disponible el acceso de SSH sudo o root y de Plesk a nivel administrador completo. Este ejemplo se basa en un host CentOS7.

**Nota**: encontrará la documentación de Plesk sobre copia de seguridad y restauración [aquí](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window}.

1. Conéctese al host a través de SSH.

2. Asegúrese de que un exista un destino de punto de montaje. <br />
   **Nota**: Plesk tiene dos opciones para almacenar copias de seguridad. Una es el almacenamiento interno de Plesk (un almacenamiento de copia de almacenamiento en el servidor Plesk). La otra es un almacenamiento FTP externo (un almacenamiento de copia de seguridad en algún servidor externo en la web o en la red local). Normalmente en las cajas Plesk, las copias de seguridad internas se almacenan en `/var/lib/psa/dumps` y utilice `/tmp` como directorio temporal. En nuestro ejemplo, mantenemos el directorio temporal local, pero movemos el directorio de volcado al destino de STaaS (`/backup/psa/dumps`). No se necesitan credenciales de usuario FTP.
   
3. Configure {{site.data.keyword.filestorage_short}} como se describe en [Acceso a {{site.data.keyword.filestorage_short}} en Red Hat Enterprise Linux](accessing-file-storage-linux.html) y [Montaje de NFS/{{site.data.keyword.filestorage_short}} en CentOS](mounting-nsf-file-storage.html). Monte el volumen en `/backup` y configúrelo en la tabla de sistema de archivos `/etc/fstab` para habilitar el montaje al arrancar. <br />
   **Nota**: De forma predeterminada, NFS degrada los archivos creados con los permisos de root al usuario nobody. Para permitir que los clientes root retengan los permisos de root en la unidad compartida NFS, se debe añadir `no_root_squash` a `/etc/exports`. <br />
   **Nota**: hay instrucciones disponibles para [Montar NFS/{{site.data.keyword.filestorage_short}} en CoreOS](mounting-storage-coreos.html) también. <br />

4. **Opcional**: Copie las copias de seguridad existentes en el nuevo almacenamiento. Utilice `rsync` por ejemplo:
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {:pre}
    
    **Nota**: este mandato transfiere sus datos comprimidos conservando todo lo posible, excepto los enlaces fijos. También proporciona información sobre los archivos que se están transfiriendo, además de un breve resumen final.
    
5. Edite `/etc/psa/psa.conf` para que apunte al valor de `DUMP_D` en el nuevo destino. 
    - Debería aparecer como: `DUMP_D /backup/psa/dumps`. 

6. **Opcional**: en función de sus necesidades específicas, elimine el almacenamiento antiguo del servidor y cancélelo desde la cuenta.

