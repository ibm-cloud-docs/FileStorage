---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.filestorage_short}} bestellen 

Es gibt zwei unterschiedliche Typen von {{site.data.keyword.filestorage_full}}, die Sie je nach Anforderungen und Präferenzen bereitstellen können. Die beiden Optionen für Datenträger sind folgende:

- **Endurance:** Sie können Endurance-Stufen bereitstellen, die über vordefinierte Leistungsstufen und Features wie Snapshots und Replikation verfügen.
- **Performance:** Sie können eine leistungsstarke Performance-Umgebung aufbauen, in der Sie die gewünschten E/A-Operationen pro Sekunde (IOPS) für Ein- und Ausgaben zuordnen können.

## Vorgehensweise zur Bestellung von Endurance for {{site.data.keyword.filestorage_short}}

1. Klicken Sie im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} auf **Speicher** > **{{site.data.keyword.filestorage_short}}** oder im Katalog von {{site.data.keyword.BluSoftlayer_full}} auf **Infrastruktur** > **Speicher** > **{{site.data.keyword.filestorage_short}}**.
2. Klicken Sie auf **{{site.data.keyword.filestorage_short}} bestellen** in der rechten oberen Ecke der Anzeige. 
3. Wählen Sie **Endurance** in der Dropdown-Liste **Speichertyp auswählen** aus.
4. Klicken Sie auf die Dropdown-Liste **Position** und wählen Sie Ihr Rechenzentrum aus.
   - Stellen Sie sicher, dass der neue Speicher an derselben Position wie der zuvor bestellte Host (bzw. die bestellten Hosts) hinzugefügt wird.
   - Wenn Sie ein Rechenzentrum mit verbesserter Funktionalität (durch einen Stern * in der Dropdown-Liste gekennzeichnet) ausgewählt haben, haben Sie die Option, zwischen monatlicher und stündlicher Rechnungsstellung zu wählen. 
     1. Bei der **stündlichen** Rechnungsstellung wird die Berechnung der Stundenzahl, die der Dateidatenträger auf dem Konto vorhanden war, zu dem Zeitpunkt berechnet, zu dem der Datenträger gelöscht wird oder zu dem der Rechnungsstellungszyklus endet, je nachdem, welcher Zeitpunkt zuerst kommt.  Die stündliche Rechnungsstellung ist eine gute Wahl für Speicher, der für einige wenige Tage oder weniger als einen ganzen Monat lang genutzt wird. Eine stündliche Rechnungsstellung ist nur für Speicher verfügbar, der in diesen [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) bereitgestellt wurde. 
     2. Bei der **monatlichen** Rechnungsstellung erfolgt die Berechnung für den Preis anteilmäßig ab dem Erstellungsdatum bis zum Ende des Rechnungsstellungszyklus und die Rechnung wird unverzüglich gestellt. Es erfolgt keine Rückvergütung, wenn ein Datenträger vor dem Ende des Rechnungsstellungszyklus gelöscht wird.  Die monatliche Rechnungsstellung ist eine gute Wahl für Speicher, der für Auslastungen im Produktionsbetrieb genutzt wird, die Daten verwenden, die über längere Zeiträume (einen Monat oder länger) gespeichert werden und zugänglich sein müssen.
     **Anmerkung:** Der monatliche Rechnungsstellungstyp wird standardmäßig für Speicher verwendet, der in Rechenzentren bereitgestellt wurde, die nicht mit der verbesserten Funktionalität aktualisiert wurden.
5. Wählen Sie das Speicherniveau für Ihre Anwendungen aus:
    - **0,25 IOPS pro GB** sind für Workloads mit niedriger E/A-Intensität gedacht. Solche Workloads sind in der Regel durch einen hohen Prozentsatz an Daten, die zu einem gegebenen Zeitpunkt inaktiv sind, gekennzeichnet. Beispiele für Anwendungen sind das Speichern von Mailboxen oder das Speichern von Dateifreigaben auf Abteilungsebene.
    - **2 IOPS pro GB** sind für die Nutzung zu allgemeinsten Zwecken vorgesehen. Beispiele für Anwendungen sind das Hosting kleiner Datenbanken zur Unterstützung von Webanwendungen oder virtueller Plattenimages für einen Hypervisor.
    - **4 IOPS pro GB** sind für Workloads höherer Intensität vorgesehen. Solche Workloads sind in der Regel durch einen hohen Prozentsatz an Daten, die zu einem gegebenen Zeitpunkt aktiv sind, gekennzeichnet. Beispielanwendungen sind transaktionsorientierte und andere leistungskritische Datenbanken.
    - **10 IOPS pro GB** sind für die anspruchsvollsten Workloads vorgesehen, wie zum Beispiel die von NoSQL-Datenbanken generierten Workloads und Workloads von Datenverarbeitungen für Analysen (Analytics).  Diese Stufe ist in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) für Speicher verfügbar, der mit einer Größe von bis zu 4 TB bereitgestellt wird.
6. Wählen Sie die Option **Nutzbare Speichergröße** in der Dropdown-Liste aus.
7. Wählen Sie **Größe des Snapshotbereichs** in der Dropdown-Liste (zusätzlich zu Ihrem nutzbaren Bereich) aus.
8. Klicken Sie auf **Weiter**. Es werden die monatlichen und anteilmäßig eingerechneten Gebühren mit einer letzten Gelegenheit zur Prüfung der Bestelldetails angezeigt.
9. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Bestellung abschicken**.
10. Ihre neue Speicherzuordnung sollte nach wenigen Minuten zur Verfügung stehen.

**Anmerkung:** Sie können standardmäßig eine kombinierte Gesamtzahl von 250 {{site.data.keyword.filestorage_short}}-Datenträgern bereitstellen. Zur Erhöhung der Anzahl Ihrer Datenträger wenden Sie sich an Ihren Vertriebsbeauftragten. Informationen zur Erhöhung von Begrenzungen finden Sie [hier](managing-storage-limits.html).

Informationen zur Begrenzung gleichzeitiger Berechtigungen finden Sie in den [häufig gestellten Fragen (FAQs)](File-Storage-FAQ.html).

## Vorgehensweise zur Bestellung von Performance for {{site.data.keyword.filestorage_short}}

1. Klicken Sie im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} auf **Speicher**, **{{site.data.keyword.filestorage_short}}** oder im Katalog von {{site.data.keyword.BluSoftlayer_full}} auf **Infrastruktur** >** Speicher** > **{{site.data.keyword.filestorage_short}}**.
2. Klicken Sie auf **{{site.data.keyword.filestorage_short}} bestellen** in der rechten oberen Ecke der Anzeige. 
3. Wählen Sie **Performance** in der Dropdown-Liste 'Speichertyp auswählen' aus.
4. Klicken Sie auf die Dropdown-Liste **Position** und wählen Sie Ihr Rechenzentrum aus.
    -  Stellen Sie sicher, dass der neue Speicher an derselben Position wie der zuvor bestellte Host (bzw. die bestellten Hosts) hinzugefügt wird.
    -  Wenn Sie ein Rechenzentrum mit verbesserter Funktionalität (durch einen Stern * in der Dropdown-Liste gekennzeichnet) ausgewählt haben, haben Sie die Option, zwischen monatlicher und stündlicher Rechnungsstellung zu wählen. 
       1.  Bei der **stündlichen** Rechnungsstellung wird die Berechnung der Stundenzahl, die der Dateidatenträger auf dem Konto vorhanden war, zu dem Zeitpunkt berechnet, zu dem der Datenträger gelöscht wird oder zu dem der Rechnungsstellungszyklus endet, je nachdem, welcher Zeitpunkt zuerst kommt.  Die stündliche Rechnungsstellung ist eine gute Wahl für Speicher, der für einige wenige Tage oder weniger als einen ganzen Monat lang genutzt wird. Eine stündliche Rechnungsstellung ist nur für Speicher verfügbar, der in ausgewählten Rechenzentren bereitgestellt wurde. 
       2. Bei der **monatlichen** Rechnungsstellung erfolgt die Berechnung für den Preis anteilmäßig ab dem Erstellungsdatum bis zum Ende des Rechnungsstellungszyklus und die Rechnung wird unverzüglich gestellt. Es erfolgt keine Rückvergütung, wenn ein Datenträger vor dem Ende des Rechnungsstellungszyklus gelöscht wird.  Die monatliche Rechnungsstellung ist eine gute Wahl für Speicher, der für Auslastungen im Produktionsbetrieb genutzt wird, die Daten verwenden, die über längere Zeiträume (einen Monat oder länger) gespeichert werden und zugänglich sein müssen.
       **Anmerkung:** Der monatliche Rechnungsstellungstyp wird standardmäßig für Speicher verwendet, der in Rechenzentren bereitgestellt wurde, die nicht mit der verbesserten Funktionalität aktualisiert wurden.  
5. Wählen Sie das Optionsfeld neben der entsprechenden **Speichergröße** aus.
6. Geben Sie die E/A-Operationen pro Sekunde (IOPS) in das Feld **IOPS angeben** ein.
7. Klicken Sie auf **Weiter**. Es werden die monatlichen und anteilmäßig eingerechneten Gebühren mit einer letzten Gelegenheit zur Prüfung der Bestelldetails angezeigt. Klicken Sie auf **Zurück**, wenn Sie Ihre Bestellung ändern möchten.
8. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Bestellung abschicken**.
9. Ihre neue Speicherzuordnung sollte nach wenigen Minuten zur Verfügung stehen.

**Anmerkung:** Sie können standardmäßig eine kombinierte Gesamtzahl von 250 {{site.data.keyword.filestorage_short}}-Datenträgern bereitstellen. Zur Erhöhung der Anzahl Ihrer Datenträger wenden Sie sich an Ihren Vertriebsbeauftragten. Informationen zur Erhöhung von Begrenzungen finden Sie [hier](managing-storage-limits.html).

Informationen zur Begrenzung gleichzeitiger Berechtigungen finden Sie in den [häufig gestellten Fragen (FAQs)](File-Storage-FAQ.html).

## Vorgehensweise zum Ermitteln der {{site.data.keyword.filestorage_short}}-Datenträger auf der Rechnung

Alle Datenträger werden auf Ihrer Rechnung als Artikelposition aufgeführt: Endurance wird als “Endurance Storage Service” und Performance wird als "Performance Storage Service" aufgeführt. Die Rate variiert je nach Speicherstufe. Sie können Endurance oder Performance erweitern, um anzuzeigen, dass es sich um einen {{site.data.keyword.filestorage_short}}-Datenträger handelt.

