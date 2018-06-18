---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-24"

---
{:new_window: target="_blank"}

# {{site.data.keyword.filestorage_short}} - häufig gestellte Fragen

## Wie werden E/A-Operationen pro Sekunde (IOPS) gemessen?

E/A-Operationen pro Sekunde werden auf der Basis einen Belastungsprofils von 16-KB-Blöcken mit 50 Prozent Zufallsleseoperationen und 50 Prozent Zufallsschreiboperationen gemessen. Workloads, die von diesem Profil abweichen, erreichen möglicherweise eine geringere Leistung.

## Was geschieht, wenn die Leistung mit einer kleineren Blockgröße gemessen wird?

Ein Maximum an E/A-Operationen pro Sekunde kann auch bei Verwendung kleinerer Blockgrößen erreicht werden. Allerdings wird der Durchsatz geringer. Ein Datenträger mit 6000 IOPS hätte bei verschiedenen Blockgrößen zum Beispiel den folgenden Durchsatz:

- 16 KB * 6000 IOPS == ~93,75 MB/Sek
- 8 KB * 6000 IOPS == ~46,88 MB/Sek
- 4 KB * 6000 IOPS == ~23,44 MB/Sek


## Benötigt der Datenträger zuvor eine Einlaufphase, um den erwarteten Durchsatz zu erzielen?

Ein Einlaufen des Datenträgers ist nicht erforderlich. Sie verfügen sofort nach Bereitstellung des Datenträgers über den angegebenen Durchsatz.

## Wie lässt sich erkennen, welche der {{site.data.keyword.filestorage_short}}-Datenträger verschlüsselt sind?

Zeigen Sie die Liste Ihrer {{site.data.keyword.filestorage_short}}-Instanzen im Kundenportal an. Bei verschlüsselten Datenträgern steht rechts vom LUN-/Datenträgernamen ein Sperrsymbol.

## Kann ich meinen {{site.data.keyword.filestorage_short}}-Speicher verschlüsseln, wenn ich nicht verschlüsselten {{site.data.keyword.filestorage_short}}-Speicher in einem Rechenzentrum erworben habe, das für die Verschlüsselung aktualisiert wurde?

{{site.data.keyword.filestorage_short}}-Speicher, der vor der Aktualisierung eines Rechenzentrums bereitgestellt wurde, kann nicht verschlüsselt werden. Neuer {{site.data.keyword.filestorage_short}}-Speicher, der in aktualisierten Rechenzentren bereitgestellt wird, ist automatisch verschlüsselt. Es gibt keine auswählbare Einstellung für die Verschlüsselung, sie erfolgt automatisch. Daten im nicht verschlüsselten Speicher können verschlüsselt werden, indem ein neuer Datenträger erstellt wird und die Daten mithilfe der hostbasierten Migration auf den neuen verschlüsselten Datenträger kopiert werden. Anweisungen zur Ausführung der Migration finden Sie in [diesem Artikel](/docs/infrastructure/FileStorage/migrate-file-storage-encrypted-file-storage.html).

## Wie lässt sich erkennen, ob {{site.data.keyword.filestorage_short}} in einem aktualisierten Rechenzentrum bereitgestellt wird?

Im {{site.data.keyword.filestorage_short}}-Bestellformular sind alle aktualisierten Rechenzentren mit einem Stern (`*`) gekennzeichnet. Während des Bestellablaufs erhalten Sie eine Meldung des Inhalts, dass Sie Speicher mit Verschlüsselung erwerben. Wird der Speicher bereitgestellt, gibt ein Symbol in der Speicherliste an, dass der Datenträger verschlüsselt ist. 

Alle verschlüsselten Datenträger und Dateifreigaben werden nur in aktualisierten Rechenzentren bereitgestellt. Eine vollständige Liste der aktualisierten Rechenzentren und der verfügbaren Features finden Sie [hier](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html).

## Warum kann {{site.data.keyword.filestorage_short}} mit einer Endurance-Stufe von 10 IOPS in einigen Rechenzentren bereitgestellt werden, in anderen jedoch nicht?

Der {{site.data.keyword.filestorage_short}}-Typ Endurance mit der 10 IOPS/GB-Stufe ist nur in ausgewählten Rechenzentren verfügbar. Weitere Rechenzentren werden in Kürze hinzugefügt. Eine vollständige Liste der aktualisierten Rechenzentren und der verfügbaren Features finden Sie [hier](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html).

## Wie wird der richtige Mountpunkt für den eigenen {{site.data.keyword.filestorage_short}}-Speicher ermittelt?

Alle verschlüsselten {{site.data.keyword.filestorage_short}}-Datenträger, die in den erweiterten Rechenzentren bereitgestellt werden, haben einen anderen Mountpunkt als nicht verschlüsselte Datenträger. Um sicherzustellen, dass Sie den richtigen Mountpunkt verwenden, zeigen Sie die Mountpunktinformationen auf der Seite **Datenträgerdetails** in der Benutzerschnittstelle an. Sie können auch über einen API-Aufruf auf den richtigen Mountpunkt zugreifen: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

## Wie viele Dateifreigaben sind pro Dateidatenträgergröße zulässig? Welche maximalen Anzahlen von Dateifreigaben sind pro Datenträgergröße zulässig?

<table>
  <caption>Tabelle 1 zeigt die maximale Anzahl von zulässigen I-Nodes auf der Basis der Datenträgergröße an. Die Datenträgergrößen stehen auf der linken Seite. Die Anzahl der I-Nodes/Dateifreigaben steht auf der rechten Seite.</caption>
  <thead>
    <tr>
      <th>Datenträgergröße</th>
      <th>I-Nodes/Dateifreigaben</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>20 GB </td>
      <td>622.484</td>
    </tr>
    <tr>
      <td>40 GB </td>
      <td>1.245.084</td>
    </tr>          
    <tr>
      <td>80 GB</td>
      <td>2.490.263</td>
    </tr>          
    <tr>
      <td>100 GB</td>
      <td>3.112.863</td>
    </tr>          
    <tr>
      <td>250 GB</td>
      <td>7.782.300</td>
    </tr>          
    <tr>
      <td>500 GB</td>
      <td>15.564.695</td>
    </tr>
    <tr>
      <td>1 TB+</td>
      <td>31.876.593</td>
    </tr>
   </tbody>
</table>

## Wie viele Datenträger können bereitgestellt werden?

Sie können standardmäßig eine kombinierte Gesamtzahl von 250 Block- und Dateispeicherdatenträger bereitstellen. Wenden Sie sich an Ihren Vertriebsbeauftragten, wenn Sie Ihren Grenzwert erhöhen möchten.

## Wie viele Instanzen können einen bereitgestellten {{site.data.keyword.filestorage_short}}-Datenträger gemeinsam nutzen?

Die Standardbegrenzung für die Anzahl von Autorisierungen pro Datenträger ist 64. Wenden Sie sich an Ihren Vertriebsbeauftragten, wenn Sie diesen Grenzwert erhöhen möchten.

## Werden bei einer Bereitstellung von Performance oder Endurance {{site.data.keyword.filestorage_short}} die zugeordneten E/A-Operationen pro Sekunde auf Instanz- oder Datenträgerebene umgesetzt?

Die E/A-Operationen pro Sekunde (IOPS) werden auf Datenträgerebene umgesetzt. Mit anderen Worten, zwei Hosts, die mit einem Datenträger mit 6000 IOPS verbunden sind, nutzen diese 6000 IOPS gemeinsam.

## Lässt sich durch die Verwendung einer schnelleren Ethernet-Verbindung ein höherer Durchsatz erzielen?

Durchsatzbegrenzungen werden auf Datenträger- bzw. LUN-Ebene festgelegt. Daher führt die Verwendung einer schnelleren Ethernet-Verbindung nicht zu einer Erhöhung dieser Begrenzung. Bei einer langsameren Ethernet-Verbindung kann Ihre Bandbreite allerdings einen Engpass verursachen.

## Wirken sich Firewalls/Sicherheitsgruppen auf die Leistung aus?

Als bewährtes Verfahren wird empfohlen, den Speicherdatenverkehr über ein VLAN zu leiten, das die Firewall umgeht. Wenn der Speicherdatenverkehr über Software-Firewalls geleitet wird, erhöht sich dadurch die Latenz und die Speicherleistung wird beeinträchtigt.

## Welche Leistungslatenz ist von {{site.data.keyword.filestorage_short}}-Speicher zu erwarten?   

Die Ziellatenz innerhalb des Speichers beträgt < 1 ms. File Storage-Speicher wird mit Compute-Instanzen in einem gemeinsam genutzten Netz verbunden, sodass die genaue Leistungslatenz vom Netzverkehr in einem bestimmten Zeitrahmen abhängig ist.

## Was geschieht mit den Daten, wenn {{site.data.keyword.filestorage_short}}-Datenträger gelöscht werden?

Wenn Speicher gelöscht wird, werden alle Verweise auf die Daten in diesem Datenträger entfernt, sodass die Daten völlig unzugänglich werden. Wenn der physische Speicher erneut für ein anderes Konto bereitgestellt wird, wird eine neue Gruppe von Verweisen zugeordnet. Es besteht keine Möglichkeit für das neue Konto, auf Daten zuzugreifen, die sich möglicherweise einmal auf dem physischen Speicher befunden haben. Die neue Gruppe von Verweisen zeigt ausschließlich Nullen (0) an. Wenn neue Daten auf den Datenträger/die LUN geschrieben werden, werden alle unzugänglichen Daten, die noch vorhanden sein könnten, überschrieben.

## Was passiert mit den Laufwerken, die über das Cloud-Rechenzentrum außer Betrieb gesetzt werden?

Wenn Laufwerke außer Betrieb gesetzt werden, werden sie vor dem Entsorgen durch IBM zerstört, sodass sie nicht mehr verwendet werden können. Auf Daten, die auf diesem Laufwerk gespeichert waren, kann nicht mehr zugegriffen werden.
