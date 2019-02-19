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
{:faq: data-hd-content-type='faq'}

# Preguntas más frecuentes
{: #faqs}

## ¿Cómo puedo saber cuáles de mis volúmenes de {{site.data.keyword.filestorage_short}} están cifrados?
{: faq}

Examine su lista de {{site.data.keyword.filestorage_short}} en el portal de clientes. Verá un icono de bloqueo a la derecha del nombre de volumen para los volúmenes que están cifrados.

## Si he adquirido {{site.data.keyword.filestorage_short}} no cifrado en un centro de datos que se ha actualizado para admitir cifrado, ¿puedo cifrar mi {{site.data.keyword.filestorage_short}}?
{: faq}

{{site.data.keyword.filestorage_short}} suministrado antes de una actualización del centro de datos no se puede cifrar. El nuevo {{site.data.keyword.filestorage_short}} que se suministra en centros de datos actualizados se cifra automáticamente. Es una característica automática, no un valor de suministro que se puede seleccionar o descartar. Para cifrar los datos que residen en almacenamiento no cifrado, hay que crear un nuevo volumen y luego copiar los datos en el nuevo volumen cifrado con una migración basada en host. Para obtener más información, consulte [Migración de File Storage](/docs/infrastructure/FileStorage?topic=FileStorage-migratestorage).

## ¿Cómo sé si estoy suministrando {{site.data.keyword.filestorage_short}} en un centro de datos actualizado?
{: faq}

En el formulario de solicitud de {{site.data.keyword.filestorage_short}}, todos los centros de datos actualizados están indicados con un asterisco (`*`). Durante el proceso de solicitud se le indicará que va a suministrar almacenamiento con cifrado. Una vez suministrado el almacenamiento, puede ver un icono en la lista de almacenamiento que muestra que dicho volumen está cifrado.

Todos los volúmenes y comparticiones de archivos cifrados se suministran únicamente en centros de datos actualizados. Puede consultar una lista completa de centros de datos actualizados y características disponibles [aquí](/docs/infrastructure/FileStorage?topic=FileStorage-news).

## ¿Por qué {{site.data.keyword.filestorage_short}} con un nivel 10 de IOPS de Resistencia se puede suministrar en algunos centros de datos y en otros no?
{: faq}

{{site.data.keyword.filestorage_short}} de tipo Resistencia con nivel 10 IOPS/GB solo está disponible en determinados centros de datos, y pronto se añadirán nuevos centros de datos. Puede consultar una lista completa de centros de datos actualizados y características disponibles [aquí](/docs/infrastructure/FileStorage?topic=FileStorage-news).

## ¿Cómo puedo encontrar el punto de montaje correcto para mi {{site.data.keyword.filestorage_short}}?
{: faq}

Todos los volúmenes de {{site.data.keyword.filestorage_short}} cifrados suministrados en los centros de datos mejorados tienen un punto de montaje distinto que los volúmenes no cifrados. Para asegurarse de que está utilizando el punto de montaje correcto, consulte la información sobre punto de montaje en la página **Detalles del volumen** de la interfaz de usuario. También puede acceder al punto de montaje correcto mediante una llamada de API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

## ¿Cuántos volúmenes puedo suministrar?
{: faq}

De forma predeterminada, puede suministrar un total combinado de 250 volúmenes de almacenamiento de archivos y bloques. Para aumentar el límite, póngase en contacto con el representante de ventas. Para obtener más información, consulte [Gestión de los límites de almacenamiento](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits).

## ¿Cuántas instancias pueden compartir el uso de un volumen de {{site.data.keyword.filestorage_short}} suministrado?
{: faq}

El límite predeterminado para el número de autorizaciones por volumen de archivo es de 64. Para aumentar este límite, póngase en contacto con el representante de ventas.

## ¿Cuántos volúmenes de {{site.data.keyword.filestorage_short}} se pueden adjuntar a un único host?
{: faq}

Eso depende de lo que el sistema operativo del host pueda manejar, no se trata de una limitación de {{site.data.keyword.BluSoftlayer_full}}. Consulte la documentación de su sistema operativo para conocer los límites en la cantidad de comparticiones de archivo que se pueden montar.

## ¿Cuántos recursos compartidos de archivos se permiten por tamaño de volumen de archivos? ¿Cuál es el máximo de recursos compartidos de archivos permitidos por tamaño de volumen?
{: faq}

<table>
  <caption>La Tabla 1 muestra el número máximo de inodes permitidos según el tamaño del volumen. Los tamaños de volumen están en la columna de la izquierda. El número de inodes y comparticiones de archivo está a la derecha.</caption>
  <thead>
    <tr>
      <th>Tamaño del volumen</th>
      <th>Inodes y comparticiones de archivos</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>20 GB - 39 GB</td>
      <td>622.484</td>
    </tr>
    <tr>
      <td>40 GB - 79 GB</td>
      <td>1.245.084</td>
    </tr>          
    <tr>
      <td>80 GB - 99 GB</td>
      <td>2.490.263</td>
    </tr>          
    <tr>
      <td>100 GB - 249 GB</td>
      <td>3.112.863</td>
    </tr>          
    <tr>
      <td>250 GB - 499 GB</td>
      <td>7.782.300</td>
    </tr>          
    <tr>
      <td>500 GB - 999 GB</td>
      <td>15.564.695</td>
    </tr>
    <tr>
      <td>1 TB</td>
      <td>31.876.593</td>
    </tr>
    <tr>
      <td>2 TB</td>
      <td>63.753.186</td>
    </tr>
    <tr>
      <td>3 TB</td>
      <td>95.629.970</td>
    </tr>
    <tr>
      <td>4 TB - 12 TB</td>
      <td>127.506.359</td>
    </tr>
   </tbody>
</table>

## Medición de IOPS
{: faq}

IOPS se gestiona en función de un perfil de carga de bloques de 16 KB con un 50 por ciento de lecturas y un 50 por ciento de escrituras aleatorias. Las cargas de trabajo que difieren de este perfil pueden experimentar un rendimiento pobre.

## ¿Qué ocurre si utilizo un tamaño de bloque inferior al medir el rendimiento?
{: faq}

Se puede alcanzar un IOPS máximo aunque utilice tamaños de bloque de menor tamaño. Sin embargo, el rendimiento será inferior en este caso. Por ejemplo, un volumen con 6000 IOPS tiene el siguiente rendimiento en varios tamaños de bloque:

- 16 KB * 6000 IOPS == ~93,75 MB/seg.
- 8 KB * 6000 IOPS == ~46,88 MB/seg.
- 4 KB * 6000 IOPS == ~23,44 MB/seg.


## ¿IOPS asignado se aplica por instancia o por volumen?
{: faq}

IOPS se aplica a nivel de volumen. Dicho de otro modo, dos hosts conectados a un volumen con 6000 IOPS comparten estos 6000 IOPS.

## ¿El volumen debe calentarse previamente para alcanzar el rendimiento esperado?
{: faq}

No es necesario ningún calentamiento previo. Puede observar el rendimiento especificado inmediatamente después de suministrar el volumen.

## ¿Se puede lograr más rendimiento si se utiliza una conexión Ethernet más rápida?
{: faq}

Los límites de rendimiento se establecen a nivel de volumen. Este límite no puede aumentarse utilizando una conexión Ethernet más rápida. Sin embargo, con una conexión Ethernet más lenta, el ancho de banda sí que puede ser un posible cuello de botella.

## ¿Los cortafuegos y los grupos de seguridad afectan al rendimiento?
{: faq}

Es mejor ejecutar el tráfico de almacenamiento en una VLAN, que omita el cortafuegos. La ejecución del tráfico de almacenamiento a través de cortafuegos de software aumenta la latencia y afecta negativamente al rendimiento del almacenamiento.

## ¿Qué latencia de rendimiento se puede esperar de {{site.data.keyword.filestorage_short}}?   
{: faq}

La latencia de destino dentro del almacenamiento es inferior a 1 ms. El almacenamiento está conectado a instancias de cálculo en una red compartida, por lo que la latencia de rendimiento exacta depende del tráfico de red durante la operación.

## ¿Qué ocurre con los datos cuando se suprimen volúmenes de {{site.data.keyword.filestorage_short}}?
{: faq}

{{site.data.keyword.filestorage_full}} presenta comparticiones de archivos a los clientes en almacenamiento físico que se borra antes de cualquier reutilización. Los clientes con requisitos especiales de cumplimiento, como las directrices NIST 800-88 para el saneamiento de datos, deben realizar el procedimiento de saneamiento de datos antes de suprimir su almacenamiento.

## ¿Qué pasa a las unidades que quedan fuera de servicio en el centro de datos de nube?
{: faq}

Cuando las unidades quedan fuera de servicio, IBM las destruye antes de desecharlas. Las unidades se vuelven inutilizables. Los datos escritos en dicha unidad se vuelven inaccesibles.
