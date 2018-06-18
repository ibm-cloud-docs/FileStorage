---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-22"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Mit Replikation arbeiten

Bei der Replikation wird einer von vier Snapshotzeitplänen verwendet, um Snapshots automatisch auf einen Zieldatenträger in einem fernen Rechenzentrum zu kopieren. Die Kopien können am fernen Standort wiederhergestellt werden, falls Daten beschädigt werden oder ein Elementarereignis auftritt.

Replikate ermöglichen Folgendes:

- Schnelle Wiederherstellung nach Standortausfällen oder anderen Katastrophen durch Failover auf den Zieldatenträger.
- Failover auf einen bestimmten Zeitpunkt in der Disaster Recovery-Kopie (DR-Kopie)

Bevor Sie replizieren können, müssen Sie einen Snapshotplan erstellen. Wenn Sie einen Failover durchführen, 'schalten Sie um', und zwar von Ihrem Speicherdatenträger in Ihrem primären Rechenzentrum auf den Zieldatenträger in Ihrem fernen Rechenzentrum. Ihr primäres Rechenzentrum ist zum Beispiel London und Ihr sekundäres Rechenzentrum ist Amsterdam. Wenn ein Fehlerereignis auftritt, führen Sie ein Failover auf Amsterdam durch – dazu stellen Sie zu dem jetzigen primären Datenträger eine Verbindung von einer Compute-Instanz in Amsterdam her. Nachdem Ihr Datenträger in London repariert wurde, wird ein Snapshot des Datenträgers in Amsterdam erstellt, um die Rückübertragung auf den Datenträger in London und den nun wieder primären Datenträger von einer Compute-Instanz in London durchzuführen.


## Wie wird das ferne Rechenzentrum für meinen replizierten Speicherdatenträger ermittelt?

Die verfügbaren Rechenzentren von {{site.data.keyword.BluSoftlayer_full}} wurden weltweit zu Paaren aus primären und fernen Rechenzentren kombiniert. In Tabelle 1 finden Sie die vollständige Liste der verfügbaren Rechenzentren und Replikationsziele.


<table style="width: 80.0%;">
	<caption style="text-align: left;"><p>Tabelle – Diese Tabelle zeigt die vollständige Liste der Rechenzentren mit erweiterten Funktionen, die in den einzelnen Regionen vorhanden sind. Jede Region bildet eine eigene Spalte. Einige Städte wie beispielsweise Dallas, San Jose, Washington D.C., Amsterdam, Frankfurt, London und Sydney haben mehrere Rechenzentren.</p>
		<p>&#42; Datenzentren in der Region US 1 haben KEINEN erweiterten Speicher. Hosts in Rechenzentren, die eine erweiterte Funktionalität für Speicher aufweisen, können die Replikation <strong>nicht</strong> mit Replikationszielen in Rechenzentren der Region US 1 initialisieren.</p>
</caption>
	<thead>
		<tr>
			<th>US 1 &#42;</th>
			<th>US 2</th>
			<th>Lateinamerika</th>
			<th>Kanada</th>
			<th>Europa</th>
			<th>Asien-Pazifik</th>
			<th>Australien</t>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>DAL01<br />
				DAL05<br />
				DAL06<br />
				HOU02<br />
				SJC01<br />
				WDC01<br />
				<br />
				<br />
				<br />
				<br />
				<br />
			</td>
			<td>SJC03<br />
			       SJC04<br />
			       WDC04<br />
			       WDC06<br />
			       WDC07<br />
			       DAL09<br />
				DAL10<br />
				DAL12<br />
				DAL13<br />
				<br /><br />
			</td>
			<td>MEX01<br />
				SAO01<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
			</td>
			<td>TOR01<br />
				MON01<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
			</td>
			<td>
				AMS01<br />
				AMS03<br />
				FRA02<br />
				FRA04<br />
				FRA05<br />
				LON02<br />
				LON04<br />
				LON06<br />
				OSL01<br />
				PAR01<br />
				MIL01<br />
			</td>
			<td>HKG02<br />
				TOK02<br />
				SNG01<br />
				SEO01<br />
                                CHE01<br />
				<br />
				<br />
				<br />
				<br />
				<br />
				<br />
			</td>
			<td>
				SYD01<br />
				SYD04<br />
				MEL01<br />
				<br /><br /><br /><br /><br /><br /><br /><br />
			</td>
		</tr>
	</tbody>
</table>


## Wie wird eine Erstreplikation erstellt?

Replikationen arbeiten nach einem Snapshotplan. Sie müssen zuerst einen Snapshotbereich und einen Snapshotplan für den Quellendatenträger eingerichtet haben, bevor Sie replizieren können. Sie empfangen Eingabeaufforderungen, die Sie darüber informieren, dass Speicherbereich gekauft oder ein Zeitplan eingerichtet werden muss, wenn Sie versuchen, die Replikation einzurichten und das eine oder andere noch nicht erfolgt ist. Replikationen werden unter **Storage** > **{{site.data.keyword.filestorage_short}}** im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} verwaltet.

1. Klicken Sie auf Ihren Speicherdatenträger.
2. Klicken Sie auf die Registerkarte **Replikat** und klicken Sie auf den Link **Replikation kaufen**.
3. Wählen Sie einen vorhandenen Snapshotplan aus, den Ihre Replikationen befolgen sollen. Die Liste enthält alle Ihre aktiven Snapshotpläne.
  **Anmerkung:** Sie können nur einen Plan auswählen, selbst wenn Sie einen Mix aus stündlicher, täglicher und wöchentlicher Rechnungsstellung haben.  Alle Snapshots, die seit dem vorherigen Replikationszyklus erfasst wurden, werden unabhängig von dem Zeitplan, auf dem sie basieren, repliziert.
4. Klicken Sie auf den Dropdown-Pfeil neben **Position** und wählen Sie das Rechenzentrum (Data Center) aus, das als Ihre Disaster Recovery-Site (DR-Site) fungieren soll.
5. Klicken Sie auf **Weiter**.
6. Geben Sie einen **Werbeaktionscode** (Promo Code) ein, wenn Sie einen haben, und klicken Sie auf **Neu berechnen**. Die anderen Felder im Dialogfenster erhalten Standardwerte.
7. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Bestellung abschicken**.


## Wie wird eine vorhandene Replikation bearbeitet?

Im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} auf der Registerkarte **Primär** oder **Replikat** unter **Speicher** > **{{site.data.keyword.filestorage_short}}** können Sie Ihren Replikationsplan bearbeiten und Ihren Replikationsspeicherbereich ändern.


## Wie wird ein Replikationsplan bearbeitet?

Tatsächlich ändern Sie einen Snapshotplan, da Ihr Replikationsplan auf einem vorhandenen Snapshotplan basiert. Wenn Sie den Replikationsplan zum Beispiel von 'Stündlich' in 'Wöchentlich' ändern möchten, müssen Sie den Replikationsplan abbrechen und einen neuen erstellen.

Das Ändern des Zeitplans kann über die Registerkarte **Primär** oder **Replikat** erfolgen.

1. Klicken Sie auf der Registerkarte **Primär** oder auf der Registerkarte **Replikat** auf das Menü **Aktionen**.
2. Wählen Sie die Option **Snapshotplan bearbeiten** aus.
3. Prüfen Sie das Feld 'Snapshot' unter 'Zeitplan', um festzustellen, welchen Zeitplan Sie für die Replikation verwenden. Nehmen Sie die Änderungen an dem Zeitplan vor, der für die Replikation verwendet wird. Wenn Ihr Replikationsplan zum Beispiel auf **Täglich** eingestellt ist, können Sie die Tageszeit ändern, zu der die Replikation stattfinden soll.
4. Klicken Sie auf **Speichern**.


## Wie wird der Replikationsbereich geändert?

Ihr primärer Snapshotbereich und Ihr Replikatbereich müssen identisch sein. Wenn Sie den Speicherbereich auf der Registerkarte **Primär** oder **Replikat** ändern, wird dadurch automatisch Speicherbereich in Ihrem Quellen- und Ihrem Zielrechenzentrum hinzugefügt. Beachten Sie, dass eine Vergrößerung des Snapshotbereichs eine unverzügliche Replikationsaktualisierung auslöst.

1. Klicken Sie auf der Registerkarte **Primär** oder auf der Registerkarte **Replikat** auf das Menü **Aktionen**.
2. Wählen Sie die Option **Weiteren Snapshotbereich hinzufügen** hinzufügen) aus.
3. Wählen Sie die Speichergröße in der Liste aus und klicken Sie auf **Weiter**.
4. Geben Sie einen **Werbeaktionscode** ein, falls Sie über einen verfügen, und klicken Sie auf **Neu berechnen**. Die anderen Felder im Dialogfenster erhalten Standardwerte.
5. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Bestellung abschicken**.


## Wie werden meine Replikatdatenträger in der Datenträgerliste angezeigt?

Sie können Ihre Replikationsdatenträger auf der {{site.data.keyword.filestorage_short}}-Seite unter **Speicher** > **{{site.data.keyword.filestorage_short}}** anzeigen. Der Datenträgername enthält den Namen des primären Datenträgers gefolgt von REP. Der Typ ist Endurance(Performance) – Replikat. Die Zieladresse ist n/v, da der Replikatdatenträger nicht am Replikatrechenzentrum angehängt ist, und der Status ist 'inaktiv'.



## Wie werden die Details eines replizierten Datenträgers im Replikatrechenzentrum angezeigt?

Sie können die Details von Replikatdatenträgern auf der Registerkarte **Replikat** unter **Speicher** > **{{site.data.keyword.filestorage_short}}** anzeigen. Eine weitere Option besteht darin, den Replikatdatenträger auf der **{{site.data.keyword.filestorage_short}}**-Seite auszuwählen und auf die Registerkarte **Replikat** zu klicken.



## Wie werden vor einem Failover auf das sekundäre Rechenzentrum Hostberechtigungen angegeben?

Berechtigte (bzw. autorisierte) Hosts und Datenträger müssen sich im selben Rechenzentrum befinden. Sie können nicht den Replikatdatenträger in London und den Host in Amsterdam haben: beide müssen entweder in London oder in Amsterdam vorhanden sein.

1. Klicken Sie auf Ihren Quellen- oder Zieldatenträger auf der **{{site.data.keyword.filestorage_short}}**-Seite.
2. Klicken Sie auf die Registerkarte **Replikat**.
3. Blättern Sie zum Rahmen **Hosts autorisieren** und klicken Sie auf der rechten Seite auf **Hosts autorisieren**.
4. Heben Sie den Host hervor, der für Replikationen autorisiert werden soll. Zur Auswahl mehrerer Hosts drücken Sie die Steuertaste (STRG) und klicken Sie auf die betreffenden Hosts.
5. Klicken Sie auf **Abschicken**. Wenn Sie keine Hosts haben, bietet das Dialogfenster die Option an, Compute-Ressourcen im selben Rechenzentrum zu kaufen. Alternativ können Sie auf **Schließen** klicken.


## Wie erhöhe ich meinen Snapshotbereich in meinem Replikatrechenzentrum, wenn ich den Bereich in meinem primären Rechenzentrum erhöhe?

Die Datenträgergröße Ihres primären Speicherdatenträgers und Ihres Replikatspeicherdatenträgers muss übereinstimmen. Keiner der Datenträger darf größer als der andere sein. Wenn Sie Ihren Snapshotbereich für Ihren primären Datenträger vergrößern, wird der Replikatbereich automatisch vergrößert. Beachten Sie, dass eine Vergrößerung des Snapshotbereichs eine unverzügliche Replikationsaktualisierung auslöst. Die Vergrößerung beider Datenträger werden als Artikelpositionen auf Ihrer Rechnung aufgeführt und wie erforderlich anteilmäßig berechnet.

Klicken Sie [hier](snapshots.html), um Informationen zur Vergrößerung Ihres Snapshotbereichs anzuzeigen.

## Wie wird ein Failover von einem Datenträger auf sein Replikat eingeleitet?

Bei einem Fehlerereignis können Sie einen **Failover** auf Ihren Zieldatenträger einleiten. Der Zieldatenträger wird aktiv. Der letzte erfolgreich replizierte Snapshot wird aktiviert und der Datenträger wird zum Anhängen (Mount) aktiviert. Alle Daten, die seit dem letzten Replikationszyklus auf den Quellendatenträger geschrieben wurden, werden vernichtet. Beachten Sie, dass die Replikationsbeziehung nach dem Einleiten eines Failovers umgekehrt ist. Ihr Zieldatenträger ist jetzt Ihr Quellendatenträger und Ihr früherer Quellendatenträger wird zum Zieldatenträger; dies ist durch die Zeichenfolge **REP** angezeigt, die auf den ursprünglichen LUN-Namen folgt.

Failoveroperationen werden unter **Speicher** > **{{site.data.keyword.filestorage_short}}** im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} eingeleitet.

**Bevor Sie mit diesem Prozess fortfahren, wird empfohlen, die Verbindung zum Datenträger zu trennen. Wenn dies nicht erfolgt, kann dies zu Beschädigung und Datenverlust führen.**

1. Klicken Sie auf Ihre aktive LUN ('Quelle').
2. Klicken Sie auf die Registerkarte 'Replikat' und klicken Sie in der rechten oberen Ecke auf **Aktionen**.
3. Wählen Sie die Option für **Failover** aus. Quer über den oberen Bereich der Seite wird die Nachricht angezeigt, dass der Failover in Bearbeitung ist. Darüber hinaus wird neben Ihrem Datenträger auf der **{{site.data.keyword.filestorage_short}}**-Seite ein Symbol angezeigt, das darauf hinweist, dass zurzeit eine Transaktion aktiv ist. Bei Bewegen des Mauszeigers über das Symbol wird die Transaktion in einem Dialogfeld angezeigt. Das Symbol wird ausgeblendet, sobald die Transaktion abgeschlossen ist. Während des Failover-Prozesses sind konfigurationsbezogene Aktionen schreibgeschützt. Sie können Snapshotpläne nicht bearbeiten oder Snapshotbereiche ändern usw. Das Ereignis wird im Replikationsprotokoll aufgezeichnet.
4. Wenn Ihr Zieldatenträger live ist, wird der LUN-Name des ursprünglichen Quellendatenträgers aktualisiert, sodass er die Zeichenfolge 'REP' enthält, und als zugehöriger Status wird 'inaktiv' angezeigt.
4. Klicken Sie in der rechten oberen Ecke auf den Link **Alle {{site.data.keyword.filestorage_short}}-Speicher anzeigen**.
5. Klicken Sie auf Ihre aktive LUN (früher Ihr Zieldatenträger). Dieser Datenträger hat nun den Status **Aktiv**.
6. Hängen Sie Ihren Speicherdatenträger an den Host an und verbinden Sie ihn.


## Wie leite ich eine Rückübertragung von einem Datenträger auf dessen Replikat ein?

Sobald Ihr Originalquellendatenträger repariert ist, können Sie mit der Aktion **Rückübertragung** eine gesteuerte Rückübertragung auf Ihren Originalquellendatenträger einleiten. Bei einer gesteuerten Rückübertragung geschieht Folgendes:

- Der aktive Quellendatenträger wird offline geschaltet.
- Ein Snapshot wird erfasst.
- Der Replikationszyklus wird abgeschlossen.
- Der soeben erfasste Datensnapshot wird aktiviert.
- Und der Quellendatenträger wird für das Anhängen (den Mount) aktiviert.

Beachten Sie, dass die Replikationsbeziehung nach dem Einleiten einer Rückübertragung erneut umgekehrt wird. Ihr Quellendatenträger wird als Quellendatenträger wiederhergestellt und Ihr Zieldatenträger ist wieder Ihr Zieldatenträger, wie durch die Zeichenfolge **REP** angezeigt, die auf den LUN-Namen folgt.

Rückübertragungen werden unter **Speicher** > **{{site.data.keyword.filestorage_short}}** im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} eingeleitet.

1. Klicken Sie auf Ihre aktive Endurance-LUN ('das Ziel').
2. Klicken Sie auf die Registerkarte **Replikat** und klicken Sie in der rechten oberen Ecke auf **Aktionen**.
3. Wählen Sie **Rückübertragung** aus. Quer über den oberen Bereich der Seite wird die Nachricht angezeigt, dass der Failover in Bearbeitung ist. Darüber hinaus wird neben Ihrem Datenträger ein Symbol angezeigt, dass eine Transaktion aktiv ist. Wenn der Mauszeiger über das Symbol bewegt wird, wird die Transaktion in einem Dialogfeld angezeigt. Das Symbol wird ausgeblendet, sobald die Transaktion abgeschlossen ist. <br /> Während des Prozesses der Rückübertragung sind konfigurationsbezogene Aktionen schreibgeschützt. Sie können Snapshotpläne nicht bearbeiten oder Snapshotbereiche ändern usw. Das Ereignis wird im Replikationsprotokoll aufgezeichnet.
5. Wenn Ihr Quellendatenträger live ist, wird Ihr Zieldatenträger inaktiv.
4. Klicken Sie in der rechten oberen Ecke auf **Alle {{site.data.keyword.filestorage_short}}-Speicher anzeigen**.
5. Klicken Sie auf Ihre aktive LUN (die 'Quelle'). Dieser Datenträger hat nun den Status **Aktiv**.
6. Hängen Sie Ihren Speicherdatenträger an den Host an und verbinden Sie ihn.Weitere Anweisungen finden Sie [hier](provisioning-file-storage.html).


## Wie wird das Replikationsprotokoll angezeigt?

Das Replikationsprotokoll wird auf der Registerkarte **Konto** unter **Verwalten** im **Prüfprotokoll** angezeigt. Der primäre und der Replikatdatenträger zeigen ein identisches Replikationsprotokoll an, das folgende Informationen enthält:

- Art der Replikation (Failover oder Rückübertragung)
- Zeitpunkt der Einleitung
- Für die Replikation verwendeter Snapshot
- Größe der Replikation
- Zeitpunkt des Abschlusses


## Wie wird eine vorhandene Replikation abgebrochen?

Der Abbruch kann entweder sofort oder zum Rechnungsstichtag erfolgen, sodass die Rechnungsstellung beendet wird. Die Replikation kann auf der Registerkarte **Primär** oder **Replikat** abgebrochen werden.

1. Klicken Sie auf der Seite **{{site.data.keyword.filestorage_short}}** auf den Datenträger.
2. Klicken Sie auf der Registerkarte **Primär** bzw. **Replikat** auf **Aktionen**.
3. Wählen Sie **Replikation abbrechen** aus.
4. Wählen Sie einen Zeitpunkt für den Abbruch aus. **Sofort** oder **Rechnungsstichtag** und klicken Sie auf **Weiter**.
5. Klicken Sie auf das Kontrollkästchen **Ich bin mir bewusst, dass es durch den Abbruch zu Datenverlust kommen kann** und klicken Sie auf **Replikation abbrechen**.


## Wie wird eine Replikation abgebrochen, wenn der Primärdatenträger storniert wird?

Wenn ein primärer Datenträger storniert wurde, werden der Replikationsplan und der Datenträger im Replikatrechenzentrum gelöscht. Replikate werden auf der Seite **{{site.data.keyword.filestorage_short}}** abgebrochen.

 1. Heben Sie Ihren Datenträger auf der Seite **{{site.data.keyword.filestorage_short}}** hervor.
 2. Klicken Sie auf **Aktionen** und wählen sie **Für {{site.data.keyword.filestorage_short}} stornieren**.
 3. Wählen Sie einen Zeitpunkt für das Stornieren des Datenträgers aus. **Sofort** oder **Rechnungsstichtag** und klicken Sie auf **Weiter**.
 4. Klicken Sie auf das Kontrollkästchen **Ich bin mir bewusst, dass es durch den Abbruch zu Datenverlust kommen kann** und klicken Sie auf **Abbrechen**.
