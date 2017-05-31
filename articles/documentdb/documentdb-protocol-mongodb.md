---
title: "Einführung in Azure Cosmos DB: API für MongoDB | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Azure Cosmos DB verwenden können, um riesige Mengen von JSON-Dokumenten mit geringer Latenzzeit mithilfe der gängigen OSS MongoDB-APIs zu speichern und abzufragen."
keywords: what is MongoDB
services: cosmosdb
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 4afaf40d-c560-42e0-83b4-a64d94671f0a
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/05/2017
ms.author: anhoh
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 7dc474f9f940132dd363740e8288a70ee2f2b970
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="introduction-to-azure-cosmos-db-api-for-mongodb"></a>Einführung in Azure Cosmos DB: API für MongoDB

[Azure Cosmos DB](../cosmos-db/introduction.md) ist ein global verteilter Datenbankdienst von Microsoft mit mehreren Modellen für unternehmenskritische Anwendungen. Azure Cosmos DB bietet [sofort einsetzbare globale Verteilung](../documentdb/documentdb-distribute-data-globally.md), [flexible Skalierung von Durchsatz und Speicher](../documentdb/documentdb-partition-data.md) weltweit, Latenzzeiten im einstelligen Millisekundenbereich beim 99. Perzentil, [fünf wohldefinierte Konsistenzebenen](../documentdb/documentdb-consistency-levels.md) sowie garantierte hohe Verfügbarkeit, gestützt durch [branchenführende Vereinbarungen zum Servicelevel](https://azure.microsoft.com/support/legal/sla/documentdb/v1_1/) (SLAs, Service Level Agreements). Azure Cosmos DB [indiziert automatisch Daten](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf), sodass Sie sich nicht mit der Schema- und Indexverwaltung befassen müssen. Es unterstützt mehrere Datenmodelle – Dokumente, Schlüsselwerte, Diagramme und spaltenorientierte Datenmodelle. 

![Azure-API für MongoDB](./media/documentdb-protocol-mongodb/cosmosdb-mongodb.png) 

DocumentDB-Datenbanken können als Datenspeicher für Apps verwendet werden, die für [MongoDB](https://docs.mongodb.com/manual/introduction/) geschrieben wurden. Dies bedeutet, dass Ihre für MongoDB geschriebene Anwendung durch Nutzung vorhandener [Treiber](https://docs.mongodb.org/ecosystem/drivers/) jetzt mit Cosmos DB kommunizieren und DocumentDB-Datenbanken statt MongoDB-Datenbanken verwenden kann. In vielen Fällen können Sie von der Verwendung von MongoDB zu DocumentDB wechseln, indem Sie einfach eine Verbindungszeichenfolge ändern. Mit dieser Funktion können Sie ganz einfach MongoDB-Datenbankanwendungen in der Azure-Cloud mit der globalen Verteilung von Azure Cosmos DB sowie mit [umfassenden branchenführenden SLAs](https://azure.microsoft.com/support/legal/sla/documentdb/v1_1/) erstellen und ausführen, während Sie die vorhandenen Kenntnisse und Tools für MongoDB weiterhin verwenden.


## <a name="what-is-the-benefit-of-using-azure-cosmos-db-for-mongodb-applications"></a>Vorteil der Verwendung von Azure Cosmos DB für MongoDB-Anwendungen

* **Flexibel skalierbarer Durchsatz und Speicher**: Die MongoDB-Datenbank lässt sich problemlos zentral hoch- und herunterskalieren, um Ihren Anwendungsanforderungen gerecht zu werden. Ihre Daten werden für niedrige vorhersagbare Latenzen auf Solid-State-Datenträgern (SSDs) gespeichert. Cosmos DB unterstützt MongoDB-Sammlungen, die auf eine nahezu unbegrenzte Speichergröße und einen nahezu unbegrenzten bereitgestellten Durchsatz skaliert werden können. Sie können Cosmos DB mit vorhersagbarer Leistung flexibel skalieren, wenn Ihre Anwendung wächst. 

* **Replikation in mehreren Regionen**: Cosmos DB repliziert Ihre Daten auf transparente Weise in allen Regionen, die Sie Ihrem MongoDB-Konto zugeordnet haben. Sie können Anwendungen entwickeln, die globalen Zugriff auf Daten benötigen, und dabei für ein ausgewogenes Maß an Konsistenz, Verfügbarkeit und Leistung sorgen – jeweils mit den entsprechenden Garantien. Cosmos DB bietet transparentes regionales Failover mit Multihosting-APIs sowie die Möglichkeit, Durchsatz und Speicher für alle Konten weltweit flexibel zu skalieren. Weitere Informationen finden Sie unter [Globale Verteilung von Daten](documentdb-distribute-data-globally.md).

**MongoDB-Kompatibilität**: Sie können Ihre vorhandene MongoDB-Kenntnisse, Anwendungscodes und Tools verwenden. Sie können Anwendungen mithilfe von MongoDB entwickeln, und sie mithilfe des vollständig verwalteten und global verteilten Cosmos DB-Diensts für die Produktion bereitstellen.

**Keine Serververwaltung**: Sie müssen Ihre MongoDB-Datenbanken nicht verwalten und skalieren. Cosmos DB ist ein vollständig verwalteter Dienst, d. h. Sie müssen selbst keine Infrastruktur oder virtuellen Computer verwalten. Cosmos DB ist in mehr als 30 [Azure-Regionen](https://azure.microsoft.com/regions/services/) verfügbar.

* **Einstellbare Konsistenzebenen:** Die Konsistenz kann über fünf klar definierte Ebenen abgestimmt werden, um für ein ausgewogenes Verhältnis zwischen Konsistenz und Leistung zu sorgen. Für Abfragen und Lesevorgänge bietet Cosmos DB fünf verschiedene Konsistenzebenen – „stark“, „begrenzte Veraltung“, „Sitzung“, „Präfixkonsistenz“ und „letztlich“. Mit diesen granularen, gut abgegrenzten Konsistenzebenen können fundierte Kompromisse zwischen Konsistenz, Verfügbarkeit und Latenz geschlossen werden. Weitere Informationen finden Sie unter [Verwenden von Konsistenzebenen zum Maximieren der Verfügbarkeit und Leistung](documentdb-consistency-levels.md).

* **Automatische Indizierung**: Alle Eigenschaften in Dokumenten in Ihrer MongoDB-Datenbank werden von Cosmos DB automatisch indiziert, ohne dass ein Schema oder die Erstellung sekundärer Indizes erwartet oder gefordert wird.

**Unternehmensniveau**: Azure Cosmos DB unterstützt mehrere lokale Replikate, um eine Verfügbarkeit von 99,99 % sowie Datenschutz bei lokalen und regionalen Ausfällen bereitzustellen. Azure Cosmos DB verfügt über [Kompatibilitätszertifizierungen](https://www.microsoft.com/trustcenter) und Sicherheitsfunktionen auf Unternehmensniveau. 

In diesem Azure Friday-Video mit Scott Hanselman und Kirill Gavrylyuk (Azure Cosmos DB Principal Engineering Manager) erhalten Sie weitere Informationen.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/DocumentDB-Database-as-a-Service-for-MongoDB-Developers/player]
> 

## <a name="how-to-get-started"></a>Erste Schritte

Erstellen Sie im [Azure-Portal](https://portal.azure.com) ein Azure Cosmos DB-Konto, und tauschen Sie die MongoDB-Verbindungszeichenfolge mit Ihrem neuen Konto. 

*Das ist auch schon alles!*

Detailliertere Anweisungen finden Sie unter [Erstellen eines Kontos](documentdb-create-mongodb-account.md) und [Herstellen einer Verbindung mit Ihrem Konto](documentdb-connect-mongodb-account.md).

## <a name="next-steps"></a>Nächste Schritte

Informationen zur MongoDB-API für Azure Cosmos DB sind in die Azure Cosmos DB-Dokumentation integriert. Im Folgenden finden Sie einige Verweise für den Einstieg:

* Im Tutorial [Verbinden mit einem MongoDB-Konto](documentdb-connect-mongodb-account.md) erfahren Sie, wie Sie die Information zur Verbindungszeichenfolge für Ihr Konto abrufen.
* Im Tutorial [Verwenden von MongoChef mit Azure Cosmos DB](documentdb-mongodb-mongochef.md) erfahren Sie, wie Sie eine Verbindung zwischen Ihrer Azure Cosmos DB-Datenbank und einer MongoDB-App in MongoChef erstellen.
* Arbeiten Sie das Tutorial [Migrieren von Daten zu Azure Cosmos DB mit Protokollunterstützung für MongoDB](documentdb-mongodb-migrate.md) durch, um Ihre Daten in eine API für MongoDB-Datenbank zu importieren.
* Erstellen Sie Ihre erste API für MongoDB-App mithilfe von [Node.js](documentdb-mongodb-samples.md).
* Erstellen Sie Ihre erste API für MongoDB-Web-App mithilfe von [.NET](documentdb-mongodb-application.md).
* Stellen Sie mithilfe von [Robomongo](documentdb-mongodb-robomongo.md) eine Verbindung mit einem API für MongoDB-Konto her.
* Finden Sie mit dem [GetLastRequestStatistics-Befehl und den Metriken im Azure-Portal](documentdb-request-units.md#GetLastRequestStatistics) heraus, wie viele RUs von Ihren Vorgängen verwendet werden.
* Erfahren Sie, wie Sie [Lesevoreinstellungen für global verteilte Apps konfigurieren](../cosmos-db/tutorial-global-distribution-mongodb.md).

