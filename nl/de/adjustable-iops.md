---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-23"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Konfigurierbare E/A-Operationen pro Sekunde (IOPS)

Mit dieser neuen Funktion können {{site.data.keyword.filestorage_full}}-Speicherbenutzer die E/A-Operationen pro Sekunde (IOPS) ihrer vorhandenen {{site.data.keyword.filestorage_short}}-Instanz während des Betriebs anpassen, ohne ein Duplikat erstellen oder Daten manuell in den neuen Speicher migrieren zu müssen. Während der Anpassungsoperation brauchen Benutzer keinen Ausfall oder Verlust des Zugriffs auf den Speicher zu gegenwärtigen. 

Die Abrechnung für den Speicher wird aktualisiert, um die anteilmäßige Differenz des neuen Preises in den laufenden Rechnungsstellungszyklus einzuarbeiten und den vollen Betrag im nächsten Rechnungsstellungszyklus in Rechnung zu stellen.

Diese Funktion ist nur in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) verfügbar. 

## Welchen Nutzen haben konfigurierbare E/A-Operationen pro Sekunde (IOPS)?

- Kostenmanagement – Einige Kunden benötigen eine hohe IOPS-Einstellung möglicherweise nur während Phasen hoher Auslastung. Ein großes Einzelhandelsgeschäft hat vielleicht hohe Auslastungen an Feiertagen und benötigt dann höhere IOPS für seinen Speicher als zum Beispiel in der Mitte des Sommers. Diese Funktion bietet ihnen die Möglichkeit, ihre Kosten zu verwalten und nur dann für höhere IOPS zu zahlen, wenn sie diese tatsächlich benötigen.

## Gibt es irgendwelche Einschränkungen?

Dieses Feature ist nur für Speicher verfügbar, der in [Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) mit erweiterter Funktionalität bereitgestellt wird.

Clients können nicht zwischen Endurance und Performance umschalten, wenn sie ihre IOPS-Kapazität anpassen. Benutzer können eine neue IOPS-Stufe oder IOPS-Ebene für ihren Speicher unter den folgenden Kriterien/Einschränkungen angeben: 

- Wenn der ursprüngliche Datenträger den Typ Endurance mit der Stufe 0,25 hat, kann die IOPS-Stufe nicht aktualisiert werden.
- Wenn der ursprüngliche Datenträger den Typ Performance mit < 0,30 IOPS/GB hat, sollten zu den verfügbaren Optionen nur die Kombinationen aus Größe und IOPS-Werten gehören, die < 0,30 IOPS/GB ergeben. 
- Wenn der ursprüngliche Datenträger den Typ Performance mit >= 0,30 IOPS/GB hat, sollten zu den verfügbaren Optionen nur die Kombinationen aus Größe und IOPS-Werten gehören, die >= 0,30 IOPS/GB ergeben. (Wählen Sie eine Größe aus, die größer-gleich dem ursprünglichen Datenträger ist.)

## Welche Auswirkung hat die Konfiguration der E/A-Operationen pro Sekunde (IOPS) auf die Replikation?

Wenn für den Datenträger eine Replikation eingerichtet ist, wird das Replikat automatisch mit der IOPS-Auswahl des primären Datenträgers aktualisiert. 

## Wie wird die IOPS-Kapazität für den Speicher konfiguriert?

1. Klicken Sie im {{site.data.keyword.slportal}} auf **Speicher** > **{{site.data.keyword.filestorage_short}}** oder im Katalog von {{site.data.keyword.BluSoftlayer_full}} auf **Infrastruktur** > **Speicher** > **{{site.data.keyword.filestorage_short}}**.
2. Wählen Sie den Datenträger in der Liste aus und klicken Sie auf **Aktionen** > **Datenträger ändern**.
3. Treffen Sie unter **IOPS-Optionen für Speicher** eine neue Auswahl:
    - Endurance (abgestufte E/A-Operationen pro Sekunde): Wählen Sie eine IOPS-Stufe größer als 0,25 IOPS/GB für Ihren Speicher aus. Sie können die IOPS-Stufe jederzeit erhöhen. Eine Verringerung ist jedoch nur einmal im Monat möglich.
    - Performance (zugeordnete E/A-Operationen pro Sekunde): Geben Sie die neue IOPS-Option für Ihren Speicher an, indem Sie einen Wert zwischen 100 und 48.000 IOPS eingeben. (Beachten Sie bestimmte Begrenzungen, die für Größen gelten können, im Bestellformular.)
4. Prüfen Sie Ihre Auswahl und die neue Preisgestaltung.
5. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen...** und klicken Sie auf **Bestellung abschicken**.
6. Ihre neue Speicherzuordnung sollte nach wenigen Minuten zur Verfügung stehen.
