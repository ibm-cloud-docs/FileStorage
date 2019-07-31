---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-25"

keywords: File Storage, file storage, NFS, authorizing hosts, rewoke access, grant access, view authorizations

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}


# {{site.data.keyword.filestorage_short}} verwalten
{: #managingstorage}

Sie können Ihre {{site.data.keyword.filestorage_full}}-Datenträger über die {{site.data.keyword.cloud}}-Konsole verwalten.

## Hosts für den Zugriff auf {{site.data.keyword.filestorage_short}} berechtigen

Berechtigte (“autorisierte”) Hosts sind Hosts, denen Zugriff auf einen bestimmten Datenträger erteilt wurde. Ohne die Hostberechtigung können Sie nicht über Ihr System auf den Speicher zugreifen oder ihn verwenden. Durch das Berechtigen eines Hosts für den Zugriff auf Ihren Datenträger werden der Benutzername und das Kennwort generiert.

Sie können Hosts autorisieren und verbinden, die sich in demselben Rechenzentrum wie Ihr Speicher befinden. Sie können mehrere Konten haben, Sie können jedoch nicht einen Host über das eine Konto für den Zugriff auf Ihren Speicher in einem anderen Konto berechtigen.
{:important}

1. Rufen Sie die [{{site.data.keyword.cloud}}-Konsole](https://{DomainName}/){: external} auf. Wählen Sie im Menü **Klassische Infrastruktur** aus.
2. Klicken Sie auf **Speicher** > **{{site.data.keyword.filestorage_short}}** und klicken Sie auf den Namen Ihres Datenträgers im Feld **Datenträgername**.
3. Blättern Sie zum Abschnitt **Autorisierte Hosts** auf der Seite.
4. Klicken Sie rechts auf **Host autorisieren**.
5. Filtern Sie die Liste der verfügbaren Hosts, indem Sie den Gerätetyp, das Teilnetz oder die IP-Adresse auswählen. 

   Wenn die Liste nach Teilnetzen gefiltert wird, handelt es sich bei den angezeigten Teilnetzen um abonnierte Teilnetze, die sich im selben Rechenzentrum befinden wie der Speicherdatenträger.
   {:note}
6. Wählen Sie einen oder mehrere Hosts in der Liste aus und klicken Sie auf **Speichern**.

Alternativ dazu können Sie den folgenden Befehl in der SLCLI verwenden.
```
# slcli file access-authorize --help
Syntax: slcli file access-authorize [OPTIONS] VOLUME_ID

Optionen:
  -h, --hardware-id TEXT    ID eines einzelnen Hardware-Servers für die Autorisierung.
  -v, --virtual-id TEXT     ID eines virtuellen Servers für die Autorisierung.
  -i, --ip-address-id TEXT  ID einer einzelnen IP-Adresse für die Autorisierung.
  -p, --ip-address TEXT     IP-Adresse für die Autorisierung.
  -s, --subnet-id TEXT      ID eines einzelnen Teilnetzes für die Autorisierung.
  --help                    Diese Nachricht anzeigen und Ausführung beenden.
```

## Liste der Hosts anzeigen, die zum Zugriff auf einen {{site.data.keyword.filestorage_short}}-Datenträger berechtigt sind

1. Rufen Sie die [{{site.data.keyword.cloud}}-Konsole](https://{DomainName}/){: external} auf. Wählen Sie im Menü **Klassische Infrastruktur** aus.
2. Klicken Sie auf **Speicher** > **{{site.data.keyword.filestorage_short}}** und klicken Sie auf den Namen Ihres Datenträgers im Feld **Datenträgername**.
3. Blättern Sie auf der Seite nach unten bis zum Abschnitt **Autorisierte Hosts**.

Dort wird eine Liste der Hosts angezeigt, die derzeit für den Zugriff auf den Datenträger berechtigt sind.

Alternativ dazu können Sie den folgenden Befehl in der SLCLI verwenden.
```
# slcli file access-list --help
Syntax: slcli file access-list [OPTIONEN] DATENTRÄGER-ID

Optionen:
  --sortby TEXT   Spalten für die Sortierung
  --columns TEXT  Spalten für die Anzeige. Optionen: ID, Name, Typ,
                  private IP-Adresse, Quellenteilnetz, Host-IQN,
                  Benutzername, Kennwort, zulässige Host-ID
  -h, --help      Diese Nachricht anzeigen und Ausführung beenden.
```


## {{site.data.keyword.filestorage_short}}-Datenträger anzeigen, für die ein Host zugriffsberechtigt ist

Sie können die Datenträger anzeigen, auf die ein Host Zugriff hat, sowie Informationen, die zum Herstellen einer Verbindung erforderlich sind – Datenträgername, Speichertyp, Zieladresse, Kapazität und Position.

1. Rufen Sie die [{{site.data.keyword.cloud}}-Konsole](https://{DomainName}/){: external} auf.
2. Wählen Sie im Menü **Klassische Infrastruktur** aus.
3. Klicken Sie auf **Geräte** > **Geräteliste**.
4. Klicken Sie auf das betreffende Gerät.
5. Wählen Sie die Registerkarte 'Speicher' aus.

Es wird eine Liste von Speicherdatenträgern angezeigt, auf die dieser bestimmte Host Zugriff hat. Die Datenträger werden nach Speichertyp (Block, Datei, andere) gruppiert. Über die jeweiligen **Aktion**-Menüs können Sie zusätzlichen Speicher autorisieren oder Zugriffsberechtigungen entfernen.


## {{site.data.keyword.filestorage_short}} anhängen und abhängen

Sie können die in der Ansicht **Datenträgerdetails** bereitgestellten Mountpunktinformationen verwenden, um {{site.data.keyword.filestorage_short}} von einem Host aus anzuhängen. Weitere Informationen finden Sie im Abschnitt [Zugriff auf {{site.data.keyword.filestorage_short}} unter Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux).


## Hostzugriff auf {{site.data.keyword.filestorage_short}} widerrufen

Wenn Sie den Zugriff von einem Host auf einen bestimmten Speicherdatenträger beenden wollen, können Sie den Zugriff widerrufen. Beim Widerrufen des Zugriffs wird die Hostverbindung von dem Datenträger gelöscht. Das Betriebssystem und die Anwendungen können nicht mehr mit dem Datenträger kommunizieren.

Zur Vermeidung von hostseitigen Problemen hängen Sie den Speicherdatenträger von Ihrem Betriebssystem ab, bevor Sie den Zugriff widerrufen, sodass keine Laufwerke fehlen oder Daten beschädigt werden.
{:important}

Sie können den Zugriff über 'Speicher' in der Ansicht 'Geräteliste' oder in der Ansicht 'Speicher' widerrufen.

### Zugriff über die Geräteliste widerrufen

1. Rufen Sie die [{{site.data.keyword.cloud}}-Konsole](https://{DomainName}/){: external} auf.
2. Wählen Sie im Menü **Klassische Infrastruktur** aus.
3. Klicken Sie auf **Geräte** > **Geräteliste**.
2. Doppelklicken Sie auf das betreffende Gerät.
3. Wählen Sie die Registerkarte **Speicher** aus.
4. Es wird gruppiert nach Speichertyp (Block, Datei, andere) eine Liste von Speicherdatenträgern angezeigt, auf die dieser bestimmte Host Zugriff. Wählen Sie das jeweilige Menü **Aktion** neben dem Datenträger aus, für den Sie den Zugriff widerrufen möchten, und klicken Sie auf **Zugriff widerrufen**.
5. Bestätigen Sie, dass Sie den Zugriff auf einen Datenträger widerrufen möchten, da die Aktion nicht rückgängig gemacht werden kann. Klicken Sie auf **Ja**, um den Datenträgerzugriff zu widerrufen, oder auf **Nein**, um die Aktion abzubrechen.

Wenn Sie die Verbindungen mehrerer Datenträger von einem bestimmten Host trennen wollen, müssen Sie die Aktion 'Zugriff widerrufen' für jeden Datenträger wiederholen.
{:tip}


### Zugriff über die Speicheransicht widerrufen

1. Rufen Sie die [{{site.data.keyword.cloud}}-Konsole](https://{DomainName}/){: external} auf.
2. Wählen Sie im Menü **Klassische Infrastruktur** aus.
3. Klicken Sie auf **Speicher** > **{{site.data.keyword.filestorage_short}}** und wählen Sie den **Datenträger** aus, für den die Zugriffsberechtigung widerrufen werden soll.
4. Blättern Sie zum Abschnitt **Autorisierte Hosts** auf der Seite.
5. Klicken Sie neben dem Host, dessen Zugriff widerrufen werden soll, auf **Aktionen** und wählen Sie **Zugriff widerrufen** aus.
6. Bestätigen Sie, dass Sie den Zugriff auf einen Datenträger widerrufen möchten, da die Aktion nicht rückgängig gemacht werden kann. Klicken Sie auf **Ja**, um den Datenträgerzugriff zu widerrufen, oder auf **Nein**, um die Aktion abzubrechen.

Wenn Sie die Verbindung zwischen mehreren Hosts und einem bestimmten Datenträger trennen wollen, müssen Sie die Aktion 'Zugriff widerrufen' für jeden Host wiederholen.
{:tip}

### Zugriff über die SLCLI widerrufen
Sie können den folgenden Befehl in der SLCLI verwenden.
```
# slcli file access-revoke --help
Syntax: slcli file access-revoke [OPTIONEN] DATENTRÄGER-ID

Optionen:
  -h, --hardware-id TEXT    ID eines einzelnen Hardware-Servers für das Widerrufen der Autorisierung.
  -v, --virtual-id TEXT     ID eines einzelnen virtuellen Servers für das Widerrufen der Autorisierung.
  -i, --ip-address-id TEXT  ID einer einzelnen IP-Adresse für das Widerrufen der Autorisierung.
  -p, --ip-address TEXT     IP-Adresse für das Widerrufen der Autorisierung.
  -s, --subnet-id TEXT      ID eines einzelnen Teilnetzes für das Widerrufen der Autorisierung.
  --help                    Diese Nachricht anzeigen und Ausführung beenden.
```

## Speicherdatenträger stornieren

Wenn Sie einen bestimmten Datenträger nicht mehr benötigen, können Sie ihn stornieren. Zur Stornierung eines Speicherdatenträgers müssen Sie zunächst den Zugriff für alle Hosts widerrufen.

1. Rufen Sie die [{{site.data.keyword.cloud}}-Konsole](https://{DomainName}/){: external} auf. Wählen Sie im Menü **Klassische Infrastruktur** aus.
2. Klicken Sie auf **Speicher** > **{{site.data.keyword.filestorage_short}}**.
3. Klicken Sie für den zu stornierenden Datenträger auf **Aktionen** und wählen Sie die Option zum Stornieren der {{site.data.keyword.filestorage_short}}-Instanz aus.****
4. Bestätigen Sie, ob der Datenträger sofort oder am Rechnungsstichtag für den Bereitstellungszeitpunkt des Datenträgers storniert werden soll.

   Wenn Sie die Option auswählen, den Datenträger am Rechnungsstichtag zu stornieren, können Sie die Stornierungsanforderung vor ihrem Stichtag annullieren.
   {:tip}
5. Klicken Sie auf **Weiter** oder **Schließen**.
6. Klicken Sie auf das Kontrollkästchen für die Bestätigung und klicken Sie auf **Bestätigen**.

Alternativ dazu können Sie den folgenden Befehl in der SLCLI verwenden.
```
# slcli file volume-cancel --help
Syntax: slcli file volume-cancel [OPTIONEN] DATENTRÄGER-ID

Optionen:
  --reason TEXT  Optionaler Grund für die Stornierung
  --immediate    Storniert den Dateispeicherdatenträger sofort, nicht
                 am Abrechnungsstichtag.
  -h, --help     Diese Nachricht anzeigen und Ausführung beenden.
```

Wenn der Datenträger storniert wird, gilt eine Wartezeit von 24 für die Freigabe. Während dieses 24-stündigen Zeitraums wird der Datenträger weiterhin in der Konsole angezeigt. Nach Ablauf des Freigabezeitraums werden die Daten gelöscht und der Datenträger wird auch aus der Konsole entfernt. Die Abrechnung für den Datenträger wird jedoch sofort gestoppt. Weitere Informationen finden Sie in [Häufig gestellte Fragen](/docs/infrastructure/FileStorage?topic=FileStorage-file-storage-faqs).
{:note}

 Aktive Replikate können die Freigabe des Speicherdatenträgers blockieren. Stellen Sie sicher, dass der Datenträger nicht mehr angehängt ist, dass die Hostberechtigungen widerrufen wurden und das die Replikation abgebrochen wurde, bevor Sie den ursprünglichen Datenträger stornieren. 
