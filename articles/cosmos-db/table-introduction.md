---
title: "Einführung in die Table-API von Azure Cosmos DB | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Azure Cosmos DB verwenden können, um riesige Mengen von Schlüssel-Wert-Daten mit geringer Latenzzeit mithilfe der gängigen OSS MongoDB-APIs zu speichern und abzufragen."
services: cosmos-db
author: bhanupr
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/29/2017
ms.author: arramac
ms.translationtype: HT
ms.sourcegitcommit: f2ac16c2f514aaa7e3f90fdf0d0b6d2912ef8485
ms.openlocfilehash: 2073948d44ccc4b9b83e4eaf4f250dc272e46292
ms.contentlocale: de-de
ms.lasthandoff: 09/08/2017

---
# <a name="introduction-to-azure-cosmos-db-table-api"></a>Einführung in die Table-API von Azure Cosmos DB

Mit [Azure Cosmos DB](introduction.md) wird die Table-API (Vorschauversion) für Anwendungen bereitgestellt, die für Azure Table Storage geschrieben wurden und beispielsweise folgende Premium-Funktionen benötigen:

* [Globale, sofort einsatzbereite Verteilung](distribute-data-globally.md).
* [Dedizierter Durchsatz](partition-data.md) weltweit.
* Einstellige Latenzzeiten im Millisekundenbereich im 99. Perzentil.
* Garantierte Hochverfügbarkeit.
* [Automatische sekundäre Indizierung](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf).

Diese Anwendungen können mit der Table-API ohne Codeänderungen zu Azure Cosmos DB migriert werden und Premium-Funktionen nutzen.

Wir empfehlen Ihnen, sich das folgende Video anzusehen, in dem Aravind Ramachandran die ersten Schritte mit der Table-API für Azure Cosmos DB erläutert:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Table-API-for-Azure-Cosmos-DB/player]
> 
> 

## <a name="premium-and-standard-table-apis"></a>Premium- und Standard-Table-APIs
Wenn Sie derzeit Table Storage verwenden, bietet Ihnen der Wechsel zur „Premium Table“-Vorschauversion von Azure Cosmos DB folgende Vorteile:

| | Azure-Tabellenspeicher | Azure Cosmos DB: Table Storage (Vorschau) |
| --- | --- | --- |
| Latenz | Schnell, aber keine Obergrenzen für die Wartezeit. | Wartezeit im einstelligen Millisekundenbereich für Lese- und Schreibvorgänge, unterstützt durch weniger als 10 ms Wartezeit bei Lese- und weniger als 15 ms Wartezeit bei Schreibvorgängen im 99. Perzentil, bei beliebiger Skalierung weltweit. |
| Durchsatz | Variables Durchsatzmodell. Tabellen verfügen über eine maximale Skalierbarkeit von 20.000 Vorgängen/s. | Hochgradig skalierbar mit einem [dedizierten reservierten Durchsatz pro Tabelle](request-units.md), der durch SLAs abgedeckt ist. Konten haben keine Obergrenze für den Durchsatz und unterstützen pro Tabelle mehr als 10 Millionen Vorgänge/s. |
| Globale Verteilung | Einzelne Region mit einer optionalen sekundären Leseregion für Hochverfügbarkeit. Es kann kein Failover initiiert werden. | [Globale, sofort einsatzbereite Verteilung](distribute-data-globally.md) für 1 bis mehr als 30 Regionen. Unterstützung von [automatischen und manuellen Failovern](regional-failover.md) zu jeder Zeit und an jedem Ort der Welt. |
| Indizierung | Nur primärer Index für PartitionKey und RowKey. Keine sekundären Indizes. | Automatische und vollständige Indizierung für alle Eigenschaften, keine Indexverwaltung. |
| Abfrage | Abfrageausführung verwendet Index für Primärschlüssel, andernfalls die Suche. | Abfragen können die automatische Indizierung für Eigenschaften für schnelle Abfragezeiten nutzen. Das Datenbankmodul von Azure Cosmos DB kann Aggregate, Geofunktionen und die Sortierung unterstützen. |
| Konsistenz | „Stark“ in der primären Region. „Möglich“ in der sekundären Region. | [Fünf klar definierte Konsistenzebenen](consistency-levels.md) zur Abstimmung von Verfügbarkeit, Latenz, Durchsatz und Konsistenz basierend auf Ihren Anwendungsanforderungen. |
| Preise | Speicheroptimiert | Durchsatzoptimiert |
| SLAs | Verfügbarkeit von 99,99%. | Verfügbarkeit von 99,99% in einer einzelnen Region und die Möglichkeit, weitere Regionen hinzuzufügen, um die Verfügbarkeit zu erhöhen. [Branchenführende, umfassende SLAs](https://azure.microsoft.com/support/legal/sla/cosmos-db/) zur allgemeinen Verfügbarkeit. |

## <a name="get-started"></a>Erste Schritte

Erstellen Sie im [Azure-Portal](https://portal.azure.com) ein Azure Cosmos DB-Konto. Fahren Sie anschließend mit dem [Schnellstart für die Table-API per .NET](create-table-dotnet.md) fort. 

## <a name="next-steps"></a>Nächste Schritte

Hier einige Hinweise, um Ihnen den Einstieg zu erleichtern:
* [Erstellen einer .NET-Anwendung mit der Table-API](create-table-dotnet.md)
* [Entwickeln mit der Table-API in .NET](tutorial-develop-table-dotnet.md)
* [Abfragen von Tabellendaten mit der Table-API](tutorial-query-table.md)
* [Einrichten der globalen Verteilung von Azure Cosmos DB mithilfe der Table-API](tutorial-global-distribution-table.md)
* [Azure Cosmos DB Table-API SDK für .NET](table-sdk-dotnet.md)

