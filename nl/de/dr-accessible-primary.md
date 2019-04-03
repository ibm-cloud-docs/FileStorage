---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, disaster recovery, duplicate volume, replica volume, failover, failback,

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Disaster-Recovery - Failover mit einem zugänglichen Primärdatenträger
{: #dr-accessible}

Wenn eine Betriebsunterbrechung oder einer Katastrophe, die einen Ausfall am primären Standort verursacht, auftritt und der primäre Speicher noch zugänglich ist, können Kunden die folgenden Aktionen ausführen, um schnell am sekundären Standort auf ihre Daten zuzugreifen.

Bevor Sie mit dem Failover beginnen, stellen Sie sicher, dass die gesamte Hostberechtigung in Kraft ist.

Berechtigte (bzw. autorisierte) Hosts und Datenträger müssen sich im selben Rechenzentrum befinden. Wenn sich der Replikatdatenträger beispielsweise in London befindet, kann sich der zugehörige Host nicht in Amsterdam befinden. Beide müssen entweder in London oder in Amsterdam sein.
{:note}

1. Melden Sie sich an [der {{site.data.keyword.cloud}}-Konsole ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/catalog/){:new_window} an und klicken Sie links oben auf das Symbol **Menü**. Wählen Sie **Klassische Infrastruktur** aus.


   Alternativ können Sie sich beim [{{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){:new_window} anmelden.
1. Klicken Sie auf Ihren Quellen- oder Zieldatenträger auf der **{{site.data.keyword.filestorage_short}}**-Seite.
2. Klicken Sie auf **Replikat**.
3. Blättern Sie abwärts zum Rahmen **Hosts autorisieren** und klicken Sie auf der rechten Seite auf **Hosts autorisieren**.
4. Heben Sie den Host hervor, der für Replikationen autorisiert werden soll. Zur Auswahl mehrerer Hosts drücken Sie die Steuertaste (STRG) und klicken Sie auf die betreffenden Hosts.
5. Klicken Sie auf **Abschicken**. Wenn Sie keine Hosts haben, werden Sie aufgefordert, Rechenressourcen im selben Rechenzentrum zu kaufen.

## Failover von einem Datenträger auf sein Replikat starten

Bei einem Fehlerereignis können Sie einen **Failover** auf Ihren Zieldatenträger einleiten. Der Zieldatenträger wird aktiv. Der letzte erfolgreich replizierte Snapshot wird aktiviert und der Datenträger wird zum Anhängen (Mount) aktiviert. Alle Daten, die seit dem letzten Replikationszyklus auf den Quellendatenträger geschrieben wurden, gehen verloren. Beim Start eines Failovers wird die Replikationsbeziehung umgekehrt. Ihr Zieldatenträger wird zum Quellendatenträger und Ihr früherer Quellendatenträger wird zum Zieldatenträger. Dies wird durch den **LUN-Namen** angezeigt, gefolgt von der Zeichenfolge **REP**.

Failover werden unter **Speicher** **{{site.data.keyword.filestorage_short}}** im [{{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){:new_window} gestartet.

Bevor Sie mit den folgenden Schritten fortfahren, unterbrechen Sie die Verbindung zum Datenträger. Wenn Sie das nicht tun, sind Datenbeschädigungen und/oder Datenverlust die Folge.
{:important}

1. Klicken Sie auf Ihren aktiven Datenträger ('Quelle').
2. Klicken Sie rechts oben auf **Replikat** und **Aktionen**.
3. Wählen Sie die Option für **Failover** aus.

   Es wird die Nachricht angezeigt, dass der Failover in Bearbeitung ist. Darüber hinaus wird neben Ihrem Datenträger auf der **{{site.data.keyword.filestorage_short}}**-Seite ein Symbol angezeigt, das darauf hinweist, dass zurzeit eine Transaktion aktiv ist. Bei Bewegen des Mauszeigers über das Symbol wird die Transaktion in einem Fenster angezeigt. Das Symbol wird ausgeblendet, sobald die Transaktion abgeschlossen ist. Während des Failover-Prozesses sind konfigurationsbezogene Aktionen schreibgeschützt. Sie können Snapshotpläne nicht bearbeiten oder Snapshotbereiche ändern. Das Ereignis wird im Replikationsprotokoll aufgezeichnet.<br/> Wenn der Zieldatenträger aktiv ist, wird eine andere Nachricht angezeigt. Der LUN-Name Ihres ursprünglichen Quellendatenträgers wird so aktualisiert, dass er mit "REP" endet, und sein Status ändert sich in "Inaktiv".
   {:note}
4. Klicken Sie auf **Alle anzeigen ({{site.data.keyword.filestorage_short}})**.
5. Klicken Sie auf Ihren aktiven Datenträger (früher Ihr Zieldatenträger). Dieser Datenträger hat nun den Status **Aktiv**.
6. Hängen Sie Ihren Speicherdatenträger an den Host an und verbinden Sie ihn. Weitere Anweisungen finden Sie [hier](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole).


## Rückübertragung von einem Datenträger auf sein Replikat starten

Wenn Ihr ursprünglicher Quellendatenträger repariert ist, können Sie eine gesteuerte Rückübertragung auf Ihren Originalquellendatenträger starten. Bei einer gesteuerten Rückübertragung geschieht Folgendes:

- Der aktive Quellendatenträger wird offline geschaltet.
- Ein Snapshot wird erfasst.
- Der Replikationszyklus wird abgeschlossen.
- Der soeben erfasste Datensnapshot wird aktiviert.
- Und der Quellendatenträger wird für das Anhängen (den Mount) aktiviert.

Beim Start einer Rückübertragung wird die Replikationsbeziehung wieder umgekehrt. Ihr Quellendatenträger wird als Quellendatenträger wiederhergestellt und Ihr Zieldatenträger ist wieder Ihr Zieldatenträger. Dies wird durch den **LUN-Namen** angezeigt, gefolgt von der Zeichenfolge **REP**.

Failbacks werden unter **Speicher** **{{site.data.keyword.filestorage_short}}** im [{{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){:new_window} gestartet.

1. Klicken Sie auf Ihren aktiven Datenträger ("Ziel").
2. Klicken Sie rechts oben auf **Replikat** und **Aktionen**.
3. Wählen Sie **Rückübertragung** aus.

   Es wird die Nachricht angezeigt, dass die Rückübertragung in Bearbeitung ist. Darüber hinaus wird neben Ihrem Datenträger auf der **{{site.data.keyword.filestorage_short}}**-Seite ein Symbol angezeigt, das darauf hinweist, dass zurzeit eine Transaktion aktiv ist. Bei Bewegen des Mauszeigers über das Symbol wird die Transaktion in einem Fenster angezeigt. Das Symbol wird ausgeblendet, sobald die Transaktion abgeschlossen ist. Während des Prozesses der Rückübertragung sind konfigurationsbezogene Aktionen schreibgeschützt. Sie können Snapshotpläne nicht bearbeiten oder Snapshotbereiche ändern. Das Ereignis wird im Replikationsprotokoll aufgezeichnet.
   {:note}
4. Klicken Sie rechts oben auf den Link **Alle {{site.data.keyword.filestorage_short}}-Instanzen anzeigen**.
5. Klicken Sie auf Ihren aktiven Datenträger ("Quelle").
6. Hängen Sie Ihren Speicherdatenträger an den Host an und verbinden Sie ihn. Weitere Anweisungen finden Sie [hier](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole).
