---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, cPanel, backups

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Configuración de {{site.data.keyword.filestorage_short}} para la copia de seguridad con cPanel
{: #cPanelBackups}

Puede utilizar estas instrucciones para configurar que cPanel almacene sus copias de seguridad en {{site.data.keyword.filestorage_full}}. Suponemos que está disponible el acceso de SSH sudo o root y de WebHost Manager (WHM) completo. Este ejemplo se basa en un host **CentOS 7**.

Para obtener más información, consulte [cPanel - Configuración del directorio de copia de seguridad](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){: external}.
{:tip}

1. Conéctese al host a través de SSH.
2. Asegúrese de que un exista un destino de punto de montaje. <br />

   De forma predeterminada, el sistema cPanel guarda los archivos de copia de seguridad en local, en el directorio `/backup`. En este documento, se presupone que la carpeta `/backup` ya existe y contiene copias de seguridad, de modo que se puede utilizar `/backup2` como el nuevo punto de montaje.
   {:note}

3. Configure {{site.data.keyword.filestorage_short}} como se describe en [Acceso a {{site.data.keyword.filestorage_short}} en Red Hat Enterprise Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux) y [Montaje de {{site.data.keyword.filestorage_short}} en CentOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS) o [Montaje de NFS/{{site.data.keyword.filestorage_short}} en CoreOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS). Monte el volumen en `/backup2` y configúrelo en la tabla de sistema de archivos (`/etc/fstab`) para habilitar el montaje al iniciar. <br />

   De forma predeterminada, NFS degrada los archivos creados con los permisos de root al usuario nobody. Para permitir que los clientes root retengan los permisos de root en la unidad compartida NFS, se debe añadir `no_root_squash` a `/etc/exports`.
   {:tip}

4. **Opcional**. Copie las copias de seguridad existentes en el nuevo almacenamiento. Por ejemplo, puede utilizar `rsync`.
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}

    Este mandato comprime y transfiere los datos y conserva todo lo posible, excepto los enlaces fijos. También proporciona información sobre los archivos que se están transfiriendo, además de un breve resumen final.
    {:tip}

5. Inicie una sesión en WebHost Manager y vaya a la configuración de la copia de seguridad a través de **Inicio** > **Copia de seguridad** > **Configuración de copia de seguridad**.

6. Edite la configuración para guardar las copias seguridad en el nuevo punto de montaje.
    - Cambie el directorio de copia de seguridad predeterminado especificando la vía de acceso absoluta a la nueva ubicación en lugar del directorio `/backup/`.
    - Seleccione **Habilitar el montaje de una unidad de copia de seguridad**. Este valor hace que el proceso de configuración compruebe el archivo `/etc/fstab` para el montaje de la copia de seguridad (`/backup2`). <br />

      Si existe un montaje con el mismo nombre que el directorio intermedio, el proceso de configuración de copia de seguridad monta la unidad y realiza aquí la copia de seguridad de la información. Cuando el proceso de copia de seguridad finaliza, desmonta la unidad.
      {:note}
7. Aplique los cambios pulsando **Guardar configuración**.
8. **Opcional**. En función de sus necesidades específicas, elimine el almacenamiento antiguo del servidor y cancélelo desde la cuenta.
