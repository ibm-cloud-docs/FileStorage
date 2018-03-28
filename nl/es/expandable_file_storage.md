---

copyright:
  years: 2014, 2017
lastupdated: "2017-12-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Capacidad de compartición de archivos ampliable

Con esta nueva característica, nuestros usuarios de {{site.data.keyword.filestorage_full}} actuales podrán ampliar el tamaño de su {{site.data.keyword.filestorage_short}} existente en incrementos de GB hasta 12 TB sobre la marcha, sin necesidad de crear un duplicado o migrar datos manualmente a un volumen más grande.  No se producen paradas ni falta de acceso al almacenamiento mientras se realiza el redimensionamiento. 

La facturación del volumen se actualizará automáticamente para añadir la diferencia prorrateada del nuevo precio en el ciclo de facturación actual y posteriormente se facturará el nuevo importe total en el próximo ciclo de facturación.

Esta característica solo está disponible en [centros de datos seleccionados](new-ibm-block-and-file-storage-location-and-features.html). 

## ¿Por qué utilizar los recursos compartidos de archivos ampliables?

- **Gestión de costes** – Es consciente del potencial de crecimiento de sus datos, pero necesitara una cantidad de almacenamiento inferior para empezar. La capacidad de ampliar permite a nuestros clientes ahorrar en costes de almacenamiento al inicio e ir creciendo en función de sus necesidades.  

- **Necesidades de almacenamiento crecientes** - Los clientes con un rápido crecimiento, por encima de sus necesidades, necesitan un modo rápido y sencillo de incrementar su almacenamiento para gestionar este crecimiento.

## ¿Cómo afecta el redimensionamiento del almacenamiento a la réplica?

La acción de ampliar el almacenamiento primario generará un redimensionamiento automático de la réplica.

## ¿Existe alguna limitación?

Esta característica solo está disponible para el almacenamiento que se suministra en [centros de datos](new-ibm-block-and-file-storage-location-and-features.html) con funciones ampliadas. El almacenamiento cifrado suministrado en estos centros de datos se puede incrementar hasta 12 TB. 

Las limitaciones de tamaño actuales para {{site.data.keyword.filestorage_short}} suministrado con Resistencia aún se aplican (hasta 4 TB para IOPS de nivel 10 y hasta 12 TB para los demás niveles).

## ¿Cómo puedo saber si mi almacenamiento suministrado es ampliable?

El almacenamiento suministrado con funciones mejoradas siempre incluye cifrado en descanso.  Puede saber fácilmente si es aplicable a su almacenamiento si tiene un icono de bloqueo en la interfaz de usuario del portal. 

## ¿Cómo puedo redimensionar mi almacenamiento?

1. Desde el {{site.data.keyword.slportal}}, pulse **Almacenamiento** > **{{site.data.keyword.filestorage_short}}** O desde el catálogo de {{site.data.keyword.BluSoftlayer_full}}, pulse **Infraestructura** > **Almacenamiento** > **{{site.data.keyword.filestorage_short}}**.
2. Seleccione el volumen de la lista y pulse **Acciones** > **Modificar volumen**
3. Especifique el nuevo tamaño de almacenamiento en GB.
4. Revise su selección y el nuevo precio.
5. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro...** y pulse **Realizar pedido**.
6. La nueva asignación de almacenamiento debería estar disponible en unos minutos.

