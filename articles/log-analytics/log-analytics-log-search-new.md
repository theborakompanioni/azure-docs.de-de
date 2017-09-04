---
title: Protokollsuchen in OMS Log Analytics | Microsoft-Dokumentation
description: "Sie benötigen eine Protokollsuche zum Abrufen von Daten aus Log Analytics.  In diesem Artikel wird beschrieben, wie neue Protokollsuchen in Log Analytics verwendet werden. Er enthält Konzepte, die Ihnen vor dem Erstellen bekannt sein sollten."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: bwren
ms.translationtype: HT
ms.sourcegitcommit: 7456da29aa07372156f2b9c08ab83626dab7cc45
ms.openlocfilehash: 0f27db7018e398f71a8d7bd0b86e643367b15875
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---
# <a name="understanding-log-searches-in-log-analytics"></a>Grundlegendes zu Protokollsuchvorgängen in Log Analytics

> [!NOTE]
> Dieser Artikel beschreibt Protokollsuchen in Azure Log Analytics mithilfe der neuen Abfragesprache.  Weitere Informationen zur neuen Sprache und zum Upgradeverfahren für den Arbeitsbereich finden Sie unter [Upgrade des Azure Log Analytics-Arbeitsbereichs auf die neue Protokollsuche](log-analytics-log-search-upgrade.md).  
>
> Wenn für Ihren Arbeitsbereich noch kein Upgrade auf die neue Abfragesprache durchgeführt wurde, lesen Sie [Suchen von Daten mithilfe der Protokollsuche in Log Analytics](log-analytics-log-searches.md).

Sie benötigen eine Protokollsuche zum Abrufen von Daten aus Log Analytics.  Wenn Sie Daten im Portal analysieren, eine Warnungsregel für die Benachrichtigung bei einer bestimmten Bedingung konfigurieren oder Daten mithilfe der Log Analytics-API abrufen, verwenden Sie jeweils eine Protokollsuche, um die gewünschten Daten anzugeben.  In diesem Artikel wird beschrieben, wie Protokollsuchen in Log Analytics verwendet werden. Er enthält Konzepte, die Ihnen vor dem Erstellen bekannt sein sollten. Im Abschnitt [Nächste Schritte](#next-steps) finden Sie ausführliche Informationen zum Erstellen und Bearbeiten von Protokollsuchen sowie Verweise auf die Abfragesprache.

## <a name="where-log-searches-are-used"></a>Einsatzmöglichkeiten von Protokollsuchen

Zu den verschiedenen Möglichkeiten, wie Sie Protokollsuchen in Log Analytics verwenden können, gehören die folgenden:

- **Portale.** Sie können interaktive Analysen von Daten im Repository über das [Protokollsuchportal](log-analytics-log-search-log-search-portal.md) oder das [Advanced Analytics-Portal](https://go.microsoft.com/fwlink/?linkid=856587) durchführen.  Dadurch können Sie Ihre Abfrage bearbeiten und die Ergebnisse in einer Vielzahl von Formaten und Visualisierungen analysieren.  Die meisten Abfragen, die Sie erstellen, beginnen in einem der Portale und werden anschließend kopiert, nachdem Sie sich vergewissert haben, dass sie erwartungsgemäß funktionieren.
- **Warnungsregeln.** [Warnungsregeln](log-analytics-alerts.md) identifizieren proaktiv Probleme durch die Daten in Ihrem Arbeitsbereich.  Jede Warnungsregel basiert auf einer Protokollsuche, die in regelmäßigen Abständen automatisch ausgeführt wird.  Die Ergebnisse werden überprüft, um zu ermitteln, ob eine Warnung erstellt werden soll.
- **Ansichten.**  Sie können mit [Ansicht-Designer](log-analytics-view-designer.md) Visualisierungen von Daten erstellen, die in Benutzerdashboards einbezogen werden sollen.  Protokollsuchen stellen die von [Kacheln](log-analytics-view-designer-tiles.md) und [Visualisierungsteilen](log-analytics-view-designer-parts.md) in jeder Ansicht verwendeten Daten bereit.  Sie können von den Visualisierungsteilen einen Drilldown in das Protokollsuchportal ausführen, um die Daten weitergehend zu analysieren.
- **Export:**  Wenn Sie Daten vom Log Analytics-Arbeitsbereich nach Excel oder [Power BI](log-analytics-powerbi.md) exportieren, erstellen Sie eine Protokollsuche, um die zu exportierenden Daten zu definieren.
- **PowerShell.** Sie können ein PowerShell-Skript über eine Befehlszeile oder ein Azure Automation-Runbook ausführen, das Daten mithilfe von [Get-AzureRmOperationalInsightsSearchResults](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightssearchresults?view=azurermps-4.0.0) von Log Analytics abruft.  Dieses Cmdlet erfordert eine Abfrage, um die abzurufenden Daten festzulegen.
- **Log Analytics-API.**  Die [Log Analytics-Protokollsuch-API](log-analytics-log-search-api.md) ermöglicht einem beliebigen REST-API-Client, Daten aus einem Arbeitsbereich abzurufen.  Die API-Anforderung enthält eine Abfrage, die für Log Analytics ausgeführt wird, um die abzurufenden Daten zu ermitteln.

![Protokollsuchvorgänge](media/log-analytics-log-search-new/log-search-overview.png)

## <a name="how-log-analytics-data-is-organized"></a>Aufbau von Log Analytics-Daten
Wenn Sie eine Abfrage erstellen, legen Sie zuerst fest, welche Tabellen die gesuchten Daten aufweisen. Jede [Datenquelle](log-analytics-data-sources.md) und [Lösung](../operations-management-suite/operations-management-suite-solutions.md) speichert ihre Daten in dedizierten Tabellen im Log Analytics-Arbeitsbereich.  Die Dokumentation für jede Datenquelle und Lösung enthält den Namen des Datentyps, die sie erstellt, und eine Beschreibung ihrer einzelnen Eigenschaften.     Für viele Abfragen werden nur Daten aus einer einzelnen Tabellen benötigt, andere greifen jedoch möglicherweise verschiedene Optionen zurück, um Daten aus mehreren Tabellen einzuschließen.

![Tabellen](media/log-analytics-log-search-new/queries-tables.png)


## <a name="writing-a-query"></a>Schreiben einer Abfrage
Den Kern von Protokollsuchen in Log Analytics bildet [eine umfangreiche Abfragesprache](https://docs.loganalytics.io/), mit der Sie Daten aus dem Repository auf vielfältige Weise abrufen und analysieren können.  Dieselbe Abfragesprache wird auch für [Application Insights](../application-insights/app-insights-analytics.md) verwendet.  Wenn Sie Protokollsuchen in Log Analytics schreiben möchten, müssen Sie lernen, wie Sie eine Abfrage verfassen.  Sie beginnen in der Regel mit grundlegenden Abfragen und arbeiten sich dann zu erweiterten Funktionen weiter, wenn Ihre Anforderungen komplexer werden.

Die Grundstruktur einer Abfrage ist eine Quelltabelle, gefolgt von einer Reihe von Operatoren, die durch einen senkrechten Strich (`|`) getrennt sind.  Sie können mehrere Operatoren verketten, um die Daten einzugrenzen und erweiterte Funktionen auszuführen.

Beispiel: Sie möchten die zehn Computer mit den meisten Fehlerereignissen über den letzten Tag suchen.

    Event
    | where (EventLevelName == "Error")
    | where (TimeGenerated > ago(1days))
    | summarize ErrorCount = count() by Computer
    | top 10 by ErrorCount desc

Oder Sie möchten Computer suchen, die über den letzten Tag keinen Takt aufwiesen.

    Heartbeat
    | where TimeGenerated > ago(7d)
    | summarize max(TimeGenerated) by Computer
    | where max_TimeGenerated < ago(1d)  

Ein anderes Beispiel ist ein Liniendiagramm mit der Prozessorauslastung für jeden Computer in der letzten Woche.

    Perf
    | where ObjectName == "Processor" and CounterName == "% Processor Time"
    | where TimeGenerated  between (startofweek(ago(7d)) .. endofweek(ago(7d)) )
    | summarize avg(CounterValue) by Computer, bin(TimeGenerated, 5min)
    | render timechart    

Durch diese kurzen Beispiele können Sie sehen, dass unabhängig von der Art der Daten, die denen Sie arbeiten, die Struktur der Abfrage ähnlich ist.  Sie können sie in abgegrenzte Schritte unterteilen, bei denen die resultierenden Daten eines Befehls über die Pipeline an den nächsten Befehl gesendet werden.

Eine vollständige Dokumentation der Azure Log Analytics-Abfragesprache, einschließlich Tutorials und Sprachreferenz, finden Sie in der [Dokumentation der Azure Log Analytics-Abfragesprache](https://docs.loganalytics.io/).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Portale, die Sie zum Erstellen und Bearbeiten von Protokollsuchen verwenden](log-analytics-log-search-portals.md).
- Sehen Sie sich ein [Tutorial zum Schreiben von Abfragen](https://go.microsoft.com/fwlink/?linkid=856078) mit der neuen Abfragesprache an.

