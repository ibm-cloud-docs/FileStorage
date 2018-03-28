---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-06"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.filestorage_full_notm}} bestellen und verwalten

Es gibt zwei unterschiedliche Typen von Speicher, die Sie je nach Anforderungen und Präferenzen bereitstellen können. Die beiden Optionen für {{site.data.keyword.filestorage_short}}-Datenträger sind folgende:

- **Endurance:** Sie können Endurance-Stufen bereitstellen, die über vordefinierte Leistungsstufen und Features wie Snapshots und Replikation verfügen.
- **Performance:** Sie können eine leistungsstarke Performance-Umgebung aufbauen, in der Sie die gewünschten E/A-Operationen pro Sekunde (IOPS) für Ein- und Ausgaben zuordnen können.

## {{site.data.keyword.filestorage_short}} bereitstellen

### Vorgehensweise zur Bestellung von Endurance for {{site.data.keyword.filestorage_short}}

1. Klicken Sie im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} auf **Speicher** > **{{site.data.keyword.filestorage_short}}** oder im Katalog von {{site.data.keyword.BluSoftlayer_full}} auf **Infrastruktur** > **Speicher** > **{{site.data.keyword.filestorage_short}}**.
2. Klicken Sie auf **{{site.data.keyword.filestorage_short}} bestellen** in der rechten oberen Ecke der Anzeige. 
3. Wählen Sie **Endurance** in der Dropdown-Liste **Speichertyp auswählen** aus.
4. Klicken Sie auf die Dropdown-Liste **Position** und wählen Sie Ihr Rechenzentrum aus.
   - Stellen Sie sicher, dass der neue Speicher an derselben Position wie der zuvor bestellte Host (bzw. die bestellten Hosts) hinzugefügt wird.
   - Wenn Sie ein Rechenzentrum mit verbesserter Funktionalität (durch einen Stern * in der Dropdown-Liste gekennzeichnet) ausgewählt haben, haben Sie die Option, zwischen monatlicher und stündlicher Rechnungsstellung zu wählen. 
     1. Bei der **stündlichen** Rechnungsstellung wird die Berechnung der Stundenzahl, die der Dateidatenträger auf dem Konto vorhanden war, zu dem Zeitpunkt berechnet, zu dem der Datenträger gelöscht wird oder zu dem der Rechnungsstellungszyklus endet, je nachdem, welcher Zeitpunkt zuerst kommt. Die stündliche Rechnungsstellung ist eine gute Wahl für Speicher, der für einige wenige Tage oder weniger als einen ganzen Monat lang genutzt wird. Eine stündliche Rechnungsstellung ist nur für Speicher verfügbar, der in diesen [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) bereitgestellt wurde. 
     2. Bei der **monatlichen** Rechnungsstellung erfolgt die Berechnung für den Preis anteilmäßig ab dem Erstellungsdatum bis zum Ende des Rechnungsstellungszyklus und die Rechnung wird unverzüglich gestellt. Es erfolgt keine Rückvergütung, wenn ein Datenträger vor dem Ende des Rechnungsstellungszyklus gelöscht wird. Die monatliche Rechnungsstellung ist eine gute Wahl für Speicher, der für Auslastungen im Produktionsbetrieb genutzt wird, die Daten verwenden, die über längere Zeiträume (einen Monat oder länger) gespeichert werden und zugänglich sein müssen.
     **Anmerkung:** Der monatliche Rechnungsstellungstyp wird standardmäßig für Speicher verwendet, der in Rechenzentren bereitgestellt wurde, die nicht mit der verbesserten Funktionalität aktualisiert wurden.
5. Wählen Sie das Speicherniveau für Ihre Anwendungen aus:
    - **0,25 IOPS pro GB** sind für Workloads mit niedriger E/A-Intensität gedacht. Solche Workloads sind in der Regel durch einen hohen Prozentsatz an Daten, die zu einem gegebenen Zeitpunkt inaktiv sind, gekennzeichnet. Beispiele für Anwendungen sind das Speichern von Mailboxen oder das Speichern von Dateifreigaben auf Abteilungsebene.
    - **2 IOPS pro GB** sind für die Nutzung zu allgemeinsten Zwecken vorgesehen. Beispiele für Anwendungen sind das Hosting kleiner Datenbanken zur Unterstützung von Webanwendungen oder virtueller Plattenimages für einen Hypervisor.
    - **4 IOPS pro GB** sind für Workloads höherer Intensität vorgesehen. Solche Workloads sind in der Regel durch einen hohen Prozentsatz an Daten, die zu einem gegebenen Zeitpunkt aktiv sind, gekennzeichnet. Beispielanwendungen sind transaktionsorientierte und andere leistungskritische Datenbanken.
    - **10 IOPS pro GB** sind für die anspruchsvollsten Workloads vorgesehen, wie zum Beispiel die von NoSQL-Datenbanken generierten Workloads und Workloads von Datenverarbeitungen für Analysen (Analytics). Diese Stufe ist in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) für Speicher verfügbar, der mit einer Größe von bis zu 4 TB bereitgestellt wird.
6. Wählen Sie die Option **Nutzbare Speichergröße** in der Dropdown-Liste aus.
7. Wählen Sie **Größe des Snapshotbereichs** in der Dropdown-Liste (zusätzlich zu Ihrem nutzbaren Bereich) aus.
8. Klicken Sie auf **Weiter**. Es werden die monatlichen und anteilmäßig eingerechneten Gebühren mit einer letzten Gelegenheit zur Prüfung der Bestelldetails angezeigt.
9. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Bestellung abschicken**.
10. Ihre neue Speicherzuordnung sollte nach wenigen Minuten zur Verfügung stehen.

**Anmerkung:** Sie können standardmäßig eine kombinierte Gesamtzahl von 250 {{site.data.keyword.blockstorageshort}}-Datenträgern bereitstellen. Zur Erhöhung der Anzahl Ihrer Datenträger wenden Sie sich an Ihren Vertriebsbeauftragten. Informationen zur Erhöhung von Begrenzungen finden Sie [hier](managing-storage-limits.html).

Informationen zur Begrenzung gleichzeitiger Berechtigungen finden Sie in den [häufig gestellten Fragen (FAQs)](File-Storage-FAQ.html).

### Vorgehensweise zur Bestellung von Performance for {{site.data.keyword.filestorage_short}}

1. Klicken Sie im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} auf **Speicher**, **{{site.data.keyword.filestorage_short}}** oder im Katalog von {{site.data.keyword.BluSoftlayer_full}} auf **Infrastruktur** >** Speicher** > **{{site.data.keyword.filestorage_short}}**.
2. Klicken Sie auf **{{site.data.keyword.filestorage_short}} bestellen** in der rechten oberen Ecke der Anzeige. 
3. Wählen Sie **Performance** in der Dropdown-Liste 'Speichertyp auswählen' aus.
4. Klicken Sie auf die Dropdown-Liste **Position** und wählen Sie Ihr Rechenzentrum aus.
    -  Stellen Sie sicher, dass der neue Speicher an derselben Position wie der zuvor bestellte Host (bzw. die bestellten Hosts) hinzugefügt wird.
    -  Wenn Sie ein Rechenzentrum mit verbesserter Funktionalität (durch einen Stern * in der Dropdown-Liste gekennzeichnet) ausgewählt haben, haben Sie die Option, zwischen monatlicher und stündlicher Rechnungsstellung zu wählen. 
       1.  Bei der **stündlichen** Rechnungsstellung wird die Berechnung der Stundenzahl, die der Dateidatenträger auf dem Konto vorhanden war, zu dem Zeitpunkt berechnet, zu dem der Datenträger gelöscht wird oder zu dem der Rechnungsstellungszyklus endet, je nachdem, welcher Zeitpunkt zuerst kommt. Die stündliche Rechnungsstellung ist eine gute Wahl für Speicher, der für einige wenige Tage oder weniger als einen ganzen Monat lang genutzt wird. Eine stündliche Rechnungsstellung ist nur für Speicher verfügbar, der in ausgewählten Rechenzentren bereitgestellt wurde. 
       2. Bei der **monatlichen** Rechnungsstellung erfolgt die Berechnung für den Preis anteilmäßig ab dem Erstellungsdatum bis zum Ende des Rechnungsstellungszyklus und die Rechnung wird unverzüglich gestellt. Es erfolgt keine Rückvergütung, wenn ein Datenträger vor dem Ende des Rechnungsstellungszyklus gelöscht wird. Die monatliche Rechnungsstellung ist eine gute Wahl für Speicher, der für Auslastungen im Produktionsbetrieb genutzt wird, die Daten verwenden, die über längere Zeiträume (einen Monat oder länger) gespeichert werden und zugänglich sein müssen.
       **Anmerkung:** Der monatliche Rechnungsstellungstyp wird standardmäßig für Speicher verwendet, der in Rechenzentren bereitgestellt wurde, die nicht mit der verbesserten Funktionalität aktualisiert wurden.  
5. Wählen Sie das Optionsfeld neben der entsprechenden **Speichergröße** aus.
6. Geben Sie die E/A-Operationen pro Sekunde (IOPS) in das Feld **IOPS angeben** ein.
7. Klicken Sie auf **Weiter**. Es werden die monatlichen und anteilmäßig eingerechneten Gebühren mit einer letzten Gelegenheit zur Prüfung der Bestelldetails angezeigt. Klicken Sie auf **Zurück**, wenn Sie Ihre Bestellung ändern möchten.
8. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Bestellung abschicken**.
9. Ihre neue Speicherzuordnung sollte nach wenigen Minuten zur Verfügung stehen.

**Anmerkung:** Sie können standardmäßig eine kombinierte Gesamtzahl von 250 {{site.data.keyword.blockstorageshort}}-Datenträgern bereitstellen. Zur Erhöhung der Anzahl Ihrer Datenträger wenden Sie sich an Ihren Vertriebsbeauftragten. Informationen zur Erhöhung von Begrenzungen finden Sie [hier](managing-storage-limits.html).

Informationen zur Begrenzung gleichzeitiger Berechtigungen finden Sie in den [häufig gestellten Fragen (FAQs)](File-Storage-FAQ.html).

## {{site.data.keyword.filestorage_short}} verwalten

### Vorgehensweise zur Berechtigung von Hosts für den Zugriff auf {{site.data.keyword.filestorage_short}}

Berechtigte (“autorisierte”) Hosts sind Hosts, denen Zugriffsrechte für einen bestimmten Datenträger erteilt wurden. Ohne die Hostberechtigung können Sie über Ihr System nicht auf den Speicher zugreifen und ihn nicht verwenden. Durch das Berechtigen eines Hosts für den Zugriff auf Ihren Datenträger werden der Benutzername und das Kennwort generiert. 

**Anmerkung:** Sie können nur Hosts berechtigen und verbinden, die sich im selben Rechenzentrum wie Ihr Speicher befinden. Wenn Sie über mehrere Konten verfügen, können Sie einen Host nicht über das Konto für den Zugriff auf Ihren Speicher auf einem anderen Konto berechtigen. 

1. Klicken Sie auf **Speicher** > **{{site.data.keyword.filestorage_short}}** und klicken Sie auf den Namen Ihres Datenträgers im Feld **Datenträgername**.
2. Blättern Sie zum Abschnitt **Autorisierte Hosts** auf der Seite.
3. Klicken Sie auf den Link **Host autorisieren** auf der rechten Seite der Anzeige. Wählen Sie die Hosts aus, die auf diesen bestimmten Datenträger zugreifen können.

 

### Vorgehensweise zum Anzeigen der Liste von Hosts mit Berechtigung für einen {{site.data.keyword.filestorage_short}}-Datenträger

Führen Sie die folgenden Schritte aus, um die Liste der Hosts anzuzeigen, die für einen Datenträger zugriffsberechtigt sind.

1. Klicken Sie auf **Speicher > {{site.data.keyword.filestorage_short}}** und klicken Sie auf den Namen Ihres Datenträgers im Feld **Datenträgername**.
2. Blättern Sie zum Abschnitt **Autorisierte Hosts** am Ende der Seite.

Hier finden Sie die Liste der Hosts, die zurzeit dazu berechtigt sind, auf den Datenträger zuzugreifen.


### Vorgehensweise zum Anzeigen der {{site.data.keyword.filestorage_short}}-Datenträger, für die ein Host zugriffsberechtigt ist

Sie können die Datenträger, auf die ein Host Zugriff hat, sowie Informationen, die zum Herstellen einer Verbindung erforderlich sind, anzeigen – Datenträgername, Speichertyp, Zieladresse, Kapazität und Position:

1. Klicken Sie auf **Geräte** > **Geräteliste** im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} und klicken Sie auf das gewünschte Gerät.
2. Wählen Sie die Registerkarte 'Speicher' aus.

Es wird eine Liste von Speicherdatenträgern angezeigt, auf die dieser bestimmte Host Zugriff hat. Die Datenträger werden nach Speichertyp (Block, Datei, andere) gruppiert. Über die jeweiligen Menüs **Aktion** können Sie zusätzlichen Speicher autorisieren oder Zugriffsberechtigungen entfernen.

 

### Vorgehensweise zum An- und Abhängen von {{site.data.keyword.filestorage_short}}-Speicher

Sie können die in der Ansicht **Datenträgerdetails** bereitgestellten Mountpunktinformationen verwenden, um {{site.data.keyword.filestorage_short}} von einem Host aus anzuhängen.

In dem folgenden Artikel finden Sie Details zum Anhängen und Abhängen von {{site.data.keyword.filestorage_short}}-Speicher über einen Host: [Auf {{site.data.keyword.filestorage_short}} unter Linux zugreifen](accessing-file-storage-linux.html).

 

### Vorgehensweise zum Widerrufen des Hostzugriffs auf {{site.data.keyword.filestorage_short}}

Wenn Sie den Zugriff von einem Host auf einen bestimmten Speicherdatenträger beenden wollen, können Sie den Zugriff widerrufen. Nach dem Widerruf des Zugriffs wird die Hostverbindung von dem Datenträger getrennt und weder das Betriebssystem noch Anwendungen können mit dem Datenträger kommunizieren. 

**Anmerkung:** Zur Vermeidung unerwünschter Nebeneffekte für den Host hängen Sie den Speicherdatenträger von Ihrem Betriebssystem ab, bevor Sie den Zugriff widerrufen, sodass keine Laufwerke vermisst oder Daten beschädigt werden.

Sie können den Zugriff über 'Speicher' in der Ansicht 'Geräteliste' oder in der Ansicht 'Speicher' widerrufen.

#### Vorgehensweise zum Widerrufen des Zugriffs über die Geräteliste:

1. Klicken Sie auf **Geräte** > **Geräteliste** im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} und klicken Sie doppelt auf das betreffende Gerät.
2. Wählen Sie die Registerkarte **Speicher** aus.
3. Es wird eine Liste von Speicherdatenträgern angezeigt, auf die dieser bestimmte Host Zugriff hat. Die Datenträger werden nach Speichertyp (Block, Datei, andere) gruppiert. Wählen Sie das jeweilige Menü **Aktion** neben dem Datenträger aus, für den Sie den Zugriff widerrufen möchten, und klicken Sie auf **Zugriff widerrufen**.
4. Sie werden aufgefordert, den Zugriffswiderruf für einen Datenträger zu bestätigen, da die Aktion nicht rückgängig gemacht werden kann. Klicken Sie auf **Ja**, um den Datenträgerzugriff zu widerrufen, oder auf **Nein**, um die Aktion abzubrechen.

**Anmerkung:** Wenn Sie die Verbindungen mehrerer Datenträger von einem bestimmten Host trennen wollen, müssen Sie die Aktion 'Zugriff widerrufen' für jeden Datenträger wiederholen.

 

#### Vorgehensweise zum Widerrufen des Zugriffs über die Ansicht 'Speicher':
1. Klicken Sie auf **Speicher > {{site.data.keyword.filestorage_short}}** und wählen Sie den **Datenträger** aus, für den Sie den Zugriff widerrufen wollen.
2. Blättern Sie hinunter zum Abschnitt **Autorisierte Hosts** auf der Seite.
3. Klicken Sie auf den Dropdown-Pfeil für **Aktionen** neben dem Host, dessen Zugriff widerrufen werden soll, und wählen Sie **Zugriff widerrufen** aus.
4. Sie werden aufgefordert, den Zugriffswiderruf für einen Datenträger zu bestätigen, da die Aktion nicht rückgängig gemacht werden kann. Klicken Sie auf **Ja**, um den Datenträgerzugriff zu widerrufen, oder auf **Nein**, um die Aktion abzubrechen.

**Anmerkung:** Wenn Sie die Verbindungen mehrerer Hosts von einem bestimmten Datenträger trennen wollen, müssen Sie die Aktion 'Zugriff widerrufen' für jeden Host wiederholen.

 

### Vorgehensweise zum Stornieren eines Speicherdatenträgers

Wenn Sie einen bestimmten Datenträger nicht mehr benötigen, können Sie ihn stornieren. Zur Stornierung eines Speicherdatenträgers müssen Sie zunächst den Zugriff für alle Hosts widerrufen.

1. Klicken Sie auf **Speicher**>**{{site.data.keyword.filestorage_short}}**.
2. Klicken Sie auf den Dropdown-Pfeil für **Aktionen** für den zu stornierenden Datenträger und wählen Sie **{{site.data.keyword.filestorage_short}} stornieren** aus.
3. Sie werden zu der Angabe aufgefordert, ob der Datenträger sofort oder am Rechnungsstichtag für den Bereitstellungszeitpunkt des Datenträgers storniert werden soll. Klicken Sie auf **Weiter** oder **Schließen**.
**Anmerkung:** Wenn Sie die Option zum Stornieren des Datenträgers am Rechnungsstichtag auswählen, können Sie die Stornierungsanforderung vor ihrem Stichtag nichtig machen.
4. Klicken Sie auf das Kontrollkästchen zur Bestätigung und klicken Sie auf **Bestätigen**.

 

### Vorgehensweise zum Anzeigen der Details eines bereitgestellten {{site.data.keyword.filestorage_short}}-Datenträgers

Sie können eine Zusammenfassung der wichtigen Informationen für den ausgewählten Speicherdatenträger, einschließlich der Snapshot- und Replikationsfunktionen anzeigen, die dem Speicher hinzugefügt wurden.

1. Klicken Sie auf **Speicher**>**{{site.data.keyword.filestorage_short}}**.
2. Klicken Sie auf den gewünschten **Datenträgernamen** in der Liste.

### Vorgehensweise zum Ermitteln der {{site.data.keyword.filestorage_short}}-Datenträger auf der Rechnung

Alle Datenträger werden auf Ihrer Rechnung als Artikelposition aufgeführt: Endurance wird als “Endurance Storage Service” und Performance wird als "Performance Storage Service" aufgeführt. Die Rate variiert je nach Speicherstufe. Sie können Endurance oder Performance erweitern, um anzuzeigen, dass es sich um einen {{site.data.keyword.filestorage_short}}-Datenträger handelt.
