---

copyright:
  years: 2014, 2018
lastupdated: "2018-08-01"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Duplikat eines {{site.data.keyword.filestorage_short}}-Datenträgers erstellen

Sie können ein Duplikat eines vorhandenen {{site.data.keyword.BluSoftlayer_full}} {{site.data.keyword.filestorage_full}}-Datenträgers erstellen. Der Duplikatdatenträger übernimmt standardmäßig die Kapazitäts- und Leistungsoptionen des ursprünglichen Datenträgers und enthält eine Kopie der Daten bis zum Zeitpunkt eines Snapshots.   

Da der Duplikatdatenträger auf den Daten eines zeitpunktgesteuerten Snapshots basiert, ist auf dem ursprünglichen Datenträger Snapshotbereich erforderlich, bevor ein Duplikat erstellt werden kann. Weitere Informationen zu Snapshots sowie zur Bestellung von Snapshotbereich finden Sie in der [Dokumentation zu Snapshots](snapshots.html).  

Duplikate können sowohl von **primären** Datenträgern als auch von **Replikat**datenträgern erstellt werden. Das neue Duplikat wird im selben Rechenzentrum wie der ursprüngliche Datenträger erstellt. Wenn Sie einen Duplikatdatenträger von einem Replikatdatenträger erstellen, wird der neue Datenträger im selben Rechenzentrum wie der Replikatdatenträger erstellt.

Auf Duplikatdatenträger kann ein Host für Lese-/Schreiboperationen zugreifen, sobald der Speicher bereitgestellt ist. Allerdings sind Snapshots und die Replikation erst zulässig, wenn das Erstellen der Datenkopie vom ursprünglichen Datenträger auf den Duplikatdatenträger abgeschlossen ist. Wenn das Erstellen der Datenkopie abgeschlossen ist, kann der Duplikatdatenträger als vollständig unabhängiger Datenträger verwaltet und verwendet werden. 

Diese Funktion ist an den meisten Standorten verfügbar. Klicken Sie [hier](new-ibm-block-and-file-storage-location-and-features.html), um zur Liste der verfügbaren Rechenzentren zu gelangen.

Einige allgemeine Verwendungen für einen duplizierten Datenträger:
- **Disaster Recovery-Test:** Erstellen Sie ein Duplikat Ihres Replikatdatenträgers, um sicherzustellen, dass die Daten intakt sind und bei einer schweren Störung ohne Unterbrechung der Replikation verwendet werden können. 
- **Goldene Kopie:** Verwenden Sie einen Speicherdatenträger als goldene Kopie, von der Sie mehrere Instanzen zu verschiedenen Zwecken erstellen können. 
- **Datenaktualisierungen:** Erstellen Sie eine Kopie Ihrer Produktionsdaten, die zu Testzwecken an Ihre nicht für die Produktion verwendete Umgebung angehängt werden kann. 
- **Wiederherstellung aus Snapshots:** Mithilfe der Snapshotwiederherstellungsfunktion können Sie Daten auf dem ursprünglichen Datenträger mit bestimmten Dateien oder mit einem bestimmten Datum aus einem Snapshot wiederherstellen, ohne den gesamten ursprünglichen Datenträger zu überschreiben. 
- **Entwicklung und Test:** Sie können gleichzeitig bis zu vier simultane Duplikate eines Datenträgers erstellen, um Duplikatdaten zu Entwicklungs- und Testzwecken zu erstellen. 
- **Änderung der Speichergröße:** Sie können einen Datenträger mit der neuen Größe und/oder IOPS-Rate erstellen, ohne Ihre Daten verschieben zu müssen.  
	
Es gibt mehrere Möglichkeiten, einen Duplikatdatenträger über das [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} zu erstellen.


## Duplikat von einem bestimmten Datenträger in der Speicherliste erstellen

1. Rufen Sie Ihre {{site.data.keyword.blockstorageshort}}-Liste auf.
    - Klicken Sie im Kundenportal auf **Speicher** > **{{site.data.keyword.filestorage_short}}** ODER
    - im Katalog von {{site.data.keyword.BluSoftlayer_full}} auf **Infrastruktur** > **Speicher** > **{{site.data.keyword.filestorage_short}}**. 
2. Wählen Sie eine LUN in der Liste aus und klicken Sie auf **Aktionen** > **LUN (Datenträger) duplizieren**. 
3. Wählen Sie Ihre Snapshotoption aus: 
    - Bei Bestellung von einem Datenträger aus, der kein Replikat ist:
      - Wählen Sie die Option **Aus neuem Snapshot erstellen** aus – mit dieser Aktion wird ein Snapshot erstellt, der für das Duplikat zu verwenden ist. Verwenden Sie diese Option, wenn Ihr Datenträger gerade keine Snapshots aufweist oder wenn Sie ein Duplikat genau zu diesem Zeitpunkt erstellen wollen.</br> 
      - Wählen Sie die Option **Aus letztem Snapshot erstellen** aus – mit dieser Aktion wird ein Duplikat aus dem letzten Snapshot erstellt, der für den betreffenden Datenträger vorhanden ist. 
    - Bei Bestellung von einem Replikatdatenträger aus – hier ist nur die Snapshotoption der Verwendung des letzten Snapshots verfügbar. 
4. Speichertyp und Position bleiben die gleichen wie beim ursprünglichen Datenträger.
5. Stündliche oder monatliche Rechnungsstellung – Sie können die duplizierte LUN mit stündlicher oder monatlicher Rechnungsstellung bereitstellen. Der Rechnungsstellungstyp für den ursprünglichen Datenträger wird automatisch ausgewählt. Wenn Sie einen anderen Rechnungsstellungstyp für Ihren Duplikatspeicher auswählen möchten, können Sie diese Auswahl hier treffen. 
5. Wenn Sie möchten, können Sie die E/A-Operationen pro Sekunde (IOPS) oder die IOPS-Stufe für den neuen Datenträger angeben. Standardmäßig wird der IOPS-Wert des ursprünglichen Datenträgers festgelegt. Die verfügbaren Performance- und Größenkombinationen werden angezeigt.
    - Wenn der ursprüngliche Datenträger die Endurance-Stufe von 0,25 IOPS hat, können Sie keine neue Auswahl angeben. 
    - Wenn der ursprüngliche Datenträger die Endurance-Stufe von 2, 4 oder 10 IOPS hat, können Sie für den neuen Datenträger einen beliebigen Wert zwischen diesen Stufen angeben. 
6. Sie können die Größe des neuen Datenträgers so aktualisieren, dass er größer als der ursprüngliche Datenträger wird. Standardmäßig wird die Größe des ursprünglichen Datenträgers festgelegt. 
    - **Anmerkung:** Die {{site.data.keyword.blockstorageshort}}-Speichergröße kann bis auf das Zehnfache der ursprünglichen Größe des Datenträgers erhöht werden. 
7. Sie können den Snapshotbereich für den neuen Datenträger aktualisieren, um mehr, weniger oder keinen Snapshotbereich hinzuzufügen. Standardmäßig wird der Snapshotbereich des ursprünglichen Datenträgers festgelegt. 
8. Klicken Sie auf **Weiter**, um Ihre Bestellung abzuschicken. 


## Duplikat von einem bestimmten Snapshot erstellen

1. Rufen Sie Ihre {{site.data.keyword.blockstorageshort}}-Liste auf.
2. Klicken Sie auf eine(n) **LUN/Datenträger** in der Liste, um die Detailseite anzuzeigen. (Dabei kann es sich um einen Replikatdatenträger oder einen anderen Datenträger handeln.) 
3. Blättern Sie nach unten und wählen Sie einen vorhandenen Snapshot in der Liste auf der Detailseite aus und klicken Sie auf **Aktionen ** > **Duplizieren**.   
4. Der Speichertyp (Endurance oder Leistung) und die Position bleiben mit dem ursprünglichen Datenträger identisch. 
5. Die verfügbaren Performance- und Größenkombinationen werden angezeigt. Standardmäßig wird der IOPS-Wert des ursprünglichen Datenträgers festgelegt. Sie können die E/A-Operationen pro Sekunde (IOPS) oder die IOPS-Stufe für den neuen Datenträger angeben. 
    - Wenn der ursprüngliche Datenträger die Endurance-Stufe von 0,25 IOPS hat, können Sie keine neue Auswahl angeben. 
    - Wenn der ursprüngliche Datenträger die Endurance-Stufe von 2, 4 oder 10 IOPS hat, können Sie für den neuen Datenträger einen beliebigen Wert zwischen diesen Stufen angeben. 
6. Sie können die Größe des neuen Datenträgers so aktualisieren, dass er größer als der ursprüngliche Datenträger wird. Standardmäßig wird die Größe des ursprünglichen Datenträgers festgelegt. 
    - **Anmerkung:** Die {{site.data.keyword.blockstorageshort}}-Speichergröße kann bis auf das Zehnfache der ursprünglichen Größe des Datenträgers erhöht werden. 
7. Sie können den Snapshotbereich für den neuen Datenträger aktualisieren, um mehr, weniger oder keinen Snapshotbereich hinzuzufügen. Standardmäßig wird der Snapshotbereich des ursprünglichen Datenträgers festgelegt. 
8. Klicken Sie auf **Weiter**, um Ihre Bestellung für das Duplikat abzuschicken. 


## Duplizierten Datenträger verwalten

Während Daten vom ursprünglichen Datenträger auf den Duplikatdatenträger kopiert werden, wird auf der Detailseite ein Status mit der Information angezeigt, dass die Duplizierung in Bearbeitung ist. Während dieses Zeitraums können Sie den Datenträger an einen Host anhängen und Lese-/Schreiboperationen auf dem Datenträger durchführen, Sie können jedoch keine Zeitpläne für Snapshots erstellen. Nach Abschluss des Duplizierungsprozesses ist der neue Datenträger unabhängig vom ursprünglichen Datenträger und kann ganz normal mit Snapshots und Replikationen verwaltet werden. 
