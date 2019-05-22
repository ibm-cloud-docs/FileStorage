---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, locations, data centers

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Nuevas ubicaciones y características
{: #news}

{{site.data.keyword.BluSoftlayer_full}} presenta una nueva versión de {{site.data.keyword.filestorage_full}}. El nuevo almacenamiento está disponible en centros de datos seleccionados, y está respaldado por el almacenamiento flash a niveles de IOPS superiores con cifrado de disco para datos en reposo. Todo el almacenamiento solicitado en los centros de datos seleccionados se crea automáticamente con la nueva versión de {{site.data.keyword.filestorage_short}}.

Ha cambiado el punto de montaje de NFS para los volúmenes nuevos. Consulte la sección [Nuevo punto de montaje para volúmenes de {{site.data.keyword.filestorage_short}} mejorado](#new-mount-point-for-enhanced-file-storage-volumes) que hay a continuación para ver detalles.
{:important}

## Nuevas ubicaciones
{: #new-locations}

El nuevo {{site.data.keyword.filestorage_short}} está disponible actualmente en los siguientes centros de datos y regiones, y pronto se incrementará la disponibilidad de centros de datos.

<table role="presentation">
  <tr>
    <td><strong>EE.UU. 2</strong></td>
    <td><strong>UE</strong></td>
    <td><strong>Australia</strong></td>
    <td><strong>Canadá</strong></td>
    <td><strong>Latinoamérica</strong></td>
    <td><strong>Asia-Pacífico</strong></td>
  </tr>
  <tr>
    <td>DAL09<br />
	DAL10<br />
	DAL12<br />
	DAL13<br />
	SJC03<br />
        SJC04<br />
	WDC04<br />
	WDC06<br />
	WDC07<br />
	<br /><br /><br />
    </td>
    <td>AMS01<br />
        AMS03<br />
	FRA02<br />
	FRA04<br />
	FRA05<br />
	LON02<br />
	LON04<br />
	LON05<br />
	LON06<br />
	MIL01<br />
	OSLO1<br />
	PAR01<br />
    </td>
    <td>MEL01<br />
        SYD01<br />
        SYD04<br />
        SYD05<br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>MON01<br />
        TOR01<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>MEX01<br />
        SAO01<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>CHE01<br />
        HKG02<br />
	SEO01<br />
	SNG01<br />
        TOK02<br />
	TOK04<br />
	TOK05<br />
	<br /><br /><br /><br /><br />
    </td>
  </tr>
</table>

*La Tabla 1 muestra nuestra disponibilidad de centro de datos. Cada región tiene su propia columna. Algunas ciudades, como Dallas, San José, Washington DC, Ámsterdam, Frankfurt, Londres y Sídney, tienen varios centros de datos.*

## Nuevas características y prestaciones
{: #features}

- [Cifrado gestionado por el proveedor para datos en reposo](/docs/infrastructure/FileStorage?topic=FileStorage-encryption). <br/> Todos los volúmenes de {{site.data.keyword.filestorage_short}} se suministran automáticamente como cifrados sin cargo adicional.
- Opción de nivel de 10 IOPS por GB. <br/> Se ha añadido un nuevo nivel al {{site.data.keyword.filestorage_short}} de tipo Resistencia para dar soporte a las cargas de trabajo más exigentes.
- Almacenamiento respaldado por all flash. <br/> El {{site.data.keyword.filestorage_short}} que se suministra con las opciones de Resistencia o Rendimiento a 2 IOPS por GB o superior disponen del respaldo del almacenamiento de tipo all flash.
- Soporte de instantáneas y de réplica.
- Se ha añadido la opción de facturación por horas para el almacenamiento que se prevé utilizar menos de un mes completo.
- Hasta 48.000 IOPS para {{site.data.keyword.filestorage_short}} suministrado con el tipo Rendimiento.
- Las tasas de IOPS se pueden ajustar para mejorar el rendimiento en caso de cambios estacionales en la carga de trabajo. [Aquí](/docs/infrastructure/FileStorage?topic=FileStorage-adjustingIOPS) puede leer más información sobre esta característica.
- Cree un clon de sus datos con la característica de [duplicación de volúmenes de {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume).
- El almacenamiento es se puede ampliar en incrementos de GB hasta un máximo de 12 TB de inmediato, sin necesidad de crear un duplicado o transferir datos manualmente a un volumen de mayor tamaño. [Aquí](/docs/infrastructure/FileStorage?topic=FileStorage-expandCapacity) puede leer más información sobre esta característica.

## Nuevo punto de montaje para volúmenes de {{site.data.keyword.filestorage_short}} mejorado

Todos los volúmenes de {{site.data.keyword.filestorage_short}} mejorados suministrados en estos centros de datos tienen un punto de montaje distinto que los volúmenes no cifrados. Para asegurarse de que utiliza el punto de montaje correcto para los volúmenes de almacenamiento, puede consultar la información sobre el punto de montaje en la página **Detalles del volumen** en la consola. También puede acceder al punto de montaje correcto mediante una llamada de API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Para poder acceder a todas las nuevas características, seleccione `Storage-as-a-Service Package 759` cuando realice el pedido a través de la API. Para obtener más información sobre cómo solicitar {{site.data.keyword.filestorage_short}} a través de la API, consulte [order_file_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/file/#SoftLayer.managers.file.FileStorageManager.order_file_volume){: external}.
{:important}

Aquí puede consultar si se han actualizado más centros de datos y si se han añadido nuevas funciones y características a {{site.data.keyword.filestorage_short}}.
{:tip}
