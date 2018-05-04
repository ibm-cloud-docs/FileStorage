---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-23"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Daten schützen - durch providerseits verwaltete Verschlüsselung ruhender Daten 

{{site.data.keyword.BluSoftlayer_full}} nimmt die erforderlichen Sicherheitsaspekte sehr ernst und ist sich bewusst, wie wichtig es ist, Daten durch Verschlüsselung schützen zu können. Mit der durch den Provider verwalteten Verschlüsselung wird {{site.data.keyword.filestorage_full}} (mit Endurance oder Performance) standardmäßig ohne zusätzliche Kosten und ohne Auswirkungen auf das Leistungsverhalten verschlüsselt.

Die Funktion der durch den Provider verwalteten Verschlüsselung ruhender Daten (Encryption-at-rest) arbeitet mit den folgenden Standardprotokollen:

* Verschlüsselung mit dem Industriestandard AES-256
* Hausinterne Schlüsselverwaltung mit dem Industriestandard Key Management Interoperability Protocol (KMIP)
* Der Speicher wurde gemäß dem Federal Information Processing Standard (FIPS), Publication 140-2, auf Konformität mit dem Federal Information Security Management Act (FISMA), mit dem Health Insurance Portability and Accountability Act (HIPAA), mit dem Standard Payment Card Industry (PCI), mit Basel II, mit dem California Security Breach Information Act (SB 1386) und mit der EU-Datenschutzdirektive 95/46/EC (EU Data Protection Directive 95/46/EC) geprüft.

## Verschlüsselung ruhender Daten für Snapshots und replizierten Speicher  

Alle Snapshots und Replikate verschlüsselter Dateispeicher werden standardmäßig ebenfalls verschlüsselt. Diese Funktion kann auf Datenträgerebene nicht inaktiviert werden.

## Speicher mit Verschlüsselung bereitstellen

Die vom Provider verwaltete Funktion zur Verschlüsselung ruhender Daten ist nur für {{site.data.keyword.filestorage_short}}-Speicher verfügbar, der in ausgewählten Rechenzentren bereitgestellt wird. Weitere Rechenzentren werden in regelmäßigen Abständen zur Verfügung gestellt. Aller Speicher, der in diesen Rechenzentren bereitgestellt wird, wird automatisch mit Verschlüsselung für ruhende Daten bereitgestellt. Klicken Sie [hier](new-ibm-block-and-file-storage-location-and-features.html), um die aktuelle Liste der Rechenzentren anzuzeigen, in denen die {{site.data.keyword.filestorage_short}}-Verschlüsselung ruhender Daten verfügbar ist.


Wählen Sie beim Bestellen Ihres {{site.data.keyword.filestorage_short}}-Speichers ein Rechenzentrum aus, das mit einem Stern (`*`) und einem Hinweis gekennzeichnet ist, dass die Verschlüsselung verfügbar ist. Es wird ein Sperrsymbol rechts neben dem Feld 'LUN/Datenträgername' angezeigt, das darauf hinweist, dass der Datenträger verschlüsselt wird. Siehe Abbildung 1.

![Das Sperrsymbol weist darauf hin, dass die LUN verschlüsselt ist.](/images/encryptedstorage.png)
<caption>Abbildung 1. Beispiel für das Sperrsymbol, das auf die Verschlüsselung des Datenträgers hinweist.</caption>



**Anmerkung:** Nicht verschlüsselter Speicher, der vor der Aktualisierung eines Rechenzentrums bereitgestellt wird, wird **nicht** automatisch verschlüsselt. Wenn Sie nicht verschlüsselten Speicher in einem aktualisierten Rechenzentrum haben, müssen Sie einen neuen Datenträger erstellen und eine Datenmigration durchführen. Im folgenden Artikel finden Sie eine Anleitung dazu.

* [File Storage-Speicher in aktualisierten Rechenzentren migrieren](migrate-file-storage-encrypted-file-storage.html)
