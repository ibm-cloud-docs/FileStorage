---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Einführung in {{site.data.keyword.filestorage_short}}

{{site.data.keyword.filestorage_full}} ist ein persistenter, schneller und flexibler, NFS-basierter {{site.data.keyword.filestorage_short}}, der über ein Netz angeschlossen ist. In dieser Umgebung mit NAS-Speicher (NAS - Network Attached Storage) haben Sie vollständige Kontrolle über die Funktion und Leistung Ihrer Dateifreigaben. {{site.data.keyword.filestorage_short}}-Freigaben können aus Gründen der Ausfallsicherheit mit bis zu 64 autorisierten Geräten über gesteuerte TCP/IP-Verbindungen verbunden werden.

{{site.data.keyword.filestorage_short}} ist aufgrund höchster Permanenz und Verfügbarkeit, kombiniert mit einem außergewöhnlichen Feature-Set, sowie durch die Nutzung von Industriestandards und bewährten Verfahren darauf ausgerichtet, die Integrität der Daten zu schützen und die Verfügbarkeit sowohl bei Wartungsereignissen als auch bei ungeplanten Ausfällen aufrechtzuerhalten und gleichzeitig eine konsistentes Leistungsniveau sicherzustellen.

Nutzen Sie die folgenden Kernfunktionen von {{site.data.keyword.filestorage_short}}:

- **Konsistentes Leistungsniveau**
   - Wird durch die Zuordnung von E/A-Operationen pro Sekunde (IOPS) zu einzelnen Datenträgern auf Protokollebene bereitgestellt.
- **{{site.data.keyword.filestorage_short}}**
   - Ist für dateibasierte NFS-Freigaben verfügbar.
- **Hohe Dauerhaftigkeit und Ausfallsicherheit**
   - Schützt die Integrität der Daten und gewährleistet die Verfügbarkeit bei Wartungsereignissen und ungeplanten Ausfällen, ohne dass RAID-Arrays (Redundant Arrays of Independent Disks) auf Betriebssystemebene erstellt und verwaltet werden müssen.
- **Verschlüsselung ruhender Daten** [(in ausgewählten Rechenzentren verfügbar)](new-ibm-block-and-file-storage-location-and-features.html)
   - Vom Provider verwaltete Verschlüsselung für ruhende Daten ohne Zusatzkosten
- **Gesamter Speicher Flash-gestützt** [(in ausgewählten Rechenzentren verfügbar)](new-ibm-block-and-file-storage-location-and-features.html)
   - Gesamter Speicher ist Flashspeicher für Datenträger, die mit Endurance oder Performance und 2 IOPS/GB oder höher bereitgestellt werden.
- **Snapshots (Bei Bereitstellung mit Endurance oder Performance in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html))**
   - Erfasst zeitpunktbezogene Datensnapshots ohne Betriebsunterbrechungen.
- **Replikation** (Bei Bereitstellung mit Endurance oder Performance in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html))
   - Snapshots werden automatisch in ein Partnerrechenzentrum von {{site.data.keyword.BluSoftlayer_full}} kopiert.
- **Hoch verfügbare Konnektivität**
   - Verwendet redundante Netzverbindungen zur Maximierung der Verfügbarkeit - durch NFS-basierten {{site.data.keyword.filestorage_short}} geleitete TCP/IP-Verbindungen
- **Gleichzeitiger Zugriff**
   - Ermöglicht den gleichzeitigen Zugriff durch mehrere Hosts (bis zu 64) auf Datendatenträger.
- **Datenbanken in Clusterkonfiguration**
   - Unterstützt erweiterte Anwendungsfälle wie Datenbanken mit Clusterkonfiguration.

## Stündliche/Monatliche Rechnungsstellung

Sie können stündliche oder monatliche Rechnungsstellung für einen Dateidatenträger auswählen. Der Typ der Rechnungsstellung, der für eine LUN ausgewählt wird, gilt für den zugehörigen Snapshotbereich und für Replikate. Wenn Sie zum Beispiel eine LUN mit stündlicher Rechnungsstellung bereitstellen, werden alle Gebühren für Snapshots oder Replikate stündlich in Rechnung gestellt. Wenn Sie eine LUN mit monatlicher Rechnungsstellung bereitstellen, werden Gebühren für Snapshots oder Replikate monatlich in Rechnung gestellt. 

Bei der **stündlichen Rechnungsstellung** wird die Berechnung der Stundenzahl, die der Dateidatenträger auf dem Konto vorhanden war, zu dem Zeitpunkt berechnet, zu dem der Datenträger gelöscht wird oder zu dem der Rechnungsstellungszyklus endet, je nachdem, welcher Zeitpunkt zuerst kommt. Die stündliche Rechnungsstellung ist eine gute Wahl für Speicher, der für einige wenige Tage oder weniger als einen ganzen Monat lang genutzt wird. Eine stündliche Rechnungsstellung ist nur für Speicher verfügbar, der in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) bereitgestellt wurde. 

Bei der **monatlichen Rechnungsstellung** erfolgt die Berechnung für den Preis anteilmäßig ab dem Erstellungsdatum bis zum Ende des Rechnungsstellungszyklus und die Rechnung wird unverzüglich gestellt. Es erfolgt keine Rückvergütung, wenn ein Datenträger vor dem Ende des Rechnungsstellungszyklus gelöscht wird. Die monatliche Rechnungsstellung ist eine gute Wahl für Speicher, der für Auslastungen im Produktionsbetrieb genutzt wird, die Daten verwenden, die über längere Zeiträume (einen Monat oder länger) gespeichert werden und zugänglich sein müssen.

 
### Performance:
<table>
 <tbody>
  <tr>
   <th>Monatlicher Preis</th>
   <td>$0,10/GB + $0,07/IOP</td>
  </tr>
  <tr>
   <th>Stündlicher Preis</th>
   <td>$0,0001/GB + $0,0002/IOP</td>
  </tr>
  </tbody>
</table>
 
### Endurance:
<table>
 <tbody>
  <tr>
   <th>IOPS-Stufe</th>
   <th>0,25 IOPS/GB</th>
   <th>2 IOPS/GB</th>
   <th>4 IOPS/GB</th>
   <th>10 IOPS/GB</th>
  </tr>
  <tr>
   <th>Monatlicher Preis</th>
   <td>$0,10/GB</td>
   <td>$0,20/GB</td>
   <td>$0,35/GB</td>
   <td>$0,58/GB</td>
  </tr>
  <tr>
   <th>Stündlicher Preis</th>
   <td>0,0002/GB</td>
   <td>$0,0003/GB</td>
   <td>$0,0005/GB</td>
   <td>$0,0009/GB</td>
  </tr>
  </tbody>
</table>

 

## Bereitstellung

{{site.data.keyword.filestorage_short}}-Datenträger können von 20 GB bis 12 TB mit zwei Bereitstellungsoptionen bereitgestellt werden: <br/>
- Sie können mit **Endurance-Stufen** bereitstellen, die über vordefinierte Leistungsstufen und Funktionen wie Snapshots und Replikation verfügen.
- Sie können eine hochleistungsfähige **Performance-Umgebung** mit einer zugeordneten Kapazität an E/A-Operationen pro Sekunde (IOPS) erstellen.

 
### Endurance-Stufen

Bei einer Bestellung mit Endurance können Sie unter mehreren Leistungsstufen zur Unterstützung verschiedenartiger Anforderungen wählen.

Endurance ist in drei IOPS-Leistungsstufen zur Unterstützung verschiedener Anwendungsanforderungen verfügbar.

- **0,25 IOPS pro GB** sind für Workloads mit niedriger E/A-Intensität gedacht. Solche Workloads sind in der Regel durch einen hohen Prozentsatz an Daten, die zu einem gegebenen Zeitpunkt inaktiv sind, gekennzeichnet. Beispiele für Anwendungen sind das Speichern von Mailboxen oder das Speichern von Dateifreigaben auf Abteilungsebene.
- **2 IOPS pro GB** sind für die Nutzung zu allgemeinsten Zwecken vorgesehen. Beispiele für Anwendungen sind das Hosting kleiner Datenbanken zur Unterstützung von Webanwendungen oder virtueller Plattenimages für einen Hypervisor.
- **4 IOPS pro GB** sind für Workloads höherer Intensität vorgesehen. Solche Workloads sind in der Regel durch einen hohen Prozentsatz an Daten, die zu einem gegebenen Zeitpunkt aktiv sind, gekennzeichnet. Beispielanwendungen sind transaktionsorientierte und andere leistungskritische Datenbanken.
- **10 IOPS pro GB** sind für die anspruchsvollsten Workloads vorgesehen, wie zum Beispiel die von NoSQL-Datenbanken generierten Workloads und Workloads von Datenverarbeitungen für Analysen (Analytics). Diese Stufe ist für Speicher verfügbar, der in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) mit einer Größe von bis zu 4 TB bereitgestellt wird.

Bis zu 48.000 E/A-Operationen pro Sekunde sind mit einem Endurance-Datenträger in der Größe von 12 TB verfügbar.


Die Auswahl der richtigen Stufe (Tier) von Endurance {{site.data.keyword.filestorage_short}} für Ihre Workload spielt eine wichtige Rolle. Ebenso wichtig ist die Verwendung der Blockgröße, der Ethernet-Verbindungsgeschwindigkeit sowie der Anzahl der Hosts, die zur Erzielung der maximalen Leistung erforderlich sind. Wenn einer dieser Aspekte nicht mit den anderen im Einklang steht, kann dies erhebliche Auswirkungen auf den resultierenden Durchsatz haben.
 
### Performance

Performance ist eine Klasse von {{site.data.keyword.filestorage_full}}, die auf die Unterstützung von Anwendungen mit hohen E/A-Raten mit bekannten Leistungsanforderungen ausgerichtet ist, die sich nicht geeignet mit einer Endurance-Stufe darstellen lassen. Eine vorhersagbare Leistung wird durch die Zuordnung einer IOPS-Kapazität auf Protokollebene zu einzelnen Datenträgern erzielt. IOPS-Kapazitäten von 100 bis 6.000 können mit Speichergrößen von 20 GB bis 12 TB bereitgestellt werden. 

Der Zugriff auf Performance for {{site.data.keyword.filestorage_short}} und das Anhängen erfolgen über eine NFS-Verbindung (NFS - Network File System). {{site.data.keyword.filestorage_short}} wird in der Regel dann verwendet, wenn Zugriffe auf den Datenträger durch mehrere Maschinen gleichzeitig stattfinden. Konsistente Performance-Datenträger können nach den Größen und E/A-Operationen pro Sekunde (IOPS) in Tabelle 1 bestellt und mit Linux-Betriebssystemen verwendet werden.

<table cellpadding="1" cellspacing="1" style="width: 99%;">
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <tbody>
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
            <td>6.000 oder 20,000<sup><img src="/images/numberone.png" alt="Fußnote" /></sup></td>
          </tr>
          <tr>
            <td>2.000-3.000</td>
            <td>200</td>
            <td>6.000 oder 40.000<sup><img src="/images/numberone.png" alt="Fußnote" /></sup></td>
          </tr>
          <tr>
            <td>4.000-7.000</td>
            <td>300</td>
            <td>6.000 oder 48.000<sup><img src="/images/numberone.png" alt="Fußnote" /></sup></td>
          </tr>
          <tr>
            <td>8.000-9.000</td>
            <td>500</td>
            <td>6.000 oder 48.000<sup><img src="/images/numberone.png" alt="Fußnote" /></sup></td>
          </tr>
          <tr>
            <td>10.000-12.000</td>
            <td>1.000</td>
            <td>6.000 oder 48.000<sup><img src="/images/numberone.png" alt="Fußnote" /></sup></td>
          </tr>
        </tbody>
</table>

<sup>![Fußnote](/images/numberone.png)</sup>  Mehr als 6.000 E/A-Operationen pro Sekunde (IOPS) sind in ausgewählten Rechenzentren verfügbar.


Performance-Datenträger wurden dazu entwickelt, eine konsistente Leistung nahe am bereitgestellten IOPS-Niveau zu liefern. Die Konsistenz erleichtert die Dimensionierung und Skalierung von Anwendungsumgebungen auf einem bestimmten Leistungsniveau. Darüber hinaus ist es bei dem gegebenen Bereich von Datenträgergrößen und IOPS-Werten möglich, eine Umgebung durch die Erstellung eines Datenträgers mit idealem Preis-/Leistungsverhältnis zu optimieren.

E/A-Operationen pro Sekunde (IOPS) werden für Endurance und Performance auf Basis einer Blockgröße von 16 KB mit einem 50:50-Mix aus Schreib- und Leseoperationen gemessen. Zur Erzielung der maximalen IOPS-Kapazität auf einem Datenträger müssen dementsprechend geeignete Netzressourcen verwendet werden. Weitere Aspekte sind die private Netznutzung außerhalb von Speicher sowie hostseitige und anwendungsspezifische Optimierungen (IP-Stack, Warteschlangenlängen usw.). 

## Tipps zur Bereitstellung von IOPS für {{site.data.keyword.filestorage_short}}

IOPS für Endurance und Performance basieren auf einer Blockgröße von 16 KB und einer 50-prozentigen Zufallsworkload aus einem 50:50-Mix von Schreib- und Leseoperationen. Ein ~16-KB-Block entspricht einer Schreiboperation auf dem Datenträger.

Die Blockgröße, die von Ihrer Anwendung verwendet wird, hat eine direkte Auswirkung auf die Speicherleistung. Wenn die von Ihrer Anwendung verwendete Blockgröße kleiner als 16 KB ist, wird die IOPS-Grenze vor der Durchsatzbegrenzung erreicht. Umgekehrt, wenn die von Ihrer Anwendung verwendete Blockgröße größer als 15 KB ist, wird die Durchsatzbegrenzung vor der IOPS-Grenze erreicht.

Eine Änderung der Blockgröße wirkt sich wie folgt auf die Leistung aus:

<table cellpadding="1" cellspacing="1" style="width: 99%;">
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <tbody>
          <tr>
            <th>Blockgröße (KB)</th>
            <th>IOPS</th>
            <th>Durchsatz (MB/s)</th>
          </tr>
          <tr>
            <td>4 (typisch für Linux)</td>
            <td>1.000</td>
            <td>4</td>
          </tr>
          <tr>
            <td>8 (typisch für Oracle)</td>
            <td>1.000</td>
            <td>8</td>
          </tr>
          <tr>
            <td>16</td>
            <td>1.000</td>
            <td>16</td>
          </tr>
          <tr>
            <td>32 (typisch für SQLServer)</td>
            <td>500</td>
            <td>16</td>
          </tr>          
          <tr>
            <td>64</td>
            <td>250</td>
            <td>16</td>
          </tr>
          <tr>
            <td>128</td>
            <td>128</td>
            <td>16</td>
          </tr>
          <tr>
            <td>512</td>
            <td>32</td>
            <td>16</td>
          </tr>
        </tbody>
</table>

Die richtige Auswahl von {{site.data.keyword.blockstorageshort}}-Speicher für Ihre Workload ist wichtig, ebenso wichtig wie die Vermeidung von Engpässen. Die Geschwindigkeit Ihrer Ethernet-Verbindung muss höher als der erwartete maximale Durchsatz von Ihrem Datenträger sein. Als allgemeine Regel gilt, dass Ihre Ethernet-Verbindung nicht über 70% der verfügbaren Bandbreite hinaus ausgelastet werden sollte. Wenn Sie zum Beispiel 6.000 IOPS haben und eine Blockgröße von 16 KB verwenden, kann der Datenträger annähernd 94 MB pro Sekunde leisten. Wenn Sie eine 1-Gb/s-Ethernet-Verbindung zu Ihrer LUN haben, wird diese zu einem Engpass, wenn Ihre Server versuchen, den maximal verfügbaren Durchsatz zu nutzen, weil 70% der theoretischen Begrenzung einer 1-Gb/s-Ethernet-Verbindung (125 MB pro Sekunde) nur 88 MB pro Sekunde zulassen würden.


Ein weitere Faktor, der zu beachten ist, ist die Anzahl der Hosts, die Ihren Datenträger nutzen. Wenn ein einzelner Host vorhanden ist, der auf den Datenträger zugreift, kann es schwierig sein, die maximal verfügbare IOPS-Kapazität zu erreichen, insbesondere bei sehr hohen IOPS-Werten (10.000). Wenn Ihre Workload einen hohen Durchsatz erfordert, wäre es am besten, mindestens zwei oder drei Server für den Zugriff auf Ihren Datenträger zu konfigurieren, um den Engpass eines einzelnen Servers zu vermeiden.


Zur Erzielung der maximalen E/A-Operationen pro Sekunde müssen geeignete Netzressourcen eingesetzt werden. Weitere Aspekte sind die private Netznutzung außerhalb von Speicher sowie hostseitige und anwendungsspezifische Optimierungen (IP-Stack, Warteschlangenlängen usw.).

NFS Version 3 und NFS Version 4.1 werden in der Umgebung von {{site.data.keyword.BluSoftlayer_full}} unterstützt. Es wird jedoch empfohlen, NFS v3 zu verwenden. NFS Version 4.1 ist ein Protokoll mit Zustandsüberwachung (und nicht wie NFSv3 ohne Zustandsüberwachung), sodass bei Netzereignissen Probleme auftreten können. NFS Version 4.1 muss Operationen ruhen lassen und eine Sperrenrückforderung ausführen. Auf einem relativ ausgelasteten NFS-Dateiserver kann die erhöhte Latenz zu Unterbrechungen führen. Die fehlende Multipath/Trunking-Funktionalität in NFS v4.1 kann darüber hinaus die Wiederherstellung des NFS-Betriebs verlängern.
