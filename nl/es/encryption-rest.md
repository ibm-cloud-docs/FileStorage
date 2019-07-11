---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-18'"

keywords: File Storage, file storage, NFS, security, encryption

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Cifrado en reposo gestionado por el proveedor
{: #encryption}

{{site.data.keyword.cloud}} se toma en serio la seguridad y comprende la importancia de cifrar los datos para mantenerlos protegidos. Mediante el cifrado gestionado por proveedor, los datos de {{site.data.keyword.filestorage_full}} suministrados con las opciones de Resistencia o de Rendimiento se cifran de forma predeterminada sin coste adicional ni impacto sobre el rendimiento.

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
    - Conformidad con EU Data Protection Directive (95/46/EC).

## Protección de las instantáneas o almacenamiento replicado  

Todas las instantáneas y réplicas de almacenamiento de archivos cifrados también se cifran de forma predeterminada. Esta característica no se puede desactivar por volumen.

## Suministro del almacenamiento con cifrado

La característica de cifrado en reposo gestionado por el proveedor está disponible en [determinados centros de datos](/docs/infrastructure/FileStorage?topic=FileStorage-selectDC). Todo el almacenamiento que se solicita en estos centros de datos se suministra automáticamente con el cifrado de datos en reposo.

Al realizar el pedido de {{site.data.keyword.filestorage_short}}, seleccione un centro de datos marcado con un asterisco (*) (`*`). Puede ver un icono de bloqueo a la derecha del campo Nombre de volumen, que indica que el volumen está cifrado. Consulte la Figura 1.

![El icono de bloqueo indica que el volumen está cifrado](/images/encryptedstorage.png)
<caption>Figura 1. Ejemplo del icono de bloqueo que indica que el volumen está cifrado.</caption>

El almacenamiento no cifrado que se haya suministrado antes de una actualización del centro de datos **no** se cifrará automáticamente. Si dispone de almacenamiento no cifrado en un centro de datos actualizado y desea cifrarlo, debe crear un volumen y mover los datos. Para obtener más información, consulte [Migración del almacenamiento de archivos en centros de datos actualizados](/docs/infrastructure/FileStorage?topic=FileStorage-migratestorage)
{:important}
