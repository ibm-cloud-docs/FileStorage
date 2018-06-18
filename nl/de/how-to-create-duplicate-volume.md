---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Duplikat eines {{site.data.keyword.filestorage_short}}-Datenträgers erstellen

Es besteht die Möglichkeit, von einem vorhandenen {{site.data.keyword.filestorage_full}}-Datenträger ein Duplikat zu erstellen. Der Duplikatdatenträger übernimmt standardmäßig die Kapazitäts- und Leistungsoptionen des ursprünglichen Datenträgers und enthält eine Kopie der Daten bis zum Zeitpunkt eines Snapshots.   
Da der Duplikatdatenträger auf den Daten eines zeitpunktgesteuerten Snapshots basiert, ist auf dem ursprünglichen Datenträger Snapshotbereich erforderlich, bevor ein Duplikat erstellt werden kann. Weitere Informationen zu Snapshots sowie zur Bestellung von Snapshotbereich finden Sie in der [Dokumentation zu Snapshots](snapshots.html).

Duplikate können sowohl von primären Datenträgern als auch von Replikatdatenträgern erstellt werden, wobei das neue Duplikat im selben Rechenzentrum wie der ursprüngliche Datenträger erstellt wird. Wenn Sie zum Beispiel einen Duplikatdatenträger von einem Replikatdatenträger erstellen, wird der neue Datenträger im selben Rechenzentrum wie der Replikatdatenträger erstellt.    

Auf Duplikatdatenträger kann ein Host für Lese-/Schreiboperationen zugreifen, sobald der Speicher bereitgestellt ist. Snapshots und die Replikation sind erst zulässig, wenn das Erstellen der Datenkopie vom ursprünglichen Datenträger auf den Duplikatdatenträger abgeschlossen ist. 

Wenn das Erstellen der Datenkopie abgeschlossen ist, kann der Duplikatdatenträger als vom Original vollständig unabhängiger Datenträger verwaltet und verwendet werden. 

Dieses Feature ist nur für Speicher verfügbar, der mit Verschlüsselung bereitgestellt wird. Klicken Sie [hier](new-ibm-block-and-file-storage-location-and-features.html), um zu einer Liste der verfügbaren Rechenzentren zu gelangen. 

Einige allgemeine Verwendungen für einen duplizierten Datenträger:
  - **Disaster Recovery-Test:** Erstellen Sie ein Duplikat Ihres Replikatdatenträgers, um zu prüfen, ob die Daten intakt sind und im Fall einer schweren Störung ohne Unterbrechung der Replikation verwendet werden können. 
  - **Goldene Kopie:** Verwenden Sie einen Speicherdatenträger als goldene Kopie, von der Sie mehrere Instanzen zu verschiedenen Zwecken erstellen können. 
  - **Datenaktualisierung:** Erstellen Sie eine Kopie Ihrer Produktionsdaten, die zu Testzwecken an Ihre nicht für die Produktion verwendete Umgebung angehängt werden kann. 
  - **Wiederherstellung aus Snapshots:** Mithilfe einer Snapshotwiederherstellungsfunktion können Sie Daten auf dem ursprünglichen Datenträger mit bestimmten Dateien oder mit einem bestimmten Datum aus einem Snapshot wiederherstellen, ohne den gesamten ursprünglichen Datenträger zu überschreiben. 
  - **Entwicklung/Test:** Sie können gleichzeitig bis zu vier simultane Duplikate eines Datenträgers erstellen, um Datenträger mit Duplikatdaten zu Entwicklungs- und Testzwecken zu erstellen. 
  - **Änderung der Speichergröße:** Sie können einen Datenträger mit der neuen Größe und/oder IOPS-Kapazität erstellen, ohne eine hostbasierte Migration Ihrer Daten ausführen zu müssen.  
	

Es gibt mehrere Möglichkeiten, um einen Duplikatdatenträger über das [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} zu erstellen: 

## Vorgehensweise zum Erstellen eines Duplikats von einem bestimmten Datenträger in der Speicherliste

Navigieren Sie zu Ihrer Liste von {{site.data.keyword.filestorage_short}}:

Klicken Sie im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} auf **Speicher**, **{{site.data.keyword.filestorage_short}}** oder klicken Sie im Katalog von {{site.data.keyword.BluSoftlayer_full}} auf **Infrastruktur > Speicher > {{site.data.keyword.filestorage_short}}**. 

1.	Wählen Sie einen Datenträger in der Liste aus und klicken Sie auf **Aktionen** > **LUN (Datenträger) duplizieren**. 
2.	Wählen Sie Ihre Snapshotoption aus: 
    -	Bei Bestellung von einem Datenträger aus, der kein Replikat ist:
      -	Wählen Sie die Option **Aus neuem Snapshot erstellen** aus – durch diese Auswahl wird ein neuer Snapshot erstellt, der für das Duplikat verwendet wird. Verwenden Sie diese Option, wenn zurzeit keine Snapshots für Ihren Datenträger vorhanden sind oder wenn Sie ein Duplikat genau zu diesem Zeitpunkt erstellen wollen.
                      ODER 

      -	Wählen Sie die Option **Aus letztem Snapshot erstellen** aus – durch diese Auswahl wird ein Duplikat aus dem letzten Snapshot erstellt, der für den betreffenden Datenträger vorhanden ist. 
    -	Bei Bestellung von einem Replikatdatenträger aus – hier ist nur die Snapshotoption der Verwendung des letzten Snapshots verfügbar. 
3.	Stündliche oder monatliche Rechnungsstellung – Sie können den neuen Duplikatdatenträger mit stündlicher oder monatlicher Rechnungsstellung bereitstellen. Der Rechnungsstellungstyp für den ursprünglichen Datenträger wird automatisch ausgewählt. Wenn Sie jedoch einen anderen Rechnungsstellungstyp für Ihren neuen Duplikatspeicher auswählen möchten, können Sie diese Auswahl hier treffen.
4. 	Speichertyp (Endurance oder Performance) und Position bleiben die gleichen wie beim ursprünglichen Datenträger. 
5.	Wenn Sie möchten, können Sie die E/A-Operationen pro Sekunde (IOPS) oder die IOPS-Stufe für den neuen Datenträger angeben. Standardmäßig wird der IOPS-Wert des ursprünglichen Datenträgers festgelegt. 
      -	Wenn der ursprüngliche Datenträger die Endurance-Stufe von 0,25 IOPS hat, können Sie keine neue Auswahl angeben. 
      -	Wenn der ursprüngliche Datenträger die Endurance-Stufe von 2, 4 oder 10 IOPS hat, können Sie für den neuen Datenträger einen beliebigen Wert zwischen diesen Stufen angeben. 
      -	Die verfügbaren Performance- und Größenkombinationen werden angezeigt. 
6.	Wenn Sie möchten, können Sie die Größe des neuen Datenträgers so aktualisieren, dass er größer als der ursprüngliche Datenträger wird.  Standardmäßig wird die Größe des ursprünglichen Datenträgers festgelegt. 
  	-	**Anmerkung:** Die {{site.data.keyword.filestorage_short}}-Speichergröße kann nur bis auf das Zehnfache der ursprünglichen Größe des Datenträgers erhöht werden. 
7.	Falls gewünscht, können Sie den Snapshotbereich für den neuen Datenträger aktualisieren, um mehr, weniger oder keinen Snapshotbereich hinzuzufügen. Standardmäßig wird der Snapshotbereich des ursprünglichen Datenträgers festgelegt. 
8.	Klicken Sie auf **Weiter**, um Ihre Bestellung für das Duplikat abzuschicken. 



## Vorgehensweise zum Erstellen eines Duplikats aus einem bestimmten Snapshot

Navigieren Sie zu Ihrer Liste von {{site.data.keyword.filestorage_short}}:

1.	Klicken Sie auf eine(n) **LUN/Datenträger** in der Liste, um die Detailseite anzuzeigen. (Dabei kann es sich um einen Replikatdatenträger oder einen anderen Datenträger handeln.) 
2.	Blättern Sie nach unten und wählen Sie einen vorhandenen Snapshot in der Liste auf der Detailseite aus und klicken Sie auf **Aktionen > Duplizieren**.   
3.	Speichertyp (Endurance oder Performance) und Position bleiben die gleichen wie beim ursprünglichen Datenträger. 
4.	Wenn Sie möchten, können Sie die E/A-Operationen pro Sekunde (IOPS) oder die IOPS-Stufe für den neuen Datenträger angeben. Standardmäßig wird der IOPS-Wert des ursprünglichen Datenträgers festgelegt. 
      - Wenn der ursprüngliche Datenträger die Endurance-Stufe von 0,25 IOPS hat, können Sie keine neue Auswahl angeben. 
      - Wenn der ursprüngliche Datenträger die Endurance-Stufe von 2, 4 oder 10 IOPS hat, können Sie für den neuen Datenträger einen beliebigen Wert zwischen diesen Stufen angeben. 
      - Die verfügbaren Performance- und Größenkombinationen werden angezeigt. 
5.	Wenn Sie möchten, können Sie die Größe des neuen Datenträgers so aktualisieren, dass er größer als der ursprüngliche Datenträger wird.  Standardmäßig wird die Größe des ursprünglichen Datenträgers festgelegt. 
      - **Anmerkung:** Die {{site.data.keyword.filestorage_short}}-Speichergröße kann nur bis auf das Zehnfache der ursprünglichen Größe des Datenträgers erhöht werden. 
6.	Falls gewünscht, können Sie den Snapshotbereich für den neuen Datenträger aktualisieren, um mehr, weniger oder keinen Snapshotbereich hinzuzufügen. Standardmäßig wird der Snapshotbereich des ursprünglichen Datenträgers festgelegt. 
7.	Klicken Sie auf **Weiter**, um Ihre Bestellung für das Duplikat abzuschicken. 


## Verwaltung des Duplikatdatenträgers

Während Daten vom ursprünglichen Datenträger auf den Duplikatdatenträger kopiert werden, wird auf der Detailseite ein Status mit der Information angezeigt, dass die Duplizierung in Bearbeitung ist. Während dieses Zeitraums können Sie den Datenträger an einen Host anhängen und Lese-/Schreiboperationen auf dem Datenträger durchführen, Sie können jedoch keine Zeitpläne für Snapshots erstellen. Nach Abschluss des Duplizierungsprozesses ist der neue Datenträger völlig unabhängig vom ursprünglichen Datenträger und kann ganz normal mit Snapshots und Replikationen usw. verwaltet werden. 
