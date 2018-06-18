---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Snapshots verwalten

## Wie wird ein Snapshotzeitplan erstellt?

Durch Erstellen von Snapshotzeitplänen können Sie entscheiden, wie häufig und wann Sie eine zeitpunktbasierte Referenz des Speicherdatenträgers erstellen wollen. Es können maximal 50 Snapshots pro Speicherdatenträger erstellt werden. Zeitpläne werden über die Registerkarte **Speicher** > **{{site.data.keyword.filestorage_short}}** im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} verwaltet.

Bevor Sie Ihren ersten Zeitplan einrichten können, müssen Sie zunächst Snapshotbereich kaufen, sofern Sie diesen nicht bei der Erstbereitstellung des Speicherdatenträgers gekauft haben.

### Snapshotzeitplan hinzufügen

Snapshotzeitpläne können für stündliche, tägliche und wöchentliche Intervalle jeweils mit einem eigenen Aufbewahrungszyklus eingerichtet werden. Pro Speicherdatenträger besteht ein Grenzwert von maximal 50 geplanten (dies kann eine Mischung aus stündlichen, täglichen und wöchentlichen Zeitplänen sein) und manuellen Snapshots.

1. Klicken Sie auf Ihren Speicherdatenträger, klicken Sie auf die Dropdown-Liste **Aktionen** und klicken Sie auf die Option **Snapshot planen**.
2. Im Dialogfeld für den neuen Snapshotzeitplan ****können Sie aus drei verschiedenen Snapshothäufigkeiten wählen. Sie können eine beliebige Kombination aus diesen drei verwenden, um einen umfassenden Snapshotzeitplan zu erstellen.
   - Stündlich
      - Geben Sie die Minute an, in der ein Snapshot stündlich ausgeführt werden soll. Der Standardwert ist die aktuelle Minute.
      - Geben Sie die Anzahl der stündlichen Snapshots an, die aufbewahrt werden sollen, bevor der älteste Snapshot gelöscht wird.
   - Täglich
      - Geben Sie die Stunde und die Minute an, in der ein Snapshot ausgeführt werden soll. Standardwert ist die aktuelle Stunde und Minute.
      - Wählen Sie die Anzahl der stündlichen Snapshots an, die aufbewahrt werden sollen, bevor der älteste Snapshot gelöscht wird.
   - Wöchentlich
      - Geben Sie den Wochentag, die Stunde und die Minute des Zeitpunkts an, zu dem ein Snapshot ausgeführt werden soll. Standardwert ist der aktuelle Zeitpunkt (Tag, Stunde und Minute).
      - Wählen Sie die Anzahl der wöchentlichen Snapshots an, die aufbewahrt werden sollen, bevor der älteste Snapshot gelöscht wird.
3. Klicken Sie auf **Speichern** und erstellen Sie einen weiteren Zeitplan mit einer anderen Häufigkeit. </br> **Anmerkung**: Wenn die Gesamtzahl der geplanten Snapshots 50 übersteigt, erhalten Sie eine Warnnachricht und Sie können Ihren Zeitplan nicht speichern.

Eine Liste der Snapshots wird in der Reihenfolge ihrer Ausführung im Abschnitt **Snapshots** der Seite **Detail** angezeigt.

## Wie wird ein manueller Snapshot erstellt?

Manuelle Snapshots können an verschiedenen Punkten während eines Anwendungsupgrades oder einer Wartungsoperation erfasst werden. Darüber hinaus können Sie Snapshots serverübergreifend für mehrere Server erstellen, die auf der Anwendungsebene vorübergehend inaktiviert wurden.

Die Anzahl manueller Snapshots ist auf maximal 50 pro Speicherdatenträger begrenzt.

1. Klicken Sie auf Ihren Speicherdatenträger.
2. Klicken Sie auf die Dropdown-Liste 'Aktionen'.
3. Klicken Sie auf **Manuellen Snapshot erstellen**.

Der Snapshot wird sofort erstellt und im Abschnitt für Snapshots der Seite **Detail** angezeigt. Als Zeitplanangabe wird 'Manuell' angezeigt.

## Wie kann eine Liste der Snapshots mit belegtem Speicherbereich und Managementfunktionen angezeigt werden?

Eine Liste der aufbewahrten Snapshots und des belegten Speicherbereichs wird auf der Seite **Detail** (**Speicher** > **{{site.data.keyword.filestorage_short}}**) angezeigt. Managementfunktionen (Zeitplanbearbeitung und Hinzufügen weiteren Speicherbereichs) werden auf der Seite **Details** über das Menü **Aktionen** oder über Links in den verschiedenen Abschnitten der Seite ausgeführt.

## Wie wird eine Liste der aufbewahrten Snapshots angezeigt?

Die Anzahl der aufbewahrten Snapshots richtet sich nach dem Wert, den Sie in das Feld **Letzte behalten** bei der Einrichtung Ihrer Zeitpläne eingegeben haben. Die Snapshots, die erfasst wurden, werden im Abschnitt **Snapshot** angezeigt. Die Snapshots werden nach Zeitplan aufgelistet.

## Wie kann ich anzeigen, wieviel Snapshotbereich belegt ist?

Das Kreisdiagramm oben auf der Seite **Details** zeigt an, wie viel Speicherbereich belegt ist und wie viel Speicherbereich noch verfügbar ist. Sie empfangen Benachrichtigungen, wenn Schwellenwerte für die Speicherbelegung erreicht werden – bei 75 Prozent, 90 Prozent und 95 Prozent.

## Wie lässt sich die Größe des Snapshotbereichs für einen Datenträger ändern?

Sie müssen einem Datenträger, der zuvor keinen Snapshotbereich hatte oder zusätzlichen Snapshotbereich benötigt, möglicherweise Snapshotbereich hinzufügen. Sie können je nach Bedarf zwischen 5 GB und 4.000 GB hinzufügen. **Anmerkung**: Snapshotbereich kann nur vergrößert, nicht aber verkleinert werden. Sie können zunächst einen kleinere Speichergröße auswählen, bis Sie ermittelt haben, wie viel Speicherbereich Sie tatsächlich benötigen. Beachten Sie, dass automatisierte und manuelle Snapshots den gleichen Speicherbereich gemeinsam nutzen.

Der Snapshotbereich wird über **Speicher** > **{{site.data.keyword.filestorage_short}}** geändert.

1. Klicken Sie auf Ihre Speicherdatenträger, klicken Sie auf die Dropdown-Liste **Aktionen** und klicken Sie auf die Option **Weiteren Snapshotbereich hinzufügen**.
2. Wählen Sie in der Eingabeaufforderung unter eine Reihe von Größe die gewünschte Größe aus. Größen reichen in der Regel von 0 bis zur Größe Ihres Datenträgers.
3. Klicken Sie auf **Weiter**, um den zusätzlichen Speicherbereich bereitzustellen.
4. Geben Sie einen Werbeaktionscode ein, wenn Sie einen haben, und klicken Sie auf **Neu berechnen**. Die Felder **Gebühren für diese Bestellung** und **Bestellprüfung** zeigen Standardwerte an.
5. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Bestellung abschicken**. Der zusätzliche Snapshotbereich wird innerhalb weniger Minuten bereitgestellt.

## Wie erfolgt die Benachrichtigung, wenn die Größe des Snapshotbereichs fast den Grenzwert erreicht und Snapshots gelöscht werden?

Über Support-Tickets werden vom Support an den Masterbenutzer Benachrichtigungen auf Ihr Konto gesendet, wenn drei verschiedene Speicherschwellenwerte erreicht werden – 75 Prozent, 90 Prozent und 95 Prozent.

- 75 Prozent der Kapazität: Eine Warnung wird gesendet, dass die Belegung des Snapshotbereichs 75 Prozent überschritten hat. Wenn Sie auf die Warnung reagieren und Speicherbereich manuell hinzufügen oder aufbewahrte und unnötige Snapshots löschen, wird die Aktion vermerkt und das Ticket geschlossen. Wenn Sie nichts unternehmen, müssen Sie das Ticket manuell bestätigen und es wird anschließend geschlossen.
- 90 Prozent der Kapazität: Eine zweite Warnung wird gesendet, wenn die Belegung des Snapshotbereichs 90 Prozent überschritten hat. Wie bei Erreichen von 75 Prozent der Kapazität gilt auch hier: Wenn Sie die erforderlichen Aktionen ausführen, mit denen die Belegung des Speicherbereichs verringert wird, wird die Aktion vermerkt und das Ticket geschlossen. Wenn Sie nichts unternehmen, müssen Sie das Ticket manuell bestätigen und es wird anschließend geschlossen.
- 95 Prozent der Kapazität: Eine letzte Warnung wird gesendet. Wenn keine Aktion ausgeführt wird, um die Speicherbelegung unter den Schwellenwert zu senken, wird eine Benachrichtigung generiert und eine automatische Löschung durchgeführt, sodass zukünftige Snapshots erstellt werden können. Geplante Snapshots werden gelöscht, beginnend mit dem ältesten, bis die Belegung unter 95 Prozent liegt. Wenn die Belegung 95 Prozent überschreitet, werden Snapshots weiterhin solange gelöscht, bis die Belegung unter dem Schwellenwert liegt. Wenn der Speicherbereich manuell vergrößert oder Snapshots gelöscht werden, wird die Warnung zurückgesetzt und erneut ausgegeben, wenn der Schwellenwert wieder überschritten wird. Wenn keine Aktion ausgeführt wird, ist dies die einzige Warnung, die empfangen wird.

## Wie wird ein Snapshotzeitplan gelöscht?

Snapshotzeitpläne können über **Speicher** > **{{site.data.keyword.filestorage_short}}** gelöscht werden.

1. Klicken Sie im Feld **Snapshotpläne** auf der Seite **Details** auf den zu löschenden Zeitplan.
2. Klicken Sie auf das Kontrollkästchen neben dem zu löschenden Zeitplan und klicken Sie auf **Speichern**.<br/>
**Vorsicht**: Wenn Sie die Replikationsfunktion verwenden, müssen Sie sicherstellen, dass der Zeitplan, den Sie löschen, nicht der von der Replikation verwendete Zeitplan ist. Klicken Sie [hier](replication.html), um weitere Informationen zum Löschen eines Replikationsplans zu erhalten.

## Wie wird ein Snapshot gelöscht?

Snapshots, die nicht mehr benötigt werden, können manuell entfernt werden, um Speicherplatz für zukünftige Snapshots freizugeben. Das Löschen erfolgt über **Speicher** > **{{site.data.keyword.filestorage_short}}**.

1. Klicken Sie auf Ihren Speicherdatenträger und blättern Sie nach unten zum Abschnitt **Snapshot**, um eine Liste der vorhandenen Snapshots anzuzeigen.
2. Klicken Sie auf die Dropdown-Liste **Aktionen** neben dem gewünschten Snapshot und klicken Sie auf **Löschen**, um den Snapshot zu löschen. Dies hat keine Auswirkung auf zukünftige oder frühere Snapshots im selben Zeitplan, da zwischen Snapshots keine Abhängigkeit besteht.

Manuelle Snapshots, die nicht mit der hier beschriebenen Methode gelöscht werden, werden automatisch (älteste zuerst) gelöscht, wenn Sie den Grenzwert des Speicherbereichs erreichen.

## Wie wird ein Speicherdatenträger mithilfe eines Snapshots auf dem Stand eines bestimmten Zeitpunkts wiederhergestellt?

Es ist möglich, dass Sie Ihren Speicherdatenträger aufgrund eines Benutzerfehlers oder einer Datenbeschädigung auf einen bestimmten Zeitpunkt zurücksetzen müssen.

1. Hängen Sie Ihren Speicherdatenträger vom Host ab und trennen Sie die Verbindung.
   - Klicken Sie hier [hier](accessing-file-storage-linux.html), um weitere Anweisungen für {{site.data.keyword.filestorage_short}} unter Linux anzuzeigen.
2. Klicken Sie auf **Speicher** > **{{site.data.keyword.filestorage_short}}** im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
3. Blättern Sie nach unten und klicken Sie auf den Datenträger, der wiederhergestellt werden soll. Im Abschnitt **Snapshots** der Seite **Detail** wird eine Liste aller gespeicherten Snapshots mit Angabe ihrer Größe und ihres Erstellungsdatums angezeigt.
4. Klicken Sie auf **Aktionen** für den zu verwendenden Snapshot und klicken Sie auf **Wiederherstellen**. 

   **Anmerkung:** Die Wiederherstellung hat zur Folge, dass Daten verloren gehen, die seit der Erfassung des verwendeten Snapshots erstellt oder geändert wurden. Nach Abschluss der Wiederherstellung nimmt Ihr Speicherdatenträger den Status des Zeitpunkts an, an dem der Snapshot erstellt wurde. Sie werden in einer Eingabeaufforderung darüber informiert.
5. Klicken Sie auf **Ja**, um die Wiederherstellung einzuleiten. Quer über den oberen Bereich der Seite wird die Nachricht angezeigt, dass der Datenträger mit dem ausgewählten Snapshot wiederhergestellt wurde. Darüber hinaus wird ein Symbol neben Ihrem Datenträger auf der {{site.data.keyword.filestorage_short}}-Seite angezeigt, das darauf hinweist, dass zurzeit eine Transaktion aktiv ist. Wenn der Mauszeiger über das Symbol bewegt wird, wird die Transaktion in einem Dialogfeld angezeigt. Das Symbol wird ausgeblendet, wenn die Transaktion abgeschlossen ist.
6. Hängen Sie Ihren Speicherdatenträger an den Host an und ordnen Sie ihn erneut zu.
   - Klicken Sie hier [hier](accessing-file-storage-linux.html), um weitere Anweisungen für {{site.data.keyword.filestorage_short}} unter Linux anzuzeigen.
   
**Anmerkung:** Die Wiederherstellung eines Datenträgers hat zur Folge, dass alle Snapshots vor dem wiederhergestellten Snapshot gelöscht werden.
