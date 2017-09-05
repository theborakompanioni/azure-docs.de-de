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
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: 3ccc3b176df2f0a5d864554a74508292d272bd5a
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="introduction-to-azure-cosmos-db-table-api"></a>Einführung in die Table-API von Azure Cosmos DB

[Microsoft Azure Cosmos DB](introduction.md) bietet die Table-API (Vorschauversion) für Anwendungen, die für Azure Table Storage-Dienst entwickelt wurden, und benötigt Premium-Features wie [sofort einsatzbereite globale Verteilung](distribute-data-globally.md), [dedizierten Durchsatz](partition-data.md) weltweit, vorhersagbare Latenzen im einstelligen Millisekundenbereich im 99. Perzentil, garantierte Hochverfügbarkeit und [automatische sekundäre Indizierung](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). Diese Anwendungen können mit der Table-API ohne Codeänderungen zu Azure-Cosmos DB migriert werden und Premium-Funktionen nutzen.

Wir empfehlen Ihnen, sich zunächst das folgende Video anzusehen, in dem Aravind Ramachandran die ersten Schritte mit der Table-API für Azure Cosmos DB erläutert.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Table-API-for-Azure-Cosmos-DB/player]
> 
> 

## <a name="premium-and-standard-table-apis"></a>Premium- und Standard-Table-APIs
Wenn Sie derzeit Azure Table Storage verwenden, bietet Ihnen der Wechsel zur „Premium Table“-Vorschau von Azure Cosmos DB folgende Vorteile:

|  | Azure Table Storage | Azure Cosmos DB: Table Storage (Vorschau) |
| --- | --- | --- |
| Latenz | Schnell, aber keine Obergrenzen für die Latenzzeit | Latenzzeit im einstelligen Millisekundenbereich für Lese- und Schreibvorgänge, gesichert durch <10 ms Latenz bei Lese- und <15 ms Latenz bei Schreibvorgängen im 99. Perzentil, bei beliebiger Skalierung weltweit |
| Durchsatz | Variables Durchsatzmodell. Tabellen verfügen über eine maximale Skalierbarkeit von 20.000 Vorgängen/s | Hochgradig skalierbar mit einem [dedizierten reservierten Durchsatz pro Tabelle](request-units.md), der durch SLAs gesichert wird. Konten haben keine Obergrenze für den Durchsatz und unterstützen >10 Millionen Vorgänge/s pro Tabelle |
| Globale Verteilung | Einzelne Region mit einer optionalen sekundären Leseregion für hohe Verfügbarkeit. Es kann kein Failover initiiert werden | [Sofort einsetzbare globale Verteilung](distribute-data-globally.md) aus einer bis in über 30 Regionen, Unterstützung von [automatischen und manuellen Failovern](regional-failover.md) jederzeit weltweit |
| Indizierung | Nur primärer Index für PartitionKey und RowKey. Keine sekundären Indizes | Automatische und vollständige Indizierung für alle Eigenschaften, keine Indexverwaltung |
| Abfrage | Abfrageausführung verwendet Index für Primärschlüssel, andernfalls die Suche. | Abfragen können die automatische Indizierung für Eigenschaften für schnelle Abfragezeiten nutzen. Das Datenbankmodul von Azure Cosmos DB kann Aggregate, Geoabfragen und Sortieren unterstützen |
| Konsistenz | Stark innerhalb der primären Region, eventuell in der sekundären Region | [Fünf klar definierte Konsistenzebenen](consistency-levels.md) zur Abstimmung von Verfügbarkeit, Latenz, Durchsatz und Konsistenz basierend auf Ihren Anwendungsanforderungen |
| Preise | Speicheroptimiert  | Durchsatzoptimiert |
| SLAs | Verfügbarkeit von 99,9 % | Verfügbarkeit von 99,99 % in einer einzelnen Region und die Möglichkeit, weitere Regionen für höhere Verfügbarkeit hinzuzufügen. [Branchenführende, umfassende SLAs](https://azure.microsoft.com/support/legal/sla/cosmos-db/) zur allgemeinen Verfügbarkeit |

## <a name="how-to-get-started"></a>Erste Schritte

Erstellen Sie im [Azure-Portal](https://portal.azure.com) ein Azure Cosmos DB-Konto, und beginnen Sie mit [Quickstart for Table API using .NET (Schnellstart für Table-API mit .NET)](create-table-dotnet.md). 

## <a name="next-steps"></a>Nächste Schritte

Hier einige Hinweise, um Ihnen den Einstieg zu erleichtern:
* [Erstellen einer .NET-Anwendung mit der Table-API](create-table-dotnet.md)
* [Entwickeln mit der Table-API in .NET](tutorial-develop-table-dotnet.md)
* [Abfragen von Tabellendaten mit der Table-API](tutorial-query-table.md)
* [Einrichten der globalen Verteilung von Azure Cosmos DB mit der Table-API](tutorial-global-distribution-table.md)
* [Azure Cosmos DB Table-API SDK für .NET](table-sdk-dotnet.md)

