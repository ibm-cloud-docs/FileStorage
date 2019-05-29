---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, upgrade, migrate to new

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Migración de {{site.data.keyword.filestorage_short}} a {{site.data.keyword.filestorage_short}} mejorado
{: #migratestorage}

{{site.data.keyword.filestorage_full}} mejorado ya está disponible en determinados centros de datos. Para ver la lista de los centros de datos actualizados y las características disponibles, como tasas de IOPS ajustables y volúmenes ampliables, pulse [aquí](/docs/infrastructure/FileStorage?topic=FileStorage-news). Para obtener más información sobre el cifrado gestionado por el proveedor, consulte [Cifrado en reposo de {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-encryption).

El método de migración recomendado es conectarse a ambos volúmenes simultáneamente y transferir datos directamente desde un LUN a otro. Los detalles dependen de su sistema operativo y de si se espera que los datos cambien durante la operación de copia.

El supuesto es que ya tiene su LUN no cifrado conectado al host. Si no es así, siga las directrices correspondientes a su sistema operativo que mejor se ajusten a esta tarea.

- [Montaje de {{site.data.keyword.filestorage_short}} en Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [Montaje de {{site.data.keyword.filestorage_short}} en CentOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [Montaje de {{site.data.keyword.filestorage_short}} en CoreOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)

Todos los volúmenes de {{site.data.keyword.filestorage_short}} mejorados suministrados en estos centros de datos tienen un punto de montaje distinto que los volúmenes no cifrados. Para asegurarse de que utiliza el punto de montaje correcto para los volúmenes de almacenamiento, puede consultar la información sobre el punto de montaje en la página **Detalles del volumen** en la consola. También puede acceder al punto de montaje correcto mediante una llamada de API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}


## Creación de un {{site.data.keyword.filestorage_short}}

Cuando realice un pedido con API, especifique el paquete "Almacenamiento como un servicio" para asegurarse de recibir las características actualizadas con el nuevo almacenamiento.
{:important}

Puede solicitar un LUN mejorado desde el catálogo de {{site.data.keyword.cloud}} y el {{site.data.keyword.slportal}}. El nuevo volumen debe tener el mismo tamaño o mayor que la compartición de archivos original para facilitar la migración.

- [Solicitud de {{site.data.keyword.filestorage_short}} con los niveles de IOPS predefinidos (Resistencia)](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole#endurance)
- [Solicitud de {{site.data.keyword.filestorage_short}} con IOPS personalizados (Rendimiento)](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole#performance)

Su nuevo almacenamiento está preparado para que se monte en pocos minutos. Puede verlo en la lista de recursos y en la lista de {{site.data.keyword.blockstorageshort}}.


## Autorización de un host para el nuevo {{site.data.keyword.filestorage_short}}

Los hosts "autorizados" son hosts a los que se les ha otorgado acceso a un volumen. Sin la autorización del host, no puede acceder ni utilizar el almacenamiento de su sistema.

1. Pulse el nombre del nuevo volumen.
2. Desplácese hasta la sección **Hosts autorizados**.
3. Pulse el enlace **Autorizar host** en la parte derecha. Seleccione los hosts que pueden acceder al volumen.

Una vez autorizado el host, conecte el volumen al host.


## Configuración de instantáneas y réplica

Si se han establecido instantáneas y réplica para el volumen original, debe configurarlas para el nuevo volumen. Configure la réplica, el espacio de instantáneas y cree planificaciones de instantáneas con los mismos valores que el volumen original.

Si su centro de datos de destino no tiene cifrado, no puede establecer la réplica para el nuevo volumen hasta que se actualice el centro de datos.
{:important}


## Migración de los datos

1. Conéctese a los volúmenes de {{site.data.keyword.filestorage_short}} originales y nuevos.
  - Si necesita ayuda para conectar las dos comparticiones de archivos a su host, abra una incidencia de soporte.

2. Piense en el tipo de datos que tiene en el volumen de {{site.data.keyword.filestorage_short}} original y decida la mejor forma de copiarlos en la nueva compartición de archivos.
  - Si tiene copias de seguridad, contenido estático y cosas que no se espera que cambien durante la copia, no debe preocuparse.
  - Si está ejecutando una base de datos o una máquina virtual en su {{site.data.keyword.filestorage_short}}, asegúrese de que los datos no se modifiquen durante la copia para evitar que resulten dañados.
  - Si tiene problemas con el ancho de banda, realice la migración fuera de las horas punta.
  - Si necesita ayuda con estas consideraciones, abra una incidencia de soporte.

3. Copie los datos.
   - **Microsoft
Windows**
     - Para copiar los datos de su LUN de {{site.data.keyword.filestorage_short}} original en el nuevo LUN, formatee el nuevo almacenamiento y copie los archivos encima utilizando Windows Explorer.
   - **Linux**
     - Puede utilizar `rsync` para copiar los datos.
       ```
       [root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage
       ```

   Es una buena idea utilizar el mandato anterior con el distintivo `--dry-run` una vez para asegurarse de que las vías de acceso se alinean correctamente. Si este proceso se interrumpe, puede suprimir el último archivo de destino que se ha copiado para asegurarse de que se copie en la nueva ubicación desde el principio.

   Cuando este mandato finaliza sin el distintivo `--dry-run`, los datos se copian en el nuevo volumen de {{site.data.keyword.filestorage_short}}. Ejecute el mandato de nuevo para asegurarse de que no hace falta nada. También puede revisar manualmente ambas ubicaciones por si se ha omitido algo.

   Una vez completada la migración, puede pasar el entorno de producción al nuevo LUN. A continuación, puede desconectar y suprimir el volumen original de la configuración. La supresión también elimina cualquier instantánea o réplica en el sitio de destino que estuviera asociada al volumen original.
