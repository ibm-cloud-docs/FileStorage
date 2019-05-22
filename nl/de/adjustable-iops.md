---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, adjusting IOPS, increase IOPS, decrease IOPS, modify IOPS

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# IOPS anpassen
{: #adjustingIOPS}

Mit dieser neuen Funktion können {{site.data.keyword.filestorage_full}}-Speicherbenutzer die E/A-Operationen pro Sekunde (IOPS) ihrer vorhandenen {{site.data.keyword.filestorage_short}}-Instanz sofort anpassen. Sie müssen kein Duplikat erstellen oder Daten manuell in den neuen Speicher kopieren. Während der Anpassungsoperation tritt für den Benutzer kein Ausfall oder ein Verlust des Zugriffs auf den Speicher auf.

Die Abrechnung für den Speicher wird aktualisiert, um die anteilmäßige Differenz des neuen Preises in den laufenden Rechnungsstellungszyklus einzuarbeiten. Der neue Betrag wird vollständig im nächsten Rechnungsstellungszyklus in Rechnung gestellt.


## Vorteile von anpassbaren E/A-Operationen pro Sekunde

- Kostenmanagement – Einige Kunden benötigen eine hohe Einstellung für die E/A-Operationen pro Sekunde möglicherweise nur während Phasen hoher Auslastung. Ein großes Einzelhandelsgeschäft hat möglicherweise an Feiertagen hohe Auslastungen und benötigt dann höhere E/A-Operationen pro Sekunde (IOPS) für seinen Speicher als zum Beispiel in der Mitte des Sommers. Diese Funktion bietet ihnen die Möglichkeit, ihre Kosten zu verwalten und nur dann für höhere IOPS zu zahlen, wenn sie diese benötigen.

## Beschränkungen
{: #limitsofadjustIOPS}

Diese Funktion ist nur in [ausgewählten Rechenzentren](/docs/infrastructure/FileStorage?topic=FileStorage-news) verfügbar.

Clients können nicht zwischen Endurance und Performance umschalten, wenn sie ihre IOPS-Kapazität anpassen. Benutzer können eine neue IOPS-Stufe oder IOPS-Ebene für ihren Speicher unter den folgenden Kriterien und Einschränkungen angeben.

- Wenn der ursprüngliche Datenträger den Typ Endurance mit der Stufe 0,25 hat, kann die IOPS-Stufe nicht aktualisiert werden.
- Wenn der Originaldatenträger ein Performance-Tier mit kleiner-gleich 0,30 IOPS/GB ist, schließen Sie nur Größen- und IOPS-Kombinationen ein, deren Ergebnis kleiner-gleich 0,30 IOPS/GB ist.
- Wenn der Originaldatenträger ein Performance-Tier mit mehr als 0,30 IOPS/GB ist, schließen Sie nur Größen- und IOPS-Kombinationen ein, deren Ergebnis höher als 0,30 IOPS/GB ist.

## Auswirkung der IOPS-Anpassung auf Replikation

Wenn für den Datenträger eine Replikation eingerichtet ist, wird das Replikat automatisch mit der IOPS-Auswahl des primären Datenträgers aktualisiert.

## IOPS in Ihrem Speicher anpassen
{: #adjustingsteps}

1. Rufen Sie Ihre {{site.data.keyword.filestorage_short}}-Liste auf.
    - Klicken Sie im Kundenportal auf **Speicher** > **{{site.data.keyword.filestorage_short}}** ODER
    - Klicken Sie in der {{site.data.keyword.BluSoftlayer_full}}-Konsole auf **Infrastruktur** > **Speicher** > **{{site.data.keyword.filestorage_short}}**.
2. Wählen Sie den Datenträger in der Liste aus und klicken Sie auf **Aktionen** > **Datenträger ändern**.
3. Treffen Sie unter **IOPS-Optionen für Speicher** eine neue Auswahl:
    - Wählen Sie für 'Endurance (Gestaffelte IOPS)' Sie ein IOPS-Tier aus, das größer als 0,25 IOPS/GB Ihres Speichers ist. Sie können die IOPS-Stufe jederzeit erhöhen. Eine Verringerung ist jedoch nur einmal im Monat möglich.
    - Geben Sie für 'Performance (zugeordnete E/A-Operationen pro Sekunde)' die neue IOPS-Option für Ihren Speicher an, indem Sie einen Wert im Bereich von 100 bis 48.000 IOPS eingeben. (Beachten Sie die Grenzwerte im Bestellformular, die für Größen möglicherweise gelten.)
4. Prüfen Sie Ihre Auswahl und die neue Preisgestaltung.
5. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Auftrag erteilen**.
6. Ihre neue Speicherzuordnung steht nach wenigen Minuten zur Verfügung.

Alternativ dazu können Sie die IOPS über die SLCLI aktualisieren.
```
# slcli file volume-modify --help
Syntax: slcli file volume-modify [OPTIONEN] DATENTRÄGER-ID

Optionen:
  -c, --new-size INTEGER        Neue Größe des Dateidatenträgers in GB. ***Wird keine Größe angegeben, wird die ursprüngliche
                                Größe des Datenträgers verwendet.***
                                Mögliche Größen: [20, 40, 80, 100,
                                250, 500, 1000, 2000, 4000, 8000, 12000]
                                Minimum: [ursprüngliche Größe des Datenträgers]
  -i, --new-iops INTEGER        Performance-Speicher-IOPS, zwischen 100 und 6000
                                in Vielfachen von 100 [nur für Performance-
                                Datenträger] ***Wird kein IOPS-Wert angegeben,
                                wird der ursprüngliche IOPS-Wert des Datenträgers
                                verwendet.***
                                Voraussetzungen: [Wenn der ursprüngliche IOPS/GB-
                                Wert für den Datenträger geringer als 0,3 ist, muss
                                der neue IOPS/GB-Wert ebenfalls geringer als 0,3 sein. Wenn der ursprüngliche IOPS/GB-Wert für den
                                Datenträger größer-gleich 0,3 ist, muss der neue
                                IOPS/GB-Wert für den Datenträger ebenfalls
                                größer-gleich 0,3 sein.]
  -t, --new-tier [0.25|2|4|10]  Endurance-Speichertier (IOPS pro GB) [nur
                                für Endurance-Datenträger] ***Wird kein Tier
                                angegeben, wird das ursprüngliche Tier des
                                Datenträgers verwendet.***
                                Voraussetzungen: [Wenn der ursprüngliche IOPS/GB-
                                Wert für den Datenträger 0,25 ist, muss der neue
                                IOPS/GB-Wert für Datenträger ebenfalls 0,25 sein. Wenn der ursprüngliche IOPS/GB-Wert für den
                                Datenträger größer als 0,25 ist, muss der neue
                                IOPS/GB-Wert für den Datenträger ebenfalls
                                größer als 0,25 sein.]
  -h, --help                    Diese Nachricht anzeigen und Ausführung beenden.
```
