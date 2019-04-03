---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, SLCLI, provisioning, API

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.filestorage_short}} über die SLCLI bestellen
{: #orderingSLCLI}

Sie können die SLCLI verwenden, um Bestellungen für Produkte zu platzieren, die normalerweise über das [ {{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") ](https://control.softlayer.com/){:new_window} bestellt werden. In der SL-API kann eine Bestellung aus mehreren Bestellungscontainern bestehen. Die Bestell-Befehlszeilenschnittstelle funktioniert nur mit einem Bestellcontainer.

Weitere Informationen zur Installation und Verwendung der SLCLI finden Sie unter [Python-API-Client ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}.
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

Weitere Informationen zur {{site.data.keyword.filestorage_short}}-Bestellung finden über die API Sie unter [order_file_volume ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://softlayer-python.readthedocs.io/en/latest/api/managers/file.html#SoftLayer.managers.file.FileStorageManager.order_file_volume){:new_window}.
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

Standardmäßig können Sie insgesamt 250 {{site.data.keyword.blockstorageshort}}- und {{site.data.keyword.filestorage_short}}-Datenträger bereitstellen. Wenden Sie sich an Ihren Vertriebsbeauftragten, wenn Sie die Anzahl Ihrer Datenträger erhöhen möchten. Weitere Informationen zum Erhöhen der Grenzwerte finden Sie in [Speichergrenzwerte verwalten](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits).
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

Weitere Informationen zum Autorisieren von Hosts für den Zugriff auf {{site.data.keyword.filestorage_short}} über die API finden Sie in [authorize_host_to_volume ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://softlayer-python.readthedocs.io/en/latest/api/managers/file.html#SoftLayer.managers.file.FileStorageManager.authorize_host_to_volume){:new_window}.
{:tip}

Weitere Informationen zum Grenzwert für gleichzeitige Autorisierungen finden Sie in den [FAQs](/docs/infrastructure/FileStorage?topic=FileStorage-faqs).
{:important}

## Verbindung zum neuen Speicher herstellen
{: #mountingvolumesCLI}

Folgen Sie je nach dem Betriebssystem Ihres Hosts dem entsprechenden Link.
- [{{site.data.keyword.filestorage_short}} unter Linux anhängen](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [{{site.data.keyword.filestorage_short}} unter CentOS anhängen](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [{{site.data.keyword.filestorage_short}} unter Container Linux anhängen](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)
- [{{site.data.keyword.filestorage_short}} für Sicherung mit cPanel konfigurieren](/docs/infrastructure/FileStorage?topic=FileStorage-cPanelBackups)
- [{{site.data.keyword.filestorage_short}} für Sicherung mit Plesk konfigurieren](/docs/infrastructure/FileStorage?topic=FileStorage-PleskBackup)
- [{{site.data.keyword.filestorage_short}}-Datenträger an ESXi-Hosts anhängen](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)
