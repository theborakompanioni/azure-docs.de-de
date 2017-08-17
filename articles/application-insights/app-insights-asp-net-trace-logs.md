---
title: Untersuchen von .NET-Ablaufverfolgungsprotokollen in Application Insights
description: Suchen Sie nach mit der Ablaufverfolgung, NLog oder Log4Net generierten Protokollen.
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 0c2a084f-6e71-467b-a6aa-4ab222f17153
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/3/2017
ms.author: bwren
ms.translationtype: HT
ms.sourcegitcommit: 99523f27fe43f07081bd43f5d563e554bda4426f
ms.openlocfilehash: 93808e617f33a5c78e5c89f4dac4680d44724d1c
ms.contentlocale: de-de
ms.lasthandoff: 08/05/2017

---
# <a name="explore-net-trace-logs-in-application-insights"></a>Untersuchen von .NET-Ablaufverfolgungsprotokollen in Application Insights
Wenn Sie NLog, log4Net oder „System.Diagnostics.Trace“ für die Diagnoseablaufverfolgung in Ihrer ASP.NET-Anwendung verwenden, können Sie Ihre Protokolle an [Azure Application Insights][start] senden, um sie dort zu untersuchen und zu durchsuchen. Die Protokolle werden mit den anderen Telemetriedaten aus Ihrer Anwendung zusammengeführt, damit Sie die Ablaufverfolgungen für die Verarbeitung der einzelnen Benutzeranforderungen identifizieren und mit anderen Ereignissen und Ausnahmeberichten in Beziehung setzen können.

> [!NOTE]
> Benötigen Sie das Protokollerfassungsmodul? Dabei handelt es sich um einen nützlichen Adapter für die Protokollierung von Drittanbietern. Falls Sie nicht bereits NLog, log4Net oder „System.Diagnostics.Trace“ verwenden, können Sie auch einfach direkt [Application Insights TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) aufrufen.
>
>

## <a name="install-logging-on-your-app"></a>Installieren der Protokollierung in Ihrer App
Installieren Sie das ausgewählte Protokollierungsframework in Ihrem Projekt. Damit sollte ein Eintrag in der Datei „app.config“ oder „web.config“ vorgenommen werden.

Wenn Sie System.Diagnostics.Trace verwenden, müssen Sie einen Eintrag in „web.config“ hinzufügen:

```XML

    <configuration>
     <system.diagnostics>
       <trace autoflush="false" indentsize="4">
         <listeners>
           <add name="myListener"
             type="System.Diagnostics.TextWriterTraceListener"
             initializeData="TextWriterOutput.log" />
           <remove name="Default" />
         </listeners>
       </trace>
     </system.diagnostics>
   </configuration>
```
## <a name="configure-application-insights-to-collect-logs"></a>Konfigurieren von Application Insights für das Erfassen von Protokollen
Falls Sie dies noch nicht getan haben, **[fügen Sie Ihrem Projekt Application Insights hinzu](app-insights-asp-net.md)**. Es wird eine Option angezeigt, mit der Sie den Log Collector einfügen können.

Sie können auch im Projektmappen-Explorer mit der rechten Maustaste auf **Application Insights konfigurieren** klicken. Wählen Sie die Option **Ablaufverfolgungssammlung konfigurieren**.

*Kein Application Insights-Menü und keine Log Collector-Option?* Lesen Sie in der [Problembehandlung](#troubleshooting)nach.

## <a name="manual-installation"></a>Manuelle Installation
Verwenden Sie diese Methode, wenn der Projekttyp vom Application Insights-Installationsprogramm nicht unterstützt wird (z. B. bei einem Windows-Desktopprojekt).

1. Wenn Sie log4Net oder NLog verwenden möchten, installieren Sie es in Ihrem Projekt.
2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie dann **NuGet-Pakete verwalten**aus.
3. Suchen Sie nach "Application Insights".
4. Wählen Sie das entsprechende Paket aus:

   * Microsoft.ApplicationInsights.TraceListener (zum Erfassen von System.Diagnostics.Trace-Aufrufen)
   * Microsoft.ApplicationInsights.EventSourceListener (zum Erfassen von EventSource-Ereignissen)
   * Microsoft.ApplicationInsights.EtwListener (zum Erfassen von ETW-Ereignissen)
   * Microsoft.ApplicationInsights.NLogTarget
   * Microsoft.ApplicationInsights.Log4NetAppender

Das NuGet-Paket installiert die erforderlichen Assemblys und ändert auch die Datei "web.config" oder "app.config".

## <a name="insert-diagnostic-log-calls"></a>Einfügen von Diagnoseprotokollaufrufen
Wenn Sie System.Diagnostics.Trace verwenden, wäre ein typischer Aufruf:

    System.Diagnostics.Trace.TraceWarning("Slow response - database01");

Wenn Sie log4net oder NLog bevorzugen:

    logger.Warn("Slow response - database01");

## <a name="using-eventsource-events"></a>Verwenden von EventSource-Ereignissen
Sie können [System.Diagnostics.Tracing.EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx)-Ereignisse so konfigurieren, dass Sie als Ablaufverfolgungen an Application Insights gesendet werden. Installieren Sie zunächst das `Microsoft.ApplicationInsights.EventSourceListener`-NuGet-Paket. Bearbeiten Sie anschließend den Abschnitt `TelemetryModules` der Datei [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md).

```xml
    <Add Type="Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule, Microsoft.ApplicationInsights.EventSourceListener">
      <Sources>
        <Add Name="MyCompany" Level="Verbose" />
      </Sources>
    </Add>
```

Für jede Datenquelle können Sie die folgenden Parameter festlegen:
 * `Name` gibt den Namen des zu erfassenden EventSource-Elements an.
 * `Level` gibt den zu erfassenden Protokolliergrad an. Kann `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose` oder `Warning` sein.
 * `Keywords` (Optional) Gibt den ganzzahligen Wert der zu verwendenden Schlüsselwortkombinationen an.

## <a name="using-diagnosticsource-events"></a>Verwenden von DiagnosticSource-Ereignissen
Sie können [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md)-Ereignisse so konfigurieren, dass sie als Ablaufverfolgungen an Application Insights gesendet werden. Installieren Sie zunächst das NuGet-Paket [`Microsoft.ApplicationInsights.DiagnosticSourceListener`](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener). Bearbeiten Sie anschließend den Abschnitt `TelemetryModules` der Datei [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md).

```xml
    <Add Type="Microsoft.ApplicationInsights.DiagnsoticSourceListener.DiagnosticSourceTelemetryModule, Microsoft.ApplicationInsights.DiagnosticSourceListener">
      <Sources>
        <Add Name="MyDiagnosticSourceName" />
      </Sources>
    </Add>
```

Fügen Sie für jedes DiagnosticSource-Ereignis, das nachverfolgt werden soll, einen Eintrag hinzu, bei dem das Attribut `Name` auf den Namen von DiagnosticSource festgelegt ist.

## <a name="using-etw-events"></a>Verwenden von ETW-Ereignissen
Sie können ETW-Ereignisse so konfigurieren, dass sie als Ablaufverfolgungen an Application Insights gesendet werden. Installieren Sie zunächst das `Microsoft.ApplicationInsights.EtwCollector`-NuGet-Paket. Bearbeiten Sie anschließend den Abschnitt `TelemetryModules` der Datei [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md).

> [!NOTE] 
> ETW-Ereignisse können nur gesammelt werden, wenn der Prozess, der das SDK hostet, unter einer Identität ausgeführt wird, die Mitglied von „Leistungsprotokollbenutzer“ oder „Administratoren“ ist.

```xml
    <Add Type="Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule, Microsoft.ApplicationInsights.EtwCollector">
      <Sources>
        <Add ProviderName="MyCompanyEventSourceName" Level="Verbose" />
      </Sources>
    </Add>
```

Für jede Datenquelle können Sie die folgenden Parameter festlegen:
 * `ProviderName` ist der Name des zu erfassenden ETW-Anbieters.
 * `ProviderGuid` gibt die GUID des zu erfassenden ETW-Anbieter an. Kann anstelle von `ProviderName` verwendet werden.
 * `Level` legt den zu erfassenden Protokolliergrad fest. Kann `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose` oder `Warning` sein.
 * `Keywords` (Optional) Legt den ganzzahligen Wert der Schlüsselwortkombinationen fest, die verwendet werden sollen.

## <a name="using-the-trace-api-directly"></a>Direktes Verwenden der Ablaufverfolgungs-API
Sie können die API zur Application Insights-Ablaufverfolgung direkt aufrufen. Die Protokollierungsadapter verwenden diese API.

Beispiel:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow response - database01");

Ein Vorteil von TrackTrace ist, dass relativ lange Daten in die Nachricht eingefügt werden können. Sie können dort z. B. POST-Daten codieren.

Darüber hinaus können Sie Ihrer Nachricht einen Schweregrad hinzufügen. Wie andere Telemetriedaten können Sie Eigenschaftswerte hinzufügen, die Sie verwenden können, um zu filtern oder nach verschiedene Spuren zu suchen. Beispiel:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

Auf diese Weise können Sie in der [Suche][diagnostic] alle Nachrichten eines bestimmten Schweregrads, die im Zusammenhang mit einer bestimmten Datenbank stehen, einfach herausfiltern.

## <a name="explore-your-logs"></a>Untersuchen Ihrer Protokolle
Führen Sie die App im Debugmodus aus, oder stellen Sie sie live bereit.

Wählen Sie im Übersichtsblatt im [Application Insights-Portal][portal] die Option [Suchen][diagnostic].

![Wählen Sie in Application Insights die Option "Suchen".](./media/app-insights-asp-net-trace-logs/020-diagnostic-search.png)

![Suche](./media/app-insights-asp-net-trace-logs/10-diagnostics.png)

Sie haben beispielsweise folgende Möglichkeiten:

* Filtern nach Protokollablaufverfolgungen oder nach Elementen mit bestimmten Eigenschaften
* Untersuchen eines bestimmten Elements im Detail
* Finden weiterer Telemetriedaten im Zusammenhang mit derselben Benutzeranforderung (d. h. mit derselben OperationId)
* Speichern der Konfiguration dieser Seite als Favorit

> [!NOTE]
> **Stichproben** Wenn Ihre Anwendung eine große Menge von Daten sendet und Sie das Application Insights-SDK für ASP.NET Version 2.0.0-beta3 oder höher verwenden, wird möglicherweise die adaptive Stichprobenerstellung verwendet, bei der nur ein bestimmter Prozentsatz der Telemetriedaten übermittelt wird. [Erfahren Sie mehr über das Erstellen von Stichproben.](app-insights-sampling.md)
>
>

## <a name="next-steps"></a>Nächste Schritte
[Diagnostizieren von Fehlern und Ausnahmen in ASP.NET][exceptions]

[Weitere Informationen zur Suche.][diagnostic]

## <a name="troubleshooting"></a>Problembehandlung
### <a name="how-do-i-do-this-for-java"></a>Wie geht das mit Java?
Verwenden Sie die [Java-Protokolladapter](app-insights-java-trace-logs.md).

### <a name="theres-no-application-insights-option-on-the-project-context-menu"></a>Keine Application Insights-Option im Kontextmenü des Projekts
* Überprüfen Sie, ob die Application Insights-Tools auf diesem Entwicklungscomputer installiert sind. Suchen Sie in Visual Studio im Menü „Extras“ unter „Erweiterungen und Updates“ nach „Application Insights-Tools“. Wenn die Option nicht auf der Registerkarte „Installiert“ angezeigt wird, öffnen Sie die Registerkarte „Online“ und installieren sie.
* Möglicherweise wird das Projekt von den Application Insights-Tools nicht unterstützt. Verwenden Sie die [manuelle Installation](#manual-installation).

### <a name="no-log-adapter-option-in-the-configuration-tool"></a>Keine Option für die Protokolladapter im Konfigurationstool
* Sie müssen zunächst das Protokollierungsframework installieren.
* Wenn Sie „System.Diagnostics.Trace“ verwenden, müssen Sie die [Konfiguration in `web.config` vornehmen](https://msdn.microsoft.com/library/system.diagnostics.eventlogtracelistener.aspx).
* Haben Sie die neueste Version von Application Insights? Wählen Sie in Visual Studio im Menü **Extras** die Option **Erweiterungen und Updates** aus, und öffnen Sie die Registerkarte **Updates**. Wenn die Developer Analytics-Tools hier aufgeführt werden, klicken Sie darauf, um sie zu aktualisieren.

### <a name="emptykey"></a>Eine Fehlermeldung "Instrumentationsschlüssel darf nicht leer sein" wird angezeigt.
Anscheinend haben Sie das Protokollierungsadapter-Nuget-Paket installiert, ohne Application Insights zu installieren.

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf `ApplicationInsights.config`. Wählen Sie dann **Application Insights aktualisieren** aus. Ein Dialogfeld wird angezeigt, das Sie zur Anmeldung bei Azure und zum Erstellen einer Application Insights-Ressource oder dem Wiederverwenden einer vorhandenen Ressource einlädt. Damit sollte das Problem behoben sein.

### <a name="i-can-see-traces-in-diagnostic-search-but-not-the-other-events"></a>Ablaufverfolgungen werden in der Diagnosesuche angezeigt, aber nicht die anderen Ereignisse.
Manchmal kann es eine Weile dauern, bis alle Ereignisse und Anforderungen über die Pipeline abgerufen werden.

### <a name="limits"></a>Wie viele Daten werden beibehalten?
Die Menge der beibehaltenen Daten hängt von mehreren Faktoren ab. Weitere Informationen finden Sie auf der Seite mit den Metriken für benutzerdefinierte Ereignisse im Abschnitt [Grenzwerte](app-insights-api-custom-events-metrics.md#limits). 

### <a name="im-not-seeing-some-of-the-log-entries-that-i-expect"></a>Ich kann einige erwartete Protokolleinträge nicht finden.
Wenn Ihre Anwendung eine große Menge von Daten sendet und Sie das Application Insights-SDK für ASP.NET Version 2.0.0-beta3 oder höher verwenden, wird möglicherweise die adaptive Stichprobenerstellung verwendet, bei der nur ein bestimmter Prozentsatz der Telemetriedaten übermittelt wird. [Erfahren Sie mehr über das Erstellen von Stichproben.](app-insights-sampling.md)

## <a name="add"></a>Nächste Schritte
* [Einrichten von Tests zu Verfügbarkeit und Reaktionsfähigkeit][availability]
* [Behandeln von Problemen][qna]

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[portal]: https://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md

