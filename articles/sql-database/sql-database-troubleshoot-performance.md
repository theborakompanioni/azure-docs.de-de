---
title: "Überwachen und Optimieren der Datenbankleistung in Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Tipps für die Leistungsoptimierung in Azure SQL-Datenbank durch Auswertung und Verbesserung."
services: sql-database
documentationcenter: 
author: v-shysun
manager: felixwu
editor: 
keywords: SQL-Leistungsoptimierung, Datenbankleistungsoptimierung, Tipps zur SQL-Leistungsoptimierung, Optimierung der SQL-Datenbankleistung
ms.assetid: eb7b3f66-3b33-4e1b-84fb-424a928a6672
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: v-shysun
ms.translationtype: Human Translation
ms.sourcegitcommit: fc27849f3309f8a780925e3ceec12f318971872c
ms.openlocfilehash: 7a2f1199a56e0bd32eafef9f420879c756673e7f
ms.contentlocale: de-de
ms.lasthandoff: 06/14/2017


---
# <a name="monitoring-and-performance-tuning"></a>Überwachen und Optimieren der Datenbankleistung

Azure SQL-Datenbank ist ein automatisch verwalteter und flexibler Datendienst, in dem Sie leicht die Nutzung überwachen, Ressourcen (CPU, Arbeitsspeicher, E/A) hinzufügen oder entfernen und Empfehlungen zur Leistungsverbesserung Ihrer Datenbank suchen können. Außerdem können Sie die Datenbank an Ihre Workload anpassen lassen und automatisch die Leistung optimieren.

Dieser Artikel bietet eine Übersicht über die Optionen zur Überwachung und Leistungsoptimierung, die in Azure SQL-Datenbank verfügbar sind.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="monitoring-and-troubleshooting-database-performance"></a>Überwachung und Problembehandlung der Datenbankleistung

Azure SQL-Datenbank ermöglicht Ihnen, die Nutzung Ihrer Datenbank einfach zu überwachen und Abfragen zu erkennen, die möglicherweise die Leistungsprobleme verursachen. Sie können die Datenbankleistung mithilfe des Azure-Portals oder Systemansichten überwachen. Es gibt folgende Optionen für die Überwachung und Problembehandlung der Datenbankleistung:

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **SQL-Datenbanken**, wählen Sie die Datenbank aus, und suchen Sie dann mithilfe des Diagramms „Überwachung“ nach Ressourcen, die sich ihrer maximalen Leistung annähern. „DTU-Verbrauch“ wird standardmäßig angezeigt. Klicken Sie auf **Bearbeiten** , um den Zeitraum und die angezeigten Werte zu ändern.
2. Verwenden Sie die [Statistik zur Abfrageleistung](sql-database-query-performance.md), um die Abfragen zu identifizieren, die die meisten Ressourcen verbrauchen.
3. Mithilfe dynamischer Verwaltungsansichten (DMVs), erweiterter Ereignisse (`XEvents`) und des Abfragespeichers in SSMS können Sie Leistungsparameter in Echtzeit abrufen.

Im [Leistungsleitfaden](sql-database-performance-guidance.md) finden Sie Methoden zur Leistungsverbesserung von Azure SQL-Datenbank, wenn Sie mithilfe dieser Berichte oder Ansichten Probleme feststellen.

> [!IMPORTANT] 
> Es wird empfohlen, immer die neueste Version von Management Studio zu verwenden, damit Sie mit Updates von Microsoft Azure und SQL-Datenbank synchron sind. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
>

## <a name="optimize-database-to-improve-performance"></a>Optimieren der Datenbank zur Verbesserung der Leistung

Mit Azure SQL-Datenbank können Sie Möglichkeiten zur Verbesserung und Optimierung der Abfrageleistung identifizieren, ohne dass Sie Ressourcen durch Überprüfen des [Datenbankratgebers](sql-database-advisor.md) ändern müssen. Fehlende Indizes und falsch optimierte Abfragen sind häufige Ursachen für eine schlechte Datenbankleistung. Sie können diese Empfehlungen zur Optimierung anwenden, um die Leistung Ihrer Workload zu verbessern.
Sie können die Leistung Ihrer Abfragen auch automatisch durch Azure SQL-Datenbank optimieren lassen, indem Sie alle erkannten Empfehlungen anwenden und bestätigen, dass diese die Datenbankleistung verbessern (weitere Informationen finden Sie unter [automatically optimize performance of your queries (Automatisches Optimieren der Leistung Ihrer Abfragen)](sql-database-automatic-tuning.md)). Sie können folgende Optionen verwenden, um die Leistung Ihrer Datenbank zu verbessern:

1. Verwenden Sie [SQL Database Advisor](sql-database-advisor-portal.md), um Empfehlungen zum Erstellen und Löschen von Indizes, zum Parametrisieren von Abfragen und zum Beheben von Schemaproblemen anzuzeigen.
2. [Aktivieren Sie die automatische Optimierung](sql-database-automatic-tuning-enable.md), und lassen Sie erkannte Leistungsprobleme automatisch durch Azure SQL-Datenbank verbessern.

## <a name="improving-database-performance-with-more-resources"></a>Verbesserung der Datenbankleistung mit mehr Ressourcen

Schließlich können Sie noch die Anzahl der in Azure SQL-Datenbank verfügbaren Ressourcen ändern, wenn es keine umsetzbaren Elemente gibt, die die Leistung Ihrer Datenbank verbessern können. Sie können mehr Ressourcen zuweisen, indem Sie die [Dienstebene](sql-database-service-tiers.md) einer eigenständigen Datenbank verändern oder die eDTUs eines elastischen Pools zu einer beliebigen Zeit erhöhen.
1. Für eigenständige Datenbanken können Sie die [Diensttarife bedarfsgesteuert ändern](sql-database-service-tiers.md), um die Datenbankleistung zu steigern.
2. Ziehen Sie bei mehreren Datenbanken [elastische Pools](sql-database-elastic-pool-guidance.md) in Betracht, um Ressourcen automatisch zu skalieren.

## <a name="tune-and-refactor-application-or-database-code"></a>Optimieren und Ändern von Anwendungs- oder Datenbankcode

Sie können Anwendungscode ändern, um effektiver die Datenbank zu nutzen, Indizes zu verändern, Pläne zu erzwingen oder Hinweise zu nutzen, um die Datenbank manuell an Ihre Workload anzupassen. Im [Leistungsleitfaden](sql-database-performance-guidance.md) finden Sie hilfreiche Informationen und Tipps zur Überwachung und Optimierung.


## <a name="next-steps"></a>Nächste Schritte

- Informationen zur Aktivierung der automatischen Optimierung in Azure SQL-Datenbank und zur Verwendung für die vollständige Verwaltung Ihrer Workload finden Sie unter [Aktivieren der automatischen Optimierung](sql-database-automatic-tuning-enable.md).
- Bei der Verwendung der manuellen Optimierung helfen Ihnen die Informationen unter [Azure SQL Database Advisor im Azure-Portal](sql-database-advisor-portal.md) weiter. Außerdem können Sie manuell die Empfehlungen anwenden, die zu einer Verbesserung der Leistung Ihrer Abfragen führen.
- Ändern Sie in der Datenbank verfügbare Ressourcen, indem Sie [Dienst- und Leistungsebenen für Azure SQL-Datenbanken](sql-database-performance-guidance.md) ändern.
