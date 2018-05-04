---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Snapshots verwalten

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

1. Klicken Sie im Feld **Snapshotpläne** auf der Seite **Details** auf den zu löschenden Zeitplan.
2. Klicken Sie auf das Kontrollkästchen neben dem zu löschenden Zeitplan und klicken Sie auf **Speichern**.<br/>
**Achtung**: Wenn Sie die Replikationsfunktion verwenden, stellen Sie sicher, dass der Zeitplan, den Sie löschen, nicht der von der Replikation verwendete Zeitplan ist. Klicken Sie [hier](replication.html), um weitere Informationen zum Löschen eines Replikationsplans zu erhalten.

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
