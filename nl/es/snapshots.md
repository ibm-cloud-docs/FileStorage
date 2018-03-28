---

copyright:
  years: 2014, 2017
lastupdated: "2017-12-11"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Trabajar con
instantáneas

Las instantáneas son una característica de {{site.data.keyword.filestorage_full}}. Una instantánea representa el contenido de un volumen en un momento específico. Las instantáneas le permiten proteger los datos sin afectar al rendimiento, con un consumo mínimo de espacio y son consideradas su primera línea de defensa para la protección de datos. Los datos se pueden restaurar de forma rápida y sencilla desde una copia de instantánea si un usuario modifica o suprime accidentalmente los datos de un volumen con la característica de instantánea.

{{site.data.keyword.filestorage_short}} le ofrece dos formas de realizar las instantáneas: mediante una planificación de instantáneas configurable que crea y suprime copias de instantáneas automáticamente para cada volumen de almacenamiento. También puede crear planificaciones de instantáneas adicionales, suprimir copias manualmente y gestionar las planificaciones en base a sus requisitos. La segunda forma es realizar una instantánea manual.

Una copia de instantánea es una imagen de solo lectura de un volumen de {{site.data.keyword.filestorage_short}}, que captura el estado del volumen en un momento específico. Las copias de instantáneas son extremadamente eficientes, por el tiempo necesario en crearlas y por el espacio de almacenamiento. Una copia de instantánea de {{site.data.keyword.filestorage_short}} solo tarda unos segundos en crearse - normalmente menos de 1 segundo, independientemente del tamaño del volumen o del nivel de actividad en el almacenamiento. Una vez creada una copia de instantánea, los cambios en los objetos de datos se reflejan en actualizaciones de la versión actual de los objetos, como si las copias de instantáneas no existieran. Mientras tanto, la copia de los datos permanece completamente estable. 

Una copia de instantánea no genera sobrecarga de rendimiento, los usuarios pueden almacenar fácilmente hasta 50 instantáneas planificadas y 50 instantáneas manuales por volumen de {{site.data.keyword.blockstorageshort}}, todas ellas accesibles como solo lectura y versiones online de los datos.

Las instantáneas permiten a los usuarios

- Crear puntos de recuperación de un punto en el tiempo sin interrupciones
- Revertir los volúmenes a puntos en el tiempo anteriores
- Debe adquirir cierta cantidad de espacio de instantáneas para su volumen para poder realizar instantáneas. El espacio de instantáneas se puede añadir durante el pedido de volumen inicial o después del suministro inicial, a través de la página Detalles y pulsando el botón desplegable Acciones y seleccionando Añadir espacio de instantáneas. Tenga en cuenta que las instantáneas planificadas y manuales comparten el espacio de instantáneas, realice el pedido del espacio en concordancia.

## Prácticas recomendadas en torno a las instantáneas
El diseño de instantáneas depende del entorno del cliente. Las siguientes consideraciones de diseño le ayudarán a planificar e implementar copias de instantáneas: 
- 	Pueden crearse hasta 50 instantáneas a través de la planificación y hasta 50 manualmente en cada volumen o LUN. 
- 	No supere el máximo de instantáneas. Asegúrese de que la frecuencia de instantáneas planificadas cumpla sus necesidades de objetivo de tiempo de recuperación (RTO) y objetivo de punto de recuperación (RPO), así como los requisitos empresariales de las aplicaciones planificando las instantáneas por hora, a diario o semanalmente. 
- 	La opción de supresión automática de instantáneas debe utilizarse para controlar el crecimiento del consumo de almacenamiento. <br/>
    **Nota**: el umbral de supresión automática está fijado en 95%.
    
## ¿Cómo consumen espacio de disco las copias de instantáneas?
Las copias de instantáneas minimizan el consumo de disco conservando bloques individuales en lugar de archivos completos. Las copias instantáneas empiezan a consumir espacio adicional solo cuando se cambian o suprimen archivos en el sistema de archivos activo. Cuando esto sucede, los bloques de archivos originales aún se conservan como parte de una o más copias de instantáneas.
En el sistema de archivos activo, los bloques modificados se vuelven a escribir en diferentes ubicaciones del disco o se eliminan como bloques de archivos activos por completo. Como resultado, además del espacio de disco utilizado por los bloques en el sistema de archivos activo modificado, aún se reserva espacio de disco utilizado por los bloques originales para reflejar el estado del sistema de archivos activo antes del cambio.

<table>
    <colgroup>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
    </colgroup>
    <tbody>
      <tr>
        <th colspan="3" style="border: 0.0px;text-align: center;">Uso de espacio de disco antes y después de una copia de instantánea</th>
     </tr><tr>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle1.png" alt="Antes de una copia de instantánea"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle3.png" alt="Después de una copia de instantánea"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle2.png" alt="Cambios después de una copia de instantánea"></td>
     </tr><tr>
        <td style="border: 0.0px;">Antes de crear cualquier copia de instantánea, el espacio de disco solo es consumido por el sistema de archivos activo.</td>
        <td style="border: 0.0px;">Después de crear una copia de instantánea, el sistema de archivos activo y la copia de instantánea apuntan a los mismos bloques de discos. La copia de instantánea no utiliza espacio de disco adicional.</td>
        <td style="border: 0.0px;">Tras suprimir <i>myfile.txt</i> del sistema de archivos activo, la copia de instantánea aún incluye el archivo y hace referencia a sus bloques de discos. Por ello, suprimir los datos del sistema de archivos activo no siempre libera espacio de disco.</td>
      </tr>
    </tbody>
</table>



## ¿Cómo adquiero espacio de instantáneas?

Para crear instantáneas de su volumen de almacenamiento, automática o manualmente, necesita adquirir espacio para mantenerlas. Puede adquirir capacidad hasta la cantidad de su volumen de almacenamiento (durante la adquisición de volumen inicial o posteriormente siguiendo los pasos a continuación).

1. Acceda a su Almacenamiento a través del separador **Almacenamiento** > **{{site.data.keyword.filestorage_short}}** del [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}
2. Pulse el enlace Añadir espacio de instantáneas en el marco Instantáneas. Pulse el enlace **Adquirir ahora espacio de instantáneas** en el marco Instantáneas.
3. Seleccione la cantidad de espacio que necesita pulsando el botón de selección situado junto a la cantidad adecuada.
4. Pulse **Continuar**.
5. Especifique cualquier código promocional que tenga y pulse **Recalcular**. Las opciones Cargos para este pedido y Revisión del pedido tendrán valores predeterminados.
6. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro…** y pulse **Realizar pedido**. El espacio de instantáneas se suministrará en unos minutos.

## ¿Cómo creo una planificación de instantáneas?

Las planificaciones de instantáneas le permiten decidir con qué frecuencia y cuándo desea crear una referencia de un punto en el tiempo de su volumen de almacenamiento. Puede tener un máximo de 50 instantáneas del volumen de almacenamiento. Las planificaciones se gestionan a través del separador **Almacenamiento** > **{{site.data.keyword.filestorage_short}}** del [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

Antes de poder configurar la planificación inicial, debe adquirir el espacio de instantáneas si no lo ha comprado durante el suministro inicial del volumen de almacenamiento.

### Añadir una planificación de instantáneas

Las planificaciones de instantáneas se pueden configurar para intervalos por horas, diarios y semanales, cada uno con un ciclo de retención distinto. Hay un máximo de 50 instantáneas planificadas, que pueden ser una combinación de planificaciones por horas, diarias y semanales, e instantáneas manuales por volumen de almacenamiento.

1. Pulse en el volumen de almacenamiento, seleccione el recuadro desplegable **Acciones** y pulse **Planificar instantánea**.
2. En el diálogo Nueva planificación de instantáneas, puede elegir entre tres frecuencias de instantáneas distintas. Utilice cualquier combinación de las tres para crear una planificación de instantáneas completa.
   - Por hora
      - Especifique el minuto de cada hora a la que deberá realizarse una instantánea; el valor predeterminado es el minuto actual.
      - Especifique el número de instantáneas por hora que se conservarán antes de descartar las más antiguas.
   - Diario
      - Especifique la hora y el minuto a la que deberá realizarse una instantánea; el valor predeterminado es la hora y el minuto actual.
      - Especifique el número de instantáneas diarias que se conservarán antes de descartar las más antiguas.
   - Semanal
      - Especifique el día de la semana, la hora y el minuto a la que deberá realizarse una instantánea; el valor predeterminado es el día, hora y minuto actual.
      - Especifique el número de instantáneas semanales que se conservarán antes de descartar las más antiguas.
3. Pulse **Guardar** y cree otra planificación con una frecuencia diferente. Tenga en cuenta que recibirá un mensaje de aviso y no podrá guardar si el número total de instantáneas planificadas es superior a 50.

Se mostrará una lista de las instantáneas a medida que se realizan en la sección Instantáneas de la página de detalles.

## ¿Cómo realizo una instantánea manual?

Las instantáneas manuales se pueden realizar en distintos puntos durante una actualización o mantenimiento de la aplicación. También se permite realizar instantáneas en varias máquinas que se hayan desactivado temporalmente a nivel de aplicación.

Hay un máximo de 50 instantáneas manual por volumen de almacenamiento.

1. Pulse en el volumen de almacenamiento.
2. Pulse el recuadro desplegable Acciones.
3. Pulse **Realizar instantánea manual**.
La instantánea se realizará y se mostrará en la sección Instantáneas de la página de detalles. Su planificación será Manual.

## ¿Cómo visualizo una lista de instantáneas con las funciones de gestión y espacio consumido?

Se puede visualizar una lista de las instantáneas retenidas y el espacio consumido en la página de detalles (**Almacenamiento** > **{{site.data.keyword.filestorage_short}}**). Las funciones de gestión (editar planificaciones y añadir más espacio) se realizan en la página **Detalle** utilizando el menú desplegable **Acciones** o enlaces en las distintas secciones de la página.

## ¿Cómo visualizo una lista de instantáneas retenidas?

Las instantáneas retenidas se basan en el número que haya especificado en **Mantener el último campo** al configurar las planificaciones. Puede visualizar las instantáneas que se han realizado en la sección **Instantánea**. Las instantáneas se listan por planificación.

## ¿Cómo visualizo cuánto espacio de instantáneas se ha utilizado?

El gráfico circular en la parte superior de la página **Detalles** muestra cuánto espacio se ha utilizado y cuánto espacio queda. Recibirá notificaciones cuando empiece a alcanzar los umbrales de espacio – 75%, 90%, y 95%.

## ¿Cómo cambio la cantidad de espacio de instantáneas para mi volumen?

Es posible que necesite añadir espacio de instantáneas a un volumen que anteriormente no tuviera o que requiera espacio de instantáneas adicional. Puede añadir entre 5 GB y 4.000 GB, en función de sus necesidades. Nota: el espacio de instantáneas sólo se puede aumentar, no reducir. Le conviene seleccionar una cantidad de espacio inferior hasta que determine cuánto espacio necesita realmente. Recuerde, las instantáneas automatizadas y manuales comparten el mismo espacio.

El espacio de instantáneas se cambia a través de **Almacenamiento** > **{{site.data.keyword.filestorage_short}}**.

1. Pulse en los volúmenes de almacenamiento, seleccione el recuadro desplegable **Acciones** y pulse **Añadir más espacio de instantáneas**.
2. Seleccione entre un rango de tamaños que se le ofrece. Los tamaños normalmente oscilan entre 0 y el tamaño de su volumen.
3. Pulse **Continuar** para suministrar el espacio adicional.
4. Especifique cualquier código promocional que tenga y pulse **Recalcular**. Las opciones **Cargos para este pedido** y **Revisión del pedido** muestran valores predeterminados.
5. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro…** y pulse **Realizar pedido**. El espacio de instantáneas adicional se suministrará en unos minutos.

## ¿Cómo recibo notificaciones cuando me acerque al límite de espacio de instantáneas y las instantáneas se supriman?

Las notificaciones se envían a través de incidencias de soporte desde el Soporte al Usuario maestro en su cuenta, cuando alcance tres umbrales de espacio distintos – 75%, 90%, y 95%.

- 75% de capacidad: se envía un aviso de que la utilización de espacio de instantáneas ha superado el 75%. Si presta atención al aviso y manualmente añade espacio o suprime instantáneas retenidas innecesarias, la acción se anota y la incidencia se cierra. Si no hace nada, deberá reconocer la incidencia manualmente y se cerrará.
- 90% de capacidad: se envía un segundo aviso cuando la utilización de espacio de instantáneas ha superado el 90%. Al igual que cuando alcanza el 75% de capacidad, si realiza las acciones necesarias para disminuir el espacio utilizado, la acción se anota y la incidencia se cierra. Si no hace nada, deberá reconocer la incidencia manualmente y se cerrará.
- 95% de capacidad: se envía un aviso final. Si no se realiza ninguna acción para reducir el espacio por debajo del umbral, se genera una notificación y se produce la supresión automática para que se puedan crear instantáneas futuras. Las instantáneas planificadas se suprimen, empezando por las más antiguas, hasta que el uso cae por debajo del 95%, y se seguirán suprimiendo cada vez que la utilización supere el 95% hasta quedar por debajo del umbral. Si el espacio se aumenta manualmente o las instantáneas se suprimen, el aviso se restablece y se volverá a enviar si se vuelve a superar el umbral. Si no se lleva a cabo ninguna acción, este será el único aviso que se reciba.

## ¿Cómo suprimo una planificación de instantáneas?

Las planificaciones de instantáneas se pueden cancelar a través de **Almacenamiento** > **{{site.data.keyword.filestorage_short}}**.

1. Pulse en la planificación que se va a suprimir en el marco **Planificaciones de instantáneas** de la página **Detalles**.
2. Marque el recuadro de selección junto a la planificación que se va a suprimir y pulse **Guardar**.<br/>
Precaución: si está utilizando la característica de réplica, asegúrese de que la planificación que está suprimiendo no sea la planificación utilizada por la réplica. Pulse [aquí](replication.html) para obtener más información sobre cómo suprimir una planificación de réplica.

## ¿Cómo suprimo una instantánea?

Las instantáneas que ya no se necesiten se pueden eliminar manualmente para liberar espacio para futuras instantáneas. La supresión se realiza a través de **Almacenamiento** > **{{site.data.keyword.filestorage_short}}**.

1. Pulse en el volumen de almacenamiento y desplácese a la sección Instantáneas para ver una lista de las instantáneas existentes.
2. Pulse la lista desplegable Acciones situada junto a una instantánea particular y pulse **Suprimir** para suprimir la instantánea. Esto no afectará a ninguna instantánea anterior ni futura de la misma planificación, ya que no existe dependencia entre instantáneas.

Las instantáneas manuales que no se supriman del modo descrito anteriormente, se suprimirán automáticamente (las más antiguas primero) cuando alcance las limitaciones de espacio.

## ¿Cómo restauro mi volumen de almacenamiento a un momento específico utilizando una instantánea?

Es posible que necesite recuperar el volumen de almacenamiento a un momento específico debido a un error de usuario o una corrupción de datos.

1. Desmonte y desconecte el volumen de almacenamiento del host.
   - Pulse [aquí](accessing-file-storage-linux.html) para obtener las instrucciones de {{site.data.keyword.filestorage_short}} en Linux.
2. Pulse **Almacenamiento**, **{{site.data.keyword.filestorage_short}}** en el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
3. Desplácese y pulse el volumen que se va a restaurar. La sección Instantáneas de la página de detalles mostrará una lista de todas las instantáneas guardadas junto con su tamaño y fecha de creación.
4. Pulse el botón **Acciones** de la instantánea que se va a utilizar y pulse **Restaurar**. 

   **Nota**: la realización de una restauración provocará la pérdida de los datos que se hayan creado o modificado desde que la instantánea utilizada se creó. Una vez completada, el volumen de almacenamiento volverá al mismo estado que cuando se realizó la instantánea. Aparecerá una solicitud que le informará de ello.
5. Pulse **Sí** para iniciar la restauración. Recibirá un mensaje en la parte superior de la página indicando que el volumen se ha restaurado utilizando la instantánea seleccionada. Además, aparecerá un icono junto al volumen en el {{site.data.keyword.filestorage_short}} indicando que hay una transacción activa en curso. Al pasar el ratón sobre el icono se abre un diálogo que indica la transacción. El icono desaparecerá una vez completada la transacción.
6. Monte y vuelva a conectar el volumen de almacenamiento al host.
   - Pulse [aquí](accessing-file-storage-linux.html) para obtener las instrucciones de {{site.data.keyword.filestorage_short}} en Linux.
   
**Nota**: restaurar un volumen provocará la supresión de todas las instantáneas anteriores a la instantánea restaurada.

