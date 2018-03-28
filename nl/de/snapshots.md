---

copyright:
  years: 2014, 2017
lastupdated: "2017-12-11"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Mit Snapshots arbeiten

Snapshots sind eine Funktion von {{site.data.keyword.filestorage_full}}. Ein Snapshot stellt den Inhalt eines Datenträgers zu einem bestimmten Zeitpunkt dar. Mithilfe von Snapshots können Sie Ihre Daten ohne Auswirkungen auf die Leistung unter minimalem Speicherplatzbedarf schützen. Daher sind Snapshots als erste Verteidigungslinie für den Schutz Ihrer Daten zu betrachten. Mit der Snapshotfunktion lassen sich Daten problemlos und schnell aus einer Snapshotkopie wiederherstellen, falls ein Benutzer wichtige Daten versehentlich ändert oder von einem Datenträger löscht.

{{site.data.keyword.filestorage_short}} stellt zwei Möglichkeiten zur Erstellung von Snapshots bereit – durch einen konfigurierbaren Snapshotplan, der Snapshotkopien für jeden Speicherdatenträger automatisch erstellt und löscht. Sie können außerdem weitere Snapshotpläne erstellen, Kopien manuell löschen und Zeitpläne Ihren Anforderungen entsprechend verwalten. Die zweite Möglichkeit ist die Erstellung eines manuellen Snapshots.

Eine Snapshotkopie ist ein schreibgeschütztes Image eines {{site.data.keyword.filestorage_short}}-Datenträgers, das den Zustand des Datenträgers zu einem Zeitpunkt erfasst. Snapshotkopien sind sowohl hinsichtlich ihrer Erstellung als auch hinsichtlich ihres Speicherplatzbedarfs extrem effizient. Die Erstellung einer {{site.data.keyword.filestorage_short}}-Snapshotkopie dauert nur einige Sekunden - in der Regel sogar weniger als 1 Sekunde, unabhängig von der Größe des Datenträgers oder des Aktivitätsniveaus auf dem Speicher. Nach der Erstellung einer Snapshotkopie werden Änderungen an Datenobjekten in Aktualisierungen (Updates) an der aktuellen Version der Objekte verfolgt, so als ob keine Snapshotkopien vorhanden wären. In der Zwischenzeit bleibt die Kopie der Daten vollständig stabil. 

Eine Snapshotkopie verursacht keine Leistungseinbuße. Benutzer können leicht bis zu 50 geplante Snapshots und 50 manuelle Snapshots pro {{site.data.keyword.blockstorageshort}}-Datenträger speichern, die alle als schreibgeschützte Versionen und Online-Versionen der Daten zugänglich sind.

Snapshots bieten Benutzern die folgenden Möglichkeiten:

- Erstellung ohne Betriebsunterbrechung von zeitpunktbezogenen Recovery-Punkten
- Zurücksetzung von Datenträgern auf frühere Zeitpunkte
- Sie müssen eine gewisse Menge an Snapshotbereich für Ihren Datenträger kaufen, um Snapshots des Datenträgers erstellen zu können. Der Snapshotbereich kann bei der ersten Datenträgerbestellung oder nach der Erstbereitstellung über die Seite der Datenträgerdetails durch Klicken auf die Dropdown-Schaltfläche 'Aktionen' und Auswählen der Option 'Snapshotbereich hinzufügen' hinzugefügt werden. Beachten Sie, dass geplante und manuelle Snapshots den Snapshotbereich gemeinsam nutzen, sodass Sie Ihren Bereich dementsprechend bestellen müssen.

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



## Wie wird Snapshotbereich gekauft?

Zum automatischen oder manuellen Erstellen von Snapshots für Ihren Speicherdatenträger müssen Sie Speicherbereich kaufen, um diese Snapshots aufzubewahren. Sie können Kapazität bis zur Größe Ihres Speicherdatenträgers kaufen (beim Erstkauf des Datenträgers oder später, indem Sie die nachfolgenden Schritte ausführen).

1. Greifen Sie auf Ihren Speicher über die Registerkarte **Speicher** > **{{site.data.keyword.filestorage_short}}** im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} zu.
2. Klicken Sie im Snapshotrahmen auf den Link 'Snapshotbereich hinzufügen'. Klicken Sie im Snapshotrahmen auf den Link **Snapshotbereich jetzt kaufen**.
3. Wählen Sie die Speichergröße aus, die Sie benötigen, indem auf das Optionsfeld neben dem entsprechenden Wert klicken.
4. Klicken Sie auf **Weiter**.
5. Geben Sie einen Werbeaktionscode ein, wenn Sie einen haben, und klicken Sie auf **Neu berechnen**. Die Gebührenaufstellung für diese Bestellung und die Bestellungsprüfung enthalten Standardwerte.
6. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen…** und klicken Sie auf **Bestellung abschicken**. Ihr Snapshotbereich wird innerhalb weniger Minuten bereitgestellt.

## Wie wird ein Snapshotplan erstellt?

Snapshotpläne bieten die Möglichkeit, selbst zu entscheiden, wie oft und wann eine zeitpunktbezogene Referenz des Speicherdatenträgers erstellt werden soll. Es können maximal 50 Snapshots pro Speicherdatenträger erstellt werden. Zeitpläne werden über die Registerkarte **Speicher** > **{{site.data.keyword.filestorage_short}}** im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} verwaltet.

Bevor Sie Ihren ersten Zeitplan einrichten können, müssen Sie zunächst Snapshotbereich kaufen, sofern Sie diesen nicht bei der Erstbereitstellung des Speicherdatenträgers gekauft haben.

### Snapshotplan hinzufügen

Snapshotpläne können für stündliche, tägliche und wöchentliche Intervalle jeweils mit einem eigenen Aufbewahrungszyklus eingerichtet werden. Es besteht eine Begrenzung auf maximal 50 geplante Snapshots, wobei es sich aus einem Mix von stündlichen, täglichen und wöchentlichen Zeitplänen handeln kann, sowie auf maximal 50 manuelle Snapshots pro Speicherdatenträger.

1. Klicken Sie auf Ihren Speicherdatenträger, klicken Sie auf die Dropdown-Liste **Aktionen** und klicken Sie auf die Option **Snapshot planen**.
2. Im Dialogfenster für den neuen Zeitplansnapshot stehen drei verschiedene Snapshothäufigkeiten zur Auswahl. Verwenden Sie eine beliebige Kombination der drei Optionen, um einen umfassenden Snapshotplan zu erstellen.
   - Stündlich
      - Geben Sie die Minute jeder Stunde an, in der ein Snapshot ausgeführt werden soll. Standardwert ist die aktuelle Minute.
      - Geben Sie die Anzahl der stündlichen Snapshots an, die aufbewahrt werden sollen, bevor der älteste Snapshot gelöscht wird.
   - Täglich
      - Geben Sie die Stunde und die Minute an, in der ein Snapshot ausgeführt werden soll. Standardwert ist die aktuelle Stunde und Minute.
      - Wählen Sie die Anzahl der stündlichen Snapshots an, die aufbewahrt werden sollen, bevor der älteste Snapshot gelöscht wird.
   - Wöchentlich
      - Geben Sie den Wochentag, die Stunde und die Minute des Zeitpunkts an, zu dem ein Snapshot ausgeführt werden soll. Standardwert ist der aktuelle Zeitpunkt (Tag, Stunde und Minute).
      - Wählen Sie die Anzahl der wöchentlichen Snapshots an, die aufbewahrt werden sollen, bevor der älteste Snapshot gelöscht wird.
3. Klicken Sie auf **Speichern** und erstellen Sie einen weiteren Zeitplan mit einer anderen Häufigkeit. Beachten Sie, dass Sie eine Warnung empfangen und nicht speichern können, wenn die Gesamtzahl der geplanten Snapshots über 50 liegt.

Eine Liste der jeweils erfassten Snapshots wird im Snapshotabschnitt der Detailseite angezeigt.

## Wie wird ein manueller Snapshot erstellt?

Manuelle Snapshots können an verschiedenen Punkten während eines Anwendungsupgrades oder einer Wartungsoperation erfasst werden. Darüber hinaus können Sie übergreifende Snapshots für mehrere Maschinen erstellen, die auf der Anwendungsebene zeitweilig inaktiviert wurden.

Die Anzahl manueller Snapshots ist auf maximal 50 pro Speicherdatenträger begrenzt.

1. Klicken Sie auf Ihren Speicherdatenträger.
2. Klicken Sie auf die Dropdown-Liste 'Aktionen'.
3. Klicken Sie auf **Manuellen Snapshot erstellen**.
Der Snapshot wird erstellt und im Snapshotabschnitt der Detailseite angezeigt. Die Zeitplanangabe lautet 'Manuell'.

## Wie kann eine Liste der Snapshots mit belegten Speicherbereicht und Managementfunktionen angezeigt werden?

Eine Liste der aufbewahrten Snapshots und des belegten Speicherbereichs wird auf der Detailseite (**Speicher** > **{{site.data.keyword.filestorage_short}}**) angezeigt. Managementfunktionen (Zeitplanbearbeitung und Hinzufügen weiteren Speicherbereichs) werden auf der Seite **Details** über das Menü **Aktionen** oder über Links in den verschiedenen Abschnitten der Seite ausgeführt.

## Wie wird eine Liste der aufbewahrten Snapshots angezeigt?

Die Anzahl der aufbewahrten Snapshots richtet sich nach dem Wert, den Sie in das Feld **Letzte behalten** bei der Einrichtung Ihrer Zeitpläne eingegeben haben. Die Snapshots, die erfasst wurden, werden im Abschnitt **Snapshot** angezeigt. Die Snapshots werden nach Zeitplan aufgelistet.

## Wie kann der belegte Snapshotbereich geprüft werden?

Das Kreisdiagramm oben auf der Seite **Details** zeigt an, wie viel Speicherbereich belegt ist und wie viel Speicherbereich noch verfügbar ist. Sie empfangen Benachrichtigungen, wenn Schwellenwerte für die Speicherbelegung erreicht werden – 75%, 90% und 95%.

## Wie lässt sich die Größe des Snapshotbereichs für einen Datenträger ändern?

Sie müssen möglicherweise einem Datenträger Snapshotbereich hinzufügen, der zuvor keinen Snapshotbereich hatte oder zusätzlichen Snapshotbereich benötigt. Sie können je nach Bedarf zwischen 5 GB und 4.000 GB anhängen. Anmerkung: Snapshotbereich kann nur vergrößert und nicht verkleinert werden. Sie können zunächst einen kleinere Speichergröße auswählen, bis Sie ermittelt haben, wie viel Speicherbereich Sie tatsächlich benötigen. Beachten Sie, dass automatisierte und manuelle Snapshots den gleichen Speicherbereich gemeinsam nutzen.

Der Snapshotbereich wird über **Speicher** > **{{site.data.keyword.filestorage_short}}** geändert.

1. Klicken Sie auf Ihre Speicherdatenträger, klicken Sie auf die Dropdown-Liste **Aktionen** und klicken Sie auf die Option **Weiteren Snapshotbereich hinzufügen**.
2. Wählen Sie in der Eingabeaufforderung unter eine Reihe von Größe die gewünschte Größe aus. Größen reichen in der Regel von 0 bis zur Größe Ihres Datenträgers.
3. Klicken Sie auf **Weiter**, um den zusätzlichen Speicherbereich bereitzustellen.
4. Geben Sie einen Werbeaktionscode ein, wenn Sie einen haben, und klicken Sie auf **Neu berechnen**. Die Felder **Gebühren für diese Bestellung** und **Bestellprüfung** zeigen Standardwerte an.
5. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen…** und klicken Sie auf **Bestellung abschicken**. Der zusätzliche Snapshotbereich wird innerhalb weniger Minuten bereitgestellt.

## Wie erfolgt die Benachrichtigung, wenn sich der Snapshotbereich der Begrenzung nähert und Snapshots gelöscht werden?

Benachrichtigungen werden über Support-Tickets vom Support an den Masterbenutzer auf Ihrem Konto gesendet, wenn drei verschiedene Speicherschwellenwerte erreicht werden – 75%, 90% und 95%.

- 75% der Kapazität: Eine Warnung wird gesendet, die besagt, dass 75% Speicherbelegung überschritten wurden. Wenn Sie auf die Warnung reagieren und Speicherbereich manuell hinzufügen oder aufbewahrte und unnötige Snapshots löschen, wird die Aktion notiert und das Ticket geschlossen. Wenn Sie nichts unternehmen, müssen Sie das Ticket manuell bestätigen. Das Ticket wird in diesem Fall geschlossen.
- 90% der Kapazität: Eine zweite Warnung wird gesendet, wenn die Belegung des Snapshotbereichs 90% erreicht hat. Wie bei Erreichen von 75% der Kapazität gilt, dass, wenn Sie auf die Warnung reagieren und Speicherbereich manuell hinzufügen oder aufbewahrte und unnötige Snapshots löschen, die Aktion notiert und das Ticket geschlossen wird. Wenn Sie nichts unternehmen, müssen Sie das Ticket manuell bestätigen. Das Ticket wird in diesem Fall geschlossen.
- 95% der Kapazität: Eine letzte Warnung wird gesendet. Wenn keine Aktion ausgeführt wird, um die Speicherbelegung unter den Schwellenwert zu senken, wird eine Benachrichtigung generiert und eine automatische Löschung durchgeführt, sodass zukünftige Snapshots erstellt werden können. Geplante Snapshots werden gelöscht, beginnend mit dem ältesten, bis die Belegung unter 95% liegt. Snapshots werden weiterhin jedesmal gelöscht, wenn die Belegung 95% überschreitet, bis die Belegung unter dem Schwellenwert liegt. Wenn der Speicherbereich manuell vergrößert oder Snapshots gelöscht werden, wird die Warnung zurückgesetzt und erneut ausgegeben, wenn der Schwellenwert wieder überschritten wird. Wenn keine Aktion ausgeführt wird, ist dies die einzige Warnung, die empfangen wird.

## Wie wird ein Screenshotplan gelöscht?

Snapshotpläne können über **Speicher** > **{{site.data.keyword.filestorage_short}}** gelöscht werden.

1. Klicken Sie auf den zu löschenden Zeitplan im Rahmen **Snapshotpläne** auf der Seite **Details**.
2. Klicken Sie auf das Kontrollkästchen neben dem zu löschenden Zeitplan und klicken Sie auf **Speichern**.<br/>
Achtung: Wenn Sie die Replikationsfunktion verwenden, stellen Sie sicher, dass der Zeitplan, den Sie löschen, nicht der von der Replikation verwendete Zeitplan ist. Klicken Sie [hier](replication.html), um weitere Informationen zum Löschen eines Replikationsplans zu erhalten.

## Wie wird ein Snapshot gelöscht?

Snapshots, die nicht mehr benötigt werden, können manuell entfernt werden, um Speicherplatz für zukünftige Snapshots freizugeben. Das Löschen erfolgt über **Speicher** > **{{site.data.keyword.filestorage_short}}**.

1. Klicken Sie auf Ihren Speicherdatenträger und blättern Sie nach unten zum Snapshotabschnitt, um eine Liste der vorhandenen Snapshots anzuzeigen.
2. Klicken Sie auf die Dropdown-Liste 'Aktionen' neben dem gewünschten Snapshot und klicken Sie auf **Löschen**, um den Snapshot zu löschen. Diese Aktion hat keine Auswirkung auf zukünftige oder frühere Snapshots im selben Zeitplan, da zwischen Snapshots keine Abhängigkeit besteht.

Manuelle Snapshots, die nicht mit der oben beschriebenen Methode gelöscht werden, werden automatisch (älteste zuerst) gelöscht, wenn Sie die Speicherbereichsbegrenzung erreichen.

## Wie wird ein Speicherdatenträger mit einem Snapshot auf dem Stand eines bestimmten Zeitpunkts wiederhergestellt?

Es ist möglich, dass Sie Ihren Speicherdatenträger aufgrund eines Benutzerfehlers oder einer Datenbeschädigung auf einen bestimmten Zeitpunkt zurücksetzen müssen.

1. Hängen Sie Ihren Speicherdatenträger vom Host ab und trennen Sie die Verbindung.
   - Klicken Sie hier [hier](accessing-file-storage-linux.html), um weitere Anweisungen für {{site.data.keyword.filestorage_short}} unter Linux anzuzeigen.
2. Klicken Sie auf **Speicher** > **{{site.data.keyword.filestorage_short}}** im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
3. Blättern Sie nach unten und klicken Sie auf den Datenträger, der wiederhergestellt werden soll. Im Snapshotabschnitt der Detailseite wird eine Liste aller gespeicherten Snapshots mit Angabe ihrer Größe und ihres Erstellungsdatums angezeigt.
4. Klicken Sie auf die Schaltfläche **Aktionen** für den zu verwendenden Snapshot und klicken Sie auf **Wiederherstellen**. 

   **Anmerkung:** Die Wiederherstellung hat zur Folge, dass Daten verloren gehen, die seit der Erfassung des verwendeten Snapshots erstellt oder geändert wurden. Nach Abschluss der Wiederherstellung befindet sich Ihr Speicherdatenträger in dem Status des Zeitpunkts, zu dem der Snapshot erfasst wurde. Sie werden in einer Eingabeaufforderung darüber informiert.
5. Klicken Sie auf **Ja**, um die Wiederherstellung einzuleiten. Sie empfangen eine Nachricht im oberen Bereich der Seite, die besagt, dass der Datenträger mit dem ausgewählten Snapshot wiederhergestellt wurde. Darüber hinaus wird ein Symbol neben Ihrem Datenträger auf der {{site.data.keyword.filestorage_short}}-Seite angezeigt, das darauf hinweist, dass gerade eine aktive Transaktion stattfindet. Wenn der Mauszeiger über das Symbol bewegt wird, wird die Transaktion in einem Dialogfeld angezeigt. Das Symbol wird ausgeblendet, sobald die Transaktion abgeschlossen ist.
6. Hängen Sie Ihren Speicherdatenträger wieder am Host an und verbinden Sie ihn erneut.
   - Klicken Sie hier [hier](accessing-file-storage-linux.html), um weitere Anweisungen für {{site.data.keyword.filestorage_short}} unter Linux anzuzeigen.
   
**Anmerkung:** Die Wiederherstellung eines Datenträgers hat zur Folge, dass alle Snapshots vor dem wiederhergestellten Snapshot gelöscht werden.

