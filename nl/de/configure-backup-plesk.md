---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, Plesk, backups

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.filestorage_short}} für Sicherung mit Plesk konfigurieren
{: #PleskBackup}

Mithilfe dieser Anweisungen können Sie {{site.data.keyword.filestorage_full}} für Ihre Sicherungen in Plesk konfigurieren. Dabei wird angenommen, dass root- oder sudo SSH- sowie ein vollständiger Plesk-Zugriff auf Administratorebene verfügbar ist. Das vorliegende Beispiel basiert auf einem CentOS 7-Host.

Weitere Informationen finden Sie in der Dokumentation von Plesk zum Thema Sicherung und Wiederherstellung unter [Plesk's documentation for backing up and restoration](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){: external}.
{:tip}

1. Stellen Sie über SSH eine Verbindung zum Host her.
2. Stellen Sie sicher, dass ein Mountpunktziel vorhanden ist. <br />

   Plesk bietet zwei Optionen zum Speichern von Sicherungen. Die eine ist der interne Plesk-Speicher, ein Speicher auf Ihrem Plesk-Server. Die andere ist ein externer FTP-Speicher – ein Speicher auf einem externen Server im Web oder in Ihrem lokalen Netz. In der Regel werden interne Sicherungen in Plesk-Fenstern im Pfad `/var/lib/psa/dumps` gespeichert und verwenden `/tmp` als temporäres Verzeichnis. Im vorliegenden Beispiel bleibt das temporäre Verzeichnis lokal, das Verzeichnis `dumps` dagegen wird in das {{site.data.keyword.filestorage_short}}-Ziel (`/backup/psa/dumps`) verschoben. Es sind keine FTP-Benutzerberechtigungsnachweise erforderlich.
   {:note}
3. Konfigurieren Sie Ihre {{site.data.keyword.filestorage_short}}-Instanz anhand der Beschreibung in [Zugriff auf {{site.data.keyword.filestorage_short}} unter Red Hat Enterprise Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux) und [Mount für NFS/{{site.data.keyword.filestorage_short}} in CentOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS) oder [Mount für NFS/{{site.data.keyword.filestorage_short}} in CoreOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS). Hängen Sie den Datenträger an `/backup` an und konfigurieren Sie ihn in der Dateisystemtabelle (`/etc/fstab`), um den Mount während des Startvorgangs zu ermöglichen. <br />

   Standardmäßig stuft NFS alle Dateien, die mit Rootberechtigung erstellt wurden, auf den Benutzer 'nobody' herab. Damit Root-Clients die Rootberechtigung für die gemeinsam genutzte NFS-Ressource behalten können, muss `no_root_squash` zu `/etc/exports` hinzugefügt werden.
   {:tip}
4. **Optional**. Kopieren Sie die vorhandenen Sicherungen in den neuen Speicher. Verwenden Sie `rsync`. Beispiel:
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {:pre}

   Dieser Befehl komprimiert und überträgt Ihre Daten und behält möglichst viel bei (außer festen Verbindungen). Außerdem werden Informationen zu den per NFS zu übertragenden Dateien sowie am Ende eine kurze Zusammenfassung bereitgestellt.
   {:tip}
5. Bearbeiten Sie `/etc/psa/psa.conf` so, dass der Wert für `DUMP_D` auf das neue Ziel verweist.
    - Es sieht wie folgt aus: `DUMP_D /backup/psa/dumps`.
6. **Optional**. Entfernen Sie je nach Ihrem konkreten Anwendungsfall und Ihren Geschäftsanforderungen den alten Speicher vom Server und löschen Sie ihn im Konto.
