<properties 
	pageTitle="DocumentDB – Node.js-API und -SDK | Microsoft Azure" 
	description="Wichtige Informationen zu Node.js-API und -SDK einschließlich Veröffentlichungstermine, Deaktivierungstermine und Änderungen an den einzelnen Versionen des DocumentDB-Node.js-SDK." 
	services="documentdb" 
	documentationCenter="nodejs" 
	authors="rnagpal" 
	manager="jhubbard" 
	editor="cgronlun"/>

<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="nodejs" 
	ms.topic="article" 
	ms.date="08/09/2016" 
	ms.author="rnagpal"/>

# DocumentDB-APIs und -SDKs

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [REST](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

##DocumentDB – Node.js-API und -SDK

<table>
<tr><td>**SDK herunterladen**</td><td>[NPM](https://www.npmjs.com/package/documentdb)</td></tr>
<tr><td>**API-Dokumentation**</td><td>[Node.js-API-Referenzdokumentation](http://azure.github.io/azure-documentdb-node/DocumentClient.html)</td></tr>
<tr><td>**SDK-Installationsanweisungen**</td><td>[Installationsanweisungen](http://azure.github.io/azure-documentdb-node/)</td></tr>
<tr><td>**Am SDK mitwirken**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-node/tree/master/source)</td></tr>
<tr><td>**Beispiele**</td><td>[Node.js-Codebeispiele](documentdb-nodejs-samples.md)</td></tr>
<tr><td>**Tutorial für den Einstieg**</td><td>[Erste Schritte mit dem Node.js-SDK](documentdb-nodejs-get-started.md)</td></tr>
<tr><td>**Web-App-Tutorial**</td><td>[Erstellen einer Node.js-Webanwendung mit DocumentDB](documentdb-nodejs-application.md)</td></tr>
<tr><td>**Aktuelle unterstützte Plattform**</td><td>[Node.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/)<br/>[Node.js v0.12](https://nodejs.org/en/blog/release/v0.12.0/)<br/>[Node.js v4.2.0](https://nodejs.org/en/blog/release/v4.2.0/)</td></tr>
</table></br>

##Versionshinweise

###<a name="1.9.0"/>1.9.0</a>

- Unterstützung für Wiederholungsrichtlinie für gedrosselte Anforderungen hinzugefügt. (Bei gedrosselten Anforderungen wird die Ausnahme „Anforderungsrate zu groß“, Fehlercode 429, angezeigt.) Standardmäßig führt DocumentDB für jede Anforderung neun Wiederholungen durch, wenn der Fehlercode 429 auftritt, und berücksichtigt dabei die „retryAfter“-Zeit im Antwortheader. Eine feste Wiederholungsintervalldauer kann jetzt als Teil der „RetryOptions“-Eigenschaft für das „ConnectionPolicy“-Objekt festgelegt werden, wenn Sie die „retryAfter“-Zeit ignorieren möchten, die vom Server zwischen den Wiederholungen zurückgegeben wird. DocumentDB wartet jetzt bei jeder gedrosselten Anforderung (unabhängig von der Anzahl der Wiederholungen) maximal 30 Sekunden und gibt die Antwort mit dem Fehlercode 429 zurück. Diese Dauer kann auch in der „RetryOptions“-Eigenschaft im „ConnectionPolicy“-Objekt überschrieben werden.

- DocumentDB gibt nun „x-ms-throttle-retry-count“ und „x-ms-throttle-retry-wait-time-ms“ als Antwortheader in jeder Anforderung zurück, um die Anzahl der Wiederholungen bei einer Drosselung und die kumulative Zeit anzugeben, die die Anforderung zwischen den Wiederholungen gewartet hat.

- Die „RetryOptions“-Klasse wurde hinzugefügt, die die „RetryOptions“-Eigenschaft in der „ConnectionPolicy“-Klasse verfügbar macht, welche zum Überschreiben einiger der Standardwiederholungsoptionen verwendet werden kann.

###<a name="1.8.0"/>1.8.0</a>

 - Unterstützung für Datenbankkonten in mehreren Regionen hinzugefügt.

###<a name="1.7.0"/>1.7.0</a>

- Unterstützung des TTL-Features (Time To Live) für Dokumente hinzugefügt.

###<a name="1.6.0"/>1.6.0</a>

- [Partitionierte Sammlungen](documentdb-partition-data.md) und [benutzerdefinierte Leistungsstufen](documentdb-performance-levels.md) implementiert.

###<a name="1.5.6"/>1.5.6</a>

- Fehler in „RangePartitionResolver.resolveForRead“ behoben, durch den aufgrund einer ungültigen Verkettung der Ergebnisse keine Links zurückgegeben wurden.

###<a name="1.5.5"/>1.5.5</a>

- Problem mit „hashParitionResolver resolveForRead()“ behoben: Bei einem nicht angegebenen Partitionsschlüssel wurde eine Ausnahme ausgelöst, anstatt eine Liste aller registrierten Links zurückgegeben.

###<a name="1.5.4"/>1.5.4</a>

- Behebt Problem [Nr. 100](https://github.com/Azure/azure-documentdb-node/issues/100) – Dedizierter HTTPS-Agent: Vermeidung des Änderns des globalen Agents für DocumentDB-Zwecke. Verwenden Sie einen dedizierten-Agent für alle Anforderungen der Bibliothek.

###<a name="1.5.3"/>1.5.3</a>

- Behebt Problem [Nr. 81](https://github.com/Azure/azure-documentdb-node/issues/81): Ordnungsgemäße Behandlung von Bindestrichen in Medien-IDs.

###<a name="1.5.2"/>1.5.2</a>

- Behebt Problem [Nr. 95](https://github.com/Azure/azure-documentdb-node/issues/95): Warnung des EventEmitter-Listeners bei Arbeitsspeicherverlusten.

###<a name="1.5.1"/>1.5.1</a>

- Behebt Problem [92](https://github.com/Azure/azure-documentdb-node/issues/90): Umbenennung des Ordners „Hash“ in „hash“ für Systeme, bei denen die Groß-/Kleinschreibung beachtet wird.

### <a name="1.5.0"/>1.5.0</a>

- Unterstützung für Sharding implementiert, indem Hash- und Bereichspartitionsresolver hinzugefügt wurden.

### <a name="1.4.0"/>1.4.0</a>

- Upsert implementiert. Neue „upsertXXX“-Methoden für „DocumentClient“.

### <a name="1.3.0"/>1.3.0</a>

- Version ausgelassen, um die Versionsnummer in Einklang mit den anderen SDKs zu bringen.

### <a name="1.2.2"/>1.2.2</a>

- Q Promises Wrapper auf neues Repository aufgeteilt.
- Paketdatei für Npm-Registrierung aktualisiert.

### <a name="1.2.1"/>1.2.1</a>

- ID-basiertes Routing implementiert.
- Behebt Problem [49](https://github.com/Azure/azure-documentdb-node/issues/49): Konflikt zwischen current-Eigenschaft und current()-Methode.

### <a name="1.2.0"/>1.2.0</a>

- Unterstützung für „GeoSpatial“-Index hinzugefügt.
- „Id“-Eigenschaft wird für alle Ressourcen überprüft. IDs für Ressourcen dürfen nicht die Zeichen ?, /, #, &#47;&#47;, enthalten und nicht mit einem Leerzeichen enden.
- Neue Überschrift „Fortschritt der Indextransformation“ zu „ResourceResponse“ hinzugefügt.

### <a name="1.1.0"/>1.1.0</a>

- V2-Indizierungsrichtlinie implementiert.

### <a name="1.0.3"/>1.0.3</a>

- Problem [40](https://github.com/Azure/azure-documentdb-node/issues/40): Implementierung der eslint- und grunt-Konfiguration im Core und Promise SDK.

### <a name="1.0.2"/>1.0.2</a>

- Problem [45](https://github.com/Azure/azure-documentdb-node/issues/45) – Promise Wrapper enthält keinen fehlerhaften Header.

### <a name="1.0.1"/>1.0.1</a>

- Möglichkeit zum Abfragen von Konflikten durch Hinzufügen von „readConflicts“, „readConflictAsync“ und „queryConflicts“ implementiert.
- API-Dokumentation aktualisiert.
- Problem [41](https://github.com/Azure/azure-documentdb-node/issues/41): client.createDocumentAsync-Fehler.

### <a name="1.0.0"/>1.0.0</a>

- Allgemeine Verfügbarkeit (GA) des SDK

## Veröffentlichungs- und Deaktivierungstermine
Wenn Microsoft ein SDK deaktiviert, werden Sie mindestens **12 Monate** vorher benachrichtigt, um einen reibungslosen Übergang zu einer neueren/unterstützten Version zu gewährleisten.

Neue Features, Funktionen und Optimierungen werden nur dem aktuellen SDK hinzugefügt. Daher wird empfohlen, immer so früh wie möglich auf die neueste SDK-Version zu aktualisieren.

Anforderungen von DocumentDB mithilfe eines deaktivierten SDK werden vom Dienst abgelehnt.

> [AZURE.WARNING]
Alle Versionen des Azure DocumentDB SDK für Node.js vor Version **1.0.0** werden am **29. Februar 2016** deaktiviert.

<br/>

| Version | Herausgabedatum | Deaktivierungstermine 
| ---	  | ---	         | ---
| [1.9.0](#1.9.0) | 07. Juli 2016 |--- 
| [1.8.0](#1.8.0) | 14. Juni 2016 |--- 
| [1.7.0](#1.7.0) | 26. April 2016 |--- 
| [1.6.0](#1.6.0) | 29. März 2016 |--- 
| [1.5.6](#1.5.6) | 08. März 2016 |--- 
| [1.5.5](#1.5.5) | 02. Februar 2016 |--- 
| [1.5.4](#1.5.4) | 01. Februar 2016 |--- 
| [1.5.2](#1.5.2) | 26. Januar 2016 |--- 
| [1.5.2](#1.5.2) | 22. Januar 2016 |--- 
| [1.5.1](#1.5.1) | 04. Januar 2016 |--- 
| [1.5.0](#1.5.0) | 31. Dezember 2015 |--- 
| [1.4.0](#1.4.0) | 06. Oktober 2015 |--- 
| [1.3.0](#1.3.0) | 06. Oktober 2015 |--- 
| [1.2.2](#1.2.2) | 10. September 2015 |--- 
| [1.2.1](#1.2.1) | 15. August 2015 |--- 
| [1.2.0](#1.2.0) | 05. August 2015 |--- 
| [1.1.0](#1.1.0) | 09. Juli 2015 |--- 
| [1.0.3](#1.0.3) | 04. Juni 2015 |--- 
| [1.0.2](#1.0.2) | 23. Mai 2015 |---
| [1.0.1](#1.0.1) | 15. Mai 2015 |--- 
| [1.0.0](#1.0.0) | 08. April 2015 |--- 
| 0.9.4-prerelease | 06. April 2015 | 29. Februar 2016 
| 0.9.3-prerelease | 14. Januar 2015 | 29. Februar 2016 
| 0.9.2-prerelease | 18. Dezember 2014 | 29. Februar 2016 
| 0.9.1-prerelease | 22. August 2014 | 29. Februar 2016 
| 0.9.0-prerelease | 21. August 2014 | 29. Februar 2016


## Häufig gestellte Fragen
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## Siehe auch

Weitere Informationen zu DocumentDB finden Sie auf der Seite zum Dienst [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/).

<!---HONumber=AcomDC_0810_2016-->