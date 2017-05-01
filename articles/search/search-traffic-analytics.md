---
title: "„Datenverkehrsanalyse durchsuchen“ für Azure Search | Microsoft Docs"
description: "Aktivieren Sie „Datenverkehrsanalyse durchsuchen“ für Azure Search, einen cloudgehosteten Suchdienst für Microsoft Azure, um Erkenntnisse über Ihre Benutzer und Daten zu gewinnen."
services: search
documentationcenter: 
author: bernitorres
manager: jlembicz
editor: 
ms.assetid: b31d79cf-5924-4522-9276-a1bb5d527b13
ms.service: search
ms.devlang: multiple
ms.workload: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/05/2017
ms.author: betorres
translationtype: Human Translation
ms.sourcegitcommit: 0b53a5ab59779dc16825887b3c970927f1f30821
ms.openlocfilehash: 303ca5c820f573dc0b58f1910f258403c3baad2a
ms.lasthandoff: 04/07/2017

---

# <a name="what-is-search-traffic-analytics"></a>Was ist „Datenverkehrsanalyse durchsuchen“?
„Datenverkehrsanalyse durchsuchen“ ist ein Muster für das Implementieren einer Feedbackschleife für Ihren Suchdienst. Dieses Muster beschreibt die erforderlichen Daten, und wie Sie sie mit Application Insights sammeln, einem branchenführenden Tool für die Überwachung von Diensten auf mehrere Plattformen.

Mit „Datenverkehrsanalyse durchsuchen“ können Sie Einblicke in Ihren Suchdienst und Erkenntnisse über Ihre Benutzer und deren Verhalten gewinnen. Wenn Sie über Daten verfügen, die Ihnen mitteilen, was Ihre Benutzer wählen, können Sie Entscheidungen treffen, die Ihre Sucherfahrung weiter verbessern, und sich zurückziehen, wenn die Ergebnisse nicht den Erwartungen entsprechen.

Azure Search bietet eine Telemetrielösung, die Azure Application Insights und Power BI integriert, um profunde Überwachung und Nachverfolgung zu ermöglichen. Da die Interaktion mit Azure Search nur über APIs erfolgt, muss die Telemetrie gemäß der Anweisungen auf dieser Seite durch die Entwickler implementiert werden, die die Suche verwenden.

## <a name="identify-the-relevant-search-data"></a>Identifizieren der relevanten Suchdaten

Um nützliche Suchmetriken zu erhalten, müssen einige Signale von den Benutzern der Suchanwendung protokolliert werden. Diese Signale kennzeichnen Inhalte, die Benutzer interessieren, und die sie als relevant für ihre Bedürfnisse einschätzen.

„Datenverkehrsanalyse durchsuchen“ benötigt zwei Signale:

1. Vom Benutzer generierte Ereignisse: Nur von einem Benutzer initiierte Suchabfragen sind interessant. Suchabfragen, die dem Auffüllen von Facets, zusätzlichem Inhalt oder internen Informationen dienen, sind nicht wichtig und verzerren und beeinflussen Ihre Ergebnisse.

2. Vom Benutzer generierte Klickereignisse: Anhand der Klicks in diesem Dokument nehmen wir Bezug auf einen Benutzer, der ein bestimmtes Suchergebnis einer Suchabfrage auswählt. Ein Klick bedeutet im Allgemeinen, dass ein Dokument ein relevantes Ergebnis für eine bestimmte Suchabfrage ist.

Durch das Verknüpfen von Suche und Klickereignissen mit einer Korrelations-ID können Sie das Verhalten von Benutzern in Ihrer Anwendung analysieren. Diese Einblicke in die Suche können Sie nicht erhalten, wenn Sie ausschließlich Suchdatenverkehrsprotokolle verwenden.

## <a name="how-to-implement-search-traffic-analytics"></a>Implementieren von „Datenverkehrsanalyse durchsuchen“

Die im vorherigen Abschnitt erwähnten Signale müssen von der Suchanwendung erfasst werden, während der Benutzer mit ihr interagiert. Application Insights ist eine für mehrere Plattformen verfügbare erweiterbare Überwachungslösung mit flexiblen Instrumentierungsoptionen. Mit der Verwendung von Application Insights nutzen Sie die von Azure Search erstellten Power BI-Suchberichte zur Vereinfachung der Datenanalyse.

Auf der Seite des [Portals](https://portal.azure.com) für Ihren Dienst von Azure Search enthält das Blatt „Datenverkehrsanalyse durchsuchen“ einen Spickzettel, anhand dessen Sie diesem Telemetriemuster folgen können. Sie können auch eine Application Insights-Ressource auswählen oder erstellen und alle erforderlichen Daten an einem Ort anzeigen.

![Anweisungen zu „Datenverkehrsanalyse durchsuchen“][1]

### <a name="1-select-an-application-insights-resource"></a>1. Auswählen einer Application Insights-Ressource

Sie müssen eine zu verwendende Application Insights-Ressource auswählen oder sie erstellen, wenn Sie noch keine besitzen. Sie können eine Ressource verwenden, die bereits verwendet wird, um die erforderlichen benutzerdefinierten Ereignisse zu protokollieren.

Wenn Sie eine neue Application Insights-Ressource erstellen, sind alle Anwendungstypen für dieses Szenario gültig. Wählen Sie dasjenige, das am besten für die Plattform geeignet ist, die Sie verwenden.

Sie benötigen den Instrumentierungsschlüssel, um den Telemetrieclient für Ihre Anwendung zu erstellen. Sie erhalten ihn über das Dashboard des Application Insights-Portals, oder rufen Sie ihn auf der Seite „Datenverkehrsanalyse durchsuchen“ ab, wozu Sie die Instanz auswählen, die Sie verwenden möchten.

### <a name="2-instrument-your-application"></a>2. Instrumentieren Ihrer Anwendung

In dieser Phase instrumentieren Sie Ihre eigene Suchanwendung mit der Application Insights-Ressource, die Sie im vorherigen Schritt erstellt haben. Dieser Prozess umfasst vier Schritte:

**I. Erstellen eines Telemetrieclients** Dies ist das Objekt, das Ereignisse an die Application Insights-Ressource sendet.

*C#*

    private TelemetryClient telemetryClient = new TelemetryClient();
    telemetryClient.InstrumentationKey = "<YOUR INSTRUMENTATION KEY>";

*JavaScript*

    <script type="text/javascript">var appInsights=window.appInsights||function(config){function r(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},u=document,e=window,o="script",s=u.createElement(o),i,f;s.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js";u.getElementsByTagName(o)[0].parentNode.appendChild(s);try{t.cookie=u.cookie}catch(h){}for(t.queue=[],i=["Event","Exception","Metric","PageView","Trace","Dependency"];i.length;)r("track"+i.pop());return r("setAuthenticatedUserContext"),r("clearAuthenticatedUserContext"),config.disableExceptionTracking||(i="onerror",r("_"+i),f=e[i],e[i]=function(config,r,u,e,o){var s=f&&f(config,r,u,e,o);return s!==!0&&t["_"+i](config,r,u,e,o),s}),t}
    ({
    instrumentationKey: "<YOUR INSTRUMENTATION KEY>"
    });
    window.appInsights=appInsights;
    </script>

Entnehmen Sie andere Sprachen und Plattformen bitte der vollständigen [Liste](https://docs.microsoft.com/azure/application-insights/app-insights-platforms).

**II. Anfordern einer Such-ID für die Korrelation** Um Suchanforderungen mit Klicks zu korrelieren, ist eine Korrelations-ID erforderlich, die diese zwei unterschiedlichen Ereignisse miteinander in Beziehung setzt. Azure Search bietet Ihnen eine Such-ID, wenn Sie sie mit einem Header anfordern:

*C#*

    // This sample uses the Azure Search .NET SDK https://www.nuget.org/packages/Microsoft.Azure.Search

    var client = new SearchIndexClient(<ServiceName>, <IndexName>, new SearchCredentials(<QueryKey>)
    var headers = new Dictionary<string, List<string>>() { { "x-ms-azs-return-searchid", new List<string>() { "true" } } };
    var response = await client.Documents.SearchWithHttpMessagesAsync(searchText: searchText, searchParameters: parameters, customHeaders: headers);
    IEnumerable<string> headerValues;
    string searchId = string.Empty;
    if (response.Response.Headers.TryGetValues("x-ms-azs-searchid", out headerValues)){
     searchId = headerValues.FirstOrDefault();
    }

*JavaScript*

    request.setRequestHeader("x-ms-azs-return-searchid", "true");
    request.setRequestHeader("Access-Control-Expose-Headers", "x-ms-azs-searchid");
    var searchId = request.getResponseHeader('x-ms-azs-searchid');

**III. Protokollsuchereignisse**

Bei jeder Suchanforderung eines Benutzers sollten Sie dies mit folgendem Schema als Suchereignis in einem benutzerdefinierten Application Insights-Ereignis protokollieren:

**ServiceName**: (String) Suchdienstname **SearchId**: (GUID) eindeutiger Bezeichner der Suchabfrage (enthalten in der Suchantwort) **IndexName**: (String) abzufragender Suchdienstindex **QueryTerms**: (String) vom Benutzer eingegebene Suchbegriffe **ResultCount**: (Int) Anzahl der zurückgegebenen Dokumente (enthalten in der Suchantwort) **ScoringProfile**: (String) Ggf. Name des verwendeten Bewertungsprofils

> [!NOTE]
> Fordern Sie die Anzahl vom Benutzer generierter Abfragen an, indem Sie „$count = true“ Ihrer Suchabfrage hinzufügen. Weitere Informationen finden Sie [hier](https://docs.microsoft.com/rest/api/searchservice/search-documents#request).
>

> [!NOTE]
> Denken Sie daran, nur Suchabfragen zu protokollieren, die von Benutzern generiert werden.
>

*C#*

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search Id>},
    {"IndexName", <index name>},
    {"QueryTerms", <search terms>},
    {"ResultCount", <results count>},
    {"ScoringProfile", <scoring profile used>}
    };
    telemetryClient.TrackEvent("Search", properties);

*JavaScript*

    appInsights.trackEvent("Search", {
    SearchServiceName: <service name>,
    SearchId: <search id>,
    IndexName: <index name>,
    QueryTerms: <search terms>,
    ResultCount: <results count>,
    ScoringProfile: <scoring profile used>
    });

**IV. Protokollieren von Klickereignissen**

Jedes Mal, wenn ein Benutzer auf ein Dokument klickt, ist dies ein Signal, dass für Suchanalysezwecke protokolliert werden muss. Verwenden Sie benutzerdefinierte Application Insights-Ereignisse, um diese Ereignisse mit dem folgenden Schema zu protokollieren:

**ServiceName**: (String) Suchdienstname **SearchId**: (Guid) eindeutiger Bezeichner der zugehörigen Suchabfrage **DocId**: (String) Dokumentbezeichner **Position**: (Int) Rang des Dokuments auf der Seite der Suchergebnisse

> [!NOTE]
> Die Position bezieht sich auf die Hauptreihenfolge in der Anwendung. Sie können diese Zahl frei festlegen, solange sie immer identisch ist, um den Vergleich zu ermöglichen.
>

*C#*

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search id>},
    {"ClickedDocId", <clicked document id>},
    {"Rank", <clicked document position>}
    };
    telemetryClient.TrackEvent("Click", properties);

*JavaScript*

    appInsights.TrackEvent("Click", {
        SearchServiceName: <service name>,
        SearchId: <search id>,
        ClickedDocId: <clicked document id>,
        Rank: <clicked document position>
    });

### <a name="3-analyze-with-power-bi-desktop"></a>3. Analysieren mit Power BI Desktop

Nachdem Sie Ihre App instrumentiert und sichergestellt haben, dass die Anwendung ordnungsgemäß mit Application Insights verbunden ist, können Sie eine vordefinierte Vorlage verwenden, die von Azure Search für Power BI Desktop erstellt wird.
Diese Vorlage enthält Diagramme und Tabellen, mit denen Sie fundiertere Entscheidungen treffen können, um Leistung und Relevanz Ihre Suche zu verbessern.

Um die Power BI Desktop-Vorlage zu instanziieren, benötigen Sie drei Arten von Informationen über Application Insights. Diese Daten finden Sie bei der Auswahl der zu verwendenden Ressource auf der Seite „Datenverkehrsanalyse durchsuchen“.

![Application Insights-Daten auf dem Blatt „Datenverkehrsanalyse durchsuchen“][2]

In der Power BI Desktop-Vorlage enthaltene Metriken:

*    Click Through Rate (CTR): Das Verhältnis zwischen den Benutzern, die auf ein bestimmtes Dokument klicken, und der Gesamtzahl von Suchvorgängen.
*    Suchvorgänge ohne Klicks: Begriffe für Spitzenabfragen, die keine Klicks registrieren.
*    Meistgeklickte Dokumente: Die in den letzten 24 Stunden, 7 Tagen und 30 Tagen meistgeklickten Dokumente nach ID.
*    Beliebte Begriff-Dokument-Paare: Begriffe, die dazu führen, dass auf das gleiche Dokument geklickt wird, geordnet nach Klicks.
*    Zeit bis zum Klicken: Klicks, die nach der seit der Suchabfrage verstrichenen Zeit im Bucket gespeichert werden.

![Power BI-Vorlage für das Lesen aus Application Insights][3]


## <a name="next-steps"></a>Nächste Schritte
Instrumentieren Sie Ihre Suchanwendung, um aussagekräftige Daten über Ihren Suchdienst zu erhalten.

Weitere Informationen zu Application Insights finden Sie [hier](https://go.microsoft.com/fwlink/?linkid=842905). Besuchen Sie die [Seite mit der Preisübersicht](https://azure.microsoft.com/pricing/details/application-insights/) von Application Insights, um mehr über die verschiedenen Dienstebenen zu erfahren.

Erfahren Sie hier mehr über das Erstellen erstaunlicher Berichte. Weitere Informationen finden Sie unter [Erste Schritte mit Power BI Desktop](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-getting-started/).

<!--Image references-->
[1]: ./media/search-traffic-analytics/AzureSearch-TrafficAnalytics.png
[2]: ./media/search-traffic-analytics/AzureSearch-AppInsightsData.png
[3]: ./media/search-traffic-analytics/AzureSearch-PBITemplate.png

