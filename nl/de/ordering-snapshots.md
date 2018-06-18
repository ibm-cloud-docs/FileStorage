---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-18"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Snapshots bestellen

Zum automatischen oder manuellen Erstellen von Snapshots Ihres Speicherdatenträgers müssen Sie Speicherbereich kaufen, um diese Snapshots aufzubewahren. Sie können Kapazität bis zur Größe Ihres Speicherdatenträgers kaufen (beim Erstkauf des Datenträgers oder später mithilfe der folgenden Schritte).

1. Greifen Sie auf Ihren Speicher über die Registerkarte **Speicher** > **{{site.data.keyword.filestorage_short}}** im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} zu.
2. Klicken Sie im Feld **Snapshots** auf **Snapshotbereich hinzufügen**.
3. Klicken Sie im Feld **Snapshots** auf **Snapshotbereich jetzt kaufen**.
3. Wählen Sie die erforderliche Speicherbereichsmenge aus.
4. Klicken Sie auf **Weiter**.
5. Geben Sie einen Werbeaktionscode ein, wenn Sie einen haben, und klicken Sie auf **Neu berechnen**. Die Felder **Gebühren für diese Bestellung** und **Bestellprüfung** enthalten Standardwerte.
6. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Bestellung abschicken**. Ihr Snapshotbereich wird innerhalb weniger Minuten bereitgestellt.

## Vorgehensweise zum Ermitteln der Menge des zu bestellenden Snapshotbereichs

Leider gibt es kein bewährtes Verfahren, das als anwendungsbasierte Empfehlung dienen könnte. Im Allgemeinen verwenden Snapshots Snapshotbereich auf der Basis von zwei Informationen:
- Menge der Änderungen Ihres aktiven Dateisystems
- Geplante Dauer der Beibehaltung der Snapshots  

Prinzipiell lässt sich der benötigte Speicherplatz wie folgt berechnen: **(Änderungsrate)** x **(Anzahl der beibehaltenen Stunden/Tage/Wochen/Monate)**.  
**Anmerkung**: Der erste Snapshot verwendet nur sehr wenig Speicherplatz, da es sich bei ihm um eine Kopie der Metadaten
(Verweise) handelt, mit denen die Blöcke des aktiven Dateisystems angegeben werden.

Ein Datenträger mit vielen Datenänderungen (beispielsweise eine Datenbank mit hoher Änderungsrate) und mit einer langen Aufbewahrungsdauer für Snapshots braucht mehr Speicherplatz für Snapshots als ein Datenträger mit einer moderaten Änderungsrate (beispielsweise ein VM-Datenspeicher) und einer moderateren Aufbewahrungsfrist für Snapshots.

Wenn Sie von einem Datenträger, der 500 GB tatsächlicher Daten enthält, 12 stündliche Snapshots machen und zwischen den einzelnen Snapshots 1 Prozent Änderungen auftreten, erreichen Sie einen Verbrauch von 60 GB für Snapshots.

*(5 GB Änderungsrate) x (12 stündliche Snapshots) = 60 GB*

Wenn diese 500 GB tatsächlicher Daten mit 12 stündlichen Snapshots dagegen eine Änderungsrate von 10 Prozent pro Stunde aufweisen, würden Sie schließlich 600 GB verwenden.

*(50 GB Änderungsrate) x (12 stündliche Snapshots) = 600 GB*

Berücksichtigen Sie also bei der Bestimmung des benötigten Snapshotbereichs in angemessener Weise die Änderungsrate. Sie hat erheblichen Einfluss auf die Menge des erforderlichen Snapshotbereichs. Auch wenn die Größe eines Datenträgers tendenziell eine größere Änderungsmenge bedeutet, belegen ein 500-GB-Datenträger mit 5 GB Änderung und ein 10-TB-Datenträger mit 5 GB Änderung denselben Snapshotspeicher.

Außerdem gilt bei den meisten Workloads, das umso weniger Speicherplatz anfangs für Snapshots reserviert werden muss, je größer ein Datenträger ist. Dies liegt in erster Linie an der zugrunde liegenden Datenleistung Ihrer Plattform sowie an der Funktionsweise von Snapshots in Ihrer Umgebung.
