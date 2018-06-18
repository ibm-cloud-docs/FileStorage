---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-24"

---
{:new_window: target="_blank"}
{:pre: .pre}
 
# {{site.data.keyword.filestorage_short}} für Sicherung mit cPanel konfigurieren

In diesem Artikel werden Anweisungen zum Konfigurieren Ihrer in {{site.data.keyword.filestorage_full}} mit cPanel zu speichernden Sicherungen bereitgestellt. Dabei wird angenommen, dass root- oder sudo SSH- sowie ein vollständiger WHM-Zugriff (WHM = WebHost Manager) verfügbar ist. Das vorliegende Beispiel basiert auf einem **CentOS 7**-Host.

**Anmerkung**: Die Dokumentation zu cPanel zum Konfigurieren eines Sicherungsverzeichnisses finden Sie [hier](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){:new_window}.

1. Stellen Sie über SSH eine Verbindung zum Host her.

2. Stellen Sie sicher, dass ein Mountpunktziel vorhanden ist.<br />
   **Hinweis**: Standardmäßig speichert das cPanel-System Sicherungsdateien lokal im Verzeichnis `/backup`. In diesem Dokument wird vorausgesetzt, dass `/backup` vorhanden ist und Sicherungen enthält, daher wird als neuer Mountpunkt `/backup2` verwendet.
   
3. Konfigurieren Sie Ihre {{site.data.keyword.filestorage_short}}-Instanz anhand der Beschreibung in [Zugriff auf {{site.data.keyword.filestorage_short}} unter Red Hat Enterprise Linux](accessing-file-storage-linux.html) und [Mount für NFS/{{site.data.keyword.filestorage_short}} in CentOS](mounting-nsf-file-storage.html). Hängen Sie den Datenträger an `/backup2` an und konfigurieren Sie ihn in der Dateisystemtabelle `/etc/fstab`, um den Mount während des Bootvorgangs zu ermöglichen. <br />
   **Anmerkung**: Standardmäßig stuft NFS Dateien, die mit Rootberechtigung erstellt wurden, auf den Benutzer 'nobody' herab. Damit Root-Clients die Rootberechtigung für die gemeinsam genutzte NFS-Ressource behalten können, muss `no_root_squash` zu `/etc/exports` hinzugefügt werden. <br />
   **Anmerkung**: Anweisungen zum [Mount für NFS/{{site.data.keyword.filestorage_short}} unter CoreOS](mounting-storage-coreos.html) stehen ebenfalls zur Verfügung. <br />

4. **Optional**: Kopieren Sie die vorhandenen Sicherungen in den neuen Speicher. Verwenden Sie `rsync`. Beispiel:
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}
    
    **Anmerkung**: Dieser Befehl überträgt Ihre Daten in komprimierter Form, wobei möglichst viel beibehalten wird (außer festen Verbindungen). Außerdem werden Informationen zu den zu übertragenden Dateien sowie am Ende eine kurze Zusammenfassung bereitgestellt.
    
5. Melden Sie sich bei WebHost Manager an und navigieren Sie über **Home** > **Sicherung** > **Sicherungskonfiguration** zur Sicherungskonfiguration.

6. Bearbeiten Sie die Konfiguration so, dass die Sicherungen am neuen Mountpunkt gespeichert werden. 
    - Ändern Sie das standardmäßige Sicherungsverzeichnis, indem Sie anstelle des Verzeichnisses `/backup/` den absoluten Pfad zur neuen Position eingeben. 
    - Wählen Sie die Option **Anhängen eines Sicherungslaufwerks aktivieren**. Diese Einstellung veranlasst den Konfigurationsprozess, die Datei `/etc/fstab` auf einen Sicherungsmount (`/backup2`) zu überprüfen. <br /> **Anmerkung**: Wenn ein Mount mit demselben Namen wie das Staging-Verzeichnis vorhanden ist, hängt der Sicherungskonfigurationsprozess das Laufwerk an und sichert dort die Informationen. Nach Abschluss des Sicherungsprozesses wird das Laufwerk abgehängt. 

7. Wenden Sie die Änderungen an, indem Sie unten in der Schnittstelle **Sicherungskonfiguration** auf **Konfiguration speichern** klicken.

8. **Optional**: Entfernen Sie je nach Ihrem konkreten Anwendungsfall und Ihren Geschäftsanforderungen den alten Speicher vom Server und brechen Sie das Konto ab.
