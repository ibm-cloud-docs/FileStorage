---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
 
# {{site.data.keyword.filestorage_short}}-Speicher auf erweiterten {{site.data.keyword.filestorage_short}}-Speicher migrieren

In ausgewählten Rechenzentren ist nun erweiterter {{site.data.keyword.filestorage_full}}-Speicher verfügbar. Sie können die Liste der aktualisierten Rechenzentren und verfügbaren Features wie beispielsweise anpassbare IOPS-Raten und erweiterbare Datenträger anzeigen, indem Sie [hier klicken](new-ibm-block-and-file-storage-location-and-features.html). Weitere Informationen zu providerverwaltetem verschlüsselten Speicher finden Sie im Artikel [{{site.data.keyword.filestorage_short}}-Verschlüsselung ruhender Daten](block-file-storage-encryption-rest.html).

Der bevorzugte Migrationspfad besteht darin, beide LUNs (LUN – Logical Unit Number) gleichzeitig zu verbinden und Daten direkt von einer LUN auf die andere zu übertragen. Die jeweiligen Details hängen dabei vom Betriebssystem ab sowie davon, ob erwartet wird, dass sich die Daten während der Kopieroperation ändern. 

Dabei wird davon ausgegangen, dass Sie Ihre nicht verschlüsselte LUN bereits an Ihren Host angehängt haben. Ist dies nicht der Fall, folgen Sie den für Ihr Betriebssystem passenden Anweisungen, um diese Task auszuführen:

- [{{site.data.keyword.filestorage_short}} unter Linux anhängen](accessing-file-storage-linux.html)
- [NFS/{{site.data.keyword.filestorage_short}} in CentOS anhängen](mounting-nsf-file-storage.html)
- [{{site.data.keyword.filestorage_short}} unter CoreOS anhängen](mounting-storage-coreos.html)

**Anmerkung:** Alle erweiterten {{site.data.keyword.filestorage_short}}-Datenträger haben einen anderen Mountpunkt als nicht verschlüsselte Datenträger. Um sicherzustellen, dass Sie sowohl für Ihre verschlüsselten als auch Ihre verschlüsselten {{site.data.keyword.filestorage_short}}-Datenträger den richtigen Mountpunkt verwenden, zeigen Sie die Mountpunktinformationen auf der Seite **Datenträgerdetails** in der Benutzerschnittstelle an. Sie können auch über einen API-Aufruf auf den richtigen Mountpunkt zugreifen: `SoftLayer_Network_Storage::getNetworkMountAddress()`.


## Neuen {{site.data.keyword.filestorage_short}}-Speicher migrieren

**Wichtig**: Geben Sie beim Abschicken einer Bestellung über die API 'Storage as a Service' an, um sicherzustellen, dass Sie Ihren neuen Speicher mit den aktualisierten Funktionen erhalten.

Folgende Anweisungen gelten für die Bestellung von erweiterten Datenträgern/Dateifreigaben über die Benutzerschnittstelle. Damit die Migration ermöglicht wird, muss Ihr neuer Datenträger dieselbe Größe wie der ursprüngliche haben oder größer sein.

### Neuen Endurance-Speicherdatenträger bestellen

1. Klicken Sie im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} auf **Speicher** > **{{site.data.keyword.filestorage_short}}** oder im Katalog von {{site.data.keyword.BluSoftlayer_full}} auf **Infrastruktur** > **Speicher** > **{{site.data.keyword.filestorage_short}}**.
2. Klicken Sie in der rechten oberen Ecke auf **{{site.data.keyword.filestorage_short}} bestellen**. 
3. Wählen Sie **Endurance** in der Liste **Speichertyp auswählen** aus.
4. Klicken Sie auf **Position** und wählen Sie Ihr Rechenzentrum aus.
   - Stellen Sie sicher, dass der neue Speicher an derselben Position hinzugefügt wird, an der sich auch der ursprüngliche Speicher befunden hat.
5. Wählen Sie die Abrechnungsoption aus. Sie können zwischen monatlicher und stündlicher Abrechnung wählen.
6. Klicken Sie auf **Endurance** und wählen Sie die IOPS-Stufe aus.
6. Wählen Sie die Option **Nutzbare Speichergröße** in der Liste aus. Ihr neuer Datenträger muss dieselbe Größe wie der ursprüngliche haben oder größer sein. 
7. Wählen Sie **Größe des Snapshotbereichs** in der Dropdown-Liste (zusätzlich zu Ihrem nutzbaren Bereich) aus.
8. Klicken Sie auf **Weiter**. Es werden die monatlichen und anteilmäßig eingerechneten Gebühren mit einer letzten Gelegenheit zur Prüfung der Bestelldetails angezeigt. Klicken Sie auf **Zurück**, wenn Sie Ihre Bestellung ändern möchten.
9. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Bestellung abschicken**.
 
### Verschlüsselten Performance-Datenträger bestellen

1. Klicken Sie im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} auf **Speicher**, **{{site.data.keyword.filestorage_short}}** oder im Katalog von {{site.data.keyword.BluSoftlayer_full}} auf **Infrastruktur** >** Speicher** > **{{site.data.keyword.filestorage_short}}**.
2. Klicken Sie in der rechten oberen Ecke auf **{{site.data.keyword.filestorage_short}} bestellen**. 
3. Wählen Sie **Performance** in der Liste 'Speichertyp auswählen' aus.
4. Klicken Sie auf **Position** und wählen Sie Ihr Rechenzentrum aus.
    -  Stellen Sie sicher, dass der neue Speicher an derselben Position hinzugefügt wird, an der sich auch der ursprüngliche Speicher befunden hat.
5. Wählen Sie die Abrechnungsoptionen aus. Sie können zwischen stündlicher und monatlicher Abrechnung wählen.
6. Wählen Sie das Optionsfeld neben der entsprechenden **Speichergröße** aus.
6. Geben Sie die E/A-Operationen pro Sekunde (IOPS) in das Feld **IOPS angeben** ein.
7. Klicken Sie auf **Weiter**. Es werden die monatlichen und anteilmäßig eingerechneten Gebühren mit einer letzten Gelegenheit zur Prüfung der Bestelldetails angezeigt. Klicken Sie auf **Zurück**, wenn Sie Ihre Bestellung ändern möchten.
8. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Bestellung abschicken**.

Der Speicher wird in weniger als einer Minute bereitgestellt und auf der Seite {{site.data.keyword.filestorage_short}} im {{site.data.keyword.slportal}} angezeigt.

 
## Neuen {{site.data.keyword.filestorage_short}}-Speicher mit Host verbinden

Berechtigte ('autorisierte') Hosts sind Hosts, denen Zugriffsrechte für einen Datenträger erteilt wurden. Ohne die Hostberechtigung können Sie nicht über Ihr System auf den Speicher zugreifen oder ihn verwenden.

1. Klicken Sie auf den Namen des neuen Datenträgers.
2. Blättern Sie zum Abschnitt **Autorisierte Hosts** auf der Seite.
3. Klicken Sie auf den Link **Host autorisieren** auf der rechten Seite der Anzeige. Wählen Sie die Hosts aus, die auf den Datenträger zugreifen können sollen.

Verbinden Sie den Datenträger nach der Erteilung der Berechtigung mit Ihrem Host.

 
## Snapshots und Replikation

Haben Sie Snapshots und eine Replikation für Ihren ursprünglichen Datenträger eingerichtet? Ist dies der Fall, müssen Sie für den neuen verschlüsselten Datenträger die Replikation und den Snapshotbereich mit denselben Einstellungen wie für den ursprünglichen Datenträger einrichten und Snapshotpläne erstellen. 

**Anmerkung**: Wenn Ihr Zieldatenzentrum nicht für die Verschlüsselung aktualisiert wurde, können Sie die Replikation für den neuen Datenträger erst einrichten, wenn das Rechenzentrum aktualisiert wurde.

 
## Daten migrieren

Ihr Host muss sowohl mit dem ursprünglichen Datenträger als auch mit dem verschlüsselten {{site.data.keyword.filestorage_short}}-Datenträger verbunden sein. Wenn dies nicht der Fall ist, gehen Sie wie folgt vor:

- Stellen Sie sicher, dass die in diesem und den angegebenen Dokumenten genannten Schritte ordnungsgemäß ausgeführt wurden.
- Öffnen Sie ein Support-Ticket, um für das Herstellen einer Verbindung zwischen den beiden Datenträgern und dem Host weitere Unterstützung anzufordern.

### Datenaspekte

Berücksichtigen Sie an diesem Punkt, welchen Typ von Daten Sie auf Ihrem ursprünglichen {{site.data.keyword.filestorage_short}}-Datenträger haben und wie die Daten am besten auf Ihren verschlüsselten Datenträger kopiert werden könnten. Wenn Sie Sicherungsdaten, statische Inhalte und Daten haben, von denen nicht zu erwarten ist, dass sie sich während des Kopierens ändern, sind weiter keine wichtigen Gesichtspunkte zu beachten.

Wenn Sie eine Datenbank oder eine virtuelle Maschine auf Ihrem {{site.data.keyword.filestorage_short}}-Speicher ausführen, müssen Sie sicherstellen, dass die Daten auf dem ursprünglichen Datenträger während des Kopiervorgangs nicht geändert werden, sodass es zu keiner Datenbeschädigung kommt. Wenn Sie Bandbreitenprobleme befürchten, sollten Sie die Migration in Zeiten geringer Systemauslastung ausführen. Öffnen Sie ein Support-Ticket, wenn Sie Hilfe im Hinblick auf diese Aspekte benötigen.

### Microsoft Windows

Zum Kopieren von Daten von Ihrem ursprünglichen {{site.data.keyword.filestorage_short}}-Datenträger auf Ihren verschlüsselten Datenträger formatieren Sie den neuen Datenträger und kopieren die Dateien mithilfe von Windows Explorer hinüber.

### Linux

Zum Kopieren der Daten können Sie überlegen, `rsync` zu verwenden. Hier ein Beispielbefehl:

```
[root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage
```

Es wird empfohlen, den Beispielbefehl einmal mit dem Flag `--dry-run` zu verwenden, um die korrekte Zusammenstellung der Pfade sicherzustellen. Wenn dieser Prozess unterbrochen wird, sollten Sie die letzte zu kopierende Zieldatei löschen, um sicherzustellen, dass sie von Anfang an an die neue Position kopiert wird.

Wenn dieser Befehl ohne das Flag `--dry-run` ausgeführt wurde, sollten Ihre Daten auf den verschlüsselten {{site.data.keyword.filestorage_short}}-Datenträger kopiert worden sein. Blättern Sie nach oben und führen Sie den Befehl erneut aus, um sicherzustellen, dass keine Daten ausgelassen wurden. Sie können außerdem beide Position manuell prüfen, um nach möglicherweise fehlenden Daten zu suchen.

Nach Abschluss der Migration können Sie die Produktion auf den verschlüsselten Datenträger verlegen, Ihren ursprünglichen Datenträger abhängen und ihn aus der Konfiguration löschen. 

**Anmerkung**: Durch das Löschen werden auf der Zielseite auch alle Snapshots oder Replikate gelöscht, die dem ursprünglichen Datenträger zugeordnet waren.
