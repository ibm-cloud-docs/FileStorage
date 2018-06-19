---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Gestión de {{site.data.keyword.filestorage_short}}

Puede gestionar los volúmenes de {{site.data.keyword.filestorage_full}} mediante {{site.data.keyword.slportal}}. Este artículo proporciona instrucciones para las tareas más comunes.

## Cómo autorizar hosts para acceder a {{site.data.keyword.filestorage_short}}

Los hosts “autorizados” son los hosts a los que se les ha otorgado derechos de acceso a un volumen determinado. Sin la autorización del host, no podrá acceder ni utilizar el almacenamiento de su sistema. El hecho de autorizar a un host el acceso al volumen genera un nombre de usuario y una contraseña. 

**Nota**: solo puede autorizar y conectar hosts que residan en el mismo centro de datos que su almacenamiento. Si tiene varias cuentas, no puede autorizar a un host de una cuenta a que acceda a su almacenamiento en otra. 

1. Pulse **Almacenamiento** > **{{site.data.keyword.filestorage_short}}** y pulse el **Nombre de volumen**.
2. Desplácese a la sección **Hosts autorizados** de la página.
3. Pulse **Autorizar host** en la parte derecha de la página. Seleccione los hosts que pueden acceder a ese volumen determinado.

 

## Cómo visualizar la lista de hosts autorizados a un volumen de {{site.data.keyword.filestorage_short}}

Efectúe los pasos siguientes para visualizar la lista de hosts autorizados a un volumen.

1. Pulse **Almacenamiento > {{site.data.keyword.filestorage_short}}** y pulse el **Nombre de volumen**.
2. Desplácese a la parte inferior de la página en la sección **Hosts autorizados**.

Aquí verá la lista de hosts que actualmente tienen autorización para acceder al volumen.


## Cómo visualizar los volúmenes de {{site.data.keyword.filestorage_short}} a los cuales un host está autorizado

Puede ver los volúmenes a los cuales un host tiene acceso, incluyendo la información necesaria para realizar una conexión: nombre de volumen, tipo de almacenamiento, dirección de destino, capacidad y ubicación:

1. Pulse **Dispositivos** > **Lista de dispositivos** desde el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} y pulse sobre el dispositivo adecuado.
2. Seleccione el separador Almacenamiento.

Se le presentará una lista de los volúmenes de almacenamiento a los cuales este host tiene acceso, todos agrupados por tipo de almacenamiento (bloque, archivo, otros). Desde los menús respectivos de **Acción**, puede autorizar almacenamiento adicional o eliminar el acceso.

 

## Cómo montar y desmontar {{site.data.keyword.filestorage_short}}

Puede utilizar la información de punto de montaje proporcionada en la vista **Detalles del volumen** para montar {{site.data.keyword.filestorage_short}} desde un host.

Consulte el siguiente artículo con detalles sobre cómo montar y desmontar {{site.data.keyword.filestorage_short}} desde un host: [Acceso a {{site.data.keyword.filestorage_short}} en Linux](accessing-file-storage-linux.html)

 

## Cómo revocar el acceso de un host a {{site.data.keyword.filestorage_short}}

Si quiere detener el acceso desde un host a un volumen de almacenamiento determinado, puede revocar el acceso. Al revocar el acceso, la conexión de host se descartará del volumen, y ni el sistema operativo ni las aplicaciones podrán comunicarse con el volumen. 

**Nota:** para impedir problemas del lado del host, desmonte el volumen de almacenamiento desde el sistema operativo antes de revocar el acceso para evitar perder unidades o corrupción de datos.

Puede revocar el acceso desde el Almacenamiento de la Lista de Dispositivos o desde las vistas de almacenamiento.

### Cómo revocar el acceso desde la lista de dispositivos:

1. Pulse **Dispositivos** > **Lista de dispositivos** desde el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} y efectúe una doble pulsación sobre el dispositivo adecuado.
2. Seleccione el separador **Almacenamiento**.
3. Se le presenta una lista de los volúmenes de almacenamiento a los cuales este host tiene acceso, todos agrupados por tipo de almacenamiento (bloque, archivo, otros). Seleccione el menú respectivo de **Acción** situado junto al volumen del que desea revocar el acceso y pulse **Revocar acceso**.
4. Confirme si desea revocar el acceso al volumen, porque la acción no puede deshacerse. Pulse **Yes** para revocar el acceso al volumen o **No** para cancelar la acción.

**Nota:** si desea desconectar varios volúmenes desde un host específico, deberá repetir la acción Revocar acceso para cada volumen.

 

### Cómo revocar el acceso desde la vista de almacenamiento:
1. Pulse **Almacenamiento, {{site.data.keyword.filestorage_short}}**, y seleccione el **Volumen** desde el cual desea revocar el acceso.
2. Desplácese a la sección **Hosts autorizados** de la página.
3. Pulse **Acciones** junto al host cuyo acceso se va a revocar y seleccione **Revocar acceso**.
4. Confirme si desea revocar el acceso al volumen, porque la acción no puede deshacerse. Pulse **Yes** para revocar el acceso al volumen o **No** para cancelar la acción.

**Nota:** Si desea desconectar varios hosts desde un host específico, debe repetir la acción Revocar acceso para cada host.

 

## Cómo cancelar un volumen de almacenamiento

Si ya no necesita un volumen específico, puede cancelarlo. Para cancelar un volumen de almacenamiento, primero es necesario revocar el acceso desde cualquier host.

1. Pulse **Almacenamiento**>**{{site.data.keyword.filestorage_short}}**.
2. Pulse **Acciones** del volumen que se va a cancelar y seleccione **Cancelar {{site.data.keyword.filestorage_short}}**.
3. Confirme si desea cancelar el volumen inmediatamente o en la fecha de aniversario en la que se suministró el volumen. Pulse **Continuar** o **Cerrar**. 
**Nota**: si selecciona la opción de cancelar el volumen en su fecha de aniversario, puede anular la solicitud de cancelación antes de su fecha de aniversario.
4. Marque el recuadro de selección de acuse de recibo y luego **Confirmar**.

 

## Cómo consultar los detalles de un volumen de {{site.data.keyword.filestorage_short}} suministrado

Puede ver un resumen de la información clave del volumen de almacenamiento seleccionado, incluidas las prestaciones adicionales de instantánea y réplica que se han añadido al almacenamiento.

1. Pulse **Almacenamiento**>**{{site.data.keyword.filestorage_short}}**.
2. Pulse el **Nombre de volumen** adecuado en la lista.
