---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, provisioning, ordering,

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# {{site.data.keyword.filestorage_short}} über die Konsole bestellen
{: #orderingConsole}

Sie können {{site.data.keyword.filestorage_short}} bereitstellen und entsprechend Ihrer Kapazität und Ihren IOPS-Anforderungen optimieren. Ihnen stehen zwei Optionen für die Angabe der Leistung zur Verfügung, um Ihren Speicher zu optimieren.

- Sie können aus Endurance-IOPS-Stufen auswählen, die vordefinierte Leistungsstufen für Workloads enthalten, die nicht über definierte Leistungsanforderungen verfügen.
- Sie können mit Performance die Gesamtzahl der E/A-Operationen pro Sekunde (IOPS) angeben, um Ihren Speicher zu optimieren und an spezielle Leistungsanforderungen anzupassen.

## {{site.data.keyword.filestorage_short}} mit vordefinierten IOPS-Stufen bestellen (Endurance)
{: #endurance}

1. Melden Sie sich beim [IBM Cloud-Katalog](https://{DomainName}/catalog){: external} an und klicken Sie auf **Speicher**. Wählen Sie anschließend {{site.data.keyword.filestorage_short}} aus. Klicken Sie auf **Erstellen**.

   Alternativ können Sie sich am [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} anmelden und auf **Speicher** > **{{site.data.keyword.filestorage_short}}** klicken. Klicken Sie rechts oben auf **{{site.data.keyword.filestorage_short}} bestellen**.
2. Wählen Sie Ihre Bereitstellungs**position** (Ihr Rechenzentrum) aus.
   - Stellen Sie sicher, dass der neue Speicher an derselben Position hinzugefügt wird, an der sich auch Ihre Rechenhosts befinden.
3. Abrechnung. Wenn Sie ein Rechenzentrum mit verbesserter Funktionalität (gekennzeichnet durch einen Stern) ausgewählt haben, können Sie zwischen monatlicher und stündlicher Abrechnung wählen.
     1. Bei der **stündlichen** Abrechnung wird die Stundenzahl, die der Dateidatenträger im Konto vorhanden war, zu dem Zeitpunkt berechnet, an dem die LUN gelöscht wird oder der Rechnungsstellungszyklus endet. Dies ist abhängig davon, welches Ereignis zuerst eintritt. Die Abrechnung nach Stunden ist eine gute Wahl für Speicher, der für einige wenige Tage oder weniger als einen ganzen Monat lang genutzt wird. Die Abrechnung nach Stunden ist nur für Speicher verfügbar, der in diesen [ausgewählten Rechenzentren](/docs/infrastructure/FileStorage?topic=FileStorage-news) bereitgestellt wird.
     2. Bei der **monatlichen** Abrechnung erfolgt die Berechnung für den Preis anteilmäßig ab dem Erstellungsdatum bis zum Ende des Rechnungsstellungszyklus und die Rechnung wird unverzüglich gestellt. Es erfolgt keine Rückerstattung, wenn ein Dateidatenträger vor dem Ende des Rechnungsstellungszyklus gelöscht wird. Die monatliche Rechnungsstellung ist eine gute Wahl für Speicher, der für Auslastungen im Produktionsbetrieb genutzt wird, die Daten verwenden, die über längere Zeiträume (einen Monat oder länger) gespeichert werden und zugänglich sein müssen.

     Der monatliche Abrechnungstyp wird standardmäßig für Speicher verwendet, der in Rechenzentren bereitgestellt wird, die **nicht** mit verbesserten Funktionen aktualisiert werden.
     {:note}
4. Geben Sie Ihre Speichergröße in das Feld **Neue Speichergröße** ein.
5. Wählen Sie im Bereich **IOPS-Optionen für Speicher** die Option **Endurance (abgestufte E/A-Operationen pro Sekunde)** aus.
6. Wählen Sie die IOPS-Stufe aus, die Ihre Anwendung benötigt.
    - **0,25 IOPS pro GB** sind für Workloads mit niedriger E/A-Intensität gedacht. Solche Workloads sind in der Regel durch einen hohen Prozentsatz an Daten gekennzeichnet, die zu jedem Zeitpunkt inaktiv sind. Beispiele für Anwendungen sind das Speichern von Mailboxen oder das Speichern von gemeinsam genutzten Dateien auf Abteilungsebene.
    - **2 IOPS pro GB** sind für die meisten Fälle allgemeiner Nutzung vorgesehen. Beispiele für Anwendungen sind das Hosting kleiner Datenbanken zur Unterstützung von Webanwendungen oder VM-Plattenimages für einen Hypervisor.
    - **4 IOPS pro GB** sind für Workloads höherer Intensität vorgesehen. Solche Workloads sind in der Regel durch einen hohen Prozentsatz an Daten gekennzeichnet, die zu jedem Zeitpunkt aktiv sind. Beispielanwendungen sind transaktionsorientierte und andere leistungskritische Datenbanken.
    - **10 IOPS pro GB** sind für anspruchsvollste Workloads vorgesehen, wie zum Beispiel für die von NoSQL-Datenbanken generierten Workloads und für die Datenverarbeitung von Analysen (Analytics). Diese Stufe ist in [ausgewählten Rechenzentren](/docs/infrastructure/FileStorage?topic=FileStorage-news) für Speicher verfügbar, der bis 4 TB bereitgestellt wird.
7. Klicken Sie auf **Größe des Snapshotbereichs angeben** und wählen Sie in der Liste die Snapshotgröße aus. Dieser Bereich wird zusätzlich zu Ihrem nutzbaren Bereich genutzt. Hinweise und Empfehlungen zum Snapshotbereich finden Sie im Abschnitt [Snapshots bestellen](/docs/infrastructure/FileStorage?topic=FileStorage-ordering-snapshots).
8. Überprüfen Sie auf der rechten Seite Ihre Bestellübersicht und wenden Sie gegebenenfalls Ihren Werbeaktionscode an.

   Rabatte werden bei der Verarbeitung der Bestellung angewendet.
   {:note}
9. Danach markieren Sie das Kontrollkästchen **Die im Folgenden aufgeführten Servicevereinbarungen anderer Anbieter habe ich gelesen und stimme ihnen zu:**.
10. Klicken Sie auf **Erstellen**. Ihre neue Speicherzuordnung steht nach wenigen Minuten zur Verfügung.

Standardmäßig können Sie insgesamt 250 {{site.data.keyword.blockstorageshort}}-Datenträger bereitstellen. Wenden Sie sich an Ihren Vertriebsbeauftragten, wenn Sie die Anzahl Ihrer Datenträger erhöhen möchten. Weitere Informationen zum Erhöhen von Grenzwerten finden Sie [hier](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits).<br/><br/>Weitere Informationen zum Grenzwert für gleichzeitige Autorisierungen finden Sie im Abschnitt [Häufig gestellte Fragen (FAQs)](/docs/infrastructure/FileStorage?topic=FileStorage-faqs#how-many-instances-can-share-the-use-of-a-provisioned-file-storage-volume-).
{:tip}

## {{site.data.keyword.filestorage_short}} mit angepassten IOPS-Raten bestellen (Performance)
{: #performance}

1. Melden Sie sich beim [IBM Cloud-Katalog](https://{DomainName}/catalog){: external} an und klicken Sie auf **Speicher**. Wählen Sie anschließend {{site.data.keyword.filestorage_short}} aus. Klicken Sie auf **Erstellen**.

   Alternativ können Sie sich am [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} anmelden und auf **Speicher** > **{{site.data.keyword.filestorage_short}}** klicken. Klicken Sie rechts oben auf **{{site.data.keyword.filestorage_short}} bestellen**.
2. Klicken Sie auf **Position** und wählen Sie Ihr Rechenzentrum aus.
   - Stellen Sie sicher, dass der neue Speicher an derselben Position hinzugefügt wird, an der sich auch Ihre Rechenhosts befinden.
3. Abrechnung. Wenn Sie ein Rechenzentrum mit verbesserter Funktionalität (gekennzeichnet durch einen Stern) ausgewählt haben, können Sie zwischen monatlicher und stündlicher Abrechnung wählen.
     1. Bei der **stündlichen** Abrechnung wird die Stundenzahl, die der Dateidatenträger im Konto vorhanden war, zu dem Zeitpunkt berechnet, an dem die LUN gelöscht wird oder der Rechnungsstellungszyklus endet. Dies ist abhängig davon, welches Ereignis zuerst eintritt. Die Abrechnung nach Stunden ist eine gute Wahl für Speicher, der für einige wenige Tage oder weniger als einen ganzen Monat lang genutzt wird. Die Abrechnung nach Stunden ist nur für Speicher verfügbar, der in diesen [ausgewählten Rechenzentren](/docs/infrastructure/FileStorage?topic=FileStorage-news) bereitgestellt wird.
     2. Bei der **monatlichen** Abrechnung erfolgt die Berechnung für den Preis anteilmäßig ab dem Erstellungsdatum bis zum Ende des Rechnungsstellungszyklus und die Rechnung wird unverzüglich gestellt. Es erfolgt keine Rückerstattung, wenn ein Dateidatenträger vor dem Ende des Rechnungsstellungszyklus gelöscht wird. Die monatliche Rechnungsstellung ist eine gute Wahl für Speicher, der für Auslastungen im Produktionsbetrieb genutzt wird, die Daten verwenden, die über längere Zeiträume (einen Monat oder länger) gespeichert werden und zugänglich sein müssen.

     Der monatliche Abrechnungstyp wird standardmäßig für Speicher verwendet, der in Rechenzentren bereitgestellt wird, die **nicht** mit verbesserten Funktionen aktualisiert werden.
     {:note}
4. Geben Sie Ihre Speichergröße in das Feld **Neue Speichergröße** ein.
5. Wählen Sie im Bereich **IOPS-Optionen für Speicher** die Option **Performance (zugeordnete E/A-Operationen pro Sekunde)** aus.
6. Geben Sie die E/A-Operationen pro Sekunde (IOPS) in das Feld **Performance (zugeordnete E/A-Operationen pro Sekunde)** ein.
7. Klicken Sie auf **Größe des Snapshotbereichs angeben** und wählen Sie in der Liste die Snapshotgröße aus. Dieser Bereich wird zusätzlich zu Ihrem nutzbaren Bereich genutzt. Hinweise und Empfehlungen zum Snapshotbereich finden Sie im Abschnitt [Snapshots bestellen](/docs/infrastructure/FileStorage?topic=FileStorage-ordering-snapshots).
8. Überprüfen Sie auf der rechten Seite Ihre Bestellübersicht und wenden Sie gegebenenfalls Ihren Werbeaktionscode an.

   Rabatte werden bei der Verarbeitung der Bestellung angewendet.
   {:note}
9. Danach markieren Sie das Kontrollkästchen **Die im Folgenden aufgeführten Servicevereinbarungen anderer Anbieter habe ich gelesen und stimme ihnen zu:**.
10. Klicken Sie auf **Erstellen**. Ihre neue Speicherzuordnung steht nach wenigen Minuten zur Verfügung.

Standardmäßig können Sie insgesamt 250 {{site.data.keyword.blockstorageshort}}-Datenträger bereitstellen. Wenden Sie sich an Ihren Vertriebsbeauftragten, wenn Sie die Anzahl Ihrer Datenträger erhöhen möchten. Weitere Informationen zum Erhöhen von Grenzwerten finden Sie [hier](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits).<br/><br/>Weitere Informationen zum Grenzwert für gleichzeitige Autorisierungen finden Sie im Abschnitt [Häufig gestellte Fragen (FAQs)](/docs/infrastructure/FileStorage?topic=FileStorage-faqs#how-many-instances-can-share-the-use-of-a-provisioned-file-storage-volume-).
{:important}


## Verbindung zum neuen Speicher herstellen
{: #mountingvolumesPortal}

Wenn Ihre Bereitstellungsanforderung abgeschlossen ist, können Sie Ihren Hosts die Berechtigung erteilen, auf den neuen Speicher zuzugreifen und die Verbindung zu konfigurieren. Folgen Sie je nach dem Betriebssystem Ihres Hosts dem entsprechenden Link.
- [{{site.data.keyword.filestorage_short}} unter Linux anhängen](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [{{site.data.keyword.filestorage_short}} unter CentOS anhängen](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [{{site.data.keyword.filestorage_short}} unter CoreOS anhängen](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)
- [{{site.data.keyword.filestorage_short}} für Sicherung mit cPanel konfigurieren](/docs/infrastructure/FileStorage?topic=FileStorage-cPanelBackups)
- [{{site.data.keyword.filestorage_short}} für Sicherung mit Plesk konfigurieren](/docs/infrastructure/FileStorage?topic=FileStorage-PleskBackup)
- [{{site.data.keyword.filestorage_short}}-Datenträger an ESXi-Hosts anhängen](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)

## Hinweise zur Disaster-Recovery

Um Datenverluste zu vermeiden und unterbrechungsfreie Geschäftsabläufe zu gewährleisten, sollten Sie in Betracht ziehen, Ihre Server und Speicher in einem anderen Rechenzentrum zu replizieren. Die Replikation hält Ihre Daten an zwei verschiedenen Positionen je nach Snapshot-Zeitplan synchron. Weitere Informationen hierzu finden Sie im Abschnitt [Daten replizieren](/docs/infrastructure/FileStorage?topic=FileStorage-replication).

Wenn Sie Ihren Datenträger klonen und unabhängig vom ursprünglichen Datenträger verwenden möchten, lesen Sie den Abschnitt [Duplikat des Blockdatenträgers erstellen](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume).

## {{site.data.keyword.filestorage_short}}-Datenträger auf der Rechnung angeben

Alle gemeinsam genutzten Dateispeicher werden auf Ihrer Rechnung als Artikelpositionen aufgeführt. Endurance-Datenträger werden als 'Endurance Storage Service' und Performance-Datenträger als 'Performance Storage Service' aufgeführt. Die Rate variiert je nach Speicherebene. Sie können Endurance oder Performance erweitern, um anzuzeigen, dass es sich um {{site.data.keyword.filestorage_short}} handelt.
