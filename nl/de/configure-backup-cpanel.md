---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.filestorage_short}} für Sicherung mit cPanel konfigurieren

Sie können Ihre Sicherungen mithilfe dieser Anweisungen so konfigurieren, dass sie von cPanel in {{site.data.keyword.filestorage_full}} gespeichert werden. Dabei wird angenommen, dass root- oder sudo SSH- sowie ein vollständiger WHM-Zugriff (WHM = WebHost Manager) verfügbar ist. Das vorliegende Beispiel basiert auf einem **CentOS 7**-Host.

Weitere Informationen finden Sie in [cPanel - Configuring backup directory ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){:new_window}.
{:tip}

1. Stellen Sie über SSH eine Verbindung zum Host her.
2. Stellen Sie sicher, dass ein Mountpunktziel vorhanden ist. <br />

   Standardmäßig speichert das cPanel-System Sicherungsdateien lokal im Verzeichnis `/backup`. In diesem Dokument wird vorausgesetzt, dass `/backup` vorhanden ist und Sicherungen enthält. Daher kann als neuer Mountpunkt `/backup2` verwendet werden.
   {:note}

3. Konfigurieren Sie Ihre {{site.data.keyword.filestorage_short}}-Instanz anhand der Beschreibung in [Zugriff auf {{site.data.keyword.filestorage_short}} unter Red Hat Enterprise Linux](accessing-file-storage-linux.html) und [Mount für NFS/{{site.data.keyword.filestorage_short}} in CentOS](mounting-nsf-file-storage.html)/[Mount für NFS/{{site.data.keyword.filestorage_short}} in CoreOS](mounting-storage-coreos.html). Hängen Sie den Datenträger an `/backup2` an und konfigurieren Sie ihn in der Dateisystemtabelle (`/etc/fstab`), um den Mount während des Startvorgangs zu ermöglichen. <br />

   Standardmäßig stuft NFS alle Dateien, die mit Rootberechtigung erstellt wurden, auf den Benutzer 'nobody' herab. Damit Root-Clients die Rootberechtigung für die gemeinsam genutzte NFS-Ressource behalten können, muss `no_root_squash` zu `/etc/exports` hinzugefügt werden.
   {:tip}

4. **Optional**. Kopieren Sie die vorhandenen Sicherungen in den neuen Speicher. Sie können dazu beispielsweise `rsync` verwenden.
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}

    Dieser Befehl komprimiert und überträgt die Daten, wobei möglichst viel (außer festen Verbindungen) beibehalten wird. Außerdem werden Informationen zu den zu übertragenden Dateien sowie am Ende eine kurze Zusammenfassung bereitgestellt.
    {:tip}

5. Melden Sie sich bei WebHost Manager an und navigieren Sie über **Home** > **Sicherung** > **Sicherungskonfiguration** zur Sicherungskonfiguration.

6. Bearbeiten Sie die Konfiguration so, dass die Sicherungen am neuen Mountpunkt gespeichert werden.
    - Ändern Sie das standardmäßige Sicherungsverzeichnis, indem Sie anstelle des Verzeichnisses `/backup/` den absoluten Pfad zur neuen Position eingeben.
    - Wählen Sie die Option **Anhängen eines Sicherungslaufwerks aktivieren**. Diese Einstellung veranlasst den Konfigurationsprozess, die Datei `/etc/fstab` auf einen Sicherungsmount (`/backup2`) zu überprüfen. <br />

      Wenn ein Mount mit demselben Namen wie das Staging-Verzeichnis vorhanden ist, hängt der Sicherungskonfigurationsprozess das Laufwerk an und sichert dort die Informationen. Nach Abschluss des Sicherungsprozesses wird das Laufwerk abgehängt.
      {:note}
7. Wenden Sie die Änderungen an, indem Sie auf **Konfiguration speichern** klicken.
8. **Optional**. Entfernen Sie je nach Ihrem konkreten Anwendungsfall und Ihren Geschäftsanforderungen den alten Speicher vom Server und brechen Sie das Konto ab.
