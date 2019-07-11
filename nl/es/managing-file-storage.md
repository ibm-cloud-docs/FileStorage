---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-10"

keywords: File Storage, file storage, NFS, authorizing hosts, rewoke access, grant access, view authorizations

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}


# Gestión de {{site.data.keyword.filestorage_short}}
{: #managingstorage}

Puede gestionar los volúmenes de {{site.data.keyword.filestorage_full}} mediante la consola de {{site.data.keyword.cloud}}.

## Autorización de hosts para acceder a {{site.data.keyword.filestorage_short}}

Los hosts “autorizados” son los hosts a los que se les ha otorgado acceso a un volumen determinado. Sin la autorización del host, no puede acceder ni utilizar el almacenamiento de su sistema. El hecho de autorizar a un host el acceso al volumen genera un nombre de usuario y una contraseña.

Puede autorizar y conectar hosts que estén ubicados en el mismo centro de datos que su almacenamiento. Puede tener varias cuentas, pero no puede autorizar a un host de una cuenta a que acceda a su almacenamiento en otra cuenta.
{:important}

1. Vaya a la [consola de {{site.data.keyword.cloud}}](https://{DomainName}/){: external}. En el menú, seleccione **Infraestructura clásica**.
2. Pulse **Almacenamiento** > **{{site.data.keyword.filestorage_short}}** y pulse el **Nombre de volumen**.
3. Desplácese a la sección **Hosts autorizados** de la página.
4. Pulse **Autorizar host** en la parte derecha. Seleccione los hosts que pueden acceder a ese volumen determinado.

De manera alternativa, puede utilizar el mandato siguiente en SLCLI.
```
# slcli file access-authorize --help
Uso: slcli file access-authorize [OPCIONES] ID_VOLUMEN

Opciones:
  -h, --hardware-id TEXTO   El ID de un servidor de hardware que se va a autorizar.
  -v, --virtual-id TEXTO    El ID de un servidor virtual que se va a autorizar.
  -i, --ip-address-id TEXTO El ID de una dirección IP que se va a autorizar.
  -p, --ip-address TEXTO    Una dirección IP que se va a autorizar.
  -s, --subnet-id TEXTO     ID de una subred que se va a autorizar.
  --help                    Mostrar este mensaje y salir.
```

## Visualización de la lista de hosts que están autorizados para acceder a un volumen de {{site.data.keyword.filestorage_short}}

1. Vaya a la [consola de {{site.data.keyword.cloud}}](https://{DomainName}/){: external}. En el menú, seleccione **Infraestructura clásica**.
2. Pulse **Almacenamiento** > **{{site.data.keyword.filestorage_short}}** y pulse el **Nombre de volumen**.
3. Desplácese en la página hasta la sección **Hosts autorizados**.

Allí puede ver la lista de hosts, que actualmente tienen autorización para acceder al volumen.

De manera alternativa, puede utilizar el mandato siguiente en SLCLI.
```
# slcli file access-list --help
Uso: slcli file access-list [OPCIONES] ID_VOLUMEN

Opciones:
  --sortby TEXTO  Columna por la que se debe ordenar
  --columns TEXTO Columnas que se deben visualizar. Opciones: id, name, type,
                  private_ip_address, source_subnet, host_iqn, username,
                  password, allowed_host_id
  -h, --help      Mostrar este mensaje y salir.
```


## Visualización de los volúmenes de {{site.data.keyword.filestorage_short}} a los cuales un host está autorizado

Puede ver los volúmenes a los cuales un host tiene acceso, incluyendo la información necesaria para realizar una conexión: nombre de volumen, tipo de almacenamiento, dirección de destino, capacidad y ubicación.

1. Vaya a la [consola de {{site.data.keyword.cloud}}](https://{DomainName}/){: external}.
2. En el menú, seleccione **Infraestructura clásica**.
3. Pulse **Dispositivos** > **Lista de dispositivos**.
4. Pulse el dispositivo adecuado.
5. Seleccione el separador Almacenamiento.

Se le presenta una lista de los volúmenes de almacenamiento a los cuales este host tiene acceso, todos están agrupados por tipo de almacenamiento (bloque, archivo, otros). Desde los menús respectivos de **Acción**, puede autorizar almacenamiento adicional o eliminar el acceso.


## Montaje y desmontaje de {{site.data.keyword.filestorage_short}}

Puede utilizar la información de punto de montaje proporcionada en la vista **Detalles del volumen** para montar {{site.data.keyword.filestorage_short}} desde un host. Consulte [Acceso a {{site.data.keyword.filestorage_short}} en Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)


## Revocación del acceso de un host a {{site.data.keyword.filestorage_short}}

Si quiere detener el acceso desde un host a un volumen de almacenamiento determinado, puede revocar el acceso. Cuando se revoca el acceso, la conexión de host se descarta del volumen. El sistema operativo y las aplicaciones ya no se pueden comunicar con el volumen.

Para impedir problemas del lado del host, desmonte el volumen de almacenamiento desde el sistema operativo antes de revocar el acceso para evitar perder unidades o corrupción de datos.
{:important}

Puede revocar el acceso desde el Almacenamiento de la Lista de Dispositivos o desde las vistas de almacenamiento.

### Revocación del acceso de la lista de dispositivos

1. Vaya a la [consola de {{site.data.keyword.cloud}}](https://{DomainName}/){: external}.
2. En el menú, seleccione **Infraestructura clásica**.
3. Pulse **Dispositivos** > **Lista de dispositivos**.
2. Efectúe una doble pulsación en el dispositivo adecuado.
3. Seleccione el separador **Almacenamiento**.
4. Se le presenta una lista de los volúmenes de almacenamiento a los cuales este host tiene acceso, todos agrupados por tipo de almacenamiento (bloque, archivo, otros). Seleccione el menú respectivo de **Acción** situado junto al volumen del que desea revocar el acceso y pulse **Revocar acceso**.
5. Confirme si desea revocar el acceso al volumen, porque la acción no puede deshacerse. Pulse **Sí** para revocar el acceso al volumen o **No** para cancelar la acción.

Si desea desconectar varios volúmenes desde un host específico, debe repetir la acción Revocar acceso para cada volumen.
{:tip}


### Revocación del acceso de la vista de almacenamiento

1. Vaya a la [consola de {{site.data.keyword.cloud}}](https://{DomainName}/){: external}.
2. En el menú, seleccione **Infraestructura clásica**.
3. Pulse **Almacenamiento** > **{{site.data.keyword.filestorage_short}}**, y seleccione el **Volumen** desde el cual desea revocar el acceso.
4. Desplácese a la sección **Hosts autorizados** de la página.
5. Pulse **Acciones** junto al host cuyo acceso se va a revocar y seleccione **Revocar acceso**.
6. Confirme si desea revocar el acceso al volumen, porque la acción no puede deshacerse. Pulse **Sí** para revocar el acceso al volumen o **No** para cancelar la acción.

Si desea desconectar varios hosts desde un host específico, debe repetir la acción Revocar acceso para cada host.
{:tip}

### Revocación del acceso mediante SLCLI.
Puede utilizar el mandato siguiente en SLCLI.
```
# slcli file access-revoke --help
Uso: slcli file access-revoke [OPCIONES] ID_VOLUMEN

Opciones:
  -h, --hardware-id TEXTO   El ID del servidor de hardware cuya autorización se va a revocar.
  -v, --virtual-id TEXTO    El ID de un servidor virtual cuya autorización se va a revocar.
  -i, --ip-address-id TEXTO El ID de una dirección IP cuya autorización se va a revocar.
  -p, --ip-address TEXTO    Una dirección IP cuya autorización se va a revocar.
  -s, --subnet-id TEXTO     ID de una subred cuya autorización se va a revocar.
  --help                    Mostrar este mensaje y salir.
```

## Cancelación de un volumen de almacenamiento

Si ya no necesita un volumen específico, puede cancelar ese almacenamiento. Para cancelar un volumen de almacenamiento, primero debe revocar el acceso desde cualquier host.

1. Vaya a la [consola de {{site.data.keyword.cloud}}](https://{DomainName}/){: external}. En el menú, seleccione **Infraestructura clásica**.
2. Pulse **Almacenamiento** > **{{site.data.keyword.filestorage_short}}**.
3. Pulse **Acciones** del volumen que se va a cancelar y seleccione **Cancelar {{site.data.keyword.filestorage_short}}**.
4. Confirme si desea cancelar el volumen inmediatamente o en la fecha de aniversario en la que se suministró el volumen.

   Si selecciona la opción de cancelar el volumen en su fecha de aniversario, puede anular la solicitud de cancelación antes de su fecha de aniversario.
   {:tip}
5. Pulse **Continuar** o **Cerrar**.
6. Marque el recuadro de selección de acuse de recibo y pulse **Confirmar**.

De manera alternativa, puede utilizar el mandato siguiente en SLCLI.
```
# slcli file volume-cancel --help
Uso: slcli file volume-cancel [OPCIONES] ID_VOLUMEN

Opciones:
  --reason TEXTO Una razón para la cancelación (opcional)
  --immediate    Cancelar el volumen de almacenamiento de archivos inmediatamente en lugar de
                 hacerlo en el aniversario de facturación
  -h, --help     Mostrar este mensaje y salir.
```
