---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Gestión de instantáneas

## ¿Cómo se crea una planificación de instantáneas?

Puede decidir con qué frecuencia y cuándo desea crear una referencia de un punto en el tiempo de su volumen de almacenamiento mediante la creación de planificaciones de instantáneas. Puede tener un máximo de 50 instantáneas del volumen de almacenamiento. Las planificaciones se gestionan mediante el separador **Almacenamiento** > **{{site.data.keyword.filestorage_short}}** del [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

Para poder configurar la planificación inicial, debe adquirir el espacio de instantáneas si no lo ha comprado durante el suministro inicial del volumen de almacenamiento.

### Añadir una planificación de instantáneas

Las planificaciones de instantáneas se pueden configurar para intervalos por horas, diarios y semanales, cada uno con un ciclo de retención distinto. Hay un máximo de 50 instantáneas planificadas, que pueden ser una combinación de planificaciones por horas, diarias y semanales, e instantáneas manuales por volumen de almacenamiento.

1. Pulse el volumen de almacenamiento, seleccione el recuadro desplegable **Acciones** y pulse **Planificar instantánea**.
2. En el recuadro de diálogo **Nueva planificación de instantáneas**, puede elegir entre tres frecuencias de instantáneas distintas. Puede utilizar cualquier combinación de las tres para crear una planificación de instantáneas completa.
   - Por hora
      - Especifique el minuto de cada hora a la que se tomará una instantánea; el valor predeterminado es el minuto actual.
      - Especifique el número de instantáneas por hora que se conservarán antes de descartar las más antiguas.
   - Diario
      - Especifique la hora y el minuto en que se tomará una instantánea; el valor predeterminado es la hora y el minuto actual.
      - Especifique el número de instantáneas diarias que se conservarán antes de descartar las más antiguas.
   - Semanal
      - Especifique el día de la semana, la hora y el minuto en que se tomará una instantánea; el valor predeterminado es el día, hora y minuto actual.
      - Especifique el número de instantáneas semanales que se conservarán antes de descartar las más antiguas.
3. Pulse **Guardar** y cree otra planificación con una frecuencia diferente. </br> **Nota**: si el número total de instantáneas planificadas es mayor que 50, recibirá un mensaje de aviso y no podrá guardar la planificación.

Se mostrará una lista de las instantáneas a medida que se vayan tomando en la sección **Instantáneas** de la página **Detalles**.

## ¿Cómo se toma una instantánea manual?

Las instantáneas manuales se pueden realizar en distintos puntos durante una actualización o mantenimiento de la aplicación. También puede realizar instantáneas en varios servidores que se hayan desactivado temporalmente a nivel de aplicación.

Hay un máximo de 50 instantáneas manuales por volumen de almacenamiento.

1. Pulse el volumen de almacenamiento.
2. Pulse el recuadro desplegable Acciones.
3. Pulse **Realizar instantánea manual**.

La instantánea se toma inmediatamente y se muestra en la sección Instantáneas de la página **Detalles**. Su planificación aparece como Manual.

## ¿Cómo se visualiza una lista de instantáneas con el espacio utilizado y las funciones de gestión?

Puede ver una lista de las instantáneas retenidas y del espacio utilizado en la página **Detalles** (**Almacenamiento** > **{{site.data.keyword.filestorage_short}}**). Las funciones de gestión (editar planificaciones y añadir más espacio) se realizan en la página **Detalle** utilizando el menú desplegable **Acciones** o enlaces en las distintas secciones de la página.

## ¿Cómo se puede ver una lista de instantáneas retenidas?

Las instantáneas retenidas se basan en el número que haya especificado en **Mantener el último campo** al configurar las planificaciones. Puede visualizar las instantáneas que se han realizado en la sección **Instantánea**. Las instantáneas se listan por planificación.

## ¿Cómo puedo consultar la cantidad de espacio de instantáneas que se ha utilizado?

El gráfico circular en la parte superior de la página **Detalles** muestra cuánto espacio se ha utilizado y cuánto espacio queda. Recibirá notificaciones cuando empiece a alcanzar los umbrales de espacio: 75 %, 90 % y 95 %.

## ¿Cómo cambio la cantidad de espacio de instantáneas para mi volumen?

Es posible que necesite añadir espacio de instantáneas a un volumen que anteriormente no tuviera o que requiera más espacio de instantáneas. Puede añadir entre 5 y 4.000 GB, en función de sus necesidades. 
**Nota**: El espacio de instantáneas sólo se puede aumentar, no reducir. Le conviene seleccionar una cantidad de espacio inferior hasta que determine cuánto espacio necesita realmente. Recuerde, las instantáneas automatizadas y manuales comparten el mismo espacio.

El espacio de instantáneas se cambia a través de **Almacenamiento** > **{{site.data.keyword.filestorage_short}}**.

1. Pulse los volúmenes de almacenamiento, marque el recuadro desplegable **Acciones** y pulse **Añadir más espacio de instantáneas**.
2. Seleccione entre un rango de tamaños que se le ofrece. Los tamaños normalmente oscilan entre 0 y el tamaño de su volumen.
3. Pulse **Continuar** para suministrar el espacio adicional.
4. Especifique cualquier código promocional que tenga y pulse **Recalcular**. Las opciones **Cargos para este pedido** y **Revisión del pedido** muestran valores predeterminados.
5. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro…** y pulse **Realizar pedido**. El espacio de instantáneas adicional se suministrará en unos minutos.

## ¿Cómo puedo recibir notificaciones cuando me acerque al límite de espacio de instantáneas y las instantáneas se supriman?

Se envían notificaciones a través de incidencias de soporte desde el sistema de soporte al usuario maestro en su cuenta cuando se alcanzan tres umbrales de espacio distintos: 75 por ciento, 90 por ciento y 95 por ciento.

- 75 por ciento de capacidad: se envía un aviso que indica que la utilización de espacio de instantáneas ha superado el 75 por ciento. Si presta atención al aviso y añade espacio manualmente o suprime las instantáneas retenidas innecesarias, la acción se anota y la incidencia se cierra. Si no hace nada, deberá reconocer la incidencia manualmente y se cerrará.
- 90 por ciento de capacidad: se envía un segundo aviso que indica que la utilización de espacio de instantáneas ha superado el 90 por ciento. Al igual que sucede cuando se alcanza el 75 por ciento de capacidad, si realiza las acciones necesarias para disminuir el espacio utilizado, la acción se anota y la incidencia se cierra. Si no hace nada, deberá reconocer la incidencia manualmente y se cerrará.
- 95 por ciento de capacidad: se envía un aviso final. Si no se realiza ninguna acción para reducir el espacio por debajo del umbral, se genera una notificación y se produce la supresión automática para que se puedan crear instantáneas futuras. Las instantáneas planificadas se suprimen, empezando por las más antiguas, hasta que el uso cae por debajo del 95 por ciento, y se seguirán suprimiendo cada vez que la utilización supere el 95 por ciento hasta quedar por debajo del umbral. Si el espacio se aumenta manualmente o si se suprimen instantáneas, el aviso se restablece y se volverá a enviar si se vuelve a superar el umbral. Si no se lleva a cabo ninguna acción, este será el único aviso que se reciba.

## ¿Cómo se suprime una planificación de instantáneas?

Las planificaciones de instantáneas se pueden cancelar seleccionando **Almacenamiento** > **{{site.data.keyword.filestorage_short}}**.

1. En el marco **Planificaciones de instantáneas** de la página **Detalles**, pulse en la planificación que se va a suprimir.
2. Junto a la planificación que se va a suprimir, marque el recuadro de selección y pulse **Guardar**.<br/>
**Precaución**: si está utilizando la característica de réplica, asegúrese de que la planificación que está suprimiendo no sea la planificación utilizada por la réplica. Pulse [aquí](replication.html) para obtener más información sobre cómo suprimir una planificación de réplica.

## ¿Cómo se suprime una instantánea?

Las instantáneas que ya no se necesiten se pueden eliminar manualmente para liberar espacio para futuras instantáneas. La supresión se realiza mediante **Almacenamiento** > **{{site.data.keyword.filestorage_short}}**.

1. Pulse el volumen de almacenamiento y desplácese hacia abajo hasta la sección **Instantánea** para ver una lista de las instantáneas existentes.
2. Pulse la lista desplegable **Acciones** situada junto a una instantánea particular y pulse **Suprimir** para suprimir la instantánea. Esto no afectará a ninguna instantánea anterior ni futura de la misma planificación, ya que no existe dependencia entre instantáneas.

Las instantáneas manuales que no se supriman del modo descrito anteriormente, se suprimirán automáticamente (las más antiguas primero) cuando se alcancen las limitaciones de espacio.

## ¿Cómo restauro mi volumen de almacenamiento a un punto del tiempo específico utilizando una instantánea?

Es posible que necesite recuperar el volumen de almacenamiento a un momento específico debido a un error de usuario o una corrupción de datos.

1. Desmonte y desconecte el volumen de almacenamiento del host.
   - Pulse [aquí](accessing-file-storage-linux.html) para obtener las instrucciones de {{site.data.keyword.filestorage_short}} en Linux.
2. Pulse **Almacenamiento**, **{{site.data.keyword.filestorage_short}}** en el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
3. Desplácese y pulse el volumen que se va a restaurar. La sección **Instantáneas** de la página **Detalles** muestra una lista de todas las instantáneas guardadas junto con su tamaño y fecha de creación.
4. Pulse **Acciones** correspondiente a la instantánea que se va a utilizar y pulse **Restaurar**. 

   **Nota**: la realización de una restauración provocará la pérdida de los datos que se hayan creado o modificado desde que la instantánea utilizada se creó. Una termine, el volumen de almacenamiento volverá al mismo estado que cuando se realizó la instantánea. Aparecerá una solicitud que le informará de ello.
5. Pulse **Sí** para iniciar la restauración. Recibirá un mensaje en la parte superior de la página que indicará que el volumen se ha restaurado utilizando la instantánea seleccionada. Además, aparecerá un icono junto al volumen en el {{site.data.keyword.filestorage_short}} indicando que hay una transacción activa en curso. Al pasar el ratón sobre el icono se abre un diálogo que indica la transacción. El icono desaparecerá una vez completada la transacción.
6. Monte y vuelva a conectar el volumen de almacenamiento al host.
   - Pulse [aquí](accessing-file-storage-linux.html) para obtener las instrucciones de {{site.data.keyword.filestorage_short}} en Linux.
   
**Nota**: la restauración de un volumen hará que se supriman todas las instantáneas anteriores a la instantánea restaurada.
