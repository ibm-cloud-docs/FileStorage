---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Guía de arquitectura para {{site.data.keyword.filestorage_short}} con VMware

A continuación se describen los pasos para realizar el pedido y configurar {{site.data.keyword.filestorage_full}} en un entorno de vSphere 5.5 y vSphere 6.0 en {{site.data.keyword.BluSoftlayer_full}}. Utilizar {{site.data.keyword.filestorage_short}} NFS es la práctica recomendada si necesita más de 8 conexiones a su host VMWare.

## Visión general de {{site.data.keyword.filestorage_short}}

Nuestro {{site.data.keyword.filestorage_short}} está diseñado para dar soporte a aplicaciones de entrada/salida alta que requieren niveles de rendimiento previsibles. El rendimiento previsible se consigue mediante la asignación de operaciones de entrada/salida a nivel de protocolo por segundo (IOPS) para volúmenes individuales.

El acceso y el montaje de la oferta de {{site.data.keyword.filestorage_short}} se realiza mediante una conexión NFS. En un despliegue de VMware, se puede montar un único volumen para hasta 64 hosts ESXi como almacenamiento compartido, o bien puede montar múltiples volúmenes para crear un clúster de almacenamiento para utilizar la Planificación de recursos DYNAMIC de almacenamiento vSphere.

Las opciones de precio y configuración para el {{site.data.keyword.filestorage_short}} de Resistencia y Rendimiento se facturan en base a una combinación del espacio reservado y para las IOPS ofrecidas.

### 1. Consideraciones de {{site.data.keyword.filestorage_short}}

Al realizar el pedido de {{site.data.keyword.filestorage_short}}, deberá tener en cuenta la siguiente información y consideraciones:

- El tamaño del almacenamiento, las IOPS y el sistema operativo no se podrán cambiar una vez suministrados los volúmenes de {{site.data.keyword.filestorage_short}}. Cualquier cambio que desee realizar con respecto a la cantidad de espacio, el número de IOPS o el sistema operativo requerirá el suministro de un nuevo volumen. Cualquier dato almacenado en el volumen anterior tendrá que migrarse al nuevo volumen utilizando VMware Storage vMotion.
- Al decidir sobre el tamaño, tenga en cuenta el tamaño de la carga de trabajo y el rendimiento necesario. El tamaño es importante con el servicio Resistencia, que escala el rendimiento de forma lineal en relación con la capacidad (IOPS/GB), en contraposición al servicio Rendimiento, que permite al administrador elegir la capacidad y el rendimiento de forma independiente. Los requisitos de rendimiento son importantes con el servicio Rendimiento. <br/> **Nota**: el cálculo del rendimiento es IOPS x 16 KB. Los IOPS se miden en base a un tamaño de bloque de 16 KB con una combinación de lectura/escritura 50/50. <br/> **Nota**: si aumenta el tamaño de bloque, aumenta el rendimiento pero disminuye las IOPS. Por ejemplo, duplicar el tamaño de bloque a 32KB bloques mantendrá el rendimiento máximo pero reducirá a la mitad las IOPS.
- NFS utiliza muchas operaciones de control de archivos adicionales tales como búsqueda, getattr o readdir, por nombrar algunos. Estas operaciones, junto con las operaciones de lectura/escritura, pueden contar como IOPS y varían según el tipo de operación y la versión de NFS.
- Técnicamente, se pueden seleccionar conjuntamente varios volúmenes para conseguir IOPS más altos y un mayor rendimiento, pero VMware recomienda un único almacén de datos Virtual Machine File System (VMFS) por volumen para evitar la degradación del rendimiento.
- Los volúmenes de {{site.data.keyword.filestorage_short}} están expuestos a dispositivos autorizados, subredes o direcciones IP.
- Los servicios de Instantánea y Réplica están disponibles de forma nativa únicamente en volúmenes de {{site.data.keyword.filestorage_short}} de Resistencia. El {{site.data.keyword.filestorage_short}} de Rendimiento no tiene estas prestaciones.
- Para evitar la desconexión del almacenamiento durante una migración tras error de vía de acceso, {{site.data.keyword.IBM}} recomienda la instalación de herramientas VMWare, que establecerán un valor de tiempo de espera adecuado. No es necesario cambiar el valor, el valor predeterminado es suficiente para garantizar que el host de VMWare no pierda la conectividad.
- Tanto NFS v3 como NFS v4.1 están soportados en el entorno de {{site.data.keyword.BluSoftlayer_full}}. Sin embargo, {{site.data.keyword.IBM}} recomienda el uso de NFS v3. Como NFS v4.1 es un protocolo con estado (no sin estado como NFSv3), se pueden producir problemas de protocolo durante sucesos de red. NFS v4.1 debe desactivar temporalmente las operaciones y realizar la reclamación de bloqueo. Durante estas operaciones, pueden producirse interrupciones.

####  Matriz de soporte de características VMware de protocolo NFS.
<table>
 <tbody>
  <tr>
   <th>Características de vSphere</th>
   <th>NFS versión 3</th>
   <th>NFS versión 4.1</th>
  </tr>
  <tr>
   <td>vMotion y Storage vMotion</td>
   <td>Sí</td>
   <td>Sí</td>
  </tr>
  <tr>
   <td>Alta disponibilidad (HA)</td>
   <td>Sí</td>
   <td>Sí</td>
  </tr>
  <tr>
   <td>Tolerancia al error (FT)</td>
   <td>Sí</td>
   <td>Sí</td>
  </tr>
  <tr>
   <td>DRS (planificador de recursos distribuidos)</td>
   <td>Sí</td>
   <td>Sí</td>
  </tr>
  <tr>
   <td>Perfiles de host</td>
   <td>Sí</td>
   <td>Sí</td>
  </tr>
  <tr>
   <td>DRS de almacenamiento</td>
   <td>Sí</td>
   <td>No</td>
  </tr>
  <tr>
   <td>Control de E/S de almacenamiento</td>
   <td>Sí</td>
   <td>No</td>
  </tr>
  <tr>
   <td>Site Recovery Manager</td>
   <td>Sí</td>
   <td>No</td>
  </tr>
  <tr>
   <td>Virtual
Volumes</td>
   <td>Sí</td>
   <td>No</td>
  </tr>
 </tbody>
</table>
*Fuente: [VMWare - NFS Protocols and ESXi](https://docs.vmware.com/en/VMware-vSphere/6.0/com.vmware.vsphere.storage.doc/GUID-8A929FE4-1207-4CC5-A086-7016D73C328F.html){:new_window}*





### 2. Instantáneas de {{site.data.keyword.filestorage_short}} de Resistencia

El {{site.data.keyword.filestorage_short}} de Resistencia permite a los administradores establecer planificaciones de instantáneas que crean y suprimen copias de instantáneas automáticamente para cada volumen de almacenamiento. También pueden crear planificaciones de instantáneas adicionales (cada hora, diariamente, semanalmente) para instantáneas automáticas y crear instantáneas adhoc manualmente para casos de continuidad del negocio y recuperación tras desastre (BCDR). Las alertas automáticas se envían a través del [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} al propietario del volumen para las instantáneas retenidas y el espacio consumido.

Tenga en cuenta que se necesita "espacio de instantáneas" para utilizar instantáneas. El espacio se puede adquirir en el pedido de volumen inicial o después del suministro inicial a través de la página **Detalles de volumen** pulsando el botón desplegable Acciones y seleccionando **Añadir espacio de instantáneas**.

Es importante señalar que los entornos de VMware no son conscientes de las instantáneas. La capacidad de instantáneas de {{site.data.keyword.filestorage_short}} de Resistencia no debe confundirse con las instantáneas de VMware. Toda recuperación que utilice la característica de instantáneas de {{site.data.keyword.filestorage_short}} de Resistencia debe manejarse desde el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}. Restaurar el volumen de {{site.data.keyword.filestorage_short}} de Resistencia requerirá apagar todas las máquinas virtuales que residen en el {{site.data.keyword.filestorage_short}} de Resistencia y desmontar temporalmente el volumen de los hosts de ESXi para evitar la corrupción de datos durante el proceso.

Consulte nuestro artículo sobre [instantáneas](snapshots.html) para obtener más detalles sobre cómo configurar las instantáneas.


### 3. Réplica de almacén de archivos

La réplica utiliza una de sus planificaciones de instantáneas para copiar automáticamente las instantáneas a un volumen de destino de un centro de datos remoto. Las copias se pueden recuperar en el sitio remoto en caso de corrupción de datos o de suceso catastrófico.


Las réplicas le permiten

- Recuperarse rápidamente de fallos del sitio y otros desastres realizando la migración al volumen de destino
- Realizando la migración tras error a un punto específico en el tiempo en la copia de recuperación tras desastre

Antes de poder replicar, debe crear una planificación de instantáneas. Cuando realiza la migración tras error, está “cambiando el interruptor” de su volumen de almacenamiento del centro de datos primario al volumen de destino del centro de datos remoto. Por ejemplo, su centro de datos primario es Londres y el centro de datos secundario es Ámsterdam. Si se produce un suceso de error, realizaría la migración a Ámsterdam – conectando al ahora volumen primario desde una instancia de clúster de vSphere en Ámsterdam.


Cuando su volumen de Londres se haya reparado, se realiza una instantánea del volumen de Ámsterdam para volver a Londres y al volumen primario de nuevo desde una instancia de cálculo de Londres. Antes de que el volumen vuelva al centro de datos primario, debe dejar de ser utilizado en el sitio remoto. Se realiza una instantánea de cualquier información nueva o modificada y se replica al centro de datos primario antes de poder montarse de nuevo en los hosts de ESXi del sitio de producción.


Consulte la página de información [Réplica](replication.html) para obtener más detalles sobre cómo configurar la réplica.

**Nota**: los datos no válidos, ya estén corruptos, pirateados o infectados, se replicarán de acuerdo con la planificación de instantáneas y la retención de instantáneas. El uso de la ventana de réplica más pequeña puede proporcionar un mejor objetivo de punto de recuperación, además de reducir el tiempo de reacción a la réplica de datos no válidos.





## Realizar el pedido de {{site.data.keyword.filestorage_short}}

Puede realizar el pedido y configurar {{site.data.keyword.filestorage_short}} para un entorno de VMware ESXi 5. Utilice la información siguiente junto con la Arquitectura avanzada de referencia de VMware de un solo sitio para establecer una de estas opciones de almacenamiento en el entorno de VMware.


Se puede realizar el pedido de {{site.data.keyword.filestorage_short}} a través del [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, accediendo a la página {{site.data.keyword.filestorage_short}} a través de **Almacenamiento** > **{{site.data.keyword.filestorage_short}}**.


### 1. Realizar el pedido de {{site.data.keyword.filestorage_short}}

Efectúe los siguientes pasos para realizar un pedido de {{site.data.keyword.filestorage_short}}:
1. Pulse **Almacenamiento** > **{{site.data.keyword.filestorage_short}}** en la página de inicio de [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
2. Pulse sobre el enlace **Realizar el pedido de {{site.data.keyword.filestorage_short}}** en la página de **{{site.data.keyword.filestorage_short}}**.
3. Seleccione **Resistencia**/**Rendimiento** en la lista desplegable Seleccionar tipo de almacenamiento.
4. Seleccione la ubicación. Los centros de datos con las prestaciones mejoradas están marcados con `*`. Asegúrese de que el nuevo almacenamiento se añada en la misma ubicación que el host de ESXi pedido anteriormente.
5. Seleccione el método de facturación. Están disponibles las opciones de facturación mensual y por horas.
6. Seleccione la cantidad de espacio de almacenamiento deseada en GB. Para TB, 1 TB equivale a 1.000 GB, y 12 TB equivale a 12.000 GB.
7. Especifique la cantidad de IOPS deseada en intervalos de 100 o seleccione un nivel de IOPS.
8. Especifique el tamaño del espacio de instantáneas.
9. Pulse **Continuar**.
10. Escriba un código promocional si tiene uno, y pulse **Recalcular**.
11. Revise el pedido.
12. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro...**.
13. Pulse **Realizar pedido** para enviar el pedido, o **Cancelar** para cerrar el formulario sin enviar ningún pedido.

El almacenamiento se suministrará en menos de un minuto y estará visible en la página de **{{site.data.keyword.filestorage_short}}** del [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.



### 2. Autorizar a los hosts el uso de {{site.data.keyword.filestorage_short}}

Una vez suministrado el volumen, los {{site.data.keyword.BluBareMetServers_full}} o {{site.data.keyword.BluVirtServers_full}} que utilizarán el volumen necesitarán autorización para acceder al almacenamiento. Utilice los siguientes pasos para autorizar el volumen:

1. Pulse en **Almacenamiento** > **{{site.data.keyword.filestorage_short}}**.
2. Seleccione **Acceder a host** en el menú **Acciones de volumen de rendimiento** o **Resistencia**.
3. Seleccione el botón de opción **Subredes**
4. Elija de la lista de subredes disponibles que están asignadas a los puertos vmkernel en los hosts de ESXi y pulse **Enviar**.<br/> **Nota**: las subredes mostradas serán redes suscritas en el mismo centro de datos que el volumen de almacenamiento.


Una vez autorizadas las subredes, anote el nombre de host del servidor de almacenamiento de Resistencia o Rendimiento que desea utilizar cuando se monte el volumen. Esta información puede encontrarse en la página de detalles de {{site.data.keyword.filestorage_short}} pulsando sobre un volumen específico.


##  Configurar el host de máquina virtual de VMware

### 1. Requisitos previos

Antes de empezar el proceso de configuración de VMware, asegúrese de que se cumplen los siguientes requisitos previos:

- Los {{site.data.keyword.BluBareMetServers}} con VMware ESXi se suministran con la configuración de almacenamiento adecuada y credenciales de inicio de sesión de ESXi.
- {{site.data.keyword.BluSoftlayer_full}} Windows físico o {{site.data.keyword.virtualmachinesshort}} en el mismo centro de datos que los {{site.data.keyword.BluBareMetServers}}. Incluyendo la dirección IP pública de la máquina virtual de {{site.data.keyword.BluSoftlayer_full}} Windows y los credenciales de inicio de sesión.
- Un sistema con acceso a Internet, y con el software de navegador web y un cliente de Remote Desktop Protocol (RDP) instalados.


### 2. Pasos de la configuración del host de VMware.

Para configurar el host virtual, siga los siguientes pasos:

1. Desde un sistema conectado a Internet, inicie un cliente RDP y establezca una sesión RDP a los {{site.data.keyword.BluVirtServers_full}} suministrados en el mismo centro de datos donde está instalado vSphere vCenter.
2. Desde los {{site.data.keyword.BluVirtServers_short}}, inicie un navegador web y conéctese a VMware vCenter a través de vSphere Web Client.
3. Desde la pantalla **Inicio**, seleccione **Hosts y clústeres**. Amplíe el panel de la izquierda y seleccione el **servidor de VMware ESXi** que se utilizará para este despliegue.
4. Asegúrese de que el puerto de cortafuegos para el cliente NFS esté abierto en todos los hosts para configurar el cliente NFS en el host de vSphere. Se abre automáticamente en las versiones más recientes de vSphere. Para comprobar si el puerto está abierto, vaya al separador **Gestionar host de ESXi** en VMware® vCenter™, seleccione **Valores** y, a continuación, **Perfil de seguridad**. En la sección **Cortafuegos**, pulse **Editar** y desplácese a **Cliente NFS**.
5. Asegúrese de que esté marcado **Permitir la conexión desde cualquier dirección IP o una lista de direcciones IP**. <br/>
   ![Permitir conexión](/images/1_4.png)
6. Configure las tramas Jumbo desde el separador **Gestionar host de ESXi**, seleccionando **Gestionar** y, a continuación, **Redes**.
7. Seleccione **Adaptadores VMkernel**, resalte **vSwitch** y pulse **Editar** (icono de lápiz).
8. Seleccione **Configuración de NIC** y asegúrese de que la MTU de NIC tenga un valor de 9000.
   - En caso necesario, defina la MTU en 9000 y pulse **Aceptar**.
9. Si lo desea, los valores de la trama Jumbo pueden validarse del siguiente modo:
   - Desde Windows: ping -f -l 8972 a.b.c.d
   - Desde Unix: ping -s 8972 a.b.c.d
   Donde a.b.c.d es la interfaz de {{site.data.keyword.BluVirtServers_short}} contigua con el mandato:
   La salida aparece similar a:
   ```ping a.b.c.d (a.b.c.d) 8972(9000) bytes of data.
   8980 bytes from a.b.c.d: icmp_seq=1 ttl=128 time=3.36 ms
   ```

Encontrará más información sobre VMware y las tramas Jumbo [aquí](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1003712){:new_window}.


### 3. Añadir un adaptador de enlace ascendente a un conmutador virtual

1. Configure un nuevo adaptador de enlace ascendente desde el separador **Gestionar host de ESXi**, seleccionando **Gestionar** y, a continuación, **Redes**.
2. Seleccione el separador **Adaptadores físicos**.
3. Pulse **Añadir redes de host** (icono de globo con un signo más).
4. Seleccione el tipo de conexión como **Adaptador de red física** y pulse **Siguiente**.
5. Seleccione el **vSwitch** existente y pulse **Siguiente**.
6. Seleccione **Adaptadores no utilizados** y pulse **Añadir adaptadores** (signo más).
7. Pulse el otro adaptador "Conectado" y pulse **Aceptar**. <br/>
   ![Añadir adaptadores físicos a un conmutador](/images/2_3.png)
8. Pulse **Siguiente** y **Finalizar**.
9. Vuelva al separador **Conmutadores virtuales** y seleccione el icono superior **Editar configuración** situado bajo la cabecera **Conmutadores virtuales** (icono de lápiz).
10. Seleccione el **Equipo vSwitch** y la entrada de migración tras error a la izquierda.
Verifique que la opción **Equilibrio de carga** esté establecida en **Ruta basada en el puerto virtual de origen** y pulse **Aceptar**.


### 4. Configurar el direccionamiento estático de ESXi (Opcional)

La configuración de red para esta guía de arquitectura utiliza un número mínimo de grupos de puertos. Si tiene configurado un grupo de puertos VMkernal para el almacenamiento NFS, deben realizarse unos pasos adicionales. De forma predeterminada, ESXi utilizará el puerto vmkernel que está en la misma subred que el volumen NFS para montar el volumen NFS en el almacenamiento de Resistencia/Rendimiento. Puesto que estamos utilizando el direccionamiento de capa 3 para montar el volumen NFS, debemos forzar a ESXi para que utilice el puerto vmkernel que hemos configurado para montar el volumen NFS. Para ello, debemos crear una ruta estática a la matriz de almacenamiento de Resistencia/Rendimiento.

1. Para configurar una ruta estática, utilice SSH para cada host de ESXi con Rendimiento o Resistencia y ejecute los mandatos siguientes. Tenga en cuenta que debe tener el mandato ping de dirección IP resultante (primer mandato) y utilizarlo con el mandato de red esxcli que se muestra a continuación:
   - `ping <hostname of the endurance/performance array>`

      **Tenga en cuenta** que el almacenamiento NFS y el nombre de host DNS es una Zona de reenvío (FZ) que tiene asignadas múltiples direcciones IP. Estas direcciones IP son estáticas y pertenecen a un nombre de host DNS específico.  Se puede utilizar cualquiera de estas direcciones IP para acceder a un volumen específico.
   - `esxcli network ip route ipv4 add –gateway GATEWAYIP –network <result of ping command>/32`

2. Tenga en cuenta que las rutas estáticas no son persistentes entre rearranques en ESXi 5.0 y versiones anteriores. Para asegurarse de que todas las rutas estáticas añadidas sean persistentes, deben añadirse estos mandatos al archivo local.sh de cada host, ubicado en el directorio /etc/rc.local.d/. Para ello, abra el archivo local.sh con el editor visual y añada el mandato anterior sobre la línea exit 0.
   - Tome nota de la dirección IP, ya que puede utilizarse para montar el volumen en el siguiente paso.
   - Este proceso debe realizarse para cada volumen NFS que tenga previsto montar en el host de ESXi.
   - Este es el enlace a un artículo de VMware KB: [Configuring static routes for VMkernel ports on an ESXi host](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2001426){:new_window}.


##  Montar volúmenes de {{site.data.keyword.filestorage_short}} en los hosts de ESXi

1. Pulse el icono **Ir a vCenter** en la parte superior de la página web y, a continuación, **Hosts y clústeres**.
2. Pulse **Almacenes de datos** en el separador **Objeto relacionado**. Pulse el icono **Crear un nuevo almacén de datos**.
3. En la pantalla **Nuevo almacén de datos**, seleccione la ubicación del almacén de datos (su host de ESXi) y pulse **Siguiente**.
4. En la pantalla **Tipo**, seleccione el botón de opción **NFS** y pulse **Siguiente**.
5. En la pantalla **Nombre y configuración**, escriba el nombre que quiera para el almacén de datos. Adicionalmente, puede especificar el nombre de host del servidor NFS. El uso de FQDN para el servidor NFS mejora la distribución del tráfico al servidor subyacente. La dirección IP también es válida pero se utiliza con menos frecuencia y solo en instancias específicas. Escriba el nombre de carpeta con el formato de /nombre_carpeta.
6. En la pantalla **Accesibilidad de host**, seleccione todos los hosts que desea para montar el almacén de datos NFS y pulse **Siguiente**.
7. Revise las entradas en la siguiente pantalla y pulse **Finalizar**.
8. Repítalo para cada volumen de {{site.data.keyword.filestorage_short}} adicional.

**Nota**: {{site.data.keyword.BluSoftlayer_full}} recomienda utilizar los nombres FQDN para conectar al almacén de datos. La utilización de direcciones IP podría ignorar el mecanismo de equilibrio de carga que se proporciona utilizando FQDN. Para utilizar la dirección IP en lugar de FQDN, ejecute el siguiente mandato para obtener la dirección IP.

  - `ping <hostname of the endurance/performance array>`
  - Desde un host de ESXi:
    ```
    ~ # vmkping nfsdal0902a-fz.service.softlayer.com
    PING nfsdal0902a-fz.service.softlayer.com (10.2.125.80): 56 data bytes
    64 bytes from 10.2.125.80: icmp_seq=0 ttl=253 time=0.187 ms
    10.2.125.80 es la dirección IP asociada al FQDN
    ```

## Habilitar los valores de control de E/S de almacenamiento de ESXi (Opcional)

El control de E/S de almacenamiento (SIOC) es una característica disponible para los clientes que utilizan una licencia Enterprise Plus. Si SIOC está habilitado en el entorno, cambiará la longitud de cola del dispositivo para la única máquina virtual. El cambio en la longitud de cola del dispositivo reduce la cola de matriz de almacenamiento para todas las máquinas virtuales en una proporción igual y regula la cola de almacenamiento. SIOC solo interviene si los recursos están restringidos y la latencia de entrada/salida del almacenamiento está por encima de un umbral definido.


Para que SIOC determine si un dispositivo de almacenamiento está restringido o congestionado, necesita un umbral definido. La latencia del umbral de congestión es diferente para los distintos tipos de almacenamiento; la selección predeterminada es 90% del rendimiento máximo. El porcentaje del valor de rendimiento máximo indica el umbral de latencia estimado cuando el almacén de datos utiliza dicho porcentaje de su rendimiento máximo estimado.


Cabe señalar que configurar incorrectamente SIOC para un almacén o para un VMDK puede afectar significativamente al rendimiento.



### 1. Control de E/S de almacenamiento para un almacén de datos

Efectúe los pasos siguientes para habilitar SIOC con valores recomendados para el almacenamiento de rendimiento y resistencia:

1. Vaya al almacén de datos con el navegador vSphere Web Client.
2. Pulse el separador **Gestionar**.
3. Pulse **Valores** y **General**.
4. Pulse **Editar** para **Funciones del almacén de datos**.
5. Marque el recuadro de selección **Habilitar control de E/S de almacenamiento**.<br/>
   ![NSFDataStore](/images/3_0.png)
6. Pulse **Aceptar**.

**Nota**: Este valor es específico del almacén de datos y no del host.


### 2. Control de E/S de almacenamiento para {{site.data.keyword.BluVirtServers_short}}

También puede limitar los discos virtuales individuales para máquinas virtuales individuales u otorgarles distintas comparticiones con SIOC. Limitar los discos y otorgar distintas comparticiones le permite alinear el entorno a la carga de trabajo con el número de IOPS de volumen de {{site.data.keyword.filestorage_short}} adquirido. El límite se establece por IOPS y es posible establecer un peso diferente o "comparticiones". Los discos virtuales con comparticiones establecidas en Alta (2.000 comparticiones) reciben el doble de E/S que un disco establecido en Normal (1.000 comparticiones) y cuatro veces más que uno establecido en Baja (500 comparticiones). Normal es el valor predeterminado para todas las máquinas virtuales, de modo que solo tendrá que ajustar los valores por encima o por debajo de normal para las máquinas virtuales que así lo requieran.


Efectúe los siguientes pasos para cambiar las comparticiones y el límite de discos virtuales:

1. Elija un {{site.data.keyword.BluVirtServers_short}} en el inventario de **Máquinas virtuales y plantillas**. El icono está ubicado en la esquina superior izquierda de la página web.
2. Seleccione el {{site.data.keyword.BluVirtServers_short}} para el control de E/S.
3. Pulse el separador **Gestionar** y pulse **Valores**. Pulse **Editar**.
4. Amplíe la flecha hacia abajo Disco duro. Modifique las comparticiones o Límite-IOP como requiera su entorno. Elija un disco duro virtual de la lista y modifique la selección de comparticiones para elegir la cantidad relativa de comparticiones que se asignarán al {{site.data.keyword.BluVirtServers_short}} (Baja, Normal o Alta). También puede pulsar **Personalizado** y especificar un valor definido por el usuario.
5. Pulse la columna Límite - IOPS y escriba el límite superior de recursos de almacenamiento que se pueden asignar a la máquina virtual.
6. Pulse **Aceptar**


   **Nota**: el proceso anterior se utiliza para establecer los límites de consumo de recursos de discos virtuales individuales en un {{site.data.keyword.BluVirtServers_short}}, incluso cuando la característica SIOC no está habilitada. Estos valores son específicos del invitado virtual, y no del host, aunque son utilizados por SIOC.





## Valores del lado del host de ESXi

Se requieren algunos valores adicionales para configurar los hosts de ESXi 5.x para el almacenamiento NFS.

|Parámetro | Establecer en... |
|----------|------------|
|Net.TcpipHeapSize |	32 |
|Net.TcpipHeapMax |	Para vSphere 5.0/5.1 establecer 128 <br/> Para vSphere 5.5 o superior establecer 512 |
|NFS.MaxVolumes |	256 |
|NFS41.MaxVolumes |	256 (solo vSphere 6.0 o posterior) |
|NFS.HeartbeatMaxFailures |	10 |
|NFS.HeartbeatFrequency |	12 |
|NFS.HeartbeatTimeout |	5 |
|NFS.MaxQueueDepth|	64 |


- Utilización de CLI en el host de ESXi 5.x para los parámetros de configuración avanzada: :
  Los siguientes ejemplos utilizan la CLI para definir estos parámetros de configuración avanzada y comprobarlos. La herramienta esxcfg-advcfg que se utiliza en los ejemplos se encuentra en el directorio /usr/sbin en los hosts de ESXi 5.x.

   - Definición de los parámetros de configuración avanzada desde ESXi 5.x CLI:
   ```
   #esxcfg-advcfg -s 32 /Net/TcpipHeapSize
   #esxcfg-advcfg -s 128 /Net/TcpipHeapMax(para vSphere 5.0/5.1)
   #esxcfg-advcfg -s 512 /Net/TcpipHeapMax(para vSphere 5.5 y superior)
   #esxcfg-advcfg -s 256 /NFS/MaxVolumes
   #esxcfg-advcfg -s 256 /NFS41/MaxVolumes (ESXi 6.0 y superior)
   #esxcfg-advcfg -s 10 /NFS/HeartbeatMaxFailures
   #esxcfg-advcfg -s 12 /NFS/HeartbeatFrequency
   #esxcfg-advcfg -s 5 /NFS/HeartbeatTimeout
   #esxcfg-advcfg -s 64 /NFS/MaxQueueDepth
   #esxcfg-advcfg -s 32 /Disk/QFullSampleSize
   #esxcfg-advcfg -s 8 /Disk/QFullThreshold
   ```

- Comprobación de los parámetros de configuración avanzada desde ESXi 5.x CLI:
   ```
   #esxcfg-advcfg -g /Net/TcpipHeapSize
   #esxcfg-advcfg -g /Net/TcpipHeapMax
   #esxcfg-advcfg -g /NFS/MaxVolumes
   #esxcfg-advcfg -g /NFS41/MaxVolumes (ESXi 6.0 y superior)
   #esxcfg-advcfg -g /NFS/HeartbeatMaxFailures
   #esxcfg-advcfg -g /NFS/HeartbeatFrequency
   #esxcfg-advcfg -g /NFS/HeartbeatTimeout
   #esxcfg-advcfg -g /NFS/MaxQueueDepth
   #esxcfg-advcfg -g /Disk/QFullSampleSize
   #esxcfg-advcfg -g /Disk/QFullThreshold
   ```


## Habilitar tramas Jumbo en SoftLayer – Windows y Linux

{{site.data.keyword.BluSoftlayer_full}} ha indicado que para alcanzar las velocidades en el almacenamiento, la trama Jumbo debe estar habilitada a 9.000 MTU.


Una trama Jumbo es una trama de Ethernet con una carga útil superior a la unidad de transmisión máxima (MTU) estándar de 1.500 bytes. Las tramas Jumbo se utilizan en redes de área local que dan soporte como mínimo a 1 Gbps y pueden alcanzar un tamaño de 9.000 bytes.


Las tramas Jumbo deben configurarse igual en toda la vía de acceso de la red desde el dispositivo de origen <-> conmutador <-> direccionador <-> conmutador <-> dispositivo de destino. Si toda la cadena no se define igual, se establecerá el valor predeterminado más bajo en toda la cadena. SoftLayer tiene sus dispositivos de red establecidos en 9.000. Todos los dispositivos del cliente deberán establecerse en el mismo valor.

### Windows

1. Abra el **Centro de redes y recursos compartidos**.
2. Pulse **Cambiar configuración del adaptador**.
3. Pulse con el botón derecho del ratón el NIC para el cual desea habilitar las tramas Jumbo y seleccione **Propiedades**.
4. En el separador **Redes**, pulse **Configurar** para el adaptador de red.
5. Seleccione el separador **Avanzado**.
6. Seleccione **Trama Jumbo** y cambie el valor de inhabilitado al valor deseado, como MTU de 9 kB o 9.014 Bytes, en función del NIC.
7. Pulse **Aceptar** para cerrar todos los diálogos.

Tenga en cuenta que cuando se realice el cambio, el NIC perderá la conectividad de red durante unos segundos. Deberá rearrancar para asegurarse de que el cambio se aplique.



### LINUX

1. Edite el archivo de configuración de red para la interfaz eth0 – por ejemplo, /etc/sysconfig/network-script/ifcfg-eth0 (CentOS / RHEL / Fedora Linux):
`# vi /etc/sysconfig/network-script/ifcfg-eth0`

2. Añada la siguiente directiva de configuración, que especifica el tamaño de la trama en bytes:
MTU 9000

3. Cierre y guarde el archivo. Reinicie la interfaz eth0:
`# /etc/init.d/networking restart (esto provocará una breve perdida de la conectividad de red)`


Nota sobre el usuario de Debian / Ubuntu Linux:
El usuario de Debian / Ubuntu Linux deberá añadir MTU=9000 al archivo de configuración /etc/network/interfaces.

Obtenga más información sobre la Arquitectura avanzada de referencia de VMware de un solo sitio [aquí](https://console.bluemix.net/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){:new_window}.
