---
title: "Azure Cosmos DB – Häufig gestellte Fragen | Microsoft-Dokumentation"
description: "Enthält Antworten auf häufig gestellte Fragen zu Azure Cosmos DB, einem weltweit verteilten Datenbankdienst, der Unterstützung für mehrere Datenbankmodelle bietet. Informieren Sie sich über Kapazität, Leistungsebenen und Skalierung."
keywords: "Datenbankfragen,häufig gestellte Fragen,DocumentDB,Azure,Microsoft Azure"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: b68d1831-35f9-443d-a0ac-dad0c89f245b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2017
ms.author: mimig
ms.translationtype: HT
ms.sourcegitcommit: 9b7316a5bffbd689bdb26e9524129ceed06606d5
ms.openlocfilehash: e8ac4bef5af7a98857278b82304bb58f1ac6e342
ms.contentlocale: de-de
ms.lasthandoff: 09/08/2017

---
# <a name="azure-cosmos-db-faq"></a>Azure Cosmos DB – Häufig gestellte Fragen
## <a name="azure-cosmos-db-fundamentals"></a>Azure DB Cosmos-Grundlagen
### <a name="what-is-azure-cosmos-db"></a>Was ist Azure Cosmos DB?
Azure Cosmos DB ist ein global replizierter Datenbankdienst mit mehreren Modellen, der das Durchführen umfassender Abfragen für schemafreie Daten, eine konfigurierbare und zuverlässige Leistung und eine schnelle Entwicklung ermöglicht. Dies wird mithilfe einer verwalteten Plattform erreicht, die auf der Leistungsstärke und hohen Reichweite von Microsoft Azure basiert. 

Azure Cosmos DB ist die geeignete Lösung für Web-, Mobil-, Gaming- und IoT-Anwendungen, wenn vorhersagbarer Durchsatz, hohe Verfügbarkeit, niedrige Latenz und schemafreie Datenmodelle wichtige Anforderungen sind. Sie bietet Schemaflexibilität und umfassende Indizierung und verfügt über die Transaktionsunterstützung mehrerer Dokumente durch integriertes JavaScript. 

Weitere Datenbankfragen, -antworten und -anweisungen zur Bereitstellung und Verwendung des Diensts finden Sie auf der Seite [Dokumentation für Azure Cosmos DB](https://azure.microsoft.com/documentation/services/cosmos-db/).

### <a name="what-happened-to-documentdb"></a>Was ist mit DocumentDB passiert?
Die DocumentDB-API stellt eine der unterstützten APIs und Datenmodelle für Azure-Cosmos DB dar. Darüber hinaus unterstützt Azure Cosmos DB Sie mit einer Graph-API (Vorschauversion), Table-API (Vorschauversion) und MongoDB-API. Weitere Informationen finden Sie unter [Fragen von DocumentDB-Kunden](#moving-to-cosmos-db).

### <a name="how-do-i-get-to-my-documentdb-account-in-the-azure-portal"></a>Wie gelange ich im Azure-Portal an mein DocumentDB-Konto?
Klicken Sie im Azure-Portal im linken Bereich auf das Azure Cosmos DB-Symbol. Wenn Sie zuvor über ein DocumentDB-Konto verfügten, besitzen Sie nun ein Azure Cosmos DB-Konto. An der Abrechnung ändert sich nichts.

### <a name="what-are-the-typical-use-cases-for-azure-cosmos-db"></a>Was sind die typischen Anwendungsfälle für Azure Cosmos DB?
Azure Cosmos DB ist eine gute Wahl für neue Web-, Mobil-, Gaming- und IoT-Anwendungen, bei denen Dinge wie automatische Skalierung, vorhersagbare Leistung, kurze Reaktionszeiten im Millisekundenbereich und die Möglichkeit zum Abfragen von schemafreien Daten wichtig sind. Azure Cosmos DB eignet sich für schnelle Entwicklungen und die Unterstützung kontinuierlicher Iterationen von Anwendungsdatenmodellen. Anwendungen, mit denen von Benutzern erzeugte Inhalte und Daten verwaltet werden, sind ein [häufiger Anwendungsfall von Azure Cosmos DB](use-cases.md). 

### <a name="how-does-azure-cosmos-db-offer-predictable-performance"></a>Wie wird bei Azure Cosmos DB für eine vorhersagbare Leistung gesorgt?
In Azure Cosmos DB wird der Durchsatz in [Anforderungseinheiten](request-units.md) (Request Units, RUs) gemessen. Ein Durchsatz von einer Anforderungseinheit entspricht dem Durchsatz der GET-Anforderung für ein Dokument mit einer Größe von 1 KB. Jeder Vorgang in Azure Cosmos DB (einschließlich der Ausführung von Lese- und Schreibvorgängen, SQL-Abfragen sowie gespeicherten Prozeduren) verfügt über einen deterministischen Wert für die Anforderungseinheiten, der auf dem erforderlichen Durchsatz zum Abschließen des Vorgangs basiert. Im Hinblick auf den Durchsatz Ihrer Anwendung müssen Sie sich also keine Gedanken um das Zusammenspiel von CPU, E/A-Leistung und Arbeitsspeicher machen, sondern können mit einer einzigen Kennzahl arbeiten: der Anforderungseinheit.

Sie können jeden Azure Cosmos DB-Container mit bereitgestelltem Durchsatz reservieren (Anforderungseinheiten des Durchsatzes pro Sekunde). Für Anwendungen jeder Größe können Sie Vergleichstests einzelner Anforderungen durchführen, um deren Werte für die Anforderungseinheiten zu messen. Außerdem können Sie einen Container bereitstellen, um die Gesamtsumme der Anforderungseinheiten über alle Anforderungen hinweg zu verarbeiten. Sie können den Durchsatz des Containers auch zentral hoch- oder herunterskalieren, wenn sich die Anforderungen Ihrer Anwendung ändern. Weitere Informationen zu Anforderungseinheiten und Hilfe zum Ermitteln Ihrer Containeranforderungen finden Sie unter [Schätzen der Durchsatzanforderungen](request-units.md#estimating-throughput-needs). Probieren Sie außerdem den [Durchsatzrechner](https://www.documentdb.com/capacityplanner) aus. Der Ausdruck *Container* bezieht sich hier auf eine DocumentDB-API-Sammlung, einen Graph-API-Graphen, eine MongoDB-API-Sammlung und eine Table-API-Tabelle. 

### <a name="how-does-azure-cosmos-db-support-various-data-models-such-as-keyvalue-columnar-document-and-graph"></a>Wie unterstützt Azure Cosmos DB verschiedene Datenmodelle wie Schlüssel/Wert-, einspaltige, Dokument- und Diagrammdaten?

Aufgrund der ARS-Konzeption (Atoms, Records and Sequences; Atoms, Datensätze und Sequenzen), auf der Azure Cosmos DB beruht, werden Schlüssel/Wert(Tabellen)-, einspaltige, Dokument- und Diagrammdaten nativ unterstützt. Atoms, Datensätze und Sequenzen können problemlos verschiedenen Datenmodellen zugeordnet und auf diese projiziert werden. Die APIs für eine Teilmenge von Modellen sind derzeit verfügbar (DocumentDB-, MongoDB-, Tabellen- und Graph-API), während andere APIs speziell für zusätzliche Datenmodelle in Zukunft bereitgestellt werden.

Azure Cosmos DB verfügt über ein schemaagnostisches Indizierungsmodul, das alle erfassten Daten automatisch ohne Schema oder sekundäre Indizes seitens des Entwicklers indizieren kann. Das Modul basiert auf einem Satz von logischen Indexlayouts (invertiert, einspaltig, Struktur), die die Speicheranordnung von den Index- und Abfrageverarbeitungssubsystemen entkoppeln. Cosmos DB bietet auch die Möglichkeit, eine Reihe von Wire Protocols und APIs in einer erweiterbaren Weise zu unterstützen und effizient im Kerndatenmodell (1) sowie in den logischen Indexlayouts (2) zu übersetzen. So bietet es eine einzigartige native Unterstützung für verschiedene Datenmodelle.

### <a name="is-azure-cosmos-db-hipaa-compliant"></a>Ist Azure Cosmos DB HIPAA-kompatibel?
Ja, Azure Cosmos DB ist HIPAA-kompatibel. HIPAA gibt Anforderungen für die Verwendung, Offenlegung und den Schutz von individuell identifizierbaren Gesundheitsinformationen vor. Weitere Informationen finden Sie im [Microsoft Trust Center](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA).

### <a name="what-are-the-storage-limits-of-azure-cosmos-db"></a>Wie lauten die Speicherbeschränkungen von Azure Cosmos DB?
Es gibt keine Beschränkung in Bezug auf die Gesamtmenge der Daten, die von einem Container in Azure Cosmos DB gespeichert werden können.

### <a name="what-are-the-throughput-limits-of-azure-cosmos-db"></a>Wo liegen die Durchsatzgrenzwerte von Azure Cosmos DB?
Es gibt keine Beschränkung in Bezug auf den Durchsatz, der von einem Container in Azure Cosmos DB unterstützt werden kann. Der wichtigste Punkt hierbei ist, dass Ihre Workload zu ungefähr gleichen Teilen auf eine ausreichend große Anzahl von Partitionsschlüsseln verteilt wird.

### <a name="how-much-does-azure-cosmos-db-cost"></a>Wie viel kostet Azure Cosmos DB?
Ausführliche Informationen hierzu finden Sie auf der Seite [Azure Cosmos DB – Preise](https://azure.microsoft.com/pricing/details/cosmos-db/). Die Nutzungsgebühren für Azure Cosmos DB werden von der Anzahl von bereitgestellten Containern, der Anzahl der Stunden, die die Container online waren, und dem bereitgestellten Durchsatz für jeden Container bestimmt. Der Ausdruck *Container* bezieht sich hier auf die DocumentDB-API-Sammlung, den Graph-API-Graphen, die MongoDB-API-Sammlung und die Table-API-Tabellen. 

### <a name="is-a-free-account-available"></a>Ist ein kostenloses Konto verfügbar?
Ja. Sie können sich für ein zeitlich begrenztes Konto registrieren – kostenlos und ohne Verpflichtung. Informationen zum Registrieren finden Sie unter [Azure Cosmos DB kostenlos testen](https://azure.microsoft.com/try/cosmosdb/), oder informieren Sie sich unter [Azure Cosmos DB testen – häufig gestellte Fragen](#try-cosmos-db).

Wenn Azure für Sie neu ist, können Sie sich für ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) registrieren. Sie erhalten eine Gutschrift für 30 Tage, in denen Sie alle Azure-Dienste ausprobieren können. Wenn Sie ein Visual Studio-Abonnement besitzen, haben Sie ebenfalls Anspruch auf eine [kostenlose Azure-Gutschrift](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), die Sie für beliebige Azure-Dienste nutzen können. 

Sie können auch den [Azure Cosmos DB-Emulator](local-emulator.md) zum kostenlosen lokalen Entwickeln und Testen Ihrer Anwendung verwenden, ohne ein Azure-Abonnement zu erstellen. Wenn Sie mit der Funktion der Anwendung im Azure Cosmos DB-Emulator zufrieden sind, können Sie zur Verwendung eines Azure Cosmos DB-Kontos in der Cloud wechseln.

### <a name="how-can-i-get-additional-help-with-azure-cosmos-db"></a>Wie kann ich zusätzliche Hilfe zu Azure Cosmos DB erhalten?
Wenn Sie Hilfe benötigen, können Sie sich über [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-cosmosdb) oder das [MSDN-Forum](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=AzureDocumentDB) an uns wenden. Oder vereinbaren Sie einen Termin für einen 1:1-Chat mit dem Entwicklungsteam von Azure Cosmos DB, indem Sie eine E-Mail an [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com) senden. 

<a id="try-cosmos-db"></a>
## <a name="try-azure-cosmos-db-subscriptions"></a>Testabonnements für Azure Cosmos DB

Nutzen Sie für eine begrenzte Zeit die Vorteile von Azure Cosmos DB auch ohne Abonnement – kostenlos und ohne jegliche Verpflichtung. Sie können sich unter [Azure Cosmos DB kostenlos testen](https://azure.microsoft.com/try/cosmosdb/) für ein Testabonnement für Azure Cosmos DB registrieren. Dieses Abonnement ist unabhängig von der [kostenlosen Azure-Testversion](https://azure.microsoft.com/free/) und kann zusätzlich zu einer kostenlosen Azure-Testversion oder einem kostenpflichtigen Azure-Abonnement verwendet werden. 

Testabonnements für Azure Cosmos DB werden im Azure-Portal neben den anderen Abonnements zu Ihrer Benutzer-ID angezeigt. 

Die folgenden Bedingungen gelten für Azure Cosmos DB-Testabonnements:

* Ein Container pro Abonnement für SQL- (DocumentDB-API), Gremlin- (Graph-API) und Tabellenkonten
* Bis zu drei Sammlungen pro Abonnement für MongoDB-Konten
* 10 GB Speicherkapazität
* Globale Replikation ist in folgenden [Azure-Regionen](https://azure.microsoft.com/regions/) verfügbar: „USA, Mitte“, „Europa, Norden“ und „Asien, Südosten“
* Maximaler Durchsatz von 5.000 RU/s
* Abonnements laufen nach 24 Stunden ab und können auf maximal 48 Stunden erweitert werden.
* Azure-Supporttickets können nicht für Azure Cosmos DB-Testkonten erstellt werden. Allerdings erhalten Abonnenten mit vorhandenen Supportplänen Support. 

## <a name="set-up-azure-cosmos-db"></a>Einrichten von Azure Cosmos DB
### <a name="how-do-i-sign-up-for-azure-cosmos-db"></a>Wie führe ich die Registrierung für Azure Cosmos DB durch?
Azure Cosmos DB steht im Azure-Portal zur Verfügung. Registrieren Sie sich zuerst für ein Azure-Abonnement. Nach der Registrierung können Sie Ihrem Azure-Abonnement ein Konto für eine DocumentDB-API, Graph-API (Vorschauversion), Table-API (Vorschauversion) oder MongoDB-API hinzufügen.

### <a name="what-is-a-master-key"></a>Was ist ein Hauptschlüssel?
Ein Hauptschlüssel ist ein Sicherheitstoken für den Zugriff auf alle Ressourcen eines Kontos. Personen, die über diesen Schlüssel verfügen, haben Lese- und Schreibzugriff auf alle Ressourcen im Datenbankkonto. Gehen Sie beim Verteilen von Hauptschlüsseln mit Bedacht vor. Der primäre und sekundäre Hauptschlüssel stehen auf dem Blatt **Schlüssel** des [Azure-Portals][azure-portal] zur Verfügung. Weitere Informationen zu Schlüsseln finden Sie unter [Anzeigen, Kopieren und erneutes Generieren von Zugriffsschlüsseln](manage-account.md#keys).

### <a name="what-are-the-regions-that-preferredlocations-can-be-set-to"></a>Auf welche Regionen kann PreferredLocations festgelegt werden? 
Der Wert für PreferredLocations kann auf alle Azure-Regionen festgelegt werden, in denen Cosmos DB verfügbar ist. Eine Liste mit den verfügbaren Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/).

### <a name="is-there-anything-i-should-be-aware-of-when-distributing-data-across-the-world-via-the-azure-datacenters"></a>Gibt es Punkte, die ich beim weltweiten Verteilen von Daten mithilfe der Azure-Datencenter beachten muss? 
Azure Cosmos DB ist in allen Azure-Regionen präsent. Dies ist auf der Seite mit den [Azure-Regionen](https://azure.microsoft.com/regions/) angegeben. Da es sich um den Kerndienst handelt, verfügt jedes neue Datencenter über eine Azure Cosmos DB-Präsenz. 

Beachten Sie beim Festlegen einer Region, dass von Azure Cosmos DB unabhängige Clouds und Government Clouds respektiert werden. Wenn Sie ein Konto in einer unabhängigen Region erstellen, können Sie also keine Replikation aus dieser unabhängigen Region heraus durchführen. Ebenso ist es nicht möglich, die Replikation an anderen unabhängigen Standorten über ein externes Konto zu ermöglichen. 

## <a name="develop-against-the-documentdb-api"></a>Entwickeln für die DocumentDB-API

### <a name="how-do-i-start-developing-against-the-documentdb-api"></a>Wie beginne ich mit dem Entwickeln für die DocumentDB-API?
Die Microsoft DocumentDB-API steht im [Azure-Portal][azure-portal] zur Verfügung. Zuerst müssen Sie sich für ein Azure-Abonnement registrieren. Nachdem Sie sich für ein Azure-Abonnement registriert haben, können Sie Ihrem Azure-Abonnement einen DocumentDB-API-Container hinzufügen. Anweisungen zum Hinzufügen eines Azure Cosmos DB-Kontos finden Sie unter [Erstellen eines Azure Cosmos DB-Datenbankkontos](create-documentdb-dotnet.md#create-account). Wenn Sie bereits über ein DocumentDB-Konto verfügten, sind Sie jetzt im Besitz eines Azure Cosmos DB-Kontos. 

[SDKs](documentdb-sdk-dotnet.md) sind für .NET, Python, Node.js, JavaScript und Java verfügbar. Entwickler können außerdem die [RESTful HTTP-APIs](/rest/api/documentdb/) zur Interaktion mit Azure Cosmos DB-Ressourcen auf unterschiedlichen Plattformen und mit verschiedenen Sprachen nutzen.

### <a name="can-i-access-some-ready-made-samples-to-get-a-head-start"></a>Kann ich auf einige fertige Beispiele als Starthilfe zugreifen?
Beispiele für die [.NET](documentdb-dotnet-samples.md)-, [Java](https://github.com/Azure/azure-documentdb-java)-, [Node.js](documentdb-nodejs-samples.md)- und [Python](documentdb-python-samples.md)-SDKs für die DocumentDB-API sind auf GitHub verfügbar.


### <a name="does-the-documentdb-api-database-support-schema-free-data"></a>Unterstützt die DocumentDB-API-Datenbank schemafreie Daten?
Ja. Die DocumentDB-API ermöglicht Anwendungen das Speichern beliebiger JSON-Dokumente ohne Schemadefinitionen oder -hinweise. Die Daten stehen unmittelbar zur Abfrage über die Azure Cosmos DB-SQL-Abfrageschnittstelle zur Verfügung.  

### <a name="does-the-documentdb-api-support-acid-transactions"></a>Unterstützt die DocumentDB-API ACID-Transaktionen?
Ja. Die DocumentDB-API unterstützt dokumentübergreifende Transaktionen, die in Form von gespeicherten JavaScript-Prozeduren und Triggern ausgedrückt werden. Die Transaktionen werden einer Partition in jeder Sammlung zugeordnet und mit ACID-Semantik nach dem Prinzip „alles oder nichts“ ausgeführt. Sie sind dabei von anderem gleichzeitig ausgeführtem Code und Benutzeranforderungen isoliert. Falls bei der serverseitigen Ausführung des JavaScript-Anwendungscodes ein Ausnahmefehler auftritt, wird für die gesamte Transaktion ein Rollback durchgeführt. Weitere Informationen zu Transaktionen finden Sie unter [Datenbankprogramm-Transaktionen](programming.md#database-program-transactions).

### <a name="what-is-a-collection"></a>Was ist eine Sammlung?
Eine Sammlung ist eine Gruppe von Dokumenten mit der zugehörigen JavaScript-Anwendungslogik. Eine Sammlung ist eine fakturierbare Entität, deren [Kosten](performance-levels.md) vom Durchsatz und belegten Speicher bestimmt werden. Sammlungen können eine oder mehrere Partitionen oder Server umfassen und können skaliert werden, um praktisch unbegrenzte Mengen an Speicher oder Durchsatz zu verarbeiten.

Sammlungen stellen außerdem die Abrechnungseinheiten für Azure Cosmos DB dar. Die Kosten für jede Sammlung werden basierend auf dem bereitgestellten Durchsatz und dem verwendeten Speicherplatz pro Stunde berechnet. Weitere Informationen finden Sie unter [Azure Cosmos DB – Preise](https://azure.microsoft.com/pricing/details/cosmos-db/). 

### <a name="how-do-i-create-a-database"></a>Wie erstelle ich eine Datenbank?
Sie können Datenbanken erstellen, indem Sie das [Azure-Portal](https://portal.azure.com), wie unter [Hinzufügen einer Sammlung](create-documentdb-dotnet.md#create-collection) beschrieben, oder eines der [Azure Cosmos DB SDKs](documentdb-sdk-dotnet.md) oder die [REST-APIs](/rest/api/documentdb/) verwenden. 

### <a name="how-do-i-set-up-users-and-permissions"></a>Wie richte ich Benutzer und Berechtigungen ein?
Sie können Benutzer und Berechtigungen mit einem der [Cosmos DB-API-SDKs](documentdb-sdk-dotnet.md) oder mithilfe der [REST-APIs](/rest/api/documentdb/) erstellen.  

### <a name="does-the-documentdb-api-support-sql"></a>Unterstützt die DocumentDB-API SQL?
Die SQL-Abfragesprache ist eine erweiterte Teilmenge der Abfragefunktionalität, die von SQL unterstützt wird. Die Azure Cosmos DB SQL-Abfragesprache bietet umfassende hierarchische und relationale Operatoren sowie die Erweiterbarkeit über JavaScript-basierte benutzerdefinierte Funktionen. Die JSON-Grammatik ermöglicht die Modellierung von JSON-Dokumenten als Struktur mit beschrifteten Knoten. Diese werden sowohl von den automatischen Indizierungstechniken als auch vom SQL-Abfragedialekt von Azure Cosmos DB verwendet. Weitere Informationen zur Verwendung der SQL-Grammatik finden Sie im Artikel [QueryDocumentDB][query].

### <a name="does-the-documentdb-api-support-sql-aggregation-functions"></a>Werden von der DocumentDB-API SQL-Aggregationsfunktionen unterstützt?
Die DocumentDB-API unterstützt per SQL-Grammatik Aggregation mit geringer Latenz und beliebiger Größe über die Aggregationsfunktionen `COUNT`, `MIN`, `MAX`, `AVG` und `SUM`. Weitere Informationen finden Sie unter [Aggregationsfunktionen](documentdb-sql-query.md#Aggregates).

### <a name="how-does-the-documentdb-api-provide-concurrency"></a>Wie stellt die DocumentDB-API die Parallelität bereit?
Die DocumentDB-API unterstützt die Steuerung für optimistische Parallelität (OCC) durch HTTP-Entitätstags bzw. ETags. Jede DocumentDB-API-Ressource verfügt über ein ETag. Das ETag wird immer dann auf dem Server festgelegt, wenn ein Dokument aktualisiert wird. Der ETag-Header und der aktuelle Wert sind in allen Antwortnachrichten enthalten. ETags können mit dem „If-Match“-Header verwendet werden, um den Server entscheiden zu lassen, ob eine Ressource aktualisiert werden soll. Der „If-Match“-Wert ist der ETag-Wert, für den eine Überprüfung erfolgt. Wenn der ETag-Wert mit dem ETag-Wert des Servers übereinstimmt, wird die Ressource aktualisiert. Wenn das ETag nicht mehr aktuell ist, lehnt der Server den Vorgang mit dem Antwortcode „HTTP 412 – Vorbedingungsfehler“ ab. Der Client ruft anschließend die Ressource erneut ab, um den aktuellen ETag-Wert für die Ressource zu erhalten. Darüber hinaus können ETags mit dem „If-None-Match“-Header verwendet werden, um festzustellen, ob ein erneutes Abrufen einer Ressource erforderlich ist.

Zum Nutzen der optimistischen Nebenläufigkeit in .NET verwenden Sie die [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) -Klasse. Ein Beispiel für .NET finden Sie unter [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) im „DocumentManagement“-Beispiel auf GitHub.

### <a name="how-do-i-perform-transactions-in-the-documentdb-api"></a>Wie führe ich Transaktionen in der DocumentDB-API durch?
Die DocumentDB-API unterstützt sprachintegrierte Transaktionen über gespeicherte JavaScript-Prozeduren und Trigger. Alle Datenbankvorgänge in Skripts werden im Rahmen der Momentaufnahmeisolation durchgeführt. Wenn es sich um eine Sammlung mit nur einer Partition handelt, wird der Umfang der Ausführung an die Sammlung angepasst. Falls die Sammlung partitioniert wird, wird die Ausführung an Dokumente mit dem gleichen Partitionsschlüsselwert in der Sammlung angepasst. Eine Momentaufnahme der Dokumentversionen (ETags) wird zu Beginn der Transaktion angefertigt und erst dann festgeschrieben, wenn das Skript erfolgreich ausgeführt wurde. Falls JavaScript einen Fehler ausgibt, wird für die Transaktion ein Rollback ausgeführt. Weitere Informationen finden Sie unter [Serverseitige JavaScript-Programmierung für Azure Cosmos DB](programming.md).

### <a name="how-can-i-bulk-insert-documents-into-cosmos-db"></a>Wie kann ich eine Masseneinfügung für Dokumente in Cosmos DB ausführen?
Sie haben zwei Möglichkeiten, um in Azure Cosmos DB die Masseneinfügung für Dokumente durchzuführen:

* Mit dem Datenmigrationstool, wie unter [Datenbankmigrationstool für Azure Cosmos DB](import-data.md) beschrieben.
* Mit serverseitigen Verfahren, wie unter [Serverseitige JavaScript-Programmierung für Azure Cosmos DB](programming.md) beschrieben.

### <a name="does-the-documentdb-api-support-resource-link-caching"></a>Unterstützt die DocumentDB-API die Zwischenspeicherung von Ressourcenlinks?
Ja. Da es sich bei Azure Cosmos DB um einen RESTful-Dienst handelt, sind Ressourcenlinks unveränderbar und können zwischengespeichert werden. DocumentDB-API-Clients können einen „If-None-Match“-Header für Lesevorgänge für jede Ressource wie Dokumente oder Sammlungen festlegen und dann ihre lokalen Kopien aktualisieren, nachdem sich die Serverversion geändert hat.

### <a name="is-a-local-instance-of-documentdb-api-available"></a>Ist eine lokale Instanz von DocumentDB-API verfügbar?
Ja. Der [Azure Cosmos DB-Emulator](local-emulator.md) stellt eine High-Fidelity-Emulation des Cosmos DB-Diensts bereit. Es werden die gleichen Funktionen wie bei Azure Cosmos DB unterstützt, z.B. Erstellen und Abfragen von JSON-Dokumenten, Bereitstellen und Skalieren von Sammlungen und Ausführen von gespeicherten Prozeduren und Triggern. Sie können Anwendungen mit dem Azure Cosmos DB-Emulator entwickeln und testen und diese in Azure auf globaler Ebene bereitstellen, indem Sie eine einzige Konfigurationsänderung am Verbindungsendpunkt für Azure Cosmos DB vornehmen.

## <a name="develop-against-the-api-for-mongodb"></a>Entwickeln mit der API für MongoDB
### <a name="what-is-the-azure-cosmos-db-api-for-mongodb"></a>Was ist die Azure Cosmos DB-API für MongoDB?
Die Azure Cosmos DB-API für MongoDB ist eine Kompatibilitätsebene, mit der Anwendungen auf einfache und transparente Weise über vorhandene, von der Community unterstützte Apache MongoDB-APIs und -Treiber mit dem nativen Azure Cosmos DB-Datenbankmodul kommunizieren können. Entwickler können jetzt vorhandene MongoDB-Toolketten und -Fähigkeiten verwenden, um Anwendungen zu erstellen, die Azure Cosmos DB nutzen. Entwickler profitieren von den einzigartigen Funktionen von Azure Cosmos DB, z.B. automatische Indizierung, Sicherungsverwaltung, finanziell abgesicherte Vereinbarungen zum Servicelevel (SLAs) usw.

### <a name="how-do-i-connect-to-my-api-for-mongodb-database"></a>Wie stelle ich eine Verbindung mit meiner API für die MongoDB-Datenbank her?
Die schnellste Möglichkeit zum Herstellen einer Verbindung mit der Azure Cosmos DB-API für MongoDB besteht darin, zum [Azure-Portal](https://portal.azure.com) zu wechseln. Navigieren Sie zu Ihrem Konto, und klicken Sie dann im Navigationsmenü auf der linken Seite auf **Schnellstart**. Der Schnellstart ist die beste Möglichkeit, um Codeausschnitte abzurufen, um eine Verbindung mit Ihrer Datenbank herzustellen. 

Azure Cosmos DB erzwingt strenge Sicherheitsanforderungen und -standards. Azure Cosmos DB-Konten erfordern eine Authentifizierung und eine sichere Kommunikation über SSL. Verwenden Sie daher unbedingt TLSv1.2.

Weitere Informationen finden Sie unter [Verbinden einer MongoDB-App mit einem DocumentDB-Konto mithilfe einer MongoDB-Verbindungszeichenfolge](connect-mongodb-account.md).

### <a name="are-there-additional-error-codes-for-an-api-for-mongodb-database"></a>Gibt es weitere Fehlercodes für eine API für die MongoDB-Datenbank?
Die MongoDB-API verfügt zusätzlich zu den allgemeinen MongoDB-Fehlercodes über eigene, spezifische Fehlercodes:


| Error               | Code  | Beschreibung  | Lösung  |
|---------------------|-------|--------------|-----------|
| TooManyRequests     | 16500 | Die Gesamtanzahl der verbrauchten Anforderungseinheiten hat die bereitgestellte Anforderungseinheitenrate für die Sammlung überschritten und wurde gedrosselt. | Skalieren Sie ggf. im Azure-Portal den Durchsatz der Sammlung, oder versuchen Sie es noch einmal. |
| ExceededMemoryLimit | 16501 | Der Vorgang ist ein mehrinstanzenfähiger Dienst und hat die Speicherzuweisung des Clients überschritten. | Verringern Sie den Umfang des Vorgangs mithilfe restriktiverer Abfragekriterien, oder wenden Sie sich im [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) an den Support. <br><br>Beispiel: *&nbsp;&nbsp;&nbsp;&nbsp;db.getCollection('users').aggregate([<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{$match: {name: "Andy"}}, <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{$sort: {age: -1}}<br>&nbsp;&nbsp;&nbsp;&nbsp;])*) |

## <a name="develop-with-the-table-api-preview"></a>Entwickeln mit der Table-API (Vorschauversion)

### <a name="terms"></a>Begriffe 
Die Table-API (Vorschauversion) von Azure Cosmos DB bezeichnet ein Premium-Angebot von Azure Cosmos DB für die anlässlich der Build 2017 vorgestellte Tabellenunterstützung. 

Beim Standard-Tabellen-SDK handelt es sich um das vorhandene Azure Storage-Tabellen-SDK. 

### <a name="how-can-i-use-the-new-table-api-preview-offering"></a>Wie kann ich das neue Angebot für die Table-API (Vorschauversion) nutzen? 
Die Azure Cosmos DB-Table-API ist im [Azure-Portal][azure-portal] verfügbar. Zuerst müssen Sie sich für ein Azure-Abonnement registrieren. Nach der Registrierung können Sie Ihrem Azure-Abonnement ein Azure Cosmos DB-Table-API-Konto und Ihrem Konto anschließend Tabellen hinzufügen. 

Während des Vorschauzeitraums können Sie, wenn [SDKs](../cosmos-db/table-sdk-dotnet.md) für .NET verfügbar sind, zuerst den Schnellstartartikel zur [Table-API](../cosmos-db/create-table-dotnet.md) durcharbeiten.

### <a name="do-i-need-a-new-sdk-to-use-the-table-api-preview"></a>Benötige ich ein neues SDK, um die Table-API (Vorschauversion) zu verwenden? 
Ja. Das [SDK „Windows Azure Storage Premium Table“ (Vorschau)](https://www.nuget.org/packages/WindowsAzure.Storage-PremiumTable) ist unter NuGet verfügbar. Weitere Informationen finden Sie auf der Seite [Azure Cosmos DB Table .NET API: Download and release notes](https://github.com/Microsoft/azure-docs-pr/cosmos-db/table-sdk-dotnet.md) (Azure Cosmos DB Table .NET API: Download und Versionsinformationen). 

### <a name="how-do-i-provide-feedback-about-the-sdk-or-bugs"></a>Wie kann ich Feedback zum SDK und zu Bugs geben?
Sie können Ihr Feedback wie folgt mitteilen:

* [UserVoice](https://feedback.azure.com/forums/599062-azure-cosmos-db-table-api)
* [MSDN-Forum](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=AzureDocumentDB)
* [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-cosmosdb)

### <a name="what-is-the-connection-string-that-i-need-to-use-to-connect-to-the-table-api-preview"></a>Welche Verbindungszeichenfolge muss ich für das Herstellen von Verbindungen mit der Table-API (Vorschauversion) verwenden?
Die Verbindungszeichenfolge lautet:
```
DefaultEndpointsProtocol=https;AccountName=<AccountNamefromCosmos DB;AccountKey=<FromKeysPaneofCosmosDB>;TableEndpoint=https://<AccountNameFromDocumentDB>.documents.azure.com
```
Sie können die Verbindungszeichenfolge auf der Seite „Schlüssel“ im Azure-Portal abrufen. 

### <a name="how-do-i-override-the-config-settings-for-the-request-options-in-the-new-table-api-preview"></a>Wie setze ich die Konfigurationseinstellungen für die Anforderungsoptionen in der neuen Table-API (Vorschauversion) außer Kraft?
Informationen zu Konfigurationseinstellungen finden Sie unter [Azure Cosmos DB-Funktionen](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities). Sie können die Einstellungen ändern, indem Sie sie im Abschnitt „appSettings“ in der Clientanwendung „app.config“ hinzufügen.

    <appSettings>
        <add key="TableConsistencyLevel" value="Eventual|Strong|Session|BoundedStaleness|ConsistentPrefix"/>
        <add key="TableThroughput" value="<PositiveIntegerValue"/>
        <add key="TableIndexingPolicy" value="<jsonindexdefn>"/>
        <add key="TableUseGatewayMode" value="True|False"/>
        <add key="TablePreferredLocations" value="Location1|Location2|Location3|Location4>"/>....
    </appSettings>


### <a name="are-there-any-changes-for-customers-who-are-using-the-existing-standard-table-sdk"></a>Sind für Kunden, die das vorhandene Standard-Tabellen-SDK verwenden, Änderungen erforderlich?
Keine. Für Bestands- oder Neukunden, die das vorhandene Standard-Tabellen-SDK verwenden, ergeben sich keine Änderungen. 

### <a name="how-do-i-view-table-data-that-is-stored-in-azure-cosmos-db-for-use-with-the-table-api-review"></a>Wie zeige ich Tabellendaten, die in Azure Cosmos DB gespeichert sind, für die Verwendung mit der Table-API (Vorschauversion) an? 
Sie können das Azure-Portal verwenden, um die Daten zu durchsuchen. Außerdem können Sie den Code der Table-API (Vorschauversion) oder die in der nächsten Antwort erwähnten Tools verwenden. 

### <a name="which-tools-work-with-the-table-api-preview"></a>Welche Tools funktionieren in Verbindung mit der Table-API (Vorschauversion)? 
Sie können die ältere Version von Azure Explorer (0.8.9) verwenden.

Tools mit der Flexibilität zum Akzeptieren einer Verbindungszeichenfolge im früher angegebenen Format können die neue Table-API (Vorschauversion) unterstützen. Eine Liste mit Tabellentools steht auf der Seite [Azure Storage-Clienttools](../storage/common/storage-explorers.md) zur Verfügung. 

### <a name="do-powershell-or-azure-cli-work-with-the-new-table-api-preview"></a>Funktioniert PowerShell oder die Azure CLI mit der neuen Table-API (Vorschauversion)?
Wir planen das Hinzufügen der Unterstützung für PowerShell und die Azure CLI für die Table-API (Vorschauversion). 

### <a name="is-the-concurrency-on-operations-controlled"></a>Ist die Parallelität im Betrieb gesteuert?
Ja. Die optimistische Nebenläufigkeit wird über den Einsatz des ETag-Mechanismus bereitgestellt. 

### <a name="is-the-odata-query-model-supported-for-entities"></a>Wird das OData-Abfragemodell für Entitäten unterstützt? 
Ja. Die Table-API (Vorschauversion) unterstützt OData-Abfragen und LINQ-Abfragen. 

### <a name="can-i-connect-to-the-standard-azure-table-and-the-new-premium-table-api-preview-side-by-side-in-the-same-application"></a>Können Verbindungen mit der Azure-Standardtabelle und der neuen Premium-Table-API (Vorschauversion) parallel in der gleichen Anwendung hergestellt werden? 
Ja. Sie können Verbindungen herstellen, indem Sie zwei separate CloudTableClient-Instanzen erstellen, die über die Verbindungszeichenfolge jeweils auf ihren eigenen URI verweisen.

### <a name="how-do-i-migrate-an-existing-azure-table-storage-application-to-this-new-offering"></a>Wie erfolgt die Migration einer vorhandenen Azure-Tabellenspeicher-Anwendung zu diesem neuen Angebot?
Wenden Sie sich an [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com), wenn Sie das neue Table-API-Angebot für Ihre vorhandenen Table Storage-Daten nutzen möchten. 

### <a name="what-is-the-roadmap-for-this-service-and-when-will-you-offer-other-standard-table-api-functionality"></a>Wie lautet die Roadmap-Planung für diesen Dienst, und ab wann wird die Funktionalität der Standard-Table-API angeboten?
Es ist geplant, die Unterstützung für SAS-Token, ServiceContext, Statistik, clientseitige Verschlüsselung, Analyse und weitere Funktionen hinzuzufügen, wenn die Umstellung auf die allgemeine Verfügbarkeit erfolgt. Sie können uns Ihr Feedback über [UserVoice](https://feedback.azure.com/forums/599062-azure-cosmos-db-table-api) zukommen lassen. 

### <a name="how-is-expansion-of-the-storage-size-done-for-this-service-if-for-example-i-start-with-n-gb-of-data-and-my-data-will-grow-to-1-tb-over-time"></a>Wie erfolgt die Erweiterung der Speichergröße für diesen Dienst, wenn ich beispielsweise mit *n* GB Daten beginne und meine Daten im Laufe der Zeit auf 1 TB anwachsen? 
Azure Cosmos DB wurde dafür ausgelegt, mithilfe von horizontaler Skalierung unbeschränkten Speicherplatz zu bieten. Der Dienst kann Ihren Speicher überwachen und effektiv vergrößern. 

### <a name="how-do-i-monitor-the-table-api-preview-offering"></a>Wie lässt sich die Table-API (Vorschauversion) überwachen?
Sie können den Bereich **Metriken** der Table-API (Vorschauversion) verwenden, um Anforderungen und die Speicherbelegung zu überwachen. 

### <a name="how-do-i-calculate-the-throughput-i-require"></a>Wie kann ich den erforderlichen Durchsatz berechnen?
Sie können den Kapazitätskalkulator verwenden, um den für die Vorgänge erforderlichen TableThroughput zu berechnen. Weitere Informationen finden Sie unter [Estimate Request Units and Data Storage](https://www.documentdb.com/capacityplanner) (Schätzen von Anforderungseinheiten und Datenspeicher). Im Allgemeinen können Sie Ihre Entität als JSON-Code darstellen und die Zahlen für Ihre Vorgänge eingeben. 

### <a name="can-i-use-the-new-table-api-preview-sdk-locally-with-the-emulator"></a>Lässt sich das SDK der neuen Table-API (Vorschauversion) lokal mit dem Emulator verwenden?
Ja. Sie können die Table-API (Vorschauversion) mit dem lokalen Emulator verwenden, wenn Sie das neue SDK nutzen. Der Download des neuen Emulators ist unter [Use the Azure Cosmos DB Emulator for local development and testing](local-emulator.md) (Verwenden des Azure Cosmos DB-Emulators für die lokale Entwicklung und das Testen) verfügbar. Der StorageConnectionString-Wert in „app.config“ muss wie folgt lauten:

```
DefaultEndpointsProtocol=https;AccountName=localhost;AccountKey=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==;TableEndpoint=https://localhost:8081`. 
```

### <a name="can-my-existing-application-work-with-the-table-api-preview"></a>Kann meine vorhandene Anwendung mit der Table-API (Vorschauversion) verwendet werden? 
Der Oberflächenbereich der neuen Table-API (Vorschauversion) ist über die Vorgänge zum Erstellen, Löschen, Aktualisieren und Abfragen mit dem vorhandenen Azure Standard-Tabellen-SDK kompatibel. Stellen Sie sicher, dass Sie über einen Zeilenschlüssel verfügen, da für die Table-API (Vorschauversion) sowohl ein Partitionsschlüssel als auch ein Zeilenschlüssel benötigt wird. Außerdem ist geplant, auf dem Weg zur allgemeinen Verfügbarkeit dieses Dienstangebots weitere SDK-Unterstützung hinzuzufügen.

### <a name="do-i-need-to-migrate-my-existing-azure-table-based-applications-to-the-new-sdk-if-i-do-not-want-to-use-the-table-api-preview-features"></a>Muss ich meine auf Azure-Tabellen basierenden Anwendungen zum neuen SDK migrieren, wenn ich die Funktionen der Table-API (Vorschauversion) nicht nutzen möchte?
Nein. Sie können den vorhandenen Standardtabellenbestand ohne irgendwelche Unterbrechungen erstellen und verwenden. Wenn Sie die neue Table-API (Vorschauversion) nicht nutzen, können Sie aber nicht von der automatischen Indizierung, der zusätzlichen Konsistenzoption oder der globalen Verteilung profitieren. 

### <a name="how-do-i-add-replication-of-the-data-in-the-premium-table-api-preview-across-multiple-regions-of-azure"></a>Wie füge ich in der Premium-Table-API (Vorschauversion) die Replikation der Daten über mehrere Regionen von Azure hinzu?
Sie können die [globalen Replikationseinstellungen](tutorial-global-distribution-documentdb.md#portal) im Azure Cosmos DB-Portal verwenden, um Regionen hinzuzufügen, die sich für Ihre Anwendung eignen. Beim Entwickeln einer global verteilten Anwendung sollten Sie Ihre Anwendung darüber hinaus mit einem auf die lokale Region festgelegten Wert von PreferredLocation hinzufügen, um für eine niedrige Leselatenz zu sorgen. 

### <a name="how-do-i-change-the-primary-write-region-for-the-account-in-the-premium-table-api-preview"></a>Wie lässt sich die primäre Schreibregion für das Konto in der Premium-Table-API (Vorschauversion) ändern?
Sie können den Portalbereich für globale Azure Cosmos DB-Replikation verwenden, um eine Region hinzuzufügen, und dann ein Failover in die benötigte Region durchführen. Anweisungen finden Sie unter [Entwickeln für Azure Cosmos DB-Konten für mehrere Regionen](regional-failover.md). 

### <a name="how-do-i-configure-my-preferred-read-regions-for-low-latency-when-i-distribute-my-data"></a>Wie lassen sich beim Verteilen meiner Daten meine bevorzugten Leseregionen konfigurieren, um niedrige Latenz zu erreichen? 
Verwenden Sie den Schlüssel PreferredLocation in der Datei „app.config“, um das Lesen vom lokalen Standort zu unterstützen. Für vorhandene Anwendungen löst die Table-API (Vorschauversion) einen Fehler aus, wenn LocationMode festgelegt ist. Entfernen Sie diesen Code, da die Premium-Table-API (Vorschauversion) diese Informationen aus der Datei „app.config“ auswählt. Weitere Informationen finden Sie unter [Azure Cosmos DB-Funktionen](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities).

### <a name="how-should-i-think-about-consistency-levels-in-the-table-api-preview"></a>Wie kann ich mir die Konsistenzebenen in der Table-API (Vorschauversion) vorstellen? 
Azure Cosmos DB bietet einen wohlüberlegten Kompromiss zwischen Konsistenz, Verfügbarkeit und Wartezeit. Azure Cosmos DB verfügt über fünf Konsistenzebenen für Entwickler, die die Table-API (Vorschauversion) nutzen. Sie können also auf Tabellenebene das richtige Konsistenzmodell auswählen und beim Abfragen der Daten einzelne Anforderungen senden. Wenn ein Client eine Verbindung herstellt, kann er eine Konsistenzebene angeben. Sie können die Ebene über die app.config-Einstellung für den Wert des Schlüssels TableConsistencyLevel ändern. 

Die Table-API (Vorschauversion) bietet Lesevorgänge mit niedriger Latenz und „Eigene Schreibvorgänge lesen“ mit Sitzungskonsistenz als Standardeinstellung. Weitere Informationen finden Sie unter [Konsistenzebenen](consistency-levels.md). 

Standardmäßig wird für Azure-Tabellenspeicher „Starke Konsistenz“ innerhalb einer Region und „Letztliche Konsistenz“ an den sekundären Standorten verwendet. 

### <a name="does-azure-cosmos-db-offer-more-consistency-levels-than-standard-tables"></a>Verfügt Azure Cosmos DB über mehr Konsistenzebenen als Standardtabellen?
Ja. Informationen dazu, wie Sie von der weiten Verteilung von Azure Cosmos DB profitieren können, finden Sie unter [Einstellbare Datenkonsistenzebenen in Azure Cosmos DB](consistency-levels.md). Da für die Konsistenzebenen Garantien gegeben werden, können Sie sie vertrauensvoll nutzen. Weitere Informationen finden Sie unter [Azure Cosmos DB-Funktionen](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities).

### <a name="when-global-distribution-is-enabled-how-long-does-it-take-to-replicate-the-data"></a>Wie viel Zeit wird für die Replikation der Daten benötigt, wenn die globale Verteilung aktiviert ist?
Wir führen für die Daten dauerhaft einen Commit in der lokalen Region durch und übertragen die Daten innerhalb von Millisekunden sofort in andere Regionen. Diese Replikation ist nur von der Roundtripzeit (Round-Trip Time, RTT) des Datencenters abhängig. Weitere Informationen zur Funktion für die globale Verteilung von Azure Cosmos DB finden Sie unter [Wie werden Daten mit Azure Cosmos DB global verteilt?](distribute-data-globally.md).

### <a name="can-the-read-request-consistency-level-be-changed"></a>Kann die Konsistenzebene für Leseanforderungen geändert werden?
Mit Azure Cosmos DB können Sie die Konsistenzebene auf Containerebene (in der Tabelle) festlegen. Mit dem SDK können Sie die Ebene ändern, indem Sie den Wert für den Schlüssel TableConsistencyLevel in der app.config-Datei angeben. Mögliche Werte sind: „Strong“ (Sicher), „Bounded Staleness“ (Begrenzte Veraltung), „Session“ (Sitzung), „Consistent Prefix“ (Präfixkonsistenz) und „Eventual“ (Letztlich). Weitere Informationen finden Sie unter [Einstellbare Datenkonsistenzebenen in Azure Cosmos DB](consistency-levels.md). Der Hauptaspekt hierbei ist, dass Sie die Konsistenzebene für Anforderungen nicht auf einen höheren Wert als für die Einstellung für die Tabelle festlegen können. Beispielsweise ist es nicht möglich, die Konsistenzebene für die Tabelle auf „Letztlich“ und die Konsistenzebene für die Anforderung auf „Sicher“ festzulegen. 

### <a name="how-does-the-premium-table-api-preview-account-handle-failover-if-a-region-goes-down"></a>Wie wird vom Konto für die Premium-Table-API (Vorschauversion) das Failover durchgeführt, wenn es in einer Region zu einem Ausfall kommt? 
Die Premium-Table-API (Vorschauversion) nutzt Teile der global verteilten Plattform von Azure Cosmos DB. Um sicherzustellen, dass Ihre Anwendung Ausfallzeiten des Datencenters tolerieren kann, sollten Sie für das Konto mindestens eine weitere Region im Azure Cosmos DB-Portal aktivieren: [Entwickeln für Azure Cosmos DB-Konten für mehrere Regionen](regional-failover.md). Sie können die Priorität der Region mithilfe des Portals festlegen: [Entwickeln für Azure Cosmos DB-Konten für mehrere Regionen](regional-failover.md). 

Sie können für das Konto beliebig viele Regionen hinzufügen und steuern, wohin das Failover erfolgen kann, indem Sie eine Failoverpriorität festlegen. Für die Verwendung der Datenbank müssen Sie natürlich auch dafür eine Anwendung bereitstellen. Wenn Sie so vorgehen, treten für Ihre Kunden keine Ausfallzeiten auf. Das Client-SDK verfügt über Auto-Homing. Es kann also die ausgefallene Region erkennen und automatisch das Failover in die neue Region durchführen.

### <a name="is-the-premium-table-api-preview-enabled-for-backups"></a>Ist die Premium-Table-API (Vorschauversion) für Sicherungen aktiviert?
Die Premium-Table-API (Vorschauversion) nutzt Teile der Plattform von Azure Cosmos DB für Sicherungen. Sicherungen werden automatisch erstellt. Weitere Informationen finden Sie unter [Automatische Onlinesicherung und -wiederherstellung mit Azure Cosmos DB](online-backup-and-restore.md).

 
### <a name="does-the-table-api-preview-index-all-attributes-of-an-entity-by-default"></a>Führt die Table-API (Vorschauversion) die Indizierung aller Attribute von Entitäten standardmäßig durch?
Ja, alle Attribute einer Entität werden standardmäßig indiziert. Weitere Informationen finden Sie unter [Unterstützen von Indexdaten durch Azure Cosmos DB](indexing-policies.md). 

### <a name="does-this-mean-i-do-not-have-to-create-multiple-indexes-to-satisfy-the-queries"></a>Bedeutet dies, dass ich nicht mehrere Indizes erstellen muss, um Abfragen zu bedienen? 
Ja. Azure Cosmos DB ermöglicht die automatische Indizierung aller Attribute ganz ohne Schemadefinition. Dank dieser Automatisierung können sich Entwickler auf die Anwendung konzentrieren und verlieren keine Zeit mehr mit der Indexerstellung und -verwaltung. Weitere Informationen finden Sie unter [Unterstützen von Indexdaten durch Azure Cosmos DB](indexing-policies.md).

### <a name="can-i-change-the-indexing-policy"></a>Kann ich die Indizierungsrichtlinie ändern?
Ja. Sie können die Indizierungsrichtlinie ändern, indem Sie die Indexdefinition bereitstellen. Weitere Informationen finden Sie unter [Azure Cosmos DB-Funktionen](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities). Sie müssen die Einstellungen richtig codieren und mit Escapezeichen versehen. 

Im JSON-Zeichenfolgenformat in der app.config-Datei:
```
{
  "indexingMode": "consistent",
  "automatic": true,
  "includedPaths": [
    {
      "path": "/somepath",
      "indexes": [
        {
          "kind": "Range",
          "dataType": "Number",
          "precision": -1
        },
        {
          "kind": "Range",
          "dataType": "String",
          "precision": -1
        } 
      ]
    }
  ],
  "excludedPaths": 
[
 {
      "path": "/anotherpath"
 }
]
}
```

### <a name="azure-cosmos-db-as-a-platform-seems-to-have-lot-of-capabilities-such-as-sorting-aggregates-hierarchy-and-other-functionality-will-you-be-adding-these-capabilities-to-the-table-api"></a>Azure Cosmos DB als Plattform verfügt scheinbar über viele Funktionen, z.B. Sortierung, Aggregate, Hierarchie und andere. Werden diese Funktionen der Table-API hinzugefügt? 
In der Vorschauversion werden mit der Table-API die gleichen Abfragefunktionen wie für Azure-Tabellenspeicher bereitgestellt. Azure Cosmos DB unterstützt auch Sortieren, Aggregieren, räumliche Abfrage, Hierarchie und eine Vielzahl von integrierten Funktionen. Wir stellen die zusätzliche Funktionalität in einem zukünftigen Dienstupdate der Table-API bereit. Weitere Informationen finden Sie unter [SQL-Abfragen für die Azure Cosmos DB-DocumentDB-API](../documentdb/documentdb-sql-query.md).
 
### <a name="when-should-i-change-tablethroughput-for-the-table-api-preview"></a>Wann sollte TableThroughput für die Table-API (Vorschauversion) geändert werden?
Sie sollten TableThroughput ändern, wenn eine der folgenden Bedingungen gilt:
* Sie führen das Extrahieren, Transformieren und Laden (ETL) für die Daten durch, oder Sie möchten innerhalb eines kurzen Zeitraums viele Daten hochladen. 
* Sie benötigen für den Container auf dem Back-End mehr Durchsatz. Beispielsweise erkennen Sie, dass der genutzte Durchsatz den bereitgestellten Durchsatz übersteigt, und es kommt zu einer Drosselung. Weitere Informationen finden Sie unter [Festlegen von Durchsatz für Azure Cosmos DB-Container](set-throughput.md).

### <a name="can-i-scale-up-or-scale-down-the-throughput-of-my-table-api-preview-table"></a>Lässt sich der Durchsatz meiner Tabelle der Table-API (Vorschauversion) zentral hoch- oder herunterskalieren? 
Ja. Sie können den Skalierungsbereich des Azure Cosmos DB-Portals verwenden, um den Durchsatz zu skalieren. Weitere Informationen finden Sie unter [Festlegen von Durchsatz für Azure Cosmos DB-Container](set-throughput.md).

### <a name="is-a-default-tablethroughput-set-for-newly-provisioned-tables"></a>Wird für neu bereitgestellte Tabellen ein TableThroughput-Standardwert festgelegt?
Ja. Wenn Sie TableThroughput nicht mithilfe von „app.config“ außer Kraft setzen und keinen fertig konfigurierten Container in Azure Cosmos DB verwenden, erstellt der Dienst eine Tabelle mit einem Durchsatz von 400.
 
### <a name="is-there-any-change-of-pricing-for-existing-customers-of-the-standard-table-api"></a>Gibt es Preisänderungen für Bestandskunden der Standard-Table-API?
Keine. Es gibt keine Preisänderungen für bestehende Kunden der Standard-Table-API. 

### <a name="how-is-the-price-calculated-for-the-table-api-preview"></a>Wie wird der Preis für die Table-API (Vorschauversion) berechnet? 
Der Preis hängt vom zugewiesenen TableThroughput-Wert ab. 

### <a name="how-do-i-handle-any-throttling-on-the-tables-in-table-api-preview-offering"></a>Wie kann ich beim Angebot „Table-API“ (Vorschauversion) die Drosselung für die Tabellen durchführen? 
Wenn die Anforderungsrate die Kapazität des bereitgestellten Durchsatzes für den zugrundeliegenden Container überschreitet, empfangen Sie einen Fehler, und das SDK wiederholt den Aufruf, indem die Wiederholungsrichtlinie angewendet wird.

### <a name="why-do-i-need-to-choose-a-throughput-apart-from-partitionkey-and-rowkey-to-take-advantage-of-the-premium-table-api-preview-offering-of-azure-cosmos-db"></a>Warum muss ich über PartitionKey und RowKey hinaus einen Durchsatz festlegen, um das Angebot „Premium-Table-API“ (Vorschauversion) von Azure Cosmos DB nutzen zu können?
Azure Cosmos DB legt einen Standarddurchsatz für Ihren Container fest, wenn Sie in der Datei „app.config“ hierzu keine Angabe machen. 

Azure Cosmos DB bietet Garantien für Leistung und Latenz mit Obergrenzen im Betrieb. Diese Garantie ist möglich, wenn das Modul die Kontrolle für die Vorgänge des Mandanten erzwingen kann. Durch das Festlegen von TableThroughput wird sichergestellt, dass Sie den garantierten Durchsatz und die Wartezeit auch erhalten, da diese Kapazität von der Plattform reserviert wird und somit der Erfolg des Vorgangs sichergestellt ist. 

Mithilfe der Durchsatzspezifikation können Sie dies flexibel ändern, um von der Saisonalität Ihrer Anwendung zu profitieren, die Durchsatzanforderungen zu erfüllen und Kosten zu sparen.

### <a name="azure-storage-sdk-has-been-very-inexpensive-for-me-because-i-pay-only-to-store-the-data-and-i-rarely-query-the-new-azure-cosmos-db-offering-seems-to-be-charging-me-even-though-i-have-not-performed-a-single-transaction-or-stored-anything-can-you-please-explain"></a>Das Azure Storage SDK war für mich sehr kostengünstig, da ich nur für die Speicherung der Daten zahle und nur selten Abfragen durchführe. Für das neue Azure Cosmos DB-Angebot fallen für mich scheinbar auch dann Kosten an, wenn ich keine einzige Transaktion durchgeführt und keine Daten gespeichert habe. Können Sie mir erklären, warum dies so ist?

Azure Cosmos DB wurde als global verteiltes, SLA-basiertes System mit Garantien für Verfügbarkeit, Wartezeit und Durchsatz entwickelt. Wenn Sie den Durchsatz in Azure Cosmos DB reservieren, ist er – im Gegensatz zum Durchsatz anderer Systeme – garantiert. Mit Azure Cosmos DB werden zusätzliche Funktionen bereitgestellt, die von Kunden gefordert wurden, z.B. sekundäre Indizes und globale Verteilung. Während des Vorschauzeitraums bieten wir ein für den Durchsatz optimiertes Modell an, und auf lange Sicht möchten wir ein speicheroptimiertes Modell anbieten, um die Anforderungen unserer Kunden zu erfüllen. 

### <a name="i-never-get-a-quota-full-notification-indicating-that-a-partition-is-full-when-i-ingest-data-into-table-storage-with-the-table-api-preview-i-do-get-this-message-is-this-offering-limiting-me-and-forcing-me-to-change-my-existing-application"></a>Ich habe die Benachrichtigung „Kontingent erschöpft“ (als Information, dass eine Partition voll ist) beim Erfassen von Daten im Tabellenspeicher nie erhalten. Bei der Table-API (Vorschauversion) erhalte ich diese Nachricht. Schränkt mich dieses Angebot ein, und zwingt es mich zum Ändern meiner vorhandenen Anwendung?

Azure Cosmos DB ist ein SLA-basiertes System, das unbeschränkte Skalierung mit Garantien für Wartezeit, Durchsatz, Verfügbarkeit und Konsistenz bietet. Achten Sie darauf, dass Ihre Datengröße und der Index verwaltbar und skalierbar sind, um sicherzustellen, dass Sie die garantierte Premium-Leistung erhalten. Mit dem 10 GB-Grenzwert für die Anzahl von Entitäten bzw. Elementen pro Partitionsschlüssel wird dafür gesorgt, dass wir eine hervorragende Such- und Abfrageleistung bieten können. Um sicherzustellen, dass Ihre Anwendung gut skaliert werden kann, raten wir Ihnen auch bei Azure Storage, *keine* Hot Partition zu erstellen, indem Sie alle Informationen in einer Partition speichern und abfragen. 

### <a name="so-partitionkey-and-rowkey-are-still-required-with-the-new-table-api-preview"></a>Also sind PartitionKey und RowKey auch bei der neuen Table-API (Vorschauversion) erforderlich? 
Ja. Da der Oberflächenbereich der Table-API (Vorschauversion) dem des Table Storage-SDK ähnelt, stellt der Partitionsschlüssel ein effizientes Verfahren zum Verteilen der Daten dar. Der Zeilenschlüssel ist innerhalb dieser Partition eindeutig. Der Zeilenschlüssel muss vorhanden sein und darf nicht wie beim Standard-SDK NULL sein. Die RowKey-Länge beträgt 255 Byte und die PartitionKey-Länge 100 Byte (die aber bald auf 1 KB heraufgesetzt wird). 

### <a name="what-are-the-error-messages-for-the-table-api-preview"></a>Wie lauten die Fehlermeldungen für die Table-API (Vorschauversion)?
Da diese Vorschauversion mit der Standardtabelle kompatibel ist, sind die meisten Fehler den Fehlern der Standardtabelle zuzuordnen. 

### <a name="why-do-i-get-throttled-when-i-try-to-create-lot-of-tables-one-after-another-in-the-table-api-preview"></a>Warum tritt für mich eine Drosselung ein, wenn ich versuche, in der Table-API (Vorschauversion) nacheinander eine große Zahl von Tabellen zu erstellen?
Azure Cosmos DB ist ein SLA-basiertes System, das Garantien für Wartezeit, Durchsatz, Verfügbarkeit und Konsistenz bietet. Da es sich um ein bereitgestelltes System handelt, werden Ressourcen reserviert, um diese Anforderungen zu garantieren. Eine hohe Erstellungsrate von Tabellen in rascher Folge wird erkannt und gedrosselt. Es wird empfohlen, dass Sie sich die Erstellungsrate der Tabellen ansehen und auf weniger als „5 pro Minute“ reduzieren. Bedenken Sie, dass die Table-API (Vorschauversion) ein bereitgestelltes System ist. Ab dem Moment, in dem Sie es bereitstellen, werden Gebühren fällig. 

## <a name="develop-against-the-graph-api-preview"></a>Entwickeln für die Graph-API (Vorschauversion)
### <a name="how-can-i-apply-the-functionality-of-graph-api-preview-to-azure-cosmos-db"></a>Wie kann ich die Funktionalität der Graph-API (Vorschauversion) auf Azure Cosmos DB anwenden?
Sie können eine Erweiterungsbibliothek verwenden, um die Funktionalität der Graph-API (Vorschauversion) anzuwenden. Diese Bibliothek wird als Microsoft Azure Graphs bezeichnet und ist über NuGet verfügbar. 

### <a name="it-looks-like-you-support-the-gremlin-graph-traversal-language-do-you-plan-to-add-more-forms-of-query"></a>Anscheinend wird die Gremlin-Graphdurchlauf-Sprache unterstützt. Ist geplant, weitere Abfrageformen hinzuzufügen?
Ja. Für die Zukunft ist die Hinzufügung von anderen Abfragemechanismen geplant. 

### <a name="how-can-i-use-the-new-graph-api-preview-offering"></a>Wie kann ich das neue Graph-API-Angebot (Vorschauversion) nutzen? 
Arbeiten Sie den Schnellstartartikel [Azure Cosmos DB: Erstellen einer .NET-Anwendung mit der Graph-API](../cosmos-db/create-graph-dotnet.md) durch.

<a id="moving-to-cosmos-db"></a>
## <a name="questions-from-documentdb-customers"></a>Fragen von DocumentDB-Kunden
### <a name="why-are-you-moving-to-azure-cosmos-db"></a>Warum wird die Umstellung auf Azure Cosmos DB durchgeführt? 

Azure Cosmos DB steht für den nächsten großen Schritt in Bezug auf global verteilte, bedarfsorientierte Clouddatenbanken. Als DocumentDB-Kunde haben Sie jetzt Zugang zu diesem bahnbrechenden System und den Funktionen von Azure Cosmos DB.

Azure Cosmos DB wurde 2010 als „Project Florence“ eingeführt, um die Problembereiche zu beseitigen, mit denen sich Entwickler beim Erstellen umfangreicher Anwendungen bei Microsoft konfrontiert sahen. Die großen Herausforderungen beim Erstellen von global verteilten Apps gelten nicht nur für Microsoft, sodass wir im Jahr 2015 die erste Generation dieser Technologie als „Azure DocumentDB“ für Azure-Entwickler zur Verfügung gestellt haben. 

Seit der Einführung haben wir neue Features hinzugefügt und bemerkenswerte neue Funktionen vorgestellt. Das Ergebnis dieser Arbeit ist Azure Cosmos DB. Im Rahmen dieser Version werden DocumentDB-Kunden (mit ihren Daten) automatisch und nahtlos zu Azure Cosmos DB-Kunden. Diese Funktionen beziehen sich auf die Bereiche des Kerndatenbankmoduls sowie auf die globale Verteilung, die elastische Skalierbarkeit und branchenführende, umfassende SLAs. Wir haben das Azure Cosmos DB-Datenbankmodul so weiterentwickelt, dass alle beliebten Datenmodelle, Typsysteme und APIs dem zugrunde liegenden Datenmodell von Azure Cosmos DB effizient zugeordnet werden. 

Für Entwickler ist dies derzeit durch die neue Unterstützung für [Gremlin](../cosmos-db/graph-introduction.md) und [Table Storage-APIs](../cosmos-db/table-introduction.md) erkennbar. Und dies ist erst der Anfang. Wir planen, im Laufe der Zeit weitere beliebte APIs und neuere Datenmodelle mit Verbesserungen in Bezug auf die globale Leistung und Speicherung hinzuzufügen. 

In diesem Zusammenhang ist der Hinweis wichtig, dass der [SQL-Dialekt](../documentdb/documentdb-sql-query.md) von DocumentDB schon immer nur eine der vielen APIs gewesen ist, die von der zugrunde liegenden Azure Cosmos DB-Technologie unterstützt werden können. Für Entwickler, die einen vollständig verwalteten Dienst wie Azure Cosmos DB nutzen, stellen die vom Dienst verfügbar gemachten APIs die einzige Schnittstelle zum Dienst dar. Für vorhandene DocumentDB-Kunden ergeben sich keine größeren Änderungen. In Azure Cosmos DB erhalten Sie genau die gleiche SQL-API wie bei DocumentDB. Zudem können Sie jetzt (und in Zukunft) auf andere Funktionen zugreifen, die vorher nicht zugänglich waren. 

Ein weiterer Beweis für unsere fortwährende Arbeit ist das erweiterte Fundament für die globale und elastische Skalierbarkeit des Durchsatzes und Speichers. Wir haben am globalen Verteilungssystem einige grundlegende Verbesserungen vorgenommen. Eines von vielen Features dieser Art für Entwickler ist das Präfixkonsistenz-Modell, sodass sich insgesamt fünf gut definierte Konsistenzmodelle ergeben. Wir werden noch viele weitere interessante Funktionen bereitstellen, sobald sie für die Veröffentlichung geeignet sind. 

### <a name="what-do-i-need-to-do-to-ensure-that-my-documentdb-resources-continue-to-run-on-azure-cosmos-db"></a>Was muss ich tun, um sicherzustellen, dass meine DocumentDB-Ressourcen unter Azure Cosmos DB weiterhin ausgeführt werden können?

Sie müssen gar keine Änderungen vornehmen. Ihre DocumentDB-Ressourcen sind jetzt Azure Cosmos DB-Ressourcen. Für diese Umstellung war keine Unterbrechung des Diensts erforderlich.

### <a name="what-changes-do-i-need-to-make-for-my-app-to-work-with-azure-cosmos-db"></a>Welche Änderungen muss ich vornehmen, damit meine App mit Azure Cosmos DB funktioniert?

Es sind keine Änderungen erforderlich. Die Namen von Klassen, Namespaces und NuGet-Paketen haben sich nicht geändert. Wie immer gilt die Empfehlung, dass Sie Ihre SDKs aktuell halten sollten, um die neuesten Features und Verbesserungen nutzen zu können. 

### <a name="whats-changed-in-the-azure-portal"></a>Was hat sich im Azure-Portal geändert?

DocumentDB wird im Portal nicht mehr als Azure-Dienst angezeigt. Stattdessen wird ein neues Azure Cosmos DB-Symbol angezeigt, wie in der folgenden Abbildung dargestellt. Ihre Sammlungen sind wie gewohnt verfügbar, und Sie können weiterhin den Durchsatz skalieren, die Konsistenzebenen ändern und SLAs überwachen. Die Funktionen des Daten-Explorers (Vorschauversion) wurden verbessert. Sie können nun Dokumente anzeigen und bearbeiten, Abfragen erstellen und ausführen und auf einer Seite mit gespeicherten Prozeduren, Triggern und UDFs arbeiten. Dies ist in der folgenden Abbildung dargestellt: 

![Blatt „Sammlungen“ von Azure Cosmos DB](./media/faq/cosmos-db-data-explorer.png)

### <a name="are-there-changes-to-pricing"></a>Haben sich die Preise geändert?

Nein. Die Kosten für die Ausführung Ihrer App unter Azure Cosmos DB sind genauso hoch wie vorher.

### <a name="are-there-changes-to-the-slas"></a>Haben sich die SLAs geändert?

Nein. Die SLAs für Verfügbarkeit, Konsistenz, Wartezeit und Durchsatz sind unverändert geblieben und werden im Portal weiterhin angezeigt. Weitere Informationen finden Sie unter [SLA für Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/).
   
![To-Do-App mit Beispieldaten](./media/faq/azure-cosmosdb-portal-metrics-slas.png)


[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md

