---
title: Azure DocumentDB .NET Core-API, -SDK und -Ressourcen | Microsoft-Dokumentation
description: "Wichtige Informationen zu .NET Core-API und .NET Core SDK einschließlich Terminen für Veröffentlichung und Außerbetriebnahme sowie Änderungen an den einzelnen Versionen des DocumentDB .NET Core SDK."
services: cosmos-db
documentationcenter: .net
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: f899b314-26ac-4ddb-86b2-bfdf05c2abf2
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/12/2017
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 54454e98a2c37736407bdac953fdfe74e9e24d37
ms.openlocfilehash: 4e842bf237bf51b020218d1f612af55939b2deb9
ms.contentlocale: de-de
ms.lasthandoff: 07/13/2017

---
# <a name="documentdb-net-core-sdk-release-notes-and-resources"></a>DocumentDB .NET Core-SDK: Versionshinweise und Ressourcen
> [!div class="op_single_selector"]
> * [.NET](documentdb-sdk-dotnet.md)
> * [.NET-Änderungsfeed](documentdb-sdk-dotnet-changefeed.md)
> * [.NET Core](documentdb-sdk-dotnet-core.md)
> * [Node.js](documentdb-sdk-node.md)
> * [Java](documentdb-sdk-java.md)
> * [Python](documentdb-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/documentdb/)
> * [REST-Ressourcenanbieter](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td>**SDK-Download**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/)</td></tr>

<tr><td>**API-Dokumentation**</td><td>[.NET API-Referenzdokumentation](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)</td></tr>

<tr><td>**Beispiele**</td><td>[.NET-Codebeispiele](documentdb-dotnet-samples.md)</td></tr>

<tr><td>**Erste Schritte**</td><td>[Erste Schritte mit dem DocumentDB .NET Core SDK](documentdb-dotnetcore-get-started.md)</td></tr>

<tr><td>**Web-App-Tutorial**</td><td>[Webanwendungsentwicklung mit DocumentDB](documentdb-dotnet-application.md)</td></tr>

<tr><td>**Aktuelles unterstütztes Framework**</td><td>[.NET Standard 1.6 und .NET Standard 1.5](https://www.nuget.org/packages/NETStandard.Library)</td></tr>
</table></br>

## <a name="release-notes"></a>Versionsinformationen

Das DocumentDB .NET Core SDK weist Funktionsparität mit der neuesten Version des [DocumentDB .NET SDK](documentdb-sdk-dotnet.md) auf.

> [!NOTE] 
> Das DocumentDB .NET Core SDK ist noch nicht mit UWP-Apps (Universelle Windows-Plattform) kompatibel. Senden Sie eine E-Mail an [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com), um Informationen zum .NET Core SDK zu erhalten, mit dem UWP-Apps unterstützt werden.

### <a name="a-name132132"></a><a name="1.3.2"/>1.3.2

*   Unterstützung von .NET Standard 1.5 als eines der Ziel-Frameworks.

### <a name="a-name131131"></a><a name="1.3.1"/>1.3.1

*   Es wurde ein Problem mit Auswirkung auf x64-Computer behoben, die die SSE4-Anweisung nicht unterstützen und beim Ausführen von DocumentDB-Abfragen SEHException auslösen.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0

*   Es wurde Unterstützung für das Feature „Anforderungseinheiten pro Minute“ (RU/m) hinzugefügt.
*   Es wurde Unterstützung für eine neue Konsistenzebene mit dem Namen „ConsistentPrefix“ hinzugefügt.
*   Es wurde Unterstützung für Abfragemetriken für einzelne Partitionen hinzugefügt.
*   Es wurde Unterstützung für die Größenbeschränkung des Fortsetzungstoken für Abfragen hinzugefügt.
*   Es wurde Unterstützung für eine ausführlichere Ablaufverfolgung für Anforderungsfehler hinzugefügt.
*   Es wurden einige Leistungsverbesserungen im SDK vorgenommen.

### <a name="a-name122122"></a><a name="1.2.2"/>1.2.2

* Problem behoben, bei dem der in „FeedOptions“ für aggregierte Abfragen angegebene „PartitionKey“-Wert ignoriert wurde.
* Problem bei der transparenten Durchführung der Partitionsverwaltung im Verlauf einer partitionsübergreifenden Ausführung von ORDER BY-Abfragen behoben.

### <a name="a-name121121"></a><a name="1.2.1"/>1.2.1

* Es wurde ein Fehler behoben, der Deadlocks in einigen der asynchronen APIs verursacht hat, wenn diese im ASP.NET-Kontext verwendet wurden.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0

* Es wurden Fehler behoben, um das SDK für automatische Failover unter bestimmten Bedingungen stabiler zu machen.

### <a name="a-name112112"></a><a name="1.1.2"/>1.1.2

* Behebung eines Problems, das bisweilen eine WebException verursacht: Der Remotename konnte nicht aufgelöst werden.
* Unterstützung zum direkten Lesen eines eingegebenen Dokuments durch Hinzufügen neuer Überladungen zur ReadDocumentAsync-API.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1

* LINQ-Unterstützung für Aggregationsabfragen (COUNT, MIN, MAX, SUM und AVG) wurde hinzugefügt.
* Behebung eines Problems mit einem Arbeitsspeicherverlust für das ConnectionPolicy-Objekt, das durch die Verwendung des Ereignishandlers verursacht wurde.
* Behebung eines Problems, bei dem UpsertAttachmentAsync nicht funktionierte, wenn ETag verwendet wurde.
* Behebung eines Problems, bei dem die partitionsübergreifende Fortsetzung einer order-by-Abfrage nicht funktionierte, wenn ein Zeichenfolgenfeld sortiert wurde.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0

* Unterstützung für Aggregationsabfragen (COUNT, MIN, MAX, SUM und AVG) wurde hinzugefügt. Siehe [Aggregationsunterstützung](documentdb-sql-query.md#Aggregates).
* Minimaler Durchsatz für partitionierte Sammlungen wurde von 10.100 RU/s auf 2.500 RU/s gesenkt.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0

Das DocumentDB .NET Core SDK ermöglicht es Ihnen, schnelle, plattformübergreifende [ASP.NET Core](https://www.asp.net/core)- und [.NET Core](https://www.microsoft.com/net/core#windows)-Apps für die Ausführung unter Windows, Mac und Linux zu erstellen. Die neueste Version des DocumentDB .NET Core SDK ist vollständig mit [Xamarin](https://www.xamarin.com) kompatibel. Es kann verwendet werden, um Anwendungen für iOS, Android und Mono (Linux) zu erstellen.  

### <a name="a-name010-preview010-preview"></a><a name="0.1.0-preview"/>0.1.0-preview

Das DocumentDB .NET Core Preview SDK ermöglicht es Ihnen, schnelle, plattformübergreifende [ASP.NET Core](https://www.asp.net/core)- und [.NET Core](https://www.microsoft.com/net/core#windows)-Apps für die Ausführung unter Windows, Mac und Linux zu erstellen.

Das DocumentDB .NET Core Preview SDK weist Funktionsparität mit der neuesten Version des [DocumentDB .NET SDK](documentdb-sdk-dotnet.md) auf und unterstützt Folgendes:
* Alle [Verbindungsmodi](performance-tips.md#networking): Gatewaymodus, TCP direkt und HTTPs direkt. 
* Alle [Konsistenzebenen](consistency-levels.md): „stark“, „Sitzung“, „begrenzte Veraltung“ und „letztlich“.
* [Partitionierte Sammlungen](partition-data.md). 
* [Datenbankkonten in mehreren Regionen und Georeplikation](distribute-data-globally.md).

Wenn Sie Fragen zu diesem SDK haben, stellen Sie diese bei [StackOverflow](http://stackoverflow.com/questions/tagged/azure-documentdb), oder melden Sie im [GitHub-Repository](https://github.com/Azure/azure-documentdb-dotnet/issues) ein Problem. 

## <a name="release--retirement-dates"></a>Veröffentlichungs- und Deaktivierungstermine

| Version | Herausgabedatum | Deaktivierungstermine |
| --- | --- | --- |
| [1.3.2](#1.3.2) |12. Juni 2017 |--- |
| [1.3.1](#1.3.1) |23. Mai 2017 |--- |
| [1.3.0](#1.3.0) |10. Mai 2017 |--- |
| [1.2.2](#1.2.2) |19. April 2017 |--- |
| [1.2.1](#1.2.1) |29. März 2017 |--- |
| [1.2.0](#1.2.0) |25. März 2017 |--- |
| [1.1.2](#1.1.2) |20. März 2017 |--- |
| [1.1.1](#1.1.1) |14. März 2017 |--- |
| [1.1.0](#1.1.0) |16. Februar 2017 |--- |
| [1.0.0](#1.0.0) |21. Dezember 2016 |--- |
| [0.1.0-preview](#0.1.0-preview) |15. November 2016 |31. Dezember 2016 |

## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zu Cosmos DB finden Sie auf der Seite zum Dienst [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). 


