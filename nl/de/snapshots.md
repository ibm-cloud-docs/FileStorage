---

copyright:
  years: 2014, 2018
lastupdated: "2018-08-01"

---
{:new_window: target="_blank"}

# Snapshots

Snapshots sind eine Funktion von {{site.data.keyword.filestorage_full}}. Ein Snapshot stellt den Inhalt eines Datenträgers zu einem bestimmten Zeitpunkt dar. Mithilfe von Snapshots können Sie Ihre Daten ohne Auswirkungen auf die Leistung unter minimalem Speicherplatzbedarf schützen. Daher sind Snapshots als erste Verteidigungslinie für den Schutz Ihrer Daten zu betrachten. Falls ein Benutzer wichtige Daten versehentlich ändert oder von einem Datenträger löscht, lassen sich diese Daten problemlos und schnell aus einer Snapshotkopie wiederherstellen.

{{site.data.keyword.filestorage_short}} stellt zwei Möglichkeiten zur Erstellung von Snapshots bereit. 

– Zum einen durch einen konfigurierbaren Snapshotplan, der Snapshotkopien für jeden Speicherdatenträger automatisch erstellt und löscht. Sie können außerdem weitere Snapshotzeitpläne erstellen, Kopien manuell löschen und Zeitpläne Ihren Anforderungen entsprechend verwalten. 
- Die zweite Möglichkeit ist die Erstellung eines manuellen Snapshots.

Eine Snapshotkopie ist ein schreibgeschütztes Image eines {{site.data.keyword.filestorage_short}}-Datenträgers, das den Zustand des Datenträgers zu einem bestimmten Zeitpunkt erfasst. Snapshotkopien sind sowohl hinsichtlich ihrer Erstellungsdauer als auch hinsichtlich ihres Speicherplatzbedarfs effizient. Die Erstellung einer {{site.data.keyword.filestorage_short}}-Snapshotkopie dauert nur einige Sekunden. In der Regel dauert die Erstellung sogar weniger als 1 Sekunde, unabhängig von der Größe des Datenträgers oder des Aktivitätsniveaus auf dem Speicher. Nach der Erstellung einer Snapshotkopie werden Änderungen an Datenobjekten in Aktualisierungen (Updates) an der aktuellen Version der Objekte abgebildet, so als seien keine Snapshotkopien vorhanden. In der Zwischenzeit bleibt die Kopie der Daten stabil. 

Eine Snapshotkopie verursacht keine Leistungseinbußen. Benutzer können leicht bis zu 50 geplante Snapshots und 50 manuelle Snapshots pro {{site.data.keyword.filestorage_short}}-Datenträger speichern, die alle als schreibgeschützte Versionen und Online-Versionen der Daten zugänglich sind.

Snapshots ermöglichen Folgendes: 

- Erstellung ohne Betriebsunterbrechung von zeitpunktbezogenen Recovery-Punkten
- Zurücksetzung von Datenträgern auf frühere Zeitpunkte

Sie müssen zuerst eine gewisse Menge an Snapshotbereich für Ihren Datenträger kaufen, um Snapshots des Datenträgers erstellen zu können. Der Snapshotbereich kann bei der ersten Bestellung oder im Nachhinein über die Seite für Datenträgerdetails **** hinzugefügt werden. Geplante und manuelle Snapshots nutzen den Snapshotbereich gemeinsam. Stellen Sie daher sicher, dass Sie genug Snapshotbereich bestellen. Im Artikel [Snapshots bestellen](ordering-snapshots.html) finden Sie dazu genauere Informationen und Anweisungen.

**Bewährte Verfahren für Snapshots**

Die Snapshotgestaltung hängt von der Umgebung des Kunden ab. Die folgenden Gestaltungsaspekte können Ihnen bei der Planung und Implementierung von Snapshotkopien helfen: 
- Bis zu 50 Snapshots können durch einen Zeitplan und bis zu 50 Snapshots können manuell auf jedem Datenträger bzw. jeder LUN erstellt werden. 
- Erstellen Sie nicht zu viele Snapshots. Stellen Sie sicher, dass die geplante Snapshothäufigkeit Ihren RTO- und RPO-Anforderungen und Ihren Geschäftsanforderungen für Anwendungen entspricht, indem Sie stündliche, tägliche oder wöchentliche Snapshots planen. 
- Die Funktion zum automatischen Löschen von Snapshots (Snapshot AutoDelete) kann zur Begrenzung der Speicherbelegung genutzt werden. <br/>
  >**Anmerkung** - Der AutoDelete-Schwellenwert ist auf 95 Prozent festgelegt.
    
Snapshots sind kein Ersatz für ausgelagerte Disaster-Recovery-Replikationen oder Sicherungen mit langer Aufbewahrungsdauer.
    
**Auswirkungen von Snapshots auf Plattenspeicher**

Snapshotkopien minimieren die Plattenbelegung, indem sie einzelne Blöcke und keine ganzen Dateien beibehalten. Snapshotkopien belegen erst dann zusätzlichen Speicherbereich, wenn Dateien im aktiven Dateisystem geändert oder gelöscht werden. Wenn dies geschieht, werden die ursprünglichen Dateiblöcke in einer oder mehreren Snapshotkopien beibehalten.
Im aktiven Dateisystem werden die geänderten Blöcke neu an verschiedene Positionen auf der Platte geschrieben oder vollständig als aktive Dateiblöcke entfernt. Das heißt im Ergebnis, dass neben dem Plattenspeicherplatz, der von Blöcken im geänderten aktiven Dateisystem belegt wird, der Plattenspeicherplatz, der von den ursprünglichen Blöcken belegt wird, weiterhin reserviert bleibt, um den Status des aktiven Dateisystems vor der Änderungen zu behalten.

<table>
    <colgroup>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
    </colgroup>
      <tr>
        <th colspan="3" style="border: 0.0px;text-align: center;">Plattenspeicherplatzbelegung vor und nach einer Snapshotkopie</th>
     </tr>
     <tr>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle1.png" alt="Vor der Snapshotkopie"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle3.png" alt="Nach der Snapshotkopie"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle2.png" alt="Änderungen nach der Snapshotkopie"></td>
     </tr>
     <tr>
        <td style="border: 0.0px;">Vor der Erstellung einer Snapshotkopie wird der Plattenspeicher nur vom aktiven Dateisystem verwendet.</td>
        <td style="border: 0.0px;">Wenn eine Snapshotkopie erstellt ist, verweisen das aktive Dateisystem und die Snapshotkopie auf dieselben Plattenblöcke. Die Snapshotkopie belegt keinen zusätzlichen Plattenspeicher.</td>
        <td style="border: 0.0px;">Nach dem Löschen der Datei <i>myfile.txt</i> aus dem aktiven Dateisystem schließt die Snapshotkopie die Datei weiterhin ein und verweist auf ihre Plattenblöcke. Deshalb führt das Löschen von Daten des aktiven Dateisystems nicht immer zur Freigabe von Plattenspeicher.</td>
      </tr>
</table>

Führen Sie die Anweisungen im Artikel [Snapshots verwalten](working-with-snapshots.html) aus, um anzuzeigen, wieviel Snapshotbereich verwendet wird.
