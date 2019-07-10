---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-10"

keywords: File Storage, NSF, mounting File Storage, mounting storage on Linux,

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.filestorage_short}} unter Linux anhängen
{: #mountingLinux}

Stellen Sie zuerst sicher, dass der Host, über den auf den {{site.data.keyword.filestorage_full}}-Datenträger zugegriffen werden soll, über die [{{site.data.keyword.cloud}}-Konsole](https://{DomainName}/classic){: external} autorisiert wird.

1. Klicken Sie auf der {{site.data.keyword.filestorage_short}}-Listenseite auf den Link **Aktionen**, die dem neuen gemeinsam genutzten Speicher zugeordnet sind, und klicken Sie auf **Host autorisieren**.
2. Wählen Sie mindestens einen Host in der Liste aus und klicken Sie auf **Abschicken**. Durch diese Aktion wird der Host für den Zugriff auf den gemeinsam genutzten Speicher autorisiert.

Alternativ dazu können Sie die Hosts auch über die SLCLI berechtigen.
```
# slcli file access-authorize --help
Syntax: slcli file access-authorize [OPTIONS] VOLUME_ID

Optionen:
  -h, --hardware-id TEXT    ID eines einzelnen Hardware-Servers für die Autorisierung.
  -v, --virtual-id TEXT     ID eines virtuellen Servers für die Autorisierung.
  -i, --ip-address-id TEXT  ID einer einzelnen IP-Adresse für die Autorisierung.
  -p, --ip-address TEXT     IP-Adresse für die Autorisierung.
  -s, --subnet-id TEXT      ID eines einzelnen Teilnetzes für die Autorisierung.
  --help                    Diese Nachricht anzeigen und Ausführung beenden.
```

## Gemeinsam genutzten {{site.data.keyword.filestorage_short}}-Speicher anhängen

Verwenden Sie die folgenden Anweisungen, um eine Linux-basierte Instanz von {{site.data.keyword.cloud}} Compute mit einem gemeinsam genutzten NFS-Speicher (NFS – Network File System) zu verbinden. Das Beispiel basiert auf Red Hat Enterprise Linux 6. Die Schritte können für andere Linux-Distributionen entsprechend der Dokumentation zum Betriebssystem (OS) des Anbieters angepasst werden.

Der Mountpunkt der Dateispeicherinstanz kann von der {{site.data.keyword.filestorage_short}}-Listenseite oder über den API-Aufruf - `SoftLayer_Network_Storage::getNetworkMountAddress()` abgerufen werden.
{:tip}

1. Installieren Sie die erforderlichen Tools.
   ```
   # yum -y install nfs-utils nfs-utils-lib
   ```
   {:pre}

2. Hängen Sie den fernen gemeinsam genutzten Speicher an.
   ```
   # mount -t "nfs version" -o "Optionen" <Mountpunkt> /mnt
   ```

   Beispiel
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

4. Wechseln Sie zu dem Mountpunkt und lesen und schreiben Sie Dateien.
   ```
   # touch /mnt/test
   # ls -la /mnt
   total 12
   drwxr-xr-x 2 nobody nobody 4096 Sep 8 15:52 .
   dr-xr-xr-x. 22 root root 4096 Sep 8 14:30 ..
   -rw-r--r-- 1 nobody nobody 0 Sep 8 15:52 test
   ```

   Die Dateien, die von root erstellt werden, haben das Eigentumsrecht an `nobody:nobody`. Um das Eigentumsrecht ordnungsgemäß anzuzeigen, muss die Datei `idmapd.conf` mit den richtigen Domäneneinstellungen aktualisiert werden. Informationen hierzu finden Sie im Abschnitt [Implementierung von no_root_squash für NFS](#norootsquash).
   {:tip}

5. Hängen Sie den fernen gemeinsam genutzten Speicher beim Start an. Um die Einrichtung abzuschließen, bearbeiten Sie die Dateisystemtabelle (`/etc/fstab`), um den fernen gemeinsam genutzten Speicher der Liste mit Einträgen hinzuzufügen, die beim Start automatisch angehängt wird:

   ```
   (Hostname):/(Benutzername) /mnt "NFS-Version" "Optionen" 0 0
   ```

   Beispiel

   ```
   nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt nfs4 defaults,hard,intr 0 0
   ```

6. Stellen Sie sicher, dass die Konfigurationsdatei keine Fehler enthält.

   ```
   # mount -fav
   ```
   {:pre}

   Wenn der Befehl ohne Fehler ausgeführt wird, ist Ihre Einrichtung abgeschlossen.

   Bei Verwendung von NFS 4.1 fügen Sie dem Mountbefehl die Angabe `sec=sys` hinzu, um Probleme mit Dateieigentumsrechten zu vermeiden.
   {:tip}

   Unter dem Hostbetriebssystem CentOS können weitere Optionen konfiguriert werden. Weitere Informationen finden Sie in [{{site.data.keyword.filestorage_short}} unter CentOS anhängen](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS).


## `no_root_squash` für NFS implementieren (optional)
{: #norootsquash}

Durch die Konfiguration von `no_root_squash` können Root-Clients die Rootberechtigungen für den gemeinsam genutzten NFS-Speichern behalten.
- Für NFSv3 sind keine besonderen Schritte für Clients erforderlich; `no_root_squash` funktioniert.
- Bei NFSv4 müssen Sie die Domäne 'nfsv4' auf `slnfsv4.com` setzen und `rpcidmapd` oder einen ähnlichen Service starten, der von Ihrem Betriebssystem verwendet wird.

Beispiel

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
## Dateisystem abhängen

Um momentan angehängte Dateisysteme auf Ihrem Host abzuhängen, müssen Sie den Befehl `umount` mit dem Plattennamen oder dem Mountpunktnamen ausführen.

```
umount /dev/sdb
```
{:pre}

```
umount /mnt
```
{:pre}
