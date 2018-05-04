---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Snapshots

Snapshots sind eine Funktion von {{site.data.keyword.filestorage_full}}. Ein Snapshot stellt den Inhalt eines Datenträgers zu einem bestimmten Zeitpunkt dar. Mithilfe von Snapshots können Sie Ihre Daten ohne Auswirkungen auf die Leistung unter minimalem Speicherplatzbedarf schützen. Daher sind Snapshots als erste Verteidigungslinie für den Schutz Ihrer Daten zu betrachten. Mit der Snapshotfunktion lassen sich Daten problemlos und schnell aus einer Snapshotkopie wiederherstellen, falls ein Benutzer wichtige Daten versehentlich ändert oder von einem Datenträger löscht.

{{site.data.keyword.filestorage_short}} stellt zwei Möglichkeiten zur Erstellung von Snapshots bereit – durch einen konfigurierbaren Snapshotplan, der Snapshotkopien für jeden Speicherdatenträger automatisch erstellt und löscht. Sie können außerdem weitere Snapshotpläne erstellen, Kopien manuell löschen und Zeitpläne Ihren Anforderungen entsprechend verwalten. Die zweite Möglichkeit ist die Erstellung eines manuellen Snapshots.

Eine Snapshotkopie ist ein schreibgeschütztes Image eines {{site.data.keyword.filestorage_short}}-Datenträgers, das den Zustand des Datenträgers zu einem Zeitpunkt erfasst. Snapshotkopien sind sowohl hinsichtlich ihrer Erstellung als auch hinsichtlich ihres Speicherplatzbedarfs extrem effizient. Die Erstellung einer {{site.data.keyword.filestorage_short}}-Snapshotkopie dauert nur einige Sekunden - in der Regel sogar weniger als 1 Sekunde, unabhängig von der Größe des Datenträgers oder des Aktivitätsniveaus auf dem Speicher. Nach der Erstellung einer Snapshotkopie werden Änderungen an Datenobjekten in Aktualisierungen (Updates) an der aktuellen Version der Objekte verfolgt, so als ob keine Snapshotkopien vorhanden wären. In der Zwischenzeit bleibt die Kopie der Daten vollständig stabil. 

Eine Snapshotkopie verursacht keine Leistungseinbuße. Benutzer können leicht bis zu 50 geplante Snapshots und 50 manuelle Snapshots pro {{site.data.keyword.blockstorageshort}}-Datenträger speichern, die alle als schreibgeschützte Versionen und Online-Versionen der Daten zugänglich sind.

Snapshots bieten Benutzern die folgenden Möglichkeiten:

- Erstellung ohne Betriebsunterbrechung von zeitpunktbezogenen Recovery-Punkten
- Zurücksetzung von Datenträgern auf frühere Zeitpunkte
- Sie müssen eine gewisse Menge an Snapshotbereich für Ihren Datenträger kaufen, um Snapshots des Datenträgers erstellen zu können. Der Snapshotbereich kann bei der ersten Datenträgerbestellung oder nach der Erstbereitstellung über die Seite der Datenträgerdetails durch Klicken auf die Dropdown-Schaltfläche 'Aktionen' und Auswählen der Option 'Snapshotbereich hinzufügen' hinzugefügt werden. Beachten Sie, dass geplante und manuelle Snapshots den Snapshotbereich gemeinsam nutzen, sodass Sie Ihren Bereich dementsprechend bestellen müssen. Im Artikel [Snapshots bestellen](ordering-snapshots.html) finden Sie dazu genauere Informationen und Anweisungen.

## Bewährte Verfahren für Snapshots
Die Snapshotgestaltung hängt von der Umgebung des Kunden ab. Die folgenden Gestaltungsaspekte sollen Ihnen bei der Planung und Implementierung von Snapshotkopien helfen: 
- 	Bis zu 50 Snapshots können durch einen Zeitplan und bis zu 50 Snapshots können manuell auf jedem Datenträger bzw. jeder LUN erstellt werden. 
- 	Erstellen Sie nicht zu viele Snapshots. Stellen Sie sicher, dass die geplante Snapshothäufigkeit Ihren RTO- und RPO-Anforderungen sowie Ihren Geschäftsanforderungen für Anwendungen entspricht, indem Sie stündliche, tägliche oder wöchentliche Snapshots planen. 
- 	Die Funktion zum automatischen Löschen von Snapshots (Snapshot AutoDelete) sollte zur Begrenzung des Speicherbelegung genutzt werden. <br/>
    **Anmerkung:** Der AutoDelete-Schwellenwert ist auf 95% festgelegt.
    
## Auf welche Weise belegen Snapshotkopien Plattenspeicherplatz?
Snapshotkopien minimieren die Plattenbelegung, indem sie einzelne Blöcke und keine ganzen Dateien beibehalten. Snapshotkopien beginnen mit der Belegung zusätzlichen Speicherbereichs erst dann, wenn Dateien im aktiven Dateisystem geändert oder gelöscht werden. Wenn dies geschieht, werden die ursprünglichen Dateiblöcke in einer oder mehreren Snapshotkopien beibehalten.
Im aktiven Dateisystem werden die geänderten Blöcke neu an verschiedene Positionen auf der Platte geschrieben oder vollständig als aktive Dateiblöcke entfernt. Das heißt im Ergebnis, dass neben dem Plattenspeicherplatz, der von Blöcken im geänderten aktiven Dateisystem belegt wird, der Plattenspeicherplatz, der von den ursprünglichen Blöcken belegt wird, weiterhin reserviert bleibt, um den Status des aktiven Dateisystems vor der Änderungen zu behalten.

<table>
    <colgroup>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
    </colgroup>
    <tbody>
      <tr>
        <th colspan="3" style="border: 0.0px;text-align: center;">Plattenspeicherplatzbelegung vor und nach einer Snapshotkopie</th>
     </tr><tr>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle1.png" alt="Vor der Snapshotkopie"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle3.png" alt="Nach der Snapshotkopie"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle2.png" alt="Änderungen nach der Snapshotkopie"></td>
     </tr><tr>
        <td style="border: 0.0px;">Vor der Erstellung einer Snapshotkopie wird nur vom aktiven Dateisystem Plattenspeicher belegt.</td>
        <td style="border: 0.0px;">Wenn eine Snapshotkopie erstellt ist, verweisen das aktive Dateisystem und die Snapshotkopie auf dieselben Plattenblöcke. Die Snapshotkopie belegt keinen zusätzlichen Plattenspeicher.</td>
        <td style="border: 0.0px;">Wenn die Datei <i>myfile.txt</i> aus dem aktiven Dateisystem gelöscht wird, schließt die Snapshotkopie die Datei weiterhin ein und verweist auf ihre Plattenblöcke. Dies ist der Grund, aus dem beim Löschen von Daten des aktiven Dateisystems nicht immer Plattenspeicher freigegeben wird.</td>
      </tr>
    </tbody>
</table>

Führen Sie die Anweisungen im Dokument [Snapshots verwalten](working-with-snapshots.html) aus, um anzuzeigen, wie viel Snapshotbereich verwendet wird.
