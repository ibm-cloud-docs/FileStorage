---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-24"

keywords: File Storage, file storage, NFS, upgrade, migrate to new

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.filestorage_short}} auf erweiterten {{site.data.keyword.filestorage_short}}-Service migrieren
{: #migratestorage}

Erweiterter {{site.data.keyword.filestorage_full}}-Service ist nun in den meisten [Rechenzentren](/docs/infrastructure/FileStorage?topic=FileStorage-selectDC) verfügbar.

Der bevorzugte Migrationspfad besteht darin, beide Datenträger gleichzeitig zu verbinden und Daten per NFS direkt von einem Datenträger auf den anderen zu übertragen. Die jeweiligen Details hängen dabei vom Betriebssystem ab sowie davon, ob erwartet wird, dass sich die Daten während der Kopieroperation ändern.

Es wird davon ausgegangen, dass Sie bereits über einen nicht verschlüsselten Datenträger verfügen, die mit dem Host verbunden ist. Ist dies nicht der Fall, folgen Sie den für Ihr Betriebssystem passenden Anweisungen, um diese Task auszuführen.

- [{{site.data.keyword.filestorage_short}} unter Linux anhängen](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [{{site.data.keyword.filestorage_short}} unter CentOS anhängen](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [{{site.data.keyword.filestorage_short}} unter CoreOS anhängen](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)

Alle erweiterten {{site.data.keyword.filestorage_short}}-Datenträger, die in diesen Rechenzentren bereitgestellt werden, haben einen anderen Mountpunkt als nicht verschlüsselte Datenträger. Um sicherzustellen, dass Sie für beide Speicherdatenträger den richtigen Mountpunkt verwenden, können Sie die Mountpunktinformationen auf der Seite **Datenträgerdetails** in der Konsole anzeigen. Sie können auch über einen API-Aufruf auf den richtigen Mountpunkt zugreifen: `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}


## {{site.data.keyword.filestorage_short}} erstellen

Wenn Sie einen Auftrag mit einer API erteilen, geben Sie das Paket 'Storage as a Service' an, um sicherzustellen, dass Sie die aktualisierten Funktionen mit dem neuen Speicher erhalten.
{:important}

Sie können einen erweiterten Datenträger über den {{site.data.keyword.cloud}}-Katalog bestellen. Der neue Datenträger muss dieselbe Größe wie die ursprüngliche gemeinsam genutzte Ressource aufweisen oder größer als diese sein, damit die Migration möglich ist.

- [{{site.data.keyword.filestorage_short}} mit vordefinierten IOPS-Tiers (Endurance) bestellen](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole#endurance)
- [{{site.data.keyword.filestorage_short}} mit angepassten IOPS-Raten (Performance) bestellen](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole#performance)

Der neue Speicher ist in einigen Minuten zum Anhängen verfügbar. Er kann in der Ressourcenliste und der {{site.data.keyword.blockstorageshort}}-Liste angezeigt werden.


## Host für die neue {{site.data.keyword.filestorage_short}}-Instanz autorisieren

Berechtigte ("autorisierte") Hosts sind Hosts, denen Zugriff auf einen bestimmten Datenträger erteilt wurde. Ohne die Hostberechtigung können Sie nicht über Ihr System auf den Speicher zugreifen oder ihn verwenden.

1. Rufen Sie in der Konsole **Klassische Infrastruktur**  > **Speicher** > **{{site.data.keyword.filestorage_short}}** auf. 
2. Blättern Sie zu der gemeinsam genutzten Dateiressource, die angehängt werden soll, und klicken Sie auf **...** (Aktionen). Wählen Sie dann **Host autorisieren** aus.
3. Filtern Sie die Liste der verfügbaren Hosts, indem Sie den Gerätetyp, das Teilnetz oder die IP-Adresse auswählen. 

   Wenn die Liste nach Teilnetzen gefiltert wird, handelt es sich bei den angezeigten Teilnetzen um abonnierte Teilnetze, die sich im selben Rechenzentrum befinden wie der Speicherdatenträger.
   {:note}
4. Wählen Sie einen oder mehrere Hosts in der Liste aus und klicken Sie auf **Speichern**.

Verbinden Sie den Datenträger mit Ihrem Host, nachdem diesem die Berechtigung erteilt wurde.


## Snapshots und Replikation einrichten

Wenn Snapshots und Replikation für Ihren ursprünglichen Datenträger erstellt wurden, müssen Sie sie für den neuen Datenträger einrichten. Konfigurieren Sie die Replikation und den Snapshotbereich und erstellen Sie Snapshotpläne mit denselben Einstellungen wie für den ursprünglichen Datenträger.

Wenn Ihr Zieldatenzentrum keine Verschlüsselung bietet, können Sie die Replikation für den neuen Datenträger erst einrichten, wenn das Rechenzentrum aktualisiert wurde.
{:important}


## Daten migrieren

1. Stellen Sie eine Verbindung zu Ihren ursprünglichen und neuen {{site.data.keyword.filestorage_short}}-Datenträgern her.
  - Wenn Sie Unterstützung beim Herstellen der Verbindung für die beiden gemeinsam genutzten Dateiressourcen zum Host benötigen, öffnen Sie ein Support-Ticket. 

2. Überlegen Sie, welchen Typ von Daten Sie auf Ihrem ursprünglichen {{site.data.keyword.filestorage_short}}-Datenträger haben und wie die Daten am besten in den neuen gemeinsam genutzten Dateispeicher kopiert werden könnten.
  - Wenn Sie Sicherungsdaten, statische Inhalte und Daten haben, von denen nicht zu erwarten ist, dass sie sich während des Kopierens ändern, sind keine größeren Überlegungen erforderlich.
  - Wenn Sie eine Datenbank oder eine virtuelle Maschine auf Ihrem {{site.data.keyword.filestorage_short}}-Speicher ausführen, müssen Sie sicherstellen, dass die Daten während des Kopiervorgangs nicht geändert werden, um Datenbeschädigung zu vermeiden.
  - Wenn Sie Bandbreitenprobleme befürchten, führen Sie die Migration in Zeiten geringer Systemauslastung aus.
  - Öffnen Sie ein Support-Ticket, wenn Sie Hilfe im Hinblick auf diese Aspekte benötigen.

3. Kopieren Sie Ihre Daten.
   - **Microsoft Windows**
     - Zum Kopieren von Daten von Ihrem ursprünglichen {{site.data.keyword.filestorage_short}}-Datenträger auf Ihren neuen Datenträger formatieren Sie den neuen Datenträger und kopieren die Dateien mithilfe von Windows Explorer hinüber.
   - **Linux**
     - Sie können die Daten mit `rsync` kopieren.
       ```
       [root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage
       ```

   Es empfiehlt sich, den oben angeführten Befehl einmal mit dem Flag `--dry-run` auszuführen, um sicherzustellen, dass die Pfade korrekt angeordnet sind. Wenn dieser Prozess unterbrochen wird, können Sie die letzte zu kopierende Zieldatei löschen, um sicherzustellen, dass sie von Anfang an an die neue Position kopiert wird.

   Wenn dieser Befehl ohne das Flag `--dry-run` ausgeführt wird, werden Ihre Daten auf den neuen {{site.data.keyword.filestorage_short}}-Datenträger kopiert. Führen Sie den Befehl erneut aus, um sicherzustellen, dass keine Daten ausgelassen wurden. Sie können außerdem beide Position manuell prüfen, um nach möglicherweise fehlenden Daten zu suchen.

   Nach Abschluss der Migration können Sie die Produktion auf den neuen Datenträger verlegen. Anschließend können Sie Ihren ursprünglichen Datenträger abhängen und ihn aus der Konfiguration löschen. Durch das Löschen werden auf der Zielseite auch alle Snapshots oder Replikate gelöscht, die dem ursprünglichen Datenträger zugeordnet waren.
