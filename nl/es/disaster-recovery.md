---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-01"

---

{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}


# Duplicación de volúmenes de réplica para la recuperación tras desastre

Ante la eventualidad de un error catastrófico o un desastre que cause una parada del sitio primario, los clientes pueden llevar a cabo las acciones siguientes para acceder rápidamente a sus datos en el sitio secundario. 

## Migración tras error con un duplicado de un volumen de réplica en el sitio secundario

1. Inicie sesión en la [consola de IBM Cloud](https://console.bluemix.net/catalog/){:new_window} y pulse en el icono **Menú** en la parte superior izquierda. Seleccione **Infraestructura clásica**. 

   Alternativamente, puede iniciar sesión en el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
2. Pulse **Almacenamiento** > **{{site.data.keyword.filestorage_short}}**.
3. Pulse en la réplica de la compartición de archivos de la lista para visualizar su página de **Detalles**.
4. En la página de **Detalles**, desplácese hacia abajo y seleccione una instantánea existente; a continuación pulse en **Acciones** > **Duplicar**.
5. Realice los cambios necesarios a la capacidad (para incrementar el tamaño) o a las IOP para el nuevo volumen.
6. Puede actualizar el espacio de instantáneas para el nuevo volumen si es necesario.
7. Pulse **Continuar** para realizar el orden de los duplicados.

Tan pronto como se haya creado el volumen podrá adjuntarlo a un host y realizar operaciones de lectura/escritura sobre dicho volumen. Mientras los datos se estén copiando desde el volumen original en el duplicado, puede ver un estado en la página de detalles que muestra que la duplicación está en curso. Cuando se complete el proceso de duplicación, el nuevo volumen pasa a ser independiente del original y se puede gestionar con instantáneas y réplica, como es habitual.

## Restablecimiento del sitio primario original

Si desea devolver la producción al sitio primario original, debe llevar a cabo los pasos siguientes.

1. Inicie sesión en la [consola de IBM Cloud](https://console.bluemix.net/catalog/){:new_window} y pulse en el icono **Menú** en la parte superior izquierda. Seleccione **Infraestructura clásica**. 

   Alternativamente, puede iniciar sesión en el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
2. Pulse **Almacenamiento** > **{{site.data.keyword.filestorage_short}}**.
3. Pulse en el nombre de LUN y cree una planificación de instantáneas (si no existe ya una). 

   Para obtener más información sobre la planificación de instantáneas, consulte [Gestión de instantáneas](working-with-snapshots.html#adding-a-snapshot-schedule).
   {:tip}
4. Pulse **Réplica** y pulse **Adquirir una réplica**.
5. Seleccione la planificación de instantáneas existente que quiera que siga la réplica. La lista contiene todas las planificaciones de instantáneas activas. 
6. Pulse **Ubicación** y seleccione el centro de datos que era su sitio de producción original.
7. Pulse **Continuar**.
8. Marque el recuadro de selección **He leído el Acuerdo de servicio maestro…** y pulse **Realizar pedido**.

Una vez se ha completado la réplica, necesita crear un volumen duplicado de la nueva réplica.
{:important}

1. Vuelva a **Almacenamiento** > **{{site.data.keyword.filestorage_short}}**.
2. Pulse en la réplica del LUN en la lista para visualizar su página de **Detalles**.
3. En la página de **Detalles**, desplácese hacia abajo y seleccione una instantánea existente; a continuación pulse en **Acciones** > **Duplicar**.
4. Realice los cambios necesarios a la capacidad (para incrementar el tamaño) o a las IOP para el nuevo volumen.
5. Actualice el espacio de instantáneas para el volumen si es necesario.
6. Pulse **Continuar** para realizar el orden de los duplicados.

Cuando se complete el proceso de duplicación, puede cancelar la réplica y los volúmenes que se hayan utilizado para devolver los datos al sitio primario original. El duplicado se convierte en el almacenamiento primario, y puede establecerse de nuevo la réplica al sitio secundario original.
