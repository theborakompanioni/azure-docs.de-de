---
title: "Application Insights für Azure Cloud Services | Microsoft-Dokumentation"
description: "Effektives Überwachen Ihrer Web- und Workerrollen mit Application Insights"
services: application-insights
documentationcenter: 
author: alancameronwills
manager: carmonm
editor: alancameronwills
ms.assetid: 5c7a5b34-329e-42b7-9330-9dcbb9ff1f88
ms.service: application-insights
ms.devlang: na
ms.tgt_pltfrm: ibiza
ms.topic: article
ms.workload: tbd
ms.date: 03/14/2017
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: fd35f1774ffda3d3751a6fa4b6e17f2132274916
ms.openlocfilehash: 5ea258a8c790dd94e019243e77f2ff694c2515d7
ms.lasthandoff: 03/16/2017


---
# <a name="application-insights-for-azure-cloud-services"></a>Application Insights für Azure Cloud Services
[Microsoft Azure Cloud Services-Apps](https://azure.microsoft.com/services/cloud-services/) können mit [Application Insights][start] auf Verfügbarkeit, Leistung, Fehler und Verwendung überwacht werden. Mit dem Feedback zur Leistung und Effektivität der App in der Praxis können Sie in jedem Entwicklungslebenszyklus eine fundierte Entscheidung für die Richtung des Entwurfs treffen.

![Beispiel](./media/app-insights-cloudservices/sample.png)

## <a name="before-you-start"></a>Vorbereitung
Sie benötigen Folgendes:

* Ein Abonnement für [Microsoft Azure](http://azure.com). Melden Sie sich mit einem Microsoft-Konto an, das Sie möglicherweise für Windows, XBox Live oder andere Microsoft-Clouddienste verwenden. 
* Microsoft Azure-Tools 2.9 oder höher
* Developer Analytics Tools 7.10 oder höher

## <a name="quick-start"></a>Schnellstart
Am schnellsten und einfachsten können Sie Ihren Clouddienst mit Application Insights überwachen, wenn Sie diese Option beim Veröffentlichen des Diensts in Azure auswählen.

![Beispiel](./media/app-insights-cloudservices/azure-cloud-application-insights.png)

Mit dieser Option wird die App zur Laufzeit instrumentiert, sodass Sie alle erforderlichen Telemetriedaten zum Überwachen von Anforderungen, Ausnahmen und Abhängigkeiten in Ihrer Webrolle sowie die Leistungsindikatoren von Ihren Workerrollen erhalten. Auch alle Diagnoseablaufverfolgungen, die von der App generiert werden, werden an Application Insights gesendet.

Wenn Sie nicht mehr als das benötigen, sind Sie damit fertig! Die nächsten Schritte sind: [Anzeigen von Metriken aus der App](app-insights-metrics-explorer.md), [Abfragen von Daten mit Analytics](app-insights-analytics.md) und eventuell Einrichten eines [Dashboards](app-insights-dashboards.md). Möglicherweise möchten Sie [Verfügbarkeitstests](app-insights-monitor-web-app-availability.md) einrichten und [Ihren Webseiten Code hinzufügen](app-insights-javascript.md), um die Leistung im Browser zu überwachen.

Sie können jedoch auch weitere Optionen abrufen:

* Senden von Daten aus verschiedenen Komponenten und Erstellen von Konfigurationen zum Trennen von Ressourcen
* Hinzufügen von benutzerdefinierten Telemetriedaten aus der App

Lesen Sie weiter, wenn diese Optionen von Interesse für Sie sind.

## <a name="sample-application-instrumented-with-application-insights"></a>Mit Application Insights instrumentierte Beispielanwendung
Sehen Sie sich diese [Beispielanwendung](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService) an, in der Application Insights mit zwei in Azure gehosteten Workerrollen einem Clouddienst hinzugefügt wird. 

Im Folgenden wird erläutert, wie Sie Ihr eigenes Clouddienstprojekt auf die gleiche Weise anpassen können.

## <a name="plan-resources-and-resource-groups"></a>Planen von Ressourcen und Ressourcengruppen
Die Telemetriedaten aus Ihrer App werden in einer Azure-Ressource des Typs Application Insights gespeichert, analysiert und angezeigt. 

Jede Ressource gehört jeweils zu einer Ressourcengruppe. Ressourcengruppen werden zum Verwalten von Kosten, zum Gewähren des Zugriffs für Teammitglieder und zum Bereitstellen von Updates in einer koordinierten Transaktion verwendet. Beispielsweise können Sie [ein Skript zum Bereitstellen](../azure-resource-manager/resource-group-template-deploy.md) eines Azure-Clouddiensts und der zugehörigen Application Insights-Überwachungsressourcen in einem kombinierten Vorgang schreiben.

### <a name="resources-for-components"></a>Ressourcen für Komponenten
Als Schema wird empfohlen, für die einzelnen Komponenten der Anwendung, d.h. für jede Webrolle und Workerrolle, jeweils eine separate Ressource zu erstellen. Sie können jede Komponente einzeln analysieren, können jedoch auch ein [Dashboard](app-insights-dashboards.md) erstellen, das die wichtigsten Diagramme aus allen Komponenten vereint, sodass Sie diese vergleichen und zusammen überwachen können. 

Ein alternatives Schema besteht darin, die Telemetriedaten von mehreren Rollen an dieselbe Ressource zu senden, jedoch [jedem Telemetrieelement eine Dimensionseigenschaft hinzuzufügen](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer), mit der die jeweilige Quellrolle identifiziert wird. In diesem Schema wird in Metrikdiagrammen (z.B. Ausnahmen) normalerweise eine Aggregation der Zählungen aus den unterschiedlichen Rollen angezeigt. Sie können jedoch das Diagramm bei Bedarf nach Rollenbezeichnern segmentieren. Suchvorgänge können auch nach der gleichen Dimension gefiltert werden. Mit diesem alternativen Schema können alle Daten etwas einfacher gleichzeitig angezeigt werden, es kann jedoch zu einer gewissen Verwirrung im Hinblick auf die Rollen führen.

Browsertelemetriedaten sind in der Regel in der gleichen Ressource wie die zugehörige serverseitige Webrolle enthalten.

Legen Sie die Application Insights-Ressourcen für die verschiedenen Komponenten in einer Ressourcengruppe ab. So können sie einfach zusammen verwaltet werden. 

### <a name="separating-development-test-and-production"></a>Trennen von Entwicklung, Test und Produktion
Wenn Sie benutzerdefinierte Ereignisse für das nächste Feature entwickeln, während die vorherige Version aktiv ist, sollten die Entwicklungstelemetriedaten an eine separate Application Insights-Ressource gesendet werden. Andernfalls ist es schwierig, die Testtelemetriedaten im gesamten Datenverkehr von der Livewebsite zu finden.

Um dies zu vermeiden, erstellen Sie separate Ressourcen für jede Buildkonfiguration oder jeden „Stempel“ (Entwicklung, Test, Produktion usw.) des Systems. Legen Sie die Ressourcen für jede Buildkonfiguration jeweils in einer separaten Ressourcengruppe ab. 

Um die Telemetriedaten an die entsprechenden Ressourcen zu senden, können Sie das Application Insights SDK so einrichten, dass abhängig von der Buildkonfiguration jeweils ein anderer Instrumentierungsschlüssel abgerufen wird. 

## <a name="create-an-application-insights-resource-for-each-role"></a>Erstellen einer Application Insights-Ressource für die einzelnen Rollen
Wenn Sie eine separate Ressource für die einzelnen Rollen – und eventuell eine separate Gruppe für die einzelnen Buildkonfigurationen – erstellen möchten, ist es am einfachsten, diese im Application Insights-Portal zu erstellen. (Wenn Sie häufig Ressourcen erstellen, können Sie [den Vorgang automatisieren](app-insights-powershell.md).

1. Erstellen Sie im [Azure-Portal][portal] eine neue Application Insights-Ressource. Wählen Sie als Anwendungstyp "ASP.NET-App" aus. 

    ![Klicken Sie auf "Neu > Application Insights"](./media/app-insights-cloudservices/01-new.png)
2. Beachten Sie, dass jede Ressource durch einen Instrumentierungsschlüssel identifiziert wird. Möglicherweise benötigen Sie diesen später, wenn Sie die Konfiguration des SDK manuell konfigurieren oder überprüfen möchten.

    ![Klicken Sie auf "Eigenschaften", wählen Sie den Schlüssel aus, und drücken Sie STRG+C](./media/app-insights-cloudservices/02-props.png) 

## <a name="set-up-azure-diagnostics-for-each-role"></a>Einrichten von Azure-Diagnose für die einzelnen Rollen
Legen Sie diese Option zum Überwachen Ihrer App mit Application Insights fest. Für Webrollen umfasst dies die Leistungsüberwachung, Warnungen und Diagnose sowie die Verwendungsanalyse. Für andere Rollen können Sie Azure-Diagnose durchsuchen und überwachen, z.B. Neustarten, Leistungsindikatoren und Aufrufe von „System.Diagnostics.Trace“. 

1. Öffnen Sie im Visual Studio-Projektmappen-Explorer unter &lt;YourCloudService&gt; die Eigenschaften der einzelnen Rollen.
2. Aktivieren Sie unter **Konfiguration** das Kontrollkästchen **Diagnosedaten an Application Insights senden**, und wählen Sie die entsprechende Application Insights-Ressource aus, die Sie zuvor erstellt haben.

Wenn Sie sich entschieden haben, eine separate Application Insights-Ressource für jede Buildkonfiguration zu verwenden, wählen Sie zuerst die Konfiguration aus.

![Konfigurieren Sie Application Insights in den Eigenschaften jeder Azure-Rolle.](./media/app-insights-cloudservices/configure-azure-diagnostics.png)

Dadurch werden die Application Insights-Instrumentierungsschlüssel in die Dateien mit dem Namen `ServiceConfiguration.*.cscfg` eingefügt. ([Beispielcode](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/AzureEmailService/ServiceConfiguration.Cloud.cscfg)).

Wenn Sie festlegen möchten, welche Diagnosedaten an Application Insights gesendet werden, können Sie dies [direkt durch Bearbeiten der `.cscfg`-Dateien erledigen](app-insights-azure-diagnostics.md).

## <a name="sdk"></a>Installieren des SDK in den einzelnen Projekten
Hierdurch haben Sie die Möglichkeit, allen Rollen benutzerdefinierte Geschäftstelemetriedaten hinzuzufügen. Dies ermöglicht eine genauere Analyse, wie die Anwendung verwendet und ausgeführt wird.

Konfigurieren Sie in Visual Studio das Application Insights SDK für jedes Cloud-App-Projekt.

1. **Webrollen**: Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **Application Insights konfigurieren** oder **Hinzufügen > Application Insights-Telemetrie**.

2. **Workerrollen**: 
 * Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **NuGet-Pakete verwalten**.
 * Fügen Sie [Application Insights für Windows-Dienste](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) hinzu.

    ![Suchen Sie nach "Application Insights".](./media/app-insights-cloudservices/04-ai-nuget.png)

3. Konfigurieren Sie das SDK zum Senden von Daten an die Application Insights-Ressource.

    Legen Sie in einer geeigneten Startfunktion den Instrumentierungsschlüssel aus der Konfigurationseinstellung in der CSCFG-Datei fest:
 
    ```C#
   
     TelemetryConfiguration.Active.InstrumentationKey = RoleEnvironment.GetConfigurationSettingValue("APPINSIGHTS_INSTRUMENTATIONKEY");
    ```
   
    Tun Sie dies für jede Rolle in Ihrer Anwendung. Beispiele:
   
   * [Webrolle](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Global.asax.cs#L27)
   * [Workerrolle](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L232)
   * [Für Webseiten](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Views/Shared/_Layout.cshtml#L13) 
4. Legen Sie für die Datei "ApplicationInsights.config" fest, dass sie immer in das Ausgabeverzeichnis kopiert wird. 
   
    (In der Datei „.config“ erscheinen Nachrichten, in denen Sie dazu aufgefordert werden, den Instrumentationsschlüssel hier zu platzieren. Für Cloudanwendungen ist es jedoch besser, ihn aus der Datei „.cscfg“ festzulegen. Dadurch wird sichergestellt, dass die Rolle im Portal korrekt identifiziert wird.)

#### <a name="run-and-publish-the-app"></a>Ausführen und Veröffentlichen der App
Führen Sie Ihre App aus, und melden Sie sich bei Azure an. Öffnen Sie die von Ihnen erstellte Application Insights-Ressource. In der [Suche](app-insights-diagnostic-search.md) sehen Sie dann einzelne Datenpunkte und im [Metrik-Explorer](app-insights-metrics-explorer.md) aggregierte Daten. 

Fügen Sie mehr Telemetrie hinzu (siehe folgende Abschnitte), und veröffentlichen Sie dann Ihre App, um Livediagnosen und Nutzungsfeedback zu erhalten. 

#### <a name="no-data"></a>Sie sehen keine Daten?
* Öffnen Sie die Kachel [Suche][diagnostic], um einzelne Ereignisse anzuzeigen.
* Verwenden Sie die Anwendung, und öffnen Sie verschiedene Seiten, damit einige Telemetriedaten generiert werden.
* Warten Sie einige Sekunden, und klicken Sie auf "Aktualisieren".
* Informationen hierzu finden Sie unter [Problembehandlung][qna].

## <a name="view-azure-diagnostic-events"></a>Anzeigen von Azure-Diagnoseereignissen
Hier finden Sie die Diagnosedaten:

* Leistungsindikatoren werden als benutzerdefinierte Metriken angezeigt. 
* Windows-Ereignisprotokolle werden als Ablaufverfolgungen und benutzerdefinierte Ereignisse angezeigt.
* Anwendungsprotokolle, ETW-Protokolle und alle Diagnoseinfrastrukturprotokolle werden als Ablaufverfolgungen angezeigt.

Um Leistungsindikatoren und Angaben zur Anzahl von Ereignissen anzuzeigen, öffnen Sie den [Metrik-Explorer](app-insights-metrics-explorer.md) und fügen ein neues Diagramm hinzu:

![Azure-Diagnosedaten](./media/app-insights-cloudservices/23-wad.png)

Verwenden Sie die [Suche](app-insights-diagnostic-search.md) oder eine [Analytics-Abfrage](app-insights-analytics-tour.md), um die verschiedenen Ablaufverfolgungsprotokolle zu durchsuchen, die von der Azure-Diagnose gesendet werden. Angenommen, es liegt eine nicht behandelte Ausnahme vor, die das Abstürzen und Wiederverwenden einer Rolle verursacht hat. Diese Informationen werden im Kanal „Anwendung“ des Windows-Ereignisprotokolls angezeigt. Sie können das Windows-Ereignisprotokoll mithilfe der Suchfunktion untersuchen und die vollständige Stapelüberwachung für die Ausnahme abrufen. Dadurch können Sie die Ursache des Problems ermitteln.

![Durchsuchen der Azure-Diagnosedaten](./media/app-insights-cloudservices/25-wad.png)

## <a name="more-telemetry"></a>Mehr Telemetrie
In den folgenden Abschnitten wird gezeigt, wie Sie weitere Telemetriedaten zu verschiedenen Aspekten Ihrer Anwendung gewinnen.

## <a name="track-requests-from-worker-roles"></a>Nachverfolgen von Anforderungen bei Workerrollen
In Webrollen sammelt das Modul für Anforderungen automatisch Daten zu HTTP-Anforderungen. Beispiele zum Überschreiben des Standardsammelverhaltens finden Sie im [MVCWebRole-Beispiel](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole) . 

Sie können die Leistung der Aufrufe von Workerrollen erfassen, indem Sie diese auf die gleiche Weise wie HTTP-Anforderungen nachverfolgen. In Application Insights misst der Telemetrietyp „Anforderungen“ eine Arbeitseinheit auf dem benannten Server, die zeitlich bestimmt werden und unabhängig erfolgreich sein oder einen Fehler verursachen kann. Während HTTP-Anforderungen durch das SDK automatisch erfasst werden, können Sie Ihren eigenen Code zum Nachverfolgen von Anforderungen an Workerrollen einfügen.

Siehe dazu die zwei Beispielworkerrollen, die zum Melden von Anforderungen instrumentiert sind: [WorkerRoleA](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/WorkerRoleA) und [WorkerRoleB](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/WorkerRoleB)

## <a name="exceptions"></a>Ausnahmen
Informationen zum Sammeln von Ausnahmefehlern von verschiedenen Webanwendungstypen finden Sie unter [Monitoring Exceptions in Application Insights](app-insights-asp-net-exceptions.md) (in englischer Sprache).

Die Beispielwebrolle verfügt über MVC5- und Web-API 2-Controller. Die nicht behandelten Ausnahmen von beiden werden mit den folgenden Handlern erfasst:

* [AiHandleErrorAttribute](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Telemetry/AiHandleErrorAttribute.cs) wird [hier](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/App_Start/FilterConfig.cs#L12) für MVC5-Controller eingerichtet
* [AiWebApiExceptionLogger](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Telemetry/AiWebApiExceptionLogger.cs) wird [hier](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/App_Start/WebApiConfig.cs#L25) für Web-API 2-Controller eingerichtet

Für Workerrollen gibt es zwei Möglichkeiten zum Nachverfolgen von Ausnahmen:

* TrackException(ex)
* Wenn Sie das NuGet-Paket für Application Insights-Ablaufverfolgungslistener hinzugefügt haben, können Sie Ausnahmen mithilfe von **System.Diagnostics.Trace** protokollieren. [Codebeispiel](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L107)

## <a name="performance-counters"></a>Leistungsindikatoren
Die folgenden Leistungsindikatoren werden standardmäßig erfasst:

    * \Process(??APP_WIN32_PROC??)\% Prozessorzeit
    * \Memory\Verfügbare Bytes
    * \.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec
    * \Process(??APP_WIN32_PROC??)\Private Bytes
    * \Process(??APP_WIN32_PROC??)\IO Data Bytes/sec
    * \Processor(_Total)\%Prozessorzeit

Für Webrollen werden auch diese Leistungsindikatoren erfasst:

    * \ASP.NET Applications(??APP_W3SVC_PROC??)\Requests/Sec
    * \ASP.NET Applications(??APP_W3SVC_PROC??)\Request Execution Time
    * \ASP.NET Applications(??APP_W3SVC_PROC??)\Requests In Application Queue

Sie können zusätzliche benutzerdefinierte oder andere Windows-Leistungsindikatoren angeben, indem Sie die Datei „ApplicationInsights.config“ bearbeiten. [Siehe dieses Beispiel](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/ApplicationInsights.config#L14).

  ![Leistungsindikatoren](./media/app-insights-cloudservices/OLfMo2f.png)

## <a name="correlated-telemetry-for-worker-roles"></a>Korrelierte Telemetriedaten für Workerrollen
Durch die umfassende Diagnose können Sie die Ursachen für Anforderungen mit Fehlern oder hoher Latenz sehen. Bei Webrollen richtet das SDK automatisch Korrelationen zwischen verknüpften Telemetriedaten ein. Für Workerrollen können Sie zu diesem Zweck mithilfe eines benutzerdefinierten Telemetrieinitialisierers ein allgemeines Operation.Id-Kontextattribut für alle Telemetriedaten festlegen. Dadurch können Sie auf einen Blick sehen, ob die Latenz oder der Fehler durch eine Abhängigkeit oder durch den Code verursacht wurde. 

Das geht so:

* Fügen Sie die Korrelations-ID wie [hier](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L36)gezeigt in einen CallContext ein. In diesem Fall wird die Anforderungs-ID als Korrelations-ID verwendet.
* Fügen Sie eine benutzerdefinierte „TelemetryInitializer“-Implementierung hinzu, die das „Operation.Id“-Attribut auf die oben festgelegte „correlationId“ festgelegt. Hier ein Beispiel: [ItemCorrelationTelemetryInitializer](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/Telemetry/ItemCorrelationTelemetryInitializer.cs#L13)
* Fügen Sie den benutzerdefinierten Telemetrieinitialisierer hinzu. Dies kann in der Datei "ApplicationInsights.config" oder im Code (wie [hier](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L233)

Das ist alles! Die Portalfunktionalität ist bereits angeschlossen, damit Sie alle zugeordneten Telemetriedaten auf einen Blick sehen können:

![Korrelierte Telemetriedaten](./media/app-insights-cloudservices/bHxuUhd.png)

## <a name="client-telemetry"></a>Clienttelemetrie
[Fügen Sie Ihren Webseiten das JavaScript SDK][client] hinzu, um browserbasierte Telemetriedaten wie Anzahl der Seitenaufrufe, Seitenladezeiten oder Skriptausnahmen zu erhalten und benutzerdefinierte Telemetrie in Ihre Seitenskripts schreiben zu können.

## <a name="availability-tests"></a>Verfügbarkeitstests
[Richten Sie Webtests ein][availability], um sicherzustellen, dass die Anwendung online und reaktionsfähig bleibt.

## <a name="display-everything-together"></a>Anzeigen aller Daten
Um eine allgemeine Übersicht über Ihr System zu erhalten, können Sie die wichtigsten Überwachungsdiagramme in einem [Dashboard](app-insights-dashboards.md) zusammenführen. Beispielsweise können Sie die Anforderungs- und Fehleranzahl der einzelnen Rollen anheften. 

Wenn in Ihrem System andere Azure-Dienste, z.B. Stream Analytics, verwendet werden, können Sie die entsprechenden Überwachungsdiagramme auch einfügen. 

Fügen Sie bei einer mobilen Client-App einen Teil des Codes zum Senden von benutzerdefinierten Ereignissen für wichtige Benutzervorgänge ein, und erstellen Sie eine [HockeyApp-Brücke](app-insights-hockeyapp-bridge-app.md). Erstellen Sie Abfragen in [Analytics](app-insights-analytics.md), um die Ereigniszähler anzuzeigen, und heften Sie sie an das Dashboard an.

## <a name="example"></a>Beispiel
[In diesem Beispiel](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService) wird ein Dienst mit einer Webrolle und zwei Workerrollen überwacht.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Ausnahme „Methode nicht gefunden“ bei der Ausführung in Azure Cloud Services
Haben Sie für .NET 4.6 erstellt? 4.6 wird nicht automatisch in Azure Cloud Services-Rollen unterstützt. [Installieren Sie 4.6 für jede Rolle](../cloud-services/cloud-services-dotnet-install-dotnet.md) , bevor Sie Ihre App ausführen.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Nächste Schritte
* [Konfigurieren der Azure-Diagnose zum Protokollieren in Application Insights](app-insights-azure-diagnostics.md)
* [Automatisierung der Erstellung von Application Insights-Ressourcen](app-insights-powershell.md)
* [Automatisierung von Azure-Diagnose](app-insights-powershell-azure-diagnostics.md)

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[azure]: app-insights-azure.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[netlogs]: app-insights-asp-net-trace-logs.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md 

