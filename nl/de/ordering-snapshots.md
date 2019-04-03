---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, snapshot, ordering snapshot, snapshot space

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Snapshots bestellen

Zum automatischen oder manuellen Erstellen von Snapshots Ihres Speicherdatenträgers müssen Sie Speicherbereich kaufen, um diese Snapshots aufzubewahren. Sie können Kapazität bis zur Größe Ihres Speicherdatenträgers kaufen (beim Erstkauf des Datenträgers oder später mithilfe der folgenden Schritte).

## Menge des zu bestellenden Snapshotbereichs bestimmen

Im Allgemeinen verwenden Snapshots Snapshotbereich auf der Basis von zwei Hauptfaktoren:
- Menge der Änderungen Ihres aktiven Dateisystems
- Geplante Dauer der Beibehaltung der Snapshots  

Der benötigte Speicherplatz lässt sich wie folgt berechnen: **(Änderungsrate)** x **(Anzahl der beibehaltenen Stunden/Tage/Wochen/Monate der Daten)**.  

Der erste Snapshot verwendet nur sehr wenig Speicherplatz, da es sich bei ihm um eine Kopie der Metadaten (Verweise) handelt, mit denen die Blöcke des aktiven Dateisystems angegeben werden.
{:note}

Ein Datenträger mit vielen Änderungen und einer langen Aufbewahrungsdauer braucht mehr Speicherplatz als ein Datenträger mit moderaten Änderungen und einer moderateren Aufbewahrungsfrist. Ein Beispiel für den ersten Typ ist eine Datenbank mit hoher Änderungsrate. Ein Beispiel für den zweiten Typ ist ein VMware-Datenspeicher.

Wenn Sie von 500 GB tatsächlicher Daten 12 stündliche Snapshots machen und zwischen den einzelnen Snapshots 1 Prozent Änderungen auftreten, erreichen Sie einen Verbrauch von 60 GB für Snapshots.

*(5 GB Änderungsrate) x (12 stündliche Snapshots) = (60 GB belegter Speicherplatz)*

Wenn diese 500 GB tatsächlicher Daten mit 12 stündlichen Snapshots dagegen eine Änderungsrate von 10 Prozent pro Stunde aufweisen, wird 600 GB Screenshotbereich verwendet.

*(50 GB Änderungsrate) x (12 stündliche Snapshots) = (600 GB belegter Speicherplatz)*

Berücksichtigen Sie also bei der Bestimmung des benötigten Snapshotbereichs in angemessener Weise die Änderungsrate. Sie hat erheblichen Einfluss auf die Menge des erforderlichen Snapshotbereichs. Ein größerer Datenträger bedeutet tendenziell eine größere Änderungsmenge. Ein 500-GB-Datenträger mit Änderungen im Umfang von 5 GB und ein 10-TB-Datenträger mit Änderungen im Umfang von 5 GB belegen dieselbe Menge an Snapshotspeicherplatz.

Außerdem gilt bei den meisten Workloads, das umso weniger Speicherplatz anfangs reserviert werden muss, je größer ein Datenträger ist. Dies liegt in erster Linie an der zugrunde liegenden Datenleistung sowie an der Funktionsweise von Snapshots in der Umgebung.

## Snapshotbereich über die {{site.data.keyword.cloud_notm}}-Konsole bestellen

1. Melden Sie sich an der [The IBM Cloud-Konsole](https://{DomainName}/){:new_window} an und klicken Sie oben links auf das Menüsymbol. Wählen Sie **Klassische Infrastruktur** aus.

   Alternativ können Sie sich beim [{{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){:new_window} anmelden.
2. Greifen Sie über **Speicher** > **{{site.data.keyword.filestorage_short}}** auf Ihren Speicher zu.
3. Klicken Sie im Rahmen 'Snapshots' auf die Option zum Ändern des Snapshotbereichs.
4. Wählen Sie die Menge an benötigtem Speicherplatz und die Zahlungsmethode aus.
5. Klicken Sie auf **Weiter**.
6. Geben Sie gegebenenfalls den Werbeaktionscode ein und klicken Sie auf **Neu berechnen**. Die Felder **Gebühren für diese Bestellung** und **Bestellprüfung** enthalten Standardwerte.

   Rabatte werden bei der Verarbeitung der Bestellung angewendet.
   {:note}
7. Wählen Sie das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen und bin mit den darin genannten Bedingungen einverstanden** aus und klicken Sie auf **Auftrag erteilen**. Der Snapshotbereich wird in wenigen Minuten bereitgestellt.

## Snapshotbereich über die SLCLI bestellen

```
# slcli file snapshot-order --help
Syntax: slcli file snapshot-order [OPTIONEN] DATENTRÄGER-ID

Optionen:
  --capacity INTEGER    Größe des zu erstellenden Snapshots in GB  [erforderlich]
  --tier [0.25|2|4|10]  Endurance-Speichertier (IOPS per GB) des Dateispeicher-
                        datenträgers, für den Speicherbereich bestellt wird [optional,
                        nur gültig für Endurance-Speicherdatenträger]
  --upgrade             Flag zur Angabe, dass es sich um ein Upgrade handelt
  -h, --help            Diese Nachricht anzeigen und Ausführung beenden.
```
