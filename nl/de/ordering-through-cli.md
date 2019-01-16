---

copyright:
  years: 2014, 2019
lastupdated: "2019-01-07"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.filestorage_short}} über die SL-Befehlszeilenschnittstelle bestellen

Sie können die SL-Befehlszeilenschnittstelle verwenden, um Bestellungen für Produkte zu platzieren, die normalerweise über das [ {{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") ](https://control.softlayer.com/){:new_window} bestellt werden. In der SL-API kann eine Bestellung aus mehreren Bestellungscontainern bestehen. Die Bestell-Befehlszeilenschnittstelle funktioniert nur mit einem Bestellcontainer.

Weitere Informationen zur Installation und Verwendung der SL-Befehlszeilenschnittstelle finden Sie unter [Python-API-Client](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}.
{:tip}

## Nach verfügbaren {{site.data.keyword.filestorage_short}}-Angeboten suchen

Die erste Komponente, nach der Sie suchen, wenn Sie einen Auftrag platzieren, ist ein Paket. Pakete werden auf die verschiedenen Produkte der höchsten Ebene aufgeteilt, die für die Bestellung in {{site.data.keyword.BluSoftlayer_full}} verfügbar sind. Einige Beispielpakete sind CLOUD_SERVER für VSIs, BARE_METAL_SERVER für Bare-Metal-Server und STORAGE_AS_A_SERVICE_STAAS für {{site.data.keyword.filestorage_short}} und {{site.data.keyword.blockstorageshort}}.

Innerhalb eines Pakets werden einige Elemente in Kategorien unterteilt. Für einige Pakete sind Voreinstellungen vorhanden, für andere müssen die Elemente einzeln angegeben werden. Wenn die Kategorie eines Pakets erforderlich ist, muss ein Element aus dieser Kategorie ausgewählt werden, um das Paket zu bestellen. Abhängig von der Kategorie können sich einige Elemente in der Kategorie gegenseitig ausschließen.

Jede Bestellung muss eine zugeordnete Position (Rechenzentrum) haben. Stellen Sie bei der Bestellung von {{site.data.keyword.filestorage_short}} sicher, dass es an derselben Position wie Ihre Berechnungsinstanzen bereitgestellt wird.
{:important}

Sie können den Befehl `slcli order package-list` verwenden, um das Paket zu suchen, das Sie bestellen möchten. Die Option `-keyword` wird bereitgestellt, um eine einfache Such- und Filterfunktion auszuführen. Diese Option erleichtert die Suche nach dem benötigten Paket. Suchen Sie nach `Storage-as-a-Service Package 759`. 

```
$ slcli order package-list --help
Syntax: slcli order package-list [OPTIONS]

  Pakete auflisten, die über die API placeOrder bestellt werden können.

  Beispiel:
      # Alle Pakete für die Bestellung auflisten
      slcli order package-list

  Schlüsselwörter können auch für einige einfache Filterfunktionen verwendet werden,
  um die Suche nach einem Paket zu vereinfachen.

  Beispiel:
     # Alle Pakete mit "Server" im Namen aufführen
      slcli order package-list --keyword server

Optionen:
  --keyword TEXT  Ein Wort (oder eine Zeichenfolge) zum Filtern von Paketnamen.
  -h, --help      Diese Nachricht anzeigen und beenden.
```

Der Befehl `slcli file volume-order` kann ebenfalls verwendet werden.

```
# slcli file volume-order --help
Syntax: slcli file volume-order [OPTIONS]

  Dateispeicherdatenträger bestellen.

Optionen:
  --storage-type [performance|endurance]
                                  Typ des Dateispeicherdatenträgers [erforderlich]
  --size INTEGER                  Größe des Dateispeicherdatenträgers in GB
                                  [erforderlich]
  --iops INTEGER                  Performance Storage-IOPs, zwischen 100 und
                                  6000 in Vielfachen von 100 [für Speichertyp
                                  Performance erforderlich]
  --tier [0.25|2|4|10]            Endurance-Speicher-Tier (IOP pro GB)
                                  [für Speichertyp Endurance erforderlich]
  --location TEXT                 Kurzname des Rechenzentrums (z. B.: dal09)
                                  [erforderlich]
  --snapshot-size INTEGER         Optionaler Parameter für die Bestellung des
                                  mit Endurance-Dateispeicher;
                                  gibt die Größe (in GB) des zu bestellenden
                                  Snapshotspeicherbereichs an
  --service-offering [storage_as_a_service|enterprise|performance]
                                  Serviceangebotspaket, das für die Bestellung
                                  verwendet werden soll [optional, Standardwert:
                                  'storage_as_a_service']
  --billing [hourly|monthly]      Optionaler Parameter den Abrechnungssatz
                                  (Standardwert: monatlich)
  -h, --help                      Diese Nachricht anzeigen und Ausführung beenden.
```

Weitere Informationen zur Bestellung von {{site.data.keyword.filestorage_short}} über die API finden Sie unter [order_file_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/file.html#SoftLayer.managers.file.FileStorageManager.order_file_volume){:new_window}.
Um auf alle neuen Funktionen zugreifen zu können, müssen Sie `Storage-as-a-Service Package 759` bestellen.
{:tip}


## Bestellung aufgeben

Das folgende Beispiel veranschaulicht die Bestellung eines 10-GB-{{site.data.keyword.filestorage_short}}-Datenträgers mit 100 E/A-Operationen pro Sekunde/GB.

```
# slcli file volume-order --storage-type performance --size 20 --location dal10 --iops 100
Order #32076317 placed successfully!
> Storage as a Service
> File Storage
> 20 GBs
> 100 IOPS
```

Standardmäßig können Sie insgesamt 250 {{site.data.keyword.blockstorageshort}}- und {{site.data.keyword.filestorage_short}}-Datenträger bereitstellen. Wenden Sie sich an Ihren Vertriebsbeauftragten, wenn Sie die Anzahl Ihrer Datenträger erhöhen möchten. Weitere Informationen zum Erhöhen der Grenzwerte finden Sie in [Speichergrenzwerte verwalten](managing-storage-limits.html).
{:important}

## Hosts für den Zugriff auf den neuen Speicher autorisieren

```
# slcli file access-authorize --help
Syntax: slcli file access-authorize [OPTIONS] VOLUME_ID

  Host für den Zugriff auf einen angegebenen Datenträger berechtigen.

Optionen:
  -h, --hardware-id TEXT    ID einer SoftLayer-Hardware zur Berechtigung
  -v, --virtual-id TEXT     ID eines virtuellen SoftLayer-Gastsystems zur Berechtigung
  -i, --ip-address-id TEXT  ID der Teilnetz-IP-Adresse eines SoftLayer-Netzes
                            zur Berechtigung
  --ip-address TEXT         IP-Adresse zur Berechtigung
  -s, --subnet-id TEXT      ID des Teilnetzes eines SoftLayer-Netzes
                            zur Berechtigung
  --help                    Diese Nachricht anzeigen und Ausführung beenden.
```

Weitere Informationen zum Autorisieren von Hosts für den Zugriff auf {{site.data.keyword.filestorage_short}} über die API finden Sie unter [authorize_host_to_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/file.html#SoftLayer.managers.file.FileStorageManager.authorize_host_to_volume){:new_window}.
{:tip}

eitere Informationen zum Grenzwert für gleichzeitige Autorisierungen finden Sie in den [FAQs](faqs.html).
{:important}

## Verbindung zum neuen Speicher herstellen

Folgen Sie je nach dem Betriebssystem Ihres Hosts dem entsprechenden Link.
- [{{site.data.keyword.filestorage_short}} unter Linux anhängen](accessing-file-storage-linux.html)
- [{{site.data.keyword.filestorage_short}} unter CentOS anhängen](mounting-nsf-file-storage.html)
- [{{site.data.keyword.filestorage_short}} unter Container Linux anhängen](mounting-storage-coreos.html)
- [{{site.data.keyword.filestorage_short}} für Sicherung mit cPanel konfigurieren](configure-backup-cpanel.html)
- [{{site.data.keyword.filestorage_short}} für Sicherung mit Plesk konfigurieren](configure-backup-plesk.html)
- [{{site.data.keyword.filestorage_short}}-Datenträger an ESXi-Hosts anhängen](architecture-guide-file-storage-vmware.html)