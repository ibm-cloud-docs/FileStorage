---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Ampliación de la capacidad de compartición de archivos
{: #expandCapacity}

Con esta nueva característica, los usuarios actuales de {{site.data.keyword.filestorage_full}} pueden ampliar el tamaño de su {{site.data.keyword.filestorage_short}} en incrementos de GB de hasta 12 TB inmediatamente. No tienen que crear un duplicado ni migrar manualmente los datos a un volumen más grande. No se producen paradas ni falta de acceso al almacenamiento mientras se realiza el redimensionamiento.

La facturación del volumen se actualizará automáticamente para añadir la diferencia prorrateada del nuevo precio en el ciclo de facturación actual. Entonces, el nuevo importe completo se factura en el siguiente ciclo de facturación.

Esta característica solo está disponible en [centros de datos seleccionados](/docs/infrastructure/FileStorage?topic=FileStorage-news).

## Ventajas de almacenamiento expandible

- **Gestión de costes**: Ya es consciente del potencial de crecimiento de sus datos, pero además necesita una menor cantidad de almacenamiento para empezar. La capacidad de ampliar permite a los clientes ahorrar en costes de almacenamiento al inicio e ir creciendo en función de sus necesidades.  

- **Necesidades de almacenamiento crecientes** - Los clientes con un rápido crecimiento, por encima de sus necesidades, necesitan un modo rápido y sencillo de incrementar su almacenamiento para gestionar este crecimiento.

## Efectos de ampliar la capacidad de almacenamiento en la Réplica

La acción de ampliar el almacenamiento primario genera un redimensionamiento automático de la réplica.

## Limitaciones
{: #limitsofextension}

Esta característica solo está disponible para el almacenamiento que se suministra en [centros de datos](/docs/infrastructure/FileStorage?topic=FileStorage-news) con funciones ampliadas. El almacenamiento cifrado suministrado en estos centros de datos se puede incrementar hasta 12 TB.

Las limitaciones de tamaño actuales para {{site.data.keyword.filestorage_short}} suministrado con Resistencia aún se aplican (hasta 4 TB para IOPS de nivel 10 y hasta 12 TB para los demás niveles).

## Redimensionamiento de almacenamiento
{: #resizingsteps}

1. En el {{site.data.keyword.slportal}}, pulse **Almacenamiento** > **{{site.data.keyword.filestorage_short}}** O desde el catálogo de {{site.data.keyword.BluSoftlayer_full}}, pulse **Infraestructura** > **Almacenamiento** > **{{site.data.keyword.filestorage_short}}**.
2. Seleccione el volumen de la lista y pulse **Acciones** > **Modificar volumen**
3. Especifique el nuevo tamaño de almacenamiento en GB.
4. Revise su selección y el nuevo precio.
5. Pulse **He leído el Acuerdo de servicio maestro...** y pulse **Realizar pedido**.
6. La nueva asignación de almacenamiento está disponible en pocos minutos.

De manera alternativa, puede utilizar el mandato siguiente en SLCLI.
```
# slcli file volume-modify --help
Uso: slcli file volume-modify [OPCIONES] ID_VOLUMEN

Opciones:
  -c, --new-size ENTERO         Nuevo tamaño del volumen de archivos en GB. ***Si no se
                                especifica tamaño, se usará el tamaño original
                                del volumen.***
                                Tamaños potenciales: [20, 40, 80, 100,
                                250, 500, 1000, 2000, 4000, 8000, 12000]
                                Mínimo: [el tamaño original del volumen]
  -i, --new-iops ENTERO         IOPS de almacenamiento de rendimiento, entre 100
                                y 6000 en múltiplos de 100 [sólo para volúmenes de
                                rendimiento] ***Si no se especifica valor de IOPS,
                                se usará el valor de IOPS original del volumen.***
                                Requisitos: [Si el IOPS/GB del volumen es menor
                                que 0,3, el nuevo IOPS/GB debe ser también menor que
                                0,3. Si el IOPS/GB original del volumen es mayor o
                                igual que 0,3 el nuevo IOPS/GB del volumen debe ser
                                también mayor o igual que 0,3.]
  -t, --new-tier [0.25|2|4|10]  Nivel de almacenamiento resistente (IOPS por GB)
                                [sólo para volúmenes de resistencia] ***Si no se
                                especifica nivel, se usará el nivel original del
                                volumen.***
                                Requisitos: [Si el IOPS/GB original del volumen es
                                0,25, el nuevo IOPS/GB del volumen debe ser
                                también 0,25. Si el IOPS/GB original del volumen es
                                mayor que 0,25 el nuevo IOPS/GB del volumen debe ser
                                también mayor que 0,25.]
  -h, --help      Mostrar este mensaje y salir.
```
