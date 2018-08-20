---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-24"

---
{:new_window: target="_blank"}

# IOPS anpassen

Mit dieser neuen Funktion können {{site.data.keyword.blockstoragefull}}-Speicherbenutzer die E/A-Operationen pro Sekunde (IOPS) ihrer vorhandenen {{site.data.keyword.blockstorageshort}}-Instanz sofort anpassen. Sie müssen kein Duplikat erstellen oder Daten manuell in den neuen Speicher kopieren. Während der Anpassungsoperation tritt für den Benutzer kein Ausfall oder ein Verlust des Zugriffs auf den Speicher auf. 

Die Abrechnung für den Speicher wird aktualisiert, um die anteilmäßige Differenz des neuen Preises in den laufenden Rechnungsstellungszyklus einzuarbeiten. Der neue Betrag wird vollständig im nächsten Rechnungsstellungszyklus in Rechnung gestellt.


## Vorteile von anpassbaren IOPS

- Kostenmanagement – Einige Kunden benötigen eine hohe IOPS-Einstellung möglicherweise nur während Phasen hoher Auslastung. Ein großes Einzelhandelsgeschäft hat möglicherweise an Feiertagen hohe Auslastungen und benötigt dann höhere E/A-Operationen pro Sekunde (IOPS) für seinen Speicher als zum Beispiel in der Mitte des Sommers. Diese Funktion bietet ihnen die Möglichkeit, ihre Kosten zu verwalten und nur dann für höhere IOPS zu zahlen, wenn sie diese benötigen.

## Beschränkungen

Diese Funktion ist nur in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) verfügbar. 

Clients können nicht zwischen Endurance und Performance umschalten, wenn sie ihre IOPS-Kapazität anpassen. Benutzer können eine neue IOPS-Stufe oder IOPS-Ebene für ihren Speicher unter den folgenden Kriterien/Einschränkungen angeben.

- Wenn der ursprüngliche Datenträger den Typ Endurance mit der Stufe 0,25 hat, kann die IOPS-Stufe nicht aktualisiert werden.
- Wenn der ursprüngliche Datenträger den Typ Performance mit kleiner als 0,30 IOPS/GB hat, umfassen die verfügbaren Optionen nur die Kombinationen aus Größe und IOPS-Werten, die kleiner als 0,30 IOPS/GB ergeben. 
- Wenn der ursprüngliche Datenträger den Typ Performance mit gleich oder größer als 0,30 IOPS/GB hat, umfassen die verfügbaren Optionen nur die Kombinationen aus Größe und IOPS-Werten, die gleich oder größer als 0,30 IOPS/GB ergeben. 

## Auswirkung der IOPS-Anpassung auf Replikation

Wenn für den Datenträger eine Replikation eingerichtet ist, wird das Replikat automatisch mit der IOPS-Auswahl des primären Datenträgers aktualisiert. 

## IOPS in Ihrem Speicher anpassen

1. Rufen Sie Ihre {{site.data.keyword.blockstorageshort}}-Liste auf.
    - Klicken Sie im Kundenportal auf **Speicher** > **{{site.data.keyword.filestorage_short}}** ODER
    - im Katalog von {{site.data.keyword.BluSoftlayer_full}} auf **Infrastruktur** > **Speicher** > **{{site.data.keyword.filestorage_short}}**. 
2. Wählen Sie den Datenträger in der Liste aus und klicken Sie auf **Aktionen** > **Datenträger ändern**.
3. Treffen Sie unter **IOPS-Optionen für Speicher** eine neue Auswahl:
    - Endurance (abgestufte E/A-Operationen pro Sekunde): Wählen Sie eine IOPS-Stufe größer als 0,25 IOPS/GB für Ihren Speicher aus. Sie können die IOPS-Stufe jederzeit erhöhen. Eine Verringerung ist jedoch nur einmal im Monat möglich.
    - Performance (zugeordnete E/A-Operationen pro Sekunde): Geben Sie die neue IOPS-Option für Ihren Speicher an, indem Sie einen Wert im Bereich von 100 bis 48.000 IOPS eingeben. (Beachten Sie die Grenzwerte im Bestellformular, die für Größen möglicherweise gelten.)
4. Prüfen Sie Ihre Auswahl und die neue Preisgestaltung.
5. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen...** und klicken Sie auf **Bestellung abschicken**.
6. Ihre neue Speicherzuordnung steht nach wenigen Minuten zur Verfügung.
