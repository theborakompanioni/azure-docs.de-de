---
title: 'Azure Cosmos DB: DocumentDB-API | Microsoft-Dokumentation'
description: "Es wird beschrieben, wie Sie Azure Cosmos DB zum Speichern und Abfragen von sehr großen Mengen von JSON-Dokumenten mit geringer Wartezeit mithilfe von SQL und JavaScript verwenden können."
keywords: JSON-Datenbank, Dokumentendatenbank
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 686cdd2b-704a-4488-921e-8eefb70d5c63
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/22/2017
ms.author: mimig
ms.translationtype: HT
ms.sourcegitcommit: 368589509b163cacf495fd0be893a8953fe2066e
ms.openlocfilehash: 2cb4bd74ea973c8ff980d208a8c5f63a98ec1edd
ms.contentlocale: de-de
ms.lasthandoff: 08/17/2017

---
# <a name="introduction-to-azure-cosmos-db-documentdb-api"></a>Einführung in Azure Cosmos DB: DocumentDB-API

[Azure Cosmos DB](introduction.md) ist ein global verteilter Datenbankdienst von Microsoft mit mehreren Modellen für unternehmenskritische Anwendungen. Azure Cosmos DB bietet [sofort einsetzbare globale Verteilung](distribute-data-globally.md), [flexible Skalierung von Durchsatz und Speicher](partition-data.md) weltweit, Latenzzeiten im einstelligen Millisekundenbereich beim 99. Perzentil, [fünf wohldefinierte Konsistenzebenen](consistency-levels.md) sowie garantierte hohe Verfügbarkeit, gestützt durch [branchenführende Vereinbarungen zum Servicelevel](https://azure.microsoft.com/support/legal/sla/cosmos-db/) (SLAs, Service Level Agreements). Azure Cosmos DB [indiziert automatisch Daten](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf), sodass Sie sich nicht mit der Schema- und Indexverwaltung befassen müssen. Es unterstützt mehrere Datenmodelle – Dokumente, Schlüsselwerte, Diagramme und spaltenorientierte Datenmodelle. 

![Azure DocumentDB-API](./media/documentdb-introduction/cosmosdb-documentdb.png) 

Mit der DocumentDB-API stellt Azure Cosmos DB umfassende und vertraute [SQL-Abfragefunktionen](documentdb-sql-query.md) mit einheitlich geringen Wartezeiten für schemalose JSON-Daten bereit. Dieser Artikel enthält eine Übersicht über die DocumentDB-API von Azure Cosmos DB. Außerdem wird beschrieben, wie Sie sie zum Speichern von sehr großen Mengen von JSON-Daten und Abfragen mit einer Wartezeit im Millisekundenbereich verwenden und das Schema leicht weiterentwickeln können. 

## <a name="what-capabilities-and-key-features-does-azure-cosmos-db-offer"></a>Welche Funktionen und wichtigen Features sind in Azure Cosmos DB enthalten?
Azure Cosmos DB verfügt über die DocumentDB-API über die folgenden wichtigen Funktionen und Vorteile:

* **Flexibel skalierbarer Durchsatz und Speicher**: Die JSON-Datenbank lässt sich problemlos zentral hoch- und herunterskalieren, um Ihren Anwendungsanforderungen gerecht zu werden. Ihre Daten werden für niedrige vorhersagbare Latenzen auf Solid-State-Datenträgern (SSDs) gespeichert. Azure Cosmos DB unterstützt Container zum Speichern von JSON-Daten, die als Sammlungen bezeichnet werden. Diese Container können auf nahezu unbegrenzte Speichergrößen und Mengen an bereitgestelltem Durchsatz skaliert werden. Sie können Azure Cosmos DB mit vorhersagbarer Leistung flexibel skalieren, wenn Ihre Anwendung wächst. 


* **Replikation in mehreren Regionen**: Azure Cosmos DB repliziert Ihre Daten auf transparente Weise in allen Regionen, die Sie Ihrem Azure Cosmos DB-Konto zugeordnet haben. Sie können Anwendungen entwickeln, die globalen Zugriff auf Daten benötigen, und dabei für ein ausgewogenes Maß an Konsistenz, Verfügbarkeit und Leistung sorgen – jeweils mit den entsprechenden Garantien. Azure Cosmos DB bietet transparentes regionales Failover mit Multihosting-APIs sowie die Möglichkeit, Durchsatz und Speicher für alle Konten weltweit flexibel zu skalieren. Weitere Informationen finden Sie unter [Globale Verteilung von Daten mit Azure Cosmos DB](distribute-data-globally.md).

* **Ad-hoc-Abfragen mit vertrauter SQL-Syntax**: Heterogene JSON-Dokumente können gespeichert und mithilfe einer vertrauten SQL-Syntax abgefragt werden. Azure Cosmos DB nutzt eine sperrfreie, protokollstrukturierte Indizierungstechnologie für die gleichzeitige Ausführung zahlreicher Vorgänge, um sämtliche Dokumentinhalte automatisch zu indizieren. Dies ermöglicht umfassende Echtzeitabfragen, ohne Schemahinweise, Sekundärindizes oder Sichten angeben zu müssen. Weitere Informationen finden Sie unter [Abfragen von Azure Cosmos DB](documentdb-sql-query.md). 
* **JavaScript-Ausführung innerhalb der Datenbank** : Anwendungslogik kann mithilfe von Standard-JavaScript als gespeicherte Prozeduren, Trigger und benutzerdefinierte Funktionen ausgedrückt werden. Auf diese Weise kann die Anwendungslogik auf Daten angewendet werden, ohne dass Sie sich Gedanken über den Konflikt zwischen Anwendung und Datenbankschema machen müssen. Die DocumentDB-API ermöglicht eine umfassende transaktionale Ausführung von JavaScript-Anwendungslogik direkt im Datenbankmodul. Die enge Integration von JavaScript ermöglicht es, Einfüge-, Ersetzungs-, Lösch- und Auswahlvorgänge über ein JavaScript-Programm als isolierte Transaktion auszuführen. Weitere Einzelheiten finden Sie unter [Serverseitige DocumentDB-Programmierung](programming.md).

* **Einstellbare Konsistenzebenen:** Die Konsistenz kann über fünf klar definierte Ebenen abgestimmt werden, um für ein ausgewogenes Verhältnis zwischen Konsistenz und Leistung zu sorgen. Für Abfragen und Lesevorgänge bietet Azure Cosmos DB fünf verschiedene Konsistenzebenen – „stark“, „begrenzte Veraltung“, „Sitzung“, „Präfixkonsistenz“ und „letztlich“. Mit diesen granularen, gut abgegrenzten Konsistenzebenen können fundierte Kompromisse zwischen Konsistenz, Verfügbarkeit und Latenz geschlossen werden. Weitere Informationen finden Sie unter [Verwenden von Konsistenzebenen zum Maximieren der Verfügbarkeit und Leistung](consistency-levels.md).

* **Vollständig verwaltet**: Sie müssen sich nicht mehr mit der Verwaltung von Datenbanken und Rechenressourcen befassen. Dank des vollständig verwalteten Microsoft Azure-Diensts müssen Sie sich nicht mit der Verwaltung virtueller Computer, der Bereitstellung und Konfiguration von Software, der Skalierung oder mit komplexen Datenebenenupgrades herumschlagen. Alle Datenbanken werden automatisch gesichert und vor regionalen Ausfällen geschützt. Sie können einfach Azure Cosmos DB-Konten hinzufügen und nach Bedarf Kapazitäten bereitstellen. Dies ermöglicht es Ihnen, sich auf Ihre Anwendung zu konzentrieren, ohne sich mit dem Betrieb und der Verwaltung der Datenbank aufhalten zu müssen. 

* **Offen konzipiert:** Mithilfe Ihrer vorhandenen Kenntnisse und Tools finden Sie ganz schnell den Einstieg. Die Programmierung mit der DocumentDB-API ist einfach und verständlich und erfordert keine neuen Tools oder benutzerdefinierten Erweiterungen für JSON oder JavaScript. Sie können auf sämtliche Datenbankfunktionen – einschließlich CRUD-, Abfrage- und JavaScript-Verarbeitung – über eine einfache HTTP-basierte RESTful-Schnittstelle zugreifen. Die DocumentDB-API bezieht vorhandene Formate, Sprachen und Standards ein und bietet gleichzeitig wichtige, darauf aufbauende Datenbankfunktionen.

* **Automatische Indizierung**: Alle Dokumente in der Datenbank werden von Azure Cosmos DB automatisch indiziert, ohne dass ein Schema oder die Erstellung sekundärer Indizes erwartet oder angefordert wird. Ist keine Indizierung aller Elemente gewünscht? Keine Sorge. Sie können [in Ihren JSON-Dateien auch Pfade ausschließen](indexing-policies.md).

* **Unterstützung von Änderungsfeeds:** Ein Änderungsfeed bietet eine Liste von Dokumenten in einer Azure Cosmos DB-Sammlung, die in der Reihenfolge sortiert ist, in der die Dokumente geändert wurden. Dieser Feed kann verwendet werden, um die Sammlung auf Änderungen an Daten zu überwachen und Daten zu replizieren, API-Aufrufe auszulösen oder die Streamverarbeitung von Updates auszuführen. Der Änderungsfeed ist automatisch aktiviert und einfach zu verwenden: [Weitere Informationen zum Änderungsfeed](https://docs.microsoft.com/azure/cosmos-db/change-feed). 

## <a name="data-management"></a>Wie verwalten Sie Daten mit der DocumentDB-API?
Die DocumentDB-API ermöglicht die Verwaltung von JSON-Daten mit klar definierten Datenbankressourcen. Diese Ressourcen werden werden repliziert, um Hochverfügbarkeit zu bieten, und sind anhand ihres logischen URI eindeutig adressierbar. Die DocumentDB-API bietet ein einfaches HTTP-basiertes RESTful-Programmiermodell für alle Ressourcen. 


Ein Azure Cosmos DB-Datenbankkonto ist ein eindeutiger Namespace, über den Sie Zugriff auf Azure Cosmos DB erhalten. Um ein Datenbankkonto erstellen zu können, müssen Sie über ein Azure-Abonnement verfügen. Dadurch erhalten Sie Zugriff auf eine Vielzahl von Azure-Diensten. 

Alle Ressourcen in Azure Cosmos DB werden als JSON-Dokumente modelliert und gespeichert. Ressourcen werden als Elemente verwaltet, bei denen es sich um JSON-Dokumente mit Metadaten handelt, und als Feeds, bei denen es sich um Auflistungen von Elementen handelt. Gruppen von Elementen sind in ihren jeweiligen Feeds enthalten.

Die folgende Abbildung veranschaulicht die Beziehung zwischen den Azure Cosmos DB-Ressourcen:

![Hierarchische Beziehung zwischen Ressourcen in Azure Cosmos DB][1] 

Ein Datenbankkonto besteht aus einer Reihe von Datenbanken, die jeweils mehrere Auflistungen umfassen, die wiederum jeweils gespeicherte Prozeduren, Trigger, UDFs, Dokumente und zugehörige Anhänge enthalten können. Einer Datenbank sind zudem Benutzer zugeordnet, die jeweils über eine Reihe von Berechtigungen für den Zugriff auf verschiedene andere Auflistungen, gespeicherte Prozeduren, Trigger, UDFs, Dokumente oder Anhänge verfügen. Während es sich bei Datenbanken, Benutzern, Berechtigungen und Auflistungen um systemdefinierte Ressourcen mit bekannten Schemas handelt, enthalten Dokumente, gespeicherte Prozeduren, Trigger, UDFs und Anhänge beliebige benutzerdefinierte JSON-Inhalte.  

> [!NOTE]
> Da die DocumentDB-API zuvor als Azure DocumentDB-Dienst verfügbar war, können Sie Konten, die mit der Azure Resource Manager-REST-API oder Tools erstellt wurden, weiterhin bereitstellen, überwachen und verwalten, indem Sie entweder die Azure DocumentDB- oder die Azure Cosmos DB-Ressourcennamen verwenden. Beim Verweisen auf die Azure DocumentDB-APIs sind diese Namen austauschbar. 

## <a name="develop"></a> Wie kann ich mit der DocumentDB-API Apps entwickeln?

Mit Azure Cosmos DB werden Ressourcen über die REST-APIs verfügbar gemacht, die von allen Sprachen aufgerufen werden können, mit denen HTTP/HTTPS-Anforderungen möglich sind. Darüber hinaus bieten wir Programmierbibliotheken für mehrere beliebte Sprachen für die DocumentDB-API an. Die Clientbibliotheken vereinfachen viele Aspekte der Arbeit mit der API, da Details wie Zwischenspeicherung von Adressen, Ausnahmeverwaltung und automatische Wiederholungsversuche verarbeitet werden können. Bibliotheken sind aktuell für die folgenden Sprachen und Plattformen erhältlich:  

| Download | Dokumentation |
| --- | --- |
| [.NET SDK](http://go.microsoft.com/fwlink/?LinkID=402989) |[.NET-Bibliothek](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) |
| [Node.js SDK](http://go.microsoft.com/fwlink/?LinkID=402990) |[Node.js-Bibliothek](http://azure.github.io/azure-documentdb-node/) |
| [Java SDK](http://go.microsoft.com/fwlink/?LinkID=402380) |[Java-Bibliothek](/java/api/com.microsoft.azure.documentdb) |
| [JavaScript SDK](http://go.microsoft.com/fwlink/?LinkID=402991) |[JavaScript-Bibliothek](http://azure.github.io/azure-documentdb-js/) |
| – |[Serverseitiges JavaScript SDK](http://azure.github.io/azure-documentdb-js-server/) |
| [Python SDK](https://pypi.python.org/pypi/pydocumentdb) |[Python-Bibliothek](http://azure.github.io/azure-documentdb-python/) |
| – | [API für MongoDB](mongodb-introduction.md)


Durch Verwendung des [Azure Cosmos DB-Emulators](local-emulator.md) können Sie die Anwendung lokal mit der DocumentDB-API entwickeln und testen, ohne ein Azure-Abonnement zu erstellen oder sonstige Kosten zu verursachen. Wenn Sie mit der Funktion der Anwendung im Emulator zufrieden sind, können Sie zur Verwendung eines Azure Cosmos DB-Kontos in der Cloud wechseln.

Abgesehen von den grundlegenden Erstellungs-, Lese-, Aktualisierungs- und Löschvorgängen bietet die DocumentDB-API eine funktionsreiche SQL-Abfrageschnittstelle für den Abruf von JSON-Dokumenten und serverseitige Unterstützung für die transaktionale Ausführung von JavaScript-Anwendungslogik. Die Schnittstellen für die Abfrage- und Skriptausführung sind über alle Plattformbibliotheken verfügbar. Gleiches gilt für die REST-APIs. 

### <a name="sql-query"></a>SQL-Abfrage
Die DocumentDB-API unterstützt die Abfrage von Dokumenten mithilfe einer SQL-Sprache, die im JavaScript-Typsystem verankert ist, sowie mithilfe von Ausdrücken, die relationale, hierarchische und räumliche Abfragen unterstützen. Die DocumentDB-Abfragesprache ist eine einfache und dennoch effektive Schnittstelle für die Abfrage von JSON-Dokumenten. Die Sprache unterstützt einen Teil der ANSI SQL-Grammatik und sorgt für eine enge Integration von JavaScript-basierten Objekt-, Array-, Objektkonstruktions- und Funktionsaufrufen. Die DocumentDB-API stellt ihr Abfragemodell ohne explizite Schema- oder Indizierungshinweise des Entwicklers bereit.

Benutzerdefinierte Funktionen (UDFs) können mit der DocumentDB-API registriert und als Teil einer SQL-Abfrage referenziert werden, um so die Grammatik zu erweitern und die benutzerdefinierte Anwendungslogik zu unterstützen. Diese UDFs werden als JavaScript-Programme programmiert und in der Datenbank ausgeführt. 

Für .NET-Entwickler wird vom [.NET-SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx) der DocumentDB-API auch ein LINQ-Abfrageanbieter bereitgestellt. 

### <a name="transactions-and-javascript-execution"></a>Transaktionen und JavaScript-Ausführung
Die DocumentDB-API ermöglicht es Ihnen, Anwendungslogik als festgelegte Programme zu programmieren, die vollständig in JavaScript verfasst sind. Diese Programme werden für eine Auflistung registriert und unterstützen die Ausführung von Datenbankvorgängen in einer gegebenen Auflistung. JavaScript kann für die Ausführung als Trigger, gespeicherte Prozedur oder UDF registriert werden. Trigger und gespeicherte Prozeduren können Dokumente erstellen, lesen, aktualisieren und löschen. UDFs hingegen werden ohne Schreibzugriff auf die Auflistung als Teil der Abfrageausführung ausgeführt.

Die JavaScript-Ausführung in der Cosmos-Datenbank wurde anhand der gleichen Konzepte modelliert wie relationale Datenbanksysteme. JavaScript dient dabei als moderner Ersatz für Transact-SQL. Sämtliche JavaScript-Logik wird innerhalb einer umgebenden ACI-Transaktion mit Snapshotisolation ausgeführt. Während des Ausführungsverlaufs wird die gesamte Transaktion abgebrochen, wenn JavaScript eine Ausnahme auslöst.

## <a name="are-there-any-online-courses-on-azure-cosmos-db"></a>Sind Onlinekurse zu Azure Cosmos DB verfügbar?

Ja, es gibt einen [Microsoft Virtual Academy-Kurs](https://mva.microsoft.com/en-US/training-courses/azure-documentdb-planetscale-nosql-16847) zu Azure DocumentDB. 

>[!VIDEO https://mva.microsoft.com/en-US/training-courses-embed/azure-documentdb-planetscale-nosql-16847]
>
>

## <a name="next-steps"></a>Nächste Schritte
Verfügen Sie bereits über ein Azure-Konto? Anschließend könne Sie mit der Nutzung von Azure Cosmos DB beginnen, indem Sie die Schritte in den [Schnellstart](../cosmos-db/create-documentdb-dotnet.md)-Dokumenten ausführen. Darin werden das Erstellen eines Kontos und die ersten Schritte mit Cosmos DB beschrieben.

[1]: ./media/documentdb-introduction/json-database-resources1.png


