---
title: Tutorial zur globalen Verteilung von Azure Cosmos DB mit der MongoDB-API | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die globale Verteilung von Azure Cosmos DB mithilfe der MongoDB-API einrichten.
services: cosmos-db
keywords: globale Verteilung, MongoDB
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: a643f139be40b9b11f865d528622bafbe7dec939
ms.openlocfilehash: 119ebb3f4966de08934c7d1fbd139229bda1d060
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-mongodb-api"></a>Einrichten der globalen Verteilung von Azure Cosmos DB mithilfe der MongoDB-API

In diesem Artikel wird beschrieben, wie Sie mit dem Azure-Portal die globale Verteilung von Azure Cosmos DB einrichten und dann mithilfe der MongoDB-API eine Verbindung herstellen.

In diesem Artikel werden die folgenden Aufgaben behandelt: 

> [!div class="checklist"]
> * Konfigurieren der globalen Verteilung mit dem Azure-Portal
> * Konfigurieren der globalen Verteilung mit der [MongoDB-API](mongodb-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]

## <a name="verifying-your-regional-setup-using-the-mongodb-api"></a>Überprüfen des regionalen Setups mit der MongoDB-API
Am einfachsten lässt sich die globale Konfiguration innerhalb von API für MongoDB überprüfen, indem Sie den Befehl *isMaster()* in der Mongo Shell ausführen.

In der Mongo Shell:

   ```
      db.isMaster()
   ```
   
Beispielergebnisse:

   ```JSON
      {
         "_t": "IsMasterResponse",
         "ok": 1,
         "ismaster": true,
         "maxMessageSizeBytes": 4194304,
         "maxWriteBatchSize": 1000,
         "minWireVersion": 0,
         "maxWireVersion": 2,
         "tags": {
            "region": "South India"
         },
         "hosts": [
            "vishi-api-for-mongodb-southcentralus.documents.azure.com:10250",
            "vishi-api-for-mongodb-westeurope.documents.azure.com:10250",
            "vishi-api-for-mongodb-southindia.documents.azure.com:10250"
         ],
         "setName": "globaldb",
         "setVersion": 1,
         "primary": "vishi-api-for-mongodb-southindia.documents.azure.com:10250",
         "me": "vishi-api-for-mongodb-southindia.documents.azure.com:10250"
      }
   ```

## <a name="connecting-to-a-preferred-region-using-the-mongodb-api"></a>Herstellen einer Verbindung mit einer bevorzugten Region mithilfe der MongoDB-API

Die MongoDB-API ermöglicht es Ihnen, Voreinstellungen für Lesevorgänge in Ihrer Sammlung für eine global verteilte Datenbank anzugeben. Sowohl für Lesevorgänge mit niedriger Latenz als auch für eine globale hohe Verfügbarkeit empfiehlt es sich, die Voreinstellung Ihrer Sammlung für Lesevorgänge auf *nächste* festzulegen. Mit der Einstellung *nächste* erfolgen Lesevorgänge aus der nächstgelegenen Region.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Nearest));
```

Bei Anwendungen mit einer primären Lese-/Schreibregion und einer sekundären Region für Notfallwiederherstellungsszenarien empfiehlt es sich, die Voreinstellung Ihrer Sammlung für Lesevorgänge auf *sekundäre bevorzugte* festzulegen. Mit der Einstellung *sekundäre bevorzugte* erfolgen Lesevorgänge aus der sekundären Region, wenn die primäre Region nicht verfügbar ist.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.SecondaryPreferred));
```

Sie können Leseregionen auch manuell angeben. Legen Sie das entsprechende Regionstag in Ihrer Voreinstellung für Lesevorgänge fest.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
var tag = new Tag("region", "Southeast Asia");
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Secondary, new[] { new TagSet(new[] { tag }) }));
```

Das ist alles, und dieses Tutorial ist abgeschlossen. Informationen dazu, wie Sie die Konsistenz Ihres global replizierten Kontos verwalten, finden Sie unter [Konsistenzebenen in Azure Cosmos DB](consistency-levels.md). Weitere Informationen zur Funktionsweise der globalen Datenbankreplikation in Azure Cosmos DB finden Sie unter [Globale Verteilung von Daten mit Azure Cosmos DB](distribute-data-globally.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die folgenden Aufgaben ausgeführt:

> [!div class="checklist"]
> * Konfigurieren der globalen Verteilung mit dem Azure-Portal
> * Konfigurieren der globalen Verteilung mit den DocumentDB-APIs

Sie können jetzt mit dem nächsten Tutorial fortfahren, um zu erfahren, wie Sie mit dem lokalen Azure Cosmos DB-Emulator lokal entwickeln können.

> [!div class="nextstepaction"]
> [Lokales Entwickeln mit dem Emulator](local-emulator.md)
