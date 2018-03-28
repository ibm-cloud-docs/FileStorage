---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-06"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Pedido y gestión de {{site.data.keyword.filestorage_full_notm}}

Existen dos tipos distintos de almacenamiento que puede suministrar en función de sus necesidades y preferencias. Las dos opciones de volúmenes de {{site.data.keyword.filestorage_short}} son:

- **Resistencia**: suministro de niveles de resistencia que presentan niveles de rendimiento predefinidos y características como instantáneas y réplica.
- **Rendimiento**: crear un entorno de rendimiento de alta potencia donde puede asignar las operaciones de entrada/salida deseadas por segundo (IOPS).

## Suministro de {{site.data.keyword.filestorage_short}}

### Cómo solicitar Resistencia para {{site.data.keyword.filestorage_short}}

1. Desde el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, pulse **Almacenamiento** > **{{site.data.keyword.filestorage_short}}** O desde el catálogo de {{site.data.keyword.BluSoftlayer_full}}, pulse **Infraestructura** > **Almacenamiento** > **{{site.data.keyword.filestorage_short}}**.
2. Pulse **Realizar el pedido de {{site.data.keyword.filestorage_short}}** en la esquina superior derecha de la pantalla. 
3. Seleccione **Resistencia** en la lista desplegable **Seleccionar tipo de almacenamiento**.
4. Pulse la lista desplegable **Ubicación** y seleccione el centro de datos.
   - Asegúrese de que el nuevo almacenamiento se añada en la misma ubicación que el host pedido anteriormente.
   - Si ha seleccionado un centro de datos con prestaciones mejoradas (marcados con un * en el desplegable), tendrá la opción de elegir entre facturación mensual o por horas. 
     1. Con la **facturación por horas**, el cálculo del número de horas que el volumen de archivos ha existido en la cuenta se realiza en el momento en que se suprime el volumen o al final del ciclo de facturación, lo que se produzca primero.  La facturación por horas es una buena opción para el almacenamiento que se utiliza unos pocos días o menos de un mes completo. La facturación por horas solo está disponible para el almacenamiento suministrado en estos [centros de datos seleccionados](new-ibm-block-and-file-storage-location-and-features.html). 
     2. Con la **facturación mensual**, el cálculo del precio se prorratea desde la fecha de creación hasta la finalización del ciclo de facturación y se factura al momento. No se reembolsará si un volumen de archivos se suprime antes de finalizar el ciclo de facturación.  La facturación mensual es una buena opción para el almacenamiento utilizado en cargas de trabajo de producción que utilizan datos que tienen que almacenarse, y por tanto acceder a ellos, durante largo periodos de tiempo (un mes o más).
     **NOTA**: el tipo de facturación mensual se utiliza de forma predeterminada para el almacenamiento suministrado en centros de datos que no están actualizados con prestaciones mejoradas.
5. Seleccione el nivel de almacenamiento para sus aplicaciones:
    - **0,25 IOPS por GB** está diseñado para cargas de trabajo con intensidad baja de E/S. Estas cargas de trabajo se suelen caracterizar por tener un porcentaje elevado de datos inactivos en un momento determinado. Aplicaciones de ejemplo incluyen el almacenamiento de buzones o el uso compartido de archivos a nivel departamental.
    - **2 IOPS por GB** está diseñado para usos de finalidad más general. Entre las aplicaciones de ejemplo, se incluye el alojamiento de bases de datos pequeñas que respaldan aplicaciones web o imágenes de disco de máquinas virtuales para un hipervisor.
    - **4 IOPS por GB** está diseñado para cargas de trabajo de mayor intensidad. Estas cargas de trabajo se suelen caracterizar por tener un porcentaje alto de datos activos en un momento determinado. Entre las aplicaciones de ejemplo, se incluyen las bases de datos transaccionales y otras bases de datos que dependen del rendimiento.
    - **10 IOPS por GB** está diseñado para las cargas de trabajo más exigentes, como las creadas por bases de datos NoSQL, y el procesamiento de datos para Analytics.  Este nivel está disponible en [centros de datos seleccionados](new-ibm-block-and-file-storage-location-and-features.html) para el almacenamiento suministrado de hasta 4 TB de tamaño.
6. Seleccione el **Tamaño de almacenamiento utilizable** en la lista desplegable.
7. Elija el **Tamaño del espacio de instantáneas** (además del espacio utilizable) en la lista desplegable.
8. Pulse **Continuar**. Se muestran los cargos mensuales y prorrateados, es una última oportunidad para revisar los detalles del pedido.
9. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro** y pulse **Realizar pedido**.
10. La nueva asignación de almacenamiento debería estar disponible en unos minutos.

**Nota**: de forma predeterminada, puede suministrar un total combinado de 250 volúmenes de {{site.data.keyword.blockstorageshort}}. Para aumentar el número de volúmenes, póngase en contacto con su representante de ventas. [Aquí](managing-storage-limits.html) puede leer más información sobre cómo aumentar los límites.

Para información sobre el límite en autorizaciones simultáneas, consulte nuestras [Preguntas más frecuentes](File-Storage-FAQ.html)

### Cómo solicitar Rendimiento para {{site.data.keyword.filestorage_short}}

1. En el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, pulse **Almacenamiento**, **{{site.data.keyword.filestorage_short}}** O desde el catálogo de {{site.data.keyword.BluSoftlayer_full}}, pulse **Infraestructura** >** Almacenamiento** > **{{site.data.keyword.filestorage_short}}**.
2. Pulse **Realizar el pedido de {{site.data.keyword.filestorage_short}}** en la esquina superior derecha de la pantalla. 
3. Seleccione **Rendimiento** en la lista desplegable Seleccionar tipo de almacenamiento.
4. Pulse la lista desplegable **Ubicación** y seleccione el centro de datos.
    -  Asegúrese de que el nuevo almacenamiento se añada en la misma ubicación que el host pedido anteriormente.
    -  Si ha seleccionado un centro de datos con prestaciones mejoradas (marcados con un * en el desplegable), tendrá la opción de elegir entre facturación mensual o por horas. 
       1.  Con la **facturación por horas**, el cálculo del número de horas que el volumen de archivos ha existido en la cuenta se realiza en el momento en que se suprime el volumen o al final del ciclo de facturación, lo que se produzca primero.  La facturación por horas es una buena opción para el almacenamiento que se utiliza unos pocos días o menos de un mes completo. La facturación por horas solo está disponible para el almacenamiento suministrado en estos centros de datos seleccionados. 
       2. Con la **facturación mensual**, el cálculo del precio se prorratea desde la fecha de creación hasta la finalización del ciclo de facturación y se factura al momento. No se reembolsará si un volumen de archivos se suprime antes de finalizar el ciclo de facturación.  La facturación mensual es una buena opción para el almacenamiento utilizado en cargas de trabajo de producción que utilizan datos que tienen que almacenarse, y por tanto acceder a ellos, durante largo periodos de tiempo (un mes o más).
       **NOTA**: el tipo de facturación mensual se utiliza de forma predeterminada para el almacenamiento suministrado en centros de datos que no están actualizados con prestaciones mejoradas.  
5. Marque el botón de selección situado junto al **Tamaño de almacenamiento** adecuado.
6. Especifique las IOPS en el campo **Especificar IOPS**.
7. Pulse **Continuar**. Se muestran los cargos mensuales y prorrateados, es una última oportunidad para revisar los detalles del pedido. Pulse **Anterior** si desea cambiar el pedido.
8. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro** y pulse **Realizar pedido**.
9. La nueva asignación de almacenamiento debería estar disponible en unos minutos.

**Nota**: de forma predeterminada, puede suministrar un total combinado de 250 volúmenes de {{site.data.keyword.blockstorageshort}}. Para aumentar el número de volúmenes, póngase en contacto con su representante de ventas. [Aquí](managing-storage-limits.html) puede leer más información sobre cómo aumentar los límites.

Para información sobre el límite en autorizaciones simultáneas, consulte nuestras [Preguntas más frecuentes](File-Storage-FAQ.html)

## Gestión de {{site.data.keyword.filestorage_short}}

### Cómo autorizar hosts para acceder a {{site.data.keyword.filestorage_short}}

Los hosts “autorizados” son los hosts a los que se les ha otorgado derechos de acceso a un volumen determinado. Sin la autorización del host, no podrá acceder ni utilizar el almacenamiento de su sistema. Autorizar a un host el acceso al volumen genera un nombre de usuario y contraseña. 

**Nota**: solo puede autorizar y conectar hosts que residan en el mismo centro de datos que su almacenamiento. Si tiene varias cuentas, puede autorizar a un host de una cuenta para acceder a su almacenamiento en otra. 

1. Pulse **Almacenamiento** > **{{site.data.keyword.filestorage_short}}**, y pulse el **Nombre de volumen**.
2. Desplácese a la sección **Hosts autorizados** de la página.
3. Pulse el enlace **Autorizar host** de la parte derecha de la página. Seleccione los hosts que pueden acceder a ese volumen determinado.

 

### Cómo visualizar la lista de hosts autorizados a un volumen de {{site.data.keyword.filestorage_short}}

Efectúe los pasos siguientes para visualizar la lista de hosts autorizados a un volumen.

1. Pulse **Almacenamiento > {{site.data.keyword.filestorage_short}}**, y pulse el **Nombre de volumen**.
2. Desplácese a la parte inferior de la página en la sección **Hosts autorizados**.

Aquí verá la lista de hosts que actualmente tienen autorización para acceder al volumen.


### Cómo visualizar los volúmenes de {{site.data.keyword.filestorage_short}} a los cuales un host está autorizado

Puede ver los volúmenes a los cuales un host tiene acceso, incluyendo la información necesaria para realizar una conexión: nombre de volumen, tipo de almacenamiento, dirección de destino, capacidad y ubicación:

1. Pulse **Dispositivos** > **Lista de dispositivos** desde el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} y pulse sobre el dispositivo adecuado.
2. Seleccione el separador Almacenamiento.

Se le presentará una lista de los volúmenes de almacenamiento a los cuales este host tiene acceso, todos agrupados por tipo de almacenamiento (bloque, archivo, otros). Desde los menús respectivos de **Acción**, puede autorizar almacenamiento adicional o eliminar el acceso.

 

### Cómo montar y desmontar {{site.data.keyword.filestorage_short}}

Puede utilizar la información de punto de montaje proporcionada en la vista **Detalles del volumen** para montar {{site.data.keyword.filestorage_short}} desde un host.

Consulte el siguiente artículo con detalles sobre cómo montar y desmontar {{site.data.keyword.filestorage_short}} desde un host: [Acceso a {{site.data.keyword.filestorage_short}} en Linux](accessing-file-storage-linux.html)

 

### Cómo revocar el acceso de un host a {{site.data.keyword.filestorage_short}}

Si quiere detener el acceso desde un host a un volumen de almacenamiento determinado, puede revocar el acceso. Al revocar el acceso, la conexión de host se descartará del volumen, y ni el sistema operativo ni las aplicaciones podrán comunicarse con el volumen. 

**Nota:** para impedir problemas del lado del host, desmonte el volumen de almacenamiento desde el sistema operativo antes de revocar el acceso para evitar perder unidades o corrupción de datos.

Puede revocar el acceso desde el Almacenamiento de la Lista de Dispositivos o desde las vistas de almacenamiento.

#### Cómo revocar el acceso desde la lista de dispositivos:

1. Pulse **Dispositivos** > **Lista de dispositivos** desde el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} y efectúe una doble pulsación sobre el dispositivo adecuado.
2. Seleccione el separador **Almacenamiento**.
3. Se le presentará una lista de los volúmenes de almacenamiento a los cuales este host tiene acceso, todos agrupados por tipo de almacenamiento (bloque, archivo, otros). Seleccione el menú respectivo de **Acción** situado junto al volumen del que desea revocar el acceso y pulse **Revocar acceso**.
4. Se le solicitará si desea revocar el acceso al volumen porque la acción no puede deshacerse. Pulse **Yes** para revocar el acceso al volumen o **No** para cancelar la acción.

**Nota:** si desea desconectar varios volúmenes desde un host específico, deberá repetir la acción Revocar acceso para cada volumen.

 

#### Cómo revocar el acceso desde la vista de almacenamiento:
1. Pulse **Almacenamiento, {{site.data.keyword.filestorage_short}}**, y seleccione el **Volumen** desde el cual desea revocar el acceso.
2. Desplácese a la sección **Hosts autorizados** de la página.
3. Pulse la flecha desplegable **Acciones** situada junto al host cuyo acceso se va a revocar, y seleccione **Revocar acceso**.
4. Se le solicitará si desea revocar el acceso al volumen porque la acción no puede deshacerse. Pulse **Yes** para revocar el acceso al volumen o **No** para cancelar la acción.

**Nota:** si desea desconectar varios hosts desde un host específico, deberá repetir la acción Revocar acceso para cada host.

 

### Cómo cancelar un volumen de almacenamiento

Si ya no necesita un volumen específico, puede cancelarlo. Para cancelar un volumen de almacenamiento, primero es necesario revocar el acceso desde cualquier host.

1. Pulse **Almacenamiento**>**{{site.data.keyword.filestorage_short}}**.
2. Pulse la flecha desplegable **Acciones** del volumen que se va a cancelar y seleccione **Cancelar {{site.data.keyword.filestorage_short}}**.
3. Se le solicitará que confirme si desea cancelar el volumen inmediatamente o en la fecha de aniversario en la que el volumen se suministró. Pulse **Continuar** o **Cerrar**. 
**Nota**: si selecciona la opción de cancelar el volumen en su fecha de aniversario, puede anular la solicitud de cancelación antes de su fecha de aniversario.
4. Marque el recuadro de selección de acuse de recibo y pulse **Confirmar**.

 

### Cómo consultar los detalles de un volumen de {{site.data.keyword.filestorage_short}} suministrado

Puede ver un resumen de la información clave para el volumen de almacenamiento seleccionado, incluyendo las prestaciones adicionales de instantánea y réplica que se han añadido al almacenamiento.

1. Pulse **Almacenamiento**>**{{site.data.keyword.filestorage_short}}**.
2. Pulse el **Nombre de volumen** adecuado en la lista.

### Cómo identificar mis volúmenes de {{site.data.keyword.filestorage_short}} en la factura

Todos los volúmenes aparecerán en la factura como un elemento de línea; Resistencia aparecerá como "Servicio de Almacenamiento de Resistencia" y Rendimiento aparecerá como "servicio de Almacenamiento de Rendimiento". La tasa variará en función de su nivel de almacenamiento. Puede ampliar el Rendimiento o la Resistencia para ver que es un volumen de {{site.data.keyword.filestorage_short}}.
