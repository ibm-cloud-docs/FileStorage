---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-29"

---
{:new_window: target="_blank"}

# {{site.data.keyword.filestorage_short}} - Preguntas más frecuentes

## ¿Cómo puedo saber cuáles de mis volúmenes de {{site.data.keyword.filestorage_short}} están cifrados?
Examine su lista de {{site.data.keyword.filestorage_short}} en el portal de clientes. Verá un icono de bloqueo a la derecha del nombre de LUN o de volumen para los volúmenes que están cifrados.

## Si he adquirido {{site.data.keyword.filestorage_short}} no cifrado en un centro de datos que se ha actualizado para admitir cifrado, ¿puedo cifrar mi {{site.data.keyword.filestorage_short}}?
{{site.data.keyword.filestorage_short}} suministrado antes de una actualización del centro de datos no se puede cifrar. El nuevo {{site.data.keyword.filestorage_short}} suministrado en centros de datos actualizados se cifra automáticamente. No hay que elegir ningún valor de cifrado; es automático. Para cifrar los datos que residen en almacenamiento no cifrado, hay que crear un nuevo volumen y luego copiar los datos en el nuevo volumen cifrado con una migración basada en host. Para obtener más información, consulte [Migración de File Storage](/docs/infrastructure/FileStorage/migrate-file-storage-encrypted-file-storage.html).

## ¿Cómo sé si estoy suministrando {{site.data.keyword.filestorage_short}} en un centro de datos actualizado?
En el formulario de solicitud de {{site.data.keyword.filestorage_short}}, todos los centros de datos actualizados están indicados con un asterisco (`*`). Durante el proceso de solicitud se le indicará que va a suministrar almacenamiento con cifrado. Una vez suministrado el almacenamiento, puede ver un icono en la lista de almacenamiento que muestra que dicho volumen está cifrado. 

Todos los volúmenes y comparticiones de archivos cifrados se suministran únicamente en centros de datos actualizados. Puede consultar una lista completa de centros de datos actualizados y características disponibles [aquí](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html).

## ¿Por qué {{site.data.keyword.filestorage_short}} con un nivel 10 de IOPS de Resistencia se puede suministrar en algunos centros de datos y en otros no?
{{site.data.keyword.filestorage_short}} de tipo Resistencia con nivel 10 IOPS/GB solo está disponible en determinados centros de datos, y pronto se añadirán nuevos centros de datos. Puede consultar una lista completa de centros de datos actualizados y características disponibles [aquí](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html).

## ¿Cómo puedo encontrar el punto de montaje correcto para mi {{site.data.keyword.filestorage_short}}?
Todos los volúmenes de {{site.data.keyword.filestorage_short}} cifrados suministrados en los centros de datos mejorados tienen un punto de montaje distinto que los volúmenes no cifrados. Para asegurarse de que está utilizando el punto de montaje correcto, consulte la información sobre punto de montaje en la página **Detalles del volumen** de la interfaz de usuario. También puede acceder al punto de montaje correcto mediante una llamada de API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

## ¿Cuántos volúmenes puedo suministrar?
De forma predeterminada, puede suministrar un total combinado de 250 volúmenes de almacenamiento de archivos y bloques. Para aumentar el límite, póngase en contacto con el representante de ventas.

## ¿Cuántas instancias pueden compartir el uso de un volumen de {{site.data.keyword.filestorage_short}} suministrado?
El límite predeterminado para el número de autorizaciones por volumen de archivo es de 64. Para aumentar este límite, póngase en contacto con el representante de ventas.

## ¿Cuántos recursos compartidos de archivos se permiten por tamaño de volumen de archivos? ¿Cuál es el máximo de recursos compartidos de archivos permitidos por tamaño de volumen?

<table>
  <caption>La Tabla 1 muestra que el número máximo de inodes permitido se basa en el tamaño del volumen. Los tamaños de volumen están a la izquierda. El número de inodos/comparticiones de archivo está a la derecha.</caption>
  <thead>
    <tr>
      <th>Tamaño del volumen</th>
      <th>Inodes/comparticiones de archivos</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>20 GB </td>
      <td>622.484</td>
    </tr>
    <tr>
      <td>40 GB </td>
      <td>1.245.084</td>
    </tr>          
    <tr>
      <td>80 GB</td>
      <td>2.490.263</td>
    </tr>          
    <tr>
      <td>100 GB</td>
      <td>3.112.863</td>
    </tr>          
    <tr>
      <td>250 GB</td>
      <td>7.782.300</td>
    </tr>          
    <tr>
      <td>500 GB</td>
      <td>15.564.695</td>
    </tr>
    <tr>
      <td>+1 TB</td>
      <td>31.876.593</td>
    </tr>
   </tbody>
</table>

## Medición de IOPS
IOPS se gestiona en función de un perfil de carga de bloques de 16 KB con un 50 por ciento de lecturas y un 50 por ciento de escrituras aleatorias. Las cargas de trabajo que difieren de este perfil pueden experimentar un rendimiento pobre.

## ¿Qué ocurre si utilizo un tamaño de bloque inferior al medir el rendimiento?
Se puede alcanzar un IOPS máximo aunque utilice tamaños de bloque de menor tamaño. Sin embargo, el rendimiento será inferior en este caso. Por ejemplo, un volumen con 6000 IOPS tiene el siguiente rendimiento en varios tamaños de bloque:

- 16 KB * 6000 IOPS == ~93,75 MB/seg.
- 8 KB * 6000 IOPS == ~46,88 MB/seg.
- 4 KB * 6000 IOPS == ~23,44 MB/seg.


## ¿IOPS asignado se aplica por instancia o por volumen?
IOPS se aplica a nivel de volumen. Dicho de otro modo, dos hosts conectados a un volumen con 6000 IOPS comparten estos 6000 IOPS.

## ¿El volumen debe calentarse previamente para alcanzar el rendimiento esperado?
No es necesario ningún calentamiento previo. Puede observar el rendimiento especificado inmediatamente después de suministrar el volumen.

## ¿Se puede lograr más rendimiento si se utiliza una conexión Ethernet más rápida?
Los límites de rendimiento están establecidos por volumen/LUN, por lo que utilizar una conexión de Ethernet más rápida no aumenta el límite establecido. Sin embargo, con una conexión Ethernet más lenta, el ancho de banda sí que puede ser un posible cuello de botella.

## ¿Los cortafuegos/grupos de seguridad afectan al rendimiento?
Es mejor ejecutar el tráfico de almacenamiento en una VLAN, que omita el cortafuegos. La ejecución del tráfico de almacenamiento a través de cortafuegos de software aumenta la latencia y afecta negativamente al rendimiento del almacenamiento.

## ¿Qué latencia de rendimiento se puede esperar de {{site.data.keyword.filestorage_short}}?   
La latencia de destino en el almacenamiento es de <1 ms. El almacenamiento está conectado a instancias de cálculo en una red compartida, por lo que la latencia de rendimiento exacta depende del tráfico de red durante la operación.

## ¿Qué ocurre con los datos cuando se suprimen volúmenes de {{site.data.keyword.filestorage_short}}?
Cuando se suprime el almacenamiento, se eliminan todos los punteros a los datos en dicho volumen, por lo que los datos de vuelven inaccesibles. Si el almacenamiento físico se vuelve a suministrar a otra cuenta, se asignará un nuevo conjunto de punteros. La nueva cuenta no tiene ningún modo de acceder a los datos que se han almacenado en el almacenamiento físico. El nuevo conjunto de punteros muestra todo 0. Los datos nuevos sobrescriben los datos inaccesibles que existían en ese almacenamiento físico.

## ¿Qué pasa a las unidades que quedan fuera de servicio en el centro de datos de nube?
Cuando las unidades quedan fuera de servicio, IBM las destruye antes de desecharlas. Las unidades se vuelven inutilizables. Los datos escritos en dicha unidad se vuelven inaccesibles.
