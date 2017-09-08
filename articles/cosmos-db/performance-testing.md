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
ms.date: 08/29/2017
ms.author: arramac
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: 11f2f72073fd23c0a7c61dedb270f64d42b98025
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="performance-and-scale-testing-with-azure-cosmos-db"></a>Leistungs- und Skalierungstests mit Azure Cosmos DB
Leistungs- und Skalierungstests sind ein wichtiger Schritt bei der Entwicklung von Anwendungen. Die Datenbankebene hat bei vielen Anwendungen einen erheblichen Einfluss auf die allgemeine Leistung und Skalierbarkeit und ist daher eine wichtige Komponente von Leistungstests. [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) wurde speziell für die flexible Skalierung und vorhersagbare Leistung entwickelt und eignet sich deshalb hervorragend für Anwendungen, die eine leistungsstarke Datenbankebene benötigen. 

Dieser Artikel ist eine Referenz für Entwickler, die Leistungstestsammlungen für ihre Cosmos DB-Workloads implementieren oder Cosmos DB für leistungsstarke Anwendungsszenarien auswerten. Er konzentriert sich hauptsächlich auf isolierte Leistungstests der Datenbank, enthält jedoch auch bewährte Methoden für Produktionsanwendungen.

Nach dem Lesen dieses Artikels können Sie die folgenden Fragen beantworten:   

* Wo kann ich eine Beispiel-.NET-Clientanwendung für Leistungstests von Cosmos DB finden? 
* Wie erreiche ich über meine Clientanwendung hohe Durchsätze mit Cosmos DB?

Sie können das Projekt aus dem [Azure Cosmos DB-Beispiel zur Leistungsüberprüfung](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark) herunterladen, um mit dem Programmieren zu beginnen. 

> [!NOTE]
> Das Ziel dieser Anwendung ist die Veranschaulichung von Methoden, um mit einer geringen Anzahl von Clientcomputern eine optimale Leistung für Cosmos DB zu erzielen. Das Ziel des Beispiels ist nicht, die maximale Durchsatzkapazität von Azure-Cosmos DB zu erreichen (die ohne Grenzwerte skaliert werden kann).
> 
> 

Die clientseitigen Konfigurationsoptionen zur Verbesserung der Leistung von Cosmos DB finden Sie unter [Tipps zur Leistungssteigerung für Azure Cosmos DB](performance-tips.md).

## <a name="run-the-performance-testing-application"></a>Ausführen der Anwendung zur Leistungsüberprüfung
Der schnellste Einstieg erfolgt durch das Kompilieren und Ausführen des .NET-Beispiels, wie in den folgenden Schritten beschrieben. Sie können auch den Quellcode überprüfen und ähnliche Konfigurationen in Ihren eigenen Clientanwendungen implementieren.

**Schritt 1:** Laden Sie das Projekt aus dem [Azure Cosmos DB-Beispiel zur Leistungsüberprüfung](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark) herunter, oder erstellen Sie eine Verzweigung des GitHub-Repositorys.

**Schritt 2:** Ändern Sie in der Datei „App.config“ die Einstellungen für EndpointUrl, AuthorizationKey, CollectionThroughput und DocumentTemplate (optional).

> [!NOTE]
> Besuchen Sie vor dem Bereitstellen von Sammlungen mit hohem Durchsatz die [Seite mit den Preisinformationen](https://azure.microsoft.com/pricing/details/cosmos-db/), um die Kosten pro Sammlung abschätzen zu können. Azure Cosmos DB rechnet Speicher und Durchsatz unabhängig voneinander auf Stundenbasis ab, sodass Sie Kosten sparen, indem Sie den Durchsatz Ihrer Azure Cosmos DB-Sammlungen nach dem Testen löschen oder verringern.
> 
> 

**Schritt 3:** Kompilieren Sie die Konsolen-App über die Befehlszeile, und führen Sie sie aus. Ihnen sollte eine Ausgabe wie die folgende angezeigt werden:

    C:\Users\documentdb\Desktop\Benchmark>DocumentDBBenchmark.exe
    Summary:
    ---------------------------------------------------------------------
    Endpoint: https://arramacquerymetrics.documents.azure.com:443/
    Collection : db.data at 100000 request units per second
    Document Template*: Player.json
    Degree of parallelism*: -1
    ---------------------------------------------------------------------

    DocumentDBBenchmark starting...
    Found collection data with 100000 RU/s
    Starting Inserts with 100 tasks
    Inserted 4503 docs @ 4491 writes/s, 47070 RU/s (122B max monthly 1KB reads)
    Inserted 17910 docs @ 8862 writes/s, 92878 RU/s (241B max monthly 1KB reads)
    Inserted 32339 docs @ 10531 writes/s, 110366 RU/s (286B max monthly 1KB reads)
    Inserted 47848 docs @ 11675 writes/s, 122357 RU/s (317B max monthly 1KB reads)
    Inserted 58857 docs @ 11545 writes/s, 120992 RU/s (314B max monthly 1KB reads)
    Inserted 69547 docs @ 11378 writes/s, 119237 RU/s (309B max monthly 1KB reads)
    Inserted 80687 docs @ 11345 writes/s, 118896 RU/s (308B max monthly 1KB reads)
    Inserted 91455 docs @ 11272 writes/s, 118131 RU/s (306B max monthly 1KB reads)
    Inserted 102129 docs @ 11208 writes/s, 117461 RU/s (304B max monthly 1KB reads)
    Inserted 112444 docs @ 11120 writes/s, 116538 RU/s (302B max monthly 1KB reads)
    Inserted 122927 docs @ 11063 writes/s, 115936 RU/s (301B max monthly 1KB reads)
    Inserted 133157 docs @ 10993 writes/s, 115208 RU/s (299B max monthly 1KB reads)
    Inserted 144078 docs @ 10988 writes/s, 115159 RU/s (298B max monthly 1KB reads)
    Inserted 155415 docs @ 11013 writes/s, 115415 RU/s (299B max monthly 1KB reads)
    Inserted 166126 docs @ 10992 writes/s, 115198 RU/s (299B max monthly 1KB reads)
    Inserted 173051 docs @ 10739 writes/s, 112544 RU/s (292B max monthly 1KB reads)
    Inserted 180169 docs @ 10527 writes/s, 110324 RU/s (286B max monthly 1KB reads)
    Inserted 192469 docs @ 10616 writes/s, 111256 RU/s (288B max monthly 1KB reads)
    Inserted 199107 docs @ 10406 writes/s, 109054 RU/s (283B max monthly 1KB reads)
    Inserted 200000 docs @ 9930 writes/s, 104065 RU/s (270B max monthly 1KB reads)

    Summary:
    ---------------------------------------------------------------------
    Inserted 200000 docs @ 9928 writes/s, 104063 RU/s (270B max monthly 1KB reads)
    ---------------------------------------------------------------------
    DocumentDBBenchmark completed successfully.
    Press any key to exit...


**Schritt 4 (bei Bedarf):** Der vom Tool gemeldete Durchsatz (RU/s) muss mindestens dem bereitgestellten Durchsatz der Sammlung entsprechen. Wenn dies nicht der Fall ist, kann die Erhöhung von „DegreeOfParallelism“ in kleinen Schritten dabei helfen, den Grenzwert zu erreichen. Wenn der Durchsatz Ihrer Client-App stagniert, starten Sie mehrere Instanzen der App auf zusätzlichen Clientcomputern. Wenn Sie bei diesem Schritt Hilfe benötigen, schreiben Sie eine E-Mail an askcosmosdb@microsoft.com, oder legen Sie im [Azure-Portal](https://portal.azure.com) ein Supportticket an.

Sobald die App ausgeführt wird, können Sie verschiedene [Indizierungsrichtlinien](indexing-policies.md) und [Konsistenzebenen](consistency-levels.md) ausprobieren, um die Auswirkungen auf Durchsatz und Latenz zu ermitteln. Sie können auch den Quellcode prüfen und Konfigurationen implementieren, die Ihren eigenen Testsammlungen oder Produktionsanwendungen ähneln.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie Sie mithilfe einer .NET-Konsolenanwendung die Leistung und Skalierung mit Cosmos DB testen können. Weitere Informationen finden Sie in den folgenden Artikeln.

* [Beispiel zu Leistungstests in Azure Cosmos DB](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [Clientkonfigurationsoptionen zur Verbesserung der Leistung von Azure Cosmos DB](performance-tips.md)
* [Serverseitige Partitionierung in Azure Cosmos DB](partition-data.md)



