---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-29"

---
{:new_window: target="_blank"}

# Nuevas ubicaciones y características de {{site.data.keyword.filestorage_short}}

{{site.data.keyword.BluSoftlayer_full}} presenta una nueva versión de {{site.data.keyword.filestorage_full}}. El nuevo almacenamiento está disponible en centros de datos seleccionados, y está respaldado por el almacenamiento flash a niveles de IOPS superiores con cifrado de disco para datos en reposo. Todo el almacenamiento solicitado en los centros de datos seleccionados se crea automáticamente con la nueva versión de {{site.data.keyword.filestorage_short}}.

**Nota:** ha cambiado el punto de montaje de NFS para los volúmenes nuevos. Consulte la sección **Nuevo punto de montaje para volúmenes de {{site.data.keyword.filestorage_short}} mejorado** que hay a continuación para ver detalles.

El nuevo {{site.data.keyword.filestorage_short}} está disponible actualmente en los siguientes centros de datos y regiones, y pronto se incrementará la disponibilidad de centros de datos.

<table style="width:100%;">
  <caption>La Tabla 1 muestra nuestra disponibilidad de centro de datos. Cada región tiene su propia columna. Algunas ciudades, como Dallas, San José, Washington DC, Amsterdam, Frankfurt, Londres y Sidney, tienen varios centros de datos.</caption>
	<tr>
		<td><strong>EE.UU. 2</strong></td>
		<td><strong>UE</strong></td>
		<td><strong>Australia</strong></td>
		<td><strong>Canadá</strong></td>
		<td><strong>Latinoamérica</strong></td>
		<td><strong>Asia-Pacífico</strong></td>
	</tr>
	<tr>
		<td><p>SJC03<br />
			SJC04<br />
			DC04<br />
			WDC06<br />
			WDC07<br />
			DAL09<br />
			DAL10<br />
			DAL12<br />
			DAL13<br /><br /><br /></p>
		</td>
		<td><p>LON02<br />
			LON04<br />
			LON06<br />
			FRA02<br />
			FRA04<br />
			FRA05<br />
			AMS01<br />
			AMS03<br />
			OSLO1<br />
			PAR01<br />
			MIL01<br /></p>
		</td>
		<td><p>SYD01<br />
			SYD04<br />
			MEL01<br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
		</td>
		<td><p>TOR01<br />
			MON01<br /><br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
		</td>
		<td><p>MEX01<br />
			SAO01<br /><br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
		</td>
		<td><p>TOK02<br />
			HKG02<br />
			SEO01<br />
			SNG01<br />
			CHE01<br /><br /><br /><br /><br /><br /><br /></p>
		</td>
	</tr>
</table>


El nuevo almacenamiento tiene las siguientes características y funciones:

- [Cifrado gestionado por el proveedor para datos en reposo](block-file-storage-encryption-rest.html). <br/> Todos los volúmenes de {{site.data.keyword.filestorage_short}} se suministran automáticamente como cifrados sin cargo adicional.
- Opción de nivel de 10 IOPS por GB. <br/> Se ha añadido un nuevo nivel al {{site.data.keyword.filestorage_short}} de tipo Resistencia para dar soporte a las cargas de trabajo más exigentes.
- Almacenamiento respaldado por all flash. <br/> El {{site.data.keyword.filestorage_short}} que se suministra con las opciones de Resistencia o Rendimiento a 2 IOPS por GB o superior disponen del respaldo del almacenamiento de tipo all flash.
- Soporte de instantáneas y de réplica.
- Se ha añadido la opción de facturación por horas para el almacenamiento que se prevé utilizar menos de un mes completo.
- Hasta 48.000 IOPS para {{site.data.keyword.filestorage_short}} suministrado con el tipo Rendimiento.
- Las tasas de IOPS se pueden ajustar para mejorar el rendimiento en caso de cambios estacionales en la carga de trabajo. [Aquí](adjustable-iops.html) puede leer más información sobre esta característica.
- Cree un clon de sus datos con la característica de [duplicación de volúmenes de {{site.data.keyword.filestorage_short}}](how-to-create-duplicate-volume.html).
- El almacenamiento es se puede ampliar en incrementos de GB hasta un máximo de 12 TB de inmediato, sin necesidad de crear un duplicado o transferir datos manualmente a un volumen de mayor tamaño. [Aquí](expandable_file_storage.html) puede leer más información sobre esta característica.

## Nuevo punto de montaje para volúmenes de {{site.data.keyword.filestorage_short}} mejorado

Todos los volúmenes de {{site.data.keyword.filestorage_short}} mejorado suministrados en estos centros de datos tienen un punto de montaje distinto que los volúmenes no cifrados. Para asegurarse de que está utilizando el punto de montaje correcto para ambos volúmenes de almacenamiento, puede ver la información de punto de montaje en la página **Detalles del volumen** en la interfaz de usuario. También puede acceder al punto de montaje correcto mediante una llamada de API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Aquí puede consultar si se han actualizado más centros de datos y si se han añadido nuevas funciones y características a {{site.data.keyword.filestorage_short}}.
