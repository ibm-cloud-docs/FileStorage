---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-29"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
 
# Migración de {{site.data.keyword.filestorage_short}} a {{site.data.keyword.filestorage_short}} mejorado

{{site.data.keyword.filestorage_full}} mejorado ya está disponible en determinados centros de datos. Para ver la lista de los centros de datos actualizados y las características disponibles, como tasas de IOPS ajustables y volúmenes ampliables, pulse [aquí](new-ibm-block-and-file-storage-location-and-features.html). Para obtener más información sobre el almacenamiento cifrado gestionado por el proveedor, consulte el artículo sobre [Cifrado en reposo de {{site.data.keyword.filestorage_short}}](block-file-storage-encryption-rest.html).

El método de migración recomendado es conectarse a ambos volúmenes simultáneamente y transferir datos directamente desde un LUN a otro. Los detalles dependen de su sistema operativo y de si se espera que los datos cambien durante la operación de copia. 

Se supone que ya tiene su LUN no cifrado conectado al host. Si no es así, siga las directrices correspondientes a su sistema operativo que mejor se ajusten a esta tarea.

- [Montaje de {{site.data.keyword.filestorage_short}} en Linux](accessing-file-storage-linux.html)
- [Montaje de NFS/{{site.data.keyword.filestorage_short}} en CentOS](mounting-nsf-file-storage.html)
- [Montaje de {{site.data.keyword.filestorage_short}} en CoreOS](mounting-storage-coreos.html)

>**NOTA**: Todos los volúmenes de {{site.data.keyword.filestorage_short}} mejorados tienen un punto de montaje distinto que los volúmenes no cifrados. Para asegurarse de que utiliza el punto de montaje correcto para los volúmenes de {{site.data.keyword.filestorage_short}} cifrados y no cifrados, puede consultar la información de punto de montaje en la página **Detalles del volumen** en {{site.data.keyword.slportal}}. También puede acceder al punto de montaje correcto mediante una llamada de API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.


## Creación de un nuevo {{site.data.keyword.filestorage_short}}

**IMPORTANTE**: Cuando realice un pedido con API, especifique el paquete "Almacenamiento como un servicio" para asegurarse de recibir las características actualizadas con el nuevo almacenamiento.

Las siguientes instrucciones son para solicitar un volumen/compartición de archivos mejorado a través del catálogo de {{site.data.keyword.BluSoftlayer_full}}/{{site.data.keyword.slportal}}. El nuevo volumen debe tener el mismo tamaño o mayor que el volumen original para facilitar la migración.

### Realización del pedido de un nuevo volumen de almacenamiento de Resistencia

1. En el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, pulse **Almacenamiento** > **{{site.data.keyword.filestorage_short}}** O desde el catálogo de {{site.data.keyword.BluSoftlayer_full}}, pulse **Infraestructura** > **Almacenamiento** > **{{site.data.keyword.filestorage_short}}**.
2. Pulse **Realizar pedido de {{site.data.keyword.filestorage_short}}**. 
3. Seleccione **Resistencia** en la lista **Seleccionar tipo de almacenamiento**.
4. Pulse **Ubicación** y seleccione el centro de datos.
   - Asegúrese de que el nuevo almacenamiento se añade en la misma ubicación que el original.
5. Seleccione la opción de facturación. Puede elegir entre facturación mensual o por hora.
6. Pulse **Resistencia** y seleccione el nivel de IOPS deseado.
6. Seleccione el **Tamaño de almacenamiento utilizable** en la lista. El nuevo volumen debe tener el mismo tamaño o mayor que el volumen original.
7. Elija el **Tamaño del espacio de instantáneas** (además del espacio utilizable) en la lista.
8. Pulse **Continuar**. Se muestran los cargos mensuales y prorrateados, es una última oportunidad para revisar los detalles del pedido. Pulse **Anterior** si desea cambiar el pedido.
9. Marque el recuadro de selección **He leído el Acuerdo de servicio maestro** y pulse **Realizar pedido**.
 
### Realización del pedido de un volumen de almacenamiento de Rendimiento cifrado

1. En el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, pulse **Almacenamiento**, **{{site.data.keyword.filestorage_short}}** O desde el catálogo de {{site.data.keyword.BluSoftlayer_full}}, pulse **Infraestructura** >** Almacenamiento** > **{{site.data.keyword.filestorage_short}}**.
2. Pulse **Realizar pedido de {{site.data.keyword.filestorage_short}}**. 
3. Seleccione **Rendimiento** en la lista **Seleccionar tipo de almacenamiento**.
4. Pulse **Ubicación** y seleccione el centro de datos.
    -  Asegúrese de que el nuevo almacenamiento se añade en la misma ubicación que el original.
5. Seleccione las opciones de facturación. Puede elegir entre facturación por hora o mensual.
6. Marque el botón de selección situado junto al **Tamaño de almacenamiento** adecuado.
6. Especifique las IOPS en el campo **Especificar IOPS**.
7. Pulse **Continuar**. Se muestran los cargos mensuales y prorrateados, es una última oportunidad para revisar los detalles del pedido. Pulse **Anterior** si desea cambiar el pedido.
8. Marque el recuadro de selección **He leído el Acuerdo de servicio maestro** y pulse **Realizar pedido**.

El almacenamiento se suministra en menos de un minuto y está visible en la página de {{site.data.keyword.filestorage_short}} del {{site.data.keyword.slportal}}.

 
## Autorización de un host para el nuevo {{site.data.keyword.filestorage_short}}

Los hosts "autorizados" son hosts a los que se les ha otorgado acceso a un volumen. Sin la autorización del host, no puede acceder ni utilizar el almacenamiento de su sistema.

1. Pulse el nombre del nuevo volumen.
2. Desplácese hasta la sección **Hosts autorizados**.
3. Pulse el enlace **Autorizar host** en la parte derecha. Seleccione los hosts que pueden acceder al volumen.

Una vez autorizado el host, conecte el volumen al host.

 
## Configuración de instantáneas y réplica

Si se han establecido instantáneas y réplica para el volumen original, debe configurarlas para el nuevo volumen. Configure la réplica, el espacio de instantáneas y cree planificaciones de instantáneas con los mismos valores que el volumen original. 

>**Nota**: Si su centro de datos de destino no tiene cifrado, no puede establecer la réplica para el nuevo volumen hasta que se actualice el centro de datos.

 
## Migración de los datos

1. Conéctese a los volúmenes de {{site.data.keyword.filestorage_short}} originales y nuevos. 
  - Si necesita ayuda para conectar las dos comparticiones de archivos a su host, abra una incidencia de soporte.

2. Piense en el tipo de datos que tiene en el volumen de {{site.data.keyword.filestorage_short}} original y decida la mejor forma de copiarlos en la nueva compartición de archivos. 
  - Si tiene copias de seguridad, contenido estático y cosas que no se espera que cambien durante la copia, no hay ninguna preocupación importante.
  - Si está ejecutando una base de datos o una máquina virtual en su {{site.data.keyword.filestorage_short}}, asegúrese de que los datos no se modifiquen durante la copia para evitar que resulten dañados. Si tiene problemas con el ancho de banda, realice la migración fuera de las horas punta. Si necesita ayuda con estas consideraciones, abra una incidencia de soporte.
 
3. Copie los datos.
   - **Microsoft
Windows** 
     - Para copiar los datos de su LUN de {{site.data.keyword.filestorage_short}} original en el nuevo LUN, formatee el nuevo almacenamiento y copie los archivos encima utilizando Windows Explorer.
   - **Linux** 
     - Puede utilizar `rsync` para copiar los datos.
       ```
       [root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage
       ```
   
   Es una buena idea utilizar el mandato anterior con el distintivo `--dry-run` una vez para asegurarse de que las vías de acceso se alinean correctamente. Si este proceso se interrumpe, puede suprimir el último archivo de destino que se ha copiado para asegurarse de que se copie en la nueva ubicación desde el principio.<br/>
   Cuando este mandato finaliza sin el distintivo `--dry-run`, los datos se copian en el nuevo volumen de {{site.data.keyword.filestorage_short}}. Ejecute el mandato de nuevo para asegurarse de que no hace falta nada. También puede revisar manualmente ambas ubicaciones por si se ha omitido algo.<br/>
   Una vez completada la migración, puede pasar el entorno de producción al nuevo LUN. A continuación, puede desconectar y suprimir el volumen original de la configuración. La supresión también elimina cualquier instantánea o réplica en el sitio de destino que estuviera asociada al volumen original.
