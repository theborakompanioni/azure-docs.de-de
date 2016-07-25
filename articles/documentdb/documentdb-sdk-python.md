<properties 
	pageTitle="DocumentDB Python SDK | Microsoft Azure" 
	description="Wichtige Informationen zum Python SDK einschließlich Veröffentlichungstermine, Deaktivierungstermine und Änderungen an den einzelnen Versionen des DocumentDB Python SDK." 
	services="documentdb" 
	documentationCenter="python" 
	authors="aliuy" 
	manager="jhubbard" 
	editor="cgronlun"/>

<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="python" 
	ms.topic="article" 
	ms.date="07/07/2016" 
	ms.author="rnagpal"/>

# DocumentDB SDK

> [AZURE.SELECTOR]
- [.NET SDK](documentdb-sdk-dotnet.md)
- [Node.js SDK](documentdb-sdk-node.md)
- [Java SDK](documentdb-sdk-java.md)
- [Python SDK](documentdb-sdk-python.md)

##DocumentDB Python SDK

<table>
<tr><td>**Download**</td><td>[PyPI](https://pypi.python.org/pypi/pydocumentdb)</td></tr>
<tr><td>**Mitwirken**</td><td>[GitHub] (https://github.com/Azure/azure-documentdb-python)</td></tr>
<tr><td>**Dokumentation**</td><td>[Python SDK-Referenzdokumentation](http://azure.github.io/azure-documentdb-python/)</td></tr>
<tr><td>**Erste Schritte**</td><td>[Erste Schritte mit der Python SDK] (documentdb-python-application.md)</td></tr>
<tr><td>**Aktuelle unterstützte Plattform**</td><td>[Python 2.7] (https://www.python.org/download/releases/2.7/)</td></tr>
</table></br>

## Versionshinweise

### <a name="1.9.0"/>[1\.9.0](https://pypi.python.org/pypi/pydocumentdb/1.9.0)
- Unterstützung für Wiederholungsrichtlinie für gedrosselte Anforderungen hinzugefügt. (Bei gedrosselten Anforderungen wird die Ausnahme „Anforderungsrate zu groß“, Fehlercode 429, angezeigt.) Standardmäßig führt DocumentDB für jede Anforderung neun Wiederholungen durch, wenn der Fehlercode 429 auftritt, und berücksichtigt dabei die „retryAfter“-Zeit im Antwortheader. Eine feste Wiederholungsintervalldauer kann jetzt als Teil der „RetryOptions“-Eigenschaft für das „ConnectionPolicy“-Objekt festgelegt werden, wenn Sie die „retryAfter“-Zeit ignorieren möchten, die vom Server zwischen den Wiederholungen zurückgegeben wird. DocumentDB wartet jetzt bei jeder gedrosselten Anforderung (unabhängig von der Anzahl der Wiederholungen) maximal 30 Sekunden und gibt die Antwort mit dem Fehlercode 429 zurück. Diese Dauer kann auch in der „RetryOptions“-Eigenschaft im „ConnectionPolicy“-Objekt überschrieben werden.

- DocumentDB gibt nun „x-ms-throttle-retry-count“ und „x-ms-throttle-retry-wait-time-ms“ als Antwortheader in jeder Anforderung zurück, um die Anzahl der Wiederholungen bei einer Drosselung und die kumulative Zeit anzugeben, die die Anforderung zwischen den Wiederholungen gewartet hat.

- Die „RetryPolicy“-Klasse und die dazugehörige Eigenschaft (retry\_policy), die für die „document\_client“-Klasse verfügbar gemacht wurden, wurden entfernt. Stattdessen wurde die „RetryOptions“-Klasse hinzugefügt, die die „RetryOptions“-Eigenschaft in der „ConnectionPolicy“-Klasse verfügbar macht, welche zum Überschreiben einiger der Standardwiederholungsoptionen verwendet werden kann.

### <a name="1.8.0"/>[1\.8.0](https://pypi.python.org/pypi/pydocumentdb/1.8.0)
  - Unterstützung für Datenbankkonten in mehreren Regionen hinzugefügt.

### <a name="1.7.0"/>[1\.7.0](https://pypi.python.org/pypi/pydocumentdb/1.7.0)
- Unterstützung des TTL-Features (Time To Live) für Dokumente hinzugefügt.

### <a name="1.6.1"/>[1\.6.1](https://pypi.python.org/pypi/pydocumentdb/1.6.1)
- Fehlerbehebungen im Zusammenhang mit serverseitiger Partitionierung, um Sonderzeichen im Pfad zum Partitionsschlüssel zuzulassen

### <a name="1.6.0"/>[1\.6.0](https://pypi.python.org/pypi/pydocumentdb/1.6.0)
- [Partitionierte Sammlungen](documentdb-partition-data.md) und [benutzerdefinierte Leistungsstufen](documentdb-performance-levels.md) implementiert.

### <a name="1.5.0"/>[1\.5.0](https://pypi.python.org/pypi/pydocumentdb/1.5.0)
- Hinzufügen von Hash- und Bereichspartitionen-Konfliktlösern, um die Freigabe von Anwendungen über mehrere Partitionen zu unterstützen.

### <a name="1.4.2"/>[1\.4.2](https://pypi.python.org/pypi/pydocumentdb/1.4.2)
- Upsert implementiert. Neue UpsertXXX-Methoden hinzugefügt, um das „Upsert“-Feature zu unterstützen.
- ID-basiertes Routing implementiert. Keine öffentliche API-Änderungen, alle Änderungen sind intern.

### <a name="1.2.0"/>[1\.2.0](https://pypi.python.org/pypi/pydocumentdb/1.2.0)
- GeoSpatial-Index unterstützt.
- „Id“-Eigenschaft wird für alle Ressourcen überprüft. IDs für Ressourcen dürfen nicht die Zeichen ?, /, #, \\ enthalten und nicht mit einem Leerzeichen enden.
- Neue Überschrift „Fortschritt der Indextransformation“ zu „ResourceResponse“ hinzugefügt.

### <a name="1.1.0"/>[1\.1.0](https://pypi.python.org/pypi/pydocumentdb/1.1.0)
- V2-Indizierungsrichtlinie implementiert.

### <a name="1.0.1"/>[1\.0.1](https://pypi.python.org/pypi/pydocumentdb/1.0.1)
- Proxyverbindung unterstützt.

### <a name="1.0.0"/>[1\.0.0](https://pypi.python.org/pypi/pydocumentdb/1.0.0)
- Allgemeine Verfügbarkeit (GA) des SDK

## Veröffentlichungs- und Deaktivierungstermine
Wenn Microsoft ein SDK deaktiviert, werden Sie mindestens **12 Monate** vorher benachrichtigt, um einen reibungslosen Übergang zu einer neueren, unterstützten Version zu gewährleisten.

Neue Features, Funktionen und Optimierungen werden nur dem aktuellen SDK hinzugefügt. Daher wird empfohlen, immer so früh wie möglich auf die neueste SDK-Version zu aktualisieren.

Anforderungen von DocumentDB mithilfe eines deaktivierten SDK werden vom Dienst abgelehnt.


> [AZURE.WARNING]Alle Versionen des Azure DocumentDB SDK für Python vor Version **1.0.0** werden am **29. Februar 2016** deaktiviert.

<br/>

| Version | Herausgabedatum | Deaktivierungstermine 
| ---	  | ---	         | ---
| [1\.9.0](#1.9.0) | 07. Juni 2016 |---
| [1\.8.0](#1.8.0) | 14. April 2016 |---
| [1\.7.0](#1.7.0) | 8. April 2016 |---
| [1\.6.1](#1.6.1) | 08. März 2016 |---
| [1\.6.0](#1.6.0) | 3. Januar 2016 |---
| [1\.5.0](#1.5.0) | 6. Oktober 2016 |---
| [1\.4.2](#1.4.2) | 6. Oktober 2015 |---
| [1\.4.1](#1.4.1) | 6. August 2015 |---
| [1\.2.0](#1.2.0) | 9. Juli 2015 |---
| [1\.1.0](#1.1.0) | 09. Mai 2015 |---
| [1\.0.1](#1.0.1) | 25. Mai 2015 |---
| [1\.0.0](#1.0.0) | 7. April 2015 |---
| 0.9.4-prelease | 14. Januar 2015 | 29. Februar 2016
| 0.9.3-prelease | December 09, 2014 | 29. Februar 2016
| 0.9.2-prelease | 25. November 2014 |29. Februar 2016
| 0.9.1-prelease | 23. September 2014 | 29. Februar 2016
| 0.9.0-prelease | 21. August 2014 | 29. Februar 2016

## Häufig gestellte Fragen
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## Weitere Informationen

Weitere Informationen zu DocumentDB finden Sie auf der Seite zum Dienst [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/).

<!---HONumber=AcomDC_0713_2016-->