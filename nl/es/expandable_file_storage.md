---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-29"

---
{:new_window: target="_blank"}

# Ampliación de la capacidad de compartición de archivos

Con esta nueva característica, los usuarios actuales de {{site.data.keyword.filestorage_full}} pueden ampliar el tamaño de su {{site.data.keyword.filestorage_short}} en incrementos de GB de hasta 12 TB inmediatamente. No tienen que crear un duplicado ni migrar manualmente los datos a un volumen más grande. No se producen paradas ni falta de acceso al almacenamiento mientras se realiza el redimensionamiento. 

La facturación del volumen se actualizará automáticamente para añadir la diferencia prorrateada del nuevo precio en el ciclo de facturación actual. Entonces, el nuevo importe completo se factura en el siguiente ciclo de facturación.

Esta característica solo está disponible en [centros de datos seleccionados](new-ibm-block-and-file-storage-location-and-features.html). 

## Ventajas de almacenamiento expandible

- **Gestión de costes**: Ya es consciente del potencial de crecimiento de sus datos, pero además necesita una menor cantidad de almacenamiento para empezar. La capacidad de ampliar permite a los clientes ahorrar en costes de almacenamiento al inicio e ir creciendo en función de sus necesidades.  

- **Necesidades de almacenamiento crecientes** - Los clientes con un rápido crecimiento, por encima de sus necesidades, necesitan un modo rápido y sencillo de incrementar su almacenamiento para gestionar este crecimiento.

## Efectos de ampliar la capacidad de almacenamiento en la Réplica

La acción de ampliar el almacenamiento primario genera un redimensionamiento automático de la réplica.

## Limitaciones

Esta característica solo está disponible para el almacenamiento que se suministra en [centros de datos](new-ibm-block-and-file-storage-location-and-features.html) con funciones ampliadas. El almacenamiento cifrado suministrado en estos centros de datos se puede incrementar hasta 12 TB. 

Las limitaciones de tamaño actuales para {{site.data.keyword.filestorage_short}} suministrado con Resistencia aún se aplican (hasta 4 TB para IOPS de nivel 10 y hasta 12 TB para los demás niveles).

## Identificación del almacenamiento elegible

El almacenamiento suministrado con funciones mejoradas siempre incluye cifrado en reposo. Puede saber fácilmente si es aplicable a su almacenamiento si tiene un icono de bloqueo en la interfaz de usuario del portal. 

## Redimensionamiento de almacenamiento

1. En el {{site.data.keyword.slportal}}, pulse **Almacenamiento** > **{{site.data.keyword.filestorage_short}}** O desde el catálogo de {{site.data.keyword.BluSoftlayer_full}}, pulse **Infraestructura** > **Almacenamiento** > **{{site.data.keyword.filestorage_short}}**.
2. Seleccione el volumen de la lista y pulse **Acciones** > **Modificar volumen**
3. Especifique el nuevo tamaño de almacenamiento en GB.
4. Revise su selección y el nuevo precio.
5. Pulse **He leído el Acuerdo de Servicio Maestro...** y pulse **Realizar pedido**.
6. La nueva asignación de almacenamiento está disponible en pocos minutos.
