<properties 
	pageTitle="DocumentDB – .NET-API und -SDK | Microsoft Azure" 
	description="Wichtige Informationen zu .NET-API und -SDK einschließlich Veröffentlichungstermine, Deaktivierungstermine und Änderungen an den einzelnen Versionen des DocumentDB-.NET-SDK." 
	services="documentdb" 
	documentationCenter=".net" 
	authors="rnagpal" 
	manager="jhubbard" 
	editor="cgronlun"/>

<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="dotnet" 
	ms.topic="article" 
	ms.date="09/01/2016" 
	ms.author="rnagpal"/>

# DocumentDB-APIs und -SDKs 

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [REST](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## DocumentDB – .NET-API und -SDK

<table>
<tr><td>**SDK-Download**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)</td></tr>
<tr><td>**API-Dokumentation**</td><td>[.NET-API-Referenzdokumentation](https://msdn.microsoft.com/library/azure/dn948556.aspx)</td></tr>
<tr><td>**Beispiele**</td><td>[.NET-Codebeispiele](documentdb-dotnet-samples.md)</td></tr>
<tr><td>**Erste Schritte**</td><td>[Erste Schritte mit dem DocumentDB .NET SDK](documentdb-get-started.md)</td></tr>
<tr><td>**Web-App-Tutorial**</td><td>[Webanwendungsentwicklung mit DocumentDB](documentdb-dotnet-application.md)</td></tr>
<tr><td>**Aktuelles unterstütztes Framework**</td><td>[Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</td></tr>
</table></br>

## Versionsinformationen

> [AZURE.IMPORTANT] Beim Abfragen partitionierter Sammlungen erhalten Sie ggf. die Ausnahme „System.NotSupportedException“. Um diesen Fehler zu vermeiden, deaktivieren Sie die auf der Registerkarte „Build“ im Fenster „Eigenschaften“ Ihres Projekts die Option „32-Bit bevorzugen“.

### <a name="1.9.5"/>[1\.9.5](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.5)

  - Es wurde ein Problem behoben, dass die folgende Ausnahme vom Typ „Nicht gefunden“ („NotFoundException“) auslöste: Die Lesesitzung für das eingegebene Sitzungstoken ist nicht verfügbar. Diese Ausnahme trat in einigen Fällen bei Abfragen für die Leseregion eines geografisch verteilten Kontos auf.
  - Es wurde die ResponseStream-Eigenschaft in der ResourceResponse-Klasse verfügbar gemacht, die einen direkten Zugriff auf den zugrunde liegenden Datenstrom aus einer Antwort ermöglicht.

### <a name="1.9.4"/>[1\.9.4](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.4)

  - Die Klassen „ResourceResponse“, „FeedResponse“, „StoredProcedureResponse“ und „MediaResponse“ wurden geändert, um die entsprechende öffentliche Schnittstelle zu implementieren, damit sie für die testgetriebene Bereitstellung (Test Driven Deployment, TDD) simuliert werden können.
  - Ein Problem wurde behoben, das einen falsch formatierten Partitionsschlüssel-Header verursacht hat, wenn für die Datenserialisierung ein benutzerdefiniertes JsonSerializerSettings-Objekt verwendet wird.

### <a name="1.9.3"/>[1\.9.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.3)

  - Es wurde ein Fehler behoben, der dazu führte, dass Abfragen mit langer Ausführungszeit mit folgendem Fehler abgebrochen wurden: Autorisierungstoken ist zum aktuellen Zeitpunkt ungültig.
  - Es wurde ein Fehler behoben, durch den die ursprüngliche SqlParameterCollection aus partitionsübergreifenden top/order-by-Abfragen entfernt wurde.

### <a name="1.9.2"/>[1\.9.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.2)

  - Unterstützung für parallele Abfragen für partitionierte Sammlungen hinzugefügt.
  - Unterstützung für partitionsübergreifende ORDER BY-und TOP-Abfragen für partitionierte Sammlungen hinzugefügt.
  - Fehlende Verweise auf DocumentDB.Spatial.Sql.dll und Microsoft.Azure.Documents.ServiceInterop.dll behoben, die beim Referenzieren eines DocumentDB-Projekts mit einem Verweis auf das DocumentDB NuGet-Paket erforderlich sind.
  - Ein Fehler in Bezug auf die Verwendung verschiedener Parametertypen in benutzerdefinierten Funktionen in LINQ wurde behoben.
  - Fehler bei global replizierten Konten behoben, bei dem Upsert-Aufrufe an Leseregionen anstatt Schreibregionen geleitet wurden.
  - Der „IDocumentClient“-Schnittstelle fehlende Methoden hinzugefügt:
      - „UpsertAttachmentAsync“-Methode, die „mediaStream“ und „options“ als Parameter akzeptiert
      - „CreateAttachmentAsync“-Methode, die „options“ als Parameter akzeptiert
      - „CreateOfferQuery“-Methode, die „querySpec“ als Parameter akzeptiert
  - Nicht versiegelte öffentliche Klassen, die in der IDocumentClient-Schnittstelle verfügbar gemacht werden.

### <a name="1.8.0"/>[1\.8.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.8.0)
  - Unterstützung für Datenbankkonten in mehreren Regionen hinzugefügt.
  - Unterstützung für Wiederholungsversuche für gedrosselte Anforderungen hinzugefügt. Benutzer kann die Anzahl von Wiederholungsversuchen und die maximale Wartezeit durch Konfigurieren der ConnectionPolicy.RetryOptions-Eigenschaft anpassen.
  - Neue IDocumentClient-Schnittstelle hinzugefügt, die die Signaturen aller DocumentClient-Eigenschaften und -Methoden definiert. Im Rahmen dieser Änderung wurden auch die Erweiterungsmethoden zum Erstellen von IQueryable und IOrderedQueryable in Methoden der DocumentClient-Klasse selbst geändert.
  - Konfigurationsoption zum Festlegen des ServicePoint.ConnectionLimit-Werts für einen bestimmten DocumentDB-Endpunkt-URI hinzugefügt. Verwenden Sie ConnectionPolicy.MaxConnectionLimit zum Ändern des Standardwerts, der auf 50 festgelegt ist.
  - IPartitionResolver und die zugehörige Implementierung wurden als „veraltet“ markiert. Unterstützung für IPartitionResolver ist jetzt veraltet. Um mehr Speicher und höheren Durchsatz zu erzielen, empfiehlt sich die Verwendung von partitionierten Sammlungen.


### <a name="1.7.1"/>[1\.7.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.1)
  - URI basierend auf der „ExecuteStoredProcedureAsync“-Methode eine Überladung hinzugefügt, die „RequestOptions“ als Parameter akzeptiert.
  
### <a name="1.7.0"/>[1\.7.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.0)
  - Unterstützung einer Gültigkeitsdauer (TTL) für Dokumente hinzugefügt.

### <a name="1.6.3"/>[1\.6.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.3)
  - Ein Fehler in der Nuget-Paketerstellung von .NET SDK wurde behoben (dient zum Verpacken von .NET SDK als Teil einer Azure Cloud Service-Lösung).
  
### <a name="1.6.2"/>[1\.6.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.2)
  - [Partitionierte Sammlungen](documentdb-partition-data.md) und [benutzerdefinierte Leistungsstufen](documentdb-performance-levels.md) wurden implementiert.

### <a name="1.5.3"/>[1\.5.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.3)
  - **[Korrigiert]** Die Abfrage des DocumentDB-Endpunkts löst Folgendes aus: „System.Net.Http.HttpRequestException: Fehler beim Kopieren von Inhalt in einen Stream“.

### <a name="1.5.2"/>[1\.5.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.2)
  - Erweiterte LINQ-Unterstützung, einschließlich neuer Operatoren für Paging, bedingte Ausdrücke und Bereichsvergleiche.
    - Take-Operator, um SELECT TOP-Verhalten in LINQ zu ermöglichen
    - CompareTo-Operator, um Vergleiche von Zeichenfolgenbereichen zu ermöglichen
    - Bedingte (?) und zusammengefügte (??) Operatoren
  - **[Korrigiert]** ArgumentOutOfRangeException beim Kombinieren der Modellprojektion mit „Where-In“ in LINQ-Abfragen. [Nr. 81](https://github.com/Azure/azure-documentdb-dotnet/issues/81)

### <a name="1.5.1"/>[1\.5.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.1)
 - **[Korrigiert]** Wenn SELECT nicht der letzte Ausdruck ist, nahm der LINQ-Anbieter keine Projektion an und erzeugte SELECT * falsch. [Nr. 58](https://github.com/Azure/azure-documentdb-dotnet/issues/58)

### <a name="1.5.0"/>[1\.5.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.0)
 - Upsert implementiert, UpsertXXXAsync-Methoden hinzugefügt
 - Leistungsverbesserungen für alle Anforderungen
 - LINQ-Anbieterunterstützung für bedingte, „Coalesce“- und „CompareTo“-Methoden für Zeichenfolgen
 - **[Korrigiert]** LINQ-Anbieter--> „Contains“-Methode in „List“ implementiert, um für „IEnumerable“ und „Array“ dieselbe SQL zu generieren.
 - **[Korrigiert]** „BackoffRetryUtility“ verwendet die gleiche „HttpRequestMessage“ erneut, anstatt bei einer Wiederholung eine neue zu erstellen.
 - **[Veraltet]** „UriFactory.CreateCollection“ --> „UriFactory.CreateDocumentCollection“ sollte stattdessen verwendet werden.
 
### <a name="1.4.1"/>[1\.4.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.1)
 - **[Korrigiert]** Lokalisierungsprobleme bei Verwenden nicht englischer Kulturinformationen wie beispielsweise nl-NL usw.
 
### <a name="1.4.0"/>[1\.4.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.0)
  - ID-basiertes Routing
    - Neues „UriFactory“-Hilfsprogramm zum Unterstützen des Erstellens ID-basierter Ressourcenlinks.
    - Neue Überladungen für „DocumentClient“ zum Aufnehmen des URI.
  - „IsValid()“ und „IsValidDetailed()“ in LINQ für Geodaten hinzugefügt.
  - LINQ-Anbieterunterstützung verbessert.
    - **Math:** Abs, Acos, Asin, Atan, Ceiling, Cos, Exp, Floor, Log, Log10, Pow, Round, Sign, Sin, Sqrt, Tan, Truncate
    - **String:** Concat, Contains, EndsWith, IndexOf, Count, ToLower, TrimStart, Replace, Reverse, TrimEnd, StartsWith, SubString, ToUpper
    - **Array:** Concat, Contains, Count
    - **IN**-Operator

### <a name="1.3.0"/>[1\.3.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.3.0)
  - Unterstützung für das Ändern der Indizierungsrichtlinie hinzugefügt.
    - Neue „ReplaceDocumentCollectionAsync“-Methode in „DocumentClient“
    - Neue IndexTransformationProgress-Eigenschaft in ResourceResponse<T> zum Nachverfolgen des prozentualen Fortschritts von Indexrichtlinienänderungen
    - „DocumentCollection.IndexingPolicy“ jetzt änderbar.
  - Unterstützung für die räumliche Indizierung und Abfrage hinzugefügt.
    - Neuer „Microsoft.Azure.Documents.Spatial“-Namespace für die Serialisierung/Deserialisierung räumlicher Typen wie Punkt und Polygon.
    - Neue „SpatialIndex“-Klasse für die Indizierung von in DocumentDB gespeicherten GeoJSON-Daten.
  - **[Korrigiert]** Falsche SQL-Abfrage aus LINQ-Ausdruck generiert [Nr. 38](https://github.com/Azure/azure-documentdb-net/issues/38)

### <a name="1.2.0"/>[1\.2.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.2.0)
- Abhängigkeit von Newtonsoft.Json v5.0.7
- Änderungen zur Unterstützung von „Order By“
  - LINQ-Anbieterunterstützung für „OrderBy()“ oder „OrderByDescending()“
  - Änderung an „IndexingPolicy“ zur Unterstützung von „Order By“
  
		**NB: Mögliche neue Änderung** 
  
    	Wenn Sie über vorhandenen Code verfügen, der Sammlungen mit einer benutzerdefinierten Indizierungsrichtlinie bereitstellt, muss Ihr vorhandener Code so aktualisiert werden, dass er die neue IndexingPolicy-Klasse unterstützt. Wenn Sie keine benutzerdefinierte Indizierungsrichtlinie haben, betrifft Sie diese Änderung nicht.

### <a name="1.1.0"/>[1\.1.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.1.0)
- Unterstützung für das Partitionieren von Daten mithilfe der neuen Klassen „HashPartitionResolver“ und „RangePartitionResolver“ und von „IPartitionResolver“
- „DataContract“-Serialisierung
- GUID-Unterstützung in LINQ-Anbieter
- UDF-Unterstützung in LINQ

### <a name="1.0.0"/>[1\.0.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.0.0)
- Allgemeine Verfügbarkeit (GA) des SDK

> [AZURE.NOTE]
In der GA-Version wurde im Vergleich zur Preview-Version der Name eines NuGet-Pakets geändert. **Microsoft.Azure.Documents.Client** wurde in **Microsoft.Azure.DocumentDB** geändert. <br/>


### <a name="0.9.x-preview"/>[0\.9.x-preview](https://www.nuget.org/packages/Microsoft.Azure.Documents.Client)
- SDK-Vorschauversionen [Veraltet]

## Veröffentlichungs- und Deaktivierungstermine
Wenn Microsoft ein SDK deaktiviert, werden Sie mindestens **12 Monate** vorher benachrichtigt, um einen reibungslosen Übergang zu einer neueren/unterstützten Version zu gewährleisten.

Neue Features, Funktionen und Optimierungen werden nur dem aktuellen SDK hinzugefügt. Daher wird empfohlen, immer so früh wie möglich auf die neueste SDK-Version zu aktualisieren.

Anforderungen von DocumentDB mithilfe eines deaktivierten SDK werden vom Dienst abgelehnt.

> [AZURE.WARNING]
Alle Versionen des Azure DocumentDB-SDK für .NET vor Version **1.0.0** werden am **29. Februar 2016** deaktiviert.
 
<br/>
 
| Version | Herausgabedatum | Deaktivierungstermine 
| ---	  | ---	         | ---
| [1\.9.5](#1.9.5) | 01. September 2016 |--- 
| [1\.9.4](#1.9.4) | 24. August 2016 |--- 
| [1\.9.3](#1.9.3) | 15. August 2016 |--- 
| [1\.9.2](#1.9.2) | 23. Juli 2016 |--- 
| 1.9.1 | Veraltet |--- 
| 1.9.0 | Veraltet |--- 
| [1\.8.0](#1.8.0) | 14. Juni 2016 |--- 
| [1\.7.1](#1.7.1) | 06. Mai 2016 |--- 
| [1\.7.0](#1.7.0) | 26. April 2016 |--- 
| [1\.6.3](#1.6.3) | 08. April 2016 |--- 
| [1\.6.2](#1.6.2) | 29. März 2016 |--- 
| [1\.5.3](#1.5.3) | 19. Februar 2016 |--- 
| [1\.5.2](#1.5.2) | 14. Dezember 2015 |--- 
| [1\.5.1](#1.5.1) | 23. November 2015 |--- 
| [1\.5.0](#1.5.0) | 05. Oktober 2015 |--- 
| [1\.4.1](#1.4.1) | 25. August 2015 |--- 
| [1\.4.0](#1.4.0) | 13. August 2015 |--- 
| [1\.3.0](#1.3.0) | 05. August 2015 |--- 
| [1\.2.0](#1.2.0) | 06. Juli 2015 |--- 
| [1\.1.0](#1.1.0) | 30. April 2015 |--- 
| [1\.0.0](#1.0.0) | 08. April 2015 |--- 
| [0\.9.3-prelease](#0.9.x-preview) | 12. März 2015 | 29. Februar 2016 
| [0\.9.2-prelease](#0.9.x-preview) | Januar 2015 | 29. Februar 2016 
| [.9.1-prelease](#0.9.x-preview) | 13. Oktober 2014 | 29. Februar 2016 
| [0\.9.0-prelease](#0.9.x-preview) | 21. August 2014 | 29. Februar 2016

## Häufig gestellte Fragen
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## Weitere Informationen

Weitere Informationen zu DocumentDB finden Sie auf der Seite zum Dienst [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/).

<!---HONumber=AcomDC_0907_2016-->