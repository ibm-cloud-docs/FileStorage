---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, security, encryption

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Vom Provider verwaltete Verschlüsselung ruhender Daten
{: #encryption}

In {{site.data.keyword.BluSoftlayer_full}} werden erforderliche Sicherheitsaspekte sehr ernst genommen und die Wichtigkeit der Verschlüsselung für die Datensicherung hat einen hohen Stellenwert. Mit der durch den Provider verwalteten Verschlüsselung wird {{site.data.keyword.filestorage_full}} (bereitgestellt mit Endurance- oder Performance-Optionen) ohne zusätzliche Kosten und ohne Auswirkung auf das Leistungsverhalten standardmäßig verschlüsselt.

Die vom Provider verwaltete Verschlüsselungsfunktion für ruhende Daten (Encryption-at-rest) verwendet folgende Standardprotokolle:

* Verschlüsselung mit dem Industriestandard AES-256
* Hausinterne Schlüsselverwaltung mit dem Industriestandard Key Management Interoperability Protocol (KMIP)
* Der Speicher wurde auf Konformität mit folgenden Standards geprüft:
    - Federal Information Processing Standard (FIPS) Publication 140-2,
    - Federal Information Security Management Act (FISMA),
    - Health Insurance Portability and Accountability Act (HIPAA),
    - Payment Card Industry (PCI),
    - Basel II,
    - California Security Breach Information Act (SB 1386) und
    - EU-Datenschutzdirektive 95/46/EC (EU Data Protection Directive 95/46/EC).

## Snapshots oder replizierten Speicher schützen  

Alle Snapshots und Replikate verschlüsselter Dateispeicher werden standardmäßig ebenfalls verschlüsselt. Diese Funktion kann auf Datenträgerebene nicht inaktiviert werden.

## Speicher mit Verschlüsselung bereitstellen

Die vom Provider verwaltete Funktion für die Verschlüsselung ruhender Daten ist in ausgewählten Rechenzentren verfügbar. Aller Speicher, der in diesen Rechenzentren bestellt wird, wird automatisch mit Verschlüsselung für ruhende Daten bereitgestellt. Klicken Sie [hier](/docs/infrastructure/FileStorage?topic=FileStorage-news), um die aktuelle Liste der Rechenzentren anzuzeigen, in denen die {{site.data.keyword.filestorage_short}}-Verschlüsselung verfügbar ist.

Wählen Sie beim Bestellen Ihres {{site.data.keyword.filestorage_short}}-Speichers ein Rechenzentrum, das mit einem Stern (`*`) markiert ist. Das rechts neben dem Feld 'LUN/Datenträgername' angezeigte Sperrsymbol gibt an, dass der Datenträger verschlüsselt wird. Siehe Abbildung 1.

![Das Sperrsymbol weist darauf hin, dass die LUN verschlüsselt ist.](/images/encryptedstorage.png)
<caption>Abbildung 1. Beispiel für das Sperrsymbol, das die Verschlüsselung des Datenträgers angibt.</caption>

Sämtlicher nicht verschlüsselter Speicher, der vor der Aktualisierung eines Rechenzentrums bereitgestellt wurde, wird **nicht** automatisch verschlüsselt. Wenn Sie nicht verschlüsselten Speicher in einem aktualisierten Rechenzentrum besitzen und ihn verschlüsseln wollen, müssen Sie einen neuen Datenträger erstellen und eine Datenmigration durchführen. Weitere Informationen finden Sie im Abschnitt [File Storage in aktualisierten Rechenzentren migrieren](/docs/infrastructure/FileStorage?topic=FileStorage-migratestorage)
{:important}
