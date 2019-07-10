---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NSF, networking, jumbo frames

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Jumbo-Frames in {{site.data.keyword.BluSoftlayer_notm}} für Windows und Linux aktivieren
{: #jumboframes}

Ein Jumbo-Frame ist ein Ethernet-Rahmen mit einem Nutzdatenvolumen, das größer als die standardmäßig maximale Übertragungseinheit (MTU - Maximum Transmission Unit) von 1.500 Byte ist. Jumbo-Frames werden in LAN-Netzen verwendet, die mindestens 1 Gb/s unterstützen, und können eine Größe von bis zu 9.000 Byte haben.

Jumbo-Frames müssen im gesamten Netzpfad von der Quelleneinheit <-> Switch <-> Router <-> Switch <-> Zieleinheit identisch konfiguriert werden. Wenn die Kette nicht insgesamt identisch konfiguriert ist, wird standardmäßig die niedrigste Einstellung innerhalb der Kette verwendet. {{site.data.keyword.cloud}} hat die Netzeinheiten derzeit auf 9.000 eingestellt. Alle Kundeneinheiten müssen auf denselben Wert eingestellt werden: 9.000.
{:important}

## Jumbo-Frames in Windows aktivieren

1. Öffnen Sie das **Center für Netze und gemeinsame Nutzung**.
2. Klicken Sie auf **Adaptereinstellungen ändern**.
3. Klicken Sie mit der rechten Maustaste auf die Netzadapterkarte (NIC), für die Sie Jumbo-Frames aktivieren wollen, und wählen Sie **Eigenschaften** aus.
4. Klicken Sie auf der Registerkarte **Netz** auf **Konfigurieren** für den Netzadapter.
5. Wählen Sie die Registerkarte **Erweitert** aus.
6. Wählen Sie **Großrahmen** aus und ändern Sie den Wert von **Deaktiviert** in den gewünschten Wert. Der Wert, beispielsweise 9 KB oder 9.014 Byte, richtet sich nach der Netzadapterkarte.
7. Klicken Sie in allen Fenstern auf **OK**.

Die Netzkonnektivität der Netzadapterkarte (NIC) wird bei Änderungen für ein paar Sekunden unterbrochen. Starten Sie das Gerät erneut, um sich zu vergewissern, dass die Änderung wirksam wurde.
{:tip}


## Jumbo-Frames in Linux aktivieren

1. Bearbeiten Sie die Netzkonfigurationsdatei für die Schnittstelle 'eth0'.
   - Benutzer von CentOS, RHEL und Fedora Linux bearbeiten `/etc/sysconfig/network-script/ifcfg-eth0`.
     ```
     # vi /etc/sysconfig/network-script/ifcfg-eth0
     ```
     {: pre}

   - Benutzer von Debian und Ubuntu Linux bearbeiten `/etc/network/interfaces`.

2. Hängen Sie die folgende Konfigurationsanweisung an, die die Größe des Rahmens in Byte angibt.
   - CentOS, RHEL, Fedora Linux
     ```
     MTU 9000
     ```
     {: pre}

   - Debian und Ubuntu Linux
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
