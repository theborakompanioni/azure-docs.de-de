---
title: "Application Insights für Java-Web-Apps, die bereits live sind"
description: "Starten Sie die Überwachung einer Webanwendung, die bereits auf dem Server ausgeführt wird"
services: application-insights
documentationcenter: java
author: harelbr
manager: douge
ms.assetid: 12f3dbb9-915f-4087-87c9-807286030b0b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/10/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 42e682eb8e0a740393648e9fe49244c3a02a9867
ms.openlocfilehash: eb6bce9be34467e472fbae6cbf154f3b789b6ddc


---
# <a name="application-insights-for-java-web-apps-that-are-already-live"></a>Application Insights für Java-Web-Apps, die bereits live sind


Wenn Sie eine Webanwendung haben, die bereits auf dem J2EE-Server ausgeführt wird, können Sie mit der Überwachung mit [Application Insights](app-insights-overview.md) beginnen, ohne Codeänderungen vornehmen oder das Projekt neu kompilieren zu müssen. Mit dieser Option erhalten Sie Informationen zu an Ihre Server gesendeten HTTP-Anforderungen, nicht behandelten Ausnahmen und Leistungsindikatoren.

Sie benötigen ein [Microsoft Azure](https://azure.com)-Abonnement.

> [!NOTE]
> Mit dem Verfahren auf dieser Seite wird Ihrer Web-App zur Laufzeit das SDK hinzugefügt. Diese Laufzeitinstrumentierung ist hilfreich, wenn Sie den Quellcode nicht aktualisieren oder neu erstellen möchten. Sofern möglich, empfehlen wir jedoch, dass Sie stattdessen [das SDK zum Quellcode hinzufügen](app-insights-java-get-started.md). Dadurch erhalten Sie weitere Optionen, Sie können z. B. Code zum Nachverfolgen der Benutzeraktivität schreiben.
> 
> 

## <a name="1-get-an-application-insights-instrumentation-key"></a>1. Abrufen eines Application Insights-Instrumentationsschlüssels
1. Melden Sie sich beim [Microsoft Azure-Portal](https://portal.azure.com)
2. Erstellen Sie eine neue Application Insights-Ressource, und legen Sie den Anwendungstyp auf Java-Webanwendung fest.
   
    ![Geben Sie einen Namen ein, wählen Sie "Java-Web-App", und klicken Sie auf "Erstellen"](./media/app-insights-java-live/02-create.png)

    Die Ressource wird in wenigen Sekunden erstellt.

4. Öffnen Sie neue Ressource, und rufen Sie den Instrumentierungsschlüssel ab. Dieser Schlüssel muss in Kürze in das Codeprojekt eingefügt werden.
   
    ![Klicken Sie in der Übersicht über neue Ressourcen auf "Eigenschaften", und kopieren Sie den Instrumentationsschlüssel](./media/app-insights-java-live/03-key.png)

## <a name="2-download-the-sdk"></a>2. Herunterladen des SDK
1. Laden Sie das [Application Insights-SDK für Java](https://aka.ms/aijavasdk)herunter. 
2. Extrahieren Sie auf dem Server den SDK-Inhalt in das Verzeichnis, aus dem die Projektbinärdateien geladen werden. Bei Verwendung von Tomcat befindet sich dieses Verzeichnis in der Regel unter `webapps/<your_app_name>/WEB-INF/lib`.

Hinweis: Dies müssen Sie auf allen Server-Instanzen und für alle Apps wiederholen.

## <a name="3-add-an-application-insights-xml-file"></a>3. Hinzufügen der Datei "ApplicationInsights.XML"
Erstellen Sie "ApplicationInsights.xml" in dem Ordner, in dem Sie das SDK hinzugefügt haben. Fügen Sie den folgenden XML-Code ein.

Fügen Sie den Instrumentationsschlüssel ein, den Sie aus dem Azure-Portal abgerufen haben.

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">


      <!-- The key from the portal: -->

      <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>


      <!-- HTTP request component (not required for bare API) -->

      <TelemetryModules>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
      </TelemetryModules>

      <!-- Events correlation (not required for bare API) -->
      <!-- These initializers add context data to each event -->

      <TelemetryInitializers>
        <Add   type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>

      </TelemetryInitializers>
    </ApplicationInsights>
```

* Der Instrumentationsschlüssel wird zusammen mit jedem Telemetrieelement übermittelt und weist Application Insights an, ihn in Ihrer Ressource anzuzeigen.
* Die Komponente "HTTP-Anforderung" ist optional. Sie sendet automatisch Telemetriedaten zu Anforderungen und Antwortzeiten zum Portal.
* Die Korrelation von Ereignissen ist eine Ergänzung der HTTP-Anforderungskomponente. Sie weist den einzelnen Anforderungen, die vom Server empfangen wurden, einen Bezeichner zu und fügt ihn als Operation.Id-Eigenschaft jedem Telemetrieelement hinzu. Diese Eigenschaft ermöglicht das Korrelieren der jeder Anforderung zugeordneten Telemetriedaten, indem in [Diagnosesuche](app-insights-diagnostic-search.md)ein Filter festgelegt wird.

## <a name="4-add-an-http-filter"></a>4. Hinzufügen eines HTTP-Filters
Suchen und öffnen Sie die Datei "web.xml" in Ihrem Projekt, und führen Sie den folgenden Codeausschnitt unter dem Knoten "web-app" zusammen, in dem Ihre Anwendungsfilter konfiguriert sind.

Um möglichst genaue Ergebnisse zu erhalten, muss der Filter vor allen anderen Filtern zugeordnet werden.

```XML

    <filter>
      <filter-name>ApplicationInsightsWebFilter</filter-name>
      <filter-class>
        com.microsoft.applicationinsights.web.internal.WebRequestTrackingFilter
      </filter-class>
    </filter>
    <filter-mapping>
       <filter-name>ApplicationInsightsWebFilter</filter-name>
       <url-pattern>/*</url-pattern>
    </filter-mapping>
```

## <a name="5-check-firewall-exceptions"></a>5. Überprüfen der Firewallausnahmen
Gegebenenfalls müssen Sie [Ausnahmen für das Senden ausgehender Daten festlegen](app-insights-ip-addresses.md).

## <a name="6-restart-your-web-app"></a>6. Neustarten der Web-App
## <a name="7-view-your-telemetry-in-application-insights"></a>7. Anzeigen Ihrer Telemetriedaten in Application Insights
Kehren Sie zur Application Insights-Ressource im [Microsoft Azure-Portal](https://portal.azure.com)zurück.

Telemetriedaten zu HTTP-Anforderungen werden auf dem Übersichtsblatt angezeigt. (Wenn sie nicht vorhanden sind, warten Sie einige Sekunden, und klicken Sie dann auf "Aktualisieren".)

![Beispieldaten](./media/app-insights-java-live/5-results.png)

Klicken Sie sich durch ein beliebiges Diagramm, um ausführlichere Metriken anzuzeigen. 

![](./media/app-insights-java-live/6-barchart.png)

Beim Anzeigen der Eigenschaften einer Anforderung können Sie die damit verbundenen Telemetrieereignisse erkennen, wie z. B. Anforderungen und Ausnahmen.

![](./media/app-insights-java-live/7-instance.png)

[Weitere Informationen zu Metriken.](app-insights-metrics-explorer.md)

## <a name="next-steps"></a>Nächste Schritte
* [Fügen Sie Ihren Webseiten Telemetrie hinzu](app-insights-web-track-usage.md) , um Seitenaufrufe und Benutzermetriken zu überwachen.
* [Richten Sie Webtests ein](app-insights-monitor-web-app-availability.md) , um sicherzustellen, dass die Anwendung online und reaktionsfähig bleibt.
* [Erfassen von Protokollablaufverfolgungen](app-insights-java-trace-logs.md)
* [Durchsuchen Sie Ereignisse und Protokolle](app-insights-diagnostic-search.md) , um Probleme besser zu diagnostizieren.




<!--HONumber=Feb17_HO1-->


