---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Kapazität für gemeinsam genutzte Dateispeicher erweitern
{: #expandCapacity}

Mit diesem neuen Feature können aktuelle Benutzer von {{site.data.keyword.filestorage_full}} die Größe ihres {{site.data.keyword.filestorage_short}}-Speichers in GB-Inkrementen sofort auf bis zu 12 TB erweitern. Sie müssen kein Duplikat erstellen oder Daten manuell auf größere Datenträger migrieren. Während die Größe geändert wird, tritt kein Ausfall auf und der Zugriff ist weiterhin möglich.

Die Abrechnung für den Datenträger wird automatisch aktualisiert, um die anteilmäßige Differenz des neuen Preises in den laufenden Rechnungsstellungszyklus einzuarbeiten. Anschließend wird der neue Betrag vollständig im nächsten Rechnungsstellungszyklus in Rechnung gestellt.

Diese Funktion ist nur in [ausgewählten Rechenzentren](/docs/infrastructure/FileStorage?topic=FileStorage-news) verfügbar.

## Vorteile des erweiterbaren Speichers

- **Kostenmanagement** – Möglicherweise wissen Sie, dass der Umfang Ihrer Daten potenziell anwächst, zu Anfang benötigen Sie jedoch nur eine kleine Speichermenge. Durch die Möglichkeit zur Erweiterung können Kunden Speicherkosten zu Beginn sparen und anschließend größere Speichermengen nutzen, um ihre Anforderungen zu erfüllen.  

- **Wachsender Speicherbedarf** - Kunden, die ein rasches Wachstum des Datenvolumens zu verzeichnen haben, benötigen eine Möglichkeit, die Größe ihres Speichers schnell und problemlos erhöhen zu können, um diesem Wachstum Rechnung zu tragen.

## Auswirkungen der Erweiterung der Speicherkapazität auf die Replikation

Die Erweiterungsaktion für den primären Speicher hat automatisch eine Größenänderung des Replikats zur Folge.

## Beschränkungen

Dieses Feature ist nur für Speicher verfügbar, der in [Rechenzentren](/docs/infrastructure/FileStorage?topic=FileStorage-news) mit erweiterter Funktionalität bereitgestellt wird. Verschlüsselter Speicher, der in diesen Rechenzentren bereitgestellt wird, kann auf bis zu 12 TB vergrößert werden.

Die bestehenden Größenbegrenzungen für {{site.data.keyword.filestorage_short}}-Speicher, der mit Endurance bereitgestellt wurde, gelten weiterhin (d. h. bis zu 4 TB für die 10-IOPS-Stufe und bis zu 12 TB für alle anderen Stufen).

## Größe des Speichers ändern
{: #steps}

1. Klicken Sie im {{site.data.keyword.slportal}} auf **Speicher** > **{{site.data.keyword.filestorage_short}}** oder im Katalog von {{site.data.keyword.BluSoftlayer_full}} auf **Infrastruktur** > **Speicher** > **{{site.data.keyword.filestorage_short}}**.
2. Wählen Sie den Datenträger in der Liste aus und klicken Sie auf **Aktionen** > **Datenträger ändern**.
3. Geben Sie die neue Speichergröße in GB ein.
4. Prüfen Sie Ihre Auswahl und die neue Preisgestaltung.
5. Klicken Sie auf **Ich habe die Rahmenvereinbarung gelesen** und auf **Bestellung abschicken**.
6. Ihre neue Speicherzuordnung steht nach wenigen Minuten zur Verfügung.

Alternativ dazu können Sie den folgenden Befehl in der SL-CLI verwenden. 
```
# slcli file volume-modify --help
Syntax: slcli file volume-modify [OPTIONEN] DATENTRÄGER-ID

Optionen:
  -c, --new-size INTEGER        Neue Größe des Dateidatenträgers in GB. 
                                ***Wird keine Größe angegeben, wird die
                                ursprüngliche Größe des Datenträgers verwendet.***
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
                                der neue IOPS/GB-Wert ebenfalls geringer als 0,3 sein.
                                Wenn der ursprüngliche IOPS/GB-Wert für den
                                Datenträger größer-gleich 0,3 ist, muss der neue
                                IOPS/GB-Wert für den Datenträger ebenfalls
                                größer-gleich 0,3 sein.]
  -t, --new-tier [0.25|2|4|10]  Endurance-Speichertier (IOPS pro GB) [nur
                                für Endurance-Datenträger] ***Wird kein Tier
                                angegeben, wird das ursprüngliche Tier des
                                Datenträgers verwendet.***
                                Voraussetzungen: [Wenn der ursprüngliche IOPS/GB-
                                Wert für den Datenträger 0,25 ist, muss der neue
                                IOPS/GB-Wert für Datenträger ebenfalls 0,25 sein.
                                Wenn der ursprüngliche IOPS/GB-Wert für den
                                Datenträger größer als 0,25 ist, muss der neue
                                IOPS/GB-Wert für den Datenträger ebenfalls
                                größer als 0,25 sein.]
  -h, --help                    Diese Nachricht anzeigen und Ausführung beenden.
```
