---
title: "Lösung „Kapazität und Leistung“ in Azure Log Analytics | Microsoft-Dokumentation"
description: "Die Lösung „Kapazität und Leistung“ in Log Analytics hilft Ihnen, die Kapazität Ihrer Hyper-V-Server zu verstehen."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 51617a6f-ffdd-4ed2-8b74-1257149ce3d4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2017
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: a26b1f6c13b11d0fb6b47599fee43f955cbf7b7a
ms.lasthandoff: 04/17/2017


---
# <a name="plan-hyper-v-virtual-machine-capacity-with-the-capacity-and-performance-solution-preview"></a>Planen der Kapazität des virtuellen Hyper-V-Computers mit der Lösung „Kapazität und Leistung“ (Vorschau)

![Lösung „Kapazität und Leistung“](./media/log-analytics-capacity/capacity-solution.png) Sie können die Lösung „Kapazität und Leistung“ in Log Analytics nutzen, um die Kapazität Ihrer Hyper-V-Server zu verstehen. Die Lösung bietet Einblicke in Ihre Hyper-V-Umgebung, indem Ihnen die Gesamtnutzung (CPU, Arbeitsspeicher und Festplatte) der Hosts und VMs angezeigt wird, die auf diesen Hyper-V-Servern ausgeführt werden. Für CPU, Arbeitsspeicher und Datenträger auf allen Hosts und VMs werden Metriken gesammelt.

Die Lösung:

-    Anzeigen von Hosts mit der höchsten und niedrigsten Auslastung von CPU und Arbeitsspeicher
-    Anzeigen von VMs mit der höchsten und niedrigsten Auslastung von CPU und Arbeitsspeicher
-    Anzeigen von VMs mit dem höchsten und niedrigsten IOPS-Wert und Durchsatz
-    Anzeigen, welche VMs auf welchen Hosts ausgeführt werden
-    Anzeigen der Datenträger mit den höchsten Werten für Durchsatz, IOPS und Wartezeit in freigegebenen Clustervolumes
- Anpassen und Filtern anhand von Gruppen

> [!NOTE]
> Für die vorherige Version der Lösung „Kapazität und Leistung“, die als „Kapazitätsverwaltung“ bezeichnet wurde, war sowohl System Center Operations Manager als auch System Center Virtual Machine Manager erforderlich. Diese Abhängigkeiten gelten für diese aktualisierte Lösung nicht.


## <a name="connected-sources"></a>Verbundene Quellen

In der folgenden Tabelle sind die verbundenen Quellen beschrieben, die von der Lösung unterstützt werden.

| Verbundene Quelle | Support | Beschreibung |
|---|---|---|
| [Windows-Agents](log-analytics-windows-agents.md) | Ja | Mit der Lösung werden Informationen zu den Kapazitäts- und Leistungsdaten von Windows-Agents gesammelt. |
| [Linux-Agents](log-analytics-linux-agents.md) | Nein    | Die Lösung sammelt keine Informationen zu den Kapazitäts- und Leistungsdaten von direkten Linux-Agents.|
| [SCOM-Verwaltungsgruppe](log-analytics-om-agents.md) | Ja |Die Lösung sammelt Informationen zu den Kapazitäts- und Leistungsdaten von Agents in einer verbundenen SCOM-Verwaltungsgruppe. Es ist keine direkte Verbindung vom SCOM-Agent mit OMS erforderlich. Daten werden von der Verwaltungsgruppe an das OMS-Repository weitergeleitet.|
| [Azure-Speicherkonto](log-analytics-azure-storage.md) | Nein | Der Azure-Speicher enthält keine Daten zur Kapazität und Leistung.|

## <a name="prerequisites"></a>Voraussetzungen

- Windows- oder Operations Manager-Agents müssen auf Hyper-V-Hosts mit Windows Server 2012 oder höher installiert sein (nicht auf virtuellen Computern).


## <a name="configuration"></a>Konfiguration

Führen Sie den folgenden Schritt aus, um die Lösung „Kapazität und Leistung“ Ihrem Arbeitsbereich hinzuzufügen.

- Fügen Sie die Lösung „Kapazität und Leistung“ mithilfe des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Verfahrens Ihrem OMS-Arbeitsbereich hinzu.

## <a name="management-packs"></a>Management Packs

Falls Ihre SCOM-Verwaltungsgruppe mit dem OMS-Arbeitsbereich verbunden ist, werden in SCOM die folgenden Management Packs installiert, wenn Sie diese Lösung hinzufügen. Für diese Management Packs ist keine Konfiguration oder Wartung erforderlich.

- Microsoft.IntelligencePacks.CapacityPerformance

Das Ereignis 1201 sieht in etwa wie folgt aus:


```
New Management Pack with id:"Microsoft.IntelligencePacks.CapacityPerformance", version:"1.10.3190.0" received.
```

Wenn die Lösung „Kapazität und Leistung“ aktualisiert wird, ändert sich die Versionsnummer.

Weitere Informationen zur Aktualisierung von Management Packs finden Sie unter [Herstellen einer Verbindung zwischen Operations Manager und Log Analytics](log-analytics-om-agents.md).

## <a name="using-the-solution"></a>Verwenden der Lösung

Wenn Sie die Lösung „Kapazität und Leistung“ Ihrem Arbeitsbereich hinzufügen, wird sie dem Dashboard „Übersicht“ hinzugefügt. Auf dieser Kachel werden die Anzahl von derzeit aktiven Hyper-V-Hosts und die Anzahl von aktiven virtuellen Computern angezeigt, die während des ausgewählten Zeitraums überwacht wurden.

![Kachel „Kapazität und Leistung“](./media/log-analytics-capacity/capacity-tile.png)


### <a name="review-utilization"></a>Überprüfen der Auslastung

Klicken Sie auf die Kachel „Kapazität und Leistung“, um das Dashboard „Kapazität und Leistung“ zu öffnen. Das Dashboard enthält die Spalten, die in der folgenden Tabelle angegeben sind. In jeder Spalte sind bis zu zehn Einträge enthalten, die die Spaltenkriterien für den angegebenen Bereich und Zeitraum erfüllen. Sie können eine Protokollsuche durchführen, mit der alle Einträge zurückgegeben werden, indem Sie unten in der Spalte auf **Alle anzeigen** oder auf die Spaltenüberschrift klicken.

- **Hosts**
    - **Host-CPU-Auslastung** Zeigt für den gewählten Zeitraum einen grafischen Trend der CPU-Auslastung von Hostcomputern und eine Liste mit Hosts an. Zeigen Sie auf das Liniendiagramm, um Details für einen bestimmten Zeitpunkt anzuzeigen. Klicken Sie auf das Diagramm, um weitere Details für die Protokollsuche anzuzeigen. Klicken Sie auf einen beliebigen Hostnamen, um die Protokollsuche zu öffnen und für gehostete VMs Details zum CPU-Leistungsindikator anzuzeigen.
    - **Hostspeicherauslastung** Zeigt für den gewählten Zeitraum einen grafischen Trend für die Arbeitsspeicherauslastung von Hostcomputern und eine Liste mit Hosts an. Zeigen Sie auf das Liniendiagramm, um Details für einen bestimmten Zeitpunkt anzuzeigen. Klicken Sie auf das Diagramm, um weitere Details für die Protokollsuche anzuzeigen. Klicken Sie auf einen beliebigen Hostnamen, um die Protokollsuche zu öffnen und für gehostete VMs Details zum Arbeitsspeicher-Leistungsindikator anzuzeigen.
- **Virtuelle Computer**
    - **VM-CPU-Auslastung** Zeigt für den gewählten Zeitraum einen grafischen Trend für die CPU-Auslastung von virtuellen Computern und eine Liste mit virtuellen Computern an. Zeigen Sie auf das Liniendiagramm, um für die drei obersten VMs Details zu einem bestimmten Zeitpunkt anzuzeigen. Klicken Sie auf das Diagramm, um weitere Details für die Protokollsuche anzuzeigen. Klicken Sie auf einen beliebigen VM-Namen, um die Protokollsuche zu öffnen und für die VM aggregierte Details zum CPU-Leistungsindikator anzuzeigen.
    - **VM-Speicherauslastung** Zeigt für den gewählten Zeitraum einen grafischen Trend für die Arbeitsspeicherauslastung von virtuellen Computern und eine Liste mit virtuellen Computern an. Zeigen Sie auf das Liniendiagramm, um für die drei obersten VMs Details zu einem bestimmten Zeitpunkt anzuzeigen. Klicken Sie auf das Diagramm, um weitere Details für die Protokollsuche anzuzeigen. Klicken Sie auf einen beliebigen VM-Namen, um die Protokollsuche zu öffnen und für die VM aggregierte Details zum Arbeitsspeicher-Leistungsindikator anzuzeigen.
    - **IOPS gesamt für VM-Datenträger** Zeigt für den gewählten Zeitraum einen grafischen Trend für den IOPS-Gesamtwert für virtuelle Computer und eine Liste mit virtuellen Computern und dem zugehörigen IOPS-Wert an. Zeigen Sie auf das Liniendiagramm, um für die drei obersten VMs Details zu einem bestimmten Zeitpunkt anzuzeigen. Klicken Sie auf das Diagramm, um weitere Details für die Protokollsuche anzuzeigen. Klicken Sie auf einen beliebigen VM-Namen, um die Protokollsuche zu öffnen und für die VM aggregierte Details zum IOPS-Leistungsindikator des Datenträgers anzuzeigen.
    - **VM-Datenträgerdurchsatz gesamt** Zeigt für den gewählten Zeitraum einen grafischen Trend für den gesamten Datenträgerdurchsatz für virtuelle Computer und eine Liste mit virtuellen Computern und dem zugehörigen Datenträgerdurchsatz an. Zeigen Sie auf das Liniendiagramm, um für die drei obersten VMs Details zu einem bestimmten Zeitpunkt anzuzeigen. Klicken Sie auf das Diagramm, um weitere Details für die Protokollsuche anzuzeigen. Klicken Sie auf einen beliebigen VM-Namen, um die Protokollsuche zu öffnen und für die VM Details zum Leistungsindikator für den aggregierten Gesamtwert zum Datenträgerdurchsatz anzuzeigen.
- **Freigegebene Clustervolumes**
    - **Durchsatz gesamt** Zeigt die Summe von Lese- und Schreibvorgängen auf freigegebenen Clustervolumes an.
    - **IOPS gesamt** Zeigt die Summe von Eingabe-/Ausgabevorgängen pro Sekunde für freigegebene Clustervolumes an.
    - **Latenz gesamt** Zeigt die Gesamtwartezeit für freigegebene Clustervolumes an.
- **Hostdichte** Auf der obersten Kachel wird die Gesamtzahl von Hosts und virtuellen Computern angezeigt, die für die Lösung verfügbar sind. Klicken Sie auf die oberste Kachel, um weitere Details der Protokollsuche anzuzeigen. Außerdem werden alle Hosts und die Anzahl von gehosteten virtuellen Computern aufgeführt. Klicken Sie auf einen Host, um einen Drilldown für die VM-Ergebnisse einer Protokollsuche durchzuführen.


![Blatt „Hosts“ im Dashboard](./media/log-analytics-capacity/dashboard-hosts.png)

![Blatt „Virtuelle Computer“ im Dashboard](./media/log-analytics-capacity/dashboard-vms.png)


### <a name="evaluate-performance"></a>Auswerten der Leistung

Computingumgebungen für die Produktion können sich von einer Organisation zur nächsten stark unterscheiden. Die Workloads in Bezug auf die Kapazität und Leistung richten sich unter Umständen danach, wie Ihre VMs ausgeführt werden und was Sie als normal betrachten. Bestimmte Verfahren, die Ihnen beim Messen der Leistung als Hilfe dienen sollen, treffen für Ihre Umgebung also ggf. nicht zu. Aus diesem Grund ist eine allgemeiner gehaltene Anleitung für Sie hilfreicher. Microsoft veröffentlicht viele verschiedene Artikel mit Anleitungen und Beschreibungen zum Messen der Leistung.

Zusammenfassend lässt sich festhalten, dass mit der Lösung Daten zur Kapazität und Leistung aus verschiedenen Quellen gesammelt werden, z.B. auch von Leistungsindikatoren. Nutzen Sie diese Daten zur Kapazität und Leistung, die auf den einzelnen Oberflächen der Lösung angezeigt werden, und vergleichen Sie Ihre Ergebnisse mit den Ergebnissen im Artikel [Measuring Performance on Hyper-V](https://msdn.microsoft.com/library/cc768535.aspx) (Messen der Leistung in Hyper-V). Der Artikel wurde zwar schon vor einiger Zeit veröffentlicht, aber die Metriken, Aspekte und Richtlinien sind immer noch gültig. Der Artikel enthält auch Links zu anderen nützlichen Ressourcen.


## <a name="sample-log-searches"></a>Beispiele für Protokollsuchen

Die folgende Tabelle enthält Beispiele für Protokollsuchen nach Daten zur Kapazität und Leistung, die mit dieser Lösung gesammelt und berechnet werden.

| Abfrage | Beschreibung |
|---|---|
| Alle Konfigurationen des Hostarbeitsspeichers | <code>Type=Perf ObjectName="Capacity and Performance" CounterName="Host Assigned Memory MB" &#124; measure avg(CounterValue) as MB by InstanceName</code> |
| Alle Konfigurationen des VM-Arbeitsspeichers | <code>Type=Perf ObjectName="Capacity and Performance" CounterName="VM Assigned Memory MB" &#124; measure avg(CounterValue) as MB by InstanceName</code> |
| Aufschlüsselung des IOPS-Gesamtwerts für die Datenträger aller VMs | <code>Type=Perf ObjectName="Capacity and Performance" (CounterName="VHD Reads/s" OR CounterName="VHD Writes/s") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |
| Aufschlüsselung des gesamten Durchsatzes für die Datenträger aller VMs | <code>Type=Perf ObjectName="Capacity and Performance" (CounterName="VHD Read MB/s" OR CounterName="VHD Write MB/s") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |
| Aufschlüsselung des IOPS-Gesamtwerts für alle freigegebenen Clustervolumes | <code>Type=Perf ObjectName="Capacity and Performance" (CounterName="CSV Reads/s" OR CounterName="CSV Writes/s") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |
| Aufschlüsselung des gesamten Durchsatzes für alle freigegebenen Clustervolumes | <code>Type=Perf ObjectName="Capacity and Performance" (CounterName="CSV Read MB/s" OR CounterName="CSV Write MB/s") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |
| Aufschlüsselung der gesamten Wartezeit für alle freigegebenen Clustervolumes | <code> Type=Perf ObjectName="Capacity and Performance" (CounterName="CSV Read Latency" OR CounterName="CSV Write Latency") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |




## <a name="next-steps"></a>Nächste Schritte
* Verwenden Sie die [Protokollsuche in Log Analytics](log-analytics-log-searches.md), um ausführliche Daten zur Kapazität und Leistung anzuzeigen.

