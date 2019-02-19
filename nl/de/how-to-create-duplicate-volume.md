---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Duplikat eines {{site.data.keyword.filestorage_short}}-Datenträgers erstellen
{: #duplicatevolume}

Sie können ein Duplikat eines vorhandenen {{site.data.keyword.BluSoftlayer_full}} {{site.data.keyword.filestorage_full}}-Datenträgers erstellen. Der Duplikatdatenträger übernimmt standardmäßig die Kapazitäts- und Leistungsoptionen des ursprünglichen Datenträgers und enthält eine Kopie der Daten bis zum Zeitpunkt eines Snapshots.   

Da der Duplikatdatenträger auf den Daten eines zeitpunktgesteuerten Snapshots basiert, ist auf dem ursprünglichen Datenträger Snapshotbereich erforderlich, bevor ein Duplikat erstellt werden kann. Weitere Informationen zu Snapshots sowie zur Bestellung von Snapshotbereich finden Sie in der [Dokumentation zu Snapshots](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots).  

Duplikate können sowohl von **primären** Datenträgern als auch von **Replikat**datenträgern erstellt werden. Das neue Duplikat wird im selben Rechenzentrum wie der ursprüngliche Datenträger erstellt. Wenn Sie einen Duplikatdatenträger von einem Replikatdatenträger erstellen, wird der neue Datenträger im selben Rechenzentrum wie der Replikatdatenträger erstellt.

Als Benutzer mit einem dedizierten Konto für {{site.data.keyword.containerlong}} finden Sie in der [{{site.data.keyword.containerlong_notm}}-Dokumentation](/docs/containers?topic=containers-backup_restore#backup_restore) Informationen zu Optionen für die Duplizierung eines Datenträgers.
{:tip}

Auf Duplikatdatenträger kann ein Host für Lese-/Schreiboperationen zugreifen, sobald der Speicher bereitgestellt ist. Allerdings sind Snapshots und die Replikation erst zulässig, wenn das Erstellen der Datenkopie vom ursprünglichen Datenträger auf den Duplikatdatenträger abgeschlossen ist. Sobald die Datenkopie abgeschlossen ist, kann das Duplikat als unabhängiger Datenträger verwaltet und verwendet werden.

Diese Funktion ist an den meisten Standorten verfügbar. Klicken Sie [hier](/docs/infrastructure/FileStorage?topic=FileStorage-news), um zur Liste der verfügbaren Rechenzentren zu gelangen.

Einige allgemeine Verwendungen für einen duplizierten Datenträger sind die folgenden Beispiele.
- **Disaster-Recovery-Test**: Erstellen Sie ein Duplikat des Replikatdatenträgers, um zu überprüfen, ob die Daten intakt sind und im Fall einer Katastrophe ohne Unterbrechung der Replikation verwendet werden können.
- **Goldene Kopie**: Verwenden Sie einen Speicherdatenträger als goldene Kopie, um damit mehrere Instanzen für verschiedene Anwendungen zu erstellen.
- **Datenaktualisierung**: Erstellen Sie eine Kopie Ihrer Produktionsdaten, um diese zu Testzwecken an die nicht für die Produktion verwendete Umgebung anzuhängen.
- **Wiederherstellung aus Snapshot**: Stellen Sie Daten auf dem Originaldatenträger mit bestimmten Dateien und Datumsangaben aus einem Snapshot wieder her, ohne den gesamten Originaldatenträger mit einer Snapshotwiederherstellungsfunktion zu überschreiben.
- **Entwicklung und Test**: Erstellen Sie gleichzeitig bis zu vier simultane Duplikate eines Datenträgers, um duplizierte Daten zu Entwicklungs- und Testzwecken zu erstellen.
- **Größenänderung des Speichers**: Erstellen Sie einen Datenträger mit der neuen Größe und/oder den IOPS-Raten, ohne die Daten verschieben zu müssen.  

Sie können einen duplizierten Datenträger über das [{{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){:new_window} erstellen.


## Duplikat von einem bestimmten Datenträger in der Speicherliste erstellen

1. Rufen Sie Ihre {{site.data.keyword.filestorage_short}}-Liste auf.
    - Klicken Sie im Kundenportal auf **Speicher** > **{{site.data.keyword.filestorage_short}}** ODER
    - Klicken Sie im {{site.data.keyword.BluSoftlayer_full}}-Katalog auf **Infrastruktur** > **Speicher** > **{{site.data.keyword.filestorage_short}}**.
2. Wählen Sie eine LUN in der Liste aus und klicken Sie auf **Aktionen** > **LUN (Datenträger) duplizieren**.
3. Wählen Sie Ihre Snapshot-Option aus.
    - Bei Bestellung von einem Datenträger aus, der kein Replikat ist:
      - Wählen Sie die Option **Aus neuem Snapshot erstellen** aus – mit dieser Aktion wird ein Snapshot erstellt, der für das Duplikat zu verwenden ist. Verwenden Sie diese Option, wenn Ihr Datenträger gerade keine Snapshots aufweist oder wenn Sie ein Duplikat genau zu diesem Zeitpunkt erstellen wollen.</br>
      - Wählen Sie die Option **Aus letztem Snapshot erstellen** aus – mit dieser Aktion wird ein Duplikat aus dem letzten Snapshot erstellt, der für den betreffenden Datenträger vorhanden ist.
    - Bei Bestellung von einem Replikatdatenträger aus: Hier ist nur die Snapshotoption der Verwendung des letzten Snapshots verfügbar.
4. Speichertyp und Position bleiben die gleichen wie beim ursprünglichen Datenträger.
5. Stündliche oder monatliche Rechnungsstellung – Sie können die duplizierte LUN mit stündlicher oder monatlicher Rechnungsstellung bereitstellen. Der Rechnungsstellungstyp für den ursprünglichen Datenträger wird automatisch ausgewählt. Wenn Sie einen anderen Rechnungsstellungstyp für Ihren Duplikatspeicher auswählen möchten, können Sie diese Auswahl hier treffen.
5. Wenn Sie möchten, können Sie die E/A-Operationen pro Sekunde (IOPS) oder die IOPS-Stufe für den neuen Datenträger angeben. Standardmäßig wird der IOPS-Wert des ursprünglichen Datenträgers festgelegt. Die verfügbaren Performance- und Größenkombinationen werden angezeigt.
    - Wenn der ursprüngliche Datenträger die Endurance-Stufe von 0,25 IOPS hat, können Sie keine neue Auswahl angeben.
    - Wenn der ursprüngliche Datenträger die Endurance-Stufe von 2, 4 oder 10 IOPS hat, können Sie für den neuen Datenträger einen beliebigen Wert zwischen diesen Stufen angeben.
6. Sie können die Größe des neuen Datenträgers so aktualisieren, dass er größer als der ursprüngliche Datenträger wird. Standardmäßig wird die Größe des ursprünglichen Datenträgers festgelegt.

   {{site.data.keyword.filestorage_short}} kann bis auf das Zehnfache der ursprünglichen Größe des Datenträgers erhöht werden.
   {:tip}
7. Sie können den Snapshotbereich für den neuen Datenträger aktualisieren, um mehr, weniger oder keinen Snapshotbereich hinzuzufügen. Standardmäßig wird der Snapshotbereich des ursprünglichen Datenträgers festgelegt.
8. Klicken Sie auf **Weiter**, um Ihre Bestellung abzuschicken.


## Duplikat von einem bestimmten Snapshot erstellen

1. Rufen Sie Ihre {{site.data.keyword.filestorage_short}}-Liste auf.
2. Klicken Sie in der Liste auf einen Datenträger, um die Detailseite anzuzeigen. Dabei kann es sich um einen Replikatdatenträger oder einen anderen Datenträger handeln.
3. Blättern Sie nach unten und wählen Sie einen vorhandenen Snapshot in der Liste auf der Detailseite aus und klicken Sie auf **Aktionen ** > **Duplizieren**.   
4. Der Speichertyp (Endurance oder Leistung) und die Position bleiben mit dem ursprünglichen Datenträger identisch.
5. Die verfügbaren Performance- und Größenkombinationen werden angezeigt. Standardmäßig wird der IOPS-Wert des ursprünglichen Datenträgers festgelegt. Sie können die E/A-Operationen pro Sekunde (IOPS) oder die IOPS-Stufe für den neuen Datenträger angeben.
    - Wenn der ursprüngliche Datenträger die Endurance-Stufe von 0,25 IOPS hat, können Sie keine neue Auswahl angeben.
    - Wenn der ursprüngliche Datenträger die Endurance-Stufe von 2, 4 oder 10 IOPS hat, können Sie für den neuen Datenträger einen beliebigen Wert zwischen diesen Stufen angeben.
6. Sie können die Größe des neuen Datenträgers so aktualisieren, dass er größer als der ursprüngliche Datenträger wird. Standardmäßig wird die Größe des ursprünglichen Datenträgers festgelegt.

   {{site.data.keyword.filestorage_short}} kann bis auf das Zehnfache der ursprünglichen Größe des Datenträgers erhöht werden.
   {:tip}
7. Sie können den Snapshotbereich für den neuen Datenträger aktualisieren, um mehr, weniger oder keinen Snapshotbereich hinzuzufügen. Standardmäßig wird der Snapshotbereich des ursprünglichen Datenträgers festgelegt.
8. Klicken Sie auf **Weiter**, um Ihre Bestellung für das Duplikat abzuschicken.

## Duplikat über die SL-CLI erstellen
```
# slcli file volume-duplicate --help
Syntax: slcli file volume-duplicate [OPTIONEN] AUSGANGSDATENTRÄGER-ID

Optionen:
  -o, --origin-snapshot-id INTEGER
                                  ID eines Ausgangsdatenträgersnapshots
                                  für die Duplizierung.
  -c, --duplicate-size INTEGER    Größe des Dateidatenträgerduplikats in GB.
                                  ***Wird keine Größe angegeben, wird die Größe
                                  des Ausgangsdatenträgers verwendet.***
                                  Minimum: [Größe des Ausgangsdatenträgers]
  -i, --duplicate-iops INTEGER    Performance-Speicher-IOPS, zwischen 100 und
                                  6000 in Vielfachen von 100 [nur für
                                  Performance-Datenträger] ***Wird kein IOPS-Wert
                                  angegeben, wird der IOPS-Wert des Ausgangs-
                                  datenträgers verwendet.**
                                  Voraussetzungen: [Wenn der IOPS/GB-Wert für den
                                  Ausgangsdatenträger kleiner als 0,3 ist, muss
                                  der IOPS/GB-Wert für das Duplikat ebenfalls
                                  kleiner als 0,3 sein. Wenn der IOPS/GB-Wert für den
                                  Ausgangsdatenträger größer-gleich 0,3 ist,
                                  muss der IOPS/GB-Wert für das Duplikat ebenfalls
                                  größer-gleich 0,3 sein.]
  -t, --duplicate-tier [0.25|2|4|10]
                                  Endurance-Speichertier (IOPS pro GB) [nur
                                  für Endurance-Datenträger] ***Wird kein Tier
                                  angegeben, wird das Tier des Ausgangsdatenträgers
                                  verwendet.***
                                  Voraussetzungen: [Wenn der IOPS/GB-Wert für den
                                  Ausgangsdatenträger 0,25 ist muss der IOPS/GB-
                                  Wert für das Duplikat ebenfalls 0,25 sein. Wenn der
                                  IOPS/GB-Wert für den Ausgangsdatenträger größer
                                  als 0,25 ist, muss der IOPS/GB-Wert für das Duplikat
                                  ebenfalls größer als 0,25 sein.]
  -s, --duplicate-snapshot-size INTEGER
                                  Größe des Snapshotbereichs, der für das
                                  Duplikat zu bestellen ist. ***Wird keine
                                  Größe für den Snapshotbereich angegeben,
                                  wird die Größe des Snapshotbereichs für den
                                  Ausgangsdateidatenträger verwendet.***
                                  Den Wert "0" für diesen Parameter angeben, um ein
                                  Duplikat ohne Snapshotbereich zu bestellen.
  --billing [hourly|monthly]      Optionaler Parameter für den Abrechnungssatz
                                  (Standardwert: monatlich)
  -h, --help                      Diese Nachricht anzeigen und Ausführung beenden.
```

## Duplizierten Datenträger verwalten

Während Daten vom ursprünglichen Datenträger auf den Duplikatdatenträger kopiert werden, wird auf der Detailseite ein Status mit der Information angezeigt, dass die Duplizierung in Bearbeitung ist. Während dieses Zeitraums können Sie den Datenträger an einen Host anhängen und Lese-/Schreiboperationen auf dem Datenträger durchführen, Sie können jedoch keine Zeitpläne für Snapshots erstellen. Nach Abschluss des Duplizierungsprozesses ist der neue Datenträger unabhängig vom ursprünglichen Datenträger und kann ganz normal mit Snapshots und Replikationen verwaltet werden.
