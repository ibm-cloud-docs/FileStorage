---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-31"

---
{:tip: .tip}
{:note: .note}
{:important: .important}

# Protección de los datos: cifrado en reposo gestionado por el proveedor

{{site.data.keyword.BluSoftlayer_full}} se toma en serio la seguridad y comprende la importancia de cifrar los datos para mantenerlos protegidos. Mediante el cifrado gestionado por proveedor, los datos de {{site.data.keyword.filestorage_full}} suministrados con las opciones de Resistencia o de Rendimiento se cifran de forma predeterminada sin coste adicional ni impacto sobre el rendimiento.

La característica de cifrado en reposo gestionado por proveedor utiliza los siguientes protocolos estándar del sector:

* Cifrado AES-256 estándar del sector
* Las claves se gestionan internamente con el protocolo estándar del sector Key Management Interoperability Protocol (KMIP)
* El almacenamiento se valida frente a los siguientes estándares:
    - Federal Information Processing Standard (FIPS) Publication 140-2,
    - Federal Information Security Management Act (FISMA),
    - Health Insurance Portability and Accountability Act (HIPAA),
    - Payment Card Industry (PCI),
    - Basel II,
    - California Security Breach Information Act (SB 1386) y
    - Conformidad con EU Data Protection Directive 95/46/EC.

## Protección de las instantáneas o almacenamiento replicado  

Todas las instantáneas y réplicas de almacenamiento de archivos cifrados también se cifran de forma predeterminada. Esta característica no se puede desactivar por volumen.

## Suministro del almacenamiento con cifrado

La característica de cifrado en reposo gestionado por el proveedor está disponible en determinados centros de datos. Todo el almacenamiento que se solicita en estos centros de datos se suministra automáticamente con el cifrado de datos en reposo. Pulse [aquí](new-ibm-block-and-file-storage-location-and-features.html) para ver la lista actual de los centros de datos donde está disponible el cifrado de {{site.data.keyword.filestorage_short}}.

Al realizar el pedido de {{site.data.keyword.filestorage_short}}, seleccione un centro de datos marcado con un asterisco (*) (`*`). Puede ver un icono de bloqueo a la derecha del campo Nombre de volumen/LUN, que indica que el volumen está cifrado. Consulte
la Figura 1.

![El icono de bloqueo indica que el LUN está cifrado](/images/encryptedstorage.png)
<caption>Figura 1. Ejemplo del icono de bloqueo que indica que el volumen está cifrado.</caption>

El almacenamiento no cifrado que se haya suministrado antes de una actualización del centro de datos **no** se cifrará automáticamente. Si dispone de almacenamiento no cifrado en un centro de datos actualizado y desea cifrarlo, debe crear un nuevo volumen y mover los datos. Para obtener más información, consulte [Migración del almacenamiento de archivos en centros de datos actualizados](migrate-file-storage-encrypted-file-storage.html)
{:important}
