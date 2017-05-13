---
title: Azure DocumentDB Python-API, SDK & Ressourcen | Microsoft-Dokumentation
description: "Wichtige Informationen zur Python-API und zum SDK, einschließlich Veröffentlichungstermine, Deaktivierungstermine und Änderungen an den einzelnen Versionen des DocumentDB Python SDK."
services: documentdb
documentationcenter: python
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 3ac344a9-b2fa-4a3f-a4cc-02d287e05469
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/01/2017
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: af45d745bccb02ce9fd45320e83284a8b33d99bd
ms.contentlocale: de-de
ms.lasthandoff: 05/02/2017


---
# <a name="documentdb-python-sdk-release-notes-and-resources"></a>DocumentDB Python SDK: Versionshinweise und Ressourcen
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

<tr><td>**SDK herunterladen**</td><td>[PyPI](https://pypi.python.org/pypi/pydocumentdb)</td></tr>

<tr><td>**API-Dokumentation**</td><td>[Python-API-Referenzdokumentation](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.html)</td></tr>

<tr><td>**SDK-Installationsanweisungen**</td><td>[Python-SDK-Installationsanweisungen](http://azure.github.io/azure-documentdb-python/)</td></tr>

<tr><td>**Am SDK mitwirken**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-python)</td></tr>

<tr><td>**Erste Schritte**</td><td>[Erste Schritte mit dem Python-SDK](documentdb-python-application.md)</td></tr>

<tr><td>**Aktuell unterstützte Plattform**</td><td>[Python 2.7](https://www.python.org/downloads/) und [Python 3.5](https://www.python.org/downloads/)</td></tr>
</table></br>

## <a name="release-notes"></a>Versionshinweise
### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0
* Unterstützung für Aggregationsabfragen (COUNT, MIN, MAX, SUM und AVG) wurde hinzugefügt.
* Eine Option zum Deaktivieren der SSL-Überprüfung bei der Ausführung für den DocumentDB-Emulator wurde hinzugefügt.
* Die Einschränkung wurde entfernt, dass das Modul für abhängige Anforderungen genau 2.10.0 sein muss.
* Minimaler Durchsatz für partitionierte Sammlungen wurde von 10.100 RU/s auf 2.500 RU/s gesenkt.
* Unterstützung für das Aktivieren der Skriptprotokollierung während der Ausführung einer gespeicherten Prozedur wurde hinzugefügt.
* REST-API-Version wurde mit dieser Version auf „2017-01-19“ (19.01.2017) festgelegt.

### <a name="a-name201201"></a><a name="2.0.1"/>2.0.1
* An Dokumentationskommentaren wurden redaktionelle Änderungen vorgenommen.

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0
* Unterstützung für Python 3.5 wurde hinzugefügt.
* Unterstützung für Verbindungspooling mithilfe eines Anforderungenmoduls wurde hinzugefügt.
* Unterstützung für Sitzungskonsistenz wurde hinzugefügt.
* Unterstützung für TOP-/ORDER BY-Abfragen für partitionierte Sammlungen wurde hinzugefügt.

### <a name="a-name190190"></a><a name="1.9.0"/>1.9.0
* Unterstützung für Wiederholungsrichtlinie für gedrosselte Anforderungen hinzugefügt. (Bei gedrosselten Anforderungen wird die Ausnahme „Anforderungsrate zu groß“, Fehlercode 429, angezeigt.) Standardmäßig führt DocumentDB für jede Anforderung neun Wiederholungen durch, wenn der Fehlercode 429 auftritt, und berücksichtigt dabei die „retryAfter“-Zeit im Antwortheader. Eine feste Wiederholungsintervalldauer kann jetzt als Teil der „RetryOptions“-Eigenschaft für das „ConnectionPolicy“-Objekt festgelegt werden, wenn Sie die „retryAfter“-Zeit ignorieren möchten, die vom Server zwischen den Wiederholungen zurückgegeben wird. DocumentDB wartet jetzt bei jeder gedrosselten Anforderung (unabhängig von der Anzahl der Wiederholungen) maximal 30 Sekunden und gibt die Antwort mit dem Fehlercode 429 zurück. Diese Dauer kann auch in der „RetryOptions“-Eigenschaft im „ConnectionPolicy“-Objekt überschrieben werden.
* DocumentDB gibt nun „x-ms-throttle-retry-count“ und „x-ms-throttle-retry-wait-time-ms“ als Antwortheader in jeder Anforderung zurück, um die Anzahl der Wiederholungen bei einer Drosselung und die kumulative Zeit anzugeben, die die Anforderung zwischen den Wiederholungen gewartet hat.
* Die „RetryPolicy“-Klasse und die dazugehörige Eigenschaft (retry_policy), die für die „document_client“-Klasse verfügbar gemacht wurden, wurden entfernt. Stattdessen wurde die „RetryOptions“-Klasse hinzugefügt, die die „RetryOptions“-Eigenschaft in der „ConnectionPolicy“-Klasse verfügbar macht, welche zum Überschreiben einiger der Standardwiederholungsoptionen verwendet werden kann.

### <a name="a-name180180"></a><a name="1.8.0"/>1.8.0
* Unterstützung für Datenbankkonten in mehreren Regionen hinzugefügt.

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
* Unterstützung des TTL-Features (Time To Live) für Dokumente hinzugefügt.

### <a name="a-name161161"></a><a name="1.6.1"/>1.6.1
* Fehlerbehebungen im Zusammenhang mit serverseitiger Partitionierung, um Sonderzeichen im Pfad zum Partitionsschlüssel zuzulassen

### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0
* [Partitionierte Sammlungen](documentdb-partition-data.md) und [benutzerdefinierte Leistungsstufen](documentdb-performance-levels.md) implementiert. 

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0
* Hinzufügen von Hash- und Bereichspartitionen-Konfliktlösern, um die Freigabe von Anwendungen über mehrere Partitionen zu unterstützen.

### <a name="a-name142142"></a><a name="1.4.2"/>1.4.2
* Upsert implementiert. Neue UpsertXXX-Methoden hinzugefügt, um das „Upsert“-Feature zu unterstützen.
* ID-basiertes Routing implementiert. Keine öffentliche API-Änderungen, alle Änderungen sind intern.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* GeoSpatial-Index unterstützt.
* „Id“-Eigenschaft wird für alle Ressourcen überprüft. IDs für Ressourcen dürfen nicht mit einem Leerzeichen enden und keins der folgenden Zeichen enthalten: ?, /, #, \,
* Neue Überschrift „Fortschritt der Indextransformation“ zu „ResourceResponse“ hinzugefügt.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* V2-Indizierungsrichtlinie implementiert.

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1
* Proxyverbindung unterstützt.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* Allgemeine Verfügbarkeit (GA) des SDK

## <a name="release--retirement-dates"></a>Veröffentlichungs- und Deaktivierungstermine
Wenn Microsoft ein SDK deaktiviert, werden Sie mindestens **12 Monate** vorher benachrichtigt, um einen reibungslosen Übergang zu einer neueren/unterstützten Version zu gewährleisten.

Neue Features, Funktionen und Optimierungen werden nur dem aktuellen SDK hinzugefügt. Daher empfiehlt es sich, immer so früh wie möglich auf die neueste SDK-Version zu aktualisieren. 

Anforderungen von DocumentDB mithilfe eines deaktivierten SDK werden vom Dienst abgelehnt.

> [!WARNING]
> Alle Versionen des Azure DocumentDB SDK für Python vor Version **1.0.0** werden am **29. Februar 2016** deaktiviert. 
> 
> 

<br/>

| Version | Herausgabedatum | Deaktivierungstermine |
| --- | --- | --- |
| [2.1.0](#2.1.0) |01. Mai 2017 |--- |
| [2.0.1](#2.0.1) |30. Oktober 2016 |--- |
| [2.0.0](#2.0.0) |29. September 2016 |--- |
| [1.9.0](#1.9.0) |7. Juli 2016 |--- |
| [1.8.0](#1.8.0) |14. Juni 2016 |--- |
| [1.7.0](#1.7.0) |26. April 2016 |--- |
| [1.6.1](#1.6.1) |8. April 2016 |--- |
| [1.6.0](#1.6.0) |29. März 2016 |--- |
| [1.5.0](#1.5.0) |03. Januar 2016 |--- |
| [1.4.2](#1.4.2) |06. Oktober 2015 |--- |
| [1.4.1](#1.4.1) |06. Oktober 2015 |--- |
| [1.2.0](#1.2.0) |06. August 2015 |--- |
| [1.1.0](#1.1.0) |9. Juli 2015 |--- |
| [1.0.1](#1.0.1) |25. Mai 2015 |--- |
| [1.0.0](#1.0.0) |7. April 2015 |--- |
| 0.9.4-prelease |14. Januar 2015 |29. Februar 2016 |
| 0.9.3-prelease |09. Dezember 2014 |29. Februar 2016 |
| 0.9.2-prelease |25. November 2014 |29. Februar 2016 |
| 0.9.1-prelease |23. September 2014 |29. Februar 2016 |
| 0.9.0-prelease |21.08.14 |29. Februar 2016 |

## <a name="faq"></a>Häufig gestellte Fragen
[!INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Siehe auch
Weitere Informationen zu DocumentDB finden Sie auf der Seite zum Dienst [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) . 


