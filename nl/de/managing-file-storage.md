---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.filestorage_short}} verwalten

Sie können Ihre {{site.data.keyword.filestorage_full}}-Datenträger über {{site.data.keyword.slportal}} verwalten. Dieser Artikel enthält Anweisungen für die gängigsten Tasks.

## Vorgehensweise zur Berechtigung von Hosts für den Zugriff auf {{site.data.keyword.filestorage_short}}

Berechtigte (“autorisierte”) Hosts sind Hosts, denen Zugriffsrechte für einen bestimmten Datenträger erteilt wurden. Ohne die Hostberechtigung können Sie über Ihr System nicht auf den Speicher zugreifen und ihn nicht verwenden. Durch das Berechtigen eines Hosts für den Zugriff auf Ihren Datenträger werden der Benutzername und das Kennwort generiert. 

**Anmerkung:** Sie können nur Hosts berechtigen und verbinden, die sich im selben Rechenzentrum wie Ihr Speicher befinden. Wenn Sie über mehrere Konten verfügen, können Sie einen Host nicht mithilfe des einen Kontos für den Zugriff auf Speicher in einem anderen Konto berechtigen. 

1. Klicken Sie auf **Speicher** > **{{site.data.keyword.filestorage_short}}** und klicken Sie auf den Namen Ihres Datenträgers im Feld **Datenträgername**.
2. Blättern Sie zum Abschnitt **Autorisierte Hosts** auf der Seite.
3. Klicken Sie rechts auf der Seite auf **Host autorisieren**. Wählen Sie die Hosts aus, die auf diesen bestimmten Datenträger zugreifen können.

 

## Vorgehensweise zum Anzeigen der Liste von Hosts mit Berechtigung für einen {{site.data.keyword.filestorage_short}}-Datenträger

Führen Sie die folgenden Schritte aus, um die Liste der Hosts anzuzeigen, die für einen Datenträger zugriffsberechtigt sind.

1. Klicken Sie auf **Speicher > {{site.data.keyword.filestorage_short}}** und klicken Sie auf den Namen Ihres Datenträgers im Feld **Datenträgername**.
2. Blättern Sie zum Abschnitt **Autorisierte Hosts** am Ende der Seite.

Hier finden Sie die Liste der Hosts, die zurzeit berechtigt sind, auf den Datenträger zuzugreifen.


## Vorgehensweise zum Anzeigen der {{site.data.keyword.filestorage_short}}-Datenträger, für die ein Host zugriffsberechtigt ist

Sie können die Datenträger, auf die ein Host Zugriff hat, sowie Informationen, die zum Herstellen einer Verbindung erforderlich sind, anzeigen – Datenträgername, Speichertyp, Zieladresse, Kapazität und Position:

1. Klicken Sie auf **Geräte** > **Geräteliste** im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} und klicken Sie auf das gewünschte Gerät.
2. Wählen Sie die Registerkarte 'Speicher' aus.

Es wird eine Liste von Speicherdatenträgern angezeigt, auf die dieser bestimmte Host Zugriff hat. Die Datenträger werden nach Speichertyp (Block, Datei, andere) gruppiert. Über die jeweiligen **Aktion**-Menüs können Sie zusätzlichen Speicher autorisieren oder Zugriffsberechtigungen entfernen.

 

## Vorgehensweise zum An- und Abhängen von {{site.data.keyword.filestorage_short}}-Speicher

Sie können die in der Ansicht **Datenträgerdetails** bereitgestellten Mountpunktinformationen verwenden, um {{site.data.keyword.filestorage_short}} von einem Host aus anzuhängen.

In dem folgenden Artikel finden Sie Details zum Anhängen und Abhängen von {{site.data.keyword.filestorage_short}}-Speicher über einen Host: [Auf {{site.data.keyword.filestorage_short}} unter Linux zugreifen](accessing-file-storage-linux.html).

 

## Vorgehensweise zum Widerrufen des Hostzugriffs auf {{site.data.keyword.filestorage_short}}

Wenn Sie den Zugriff von einem Host auf einen bestimmten Speicherdatenträger beenden wollen, können Sie den Zugriff wiederrufen. Nach dem Widerrufen des Zugriffs wird die Hostverbindung vom Datenträger gelöscht und weder das Betriebssystem noch Anwendungen können mit dem Datenträger kommunizieren. 

**Anmerkung:** Zur Vermeidung von hostseitigen Problemen hängen Sie den Speicherdatenträger von Ihrem Betriebssystem ab, bevor Sie den Zugriff widerrufen, sodass keine Laufwerke fehlen oder Daten beschädigt werden.

Sie können den Zugriff über 'Speicher' in der Ansicht 'Geräteliste' oder in der Ansicht 'Speicher' widerrufen.

### Vorgehensweise zum Widerrufen des Zugriffs über die Geräteliste:

1. Klicken Sie auf **Geräte** > **Geräteliste** im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} und klicken Sie doppelt auf das betreffende Gerät.
2. Wählen Sie die Registerkarte **Speicher** aus.
3. Es wird gruppiert nach Speichertyp (Block, Datei, andere) eine Liste von Speicherdatenträgern angezeigt, auf die dieser bestimmte Host Zugriff. Wählen Sie das jeweilige Menü **Aktion** neben dem Datenträger aus, für den Sie den Zugriff widerrufen möchten, und klicken Sie auf **Zugriff widerrufen**.
4. Bestätigen Sie, dass Sie den Zugriff auf einen Datenträger widerrufen möchten, da die Aktion nicht rückgängig gemacht werden kann. Klicken Sie auf **Ja**, um den Datenträgerzugriff zu widerrufen, oder auf **Nein**, um die Aktion abzubrechen.

**Anmerkung:** Wenn Sie die Verbindungen mehrerer Datenträger von einem bestimmten Host trennen wollen, müssen Sie die Aktion 'Zugriff widerrufen' für jeden Datenträger wiederholen.

 

### Vorgehensweise zum Widerrufen des Zugriffs über die Ansicht 'Speicher':
1. Klicken Sie auf **Speicher > {{site.data.keyword.filestorage_short}}** und wählen Sie den **Datenträger** aus, für den Sie den Zugriff widerrufen wollen.
2. Blättern Sie hinunter zum Abschnitt **Autorisierte Hosts** auf der Seite.
3. Klicken Sie neben dem Host, dessen Zugriff widerrufen werden soll, auf **Aktionen** und wählen Sie **Zugriff widerrufen** aus.
4. Bestätigen Sie, dass Sie den Zugriff auf einen Datenträger widerrufen möchten, da die Aktion nicht rückgängig gemacht werden kann. Klicken Sie auf **Ja**, um den Datenträgerzugriff zu widerrufen, oder auf **Nein**, um die Aktion abzubrechen.

**Anmerkung:** Wenn Sie die Verbindung zwischen mehreren Hosts und einem bestimmten Datenträger trennen wollen, müssen Sie die Aktion 'Zugriff widerrufen' für jeden Host wiederholen.

 

## Vorgehensweise zum Stornieren eines Speicherdatenträgers

Wenn Sie einen bestimmten Datenträger nicht mehr benötigen, können Sie ihn stornieren. Zur Stornierung eines Speicherdatenträgers müssen Sie zunächst den Zugriff für alle Hosts widerrufen.

1. Klicken Sie auf **Speicher**>**{{site.data.keyword.filestorage_short}}**.
2. Klicken Sie für den zu stornierenden Datenträger auf **Aktionen** und wählen Sie die Option zum Stornieren der {{site.data.keyword.filestorage_short}}-Instanz aus.****
3. Bestätigen Sie, ob der Datenträger sofort oder am Rechnungsstichtag für den Bereitstellungszeitpunkt des Datenträgers storniert werden soll. Klicken Sie auf **Weiter** oder **Schließen**. **Anmerkung:** Wenn Sie die Option auswählen, den Datenträger am Rechnungsstichtag zu stornieren, können Sie die Stornierungsanforderung vor ihrem Stichtag nichtig machen.
4. Klicken Sie auf das Kontrollkästchen für die Bestätigung und klicken Sie auf **Bestätigen**.

 

## Vorgehensweise zum Anzeigen der Details eines bereitgestellten {{site.data.keyword.filestorage_short}}-Datenträgers

Sie können eine Zusammenfassung der wichtigen Informationen für den ausgewählten Speicherdatenträger, einschließlich weiterer Snapshot- und Replikationsfunktionen anzeigen, die dem Speicher hinzugefügt wurden.

1. Klicken Sie auf **Speicher**>**{{site.data.keyword.filestorage_short}}**.
2. Klicken Sie auf den gewünschten **Datenträgernamen** in der Liste.
