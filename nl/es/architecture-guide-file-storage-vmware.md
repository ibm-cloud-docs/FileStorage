---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-31"

---
{:pre: .pre}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Suministro de {{site.data.keyword.filestorage_short}} con VMware

Los siguientes pasos le ayudan a realizar el pedido y configurar {{site.data.keyword.filestorage_full}} en un entorno de vSphere 5.5 y vSphere 6.0 en {{site.data.keyword.BluSoftlayer_full}}.

{{site.data.keyword.filestorage_short}} está diseñado para dar soporte a aplicaciones de entrada/salida alta que requieren niveles de rendimiento previsibles. El rendimiento previsible se consigue mediante la asignación de operaciones de entrada/salida a nivel de protocolo por segundo (IOPS) para volúmenes individuales.

Si necesita más de ocho conexiones con el host VMware, recomendamos que elija {{site.data.keyword.filestorage_short}} NFS.
{:tip}

El acceso y el montaje de la oferta de {{site.data.keyword.filestorage_short}} se realiza mediante una conexión NFS. En un despliegue de VMware, se puede montar un único volumen para hasta 64 hosts ESXi como almacenamiento compartido. También puede montar múltiples volúmenes para crear un clúster de almacenamiento que utilice DRS (Distributed Resource Scheduler) de almacenamiento de vSphere.

Las opciones de precio y configuración para el {{site.data.keyword.filestorage_short}} de Resistencia y Rendimiento se facturan basándose en una combinación del espacio reservado y las IOPS ofrecidas.

## Consideraciones para el pedido

Al realizar el pedido de {{site.data.keyword.filestorage_short}}, tenga en cuenta la información siguiente:

- Al decidir sobre el tamaño, tenga en cuenta el tamaño de la carga de trabajo y el rendimiento necesario. El tamaño es importante con el servicio Resistencia, que escala el rendimiento de forma lineal en relación con la capacidad (IOPS/GB). Por el contrario, el servicio Rendimiento permite al administrador elegir la capacidad y el rendimiento de forma independiente. Los requisitos de rendimiento son importantes con el servicio Rendimiento.

  El cálculo del rendimiento es IOPS x 16 KB. Las IOPS se miden basándose en un tamaño de bloque de 16 KB con una combinación de lectura/escritura 50/50.<br/>Al aumentar el tamaño de bloque, se aumenta el rendimiento pero se reducen las IOPS. Por ejemplo, duplicar el tamaño de bloque a bloques de 32 KB mantiene el rendimiento máximo pero reduce a la mitad las IOPS.
  {:note}
- NFS utiliza muchas operaciones de control de archivos adicionales, como `lookup`, `getattr` y `readdir`. Estas operaciones, junto con las operaciones de lectura/escritura, pueden contar como IOPS y varían según el tipo de operación y la versión de NFS.
- Los volúmenes de {{site.data.keyword.filestorage_short}} están expuestos a dispositivos autorizados, subredes o direcciones IP.
- Para evitar la desconexión del almacenamiento durante una migración tras error de vía de acceso, {{site.data.keyword.IBM}} recomienda la instalación de herramientas VMware, que establecen un valor de tiempo de espera adecuado. No es necesario cambiar el valor, el valor predeterminado es suficiente para garantizar que el host de VMware no pierda la conectividad.
- Tanto NFSv3 como NFSv4.1 están soportados en el entorno de {{site.data.keyword.BluSoftlayer_full}}. Sin embargo, {{site.data.keyword.IBM}} sugiere que utilice NFSv3. Como NFSv4.1 es un protocolo con estado (no sin estado como NFSv3), se pueden producir problemas de protocolo durante sucesos de red. NFSv4.1 debe desactivar temporalmente todas las operaciones y realizar la reclamación de bloqueo. Durante estas operaciones, pueden producirse interrupciones.

Para obtener más información, consulte el documento técnico de VMware en [Mejores prácticas para ejecutar VMware vSphere en el almacenamiento adjunto en red](https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/techpaper/vmware-nfs-bestpractices-white-paper-en.pdf){:new_window}
{:tip}

**Matriz de soporte de características VMware de protocolo NFS**
<table>
  <caption>La Tabla 1 muestra cómo se aplican las características de vSphere a las dos versiones diferentes de NFS.</caption>
 <thead>
  <tr>
   <th>Características de vSphere</th>
   <th>NFS versión 3</th>
   <th>NFS versión 4.1</th>
  </tr>
 </thead>
 <tbody>
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
*Fuente - [VMware - NFS Protocols and ESXi](https://docs.vmware.com/en/VMware-vSphere/6.0/com.vmware.vsphere.storage.doc/GUID-8A929FE4-1207-4CC5-A086-7016D73C328F.html){:new_window}*



### Uso de instantáneas

El {{site.data.keyword.filestorage_short}} permite a los administradores establecer planificaciones de instantáneas que crean y suprimen copias de instantáneas automáticamente para cada volumen de almacenamiento. También pueden crear planificaciones de instantáneas adicionales (cada hora, diariamente, semanalmente) para instantáneas automáticas y crear instantáneas ad manualmente para casos de continuidad del negocio y recuperación tras desastre (BCDR). Las alertas automáticas se envían a través del [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} al propietario del volumen para las instantáneas retenidas y el espacio utilizado.

Se necesita espacio de instantáneas para utilizar las instantáneas. El espacio se puede adquirir durante el pedido del volumen inicial o después del suministro inicial mediante la página **Detalles del volumen**, pulsando **Acciones** y seleccionando **Añadir espacio de instantáneas**.

Es importante señalar que los entornos de VMware no son conscientes de las instantáneas. La capacidad de instantáneas de {{site.data.keyword.filestorage_short}} de Resistencia no debe confundirse con las instantáneas de VMware. Toda recuperación que utilice la característica de instantáneas de {{site.data.keyword.filestorage_short}} debe manejarse desde el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

Restaurar el volumen de {{site.data.keyword.filestorage_short}} requiere apagar todas las máquinas virtuales del {{site.data.keyword.filestorage_short}}. El volumen se debe desmontar temporalmente de los hosts de ESXi para evitar la corrupción de datos durante el proceso.

Para obtener más detalles sobre configurar instantáneas, consulte el artículo sobre [instantáneas](snapshots.html).


### Utilización de la réplica

La réplica utiliza una de sus planificaciones de instantáneas para copiar automáticamente las instantáneas a un volumen de destino de un centro de datos remoto. Las copias se pueden recuperar en el sitio remoto si se produce un suceso catastrófico o los datos resultan dañados.

Con las réplicas, puede

- Recuperarse rápidamente de fallos del sitio y otros desastres realizando la migración al volumen de destino
- Realizar la migración tras error a un punto específico en el tiempo en la copia de recuperación tras desastre

La réplica mantiene sus datos sincronizados entre dos ubicaciones distintas. Si desea clonar el volumen y utilizarlo independientemente del volumen original, consulte [Creación de un volumen de archivos duplicado](how-to-create-duplicate-volume.html).
{:tip}

Antes de poder replicar, debe crear una planificación de instantáneas.

Cuando realiza la migración tras error, está “cambiando el interruptor” de su volumen de almacenamiento del centro de datos primario al volumen de destino del centro de datos remoto. Por ejemplo, su centro de datos primario está en Londres y el centro de datos secundario está en Ámsterdam. Si se produce un suceso de error, realizaría la migración a Ámsterdam. Esto significa conectar al ahora volumen primario desde una instancia de clúster de vSphere en Ámsterdam. Cuando su volumen de Londres se haya reparado, se realizará una instantánea del volumen de Ámsterdam. Luego puede volver a Londres y al volumen primario de nuevo desde una instancia de cálculo de Londres.

Antes de que el volumen vuelva al centro de datos primario, debe dejar de ser utilizado en el sitio remoto. Se realiza una instantánea de cualquier información nueva o modificada y se replica al centro de datos primario antes de poder montarse de nuevo en los hosts de ESXi del sitio de producción.

Para obtener más información sobre la configuración de réplicas, consulte [Réplica](replication.html).

Los datos no válidos, ya estén corruptos, pirateados o infectados, se replican de acuerdo con la planificación de instantáneas y la retención de instantáneas. El uso de la ventana de réplica más pequeña puede proporcionar un mejor objetivo de punto de recuperación, pero también puede reducir el tiempo de reacción a la réplica de datos no válidos.
{:note}


## Solicitud de {{site.data.keyword.filestorage_short}}

Puede realizar el pedido y configurar {{site.data.keyword.filestorage_short}} para un entorno de VMware ESXi 5. Utilice la información siguiente junto con la [Arquitectura avanzada de referencia de VMware de un solo sitio](https://console.bluemix.net/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){:new_window} para establecer una de estas opciones de almacenamiento en el entorno de VMware.

Se puede realizar el pedido de {{site.data.keyword.filestorage_short}} a través del [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, accediendo a la página {{site.data.keyword.filestorage_short}} a través de **Almacenamiento** > **{{site.data.keyword.filestorage_short}}**.


### 1. Realizar el pedido de 

Efectúe los siguientes pasos para realizar un pedido de {{site.data.keyword.filestorage_short}}:
1. Pulse **Almacenamiento** > **{{site.data.keyword.filestorage_short}}** en la página de inicio de [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
2. Pulse **Realizar pedido de {{site.data.keyword.filestorage_short}}** en la página de **{{site.data.keyword.filestorage_short}}**.
3. Seleccione **Resistencia**/**Rendimiento** en la lista **Seleccionar tipo de almacenamiento**.
4. Seleccione la ubicación. Los centros de datos con las prestaciones mejoradas están marcados con un asterisco. Asegúrese de que el nuevo almacenamiento se añada en la misma ubicación que el host de ESXi pedido anteriormente.
5. Seleccione el método de facturación. Están disponibles las opciones de facturación mensual y por horas.
6. Seleccione la cantidad de espacio de almacenamiento en GB. Para TB, 1 TB equivale a 1.000 GB, y 12 TB equivale a 12.000 GB.
7. Especifique la cantidad de IOPS en intervalos de 100 o seleccione un nivel de IOPS.
8. Especifique el tamaño del espacio de instantáneas.
9. Pulse **Continuar**.
10. Escriba un código promocional si tiene uno, y pulse **Recalcular**.
11. Revise el pedido.
12. Marque el recuadro de selección **He leído el Acuerdo de servicio maestro y acepto sus condiciones**.
13. Pulse **Realizar pedido** para enviar el pedido, o **Cancelar** para cerrar el formulario sin enviar ningún pedido.

El almacenamiento se suministra en menos de un minuto y pasa a estar visible en la página de **{{site.data.keyword.filestorage_short}}** del [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.



### 2. Autorización de los hosts para el uso de {{site.data.keyword.filestorage_short}}

Una vez suministrado el volumen, los {{site.data.keyword.BluBareMetServers_full}} o {{site.data.keyword.BluVirtServers_full}} que utilizarán el volumen necesitarán autorización para acceder al almacenamiento. Utilice los siguientes pasos para autorizar el volumen:

1. Pulse **Almacenamiento** > **{{site.data.keyword.filestorage_short}}**.
2. Seleccione **Acceder a host** en el menú **Acciones de volumen de rendimiento** o **Resistencia**.
3. Pulse **Subredes**.
4. Elija de la lista de subredes disponibles que están asignadas a los puertos VMkernel en los hosts de ESXi y pulse **Enviar**.<br/>

   Las subredes que se muestran son subredes suscritas al mismo centro de datos que el volumen de almacenamiento.
   {:note}


Una vez autorizadas las subredes, anote el nombre de host del servidor de almacenamiento de Resistencia o Rendimiento que desea utilizar al montar el volumen. Esta información puede encontrarse en la página de detalles de {{site.data.keyword.filestorage_short}} pulsando sobre un volumen específico.


##  Configuración del host de máquina virtual de VMware

### Cumplimiento de los requisitos previos

Antes de empezar el proceso de configuración de VMware, asegúrese de que se cumplen los siguientes requisitos previos:

- Los {{site.data.keyword.BluBareMetServers}} con VMware ESXi se suministran con la configuración de almacenamiento adecuada y credenciales de inicio de sesión de ESXi.
- {{site.data.keyword.BluSoftlayer_full}} Windows físico o {{site.data.keyword.virtualmachinesshort}} en el mismo centro de datos que los {{site.data.keyword.BluBareMetServers}}. Incluyendo la dirección IP pública de la máquina virtual de {{site.data.keyword.BluSoftlayer_full}} Windows y los credenciales de inicio de sesión.
- Un sistema con acceso a Internet, y con el software de navegador web y un cliente de Remote Desktop Protocol (RDP) instalados.


### 1. Configuración del host de VMware.

1. Desde un sistema conectado a Internet, inicie un cliente RDP y establezca una sesión RDP a los {{site.data.keyword.BluVirtServers_full}} suministrados en el mismo centro de datos donde está instalado vSphere vCenter.
2. Desde los {{site.data.keyword.BluVirtServers_short}}, inicie un navegador web y conéctese a VMware vCenter a través de vSphere Web Client.
3. Desde la pantalla **Inicio**, seleccione **Hosts y clústeres**. Amplíe el panel de la izquierda y seleccione el **servidor de VMware ESXi** que se utilizará para este despliegue.
4. Asegúrese de que el puerto de cortafuegos para el cliente NFS esté abierto en todos los hosts para que pueda configurar el cliente NFS en el host de vSphere. Se abre automáticamente en las versiones más recientes de vSphere. Para comprobar si el puerto está abierto, vaya al separador **Gestionar host de ESXi** en VMware® vCenter™, seleccione **Valores** y, a continuación, **Perfil de seguridad**. En la sección **Cortafuegos**, pulse **Editar** y desplácese a **Cliente NFS**.
5. Asegúrese de que esté marcado **Permitir la conexión desde cualquier dirección IP o una lista de direcciones IP**. <br/>
   ![Permitir conexión](/images/1_4.png)
6. Configure las tramas Jumbo desde el separador **Gestionar host de ESXi**, seleccionando **Gestionar** y, a continuación, **Redes**.
7. Seleccione **Adaptadores VMkernel**, resalte **vSwitch** y pulse **Editar** (icono de lápiz).
8. Seleccione **Configuración de NIC** y asegúrese de que la MTU de NIC tenga un valor de 9000.
9. **Opcional**. Valide la configuración de las tramas Jumbo.
   - Desde Windows: `ping -f -l 8972 a.b.c.d`
   - Desde UNIX: `ping -s 8972 a.b.c.d`
     Donde a.b.c.d es la interfaz de {{site.data.keyword.BluVirtServers_short}} contigua.
     Ejemplo
     ```
     ping a.b.c.d (a.b.c.d) 8972(9000) bytes of data.
     8980 bytes from a.b.c.d: icmp_seq=1 ttl=128 time=3.36 ms
     ```

Para obtener más información sobre VMware y las tramas Jumbo, pulse [aquí](https://kb.vmware.com/s/article/1003712){:new_window}.
{:tip}


### 2. Adición de un adaptador de enlace ascendente a un conmutador virtual

1. Configure un nuevo adaptador de enlace ascendente desde el separador **Gestionar host de ESXi**, seleccionando **Gestionar** y, a continuación, **Redes**.
2. Seleccione el separador **Adaptadores físicos**.
3. Pulse **Añadir redes de host** (icono de globo con un signo más).
4. Seleccione el tipo de conexión como **Adaptador de red física** y pulse **Siguiente**.
5. Seleccione el **vSwitch** existente y pulse **Siguiente**.
6. Seleccione **Adaptadores no utilizados** y pulse **Añadir adaptadores** (signo más).
7. Pulse el otro adaptador "Conectado" y pulse **Aceptar**. <br/>
   ![Añadir adaptadores físicos a un conmutador](/images/2_3.png)
8. Pulse **Siguiente** y **Finalizar**.
9. Vuelva al separador **Conmutadores virtuales** y seleccione **Editar configuración** (icono de lápiz) situado bajo la cabecera **Conmutadores virtuales**.
10. En la izquierda, seleccione la entrada **Equipo y migración tras error** de vSwitch.
11. Verifique que la opción **Equilibrio de carga** esté establecida en **Ruta basada en el puerto virtual de origen** y pulse **Aceptar**.


### 3. Configuración del direccionamiento estático de ESXi (Opcional)

La configuración de red para esta guía de arquitectura utiliza un número mínimo de grupos de puertos. Si tiene un grupo de puertos VMkernel para el almacenamiento NFS, se deben realizar pasos adicionales. De forma predeterminada, ESXi utiliza el puerto VMkernel que está en la misma subred que el volumen NFS para montar el volumen NFS en el almacenamiento de Resistencia/Rendimiento. Puesto se está utilizando el direccionamiento de capa 3 para montar el volumen NFS, se debe forzar a ESXi para que utilice el puerto VMkernel que se ha configurado para montar el volumen NFS. Para ello, se debe crear una ruta estática a la matriz de almacenamiento de Resistencia/Rendimiento.

1. Para configurar una ruta estática, utilice SSH para cada host de ESXi con Rendimiento o Resistencia y ejecute los mandatos siguientes. Anote la dirección IP de la salida del mandato `ping` (primer mandato) y utilícela con el mandato de red `esxcli`.
   ```
   ping <nombre de host de la matriz de resistencia/rendimiento>
   ```
   {: pre}

   >**Nota**: El almacenamiento NFS y el nombre de host DNS es una Zona de reenvío (FZ) que tiene asignadas múltiples direcciones IP. Estas direcciones IP son estáticas y pertenecen a un nombre de host DNS específico. Se puede utilizar cualquiera de estas direcciones IP para acceder a un volumen específico.

   ```
   esxcli network ip route ipv4 add –gateway GATEWAYIP –network <resultado del mandato ping>/32
   ```
   {: pre}

2. Las rutas estáticas no son persistentes entre rearranques en ESXi 5.0 y versiones anteriores. Para asegurarse de que todas las rutas estáticas añadidas sigan siendo persistentes, debe añadirse este mandato al archivo `local.sh` de cada host, que está ubicado en el directorio `/etc/rc.local.d/`. Abra el archivo `local.sh` con el editor visual y añada el segundo mandato del Paso 4.1., delante de la línea `exit 0`.

Tome nota de la dirección IP, ya que puede utilizarse para montar el volumen en el siguiente paso.<br/>Este proceso debe realizarse para cada volumen NFS que tenga previsto montar en el host de ESXi.<br/>Para obtener más información, consulte el artículo de VMware KB [Configuring static routes for VMkernel ports on an ESXi host](https://kb.vmware.com/s/article/2001426){:new_window}.
{:tip}


##  Creación y montaje de un volumen de {{site.data.keyword.filestorage_short}} en los hosts de ESXi

1. Pulse el icono **Ir a vCenter** y, a continuación, **Hosts y clústeres**.
2. En el separador **Objeto relacionado**, pulse **Almacenes de datos**.
3. Pulse el icono **Crear un nuevo almacén de datos**.
4. En la pantalla **Nuevo almacén de datos**, seleccione la ubicación del almacén de datos de VMware (su host de ESXi) y pulse **Siguiente**.
5. En la pantalla **Tipo**, seleccione **NFS** y pulse **Siguiente**.
6. A continuación seleccione la versión de NFS. Tanto NFSv3 como NFSv4.1 están soportados, pero es preferible NFSv3. Asegúrese de utilizar solamente una versión de NFS para acceder a un almacén de datos determinado. Montar uno o más hosts al mismo almacén de datos utilizando diferentes versiones de NFS puede provocar corrupción de datos.
7. En la pantalla **Nombre y configuración**, escriba el nombre que desee para el almacén de datos de VMware. Adicionalmente, puede especificar el nombre de host del servidor NFS. El uso de FQDN para el servidor NFS mejora la distribución del tráfico al servidor subyacente. La dirección IP también es válida pero se utiliza con menos frecuencia y solo en instancias específicas. Escriba el nombre de carpeta con el formato `/nombre_carpeta`.
8. En la pantalla **Accesibilidad de host**, seleccione uno o más hosts en los que desea montar el almacén de datos de VMware NFS y pulse **Siguiente**.
9. Revise las entradas en la siguiente pantalla y pulse **Finalizar**.
10. Repítalo para cada volumen de {{site.data.keyword.filestorage_short}} adicional.

{{site.data.keyword.BluSoftlayer_full}} recomienda utilizar los nombres FQDN para conectar al almacén de datos de VMware. La utilización de direcciones IP podría ignorar el mecanismo de equilibrio de carga que se proporciona utilizando FQDN.
{:important}

Para utilizar la dirección IP en lugar de FQDN, ejecute un ping al servidor para obtener la dirección IP.
```
ping <nombre de host de la matriz de resistencia/rendimiento>
```
{: pre}

Para obtener la dirección IP de un host de ESXi, utilice el siguiente mandato. El valor resultante, 10.2.125.80, es la IP con FQDN.
```
~ # vmkping nfsdal0902a-fz.service.softlayer.com
PING nfsdal0902a-fz.service.softlayer.com (10.2.125.80): 56 data bytes
64 bytes from 10.2.125.80: icmp_seq=0 ttl=253 time=0.187 ms
```


## Habilitación del control de E/S de almacenamiento de ESXi (Opcional)

El control de E/S de almacenamiento (SIOC) es una característica disponible para los clientes que utilizan una licencia Enterprise Plus. Si SIOC está habilitado en el entorno, cambia la longitud de cola del dispositivo para la única máquina virtual. El cambio en la longitud de cola del dispositivo reduce la cola de matriz de almacenamiento para todas las máquinas virtuales en una proporción igual. SIOC solo interviene si los recursos están restringidos y la latencia de entrada/salida del almacenamiento está por encima de un umbral definido.


Para que SIOC determine si un dispositivo de almacenamiento está restringido o congestionado, necesita un umbral definido. La latencia del umbral de congestión es diferente para los distintos tipos de almacenamiento. La selección predeterminada es 90% del rendimiento máximo. El porcentaje del valor de rendimiento máximo indica el umbral de latencia estimado cuando el almacén de datos de VMware utiliza dicho porcentaje de su rendimiento máximo estimado.


Configurar incorrectamente SIOC para un almacén de datos de VMware o para un VMDK puede afectar significativamente al rendimiento.
{:important}


### Configuración del control de E/S de almacenamiento para un almacén de datos de VMware

Efectúe los pasos siguientes para habilitar SIOC con valores recomendados para el almacenamiento de rendimiento y resistencia:

1. Vaya al almacén de datos de VMware con el navegador vSphere Web Client.
2. Pulse el separador **Gestionar**.
3. Pulse **Valores** y **General**.
4. Pulse **Editar** para **Funciones del almacén de datos**.
5. Marque el recuadro de selección **Habilitar control de E/S de almacenamiento**.<br/>
   ![Almacén de datos de VMware NSF](/images/3_0.png)
6. Pulse **Aceptar**.

Este valor es específico del almacén de datos de VMware y no del host.
{:note}


### Configuración del control de E/S de almacenamiento para {{site.data.keyword.BluVirtServers_short}}

También puede limitar los discos virtuales individuales para máquinas virtuales individuales u otorgarles distintas comparticiones con SIOC. Limitar los discos y otorgar distintas comparticiones le permite alinear el entorno a la carga de trabajo con el número de IOPS de volumen de {{site.data.keyword.filestorage_short}} adquirido. El límite se establece por IOPS y es posible establecer un peso diferente o "comparticiones". Los discos virtuales con comparticiones establecidas en Alta (2.000 comparticiones) reciben el doble de E/S que un disco establecido en Normal (1.000 comparticiones) y cuatro veces más que uno establecido en Baja (500 comparticiones). Normal es el valor predeterminado para todas las máquinas virtuales, de modo que debe ajustar los valores por encima o por debajo de normal para las máquinas virtuales que así lo requieran.

Efectúe los siguientes pasos para cambiar las comparticiones y el límite de discos virtuales.

1. Elija un {{site.data.keyword.BluVirtServers_short}} en el inventario de **Máquinas virtuales y plantillas**.
2. Seleccione el {{site.data.keyword.BluVirtServers_short}} para el control de E/S.
3. Pulse el separador **Gestionar** y pulse **Valores**. Pulse **Editar**.
4. Amplíe la flecha de **disco duro**. Seleccione **Modificar las comparticiones** o **Límite - IOPS** como requiera su entorno. Elija un disco duro virtual de la lista y modifique la selección de comparticiones para elegir el número relativo de comparticiones que se asignarán al {{site.data.keyword.BluVirtServers_short}} (Baja, Normal o Alta). También puede pulsar **Personalizado** y especificar un valor definido por el usuario.
5. Pulse la columna **Límite - IOPS** y escriba el límite superior de recursos de almacenamiento que se pueden asignar a la máquina virtual.
6. Pulse **Aceptar**


Este proceso se utiliza para establecer los límites de consumo de recursos de discos virtuales individuales en un {{site.data.keyword.BluVirtServers_short}}, incluso cuando la característica SIOC no está habilitada. Estos valores son específicos del invitado virtual, y no del host, aunque son utilizados por SIOC.
{:important}


## Configuración de los valores del lado del host de ESXi

Se requieren valores adicionales para configurar los hosts de ESXi 5.x para el almacenamiento NFS. Esta tabla muestra los valores de configuración.

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


### Actualización de los parámetros de configuración avanzada en un host de ESXi 5.x utilizando la CLI

Los siguientes ejemplos utilizan la CLI para definir los parámetros de configuración avanzada y comprobarlos. La herramienta `esxcfg-advcfg` que se utiliza en los ejemplos se encuentra en el directorio `/usr/sbin` en los hosts de ESXi 5.x.

   - Definición de los parámetros de configuración avanzada desde ESXi 5.x CLI.

     ```
     #esxcfg-advcfg -s 32 /Net/TcpipHeapSize
     #esxcfg-advcfg -s 128 /Net/TcpipHeapMax(For vSphere 5.0/5.1)
     #esxcfg-advcfg -s 512 /Net/TcpipHeapMax(For vSphere 5.5 and above)
     #esxcfg-advcfg -s 256 /NFS/MaxVolumes
     #esxcfg-advcfg -s 256 /NFS41/MaxVolumes (ESXi 6.0 and above)
     #esxcfg-advcfg -s 10 /NFS/HeartbeatMaxFailures
     #esxcfg-advcfg -s 12 /NFS/HeartbeatFrequency
     #esxcfg-advcfg -s 5 /NFS/HeartbeatTimeout   
     #esxcfg-advcfg -s 64 /NFS/MaxQueueDepth
     #esxcfg-advcfg -s 32 /Disk/QFullSampleSize
     #esxcfg-advcfg -s 8 /Disk/QFullThreshold
     ```

  - Comprobación de los parámetros de configuración avanzada desde ESXi 5.x CLI.

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

## Habilitación de tramas Jumbo en {{site.data.keyword.BluSoftlayer_notm}} para Windows y Linux

Una trama Jumbo es una trama de Ethernet con una carga útil superior a la unidad de transmisión máxima (MTU) estándar de 1.500 bytes. Las tramas Jumbo se utilizan en redes de área local que dan soporte como mínimo a 1 Gbps y pueden alcanzar un tamaño de 9.000 bytes.

Las tramas Jumbo deben configurarse igual en toda la vía de acceso de la red desde el dispositivo de origen <-> conmutador <-> direccionador <-> conmutador <-> dispositivo de destino. Si toda la cadena no se define igual, se establece el valor predeterminado más bajo en toda la cadena. {{site.data.keyword.BluSoftlayer_full}} tiene los dispositivos de red establecidos en 9.000. Todos los dispositivos del cliente deben establecerse en el mismo valor de 9.000.
{:important}

### Habilitación de tramas Jumbo en Windows

1. Abra el **Centro de redes y recursos compartidos**.
2. Pulse **Cambiar configuración del adaptador**.
3. Pulse con el botón derecho del ratón el NIC para el cual desea habilitar las tramas Jumbo y seleccione **Propiedades**.
4. En el separador **Redes**, pulse **Configurar** para el adaptador de red.
5. Seleccione el separador **Avanzado**.
6. Seleccione **Trama Jumbo** y cambie el valor de **inhabilitado** al valor que desee. El valor, como 9 kB o 9014 Bytes, depende del NIC.
7. Pulse **Aceptar** en todas las ventanas.

Cuando realice el cambio, el NIC pierde la conectividad de red durante unos segundos. Reinicie el dispositivo para confirmar que el cambio ha entrado en vigor.
{:tip}


### Habilitación de tramas Jumbo en Linux

1. Edite el archivo de configuración de red para la interfaz eth0.
   - Los usuarios de CentOS/RHEL/Fedora Linux deben editar `/etc/sysconfig/network-script/ifcfg-eth0`
     ```
     # vi /etc/sysconfig/network-script/ifcfg-eth0
     ```
     {: pre}

   - Los usuarios Debian/Ubuntu Linux deben editar `/etc/network/interfaces`.

2. Añada la siguiente directiva de configuración, que especifica el tamaño de la trama en bytes.
   - CentOS/RHEL/Fedora Linux
     ```
     MTU 9000
     ```
     {: pre}

   - Debian/Ubuntu Linux
     ```
     MTU=9000
     ```
     {: pre}

3. Cierre y guarde el archivo. Reinicie la interfaz eth0.
   ```
   # /etc/init.d/networking restart
   ```
   {: pre}

   Esta acción causa una breve pérdida de conexión de red.

Obtenga más información sobre la Arquitectura avanzada de referencia de VMware de un solo sitio [aquí](https://console.bluemix.net/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){:new_window}.
{:tip}
