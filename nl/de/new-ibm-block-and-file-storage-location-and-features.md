---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, locations, data centers

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Neue Standorte und Features
{: #news}

Mit {{site.data.keyword.cloud}} wird eine neue Version von {{site.data.keyword.filestorage_full}} eingeführt. Der neue Speicher ist in ausgewählten Rechenzentren verfügbar und wird durch Flashspeicher mit höheren IOPS-Niveaus bei der Verschlüsselung auf Plattenebene für ruhende Daten unterstützt. Sämtlicher in den ausgewählten Rechenzentren bestellter Speicher wird automatisch mit der neuen Version von {{site.data.keyword.filestorage_short}} erstellt.

Der NFS-Mountpunkt für neue Datenträger wurde geändert. Details finden Sie im Abschnitt [Neuer Mountpunkt für verschlüsselte {{site.data.keyword.filestorage_short}}-Datenträger](#new-mount-point-for-enhanced-file-storage-volumes).
{:important}

## Neue Standorte
{: #new-locations}

Die neue Version von {{site.data.keyword.filestorage_short}} steht in folgenden Regionen und Rechenzentren zur Verfügung und in Kürze werden weitere Rechenzentren hinzugefügt.

|US 2|Lateinamerika|Kanada|EU|Asien/Pazifik|Australien|
|-----|-----|-----|-----|-----|------|
| DAL09<br >DAL10<br />DAL12<br />DAL13<br />SJC03<br />SJC04<br />WDC04<br />WDC06<br />WDC07 | MEX01<br />SAO01 | MON01<br />TOR01  | AMS01<br />AMS03<br />FRA02<br />FRA04<br />FRA05<br />LON02<br />LON04<br />LON05<br />LON06<br />MIL01<br />OSLO1<br />PAR01 | CHE01<br />HKG02<br />SEO01<br />SNG01<br />TOK02<br />TOK04<br />TOK05 | MEL01<br />SYD01<br />SYD04<br />SYD05 |
{: caption="In Tabelle 1 wird die Verfügbarkeit von Rechenzentren aufgeführt. Jede Region hat eine eigene Spalte. Einige Städte wie beispielsweise Dallas, San Jose, Washington D.C., Amsterdam, Frankfurt, London und Sydney haben mehrere Rechenzentren." caption-side="top"}

## Neue Features und Funktionen
{: #features}

- [Anbietergesteuerte Verschlüsselung ruhender Daten](/docs/infrastructure/FileStorage?topic=FileStorage-encryption). <br/> Alle {{site.data.keyword.filestorage_short}}-Datenträger werden automatisch ohne Aufpreis mit Verschlüsselung bereitgestellt.
- Option für 10 IOPS/GB-Tier. <br/> Zum {{site.data.keyword.filestorage_short}}-Speicher des Typs Endurance wurde eine neue Stufe hinzugefügt, sodass auch anspruchsvollste Workloads unterstützt werden.
- Gesamter durch Flashspeicher gestützter Speicher. <br/> Alle {{site.data.keyword.filestorage_short}}-Speicher, bereitgestellt entweder mit Endurance- oder mit Performance-Optionen der Stufe 2 IOPS pro GB oder höher, werden ausschließlich durch Flashspeicher unterstützt.
- Unterstützung für Snapshots und Replikation.
- Eine stündliche Rechnungsstellungsoption wurde für Speicher hinzugefügt, dessen Verwendung für weniger als einen ganzen Monat geplant ist.
- Bis zu 48.000 IOPS für {{site.data.keyword.filestorage_short}}-Speicher, der mit Performance bereitgestellt wird.
- IOPS-Raten sind konfigurierbar, um die Leistung bei saisonalen Auslastungsänderungen zu verbessern. Weitere Informationen zu dieser Funktion finden Sie [hier](/docs/infrastructure/FileStorage?topic=FileStorage-adjustingIOPS).
- Sie können einen Klon Ihrer Daten mit der [Funktion zum Duplizieren von {{site.data.keyword.filestorage_short}}-Datenträgern](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume) erstellen.
- Der Speicher ist in GB-Inkrementen sofort auf bis zu 12 TB erweiterbar, ohne dass ein Duplikat erstellt oder Daten manuell auf einen größeren Datenträger verschoben werden müssen. Weitere Informationen zu dieser Funktion finden Sie [hier](/docs/infrastructure/FileStorage?topic=FileStorage-expandCapacity).

## Neuer Mountpunkt für erweiterte {{site.data.keyword.filestorage_short}}-Datenträger

Alle erweiterten {{site.data.keyword.filestorage_short}}-Datenträger, die in diesen Rechenzentren bereitgestellt werden, haben einen anderen Mountpunkt als nicht verschlüsselte Datenträger. Um sicherzustellen, dass Sie für beide Speicherdatenträger den richtigen Mountpunkt verwenden, können Sie die Mountpunktinformationen auf der Seite **Datenträgerdetails** in der Konsole anzeigen. Sie können auch über einen API-Aufruf auf den richtigen Mountpunkt zugreifen: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Um auf alle neuen Funktionen zugreifen zu können, wählen Sie `Storage-as-a-Service Package 759` aus, wenn Sie Ihre Bestellung über die API aufgeben. Weitere Informationen zur {{site.data.keyword.filestorage_short}}-Bestellung über die API finden Sie unter [order_file_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/file/#SoftLayer.managers.file.FileStorageManager.order_file_volume){: external}.
{:important}

Prüfen Sie diese Informationen erneut, um festzustellen, ob weitere Rechenzentren aktualisiert wurden, und um sich über neue Features und Funktionen zu informieren, die für {{site.data.keyword.filestorage_short}} hinzugefügt werden.
{:tip}
