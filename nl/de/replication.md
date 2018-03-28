---
 
copyright:
  years: 2015, 2018
lastupdated: "2018-02-13"
 
---

{:shortdesc: .shortdesc} 
{:new_window: target="_blank"}

# Mit Replikation arbeiten

Bei der Replikation wird einer von vier Snapshotzeitplänen verwendet, um Snapshots automatisch auf einen Zieldatenträger in einem fernen Rechenzentrum zu kopieren. Die Kopien können am fernen Standort wiederhergestellt werden, falls Daten beschädigt werden oder ein Elementarereignis auftritt.

Replikate bieten Ihnen folgende Möglichkeiten:

- Schnelle Wiederherstellung nach Standortausfällen oder anderen Katastrophen durch Failover auf den Zieldatenträger
- Failover auf einen bestimmten Zeitpunkt in der Disaster Recovery-Kopie (DR-Kopie)

Bevor Sie replizieren können, müssen Sie einen Snapshotplan erstellen. Wenn Sie einen Failover durchführen, “schalten Sie um”, und zwar von Ihrem Speicherdatenträger in Ihrem primären Rechenzentrum auf den Zieldatenträger in Ihrem fernen Rechenzentrum. Ihr primäres Rechenzentrum ist zum Beispiel London und Ihr sekundäres Rechenzentrum ist Amsterdam. Im Falle eines Fehlerereignisses können Sie ein Failover auf Amsterdam durchführen – stellen Sie zu dem jetzigen primären Datenträger eine Verbindung von einer Compute-Instanz in Amsterdam her. Nachdem Ihr Datenträger in London repariert wurde, wird ein Snapshot des Datenträgers in Amsterdam erstellt, um das Zurücksetzen (Fallback) auf den Datenträger in London und den nun wieder primären Datenträger von einer Compute-Instanz in London durchzuführen.

 
**Anmerkung:** Wenn nichts anderes angegeben wird, stimmen die Schritte für {{site.data.keyword.blockstorageshort}} und {{site.data.keyword.filestorage_full}} überein.

## Wie wird das ferne Rechenzentrum für den eigenen replizierten Speicherdatenträger ermittelt?

Die weltweit verfügbaren Rechenzentren von {{site.data.keyword.BluSoftlayer_full}} wurden zu Paaren von primären und fernen Rechenzentren kombiniert.
In Tabelle 1 finden Sie eine vollständige Liste der verfügbaren Rechenzentren und Replikationsziele.
Beachten Sie, dass in einigen Städten wie Dallas, San Jose, Washington D.C. und Amsterdam mehrere Rechenzentren verfügbar sind.


<table cellpadding="1" cellspacing="1">
	<caption>Tabelle 1</caption>
	<tbody>
		<tr>
			<td><strong>US 1</strong><sup><img src="/images/numberone.png" alt="1" /></sup></td>
			<td><strong>US 2</strong></td>
			<td><strong>Lateinamerika/Südamerika</strong></td>
			<td><strong>Kanada</strong></td>
			<td><strong>Europa</strong></td>
			<td><strong>Asien-Pazifik</strong></td>
			<td><strong>Australien</strong></td>
		</tr>
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
			</td>
			<td>MEX01<br />
				SAO01<br /><br /><br /><br /><br /><br /><br /><br />
			</td>
			<td>TOR01<br />
				MON01<br /><br /><br /><br /><br /><br /><br /><br />
			</td>
			<td>
				AMS01<br />
				AMS03<br />
				FRA02<br />
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
                                _____<br />
				CHE01<sup><img src="/images/numberone.png" alt="1" /></sup><br />
				<br />
				<br />
				<br />
			</td>
			<td>
				SYD01<br />
				SYD04<br />
				MEL01<br />
				<br /><br /><br /><br /><br /><br />
			</td>
		</tr>
		<tr>
			<td colspan="100%"><p><sup><img src="/images/numberone.png" alt="1" /></sup>Rechenzentren in diesen Regionen oder mit entsprechender Anmerkung in einer Region haben keinen verschlüsselten Speicher.<br /><strong>Anmerkung:</strong> Rechenzentren mit verschlüsseltem Speicher können <strong>keine</strong> Replikation mit nicht verschlüsselten Rechenzentren als Replikationszielen einleiten.<br />In der asiatisch-pazifischen Region kann CHE01 eine Replikation zu Rechenzentren mit verschlüsseltem Speicher als Replikate (Rechenzentren oberhalb der Linie) einleiten.</p>
			</td>
		</tr>
	</tbody>
</table>
 

## Wie wird eine Erstreplikation erstellt?

Replikationen arbeiten nach einem Snapshotplan. Sie müssen zuerst einen Snapshotbereich und einen Snapshotplan für den Quellendatenträger eingerichtet haben, bevor Sie replizieren können. Sie empfangen Eingabeaufforderungen, die Sie darüber informieren, dass Speicherbereich gekauft oder ein Zeitplan eingerichtet werden muss, wenn Sie versuchen, die Replikation einzurichten und das eine oder andere noch nicht erfolgt ist. Replikationen werden unter **Storage** > **{{site.data.keyword.filestorage_short}}** im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} verwaltet.

1. Klicken Sie auf Ihren Speicherdatenträger.
2. Klicken Sie auf die Registerkarte **Replikat** und klicken Sie auf den Link **Replikation kaufen**.
   Wählen Sie einen vorhandenen Snapshotplan aus, den Ihre Replikationen befolgen sollen. Die Liste enthält alle Ihre aktiven Snapshotpläne.
  **Anmerkung:** Sie können nur einen Plan auswählen, selbst wenn Sie einen Mix aus stündlicher, täglicher und wöchentlicher Rechnungsstellung haben. Alle Snapshots, die seit dem vorherigen Replikationszyklus erfasst wurden, werden unabhängig von dem Zeitplan, auf dem sie basieren, repliziert.
3. Klicken Sie auf den Dropdown-Pfeil neben **Position** und wählen Sie das Rechenzentrum (Data Center) aus, das als Ihre Disaster Recovery-Site (DR-Site) fungieren soll.
4. Klicken Sie auf **Weiter**.
5. Geben Sie einen **Werbeaktionscode** (Promo Code) ein, wenn Sie einen haben, und klicken Sie auf **Neu berechnen**. Die anderen Felder im Dialogfenster erhalten Standardwerte.
6. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Bestellung abschicken**.
 

## Wie wird eine vorhandene Replikation bearbeitet?

Im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} auf der Registerkarte **Primär** oder **Replikat** unter **Speicher** > **{{site.data.keyword.filestorage_short}}** können Sie Ihren Replikationsplan bearbeiten und Ihren Replikationsspeicherbereich ändern.

 

## Wie wird ein Replikationsplan bearbeitet?

Sie ändern einen Snapshotplan tatsächlich, da Ihr Replikationsplan auf einem vorhandenen Snapshotplan basiert. Wenn Sie den Replikationsplan zum Beispiel von 'Stündlich' in 'Wöchentlich' ändern möchten, müssen Sie den Replikationsplan abbrechen und einen neuen erstellen.

Das Ändern des Zeitplans kann über die Registerkarte **Primär** oder **Replikat** erfolgen.

1. Klicken Sie auf der Registerkarte **Primär** oder auf der Registerkarte **Replikat** auf das Dropdown-Menü **Aktionen**.
2. Wählen Sie die Option **Snapshotplan bearbeiten** aus.
3. Prüfen Sie den Rahmen 'Snapshot' unter 'Zeitplan', um festzustellen, welchen Zeitplan Sie für die Replikation verwenden. Nehmen Sie die Änderungen an dem Zeitplan vor, der für die Replikation verwendet wird. Wenn Ihr Replikationsplan zum Beispiel auf **Täglich** eingestellt ist, können Sie die Tageszeit ändern, zu der die Replikation stattfinden soll.
4. Klicken Sie auf **Speichern**.
 

## Wie wird der Replikationsbereich geändert?

Ihr primärer Snapshotbereich und Ihr Replikatbereich müssen identisch sein. Wenn Sie den Speicherbereich auf der Registerkarte **Primär** oder **Replikat** ändern, wird dadurch automatisch Speicherbereich in Ihrem Quellen- und Ihrem Zielrechenzentrum hinzugefügt. Beachten Sie, dass eine Vergrößerung des Snapshotbereichs eine unverzügliche Replikationsaktualisierung auslöst.

Klicken Sie auf der Registerkarte **Primär** oder auf der Registerkarte **Replikat** auf das Dropdown-Menü **Aktionen**.
Wählen Sie die Option **Weiteren Snapshotbereich hinzufügen** hinzufügen) aus.
Wählen Sie die Speichergröße in der Liste aus und klicken Sie auf **Weiter**.
Geben Sie einen **Werbeaktionscode** (Promo Code) ein, wenn Sie einen haben, und klicken Sie auf **Neu berechnen**. Die anderen Felder im Dialogfenster erhalten Standardwerte.
Klicken Sie auf das Kontrollkästchen 'Ich habe die Rahmenvereinbarung gelesen' und klicken Sie auf die Schaltfläche 'Bestellung abschicken'.
 

## Wie werden eigene Replikatdatenträger in der Datenträgerliste angezeigt?

Sie können Ihre Replikationsdatenträger auf der {{site.data.keyword.filestorage_short}}-Seite unter **Speicher** > **{{site.data.keyword.filestorage_short}}** anzeigen. Der Datenträgername enthält den Namen des primären Datenträgers gefolgt von REP. Der Typ ist Endurance(Performance) – Replikat, die Zieladresse ist n/v, da der Replikatdatenträger nicht am Replikatrechenzentrum angehängt ist, und der Status ist inaktiv.

 

## Wie werden die Details eines replizierten Datenträgers im Replikatrechenzentrum angezeigt?

Sie können die Details von Replikatdatenträgern auf der Registerkarte **Replikat** unter **Speicher** > **{{site.data.keyword.filestorage_short}}** anzeigen. Eine weitere Option besteht darin, den Replikatdatenträger auf der **{{site.data.keyword.filestorage_short}}**-Seite auszuwählen und auf die Registerkarte **Replikat** zu klicken.

 

## Wie werden Hostberechtigungen vor einem Failover auf das sekundäre Rechenzentrum angegeben?

Berechtigte (bzw. autorisierte) Hosts und Datenträger müssen sich im selben Rechenzentrum befinden. Es ist nicht möglich, den Replikatdatenträger in London und den Host in Amsterdam zu haben: beide müssen entweder in London oder in Amsterdam vorhanden sein.

1. Klicken Sie auf Ihren Quellen- oder Zieldatenträger auf der **{{site.data.keyword.filestorage_short}}**-Seite.
2. Klicken Sie auf die Registerkarte **Replikat**.
3. Blättern Sie zum Rahmen **Hosts autorisieren** nach unten und klicken Sie auf den Link **Hosts autorisieren** auf der rechten Seite der Anzeige.
4. Heben Sie den Host hervor, der für Replikationen autorisiert werden soll. Zur Auswahl mehrerer Hosts drücken Sie die Steuertaste (STRG) und klicken auf die betreffenden Hosts.
5. Klicken Sie auf **Abschicken**. Wenn Sie keine Hosts haben, bietet das Dialogfenster die Option an, Compute-Ressourcen im selben Rechenzentrum zu kaufen. Alternativ können Sie auf **Schließen** klicken.
 

## Wie wird der Snapshotbereich im Replikatrechenzentrum vergrößert, wenn der Speicherbereich im primären Rechenzentrum vergrößert wird?

Die Datenträgergrößen für Ihren primären Speicherdatenträger und Ihren Replikatspeicherdatenträger müssen übereinstimmen. Es darf kein Datenträger größer als der andere sein. Wenn Sie Ihren Snapshotbereich für Ihren primären Datenträger vergrößern, wird der Replikatbereich automatisch vergrößert. Beachten Sie, dass eine Vergrößerung des Snapshotbereichs eine unverzügliche Replikationsaktualisierung auslöst. Die Vergrößerung beider Datenträger werden als Artikelpositionen auf Ihrer Rechnung aufgeführt und wie erforderlich anteilmäßig berechnet.

Klicken Sie [hier](snapshots.html), um Informationen zur Vergrößerung Ihres Snapshotbereichs anzuzeigen.

 

## Wie wird ein Failover von einem Datenträger auf sein Replikat eingeleitet?

Bei einem Fehlerereignis können Sie über die Aktion **Failover** einen Failover auf Ihren Zieldatenträger einleiten. Der Zieldatenträger wird zu aktiven Datenträger, der letzte erfolgreich replizierte Snapshot wird aktiviert und der Datenträger wird zum Anhängen (Mount) aktiviert. Alle Daten, die seit dem letzten Replikationszyklus auf den Quellendatenträger geschrieben wurden, werden vernichtet. Beachten Sie, dass nach der Einleitung eines Failovers die **Replikationsbeziehung umgedreht wird**. Ihr Zieldatenträger ist jetzt der Quellendatenträger und Ihr vorheriger Quellendatenträger wird zu Ihrem Zieldatenträger, wie durch die Zeichenfolge **REP** hinter dem **LUN-Namen** angezeigt.

Failoveroperationen werden unter **Speicher** > **{{site.data.keyword.filestorage_short}}** im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} eingeleitet.

**Bevor Sie mit diesem Prozess fortfahren, wird empfohlen, die Verbindung zum Datenträger zu trennen. Wenn dies nicht erfolgt, kann dies zu Beschädigung und/oder Verlust von Daten führen.**

1. Klicken Sie auf Ihre aktive LUN (“Quelle”).
2. Klicken Sie auf die Registerkarte 'Replikat' und klicken Sie auf den Link 'Aktionen' in der rechten oberen Ecke.
3. Wählen Sie die Option 'Failover' aus.
   Sie empfangen eine Nachricht im oberen Bereich der Seite, die besagt, dass der Failover in Bearbeitung ist. Darüber hinaus wird ein Symbol neben Ihrem Datenträger auf der **{{site.data.keyword.filestorage_short}}**-Seite angezeigt, das darauf hinweist, dass gerade eine aktive Transaktion stattfindet. Wenn der Mauszeiger über das Symbol bewegt wird, wird die Transaktion in einem Dialogfeld angezeigt. Das Symbol wird ausgeblendet, sobald die Transaktion abgeschlossen ist. Während des Failover-Prozesses sind konfigurationsbezogene Aktionen schreibgeschützt. Sie können keinen Snapshotplan bearbeiten, keinen Snapshotbereich ändern usw. Das Ereignis wird im Replikationsprotokoll aufgezeichnet.
   Eine weitere Nachricht informiert Sie darüber, wann Ihr Zieldatenträger live verfügbar ist. An den LUN-Namen Ihres ursprünglichen Quellendatenträgers wird die Zeichenfolge REP angehängt und sein Status ist 'Inaktiv'.
4. Klicken Sie auf den Link **Alle {{site.data.keyword.filestorage_short}}-Speicher anzeigen** in der rechten oberen Ecke.
5. Klicken Sie auf Ihre aktive LUN (d. h. Ihren vorherigen Zieldatenträger). Dieser Datenträger hat jetzt den Status **Aktiv**.
6. Hängen Sie Ihren Speicherdatenträger am Host an und verbinden Sie ihn.
 

## Wie wird ein Fallback von einem Datenträger aus sein Replikat eingeleitet?

Wenn Ihr ursprünglicher Quellendatenträger repariert ist, können Sie über die Aktion **Fallback** einen kontrollierten Fallback auf Ihren ursprünglichen Quellendatenträger einleiten. Bei einem kontrollierten Fallback geschieht Folgendes:

- Der aktive Quellendatenträger wird offline geschaltet.
- Ein Snapshot wird erfasst.
- Der Replikationszyklus wird abgeschlossen.
- Der soeben erfasste Datensnapshot wird aktiviert.
- Und der Quellendatenträger wird zum Anhängen (Mount) aktiviert.

Beachten Sie, dass nach der Einleitung eines Fallbacks die **Replikationsbeziehung erneut umgedreht wird**. Ihr Quellendatenträger wird als Quellendatenträger wiederhergestellt und Ihr Zieldatenträger wird wieder zum Zieldatenträger, wie durch die Zeichenfolge **REP** hinter dem **LUN-Namen** angezeigt.

Fallbacks werden unter **Speicher** > **{{site.data.keyword.filestorage_short}}** im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} eingeleitet.

1. Klicken Sie auf Ihre aktive Endurance-LUN (“Ziel”).
2. Klicken Sie auf die Registerkarte **Replikat** und klicken Sie auf den Link **Aktionen** in der rechten oberen Ecke.
3. Wählen Sie **Fallback** aus.
   Sie empfangen eine Nachricht im oberen Bereich der Seite, die besagt, dass der Failover in Bearbeitung ist. Darüber hinaus wird ein Symbol neben Ihrem Datenträger auf der **{{site.data.keyword.filestorage_short}}**-Seite angezeigt, das darauf hinweist, dass gerade eine aktive Transaktion stattfindet. Wenn der Mauszeiger über das Symbol bewegt wird, wird die Transaktion in einem Dialogfeld angezeigt. Das Symbol wird ausgeblendet, sobald die Transaktion abgeschlossen ist. Während des Failback-Prozesses sind konfigurationsbezogene Aktionen schreibgeschützt. Sie können keinen Snapshotplan bearbeiten, keinen Snapshotbereich ändern usw. Das Ereignis wird im Replikationsprotokoll aufgezeichnet.
   Eine weitere Nachricht informiert Sie darüber, wann Ihr Quellendatenträger live verfügbar ist. Ihr Zieldatenträger hat jetzt den Status 'Inaktiv'.
4. Klicken Sie auf den Link **Alle {{site.data.keyword.filestorage_short}}-Speicher anzeigen** in der rechten oberen Ecke.
5. Klicken Sie auf Ihre aktive Endurance-LUN (Quelle). Dieser Datenträger hat jetzt den Status **Aktiv**.
6. Hängen Sie Ihren Speicherdatenträger am Host an und verbinden Sie ihn. Weitere Anweisungen finden Sie [hier](provisioning-file-storage.html).
 

## Wie wird das Replikationsprotokoll angezeigt?

Das Replikationsprotokoll wird im **Prüfprotokoll** über die Registerkarte **Konto** unter **Verwalten** angezeigt. Der primäre und der Replikatdatenträger zeigen ein identisches Replikationsprotokoll an, das folgende Informationen enthält:

- Typ für die Replikation (Failover oder Fallback)
- Zeitpunkt der Einleitung
- Für die Replikation verwendeter Snapshot
- Größe der Replikation
- Zeitpunkt des Abschlusses
 

## Wie wird eine vorhandene Replikation abgebrochen?

Der Abbruch kann entweder sofort oder zum Rechnungsstichtag erfolgen, sodass die Rechnungsstellung beendet wird. Die Replikation kann über die Registerkarte **Primär** oder **Replikat** abgebrochen werden.

1. Klicken Sie auf der Seite **{{site.data.keyword.filestorage_short}}** auf den Datenträger.
2. Klicken Sie auf die Dropdown-Liste **Aktionen** auf der Registerkarte **Primär** bzw. **Replikat**.
3. Wählen Sie **Replikation abbrechen** aus.
4. Wählen Sie den Abbruchtermin aus – **Sofort** oder **Rechnungsstichtag** und klicken Sie auf **Weiter**.
5. Klicken Sie auf das Kontrollkästchen **Ich bin mir bewusst, dass es durch den Abbruch zu Datenverlust kommen kann** und klicken Sie auf **Replikation abbrechen**.
 

## Wie wird die Replikation abgebrochen, wenn der Primärdatenträger storniert wird?

Wenn ein primärer Datenträger storniert wurde, werden der Replikationsplan und der Datenträger im Replikatrechenzentrum gelöscht. Replikate werden auf der Seite **{{site.data.keyword.filestorage_short}}** abgebrochen.

 1. Heben Sie Ihren Datenträger auf der Seite **{{site.data.keyword.filestorage_short}}** hervor.
 2. Klicken Sie auf das Dropdown-Menü **Aktionen** und wählen sie **Für {{site.data.keyword.filestorage_short}} stornieren**.
 3. Wählen Sie den Stornierungstermin auf – **Sofort** oder **Rechnungsstichtag** und klicken Sie auf **Weiter**.
 4. Klicken Sie auf das Kontrollkästchen **Ich bin mir bewusst, dass es durch den Abbruch zu Datenverlust kommen kann*** und* klicken Sie auf '** stornieren'.
