---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, SLCLI, provisioning, API

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Solicitud de {{site.data.keyword.filestorage_short}} mediante SLCLI
{: #orderingSLCLI}

Puede utilizar SLCLI para realizar pedidos de productos que normalmente se solicitan a través del [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}. En la API de SL, un pedido puede consistir en varios contenedores de pedidos. La CLI de pedidos funciona con un solo contenedor de pedidos.

Para obtener más información sobre cómo instalar y utilizar SLCLI, consulte [Cliente de API de Python](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.
{:tip}

## Búsqueda de las ofertas disponibles de {{site.data.keyword.filestorage_short}}

El primer componente que se debe buscar cuando se realiza un pedido es un paquete. Los paquetes se dividen entre los distintos productos de nivel superior que están disponibles para solicitar en {{site.data.keyword.BluSoftlayer_full}}. Algunos paquetes de ejemplo son CLOUD_SERVER para VSI, BARE_METAL_SERVER para servidores nativos y STORAGE_AS_A_SERVICE_STAAS para {{site.data.keyword.filestorage_short}} y {{site.data.keyword.blockstorageshort}}.

Dentro de un paquete, algunos elementos se subdividen en categorías. Algunos paquetes tienen elementos preestablecido para su comodidad y en otros es necesario especificar los elementos individualmente. Si se necesita una categoría de un paquete, se debe seleccionar un elemento de dicha categoría para solicitar el paquete. Dependiendo de la categoría, algunos elementos dentro de la categoría pueden ser mutuamente excluyentes.

Cada pedido debe tener una ubicación asociada (centro de datos). Cuando solicite {{site.data.keyword.filestorage_short}}, asegúrese de que se suministre en la misma ubicación que sus instancias de cálculo.
{:important}

Puede utilizar el mandato `slcli order package-list` para encontrar el paquete que desea solicitar. Se proporciona una opción `–keyword` para realizar una búsqueda y un filtrado simples. Esta opción hace que sea más fácil encontrar el paquete que necesita. Busque `Storage-as-a-Service Package 759`.

```
$ slcli order package-list --help
Uso: slcli order package-list [OPCIONES]

  Listar paquetes que se pueden solicitar mediante la API placeOrder.

  Ejemplo:
      # Lista de todos los paquetes para la solicitud
      slcli order package-list

  Las palabras clave también se pueden utilizar para algunas funciones de filtrado simple para que sea más fácil encontrar un paquete.

  Ejemplo:
     # Lista de todos los paquetes con "server" en el nombre
      slcli order package-list --keyword server

Opciones:
  --keyword TEXTO  Una palabra (o serie) que se utiliza para filtrar nombres de paquetes.
  -h, --help      Mostrar este mensaje y salir.
```

También puede utilizar el mandato `slcli file volume-order`.

```
# slcli file volume-order --help
Uso: slcli file volume-order [OPCIONES]

  Solicitar un volumen de almacenamiento en archivo.

Opciones:
  --storage-type [performance|endurance]
                                  Tipo de volumen de almacenamiento de archivos
                                  [obligatorio]
  --size ENTERO                  Tamaño del volumen de almacenamiento de
                                  archivos en GB [obligatorio]
  --iops ENTERO                  IOP de almacenamiento de rendimiento, entre
                                  100 y 6000 en múltiplos de 100 [obligatorio
                                  para el tipo de almacenamiento de
                                  rendimiento]
  --tier [0.25|2|4|10]            Nivel de almacenamiento resistente (IOP por
                                  GB) [obligatorio para el tipo de
                                  almacenamiento resistente]
  --location TEXTO                 Nombre abreviado del centro de datos
                                  (por ejemplo, dal09)  [obligatorio]
  --snapshot-size ENTERO         Parámetro opcional para solicitar espacio de
                                  instantáneas junto con almacenamiento de
                                  archivos resistente; especifica el tamaño
                                  (en GB) del espacio de instantáneas que se
                                  debe solicitar
  --service-offering [storage_as_a_service|enterprise|performance]
                                  El paquete de oferta de servicio que se va
                                  a utilizar para realizar el pedido
                                  [opcional, el valor predeterminado es
                                  'storage_as_a_service']
  --billing [hourly|monthly]      Parámetro opcional para la tasa de
                                  facturación (mensual de forma predeterminada)
  -h, --help                      Mostrar este mensaje y salir.
```

Para obtener más información sobre cómo solicitar {{site.data.keyword.filestorage_short}} a través de la API, consulte [order_file_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/file/#SoftLayer.managers.file.FileStorageManager.order_file_volume){: external}.
Para poder acceder a todas las nuevas características, solicite `el paquete 759 de almacenamiento como servicio`.
{:tip}


## Realización del pedido

En el siguiente ejemplo se muestra cómo solicitar un volumen de {{site.data.keyword.filestorage_short}} de 10 GB con 100 IOPS por GB.

```
# slcli file volume-order --storage-type performance --size 20 --location dal10 --iops 100
El pedido #32076317 se ha realizado correctamente.
> Almacenamiento como un servicio
> File Storage
> 20 GB
> 100 IOPS
```

De forma predeterminada, puede suministrar un total combinado de 250 volúmenes de {{site.data.keyword.blockstorageshort}} y de {{site.data.keyword.filestorage_short}}. Para aumentar el número de volúmenes, póngase en contacto con el representante de ventas. Para obtener más información sobre el aumento de los límites, consulte [Gestión de límites de almacenamiento](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits).
{:important}

## Autorización de los hosts para acceder al nuevo almacenamiento

```
# slcli file access-authorize --help
Uso: slcli file access-authorize [OPCIONES] ID_VOLUMEN

  Autoriza a los hosts a acceder a un volumen determinado

Opciones:
  -h, --hardware-id TEXTO    El id de un SoftLayer_Hardware que se va a autorizar
  -v, --virtual-id TEXTO     El id de un SoftLayer_Virtual_Guest que se va a autorizar
  -i, --ip-address-id TEXTO  El id de una SoftLayer_Network_Subnet_IpAddress
                            que se va a autorizar
  --ip-address TEXTO         Una dirección IP que se va a autorizar
  -s, --subnet-id TEXTO      El id de una SoftLayer_Network_Subnet_IpAddress
                            que se va a autorizar
  --help                    Mostrar este mensaje y salir.
```

Para obtener más información sobre la autorización de los hosts para acceder a {{site.data.keyword.filestorage_short}} mediante la API, consulte [authorize_host_to_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/file/#SoftLayer.managers.file.FileStorageManager.authorize_host_to_volume){: external}
{:tip}

Para obtener más información sobre el límite de autorizaciones simultáneas, consulte las [Preguntas más frecuentes](/docs/infrastructure/FileStorage?topic=FileStorage-faqs).
{:important}

## Conexión del nuevo almacenamiento
{: #mountingvolumesCLI}

En función del sistema operativo del host, siga el enlace adecuado.
- [Montaje de {{site.data.keyword.filestorage_short}} en Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [Montaje de {{site.data.keyword.filestorage_short}} en CentOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [Montaje de {{site.data.keyword.filestorage_short}} en Container Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)
- [Configuración de {{site.data.keyword.filestorage_short}} para la copia de seguridad con cPanel](/docs/infrastructure/FileStorage?topic=FileStorage-cPanelBackups)
- [Configuración de {{site.data.keyword.filestorage_short}} para la copia de seguridad con Plesk](/docs/infrastructure/FileStorage?topic=FileStorage-PleskBackup)
- [Montaje de un volumen de {{site.data.keyword.filestorage_short}} en hosts de ESXi](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)
