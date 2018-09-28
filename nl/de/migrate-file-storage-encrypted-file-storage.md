---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-29"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
 
# {{site.data.keyword.filestorage_short}}-Speicher auf erweiterten {{site.data.keyword.filestorage_short}}-Speicher migrieren

In ausgewählten Rechenzentren ist nun erweiterter {{site.data.keyword.filestorage_full}}-Speicher verfügbar. Sie können die Liste der aktualisierten Rechenzentren und verfügbaren Features wie beispielsweise anpassbare IOPS-Raten und erweiterbare Datenträger anzeigen, indem Sie [hier klicken](new-ibm-block-and-file-storage-location-and-features.html). Weitere Informationen zu providerverwaltetem verschlüsselten Speicher finden Sie im Artikel [{{site.data.keyword.filestorage_short}}-Verschlüsselung ruhender Daten](block-file-storage-encryption-rest.html).

Der bevorzugte Migrationspfad besteht darin, beide Datenträger gleichzeitig zu verbinden und Daten direkt von einer LUN auf die andere zu übertragen. Die jeweiligen Details hängen dabei vom Betriebssystem ab sowie davon, ob erwartet wird, dass sich die Daten während der Kopieroperation ändern. 

Dabei wird davon ausgegangen, dass Sie Ihre nicht verschlüsselte LUN bereits an Ihren Host angehängt haben. Ist dies nicht der Fall, folgen Sie den für Ihr Betriebssystem passenden Anweisungen, um diese Task auszuführen.

- [{{site.data.keyword.filestorage_short}} unter Linux anhängen](accessing-file-storage-linux.html)
- [NFS/{{site.data.keyword.filestorage_short}} in CentOS anhängen](mounting-nsf-file-storage.html)
- [{{site.data.keyword.filestorage_short}} unter CoreOS anhängen](mounting-storage-coreos.html)

>**Anmerkung** - Alle erweiterten {{site.data.keyword.filestorage_short}}-Datenträger haben einen anderen Mountpunkt als nicht verschlüsselte Datenträger. Um sicherzustellen, dass Sie für beide {{site.data.keyword.filestorage_short}}-Datenträger den richtigen Mountpunkt verwenden, können Sie die Mountpunktinformationen auf der Seite **Datenträgerdetails** im {{site.data.keyword.slportal}} anzeigen. Sie können auch über einen API-Aufruf auf den richtigen Mountpunkt zugreifen: `SoftLayer_Network_Storage::getNetworkMountAddress()`.


## Neuen {{site.data.keyword.filestorage_short}}-Speicher erstellen

**Wichtig** - Geben Sie beim Abschicken einer Bestellung über die API 'Storage as a Service' an, um sicherzustellen, dass Sie Ihren neuen Speicher mit den aktualisierten Funktionen erhalten.

Folgende Anweisungen gelten für die Bestellung von erweiterten Datenträgern/gemeinsam genutzten Dateispeichern über den {{site.data.keyword.slportal}}/{{site.data.keyword.BluSoftlayer_full}}-Katalog. Damit die Migration ermöglicht wird, muss Ihr neuer Datenträger dieselbe Größe wie der ursprüngliche haben oder größer sein.

### Neuen Endurance-Speicherdatenträger bestellen

1. Klicken Sie im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} auf **Speicher** > **{{site.data.keyword.filestorage_short}}** oder im Katalog von {{site.data.keyword.BluSoftlayer_full}} auf **Infrastruktur** > **Speicher** > **{{site.data.keyword.filestorage_short}}**.
2. Klicken Sie auf **{{site.data.keyword.filestorage_short}} bestellen**. 
3. Wählen Sie **Endurance** in der Liste **Speichertyp auswählen** aus.
4. Klicken Sie auf **Position** und wählen Sie Ihr Rechenzentrum aus.
   - Stellen Sie sicher, dass der neue Speicher an derselben Position hinzugefügt wird, an der sich auch der ursprüngliche Speicher befunden hat.
5. Wählen Sie die Abrechnungsoption aus. Sie können zwischen monatlicher und stündlicher Abrechnung wählen.
6. Klicken Sie auf **Endurance** und wählen Sie die IOPS-Stufe aus.
6. Wählen Sie die Option **Nutzbare Speichergröße** in der Liste aus. Ihr neuer Datenträger muss dieselbe Größe wie der ursprüngliche haben oder größer sein.
7. Wählen Sie (zusätzlich zu Ihrem nutzbaren Bereich) die **Größe des Snapshotbereichs** in der Liste aus.
8. Klicken Sie auf **Weiter**. Es werden die monatlichen und anteilmäßig eingerechneten Gebühren mit einer letzten Gelegenheit zur Prüfung der Bestelldetails angezeigt. Klicken Sie auf **Zurück**, wenn Sie Ihre Bestellung ändern möchten.
9. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Bestellung abschicken**.
 
### Verschlüsselten Performance-Datenträger bestellen

1. Klicken Sie im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} auf **Speicher**, **{{site.data.keyword.filestorage_short}}** oder im Katalog von {{site.data.keyword.BluSoftlayer_full}} auf **Infrastruktur** >** Speicher** > **{{site.data.keyword.filestorage_short}}**.
2. Klicken Sie auf **{{site.data.keyword.filestorage_short}} bestellen**. 
3. Wählen Sie **Performance** in der Liste **Speichertyp auswählen** aus.
4. Klicken Sie auf **Position** und wählen Sie Ihr Rechenzentrum aus.
    -  Stellen Sie sicher, dass der neue Speicher an derselben Position hinzugefügt wird, an der sich auch der ursprüngliche Speicher befunden hat.
5. Wählen Sie die Abrechnungsoptionen aus. Sie können zwischen stündlicher und monatlicher Abrechnung wählen.
6. Wählen Sie das Optionsfeld neben der entsprechenden **Speichergröße** aus.
6. Geben Sie die E/A-Operationen pro Sekunde (IOPS) in das Feld **IOPS angeben** ein.
7. Klicken Sie auf **Weiter**. Es werden die monatlichen und anteilmäßig eingerechneten Gebühren mit einer letzten Gelegenheit zur Prüfung der Bestelldetails angezeigt. Klicken Sie auf **Zurück**, wenn Sie Ihre Bestellung ändern möchten.
8. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Bestellung abschicken**.

Der Speicher wird in weniger als einer Minute bereitgestellt und auf der Seite {{site.data.keyword.filestorage_short}} im {{site.data.keyword.slportal}} angezeigt.

 
## Host für die neue {{site.data.keyword.filestorage_short}}-Instanz autorisieren

Berechtigte ("autorisierte") Hosts sind Hosts, denen Zugriff auf einen bestimmten Datenträger erteilt wurde. Ohne die Hostberechtigung können Sie nicht über Ihr System auf den Speicher zugreifen oder ihn verwenden.

1. Klicken Sie auf den Namen des neuen Datenträgers.
2. Blättern Sie zum Abschnitt **Autorisierte Hosts**.
3. Klicken Sie rechts auf den Link **Host autorisieren**. Wählen Sie die Hosts aus, die auf den Datenträger zugreifen können sollen.

Verbinden Sie den Datenträger mit Ihrem Host, nachdem diesem die Berechtigung erteilt wurde.

 
## Snapshots und Replikation einrichten

Wenn Snapshots und Replikation für Ihren ursprünglichen Datenträger erstellt wurden, müssen Sie sie für den neuen Datenträger einrichten. Konfigurieren Sie die Replikation und den Snapshotbereich und erstellen Sie Snapshotpläne mit denselben Einstellungen wie für den ursprünglichen Datenträger. 

>**Anmerkung** - Wenn Ihr Zieldatenzentrum keine Verschlüsselung besitzt, können Sie die Replikation für den neuen Datenträger erst einrichten, wenn das Rechenzentrum aktualisiert wurde.

 
## Daten migrieren

1. Stellen Sie eine Verbindung zu Ihren ursprünglichen und neuen {{site.data.keyword.filestorage_short}}-Datenträgern her. 
  - Öffnen Sie ein Support-Ticket, wenn Sie Hilfe beim Herstellen einer Verbindung zwischen den beiden gemeinsam genutzten Dateispeichern und Ihrem Host benötigen.

2. Überlegen Sie, welchen Typ von Daten Sie auf Ihrem ursprünglichen {{site.data.keyword.filestorage_short}}-Datenträger haben und wie die Daten am besten in den neuen gemeinsam genutzten Dateispeicher kopiert werden könnten. 
  - Wenn Sie Sicherungsdaten, statische Inhalte und Daten haben, von denen nicht zu erwarten ist, dass sie sich während des Kopierens ändern, sind weiter keine wichtigen Punkte zu beachten.
  - Wenn Sie eine Datenbank oder eine virtuelle Maschine auf Ihrem {{site.data.keyword.filestorage_short}}-Speicher ausführen, müssen Sie sicherstellen, dass die Daten während des Kopiervorgangs nicht geändert werden, um Datenbeschädigung zu vermeiden. Wenn Sie Bandbreitenprobleme befürchten, führen Sie die Migration in Zeiten geringer Systemauslastung aus. Öffnen Sie ein Support-Ticket, wenn Sie Hilfe im Hinblick auf diese Aspekte benötigen.
 
3. Kopieren Sie Ihre Daten.
   - **Microsoft Windows** 
     - Zum Kopieren von Daten von Ihrer ursprünglichen {{site.data.keyword.filestorage_short}}-LUN auf Ihre neue LUN formatieren Sie den neuen Datenträger und kopieren die Dateien mithilfe von Windows Explorer hinüber.
   - **Linux** 
     - Sie können die Daten mit `rsync` kopieren.
       ```
       [root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage
       ```
   
   Es ist sinnvoll, den vorigen Befehl einmal mit dem Flag `--dry-run` zu verwenden, um die korrekte Zusammenstellung der Pfade sicherzustellen. Wenn dieser Prozess unterbrochen wird, können Sie die letzte zu kopierende Zieldatei löschen, um sicherzustellen, dass sie von Anfang an an die neue Position kopiert wird.<br/>
   Wenn dieser Befehl ohne das Flag `--dry-run` ausgeführt wird, werden Ihre Daten auf den neuen {{site.data.keyword.filestorage_short}}-Datenträger kopiert. Führen Sie den Befehl erneut aus, um sicherzustellen, dass keine Daten ausgelassen wurden. Sie können außerdem beide Position manuell prüfen, um nach möglicherweise fehlenden Daten zu suchen.<br/>
   Nach Abschluss der Migration können Sie die Produktion auf die neue LUN verlegen. Anschließend können Sie Ihren ursprünglichen Datenträger abhängen und ihn aus der Konfiguration löschen. Durch das Löschen werden auf der Zielseite auch alle Snapshots oder Replikate gelöscht, die dem ursprünglichen Datenträger zugeordnet waren.
