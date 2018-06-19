---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-18"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Realizar pedidos de instantáneas

Para crear instantáneas de su volumen de almacenamiento, automática o manualmente, necesita adquirir espacio para mantenerlas. Puede adquirir capacidad hasta la cantidad de su volumen de almacenamiento (durante la adquisición del volumen inicial o posteriormente siguiendo estos pasos).

1. Acceda a su Almacenamiento mediante el separador **Almacenamiento** > **{{site.data.keyword.filestorage_short}}** del {{site.data.keyword.slportal}}{:new_window} de [
2. En el marco Instantáneas, pulse Añadir espacio de instantáneas.
3. En el marco Instantáneas, pulse Adquirir ahora espacio de instantáneas.
3. Seleccione la cantidad de espacio que necesita.
4. Pulse Continuar.
5. Especifique cualquier código promocional que tenga y pulse Recalcular. Las opciones Cargos para este pedido y Revisión del pedido contienen valores predeterminados.
6. Marque el recuadro de selección He leído el Acuerdo de Servicio Maestro… y pulse Realizar pedido. El espacio de instantáneas se suministrará en unos minutos.

## Cómo determinar cuánto espacio de instantáneas pedir

Por desgracia, no existe una mejor práctica, sino más bien una recomendación basada en la aplicación. Como norma general, el espacio de instantáneas que utilizan las instantáneas se basa en dos asuntos clave:
- cuántos cambios tiene su sistema de archivos activo, y 
- cuánto tiempo tiene previsto retener las instantáneas.

Básicamente, la forma de calcular la cantidad de espacio necesario es (Tasa de Cambio) x (número de horas/días/semanas/meses de retención).
Nota: la primera instantánea utiliza una cantidad insignificante de espacio porque solo es una copia de los metadatos (punteros) que indica los bloques del sistema de archivos activo. 

Un volumen con mucho cambio de datos (por ejemplo, una base de datos con una tasa de cambio alta) y un periodo largo de retención de instantáneas va a necesitar más espacio para las instantáneas que un volumen con una cantidad moderada de cambios (por ejemplo, un almacén de datos de máquina virtual) y una planificación de retención de instantáneas más moderada. 

Si tuviera que tomar 12 instantáneas por hora de un volumen que contuviera 500 GB de datos reales y observara un 1 por ciento de cambio entre cada instantánea, terminaría utilizando 60 GB para instantáneas.

(Tasa de cambio de 5 GB) x (12 instantáneas por hora) = 60 GB

Por el contrario, si estos 500 GB de datos reales, con 12 instantáneas por hora, tuvieran un 10 por ciento de cambios cada hora, terminaría utilizando 600 GB.

(Tasa de cambio de 50 GB) x (12 instantáneas por hora) = 600 GB

Por tanto, a la hora de determinar cuánto espacio de instantáneas necesita, preste atención a la tasa de cambio. Influye mucho sobre el espacio de instantáneas necesario.  Mientras que el tamaño de un volumen probablemente significará una cantidad mayor de cambio, un volumen de 500 GB con 5 GB de tasa de cambio y un volumen de 10 TB con 5 GB de tasa de cambio requerirán el mismo espacio de instantáneas.

Además, para la mayoría de las cargas de trabajo, cuanto mayor sea un volumen, menor será el espacio inicial necesario para instantáneas.  Esto se debe principalmente a la eficiencia de los datos subyacentes de nuestra plataforma y a la naturaleza del proceso de instantáneas en nuestro entorno.


