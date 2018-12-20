---

copyright:
  years: 2015, 2018
lastupdated: "2018-12-11"

---

{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}


# Daten replizieren

Bei der Replikation wird einer von vier Snapshotplänen verwendet, um Snapshots automatisch auf einen Zieldatenträger in einem fernen Rechenzentrum zu kopieren. Die Kopien können am fernen Standort wiederhergestellt werden, falls ein Elementarereignis auftritt oder Ihre Daten beschädigt werden.

Die Replikation hält Ihre Daten an zwei verschiedenen Positionen synchron. Wenn Sie Ihren Datenträger klonen und unabhängig vom ursprünglichen Datenträger verwenden möchten, lesen Sie die Informationen im Abschnitt [Duplikat eines Datenträgers erstellen](how-to-create-duplicate-volume.html).
{:tip}

Bevor Sie replizieren können, müssen Sie einen Snapshotplan erstellen.
{:important}


## Fernes Rechenzentrum für replizierten Speicherdatenträger ermitteln

Die verfügbaren Rechenzentren von {{site.data.keyword.BluSoftlayer_full}} wurden weltweit zu Paaren aus primären und fernen Rechenzentren kombiniert.
In Tabelle 1 finden Sie die vollständige Liste der verfügbaren Rechenzentren und Replikationsziele.

<table>
  <caption style="text-align: left;"><p>Tabelle – Diese Tabelle zeigt die vollständige Liste der Rechenzentren mit erweiterten Funktionen, die in den einzelnen Regionen vorhanden sind. Jede Region bildet eine eigene Spalte. Einige Städte wie beispielsweise Dallas, San Jose, Washington D.C., Amsterdam, Frankfurt, London und Sydney haben mehrere Rechenzentren.</p>
  <p>&#42; Datenzentren in der Region US 1 haben KEINEN erweiterten Speicher. Hosts in Rechenzentren, die eine erweiterte Funktionalität für Speicher aufweisen, können die Replikation <strong>nicht</strong> mit Replikationszielen in Rechenzentren der Region US 1 starten.</p>
  </caption>
  <thead>
    <tr>
      <th>US 1 &#42;</th>
      <th>US 2</th>
      <th>Lateinamerika</th>
      <th>Kanada</th>
      <th>Europa</th>
      <th>Asien-Pazifik</th>
      <th>Australien</th>
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
	  <br /><br /><br /><br /><br /><br />
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
	  <br /><br /><br />
      </td>
      <td>MEX01<br />
	  SAO01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
      <td>TOR01<br />
          MON01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
      <td>AMS01<br />
	  AMS03<br />
	  FRA02<br />
	  FRA04<br />
	  FRA05<br />
	  LON02<br />
	  LON04<br />
	  LON05<br />
	  LON06<br />
	  OSL01<br />
	  PAR01<br />
	  MIL01<br />
      </td>
      <td>HKG02<br />
          TOK02<br />
	  TOK04<br />
	  TOK05<br />
	  SNG01<br />
	  SEO01<br />
          CHE01<br />
	  <br /><br /><br /><br /><br />
      </td>
      <td>SYD01<br />
          SYD04<br />
          SYD05<br />
          MEL01<br />
          <br /><br /><br /><br /><br /><br /><br /><br />
      </td>
    </tr>
  </tbody>
</table>

## Erstreplikat erstellen

Replikationen arbeiten nach einem Snapshotplan. Sie müssen zuerst einen Snapshotbereich und einen Snapshotplan für den Quellendatenträger haben, bevor Sie replizieren können. Wenn Sie versuchen, die Replikation zu konfigurieren, und eines dieser beiden Dinge fehlt, werden Sie aufgefordert, mehr Speicherplatz zu kaufen oder einen Zeitplan einzurichten. Replikationen werden unter **Storage** > **{{site.data.keyword.filestorage_short}}** im [{{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){:new_window} verwaltet.

1. Klicken Sie auf Ihren Speicherdatenträger.
2. Klicken Sie auf **Replikat** und auf **Replikation kaufen**.
3. Wählen Sie einen vorhandenen Snapshotplan aus, den Ihre Replikation befolgen soll. Die Liste enthält alle Ihre aktiven Snapshotpläne. <br />

   Sie können nur einen einzigen Plan auswählen, selbst wenn Sie einen Mix aus stündlicher, täglicher und wöchentlicher Rechnungsstellung haben. Alle Snapshots, die seit dem vorherigen Replikationszyklus erfasst wurden, werden unabhängig von dem Zeitplan repliziert, von dem sie stammen.<br />Wenn Sie keine Screenshots eingerichtet haben, werden Sie dazu aufgefordert, damit Sie die Replikation bestellen können. Weitere Informationen finden Sie unter [Mit Snapshots arbeiten](snapshots.html).
   {:tip}
3. Klicken Sie auf **Position** und wählen Sie das Rechenzentrum (Data Center) aus, das als Ihre Disaster Recovery-Site (DR-Site) fungiert.
4. Klicken Sie auf **Weiter**.
5. Geben Sie einen **Werbeaktionscode** ein, falls Sie über einen verfügen, und klicken Sie auf **Neu berechnen**. Die anderen Felder im Fenster sind standardmäßig ausgefüllt.
6. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Bestellung abschicken**.


## Vorhandene Replikation bearbeiten

Sie können entweder in der Registerkarte **Primäres** oder **Replikation** unter **Speicher** > **{{site.data.keyword.filestorage_short}}** vom [{{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){:new_window} Ihren Replikationsplan bearbeiten und Ihren Replikationsspeicherbereich ändern.


## Replikationsplan bearbeiten

Ihr Replikationsplan basiert auf einem vorhandenen Snapshotplan. Wenn Sie den Replikationsplan zum Beispiel von 'Stündlich' in 'Wöchentlich' ändern möchten, müssen Sie den Replikationsplan abbrechen und einen neuen erstellen.

Das Ändern des Zeitplans kann über die Registerkarte "Primär" oder "Replikat" erfolgen.

1. Klicken Sie auf der Registerkarte **Primär** bzw. **Replikat** auf **Aktionen**.
2. Wählen Sie die Option **Snapshotplan bearbeiten** aus.
3. Prüfen Sie das Feld **Snapshot** unter **Zeitplan**, um festzustellen, welchen Zeitplan Sie für die Replikation verwenden. Ändern Sie den gewünschten Zeitplan. Wenn Ihr Replikationsplan zum Beispiel auf **Täglich** eingestellt ist, können Sie die Tageszeit ändern, zu der die Replikation stattfinden soll.
4. Klicken Sie auf **Speichern**.


## Replikationsbereich ändern

Ihr primärer Snapshotbereich und Ihr Replikatbereich müssen identisch sein. Wenn Sie den Speicherbereich auf der Registerkarte **Primär** oder **Replikat** ändern, wird dadurch automatisch Speicherbereich in Ihrem Quellen- und Ihrem Zielrechenzentrum hinzugefügt. Eine Erhöhung des Snapshotbereichs löst eine sofortige Replikationsaktualisierung aus.

1. Klicken Sie auf der Registerkarte **Primär** bzw. **Replikat** auf **Aktionen**.
2. Wählen Sie die Option **Weiteren Snapshotbereich hinzufügen** hinzufügen) aus.
3. Wählen Sie die Speichergröße in der Liste aus und klicken Sie auf **Weiter**.
4. Geben Sie einen **Werbeaktionscode** (Promo Code) ein, wenn Sie einen haben, und klicken Sie auf **Neu berechnen**. Die anderen Felder im Dialogfenster sind standardmäßig ausgefüllt.
5. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Bestellung abschicken**.


## Snapshotbereich im Replikatdatenzentrum erhöhen, wenn der Snapshotbereich im primären Rechenzentrum erhöht wird

Die Datenträgergröße Ihres primären Speicherdatenträgers und Ihres Replikatspeicherdatenträgers muss übereinstimmen. Keiner der Datenträger darf größer als der andere sein. Wenn Sie Ihren Snapshotbereich für Ihren primären Datenträger vergrößern, wird der Replikatbereich automatisch vergrößert. Eine Erhöhung des Snapshotbereichs löst eine sofortige Replikationsaktualisierung aus. Die Vergrößerung beider Datenträger wird als Artikelpositionen auf Ihrer Rechnung aufgeführt und wie erforderlich anteilmäßig berechnet.

Weitere Informationen zum Vergrößern des Snapschotbereichs finden Sie unter [Snapshots](snapshots.html).
## Replikatdatenträger in der Datenträgerliste anzeigen

Sie können Ihre Replikationsdatenträger auf der {{site.data.keyword.filestorage_short}}-Seite unter **Speicher** > **{{site.data.keyword.filestorage_short}}** anzeigen. Der Datenträgername ist der Name des primären Datenträgers, gefolgt von REP. Der **Typ** ist Endurance oder Performance – Replikat. Die **Zieladresse** ist 'n/v,' da der Replikatdatenträger nicht am Replikatrechenzentrum angehängt ist, und der **Status** ist 'Inaktiv'.


## Details eines replizierten Datenträgers im Replikatrechenzentrum anzeigen

Sie können die Details von Replikatdatenträgern auf der Registerkarte **Replikat** unter **Speicher** > **{{site.data.keyword.filestorage_short}}** anzeigen. Eine weitere Option besteht darin, den Replikatdatenträger auf der **{{site.data.keyword.filestorage_short}}**-Seite auszuwählen und auf die Registerkarte **Replikat** zu klicken.


## Replikationsprotokoll anzeigen

Das Replikationsprotokoll wird auf der Registerkarte **Konto** unter **Verwalten** im **Prüfprotokoll** angezeigt. Der primäre und der Replikatdatenträger zeigen ein identisches Replikationsprotokoll an, das folgende Informationen enthält:

- Art der Replikation (Failover oder Rückübertragung)
- Zeitpunkt der Einleitung
- Snapshot, der für die Replikation verwendet wurde
- Größe der Replikation
- Zeitpunkt des Abschlusses


## Duplikat eines Replikats erstellen

Sie können ein Duplikat eines vorhandenen {{site.data.keyword.BluSoftlayer_full}} {{site.data.keyword.filestorage_full}}-Datenträgers erstellen. Das Duplikat übernimmt standardmäßig die Kapazitäts- und Leistungsoptionen des Originaldatenträgers und enthält bis zum Zeitpunkt eines Snapshots eine Kopie der Daten.

Duplikate können sowohl von primären Datenträgern als auch von Replikatdatenträgern erstellt werden. Das neue Duplikat wird im selben Rechenzentrum wie der ursprüngliche Datenträger erstellt. Wenn Sie einen Duplikatdatenträger von einem Replikatdatenträger erstellen, wird der neue Datenträger im selben Rechenzentrum wie der Replikatdatenträger erstellt.

Auf Duplikatdatenträger kann ein Host für Lese-/Schreiboperationen zugreifen, sobald der Speicher bereitgestellt ist. Allerdings sind Snapshots und die Replikation erst zulässig, wenn das Erstellen der Datenkopie vom ursprünglichen Datenträger auf den Duplikatdatenträger abgeschlossen ist.

Weitere Informationen finden Sie im Abschnitt [Duplikat eines Datenträgers erstellen](how-to-create-duplicate-volume.html).

## Verwenden von Replikaten für ein Failover bei einem Ausfall

Wenn Sie einen Failover durchführen, 'schalten Sie um', und zwar von Ihrem Speicherdatenträger in Ihrem primären Rechenzentrum auf den Zieldatenträger in Ihrem fernen Rechenzentrum. Ihr primäres Rechenzentrum ist zum Beispiel London und Ihr sekundäres Rechenzentrum ist Amsterdam. Wenn ein Fehlerereignis auftritt, führen Sie ein Failover auf Amsterdam durch – dazu stellen Sie zu dem jetzigen primären Datenträger eine Verbindung von einer Compute-Instanz in Amsterdam her. Nachdem Ihr Datenträger in London repariert wurde, wird ein Snapshot des Datenträgers in Amsterdam erstellt, um die Rückübertragung auf den Datenträger in London und den nun wieder primären Datenträger von einer Compute-Instanz in London durchzuführen.

* Wenn das Problem in der primäre Lokation auftritt, aber der Speicher und der Host noch online sind, finden Sie weitere Informationen unter [Failover mit einem zugänglichen Primärdatenträger](dr-accessible-primary.html).
* Wenn die primäre Lokation inaktiv ist, lesen Sie die Informationen im Abschnitt [Failover mit einem nicht zugänglichen Primärdatenträger](disaster-recovery.html).

## Vorhandene Replikation stornieren

Sie können eine Replikation entweder sofort oder zum Rechnungsstichtag stornieren, sodass die Rechnungsstellung beendet wird. Die Replikation kann auf der Registerkarte **Primär** oder **Replikat** abgebrochen werden.

1. Klicken Sie auf der Seite **{{site.data.keyword.filestorage_short}}** auf den Datenträger.
2. Klicken Sie auf der Registerkarte **Primär** bzw. **Replikat** auf **Aktionen**.
3. Wählen Sie **Replikation abbrechen** aus.
4. Wählen Sie einen Zeitpunkt für den Abbruch aus. Wählen Sie **Sofort** oder **Rechnungsstichtag** aus und klicken Sie auf **Weiter**.
5. Klicken Sie auf **Ich bin mir bewusst, dass es durch den Abbruch zu Datenverlust kommen kann** und klicken Sie auf **Replikation abbrechen**.


## Replikation abbrechen, wenn der Primärdatenträger storniert wird

Wenn ein primärer Datenträger storniert wurde, werden der Replikationsplan und der Datenträger im Replikatrechenzentrum gelöscht. Replikate werden auf der Seite **{{site.data.keyword.filestorage_short}}** abgebrochen.

 1. Heben Sie Ihren Datenträger auf der Seite **{{site.data.keyword.filestorage_short}}** hervor.
 2. Klicken Sie auf **Aktionen** und wählen sie **Für {{site.data.keyword.filestorage_short}} stornieren**.
 3. Wählen Sie aus, wann der Datenträger abgebrochen werden soll. Wählen Sie **Sofort** oder **Rechnungsstichtag** aus und klicken Sie auf **Weiter**.
 4. Klicken Sie auf **Ich bin mir bewusst, dass es durch den Abbruch zu Datenverlust kommen kann** und klicken Sie auf **Abbrechen**.
