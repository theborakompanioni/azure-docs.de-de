---
title: Tutorial zur globalen Verteilung von Azure Cosmos DB mit der Tabellen-API | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die globale Verteilung von Azure Cosmos DB mithilfe der Tabellen-API einrichten.
services: cosmosdb
keywords: globale Verteilung, Tabelle
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 3282dd96fb51ba9eeb27560c6897211c55738f1e
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-table-api"></a>Einrichten der globalen Verteilung von Azure Cosmos DB mithilfe der Tabellen-API

In diesem Artikel wird beschrieben, wie Sie mit dem Azure-Portal die globale Verteilung von Azure Cosmos DB einrichten und dann mithilfe der Tabellen-API eine Verbindung herstellen (Vorschau).

In diesem Artikel werden die folgenden Aufgaben behandelt: 

> [!div class="checklist"]
> * Konfigurieren der globalen Verteilung mit dem Azure-Portal
> * Konfigurieren der globalen Verteilung mit der [Tabellen-API](table-introduction.md)

[!INCLUDE [cosmosdb-tutorial-global-distribution-portal](../../includes/cosmosdb-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-table-api"></a>Herstellen einer Verbindung mit einer bevorzugten Region mithilfe der Tabellen-API

Um von der [globalen Verteilung](../documentdb/documentdb-distribute-data-globally.md)zu profitieren, können Clientanwendungen in einer Liste die Reihenfolge angeben, in der Regionen bei Dokumentvorgängen bevorzugt verwendet werden sollen. Dies kann durch Festlegen des Konfigurationswerts `TablePreferredLocations` in der App-Konfiguration für die Vorschau des Azure Storage SDK vorgenommen werden. Basierend auf der Azure Cosmos DB-Kontokonfiguration, der aktuellen regionalen Verfügbarkeit und der angegebenen Liste der bevorzugten Regionen wählt das Azure Storage SDK den optimalen Endpunkt für Schreib- und Lesevorgänge aus.

`TablePreferredLocations` sollte eine durch Trennzeichen getrennte Liste der bevorzugten Standorte (Multihosting) für Lesevorgänge enthalten. Jede Clientinstanz kann eine Teilmenge dieser Regionen in der bevorzugten Reihenfolge für Lesevorgänge mit geringer Latenz angeben. Die Regionen müssen mit ihren [Anzeigenamen](https://msdn.microsoft.com/library/azure/gg441293.aspx) benannt sein, z.B. `West US`.

Alle Lesevorgänge werden an die erste verfügbare Region in der `TablePreferredLocations`-Liste gesendet. Wenn bei der Anforderung ein Fehler auftritt, führt der Client ein Failover zur nächsten Region auf der Liste durch, usw.

Das SDK versucht nur, in den in `TablePreferredLocations` angegebenen Regionen Lesevorgänge auszuführen. Wenn z.B. ein Datenbankkonto in drei Regionen verfügbar ist, der Client aber nur zwei Leseregionen für `TablePreferredLocations` angibt, werden auch bei einem Failover keine Lesevorgänge außerhalb der Schreibregion verarbeitet.

Das SDK sendet automatisch alle Schreibvorgänge an die aktuell für solche Vorgänge ausgewählte Region.

Wenn die Eigenschaft `TablePreferredLocations` nicht festgelegt ist, werden alle Anforderungen von der aktuellen Schreibregion verarbeitet.

```xml
    <appSettings>
      <add key="TablePreferredLocations" value="East US, West US, North Europe"/>           
    </appSettings>
```

Das ist alles, und dieses Tutorial ist abgeschlossen. Informationen dazu, wie Sie die Konsistenz Ihres global replizierten Kontos verwalten, finden Sie unter [Konsistenzebenen in Azure Cosmos DB](../documentdb/documentdb-consistency-levels.md). Weitere Informationen zur Funktionsweise der globalen Datenbankreplikation in Azure Cosmos DB finden Sie unter [Globale Verteilung von Daten mit Azure Cosmos DB](../documentdb/documentdb-distribute-data-globally.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die folgenden Aufgaben ausgeführt:

> [!div class="checklist"]
> * Konfigurieren der globalen Verteilung mit dem Azure-Portal
> * Konfigurieren der globalen Verteilung mit den DocumentDB-APIs

Sie können jetzt mit dem nächsten Tutorial fortfahren, um zu erfahren, wie Sie mit dem lokalen Azure Cosmos DB-Emulator lokal entwickeln können.

> [!div class="nextstepaction"]
> [Lokales Entwickeln mit dem Emulator](../documentdb/documentdb-nosql-local-emulator.md)
