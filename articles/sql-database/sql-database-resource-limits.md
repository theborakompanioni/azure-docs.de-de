---
title: "Ressourceneinschränkungen für Azure SQL-Datenbanken | Microsoft Docs"
description: "Diese Seite beschreibt einige allgemeine Ressourceneinschränkungen für Azure SQL-Datenbanken."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 884e519f-23bb-4b73-a718-00658629646a
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 12/06/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: b5306d864593898246d0e7ba81ed3f93253fab74
ms.openlocfilehash: 81746a6f893eab4fe2b951fb0d52bd1645d2a56c


---
# <a name="azure-sql-database-resource-limits"></a>Ressourceneinschränkungen für Azure SQL-Datenbank
## <a name="overview"></a>Übersicht
Azure SQL-Datenbank verwaltet die für eine Datenbank verfügbaren Ressourcen mithilfe zweier verschiedener Mechanismen: **Ressourcenkontrolle** und **Durchsetzung von Grenzen**. In diesem Thema werden diese beiden Hauptbereiche der Ressourcenverwaltung behandelt.

## <a name="resource-governance"></a>Ressourcenkontrolle
Eines der Entwurfsziele der Tarife Basic, Standard und Premium ist es, dass sich die Azure SQL-Datenbank so verhält, als ob sie auf einem eigenen Computer vollständig isoliert von anderen Datenbanken ausgeführt wird. Die Ressourcenkontrolle emuliert dieses Verhalten. Wenn die aggregierte Ressourcenverwendung die maximal verfügbaren Ressourcen an CPU, Arbeitsspeicher, Protokoll-E/A und Daten-E/A erreicht, die der Datenbank zugeordnet sind, reiht die Ressourcenkontrolle die Ausführung von Abfragen in eine Warteschlange ein und ordnet den Abfragen in der Warteschlange Ressourcen zu, während sie nach und nach freigegeben werden.

Wie bei einem dedizierten Computer führt das Nutzen aller verfügbarer Ressourcen zu einer längeren Ausführung der derzeit ausgeführten Abfragen, was Befehlstimeouts auf dem Client zur Folge haben kann. Anwendungen mit aggressiver Wiederholungslogik und solche, die Abfragen für die Datenbank mit hoher Frequenz ausführen, können bei dem Versuch auf Fehlermeldungen stoßen, neue Abfragen auszuführen, wenn das Limit gleichzeitiger Anforderungen bereits erreicht wurde.

### <a name="recommendations"></a>Empfehlungen:
Überwachen Sie die Ressourcenverwendung sowie die durchschnittlichen Antwortzeiten für Abfragen, wenn die maximale Auslastung einer Datenbank fast erreicht wurde. Falls längere Abfragewartezeiten auftreten, haben Sie in der Regel drei Optionen:

1. Reduzieren Sie die Menge der eingehenden Anfragen in der Datenbank, um einen Timeout und das Anhäufen von Anfragen zu verhindern.
2. Weisen Sie der Datenbank eine höhere Leistungsstufe zu.
3. Optimieren Sie Abfragen, um die Ressourcenverwendung für jede Abfrage zu reduzieren. Weitere Informationen finden Sie im Abschnitt "Abfrageoptimierung/Abfragehinweise" im Artikel "Leitfaden zur Azure SQL-Datenbankleistung".

## <a name="enforcement-of-limits"></a>Durchsetzung von Grenzen
Durch das Verweigern neuer Anforderungen bei Erreichen der Limits werden andere Ressourcen als CPU, Arbeitsspeicher, Protokoll-E/A und Daten-E/A durchgesetzt. Clients erhalten abhängig von der erreichten Grenze eine [Fehlermeldung](sql-database-develop-error-messages.md) .

Z. B. wird die Anzahl der Verbindungen mit einer SQL-Datenbank sowie die Anzahl der gleichzeitigen Anforderungen, die verarbeitet werden können, beschränkt. Mit einer SQL-Datenbank kann die Anzahl der Verbindungen mit der Datenbank größer als die Anzahl der gleichzeitigen Anforderungen sein, um Verbindungspooling zu unterstützen. Während die Anzahl der verfügbaren Verbindungen einfach von der Anwendung gesteuert werden kann, ist die Anzahl paralleler Anforderungen oft schwieriger zu schätzen und zu steuern. Insbesondere bei Spitzenbelastungen können Fehler auftreten, wenn die Anwendung entweder zu viele Anforderungen sendet oder die Datenbank seine Ressourcengrenzen erreicht und anfängt, Workerthreads aufgrund einer längeren Ausführungszeit von Abfragen anzuhäufen.

## <a name="service-tiers-and-performance-levels"></a>Tarife und Leistungsebenen
Es gibt Tarife und Leistungsstufen für sowohl eigenständige Datenbanken als auch elastische Pools.

### <a name="standalone-databases"></a>Eigenständige Datenbanken
Bei eigenständigen Datenbanken werden die Einschränkungen vom Tarif und der Leistungsstufe der Datenbank bestimmt. In der folgenden Tabelle sind die Merkmale von Basic-, Standard- und Premium-Datenbanken für unterschiedliche Leistungsstufen beschrieben.

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

### <a name="elastic-pools"></a>Elastische Pools
[Elastische Pools](sql-database-elastic-pool.md) nutzen Ressourcen in verschiedenen Datenbanken im Pool gemeinsam. In der folgenden Tabelle sind die Merkmale von elastischen Basic-, Standard- und Premium-Datenbankpools beschrieben.

[!INCLUDE [SQL DB service tiers table for elastic databases](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Eine erweiterte Definition der einzelnen Ressourcen, die in den vorangehenden Tabellen aufgeführt werden, können Sie der Beschreibungen in [Funktionen und Grenzen der Serviceebenen](sql-database-performance-guidance.md#service-tier-capabilities-and-limits)entnehmen. Eine Übersicht über die Tarife finden Sie unter [Tarife und Leistungsstufen für Azure SQL-Datenbank](sql-database-service-tiers.md).

## <a name="other-sql-database-limits"></a>Weitere Einschränkungen für SQL-Datenbanken
| Bereich | Begrenzung | Beschreibung |
| --- | --- | --- |
| Datenbanken mit automatisiertem Export pro Abonnement |10 |Automatisierter Export ermöglicht es Ihnen, einen benutzerdefinierten Zeitplan für die Sicherung Ihrer SQL-Datenbanken zu erstellen. Die Vorschau für dieses Feature wird am 1. März 2017 beendet.  |
| Datenbanken pro Server |Bis zu 5.000 |Bis zu 5.000 Datenbanken pro Server sind auf V12-Servern zulässig. |
| DTUs pro Server. |45000 |45.000 DTUs pro Server sind auf V12-Servern für die Bereitstellung von Datenbanken, elastischen Pools und Data Warehouses verfügbar. |

## <a name="resources"></a>Ressourcen
[Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md)

[Dienst- und Leistungsebenen für Azure SQL-Datenbanken](sql-database-service-tiers.md)

[Fehlermeldungen für Clientprogramme der SQL-Datenbank](sql-database-develop-error-messages.md)




<!--HONumber=Nov16_HO3-->


