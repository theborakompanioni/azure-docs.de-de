---
title: Azure Cosmos DB-Skalierungs- und -Leistungstests | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Skalierungs- und Leistungstests mit Azure Cosmos DB durchführen."
keywords: Leistungstests
services: cosmos-db
author: arramac
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: f4c96ebd-f53c-427d-a500-3f28fe7b11d0
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: a643f139be40b9b11f865d528622bafbe7dec939
ms.openlocfilehash: f85be2c605ec5dadd9296486d3a1357c39e40b64
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---
# <a name="performance-and-scale-testing-with-azure-cosmos-db"></a>Leistungs- und Skalierungstests mit Azure Cosmos DB
Leistungs- und Skalierungstests sind ein wichtiger Schritt bei der Entwicklung von Anwendungen. Die Datenbankebene hat bei vielen Anwendungen einen erheblichen Einfluss auf die allgemeine Leistung und Skalierbarkeit und ist daher eine wichtige Komponente von Leistungstests. [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) wurde speziell für die flexible Skalierung und vorhersagbare Leistung entwickelt und eignet sich deshalb hervorragend für Anwendungen, die eine leistungsstarke Datenbankebene benötigen. 

Dieser Artikel ist eine Referenz für Entwickler, die Leistungstestsammlungen für ihre Cosmos DB-Workloads implementieren oder Cosmos DB für leistungsstarke Anwendungsszenarien auswerten. Er konzentriert sich hauptsächlich auf isolierte Leistungstests der Datenbank, enthält jedoch auch bewährte Methoden für Produktionsanwendungen.

Nach dem Lesen dieses Artikels können Sie die folgenden Fragen beantworten:   

* Wo kann ich eine Beispiel-.NET-Clientanwendung für Leistungstests von Cosmos DB finden? 
* Wie erreiche ich über meine Clientanwendung hohe Durchsätze mit Cosmos DB?

Sie können das Projekt aus dem [Azure Cosmos DB-Beispiel zur Leistungsüberprüfung](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark) herunterladen, um mit dem Programmieren zu beginnen. 

> [!NOTE]
> Das Ziel dieser Anwendung ist die Veranschaulichung von bewährten Methoden, um mit einer geringeren Anzahl von Clientcomputern eine bessere Leistung für Cosmos DB zu erzielen. Diese Anwendung soll nicht die Spitzenleistung des Diensts zeigen – dieser lässt sich grenzenlos skalieren.
> 
> 

Die clientseitigen Konfigurationsoptionen zur Verbesserung der Leistung von Cosmos DB finden Sie unter [Tipps zur Leistungssteigerung für Azure Cosmos DB](performance-tips.md).

## <a name="run-the-performance-testing-application"></a>Ausführen der Anwendung zur Leistungsüberprüfung
Die schnellste Einstiegsmethode ist das nachstehende .NET-Beispiel zu kompilieren und auszuführen, wie in den folgenden Schritten beschrieben. Sie können auch den Quellcode prüfen und Konfigurationen implementieren, die Ihren eigenen Clientanwendungen ähneln.

**Schritt 1:** Laden Sie das Projekt aus dem [Azure Cosmos DB-Beispiel zur Leistungsüberprüfung](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark) herunter, oder erstellen Sie eine Verzweigung des GitHub-Repositorys.

**Schritt 2:** Ändern Sie in der Datei „App.config“ die Einstellungen für „EndpointUrl“, „AuthorizationKey“, „CollectionThroughput“ und „DocumentTemplate“ (optional).

> [!NOTE]
> Besuchen Sie vor dem Bereitstellen von Sammlungen mit hohem Durchsatz die [Seite mit den Preisinformationen](https://azure.microsoft.com/pricing/details/documentdb/) , um die Kosten pro Sammlung abschätzen zu können. Cosmos DB rechnet Speicher und Durchsatz unabhängig voneinander auf Stundenbasis ab, sodass Sie Kosten sparen, indem Sie den Durchsatz Ihrer DocumentDB-Sammlungen nach dem Testen löschen oder verringern.
> 
> 

**Schritt 3:** Kompilieren Sie die Konsolen-App über die Befehlszeile, und führen Sie sie aus. Ihnen sollte eine Ausgabe wie die folgende angezeigt werden:

    Summary:
    ---------------------------------------------------------------------
    Endpoint: https://docdb-scale-demo.documents.azure.com:443/
    Collection : db.testdata at 50000 request units per second
    Document Template*: Player.json
    Degree of parallelism*: 500
    ---------------------------------------------------------------------

    DocumentDBBenchmark starting...
    Creating database db
    Creating collection testdata
    Creating metric collection metrics
    Retrying after sleeping for 00:03:34.1720000
    Starting Inserts with 500 tasks
    Inserted 661 docs @ 656 writes/s, 6860 RU/s (18B max monthly 1KB reads)
    Inserted 6505 docs @ 2668 writes/s, 27962 RU/s (72B max monthly 1KB reads)
    Inserted 11756 docs @ 3240 writes/s, 33957 RU/s (88B max monthly 1KB reads)
    Inserted 17076 docs @ 3590 writes/s, 37627 RU/s (98B max monthly 1KB reads)
    Inserted 22106 docs @ 3748 writes/s, 39281 RU/s (102B max monthly 1KB reads)
    Inserted 28430 docs @ 3902 writes/s, 40897 RU/s (106B max monthly 1KB reads)
    Inserted 33492 docs @ 3928 writes/s, 41168 RU/s (107B max monthly 1KB reads)
    Inserted 38392 docs @ 3963 writes/s, 41528 RU/s (108B max monthly 1KB reads)
    Inserted 43371 docs @ 4012 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 48477 docs @ 4035 writes/s, 42282 RU/s (110B max monthly 1KB reads)
    Inserted 53845 docs @ 4088 writes/s, 42845 RU/s (111B max monthly 1KB reads)
    Inserted 59267 docs @ 4138 writes/s, 43364 RU/s (112B max monthly 1KB reads)
    Inserted 64703 docs @ 4197 writes/s, 43981 RU/s (114B max monthly 1KB reads)
    Inserted 70428 docs @ 4216 writes/s, 44181 RU/s (115B max monthly 1KB reads)
    Inserted 75868 docs @ 4247 writes/s, 44505 RU/s (115B max monthly 1KB reads)
    Inserted 81571 docs @ 4280 writes/s, 44852 RU/s (116B max monthly 1KB reads)
    Inserted 86271 docs @ 4273 writes/s, 44783 RU/s (116B max monthly 1KB reads)
    Inserted 91993 docs @ 4299 writes/s, 45056 RU/s (117B max monthly 1KB reads)
    Inserted 97469 docs @ 4292 writes/s, 44984 RU/s (117B max monthly 1KB reads)
    Inserted 99736 docs @ 4192 writes/s, 43930 RU/s (114B max monthly 1KB reads)
    Inserted 99997 docs @ 4013 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 100000 docs @ 3846 writes/s, 40304 RU/s (104B max monthly 1KB reads)

    Summary:
    ---------------------------------------------------------------------
    Inserted 100000 docs @ 3834 writes/s, 40180 RU/s (104B max monthly 1KB reads)
    ---------------------------------------------------------------------
    DocumentDBBenchmark completed successfully.


**Schritt 4 (bei Bedarf):** Der vom Tool gemeldete Durchsatz (RU/s) muss mindestens dem bereitgestellten Durchsatz der Sammlung entsprechen. Wenn dies nicht der Fall ist, kann die Erhöhung von „DegreeOfParallelism“ in kleinen Schritten dabei helfen, den Grenzwert zu erreichen. Wenn der Durchsatz Ihrer Clientanwendung seinen Höchstwert erreicht, hilft Ihnen das Starten mehrerer App-Instanzen auf demselben oder auf verschiedenen Computern dabei, den bereitgestellten Grenzwert auf den verschiedenen Instanzen zu erreichen. Wenn Sie bei diesem Schritt Hilfe benötigen, schreiben Sie bitte eine E-Mail an askdocdb@microsoft.com, oder legen Sie im [Azure-Portal](https://portal.azure.com) ein Supportticket an.

Sobald die App ausgeführt wird, können Sie verschiedene [Indizierungsrichtlinien](indexing-policies.md) und [Konsistenzebenen](consistency-levels.md) ausprobieren, um die Auswirkungen auf Durchsatz und Latenz zu ermitteln. Sie können auch den Quellcode prüfen und Konfigurationen implementieren, die Ihren eigenen Testsammlungen oder Produktionsanwendungen ähneln.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie Sie mithilfe einer .NET-Konsolenanwendung die Leistung und Skalierung mit Cosmos DB testen können. Weitere Informationen zu der Arbeit mit Cosmos DB finden Sie unter den folgenden Links.

* [Beispiel zu Leistungstests in Azure Cosmos DB](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [Clientkonfigurationsoptionen zur Verbesserung der Leistung von Azure Cosmos DB](performance-tips.md)
* [Serverseitige Partitionierung in Azure Cosmos DB](partition-data.md)
* [DocumentDB-Sammlungen und -Leistungsstufen](performance-levels.md)
* [DocumentDB .NET SDK-Dokumentation auf MSDN](https://msdn.microsoft.com/library/azure/dn948556.aspx)
* [DocumentDB .NET-Beispiele](https://github.com/Azure/azure-documentdb-net)
* [Azure Cosmos DB-Blog mit Leistungstipps](https://azure.microsoft.com/blog/2015/01/20/performance-tips-for-azure-documentdb-part-1-2/)


