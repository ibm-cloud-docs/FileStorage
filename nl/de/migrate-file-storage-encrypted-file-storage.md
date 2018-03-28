---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
 
# {{site.data.keyword.filestorage_short}}-Speicher auf verschlüsselten {{site.data.keyword.filestorage_short}}-Speicher migrieren

Encrypted {{site.data.keyword.filestorage_full}} for Endurance oder Performance wurde in ausgewählten Rechenzentren gestartet. Die folgenden Informationen erläutern, wie Ihr {{site.data.keyword.filestorage_short}} von der unverschlüsselten Version auf die verschlüsselte Version migriert wird. Weitere Informationen zu verschlüsseltem und durch den Provider verwalteten Speicher finden Sie im Artikel [{{site.data.keyword.filestorage_short}}-Verschlüsselung ruhender Daten](block-file-storage-encryption-rest.html). Eine Liste der aktualisierten Rechenzentren und verfügbaren Features können Sie anzeigen, indem Sie [hier klicken](new-ibm-block-and-file-storage-location-and-features).

Der bevorzugte Migrationspfad besteht darin, beide Datenträger gleichzeitig zu verbinden und Daten direkt von dem einen Dateidatenträger auf den anderen zu übertragen. Die jeweiligen Details hängen dabei vom Betriebssystem ab sowie davon, ob erwartet wird, dass sich die Daten während der Kopieroperation ändern.

Die etwas gängigeren Szenarios werden zur Veranschaulichung kurz dargestellt. Dabei wird davon ausgegangen, dass Sie Ihren nicht verschlüsselten Dateidatenträger bereits an Ihren Host angehängt haben. Wenn dies nicht der Fall ist, führen Sie von den nachfolgenden Anweisungen diejenigen aus, die am besten zu dem Betriebssystem passen, das Sie ausführen, um diese Task zu erledigen. 

**Anmerkung:** Alle verschlüsselten {{site.data.keyword.filestorage_short}}-Datenträger haben einen anderen Mountpunkt als nicht verschlüsselte Datenträger. Um sicherzustellen, dass Sie den richtigen Mountpunkt für Ihre verschlüsselten und nicht verschlüsselten {{site.data.keyword.filestorage_short}}-Datenträger verwenden, können Sie die Mountpunktinformationen auf der Seite **Datenträgerdetails** in der Benutzerschnittstelle abrufen sowie auf den richtigen Mountpunkt durch einen API-Aufruf zugreifen: SoftLayer_Network_Storage::getNetworkMountAddress().

[Auf {{site.data.keyword.filestorage_short}} unter Linux zugreifen](accessing-file-storage-linux.html)

## Verschlüsselten Dateidatenträger erstellen

Führen Sie die folgenden Schritte aus, um einen gleichgroßen oder größeren Datenträger zu erstellen, der verschlüsselt wird, um den Migrationsprozess zu ermöglichen.

### Verschlüsselten Endurance-Speicherdatenträger bestellen

1. Klicken Sie auf **Speicher** > **{{site.data.keyword.filestorage_short}}** auf der [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}-Startseite oder klicken Sie auf **Infrastruktur** > **Speicher** > **{{site.data.keyword.filestorage_short}}** im Katalog von {{site.data.keyword.BluSoftlayer_full}}.

2. Klicken Sie auf den Link **{{site.data.keyword.filestorage_short}} bestellen** auf der {{site.data.keyword.filestorage_short}}-Seite.

3. Wählen Sie **Endurance** aus.

4. Wählen Sie das Rechenzentrum aus, in dem sich Ihr ursprünglicher Datenträger befindet. Beachten Sie, dass die Verschlüsselung nur in Rechenzentren verfügbar ist, die mit einem Stern gekennzeichnet sind.

5. Geben Sie die gewünschte **IOPS-Stufe** ein.

6. Wählen Sie die gewünschte Größe des Speicherbereichs in GB aus. Bei TB ist 1 TB gleich 1.000 GB und 12 TB sind gleich 12.000 GB.

7. Geben Sie die gewünschte Größe des Speicherbereichs für Snapshots in GB ein.

8. Wählen Sie das **VMware-Betriebssystem** (VMware OS) in der Dropdown-Liste aus.

9. Schicken Sie die Bestellung ab.
 
### Verschlüsselten Performance-Datenträger bestellen

1. Klicken Sie auf **Speicher** > **{{site.data.keyword.filestorage_short}}** auf der [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}-Startseite oder klicken Sie auf **Infrastruktur** > **Speicher** > **{{site.data.keyword.filestorage_short}}** im Katalog von {{site.data.keyword.BluSoftlayer_full}}.

2. Klicken Sie auf den Link **{{site.data.keyword.filestorage_short}} bestellen**.

3. Wählen Sie **Performance** aus.

4. Wählen Sie das Rechenzentrum aus, in dem sich Ihr ursprünglicher Datenträger befindet. Beachten Sie, dass die Verschlüsselung nur in Rechenzentren verfügbar ist, die mit einem Stern (`*`) gekennzeichnet sind.

5. Wählen Sie die gewünschte Größe des Speicherbereichs in GB aus. Die Größe kann mit dem ursprünglichen Datenträger übereinstimmen oder größer sein.

6. Geben Sie den gewünschten Wert für IOPS (E/A-Operationen pro Sekunde) in Intervallen von 100 ein.

7. Wählen Sie das VMware-Betriebssystem (VMware OS) in der Dropdown-Liste aus.

8. Schicken Sie die Bestellung ab.

Speicher wird in weniger als einer Minute bereitgestellt und auf der {{site.data.keyword.filestorage_short}}-Seite des Kundenportals angezeigt.

 
## Neuen Datenträger mit Host verbinden

Berechtigte (“autorisierte”) Hosts sind Hosts, denen Zugriffsrechte für einen Datenträger erteilt wurden. Ohne die Hostberechtigung können Sie über Ihr System nicht auf den Speicher zugreifen und ihn nicht verwenden.

1. Klicken Sie auf den **Datenträgernamen** Ihres verschlüsselten Datenträgers.

2. Blättern Sie zum Abschnitt **Autorisierte Hosts** auf der Seite.

3. Klicken Sie auf den Link **Host autorisieren** auf der rechten Seite der Anzeige. Wählen Sie die Hosts aus, die auf den Datenträger zugreifen können sollen.

Verbinden Sie den Datenträger nach der Berechtigung mit Ihrem Host.

 
## Snapshots und Replikation

Haben Sie Snapshots und eine Replikation für Ihren ursprünglichen Datenträger eingerichtet? Ist dies der Fall, müssen Sie für den neuen verschlüsselten Datenträger die Replikation und den Snapshotbereich mit denselben Einstellungen wie für den ursprünglichen Datenträger einrichten und Snapshotpläne erstellen. 

Beachten Sie, dass Sie in einem Rechenzentrum, das nicht für die Verschlüsselung aktualisiert wurde, erst dann für den neuen Datenträger eine Replikation einrichten können, wenn das Rechenzentrum aktualisiert wird.

 
## Daten migrieren

Ihr Host muss sowohl mit dem ursprünglichen Datenträger als auch mit dem verschlüsselten {{site.data.keyword.filestorage_short}}-Datenträger verbunden sein. Wenn dies nicht der Fall ist, gehen Sie wie folgt vor:

• Stellen Sie sicher, dass Sie die obigen Schritte ausgeführt haben und die angegebenen Dokumente korrekt beachtet haben.

• Öffnen Sie ein Support-Ticket, um weitere Unterstützung beim Herstellen von Verbindungen der beiden Datenträger zu Ihrem Host anzufordern.

### Datenaspekte

Berücksichtigen Sie an diesem Punkt, welchen Typ von Daten Sie auf Ihrem ursprünglichen {{site.data.keyword.filestorage_short}}-Datenträger haben und wie die Daten am besten auf Ihren verschlüsselten Datenträger kopiert werden könnten. Wenn Sie Sicherungsdaten, statische Inhalte und Daten haben, von denen nicht zu erwarten ist, dass sie sich während des Kopierens ändern, sind weiter keine wichtigen Gesichtspunkte zu beachten.

Wenn Sie eine Datenbank oder eine virtuelle Maschine auf Ihrem {{site.data.keyword.filestorage_short}}-Speicher betreiben, stellen Sie sicher, dass die Daten auf dem ursprünglichen Datenträger während des Kopiervorgangs nicht geändert werden, sodass es zu keiner Datenbeschädigung kommt. Wenn Sie Bandbreitenprobleme befürchten, sollten Sie die Migration außerhalb der Stoßzeiten durchführen. Wenn Sie Hilfe im Hinblick auf diese Aspekte benötigen, öffnen Sie ein entsprechendes Support-Ticket.

### Microsoft Windows

Zum Kopieren von Daten von Ihrem ursprünglichen {{site.data.keyword.filestorage_short}}-Datenträger auf Ihren verschlüsselten Datenträger formatieren Sie den neuen Datenträger und kopieren die Dateien mithilfe von Windows Explorer hinüber.

### Linux

Sie können die Verwendung von 'rsync' zum Kopieren der Daten in Betracht ziehen. Das folgende Beispiel zeigt eine Verwendung des Befehls:

`[root@server ~]# rsync -Pavzu /Pfad/zu/ursprünglichem/Dateispeicher/* /Pfad/zu/verschlüsseltem/Dateispeicher` 

Es wird empfohlen, den obigen Befehl einmal mit dem Flag `--dry-run` zu verwenden, um die korrekte Zusammenstellung der Pfade sicherzustellen. Wenn dieser Prozess unterbrochen wird, können Sie die letzte Zieldatei, die gerade kopiert wurde, löschen, um sicherzustellen, dass sie von Anfang an an die neue Position kopiert wird.

Wenn dieser Befehl ohne das Flag `--dry-run` ausgeführt wurde, sollten Ihre Daten auf den verschlüsselten {{site.data.keyword.filestorage_short}}-Datenträger kopiert worden sein. Blättern Sie nach oben und führen Sie den Befehl erneut aus, um sicherzustellen, dass keine Daten ausgelassen wurden. Sie können außerdem beide Position manuell prüfen, um nach möglicherweise fehlenden Daten zu suchen.

Wenn Ihre Migration abgeschlossen ist, können Sie die Produktion auf den verschlüsselten Datenträger verlegen und Ihren ursprünglichen Datenträger abhängen und aus der Konfiguration löschen. Beachten Sie, dass das Löschen auch alle Snapshots und Replikate am Zielstandort entfernt, der dem ursprünglichen Datenträger zugeordnet war.
