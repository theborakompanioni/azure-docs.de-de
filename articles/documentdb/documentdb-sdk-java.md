---
title: Azure DocumentDB Java-API, SDK & Ressourcen | Microsoft-Dokumentation
description: "Wichtige Informationen zur Java-API und zum SDK, einschließlich Veröffentlichungstermine, Deaktivierungstermine und Änderungen an den einzelnen Versionen des DocumentDB Java SDK."
services: documentdb
documentationcenter: java
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 7861cadf-2a05-471a-9925-0fec0599351b
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 02/22/2017
ms.author: khdang
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 094729399070a64abc1aa05a9f585a0782142cbf
ms.openlocfilehash: 50f8f6a2d64959a1981f95bfe821a2a79f6cafc6
ms.lasthandoff: 03/07/2017


---
# <a name="documentdb-java-sdk-release-notes-and-resources"></a>DocumentDB Java-SDK: Versionshinweise und Ressourcen
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

<tr><td>**SDK-Download**</td><td>[Maven](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)</td></tr>

<tr><td>**API-Dokumentation**</td><td>[Java-API-Referenzdokumentation](http://azure.github.io/azure-documentdb-java/)</td></tr>

<tr><td>**Am SDK mitwirken**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-java/)</td></tr>

<tr><td>**Erste Schritte**</td><td>[Erste Schritte mit dem Java SDK](documentdb-java-get-started.md)</td></tr>

<tr><td>**Web-App-Tutorial**</td><td>[Webanwendungsentwicklung mit DocumentDB](documentdb-java-application.md)</td></tr>

<tr><td>**Aktuelle unterstützte Laufzeit**</td><td>[JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)</td></tr>
</table></br>

## <a name="release-notes"></a>Versionsinformationen
### <a name="a-name196196httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb196"></a><a name="1.9.6"/>[1.9.6](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.6)
* Ein Fehler in der Abfragemodulkonfiguration wurde behoben, der Ausnahmen bei Abfragen im Gatewaymodus verursachen konnte.
* Eine Anzahl von Fehlern im Sitzungscontainer wurde behoben, die eine Ausnahme „Besitzerressource wurde nicht gefunden“ für Anforderungen direkt nach dem Erstellen der Sammlung verursachen konnten.

### <a name="a-name195195httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb195"></a><a name="1.9.5"/>[1.9.5](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.5)
* Unterstützung für Aggregationsabfragen (COUNT, MIN, MAX, SUM und AVG) wurde hinzugefügt. Siehe [Aggregationsunterstützung](documentdb-sql-query.md#Aggregates).
* Unterstützung für Änderungsfeeds wurde hinzugefügt.
* Unterstützung für Sammlungskontingentinformationen über „RequestOptions.setPopulateQuotaInfo“ wurde hinzugefügt.
* Unterstützung für die Skriptprotokollierung gespeicherter Prozeduren über „RequestOptions.setScriptLoggingEnabled“ wurde hinzugefügt.
* Es wurde ein Fehler behoben, bei dem eine Abfrage im DirectHttps-Modus bei Drosselungsfehlern unter Umständen nicht mehr reagierte.
* Korrektur eines Fehlers im Sitzungskonsistenzmodus.
* Es wurde ein Fehler behoben, der bei hoher Anforderungsrate unter Umständen „NullReferenceException“ in „HttpContext“ auslöste.
* Verbesserte Leistung des DirectHttps-Modus.

### <a name="a-name194194httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb194"></a><a name="1.9.4"/>[1.9.4](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.4)
* Einfache, clientinstanzbasierte Proxyunterstützung mit ConnectionPolicy.setProxy()-API hinzugefügt.
* DocumentClient.close()-API zum ordnungsgemäßen Herunterfahren der DocumentClient-Instanz hinzugefügt.
* Abfrageleistung im direkten Konnektivitätsmodus verbessert, indem der Abfrageplan von der nativen Assembly (und nicht vom Gateway) abgeleitet wird.
* „FAIL_ON_UNKNOWN_PROPERTIES“ auf „false“ festgelegt, damit Benutzer in ihrem POJO „JsonIgnoreProperties“ nicht definieren müssen.
* Protokollierung für die Verwendung von SLF4J umgestaltet.
* Einige andere Fehler im Konsistenzleser behoben.

### <a name="a-name193193httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb193"></a><a name="1.9.3"/>[1.9.3](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.3)
* Fehler in der Verbindungsverwaltung behoben, um Verbindungsverluste im mit direkter Konnektivitätsmodus zu verhindern.
* Fehler in der TOP-Abfrage behoben, der unter Umständen zu einer NullReferenece-Ausnahme führt.
* Leistung durch Verringern der Anzahl von Netzwerkaufrufen für die internen Caches verbessert.
* Problembehandlung für „DocumentClientException“ durch Hinzufügen von Statuscode, Aktivitäts-ID und Anforderungs-URI verbessert.

### <a name="a-name192192httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb192"></a><a name="1.9.2"/>[1.9.2](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.2)
* Problem in der Verbindungsverwaltung behoben, um die Stabilität zu verbessern.

### <a name="a-name191191httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb191"></a><a name="1.9.1"/>[1.9.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.1)
* Unterstützung für BoundedStaleness-Konsistenzebene hinzugefügt.
* Unterstützung direkter Verbindungen für CRUD-Vorgänge für partitionierte Sammlungen hinzugefügt.
* Korrektur eines Fehlers beim Abfragen einer Datenbank mit SQL.
* Korrektur eines Fehlers im Sitzungscache, durch den unter Umständen das Sitzungstoken falsch festgelegt wird.

### <a name="a-name190190httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb190"></a><a name="1.9.0"/>[1.9.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.0)
* Unterstützung für parallele partitionsübergreifende Abfragen hinzugefügt.
* Unterstützung für TOP-/ORDER BY-Abfragen für partitionierte Sammlungen hinzugefügt.
* Unterstützung für starke Konsistenz hinzugefügt.
* Unterstützung für namensbasierte Anforderungen bei Verwendung von Direktverbindungen.
* Korrektur, durch die bei allen Anforderungswiederholungen der gleiche ActivityId-Wert verwendet wird.
* Korrektur eines Fehlers in Verbindung mit dem Sitzungscache bei der Neuerstellung einer Sammlung gleichen Namens.
* Polygon- und LineString-Datentypen beim Angeben der Sammlungsindizierungsrichtlinie für räumliche Geofencing-Abfragen hinzugefügt.
* Behobene Probleme mit Java Doc für Java 1.8.

### <a name="a-name181181httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb181"></a><a name="1.8.1"/>[1.8.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.1)
* Korrektur eines Fehlers in „PartitionKeyDefinitionMap“ zum Zwischenspeichern einzelner Partitionssammlungen, ohne dass zusätzliche Anforderungen zum Abrufen von Partitionsschlüsseln erfolgen.
* Korrektur eines Fehlers, sodass keine Wiederholung erfolgt, wenn ein falscher Partitionsschlüsselwert bereitgestellt wird.

### <a name="a-name180180httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb180"></a><a name="1.8.0"/>[1.8.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.0)
* Unterstützung für Datenbankkonten in mehreren Regionen hinzugefügt.
* Unterstützung für automatische Wiederholungsversuche für gedrosselte Anforderungen hinzugefügt, mit Optionen zum Anpassen der maximalen Anzahl von Wiederholungsversuchen und der maximalen Wartezeit.  Weitere Informationen unter „RetryOptions“ und „ConnectionPolicy.getRetryOptions()“.
* Auf IPartitionResolver basierender benutzerdefinierter Partitionierungscode als „veraltet“ markiert. Verwenden Sie partitionierte Sammlungen, um mehr Speicher und höheren Durchsatz zu erzielen.

### <a name="a-name171171httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb171"></a><a name="1.7.1"/>[1.7.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.1)
* Unterstützung für Wiederholungsrichtlinie für Drosselung hinzugefügt.  

### <a name="a-name170170httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb170"></a><a name="1.7.0"/>[1.7.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.0)
* Unterstützung einer Gültigkeitsdauer (TTL) für Dokumente hinzugefügt.

### <a name="a-name160160httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb160"></a><a name="1.6.0"/>[1.6.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.6.0)
* [Partitionierte Sammlungen](documentdb-partition-data.md) und [benutzerdefinierte Leistungsstufen](documentdb-performance-levels.md) implementiert.

### <a name="a-name151151httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb151"></a><a name="1.5.1"/>[1.5.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.1)
* Fehler in „HashPartitionResolver“ behoben, um Hashwerte in Little-Endian zu generieren und Konsistenz mit den anderen SDKs herzustellen.

### <a name="a-name150150httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb150"></a><a name="1.5.0"/>[1.5.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.0)
* Hinzufügen von Hash- und Bereichspartitionen-Konfliktlösern, um die Freigabe von Anwendungen über mehrere Partitionen zu unterstützen.

### <a name="a-name140140httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb140"></a><a name="1.4.0"/>[1.4.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.4.0)
* Upsert implementiert. Neue UpsertXXX-Methoden hinzugefügt, um das „Upsert“-Feature zu unterstützen.
* ID-basiertes Routing implementiert. Keine öffentliche API-Änderungen, alle Änderungen sind intern.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
* Version ausgelassen, um die Versionsnummer in Einklang mit den anderen SDKs zu bringen.

### <a name="a-name120120httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb120"></a><a name="1.2.0"/>[1.2.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.2.0)
* „GeoSpatial“-Index unterstützt.
* „Id“-Eigenschaft wird für alle Ressourcen überprüft. IDs für Ressourcen dürfen nicht mit einem Leerzeichen enden und keins der folgenden Zeichen enthalten: ?, /, #, \,
* Neue Überschrift „Fortschritt der Indextransformation“ zu „ResourceResponse“ hinzugefügt.

### <a name="a-name110110httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb110"></a><a name="1.1.0"/>[1.1.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.1.0)
* V2-Indizierungsrichtlinie implementiert.

### <a name="a-name100100httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb100"></a><a name="1.0.0"/>[1.0.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.0.0)
* Allgemeine Verfügbarkeit (GA) des SDK

## <a name="release--retirement-dates"></a>Veröffentlichungs- und Deaktivierungstermine
Wenn Microsoft ein SDK deaktiviert, werden Sie mindestens **12 Monate** vorher benachrichtigt, um einen reibungslosen Übergang zu einer neueren/unterstützten Version zu gewährleisten.

Neue Features, Funktionen und Optimierungen werden nur dem aktuellen SDK hinzugefügt. Daher empfiehlt es sich, immer so früh wie möglich auf die neueste SDK-Version zu aktualisieren.

Anforderungen von DocumentDB mithilfe eines deaktivierten SDK werden vom Dienst abgelehnt.

> [!WARNING]
> Alle Versionen des Azure DocumentDB SDK für Java vor Version **1.0.0** werden am **29. Februar 2016** deaktiviert.
> 
> 

<br/>

| Version | Herausgabedatum | Deaktivierungstermine |
| --- | --- | --- |
| [1.9.6](#1.9.6) |21. Februar 2017 |--- |
| [1.9.5](#1.9.5) |31. Januar 2017 |--- |
| [1.9.4](#1.9.4) |24. November 2016 |--- |
| [1.9.3](#1.9.3) |30. Oktober 2016 |--- |
| [1.9.2](#1.9.2) |28. Oktober 2016 |--- |
| [1.9.1](#1.9.1) |26. Oktober 2016 |--- |
| [1.9.0](#1.9.0) |3. Oktober 2016 |--- |
| [1.8.1](#1.8.1) |30. Juni 2016 |--- |
| [1.8.0](#1.8.0) |14. Juni 2016 |--- |
| [1.7.1](#1.7.1) |30. April 2016 |--- |
| [1.7.0](#1.7.0) |27. April 2016 |--- |
| [1.6.0](#1.6.0) |29. März 2016 |--- |
| [1.5.1](#1.5.1) |31. Dezember 2015 |--- |
| [1.5.0](#1.5.0) |4. Dezember 2015 |--- |
| [1.4.0](#1.4.0) |5. Oktober 2015 |--- |
| [1.3.0](#1.3.0) |5. Oktober 2015 |--- |
| [1.2.0](#1.2.0) |5. August 2015 |--- |
| [1.1.0](#1.1.0) |9. Juli 2015 |--- |
| [1.0.1](#1.0.1) |12. Mai 2015 |--- |
| [1.0.0](#1.0.0) |7. April 2015 |--- |
| 0.9.5-prelease |9. März 2015 |29. Februar 2016 |
| 0.9.4-prelease |17. Februar 2015 |29. Februar 2016 |
| 0.9.3-prelease |13. Januar 2015 |29. Februar 2016 |
| 0.9.2-prelease |19. Dezember 2014 |29. Februar 2016 |
| 0.9.1-prelease |19. Dezember 2014 |29. Februar 2016 |
| 0.9.0-prelease |10. Dezember 2014 |29. Februar 2016 |

## <a name="faq"></a>Häufig gestellte Fragen
[!INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zu DocumentDB finden Sie auf der Seite zum Dienst [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) .


