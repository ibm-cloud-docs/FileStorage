---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-11"

keywords: File Storage, file storage, NFS, replication, duplication, synchronous, replica schedule, replica space, disaster recovery

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}


# Daten replizieren
{: #replication}

Bei der Replikation wird einer von vier Snapshotplänen verwendet, um Snapshots automatisch auf einen Zieldatenträger in einem fernen Rechenzentrum zu kopieren. Die Kopien können am fernen Standort wiederhergestellt werden, falls ein Elementarereignis auftritt oder Ihre Daten beschädigt werden.

Die Replikation hält Ihre Daten an zwei verschiedenen Positionen synchron. Wenn Sie Ihren Datenträger klonen und unabhängig vom ursprünglichen Datenträger verwenden möchten, lesen Sie die Informationen im Abschnitt [Duplikat eines Datenträgers erstellen](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume).
{:tip}

Bevor Sie replizieren können, müssen Sie einen Snapshotplan erstellen.
{:important}


## Fernes Rechenzentrum für replizierten Speicherdatenträger ermitteln

Die Rechenzentren von {{site.data.keyword.cloud}} wurden weltweit in Paare aus einem primären und einem fernen Datenträger unterteilt.
In Tabelle 1 finden Sie die vollständige Liste der verfügbaren Rechenzentren und Replikationsziele.

| US 1 | US 2 | Lateinamerika | Kanada  | Europa  | Asien-Pazifik  | Australien  |
|-----|-----|-----|-----|-----|-----|-----|
| DAL01<br />DAL05<br />DAL06<br />HOU02<br />SJC01<br />WDC01 | SJC03<br />SJC04<br />WDC04<br />WDC06<br />WDC07<br />DAL09<br />DAL10<br />DAL12<br />DAL13 | MEX01<br />SAO01 | TOR01<br />MON01 | AMS01<br />AMS03<br />FRA02<br />FRA04<br />FRA05<br />LON02<br />LON04<br />LON05<br />LON06<br />OSL01<br />PAR01<br />MIL01 | HKG02<br />TOK02<br />TOK04<br />TOK05<br />SNG01<br />SEO01<br />CHE01 | SYD01<br />SYD04<br />SYD05<br />MEL01 |
{: caption="Tabelle – Diese Tabelle zeigt die vollständige Liste der Rechenzentren mit erweiterten Funktionen, die in den einzelnen Regionen vorhanden sind. Jede Region bildet eine eigene Spalte. Einige Städte wie beispielsweise Dallas, San Jose, Washington D.C., Amsterdam, Frankfurt, London und Sydney haben mehrere Rechenzentren. Rechenzentren in der Region US 1 verfügen NICHT über erweiterten Speicher. Hosts in Rechenzentren mit erweiterten Speicherleistungsmerkmalen können die Replikation mit Replikationszielen in Rechenzentren der Region US 1 nicht einleiten." caption-side="top"}


## Erstreplikat erstellen

Replikationen arbeiten nach einem Snapshotplan. Sie müssen zuerst einen Snapshotbereich und einen Snapshotplan für den Quellendatenträger haben, bevor Sie replizieren können. Wenn Sie versuchen, die Replikation zu konfigurieren, und eines dieser beiden Dinge fehlt, werden Sie aufgefordert, mehr Speicherplatz zu kaufen oder einen Zeitplan einzurichten. Replikationen werden unter **Speicher** > **{{site.data.keyword.filestorage_short}}** im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} verwaltet.

1. Klicken Sie auf Ihren Speicherdatenträger.
2. Klicken Sie auf **Replikat** und auf **Replikation kaufen**.
3. Wählen Sie einen vorhandenen Snapshotplan aus, den Ihre Replikation befolgen soll. Die Liste enthält alle Ihre aktiven Snapshotpläne. <br />

   Sie können nur einen einzigen Plan auswählen, selbst wenn Sie einen Mix aus stündlicher, täglicher und wöchentlicher Rechnungsstellung haben. Alle Snapshots, die seit dem vorherigen Replikationszyklus erfasst wurden, werden unabhängig von dem Zeitplan repliziert, von dem sie stammen.<br />Wenn Sie keine Screenshots eingerichtet haben, werden Sie dazu aufgefordert, damit Sie die Replikation bestellen können. Weitere Informationen finden Sie unter [Mit Snapshots arbeiten](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots).
   {:tip}
3. Klicken Sie auf **Position** und wählen Sie das Rechenzentrum (Data Center) aus, das als Ihre Disaster Recovery-Site (DR-Site) fungiert.
4. Klicken Sie auf **Weiter**.
5. Geben Sie einen **Werbeaktionscode** ein, falls Sie über einen verfügen, und klicken Sie auf **Neu berechnen**. Die anderen Felder im Fenster sind standardmäßig ausgefüllt.

   Rabatte werden bei der Verarbeitung der Bestellung angewendet.
   {:note}
6. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Bestellung abschicken**.


## Vorhandene Replikation bearbeiten

Sie können im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} entweder auf der Registerkarte **Primär** oder auf der Registerkarte **Replikat** unter **Speicher** > **{{site.data.keyword.filestorage_short}}** Ihren Replikationsplan bearbeiten und Ihren Replikationsspeicherbereich ändern.


## Replikationsplan bearbeiten

Ihr Replikationsplan basiert auf einem vorhandenen Snapshotplan. Um den Replikationsplan von 'Stündlich' in 'Täglich' oder 'Wöchentlich' zu ändern oder umgekehrt, müssen Sie den Replikatdatenträger löschen und einen neuen einrichten.

Wenn Sie jedoch die Tageszeit ändern möchten, zu der die **tägliche** Replikation stattfindet, können Sie den vorhandenen Zeitplan auf der Registerkarte des Primärdatenträgers oder des Replikatdatenträgers ändern.

1. Klicken Sie auf der Registerkarte **Primär** bzw. **Replikat** auf **Aktionen**.
2. Wählen Sie die Option **Snapshotplan bearbeiten** aus.
3. Prüfen Sie das Feld **Snapshot** unter **Zeitplan**, um festzustellen, welchen Zeitplan Sie für die Replikation verwenden. Ändern Sie den gewünschten Zeitplan.
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

Weitere Informationen zum Vergrößern des Snapschotbereichs finden Sie unter [Snapshots](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots).
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

Sie können ein Duplikat eines vorhandenen {{site.data.keyword.cloud}}  {{site.data.keyword.filestorage_full}}-Datenträgers erstellen. Das Duplikat übernimmt standardmäßig die Kapazitäts- und Leistungsoptionen des Originaldatenträgers und enthält bis zum Zeitpunkt eines Snapshots eine Kopie der Daten.

Duplikate können sowohl von primären Datenträgern als auch von Replikatdatenträgern erstellt werden. Das neue Duplikat wird im selben Rechenzentrum wie der ursprüngliche Datenträger erstellt. Wenn Sie einen Duplikatdatenträger von einem Replikatdatenträger erstellen, wird der neue Datenträger im selben Rechenzentrum wie der Replikatdatenträger erstellt.

Auf Duplikatdatenträger kann ein Host für Lese-/Schreiboperationen zugreifen, sobald der Speicher bereitgestellt ist. Allerdings sind Snapshots und die Replikation erst zulässig, wenn das Erstellen der Datenkopie vom ursprünglichen Datenträger auf den Duplikatdatenträger abgeschlossen ist.

Weitere Informationen finden Sie im Abschnitt [Duplikat eines Datenträgers erstellen](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume).

## Verwenden von Replikaten für ein Failover bei einem Ausfall

Wenn Sie einen Failover durchführen, 'schalten Sie um', und zwar von Ihrem Speicherdatenträger in Ihrem primären Rechenzentrum auf den Zieldatenträger in Ihrem fernen Rechenzentrum. Ihr primäres Rechenzentrum ist zum Beispiel London und Ihr sekundäres Rechenzentrum ist Amsterdam. Wenn ein Fehlerereignis auftritt, führen Sie ein Failover auf Amsterdam durch – dazu stellen Sie zu dem jetzigen primären Datenträger eine Verbindung von einer Compute-Instanz in Amsterdam her. Nachdem Ihr Datenträger in London repariert wurde, wird ein Snapshot des Datenträgers in Amsterdam erstellt, um die Rückübertragung auf den Datenträger in London und den nun wieder primären Datenträger von einer Compute-Instanz in London durchzuführen.

* Wenn das Problem in der primäre Lokation auftritt, aber der Speicher und der Host noch online sind, finden Sie weitere Informationen unter [Failover mit einem zugänglichen Primärdatenträger](/docs/infrastructure/FileStorage?topic=FileStorage-dr-accessible).
* Wenn die primäre Lokation inaktiv ist, lesen Sie die Informationen im Abschnitt [Failover mit einem nicht zugänglichen Primärdatenträger](/docs/infrastructure/FileStorage?topic=FileStorage-dr-inaccessible).

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

## SLCLI-Befehle im Zusammenhang mit der Replikation
{: #clicommands}

* Geeignete Replikationsrechenzentren für einen bestimmten Datenträger auflisten.
  ```
  # slcli file replica-locations --help
  Syntax: slcli file replica-locations [OPTIONEN] DATENTRÄGER-ID

  Optionen:
  --sortby TEXT   Spalten für die Sortierung
  --columns TEXT  Spalten für die Anzeige. Optionen: ID, langer Name, kurzer Name
  -h, --help      Diese Nachricht anzeigen und Ausführung beenden.
  ```

* Dateispeicherreplikatdatenträger bestellen.
  ```
  # slcli file replica-order --help
  Syntax: slcli file replica-order [OPTIONEN] DATENTRÄGER-ID

  Optionen:
  -s, --snapshot-schedule [INTERVAL|HOURLY|DAILY|WEEKLY]
                                  Snapshotplan für die Replikation,
                                  (INTERVAL | HOURLY | DAILY | WEEKLY)
                                  [erforderlich]
  -l, --location TEXT             Kurzname des Rechenzentrums für den
                                  Replikatdatenträger (z. B. dal09)  [erforderlich]
  --tier [0.25|2|4|10]            Endurance-Speichertier (IOPS pro GB) des primären
                                  Datenträgers, für den ein Replikatdatenträger
                                  bestellt wird [optional]
  -h, --help                      Diese Nachricht anzeigen und Ausführung beenden.
  ```

* Vorhandene Replikatdatenträger für einen Dateidatenträger auflisten.
  ```
  # slcli file replica-partners --help
  Syntax: slcli file replica-partners [OPTIONEN] DATENTRÄGER-ID

  Optionen:
  --sortby TEXT   Spalten für die Sortierung
  --columns TEXT  Spalten für die Anzeige. Optionen: ID, Benutzername, Konto-ID,
                  Kapazität (GB), Hardware-ID, Gastsystem-ID, Host-ID
  -h, --help      Diese Nachricht anzeigen und Ausführung beenden.
  ```

* Failover eines Dateidatenträgers auf einen bestimmten Replikatdatenträger durchführen.
  ```
  # slcli file replica-failover --help
  Syntax: slcli file replica-failover [OPTIONEN] DATENTRÄGER-ID

  Optionen:
  --replicant-id TEXT  ID des Replikatdatenträgers
  --immediate          Sofortige Funktionsübernahme durch den Replikatdatenträger.
  -h, --help           Diese Nachricht anzeigen und Ausführung beenden.
  ```

* Failback eines Dateidatenträgers von einem bestimmten Replikatdatenträger durchführen.
  ```
  # slcli file replica-failback --help
  Syntax: slcli file replica-failback [OPTIONEN] DATENTRÄGER-ID

  Optionen:
  --replicant-id TEXT  ID des Replikatdatenträgers
  -h, --help           Diese Nachricht anzeigen und Ausführung beenden.
  ```
