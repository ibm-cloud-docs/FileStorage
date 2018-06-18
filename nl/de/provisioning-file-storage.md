---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.filestorage_short}} bestellen 

Es gibt zwei verschiedene Möglichkeiten, wie Sie {{site.data.keyword.filestorage_full}} auf der Basis Ihrer Anforderungen und Präferenzen bereitstellen können. Dies sind die beiden Optionen:

- **Endurance:** Sie können Endurance-Stufen bereitstellen, die über vordefinierte Leistungsstufen und Features wie Snapshots und Replikation verfügen.
- **Performance:** Sie können eine leistungsstarke Performance-Umgebung aufbauen, in der Sie die gewünschten E/A-Operationen pro Sekunde (IOPS) für Ein- und Ausgaben zuordnen können.

## Vorgehensweise zur Bestellung von Endurance for {{site.data.keyword.filestorage_short}}

1. Klicken Sie im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} auf **Speicher** > **{{site.data.keyword.filestorage_short}}** oder im Katalog von {{site.data.keyword.BluSoftlayer_full}} auf **Infrastruktur** > **Speicher** > **{{site.data.keyword.filestorage_short}}**.
2. Klicken Sie in der rechten oberen Ecke auf **{{site.data.keyword.filestorage_short}} bestellen**. 
3. Wählen Sie **Endurance** in der Liste **Speichertyp auswählen** aus.
4. Klicken Sie auf **Position** und wählen Sie Ihr Rechenzentrum aus.
   - Stellen Sie sicher, dass der neue Speicher an derselben Position wie der zuvor bestellte Host hinzugefügt wird.
   - Wenn Sie ein Rechenzentrum mit verbesserter Funktionalität (gekennzeichnet durch einen Stern) ausgewählt haben, können Sie zwischen monatlicher und stündlicher Abrechnung wählen. 1. Bei
der **stündlichen** Abrechnung wird die Anzahl der Stunden, in denen der Dateidatenträger auf dem Konto vorhanden war, zum Zeitpunkt der Löschung des Datenträgers oder zum Zeitpunkt der Beendigung des Rechnungsstellungszyklus berechnet, je nachdem, welcher Zeitpunkt zuerst kommt. Die Abrechnung nach Stunden ist eine gute Wahl für Speicher, der für einige wenige Tage oder weniger als einen ganzen Monat lang genutzt wird. Die Abrechnung nach Stunden ist nur für Speicher verfügbar, der in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) bereitgestellt wurde. 
     2. Bei der **monatlichen** Abrechnung erfolgt die Berechnung für den Preis anteilmäßig ab dem Erstellungsdatum bis zum Ende des Rechnungsstellungszyklus und die Rechnung wird unverzüglich gestellt. Es erfolgt keine Rückerstattung, wenn ein Datenträger vor dem Ende des Rechnungsstellungszyklus gelöscht wird. Die monatliche Abrechnung ist eine gute Wahl für Speicher, der für Auslastungen im Produktionsbetrieb genutzt wird, die Daten verwenden, die über längere Zeiträume (einen Monat oder länger) gespeichert werden und zugänglich sein müssen.
     **Anmerkung**: Der Abrechnungstyp 'Monatlich' wird standardmäßig für Speicher verwendet, der in Rechenzentren bereitgestellt wird, die nicht mit der verbesserten Funktionalität aktualisiert wurden.
5. Wählen Sie das Speicherniveau für Ihre Anwendungen aus:
    - **0,25 IOPS pro GB** sind für Workloads mit niedriger E/A-Intensität gedacht. Solche Workloads sind in der Regel durch einen hohen Prozentsatz an Daten, die zu einem gegebenen Zeitpunkt inaktiv sind, gekennzeichnet. Beispiele für Anwendungen sind das Speichern von Mailboxen oder das Speichern von Dateifreigaben auf Abteilungsebene.
    - **2 IOPS pro GB** sind für die meisten Fälle allgemeiner Nutzung vorgesehen. Beispiele für Anwendungen sind das Hosting kleiner Datenbanken zur Unterstützung von Webanwendungen oder virtueller Plattenimages für einen Hypervisor.
    - **4 IOPS pro GB** sind für Workloads höherer Intensität vorgesehen. Solche Workloads sind in der Regel durch einen hohen Prozentsatz an Daten, die zu einem gegebenen Zeitpunkt aktiv sind, gekennzeichnet. Beispielanwendungen sind transaktionsorientierte und andere leistungskritische Datenbanken.
    - **10 IOPS pro GB** sind für anspruchvollste Workloads vorgesehen, wie zum Beispiel für die von NoSQL-Datenbanken generierten Workloads und für die Datenverarbeitung von Analysen (Analytics). Diese Stufe ist in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) für Speicher verfügbar, der bis zu einer Größe von 4 TB bereitgestellt wird.
6. Wählen Sie die Option **Nutzbare Speichergröße** in der Liste aus.
7. Wählen Sie (zusätzlich zu Ihrem nutzbaren Bereich) die **Größe des Snapshotbereichs** in der Liste aus.
8. Klicken Sie auf **Weiter**. Es werden die monatlichen und anteilmäßig eingerechneten Gebühren mit einer letzten Gelegenheit zur Prüfung der Bestelldetails angezeigt.
9. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Bestellung abschicken**.
10. Ihre neue Speicherzuordnung sollte nach wenigen Minuten zur Verfügung stehen.

**Anmerkung:** Sie können standardmäßig eine kombinierte Gesamtzahl von 250 {{site.data.keyword.filestorage_short}}-Datenträgern bereitstellen. Wenden Sie sich an Ihren Vertriebsbeauftragten, um die Anzahl Ihrer Datenträger zu erhöhen. Informationen zur Erhöhung von Begrenzungen finden Sie [hier](managing-storage-limits.html).

Informationen zum Grenzwert für gleichzeitige Berechtigungen finden Sie in den [häufig gestellten Fragen (FAQs)](File-Storage-FAQ.html).

## Vorgehensweise zur Bestellung von Performance for {{site.data.keyword.filestorage_short}}

1. Klicken Sie im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} auf **Speicher**, **{{site.data.keyword.filestorage_short}}** oder im Katalog von {{site.data.keyword.BluSoftlayer_full}} auf **Infrastruktur** >** Speicher** > **{{site.data.keyword.filestorage_short}}**.
2. Klicken Sie in der rechten oberen Ecke auf **{{site.data.keyword.filestorage_short}} bestellen**. 
3. Wählen Sie **Performance** in der Liste 'Speichertyp auswählen' aus.
4. Klicken Sie auf die Liste **Position** und wählen Sie Ihr Rechenzentrum aus.
    - Stellen Sie sicher, dass der neue Speicher an derselben Position wie der zuvor bestellte Host hinzugefügt wird.
    - Wenn Sie ein Rechenzentrum mit verbesserter Funktionalität (gekennzeichnet durch einen Stern) ausgewählt haben, können Sie zwischen monatlicher und stündlicher Abrechnung wählen.  
       1. Bei der **stündlichen** Abrechnung wird die Stundenzahl, die der Dateidatenträger auf dem Konto vorhanden war, zu dem Zeitpunkt berechnet, an dem der Datenträger gelöscht wird oder der Rechnungsstellungszyklus endet, je nachdem, welcher Zeitpunkt zuerst kommt. Die Abrechnung nach Stunden ist eine gute Wahl für Speicher, der für einige wenige Tage oder weniger als einen ganzen Monat lang genutzt wird. Die Abrechnung nach Stunden ist nur für Speicher verfügbar, der in ausgewählten Rechenzentren bereitgestellt wurde. 2. Bei der **monatlichen** Abrechnung erfolgt die Berechnung für den Preis anteilmäßig ab dem Erstellungsdatum bis zum Ende des Rechnungsstellungszyklus und die Rechnung wird unverzüglich gestellt. Es erfolgt keine Rückerstattung, wenn ein Datenträger vor dem Ende des Rechnungsstellungszyklus gelöscht wird. Die monatliche Abrechnung ist eine gute Wahl für Speicher, der für Auslastungen im Produktionsbetrieb genutzt wird, die Daten verwenden, die über längere Zeiträume (einen Monat oder länger) gespeichert werden und zugänglich sein müssen.
       **Anmerkung**: Der Abrechnungstyp 'Monatlich' wird standardmäßig für Speicher verwendet, der in Rechenzentren bereitgestellt wird, die nicht mit der verbesserten Funktionalität aktualisiert wurden.  
5. Wählen Sie die entsprechende **Speichergröße** aus.
6. Geben Sie die E/A-Operationen pro Sekunde (IOPS) in das Feld **IOPS angeben** ein.
7. Klicken Sie auf **Weiter**. Es werden die monatlichen und anteilmäßig eingerechneten Gebühren mit einer letzten Gelegenheit zur Prüfung der Bestelldetails angezeigt. Klicken Sie auf **Zurück**, wenn Sie Ihre Bestellung ändern möchten.
8. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Bestellung abschicken**.
9. Ihre neue Speicherzuordnung sollte nach wenigen Minuten zur Verfügung stehen.

**Anmerkung:** Sie können standardmäßig eine kombinierte Gesamtzahl von 250 {{site.data.keyword.filestorage_short}}-Datenträgern bereitstellen. Wenden Sie sich an Ihren Vertriebsbeauftragten, um die Anzahl Ihrer Datenträger zu erhöhen. Informationen zur Erhöhung von Begrenzungen finden Sie [hier](managing-storage-limits.html).

Informationen zum Grenzwert für gleichzeitige Berechtigungen finden Sie in den [häufig gestellten Fragen (FAQs)](File-Storage-FAQ.html).

## Vorgehensweise zum Ermitteln der {{site.data.keyword.filestorage_short}}-Datenträger auf der Rechnung

Alle Datenträger werden auf Ihrer Rechnung als Artikelpositionen aufgeführt. Endurance wird als 'Endurance Storage Service' und Performance wird als 'Performance Storage service' aufgeführt. Die Rate variiert je nach Speicherstufe. Sie können Endurance oder Performance erweitern, um anzuzeigen, dass es sich um einen {{site.data.keyword.filestorage_short}}-Datenträger handelt.

