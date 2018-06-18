---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-24"

---
{:new_window: target="_blank"}
{:pre: .pre}

# Auf {{site.data.keyword.filestorage_short}} unter Linux zugreifen

Stellen Sie zunächst sicher, dass der Host, der auf den {{site.data.keyword.filestorage_full}}-Datenträger zugreifen soll, über das [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} autorisiert ist.

1. Klicken Sie auf der {{site.data.keyword.filestorage_short}}-Listenseite auf **Aktionen**, die der neuen Freigabe zugeordnet sind, und klicken Sie auf **Host autorisieren**.
2. Wählen Sie mindestens einen gewünschten Host in der Liste aus und klicken Sie auf **Abschicken**. Dadurch wird der Host für den Zugriff auf die Freigabe autorisiert.

## {{site.data.keyword.filestorage_short}}-Freigabe anhängen

Die folgenden Schritte sind erforderlich, um eine Linux-basierte Instanz von {{site.data.keyword.BluSoftlayer_full}} Compute mit einer NFS-Freigabe (NFS – Network File System) zu verbinden. Das Beispiel basiert auf Red Hat Enterprise Linux 6. Die Schritte können für andere Linux-Distributionen entsprechend der Dokumentation zum Betriebssystem (OS) des Anbieters angepasst werden.

**Hinweis:** der Mountpunkt der File Storage-Instanz kann auf {{site.data.keyword.filestorage_short}}-Listenseite oder über den API-Aufruf `SoftLayer_Network_Storage::getNetworkMountAddress()` abgerufen werden.

1. Installieren Sie die erforderlichen Pakete/Tools.
   ```
   # yum -y install nfs-utils nfs-utils-lib
   ```
   {:pre}
    
2. Hängen Sie die ferne Freigabe an.
   ```
   # mount -t "nfs version" -o "Optionen" <Mountpunkt> /mnt
   ```
       
   Das folgende Beispiel zeigt das Anhängen einer fernen Freigabe an eine Speicherinstanz.
   ```
   # mount -t nfs4 -o hard,intr
   nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt
   ```
 
3. Überprüfen Sie, ob die Mountoperation erfolgreich war.
   ```
   # df -h
   Filesystem Size Used Avail Use% Mounted on
   /dev/xvda2 25G 1.4G 22G 6% /
   tmpfs 1.9G 0 1.9G 0% /dev/shm
   /dev/xvda1 97M 51M 42M 55%
   ```
    
4. Navigieren Sie zu dem Mountpunkt und lesen und schreiben Sie Dateien.
   ```
   # touch /mnt/test
   # ls -la /mnt
   total 12
   drwxr-xr-x 2 nobody nobody 4096 Sep 8 15:52 .
   dr-xr-xr-x. 22 root root 4096 Sep 8 14:30 ..
   -rw-r--r-- 1 nobody nobody 0 Sep 8 15:52 test
   ```

   **Anmerkung:** Die vom Rootbenutzer erstellten Dateien haben das Eigentumsrecht `nobody:nobody`. Um das Eigentumsrecht ordnungsgemäß anzuzeigen, muss die Datei `idmapd.conf` mit den richtigen Domäneneinstellungen aktualisiert werden. Weitere Informationen finden Sie im Abschnitt 'Implementierung von no_root_squash für NFS' unten auf dieser Seite.
    
5. Hängen Sie die ferne Freigabe beim Start an. Um die Einrichtung abzuschließen, bearbeiten Sie die Dateisystemtabelle `/etc/fstab`, um die ferne Freigabe der Liste mit Einträgen hinzuzufügen, die beim Start automatisch angehängt wird:

   ```
   (Hostname):/(Benutzername) /mnt "NFS-Version" "Optionen" 0 0
   ```
    
   Für die Instanz aus dem Beispiel für das Anhängen einer fernen Freigabe sähe der Eintrag wie folgt aus:
    
   ```
   nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt nfs4 defaults,hard,intr 0 0
   ```
    
6. Stellen Sie sicher, dass die Konfigurationsdatei keine Fehler enthält.

   ```
   # mount -fav
   ```
   {:pre}
    
   Wenn der Befehl ohne Fehler ausgeführt wird, ist Ihre Einrichtung abgeschlossen.

**Anmerkung:** Bei Verwendung von NFS 4.1 fügen Sie dem Mountbefehl die Angabe `sec=sys` hinzu, um Probleme mit Dateieigentumsrechten zu vermeiden.

 
## Implementierung von no_root_squash für NFS (optional)

Durch die Konfiguration von 'no_root_squash' können Root-Clients die Rootberechtigungen für die NFS-Freigabe behalten. Für NFSv3 sind keine besonderen Schritte für Clients erforderlich; 'no_root_squash' funktioniert ohne Weiteres. Bei NFSv4 müssen Sie die Domäne 'nfsv4' auf `slnfsv4.com` setzen und `rpcidmapd` oder einen ähnlichen abhängig von der Betriebssystemversion starten.

Ein Beispiel:

1. Legen Sie auf dem Host die Domäneneinstellung in der Datei `/etc/idmapd.conf` fest.

   ```
   #vi /etc/idmapd.conf
   [General]
   #Ausführlichkeit = 0
   #Folgende Angabe muss auf den lokalen NFSv4-Domänennamen gesetzt werden
   #Der Standardwert ist der DNS-Domänenname des Hosts.
   Domain = slnfsv4.com
   [Mapping]
   Nobody-User = nobody
   Nobody-Group = nobody
   ```
    
2. Führen Sie `nfsidmap -c` aus.
3. Starten Sie `rpcidmapd`.
   ```
   # /etc/init.d/rpcidmapd start
   Starting RPC idmapd: [ OK ]
   ```
