---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-02"

keywords: File Storage, file storage, NFS, provisioning, setup, configuration, mounting storage

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
 {:shortdesc: .shortdesc}


# Lernprogramm zur Einführung
{: #getting-started}

{{site.data.keyword.filestorage_full}} ist ein persistenter, schneller und flexibler, NFS-basierter {{site.data.keyword.filestorage_short}}, der über ein Netz angeschlossen ist. In dieser Umgebung mit NAS-Speicher (NAS – Network-attached Storage) haben Sie vollständige Kontrolle über die Funktion und Leistung Ihrer gemeinsam genutzten Dateispeicher. Gemeinsam genutzte {{site.data.keyword.filestorage_short}}-Speicher können aus Gründen der Ausfallsicherheit mit bis zu 64 autorisierten Geräten über gesteuerte TCP/IP-Verbindungen verbunden werden.
{:shortdesc}

## Vorbereitungen
{: #prereqs}

{{site.data.keyword.filestorage_short}}-Datenträger können von 20 GB bis 12 TB mit zwei Optionen bereitgestellt werden: <br/>
- Sie können **Endurance**-Stufen bereitstellen, die über vordefinierte Leistungsstufen und andere Funktionen wie Snapshots und Replikation verfügen.
- Sie können eine hochleistungsfähige **Performance-Umgebung** mit einer zugeordneten Kapazität an E/A-Operationen pro Sekunde (IOPS) erstellen.

Weitere Informationen zum {{site.data.keyword.filestorage_short}}-Angebot finden Sie in [Informationen zu {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-about).

## Hinweise zur Bereitstellung

### Blockgröße

IOPS für Endurance und Performance basiert auf einer Blockgröße von 16 KB mit einer 50/50-Lese-/Schreibworkload mit 50/50 zufälliger/sequenqzieller Verarbeitung. Ein 16-KB-Block entspricht einem Schreibvorgang auf dem Datenträger.
{:important}

Die Blockgröße, die von Ihrer Anwendung verwendet wird, wirkt sich direkt auf die Speicherleistung aus. Wenn die von Ihrer Anwendung verwendete Blockgröße kleiner als 16 KB ist, wird der IOPS-Grenzwert vor der Durchsatzbegrenzung erreicht. Wenn dagegen die von Ihrer Anwendung verwendete Blockgröße größer als 16 KB ist, wird die Durchsatzbegrenzung vor dem IOPS-Grenzwert erreicht.

<table>
  <caption>Tabelle 4 enthält Beispiele für die Auswirkung von Blockgröße und IOPS auf den Durchsatz.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <thead>
          <tr>
            <th>Blockgröße (KB)</th>
            <th>IOPS</th>
            <th>Durchsatz (MB/s)</th>
          </tr>
        </thead>
        <tbody>
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
            <td>32 (typisch für SQL Server)</td>
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
          <tr>
            <td>1024</td>
            <td>16</td>
            <td>16</td>
          </tr>
        </tbody>
</table>

### Autorisierte Hosts

Ein weitere Faktor, der zu beachten ist, ist die Anzahl der Hosts, die Ihren Datenträger nutzen. Wenn ein einzelner Host vorhanden ist, der auf den Datenträger zugreift, kann es schwierig sein, die maximal verfügbare IOPS-Kapazität zu erreichen, insbesondere bei sehr hohen IOPS-Werten (10.000). Wenn Ihre Workload einen hohen Durchsatz erfordert, wäre es am besten, mindestens einige Server für den Zugriff auf Ihren Datenträger zu konfigurieren, um den Engpass eines einzelnen Servers zu vermeiden.

### Netzverbindung

Die Geschwindigkeit Ihrer Ethernet-Verbindung muss höher als der erwartete maximale Durchsatz von Ihrem Datenträger sein. Grundsätzlich dürfte Ihre Ethernet-Verbindung nicht über 70% der verfügbaren Bandbreite hinaus ausgelastet werden. Wenn Sie beispielsweise über 6.000 IOPS verfügen und eine Blockgröße von 16 KB verwenden, sind auf dem Datenträger etwa 94 MBps möglich. Bei einer Ethernet-Verbindung von 1 Gb/s zu einer LUN wird diese Verbindung zu einem Engpass, wenn die Server versuchen, den maximal verfügbaren Durchsatz zu nutzen. Ursache hierfür ist, dass 70 Prozent des theoretischen Grenzwerts von einer Ethernet-Verbindung mit 1 Gb/s (125 MB pro Sekunde) nur 88 MB pro Sekunde zulassen würden.

Zur Erzielung der maximalen E/A-Operationen pro Sekunde müssen geeignete Netzressourcen eingesetzt werden. Außerdem sind die Nutzung privater Netze außerhalb des Speichers sowie hostseitige und anwendungsspezifische Optimierungen (zum Beispiel IP-Stack oder [Warteschlangenlängen](/docs/infrastructure/FileStorage?topic=FileStorage-hostqueuesettings) und andere Einstellungen) zu berücksichtigen.

Der Speicherdatenverkehr ist in der gesamten Netznutzung von öffentlichen virtuellen Servern enthalten. Weitere Informationen zu den Grenzwerten, die für die Verwendung des Service gelten können, finden Sie in der [Dokumentation zu virtuellen Servern](/docs/vsi?topic=virtual-servers-about-public-virtual-servers).

### NFS-Version

NFS Version 3 und NFS Version 4.1 werden in der Umgebung von {{site.data.keyword.BluSoftlayer_full}} unterstützt. NFS Version 3 wird jedoch bevorzugt, da NFS Version 4.1 ein Protokoll mit Zustandsüberwachung (und nicht wie NFSv3 ohne Zustandsüberwachung) ist und bei Netzereignissen Probleme mit dem Protokoll auftreten können. NFS Version 4.1 muss alle Operationen ruhen lassen und anschließend eine Sperrenrückforderung ausführen. Auf einem relativ ausgelasteten NFS-Dateiserver kann die erhöhte Latenz zu Unterbrechungen führen. Die fehlende Multipath- und Trunking-Funktionalität in NFS v4.1 kann darüber hinaus die Wiederherstellung des NFS-Betriebs verlängern.

## Bestellung abschicken

Wenn Sie bereit sind, die Bestellung aufzugeben, können Sie dies über die [Konsole](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole) oder die [SLCLI](/docs/infrastructure/FileStorage?topic=FileStorage-orderingSLCLI) tun. Zur Bereitstellung von File Storage mit VMware klicken Sie [hier](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide).

## Verbindung zum neuen Speicher herstellen
{: #mountingstorage}

Wenn Ihre Bereitstellungsanforderung abgeschlossen ist, können Sie Ihren Hosts die Berechtigung erteilen, auf den neuen Speicher zuzugreifen und die Verbindung zu konfigurieren. Folgen Sie je nach dem Betriebssystem Ihres Hosts dem entsprechenden Link.
- [Zugriff auf {{site.data.keyword.filestorage_short}} unter Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [{{site.data.keyword.filestorage_short}} unter CentOS anhängen](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [{{site.data.keyword.filestorage_short}} unter CoreOS anhängen](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)
- [{{site.data.keyword.filestorage_short}} für Sicherung mit cPanel konfigurieren](/docs/infrastructure/FileStorage?topic=FileStorage-cPanelBackups)
- [{{site.data.keyword.filestorage_short}} für Sicherung mit Plesk konfigurieren](/docs/infrastructure/FileStorage?topic=FileStorage-PleskBackup)
- [{{site.data.keyword.filestorage_short}}-Datenträger an ESXi-Hosts anhängen](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)

## Den neuen Speicher verwalten

Über das Portal oder die SLCLI können Sie verschiedene Aspekte Ihrer Instanz von {{site.data.keyword.filestorage_short}} verwalten, wie z. B. Hostberechtigungen und Stornierungen. Weitere Informationen finden Sie in [{{site.data.keyword.filestorage_short}} verwalten](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage).
