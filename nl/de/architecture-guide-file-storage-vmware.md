---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-24"

---
{:pre: .pre}
{:new_window: target="_blank"}

# {{site.data.keyword.filestorage_short}} mit VMware bereitstellen

Folgende Schritte zeigen Ihnen, wie {{site.data.keyword.filestorage_full}} in einer Umgebung mit vSphere 5.5 und vSphere 6.0 bei {{site.data.keyword.BluSoftlayer_full}} bestellt und konfiguriert wird. Wenn Sie mehr als acht Verbindungen zu Ihrem VMware-Host benötigen, stellt die Auswahl von NFS {{site.data.keyword.filestorage_short}} das beste Verfahren dar.

{{site.data.keyword.filestorage_short}} wurde zur Unterstützung von Anwendungen mit hoher E/A-Aktivität entwickelt, die vorhersagbare Leistungsniveaus erfordern. Die vorhersagbare Leistung wird durch Zuordnung von E/A-Operationen pro Sekunde (IOPS) zu einzelnen Datenträgern auf Protokollebene erzielt.

Das {{site.data.keyword.filestorage_short}}-Angebot wird über eine NFS-Verbindung angehängt und zugänglich gemacht. In einer VMware-Bereitstellung kann ein einzelner Datenträger mit bis zu 64 ESXi-Hosts als gemeinsam genutzter Speicher durch Mounts verbunden werden. Sie können auch mehrere Datenträger anhängen, um zur Verwendung von vSphere Storage Distributed Resource Scheduler (DRS) einen Speichercluster zu erstellen.

Preis- und Konfigurationsoptionen für Endurance und Performance {{site.data.keyword.filestorage_short}} werden auf Basis einer Kombination aus dem reservierten Speicherbereich und der angebotenen IOPS-Kapazität in Rechnung gestellt.

## Hinweise zur Bestellung

Beachten Sie für die Bestellung von {{site.data.keyword.filestorage_short}} folgende Informationen:

- Berücksichtigen Sie bei der Festlegung der Größe den Umfang der Workload und den benötigten Durchsatz. Für den Endurance-Service, der die Leistung linear in Relation zur Kapazität (IOPS/GB) skaliert, ist Größe wichtig. Der Performance-Service ermöglicht dem Administrator dagegen, Kapazität und Leistung unabhängig voneinander zu wählen. Für den Performance-Service sind wiederum Durchsatzanforderungen relevant.
  >**Anmerkung** - Der Durchsatz wird durch IOPS x 16 KB berechnet. IOPS werden auf der Basis der Blockgröße von 16 KB und einer 50:50-Mischung von Schreib- und Leseoperationen gemessen.<br/>Eine Erhöhung der Blockgröße erhöht den Durchsatz, verringert jedoch die IOPS. Beispiel: Eine Verdoppelung der Blockgröße auf 32-KB-Blöcke behält den maximalen Durchsatz bei, halbiert jedoch die IOPS-Kapazität.
- NFS verwendet viele zusätzliche Dateisteuerungsoperationen wie `lookup`, `getattr` und `readdir`. Diese Operationen können neben Lese- und Schreiboperationen ebenfalls als IOPS gezählt werden und sind je nach Operationstyp und NFS-Version unterschiedlich.
- {{site.data.keyword.filestorage_short}}-Datenträger werden autorisierten Einheiten (Geräten), Teilnetzen oder IP-Adressen zugänglich gemacht.
- Zur Vermeidung einer Speicherverbindungsunterbrechung während des Pfadfailovers empfiehlt {{site.data.keyword.IBM}}, VMware-Tools zu installieren, die einen angemessenen Zeitlimitwert festlegen. Der Wert muss nicht geändert werden; die Standardeinstellung reicht aus, um sicherzustellen, dass Ihr VMware-Host die Konnektivität nicht verliert.
- NFS Version 3 und NFS Version 4.1 werden in der Umgebung von {{site.data.keyword.BluSoftlayer_full}} unterstützt. {{site.data.keyword.IBM}} empfiehlt jedoch, NFS v3 zu verwenden. Da NFS Version 4.1 ein Protokoll mit Zustandsüberwachung (und nicht wie NFSv3 ohne Zustandsüberwachung) ist, können bei Netzereignissen Probleme mit dem Protokoll auftreten. NFS Version 4.1 muss alle Operationen ruhen lassen und anschließend eine Sperrenrückforderung ausführen. Während dieser Operationen kann es zu Unterbrechnungen kommen.

Weitere Informationen finden Sie im Whitepaper von VMware zu [bewährten Verfahren für die Ausführung von
VMware vSphere unter Network Attached Storage](https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/techpaper/vmware-nfs-bestpractices-white-paper-en.pdf).{:new_window}

**Unterstützungsmatrix für das NFS-Protokoll und VMware-Funktionen**
<table>
  <caption>Tabelle 1 zeigt, welche vSphere-Funktionen für die beiden unterschiedlichen Versionen von NFS gelten.</caption>
 <thead>
  <tr>
   <th>vSphere-Funktionen</th>
   <th>NFS Version 3</th>
   <th>NFS Version 4.1</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>vMotion und Storage vMotion</td>
   <td>Ja</td>
   <td>Ja</td>
  </tr>
  <tr>
   <td>Hochverfügbarkeit (HA - High Availability)</td>
   <td>Ja</td>
   <td>Ja</td>
  </tr>
  <tr>
   <td>Fehlertoleranz (FT)</td>
   <td>Ja</td>
   <td>Ja</td>
  </tr>
  <tr>
   <td>Distributed Resource Scheduler (DRS)</td>
   <td>Ja</td>
   <td>Ja</td>
  </tr>
  <tr>
   <td>Hostprofile</td>
   <td>Ja</td>
   <td>Ja</td>
  </tr>
  <tr>
   <td>Speicher-DRS</td>
   <td>Ja</td>
   <td>Nein</td>
  </tr>
  <tr>
   <td>Storage I/O Control (SIOC)</td>
   <td>Ja</td>
   <td>Nein</td>
  </tr>
  <tr>
   <td>Sitewiederherstellungsmanager (Site Recovery Manager)</td>
   <td>Ja</td>
   <td>Nein</td>
  </tr>
  <tr>
   <td>Virtuelle Datenträger</td>
   <td>Ja</td>
   <td>Nein</td>
  </tr>
 </tbody>
</table>
*Quelle - [VMware - NFS-Protokolle und ESXi](https://docs.vmware.com/en/VMware-vSphere/6.0/com.vmware.vsphere.storage.doc/GUID-8A929FE4-1207-4CC5-A086-7016D73C328F.html){:new_window}*



### Snapshots verwenden

{{site.data.keyword.filestorage_short}} gibt Administratoren die Möglichkeit, Snapshotpläne festzulegen, durch die Snapshotkopien für jeden Speicherdatenträger automatisch erstellt und gelöscht werden. Sie können darüber hinaus zuästzliche Snapshotpläne (stündlich, täglich, wöchentlich) für automatische Snapshots erstellen und manuell Ad-hoc-Snapshots für BCDR-Szenarios (BCDR – Business-Continuity/Disaster Recovery) erstellen. Automatische Alerts werden über das [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} an den Datenträgereigner in Bezug auf die aufbewahrten Snapshots und den belegten Speicherplatz zugestellt.

Zur Verwendung von Snapshots ist ein Snapshotbereich erforderlich. Der Speicherbereich kann bei der ersten Datenträgerbestellung oder nach der Erstbereitstellung über die Seite **Datenträgerdetails** durch Klicken auf **Aktionen** und Auswählen der Option **Snapshotbereich hinzufügen** gekauft werden.

Es ist wichtig zu beachten, dass VMware-Umgebungen Snapshots nicht erkennen. Die Endurance {{site.data.keyword.filestorage_short}}-Snapshotfunktionalität darf nicht mit VMware-Snapshots verwechselt werden. Jede Wiederherstellung mithilfe der {{site.data.keyword.filestorage_short}}-Snapshotfunktion muss über das [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} durchgeführt werden.

Zur Wiederherstellung des {{site.data.keyword.filestorage_short}}-Datenträgers ist es erforderlich, alle VMs auf {{site.data.keyword.filestorage_short}} auszuschalten. Der Datenträger muss vorübergehend von den ESXi-Hosts abgehängt werden, um eine Beschädigung von Daten während des Prozesses zu vermeiden.

Weitere Details zum Konfigurieren von Snapshots finden Sie im Artikel [Snapshots](snapshots.html).


### Replikation verwenden

Die Replikation verwendet einen Ihrer Snapshotpläne, um Snapshots automatisch auf einen Zieldatenträger in einem fernen Rechenzentrum zu kopieren. Die Kopien können am fernen Standort wiederhergestellt werden, falls Daten beschädigt werden oder ein Elementarereignis auftritt.

Replikate ermöglichen Folgendes:

- Schnelle Wiederherstellung nach Standortausfällen oder anderen Katastrophen durch Failover auf den Zieldatenträger
- Failover auf einen bestimmten Zeitpunkt in der Disaster Recovery-Kopie (DR-Kopie)

Bevor Sie replizieren können, müssen Sie einen Snapshotplan erstellen.

Wenn Sie einen Failover durchführen, 'schalten Sie um', und zwar von Ihrem Speicherdatenträger in Ihrem primären Rechenzentrum auf den Zieldatenträger in Ihrem fernen Rechenzentrum. Ihr primäres Rechenzentrum ist zum Beispiel in London und Ihr sekundäres Rechenzentrum ist in Amsterdam. Wenn ein Fehlerereignis auftritt, führen Sie ein Failover auf Amsterdam durch. Dazu stellen Sie zu dem jetzigen primären Datenträger eine Verbindung von einer vSphere Cluster-Instanz in Amsterdam her. Nachdem Ihr Datenträger in London repariert wurde, wird ein Snapshot des Datenträgers in Amsterdam erstellt. Sie können anschließend von einer Compute-Instanz in London eine Rückübertragung auf den Datenträger in London und den nun wieder primären Datenträger durchführen.

Bevor der Datenträger wieder auf das primäre Rechenzentrum zurückübertragen wird, muss seine Verwendung am fernen Standort gestoppt werden. Ein Snapshot neuer oder geänderter Informationen wird erstellt und an das pirmäre Rechenzentrum repliziert, bevor der Datenträger wieder an die ESXi-Hosts am Produktionsstandort angehängt werden kann.

Weitere Informationen zum Konfigurieren von Replikaten finden Sie im Abschnitt [Replikation](replication.html).

>**Anmerkung** - Ungültige Daten, seien es beschädigte, gehackte oder infizierte, werden dem Snapshotplan und der angegebenen Snapshotaufbewahrung entsprechend repliziert. Durch Verwendung der kleinsten Replikationsfenster lässt sich ein besseres Ziel in Bezug auf den maximal tolerierbaren Datenverlust bei einem Ausfall (Recovery Point Objective, RPO) realisieren. Dadurch steht möglicherweise jedoch weniger Zeit für die Reaktion auf die Replikation ungültiger Daten zur Verfügung.


## {{site.data.keyword.filestorage_short}} bestellen

Sie können {{site.data.keyword.filestorage_short}} für eine VMware ESXi 5-Umgebung bestellen und konfigurieren. Verwenden Sie folgende Informationen zusammen mit [Advanced Single-Site VMware Reference Architecture](https://console.bluemix.net/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){:new_window}, um eine dieser Speicheroptionen in Ihrer VMware-Umgebung einzurichten.

{{site.data.keyword.filestorage_short}} kann über das [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} bestellt werden. Greifen Sie zu diesem Zweck auf die {{site.data.keyword.filestorage_short}}-Seite über die Optionen **Speicher** > **{{site.data.keyword.filestorage_short}}** zu.


### 1. 

 bestellenFühren Sie die folgenden Schritte aus, um {{site.data.keyword.filestorage_short}} zu bestellen:
1. Klicken Sie auf **Speicher** > **{{site.data.keyword.filestorage_short}}** auf der [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}-Startseite.
2. Klicken Sie auf **{{site.data.keyword.filestorage_short}} bestellen** auf der Seite **{{site.data.keyword.filestorage_short}}**.
3. Wählen Sie **Endurance**/**Performance** in der Liste **Speichertyp auswählen** aus.
4. Wählen Sie die Position aus. Datenzentren mit verbesserten Funktionen sind mit einem Stern gekennzeichnet. Stellen Sie sicher, dass der neue Speicher an derselben Position wie der zuvor bestellte ESXi-Host hinzugefügt wird.
5. Wählen Sie die Rechnungsstellungsmethode aus. Die Optionen 'Monatliche' und 'Stündliche' Rechnungsstellung sind verfügbar.
6. Wählen Sie die Größe des Speicherbereichs in GB aus. Bei TB ist 1 TB gleich 1.000 GB und 12 TB sind gleich 12.000 GB.
7. Geben Sie den Wert für IOPS (E/A-Operationen pro Sekunde) in Intervallen von 100 ein oder wählen Sie eine IOPS-Stufe aus.
8. Geben Sie die Größe des Snapshotbereichs an.
9. Klicken Sie auf **Weiter**.
10. Geben Sie einen Werbeaktionscode ein und klicken Sie auf **Neu berechnen**.
11. Prüfen Sie Ihre Bestellung.
12. Wählen Sie das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen und bin mit den darin genannten Bedingungen einverstanden** aus.
13. Klicken Sie auf **Bestellung abschicken**, um die Bestellung abzuschicken, oder auf **Abbrechen**, um das Formular zu schließen, ohne die Bestellung abzuschicken.

Der Speicher wird in weniger als einer Minute bereitgestellt und auf der Seite **{{site.data.keyword.filestorage_short}}** im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} angezeigt.



### 2. Hosts für die Verwendung von {{site.data.keyword.filestorage_short}} autorisieren

Wenn ein Datenträger bereitgestellt wird, müssen {{site.data.keyword.BluBareMetServers_full}} oder {{site.data.keyword.BluVirtServers_full}}, die den Datenträger verwenden sollen, für den Zugriff auf den Speicher autorisiert werden. Führen Sie die folgenden Schritte aus, um die Autorisierung für den Datenträger einzurichten:

1. Klicken Sie auf **Speicher** > **{{site.data.keyword.filestorage_short}}**.
2. Wählen Sie die Option **Auf Host zugreifen** im Menü **Aktionen für Endurance-Datenträger** bzw. **Aktionen für Performance-Datenträger** aus.
3. Klicken Sie auf **Teilnetze**.
4. Wählen Sie in der Liste der verfügbaren Teilnetze, die den VMkernel-Ports auf den ESXi-Hosts zugeordnet sind, aus und klicken Sie auf **Abschicken**.<br/>
    >**Anmerkung** - Die angezeigten Teilnetze sind abonnierte Teilnetze in demselben Rechenzentrum wie der Speicherdatenträger.


Wenn die Teilnetze autorisiert sind, notieren Sie den Hostnamen des Endurance- oder Performance-Speicherservers, den Sie beim Anhängen des Datenträgers verwenden wollen. Diese Information ist auf der {{site.data.keyword.filestorage_short}}-Detailseite durch Klicken auf einen bestimmten Datenträger zu finden.


##  Host für virtuelle VMware-Maschinen konfigurieren

### Voraussetzungen erfüllen

Stellen Sie vor Beginn des VMware-Konfigurationsprozesses sicher, dass die folgenden Voraussetzungen erfüllt sind:

- {{site.data.keyword.BluBareMetServers}} mit VMware ESXi wurde mit der entsprechenden Konfiguration und den ESXi-Anmeldeberechtigungsnachweisen bereitgestellt.
- {{site.data.keyword.BluSoftlayer_full}} Windows ist physisch vorhanden oder {{site.data.keyword.virtualmachinesshort}} im selben Rechenzentrum wie die {{site.data.keyword.BluBareMetServers}}. Dies schließt die öffentliche IP-Adresse der {{site.data.keyword.BluSoftlayer_full}} Windows VM und die Anmeldeberechtigungsnachweise ein.
- Ein Computer mit Internetzugang und mit der Web-Browser-Software sowie mit einem installierten RDP-Client (RDP - Remote Desktop Protocol) ist verfügbar.


### 1. VMware-Host konfigurieren

1. Starten Sie auf einem mit dem internet verbundenen Computer einen RDP-Client und richten Sie eine RDP-Sitzung mit {{site.data.keyword.BluVirtServers_full}} ein, das in demselben Rechenzentrum bereitgestellt ist, in dem vSphere vCenter installiert ist.
2. Starten Sie in {{site.data.keyword.BluVirtServers_short}} einen Web-Browser und stellen Sie eine Verbindung zu VMware vCenter über den vSphere Web Client her.
3. Wählen Sie auf der Hauptanzeige (**HOME**) die Option für Hosts und Cluster (**Hosts and Clusters**) aus. Erweitern Sie das Teilfenster auf der linken Seite und wählen Sie den **VMware-ESXi-Server** aus, der für diese Bereitstellung verwendet werden soll.
4. Stellen Sie sicher, dass der Firewall-Port für den NFS-Client auf allen Hosts geöffnet ist, damit Sie den NFS-Client auf dem vSphere-Host konfigurieren können. Dieser Port wird in den zuletzt veröffentlichen Releases von vSphere automatisch geöffnet. Wechseln Sie zum Prüfen, ob der Port geöffnet ist, zur Registerkarte für die ESXi-Hostverwaltung (**ESXi host Manage**) in VMware® vCenter™, wählen Sie **Settings** (Einstellungen) und anschließend **Security Profile** (Sicherheitsprofil) aus. Klicken Sie im Abschnitt **Firewall** auf **Edit** (Bearbeiten) und blättern Sie nach unten zu **NFS Client**.
5. Stellen Sie sicher, dass **eine Verbindung über eine beliebige IP-Adresse oder aus einer angegebenen Liste von IP-Adressen** zugelassen wird. <br/>
   ![Verbindung zulassen](/images/1_4.png)
6. Konfigurieren Sie Jumbo-Frames, indem Sie zur Registerkarte **ESXi host Manage** wechseln und die Optionen **Manage** (Verwalten) und **Networking** (Netz) auswählen.
7. Wählen Sie **VMkernel adapters** (VM-Kernel-Adapter) aus, heben Sie den Eintrag **vSwitch** hervor und klicken Sie auf **Edit** (Bearbeiten - Stiftsymbol).
8. Wählen Sie **NIC setting** (NIC-Einstellung) aus und stellen Sie sicher, dass NIC MTU auf den Wert 9000 gesetzt ist.
9. **Optional**. Überprüfen Sie die Einstellungen für Jumbo-Frames.
   - Unter Windows: `ping -f -l 8972 a.b.c.d`
   - Unter UNIX: `ping -s 8972 a.b.c.d`
     Dabei ist a.b.c.d die benachbarte {{site.data.keyword.BluVirtServers_short}}-Schnittstelle.
     Beispiel
     ```
     ping a.b.c.d (a.b.c.d) 8972(9000) bytes of data.
     8980 bytes from a.b.c.d: icmp_seq=1 ttl=128 time=3.36 ms
     ```

Weitere Informationen zu VMware und Jumbo-Frames finden Sie [hier](https://kb.vmware.com/s/article/1003712){:new_window}.


### 2. Uplink-Adapter zum virtuellen Switch hinzufügen

1. Konfigurieren Sie einen neuen Uplink-Adapter, indem Sie zur Registerkarte **ESXi host Manage** wechseln und die Optionen **Manage** und **Networking** auswählen.
2. Wählen Sie die Registerkarte **Physical adapters** (Physische Adapter) aus.
3. Klicken Sie auf die Option **Add host networking** (Hostnetz hinzufügen - Globussymbol mit Pluszeichen).
4. Wählen Sie für den Verbindungstyp die Option **Physical Network Adapter** (Physischer Netzadapter) und klicken Sie auf **Next** (Weiter).
5. Wählen Sie den vorhandenen **vSwitch** aus und klicken Sie auf **Next**.
6. Wählen Sie **Unused adapters** (Nicht verwendete Adapter) aus und klicken Sie auf **Add adapters** (Adapter hinzufügen - Pluszeichen).
7. Klicken Sie auf den anderen "verbundenen" Adapter und klicken Sie auf **OK**. <br/>
   ![Physische Adapter zum Switch hinzufügen](/images/2_3.png)
8. Klicken Sie auf **Next** (Weiter) und anschließend **Finish** (Fertigstellen).
9. Navigieren Sie zur Registerkarte **Virtual Switches** zurück und wählen Sie das obere Symbol **Einstellung bearbeiten** (Stiftsymbol) unter der Überschrift **Virtual Switches** aus.
10. Wählen Sie auf der linken Seite den vSwitch-Eintrag **Teaming and Failover** aus.
11. Stellen Sie sicher, dass die Option **Load balancing** (Lastverteilung) auf **Route based on the originating virtual port** (Route auf Basis des virtuellen Herkunftsports) und klicken Sie auf **OK**.


### 3. Statisches ESXi-Routing konfigurieren (optional)

Die Netzkonfiguration für diesen Architekturleitfaden arbeitet mit einer minimalen Anzahl von Portgruppen. Handelt es sich um eine VMkernel-Portgruppe für NFS-Speicher, müssen zusätzliche Schritte ausgeführt werden. ESXi verwendet standardmäßig den VMkernel-Port, der sich im gleichen Teilnetz wie ein NFS-Datenträger befindet, um den NFS-Datenträger an einen Endurance/Performance-Speicher anzuhängen. Da Routing der Schicht 3 (Layer 3 Routing) zum Anhängen des NFS-Datenträgers verwendet wird, muss ESXi veranlasst werden, den VMkernel-Port zu verwenden, der zum Anhängen des NFS-Datenträgers konfiguriert wurde. Dazu muss eine statische Route zum Endurance/Performance-Speicherarray erstellt werden.

1. Zum Konfigurieren einer statischen Route stellen Sie eine SSH-Verbindung zu jedem ESXi-Host her, der Performance- oder Endurance-Speicher verwendet, und führen die folgenden Befehle aus. Beachten Sie die IP-Adresse, die mit dem Befehl `ping` (dem ersten Befehl) generiert wird, und verwenden Sie sie mit dem Netzbefehl `esxcli`.
   ```
   ping <Hostname des Endurance-/Performance-Arrays>
   ```
   {: pre}

   >**Anmerkung** - Der DNS-Hostname des NFS-Speichers ist eine Weiterleitungszone (Forwarding Zone - FZ), der mehrere IP-Adressen zugeordnet sind. Diese IP-Adressen sind statisch und gehören zu diesem speziellen DNS-Hostnamen. Jede dieser IP-Adressen kann für den Zugriff auf einen bestimmten Datenträger verwendet werden.

   ```
   esxcli network ip route ipv4 add –gateway GATEWAYIP –network <Ergebnis des Pingbefehls>/32
   ```
   {: pre}

2. Statische Routen bleiben unter ESXi 5.0 und früheren Versionen nicht über Neustarts hinweg bestehen. Zur Sicherstellung, dass alle hinzugefügten statischen Routen persistent bestehen bleiben, muss dieser Befehl in der Datei `local.sh` auf jedem Host im Verzeichnis `/etc/rc.local.d/` hinzugefügt werden. Öffnen Sie die Datei `local.sh` mit dem visuellen Editor und fügen Sie den zweiten Befehl in Schritt 4.1 vor der Zeile `exit 0` hinzu.

>**Anmerkungen**<br/>- Notieren Sie die IP-Adresse, da sie zum Anhängen des Datenträgers im nächsten Schritt verwendet werden kann.<br/>Dies muss für jeden NFS-Datenträger ausgeführt werden, der an Ihren ESXi-Host angehängt werden soll.<br/>Weitere Informationen finden Sie im folgend Artikel zu VMware KB: [Statische Routen für VMkernel-Ports auf einem ESXi-Host konfigurieren (englisch)](https://kb.vmware.com/s/article/2001426){:new_window}.


##  {{site.data.keyword.filestorage_short}}-Datenträger erstellen und an ESXi-Hosts anhängen

1. Klicken Sie auf das Symbol **Go to vCenter** (Zu vCenter wechseln) und anschließend auf **Hosts and Clusters** (Hosts und Cluster).
2. Klicken Sie auf der Registerkarte **Related Object** (Zugehöriges Objekt) auf **Datastores** (Datenspeicher).
3. Klicken Sie auf das Symbol **Create a new datastore** (Neuen Datenspeicher erstellen).
4. Wählen Sie auf der Anzeige **New Datastore** (Neuer Datenspeicher) die Position des WMware-Datenspeichers (Ihren ESXi-Host) aus und klicken Sie auf **Next** (Weiter).
5. Wählen Sie auf der Anzeige **Type** (Typ) die Option **NFS** aus und klicken Sie auf **Next** (Weiter).
6. Geben Sie auf der Anzeige **Name and configuration** (Name und Konfiguration) den Namen ein, den Sie dem WMware-Datenspeicher geben möchten. Geben Sie außerdem den Hostnamen des NFS-Servers ein. Die Verwendung des FQDN für den NFS-Server sorgt für die beste Datenverkehrsverteilung an den zugrunde liegenden Server. Die IP-Adresse ist ebenfalls gültig, wird jedoch weniger häufig und nur in bestimmten Instanzen verwendet. Geben Sie den Ordnernamen in der Form `/foldername` ein.
7. Wählen Sie auf der Anzeige **Host accessibility** (Hostzugänglichkeit) einen oder mehrere Hosts aus, an die Sie den NFS-WMware-Datenspeicher anhängen wollen, und klicken Sie auf **Next** (Weiter).
8. Prüfen Sie die Eingaben auf der nächsten Anzeige und klicken Sie auf **Finish** (Fertigstellen).
9. Wiederholen Sie diese Schritte für alle weiteren {{site.data.keyword.filestorage_short}}-Datenträger.

>**Anmerkung** - Für {{site.data.keyword.BluSoftlayer_full}} wird empfohlen, FQDN-Namen für die Verbindung zum WMware-Datenspeicher zu verwenden. Durch die direkte Verwendung von IP-Adressen könnte der Lastausgleichsmechanismus, der durch die Verwendung von FQDN-Namen bereitgestellt wird, umgangen werden.

Setzen Sie zur Verwendung der IP-Adresse anstelle des FQDN einfach einen Pingbefehl an den Server ab, um die IP-Adresse abzurufen.
```
ping <Hostname des Endurance-/Performance-Arrays>
```
{: pre}

Verwenden Sie den folgenden Befehl, um die IP-Adresse von einem ESXi-Host abzurufen. Die zurückgegebene Adresse 10.2.125.80 ist die IP-Adresse des FQDN.
```
~ # vmkping nfsdal0902a-fz.service.softlayer.com
PING nfsdal0902a-fz.service.softlayer.com (10.2.125.80): 56 data bytes
64 bytes from 10.2.125.80: icmp_seq=0 ttl=253 time=0.187 ms
```


## Storage I/O Control für ESXi aktivieren (optional)

Storage I/O Control (SIOC) ist ein Feature, das für Kunden mit einer Enterprise Plus-Lizenz verfügbar ist. Wenn SIOC in der Umgebung aktiviert wird, ändert sich dadurch die Einheitenwarteschlangenlänge für die einzelne VM. Die Änderung der Einheitenwarteschlangenlänge verkleinert die Speicherarray-Warteschlange für alle VMs auf einen gleichen Anteil. SIOC greift nur ein, wenn Ressourcen beschränkt sind und die E/A-Latenz des Speichers über einem definierten Schwellenwert liegt.


Zur Feststellung, wann eine Speichereinheit ausgelastet oder eingeschränkt ist, benötigt SIOC einen definierten Schwellenwert. Die Latenz für den Überlastungsschwellenwert ist für verschiedene Speichertypen unterschiedlich. Die Standardauswahl gibt 90% des Spitzendurchsatzes an. Der Prozentsatz des Spitzendurchsatzwerts gibt den geschätzten Latenzschwellenwert an, wenn der WMware-Datenspeicher diesen Prozentsatz seines geschätzten Spitzendurchsatzes verwendet.


>**Anmerkung** - Eine falsche Konfiguration von SIOC für einen WMware-Datenspeicher oder für eine VMDK kann die Leistung erheblich beeinträchtigen.


### Storage I/O Control für einen WMware-Datenspeicher konfigurieren

Führen Sie die folgenden Schritte aus, um SIOC mit den empfohlenen Werten für Endurance- und Performance-Speicher zu aktivieren:

1. Navigieren Sie zu dem WMware-Datenspeicher im vSphere Web Client-Navigator.
2. Klicken Sie auf die Registerkarte **Manage** (Verwalten).
3. Klicken Sie auf **Settings** (Einstellungen) und dann auf **General** (Allgemein).
4. Klicken Sie für **Datastore Capabilities** (Datenspeicherfunktionen) auf **Edit** (Bearbeiten).
5. Wählen Sie das Kontrollkästchen **Enable Storage I/O Control** aus.<br/>
   ![NSF-WMware-Datenspeicher](/images/3_0.png)
6. Klicken Sie auf **OK**.

**Anmerkung:** Diese Einstellung ist für den WMware-Datenspeicher, nicht für den Host spezifisch.


### Storage I/O Control für {{site.data.keyword.BluVirtServers_short}} konfigurieren

Mit SIOC können Sie auch einzelne virtuelle Platten für einzelne VMs begrenzen oder ihnen unterschiedliche Anteile (Shares) zuteilen. Die Begrenzung von Platten und die Zuteilung unterschiedlicher Anteile geben Ihnen die Möglichkeit, die Umgebung auf die Workload mit dem IOPS-Wert des erhaltenen {{site.data.keyword.filestorage_short}}-Datenträgers abzustimmen und auszurichten. Die Begrenzung wird durch E/A-Operationen pro Sekunde (IOPS) festgelegt und es ist möglich, eine andere Gewichtung oder Anteile ("Shares") festzulegen. Virtuelle Platten mit Anteilen, die auf High (2.000 Anteile) gesetzt sind, empfangen zweimal so viele E/A-Operationen wie eine Platte, die auf Normal (1.000 Anteile) gesetzt ist, und viermal so viele E/A-Operationen wie eine Platte, die auf Low (500 Anteile) gesetzt ist. Normal ist der Standardwert für alle VMs, sodass Sie die Werte über oder unter Normal für die VMs anpassen müssen, für die dies erforderlich ist.

Führen Sie die folgenden Schritte aus, um die VDisk-Anteile (Shares) und die Begrenzung zu ändern.

1. Wählen Sie einen {{site.data.keyword.BluVirtServers_short}}-Speicher in der Bestandsliste **VMs and Templates** (VMs und Vorlagen) aus.
2. Wählen Sie {{site.data.keyword.BluVirtServers_short}} for I/O Control aus.
3. Klicken Sie auf die Registerkarte **Manage** und klicken Sie auf **Settings**. Klicken Sie auf **Edit** (Bearbeiten).
4. Erweitern Sie den Pfeil **Hard Disk** (Festplatte). Wählen Sie den Wert für **Modify the Shares** (Anteile ändern) oder **Limit - IOPS** (Begrenzungs-IOPS) aus, der für Ihre Umgebung angemessen ist. Wählen Sie eine virtuelle Festplatte in der Liste aus und ändern Sie den Abschnitt für Anteile (Shares), um die relative Anzahl an Anteilen auszuwählen, die dem {{site.data.keyword.BluVirtServers_short}}-Speicher zugeordnet werden soll (Low, Normal oder High). Sie können auch auf **Custom** klicken und einen benutzerdefinierten Anteilswert eingeben.
5. Klicken Sie auf die Spalte **Limit - IOPS** und geben Sie den maximalen Wert der Speicherressourcen ein, die der virtuellen Maschine zugeordnet werden sollen.
6. Klicken Sie auf  **OK**.


> **Anmerkung:** Durch diese Vorgehensweise werden Begrenzungen für die Ressourcennutzung einzelner virtueller Platten (vDisks) in einem {{site.data.keyword.BluVirtServers_short}}-Speicher festgelegt, auch wenn SIOC nicht aktiviert ist. Diese Einstellungen sind für den einzelnen Gast spezifisch, nicht für den Host, obwohl sie von SIOC verwendet werden.


## Einstellungen auf der ESXi-Hostseite konfigurieren

Zum Konfigurieren von ESXi 5.x-Hosts für NFS-Speicher sind einige zusätzliche Einstellungen erforderlich. In der nachfolgenden Tabelle sind die korrekten Werte für die einzelnen Einstellungen aufgeführt.

|Parameter | Einstellung... |
|----------|------------|
|Net.TcpipHeapSize |	32 |
|Net.TcpipHeapMax |	Für vSphere 5.0/5.1: 128 <br/> Für vSphere 5.5 oder höher: 512 |
|NFS.MaxVolumes |	256 |
|NFS41.MaxVolumes |	256 (nur vSphere 6.0 oder höher) |
|NFS.HeartbeatMaxFailures |	10 |
|NFS.HeartbeatFrequency |	12 |
|NFS.HeartbeatTimeout |	5 |
|NFS.MaxQueueDepth|	64 |


### Erweiterte Konfigurationsparameter auf dem ESXi 5.x-Host über die CLI aktualisieren

In den folgenden Beispielen werden die erweiterten Konfigurationsparameter über die CLI festgelegt und anschließend geprüft. Das Tool `esxcfg-advcfg` in den Beispielen befindet sich auf den ESXi 5.x-Hosts im Verzeichnis `/usr/sbin`.

   - Einstellen der erweiterten Konfigurationsparameter über die ESXi 5.x-Befehlszeilenschnittstelle.

     ```
     #esxcfg-advcfg -s 32 /Net/TcpipHeapSize
     #esxcfg-advcfg -s 128 /Net/TcpipHeapMax(For vSphere 5.0/5.1)
     #esxcfg-advcfg -s 512 /Net/TcpipHeapMax(For vSphere 5.5 und höher)
     #esxcfg-advcfg -s 256 /NFS/MaxVolumes
     #esxcfg-advcfg -s 256 /NFS41/MaxVolumes (ESXi 6.0 und höher)
     #esxcfg-advcfg -s 10 /NFS/HeartbeatMaxFailures
     #esxcfg-advcfg -s 12 /NFS/HeartbeatFrequency
     #esxcfg-advcfg -s 5 /NFS/HeartbeatTimeout   
     #esxcfg-advcfg -s 64 /NFS/MaxQueueDepth
     #esxcfg-advcfg -s 32 /Disk/QFullSampleSize
     #esxcfg-advcfg -s 8 /Disk/QFullThreshold
     ```

  - Prüfen der erweiterten Konfigurationsparameter über die ESXi 5.x-Befehlszeilenschnittstelle.

    ```
    #esxcfg-advcfg -g /Net/TcpipHeapSize
   #esxcfg-advcfg -g /Net/TcpipHeapMax
   #esxcfg-advcfg -g /NFS/MaxVolumes
   #esxcfg-advcfg -g /NFS41/MaxVolumes (ESXi 6.0 und höher)
   #esxcfg-advcfg -g /NFS/HeartbeatMaxFailures
   #esxcfg-advcfg -g /NFS/HeartbeatFrequency
   #esxcfg-advcfg -g /NFS/HeartbeatTimeout
   #esxcfg-advcfg -g /NFS/MaxQueueDepth
   #esxcfg-advcfg -g /Disk/QFullSampleSize
   #esxcfg-advcfg -g /Disk/QFullThreshold
    ```

## Jumbo-Frames in {{site.data.keyword.BluSoftlayer_notm}} für Windows und Linux aktivieren

Ein Jumbo-Frame ist ein Ethernet-Rahmen mit einem Nutzdatenvolumen, das größer als die standardmäßig maximale Übertragungseinheit (MTU - Maximum Transmission Unit) von 1.500 Byte ist. Jumbo-Frames werden in LAN-Netzen verwendet, die mindestens 1 Gb/s unterstützen, und können eine Größe von bis zu 9.000 Byte haben.

Jumbo-Frames müssen im gesamten Netzpfad von der Quelleneinheit <-> Switch <-> Router <-> Switch <-> Zieleinheit identisch konfiguriert werden. Wenn die Kette nicht insgesamt identisch konfiguriert ist, wird standardmäßig die niedrigste Einstellung innerhalb der Kette verwendet. {{site.data.keyword.BluSoftlayer_full}} hat die Netzeinheiten derzeit auf 9.000 eingestellt. Alle Kundeneinheiten müssen auf denselben Wert eingestellt werden: 9.000.

### Jumbo-Frames in Windows aktivieren

1. Öffnen Sie das **Center für Netze und gemeinsame Nutzung**.
2. Klicken Sie auf **Adaptereinstellungen ändern**.
3. Klicken Sie mit der rechten Maustaste auf die Netzadapterkarte (NIC), für die Sie Jumbo-Frames aktivieren wollen, und wählen Sie **Eigenschaften** aus.
4. Klicken Sie auf der Registerkarte **Netz** auf **Konfigurieren** für den Netzadapter.
5. Wählen Sie die Registerkarte **Erweitert** aus.
6. Wählen Sie **Großrahmen** aus und ändern Sie den Wert von **Deaktiviert** in den gewünschten Wert. Der Wert, beispielsweise 9 KB oder 9.014 Byte, richtet sich nach der Netzadapterkarte.
7. Klicken Sie in allen Fenstern auf **OK**.

>**Anmerkung** - Die Netzkonnektivität der Netzadapterkarte (NIC) wird bei Änderungen für ein paar Sekunden unterbrochen. Starten Sie das Gerät erneut, um sich zu vergewissen, dass die Änderung wirksam wurde.


### Jumbo-Frames in Linux aktivieren

1. Bearbeiten Sie die Netzkonfigurationsdatei für die Schnittstelle 'eth0'.
   - Benutzer von CentOS/RHEL/Fedora Linux bearbeiten `/etc/sysconfig/network-script/ifcfg-eth0`.
     ```
     # vi /etc/sysconfig/network-script/ifcfg-eth0
     ```
     {: pre}

   - Benutzer von Debian/Ubuntu Linux bearbeiten `/etc/network/interfaces`.

2. Hängen Sie die folgende Konfigurationsanweisung an, die die Größe des Rahmens in Byte angibt.
   - CentOS/RHEL/Fedora Linux
     ```
     MTU 9000
     ```
     {: pre}

   - Debian/Ubuntu Linux
     ```
     MTU=9000
     ```
     {: pre}

3. Schließen und speichern Sie die Datei. Starten Sie die Schnittstelle 'eth0' erneut.
   ```
   # /etc/init.d/networking restart
   ```
   {: pre}

   Die Netzkonnektivität wird bei dieser Aktion für ein paar Sekunden unterbrochen.

Weitere Informationen zu Advanced Single-Site VMware Reference Architecture finden Sie [hier](https://console.bluemix.net/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){:new_window}.
