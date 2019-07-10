---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-01"

keywords: File Storage, encryption, security, provisioning, limitations, NFS

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:faq: data-hd-content-type='faq'}

# Häufig gestellte Fragen
{: #file-storage-faqs}

## Wie lässt sich erkennen, welche der {{site.data.keyword.filestorage_short}}-Datenträger verschlüsselt sind?
{: faq}

Zeigen Sie die Liste Ihrer {{site.data.keyword.filestorage_short}}-Instanzen im Kundenportal an. Bei verschlüsselten Datenträgern steht rechts vom Datenträgernamen ein Sperrsymbol.

## Kann ich meinen {{site.data.keyword.filestorage_short}}-Speicher verschlüsseln, wenn ich nicht verschlüsselten {{site.data.keyword.filestorage_short}}-Speicher in einem Rechenzentrum erworben habe, das für die Verschlüsselung aktualisiert wurde?
{: faq}

{{site.data.keyword.filestorage_short}}-Speicher, der vor der Aktualisierung eines Rechenzentrums bereitgestellt wurde, kann nicht verschlüsselt werden. Neuer {{site.data.keyword.filestorage_short}}, der in aktualisierten Rechenzentren bereitgestellt wird, wird automatisch verschlüsselt. Dies ist automatisch, keine Bereitstellungseinstellung, die ausgewählt oder weggelassen werden kann. Daten im nicht verschlüsselten Speicher können verschlüsselt werden, indem ein neuer Datenträger erstellt wird und die Daten mithilfe der hostbasierten Migration auf den neuen verschlüsselten Datenträger kopiert werden. Weitere Informationen finden Sie im Abschnitt [File Storage migrieren](/docs/infrastructure/FileStorage?topic=FileStorage-migratestorage).

## Wie lässt sich erkennen, ob {{site.data.keyword.filestorage_short}} in einem aktualisierten Rechenzentrum bereitgestellt wird?
{: faq}

Im {{site.data.keyword.filestorage_short}}-Bestellformular sind alle aktualisierten Rechenzentren mit einem Stern (`*`) gekennzeichnet. Während des Bestellablaufs erhalten Sie eine Meldung des Inhalts, dass Sie Speicher mit Verschlüsselung erwerben. Wird der Speicher bereitgestellt, gibt ein Symbol in der Speicherliste an, dass der Datenträger verschlüsselt ist.

Alle verschlüsselten Datenträger und gemeinsam genutzten Dateispeicher werden nur in aktualisierten Rechenzentren bereitgestellt. Eine vollständige Liste der aktualisierten Rechenzentren und der verfügbaren Features finden Sie [hier](/docs/infrastructure/FileStorage?topic=FileStorage-selectDC).

## Warum kann {{site.data.keyword.filestorage_short}} mit einer Endurance-Stufe von 10 IOPS in einigen Rechenzentren bereitgestellt werden, in anderen jedoch nicht?
{: faq}

Der {{site.data.keyword.filestorage_short}}-Typ Endurance mit der 10 IOPS/GB-Stufe ist nur in ausgewählten Rechenzentren verfügbar. Weitere Rechenzentren werden in Kürze hinzugefügt. Eine vollständige Liste der aktualisierten Rechenzentren und der verfügbaren Features finden Sie [hier](/docs/infrastructure/FileStorage?topic=FileStorage-selectDC).

## Wie wird der richtige Mountpunkt für den eigenen {{site.data.keyword.filestorage_short}}-Speicher ermittelt?
{: faq}

Alle verschlüsselten {{site.data.keyword.filestorage_short}}-Datenträger, die in den erweiterten Rechenzentren bereitgestellt werden, haben einen anderen Mountpunkt als nicht verschlüsselte Datenträger. Um sicherzustellen, dass Sie den richtigen Mountpunkt verwenden, zeigen Sie die Mountpunktinformationen auf der Seite **Datenträgerdetails** in der Benutzerschnittstelle an. Sie können auch über einen API-Aufruf auf den richtigen Mountpunkt zugreifen: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

## Wie viele Datenträger können bereitgestellt werden?
{: faq}

Sie können standardmäßig eine kombinierte Gesamtzahl von 250 Block- und Dateispeicherdatenträger bereitstellen. Wenden Sie sich an Ihren Vertriebsbeauftragten, wenn Sie Ihren Grenzwert erhöhen möchten. Weitere Informationen finden Sie in [Speichergrenzwerte verwalten](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits).

## Wie viele Instanzen können einen bereitgestellten {{site.data.keyword.filestorage_short}}-Datenträger gemeinsam nutzen?
{: faq}

Die Standardbegrenzung für die Anzahl von Autorisierungen pro Datenträger ist 64. Wenden Sie sich an Ihren Vertriebsbeauftragten, wenn Sie diesen Grenzwert erhöhen möchten.

## Wie viele {{site.data.keyword.filestorage_short}}-Datenträger können einem einzelnen Host zugeordnet werden?
{: faq}

Dies ist abhängig von der Kapazität des Hostbetriebssystems, nicht von {{site.data.keyword.cloud}}-Grenzwerten. Die Dokumentation des jeweiligen Betriebssystems enthält Informationen zu den Grenzwerten für die Anzahl der gemeinsam genutzten Dateisysteme, die angehängt werden können.

## Wie viele gemeinsam genutzte Dateisysteme sind pro Dateidatenträgergröße zulässig? Wie viele gemeinsam genutzte Dateisysteme sind pro Datenträgergröße maximal zulässig?
{: #maxfilevolume}
{: faq}

| Datenträgergröße | I-Nodes und gemeinsam genutzte Dateiressourcen |
|-----|-----|
| 20 GB - 39 GB | 622.484 |
| 40 GB - 79 GB | 1.245.084 |
| 80 GB - 99 GB | 2.490.263 |
| 100 GB - 249 GB | 3.112.863 |
| 250 GB - 499 GB | 7.782.300 |
| 500 GB - 999 GB | 15.564.695 |
| 1 TB | 31.876.593 |
| 2 TB | 63.753.186 |
| 3 TB | 95.629.970 |
| 4 TB | 127.506.359 |
{: row-headers}
{: class="comparison-table"}
{: caption="Tabellenvergleich" caption-side="top"}
{: summary="Table 1 shows the maximum number of inodes that are allowed based on the volume size. Volume sizes are in the left column. The number of inodes and file shares are on the right."}

## IOPS messen
{: faq}

E/A-Operationen pro Sekunde werden auf der Basis einen Belastungsprofils von 16-KB-Blöcken mit 50 Prozent Zufallsleseoperationen und 50 Prozent Zufallsschreiboperationen gemessen. Workloads, die von diesem Profil abweichen, erreichen möglicherweise eine schlechte Leistung.

## Was geschieht, wenn die Leistung mit einer kleineren Blockgröße gemessen wird?
{: faq}

Ein Maximum an E/A-Operationen pro Sekunde kann auch bei Verwendung kleinerer Blockgrößen erreicht werden. Allerdings wird der Durchsatz in diesem Fall geringer. Ein Datenträger mit 6000 IOPS hat bei verschiedenen Blockgrößen zum Beispiel den folgenden Durchsatz:

- 16 KB * 6000 IOPS == ~93,75 MB/Sek
- 8 KB * 6000 IOPS == ~46,88 MB/Sek
- 4 KB * 6000 IOPS == ~23,44 MB/Sek


## Werden die zugeordneten E/A-Operationen pro Sekunde auf Instanz- oder Datenträgerebene umgesetzt?
{: faq}

Die E/A-Operationen pro Sekunde (IOPS) werden auf Datenträgerebene umgesetzt. Mit anderen Worten, zwei Hosts, die mit einem Datenträger mit 6000 IOPS verbunden sind, nutzen diese 6000 IOPS gemeinsam.

## Benötigt der Datenträger zuvor eine Einlaufphase, um den erwarteten Durchsatz zu erzielen?
{: faq}

Ein Einlaufen des Datenträgers ist nicht erforderlich. Sie verfügen sofort nach Bereitstellung des Datenträgers über den angegebenen Durchsatz.

## Kann ein höherer Durchsatz erzielt werden, wenn eine schnellere Ethernet-Verbindung verwendet wird?
{: faq}

Durchsatzgrenzwerte werden auf Datenträgerebene festgelegt. Dieser Grenzwert kann durch eine schnellere Ethernet-Verbindung nicht erhöht werden. Bei einer langsameren Ethernet-Verbindung kann Ihre Bandbreite allerdings einen Engpass verursachen.

## Beeinträchtigen Firewalls und Sicherheitsgruppen die Leistung?
{: #isolatedstoragetraffic}
{: faq}

Es ist am besten, den Speicherdatenverkehr über ein VLAN zu leiten, das die Firewall umgeht. Wenn der Speicherdatenverkehr über Software-Firewalls geleitet wird, erhöht sich dadurch die Latenz und die Speicherleistung wird beeinträchtigt.

## Welche Leistungslatenz ist von {{site.data.keyword.filestorage_short}}-Speicher zu erwarten?   
{: faq}

Die Ziellatenz im Speicher beträgt weniger als 1 ms. File Storage-Speicher wird mit Compute-Instanzen in einem gemeinsam genutzten Netz verbunden, sodass die genaue Leistungslatenz vom Netzverkehr während der Operation abhängig ist.

## Was geschieht mit den Daten, wenn {{site.data.keyword.filestorage_short}}-Datenträger gelöscht werden?
{: faq}

{{site.data.keyword.filestorage_full}} stellt den Kunden gemeinsam genutzte Dateien auf physischem Speicher bereit, der vor der Wiederverwendung bereinigt wird. Kunden mit bestimmten Compliance-Anforderungen, z. B. hinsichtlich der Einhaltung der NIST 800-88-Richtlinien für das sichere Löschen von Datenträgern, müssen die entsprechende Bereinigungsprozedur durchführen, bevor sie den Speicher löschen.

## Welche NFS-Versionen werden unterstützt?
{: faq}

NFS Version 3 und NFS Version 4.1 werden in der Umgebung von {{site.data.keyword.cloud}} unterstützt.

Die bevorzugte Version ist NFS v3, da sie ein statusunabhängiges Protokoll verwendet und bei Netzereignissen eine größere Ausfallsicherheit bietet.

NFS v3 bietet native Unterstützung für `no_root_squash`; dies ermöglicht es Root-Clients, die Rootberechtigungen für den gemeinsam genutzten NFS-Speichern zu behalten. Sie können dieses Feature in NFS v4.1 aktivieren, indem Sie die Domäneninformationen bearbeiten und `rpcidmapd` oder einen ähnlichen Service ausführen. Weitere Informationen finden Sie in ['no_root_squash' für NFS implementieren](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux#norootsquash).

In Bezug auf vSphere-Lösungen unterstützt NFS v3 eine größere Anzahl von Features als v4.1. Zu diesen Features gehören z. B. Storage DRS und Site Recovery Manager.

## Kann VAAI- und Hardwarebeschleunigung in den eigenen VMware-Bereitstellungen aktiviert werden?
{: #isVAAIsupported}
{: faq}

Nein. Momentan wird die VAAI- und Hardwarebeschleunigung (VAAI = vStorage for API Array Integration) nicht unterstützt.

## Was passiert mit den Laufwerken, die über das Cloud-Rechenzentrum außer Betrieb gesetzt werden?
{: faq}

Wenn Laufwerke außer Betrieb gesetzt werden, werden sie vor dem Entsorgen durch IBM zerstört. Die Laufwerke werden unbrauchbar. Auf Daten, die auf diesem Laufwerk gespeichert waren, kann nicht mehr zugegriffen werden.
