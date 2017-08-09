---
title: "Cheat Sheet für die Azure Log Analytics-Abfragesprache | Microsoft-Dokumentation"
description: "Dieser Artikel hilft beim Übergang zur neuen Abfragesprache für Log Analytics, wenn Sie bereits mit der bisherigen Sprache vertraut sind."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: bwren
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: 281b6afc6aeaf65e87e1bd2820c35a14f7714aa1
ms.contentlocale: de-de
ms.lasthandoff: 07/28/2017

---

# <a name="transitioning-to-azure-log-analytics-new-query-language"></a>Übergang zur neuen Azure Log Analytics-Abfragesprache

> [!NOTE]
> Weitere Informationen zur Log Analytics-Abfragesprache und zum Upgradeverfahren für den Arbeitsbereich finden Sie unter [Upgrade des Azure Log Analytics-Arbeitsbereichs auf die neue Protokollsuche](log-analytics-log-search-upgrade.md).

Dieser Artikel hilft beim Übergang zur neuen Abfragesprache für Log Analytics, wenn Sie bereits mit der bisherigen Sprache vertraut sind.

## <a name="language-converter"></a>Sprachkonverter

Wenn Sie mit der bisherigen Log Analytics-Abfragesprache vertraut sind, ist die einfachste Möglichkeit, eine vorhandene Abfrage in der neuen Sprache zu erstellen, die Verwendung des im Portal der Protokollsuche installierten Sprachkonverters, wenn Ihr Arbeitsbereich konvertiert ist.  Die Verwendung des Konverters ist ganz einfach: Geben Sie eine Legacyabfrage in das Textfeld ein, und klicken Sie auf **Konvertieren**.  Sie können auf die Suchschaltfläche klicken, um die Abfrage auszuführen, oder die Abfrage kopieren und an einer anderen Stelle verwenden.

![Sprachkonverter](media/log-analytics-log-search-upgrade/language-converter.png)


## <a name="cheat-sheet"></a>Cheat Sheet

Die folgende Tabelle zeigt einen Vergleich zwischen verschiedenen häufigen Abfragen für äquivalente Befehle in der neuen und der alten Abfragesprache in Azure Log Analytics. 

| Beschreibung | Alt | Neu |
|:--|:--|:--|
| Auswählen von Daten aus einer Tabelle | Type=Event |  Ereignis |
|                        | Type=Event &#124; select Source, EventLog, EventID | Event &#124; project Source, EventLog, EventID |
|                        | Type=Event &#124; top 100 | Event &#124; take 100 |
| Zeichenfolgenvergleich      | Type=Event Computer=srv01.contoso.com   | Event &#124; where Computer == "srv01.contoso.com" |
|                        | Type=Event Computer=contains("contoso") | Event &#124; where Computer contains "contoso" |
|                        | Type=Event Computer=RegEx("@contoso@")  | Event &#124; where Computer matches regex ".*contoso*" |
| Datumsvergleich        | Type=Event TimeGenerated > NOW-1DAYS | Event &#124; where TimeGenerated > ago(1d) |
|                        | Type=Event TimeGenerated>2017-05-01 TimeGenerated<2017-05-31 | Event &#124; where TimeGenerated between (datetime(2017-05-01) .. datetime(2017-05-31)) |
| Boolescher Vergleich     | Type=Heartbeat IsGatewayInstalled=false  | Heartbeat | where IsGatewayInstalled == false |
| Sortieren                   | Type=Event &#124; sort Computer asc, EventLog desc, EventLevelName asc | Event \| sort by Computer asc, EventLog desc, EventLevelName asc |
| Unterscheiden               | Type=Event &#124; dedup Computer \| select Computer | Event &#124; summarize by Computer, EventLog |
| Erweitern von Spalten         | Type=Perf CounterName="% Processor Time" &#124; EXTEND if(map(CounterValue,0,50,0,1),"HIGH","LOW") as UTILIZATION | Perf &#124; where CounterName == "% Processor Time" \| extend Utilization = iff(CounterValue > 50, "HIGH", "LOW") |
| Aggregation            | Type=Event &#124; measure count() as Count by Computer | Event &#124; summarize Count = count() by Computer |
|                                | Type=Perf ObjectName=Processor CounterName="% Processor Time" &#124; measure avg(CounterValue) by Computer interval 5minute | Perf &#124; where ObjectName=="Processor" and CounterName=="% Processor Time" &#124; summarize avg(CounterValue) by Computer, bin(TimeGenerated, 5min) |
| Aggregation mit Grenzwert | Type=Event &#124; measure count() by Computer &#124; top 10 | Event &#124; summarize AggregatedValue = count() by Computer &#124; limit 10 |
| Union                  | Type=Event oder Type=Syslog | union Event, Syslog |
| Join                   | Type=NetworkMonitoring &#124; join inner AgentIP (Type=Heartbeat) ComputerIP | NetworkMonitoring &#124; join kind=inner (search Type == "Heartbeat") on $left.AgentIP == $right.ComputerIP |



## <a name="next-steps"></a>Nächste Schritte
- Sehen Sie sich ein [Tutorial zum Schreiben von Abfragen](https://docs.loganalytics.io/learn/tutorial_getting_started_with_queries.html) an, in dem die neue Abfragesprache verwendet wird.
- Details zu allen Befehlen, Operatoren und Funktionen der neuen Abfragesprache finden Sie in der [Referenz zur Abfragesprache](https://docs.loganalytics.io/queryLanguage/query_language.html).  

