---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-02"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.filestorage_short}} - Häufig gestellte Fragen (FAQ)

## Wie werden E/A-Operationen pro Sekunde (IOPS) gemessen?

E/A-Operationen pro Sekunde werden auf der Basis einen Belastungsprofils von 16-KB-Blöcken mit 50% Zufallsleseoperationen und 50% Zufallsschreiboperationen gemessen. Workloads, die von diesem Profil abweichen, erreichen möglicherweise eine geringere Leistung.

## Was geschieht, wenn die Leistung mit einer kleineren Blockgröße gemessen wird?

Die maximale Anzahl von E/A-Operationen pro Sekunde kann auch bei kleineren Blockgrößen erreicht werden, jedoch ist der Durchsatz niedriger. Ein Datenträger mit 6000 IOPS hätte bei verschiedenen Blockgrößen zum Beispiel den folgenden Durchsatz:

- 16 KB * 6000 IOPS == ~93,75 MB/Sek
- 8 KB * 6000 IOPS == ~46,88 MB/Sek
- 4 KB * 6000 IOPS == ~23,44 MB/Sek


## Benötigt der Datenträger zuvor eine Einlaufphase, um den erwarteten Durchsatz zu erzielen?

Ein Einlaufen des Datenträgers ist nicht erforderlich. Sie verfügen über den angegebenen Durchsatz sofort nach Bereitstellung des Datenträgers.

## Wie lässt sich erkennen, welche der {{site.data.keyword.filestorage_short}}-LUNs/Datenträger verschlüsselt sind?

Wenn Sie Ihre Liste von {{site.data.keyword.filestorage_short}}-Datenträgern im Kundenportal anzeigen, sehen Sie ein Sperrsymbol rechts neben dem LUN-/Datenträgernamen für die Einheiten, die verschlüsselt sind.

## Wenn nicht verschlüsselter {{site.data.keyword.filestorage_short}}-Speicher vorhanden ist, der in einem Rechenzentrum bereitgestellt wurde, das für Verschlüsselung aktualisiert wurde, kann der {{site.data.keyword.filestorage_short}}-Speicher verschlüsselt werden?

{{site.data.keyword.filestorage_short}}-Speicher, der vor der Aktualisierung des Rechenzentrums bereitgestellt wurde, kann nicht verschlüsselt werden. Neuer {{site.data.keyword.filestorage_short}}-Speicher, der in aktualisierten Rechenzentren bereitgestellt wird, wird automatisch verschlüsselt; es gibt keine auswählbare Einstellung, alles erfolgt automatisch. Daten auf nicht verschlüsseltem Speicher in einem aktualisierten Rechenzentrum können verschlüsselt werden. Dazu müssen ein neuer Dateidatenträger erstellt und die Daten dann auf den neuen verschlüsselten Datenträger oder den Datenträger mit hostbasierter Migration kopiert werden. Anweisungen zur Durchführung der Migration finden Sie in [diesem Artikel](/docs/infrastructure/FileStorage/migrate-file-storage-encrypted-file-storage.html).

## Wie lässt sich erkennen, dass {{site.data.keyword.filestorage_short}} in einem aktualisierten Rechenzentrum bereitgestellt wird?

Bei der Bereitstellung von {{site.data.keyword.filestorage_short}}-Speicher werden alle aktualisierten Rechenzentren mit einem Stern (`*`) im Bestellformular gekennzeichnet und ein Hinweis angezeigt, dass Sie Speicher mit Verschlüsselung bereitstellen. Sobald der Speicher bereitgestellt ist, wird ein Symbol in der Speicherliste angezeigt, das diesen Datenträger oder andere Datenträger als verschlüsselt kennzeichnet. Alle verschlüsselten Datenträger und anderen Datenträger werden nur in aktualisierten Rechenzentren bereitgestellt. Eine vollständige Liste der aktualisierten Rechenzentren und der verfügbaren Features finden Sie [hier](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html).

## Warum kann {{site.data.keyword.filestorage_short}} mit einer Endurance-Stufe von 10 IOPS in einigen Rechenzentren bereitgestellt werden, in anderen jedoch nicht?

Der {{site.data.keyword.filestorage_short}}-Typ Endurance mit der 10 IOPS/GB-Stufe ist nur in ausgewählten Rechenzentren verfügbar. Weitere Rechenzentren werden in Kürze hinzugefügt. Eine vollständige Liste der aktualisierten Rechenzentren und der verfügbaren Features finden Sie [hier](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html).

## Wie wird der richtige Mountpunkt für den eigenen {{site.data.keyword.filestorage_short}}-Speicher ermittelt?

Alle verschlüsselten {{site.data.keyword.filestorage_short}}-Datenträger, die in diesen Rechenzentren bereitgestellt werden, haben einen anderen Mountpunkt als nicht verschlüsselte Datenträger. Um sicherzustellen, dass Sie den richtigen Mountpunkt für Ihre verschlüsselten und nicht verschlüsselten {{site.data.keyword.filestorage_short}}-Datenträger verwenden, können Sie die Mountpunktinformationen auf der Seite **Datenträgerdetails** in der Benutzerschnittstelle abrufen und auf den richtigen Mountpunkt durch einen API-Aufruf zugreifen: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

## Wie viele Dateifreigaben sind pro Dateidatenträgergröße zulässig? Welche maximalen Anzahlen von Dateifreigaben sind pro Datenträgergröße zulässig?
In der folgenden Tabelle sind die maximalen Anzahlen von Dateidescriptoren (I-Nodes) oder Dateifreigaben nach Datenträgergröße aufgeführt:

<table>
        <tbody>
          <tr>
            <th>Datenträgergröße</th>
            <th>Dateidescriptoren (I-Nodes)/Dateien</th>
          </tr>
          <tr>
            <td>20 GB</td>
            <td>622.484</td>
          </tr>
          <tr>
            <td>40 GB</td>
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

Sie können standardmäßig eine kombinierte Gesamtzahl von 250 Block- und Dateispeicherdatenträger bereitstellen. Wenden Sie sich an Ihren Vertriebsbeauftragten, um die Anzahl Ihrer Datenträger zu erhöhen.

## Wie viele Instanzen können einen bereitgestellten {{site.data.keyword.filestorage_short}}-Datenträger gemeinsam nutzen?

Die Standardbegrenzung für die Anzahl von Autorisierungen pro Datenträger ist 64. Wenden Sie sich an Ihren Vertriebsbeauftragten, um die Begrenzung zu erhöhen.

## Werden bei einer Bereitstellung von Performance oder Endurance {{site.data.keyword.filestorage_short}} die E/A-Operationen pro Sekunde auf Instanz- oder Datenträgerebene umgesetzt?

E/A-Operationen pro Sekunde (IOPS) werden auf Datenträgerebene umgesetzt. Mit anderen Worten, zwei Hosts, die mit einem Datenträger mit 6000 IOPS verbunden sind, nutzen diese 6000 IOPS gemeinsam.

## Lässt sich durch die Verwendung einer schnelleren Ethernet-Verbindung ein höherer Durchsatz erzielen?

Durchsatzbegrenzungen werden auf Datenträger- bzw. LUN-Ebne festgelegt. Daher führt die Verwendung einer schnelleren Ethernet-Verbindung nicht zu einer Erhöhung dieser Begrenzung. Bei einer langsameren Ethernet-Verbindung kann Ihre Bandbreite allerdings einen Engpass verursachen.

## Wirken sich Firewalls/Sicherheitsgruppen auf die Leistung aus?

Als bewährtes Verfahren wird empfohlen, den Speicherdatenverkehr über ein VLAN zu leiten, das die Firewall umgeht. Wenn der Speicherdatenverkehr über Software-Firewalls geleitet wird, erhöht sich dadurch die Latenz und die Speicherleistung wird beeinträchtigt.

## Was geschieht mit den Daten, wenn {{site.data.keyword.filestorage_short}}-Datenträger gelöscht werden?

Wenn Speicher gelöscht wird, werden alle Verweise auf die Daten in diesem Datenträger entfernt, sodass die Daten völlig unzugänglich werden. Wenn der physische Speicher erneut für ein anderes Konto bereitgestellt wird, wird eine neue Gruppe von Verweisen zugeordnet. Es besteht keine Möglichkeit für das neue Konto, auf Daten zuzugreifen, die sich möglicherweise einmal auf dem physischen Speicher befunden haben. Die neue Gruppe von Verweisen zeigt ausschließlich Nullen (0) an. Wenn neue Daten auf den Datenträger/die LUN geschrieben werden, werden alle unzugänglichen Daten, die noch vorhanden sein könnten, überschrieben. 

## Welche Leistungslatenz ist von {{site.data.keyword.filestorage_short}}-Speicher zu erwarten?   

Die Ziellatenz innerhalb des Speichers beträgt < 1 ms. File Storage-Speicher wird mit Compute-Instanzen in einem gemeinsam genutzten Netz verbunden, sodass die genaue Leistungslatenz vom Netzverkehr in einem bestimmten Zeitrahmen abhängig ist.

