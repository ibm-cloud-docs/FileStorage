---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-02"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.filestorage_short}} - Preguntas más frecuentes

## ¿Cómo se miden las IOPS?

Los IOPS se miden en función de un perfil de carga de bloques de 16 KB con 50% de lecturas y 50% de escrituras aleatorias. Las cargas de trabajo que difieren de este perfil pueden experimentar un rendimiento inferior.

## ¿Qué ocurre si utilizo un tamaño de bloque inferior al medir el rendimiento?

El máximo de IOPS todavía se puede obtener si se utilizan tamaños de bloque más pequeños, aunque el rendimiento será inferior. Por ejemplo; un volumen con 6000 IOPS tendrá el siguiente rendimiento en varios tamaños de bloque:

- 16 KB * 6000 IOPS == ~93,75 MB/seg.
- 8 KB * 6000 IOPS == ~46,88 MB/seg.
- 4 KB * 6000 IOPS == ~23,44 MB/seg.


## ¿El volumen debe calentarse previamente para alcanzar el rendimiento esperado?

No es necesario ningún calentamiento previo. Observará el rendimiento especificado inmediatamente después de suministrar el volumen.

## ¿Cómo puedo saber cuáles de mis volúmenes/LUN de {{site.data.keyword.filestorage_short}} están cifrados?

Al consultar la lista de {{site.data.keyword.filestorage_short}} en el portal del cliente, verá un icono de bloqueo a la derecha del nombre de volumen/LUN en aquellos que estén cifrados.

## Si tengo {{site.data.keyword.filestorage_short}} no cifrado suministrado en el centro de datos que ha sido actualizado para el cifrado, ¿puedo cifrar mi {{site.data.keyword.filestorage_short}}?

El {{site.data.keyword.filestorage_short}} que se haya suministrado antes de una actualización del centro de datos no puede cifrarse. El nuevo {{site.data.keyword.filestorage_short}} suministrado en centros de datos actualizados se cifra automáticamente; no hay que elegir ningún valor de cifrado, es automático. Los datos que residen en almacenamiento no cifrado en un centro de datos actualizado se pueden cifrar creando un nuevo volumen de archivos para posteriormente copiar los datos al nuevo volumen cifrado o volumen con migración basada en host. Consulte [este artículo](/docs/infrastructure/FileStorage/migrate-file-storage-encrypted-file-storage.html) para obtener instrucciones sobre cómo realizar la migración.

## ¿Cómo sé si estoy suministrando el {{site.data.keyword.filestorage_short}} en un centro de datos actualizado?

Al suministrar el {{site.data.keyword.filestorage_short}}, todos los centros de datos actualizados se marcan con un asterisco (`*`) en el formulario del pedido y una indicación de que suministrará almacenamiento con cifrado. Una vez suministrado el almacenamiento, verá un icono en la lista de almacenamiento que muestra dicho volumen como cifrado. Todos los volúmenes cifrados y volúmenes se suministran únicamente en centros de datos actualizados. Puede consultar una lista completa de centros de datos actualizados y características disponibles [aquí](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html).

## ¿Por qué puedo suministrar {{site.data.keyword.filestorage_short}} con un nivel 10 de IOPS de Resistencia en algunos centros de datos y en otros no?

El {{site.data.keyword.filestorage_short}} de tipo Resistencia con nivel 10 IOPS/GB solo está disponible en centros de datos seleccionados, pronto se añadirán nuevos centros de datos.  Puede consultar una lista completa de centros de datos actualizados y características disponibles [aquí](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html).

## ¿Cómo puedo encontrar el punto de montaje correcto para mi {{site.data.keyword.filestorage_short}}?

Todos los volúmenes de {{site.data.keyword.filestorage_short}} cifrados suministrados en estos centros de datos tienen un punto de montaje distinto que los volúmenes no cifrados. Para asegurarse de que utiliza el punto de montaje correcto para los volúmenes de {{site.data.keyword.filestorage_short}} cifrados y no cifrados, puede consultar la información de punto de montaje en la página **Detalles del volumen** en la interfaz de usuario, así como acceder al punto de montaje correcto mediante una llamada de API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

## ¿Cuántos recursos compartidos de archivos se permiten por tamaño de volumen de archivos? ¿Cuál es el máximo de recursos compartidos de archivos permitidos por tamaño de volumen?
A continuación se indican el número máximo de inodes o recursos compartidos de archivos permitidos en función del tamaño de volumen:

<table>
        <tbody>
          <tr>
            <th>Tamaño del volumen</th>
            <th>Inodes/Archivos</th>
          </tr>
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

## ¿Cuántos volúmenes puedo suministrar?

De forma predeterminada, puede suministrar un total combinado de 250 volúmenes de almacenamiento de archivos y bloques.  Póngase en contacto con su representante de ventas para aumentar el número de volúmenes.

## ¿Cuántas instancias pueden compartir el uso de un volumen de {{site.data.keyword.filestorage_short}} suministrado?

El límite predeterminado para el número de autorizaciones por volumen de archivo es de 64. Póngase en contacto con su representante de ventas para aumentar el límite.

## Al suministrar el {{site.data.keyword.filestorage_short}} de Rendimiento o Resistencia, ¿las IOPS asignadas se aplican por instancia o por volumen?

Los IOPS se aplican a nivel de volumen. Dicho de otro modo, dos hosts conectados a un volumen con 6000 IOPS comparten estos 6000 IOPS.

## ¿Podré incrementar el rendimiento si utilizo una conexión de Ethernet más rápida?

Los límites de rendimiento están establecidos por volumen/LUN, por lo que utilizar una conexión de Ethernet más rápida no incrementará el límite establecido. Sin embargo, con una conexión Ethernet más lenta, el ancho de banda sí que puede ser un posible cuello de botella.

## ¿Los cortafuegos/grupos de seguridad afectarán al rendimiento?

Recomendamos que ejecute el tráfico de almacenamiento en una red de área local virtual que omita el cortafuegos. La ejecución del tráfico de almacenamiento a través de cortafuegos de software incrementará la latencia e incidirá negativamente sobre el rendimiento del almacenamiento.

## ¿Qué ocurre con mis datos cuando se suprimen volúmenes de {{site.data.keyword.filestorage_short}}?

Cuando se suprime el almacenamiento, se eliminan todos los punteros a los datos en dicho volumen, por lo que los datos serán completamente inaccesibles. Si el almacenamiento físico se vuelve a suministrar a otra cuenta, se asignará un nuevo conjunto de punteros. La nueva cuenta no tiene ningún modo de acceder a los datos que pudieran haberse guardado en el almacenamiento físico, el nuevo conjunto de punteros muestra todo 0. Cuando se escriben nuevos datos en el volumen/LUN, se sobrescriben los datos inaccesibles que aún existan. 

## ¿Qué latencia de rendimiento puedo esperar de mi {{site.data.keyword.filestorage_short}}?   

La latencia de destino en el almacenamiento es de <1 ms. Nuestro almacenamiento está conectado a instancias de cálculo en una red compartida, por lo que la latencia de rendimiento exacta dependerá del tráfico de la red dentro de un plazo determinado.

