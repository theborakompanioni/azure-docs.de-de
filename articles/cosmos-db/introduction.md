---
title: "Einführung in Azure Cosmos DB | Microsoft-Dokumentation"
description: "Hier finden Sie Informationen zu Azure Cosmos DB. Diese global verteilte Datenbank mit mehreren Modellen ist für geringe Wartezeiten, flexible Skalierbarkeit und hohe Verfügbarkeit konzipiert."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: a855183f-34d4-49cc-9609-1478e465c3b7
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/14/2017
ms.author: mimig
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 2812039649f7d2fb0705220854e4d8d0a031d31e
ms.openlocfilehash: 600894bffe367ee1412df6a82f668143829688cc
ms.contentlocale: de-de
ms.lasthandoff: 07/22/2017

---

# <a name="welcome-to-azure-cosmos-db"></a>Willkommen bei Azure Cosmos DB

Azure Cosmos DB ist eine global verteilte Datenbank von Microsoft mit mehreren Modellen. Mit einem Klick ermöglicht Azure Cosmos DB Ihnen, Durchsatz und Speicher elastisch und unabhängig voneinander über eine beliebige Anzahl von geografischen Azure-Regionen hinweg zu skalieren. Azure Cosmos DB bietet Ihnen mit umfassenden [Vereinbarungen zum Servicelevel](https://aka.ms/acdbsla) (Service Level Agreements, SLAs) Durchsatz-, Wartezeit-, Verfügbarkeits- und Konsistenzgarantien wie kein anderer Datenbankdienst.

![Azure Cosmos DB ist ein weltweit verteilter Datenbankdienst von Microsoft mit elastischer horizontaler Hochskalierung, garantierter geringer Wartezeit, fünf Konsistenzmodellen und umfassenden SLA-Garantien.](./media/introduction/azure-cosmos-db.png)

## <a name="solutions-that-benefit-from-azure-cosmos-db"></a>Lösungen, die von Azure-Cosmos-DB profitieren

Von der [garantierten](https://azure.microsoft.com/support/legal/sla/cosmos-db/) Verfügbarkeit, dem hohen Durchsatz, der kurzen Wartezeit und der justierbaren Konsistenz profitieren alle [Web-, Mobil-, Gaming- und IoT-Anwendungen](use-cases.md), die [global](distribute-data-globally.md) große Mengen an Lese- und Schreibvorgängen mit niedrigen Reaktionszeiten für verschiedenste Daten bewältigen müssen.

## <a name="key-capabilities"></a>Wichtige Funktionen
Als global verteilter Datenbankdienst bietet Azure Cosmos DB die folgenden Funktionen, die Ihnen bei der Erstellung skalierbarer, extrem reaktionsschneller Anwendungen helfen:

* **Globale, sofort einsatzbereite Verteilung**
    * Sie haben die Möglichkeit, Ihre Daten [auf Knopfdruck](tutorial-global-distribution-documentdb.md) auf beliebig viele [Azure-Regionen](https://azure.microsoft.com/regions/) zu [verteilen](distribute-data-globally.md). Dadurch können Sie Ihre Daten in der Nähe Ihrer Benutzer platzieren und eine möglichst kurze Wartezeit für Ihre Kunden gewährleisten. 
    * Durch die Multihosting-APIs von Azure Cosmos DB weiß die App immer, welche Region am nächsten ist, und kann Anforderungen an das nächstgelegene Rechenzentrum senden. Für all das sind keinerlei Konfigurationsänderungen erforderlich. Sie legen einfach nur Ihre Schreibregion und beliebig viele Leseregionen fest. Der Rest erfolgt automatisch.

* **Mehrere Datenmodelle und beliebte APIs für den Zugriff auf und die Abfrage von Daten**
    * Das ARS-basierte Datenmodell (Atom Record Sequence), das Azure Cosmos DB zugrunde liegt, unterstützt von Haus aus mehrere Datenmodelle. Hierzu zählen unter anderem Modelle für Dokumente, Diagramme, Schlüssel-Wert-Paare, Tabellen und Spaltendaten.
    * APIs für die folgenden Datenmodelle werden über SDKs unterstützt, die in verschiedenen Sprachen verfügbar sind:
        * [DocumentDB-API](documentdb-introduction.md)
        * [MongoDB-API](mongodb-introduction.md)
        * [Tabellen-API](table-introduction.md)
        * [Diagramm-API (Gremlin)](graph-introduction.md)
        * Weitere Datenmodelle folgen in Kürze. 

* **Elastische Skalierung von Durchsatz und Speicher – bedarfsorientiert und weltweit**
    * Skalieren Sie den Datenbankdurchsatz [sekunden-](request-units.md) und [minutengenau](request-units-per-minute.md), und nehmen Sie Änderungen vor, wann immer sie möchten. 
    * Skalieren Sie die Speichergröße [transparent und automatisch](partition-data.md), um jederzeit Ihre Größenanforderungen zu erfüllen.

* **Erstellen extrem reaktionsschneller unternehmenskritischer Anwendungen**
    * Azure Cosmos DB garantiert seinen Kunden eine kurze End-to-End-Wartezeit im 99. Perzentil. 
    * Für ein typisches 1-KB-Element garantiert Cosmos DB eine End-to-End-Wartezeit bei Lesevorgängen von weniger als 10 ms und bei indizierten Schreibvorgängen von weniger als 15 ms im 99. Perzentil (innerhalb derselben Azure-Region). Die mittlere Wartezeit liegt deutlich niedriger (unter 5 ms).

* **Sicherstellen der AlwaysOn-Verfügbarkeit**
    * 99,99 % Verfügbarkeit innerhalb einer Region.
    * Nehmen Sie die Bereitstellung für höhere Verfügbarkeit in einer beliebigen Anzahl von [Azure-Regionen](https://azure.microsoft.com/regions) vor.
    * [Simulieren Sie den Ausfall](regional-failover.md) einer oder mehrerer Regionen garantiert ohne Datenverluste. 

* **Korrektes Schreiben global verteilter Anwendungen**
    * Fünf [Konsistenzmodelle](consistency-levels.md) bieten ein Spektrum an hoher SQL-ähnlicher Konsistenz bis hin zu NoSQL-ähnlicher letztlicher Konsistenz – und sämtliche Abstufungen dazwischen. 
  
* **Geld-zurück-Garantien**
    * Ihre Daten sind schnell verfügbar, oder Sie erhalten Ihr Geld zurück. 
    * [Vereinbarungen zum Servicelevel (Service Level Agreements, SLAs)](https://aka.ms/acdbsla) für Verfügbarkeit, Wartezeit, Durchsatz und Konsistenz. 

* **Keine Datenbankschema-/-indexverwaltung**
    * Hören Sie auf, sich Sorgen um die Synchronisierung Ihres Datenbankschemas und der Indizes mit dem Schema Ihrer Anwendung zu machen. Wir sind schemafrei. 
    * Das Datenbankmodul von Azure Cosmos DB ist vollständig schemaunabhängig. Alle erfassten Daten werden automatisch und ohne Schema oder Indizes erfasst, was extrem schnelle Abfragen ermöglicht. 

* **Niedrige Betriebskosten**
    * Fünf- bis zehnmal [kosteneffizienter](https://aka.ms/cosmos-db-tco-paper) als eine nicht verwaltete Lösung.
    * Dreimal günstiger als DynamoDB.

## <a name="capability-comparison"></a>Vergleich der Funktionen

Azure Cosmos DB kombiniert die besten Funktionen relationaler und nicht relationaler Datenbanken.

| Funktionen | Relationale Datenbanken   | Nicht relationale Datenbanken (NoSQL) |    Azure Cosmos DB |
| --- | --- | --- | --- |
| Globale Verteilung | Nein | Nein | Ja. Sofort einsatzbereite Verteilung in über 30 Regionen mit Multihosting-APIs|
| Horizontale Skalierung | Nein | Ja | Ja. Speicher und Durchsatz können unabhängig voneinander skaliert werden. | 
| Wartezeitgarantien | Nein | Ja | Ja. 99 Prozent der Lesevorgänge in unter 10 ms und Schreibvorgänge in unter 15 ms | 
| Hohe Verfügbarkeit | Nein | Ja | Ja. Cosmos DB ist immer aktiv (AlwaysOn), verfügt über PACELC-Kompromisse und bietet Optionen für automatisches und manuelles Failover.|
| Datenmodell + API | Relational + SQL | API mit mehreren Modellen + OSS-API | API mit mehreren Modellen + SQL-API + OSS-API (weitere demnächst verfügbar) |
| SLAs | Ja | Nein | Ja. Umfassende SLAs für Wartezeit, Durchsatz, Konsistenz, Verfügbarkeit |


## <a name="next-steps"></a>Nächste Schritte
Erste Schritte mit Azure Cosmos DB mit einem unserer Schnellstarts:

* [Erste Schritte mit der DocumentDB-API von Azure Cosmos DB](create-documentdb-dotnet.md)
* [Erste Schritte mit der MongoDB-API von Azure Cosmos DB](create-mongodb-nodejs.md)
* [Erste Schritte mit der Graph-API von Azure Cosmos DB](create-graph-dotnet.md)
* [Erste Schritte mit der Table-API von Azure Cosmos DB](create-table-dotnet.md)

