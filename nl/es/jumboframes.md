---

copyright:
  years: 2014, 2019
lastupdated: "2019-08-01"

keywords: File Storage, file storage, NSF, networking, jumbo frames

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Habilitación de tramas Jumbo en {{site.data.keyword.BluSoftlayer_notm}}
{: #jumboframes}

Una trama Jumbo es una trama de Ethernet con una carga útil superior a la unidad de transmisión máxima (MTU) estándar de 1.500 bytes. Las tramas Jumbo se utilizan en redes de área local que dan soporte como mínimo a 1 Gbps y pueden alcanzar un tamaño de 9.000 bytes.

Las tramas Jumbo deben configurarse igual en toda la vía de acceso de la red desde el dispositivo de origen <-> conmutador <-> direccionador <-> conmutador <-> dispositivo de destino. Si toda la cadena no se define igual, se establece el valor predeterminado más bajo en toda la cadena. {{site.data.keyword.cloud}} tiene los dispositivos de red establecidos en 9.000. Todos los dispositivos del cliente deben establecerse en el mismo valor de 9.000.
{:important}


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
   {:important}
