---
title: "Kapazitätsverwaltungslösung in Log Analytics | Microsoft Docs"
description: "Die Kapazitätsplanungslösung in Log Analytics hilft Ihnen dabei, die Kapazität der Hyper-V-Server, die vom System Center Virtual Machine Manager verwaltet werden, besser zu verstehen."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: jwhit
editor: 
ms.assetid: 51617a6f-ffdd-4ed2-8b74-1257149ce3d4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2016
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: 57e7fbdaa393e078b62a6d6a0b181b67d532523d
ms.openlocfilehash: c34cda0da164c711c8effc78d2af38ad8df581aa


---
# <a name="capacity-management-solution-in-log-analytics"></a>Kapazitätsverwaltungslösung in Log Analytics
Die Kapazitätsplanungslösung in Log Analytics hilft Ihnen, die Kapazität Ihrer Hyper-V-Server zu verstehen. Diese Lösung erfordert System Center Operations Manager und System Center Virtual Machine Manager. Wenn Sie direkt verbundene Agents verwenden, funktioniert die Kapazitätsplanungslösung nicht. Die Lösung liest die Leistungsindikatoren auf dem überwachten Server und sendet Nutzungsdaten zur Verarbeitung an den OMS-Dienst in der Cloud. Auf die Nutzungsdaten wird Logik angewendet, und der Clouddienst zeichnet die Daten auf. Mit der Zeit entstehen Verwendungsmuster, und die Kapazität wird anhand des aktuellen Verbrauchs prognostiziert.

Beispielsweise kann anhand einer Prognose ermittelt werden, wann zusätzliche Prozessorkerne oder zusätzlicher Arbeitsspeicher für einen einzelnen Server benötigt wird. In diesem Beispiel kann die Prognose darauf hinweisen, dass der Server in 30 Tagen zusätzlichen Arbeitsspeicher benötigen. Diese Prognose kann Ihnen helfen, ein Speicherupgrade für das nächste Wartungsfenster des Servers planen.

> [!NOTE]
> Die Kapazitätsverwaltungslösung kann keinen Arbeitsbereichen hinzugefügt werden. Kunden, die die Kapazitätsverwaltungslösung installiert haben, können die Lösung weiterhin verwenden.  
> 
> 

Ein Nachfolgelösung der Kapazitäts- und Leistungslösung ist in der Phase „Private Vorschau“. Diese Nachfolgelösung dient zum Beheben der folgenden von Kunden gemeldeten Problemen mit der ursprünglichen Kapazitätsverwaltungslösung:

* Möglichkeit der Verwendung von Virtual Machine Manager und Operations Manager
* Möglichkeit zum Anpassen/Filtern auf der Grundlage von Gruppen
* Stündliche Datenaggregation ist nicht häufig genug
* Keine Einblicke auf VM-Ebene
* Datenzuverlässigkeit

Vorteile der neuen Kapazitätslösung:

* Unterstützung granularer Datensammlung mit verbesserter Zuverlässigkeit und Genauigkeit
* Unterstützung für Hyper-V ohne VMM
* Einblicke in die Nutzung auf VM-Ebene

Die neue Lösung erfordert derzeit Hyper-V Server 2012 oder höher. Die Lösung bietet Einblicke in Ihre Hyper-V-Umgebung und Transparenz bei der Gesamtnutzung (CPU, Arbeitsspeicher und Festplatte) der Hosts und VMs, die auf diesen Hyper-V-Servern ausgeführt werden. Für CPU, Arbeitsspeicher und Datenträger auf allen Hosts und VMs werden Metriken gesammelt.

Die übrige Dokumentation auf dieser Seite bezieht sich auf die alte Kapazitätsverwaltungslösung. Diese Dokumentation wird aktualisiert, sobald die neue Lösung in der Version „Öffentliche Vorschau“ verfügbar ist.

## <a name="installing-and-configuring-the-solution"></a>Installieren und Konfigurieren der Lösung
Verwenden Sie die folgenden Informationen zum Installieren und Konfigurieren der Lösung.

* Operations Manager ist für die Kapazitätsverwaltungslösung erforderlich.
* Virtual Machine Manager ist für die Kapazitätsverwaltungslösung erforderlich.
* Operations Manager-Verbindung mit Virtual Machine Manager (VMM) ist erforderlich. Weitere Informationen zum Verbinden der Systeme finden Sie unter [Verbinden von VMM mit Operations Manager](http://technet.microsoft.com/library/hh882396.aspx).
* Operations Manager muss mit Log Analytics verbunden sein.
* Fügen Sie mithilfe des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md)beschriebenen Verfahrens Ihrem OMS-Arbeitsbereich die Kapazitätsverwaltungslösung hinzu.  Es ist keine weitere Konfiguration erforderlich.

## <a name="capacity-management-data-collection-details"></a>Details zur Datensammlung für die Kapazitätsverwaltung
Die Kapazitätsverwaltung sammelt Leistungsdaten, Metadaten und Statusdaten mit den Agents, die Sie aktiviert haben.

Die folgende Tabelle zeigt die Datensammlungsmethoden und andere Details dazu, wie Daten für die Kapazitätsverwaltung erfasst werden.

| Plattform | Direkt-Agent | Operations Manager-Agent | Azure Storage | Operations Manager erforderlich? | Daten vom Operations Manager-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |![Nein](./media/log-analytics-capacity/oms-bullet-red.png) |![Ja](./media/log-analytics-capacity/oms-bullet-green.png) |![Nein](./media/log-analytics-capacity/oms-bullet-red.png) |![Ja](./media/log-analytics-capacity/oms-bullet-green.png) |![Ja](./media/log-analytics-capacity/oms-bullet-green.png) |Stündlich |

Die folgende Tabelle zeigt Beispiele für Datentypen, die von der Kapazitätsverwaltung erfasst werden:

| **Datentyp** | **Felder** |
| --- | --- |
| Metadaten |BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, NetworkName, IPAddress, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, NetbiosDomainName, LogicalProcessors, DNSName, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime |
| Leistung |ObjectName, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded |
| Zustand |StateChangeEventId, StateId, NewHealthState, OldHealthState, Context, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified |

## <a name="capacity-management-page"></a>Kapazitätsverwaltungsseite
Nach der Installation der Kapazitätsverwaltungslösung können Sie die Kapazität der überwachten Server in OMS über die Kachel **Kapazitätsplanung** auf der Seite **Übersicht** anzeigen.

![Abbildung der Kachel "Capacity Planning"](./media/log-analytics-capacity/oms-capacity01.png)

Über die Kachel wird das Dashboard **Capacity Management** geöffnet, auf der Sie eine Zusammenfassung Ihrer Serverkapazität anzeigen können. Die Seite zeigt die folgenden Kacheln, auf die Sie klicken können:

* *Virtual machine count*: Zeigt die Anzahl der Tage, die für die Kapazität virtueller Computer verbleiben.
* *Compute*: Zeigt Prozessorkerne und verfügbaren Speicher.
* *Storage*: Zeigt den Speicherplatz und die durchschnittliche Latenz der Datenträger.
* *Search:*Daten-Explorer, in dem Sie Daten im OMS-System suchen können.

![Abbildung des Dashboards "Capacity Management"](./media/log-analytics-capacity/oms-capacity02.png)

### <a name="to-view-a-capacity-page"></a>So zeigen Sie eine Kapazitätsseite an
* Klicken Sie auf der Seite **Übersicht** auf **Kapazitätsverwaltung** und dann auf **Compute** oder **Storage**.

## <a name="compute-page"></a>Seite "Compute"
Im Dashboard **Compute** in Microsoft Azure OMS können Sie Kapazitätsdaten zur Auslastung, projizierte Tage von Kapazität und die Effizienz Ihrer Infrastruktur anzeigen. Im Bereich **Utilization** können Sie die CPU-Kern- und Arbeitsspeicherauslastung der Hosts der virtuellen Computer anzeigen. Mithilfe des Projektionstools können Sie schätzen, wie viel Kapazität in einem bestimmten Zeitraum verfügbar sein wird. Im Bereich **Efficiency** können Sie feststellen, wie effizient die Hosts Ihrer virtuellen Computer sind. Sie können Informationen zu verknüpften Elementen anzeigen, indem Sie darauf klicken.

Sie können eine Excel-Arbeitsmappe für die folgenden Kategorien erstellen:

* Wichtigste Hosts mit der höchsten Kernauslastung
* Wichtigste Hosts mit der höchsten Arbeitsspeicherauslastung
* Wichtigste Hosts mit ineffizienten virtuellen Computern
* Wichtigste Hosts nach Auslastung
* Am wenigsten wichtige Hosts nach Auslastung

![Abbildung der "Capacity Management"-Seite "Compute"](./media/log-analytics-capacity/oms-capacity03.png)

Die folgenden Bereiche werden auf dem Dashboard **Compute** gezeigt:

**Utilization**: CPU-Kern- und Arbeitsspeicherauslastung der Hosts Ihrer virtuellen Computer.

* *Used Cores*: Summe für alle Hosts (% der CPU-Auslastung multipliziert mit der Anzahl der physischen Kerne auf dem Host).
* *Free Cores*: Gesamtanzahl physischer Kerne minus verwendete Kerne.
* *Percentage Cores Available*: Freie physische Kerne dividiert durch die Gesamtanzahl physischer Kerne.
* *Virtual Cores per VM*: Gesamtanzahl virtueller Kerne im System dividiert durch die Gesamtanzahl virtueller Computer im System.
* *Virtual Cores to Physical Cores Ratio*: Verhältnis der Gesamtanzahl physischer Kerne und der physischen Kerne, die von virtuellen Computern im System verwendet werden.
* *Number of virtual Cores Available*: Verhältnis virtueller Kerne und physischer Kerne multipliziert mit den verfügbaren physischen Kernen.
* *Used Memory*: Gesamtmenge des von allen Hosts verwendeten Arbeitsspeichers.
* *Free Memory*: Gesamter physischer Arbeitsspeicher minus verwendeter Arbeitsspeicher.
* *Percentage Memory Available*: Freier physischer Arbeitsspeicher dividiert durch den gesamten physischen Arbeitsspeicher.
* *Virtual Memory per VM*: Gesamter virtueller Arbeitsspeicher im System dividiert durch die Gesamtzahl der virtuellen Computer im System.
* *Virtual Memory to Physical Memory Ratio*: Gesamter virtueller Arbeitsspeicher im System dividiert durch den gesamten physischen Arbeitsspeicher im System.
* *Virtual Memory Available*: Verhältnis von virtuellem und physischem Arbeitsspeicher multipliziert mit dem verfügbaren physischen Arbeitsspeicher.

**Projektionstool**

Mithilfe des Projektionstools können Sie historische Trends für die Ressourcenauslastung anzeigen. Dies schließt die Auslastungstrends für virtuelle Computer, Arbeitsspeicher, Kerne und Speicher ein. Die Projektionsfunktion verwendet einen Projektionsalgorithmus, über den Ihnen mitgeteilt wird, wann die einzelnen Ressourcen knapp werden. Dadurch lässt sich eine exakte Kapazitätsplanung durchführen, sodass Sie wissen, wann Sie mehr Kapazität (z. B. Arbeitsspeicher, Kerne oder Speicher) erwerben müssen.

**Effizienz**

* *Idle VM*: Verwendung von weniger als 10 % der CPU-Leistung und 10 % des Arbeitsspeichers im angegebenen Zeitraum.
* *Overutilized VM*: Verwendung von mehr als 90 % der CPU-Leistung und 90 % des Arbeitsspeichers im angegebenen Zeitraum.
* *Idle Host*: Verwendung von weniger als 10 % der CPU-Leistung und 10 % des Arbeitsspeichers im angegebenen Zeitraum.
* *Overutilized Host*: Verwendung von mehr als 90 % der CPU-Leistung und 90 % des Arbeitsspeichers im angegebenen Zeitraum.

### <a name="to-work-with-items-on-the-compute-page"></a>Arbeiten mit Elementen auf der Seite "Compute"
1. Im Dashboard **Compute** können Sie im Bereich **Nutzung** die Kapazitätsinformationen zu den verwendeten CPU-Kernen und zum verwendeten Arbeitsspeicher anzeigen.
2. Klicken Sie auf ein Element, um es auf der Seite **Search** zu öffnen und detaillierte Informationen dazu anzuzeigen.
3. Verschieben Sie im **Projektionstool** den Datumsschieberegler, um eine Projektion für die Kapazität anzuzeigen, die an dem von Ihnen gewählten Datum verwendet werden wird.
4. Im Bereich **Efficiency** können Sie Kapazitätseffizienzinformationen zu virtuellen Computern und Hosts für virtuelle Computer anzeigen.

## <a name="direct-attached-storage-page"></a>Seite "Direct Attached Storage"
Mithilfe des Dashboards **Direct Attached Storage** in OMS können Sie Kapazitätsinformationen zur Speicherplatzauslastung, Datenträgerleistung und zu den projizierten Tagen der Datenträgerkapazität anzeigen. Im Bereich **Utilization** können Sie die Speicherplatzbelegung der Hosts Ihrer virtuellen Computer anzeigen. Im Bereich **Disk Performance** können Sie den Datenträgerdurchsatz und die Latenz der Hosts Ihrer virtuellen Computer anzeigen. Sie können außerdem mit dem Projektionstool abschätzen, wie viel Kapazität für einen bestimmten Zeitraum voraussichtlich verfügbar sein wird. Sie können Informationen zu verknüpften Elementen anzeigen, indem Sie darauf klicken.

Sie können aus diesen Kapazitätsinformationen eine Excel-Arbeitsmappe für die folgenden Kategorien generieren:

* Maximale Speicherplatzbelegung nach Host
* Maximale durchschnittliche Latenz nach Host

Die folgenden Bereiche werden auf der Seite **Storage** angezeigt:

* *Utilization*: Anzeigen der Speicherplatzbelegung auf den Hosts Ihrer virtuellen Computer.
* *Total Disk Space*: Summe (logischer Speicherplatz) für alle Hosts.
* *Used Disk Space*: Summe (verwendeter logischer Speicherplatz) für alle Hosts.
* *Available Disk Space*: Gesamter Speicherplatz abzüglich verwendeter Speicherplatz.
* *Percentage Disk Used*: Verwendeter Speicherplatz geteilt durch den Gesamtspeicherplatz.
* *Percentage Disk Available*: Verfügbarer Speicherplatz geteilt durch den Gesamtspeicherplatz.

![Abbildung der "Capacity Management"-Seite "Direct Attached Storage"](./media/log-analytics-capacity/oms-capacity04.png)

**Datenträgerleistung**

Mithilfe von OMS können Sie den Verlauf des Nutzungstrends Ihres Speicherplatzes anzeigen. Die Projektionsfunktion verwendet einen Algorithmus, um die künftige Verwendung vorherzusagen. Insbesondere bei der Speicherplatzverwendung ermöglicht Ihnen die Projektionsfunktion die Vorhersage, wann Sie eventuell keinen Speicherplatz mehr zur Verfügung haben werden. Diese Projektion hilft Ihnen bei der Planung von ausreichendem Speicher, sodass Sie wissen, wann Sie zusätzlichen Speicher erwerben müssen.

**Projektionstool**

Mithilfe des Projektionstools können Sie historische Trends für die Speicherplatzauslastung anzeigen. Die Projektionsfunktion ermöglicht Ihnen außerdem die Vorhersage, wann nicht mehr genügend Speicherplatz vorhanden sein wird. Diese Projektion hilft Ihnen bei der Planung von ausreichender Kapazität, sodass Sie wissen, wann Sie zusätzliche Speicherkapazität erwerben müssen.

### <a name="to-work-with-items-on-the-direct-attached-storage-page"></a>Arbeiten mit Elementen auf der Seite "Direct Attached Storage"
1. Im Dashboard **Direct Attached Storage** können Sie im Bereich **Nutzung** Informationen zur Datenträgerauslastung anzeigen.
2. Klicken Sie auf ein verknüpftes Element, um es auf der Seite **Search** zu öffnen und detaillierte Informationen dazu anzuzeigen.
3. lm Bereich **Disk Performance** können Sie den Datenträgerdurchsatz und Informationen zur Latenz anzeigen.
4. Verschieben Sie im **Projektionstool**den Datumsschieberegler, um eine Projektion für die Kapazität anzuzeigen, die an dem von Ihnen gewählten Datum verwendet werden wird.

## <a name="next-steps"></a>Nächste Schritte
* Verwenden Sie die [Protokollsuche in Log Analytics](log-analytics-log-searches.md) , um ausführliche Daten zur Kapazitätsverwaltung anzuzeigen.




<!--HONumber=Nov16_HO3-->


