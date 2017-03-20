---
title: Was ist Azure Search? | Microsoft-Dokumentation
description: "Ein Suchmodul mit erweiterten Suchfunktionen für Ihre Suchleiste auf benutzerdefinierten Websites, in Apps sowie in Unternehmensdaten und Dateispeichern."
services: search
manager: jhubbard
author: HeidiSteen
documentationcenter: 
ms.assetid: 
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 02/15/2017
ms.author: heidist
experiment_id: heidist-20170221
translationtype: Human Translation
ms.sourcegitcommit: 72b2d9142479f9ba0380c5bd2dd82734e370dee7
ms.openlocfilehash: 8f4947ba2641bc8a39a71945ff2a6818d1ff7830
ms.lasthandoff: 03/08/2017

---
# <a name="what-is-azure-search"></a>Was ist Azure Search?

Azure Search bietet ein dediziertes und programmierbares Suchmodul mit intelligentem Suchverhalten für eine leistungsstarke Suchleiste für den Inhalt auf Ihrer Website, in Ihren Apps sowie in Unternehmensdaten oder Dateispeichern. 

![Suchleiste unterstützt von Azure Search](./media/search-what-is-azure-search/search-powered-by-azsearch3.png)

Eine zuverlässige Suchfunktion ist für die meisten Web-Apps und mobilen Apps unerlässlich. Eine Volltextsuche mit automatischer Vervollständigung der Suchbegriffe, Rechtschreibkorrektur und Abgleich semantisch identischer, aber unterschiedlich eingegebener Begriffe (Englisch: „car“ und „auto“) wird trotz der Komplexität, die hinter einer solchen Funktion steht, heute praktisch vorausgesetzt. Ebenso wichtig sind operative Anforderungen im Hinblick auf Skalierung, Zuverlässigkeit und Synchronisierung zwischen dem Suchmodul und den Back-End-Datenspeichern. 

Azure Search bietet eine umfangreiche Funktionalität, mit deren Hilfe Sie sowohl die Anforderungen an die Suchfunktion als auch die operativen Anforderungen erfüllen können.

![Suchleiste und benutzerdefinierte Suchseite mit typischen Suchfeatures](./media/search-what-is-azure-search/search-page-callouts3.png)

## <a name="how-it-works"></a>So funktioniert's

Um Azure Search zu verwenden, stellen Sie einen kostenlosen oder zahlungspflichtigen Dienst in Ihren Azure-Abonnements bereit, erstellen und laden einen Index mit dem durchsuchbaren Inhalt und rufen dann APIs auf, um Suchanforderungen auszugeben und Ergebnisse zu verarbeiten. Wenn Sie dedizierte Ressourcen mit der Möglichkeit zur Skalierung verwenden möchten, wird ein kostenpflichtiger Dienst benötigt.

|Tarif |Beschreibung |
|-----|------------|
|[Free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) | Ein freigegebener Dienst, der schnell bereitgestellt werden kann und für kleine Workloads eingesetzt wird, typischerweise in Schul- und Evaluierungsszenarien. |
|[Kostenpflichtig](search-sku-tier.md) | Ein dedizierter Dienst mit gestaffelter Kapazität, die von Basic bis zu Standard mit hoher Dichte reicht. Die verschiedenen Tarife unterstützen eine Vielzahl von Bereitstellungskonfigurationen.|

Azure Search wird in der Cloud als verwalteter Dienst von Microsoft ausgeführt. Azure Search kann auf jeder beliebigen Anwendungsplattform in benutzerdefinierten Code integriert werden. Der vollständig verwaltete Dienst und der private Inhalt sind global verfügbar, programmierbar, skalierbar und wiederherstellbar. 

Dedizierte Dienste werden rund um die Uhr an 7 Tagen pro Woche ausgeführt, mit der Möglichkeit zur Skalierung und unterstützt durch [eine Servicelevelvereinbarung von 99,9 %](https://azure.microsoft.com/support/legal/sla/search/v1_0/) von Microsoft Azure.

## <a name="how-it-compares"></a>Im Vergleich

Verschiedene Clouddienste stellen benutzerdefinierte Suchmodule mit Features für eine Suchleiste in benutzerdefinierten Apps bereit. Einige bieten vergleichbare Basisfunktionen, mit Volltextsuche, geografischer Suche und der Möglichkeit zur Verarbeitung einer gewissen Ungenauigkeit bei der Sucheingabe. Das passende Suchmodul ergibt sich in der Regel durch ein [spezialisiertes Feature](#feature-drilldown) oder durch die Benutzerfreundlichkeit und Einfachheit von APIs, Tools und Verwaltungsfunktionen.

Im Vergleich zu anderen Suchlösungen eignet Azure Search sich am besten für die Volltextsuche in Inhaltsspeichern und Datenbanken in Azure. Der Dienst bietet zudem eine zusätzliche Funktionsebene für Apps, bei denen Informationsabruf und Inhaltsnavigation primär auf der Suche basieren. Wichtige Vorteile umfassen:

+ Azure-Datenintegration (Crawler) auf Indexebene
+ Azure-Portal für eine zentrale Verwaltung
+ Skalierung, Zuverlässigkeit und erstklassige Verfügbarkeit von Azure
+ Linguistische und benutzerdefinierte Analyse, mit Analysemodulen für eine robuste Volltextsuche in 56 Sprachen
+ Kernfunktionen für auf die Suche ausgerichtete Apps, darunter beispielsweise Relevanz, Facettennavigation, Vorschläge, Synonyme, geografische Suche und mehr (siehe Liste unten).

> [!Note]
> Nicht-Azure-Daten werden vollständig unterstützt. Sie können eine beliebige JSON-Dokumentsammlung an einen Azure Search-Index weiterreichen.

Onlinekataloge, Branchenprogramme und Anwendungen zur Dokumentermittlung sind einige Beispiele für Anwendungen, die optimal vom umfassenden Funktionsumfang von Azure Search profitieren können.

### <a name="feature-drilldown"></a>Detaillierte Funktionsübersicht

#### <a name="full-text-search-and-text-analysis"></a>Volltextsuche und Textanalyse

Sie können Abfragen formulieren, indem Sie die [einfache Abfragesyntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) verwenden, die logische Operatoren, Operatoren für die Suche von Ausdrücken, Suffixoperatoren und Rangfolgenoperatoren bereitstellt. Die [Lucene-Abfragesyntax](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) ermöglicht zudem die Verwendung von Fuzzy- und NEAR-Suchen, Term Boosts und regulären Ausdrücken. Azure Search unterstützt darüber hinaus [benutzerdefinierte lexikalische Analysen](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search), wodurch Ihre Anwendung komplexe Suchabfragen mit phonetischem Abgleich und regulären Ausdrücken verarbeiten kann.

#### <a name="language-support"></a>Sprachunterstützung

Azure Search unterstützt lexikalische Analysen für [56 verschiedene Sprachen](https://docs.microsoft.com/rest/api/searchservice/language-support). Durch den kombinierten Einsatz von Lucene- und Microsoft-Analysen (mit jahrelanger Erfahrung bei der Sprachdatenverarbeitung in Office und Bing) kann Text mit Azure Search im Suchfeld Ihrer Anwendung analysiert werden. Sprachspezifische linguistische Probleme können intelligent gelöst werden, z.B. Zeiten von Verben, Geschlecht, unregelmäßiger Plural von Substantiven (Englisch: mouse/mice), Auflösen von zusammengesetzten Wörtern, Worttrennung (für Sprachen ohne Leerstellen) und vieles mehr.

#### <a name="data-integration"></a>Datenintegration

Sie können JSON-Datenstrukturen per Push in einen Azure Search-Index übertragen. Bei unterstützten Datenquellen können Sie außerdem [Indexer](search-indexer-overview.md) für einen automatischen Crawl in Azure SQL-Datenbank, Azure DocumentDB oder [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) einsetzen, um den Inhalt Ihres Suchindex mit Ihrem primären Datenspeicher zu synchronisieren.

Zum [Indizieren gängiger Dateiformate](search-howto-indexing-azure-blob-storage.md), wie z.B. Microsoft Office, PDF und HTML, steht eine **Funktion zum Knacken von Dokumenten** zur Verfügung.

#### <a name="search-experience"></a>Suchoberfläche

+ Für Suchleisten mit AutoVervollständigen und für Abfragen mit Textvervollständigung können **Suchvorschläge** aktiviert werden. Bei unvollständigen Sucheingaben von Benutzern werden [Vorschläge für Dokumente aus Ihrem Index](https://docs.microsoft.com/rest/api/searchservice/suggesters) angezeigt.

+ Die **Facettennavigation** wird über [einen einzigen Abfrageparameter](https://docs.microsoft.com/azure/search/search-faceted-navigation) aktiviert. Azure Search gibt eine Struktur für die Facettennavigation zurück, die Sie als Code hinter einer Kategorieliste für eine selbstgesteuerte Filterung nutzen können (z.B., um Katalogartikel nach Preisbereich oder Marke zu filtern).

+ **Filter** ermöglichen die Integration einer Facettennavigation in die Benutzeroberfläche Ihrer Anwendung, die Erweiterung von Abfrageformulierungen sowie die Filterung auf der Grundlage von Kriterien von Benutzern oder Entwicklern. Erstellen Sie Filter mithilfe der [OData-Syntax](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search).

+ Mit der **Treffermarkierung** [wird eine Formatierung auf ein übereinstimmendes Schlüsselwort in den Suchergebnissen angewendet](https://docs.microsoft.com/rest/api/searchservice/Search-Documents). Sie können auswählen, für welche Felder hervorgehobene Ausschnitte zurückgegeben werden sollen.

+ Ein zentraler Vorteil von Azure Search ist die **einfache Bewertung**. Anhand von [Bewertungsprofilen](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) kann die Relevanz als Funktion der Werte in den Dokumenten selbst modelliert werden. Beispielsweise kann es wünschenswert sein, dass neuere Produkte oder herabgesetzte Produkte in den Suchergebnissen weiter oben angezeigt werden. Sie können Bewertungsprofile auch mithilfe von Tags für eine personalisierte Bewertung erstellen, die auf den Sucheinstellungen der Kunden basiert, die Sie nachverfolgt und separat gespeichert haben.

+ Eine **Sortierung** wird über das Indexschema für mehrere Felder bereitgestellt und dann zur Abfragezeit mit einem einzelnen Suchparameter umgeschaltet.

+ Dank der **fein abgestimmten Steuerung**, die Azure Search für Ihre Suchergebnisse bietet, sind [Paginierung](search-pagination-page-layout.md) und Einschränkung Ihrer Suchergebnisse kein Problem.  

#### <a name="geosearch"></a>Geografische Suche

Azure Search bietet eine intelligente Verarbeitung, Filterung und Anzeige geografischer Standorte. Mit Azure Search können die Benutzer Daten auf der Grundlage der Nähe eines Suchergebnisses zu einem bestimmten Ort oder auf der Grundlage einer bestimmten geografischen Region untersuchen. In diesem Video wird erläutert, wie dies funktioniert: [Channel 9: Azure Search and Geospatial Data](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data).

#### <a name="cloud-service-advantages"></a>Vorteile des Clouddiensts

+ Die **hohe Verfügbarkeit** macht den Suchdienst äußerst zuverlässig. [Azure Search bietet eine SLA von 99,9 Prozent](https://azure.microsoft.com/support/legal/sla/search/v1_0/) (bei ordnungsgemäßer Skalierung).

+ Azure Search ist eine **vollständig verwaltete** End-to-End-Lösung, für die keinerlei Infrastrukturverwaltungsaufwand anfällt. Eine zweidimensionale Skalierung ermöglicht die Anpassung des Diensts an Ihre Anforderungen, um mehr Dokumentspeicher, ein höheres Abfrageaufkommen oder beides zu bewältigen.

#### <a name="monitoring-and-reporting"></a>Überwachung und Berichterstellung

+ **Analysedaten für Suchdatenverkehr** werden [erfasst und analysiert](search-traffic-analytics.md) und ermöglichen Rückschlüsse auf Benutzereingaben im Suchfeld.

+ Metriken zu Abfragen pro Sekunde, Latenz und Drosselung werden erfasst und in Portalseiten protokolliert, ohne dass hierzu eine zusätzliche Konfiguration erforderlich ist. Sie können außerdem auf einfache Weise die Index- und Dokumentanzahl überwachen, um bei Bedarf die Kapazität anzupassen. 

#### <a name="tools-for-prototyping-and-inspection"></a>Tools für Prototyperstellung und Überprüfung

Im Portal können Sie den **Datenimport-Assistenten** zum Konfigurieren von Indexern, den Index-Designer zum Einrichten eines Index und den **Search-Explorer** zum Ausgeben von Abfragen für all Ihre Indizes direkt aus dem Azure-Portal für Ihr Konto nutzen, sodass Sie Abfragen testen und Bewertungsprofile optimieren können. Sie können außerdem einen beliebigen Index öffnen, um sein Schema anzuzeigen.

## <a name="how-to-get-started"></a>Erste Schritte

Starten Sie mit einem kostenlosen Dienst, und [erstellen Sie einen Index mithilfe der integrierten Beispieldaten, den Sie anschließend abfragen können](search-get-started-portal.md). Sie können das Portal für sämtliche Aufgaben verwenden. Damit bietet das Portal eine schnelle Methode zum Testen von Azure Search, bevor Sie Code schreiben müssen.

1. Azure-Abonnenten können [einen Dienst im Free-Tarif bereitstellen](search-create-service-portal.md).

  Benutzer ohne Abonnement können [ein kostenloses Azure-Konto erstellen](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) und das kostenlose Guthaben nutzen, um unsere kostenpflichtigen Azure-Dienste zu testen. Wenn das Guthaben aufgebraucht ist, können Sie das Konto behalten und weiterhin kostenlose Azure-Dienste wie z.B. Websites nutzen. Ihre Kreditkarte wird nur dann belastet, wenn Sie Ihre Einstellungen explizit ändern und mit der Berechnung von Gebühren einverstanden sind.

  Alternativ dazu können Sie Ihre [Vorteile für MSDN-Abonnenten aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). Ihr MSDN-Abonnement beinhaltet ein monatliches Guthaben, das Sie für zahlungspflichtige Azure-Dienste verwenden können. 

2. Führen Sie den **Datenimport-Assistenten** für das [integrierte Immobilien-Beispieldataset](search-get-started-portal.md#create-index) aus.

  Dieser Assistent kann aus den meisten Azure-Datenquellen einen Index generieren und laden. Code ist für Datenquellen erforderlich, die nicht explizit vom Assistenten unterstützt werden.

3. Verwenden Sie den [Search-Explorer](search-get-started-portal.md#query-index), um den Index abzufragen.

## <a name="rest-api--net-sdk"></a>REST API | .NET SDK

Auch wenn im Portal zahlreiche Aufgaben ausgeführt werden können, ist Azure Search für Entwickler konzipiert, die Suchfunktionalität in vorhandene Anwendungen integrieren möchten. Folgende Programmierschnittstellen stehen zur Verfügung:

|Plattform |Beschreibung |
|-----|------------|
|[REST](https://docs.microsoft.com/rest/api/searchservice/) | Durch eine beliebige Programmierplattform und -sprache (darunter Xamarin, Java und JavaScript) unterstützte HTTP-Befehle.|
|[.NET SDK](search-howto-dotnet-sdk.md) | Der .NET-Wrapper für die REST-API ermöglicht eine effiziente Codierung in C# und andere Sprachen für verwalteten Code, die auf das .NET Framework ausgerichtet sind. |

## <a name="watch-a-short-video"></a>Sehen Sie sich ein kurzes Video an

Suchmodule sind die wichtigsten Komponenten für den Informationsabruf in mobilen Apps, im Web und in Unternehmensdatenspeichern. Azure Search bietet Tools zum Erstellen einer Suchoberfläche, die denen großer kommerzieller Websites ähnelt. Dieses 9-minütige Video von Programm-Manager Liam Cavanagh erläutert, wie die Integration eines Suchmoduls Ihre App aufwerten kann, stellt die wichtigsten Features von Azure Search vor und zeigt einen typischen Workflow. 

>[!VIDEO https://channel9.msdn.com/Events/Connect/2016/138/player]
 
+ Minute&0;–3: Vorstellung der wichtigste Features und Anwendungsfälle.
+ Minute&3;–4: Erläuterung der Dienstbereitstellung. 
+ Minute&4;–6: Vorstellung des Datenimport-Assistenten zum Erstellen eines Index mithilfe des integrierten Immobilien-Beispieldatasets.
+ Minute&6;–9: Vorstellung des Search-Explorers und verschiedener Abfragen.


