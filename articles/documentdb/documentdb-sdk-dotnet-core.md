---
title: Azure DocumentDB .NET Core-API, -SDK und -Ressourcen | Microsoft-Dokumentation
description: "Wichtige Informationen zu .NET Core-API und .NET Core SDK einschließlich Terminen für Veröffentlichung und Außerbetriebnahme sowie Änderungen an den einzelnen Versionen des DocumentDB .NET Core SDK."
services: documentdb
documentationcenter: .net
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: f899b314-26ac-4ddb-86b2-bfdf05c2abf2
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/29/2017
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: 24b575a3c32c9df963f889954abd152ada4f3102
ms.lasthandoff: 03/30/2017


---
# <a name="documentdb-net-core-sdk-release-notes-and-resources"></a>DocumentDB .NET Core-SDK: Versionshinweise und Ressourcen
> [!div class="op_single_selector"]
> * [.NET](documentdb-sdk-dotnet.md)
> * [.NET Core](documentdb-sdk-dotnet-core.md)
> * [Node.js](documentdb-sdk-node.md)
> * [Java](documentdb-sdk-java.md)
> * [Python](documentdb-sdk-python.md)
> * [REST](https://docs.microsoft.com/en-us/rest/api/documentdb/)
> * [REST-Ressourcenanbieter](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td>**SDK-Download**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/)</td></tr>

<tr><td>**API-Dokumentation**</td><td>[.NET API-Referenzdokumentation](https://msdn.microsoft.com/library/azure/dn948556.aspx)</td></tr>

<tr><td>**Beispiele**</td><td>[.NET-Codebeispiele](documentdb-dotnet-samples.md)</td></tr>

<tr><td>**Erste Schritte**</td><td>[Erste Schritte mit dem DocumentDB .NET Core SDK](documentdb-dotnetcore-get-started.md)</td></tr>

<tr><td>**Web-App-Tutorial**</td><td>[Webanwendungsentwicklung mit DocumentDB](documentdb-dotnet-application.md)</td></tr>

<tr><td>**Aktuelles unterstütztes Framework**</td><td>[.NET Standard 1.6](https://www.nuget.org/packages/NETStandard.Library)</td></tr>
</table></br>

## <a name="release-notes"></a>Versionsinformationen

Das DocumentDB .NET Core SDK weist Funktionsparität mit der neuesten Version des [DocumentDB .NET SDK](documentdb-sdk-dotnet.md) auf.

> [!NOTE] 
> Das DocumentDB .NET Core SDK ist noch nicht mit UWP-Apps (Universelle Windows-Plattform) kompatibel. Senden Sie eine E-Mail an [askdocdb@microsoft.com](mailto:askdocdb@microsoft.com), um Informationen zum .NET Core SDK zu erhalten, mit dem UWP-Apps unterstützt werden.

### <a name="a-name121121httpswwwnugetorgpackagesmicrosoftazuredocumentdbcore121"></a><a name="1.2.1"/>[1.2.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/1.2.1)

* Es wurde ein Fehler behoben, der Deadlocks in einigen der asynchronen APIs verursacht hat, wenn diese im ASP.NET-Kontext verwendet wurden.

### <a name="a-name120120httpswwwnugetorgpackagesmicrosoftazuredocumentdbcore120"></a><a name="1.2.0"/>[1.2.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/1.2.0)

* Es wurden Fehler behoben, um das SDK für automatische Failover unter bestimmten Bedingungen stabiler zu machen.

### <a name="a-name112112httpswwwnugetorgpackagesmicrosoftazuredocumentdbcore112"></a><a name="1.1.2"/>[1.1.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/1.1.2)

* Behebung eines Problems, das bisweilen eine WebException verursacht: Der Remotename konnte nicht aufgelöst werden.
* Unterstützung zum direkten Lesen eines eingegebenen Dokuments durch Hinzufügen neuer Überladungen zur ReadDocumentAsync-API.

### <a name="a-name111111httpswwwnugetorgpackagesmicrosoftazuredocumentdbcore111"></a><a name="1.1.1"/>[1.1.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/1.1.1)

* LINQ-Unterstützung für Aggregationsabfragen (COUNT, MIN, MAX, SUM und AVG) wurde hinzugefügt.
* Behebung eines Problems mit einem Arbeitsspeicherverlust für das ConnectionPolicy-Objekt, das durch die Verwendung des Ereignishandlers verursacht wurde.
* Behebung eines Problems, bei dem UpsertAttachmentAsync nicht funktionierte, wenn ETag verwendet wurde.
* Behebung eines Problems, bei dem die partitionsübergreifende Fortsetzung einer order-by-Abfrage nicht funktionierte, wenn ein Zeichenfolgenfeld sortiert wurde.

### <a name="a-name110110httpswwwnugetorgpackagesmicrosoftazuredocumentdbcore110"></a><a name="1.1.0"/>[1.1.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/1.1.0)

* Unterstützung für Aggregationsabfragen (COUNT, MIN, MAX, SUM und AVG) wurde hinzugefügt. Siehe [Aggregationsunterstützung](documentdb-sql-query.md#Aggregates).
* Minimaler Durchsatz für partitionierte Sammlungen wurde von 10.100 RU/s auf 2.500 RU/s gesenkt.

### <a name="a-name100100httpswwwnugetorgpackagesmicrosoftazuredocumentdbcore100"></a><a name="1.0.0"/>[1.0.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/1.0.0)

Das DocumentDB .NET Core SDK ermöglicht es Ihnen, schnelle, plattformübergreifende [ASP.NET Core](https://www.asp.net/core)- und [.NET Core](https://www.microsoft.com/net/core#windows)-Apps für die Ausführung unter Windows, Mac und Linux zu erstellen. Die neueste Version des DocumentDB .NET Core SDK ist vollständig mit [Xamarin](https://www.xamarin.com) kompatibel. Es kann verwendet werden, um Anwendungen für iOS, Android und Mono (Linux) zu erstellen.  

### <a name="a-name010-preview010-previewhttpswwwnugetorgpackagesmicrosoftazuredocumentdbcore010-preview"></a><a name="0.1.0-preview"/>[0.1.0-preview](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/0.1.0-preview)

Das DocumentDB .NET Core Preview SDK ermöglicht es Ihnen, schnelle, plattformübergreifende [ASP.NET Core](https://www.asp.net/core)- und [.NET Core](https://www.microsoft.com/net/core#windows)-Apps für die Ausführung unter Windows, Mac und Linux zu erstellen.

Das DocumentDB .NET Core Preview SDK weist Funktionsparität mit der neuesten Version des [DocumentDB .NET SDK](documentdb-sdk-dotnet.md) auf und unterstützt Folgendes:
* Alle [Verbindungsmodi](documentdb-performance-tips.md#networking): Gatewaymodus, TCP direkt und HTTPs direkt. 
* Alle [Konsistenzebenen](documentdb-consistency-levels.md): „stark“, „Sitzung“, „begrenzte Veraltung“ und „letztlich“.
* [Partitionierte Sammlungen](documentdb-partition-data.md). 
* [Datenbankkonten in mehreren Regionen und Georeplikation](documentdb-distribute-data-globally.md).

Wenn Sie Fragen zu diesem SDK haben, stellen Sie diese bei [StackOverflow](http://stackoverflow.com/questions/tagged/azure-documentdb), oder melden Sie im [GitHub-Repository](https://github.com/Azure/azure-documentdb-dotnet/issues) ein Problem. 

## <a name="release--retirement-dates"></a>Veröffentlichungs- und Deaktivierungstermine

| Version | Herausgabedatum | Deaktivierungstermine |
| --- | --- | --- |
| [1.2.1](#1.2.1) |29. März 2017 |--- |
| [1.2.0](#1.2.0) |25. März 2017 |--- |
| [1.1.2](#1.1.2) |20. März 2017 |--- |
| [1.1.1](#1.1.1) |14. März 2017 |--- |
| [1.1.0](#1.1.0) |16. Februar 2017 |--- |
| [1.0.0](#1.0.0) |21. Dezember 2016 |--- |
| [0.1.0-preview](#0.1.0-preview) |15. November 2016 |31. Dezember 2016 |

## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zu DocumentDB finden Sie auf der Seite zum Dienst [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) . 


