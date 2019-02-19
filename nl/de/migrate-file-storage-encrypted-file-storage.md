---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.filestorage_short}}-Speicher auf erweiterten {{site.data.keyword.filestorage_short}}-Speicher migrieren
{: #migratestorage}

In ausgewählten Rechenzentren ist nun erweiterter {{site.data.keyword.filestorage_full}}-Speicher verfügbar. Wenn Sie eine Liste der aktualisierten Rechenzentren und verfügbaren Funktionen anzeigen möchten, zum Beispiel konfigurierbare IOPS-Raten und erweiterbare Datenträger, klicken Sie [hier](/docs/infrastructure/FileStorage?topic=FileStorage-news). Weitere Informationen zur vom Provider verwalteten Verschlüsselung finden Sie unter [{{site.data.keyword.filestorage_short}}-Verschlüsselung ruhender Daten](/docs/infrastructure/FileStorage?topic=FileStorage-encryption).

Der bevorzugte Migrationspfad besteht darin, beide Datenträger gleichzeitig zu verbinden und Daten direkt von einer LUN auf die andere zu übertragen. Die jeweiligen Details hängen dabei vom Betriebssystem ab sowie davon, ob erwartet wird, dass sich die Daten während der Kopieroperation ändern.

Es wird davon ausgegangen, dass Sie bereits über eine nicht verschlüsselte LUN verfügen, die mit dem Host verbunden ist. Ist dies nicht der Fall, folgen Sie den für Ihr Betriebssystem passenden Anweisungen, um diese Task auszuführen.

- [{{site.data.keyword.filestorage_short}} unter Linux anhängen](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [{{site.data.keyword.filestorage_short}} unter CentOS anhängen](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [{{site.data.keyword.filestorage_short}} unter CoreOS anhängen](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)

Alle erweiterten {{site.data.keyword.filestorage_short}}-Datenträger, die in diesen Rechenzentren bereitgestellt werden, haben einen anderen Mountpunkt als nicht verschlüsselte Datenträger. Um sicherzustellen, dass Sie für beide Speicherdatenträger den richtigen Mountpunkt verwenden, können Sie die Mountpunktinformationen auf der Seite **Datenträgerdetails** in der Konsole anzeigen. Sie können auch über einen API-Aufruf auf den richtigen Mountpunkt zugreifen: `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}


## {{site.data.keyword.filestorage_short}} erstellen

Wenn Sie einen Auftrag mit einer API erteilen, geben Sie das Paket 'Storage as a Service' an, um sicherzustellen, dass Sie die aktualisierten Funktionen mit dem neuen Speicher erhalten.
{:important}

Sie können eine erweiterte LUN über den {{site.data.keyword.BluSoftlayer_full}}-Katalog und das {{site.data.keyword.slportal}} bestellen. Der neue Datenträger muss dieselbe Größe wie die ursprüngliche gemeinsam genutzte Ressource aufweisen oder größer als diese sein, damit die Migration möglich ist.

- [{{site.data.keyword.filestorage_short}} mit vordefinierten IOPS-Tiers (Endurance) bestellen](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole#endurance)
- [{{site.data.keyword.filestorage_short}} mit angepassten IOPS-Raten (Performance) bestellen](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole#performance)

Der neue Speicher ist in einigen Minuten zum Anhängen verfügbar. Er kann in der Ressourcenliste und der {{site.data.keyword.blockstorageshort}}-Liste angezeigt werden.


## Host für die neue {{site.data.keyword.filestorage_short}}-Instanz autorisieren

Berechtigte ("autorisierte") Hosts sind Hosts, denen Zugriff auf einen bestimmten Datenträger erteilt wurde. Ohne die Hostberechtigung können Sie nicht über Ihr System auf den Speicher zugreifen oder ihn verwenden.

1. Klicken Sie auf den Namen des neuen Datenträgers.
2. Blättern Sie zum Abschnitt **Autorisierte Hosts**.
3. Klicken Sie rechts auf den Link **Host autorisieren**. Wählen Sie die Hosts aus, die auf den Datenträger zugreifen können sollen.

Verbinden Sie den Datenträger mit Ihrem Host, nachdem diesem die Berechtigung erteilt wurde.


## Snapshots und Replikation einrichten

Wenn Snapshots und Replikation für Ihren ursprünglichen Datenträger erstellt wurden, müssen Sie sie für den neuen Datenträger einrichten. Konfigurieren Sie die Replikation und den Snapshotbereich und erstellen Sie Snapshotpläne mit denselben Einstellungen wie für den ursprünglichen Datenträger.

Wenn Ihr Zieldatenzentrum keine Verschlüsselung bietet, können Sie die Replikation für den neuen Datenträger erst einrichten, wenn das Rechenzentrum aktualisiert wurde.
{:important}


## Daten migrieren

1. Stellen Sie eine Verbindung zu Ihren ursprünglichen und neuen {{site.data.keyword.filestorage_short}}-Datenträgern her.
  - Öffnen Sie ein Support-Ticket, wenn Sie Hilfe beim Herstellen einer Verbindung zwischen den beiden gemeinsam genutzten Dateispeichern und Ihrem Host benötigen.

2. Überlegen Sie, welchen Typ von Daten Sie auf Ihrem ursprünglichen {{site.data.keyword.filestorage_short}}-Datenträger haben und wie die Daten am besten in den neuen gemeinsam genutzten Dateispeicher kopiert werden könnten.
  - Wenn Sie Sicherungsdaten, statische Inhalte und Daten haben, von denen nicht zu erwarten ist, dass sie sich während des Kopierens ändern, sind keine größeren Überlegungen erforderlich.
  - Wenn Sie eine Datenbank oder eine virtuelle Maschine auf Ihrem {{site.data.keyword.filestorage_short}}-Speicher ausführen, müssen Sie sicherstellen, dass die Daten während des Kopiervorgangs nicht geändert werden, um Datenbeschädigung zu vermeiden.
  - Wenn Sie Bandbreitenprobleme befürchten, führen Sie die Migration in Zeiten geringer Systemauslastung aus.
  - Öffnen Sie ein Support-Ticket, wenn Sie Hilfe im Hinblick auf diese Aspekte benötigen.

3. Kopieren Sie Ihre Daten.
   - **Microsoft Windows**
     - Zum Kopieren von Daten von Ihrer ursprünglichen {{site.data.keyword.filestorage_short}}-LUN auf Ihre neue LUN formatieren Sie den neuen Datenträger und kopieren die Dateien mithilfe von Windows Explorer hinüber.
   - **Linux**
     - Sie können die Daten mit `rsync` kopieren.
       ```
       [root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage
       ```

   Es ist sinnvoll, den vorigen Befehl einmal mit dem Flag `--dry-run` zu verwenden, um die korrekte Zusammenstellung der Pfade sicherzustellen. Wenn dieser Prozess unterbrochen wird, können Sie die letzte zu kopierende Zieldatei löschen, um sicherzustellen, dass sie von Anfang an an die neue Position kopiert wird.

   Wenn dieser Befehl ohne das Flag `--dry-run` ausgeführt wird, werden Ihre Daten auf den neuen {{site.data.keyword.filestorage_short}}-Datenträger kopiert. Führen Sie den Befehl erneut aus, um sicherzustellen, dass keine Daten ausgelassen wurden. Sie können außerdem beide Position manuell prüfen, um nach möglicherweise fehlenden Daten zu suchen.

   Nach Abschluss der Migration können Sie die Produktion auf die neue LUN verlegen. Anschließend können Sie Ihren ursprünglichen Datenträger abhängen und ihn aus der Konfiguration löschen. Durch das Löschen werden auf der Zielseite auch alle Snapshots oder Replikate gelöscht, die dem ursprünglichen Datenträger zugeordnet waren.
