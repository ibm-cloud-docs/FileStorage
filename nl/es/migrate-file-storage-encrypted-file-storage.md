---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
 
# Migración de {{site.data.keyword.filestorage_short}} a {{site.data.keyword.filestorage_short}} cifrado

Se ha iniciado el {{site.data.keyword.filestorage_full}} para Resistencia o Rendimiento en centros de datos seleccionados. A continuación, encontrará información sobre cómo migrar su {{site.data.keyword.filestorage_short}} de no cifrado a cifrado. Para obtener más información sobre el almacenamiento cifrado gestionado por el proveedor, lea el artículo [Cifrado en reposo de {{site.data.keyword.filestorage_short}}](block-file-storage-encryption-rest.html). Para consultar una lista de centros de datos actualizados y características disponibles, pulse [aquí](new-ibm-block-and-file-storage-location-and-features).

La vía de acceso de migración preferida es conectarse a ambos volúmenes simultáneamente y transferir datos directamente dese un volumen de archivos a otro. Los detalles dependerán de su sistema operativo y de si se espera que los datos cambien durante la operación de copia.

Se describen los casos más comunes para su comodidad. Se supone que ya tiene el volumen de archivos no cifrados conectado al host. De lo contrario, siga las direcciones a continuación que mejor se ajusten a su sistema operativo en ejecución para completar esta tarea. 

**NOTA:** todos los volúmenes de {{site.data.keyword.filestorage_short}} cifrados tienen un punto de montaje distinto que los volúmenes no cifrados.  Para asegurarse de que utiliza el punto de montaje correcto para los volúmenes de {{site.data.keyword.filestorage_short}} cifrados y no cifrados, puede consultar la información de punto de montaje en la página **Detalles del volumen** en la interfaz de usuario, así como acceder al punto de montaje correcto mediante una llamada de API: SoftLayer_Network_Storage::getNetworkMountAddress().

[Acceso a {{site.data.keyword.filestorage_short}} en Linux](accessing-file-storage-linux.html)

## Crear un volumen de archivos cifrados

Efectúe los pasos siguientes para crear un volumen del mismo tamaño o más grande que esté cifrado para facilitar el proceso de migración.

### Realizar un pedido de un volumen de almacenamiento de Resistencia cifrado

1. Pulse **Almacenamiento** > **{{site.data.keyword.filestorage_short}}** desde la página de inicio de [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} O pulse **Infraestructura** > **Almacenamiento** > **{{site.data.keyword.filestorage_short}}** en el catálogo de {{site.data.keyword.BluSoftlayer_full}}.

2. Pulse el enlace **Realizar pedido de {{site.data.keyword.filestorage_short}}** en la página de {{site.data.keyword.filestorage_short}}.

3. Seleccione **Resistencia**.

4. Seleccione el centro de datos donde esté ubicado el volumen original. Tenga en cuenta que el cifrado solo está disponible en centros de datos con un asterisco.

5. Especifique el **nivel de IOPS** deseado.

6. Seleccione la cantidad de espacio de almacenamiento deseada en GB. Para TB, 1 TB equivale a 1.000 GB, y 12 TB equivale a 12.000 GB.

7. Especifique la cantidad deseada de almacenamiento en GB para las instantáneas.

8. Seleccione el **Sistema operativo VMware** de la lista desplegable.

9. Envíe el pedido.
 
### Realizar un pedido de un volumen de almacenamiento de Rendimiento cifrado

1. Pulse **Almacenamiento** > **{{site.data.keyword.filestorage_short}}** desde la página de inicio de [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} O pulse **Infraestructura** > **Almacenamiento** > **{{site.data.keyword.filestorage_short}}** en el catálogo de {{site.data.keyword.BluSoftlayer_full}}.

2. Pulse en **Realizar un pedido de {{site.data.keyword.filestorage_short}}**.

3. Seleccione **Rendimiento**.

4. Seleccione el centro de datos donde esté ubicado el volumen original. Tenga en cuenta que el cifrado solo está disponible en centros de datos con un asterisco (`*`).

5. Seleccione la cantidad deseada de almacenamiento en GB, del mismo tamaño que el volumen original o más grande.

6. Especifique la cantidad de IOPS deseada en intervalos de 100.

7. Seleccione el sistema operativo VMware OS de la lista desplegable.

8. Envíe el pedido.

El almacenamiento se suministrará en menos de un minuto y estará visible en la página de {{site.data.keyword.filestorage_short}} en el portal del cliente.

 
## Conectar un nuevo volumen al host

Los hosts “autorizados” son los hosts a los que se les ha otorgado derechos de acceso a un volumen. Sin la autorización del host, no podrá acceder ni utilizar el almacenamiento de su sistema.

1. Pulse en el **Nombre del volumen** cifrado.

2. Desplácese a la sección **Hosts autorizados** de la página.

3. Pulse el enlace **Autorizar host** de la parte derecha de la página. Seleccione los hosts que pueden acceder al volumen.

Una vez autorizado, conecte el volumen al host.

 
## Instantáneas y réplica

¿Tiene instantáneas y réplicas establecidas para su volumen original? Si es así, deberá configurar la réplica, el espacio de instantáneas y crear planificaciones de instantáneas para el nuevo volumen cifrado con los mismos valores que el volumen original. 

Tenga en cuenta que si su centro de datos de destino no se ha actualizado para el cifrado, no podrá establecer la réplica para el nuevo volumen hasta que actualice el centro de datos.

 
## Migrar los datos

El host debe estar conectado al volumen de {{site.data.keyword.filestorage_short}} original y al cifrado. En caso contrario:

• Asegúrese de que ha seguido correctamente los pasos anteriores y los documentos de referencia.

• Abra una incidencia de soporte para recibir asistencia para conectar los dos volúmenes al host.

### Consideraciones sobre los datos

En este punto, considere qué tipo de datos tiene en el volumen de {{site.data.keyword.filestorage_short}} original y cómo copiarlos mejor al volumen cifrado. Si tiene copias de seguridad, contenido estático y cosas que no se espera que cambien durante la copia, no son consideraciones importantes.

Si está ejecutando una base de datos o una máquina virtual en su {{site.data.keyword.filestorage_short}}, asegúrese de que los datos del volumen original no se modifiquen durante la copia, para que no se corrompan. Si tiene problemas con el ancho de banda, debería realizar la migración fuera de las horas punta. Si necesita ayuda con estas consideraciones, no dude en abrir una incidencia de soporte.

### Microsoft
Windows

Para copiar los datos desde su volumen de {{site.data.keyword.filestorage_short}} original al volumen cifrado, formatee el nuevo almacenamiento y copie los archivos encima utilizando Windows Explorer.

### Linux

Debería considerar utilizar rsync para copiar sobre los datos. A continuación se muestra un mandato de ejemplo

`[root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage` 

Se recomienda utilizar el mandato anterior con el distintivo `--dry-run` una vez para asegurarse de que las vías de acceso se alinean correctamente. Si este proceso se interrumpe, suprima mejor el último archivo de destino que se ha copiado para asegurarse de que se ha copiado desde el principio a la nueva ubicación.

Una vez completado este mandato sin el distintivo `--dry-run`, los datos deberían copiarse al volumen de {{site.data.keyword.filestorage_short}} cifrado. Debería ejecutar el mandato de nuevo para asegurarse de que no falta nada. También podría revisar manualmente ambas ubicaciones por si se ha omitido algo.

Una vez completada la migración, podrá pasar la producción al volumen cifrado y desconectar y suprimir el volumen original de la configuración. Tenga en cuenta que la supresión también eliminará cualquier instantánea o réplica en el sitio de destino que estuviera asociada al volumen original.
