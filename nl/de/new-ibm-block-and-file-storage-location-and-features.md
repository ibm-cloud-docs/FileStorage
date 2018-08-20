---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-20"

---
{:new_window: target="_blank"}

# Neue Standorte und Features für {{site.data.keyword.filestorage_short}}

Mit {{site.data.keyword.BluSoftlayer_full}} wird eine neue Version von {{site.data.keyword.filestorage_full}} eingeführt. Der neue Speicher ist in ausgewählten Rechenzentren verfügbar und wird durch Flashspeicher mit höheren IOPS-Niveaus bei der Verschlüsselung auf Plattenebene für ruhende Daten unterstützt. Sämtlicher in den ausgewählten Rechenzentren bestellter Speicher wird automatisch mit der neuen Version von {{site.data.keyword.filestorage_short}} erstellt.

**Anmerkung:** Der NFS-Mountpunkt für neue Datenträger wurde geändert. Details finden Sie im Abschnitt **Neuer Mountpunkt für verschlüsselte {{site.data.keyword.filestorage_short}}-Datenträger**.

Die neue Version von {{site.data.keyword.filestorage_short}} steht in folgenden Regionen/Rechenzentren zur Verfügung und in Kürze werden weitere Rechenzentren hinzugefügt.

<table role="presentation">
	<tr>
		<td><strong>US 2</strong></td>
		<td><strong>EU</strong></td>
		<td><strong>Australien</strong></td>
		<td><strong>Kanada</strong></td>
		<td><strong>Lateinamerika</strong></td>
		<td><strong>Asiatisch-pazifischer Raum</strong></td>
	</tr>
	<tr>
		<td><p>SJC03<br />
			SJC04<br />
			DC04<br />
			WDC06<br />
			WDC07<br />
			DAL09<br />
			DAL10<br />
			DAL12<br />
			DAL13<br /><br /><br /></p>
		</td>
		<td><p>LON02<br />
			LON04<br />
			LON06<br />
			FRA02<br />
			FRA04<br />
			FRA05<br />
			AMS01<br />
			AMS03<br />
			OSLO1<br />
			PAR01<br />
			MIL01<br /></p>
		</td>
		<td><p>SYD01<br />
			SYD04<br />
			MEL01<br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
		</td>
		<td><p>TOR01<br />
			MON01<br /><br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
		</td>
		<td><p>MEX01<br />
			SAO01<br /><br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
		</td>
		<td><p>TOK02<br />
			HKG02<br />
			SEO01<br />
			SNG01<br />
			CHE01<br /><br /><br /><br /><br /><br /><br /></p>
		</td>
	</tr>
</table>

*Tabelle 1 zeigt die Verfügbarkeit von Rechenzentren. Jede Region hat eine eigene Spalte. Einige Städte wie beispielsweise Dallas, San Jose, Washington D.C., Amsterdam, Frankfurt, London und Sydney haben mehrere Rechenzentren.*

Der neue Speicher verfügt über die folgenden Features und Funktionen:

- [Providerverwaltete Verschlüsselung für ruhende Daten](block-file-storage-encryption-rest.html). <br/> Alle {{site.data.keyword.filestorage_short}}-Datenträger werden automatisch ohne Aufpreis mit Verschlüsselung bereitgestellt.
- Option für Stufe 10 IOPS pro GB. <br/> Zum {{site.data.keyword.filestorage_short}}-Speicher des Typs Endurance wurde eine neue Stufe hinzugefügt, sodass auch anspruchsvollste Workloads unterstützt werden.
- Sämtlicher Speicher ist Flashspeicher. <br/> Alle {{site.data.keyword.filestorage_short}}-Speicher, bereitgestellt entweder mit Endurance- oder mit Performance-Optionen der Stufe 2 IOPS pro GB oder höher, werden ausschließlich durch Flashspeicher unterstützt.
- Unterstützung für Snapshots und Replikation.
- Eine stündliche Rechnungsstellungsoption wurde für Speicher hinzugefügt, dessen Verwendung für weniger als einen ganzen Monat geplant ist.
- Bis zu 48.000 IOPS für {{site.data.keyword.filestorage_short}}-Speicher, der mit Performance bereitgestellt wird.
- IOPS-Raten sind konfigurierbar, um die Leistung bei saisonalen Auslastungsänderungen zu verbessern. Weitere Informationen zu dieser Funktion finden Sie [hier](adjustable-iops.html).
- Sie können einen Klon Ihrer Daten mit der [Funktion zum Duplizieren von {{site.data.keyword.filestorage_short}}-Datenträgern](how-to-create-duplicate-volume.html) erstellen.
- Der Speicher ist in GB-Inkrementen sofort auf bis zu 12 TB erweiterbar, ohne dass ein Duplikat erstellt oder Daten manuell auf einen größeren Datenträger verschoben werden müssen. Weitere Informationen zu dieser Funktion finden Sie [hier](expandable_file_storage.html).

## Neuer Mountpunkt für erweiterte {{site.data.keyword.filestorage_short}}-Datenträger

Alle erweiterten {{site.data.keyword.filestorage_short}}-Datenträger, die in diesen Rechenzentren bereitgestellt werden, haben einen anderen Mountpunkt als nicht verschlüsselte Datenträger. Um sicherzustellen, dass Sie für beide Speicherdatenträger den richtigen Mountpunkt verwenden, können Sie die Mountpunktinformationen auf der Seite **Datenträgerdetails** in der Benutzerschnittstelle anzeigen. Sie können auch über einen API-Aufruf auf den richtigen Mountpunkt zugreifen: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Prüfen Sie diese Informationen erneut, um festzustellen, ob weitere Rechenzentren aktualisiert wurden, und um sich über neue Features und Funktionen zu informieren, die für {{site.data.keyword.filestorage_short}} hinzugefügt werden.
