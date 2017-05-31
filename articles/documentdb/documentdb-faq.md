---
title: "Datenbankfragen zu Azure Cosmos DB – Häufig gestellte Fragen | Microsoft-Dokumentation"
description: "Erhalten Sie Antworten auf häufig gestellte Fragen zu Azure Cosmos DB, einem weltweit verteilten Datenbankdienst, der Unterstützung für mehrere Datenbankmodelle bietet. Beantworten Sie Datenbankfragen zu Kapazität, Leistung und Skalierung."
keywords: "Datenbankfragen,häufig gestellte Fragen,DocumentDB,Azure,Microsoft Azure"
services: cosmosdb
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: b68d1831-35f9-443d-a0ac-dad0c89f245b
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 687690fc3b99d52d7c822b7b7ff71c282541be6d
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="frequently-asked-questions-about-azure-cosmos-db"></a>Häufig gestellte Fragen zu Azure Cosmos DB
## <a name="database-questions-about-microsoft-azure-cosmos-db-fundamentals"></a>Datenbankfragen zu den Grundlagen von Microsoft Azure Cosmos DB
### <a name="what-is-microsoft-azure-cosmos-db"></a>Was ist Microsoft Azure Cosmos DB?
Microsoft Azure Cosmos DB ist ein global replizierter Datenbankdienst, der mehrere Modelle unterstützt und umfangreiche Abfragen von nicht schematisierten Daten bietet, konfigurierbare, zuverlässige Leistung zur Verfügung stellt und eine schnelle Bereitstellung ermöglicht – und all das auf einer verwalteten Plattform, die auf der Leistungsfähigkeit und Reichweite von Microsoft Azure basiert. Azure Cosmos DB ist die geeignete Lösung für Web-, Mobil-, Gaming- und IoT-Anwendungen, wenn vorhersagbarer Durchsatz, hohe Verfügbarkeit, niedrige Latenz und schemafreie Datenmodelle wichtige Anforderungen sind. Azure Cosmos DB bietet Schemaflexibilität und umfassende Indizierung und beinhaltet die Transaktionsunterstützung mehrerer Dokumente durch integriertes JavaScript.  

Azure Cosmos DB wurde Ende 2010 eingeführt, um die Problembereiche zu beseitigen, mit denen sich Entwickler umfangreicher Anwendungen bei Microsoft konfrontiert sahen. Da die Erstellung weltweit verteilter Anwendungen nicht nur für Microsoft Probleme mit sich bringt, haben wir den Dienst extern allen Azure-Entwicklern in Form von Azure DocumentDB zur Verfügung gestellt. Mit Azure Cosmos DB ist der nächste große Schritt in der Entwicklung von DocumentDB geschafft, daher machen wir Azure Cosmos DB jetzt für Sie verfügbar. Im Rahmen dieser Version von Azure Cosmos DB werden DocumentDB-Kunden (mit ihren Daten) automatisch zu Azure Cosmos DB-Kunden. Der Übergang erfolgt nahtlos, und Sie erhalten nun Zugriff auf eine breitere Palette neuer Funktionen von Azure Cosmos DB. 

Weitere Datenbankfragen, -antworten und -anweisungen zur Bereitstellung und Verwendung des Diensts finden Sie auf der Seite [Azure Cosmos DB-Dokumentation](https://azure.microsoft.com/documentation/services/documentdb/).

### <a name="what-happened-to-documentdb"></a>Was ist mit DocumentDB passiert?
Die DocumentDB-API stellt eine der unterstützten APIs und Datenmodelle für Azure-Cosmos DB dar. Darüber hinaus unterstützt Azure Cosmos DB Sie mit Graph-API (Vorschauversion), Table-API (Vorschauversion) und MongoDB-API. 

### <a name="how-do-i-get-to-my-documentdb-account-in-the-azure-portal"></a>Wie gelange ich im Azure-Portal an mein DocumentDB-Konto?
Klicken Sie einfach im linken Menü im Azure-Portal auf das Azure Cosmos DB-Symbol. Wenn Sie zuvor über ein DocumentDB-Konto verfügten, besitzen Sie nun ein Azure Cosmos DB-Konto. An der Abrechnung ändert sich nichts.

### <a name="what-are-the-typical-use-cases-for-azure-cosmos-db"></a>Was sind die typischen Anwendungsfälle für Azure Cosmos DB?
Azure Cosmos DB ist eine gute Wahl für neue Web-, Mobil-, Gaming- und IoT-Anwendungen, bei denen Dinge wie automatische Skalierung, vorhersagbare Leistung, kurze Reaktionszeiten im Millisekundenbereich und die Möglichkeit zum Abfragen von schemalosen Daten wichtig sind. Azure Cosmos DB eignet sich für schnelle Entwicklungen und die Unterstützung kontinuierlicher Iterationen von Anwendungsdatenmodellen.  Anwendungen, die von Benutzern erzeugte Inhalte und Daten verwalten, sind ein [häufiger Anwendungsfall von Azure Cosmos DB](documentdb-use-cases.md).  

### <a name="how-does-azure-cosmos-db-offer-predictable-performance"></a>Wie wird bei Azure Cosmos DB für eine vorhersagbare Leistung gesorgt?
In Azure Cosmos DB wird der Durchsatz in [Anforderungseinheiten](documentdb-request-units.md) gemessen. 1 Anforderungseinheit entspricht dem Durchsatz der GET-Anforderung für ein Dokument mit einer Größe von 1 KB. Jeder Vorgang in Azure Cosmos DB (einschließlich der Ausführung von Lese- und Schreibvorgängen, SQL-Abfragen sowie gespeicherten Prozeduren) verfügt über einen deterministischen Wert für die Anforderungseinheiten, der auf dem erforderlichen Durchsatz zum Abschließen des Vorgangs basiert. Im Hinblick auf den Durchsatz Ihrer Anwendung müssen Sie sich also keine Gedanken um das Zusammenspiel von CPU, E/A-Leistung und Arbeitsspeicher machen, sondern können mit einer einzigen Kennzahl arbeiten: der Anforderungseinheit.

Jeder Azure Cosmos DB-Container kann mit bereitgestelltem Durchsatz reserviert werden (Anforderungseinheiten des Durchsatzes pro Sekunde). Für Anwendungen jeder Größe können Sie Vergleichstests einzelner Anforderungen durchführen, um deren Werte für die Anforderungseinheiten zu messen. Außerdem können Sie Container bereitstellen, um die Gesamtsumme der Anforderungseinheiten über alle Anforderungen hinweg zu verarbeiten. Sie können den Durchsatz des Containers auch zentral hoch- oder herunterskalieren, wenn sich die Anforderungen Ihrer Anwendung ändern. Weitere Informationen zu Anforderungseinheiten und Hilfe zum Ermitteln Ihrer Containeranforderungen finden Sie unter [Schätzen der Durchsatzanforderungen](documentdb-request-units.md#estimating-throughput-needs). Probieren Sie außerdem den [Durchsatzrechner](https://www.documentdb.com/capacityplanner) aus. Container werden hier als Sammelbegriff für die Sammlungen der DocumentDB-API, die Diagramme von Graph-API, Sammlungen von MongoDB-API und Tabellen von Table-API verwendet.  


### <a name="is-azure-cosmos-db-hipaa-compliant"></a>Ist Azure Cosmos DB HIPAA-kompatibel?
Ja, Azure Cosmos DB ist HIPAA-kompatibel. HIPAA gibt Anforderungen für die Verwendung, Offenlegung und den Schutz von individuell identifizierbaren Gesundheitsinformationen vor. Weitere Informationen finden Sie im [Microsoft Trust Center](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA).

### <a name="what-are-the-storage-limits-of-azure-cosmos-db"></a>Wie lauten die Speicherbeschränkungen von Azure Cosmos DB?
Es gibt keine Beschränkung in Bezug auf die Gesamtmenge der Daten, die von einem Container in Azure Cosmos DB gespeichert werden können.

### <a name="what-are-the-throughput-limits-of-azure-cosmos-db"></a>Wo liegen die Durchsatzgrenzwerte von Azure Cosmos DB?
Es gibt kein Limit für den Gesamtdurchsatz, der von einem Container in Azure Cosmos DB unterstützt werden kann. Der kritische Punkt liegt in der ungefähr gleichmäßigen Verteilung des Workloads auf eine ausreichend große Zahl von Partitionsschlüsseln.

### <a name="how-much-does-microsoft-azure-cosmos-db-cost"></a>Wie viel kostet Microsoft Azure Cosmos DB?
Ausführliche Informationen dazu finden Sie auf der Seite [Azure Cosmos DB – Preise](https://azure.microsoft.com/pricing/details/documentdb/). Die Nutzungsgebühren für Azure Cosmos DB werden von der Anzahl von bereitgestellten Containern, der Anzahl der Stunden, die die Container online waren, und dem bereitgestellten Durchsatz für jeden Container bestimmt. Container werden hier als Sammelbegriff für die Sammlungen der DocumentDB-API, die Diagramme von Graph-API, Sammlungen von MongoDB-API und Tabellen von Table-API verwendet. 

### <a name="is-there-a-free-account-available"></a>Ist ein kostenloses Konto verfügbar?
Wenn Azure für Sie neu ist, können Sie sich für ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) registrieren. Sie erhalten eine Gutschrift von 200 US-Dollar für 30 Tage, in denen Sie alle Azure-Dienste ausprobieren können. Wenn Sie ein Visual Studio-Abonnement besitzen, haben Sie Anspruch auf ein [kostenloses Azure-Guthaben von 150 US-Dollar pro Monat](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), das Sie für beliebige Azure-Dienste nutzen können.  

Sie können auch den [Azure Cosmos DB-Emulator](documentdb-nosql-local-emulator.md) zum kostenlosen lokalen Entwickeln und Testen Ihrer Anwendung verwenden, ohne ein Azure-Abonnement zu erstellen. Wenn Sie mit der Funktion der Anwendung im Azure Cosmos DB-Emulator zufrieden sind, können Sie zur Verwendung eines Azure Cosmos DB-Kontos in der Cloud wechseln.

### <a name="how-can-i-get-additional-help-with-azure-cosmos-db"></a>Wie kann ich zusätzliche Hilfe zu Azure Cosmos DB erhalten?
Wenn Sie Hilfe benötigen, wenden Sie sich auf [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-documentdb), im [MSDN-Forum](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=AzureDocumentDB) an uns, oder terminieren Sie einen 1:1-Chat mit dem Entwicklungsteam von Azure Cosmos DB, indem Sie eine E-Mail an [askdocdb@microsoft.com](mailto:askdocdb@microsoft.com) senden. 

## <a name="set-up-microsoft-azure-cosmos-db"></a>Einrichten von Microsoft Azure Cosmos DB
### <a name="how-do-i-sign-up-for-microsoft-azure-cosmos-db"></a>Wie registriere ich mich für Microsoft Azure Cosmos DB?
Microsoft Azure Cosmos DB steht im Azure-Portal zur Verfügung. Zuerst müssen Sie sich für ein Azure-Abonnement anmelden. Nachdem Sie sich für ein Microsoft Azure-Abonnement registriert haben, können Sie Ihrem Azure-Abonnement ein Konto für die DocumentDB-API, Graph-API (Vorschauversion), Table-API (Vorschauversion) oder MongoDB-API hinzufügen.

### <a name="what-is-a-master-key"></a>Was ist ein Hauptschlüssel?
Ein Hauptschlüssel ist ein Sicherheitstoken für den Zugriff auf alle Ressourcen eines Kontos. Personen, die über diesen Schlüssel verfügen, haben Lese- und Schreibzugriff auf alle Ressourcen im Datenbankkonto. Gehen Sie beim Verteilen der Hauptschlüssel mit Bedacht vor. Der primäre und sekundäre Hauptschlüssel stehen auf dem Blatt **Schlüssel** im [Azure-Portal][azure-portal] zur Verfügung. Weitere Informationen zu Schlüsseln finden Sie unter [Anzeigen, Kopieren und erneutes Generieren von Zugriffsschlüsseln](documentdb-manage-account.md#keys).

### <a name="what-are-the-regions-to-which-preferredlocations-be-set-to"></a>Auf welche Regionen kann PreferredLocations festgelegt werden? 
Aktuell sind diese Regionen für PrefferredLocations gültig: „USA, Westen“, „ USA, Westen-Mitte“, „USA, Westen 2“, „USA, Osten“, „USA, Osten 2“, „USA, Mitte“, „USA, Süden-Mitte“, „USA, Norden-Mitte“, „Europa, Westen“, „Europa, Norden“, „Asien, Osten“, „Asien, Südosten“, „Japan, Westen“, „Japan, Osten“, „Australien, Südosten“, „Australien, Osten“, „Indien, Mitte“, „Indien, Süden“, „Indien, Westen“, „Kanada, Osten“, „Kanada, Mitte“, „Deutschland, Mitte“, „Deutschland, Nordosten“,„China, Norden“, „China, Osten“, "Korea, Süden“, „Korea, Mitte“,„Vereinigtes Königreich, Westen“, „Vereinigtes Königreich, Süden“, „Brasilien, Süden“, „USGov, Arizona“, „USGov, Texas“.

### <a name="is-there-something-i-should-be-aware-of-when-distributing-data-across-the-world-via-azures-data-centers"></a>Gibt es Punkte, die ich beim weltweiten Verteilen von Daten mithilfe der Azure-Rechenzentren beachten muss? 
Cosmos DB ist übergreifend in allen Regionen vorhanden. Da es sich um den Kerndienst handelt, wird jedes neue Rechenzentrum ebenfalls über Cosmos DB verfügen. Der heutige Stand ist in der voranstehenden Liste dokumentiert. Beim Festlegen der Regionen müssen Sie sich bewusst sein, dass Cosmos DB souveräne und Behördenclouds respektiert. Dies bedeutet, dass Ihnen mit einem dort erstellten Konto die Replikation nach außen verweigert wird – es wird Ihnen nicht gestattet, wie Sie analog auch nicht mit einem Konto von außerhalb in diese Speicherorte gelangen können, um dort Replikation einzurichten. 



### <a name="do-you-plan-to-provide-more-price-options-in-the-future"></a>Werden Sie in Zukunft weitere Preisoptionen zur Verfügung stellen?
Ja, Cosmos DB stellt heute ein durchsatzoptimiertes Modell bereit. Wir beabsichtigen, in naher Zukunft weitere optimierte Preismodelle zur Verfügung zu stellen. 

 
# <a name="documentdb-api"></a>DocumentDB-API 
## <a name="database-questions-about-developing-against-documentdb-api"></a>Datenbankfragen zum Entwickeln für die DocumentDB-API

### <a name="how-to-do-i-start-developing-against-documentdb-api"></a>Wie beginne ich mit dem Entwickeln für die DocumentDB-API?
Die Microsoft DocumentDB-API steht im [Azure-Portal][azure-portal] zur Verfügung.  Zuerst müssen Sie sich für ein Azure-Abonnement anmelden.  Nachdem Sie sich für ein Microsoft Azure-Abonnement registriert haben, können Sie Ihrem Azure-Abonnement einen DocumentDB-API-Container hinzufügen. Anweisungen zum Hinzufügen eines Azure Cosmos DB-Kontos finden Sie unter [Erstellen eines Azure Cosmos DB-Datenbankkontos](documentdb-create-account.md). Wenn Sie bereits über ein DocumentDB-Konto verfügten, sind Sie jetzt im Besitz eines Azure Cosmos DB-Kontos.  

[SDKs](documentdb-sdk-dotnet.md) sind für .NET, Python, Node.js, JavaScript und Java verfügbar.  Entwickler können außerdem die [RESTful HTTP-APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx) zur Interaktion mit Azure Cosmos DB-Ressourcen auf unterschiedlichen Plattformen und mit verschiedenen Sprachen nutzen.

### <a name="can-i-access-some-ready-made-samples-for-getting-headstart"></a>Stehen vorkonfigurierte Beispiele für den schnellen Einstieg zur Verfügung?
Beispiele für die [.NET](documentdb-dotnet-samples.md)-, [Java](https://github.com/Azure/azure-documentdb-java)-, [Node.js](documentdb-nodejs-samples.md)- und [Python](documentdb-python-samples.md)-SDKs für die DocumentDB-API sind auf GitHub verfügbar.


### <a name="does-documentdb-api--database-support-schema-free-data"></a>Unterstützen DocumentDB-API-Datenbanken schemafreie Daten?
Ja, die DocumentDB-API ermöglicht Anwendungen das Speichern beliebiger JSON-Dokumente ohne Schemadefinition oder -hinweise. Die Daten stehen unmittelbar zur Abfrage über die Azure Cosmos DB-SQL-Abfrageschnittstelle zur Verfügung.   

### <a name="does-documentdb-api-support-acid-transactions"></a>Unterstützt die DocumentDB-API ACID-Transaktionen?
Ja, die DocumentDB-API unterstützt dokumentübergreifende Transaktionen, die in Form von gespeicherten JavaScript-Prozeduren und Triggern ausgedrückt werden. Die Transaktionen werden einer Partition in jeder Sammlung zugeordnet und mit ACID-Semantik nach dem Prinzip „alles oder nichts“ ausgeführt. Sie sind dabei von anderem gleichzeitig ausgeführtem Code und Benutzeranforderungen isoliert.  Falls bei der serverseitigen Ausführung des JavaScript-Anwendungscodes ein Ausnahmefehler auftritt, wird für die gesamte Transaktion ein Rollback durchgeführt. Weitere Informationen zu Transaktionen finden Sie unter [Datenbankprogramm-Transaktionen](documentdb-programming.md#database-program-transactions).

### <a name="what-is-a-collection"></a>Was ist eine Sammlung?
Eine Sammlung ist eine Gruppe von Dokumenten mit der zugehörigen JavaScript-Anwendungslogik. Eine Sammlung ist eine fakturierbare Entität, deren [Kosten](documentdb-performance-levels.md) vom Durchsatz und belegten Speicher bestimmt werden. Sammlungen können eine/n oder mehrere Partitionen oder Server umfassen und können skaliert werden, um praktisch unbegrenzte Mengen an Speicher oder Durchsatz zu verarbeiten.

Sammlungen stellen außerdem die Abrechnungseinheiten für Azure Cosmos DB dar. Die Kosten für jede Sammlung werden basierend auf dem bereitgestellten Durchsatz und dem verwendeten Speicherplatz pro Stunde berechnet. Weitere Informationen finden Sie unter [DocumentDB-API-Preise](https://azure.microsoft.com/pricing/details/documentdb/).  

### <a name="how-do-i-create-a-database"></a>Wie erstelle ich eine Datenbank?
Sie können Datenbanken – wie in [Erstellen einer Azure Cosmos DB-Sammlung und -Datenbank](documentdb-create-collection.md) beschrieben – im [Azure-Portal]() über eines der [Azure Cosmos DB-SDKs](documentdb-sdk-dotnet.md) oder die [REST-APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx) erstellen.  

### <a name="how-do-i-set-up-users-and-permissions"></a>Wie richte ich Benutzer und Berechtigungen ein?
Sie können Benutzer und Berechtigungen mit einem der [DocumentDB-API-SDKs](documentdb-sdk-dotnet.md) oder mithilfe der [REST-APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx) erstellen.   

### <a name="does-documentdb-api-support-sql"></a>Unterstützt die DocumentDB-API SQL?
Die SQL-Abfragesprache ist eine erweiterte Teilmenge der Abfragefunktionalität, die von SQL unterstützt wird. Die Azure Cosmos DB SQL-Abfragesprache bietet umfassende hierarchische und relationale Operatoren sowie die Erweiterbarkeit über JavaScript-basierte benutzerdefinierte Funktionen. Die JSON-Grammatik ermöglicht die Modellierung von JSON-Dokumenten als Struktur mit den Beschriftungen als Strukturknoten. Dies wird sowohl von den automatischen Indizierungstechniken als auch beim SQL-Abfragedialekt von Azure Cosmos DB verwendet.  Ausführliche Informationen zur Verwendung der SQL-Grammatik finden Sie im Artikel [Abfragen von DocumentDB][query].

### <a name="does-documentdb-api--support-sql-aggregation-functions"></a>Werden von der DocumentDB-API SQL-Aggregationsfunktionen unterstützt?
In der DocumentDB-API wird über die SQL-Grammatik Aggregation mit geringer Latenz und beliebiger Größe über die Aggregationsfunktionen `COUNT`, `MIN`, `MAX`, `AVG` und `SUM` unterstützt. Weitere Informationen finden Sie unter [Aggregationsfunktionen](documentdb-sql-query.md#Aggregates).

### <a name="how-does-documentdb-api--provide-concurrency"></a>Wie stellt die DocumentDB-API Parallelität zur Verfügung?
Die DocumentDB-API unterstützt die Steuerung für optimistische Parallelität (OCC) durch HTTP-Entitätstags bzw. ETags. Jede DocumentDB-API-Ressource verfügt über ein ETag. Das ETag wird auf dem Server festgelegt, immer wenn ein Dokument aktualisiert wird. Der ETag-Header und aktuelle Wert sind in allen Antwortnachrichten enthalten. ETags können mit dem „If-Match“-Header verwendet werden, um den Server entscheiden zu lassen, ob eine Ressource aktualisiert werden soll. Der „If-Match“-Wert ist der ETag-Wert, für den eine Überprüfung erfolgt. Wenn der ETag-Wert mit dem ETag-Wert des Servers übereinstimmt, wird die Ressource aktualisiert. Wenn das ETag nicht mehr aktuell ist, lehnt der Server den Vorgang mit dem Antwortcode „HTTP 412 – Vorbedingungsfehler“ ab. Der Client muss anschließend die Ressource erneut abrufen, um den aktuellen ETag-Wert für die Ressource zu erhalten. Darüber hinaus können ETags mit einem „If-None-Match“-Header verwendet werden, um festzustellen, ob ein erneutes Abrufen einer Ressource erforderlich ist.

Zum Nutzen der optimistischen Nebenläufigkeit in .NET verwenden Sie die [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) -Klasse. Ein Beispiel für .NET finden Sie unter [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) im „DocumentManagement“-Beispiel auf GitHub.

### <a name="how-do-i-perform-transactions-in-documentdb-api"></a>Wie führe ich Transaktionen in der DocumentDB-API durch?
Die DocumentDB-API unterstützt sprachintegrierte Transaktionen über gespeicherte JavaScript-Prozeduren und Trigger. Alle Datenbankvorgänge in Skripts werden per Momentaufnahmeisolation ausgeführt, für die die Sammlung als Bereich gilt, wenn es sich um eine Sammlung mit einer Partition handelt. Es kann sich auch um Dokumente innerhalb desselben Partitionsschlüsselwerts handeln, wenn die Sammlung partitioniert ist. Eine Momentaufnahme der Dokumentversionen (ETags) wird zu Beginn der Transaktion angefertigt und erst dann festgeschrieben, wenn das Skript erfolgreich ausgeführt wurde. Falls JavaScript einen Fehler ausgibt, wird für die Transaktion ein Rollback ausgeführt. Weitere Einzelheiten finden Sie unter [Serverseitige DocumentDB-API-Programmierung](documentdb-programming.md).

### <a name="how-can-i-bulk-insert-documents-into-documentdb-api"></a>Wie kann ich Masseneinfügung für Dokumente in DocumentDB-API ausführen?
Es gibt drei Möglichkeiten zur Masseneinfügung von Dokumenten in Azure Cosmos-DB:

* Das Datenmigrationstool, wie in [Importieren von Daten in DocumentDB-API](documentdb-import-data.md) beschrieben.
* Den Dokument-Explorer im Azure-Portal, beschrieben in [Massenhinzufügen von Dokumenten mit dem Dokument-Explorer](documentdb-view-json-document-explorer.md#bulk-add-documents).
* Gespeicherte Prozeduren, wie in [DocumentDB-API, serverseitige Programmierung](documentdb-programming.md) beschrieben.

### <a name="does-documentdb-api-support-resource-link-caching"></a>Unterstützt DocumentDB-API die Zwischenspeicherung von Ressourcenlinks?
Ja. Da es sich bei Cosmos DB um einen RESTful-Dienst handelt, sind Ressourcenlinks unveränderbar und können zwischengespeichert werden. DocumentDB-API-Clients können einen „If-None-Match“-Header für Lesevorgänge für jede Ressource wie Dokumente oder Sammlungen festlegen und ihre lokalen Kopien nur dann aktualisieren, wenn sich die Serverversion geändert hat.

### <a name="is-a-local-instance-of-documentdb-api-available"></a>Ist eine lokale Instanz von DocumentDB-API verfügbar?
Ja. Der [Azure Cosmos DB-Emulator](documentdb-nosql-local-emulator.md) stellt eine High-Fidelity-Emulation des DocumentDB-API-Diensts bereit. Er unterstützt die gleichen Funktionen wie Azure Cosmos DB, z.B. Erstellen und Abfragen von JSON-Dokumenten, Bereitstellen und Skalieren von Sammlungen und Ausführen von gespeicherten Prozeduren und Triggern. Sie können Anwendungen mit dem Azure Cosmos DB-Emulator entwickeln und testen und diese in Azure auf globaler Ebene bereitstellen, indem Sie nur eine einzige Konfigurationsänderung am Verbindungsendpunkt für Azure Cosmos DB vornehmen.

## <a name="database-questions-about-developing-against-api-for-mongodb"></a>Datenbankfragen zum Entwickeln mit API für MongoDB
### <a name="what-is-azure-cosmos-dbs-api-for-mongodb"></a>Was ist die Azure Cosmos-DB-API für MongoDB?
Die API für MongoDB von Microsoft Azure Cosmos DB ist eine Kompatibilitätsebene, mit der Anwendungen auf einfache und transparente Weise über vorhandene, von der Community unterstützte Apache MongoDB-APIs und -Treiber mit dem nativen Azure Cosmos DB-Datenbankmodul kommunizieren können. Entwickler können jetzt vorhandene MongoDB-Toolketten und -Fähigkeiten nutzen, um Anwendungen zu erstellen, die Azure Cosmos DB nutzen und dabei von den einzigartigen Funktionen von Azure Cosmos DB profitieren. Diese umfassen die automatische Indizierung, Sicherungswartung, finanziell abgesicherte Vereinbarungen zum Servicelevel usw.

### <a name="how-to-do-i-connect-to-my-api-for-mongodb-database"></a>Wie stelle ich eine Verbindung mit meiner API für die MongoDB-Datenbank her?
Die schnellste Möglichkeit zum Herstellen einer Verbindung mit der API für MongoDB von Azure Cosmos DB besteht darin, dass Sie zum [Azure-Portal](https://portal.azure.com) wechseln. Navigieren Sie zu Ihrem Konto. Klicken Sie im *linken Navigationsbereich* des Kontos auf *Schnellstart*. Der *Schnellstart* ist die beste Möglichkeit, um Codeausschnitte abzurufen, um eine Verbindung mit Ihrer Datenbank herzustellen. 

Azure Cosmos DB erzwingt strenge Sicherheitsanforderungen und -standards. Azure Cosmos DB-Konten erfordern eine Authentifizierung und eine sichere Kommunikation über *SSL*. Verwenden Sie daher unbedingt TLSv1.2.

Weitere Informationen finden Sie unter [Verbinden einer MongoDB-App mit einem DocumentDB-Konto mithilfe einer MongoDB-Verbindungszeichenfolge](documentdb-connect-mongodb-account.md).

### <a name="are-there-additional-error-codes-for-an-api-for-mongodb-database"></a>Gibt es weitere Fehlercodes für eine API für die MongoDB-Datenbank?
API für MongoDB verfügt zusätzlich zu den allgemeinen MongoDB-Fehlercodes über eigene, spezifische Fehlercodes.


| Error               | Code  | Beschreibung  | Lösung  |
|---------------------|-------|--------------|-----------|
| TooManyRequests     | 16500 | Die Gesamtanzahl der verbrauchten Anforderungseinheiten hat die bereitgestellte Anforderungseinheitenrate für die Sammlung überschritten und wurde gedrosselt. | Skalieren Sie ggf. im Azure-Portal den Durchsatz der Sammlung, oder versuchen Sie es noch mal. |
| ExceededMemoryLimit | 16501 | Der Vorgang ist ein mehrinstanzenfähiger Dienst und hat die Speicherzuweisung des Clients überschritten. | Verringern Sie den Umfang des Vorgangs mithilfe restriktiverer Abfragekriterien, oder wenden Sie sich im [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) an den Support. <br><br>*Ex:  &nbsp;&nbsp;&nbsp;&nbsp;db.getCollection('users').aggregate([<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{$match: {name: "Andy"}}, <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{$sort: {age: -1}}<br>&nbsp;&nbsp;&nbsp;&nbsp;])*) |

## <a name="database-questions-about-developing-with-azure-cosmos-db-table-api-preview"></a>Datenbankfragen zum Entwickeln mit Azure Cosmos DB: Table-API (Vorschauversion)

### <a name="terms"></a>Begriffe 
Die Azure Cosmos DB: Table-API (Vorschauversion) bezeichnet ein Premium-Angebot von Azure Cosmos DB für die anlässlich der Build 2017 vorgestellte Tabellenunterstützung. 

Bei dem Standard-Table-SDK handelt es sich um das bereits vorhandene Azure Storage Table-SDK. 

### <a name="how-can-i-use-the-new-table-api-preview-offering"></a>Wie kann ich das neue Angebot für Table-API (Vorschauversion) nutzen? 
Die Microsoft Table-API steht im [Azure-Portal][azure-portal] zur Verfügung.  Zuerst müssen Sie sich für ein Azure-Abonnement anmelden.  Nachdem Sie sich für ein Microsoft Azure-Abonnement registriert haben, können Sie Ihrem Azure-Abonnement einen Table-API-Container hinzufügen.  
Innerhalb des Vorschauzeitraums stehen die [SDKs](../cosmos-db/table-sdk-dotnet.md) für .NET zur Verfügung, für den Einstieg können den Schnellstartleitfaden für [Table-API](../cosmos-db/create-table-dotnet.md) durcharbeiten.

### <a name="do-i-need-a-new-sdk-to-use-the-table-api-preview"></a>Benötige ich ein neues SDK, um die Table-API (Vorschauversion) zu verwenden? 
Ja. Ein neues SDK steht in Form eines NuGet-Pakets hier zur Verfügung: [Table-API](../cosmos-db/table-sdk-dotnet.md). Es trägt die Bezeichnung SDK für Windows Azure Storage 8.1.2 mit Premium Table-API (Vorschauversion).  

### <a name="how-do-i-provide-the-feedback-about-the-sdk-bugs"></a>Wie kann ich Feedback zum SDK und Bugs geben?
Geben Sie Ihr Feedback mithilfe einer der folgenden Methoden:

* [Auf UserVoice](https://feedback.azure.com/forums/263030-documentdb)
* [MSDN-Forum](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=AzureDocumentDB)
* [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-documentdb)

### <a name="what-is-the-connection-string-that-i-need-to-use-to-connect-to-the-table-api-preview"></a>Welche Verbindungszeichenfolge muss ich für das Herstellen von Verbindungen mit der Table-API (Vorschauversion) verwenden?
Die Verbindungszeichenfolge lautet `DefaultEndpointsProtocol=https;AccountName=<AccountNamefromCosmos DB;AccountKey=<FromKeysPaneofCosmosDB>;TableEndpoint=https://<AccountNameFromDocumentDB>.documents.azure.com`. Sie können die Verbindungszeichenfolge auf der Seite „Schlüssel“ im Azure-Portal abrufen. 

### <a name="how-do-i-override-the-config-settings-for-the-request-options-in-the-new-table-apipreview"></a>Wie setze ich die Konfigurationseinstellungen für die Anforderungsoptionen in der neuen Table-API (Vorschauversion) außer Kraft?
Diese Einstellungen sind in den [Azure Cosmos DB-Funktionen](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities) dokumentiert. Sie können die Einstellungen ändern, indem Sie sie im Abschnitt „appsettings“ in der Clientanwendung zu „app.config“ hinzufügen.
<appSettings>
    <add key="TableConsistencyLevel" value="Eventual|Strong|Session|BoundedStaleness|ConsistentPrefix"/>
    <add key="TableThroughput" value="<PositiveIntegerValue"/>
    <add key="TableIndexingPolicy" value="<jsonindexdefn>"/>
    <add key="TableUseGatewayMode" value="True|False"/>
    <add key="TablePreferredLocations" value="Location1|Location2|Location3|Location4>"/>....
</appSettings>


### <a name="is-there-any-change-to-existing-customers-that-use-the-existing-standard-table-sdk"></a>Gibt es Änderungen für Bestandskunden, die das vorhandene Tabellen-Standard-SDK verwenden?
Keine Für Bestands- oder Neukunden, die das vorhandene Tabellen-Standard-SDK verwenden, ergeben sich keine Änderungen. 

### <a name="how-do-i-view-table-data-that-is-stored-in-azure-cosmos-db-for-use-with-the-table-api-review"></a>Wie zeige ich Tabellendaten, die in Azure Cosmos DB gespeichert sind, für die Verwendung mit der Table-API (Vorschauversion) an? 
Sie können das Azure-Portal verwenden, um die Daten zu durchsuchen. Sie können ebenfalls den Code der Table-API (Vorschauversion) oder die unten genannten Tools verwenden. 

### <a name="which-tools-will-work-with-table-api-preview"></a>Welche Tools funktionieren in Verbindung mit der Table-API (Vorschauversion)? 
Ältere Version von Azure-Explorer (0.8.9).

Tools mit der Flexibilität, eine Verbindungszeichenfolge in dem früher angegebenen Format zu akzeptieren, können die neue Table-API (Vorschauversion) unterstützen. Eine Liste mit Tabellentools steht auf der Seite [Azure Storage-Clienttools](../storage/storage-explorers.md) zur Verfügung. 

### <a name="does-powershellcli-work-with-the-new-table-api-preview-"></a>Funktioniert PowerShell/CLI mit der neuen Table-API (Vorschauversion) ?
Wir planen Unterstützung für PowerShell/CLI für die Table-API (Vorschauversion). 

### <a name="is-the-concurrency-on-operations-controlled"></a>Ist die Parallelität im Betrieb gesteuert?
Ja, optimistische Parallelität wird mithilfe des ETag-Mechanismus bereitgestellt, wie in der Standard-Tabellen-API zu erwarten. 

### <a name="is-odata-query-model-supported-for-entities"></a>Wird das OData-Abfragemodell für Entitäten unterstützt? 
Ja, die Table-API (Vorschauversion) unterstützt OData-Abfragen und Linq-Abfragen. 

### <a name="can-i-connect-to-standard-azure-table-and-the-new-premium-table-api-preview-at-side-by-side-in-same-application-"></a>Können Verbindungen mit der Azure-Standardtabelle und der neuen Premium-Table-API (Vorschauversion) parallel in der gleichen Anwendung hergestellt werden? 
Ja, dies kann durch Erstellen von zwei verschiedenen Instanzen von CloudTableClient erreicht werden, die jeweils mithilfe der Verbindungszeichenfolge auf ihre eigenen URIs verweisen.

### <a name="how-do-i-migrate-existing-table-storage-application-to-this-new-offering"></a>Wie erfolgt die Migration von vorhandenen Table Storage-Anwendungen zu diesem neuen Angebot?
Wenden Sie sich an askdocdb@microsoft.com, wenn Sie das neue Table-API-Angebot für Ihre vorhandenen Table Storage-Daten nutzen möchten. 

### <a name="what-is-the-roadmap-for-this-service-when-will-other-functionality-of-standard-table-api-be-offered"></a>Wie sieht die Roadmap für diesen Dienst aus, wann werden weitere Funktionen der Standard-Tabellen-API verfügbar sein?
Wir planen Unterstützung für SAS-Token, ServiceContext, Statistik, Verschlüsselung, Analyse und weitere Funktionen hin auf dem Weg zur allgemeinen Verfügbarkeit.  Bitte geben Sie Ihr Feedback auf [Uservoice](https://feedback.azure.com/forums/263030-documentdb). 

### <a name="how-is-expansion-of-the-storage-size-done-for-this-service-say-i-start-with-n-gbs-of-data-and-my-data-will-grow-to-1-tb-overtime"></a>Wie erfolgt die Erweiterung der Speichergröße für diesen Dienst – angenommen, ich fange mit N GB Daten an, und meine Daten wachsen im Lauf der Zeit auf 1 TB an?  
Cosmos DB wurde dafür ausgelegt, mithilfe von horizontaler Skalierung unbeschränkten Speicherplatz zu bieten. Unser Dienst überwacht Ihren Speicher und vergrößert ihn effektiv. 

### <a name="how-do-i-monitor-the-table-api-preview-offering"></a>Wie lässt sich die Table-API (Vorschauversion) überwachen?
Sie können den Metrikbereich der Table-API (Vorschauversion) verwenden, um Anforderungen und Speicherbelegung zu überwachen. 

### <a name="how-do-i-calculate-throughput-i-require"></a>Wie kann der erforderliche Durchsatz berechnet werden?
Sie können den Kapazitätskalkulator verwenden, um den für die Vorgänge erforderlichen TableThroughput zu berechnen, wie hier dokumentiert: [Schätzen von Anforderungseinheiten und Datenspeicher](https://www.documentdb.com/capacityplanner). Im allgemeinen können Sie Ihre Entität als JSON darstellen und die Zahlen für Ihre Operationen eingeben. 

### <a name="can-i-use-the-new-table-api-preview-sdk-locally-with-the-emulator"></a>Lässt sich das SDK der neuen Table-API (Vorschauversion) lokal mit dem Emulator verwenden?
Ja, Sie können die Table-API (Vorschauversion) mit dem lokalen Emulator verwenden, wenn Sie dazu das neue SDK verwenden. Laden Sie den neuen Emulator [hier](documentdb-nosql-local-emulator.md) herunter. Der Wert von StorageConnectionString in „app.config“ muss „DefaultEndpointsProtocol=https;AccountName=localhost;AccountKey=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==;TableEndpoint=https://localhost:8081“ lauten. 

### <a name="can-my-existing-application-work-with-the-table-api-preview"></a>Kann meine vorhandene Anwendung mit der Table-API (Vorschauversion) verwendet werden? 
Der Oberflächenbereich der neuen Table-API (Vorschauversion) ist über die Erstellen-, Löschen-, Aktualisieren- und Abfragevorgänge mit dem vorhandenen Azure Standard-Tabellen-SDK kompatibel. Sie müssen sicherstellen, dass Sie über den Zeilenschlüssel verfügen, da die Table-API (Vorschauversion) sowohl den Partitionsschlüssel als auch den Zeilenschlüssel erfordert. Wir planen darüber hinaus, auf dem Weg zur allgemeinen Verfügbarkeit dieses Dienstangebots Unterstützung für weitere SDK hinzuzufügen.

### <a name="do-i-need-to-migrate-existing-azure-table-based-application-to-new-sdk-if-i-do-not-want-to-use-table-api-preview-features"></a>Müssen vorhandene, auf Azure-Tabellen basierende Anwendungen zum neuen SDK migriert werden, wenn ich die Funktionen von Table-API (Vorschauversion) nicht verwenden möchte?
Nein, Bestandskunden können den vorhandenen Standardtabellenbestand ohne irgendwelche Unterbrechungen erstellen und verwenden. Wenn Sie die neue Table-API (Vorschauversion) nicht nutzen, können Sie jedoch nicht von der automatischen Indizierung, der zusätzlichen Konsistenzoption oder der globalen Verteilung profitieren. 

### <a name="how-do-i-add-replication-for-the-data-in-premium-table-api-preview-across-multiple-regions-of-azure"></a>Wie füge ich in der Premium-Table-API (Vorschauversion) Replikation für die Daten über mehrere Regionen von Azure hinzu?
Sie können die [globalen Replikationseinstellungen](documentdb-portal-global-replication.md) im Cosmos DB-Portal verwenden, um Regionen hinzuzufügen, die sich für Ihre Anwendung eignen. Beim Entwickeln einer global verteilten Anwendung sollten Sie Ihre Anwendung darüber hinaus mit einem auf die lokale Region festgelegten Wert von PreferredLocation hinzufügen, um niedrige Leselatenz bereitzustellen. 

### <a name="how-do-i-change-the-primary-write-region-for-the-account-in--premium-table-apipreview"></a>Wie lässt sich die primäre Schreibregion für das Konto in der Premium-Table-API (Vorschauversion) ändern?
Sie können den Portalbereich für globale Replikation in Cosmos DB verwenden, um eine Region hinzuzufügen, und dann ein Failover auf die benötigte Region ausführen. Anweisungen finden Sie unter [Entwickeln für Azure Cosmos DB-Konten für mehrere Regionen](documentdb-regional-failovers.md). 

### <a name="how-do-i-configure-my-preferred-read-regions-for-low-latency-when-i-distribute-my-data"></a>Wie lassen sich beim Verteilen meiner Daten meine bevorzugten Leseregionen konfigurieren, um niedrige Latenz zu erreichen? 
Sie müssen den PreferredLocation-Schlüssel in der app.config-Datei nutzen, um Sie beim Einrichten des Lesens vom lokalen Speicherort zu unterstützen. Für vorhandene Anwendungen löst die Table-API (Vorschauversion) einen Fehler aus, wenn LocationMode festgelegt ist, daher entfernen Sie den entsprechenden Code, da die Premium-Table-API (Vorschauversion) diese Informationen aus der app.config-Datei liest.  Diese Einstellungen sind in den [Azure Cosmos DB-Funktionen](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities) dokumentiert.


### <a name="how-do-i-think-of-consistency-in-table-api-preview"></a>Wie stelle ich Konsistenz in Table-API (Vorschauversion) sicher? 
Cosmos DB bietet einen wohlüberlegten Kompromiss zwischen Konsistenz, Verfügbarkeit und Latenz. Beim Stand dieses Releases stehen 5 Konsistenzebenen zur Wahl. Diese Konsistenzebenen stehen den Entwicklern in Table-API (Vorschauversion) zur Verfügung. Dadurch können Entwickler das optimale Konsistenzmodell auf Tabellenebene auswählen und darüber hinaus beim Abfragen der Daten individuelle Anforderungen berücksichtigen.  Beim Herstellen einer Clientverbindung kann der Client eine Konsistenzebene angeben; diese kann mithilfe der app.config-Einstellung für den Wert des TableConsistencyLevel-Schlüssels geändert werden. Table-API (Vorschauversion) bietet Lesezugriff mit niedriger Latenz mit „Eigene Schreibvorgänge lesen“ mit Sitzungskonsistenz als Standardeinstellung. Weitere Informationen finden Sie unter [Konsistenzebenen](documentdb-consistency-levels.md). Standardmäßig bietet die Standardtabelle heute starke Konsistenz innerhalb einer Region und letztliche Konsistenz an sekundären Speicherorten.  

### <a name="does-this-mean-compared-to-eventual--strong-consistency-that-is-possible-with-standard-table---we-now-have-more-choices"></a>Bedeutet dies, dass gemessen an letztlicher und starker Konsistenz, die mit der Standardtabelle möglich sind, jetzt mehr Optionen zur Wahl stehen?
Ja, diese Optionen sind im Artikel [Konsistenzebenen](documentdb-consistency-levels.md) dokumentiert, um Entwickler bei der Nutzung der verteilten Natur von Cosmos DB zu unterstützen. Da auch für die Konsistenz eine Garantie gegeben wird, können Sie diese bereits heute mit Zuversicht nutzen. Diese Einstellungen sind in den [Azure Cosmos DB-Funktionen](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities) dokumentiert.

### <a name="when-global-distribution-is-enabled--how-long-does-it-take-to-replicate-the-data"></a>Wenn globale Verteilung aktiviert ist, wie viel Zeit wird für die Replikation der Daten benötigt?
Wir committen die Daten dauerhaft in der lokalen Region und übertragen sie sofort innerhalb von Millisekunden per Push in andere Regionen, und diese Replikation hängt nur von der RTT des Rechenzentrums ab. Informieren Sie sich über die Funktionen zur globalen Verteilung von Cosmos DB in [Azure Cosmos DB: Ein global verteilter Datenbankdienst auf Azure](documentdb-distribute-data-globally.md).

### <a name="can-the-read-request-consistency-be-changed"></a>Kann die Konsistenz für Leseanforderungen geändert werden?
Cosmos DB erlaubt das Festlegen der Konsistenz auf Containerebene, die eine Tabelle darstellt. Mit dem SDK können Sie darüber hinaus die Ebene ändern, indem Sie den Wert für den Schlüssel TableConsistencyLevel in der app.config-Datei angeben. Dies sind die möglichen Werte: Strong|Bounded Staleness|Session|ConsistentPrefix|Eventual. Dies ist im Artikel [Konsistenzebenen](documentdb-consistency-levels.md) dokumentiert. Hier ist zu beachten, dass die angeforderte Konsistenz nicht höher sein kann als die Einstellung für die Tabelle. Wenn Sie beispielsweise die Konsistenz für die Tabelle auf letztlich (eventual) festgelegt haben, und die Konsistenzebene in der Anforderung auf stark festlegen, funktioniert das nicht. 

### <a name="how-does-premium-table-api-preview-account-take-care-of-failover-in-case-a-region-goes-down"></a>Wie verarbeitet die Premium-Table-API (Vorschauversion) ein Failover, falls eine Region ausfällt? 
Die Premium-Table-API (Vorschauversion) nutzt die global verteilte Plattform von Cosmos DB. Um sicherzustellen, dass Ihre Anwendung Ausfallzeiten des Rechenzentrums tolerieren kann, müssen Sie für das Konto mindestens eine weitere Region im Cosmos DB-Portal aktivieren: [Entwickeln für Azure Cosmos DB-Konten für mehrere Regionen](documentdb-regional-failovers.md). Sie können die Priorität der Region mithilfe des Portals festlegen: [Entwickeln für Azure Cosmos DB-Konten für mehrere Regionen](documentdb-regional-failovers.md). Sie können für das Konto beliebig viele Regionen hinzufügen, und steuern, wohin das Failover erfolgt, indem Sie die Priorität festlegen. Es versteht sich von selbst, dass Sie dort jeweils auch eine Anwendung bereitstellen müssen, um die Datenbank zu nutzen. Auf diese Weise treten für Ihre Kunden keine Ausfallzeiten auf. Das Client-SDK verfügt über Auto-Homing: Es kann die ausgefallene Region erkennen und automatisch das Failover auf die neue Region ausführen.

### <a name="is-premium-table-api-preview-enabled-for-backups"></a>Ist Premium-Table-API (Vorschauversion) für Sicherungen aktiviert?
Ja, Premium-Table-API (Vorschauversion) nutzt die Plattform von Cosmos DB für Sicherungen. Sicherungen werden automatisch erstellt. Cosmos DB-Sicherung ist hier dokumentiert: [Onlinesicherung und -wiederherstellung mit Azure Cosmos DB](C:\Users\govindk\azure-docs-pr\articles\documentdb\documentdb-online-backup-and-restore.md)

 
### <a name="does-the-table-api-preview-index-all-attributes-of-entities-by-default"></a>Führt die Table-API (Vorschauversion) die Indizierung aller Attribute von Entitäten standardmäßig aus?
Ja, standardmäßig werden alle Attribute einer Entität indiziert. Die Indizierungsdetails sind im Artikel [Azure Cosmos DB: Indizierungsrichtlinien](documentdb-indexing-policies.md) dokumentiert. 

### <a name="does-this-mean-i-do-not-have-to-create-different-indexes-to-satisfy-the-queries"></a>Bedeutet dies, dass ich keine weiteren Indizes erstellen muss, um Abfragen zu bedienen? 
Ja, Cosmos DB bietet die automatische Indizierung aller Attribute ganz ohne Schemadefinition. Dies ermöglicht Entwicklern, sich auf die Anwendung anstelle der Indexerstellung und -verwaltung zu konzentrieren. Die Indizierungsdetails sind im Artikel [Azure Cosmos DB: Indizierungsrichtlinien](documentdb-indexing-policies.md) dokumentiert.

### <a name="can-the-indexing-policy-be-changed"></a>Kann die Indizierungsrichtlinie geändert werden?
Ja, Sie können den Index ändern, indem Sie die Indexdefinition bereitstellen. Die Bedeutung dieser Einstellungen ist in dem Artikel [Azure Cosmos DB-Funktionen](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities) dokumentiert. Sie müssen diese Einstellungen ordnungsgemäß codieren und escapen.  

Im JSON-Zeichenfolgenformat in der app.config-Datei.
{ "indexingMode": "consistent", "automatic": true, "includedPaths": [ { "path": "/somepath", "indexes": [ { "kind": "Range", "dataType": "Number", "precision": -1 }, { "kind": "Range", "dataType": "String", "precision": -1 } ] } ], "excludedPaths": [ { "path": "/anotherpath" } ] }

### <a name="cosmos-db-as-a-platform-seems-to-have-lot-of-capabilities-like-sorting-aggregates-hierarchy-and-other-functionality---is-this-planned-to-be-added-to-the-table-api-"></a>Cosmos DB als Plattform scheint eine Menge Funktionen wie Sortierung, Aggregierung, Hierarchie und weitere Funktionen zu bieten; ist eine Einführung in der Tabellen-API geplant? 
In der Vorschau unterstützt Azure Cosmos DB die gleiche Abfragefunktionalität wie Azure Table Storage für die Tabellen-API. Azure Cosmos DB unterstützt auch Sortieren, Aggregieren, räumliche Abfrage, Hierarchie und eine Vielzahl von integrierten Funktionen. Die zusätzliche Funktionalität wird in einem zukünftigen Dienstupdate in der Tabellen-API bereitgestellt. Unter [SQL-Abfrage und SQL-Syntax in DocumentDB](../documentdb/documentdb-sql-query.md) finden Sie eine Übersicht dieser Funktionen.
 
### <a name="when-should-i-change-tablethroughput-for-the-table-api-preview"></a>Wann sollte TableThroughput für die Table-API (Vorschauversion) geändert werden?
Sie sollten TableThroughput ändern, wenn Sie das ETL der Daten durchführen oder eine große Menge Daten in kurzer Zeit hochladen möchten. 

ODER

Sie benötigen mehr Durchsatz vom Container im Back-End, da Sie in der Metrik sehen, dass der abgerufene Durchsatz höher als der bereitgestellte Durchsatz ist und Drosselung eintritt. Dies ist im Artikel [Festlegen des Durchsatzes](documentdb-set-throughput.md) dokumentiert.

### <a name="can-i-scale-up-or-down-the-throughput-of-my-table-api-preview-table"></a>Lässt sich der Durchsatz meiner Table-API (Vorschauversion)-Tabelle zentral hoch- oder herunterskalieren? 
Ja, das können Sie im Skalierungsbereich im Cosmos DB-Portal vornehmen. Dies ist im Thema [Festlegen des Durchsatzes](documentdb-set-throughput.md) dokumentiert.

### <a name="can-premium-table-api-preview-leverage-ru-per-minute-offering"></a>Kann die Premium-Table-API (Vorschauversion) Angebote für Anforderungseinheiten pro Minute nutzen? 
Ja, Premium Table (Vorschauversion) nutzt Funktionen von Cosmos DB, um SLAs für Leistung, Latenz, Verfügbarkeit und Konsistenz bereitzustellen. Dadurch wird sichergestellt, dass das Angebot für Anforderungseinheiten pro Minute genutzt werden kann, wie in [Anforderungseinheiten](documentdb-request-units.md) dokumentiert. Diese Funktion ermöglicht es Kunden, die Bereitstellung nicht an der Spitzenlast zu orientieren und die Spitzen im Workload zu glätten.

### <a name="is-there-a-default-tablethroughput-which-is-set-for-newly-provisioned-tables"></a>Gibt es einen Standardwert für TableThroughput, der für neu bereitgestellte Tabellen festgelegt wird?
Ja, wenn Sie TableThroughput nicht mithilfe von „app.config“ außer Kraft setzen und keinen fertig konfigurierten Container in Cosmos DB verwenden, erstellt der Dienst eine Tabelle mit einem Durchsatz von 400.
 
### <a name="is-there-any-change-of-pricing-for-existing-customers-of-standard-table-api"></a>Gibt es Preisänderungen für Bestandskunden der Standard-Tabellen-API?
Keine Es gibt keine Preisänderung für bestehende Kunden der Standard-Tabellen-API. 

### <a name="how-is-the-price-calculated-for-the-table-apipreview"></a>Wie wird der Preis für die Table-API (Vorschauversion) berechnet? 
Das hängt von dem zugewiesenen TableThroughput ab. 

### <a name="how-do-i-handle-any-throttling-on-the-tables-in-table-api-preview-offering"></a>Wie lässt sich mit Drosselung in den Tabellen im Table-API (Vorschauversion)-Angebot umgehen? 
Wenn die Anforderungsrate die Kapazität des bereitgestellten Durchsatzes für den zugrundeliegenden Container überschreitet, empfangen Sie einen Fehler, und das SDK wiederholt den Aufruf mithilfe der Wiederholungsrichtlinie.

### <a name="why-do-i-need-to-choose-a-throughput-apart-from-partitionkey-and-rowkey-to-leverage-premium-table-preview-offering-of-cosmos-db"></a>Warum muss ich über PartitionKey und RowKey hinaus einen Durchsatz festlegen, um das Premium-Table (Vorschauversion)-Angebot von Cosmos DB zu nutzen?
Cosmos DB legt einen Standarddurchsatz für Ihren Container fest, wenn Sie keinen in „app.config“ angeben. 

Cosmos DB bietet Garantien für Leistung und Latenz mit Obergrenzen im Betrieb. Dies ist möglich, wenn das Modul Governance über die Operationen des Mandanten ausüben kann. Durch das Festlegen von TableThroughput stellen Sie im Grunde sicher, dass Sie den garantierten Durchsatz und die garantierte Latenz erhalten, da die Plattform nun diese Kapazität reserviert und den erfolgreichen Betrieb sicherstellt.  
Die Angabe des Durchsatzes ermöglicht Ihnen darüber hinaus die elastische Veränderung, um der Saisonalität Ihrer Anwendung Rechnung zu tragen, dabei die Anforderungen an den Durchsatz zu erfüllen und Kosten zu sparen.

### <a name="azure-storage-sdk-has-been-very-cheap-for-me-as-i-only-pay-to-store-the-data-and-i-rarely-query-cosmos-dbs-new-offering-seems-to-be-charging-me-even-though-i-have-not-performed-single-transaction-or-stored-anything-whats-going-on-here"></a>Azure Storage SDK war für mich sehr preiswert, da ich nur für die Speicherung der Daten bezahle und kaum abfrage. Das neue Angebot von Cosmos DB scheint mich zu belasten, obwohl ich keine einzige Transaktion ausgeführt und nichts gespeichert habe. Was geht da vor?

Cosmos DB wurde als global verteiltes, SLA-basiertes System mit Garantien für Verfügbarkeit, Latenz und Durchsatz entwickelt. Wenn Sie Durchsatz in Cosmos DB reservieren, wird dieser garantiert, anders als bei anderen Systemen. Cosmos DB stellt darüber hinaus weitere Funktionen zur Verfügung, nach denen Kunden schon lange gefragt haben: sekundäre Indizes, globale Verteilung usw. Während des Vorschauzeitraums bieten wir das Durchsatzmodell an, und auf lange Sicht möchten wir ein speicheroptimiertes Modell anbieten, um den Bedürfnissen unsere Kunden gerecht zu werden.  

### <a name="i-never-get-quota-full-indicating-a-partition-is-full-when-i-keep-ingesting-data-into-azure-table-storage-with-the-table-apipreview--i-can-get-this-error-is-this-offering-limiting-me-and-forcing-me-to-change-my-present-application"></a>Ich erhalte nie eine Meldung „Kontingent erschöpft“ (die darauf hinweist, dass eine Partition voll ist), wenn ich Daten in den Azure Table Storage lade. Bei der Table-API (Vorschauversion) erhalte ich diesen Fehler. Schränkt mich dieses Angebot ein, und zwingt es mich, meine derzeitige Anwendung zu wechseln?

Cosmos DB ist ein SLA-basiertes System, das unbeschränkte Skalierung mit Garantien für Latenz, Durchsatz, Verfügbarkeit und Konsistenz bietet. Um sicherzustellen, dass Sie garantierte Premium-Leistung erhalten, müssen Sie sicherstellen, dass das Datenvolumen und der Index verwaltbar und skalierbar sind. Der 10 GB-Grenzwert für die Anzahl der Entitäten/Elemente pro Partitionsschlüssel dient dazu, sicherzustellen, dass wir eine hervorragende Sperr-/Abfrageleistung bieten können. Um sicherzustellen, dass Ihre Anwendung gut skaliert, fordern wir Sie auch bei Azure Storage auf, keine Hot Partition zu erstellen, indem Sie alle Informationen in einer Partition speichern und abfragen.  

### <a name="so-partitionkey-and-rowkey-are-still-required-with-the-new-table-api-preview"></a>Also sind PartitionKey und RowKey auch bei der neuen Table-API (Vorschauversion) erforderlich? 
Ja. Da der Oberflächenbereich von Table-API (Vorschauversion) dem des Azure Table Storage-SDKs ähnlich ist, stellt der Partitionsschlüssel ein hervorragendes Verfahren zum Verteilen der Daten dar. Der Zeilenschlüssel ist innerhalb jeder Partition eindeutig. Ja, der Zeilenschlüssel muss vorhanden sein und darf nicht NULL sein, wie im Standard-SDK. Zum Zeitpunkt dieser Buildvorschauversion beträgt die Länge von RowKey 255 Byte und die von PartitionKey 100 Byte (die aber bald auf 1 KB heraufgesetzt wird). 

### <a name="what-will-be-the-error-messages-of-table-api-preview-"></a>Welche Fehlermeldungen wird es in der Table-API (Vorschauversion) geben?
Da diese Vorschauversion mit der Standardtabelle kompatibel ist, wird die Mehrheit der Fehler auf Fehler aus der Standardtabelle abgebildet. 

### <a name="why-do-i-get-throttled-when-i-try-to-create-lot-of-tables-one-after-another--in-tables-api-preview"></a>Warum tritt Drosselung ein, wenn ich versuche, eine große Menge Tabellen hintereinander in Tables-API (Vorschauversion) zu erstellen?
Cosmos DB ist ein SLA-basiertes System, das Garantien für Latenz, Durchsatz, Verfügbarkeit und Konsistenz bietet. Da es sich um ein bereitgestelltes System handelt, reserviert es Ressourcen, um diese Anforderungen zu garantieren. Eine hohe Erstellungsrate von Tabellen in rascher Folge wird erkannt und gedrosselt. Wir raten Ihnen, sich die Erstellungsrate von Tabellen anzusehen und sie auf weniger als 5 pro Minute herabzusetzen. Sie sollten auch bedenken, dass Table-API (Vorschauversion) ein bereitgestelltes System darstellt – d.h., es entstehen Ihnen Kosten, sobald Sie es bereitstellen.  

# <a name="graph-apipreview"></a>Graph-API (Vorschauversion)
## <a name="database-questions-about-developing-against-graph-apipreview"></a>Datenbankfragen zum Entwickeln für die Graph-API (Vorschauversion)
### <a name="how-can-i-leverage-graph-api-preview-with-cosmos-db"></a>Wie kann ich die Graph-API (Vorschauversion) mit Cosmos DB nutzen?
Die Funktionalität von Graph-API (Vorschauversion) kann von einer Erweiterungsbibliothek aus genutzt werden. Sie heißt Microsoft Azure Graphs und ist in NuGet verfügbar. 

### <a name="it-looks-like-you-support-gremlin-as-traversal-language-do-you-plan-to-add-some-more-forms-of-query-"></a>Anscheinend unterstützen Sie Gremlin als Traversalsprache – bestehen Pläne für weitere Abfrageformen?
Ja, zukünftig möchten wir weitere Abfragemechanismen hinzufügen. 

### <a name="how-can-i-use-the-new-graph-apipreview-offering"></a>Wie kann ich das neue Graph-API (Vorschauversion)-Angebot nutzen? 
Arbeiten Sie für den Einstieg den [Graph-API](../cosmos-db/create-graph-dotnet.md)-Schnellstart-Leitfaden durch.




[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md

