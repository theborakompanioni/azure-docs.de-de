---
title: Was ist Azure Search | Microsoft Docs
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
ms.date: 01/12/2017
ms.author: ashmaka
translationtype: Human Translation
ms.sourcegitcommit: 292c9150822363aba3336b1efce579dc5362cb14
ms.openlocfilehash: 99b3babee9b252b2d741515391295d8b1dc5c747


---
# <a name="what-is-azure-search"></a>Was ist Azure Search?
Azure Search ist eine cloudbasierte SaaS-Lösung, deren Server- und Infrastrukturtechnologien von Microsoft verwaltet werden. Dadurch erhalten Sie einen sofort einsatzbereiten Dienst, den Sie mit Ihren Daten füllen und anschließend verwenden können, um Ihrer Webanwendung oder mobilen Anwendung eine Suchfunktion hinzuzufügen. Mit Azure Search können Sie Ihre Anwendungen unter Verwendung einer einfachen [REST-API](https://msdn.microsoft.com/library/azure/dn798935.aspx) oder eines [.NET-SDK](search-howto-dotnet-sdk.md) problemlos mit einer zuverlässigen Suchfunktion ausstatten, ohne sich mit der Suchinfrastruktur auseinanderzusetzen oder sich umfassende Kenntnisse zu Suchvorgängen aneignen zu müssen.

## <a name="give-your-users-a-powerful-search-experience"></a>Leistungsfähige Suchfunktion für Benutzer
**leistungsfähige Abfragen** formulieren, indem Sie die [einfache Abfragesyntax](https://msdn.microsoft.com/library/azure/dn798920.aspx)verwenden, die über logische Operatoren, Operatoren für die Suche von Ausdrücken, Suffixoperatoren und Rangfolgenoperatoren verfügt. Die [Lucene-Abfragesyntax](https://msdn.microsoft.com/library/azure/mt589323.aspx) ermöglicht zudem die Verwendung von Fuzzy- und NEAR-Suchen, Term Boosts und regulären Ausdrücken. Azure Search unterstützt auch benutzerdefinierte lexikalische Analysen, was Ihrer Anwendung die Behandlung komplexer Suchabfragen mit phonetischem Abgleich und regulären Ausdrücken ermöglicht.

**Sprachunterstützung** steht für [56 Sprachen zur Verfügung](https://msdn.microsoft.com/library/azure/dn879793.aspx). Durch den kombinierten Einsatz von Lucene- und Microsoft-Analysen (mit jahrelanger Erfahrung bei der Sprachdatenverarbeitung in Office und Bing) kann Text mit Azure Search im Suchfeld Ihrer Anwendung analysiert werden. Sprachspezifische linguistische Probleme können intelligent gelöst werden, z.B. Zeiten von Verben, Geschlecht, unregelmäßiger Plural von Substantiven (Englisch: mouse/mice), Auflösen von zusammengesetzten Wörtern, Worttrennung (für Sprachen ohne Leerstellen) und vieles mehr.

**Suchvorschläge** aktiviert werden. [Vorschläge für Dokumente aus Ihrem Index](https://msdn.microsoft.com/library/azure/dn798936.aspx) angezeigt.

**Treffermarkierungen** [vereinfachen](https://msdn.microsoft.com/library/azure/dn798927.aspx) die Erkennung des Textausschnitts in allen Ergebnissen, die Übereinstimmungen für die Abfrage enthalten. Sie können wählen, für welche Felder hervorgehobene Ausschnitte zurückgegeben werden sollen.

Mit Azure Search kann Ihre Suchergebnisseite problemlos mit einer **Facettennavigation** versehen werden. Mit [einem einzigen Abfrageparameter](https://msdn.microsoft.com/library/azure/dn798927.aspx) gibt Azure Search alle Informationen zurück, die für die Facettensuche auf Ihrer App-Benutzeroberfläche erforderlich sind. Dadurch können die Benutzer Detailinformationen zu Suchergebnissen anzeigen und Suchergebnisse filtern, um beispielsweise Katalogelemente nach Preisbereich oder Marke zu filtern.

**geografischer Daten** ermöglicht eine intelligente Verarbeitung, Filterung und Anzeige geografischer Standorte. Mit Azure Search können die Benutzer Daten auf der Grundlage der Nähe eines Suchergebnisses zu einem bestimmten Ort oder auf der Grundlage einer bestimmten geografischen Region untersuchen. In diesem Video wird erläutert, wie das funktioniert: [Channel 9: Azure Search and Geospatial Data](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data).

**Filter** ermöglichen die problemlose Integration einer Facettennavigation in die UI Ihrer Anwendung, die Erweiterung von Abfrageformulierungen sowie die Filterung auf der Grundlage von Kriterien von Benutzern oder Entwicklern. Erstellen Sie leistungsstarke Filter, indem Sie die [OData-Syntax](https://msdn.microsoft.com/library/azure/dn798921.aspx)verwenden.

## <a name="empower-your-developers-with-an-easy-to-use-service"></a>Benutzerfreundlicher Dienst für Ihre Entwickler
**hohe Verfügbarkeit** macht den Suchdienst äußerst zuverlässig. [Azure Search bietet eine SLA von 99,9 Prozent](https://azure.microsoft.com/support/legal/sla/search/v1_0/)(bei ordnungsgemäßer Skalierung).

**vollständig verwaltete** End-to-End-Lösung, für die keinerlei Infrastrukturverwaltungsaufwand anfällt. Eine zweidimensionale Skalierung ermöglicht die problemlose Anpassung des Diensts an Ihre Anforderungen, um mehr Dokumentspeicher, ein höheres Abfrageaufkommen oder beides zu bewältigen.

Dank **Datenintegration** mit [Indexern](https://msdn.microsoft.com/library/azure/dn946891.aspx) kann Azure Search automatisch die Azure SQL-Datenbank, Azure DocumentDB oder [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) durchforsten, um den Inhalt Ihres Suchindex mit Ihrem primären Datenspeicher zu synchronisieren.

**Lesen und Indizieren gängiger Dateiformate** (wie etwa Microsoft Office, PDF und HTML) steht eine [Funktion zum Knacken von Dokumenten](search-howto-indexing-azure-blob-storage.md) (derzeit in der Vorschau) zur Verfügung.

**Analysedaten für Suchdatenverkehr** werden [gesammelt und analysiert](search-traffic-analytics.md) und ermöglichen Rückschlüsse auf Benutzereingaben im Suchfeld.

**einfache Bewertung** . [Bewertungsprofilen](https://msdn.microsoft.com/library/azure/dn798928.aspx) können Organisationen die Relevanz als Funktion der Werte aus den eigentlichen Dokumenten modellieren. Beispielsweise kann es wünschenswert sein, dass neuere Produkte oder herabgesetzte Produkte in den Suchergebnissen weiter oben angezeigt werden. Sie können Bewertungsprofile auch mithilfe von Tags für eine personalisierte Bewertung erstellen, die auf den Sucheinstellungen der Kunden basiert, die Sie nachverfolgt und separat gespeichert haben.

**Sortierung** wird über das Indexschema für mehrere Felder bereitgestellt und dann zur Abfragezeit mit einem einzelnen Suchparameter umgeschaltet.

**fein abgestimmten Steuerung** , die Azure Search für Ihre Suchergebnisse bietet, sind [Paginierung](search-pagination-page-layout.md) und Drosselung Ihrer Suchergebnisse kein Problem.  

**Such-Explorer** können Sie Abfragen für alle Ihre Indizes direkt über das Azure-Portal Ihres Kontos durchführen und so problemlos Abfragen testen und Bewertungsprofile optimieren.

## <a name="how-it-works"></a>So funktioniert's
### <a name="1-provision-service"></a>1. Bereitstellen des Diensts
Ein Azure Search-Dienst kann entweder über das [Azure-Portal](https://portal.azure.com/) oder über die [Azure-Ressourcenverwaltungs-API](https://msdn.microsoft.com/library/azure/dn832684.aspx) bereitgestellt werden.

Verwenden Sie je nach Konfiguration des Suchdiensts entweder den gemeinsam mit anderen Azure Search-Abonnenten verwendeten Dienst im Free-Tarif oder einen [kostenpflichtigen Tarif](https://azure.microsoft.com/pricing/details/search/) mit speziell für Ihren Dienst reservierten Ressourcen. Beim Bereitstellen des Diensts wird auch die Region des Datencenters ausgewählt, das den Dienst hostet.

Je nach Wahl Ihres Tarifs können Sie Ihren Dienst in zwei Dimensionen skalieren: 1) Sie können Replikate hinzufügen, um die Kapazität für ein hohes Abfrageaufkommen zu erhöhen, und 2) Sie können Partitionen hinzufügen, um den Speicherplatz für weitere Dokumente zu erhöhen. Durch die getrennte Behandlung von Dokumentspeicher und Durchsatz können Sie Ihren Suchdienst exakt an Ihre Anforderungen anpassen.

### <a name="2-create-index"></a>2. Erstellen des Index
Bevor Sie Ihre Inhalte an den Azure Search-Dienst hochladen können, müssen Sie einen Azure Search-Index definieren. Einen Index können Sie sich wie eine Datenbanktabelle vorstellen, die Ihre Daten enthält und Suchabfragen entgegennimmt. Sie definieren das Indexschema für die Zuordnung zur Struktur der Dokumente, die Sie durchsuchen möchten (ähnlich wie bei Feldern in einer Datenbank).

Das Schema dieser Indizes kann entweder im Azure-Portal oder programmgesteuert mithilfe des [.NET SDK](search-howto-dotnet-sdk.md) oder der [REST-API](https://msdn.microsoft.com/library/azure/dn798941.aspx) erstellt werden. Nach dem Definieren des Index können Sie Ihre Daten an den Azure Search-Dienst hochladen, wo sie anschließend indiziert werden.

### <a name="3-index-data"></a>3. Indizieren der Daten
Nachdem Sie die Felder und Attribute Ihres Index definiert haben, können Sie mit dem Hochladen von Inhalten an den Index beginnen. Dazu können Sie entweder ein Push- oder ein Pullmodell verwenden.

Das Pullmodell wird über Indexer bereitgestellt, die für bedarfsabhängige oder geplante Aktualisierungen konfiguriert werden können (siehe [Indizierungsvorgänge (REST-API für den Azure Search-Dienst)](https://msdn.microsoft.com/library/azure/dn946891.aspx)), sodass Sie problemlos Daten und Datenänderungen aus einer Azure-DocumentDB, einer Azure SQL-Datenbank, einem Azure-BLOB-Speicher oder einem in einer Azure-VM gehosteten SQL-Server erfassen können.

Das Pushmodell wird über das SDK oder die REST-APIs bereitgestellt, die zum Senden von aktualisierten Dokumenten an einen Index verwendet werden. Sie können Daten aus praktisch jedem Dataset mit JSON-Format übertragen. Anleitungen zum Laden von Daten finden Sie unter [Hinzufügen, Aktualisieren oder Löschen von Dokumenten](https://msdn.microsoft.com/library/azure/dn798930.aspx) oder [Verwenden des .NET SDK](search-howto-dotnet-sdk.md).

### <a name="4-search"></a>4. Suche
Nach dem Füllen Ihres Azure Search-Index können Sie über einfache HTTP-Anforderungen mit REST-API oder dem .NET-SDK [Suchabfragen](https://msdn.microsoft.com/library/azure/dn798927.aspx) an Ihren Dienstendpunkt richten.

## <a name="try-it-now-for-free"></a>Gleich (kostenlos) ausprobieren!
Azure-Abonnenten können [einen Dienst im Free-Tarif bereitstellen](search-create-service-portal.md).

Wenn Sie kein Abonnent sind, können Sie [kostenlos ein Azure-Konto erstellen](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F): Sie erhalten ein Guthaben, das Sie zum Ausprobieren zahlungspflichtiger Azure-Dienste nutzen können, und Sie können das Konto selbst dann behalten und die kostenlosen Azure-Dienste wie Websites nutzen, wenn das Guthaben aufgebraucht ist. Ihre Kreditkarte wird nur dann belastet, wenn Sie Ihre Einstellungen explizit ändern und mit einer Zahlung einverstanden sind.

Alternativ können Sie Ihre [Vorteile für MSDN-Abonnenten aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): Ihr MSDN-Abonnement beinhaltet ein monatliches Guthaben, das Sie für zahlungspflichtige Azure-Dienste verwenden können. 





<!--HONumber=Jan17_HO2-->


