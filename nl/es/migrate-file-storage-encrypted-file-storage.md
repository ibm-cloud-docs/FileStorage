---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
 
# Migración de {{site.data.keyword.filestorage_short}} a {{site.data.keyword.filestorage_short}} mejorado

{{site.data.keyword.filestorage_full}} mejorado ya está disponible en determinados centros de datos. Para ver la lista de los centros de datos actualizados y las características disponibles, como tasas de IOPS ajustables y volúmenes ampliables, pulse [aquí](new-ibm-block-and-file-storage-location-and-features.html). Para obtener más información sobre el almacenamiento cifrado gestionado por el proveedor, consulte el artículo sobre [Cifrado en reposo de {{site.data.keyword.filestorage_short}}](block-file-storage-encryption-rest.html).

El método de migración recomendado es conectarse a ambos LUN simultáneamente y transferir datos directamente desde un LUN a otro. Los detalles dependen de su sistema operativo y de si se espera que los datos cambien durante la operación de copia. 

Se supone que ya tiene su LUN no cifrado conectado al host. Si no es así, siga las directrices correspondientes a su sistema operativo que mejor se ajusten a esta tarea:

- [Montaje de {{site.data.keyword.filestorage_short}} en Linux](accessing-file-storage-linux.html)
- [Montaje de NFS/{{site.data.keyword.filestorage_short}} en CentOS](mounting-nsf-file-storage.html)
- [Montaje de {{site.data.keyword.filestorage_short}} en CoreOS](mounting-storage-coreos.html)

**NOTA:** Todos los volúmenes de {{site.data.keyword.filestorage_short}} mejorados tienen un punto de montaje distinto que los volúmenes no cifrados. Para asegurarse de que utiliza el punto de montaje correcto para los volúmenes de {{site.data.keyword.filestorage_short}} cifrados y no cifrados, puede consultar la información de punto de montaje en la página **Detalles del volumen** en la interfaz de usuario. También puede acceder al punto de montaje correcto mediante una llamada de API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.


## Crear un nuevo {{site.data.keyword.filestorage_short}}

**IMPORTANTE**: cuando realice un pedido con API, especifique el paquete "Almacenamiento como un servicio" para asegurarse de recibir las características actualizadas con el nuevo almacenamiento.

Las siguientes instrucciones son para solicitar un volumen o una compartición de archivos mejorados a través de la interfaz de usuario. El nuevo volumen debe tener el mismo tamaño o mayor que el volumen original para facilitar la migración.

### Realizar un pedido de un nuevo volumen de almacenamiento de Resistencia

1. En el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, pulse **Almacenamiento** > **{{site.data.keyword.filestorage_short}}** O desde el catálogo de {{site.data.keyword.BluSoftlayer_full}}, pulse **Infraestructura** > **Almacenamiento** > **{{site.data.keyword.filestorage_short}}**.
2. Pulse **Realizar el pedido de {{site.data.keyword.filestorage_short}}** en la esquina superior derecha. 
3. Seleccione **Resistencia** en la lista **Seleccionar tipo de almacenamiento**.
4. Pulse **Ubicación** y seleccione el centro de datos.
   - Asegúrese de que el nuevo almacenamiento se añadirá en la misma ubicación que el original.
5. Seleccione la opción de facturación. Puede elegir entre facturación mensual o por hora.
6. Pulse **Resistencia** y seleccione el nivel de IOPS deseado.
6. Seleccione el **Tamaño de almacenamiento utilizable** en la lista. El nuevo volumen debe tener el mismo tamaño o mayor que el volumen original.
7. Elija el **Tamaño del espacio de instantáneas** (además del espacio utilizable) en la lista desplegable.
8. Pulse **Continuar**. Se muestran los cargos mensuales y prorrateados, es una última oportunidad para revisar los detalles del pedido. Pulse **Anterior** si desea cambiar el pedido.
9. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro** y pulse **Realizar pedido**.
 
### Realizar un pedido de un volumen de almacenamiento de Rendimiento cifrado

1. En el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, pulse **Almacenamiento**, **{{site.data.keyword.filestorage_short}}** O desde el catálogo de {{site.data.keyword.BluSoftlayer_full}}, pulse **Infraestructura** >** Almacenamiento** > **{{site.data.keyword.filestorage_short}}**.
2. Pulse **Realizar el pedido de {{site.data.keyword.filestorage_short}}** en la esquina superior derecha. 
3. Seleccione **Rendimiento** en la lista Seleccionar tipo de almacenamiento.
4. Pulse **Ubicación** y seleccione el centro de datos.
    -  Asegúrese de que el nuevo almacenamiento se añadirá en la misma ubicación que el original.
5. Seleccione las opciones de facturación. Puede elegir entre facturación por hora o mensual.
6. Marque el botón de selección situado junto al **Tamaño de almacenamiento** adecuado.
6. Especifique las IOPS en el campo **Especificar IOPS**.
7. Pulse **Continuar**. Se muestran los cargos mensuales y prorrateados, es una última oportunidad para revisar los detalles del pedido. Pulse **Anterior** si desea cambiar el pedido.
8. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro** y pulse **Realizar pedido**.

El almacenamiento se suministrará en menos de un minuto y estará visible en la página de {{site.data.keyword.filestorage_short}} del {{site.data.keyword.slportal}}.

 
## Conectar el nuevo {{site.data.keyword.filestorage_short}} al host

Los hosts "autorizados" son los hosts a los que se les ha otorgado derechos de acceso a un volumen. Sin la autorización del host, no podrá acceder ni utilizar el almacenamiento de su sistema.

1. Pulse el nombre del nuevo volumen.
2. Desplácese a la sección **Hosts autorizados** de la página.
3. Pulse el enlace **Autorizar host** de la parte derecha de la página. Seleccione los hosts que pueden acceder al volumen.

Una vez autorizado, conecte el volumen al host.

 
## Instantáneas y réplica

¿Tiene instantáneas y réplicas establecidas para su volumen original? Si es así, deberá configurar la réplica, el espacio de instantáneas y crear planificaciones de instantáneas para el nuevo volumen cifrado con los mismos valores que el volumen original. 

**Nota**: Si su centro de datos de destino no se ha actualizado para el cifrado, no podrá establecer la réplica para el nuevo volumen hasta que se actualice el centro de datos.

 
## Migrar los datos

El host debe estar conectado al volumen de {{site.data.keyword.filestorage_short}} original y al cifrado. En caso contrario:

- Asegúrese de que ha seguido correctamente los pasos de este documento y los documentos de referencia.
- Abra una incidencia de soporte para recibir asistencia para conectar los dos volúmenes al host.

### Consideraciones sobre los datos

En este punto, considere qué tipo de datos tiene en el volumen de {{site.data.keyword.filestorage_short}} original y cómo copiarlos mejor al volumen cifrado. Si tiene copias de seguridad, contenido estático y cosas que no se espera que cambien durante la copia, no son consideraciones importantes.

Si está ejecutando una base de datos o una máquina virtual en su {{site.data.keyword.filestorage_short}}, asegúrese de que los datos del volumen original no se modifiquen durante la copia, para que no se corrompan. Si tiene problemas con el ancho de banda, debería realizar la migración fuera de las horas punta. Si necesita ayuda con estas consideraciones, abra una incidencia de soporte.

### Microsoft
Windows

Para copiar los datos desde su volumen de {{site.data.keyword.filestorage_short}} original al volumen cifrado, formatee el nuevo almacenamiento y copie los archivos encima utilizando Windows Explorer.

### Linux

Tenga en cuenta la posibilidad de utilizar `rsync` para copiar los datos. A continuación se muestra un mandato de ejemplo:

```
[root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage
```

Se recomienda utilizar el mandato de ejemplo con el distintivo `--dry-run` una vez para asegurarse de que las vías de acceso se alinean correctamente. Si este proceso se interrumpe, es mejor que suprima el último archivo de destino que se ha copiado para asegurarse de que se copie en la nueva ubicación desde el principio.

Una vez completado este mandato sin el distintivo `--dry-run`, los datos deberían copiarse al volumen de {{site.data.keyword.filestorage_short}} cifrado. Debería ejecutar el mandato de nuevo para asegurarse de que no falta nada. También podría revisar manualmente ambas ubicaciones por si se ha omitido algo.

Una vez completada la migración, podrá pasar la producción al volumen cifrado y desconectar y suprimir el volumen original de la configuración. 

**Nota**: La supresión también eliminará cualquier instantánea o réplica en el sitio de destino que estuviera asociada al volumen original.
