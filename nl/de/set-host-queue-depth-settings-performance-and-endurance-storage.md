---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, host queue depth, performance tuning

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Einstellungen der Länge der Hostwarteschlange anpassen
{: #hostqueuesettings}

{{site.data.keyword.cloud}} empfiehlt für jede Leistungsstufe eine maximale Länge für die Ein-/Ausgabewarteschlangen für den Host und die Anwendung.

| Leistungsstufe | Maximale Länge der Hostwarteschlange |
|------|------|
| 0,25 IOPS pro GB | 8 |
| 2 IOPS pro GB | 24 |
| 4 IOPS pro GB | 56 |
{: caption="Empfohlene Warteschlangenlänge für die einzelnen IOPS-Stufen" caption-side="top"}

Die Hosteinstellung wirkt sich nicht auf die Platten- und Controllerlatenz aus. Sie wirkt sich nur auf die Latenz aus, die für den Host und die Anwendung beobachtet wird.

Eine Warteschlangenlänge über die ausgeführten Werte hinaus kann die E/A-Latenz erhöhen. Eine Warteschlangenlänge unter dem ausgeführten Wert kann die E/A-Leistung des Hosts verringern. Da jede Anwendung anders ist, sind Anpassungen und Beobachtungen erforderlich, um die maximale Speicherleistung zu erzielen.

Die Warteschlangenlänge des Hosts wird in der Regel im Hostbusadaptertreiber (host bus adapter) oder im Hypervisor und manchmal auch in der Anwendung angepasst. Standardwerte wie 32 oder 64 können eine übermäßige Latenz des Hosts oder der Anwendung zur Folge haben.

Wenn ein Host oder Hypervisor mehrere Leistungsstufen verwendet, verwenden Sie die Warteschlangenlänge für die schnellste Stufe und beobachten die Latenz auf der langsamsten Leistungsstufe.

Wenn die Latenz auf der niedrigsten Stufe nicht akzeptabel ist, passen Sie die Warteschlangenlänge an, bis eine ausgeglichene Latenz und Leistung für alle Stufen erreicht ist.
