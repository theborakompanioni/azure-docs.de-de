---
title: "Azure Application Insights Snapshot-Debugger für .NET-Apps | Microsoft Docs"
description: "Debuggen von Momentaufnahmen, die beim Auslösen von Ausnahmen in .NET-Produktions-Apps automatisch erfasst werden"
services: application-insights
documentationcenter: 
author: qubitron
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/30/2017
ms.author: cfreeman
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 94b073df00b690f1347e246eeb7e12fdd1bd7cc1
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="debug-snapshots-on-exceptions-in-net-apps"></a>Debuggen von Momentaufnahmen zu Ausnahmen in .NET Apps

*Dieses Feature befindet sich in der Vorschau.*

Erfassen Sie automatisch eine Debug-Momentaufnahme aus Ihrer Liv-Webanwendung, wenn eine Ausnahme auftritt. Die Momentaufnahme zeigt den Status von Quellcode und Variablen in dem Moment, in dem die Ausnahme ausgelöst wurde. Der Snapshot Debugger in [Application Insights](app-insights-overview.md) überwacht Ausnahmetelemetriedaten aus Ihrer Web-App. Er erfasst Momentaufnahmen Ihrer am häufigsten ausgelösten Ausnahmen, damit Sie die erforderlichen Informationen zur Diagnose von Problemen in der Produktion erhalten. Binden Sie das [Snapshot Collector NuGet-Paket](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) in Ihre Anwendung ein, und konfigurieren Sie optional Parameter für die Datensammlung in [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). Momentaufnahmen finden Sie im Application Insights-Portal unter [Ausnahmen](app-insights-asp-net-exceptions.md).

Sie können Debug-Momentaufnahmen im Portal aufrufen, um die Aufrufliste anzuzeigen und die Variablen in jedem Stapelrahmen zu überprüfen. Öffnen Sie zum Verbessern Ihrer Debugleistung mit Quellcode die Momentaufnahmen mit Visual Studio 2017 Enterprise, indem Sie [die Snapshot-Debugger-Erweiterung für Visual Studio herunterladen](https://aka.ms/snapshotdebugger).

Die Erfassung von Momentaufnahmen ist für ASP.NET Web-Apps verfügbar, die auf .NET Framework 4.6 und höher ausgeführt und auf IIS in Azure Compute oder in Azure App Service gehostet werden.

## <a name="configure-snapshot-collection"></a>Konfigurieren der Erfassung von Momentaufnahmen

1. Falls noch nicht geschehen, [aktivieren Sie Application Insights für Ihre Web-App](app-insights-asp-net.md).

2. Binden Sie das NuGet-Paket [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) in Ihre App ein. 

3. Überprüfen Sie die Standardoptionen, die [ ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) vom Paket hinzugefügt wurden.

```xml
  <TelemetryProcessors>
    <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
      <!-- The default is true, but you can disable Snapshot Debugging by setting it to false -->
      <IsEnabled>true</IsEnabled>
      <!-- Snapshot Debugging is usually disabled in developer mode, but you can enable it by setting this to true. -->
      <!-- DeveloperMode is a property on the active TelemetryChannel. -->
      <IsEnabledInDeveloperMode>false</IsEnabledInDeveloperMode>
      <!-- How many times we need to see an exception before we ask for snapshots. -->
      <ThresholdForSnapshotting>5</ThresholdForSnapshotting>
      <!-- The maximum number of examples we create for a single problem. -->
      <MaximumSnapshotsRequired>3</MaximumSnapshotsRequired>
      <!-- The maximum number of problems that we can be tracking at any time. -->
      <MaximumCollectionPlanSize>50</MaximumCollectionPlanSize>
      <!-- How often to reset problem counters. -->
      <ProblemCounterResetInterval>06:00:00</ProblemCounterResetInterval>
      <!-- The maximum number of snapshots allowed in one minute. -->
      <SnapshotsPerMinuteLimit>2</SnapshotsPerMinuteLimit>
      <!-- The maximum number of snapshots allowed per day. -->
      <SnapshotsPerDayLimit>50</SnapshotsPerDayLimit>
    </Add>
  </TelemetryProcessors>
```

Momentaufnahmen werden nur von Ausnahmen erfasst, die für das Application Insights SDK-sichtbar sind. In einigen Fällen müssen Sie möglicherweise [die Erfassung von Ausnahmen konfigurieren](app-insights-asp-net-exceptions.md#exceptions), damit Ausnahmen mit Momentaufnahmen im Portal angezeigt werden.

## <a name="debugging-snapshots-in-the-application-insights-portal"></a>Debuggen von Momentaufnahmen im Application Insights-Portal

Wenn eine Momentaufnahme für eine bestimmte Ausnahme oder eine Problem-ID verfügbar ist, wird ein Link zum *Erstellen einer Debug-Momentaufnahme* unter der [Ausnahme](app-insights-asp-net-exceptions.md) im Application Insights-Portal angezeigt.

![Schaltfläche zum Erstellen einer Debug-Momentaufnahme für eine Ausnahme](./media/app-insights-snapshot-debugger/snapshot-on-exception.png)

In der Ansicht der Debug-Momentaufnahme sehen Sie eine Aufrufliste und einen Variablenbereich. Wenn Sie Frames der Aufrufliste im Aufruflistenbereich auswählen, können Sie lokale Variablen und Parameter für diesen Funktionsaufruf im Variablenbereich anzeigen.

![Anzeigen der Debug-Momentaufnahme im Portal](./media/app-insights-snapshot-debugger/open-snapshot-portal.png)

Momentaufnahmen enthalten möglicherweise vertrauliche Informationen und sind standardmäßig nicht sichtbar. Wenn Sie Momentaufnahmen anzeigen möchten, muss Ihnen im Verwaltungsportal für das Abonnement oder die Ressource die Rolle `Application Insights Snapshot Debugger` zugewiesen sein. Diese Rolle kann derzeit nur von Abonnementbesitzern auf Benutzerbasis zugewiesen werden. Azure Active Directory-Gruppen kann diese Rolle derzeit nicht zugewiesen werden.

## <a name="debugging-snapshots-with-visual-studio-2017-enterprise"></a>Debuggen von Momentaufnahmen mit Visual Studio 2017 Enterprise
Klicken Sie auf die Schaltfläche *Momentaufnahme herunterladen*, um eine `.diagsession`-Datei herunterzuladen, die von Visual Studio 2017 Enterprise geöffnet werden kann. Zum Öffnen der `.diagsession`-Datei müssen Sie derzeit zuerst [die Snapshot-Debugger-Erweiterung für Visual Studio herunterladen und installieren](https://aka.ms/snapshotdebugger).

Nach dem Öffnen der Momentaufnahme-Datei werden Sie auf die Minidump-Debugging-Seite von Visual Studio weitergeleitet, auf der Sie das Debuggen der Momentaufnahme starten können, indem Sie auf *Debug Managed Code* (Verwalteten Code debuggen) klicken. Sie werden zu der Codezeile geleitet, in der die Ausnahme ausgelöst wurde, und können den aktuellen Status des Prozesses debuggen.

![Anzeigen der Debug-Momentaufnahme in Visual Studio](./media/app-insights-snapshot-debugger/open-snapshot-visualstudio.png)

Die heruntergeladene Momentaufnahme enthält alle Symboldateien, die auf Ihrem Webanwendungsserver gefunden wurden. Diese Symboldateien sind zum Zuordnen von Quellcode und Momentaufnahmedaten erforderlich. Aktivieren Sie für Azure App Service-Apps die Bereitstellung von Symbolen, wenn Sie Ihre Web-Apps veröffentlichen.

## <a name="how-snapshots-work"></a>Funktionsweise von Momentaufnahmen

Beim Starten Ihrer Anwendung wird ein separater Uploader-Prozess für die Momentaufnahme erstellt, der Ihre Anwendung auf Momentaufnahmeanforderungen überwacht. Beim Anfordern einer Momentaufnahme wird in etwa 10 bis 20 ms eine Schattenkopie des ausgeführten Prozesses erstellt. Der Schattenprozess wird dann analysiert, und eine Momentaufnahme wird erstellt, während der Hauptprozess weiterhin ausgeführt wird und Datenverkehr für Benutzer verarbeitet. Die Momentaufnahme wird dann mit allen relevanten Symboldateien (.pdb), die zum Anzeigen der Momentaufnahme erforderlich sind, in Application Insights hochgeladen.

## <a name="current-limitations"></a>Aktuelle Einschränkungen

### <a name="publishing-symbols"></a>Veröffentlichen von Symbolen
Der Momentaufnahme-Debugger erfordert, dass Symboldateien auf dem Produktionsserver vorhanden sein müssen, um Variablen zu decodieren und eine gute Debugleistung in Visual Studio zu erzielen. Die Version 15.2 von Visual Studio 2017 veröffentlicht Symbole für Releasebuilds standardmäßig beim Veröffentlichen in Azure App Service. In früheren Versionen mussten Sie der `.pubxml`-Datei Ihres Veröffentlichungsprofils die folgende Zeile hinzufügen, um Symbole im Releasemodus zu veröffentlichen.

```xml
    <ExcludeGeneratedDebugSymbol>False</ExcludeGeneratedDebugSymbol>
```

Stellen Sie für Azure Compute und andere Typen sicher, dass die Symboldateien im selben Ordner wie die .dll-Datei der Hauptanwendung (in der Regel `wwwroot/bin`) liegen oder auf dem aktuellen Pfad verfügbar sind.

### <a name="optimized-builds"></a>Optimierte Builds
In einigen Fällen können lokale Variablen aufgrund von Optimierungen, die während des Buildprozesses angewendet werden, nicht in Releasebuilds angezeigt werden. Diese Einschränkung wird in einer zukünftigen Version des NuGet-Pakets behoben.

## <a name="next-steps"></a>Nächste Schritte

* Unter [Diagnostizieren von Ausnahmen in Ihren Web-Apps](app-insights-asp-net-exceptions.md) erfahren Sie, wie Sie weitere Ausnahmen für Application Insights sichtbar machen. 
* Die [intelligente Erkennung](app-insights-proactive-diagnostics.md) ermittelt automatisch Leistungsanomalien.

