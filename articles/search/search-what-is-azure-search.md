---
title: Was ist Azure Search? | Microsoft Docs
description: "Azure Search ist ein vollständig verwalteter, gehosteter Cloudsuchdienst. Weitere Informationen finden Sie in dieser Featureübersicht."
services: search
manager: jhubbard
author: ashmaka
documentationcenter: 
ms.assetid: 50bed849-b716-4cc9-bbbc-b5b34e2c6153
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/24/2017
ms.author: ashmaka
ms.translationtype: Human Translation
ms.sourcegitcommit: e155891ff8dc736e2f7de1b95f07ff7b2d5d4e1b
ms.openlocfilehash: 25f4ef15390ed5b97bd2927126f5ecf250d2daf9
ms.contentlocale: de-de
ms.lasthandoff: 05/02/2017

---
# <a name="what-is-azure-search"></a>Was ist Azure Search?
Azure Search ist eine cloudbasierte SaaS-Lösung, deren Server- und Infrastrukturtechnologien von Microsoft verwaltet werden. Dadurch erhalten Sie einen sofort einsatzbereiten Dienst, den Sie mit Ihren Daten füllen und anschließend verwenden können, um Ihrer Webanwendung oder mobilen Anwendung eine Suchfunktion hinzuzufügen. Mit Azure Search können Sie Ihre Anwendungen unter Verwendung einer einfachen [REST-API](https://msdn.microsoft.com/library/azure/dn798935.aspx) oder eines [.NET-SDK](search-howto-dotnet-sdk.md) problemlos mit einer zuverlässigen Suchfunktion ausstatten, ohne sich mit der Suchinfrastruktur auseinanderzusetzen oder sich umfassende Kenntnisse zu Suchvorgängen aneignen zu müssen.

<a name="feature-drilldown"></a>

## <a name="embed-a-powerful-search-experience-in-your-app-or-site"></a>Integrieren leistungsfähiger Suchfunktionen in Ihre App oder Website 

Lernen Sie die Features in Azure Search kennen.

### <a name="full-text-search-and-text-analysis"></a>Volltextsuche und Textanalyse

[Volltextsuche](https://en.wikipedia.org/wiki/Full_text_search) ist für die meisten Apps auf Suchbasis ein primärer Anwendungsfall. In Azure Search können Abfragen in der [einfachen Abfragesyntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) formuliert werden, die über logische Operatoren, Operatoren für die Suche von Ausdrücken, Suffixoperatoren und Rangfolgenoperatoren verfügt. Die [Lucene-Abfragesyntax](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) ermöglicht zudem die Verwendung von Fuzzy- und NEAR-Suchen, Term Boosts und regulären Ausdrücken. Azure Search unterstützt darüber hinaus [benutzerdefinierte lexikalische Analysen](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search), wodurch Ihre Anwendung komplexe Suchabfragen mit phonetischem Abgleich und regulären Ausdrücken verarbeiten kann.

### <a name="language-support"></a>Sprachunterstützung

Azure Search unterstützt lexikalische Analysen für [56 verschiedene Sprachen](https://docs.microsoft.com/rest/api/searchservice/language-support). Durch den kombinierten Einsatz von Lucene- und Microsoft-Analysen (mit jahrelanger Erfahrung bei der Sprachdatenverarbeitung in Office und Bing) kann Text mit Azure Search im Suchfeld Ihrer Anwendung analysiert werden. Sprachspezifische linguistische Probleme können intelligent gelöst werden, z.B. Zeiten von Verben, Geschlecht, unregelmäßiger Plural von Substantiven (Englisch: mouse/mice), Auflösen von zusammengesetzten Wörtern, Worttrennung (für Sprachen ohne Leerstellen) und vieles mehr.

### <a name="data-integration"></a>Datenintegration

Sie können JSON-Datenstrukturen per Push in einen Azure Search-Index übertragen. Bei unterstützten Datenquellen können Sie außerdem [Indexer](search-indexer-overview.md) für einen automatischen Crawl in Azure SQL-Datenbank, Azure DocumentDB oder [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) einsetzen, um den Inhalt Ihres Suchindex mit Ihrem primären Datenspeicher zu synchronisieren.

Zum [Indizieren gängiger Dateiformate](search-howto-indexing-azure-blob-storage.md), wie z.B. Microsoft Office, PDF und HTML, steht eine *Funktion zum Knacken von Dokumenten* zur Verfügung.

### <a name="search-experience"></a>Suchoberfläche

+ Für Suchleisten mit AutoVervollständigen und für Abfragen mit Textvervollständigung können **Suchvorschläge** aktiviert werden. Bei unvollständigen Sucheingaben von Benutzern werden [Vorschläge für Dokumente aus Ihrem Index](https://docs.microsoft.com/rest/api/searchservice/suggesters) angezeigt.

+ Die **Facettennavigation** wird über [einen einzigen Abfrageparameter](https://docs.microsoft.com/azure/search/search-faceted-navigation) aktiviert. Azure Search gibt eine Struktur für die Facettennavigation zurück, die Sie als Code hinter einer Kategorieliste für eine selbstgesteuerte Filterung nutzen können (z.B., um Katalogartikel nach Preisbereich oder Marke zu filtern).

+ **Filter** ermöglichen die Integration einer Facettennavigation in die Benutzeroberfläche Ihrer Anwendung, die Erweiterung von Abfrageformulierungen sowie die Filterung auf der Grundlage von Kriterien von Benutzern oder Entwicklern. Erstellen Sie Filter mithilfe der [OData-Syntax](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search).

+ Mit der **Treffermarkierung** [wird eine Formatierung auf ein übereinstimmendes Schlüsselwort in den Suchergebnissen angewendet](https://docs.microsoft.com/rest/api/searchservice/Search-Documents). Sie können auswählen, für welche Felder hervorgehobene Ausschnitte zurückgegeben werden sollen.

+ Ein zentraler Vorteil von Azure Search ist die **einfache Bewertung**. Anhand von [Bewertungsprofilen](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) kann die Relevanz als Funktion der Werte in den Dokumenten selbst modelliert werden. Beispielsweise kann es wünschenswert sein, dass neuere Produkte oder herabgesetzte Produkte in den Suchergebnissen weiter oben angezeigt werden. Sie können Bewertungsprofile auch mithilfe von Tags für eine personalisierte Bewertung erstellen, die auf den Sucheinstellungen der Kunden basiert, die Sie nachverfolgt und separat gespeichert haben.

+ Eine **Sortierung** wird über das Indexschema für mehrere Felder bereitgestellt und dann zur Abfragezeit mit einem einzelnen Suchparameter umgeschaltet.

+ Dank der **fein abgestimmten Steuerung**, die Azure Search für Ihre Suchergebnisse bietet, sind [Paginierung](search-pagination-page-layout.md) und Einschränkung Ihrer Suchergebnisse kein Problem.  

### <a name="geosearch"></a>Geografische Suche

Azure Search bietet eine intelligente Verarbeitung, Filterung und Anzeige geografischer Standorte. Mit Azure Search können die Benutzer Daten auf der Grundlage der Nähe eines Suchergebnisses zu einem physischen Ort untersuchen. In diesem Video wird erläutert, wie dies funktioniert: [Channel 9: Azure Search and Geospatial Data](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data).

### <a name="monitoring-and-reporting"></a>Überwachung und Berichterstellung

+ **Analysedaten für Suchdatenverkehr** werden [erfasst und analysiert](search-traffic-analytics.md) und ermöglichen Rückschlüsse auf Benutzereingaben im Suchfeld.

+ Metriken zu Abfragen pro Sekunde, Latenz und Drosselung werden erfasst und in Portalseiten protokolliert, ohne dass hierzu eine zusätzliche Konfiguration erforderlich ist. Sie können außerdem auf einfache Weise die Index- und Dokumentanzahl überwachen, um bei Bedarf die Kapazität anzupassen. 

### <a name="tools-for-prototyping-and-inspection"></a>Tools für Prototyperstellung und Überprüfung

Im Portal können Sie den **Datenimport-Assistenten** zum Konfigurieren von Indexern, den Index-Designer zum Einrichten eines Indexes und den **Search-Explorer** zum Testen von Abfragen und Optimieren von Bewertungsprofilen verwenden. Sie können außerdem einen beliebigen Index öffnen, um sein Schema anzuzeigen.

<a name="cloud-service-advantage"></a>

### <a name="cloud-service-advantages"></a>Vorteile des Clouddiensts

+ Die **hohe Verfügbarkeit** macht den Suchdienst äußerst zuverlässig. [Azure Search bietet eine SLA von 99,9 Prozent](https://azure.microsoft.com/support/legal/sla/search/v1_0/) (bei ordnungsgemäßer Skalierung).

+ Azure Search ist eine **vollständig verwaltete und skalierbare** End-to-End-Lösung, für die keinerlei Infrastrukturverwaltungsaufwand anfällt. Eine zweidimensionale Skalierung ermöglicht die Anpassung des Diensts an Ihre Anforderungen, um mehr Dokumentspeicher, ein höheres Abfrageaufkommen oder beides zu bewältigen.

## <a name="how-it-works"></a>So funktioniert's
### <a name="step-1-provision-service"></a>Schritt 1: Bereitstellen des Diensts
Einen Azure Search-Dienst können Sie entweder über das [Azure-Portal](https://portal.azure.com/) oder die [Azure-Ressourcenverwaltungs-API](https://msdn.microsoft.com/library/azure/dn832684.aspx) bereitstellen. Sie können entweder den gemeinsam mit anderen Abonnenten genutzten kostenlosen Dienst auswählen oder eine [kostenpflichtige Ebene](https://azure.microsoft.com/pricing/details/search/), die Ressourcen zur ausschließlichen Nutzung für Ihren Dienst reserviert.

Für kostenpflichtige Ebenen können Sie einen Dienst in zwei Dimensionen skalieren: 

- Fügen Sie Replikate hinzu, um Ihre Kapazität zum Verarbeiten intensiver Abfragelasten zu erweitern.   
- Fügen Sie Partitionen hinzu, um den Speicher für weitere Dokumente zu vergrößern. 

Durch getrenntes Verarbeiten von Dokumentspeicherung und Abfragedurchsatz können Sie Ressourcen auf Produktionsanforderungen basierend kalibrieren.

### <a name="step-2-create-index"></a>Schritt 2: Erstellen des Indexes
Bevor Sie durchsuchbaren Inhalt hochladen können, müssen Sie einen Azure Search-Index definieren. Einen Index können Sie sich wie eine Datenbanktabelle vorstellen, die Ihre Daten enthält und Suchabfragen entgegennimmt. Sie definieren das zuzuordnende Indexschema, um die Struktur der Dokumente widerzuspiegeln, die Sie durchsuchen möchten (ähnlich wie bei Feldern in einer Datenbank).

Ein Schema kann im Azure-Portal oder programmgesteuert mithilfe des [.NET SDK](search-howto-dotnet-sdk.md) oder der [REST-API](https://msdn.microsoft.com/library/azure/dn798941.aspx) erstellt werden.

### <a name="step-3-index-data"></a>Schritt 3: Indizieren der Daten
Nachdem Sie einen Index definiert haben, können Sie Inhalte hochladen. Sie können entweder ein Push- oder Pullmodell verwenden.

Das Pullmodell ruft Daten aus externen Datenquellen ab. Es wird durch *Indexer* unterstützt, die Aspekte der Erfassung von Daten, z.B. Herstellen einer Verbindung mit Daten, Lesen und Serialisieren von Daten optimieren und automatisieren. [Indexer](/rest/api/searchservice/Indexer-ope) stehen für Azure DocumentDB, Azure SQL-Datenbank, Azure Blob Storage und in einer Azure-VM gehostete SQL Server-Instanzen zur Verfügung. Sie können einen Indexer für bedarfsgesteuerte oder geplante Datenaktualisierung konfigurieren.

Das Pushmodell wird über das SDK oder die REST-APIs bereitgestellt, die zum Senden von aktualisierten Dokumenten an einen Index verwendet werden. Sie können Daten aus praktisch jedem Dataset mit JSON-Format übertragen. Anleitungen zum Laden von Daten finden Sie unter [Hinzufügen, Aktualisieren oder Löschen von Dokumenten](https://msdn.microsoft.com/library/azure/dn798930.aspx) oder [Verwenden des .NET SDK](search-howto-dotnet-sdk.md).

### <a name="step-4-search"></a>Schritt 4: Suchen
Nach dem Füllen eines Indexes können Sie über einfache HTTP-Anforderungen mit REST-API oder dem .NET-SDK [Suchabfragen](/rest/api/searchservice/Search-Documents) an Ihren Dienstendpunkt richten.

## <a name="how-it-compares"></a>Im Vergleich

Kunden fragen häufig nach Gemeinsamkeiten der [Volltextsuche in Azure Search](search-lucene-query-architecture.md) mit der [Volltextsuche](https://en.wikipedia.org/wiki/Full_text_search) in ihrem Datenbankprodukt und den Unterschieden. Wir antworten, dass die Azure Search-Sprachfunktionen mit Unterstützung für Lucene-Abfragen, Sprachanalysefunktionen von Lucene und Microsoft, benutzerdefinierten Analysen für phonetische oder andere spezielle Eingaben und der Möglichkeit zum Zusammenführen von Daten aus mehreren Quellen im Suchindex umfangreicher und flexibler sind. 

Ein anderer entscheidender Punkt ist, dass eine Suchlösung allen Suchanforderungen gerecht wird. Beispielsweise wünschen Sie vielleicht benutzerdefinierte Bewertung der Ergebnisse, Facettennavigation für selbstgesteuertes Filtern, Treffermarkierung und automatische Vervollständigung mit Abfragevorschlägen. 

Tools zum Überwachen und Verstehen der Abfrageaktivität können die Entscheidung für eine Suchlösung auch beeinflussen. Azure Search unterstützt z.B. die [Analyse des Suchdatenverkehrs](search-traffic-analytics.md) mit Metriken zu Durchklickrate, häufigsten Suchen, Suchen ohne Klicks usw. In Produkten, bei denen die Suche nur ein Add-On ist, werden Sie diese Features kaum finden.

Ressourcenverwendung ist ein weiterer Aspekt. Suche unter Verwendung natürlicher Sprache ist häufig rechenintensiv. Einige unserer Kunden nutzten das Auslagern von Suchvorgängen nach Azure Search als Möglichkeit, Computerressourcen für die Transaktionsverarbeitung beizubehalten. Wenn Sie die Suche nach außen verlagern, können Sie problemlos Ihrem Abfragevolumen gemäß skalieren.

Sobald Sie sich für eine dedizierte Suche entschieden haben, fällt Ihre nächste Entscheidung zwischen einem Clouddienst und einem lokalen Server. Ein Clouddienst ist die richtige Wahl, wenn Sie eine [sofort einsetzbare Lösung mit minimalem Aufwand, minimaler Wartung und anpassbarer Skalierung](#cloud-service-advantage) wünschen.

Innerhalb des Cloudparadigmas bieten einige Anbieter vergleichbare Basisfunktionen mit Volltextsuche, geografischer Suche und der Möglichkeit zur Verarbeitung einer gewissen Ungenauigkeit bei der Sucheingabe. Das passende Suchmodul ergibt sich in der Regel durch ein [spezialisiertes Feature](#feature-drilldown) oder durch die Benutzerfreundlichkeit und Einfachheit von APIs, Tools und Verwaltungsfunktionen.

Unter den Cloudanbietern eignet Azure Search sich am besten für die Volltextsuche in Inhaltsspeichern und Datenbanken in Azure, für Apps, bei denen Informationsabruf und Inhaltsnavigation primär auf der Suche basieren. Wichtige Vorteile umfassen:

+ Azure-Datenintegration (Crawler) auf Indexebene
+ Azure-Portal für eine zentrale Verwaltung
+ Skalierung, Zuverlässigkeit und erstklassige Verfügbarkeit von Azure
+ Linguistische und benutzerdefinierte Analyse, mit Analysemodulen für eine robuste Volltextsuche in 56 Sprachen
+ [Kernfeatures für auf die Suche ausgerichtete Apps](#feature-drilldown): Bewertung, Facettennavigation, Vorschläge, Synonyme, geografische Suche und mehr.

> [!Note]
> Klar gesagt: Datenquellen, die nicht aus Azure stammen, werden vollständig unterstützt, unterliegen jedoch statt Indexern einer codeintensiveren Pushmethode. Mit unseren APIs können Sie eine beliebige JSON-Dokumentsammlung an einen Azure Search-Index weiterreichen.

Unter unseren Kunden zählen diejenigen mit Onlinekatalogen, Branchenprogrammen und Anwendungen zur Dokumentermittlung zu denen, die am meisten von den umfassenden Features in Azure Search profitieren.

## <a name="rest-api--net-sdk"></a>REST API | .NET SDK

Auch wenn im Portal viele Aufgaben ausgeführt werden können, ist Azure Search in erster Linie für Entwickler konzipiert, die Suchfunktionalität in vorhandene Anwendungen integrieren möchten. Folgende Programmierschnittstellen stehen zur Verfügung:

|Plattform |Beschreibung |
|-----|------------|
|[REST](https://docs.microsoft.com/rest/api/searchservice/) | Durch eine beliebige Programmierplattform und -sprache (darunter Xamarin, Java und JavaScript) unterstützte HTTP-Befehle.|
|[.NET SDK](search-howto-dotnet-sdk.md) | Der .NET-Wrapper für die REST-API ermöglicht eine effiziente Codierung in C# und andere Sprachen für verwalteten Code, die auf das .NET Framework ausgerichtet sind. |

## <a name="free-trial"></a>Kostenlose Testversion
Azure-Abonnenten können [einen Dienst im Free-Tarif bereitstellen](search-create-service-portal.md).

Wenn Sie kein Abonnent sind, können Sie [ein kostenloses Azure-Konto eröffnen](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F). Sie erhalten Gutschriften zum Testen kostenpflichtiger Azure-Dienste. Wenn das Guthaben aufgebraucht ist, können Sie das Konto behalten und weiterhin [kostenlose Azure-Dienste](https://azure.microsoft.com/free/) nutzen. Ihre Kreditkarte wird nur dann belastet, wenn Sie Ihre Einstellungen explizit ändern und mit der Berechnung von Gebühren einverstanden sind.

Alternativ können Sie Ihre [Vorteile für MSDN-Abonnenten aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): Ihr MSDN-Abonnement beinhaltet ein monatliches Guthaben, das Sie für zahlungspflichtige Azure-Dienste verwenden können. 

## <a name="watch-a-short-video"></a>Sehen Sie sich ein kurzes Video an

Suchmodule sind die wichtigsten Komponenten für den Informationsabruf in mobilen Apps, im Web und in Unternehmensdatenspeichern. Azure Search bietet Tools zum Erstellen einer Suchoberfläche, die denen großer kommerzieller Websites ähnelt.

In diesem 9-Minuten-Video von Programm-Manager Liam Cavanagh erfahren Sie, wie Ihre App von der Integration einer Suchmaschine profitieren kann. Kurze Demos behandeln Schlüsselfeatures in Azure Search und zeigen, wie ein typischer Workflow aussieht. 

>[!VIDEO https://channel9.msdn.com/Events/Connect/2016/138/player]
 
+ Minute 0–3: Vorstellung der wichtigste Features und Anwendungsfälle.
+ Minute 3–4: Erläuterung der Dienstbereitstellung. 
+ Minute 4–6: Vorstellung des Datenimport-Assistenten zum Erstellen eines Index mithilfe des integrierten Immobilien-Beispieldatasets.
+ Minute 6–9: Vorstellung des Search-Explorers und verschiedener Abfragen.



