---

copyright:
  years: 2014, 2018
lastupdated: "2018-12-11"

---
{:pre: .pre}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Habilitación de tramas Jumbo en {{site.data.keyword.BluSoftlayer_notm}} para Windows y Linux

Una trama Jumbo es una trama de Ethernet con una carga útil superior a la unidad de transmisión máxima (MTU) estándar de 1.500 bytes. Las tramas Jumbo se utilizan en redes de área local que dan soporte como mínimo a 1 Gbps y pueden alcanzar un tamaño de 9.000 bytes.

Las tramas Jumbo deben configurarse igual en toda la vía de acceso de la red desde el dispositivo de origen <-> conmutador <-> direccionador <-> conmutador <-> dispositivo de destino. Si toda la cadena no se define igual, se establece el valor predeterminado más bajo en toda la cadena. {{site.data.keyword.BluSoftlayer_full}} tiene los dispositivos de red establecidos en 9.000. Todos los dispositivos del cliente deben establecerse en el mismo valor de 9.000.
{:important}

## Habilitación de tramas Jumbo en Windows

1. Abra el **Centro de redes y recursos compartidos**.
2. Pulse **Cambiar configuración del adaptador**.
3. Pulse con el botón derecho del ratón el NIC para el cual desea habilitar las tramas Jumbo y seleccione **Propiedades**.
4. En el separador **Redes**, pulse **Configurar** para el adaptador de red.
5. Seleccione el separador **Avanzado**.
6. Seleccione **Trama Jumbo** y cambie el valor de **inhabilitado** al valor que desee. El valor, como 9 kB o 9014 Bytes, depende del NIC.
7. Pulse **Aceptar** en todas las ventanas.

Cuando realice el cambio, el NIC pierde la conectividad de red durante unos segundos. Reinicie el dispositivo para confirmar que el cambio ha entrado en vigor.
{:tip}


## Habilitación de tramas Jumbo en Linux

1. Edite el archivo de configuración de red para la interfaz eth0.
   - Los usuarios de CentOS, RHEL y Fedora Linux deben editar `/etc/sysconfig/network-script/ifcfg-eth0`
     ```
     # vi /etc/sysconfig/network-script/ifcfg-eth0
     ```
     {: pre}

   - Los usuarios de Debian y Ubuntu Linux deben editar `/etc/network/interfaces`.

2. Añada la siguiente directiva de configuración, que especifica el tamaño de la trama en bytes.
   - CentOS, RHEL, Fedora Linux
     ```
     MTU 9000
     ```
     {: pre}

   - Debian y Ubuntu Linux
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
