---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Configuración de {{site.data.keyword.filestorage_short}} para la copia de seguridad con Plesk
{: #PleskBackup}

Puede utilizar estas instrucciones para configurar {{site.data.keyword.filestorage_full}} para sus copias de seguridad en Plesk. Suponemos que está disponible el acceso de SSH sudo o root y de Plesk a nivel administrador completo. Este ejemplo se basa en un host CentOS7.

Para obtener más información, consulte la [documentación de Plesk sobre copia de seguridad y restauración ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window}.
{:tip}

1. Conéctese al host a través de SSH.
2. Asegúrese de que un exista un destino de punto de montaje. <br />

   Plesk tiene dos opciones para almacenar copias de seguridad. Una es el almacenamiento interno de Plesk, que es almacenamiento en el servidor Plesk. La otra es almacenamiento FTP externo, que es almacenamiento en algún servidor externo en la web o en la red local. Normalmente en las cajas Plesk, las copias de seguridad internas se almacenan en `/var/lib/psa/dumps` y utilice `/tmp` como directorio temporal. En este ejemplo, el directorio temporal se mantiene local, pero el directorio `dumps` se mueve al destino de {{site.data.keyword.filestorage_short}} (`/backup/psa/dumps`). No se necesitan credenciales de usuario FTP.
   {:note}
3. Configure {{site.data.keyword.filestorage_short}} como se describe en [Acceso a {{site.data.keyword.filestorage_short}} en Red Hat Enterprise Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux) y [Montaje de NFS/{{site.data.keyword.filestorage_short}} en CentOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS) o [Montaje de NFS/{{site.data.keyword.filestorage_short}} en CoreOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS). Monte el volumen en `/backup` y configúrelo en la tabla de sistema de archivos (`/etc/fstab`) para habilitar el montaje al iniciar. <br />

   De forma predeterminada, NFS degrada los archivos creados con los permisos de root al usuario nobody. Para permitir que los clientes root retengan los permisos de root en la unidad compartida NFS, se debe añadir `no_root_squash` a `/etc/exports`.
   {:tip}
4. **Opcional**. Copie las copias de seguridad existentes en el nuevo almacenamiento. Utilice `rsync` por ejemplo:
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {:pre}

   Este mandato comprime y transfiere sus datos conservando todo lo posible, excepto los enlaces fijos. También proporciona información sobre los archivos que se están transfiriendo, además de un breve resumen final.
   {:tip}
5. Edite `/etc/psa/psa.conf` para que apunte al valor de `DUMP_D` en el nuevo destino.
    - Se muestra como: `DUMP_D /backup/psa/dumps`.
6. **Opcional**. En función de sus necesidades específicas, elimine el almacenamiento antiguo del servidor y cancélelo desde la cuenta.
