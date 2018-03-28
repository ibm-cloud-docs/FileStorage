---

copyright:
  years: 2014, 2017
lastupdated: "2017-12-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Erweiterbare Dateifreigabekapazität

Mit diesem neuen Feature können aktuelle Benutzer von {{site.data.keyword.filestorage_full}} die Größe ihres vorhandenen {{site.data.keyword.filestorage_short}}-Speichers in GB-Inkrementen auf bis zu 12 TB sofort erweitern, ohne ein Duplikat erstellen oder Daten manuell auf größere Datenträger migrieren zu müssen. Die Vergrößerungsoperation erfolgt ausfallsicher und ohne Verlust des Speicherzugriffs. 

Die Rechnungsstellung für den Datenträger wird automatisch aktualisiert, um die anteilmäßige Differenz des neuen Preises in den laufenden Rechnungsstellungszyklus einzuarbeiten und den vollen Betrag im nächsten Rechnungsstellungszyklus in Rechnung zu stellen.

Diese Funktion ist nur in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) verfügbar. 

## Welchen Nutzen haben erweiterbare Dateifreigaben?

- **Kostenmanagement** – Sie wissen möglicherweise, dass es ein Potenzial für Wachstum Ihrer Daten gibt, jedoch benötigen Sie für den Anfang nur eine kleinere Menge an Speicher. Durch die Möglichkeit zur Erweiterung können Kunden Speicherkosten zu Beginn sparen und anschließend größere Speichermengen nutzen, um ihre Anforderungen zu erfüllen.  

- **Wachsender Speicherbedarf** - Kunden, die ein rasches Wachstum des Datenvolumens zu verzeichnen haben, benötigen eine Möglichkeit, die Größe ihres Speichers schnell und problemlos erhöhen zu können, um diesem Wachstum Rechnung zu tragen.

## Wie wirkt sich die Größenänderung des Speichers auf die Replikation aus?

Die Erweiterungsaktion für den primären Speicher hat automatisch eine Größenänderung des Replikats zur Folge.

## Sind irgendwelche Einschränkungen zu beachten?

Dieses Feature ist nur für Speicher verfügbar, der in [Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) mit erweiterter Funktionalität bereitgestellt wird. Der verschlüsselte Speicher, der in diesen Rechenzentren bereitgestellt wird, kann auf bis zu 12 TB vergrößert werden. 

Die bestehenden Größenbegrenzungen für {{site.data.keyword.filestorage_short}}-Speicher, der mit Endurance bereitgestellt wird, gelten weiterhin (d. h. bis zu 4 TB für die 10-IOPS-Stufe und bis zu 12 TB für alle anderen Stufen).

## Wie lässt sich feststellen, ob der bereitgestellte Speicher erweiterbar ist?

Für Speicher, der mit erweiterter Funktionalität bereitgestellt wird, werden ruhende Daten immer verschlüsselt. Sie können leicht feststellen, dass Ihr Speicher dazu gehört, wenn neben ihm das Sperrsymbol in der Portalbenutzerschnittstelle angezeigt wird. 

## Wie lässt sich die Größe des Speichers ändern?

1. Klicken Sie im {{site.data.keyword.slportal}} auf **Speicher** > **{{site.data.keyword.filestorage_short}}** oder im Katalog von {{site.data.keyword.BluSoftlayer_full}} auf **Infrastruktur** > **Speicher** > **{{site.data.keyword.filestorage_short}}**.
2. Wählen Sie den Datenträger in der Liste aus und klicken Sie auf **Aktionen** > **Datenträger ändern**.
3. Geben Sie die neue Speichergröße in GB ein.
4. Prüfen Sie Ihre Auswahl und die neue Preisgestaltung.
5. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen...** und klicken Sie auf **Bestellung abschicken**.
6. Ihre neue Speicherzuordnung sollte nach wenigen Minuten zur Verfügung stehen.

