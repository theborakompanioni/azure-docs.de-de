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
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: 3720aed95706f6e8dd9a28085a6de0bc1f756a9b
ms.lasthandoff: 04/26/2017

---
# <a name="what-is-azure-search"></a>Was ist Azure Search?
Azure Search ist eine cloudbasierte SaaS-Lösung, deren Server- und Infrastrukturtechnologien von Microsoft verwaltet werden. Dadurch erhalten Sie einen sofort einsatzbereiten Dienst, den Sie mit Ihren Daten füllen und anschließend verwenden können, um Ihrer Webanwendung oder mobilen Anwendung eine Suchfunktion hinzuzufügen. Mit Azure Search können Sie Ihre Anwendungen unter Verwendung einer einfachen [REST-API](https://msdn.microsoft.com/library/azure/dn798935.aspx) oder eines [.NET-SDK](search-howto-dotnet-sdk.md) problemlos mit einer zuverlässigen Suchfunktion ausstatten, ohne sich mit der Suchinfrastruktur auseinanderzusetzen oder sich umfassende Kenntnisse zu Suchvorgängen aneignen zu müssen.

## <a name="embed-a-powerful-search-experience-in-your-app-or-site"></a>Integrieren leistungsfähiger Suchfunktionen in Ihre App oder Website


### <a name="full-text-search-and-text-analysis"></a>Volltextsuche und Textanalyse

Sie können Abfragen formulieren, indem Sie die [einfache Abfragesyntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) verwenden, die logische Operatoren, Operatoren für die Suche von Ausdrücken, Suffixoperatoren und Rangfolgenoperatoren bereitstellt. Die [Lucene-Abfragesyntax](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) ermöglicht zudem die Verwendung von Fuzzy- und NEAR-Suchen, Term Boosts und regulären Ausdrücken. Azure Search unterstützt darüber hinaus [benutzerdefinierte lexikalische Analysen](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search), wodurch Ihre Anwendung komplexe Suchabfragen mit phonetischem Abgleich und regulären Ausdrücken verarbeiten kann.

### <a name="language-support"></a>Sprachunterstützung

Azure Search unterstützt lexikalische Analysen für [56 verschiedene Sprachen](https://docs.microsoft.com/rest/api/searchservice/language-support). Durch den kombinierten Einsatz von Lucene- und Microsoft-Analysen (mit jahrelanger Erfahrung bei der Sprachdatenverarbeitung in Office und Bing) kann Text mit Azure Search im Suchfeld Ihrer Anwendung analysiert werden. Sprachspezifische linguistische Probleme können intelligent gelöst werden, z.B. Zeiten von Verben, Geschlecht, unregelmäßiger Plural von Substantiven (Englisch: mouse/mice), Auflösen von zusammengesetzten Wörtern, Worttrennung (für Sprachen ohne Leerstellen) und vieles mehr.

### <a name="data-integration"></a>Datenintegration

Sie können JSON-Datenstrukturen per Push in einen Azure Search-Index übertragen. Bei unterstützten Datenquellen können Sie außerdem [Indexer](search-indexer-overview.md) für einen automatischen Crawl in Azure SQL-Datenbank, Azure DocumentDB oder [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) einsetzen, um den Inhalt Ihres Suchindex mit Ihrem primären Datenspeicher zu synchronisieren.

Zum [Indizieren gängiger Dateiformate](search-howto-indexing-azure-blob-storage.md), wie z.B. Microsoft Office, PDF und HTML, steht eine **Funktion zum Knacken von Dokumenten** zur Verfügung.

### <a name="search-experience"></a>Suchoberfläche

+ Für Suchleisten mit AutoVervollständigen und für Abfragen mit Textvervollständigung können **Suchvorschläge** aktiviert werden. Bei unvollständigen Sucheingaben von Benutzern werden [Vorschläge für Dokumente aus Ihrem Index](https://docs.microsoft.com/rest/api/searchservice/suggesters) angezeigt.

+ Die **Facettennavigation** wird über [einen einzigen Abfrageparameter](https://docs.microsoft.com/azure/search/search-faceted-navigation) aktiviert. Azure Search gibt eine Struktur für die Facettennavigation zurück, die Sie als Code hinter einer Kategorieliste für eine selbstgesteuerte Filterung nutzen können (z.B., um Katalogartikel nach Preisbereich oder Marke zu filtern).

+ **Filter** ermöglichen die Integration einer Facettennavigation in die Benutzeroberfläche Ihrer Anwendung, die Erweiterung von Abfrageformulierungen sowie die Filterung auf der Grundlage von Kriterien von Benutzern oder Entwicklern. Erstellen Sie Filter mithilfe der [OData-Syntax](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search).

+ Mit der **Treffermarkierung** [wird eine Formatierung auf ein übereinstimmendes Schlüsselwort in den Suchergebnissen angewendet](https://docs.microsoft.com/rest/api/searchservice/Search-Documents). Sie können auswählen, für welche Felder hervorgehobene Ausschnitte zurückgegeben werden sollen.

+ Ein zentraler Vorteil von Azure Search ist die **einfache Bewertung**. Anhand von [Bewertungsprofilen](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) kann die Relevanz als Funktion der Werte in den Dokumenten selbst modelliert werden. Beispielsweise kann es wünschenswert sein, dass neuere Produkte oder herabgesetzte Produkte in den Suchergebnissen weiter oben angezeigt werden. Sie können Bewertungsprofile auch mithilfe von Tags für eine personalisierte Bewertung erstellen, die auf den Sucheinstellungen der Kunden basiert, die Sie nachverfolgt und separat gespeichert haben.

+ Eine **Sortierung** wird über das Indexschema für mehrere Felder bereitgestellt und dann zur Abfragezeit mit einem einzelnen Suchparameter umgeschaltet.

+ Dank der **fein abgestimmten Steuerung**, die Azure Search für Ihre Suchergebnisse bietet, sind [Paginierung](search-pagination-page-layout.md) und Einschränkung Ihrer Suchergebnisse kein Problem.  

### <a name="geosearch"></a>Geografische Suche

Azure Search bietet eine intelligente Verarbeitung, Filterung und Anzeige geografischer Standorte. Mit Azure Search können die Benutzer Daten auf der Grundlage der Nähe eines Suchergebnisses zu einem bestimmten Ort oder auf der Grundlage einer bestimmten geografischen Region untersuchen. In diesem Video wird erläutert, wie dies funktioniert: [Channel 9: Azure Search and Geospatial Data](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data).

### <a name="cloud-service-advantages"></a>Vorteile des Clouddiensts

+ Die **hohe Verfügbarkeit** macht den Suchdienst äußerst zuverlässig. [Azure Search bietet eine SLA von 99,9 Prozent](https://azure.microsoft.com/support/legal/sla/search/v1_0/) (bei ordnungsgemäßer Skalierung).

+ Azure Search ist eine **vollständig verwaltete** End-to-End-Lösung, für die keinerlei Infrastrukturverwaltungsaufwand anfällt. Eine zweidimensionale Skalierung ermöglicht die Anpassung des Diensts an Ihre Anforderungen, um mehr Dokumentspeicher, ein höheres Abfrageaufkommen oder beides zu bewältigen.

### <a name="monitoring-and-reporting"></a>Überwachung und Berichterstellung

+ **Analysedaten für Suchdatenverkehr** werden [erfasst und analysiert](search-traffic-analytics.md) und ermöglichen Rückschlüsse auf Benutzereingaben im Suchfeld.

+ Metriken zu Abfragen pro Sekunde, Latenz und Drosselung werden erfasst und in Portalseiten protokolliert, ohne dass hierzu eine zusätzliche Konfiguration erforderlich ist. Sie können außerdem auf einfache Weise die Index- und Dokumentanzahl überwachen, um bei Bedarf die Kapazität anzupassen. 

### <a name="tools-for-prototyping-and-inspection"></a>Tools für Prototyperstellung und Überprüfung

Im Portal können Sie den **Datenimport-Assistenten** zum Konfigurieren von Indexern, den Index-Designer zum Einrichten eines Index und den **Search-Explorer** zum Ausgeben von Abfragen für all Ihre Indizes direkt aus dem Azure-Portal für Ihr Konto nutzen, sodass Sie Abfragen testen und Bewertungsprofile optimieren können. Sie können außerdem einen beliebigen Index öffnen, um sein Schema anzuzeigen.

## <a name="how-it-works"></a>So funktioniert's
### <a name="step-1-provision-service"></a>Schritt 1: Bereitstellen des Diensts
Ein Azure Search-Dienst kann entweder über das [Azure-Portal](https://portal.azure.com/) oder über die [Azure-Ressourcenverwaltungs-API](https://msdn.microsoft.com/library/azure/dn832684.aspx) bereitgestellt werden.

Verwenden Sie je nach Konfiguration des Suchdiensts entweder den gemeinsam mit anderen Azure Search-Abonnenten verwendeten Dienst im Free-Tarif oder einen [kostenpflichtigen Tarif](https://azure.microsoft.com/pricing/details/search/) mit speziell für Ihren Dienst reservierten Ressourcen. Beim Bereitstellen des Diensts wird auch die Region des Datencenters ausgewählt, das den Dienst hostet.

Je nach Wahl Ihres Tarifs können Sie Ihren Dienst in zwei Dimensionen skalieren: 1) Sie können Replikate hinzufügen, um die Kapazität für ein hohes Abfrageaufkommen zu erhöhen, und 2) Sie können Partitionen hinzufügen, um den Speicherplatz für weitere Dokumente zu erhöhen. Durch die getrennte Behandlung von Dokumentspeicher und Durchsatz können Sie Ihren Suchdienst exakt an Ihre Anforderungen anpassen.

### <a name="step-2-create-index"></a>Schritt 2: Erstellen des Indexes
Bevor Sie Ihre Inhalte an den Azure Search-Dienst hochladen können, müssen Sie einen Azure Search-Index definieren. Einen Index können Sie sich wie eine Datenbanktabelle vorstellen, die Ihre Daten enthält und Suchabfragen entgegennimmt. Sie definieren das Indexschema für die Zuordnung zur Struktur der Dokumente, die Sie durchsuchen möchten (ähnlich wie bei Feldern in einer Datenbank).

Das Schema dieser Indizes kann entweder im Azure-Portal oder programmgesteuert mithilfe des [.NET SDK](search-howto-dotnet-sdk.md) oder der [REST-API](https://msdn.microsoft.com/library/azure/dn798941.aspx) erstellt werden. Nach dem Definieren des Index können Sie Ihre Daten an den Azure Search-Dienst hochladen, wo sie anschließend indiziert werden.

### <a name="step-3-index-data"></a>Schritt 3: Indizieren der Daten
Nachdem Sie die Felder und Attribute Ihres Index definiert haben, können Sie mit dem Hochladen von Inhalten an den Index beginnen. Dazu können Sie entweder ein Push- oder ein Pullmodell verwenden.

Das Pullmodell wird über Indexer bereitgestellt, die für bedarfsabhängige oder geplante Aktualisierungen konfiguriert werden können (siehe [Indizierungsvorgänge (REST-API für den Azure Search-Dienst)](https://msdn.microsoft.com/library/azure/dn946891.aspx)), sodass Sie problemlos Daten und Datenänderungen aus einer Azure-DocumentDB, einer Azure SQL-Datenbank, einem Azure-BLOB-Speicher oder einem in einer Azure-VM gehosteten SQL-Server erfassen können.

Das Pushmodell wird über das SDK oder die REST-APIs bereitgestellt, die zum Senden von aktualisierten Dokumenten an einen Index verwendet werden. Sie können Daten aus praktisch jedem Dataset mit JSON-Format übertragen. Anleitungen zum Laden von Daten finden Sie unter [Hinzufügen, Aktualisieren oder Löschen von Dokumenten](https://msdn.microsoft.com/library/azure/dn798930.aspx) oder [Verwenden des .NET SDK](search-howto-dotnet-sdk.md).

### <a name="step-4-search"></a>Schritt 4: Suchen
Nach dem Füllen Ihres Azure Search-Index können Sie über einfache HTTP-Anforderungen mit REST-API oder dem .NET-SDK [Suchabfragen](https://msdn.microsoft.com/library/azure/dn798927.aspx) an Ihren Dienstendpunkt richten.

## <a name="rest-api--net-sdk"></a>REST API | .NET SDK

Auch wenn im Portal zahlreiche Aufgaben ausgeführt werden können, ist Azure Search für Entwickler konzipiert, die Suchfunktionalität in vorhandene Anwendungen integrieren möchten. Folgende Programmierschnittstellen stehen zur Verfügung:

|Plattform |Beschreibung |
|-----|------------|
|[REST](https://docs.microsoft.com/rest/api/searchservice/) | Durch eine beliebige Programmierplattform und -sprache (darunter Xamarin, Java und JavaScript) unterstützte HTTP-Befehle.|
|[.NET SDK](search-howto-dotnet-sdk.md) | Der .NET-Wrapper für die REST-API ermöglicht eine effiziente Codierung in C# und andere Sprachen für verwalteten Code, die auf das .NET Framework ausgerichtet sind. |

## <a name="free-trial"></a>Kostenlose Testversion
Azure-Abonnenten können [einen Dienst im Free-Tarif bereitstellen](search-create-service-portal.md).

Wenn Sie kein Abonnent sind, können Sie [kostenlos ein Azure-Konto erstellen](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F): Sie erhalten ein Guthaben, das Sie zum Ausprobieren zahlungspflichtiger Azure-Dienste nutzen können, und Sie können das Konto selbst dann behalten und die kostenlosen Azure-Dienste wie Websites nutzen, wenn das Guthaben aufgebraucht ist. Ihre Kreditkarte wird nur dann belastet, wenn Sie Ihre Einstellungen explizit ändern und mit einer Zahlung einverstanden sind.

Alternativ können Sie Ihre [Vorteile für MSDN-Abonnenten aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): Ihr MSDN-Abonnement beinhaltet ein monatliches Guthaben, das Sie für zahlungspflichtige Azure-Dienste verwenden können. 

## <a name="watch-a-short-video"></a>Sehen Sie sich ein kurzes Video an

Suchmodule sind die wichtigsten Komponenten für den Informationsabruf in mobilen Apps, im Web und in Unternehmensdatenspeichern. Azure Search bietet Tools zum Erstellen einer Suchoberfläche, die denen großer kommerzieller Websites ähnelt. Dieses 9-minütige Video von Programm-Manager Liam Cavanagh erläutert, wie die Integration eines Suchmoduls Ihre App aufwerten kann, stellt die wichtigsten Features von Azure Search vor und zeigt einen typischen Workflow. 

>[!VIDEO https://channel9.msdn.com/Events/Connect/2016/138/player]
 
+ Minute 0–3: Vorstellung der wichtigste Features und Anwendungsfälle.
+ Minute 3–4: Erläuterung der Dienstbereitstellung. 
+ Minute 4–6: Vorstellung des Datenimport-Assistenten zum Erstellen eines Index mithilfe des integrierten Immobilien-Beispieldatasets.
+ Minute 6–9: Vorstellung des Search-Explorers und verschiedener Abfragen.



