---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-14"

keywords: File Storage, Endurance, Performance, IOPS, replication, billing, file storage, NFS,

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
 {:shortdesc: .shortdesc}


# Informationen zu {{site.data.keyword.filestorage_short}}
{: #about}

{{site.data.keyword.filestorage_full}} ist ein persistenter, schneller und flexibler, NFS-basierter {{site.data.keyword.filestorage_short}}, der über ein Netz angeschlossen ist. In dieser Umgebung mit NAS-Speicher (NAS – Network-attached Storage) haben Sie vollständige Kontrolle über die Funktion und Leistung Ihrer gemeinsam genutzten Dateispeicher. Gemeinsam genutzte {{site.data.keyword.filestorage_short}}-Speicher können aus Gründen der Ausfallsicherheit mit bis zu 64 autorisierten Geräten über gesteuerte TCP/IP-Verbindungen verbunden werden.
{:shortdesc}

## Features
{: #FileStorageFeatures}

{{site.data.keyword.filestorage_short}} bietet kombiniert mit einem außergewöhnlichen Feature-Set eine leistungsfähige Permanenz und Verfügbarkeit. {{site.data.keyword.filestorage_short}} wurde unter Verwendung von Industriestandards und bewährten Verfahren erstellt und ist darauf ausgerichtet, die Integrität der Daten zu schützen. {{site.data.keyword.filestorage_short}} hält die Verfügbarkeit sowohl bei Wartungsereignissen als auch bei ungeplanten Ausfällen aufrecht und stellt gleichzeitig ein konsistentes Leistungsniveau sicher.

Nutzen Sie die folgenden Kernfunktionen von {{site.data.keyword.filestorage_short}}:

- **Konsistentes Leistungsniveau**
   - Wird durch die Zuordnung von E/A-Operationen pro Sekunde (IOPS) zu einzelnen Datenträgern auf Protokollebene bereitgestellt.
- **{{site.data.keyword.filestorage_short}}**
   - Ist für dateibasierte, gemeinsam genutzte NFS-Speicher verfügbar.
- **Hohe Dauerhaftigkeit und Ausfallsicherheit**
   - Schützt die Integrität der Daten und gewährleistet die Verfügbarkeit bei Wartungsereignissen und ungeplanten Ausfällen, ohne dass RAID-Arrays (Redundant Arrays of Independent Disks) auf Betriebssystemebene erstellt und verwaltet werden müssen.
- **Verschlüsselung ruhender Daten** [(in ausgewählten Rechenzentren verfügbar)](/docs/infrastructure/FileStorage?topic=FileStorage-news)
   - Providerverwaltete Verschlüsselung für ruhende Daten wird ohne Zusatzkosten bereitgestellt.
- **Gesamter Speicher Flash-gestützt** [(in ausgewählten Rechenzentren verfügbar)](/docs/infrastructure/FileStorage?topic=FileStorage-news)
   - Gesamter Speicher ist Flashspeicher für Datenträger und kann mit 2 IOPS/GB oder höher bereitgestellt werden.
- **Snapshots** [(in ausgewählten Rechenzentren verfügbar)](/docs/infrastructure/FileStorage?topic=FileStorage-news).
   - Erfasst zeitpunktbezogene Datensnapshots ohne Betriebsunterbrechungen.
- **Replikation**  [(in ausgewählten Rechenzentren verfügbar)](/docs/infrastructure/FileStorage?topic=FileStorage-news)
   - Ist nur verfügbar, wenn Speicher in [ausgewählten Rechenzentren](/docs/infrastructure/FileStorage?topic=FileStorage-news) bereitgestellt wird.
   - Snapshots werden automatisch in ein Partnerrechenzentrum von {{site.data.keyword.BluSoftlayer_full}} kopiert.
- **Hoch verfügbare Konnektivität**
   - Verwendet redundante Netzverbindungen zur Maximierung der Verfügbarkeit.
   - Durch NFS-basierten {{site.data.keyword.filestorage_short}} geleitete TCP/IP-Verbindungen.
- **Gleichzeitiger Zugriff**
   - Ermöglicht den gleichzeitigen Zugriff durch mehrere Hosts (bis zu 64) auf Datendatenträger.
- **Datenbanken in Clusterkonfiguration**
   - Unterstützt erweiterte Anwendungsfälle wie Datenbanken mit Clusterkonfiguration.

## Rechnungsstellung

Sie können stündliche oder monatliche Rechnungsstellung für einen Dateidatenträger auswählen. Der Typ der Rechnungsstellung, der für eine LUN ausgewählt wird, gilt für den zugehörigen Snapshotbereich und für Replikate. Wenn Sie zum Beispiel eine LUN mit stündlicher Rechnungsstellung bereitstellen, werden alle Gebühren für Snapshots oder Replikate stündlich in Rechnung gestellt. Wenn Sie eine LUN mit monatlicher Rechnungsstellung bereitstellen, werden Gebühren für Snapshots oder Replikate monatlich in Rechnung gestellt.

Bei der **stündlichen Rechnungsstellung** wird die Stundenzahl, die der Dateidatenträger auf dem Konto vorhanden war, zu dem Zeitpunkt berechnet, an dem die LUN gelöscht wird oder der Rechnungsstellungszyklus endet, je nachdem, welcher Zeitpunkt zuerst kommt. Die stündliche Rechnungsstellung ist eine gute Wahl für Speicher, der für einige wenige Tage oder weniger als einen ganzen Monat lang genutzt wird. Die Abrechnung nach Stunden ist nur für Speicher verfügbar, der in [ausgewählten Rechenzentren](/docs/infrastructure/FileStorage?topic=FileStorage-news) bereitgestellt wird.

Bei der **monatlichen Rechnungsstellung** erfolgt die Berechnung für den Preis anteilmäßig ab dem Erstellungsdatum bis zum Ende des Rechnungsstellungszyklus und die Rechnung wird unverzüglich gestellt. Es erfolgt keine Rückerstattung, wenn ein Datenträger vor dem Ende des Rechnungsstellungszyklus gelöscht wird. Die monatliche Rechnungsstellung ist eine gute Wahl für Speicher, der für Auslastungen im Produktionsbetrieb genutzt wird, die Daten verwenden, die über längere Zeiträume (einen Monat oder länger) gespeichert werden und zugänglich sein müssen.


**Performance**
<table>
  <caption>Tabelle 1 enthält die Preise für Performance-Speicher mit monatlicher und stündlicher Rechnungsstellung.</caption>
  <tr>
   <th>Monatlicher Preis</th>
   <td>$0,10/GB + $0,07/IOPS</td>
  </tr>
  <tr>
   <th>Stündlicher Preis</th>
   <td>$0,0001/GB + $0,0002/IOPS</td>
  </tr>
</table>

**Endurance**
<table>
  <caption>Tabelle 2 enthält die Preise für Endurance-Speicher für die einzelnen Stufen mit den Optionen für monatliche und stündliche Rechnungsstellung.</caption>
  <tr>
   <th>IOPS-Stufe</th>
   <th>0,25 IOPS/GB</th>
   <th>2 IOPS/GB</th>
   <th>4 IOPS/GB</th>
   <th>10 IOPS/GB</th>
  </tr>
  <tr>
   <th>Monatlicher Preis</th>
   <td>$0,06/GB</td>
   <td>$0,15/GB</td>
   <td>$0,20/GB</td>
   <td>$0,58/GB</td>
  </tr>
  <tr>
   <th>Stündlicher Preis</th>
   <td>$0,0001/GB</td>
   <td>$0,0002/GB</td>
   <td>$0,0003/GB</td>
   <td>$0,0009/GB</td>
  </tr>
</table>



## Bereitstellung

{{site.data.keyword.filestorage_short}}-Datenträger können von 20 GB bis 12 TB mit zwei Optionen bereitgestellt werden: <br/>
- Sie können **Endurance**-Stufen bereitstellen, die über vordefinierte Leistungsstufen und andere Funktionen wie Snapshots und Replikation verfügen.
- Sie können eine hochleistungsfähige **Performance-Umgebung** mit einer zugeordneten Kapazität an E/A-Operationen pro Sekunde (IOPS) erstellen.


### Bereitstellung mit Endurance-Stufen

Endurance {{site.data.keyword.filestorage_short}} ist in vier IOPS-Leistungsstufen zur Unterstützung verschiedener Anwendungsanforderungen verfügbar. <br />

- **0,25 IOPS pro GB** sind für Workloads mit niedriger E/A-Intensität gedacht. Solche Workloads sind in der Regel durch einen hohen Prozentsatz an Daten gekennzeichnet, die zu jedem Zeitpunkt inaktiv sind. Beispiele für Anwendungen sind das Speichern von Mailboxen oder das Speichern von gemeinsam genutzten Dateien auf Abteilungsebene.

- **2 IOPS pro GB** sind für die meisten Fälle allgemeiner Nutzung vorgesehen. Beispiele für Anwendungen sind das Hosting kleiner Datenbanken zur Unterstützung von Webanwendungen oder VM-Plattenimages für einen Hypervisor.

- **4 IOPS pro GB** sind für Workloads höherer Intensität vorgesehen. Solche Workloads sind in der Regel durch einen hohen Prozentsatz an Daten gekennzeichnet, die zu jedem Zeitpunkt aktiv sind. Beispielanwendungen sind transaktionsorientierte und andere leistungskritische Datenbanken.

- **10 IOPS pro GB** sind für anspruchsvollste Workloads vorgesehen, wie zum Beispiel für die von NoSQL-Datenbanken generierten Workloads und für die Datenverarbeitung von Analysen (Analytics). Diese Stufe ist nur für Speicher verfügbar, der in [ausgewählten Rechenzentren](/docs/infrastructure/FileStorage?topic=FileStorage-news) mit einer Größe von bis zu 4 TB bereitgestellt wird.

Bis zu 48.000 IOPS sind bei einem 12-TB-Endurance-Datenträger verfügbar.

Die Auswahl der richtigen Endurance-Stufe für Ihre Workload spielt eine wichtige Rolle. Ebenso wichtig ist die Verwendung der richtigen Blockgröße, der Ethernet-Verbindungsgeschwindigkeit sowie der Anzahl der Hosts, die zur Erzielung der maximalen Leistung erforderlich sind. Wenn einer dieser Aspekte nicht mit den anderen im Einklang steht, kann dies erhebliche Auswirkungen auf den resultierenden Durchsatz haben.

### Bereitstellung mit Performance

Performance ist eine Klasse von {{site.data.keyword.filestorage_short}}, die auf die Unterstützung von Anwendungen mit hohen E/A-Raten mit bekannten Leistungsanforderungen ausgerichtet ist, die sich nicht geeignet mit einer Endurance-Stufe darstellen lassen. Eine vorhersagbare Leistung wird durch die Zuordnung einer IOPS-Kapazität auf Protokollebene zu einzelnen Datenträgern erzielt. Verschiedene IOPS-Raten (100-48.000) können mit Speichergrößen von 20 GB bis 12 TB bereitgestellt werden.

Der Zugriff auf Performance for {{site.data.keyword.filestorage_short}} und das Anhängen erfolgen über eine NFS-Verbindung (NFS - Network File System). {{site.data.keyword.filestorage_short}} wird in der Regel dann verwendet, wenn Zugriffe auf den Datenträger durch mehrere Server gleichzeitig stattfinden. Konsistente Performance-Datenträger können nach den Größen und E/A-Operationen pro Sekunde (IOPS) in Tabelle 1 bestellt und mit Linux-Betriebssystemen verwendet werden.

<table cellpadding="1" cellspacing="1" style="width: 99%;">
 <caption>Tabelle 3 enthält die Größe und die IOPS-Kombinationen von Performance-Speicher.<br/><sup><img src="/images/numberone.png" alt="Fußnote" /></sup> Ein IOPS-Grenzwert über 6.000 ist in ausgewählten Rechenzentren verfügbar.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
          <tr>
            <th>Größe (GB)</th>
            <th>Min. IOPS</th>
            <th>Max. IOPS</th>
          </tr>
          <tr>
            <td>20</td>
            <td>100</td>
            <td>1.000</td>
          </tr>
          <tr>
            <td>40</td>
            <td>100</td>
            <td>2.000</td>
          </tr>
          <tr>
            <td>80</td>
            <td>100</td>
            <td>4.000</td>
          </tr>
          <tr>
            <td>100</td>
            <td>100</td>
            <td>6.000</td>
          </tr>
          <tr>
            <td>250</td>
            <td>100</td>
            <td>6.000</td>
          </tr>
          <tr>
            <td>500</td>
            <td>100</td>
            <td>6.000 oder 10.,000<sup><img src="/images/numberone.png" alt="Fußnote" /></sup></td>
          </tr>
          <tr>
            <td>1.000</td>
            <td>100</td>
            <td>6.000 oder 20.000<sup><img src="/images/numberone.png" alt="Fußnote" /></sup></td>
          </tr>
          <tr>
            <td>2.000</td>
            <td>200</td>
            <td>6.000 oder 40.000<sup><img src="/images/numberone.png" alt="Fußnote" /></sup></td>
          </tr>
          <tr>
            <td>3.000 - 7.000</td>
            <td>300</td>
            <td>6.000 oder 48.000<sup><img src="/images/numberone.png" alt="Fußnote" /></sup></td>
          </tr>
          <tr>
            <td>8.000 - 9.000</td>
            <td>500</td>
            <td>6.000 oder 48.000<sup><img src="/images/numberone.png" alt="Fußnote" /></sup></td>
          </tr>
          <tr>
            <td>10.000 - 12.000</td>
            <td>1.000</td>
            <td>6.000 oder 48.000<sup><img src="/images/numberone.png" alt="Fußnote" /></sup></td>
          </tr>
</table>


Performance-Datenträger wurden dazu entwickelt, einen konsistenten Betrieb nahe am bereitgestellten IOPS-Niveau zu gewährleisten. Die Konsistenz erleichtert die Dimensionierung und Skalierung von Anwendungsumgebungen auf einem bestimmten Leistungsniveau. Darüber hinaus ist es möglich, eine Umgebung durch die Erstellung eines Datenträgers mit idealem Preis-/Leistungsverhältnis zu optimieren.
