---
title: Leistungsindikatoren in Application Insights | Microsoft Docs
description: "Überwachen Sie systemeigene und benutzerdefinierte .NET-Leistungsindikatoren in Application Insights."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 5b816f4c-a77a-4674-ae36-802ee3a2f56d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/11/2016
ms.author: cfreeman
ms.translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 94a4d2b85c845b135201fd919e8eac64a5e1cb37
ms.contentlocale: de-de
ms.lasthandoff: 11/17/2016


---
# <a name="system-performance-counters-in-application-insights"></a>Systemleistungsindikatoren in Application Insights
Windows bietet eine Vielzahl von [Leistungsindikatoren](http://www.codeproject.com/Articles/8590/An-Introduction-To-Performance-Counters) wie z.B. CPU-Belegung, Arbeitsspeicher, Datenträger und Netzwerkverwendung. Sie können auch eigene definieren. Diese Leistungsindikatoren können in [Application Insights](app-insights-overview.md) angezeigt werden, sofern Ihre Anwendung unter IIS auf einem lokalen Host oder auf einem virtuellen Computer ausgeführt wird, auf den Sie Administratorzugriff haben. In den Diagrammen werden die für Ihre aktive Anwendung verfügbaren Ressourcen angezeigt. Zudem können Sie anhand der Diagramme eine unausgeglichene Belastung der Serverinstanzen erkennen.

Leistungsindikatoren werden auf dem Blatt „Server“ angezeigt, das eine nach Serverinstanzen segmentierte Tabelle enthält.

![In Application Insights gemeldete Leistungsindikatoren](./media/app-insights-performance-counters/counters-by-server-instance.png)

(Leistungsindikatoren sind für Azure-Web-Apps nicht verfügbar. Sie können jedoch [Azure-Diagnosedaten an Application Insights senden](app-insights-azure-diagnostics.md).)

## <a name="configure"></a>Konfigurieren
Wenn Application Insights-Statusmonitor noch nicht auf den Server-Computern installiert ist, müssen Sie ihn installieren, damit Leistungsindikatoren angezeigt werden.

Laden Sie das [Installationsprogramm für den Statusmonitor](http://go.microsoft.com/fwlink/?LinkId=506648) herunter, und führen Sie es auf jeder Serverinstanz aus. Wenn der Monitor bereits installiert ist, müssen Sie ihn nicht erneut installieren.

* *Ich [habe das Application Insights SDK in meiner Anwendung während der Entwicklung installiert](app-insights-asp-net.md). Benötige ich dennoch den Statusmonitor?*
  
    Ja, der Statusmonitor wird zum Erfassen von Leistungsindikatoren für ASP.NET-Webanwendungen benötigt. Wie Sie möglicherweise bereits wissen, kann der Statusmonitor auch zum [Überwachen von bereits aktiven Webanwendungen](app-insights-monitor-performance-live-website-now.md) verwendet werden, ohne dass das SDK während der Entwicklung installiert werden muss.

## <a name="view-counters"></a>Anzeigen von Indikatoren
Auf dem Blatt „Server“ wird ein Standardsatz von Leistungsindikatoren angezeigt. 

Wenn Sie andere Leistungsindikatoren anzeigen möchten, können Sie entweder die Diagramme auf dem Blatt „Server“ bearbeiten oder im [Metrik-Explorer](app-insights-metrics-explorer.md) ein neues Blatt öffnen und neue Diagramme hinzufügen. 

Die verfügbaren Leistungsindikatoren werden beim Bearbeiten eines Diagramms als Metriken angezeigt.

![In Application Insights gemeldete Leistungsindikatoren](./media/app-insights-performance-counters/choose-performance-counters.png)

Damit die nützlichsten Diagramme an einer Stelle angezeigt werden, erstellen Sie ein [Dashboard](app-insights-dashboards.md) und heften Sie die Diagramme an das Dashboard.

## <a name="add-counters"></a>Hinzufügen von Leistungsindikatoren
Wenn der gewünschte Leistungsindikator in der Liste der Metriken nicht angezeigt wird, liegt das daran, dass dieser vom Application Insights SDK auf Ihrem Webserver nicht erfasst wird. Sie können dies entsprechend konfigurieren.

1. Ermitteln Sie mit dem folgenden PowerShell-Befehl, welche Indikatoren auf dem Server verfügbar sind:
   
    `Get-Counter -ListSet *`
   
    (Informationen hierzu finden Sie unter [`Get-Counter`](https://technet.microsoft.com/library/hh849685.aspx).)
2. Öffnen Sie "ApplicationInsights.config".
   
   * Wenn Sie Application Insights während der Entwicklung zu Ihrer Anwendung hinzugefügt haben, bearbeiten Sie die Datei „ApplicationInsights.config“ in Ihrem Projekt und stellen Sie sie anschließend erneut auf Ihren Servern bereit.
   * Wenn Sie den Statusmonitor zum Instrumentieren einer Web-App zur Laufzeit verwendet haben, finden Sie die Datei „ApplicationInsights.config“ im Stammverzeichnis der App in IIS. Aktualisieren Sie sie dort in allen Serverinstanzen.
3. Bearbeiten Sie Leistungserfassungsanweisung:
   
```XML
   
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <Add PerformanceCounter="\Objects\Processes"/>
        <Add PerformanceCounter="\Sales(photo)\# Items Sold" ReportAs="Photo sales"/>
      </Counters>
    </Add>

```

Sie können sowohl Standardindikatoren als auch Indikatoren erfassen, die Sie selbst implementiert haben. `\Objects\Processes` ist ein Beispiel für einen Standardindikator, der auf allen Windows-Systemen verfügbar ist. `\Sales(photo)\# Items Sold` ist ein Beispiel für einen benutzerdefinierten Indikator, der in einem Webdienst implementiert sein kann. 

Das Format lautet `\Category(instance)\Counter"` bzw. für Kategorien, die keine Instanzen besitzen, einfach `\Category\Counter`.

`ReportAs` ist für Leistungsindikatornamen erforderlich, die mit diesen Zeichen `[a-zA-Z()/-_ \.]+` nicht übereinstimmen, d.h., sie enthalten Zeichen, die nicht zu folgenden Gruppen zählen: Buchstaben, runde Klammern, Schrägstrich, Bindestrich, Unterstrich, Leerzeichen, Punkt.

Wenn Sie eine Instanz angeben, wird sie als CounterInstanceName-Dimension der gemeldeten Metrik erfasst.

### <a name="collecting-performance-counters-in-code"></a>Erfassen von Leistungsindikatoren im Code
Um Systemleistungsindikatoren zu erfassen und an Application Insights zu senden, können Sie den folgenden Codeausschnitt anpassen:


``` C#

    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\.NET CLR Memory([replace-with-application-process-name])\# GC Handles", "GC Handles")));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

Alternativ können Sie dieselben Schritte mit von Ihnen erstellten benutzerdefinierten Metriken ausführen:

``` C#
    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\Sales(photo)\# Items Sold", "Photo sales"));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

## <a name="performance-counters-in-analytics"></a>Leistungsindikatoren in Analytics
In [Analytics](app-insights-analytics.md) können Sie nach Leistungsindikatorberichten suchen und diese anzeigen.

Das Schema **performanceCounters** zeigt die `category`, den `counter`-Namen und `instance`-Namen der einzelnen Leistungsindikatoren.  In den Telemetriedaten jeder Anwendung sehen Sie nur die Indikatoren für diese Anwendung. Beispielsweise, um verfügbare Leistungsindikatoren anzuzeigen: 

![Leistungsindikatoren in der Application Insights-Analyse](./media/app-insights-performance-counters/analytics-performance-counters.png)

('Instance' bezieht sich hier auf die Instanz des Leistungsindikators, nicht auf die Instanz der Rolle oder des Server-Computers. Leistungsindikatoren wie etwa die Prozessorzeit werden vom Namen der Leistungsindikatorinstanz in der Regel nach dem Namen des Prozesses oder der Anwendung segmentiert.)

So erhalten Sie ein Diagramm des verfügbaren Arbeitsspeichers im aktuellen Zeitraum: 

![Zeitdiagramm der Speicherauslastung in der Application Insights-Analyse](./media/app-insights-performance-counters/analytics-available-memory.png)

Wie andere Telemetriedaten umfasst auch **performanceCounters** eine Spalte `cloud_RoleInstance`, die die Identität der Hostserverinstanz angibt, auf dem Ihre Anwendung ausgeführt wird. Geben Sie beispielsweise Folgendes ein, um die Leistung Ihrer App auf verschiedenen Computern vergleichen: 

![Nach Rolleninstanz in der Application Insights-Analyse segmentierte Leistung](./media/app-insights-performance-counters/analytics-metrics-role-instance.png)

## <a name="aspnet-and-application-insights-counts"></a>ASP.NET- und Application Insights-Zahlen
*Worin besteht der Unterschied zwischen der Ausnahmerate und der Ausnahmenmetrik?*

* *Ausnahmerate* ist ein Systemleistungsindikator. Die CLR zählt alle behandelten und nicht behandelten Ausnahmen, die ausgelöst werden, und dividiert das Ergebnis innerhalb eines Samplingintervalls durch die Länge dieses Intervalls. Das Application Insights SDK sammelt dieses Ergebnis und sendet es an das Portal.
* *Ausnahmen* ist die Anzahl der TrackException-Meldungen, die das Portal innerhalb des Samplingintervalls des Diagramms empfangen hat. Sie enthält nur die behandelten Ausnahmen, wo Sie TrackException-Aufrufe in Ihren Code geschrieben haben, und enthält nicht alle [nicht behandelten Ausnahmen](app-insights-asp-net-exceptions.md). 

## <a name="alerts"></a>Warnungen
Wie bei anderen Metriken können Sie [eine Warnung festlegen](app-insights-alerts.md), damit Sie gewarnt werden, wenn ein Leistungsindikator einen von Ihnen festgelegten Grenzwert überschreitet. Öffnen Sie das Blatt „Warnungen“, und klicken Sie auf „Warnung hinzufügen“.

## <a name="next"></a>Nächste Schritte
* [Abhängigkeitsüberwachung](app-insights-asp-net-dependencies.md)
* [Ausnahmeverfolgung](app-insights-asp-net-exceptions.md)


