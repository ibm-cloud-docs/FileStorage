---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Ajuste de IOPS
{: #adjustingIOPS}

Con esta nueva característica, los usuarios de almacenamiento {{site.data.keyword.filestorage_full}} podrán ajustar IOPS de su {{site.data.keyword.filestorage_short}} existente de inmediato. No tienen que crear un duplicado ni copiar los datos manualmente en el almacenamiento nuevo. Los usuarios no experimentan ningún tipo de parada ni falta de acceso al almacenamiento mientras se realiza el ajuste.

La facturación del almacenamiento se actualiza para añadir la diferencia prorrateada del nuevo precio en el ciclo de facturación actual. El nuevo importe completo se factura en el siguiente ciclo de facturación.


## Ventajas de IOPS ajustables

- Gestión de costes – Algunos de nuestros clientes pueden necesitar IOPS altos solo durante picos de uso. Por ejemplo, un almacén de una tienda de gran tamaño tiene picos de uso durante las vacaciones y podría necesitar más IOPS en el almacenamiento en ese momento que a mediados de verano. Esta característica les permite gestionar sus costes y pagar más IOPS cuando lo necesitan.

## Limitaciones

Esta característica solo está disponible en [centros de datos seleccionados](/docs/infrastructure/BlockStorage?topic=BlockStorage-news).

Los clientes no pueden cambiar entre Resistencia y Rendimiento al ajustar su IOPS. Los usuarios pueden especificar un nuevo nivel de IOPS para su almacenamiento en función de los siguientes criterios y restricciones.

- Si el volumen original es Resistencia nivel 0,25, el nivel de IOPS no se puede actualizar.
- Si el volumen original es Rendimiento con menos que o igual a 0,30 IOPS/GB, las opciones disponibles solo incluyen el tamaño y las combinaciones de IOPS que resulten en menos que o igual a 0,30 IOPS/GB.
- Si el volumen original es Rendimiento con más de 0,30 IOPS/GB, las opciones disponibles solo incluyen el tamaño y combinaciones de IOPS que resulten en más de 0,30 IOPS/GB.

## Efecto del ajuste de IOPS en la réplica

Si el volumen tiene réplica, la réplica se actualiza automáticamente para coincidir con la selección de IOPS de la primaria.

## Ajuste de IOPS en el almacenamiento
{: #steps}

1. Vaya a su lista de {{site.data.keyword.filestorage_short}}
    - Desde el portal de clientes, pulse **Almacenamiento** > **{{site.data.keyword.filestorage_short}}** O
    - Desde la consola de {{site.data.keyword.BluSoftlayer_full}}, pulse **Infraestructura** > **Almacenamiento** > **{{site.data.keyword.filestorage_short}}**.
2. Seleccione el volumen de la lista y pulse **Acciones** > **Modificar volumen**
3. En las **Opciones IOPS de almacenamiento**, realice una nueva selección:
    - En Resistencia (IOPS por niveles), seleccione un nivel de IOPS superior a 0,25 IOPS/GB de su almacenamiento. Puede aumentar el nivel de IOPS en cualquier momento. Sin embargo, la disminución solo está disponible una vez al mes.
    - En Rendimiento (IOPS asignado), especifique la nueva opción de IOPS para su almacenamiento especificando un valor comprendido entre 100 y 48.000 IOPS. (Asegúrese de comprobar los límites específicos requeridos por tamaño en el formulario de pedido).
4. Revise su selección y el nuevo precio.
5. Marque el recuadro de selección **He leído el Acuerdo de servicio maestro...** y pulse **Realizar pedido**.
6. La nueva asignación de almacenamiento estará disponible en pocos minutos.

De manera alternativa, puede actualizar el IOPS a través de la SLCLI.
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
