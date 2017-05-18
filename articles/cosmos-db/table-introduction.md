---
title: "Einführung in die Table-API von Azure Cosmos DB | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Azure Cosmos DB verwenden können, um riesige Mengen von Schlüssel-Wert-Daten mit geringer Latenzzeit mithilfe der gängigen OSS MongoDB-APIs zu speichern und abzufragen."
services: cosmosdb
author: bhanupr
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/10/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 8226ecfb5fa8c70b162ae776679302db6cfdcdbc
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="introduction-to-azure-cosmos-db-table-api"></a>Einführung in die Table-API von Azure Cosmos DB

[Azure Cosmos DB](introduction.md) ist ein global verteilter Datenbankdienst von Microsoft mit mehreren Modellen für unternehmenskritische Anwendungen. Azure Cosmos DB bietet [sofort einsetzbare globale Verteilung](../documentdb/documentdb-distribute-data-globally.md), [flexible Skalierung von Durchsatz und Speicher](partition-data.md) weltweit, Latenzzeiten im einstelligen Millisekundenbereich im 99. Perzentil, [fünf wohldefinierte Konsistenzebenen](../documentdb/documentdb-consistency-levels.md) sowie garantierte hohe Verfügbarkeit, gestützt durch [branchenführende Vereinbarungen zum Servicelevel](https://azure.microsoft.com/support/legal/sla/documentdb/v1_1/) (SLA, Service Level Agreement). Azure Cosmos DB [indiziert Daten automatisch](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf), ohne dass Sie sich mit der Verwaltung von Schemas und Indizes auseinandersetzen müssen. Es unterstützt mehrere Datenmodelle – Dokumente, Schlüssel-Werte, Diagramme und spaltenorientierte Datenmodelle. 

![Azure Table Storage-API und Azure Cosmos DB](./media/table-introduction/premium-tables.png) 

Azure Cosmos DB bietet die Table-API für Anwendungen, die einen Schlüssel-Wert-Speicher mit flexiblen Schemas benötigen. Die SDKs und REST-APIs von [Azure Table Storage](../storage/storage-introduction.md) können zum Arbeiten mit Azure Cosmos DB verwendet werden. Mit Azure Cosmos DB können Sie Tabellen mit hohem Durchsatzanforderungen erstellen. Azure Cosmos DB unterstützt durchsatzoptimierte Tabellen (informell als „Premium Tables“ bezeichnet), die derzeit in der öffentlichen Vorschau verfügbar sind. 

Sie können Azure Table Storage weiterhin für Tabellen mit hohen Speicheranforderungen und niedrigeren Durchsatzanforderungen verwenden. In einem zukünftigen Update von Azure Cosmos DB wird die Unterstützung speicheroptimierter Tabellen eingeführt, und für vorhandene und neue Azure Table Storage-Konten wird ein Upgrade auf Azure Cosmos DB durchgeführt.

## <a name="premium-and-standard-table-apis"></a>Premium- und Standard-Table-APIs
Wenn Sie derzeit Azure Table Storage verwenden, bietet Ihnen der Wechsel zur „Premium Table“-Vorschau von Azure Cosmos DB folgende Vorteile:

|  | Azure Table Storage | Azure Cosmos DB: Table Storage (Vorschau) |
| --- | --- | --- |
| Latenz | Schnell, aber keine Obergrenzen für die Latenzzeit | Latenzzeit im einstelligen Millisekundenbereich für Lese- und Schreibvorgänge, gesichert durch <10 ms Latenz bei Lese- und <15 ms Latenz bei Schreibvorgängen im 99. Perzentil, bei beliebiger Skalierung weltweit |
| Durchsatz | Hochgradig skalierbar, aber kein dediziertes Durchsatzmodell. Tabellen verfügen über eine maximale Skalierbarkeit von 20.000 Vorgängen/s | Hochgradig skalierbar mit einem [dedizierten reservierten Durchsatz pro Tabelle](../documentdb/documentdb-request-units.md), der durch SLAs gesichert wird. Konten haben keine Obergrenze für den Durchsatz und unterstützen >10 Millionen Vorgänge/s pro Tabelle |
| Globale Verteilung | Einzelne Region mit einer optionalen sekundären Leseregion für hohe Verfügbarkeit. Es kann kein Failover initiiert werden | [Sofort einsetzbare globale Verteilung](../documentdb/documentdb-distribute-data-globally.md) aus einer bis in über 30 Regionen, Unterstützung von [automatischen und manuellen Failovern](../documentdb/documentdb-regional-failovers.md) jederzeit weltweit |
| Indizierung | Nur primärer Index für PartitionKey und RowKey. Keine sekundären Indizes | Automatische und vollständige Indizierung für alle Eigenschaften, keine Indexverwaltung |
| Abfrage | Abfrageausführung verwendet Index für Primärschlüssel, andernfalls die Suche. | Abfragen können die automatische Indizierung für Eigenschaften für schnelle Abfragezeiten nutzen. Das Datenbankmodul von Azure Cosmos DB kann Aggregate, Geoabfragen und Sortieren unterstützen |
| Konsistenz | Stark innerhalb der primären Region, eventuell in der sekundären Region | [Fünf wohldefinierte Konsistenzebenen](../documentdb/documentdb-consistency-levels.md) zur Abstimmung von Verfügbarkeit, Latenz, Durchsatz und Konsistenz basierend auf Ihren Anwendungsanforderungen |
| Preise | Speicheroptimiert  | Durchsatzoptimiert |
| SLAs | Verfügbarkeit von 99,9 % | Verfügbarkeit von 99,99 % in einer einzelnen Region und die Möglichkeit, weitere Regionen für höhere Verfügbarkeit hinzuzufügen. [Branchenführende, umfassende SLAs](https://azure.microsoft.com/support/legal/sla/documentdb/v1_1/) zur allgemeinen Verfügbarkeit |

## <a name="how-to-get-started"></a>Erste Schritte

Erstellen Sie im [Azure-Portal](https://portal.azure.com) ein Azure Cosmos DB-Konto, und beginnen Sie mit [Quickstart for Table API using .NET (Schnellstart für Table-API mit .NET)](create-table-dotnet.md). 

## <a name="next-steps"></a>Nächste Schritte

Hier einige Hinweise, um Ihnen den Einstieg zu erleichtern:
* Machen Sie die ersten Schritte mit [Azure Cosmos DB: Build a .NET application using the Table API (Azure Cosmos DB: Erstellen einer .NET-Anwendung mit der Table-API)](create-table-dotnet.md) mithilfe des vorhandenen .NET Table SDK.
* Erfahren Sie mehr über die [globale Verteilung mit Azure Cosmos DB](../documentdb/documentdb-distribute-data-globally.md).
* Erfahren Sie mehr über den [bereitgestellten Durchsatz in Azure Cosmos DB](../documentdb/documentdb-request-units.md).
