---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS,

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Speicherbegrenzungen verwalten
{: #managinglimits}

Standardmäßig können Sie global insgesamt 250 {{site.data.keyword.blockstorageshort}}- und {{site.data.keyword.filestorage_short}}-Datenträger bereitstellen.

Wenn Sie die Anzahl Ihrer Datenträger ermitteln möchten, können Sie mit dem folgenden `slcli`-Befehl Ihre Datenträger für die einzelnen Rechenzentren auflisten.
```
# slcli file volume-count --help
Syntax: slcli file volume-count [OPTIONEN]

Optionen:
  -d, --datacenter TEXT  Kurzname des Rechnzentrums
  --sortby TEXT          Spalten für die Sortierung
  -h, --help             Diese Nachricht anzeigen und Ausführung beenden.
```

Sie können eine Erhöhung der Begrenzung anfordern, indem Sie ein Ticket beim [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} einreichen. Wenn die Anforderung genehmigt ist, wird eine Datenträgerbegrenzung für ein bestimmtes Rechenzentrum festgelegt.  

Zum Anfordern einer Erhöhung einer Begrenzung öffnen Sie ein Ticket und richten es an Ihren Vertriebsbeauftragten.

Geben Sie in diesem Ticket die folgenden Informationen an:

- **Ticketbetreff:** Anforderung einer Erhöhung der Speicherbegrenzung für Datenträgeranzahl und Rechenzentren

- **Was ist der Anwendungsfall für die Anforderung zusätzlicher Datenträger?** <br />
*Ihre Antwort könnte beispielsweise ein neuer VMware-Datenspeicher, eine neue Entwicklungs- bzw. Testumgebung (Dev/Test), eine SQL-Datenbank, Protokollierung etc. sein.*

- **Wie viele zusätzliche Blockdatenträger werden nach Typ, Größe, IOPS und Position benötigt?** <br />
*Ihre Antwort könnte zum Beispiel ähnlich wie folgt lauten: "25x Endurance 2 TB @ 4 IOPS in DAL09" oder "25x Performance 4 TB @ 2 IOPS in WDC04".*

- **Wie viele zusätzliche Dateidatenträger werden nach Typ, Größe, IOPS und Position benötigt?** <br />
*Ihre Antwort könnte zum Beispiel ähnlich wie folgt lauten: "25x Performance 20 GB @ 10 IOPS in DAL09" oder "50x Endurance 2 TB @ 0,25 IOPS in SJC03".*

- **Geben Sie einen geschätzten Zeitpunkt an, zu dem Sie erwarten, dass alle angeforderten Datenträgervergrößerungen bereitgestellt oder geplant sind.** <br />
 *Ihre Antwort könnte zum Beispiel ähnlich wie folgt lauten: "90 Tage".*

- **Geben Sie eine 90-Tage-Vorhersage für die erwartete durchschnittliche Kapazitätsnutzung dieser Datenträger an.** <br />
*Ihre Antwort könnte zum Beispiel ähnlich wie folgt lauten: "Erwarte 25 Prozent belegt in 30 Tagen, 50 Prozent belegt in 60 Tagen und 75 Prozent belegt in 90 Tagen".*

Beantworten Sie alle Fragen und Aussagen in Ihrer Anfrage. ie sind für die Verarbeitung und Genehmigung erforderlich.
{:important}

Sie werden von der Aktualisierung Ihrer Begrenzungen durch den Ticket-Prozess benachrichtigt.
