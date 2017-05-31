---
title: Azure DocumentDB .NET-SDK und -Ressourcen | Microsoft-Dokumentation
description: "Wichtige Informationen zu .NET-API und -SDK einschließlich Veröffentlichungstermine, Deaktivierungstermine und Änderungen an den einzelnen Versionen des DocumentDB-.NET-SDK."
services: cosmosdb
documentationcenter: .net
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 8e239217-9085-49f5-b0a7-58d6e6b61949
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/10/2017
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 772af5812941ff12e64665ec4fb505ef7d41462c
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="documentdb-net-sdk-download-and-release-notes"></a>DocumentDB .NET SDK: Download und Anmerkungen zur Version
> [!div class="op_single_selector"]
> * [.NET](documentdb-sdk-dotnet.md)
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

<tr><td>**SDK-Download**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)</td></tr>

<tr><td>**API-Dokumentation**</td><td>[.NET API-Referenzdokumentation](https://msdn.microsoft.com/library/azure/dn948556.aspx)</td></tr>

<tr><td>**Beispiele**</td><td>[.NET-Codebeispiele](documentdb-dotnet-samples.md)</td></tr>

<tr><td>**Erste Schritte**</td><td>[Erste Schritte mit dem DocumentDB .NET SDK](documentdb-get-started.md)</td></tr>

<tr><td>**Web-App-Tutorial**</td><td>[Entwicklung von Webanwendungen mit Azure Cosmos DB](documentdb-dotnet-application.md)</td></tr>

<tr><td>**Aktuelles unterstütztes Framework**</td><td>[Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</td></tr>
</table></br>

## <a name="release-notes"></a>Versionshinweise

### <a name="a-name11401140"></a><a name="1.14.0"/>1.14.0
*    Es wurde Unterstützung für das Feature „Anforderungseinheiten pro Minute“ (RU/m) hinzugefügt.
*    Es wurde Unterstützung für eine neue Konsistenzebene mit dem Namen „ConsistentPrefix“ hinzugefügt.
*    Es wurde Unterstützung für Abfragemetriken für einzelne Partitionen hinzugefügt.
*    Es wurde Unterstützung für die Größenbeschränkung des Fortsetzungstoken für Abfragen hinzugefügt.
*    Es wurde Unterstützung für eine ausführlichere Ablaufverfolgung für Anforderungsfehler hinzugefügt.
*    Es wurden einige Leistungsverbesserungen im SDK vorgenommen.

### <a name="a-name11341134"></a><a name="1.13.4"/>1.13.4
* Funktionell identisch mit 1.13.4. Es wurden einige interne Änderungen vorgenommen.

### <a name="a-name11331133"></a><a name="1.13.3"/>1.13.3
* Funktionell identisch mit 1.13.2. Es wurden einige interne Änderungen vorgenommen.

### <a name="a-name11321132"></a><a name="1.13.2"/>1.13.2
* Problem behoben, bei dem der in „FeedOptions“ für aggregierte Abfragen angegebene „PartitionKey“-Wert ignoriert wurde.
* Problem bei der transparenten Durchführung der Partitionsverwaltung im Verlauf einer partitionsübergreifenden Ausführung von ORDER BY-Abfragen behoben.

### <a name="a-name11311131"></a><a name="1.13.1"/>1.13.1
* Es wurde ein Fehler behoben, der Deadlocks in einigen der asynchronen APIs verursacht hat, wenn diese im ASP.NET-Kontext verwendet wurden.

### <a name="a-name11301130"></a><a name="1.13.0"/>1.13.0
* Es wurden Fehler behoben, um das SDK für automatische Failover unter bestimmten Bedingungen stabiler zu machen.

### <a name="a-name11221122"></a><a name="1.12.2"/>1.12.2
* Behebung eines Problems, das bisweilen eine WebException verursacht: Der Remotename konnte nicht aufgelöst werden.
* Unterstützung zum direkten Lesen eines eingegebenen Dokuments durch Hinzufügen neuer Überladungen zur ReadDocumentAsync-API.

### <a name="a-name11211121"></a><a name="1.12.1"/>1.12.1
* LINQ-Unterstützung für Aggregationsabfragen (COUNT, MIN, MAX, SUM und AVG) wurde hinzugefügt.
* Behebung eines Problems mit einem Arbeitsspeicherverlust für das ConnectionPolicy-Objekt, das durch die Verwendung des Ereignishandlers verursacht wurde.
* Behebung eines Problems, bei dem UpsertAttachmentAsync nicht funktionierte, wenn ETag verwendet wurde.
* Behebung eines Problems, bei dem die partitionsübergreifende Fortsetzung einer order-by-Abfrage nicht funktionierte, wenn ein Zeichenfolgenfeld sortiert wurde.

### <a name="a-name11201120"></a><a name="1.12.0"/>1.12.0
* Unterstützung für Aggregationsabfragen (COUNT, MIN, MAX, SUM und AVG) wurde hinzugefügt. Siehe [Aggregationsunterstützung](documentdb-sql-query.md#Aggregates).
* Minimaler Durchsatz für partitionierte Sammlungen wurde von 10.100 RU/s auf 2.500 RU/s gesenkt.

### <a name="a-name11141114"></a><a name="1.11.4"/>1.11.4
* Behebung eines Problems, bei dem einige der partitionsübergreifenden Abfragen im 32-Bit-Hostprozess fehlschlugen.
* Behebung eines Problems, bei dem der Sitzungscontainer nicht mit dem Token für fehlgeschlagene Anforderungen im Gateway-Modus aktualisiert wurde.
* Behebung eines Problems, bei dem eine Abfrage mit UDF-Aufrufen in der Projektion in einigen Fällen fehlschlug.
* Clientseitige Leistungskorrekturen zur Erhöhung des Lese- und Schreibdurchsatzes der Anforderungen.

### <a name="a-name11131113"></a><a name="1.11.3"/>1.11.3
* Behebung eines Problems, bei dem der Sitzungscontainer nicht mit dem Token für fehlgeschlagene Anforderungen im Gateway-Modus aktualisiert wurde.
* Unterstützung für die Verwendung des SDK in einem 32-Bit-Hostprozess wurde hinzugefügt. Hinweis: Bei Verwendung von partitionsübergreifenden Abfragen wird die 64-Bit-Hostverarbeitung empfohlen, um eine bessere Leistung zu erzielen.
* Verbesserte Leistung für Szenarien im Zusammenhang mit Abfragen mit einer großen Anzahl von Partitionsschlüsselwerten in einem IN-Ausdruck.
* Verschiedene Ressourcenkontingentstatistiken in ResourceResponse für Leseanforderungen von Dokumentsammlungen wurden aufgefüllt, wenn die Anforderungsoption „PopulateQuotaInfo“ festgelegt ist.

### <a name="a-name11111111"></a><a name="1.11.1"/>1.11.1
* In 1.11.0 wurden kleinere Probleme mit der Leistung der CreateDocumentCollectionIfNotExistsAsync-API behoben.
* Behebung von Leistungsproblemen im SDK für Szenarien mit zahlreichen gleichzeitigen Anforderungen.

### <a name="a-name11101110"></a><a name="1.11.0"/>1.11.0
* Unterstützung für neue Klassen und Methoden zum Verarbeiten des [Änderungsfeeds](documentdb-change-feed.md) von Dokumenten in einer Sammlung.
* Unterstützung für die partitionsübergreifende Abfragefortsetzung und einige Leistungsverbesserungen für partitionsübergreifende Abfragen.
* Die Methoden CreateDatabaseIfNotExistsAsync und CreateDocumentCollectionIfNotExistsAsync wurden hinzugefügt.
* LINQ-Unterstützung für Systemfunktionen: IsDefined, IsNull und IsPrimitive.
* Behebung des Problems mit der automatischen Platzierung der Assemblys „Microsoft.Azure.Documents.ServiceInterop.dll“ und „DocumentDB.Spatial.Sql.dll“ im Ordner „bin“ der Anwendung bei Verwendung des NuGet-Pakets mit Projekten, die project.json-Tools enthalten.
* Unterstützung für das Ausgeben clientseitiger ETW-Ablaufverfolgungen, die in Debugszenarien hilfreich sein können.

### <a name="a-name11001100"></a><a name="1.10.0"/>1.10.0
* Unterstützung direkter Verbindungen für partitionierte Sammlungen hinzugefügt.
* Verbesserte Leistung für die Konsistenzebene „Begrenzte Veraltung (Bounded Staleness)“.
* Polygon- und LineString-Datentypen beim Angeben der Sammlungsindizierungsrichtlinie für räumliche Geofencing-Abfragen hinzugefügt.
* LINQ-Unterstützung für StringEnumConverter, IsoDateTimeConverter und UnixDateTimeConverter beim Übersetzen von Prädikaten hinzugefügt.
* Verschiedene Fehlerbehebungen im SDK.

### <a name="a-name195195"></a><a name="1.9.5"/>1.9.5
* Es wurde ein Problem behoben, dass die folgende Ausnahme vom Typ „Nicht gefunden“ („NotFoundException“) auslöste: Die Lesesitzung für das eingegebene Sitzungstoken ist nicht verfügbar. Diese Ausnahme trat in einigen Fällen bei Abfragen für die Leseregion eines geografisch verteilten Kontos auf.
* Es wurde die ResponseStream-Eigenschaft in der ResourceResponse-Klasse verfügbar gemacht, die einen direkten Zugriff auf den zugrunde liegenden Datenstrom aus einer Antwort ermöglicht.

### <a name="a-name194194"></a><a name="1.9.4"/>1.9.4
* Die Klassen „ResourceResponse“, „FeedResponse“, „StoredProcedureResponse“ und „MediaResponse“ wurden geändert, um die entsprechende öffentliche Schnittstelle zu implementieren, damit sie für die testgetriebene Bereitstellung (Test Driven Deployment, TDD) simuliert werden können.
* Ein Problem wurde behoben, das einen falsch formatierten Partitionsschlüssel-Header verursacht hat, wenn für die Datenserialisierung ein benutzerdefiniertes JsonSerializerSettings-Objekt verwendet wird.

### <a name="a-name193193"></a><a name="1.9.3"/>1.9.3
* Es wurde ein Fehler behoben, der dazu führte, dass Abfragen mit langer Ausführungszeit mit folgendem Fehler abgebrochen wurden: Autorisierungstoken ist zum aktuellen Zeitpunkt ungültig.
* Es wurde ein Fehler behoben, durch den die ursprüngliche SqlParameterCollection aus partitionsübergreifenden top/order-by-Abfragen entfernt wurde.

### <a name="a-name192192"></a><a name="1.9.2"/>1.9.2
* Unterstützung für parallele Abfragen für partitionierte Sammlungen hinzugefügt.
* Unterstützung für partitionsübergreifende ORDER BY-und TOP-Abfragen für partitionierte Sammlungen hinzugefügt.
* Fehlende Verweise auf DocumentDB.Spatial.Sql.dll und Microsoft.Azure.Documents.ServiceInterop.dll behoben, die beim Referenzieren eines DocumentDB-Projekts mit einem Verweis auf das DocumentDB NuGet-Paket erforderlich sind.
* Ein Fehler in Bezug auf die Verwendung verschiedener Parametertypen in benutzerdefinierten Funktionen in LINQ wurde behoben. 
* Fehler bei global replizierten Konten behoben, bei dem Upsert-Aufrufe an Leseregionen anstatt Schreibregionen geleitet wurden.
* Der „IDocumentClient“-Schnittstelle fehlende Methoden hinzugefügt: 
  * „UpsertAttachmentAsync“-Methode, die „mediaStream“ und „options“ als Parameter akzeptiert
  * „CreateAttachmentAsync“-Methode, die „options“ als Parameter akzeptiert
  * „CreateOfferQuery“-Methode, die „querySpec“ als Parameter akzeptiert
* Nicht versiegelte öffentliche Klassen, die in der IDocumentClient-Schnittstelle verfügbar gemacht werden.

### <a name="a-name180180"></a><a name="1.8.0"/>1.8.0
* Unterstützung für Datenbankkonten in mehreren Regionen hinzugefügt.
* Unterstützung für Wiederholungsversuche für gedrosselte Anforderungen hinzugefügt.  Benutzer kann die Anzahl von Wiederholungsversuchen und die maximale Wartezeit durch Konfigurieren der ConnectionPolicy.RetryOptions-Eigenschaft anpassen.
* Neue IDocumentClient-Schnittstelle hinzugefügt, die die Signaturen aller DocumentClient-Eigenschaften und -Methoden definiert.  Im Rahmen dieser Änderung wurden auch die Erweiterungsmethoden zum Erstellen von IQueryable und IOrderedQueryable in Methoden der DocumentClient-Klasse selbst geändert.
* Konfigurationsoption zum Festlegen des ServicePoint.ConnectionLimit-Werts für einen bestimmten DocumentDB-Endpunkt-URI hinzugefügt.  Verwenden Sie ConnectionPolicy.MaxConnectionLimit zum Ändern des Standardwerts, der auf 50 festgelegt ist.
* IPartitionResolver und die zugehörige Implementierung wurden als „veraltet“ markiert.  Unterstützung für IPartitionResolver ist jetzt veraltet. Um mehr Speicher und höheren Durchsatz zu erzielen, empfiehlt sich die Verwendung von partitionierten Sammlungen.

### <a name="a-name171171"></a><a name="1.7.1"/>1.7.1
* URI basierend auf der „ExecuteStoredProcedureAsync“-Methode eine Überladung hinzugefügt, die „RequestOptions“ als Parameter akzeptiert.

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
* Unterstützung einer Gültigkeitsdauer (TTL) für Dokumente hinzugefügt.

### <a name="a-name163163"></a><a name="1.6.3"/>1.6.3
* Ein Fehler in der Nuget-Paketerstellung von .NET SDK wurde behoben (dient zum Verpacken von .NET SDK als Teil einer Azure Cloud Service-Lösung).

### <a name="a-name162162"></a><a name="1.6.2"/>1.6.2
* [Partitionierte Sammlungen](documentdb-partition-data.md) und [benutzerdefinierte Leistungsstufen](documentdb-performance-levels.md) implementiert. 

### <a name="a-name153153"></a><a name="1.5.3"/>1.5.3
* **[Korrigiert]** Die Abfrage des DocumentDB-Endpunkts löst Folgendes aus: „System.Net.Http.HttpRequestException: Fehler beim Kopieren von Inhalt in einen Stream“.

### <a name="a-name152152"></a><a name="1.5.2"/>1.5.2
* Erweiterte LINQ-Unterstützung, einschließlich neuer Operatoren für Paging, bedingte Ausdrücke und Bereichsvergleiche.
  * Take-Operator, um SELECT TOP-Verhalten in LINQ zu ermöglichen
  * CompareTo-Operator, um Vergleiche von Zeichenfolgenbereichen zu ermöglichen
  * Bedingte (?) und zusammengefügte (??) Operatoren
* **[Korrigiert]** ArgumentOutOfRangeException beim Kombinieren der Modellprojektion mit „Where-In“ in LINQ-Abfragen.  [Nr. 81](https://github.com/Azure/azure-documentdb-dotnet/issues/81)

### <a name="a-name151151"></a><a name="1.5.1"/>1.5.1
* **[Korrigiert]** Wenn SELECT nicht der letzte Ausdruck ist, nahm der LINQ-Anbieter keine Projektion an und erzeugte SELECT * falsch.  [Nr. 58](https://github.com/Azure/azure-documentdb-dotnet/issues/58)

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0
* Upsert implementiert, UpsertXXXAsync-Methoden hinzugefügt
* Leistungsverbesserungen für alle Anforderungen
* LINQ-Anbieterunterstützung für bedingte, „Coalesce“- und „CompareTo“-Methoden für Zeichenfolgen
* **[Korrigiert]** LINQ-Anbieter--> „Contains“-Methode in „List“ implementiert, um für „IEnumerable“ und „Array“ dieselbe SQL zu generieren.
* **[Korrigiert]** „BackoffRetryUtility“ verwendet die gleiche „HttpRequestMessage“ erneut, anstatt bei einer Wiederholung eine neue zu erstellen.
* **[Veraltet]**„UriFactory.CreateCollection“ --> „UriFactory.CreateDocumentCollection“ sollte stattdessen verwendet werden.

### <a name="a-name141141"></a><a name="1.4.1"/>1.4.1
* **[Korrigiert]** Lokalisierungsprobleme bei Verwenden nicht englischer Kulturinformationen wie beispielsweise nl-NL usw. 

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0
* ID-basiertes Routing
  * Neues „UriFactory“-Hilfsprogramm zum Unterstützen des Erstellens ID-basierter Ressourcenlinks.
  * Neue Überladungen für „DocumentClient“ zum Aufnehmen des URI.
* „IsValid()“ und „IsValidDetailed()“ in LINQ für Geodaten hinzugefügt.
* LINQ-Anbieterunterstützung verbessert.
  * **Math:** Abs, Acos, Asin, Atan, Ceiling, Cos, Exp, Floor, Log, Log10, Pow, Round, Sign, Sin, Sqrt, Tan, Truncate
  * **String:** Concat, Contains, EndsWith, IndexOf, Count, ToLower, TrimStart, Replace, Reverse, TrimEnd, StartsWith, SubString, ToUpper
  * **Array:** Concat, Contains, Count
  * **IN** -Operator

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
* Unterstützung für das Ändern der Indizierungsrichtlinie hinzugefügt.
  * Neue „ReplaceDocumentCollectionAsync“-Methode in „DocumentClient“
  * Neue „IndexTransformationProgress“-Eigenschaft in „ResourceResponse“<T> zum Nachverfolgen des prozentualen Fortschritts von Indexrichtlinienänderungen
  * „DocumentCollection.IndexingPolicy“ jetzt änderbar.
* Unterstützung für die räumliche Indizierung und Abfrage hinzugefügt.
  * Neuer „Microsoft.Azure.Documents.Spatial“-Namespace für die Serialisierung/Deserialisierung räumlicher Typen wie Punkt und Polygon.
  * Neue „SpatialIndex“-Klasse für die Indizierung von in Cosmos DB gespeicherten GeoJSON-Daten.
* **[Korrigiert]** Falsche SQL-Abfrage aus LINQ-Ausdruck generiert [Nr. 38](https://github.com/Azure/azure-documentdb-net/issues/38)

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Abhängigkeit von Newtonsoft.Json v5.0.7 
* Änderungen zur Unterstützung von „Order By“
  
  * LINQ-Anbieterunterstützung für „OrderBy()“ oder „OrderByDescending()“
  * Änderung an „IndexingPolicy“ zur Unterstützung von „Order By“ 
    
    **NB: Mögliche neue Änderung** 
    
    Wenn Sie über vorhandenen Code verfügen, der Sammlungen mit einer benutzerdefinierten Indizierungsrichtlinie bereitstellt, muss Ihr vorhandener Code so aktualisiert werden, dass er die neue IndexingPolicy-Klasse unterstützt. Wenn Sie keine benutzerdefinierte Indizierungsrichtlinie haben, betrifft Sie diese Änderung nicht.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Unterstützung für das Partitionieren von Daten mithilfe der neuen Klassen „HashPartitionResolver“ und „RangePartitionResolver“ und von „IPartitionResolver“
* „DataContract“-Serialisierung
* GUID-Unterstützung in LINQ-Anbieter
* UDF-Unterstützung in LINQ

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* Allgemeine Verfügbarkeit (GA) des SDK

## <a name="release--retirement-dates"></a>Veröffentlichungs- und Deaktivierungstermine
Wenn Microsoft ein SDK deaktiviert, werden Sie mindestens **12 Monate** vorher benachrichtigt, um einen reibungslosen Übergang zu einer neueren/unterstützten Version zu gewährleisten.

Neue Features, Funktionen und Optimierungen werden nur dem aktuellen SDK hinzugefügt. Daher wird empfohlen, immer so früh wie möglich auf die neueste SDK-Version zu aktualisieren. 

Anforderungen an Cosmos DB mithilfe eines deaktivierten SDK werden vom Dienst abgelehnt.

<br/>

| Version | Herausgabedatum | Deaktivierungstermine |
| --- | --- | --- |
| [1.14.0](#1.14.0) |10. Mai 2017 |--- |
| [1.13.4](#1.13.4) |09. Mai 2017 |--- |
| [1.13.3](#1.13.3) |06. Mai 2017 |--- |
| [1.13.2](#1.13.2) |19. April 2017 |--- |
| [1.13.1](#1.13.1) |29. März 2017 |--- |
| [1.13.0](#1.13.0) |24. März 2017 |--- |
| [1.12.2](#1.12.2) |20. März 2017 |--- |
| [1.12.1](#1.12.1) |14. März 2017 |--- |
| [1.12.0](#1.12.0) |15. Februar 2017 |--- |
| [1.11.4](#1.11.4) |6. Februar 2017 |--- |
| [1.11.3](#1.11.3) |26. Januar 2017 |--- |
| [1.11.1](#1.11.1) |21. Dezember 2016 |--- |
| [1.11.0](#1.11.0) |8. Dezember 2016 |--- |
| [1.10.0](#1.10.0) |27. September 2016 |--- |
| [1.9.5](#1.9.5) |1. September 2016 |--- |
| [1.9.4](#1.9.4) |24. August 2016 |--- |
| [1.9.3](#1.9.3) |15. August 2016 |--- |
| [1.9.2](#1.9.2) |23. Juli 2016 |--- |
| [1.8.0](#1.8.0) |14. Juni 2016 |--- |
| [1.7.1](#1.7.1) |6. Mai 2016 |--- |
| [1.7.0](#1.7.0) |26. April 2016 |--- |
| [1.6.3](#1.6.3) |8. April 2016 |--- |
| [1.6.2](#1.6.2) |29. März 2016 |--- |
| [1.5.3](#1.5.3) |19. Februar 2016 |--- |
| [1.5.2](#1.5.2) |14. Dezember 2015 |--- |
| [1.5.1](#1.5.1) |23. November 2015 |--- |
| [1.5.0](#1.5.0) |5 Oktober 2015 |--- |
| [1.4.1](#1.4.1) |25. August 2015 |--- |
| [1.4.0](#1.4.0) |13. August 2015 |--- |
| [1.3.0](#1.3.0) |5. August 2015 |--- |
| [1.2.0](#1.2.0) |6. Juli 2015 |--- |
| [1.1.0](#1.1.0) |30. April 2015 |--- |
| [1.0.0](#1.0.0) |8. April 2015 |--- |


## <a name="faq"></a>Häufig gestellte Fragen
[!INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zu Cosmos DB finden Sie auf der Seite zum Dienst [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/). 


