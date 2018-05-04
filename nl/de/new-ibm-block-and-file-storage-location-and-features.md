---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-25"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Neue Standorte und Features für {{site.data.keyword.blockstorageshort}} und {{site.data.keyword.filestorage_short}}

Mit {{site.data.keyword.BluSoftlayer_full}} wird eine neue Version von {{site.data.keyword.blockstorageshort}} und {{site.data.keyword.filestorage_full}} eingeführt.  Der neue Speicher ist in ausgewählten Rechenzentren verfügbar und wird durch Flashspeicher mit höheren IOPS-Niveaus bei der Verschlüsselung auf Plattenebene für ruhende Daten unterstützt. Sämtlicher bereitgestellter Speicher in den ausgewählten Rechenzentren wird automatisch mit der neuen Version von {{site.data.keyword.blockstorageshort}} und {{site.data.keyword.filestorage_short}} bereitgestellt.

**Anmerkung:** Der NFS-Mountpunkt für neue Datenträger hat sich geändert. Ausführlichere Informationen finden Sie unter **Neuer Mountpunkt für verschlüsselte {{site.data.keyword.filestorage_short}}-Datenträger**.

Die neue Version von {{site.data.keyword.filestorage_short}} ist gegenwärtig in den folgenden Regionen/Rechenzentren verfügbar, wobei in Kürze weitere Rechenzentren verfügbar sein werden.
<table style="width:100%;">
	<caption>Verfügbarkeit von Rechenzentren</caption>
	<tbody>
		<tr>
			<td><strong>US 2</strong></td>
			<td><strong>EU</strong></td>
			<td><strong>Australien</strong></td>
			<td><strong>Kanada</strong></td>
			<td><strong>Lateinamerika</strong></td>
			<td><strong>Asiatisch-pazifischer Raum</strong></td>
		</tr>
		<tr>
			<td>
				<p>SJC03<br />
				SJC04<br />
				WDC04<br />
				WDC06<br />
				WDC07<br />
				DAL09<br />
				DAL10<br />
				DAL12<br />
				DAL13<br /><br /></p>
			</td>
			<td>
				<p>LON02<br />
				LON04<br />
				LON06<br />
				FRA02<br />
				FRA04<br />
				AMS01<br />
				AMS03<br />
				OSLO1<br />
				PAR01<br />
				MIL01<br /></p>
			</td>
			<td>
				<p>SYD01<br />
				SYD04<br />
				MEL01<br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>TOR01<br />
				MON01<br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>MEX01<br />SAO01<br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
						<td>
				<p>TOK02<br />
				HKG02<br />
				SEO01<br />
				SNG01<br />
				CHE01<br /><br /><br /><br /><br /><br /></p>
			</td>
			</tr>
	</tbody>
</table>


Der neue Speicher verfügt über die folgenden Features und Funktionen:

-  [Vom Provider verwaltete Verschlüsselung für ruhende Daten](block-file-storage-encryption-rest.html). Alle {{site.data.keyword.blockstorageshort}}- und {{site.data.keyword.filestorage_short}}-Speicher werden automatisch ohne Aufpreis mit Verschlüsselung bereitgestellt.
-  Option für Stufe 10 IOPS pro GB. Dem {{site.data.keyword.blockstorageshort}}- und {{site.data.keyword.filestorage_short}}-Speicher vom Endurance-Typ wurde zur Unterstützung der anspruchsvollsten Workloads eine neue Stufe hinzugefügt.
-  Sämtlicher Speicher ist Flashspeicher.  Alle {{site.data.keyword.blockstorageshort}}- und {{site.data.keyword.filestorage_short}}-Speicher, die entweder mit Endurance oder mit Performance der Stufe 2 IOPS pro GB oder höher bereitgestellt werden, werden durchgehend durch Flashspeicher unterstützt.
-  Snapshot- und Replikationsunterstützung mit {{site.data.keyword.blockstorageshort}}- und {{site.data.keyword.filestorage_short}}-Speicher, der mit Endurance oder Performance bereitgestellt wird.
-  Eine stündliche Rechnungsstellungsoption wurde für Speicher hinzugefügt, dessen Verwendung für weniger als einen ganzen Monat geplant ist. 
-  Bis zu 48.000 IOPS für {{site.data.keyword.blockstorageshort}}- und {{site.data.keyword.filestorage_short}}-Speicher, der mit Performance bereitgestellt wird.
-  IOPS-Raten sind konfigurierbar, um die Leistung bei saisonalen Auslastungsänderungen zu verbessern. Weitere Informationen zu dieser Funktion finden Sie [hier](adjustable-iops.html).
-  Sie können einen Klon Ihrer Daten mit der [Funktion zum Duplizieren von {{site.data.keyword.filestorage_short}}-Datenträgern](how-to-create-duplicate-volume.html) erstellen.
- Der Speicher ist in GB-Inkrementen auf bis zu 12 TB während des Betriebs erweiterbar, ohne dass ein Duplikat erstellt oder Daten manuell auf einen größeren Datenträger migriert werden müssen. Weitere Informationen zu dieser Funktion finden Sie [hier](expandable_file_storage.html).

## Neuer Mountpunkt für verschlüsselte {{site.data.keyword.filestorage_short}}-Datenträger

Alle verschlüsselten {{site.data.keyword.filestorage_short}}-Datenträger, die in diesen Rechenzentren bereitgestellt werden, haben einen anderen Mountpunkt als nicht verschlüsselte Datenträger.  Um sicherzustellen, dass Sie den richtigen Mountpunkt für Ihre verschlüsselten und nicht verschlüsselten File Storage-Datenträger verwenden, können Sie die Mountpunktinformationen auf der Datenträgerdetailsseite in der Benutzerschnittstelle abrufen und auf den richtigen Mountpunkt durch einen API-Aufruf zugreifen: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Prüfen Sie diese Informationen erneut, um festzustellen, ob weitere Rechenzentren aktualisiert wurden, und um sich über neu verfügbare Features und Funktionen zu informieren, die für {{site.data.keyword.blockstorageshort}} und {{site.data.keyword.filestorage_short}} hinzugefügt werden.
