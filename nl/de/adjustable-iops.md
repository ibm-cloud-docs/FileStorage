---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-24"

---
{:new_window: target="_blank"}

# Konfigurierbare E/A-Operationen pro Sekunde (IOPS)

Mit dieser neuen Funktion können {{site.data.keyword.filestorage_full}}-Speicherbenutzer die E/A-Operationen pro Sekunde (IOPS) ihrer vorhandenen {{site.data.keyword.filestorage_short}}-Instanz sofort anpassen. Es ist nicht erforderlich, ein Duplikat zu erstellen oder Daten manuell in den neuen Speicher migrieren zu müssen. Während der Anpassungsoperation tritt für den Benutzer kein Ausfall oder ein Verlust des Zugriffs auf den Speicher auf. 

Die Abrechnung für den Speicher wird aktualisiert, um die anteilmäßige Differenz des neuen Preises in den laufenden Rechnungsstellungszyklus einzuarbeiten. Anschließend wird der neue Betrag vollständig im nächsten Rechnungsstellungszyklus in Rechnung gestellt.

Diese Funktion ist nur in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) verfügbar. 

## Welchen Vorteil haben konfigurierbare E/A-Operationen pro Sekunde (IOPS) für Sie?

- Kostenmanagement – Einige Kunden benötigen eine hohe IOPS-Einstellung möglicherweise nur während Phasen hoher Auslastung. Ein großes Einzelhandelsgeschäft hat möglicherweise an Feiertagen hohe Auslastungen und benötigt dann höhere E/A-Operationen pro Sekunde (IOPS) für seinen Speicher als zum Beispiel in der Mitte des Sommers. Diese Funktion bietet ihnen die Möglichkeit, ihre Kosten zu verwalten und nur dann für höhere IOPS zu zahlen, wenn sie diese tatsächlich benötigen.

## Gibt es irgendwelche Einschränkungen?

Dieses Feature ist nur für Speicher verfügbar, der in [Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) mit erweiterter Funktionalität bereitgestellt wird.

Clients können nicht zwischen Endurance und Performance umschalten, wenn sie ihre IOPS-Kapazität anpassen. Benutzer können eine neue IOPS-Stufe oder IOPS-Ebene für ihren Speicher unter den folgenden Kriterien/Einschränkungen angeben: 

- Wenn der ursprüngliche Datenträger den Typ Endurance mit der Stufe 0,25 hat, kann die IOPS-Stufe nicht aktualisiert werden.
- Wenn der ursprüngliche Datenträger den Typ Performance mit < 0,30 IOPS/GB hat, umfassen die verfügbaren Optionen nur die Kombinationen aus Größe und IOPS-Werten, die < 0,30 IOPS/GB ergeben. 
- Wenn der ursprüngliche Datenträger den Typ Performance mit >= 0,30 IOPS/GB hat, umfassen die verfügbaren Optionen nur die Kombinationen aus Größe und IOPS-Werten, die >= 0,30 IOPS/GB ergeben. 

## Welche Auswirkung hat die Konfiguration der E/A-Operationen pro Sekunde (IOPS) auf die Replikation?

Wenn für den Datenträger eine Replikation eingerichtet ist, wird das Replikat automatisch mit der IOPS-Auswahl des primären Datenträgers aktualisiert. 

## Wie wird die IOPS-Kapazität für den Speicher konfiguriert?

1. Klicken Sie im {{site.data.keyword.slportal}} auf **Speicher** > **{{site.data.keyword.filestorage_short}}** oder im Katalog von {{site.data.keyword.BluSoftlayer_full}} auf **Infrastruktur** > **Speicher** > **{{site.data.keyword.filestorage_short}}**.
2. Wählen Sie den Datenträger in der Liste aus und klicken Sie auf **Aktionen** > **Datenträger ändern**.
3. Treffen Sie unter **IOPS-Optionen für Speicher** eine neue Auswahl:
    - Endurance (abgestufte E/A-Operationen pro Sekunde): Wählen Sie eine IOPS-Stufe größer als 0,25 IOPS/GB für Ihren Speicher aus. Sie können die IOPS-Stufe jederzeit erhöhen. Eine Verringerung ist jedoch nur einmal im Monat möglich.
    - Performance (zugeordnete E/A-Operationen pro Sekunde): Geben Sie die neue IOPS-Option für Ihren Speicher an, indem Sie einen Wert im Bereich von 100 bis 48.000 IOPS eingeben. (Beachten Sie die Grenzwerte im Bestellformular, die für Größen möglicherweise gelten.)
4. Prüfen Sie Ihre Auswahl und die neue Preisgestaltung.
5. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen...** und klicken Sie auf **Bestellung abschicken**.
6. Ihre neue Speicherzuordnung steht nach wenigen Minuten zur Verfügung.
