---
title: "Azure Application Insights für Windows-Server und Workerrollen | Microsoft-Dokumentation"
description: "Fügen Sie das Application Insights SDK manuell zu Ihrer ASP.NET-Anwendung hinzu, um Nutzung, Verfügbarkeit und Leistung zu analysieren."
services: application-insights
documentationcenter: .net
author: alancameronwills
manager: carmonm
ms.assetid: 106ba99b-b57a-43b8-8866-e02f626c8190
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/01/2016
ms.author: cfreeman
ms.translationtype: Human Translation
ms.sourcegitcommit: 0c4554d6289fb0050998765485d965d1fbc6ab3e
ms.openlocfilehash: 29598f052778759ed362e3aa4b997acb799717ef
ms.contentlocale: de-de
ms.lasthandoff: 04/13/2017


---
# <a name="manually-configure-application-insights-for-aspnet-applications"></a>Manuelles Konfigurieren von Application Insights für ASP.NET-Anwendungen
[Application Insights](app-insights-overview.md) ist ein erweiterbares Tool für Webentwickler zum Überwachen der Leistung und Verwendung der Liveanwendung. Sie können dieses Tool manuell für die Überwachung von Windows-Server, Workerrollen und anderen ASP.NET-Anwendungen konfigurieren. Bei Web-Apps ist die manuelle Konfiguration eine Alternative zu der von Visual Studio bereitgestellten [automatischen Einrichtung](app-insights-asp-net.md) .

![Beispiel für Leistungsüberwachungsdiagramm](./media/app-insights-windows-services/10-perf.png)

#### <a name="before-you-start"></a>Vorbereitung
Erforderlich:

* Ein Abonnement für [Microsoft Azure](http://azure.com). Wenn Ihr Team oder Ihre Organisation über ein Azure-Abonnement verfügt, kann der Besitzer Sie mit Ihrem [Microsoft-Konto](http://live.com)hinzufügen.
* Visual Studio 2013 oder höher

## <a name="add"></a>1. Erstellen einer Application Insights-Ressource
Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an, und erstellen Sie eine neue Application Insights-Ressource. Wählen Sie als Anwendungstyp "ASP.NET" aus.

![Klicken Sie auf "Neu > Application Insights"](./media/app-insights-windows-services/01-new-asp.png)

Eine [Ressource](app-insights-resources-roles-access-control.md) in Azure ist eine Instanz eines Diensts. In dieser Ressource werden Telemetriedaten aus Ihrer App analysiert und Ihnen angezeigt.

Durch Auswahl des Anwendungstyps werden der Standardinhalt der Ressourcenblätter und die im [Metrik-Explorer](app-insights-metrics-explorer.md)sichtbaren Eigenschaften festgelegt.

#### <a name="copy-the-instrumentation-key"></a>Kopieren des Instrumentationsschlüssels
Der Schlüssel identifiziert die Ressource, den Sie bald im SDK installieren können, um die Daten an die Ressource zu leiten.

![Klicken Sie auf "Eigenschaften", wählen Sie den Schlüssel aus, und drücken Sie STRG+C](./media/app-insights-windows-services/02-props-asp.png)

Die Schritte, die Sie gerade zum Erstellen einer neuen Ressource getan haben, sind eine gute Möglichkeit zum Starten der Überwachung einer Anwendung. Nun können Sie Daten zur Anwendung senden.

## <a name="sdk"></a>2. Installieren des Application Insights-Pakets in Ihrer Anwendung
Die Vorgehensweise zum Installieren und Konfigurieren des Application Insights-Pakets variiert abhängig von der Plattform, mit der Sie gerade arbeiten. Bei ASP.NET-Apps ist es einfach.

1. Bearbeiten Sie die NuGet-Pakete Ihres Web-App-Projekts in Visual Studio.
   
    ![Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie "NuGet-Pakete verwalten".](./media/app-insights-windows-services/03-nuget.png)
2. Installieren Sie das Application Insights-Paket für Windows-Server-Apps.
   
    ![Suchen Sie nach "Application Insights".](./media/app-insights-windows-services/04-ai-nuget.png)
   
    *Kann ich andere Pakete verwenden?*
   
    Ja. Wählen Sie die Kern-API (Microsoft.ApplicationInsights), wenn Sie die API nur verwenden möchten, um Ihre eigenen Telemetriedaten zu senden. Das Windows Server-Paket beinhaltet automatisch die Kern-API sowie einige weitere Pakete wie etwa die Sammlung von Leistungsindikatoren und die Abhängigkeitsüberwachung. 

#### <a name="to-upgrade-to-future-package-versions"></a>So führen Sie ein Upgrade auf zukünftige Paketversionen durch
Von Zeit zu Zeit veröffentlichen wir eine neue Version des SDK.

Wenn Sie ein Upgrade auf eine [neue Paketversion](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases/) durchführen möchten, öffnen Sie erneut den NuGet-Paket-Manager, und filtern Sie nach installierten Paketen. Wählen Sie **Microsoft.ApplicationInsights.WindowsServer** und anschließend **Upgrade** aus.

Wenn Sie Anpassungen an der Datei "ApplicationInsights.config" vorgenommen haben, speichern Sie vor dem Upgrade eine Kopie davon. Sie können anschließend die Änderungen in die neue Version übernehmen.

## <a name="3-send-telemetry"></a>3. Senden von Telemetriedaten
**Wenn nur das Core-API-Paket installiert wurde:**

* Legen Sie den Instrumentationsschlüssel im Code (beispielsweise in `main()`) fest: 
  
    `TelemetryConfiguration.Active.InstrumentationKey = "` *Ihr Schlüssel* `";` 
* [Schreiben Sie mithilfe der API eigene Telemetrie.](app-insights-api-custom-events-metrics.md#ikey)

**Wenn Sie andere Application Insights-Pakete installiert haben** , können Sie zum Festlegen des Instrumentationsschlüssels auch die CONFIG-Datei verwenden:

* Bearbeiten Sie die Datei "ApplicationInsights.config" (die bei der NuGet-Installation hinzugefügt wurde). Fügen Sie Folgendes direkt vor dem Endtag ein:
  
    `<InstrumentationKey>` *Der kopierte Instrumentationsschlüssel* `</InstrumentationKey>`
* Stellen Sie sicher, dass die Eigenschaften von „ApplicationInsights.config“ im Projektmappen-Explorer auf **Buildvorgang = Inhalt, In Ausgabeverzeichnis kopieren = Kopieren**festgelegt sind.

Es ist nützlich, den Instrumentierungsschlüssel im Code festzulegen, wenn Sie [den Schlüssel für verschiedene Buildkonfigurationen wechseln möchten](app-insights-separate-resources.md). Wenn Sie den Schlüssel im Code festlegen, müssen Sie ihn nicht in der Datei `.config` festlegen.

## <a name="run"></a> Ausführen des Projekts
Starten Sie Ihre Anwendung mit **F5** , und testen Sie sie: Öffnen Sie verschiedene Seiten, um einige Telemetriedaten zu generieren.

In Visual Studio sehen Sie eine Anzahl der gesendeten Ereignisse.

![Ereignisanzahl in Visual Studio](./media/app-insights-windows-services/appinsights-09eventcount.png)

## <a name="monitor"></a> Anzeigen der Telemetrie
Kehren Sie zum [Azure-Portal](https://portal.azure.com/) zurück, und navigieren Sie zur Application Insights-Ressource.

Suchen Sie nach Daten in der Übersichtsdiagrammen. Zuerst sehen Sie lediglich einen oder zwei Punkte. Zum Beispiel:

![Klicken Sie, um weitere Daten anzuzeigen.](./media/app-insights-windows-services/12-first-perf.png)

Klicken Sie sich durch ein beliebiges Diagramm, um ausführlichere Metriken anzuzeigen. [Weitere Informationen zu Metriken.](app-insights-web-monitor-performance.md)

#### <a name="no-data"></a>Sie sehen keine Daten?
* Verwenden Sie die Anwendung, und öffnen Sie verschiedene Seiten, damit einige Telemetriedaten generiert werden.
* Öffnen Sie die Kachel [Suche](app-insights-diagnostic-search.md) , um einzelne Ereignisse anzuzeigen. Manchmal dauert es eine Weile, bis Ereignisse über die Metrikpipeline übertragen werden.
* Warten Sie einige Sekunden, und klicken Sie auf **Aktualisieren**. Diagramme aktualisieren sich in regelmäßigen Abständen selbst, doch Sie können sie auch manuell aktualisieren, wenn Sie auf anzuzeigende Daten warten.
* Informationen hierzu finden Sie unter [Problembehandlung](app-insights-troubleshoot-faq.md).

## <a name="publish-your-app"></a>Veröffentlichen der App
Stellen Sie jetzt Ihre Anwendung auf Ihrem Server oder in Azure bereit, und beobachten Sie, wie die Daten gesammelt werden.

![Veröffentlichen Sie die App mit Visual Studio.](./media/app-insights-windows-services/15-publish.png)

Beim Betrieb im Debugmodus wird Telemetrie über die Pipeline geliefert, sodass Ihnen innerhalb von wenigen Sekunden Daten angezeigt werden. Wenn Sie die App in der Releasekonfiguration bereitstellen, sammeln sich die Daten langsamer an.

#### <a name="no-data-after-you-publish-to-your-server"></a>Keine Daten nach dem Veröffentlichen auf Ihrem Server?
Öffnen Sie in der Firewall des Servers Ports für den ausgehenden Datenverkehr. Die Liste mit den erforderlichen Adressen finden Sie auf [dieser Seite](https://docs.microsoft.com/azure/application-insights/app-insights-ip-addresses). 

#### <a name="trouble-on-your-build-server"></a>Probleme auf dem Buildserver?
Weitere Informationen finden Sie in [diesem Artikel zur Problembehandlung](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).

> [!NOTE]
> Wenn die Anwendung viele Telemetriedaten generiert (und Sie Version 2.0.0-beta3 oder höher des ASP.NET-SDK verwenden), reduziert das adaptive Stichprobenmodul automatisch die an das Portal gesendete Datenmenge, indem nur ein repräsentativer Bruchteil der Ereignisse gesendet wird. Ereignisse, die mit derselben Anforderung im Zusammenhang stehen, werden als Gruppe aus- oder abgewählt, sodass Sie zwischen verwandten Ereignissen navigieren können. 
> [Erfahren Sie mehr über das Erstellen von Stichproben](app-insights-sampling.md).
> 
> 

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Nächste Schritte
* [Fügen Sie weitere Telemetriefunktionen hinzu](app-insights-asp-net-more.md) , um eine Rundumansicht Ihrer Anwendung zu erhalten:


