---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}


# {{site.data.keyword.filestorage_short}} verwalten
{: #managingstorage}

Sie können Ihre {{site.data.keyword.filestorage_full}}-Datenträger über {{site.data.keyword.slportal}} verwalten.

## Hosts für den Zugriff auf {{site.data.keyword.filestorage_short}} berechtigen

Berechtigte (“autorisierte”) Hosts sind Hosts, denen Zugriff auf einen bestimmten Datenträger erteilt wurde. Ohne die Hostberechtigung können Sie nicht über Ihr System auf den Speicher zugreifen oder ihn verwenden. Durch das Berechtigen eines Hosts für den Zugriff auf Ihren Datenträger werden der Benutzername und das Kennwort generiert.

Sie können Hosts autorisieren und verbinden, die sich in demselben Rechenzentrum wie Ihr Speicher befinden. Sie können mehrere Konten haben, Sie können jedoch nicht einen Host über das eine Konto für den Zugriff auf Ihren Speicher in einem anderen Konto berechtigen.
{:important}

1. Klicken Sie auf **Speicher** > **{{site.data.keyword.filestorage_short}}** und klicken Sie auf den Namen Ihres Datenträgers im Feld **Datenträgername**.
2. Blättern Sie zum Abschnitt **Autorisierte Hosts** auf der Seite.
3. Klicken Sie rechts auf **Host autorisieren**. Wählen Sie die Hosts aus, die auf diesen bestimmten Datenträger zugreifen können.

Alternativ dazu können Sie den folgenden Befehl in der SLCLI verwenden. 
```
# slcli file access-authorize --help
Syntax: slcli file access-authorize [OPTIONS] VOLUME_ID

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

## Liste der Hosts anzeigen, die zum Zugriff auf einen {{site.data.keyword.filestorage_short}}-Datenträger berechtigt sind

1. Klicken Sie auf **Speicher > {{site.data.keyword.filestorage_short}}** und klicken Sie auf den Namen Ihres Datenträgers im Feld **Datenträgername**.
2. Blättern Sie auf der Seite nach unten bis zum Abschnitt **Autorisierte Hosts**.

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

1. Klicken Sie auf **Geräte** > **Geräteliste** im [{{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){:new_window}.
2. Klicken Sie auf das betreffende Gerät.
2. Wählen Sie die Registerkarte 'Speicher' aus.

Es wird eine Liste von Speicherdatenträgern angezeigt, auf die dieser bestimmte Host Zugriff hat. Die Datenträger werden nach Speichertyp (Block, Datei, andere) gruppiert. Über die jeweiligen **Aktion**-Menüs können Sie zusätzlichen Speicher autorisieren oder Zugriffsberechtigungen entfernen.


## {{site.data.keyword.filestorage_short}} anhängen und abhängen

Sie können die in der Ansicht **Datenträgerdetails** bereitgestellten Mountpunktinformationen verwenden, um {{site.data.keyword.filestorage_short}} von einem Host aus anzuhängen. Weitere Informationen finden Sie im Abschnitt [Zugriff auf {{site.data.keyword.filestorage_short}} unter Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux).


## Hostzugriff auf {{site.data.keyword.filestorage_short}} widerrufen

Wenn Sie den Zugriff von einem Host auf einen bestimmten Speicherdatenträger beenden wollen, können Sie den Zugriff widerrufen. Beim Widerrufen des Zugriffs wird die Hostverbindung von dem Datenträger gelöscht. Das Betriebssystem und die Anwendungen können nicht mehr mit dem Datenträger kommunizieren.

Zur Vermeidung von hostseitigen Problemen hängen Sie den Speicherdatenträger von Ihrem Betriebssystem ab, bevor Sie den Zugriff widerrufen, sodass keine Laufwerke fehlen oder Daten beschädigt werden.
{:important}

Sie können den Zugriff über 'Speicher' in der Ansicht 'Geräteliste' oder in der Ansicht 'Speicher' widerrufen.

### Zugriff über die Geräteliste widerrufen

1. Klicken Sie auf **Geräte** > **Geräteliste** im [{{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){:new_window}.
2. Doppelklicken Sie auf das betreffende Gerät.
3. Wählen Sie die Registerkarte **Speicher** aus.
4. Es wird gruppiert nach Speichertyp (Block, Datei, andere) eine Liste von Speicherdatenträgern angezeigt, auf die dieser bestimmte Host Zugriff. Wählen Sie das jeweilige Menü **Aktion** neben dem Datenträger aus, für den Sie den Zugriff widerrufen möchten, und klicken Sie auf **Zugriff widerrufen**.
5. Bestätigen Sie, dass Sie den Zugriff auf einen Datenträger widerrufen möchten, da die Aktion nicht rückgängig gemacht werden kann. Klicken Sie auf **Ja**, um den Datenträgerzugriff zu widerrufen, oder auf **Nein**, um die Aktion abzubrechen.

Wenn Sie die Verbindungen mehrerer Datenträger von einem bestimmten Host trennen wollen, müssen Sie die Aktion 'Zugriff widerrufen' für jeden Datenträger wiederholen.
{:tip}


### Zugriff über die Speicheransicht widerrufen

1. Klicken Sie auf **Speicher > {{site.data.keyword.filestorage_short}}** und wählen Sie den **Datenträger** aus, für den Sie den Zugriff widerrufen wollen.
2. Blättern Sie zum Abschnitt **Autorisierte Hosts** auf der Seite.
3. Klicken Sie neben dem Host, dessen Zugriff widerrufen werden soll, auf **Aktionen** und wählen Sie **Zugriff widerrufen** aus.
4. Bestätigen Sie, dass Sie den Zugriff auf einen Datenträger widerrufen möchten, da die Aktion nicht rückgängig gemacht werden kann. Klicken Sie auf **Ja**, um den Datenträgerzugriff zu widerrufen, oder auf **Nein**, um die Aktion abzubrechen.

Wenn Sie die Verbindung zwischen mehreren Hosts und einem bestimmten Datenträger trennen wollen, müssen Sie die Aktion 'Zugriff widerrufen' für jeden Host wiederholen.
{:tip}

### Zugriff über die SLCLI widerrufen
Alternativ dazu können Sie den folgenden Befehl in der SLCLI verwenden. 
```
# slcli file access-revoke --help
Syntax: slcli file access-revoke [OPTIONEN] DATENTRÄGER-ID

Optionen:
  -h, --hardware-id TEXT    ID einer SoftLayer-Hardware zum Widerrufen der
                            Berechtigung
  -v, --virtual-id TEXT     ID eines virtuellen SoftLayer-Gastsystems zum
                            Widerrufen der Berechtigung
  -i, --ip-address-id TEXT  ID der Teilnetz-IP-Adresse eines SoftLayer-Netzes
                            zum Widerrufen der Berechtigung
  --ip-address TEXT         IP-Adresse zum Widerrufen der Berechtigung
  -s, --subnet-id TEXT      ID des Teilnetzes eines SoftLayer-Netzes zum
                            Widerrufen der Berechtigung
  --help                    Diese Nachricht anzeigen und Ausführung beenden.
```

## Speicherdatenträger stornieren

Wenn Sie einen bestimmten Datenträger nicht mehr benötigen, können Sie ihn stornieren. Zur Stornierung eines Speicherdatenträgers müssen Sie zunächst den Zugriff für alle Hosts widerrufen.

1. Klicken Sie auf **Speicher**>**{{site.data.keyword.filestorage_short}}**.
2. Klicken Sie für den zu stornierenden Datenträger auf **Aktionen** und wählen Sie die Option zum Stornieren der {{site.data.keyword.filestorage_short}}-Instanz aus.****
3. Bestätigen Sie, ob der Datenträger sofort oder am Rechnungsstichtag für den Bereitstellungszeitpunkt des Datenträgers storniert werden soll.

   Wenn Sie die Option auswählen, den Datenträger am Rechnungsstichtag zu stornieren, können Sie die Stornierungsanforderung vor ihrem Stichtag annullieren.
   {:tip}
4. Klicken Sie auf **Weiter** oder **Schließen**.
5. Klicken Sie auf das Kontrollkästchen für die Bestätigung und klicken Sie auf **Bestätigen**.

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
