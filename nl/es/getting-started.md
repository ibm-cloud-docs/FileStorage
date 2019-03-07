---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-28"

keywords:

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
 {:shortdesc: .shortdesc}


# Guía de aprendizaje de iniciación
{: #gettingstarted}

{{site.data.keyword.filestorage_full}} es {{site.data.keyword.filestorage_short}} conectado a red, basado en NFS, flexible, rápido y persistente. En este entorno de almacenamiento adjunto de red (NAS), tiene el control total sobre las funciones y el rendimiento de los recursos compartidos de archivos. Los recursos compartidos de {{site.data.keyword.filestorage_short}} se pueden conectar a hasta 64 dispositivos autorizados sobre conexiones TCP/IP direccionadas para aumentar la capacidad de recuperación.
{:shortdesc}

## Antes de empezar
{: #prereqs}

Los volúmenes de {{site.data.keyword.filestorage_short}} se pueden suministrar de 20 GB a 12 TB con dos opciones: <br/>
- Suministro de niveles de **Resistencia** que presentan niveles de rendimiento predefinidos y otras características como instantáneas y réplica.
- Crear un entorno de **Rendimiento** de alta potencia con operaciones de entrada/salida asignadas por segundo (IOPS).

Para obtener más información sobre la oferta de {{site.data.keyword.filestorage_short}}, consulta [Acerca de {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-about)

### Consideraciones sobre el suministro

**Tamaño de bloque**

Los IOPS, para Resistencia y Rendimiento, se basan en un tamaño de bloque de 16 KB con una carga de trabajo aleatoria del 50 % de lectura/escritura 50/50. Un bloque de 16 KB es el equivalente a una escritura en el volumen.
{:important}

El tamaño de bloque que utiliza la aplicación afecta directamente al rendimiento del almacenamiento. Si el tamaño de bloque utilizado por la aplicación es inferior a 16 KB, se alcanza el límite de IOPS antes que el límite de rendimiento. Por el contrario, si el tamaño de bloque utilizado por la aplicación es superior a 16 KB, se alcanza el límite de rendimiento antes que el límite de IOPS.

<table>
  <caption>En la Tabla 4 se muestran ejemplos de cómo el tamaño de bloque e IOPS afectan al rendimiento.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <thead>
          <tr>
            <th>Tamaño de bloque (KB)</th>
            <th>IOPS</th>
            <th>Rendimiento (MB/s)</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>4 (típico para Linux)</td>
            <td>1.000</td>
            <td>4</td>
          </tr>
          <tr>
            <td>8 (típico para Oracle)</td>
            <td>1.000</td>
            <td>8</td>
          </tr>
          <tr>
            <td>16</td>
            <td>1.000</td>
            <td>16</td>
          </tr>
          <tr>
            <td>32 (típico para SQL Server)</td>
            <td>500</td>
            <td>16</td>
          </tr>          
          <tr>
            <td>64</td>
            <td>250</td>
            <td>16</td>
          </tr>
          <tr>
            <td>128</td>
            <td>128</td>
            <td>16</td>
          </tr>
          <tr>
            <td>512</td>
            <td>32</td>
            <td>16</td>
          </tr>
        </tbody>
</table>

**Hosts autorizados**

Otro factor a tener en cuenta es el número de hosts que están utilizando el volumen. Si solo hay un host que accede al volumen, puede ser difícil alcanzar el máximo de IOPS, sobre todo en recuentos de IOPS extremos (10.000 s). Si su carga de trabajo requiere un alto rendimiento, será mejor configurar al menos dos servidores para acceder al volumen a fin de evitar un cuello de botella por un servidor.

**Conexión de red**

La velocidad de la conexión de Ethernet debe ser más rápida que el rendimiento máximo previsto de su volumen. Generalmente, no espere que se sature la conexión de Ethernet más allá del 70 % del ancho de banda disponible. Por ejemplo, si tiene 6.000 IOPS y está utilizando un tamaño de bloque de 16 KB, el volumen puede manejar aproximadamente un rendimiento de 94 MBps. Si tiene una conexión de Ethernet de 1 Gbps para el LUN, se genera un cuello de botella cuando los servidores intenten utilizar el máximo rendimiento disponible. Esto se debe a que el 70 % del límite teórico de una conexión de Ethernet de 1 Gbps (125 MB por segundo) solo permitiría 88 MB por segundo.

Para alcanzar el número máximo de IOPS, es necesario disponer de los recursos de red adecuados. Otros aspectos a tener en cuenta son el uso de la red privada fuera del almacenamiento y los ajustes del lado del host y específicos de la aplicación (pila IP o [profundidades de colas](/docs/infrastructure/FileStorage?topic=FileStorage-hostqueuesettings), y otros valores).

El tráfico de almacenamiento se incluye en el uso total de la red de los servidores virtuales públicos. Para obtener más información acerca de los límites que puede imponer el servicio, consulte la [Documentación de servidor virtual](/docs/vsi?topic=virtual-servers-about-public-virtual-servers).

**Versión de NFS**

Tanto NFS v3 como NFS v4.1 están soportados en el entorno de {{site.data.keyword.BluSoftlayer_full}}. Sin embargo, se prefiere NFS v3 porque NFS v4.1 es un protocolo con estado (no sin estado como NFSv3) y esto puede generar problemas de protocolo durante sucesos de red. NFS v4.1 debe desactivar temporalmente todas las operaciones y realizar la reclamación de bloqueo. En un servidor de archivos NFS relativamente ocupado, la latencia incrementada puede causar interrupciones. La falta de conexión troncal y multivía de NFS v4.1 también puede alargar la recuperación de operaciones de NFS.

## Envío de su pedido

Cuando esté listo para enviar el pedido, puede realizarlo a través de la [consola](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole) o de la [SLCLI](/docs/infrastructure/FileStorage?topic=FileStorage-orderingSLCLI). Para suministrar almacenamiento de archivos con VMware, pulse [aquí](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)

## Conexión del nuevo almacenamiento
{: #mountingstorage}

Cuando se haya completado la solicitud de suministro, autorice a los hosts a acceder al nuevo almacenamiento y configure la conexión. En función del sistema operativo del host, siga el enlace adecuado.
- [Acceso a {{site.data.keyword.filestorage_short}} en Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [Montaje de {{site.data.keyword.filestorage_short}} en CentOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [Montaje de {{site.data.keyword.filestorage_short}} en CoreOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)
- [Configuración de {{site.data.keyword.filestorage_short}} para la copia de seguridad con cPanel](/docs/infrastructure/FileStorage?topic=FileStorage-cPanelBackups)
- [Configuración de {{site.data.keyword.filestorage_short}} para la copia de seguridad con Plesk](/docs/infrastructure/FileStorage?topic=FileStorage-PleskBackup)
- [Montaje de un volumen de {{site.data.keyword.filestorage_short}} en hosts de ESXi](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)

## Gestión del nuevo almacenamiento

Mediante el portal o SLCLI, puede gestionar varios aspectos de su {{site.data.keyword.filestorage_short}}, como autorizaciones de hosts y cancelaciones. Para obtener más información, consulte [Gestión de {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage).
