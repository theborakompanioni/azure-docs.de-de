---
title: Einblicke in die Leistung der Azure SQL-Datenbank | Microsoft Docs
description: "Die Azure SQL-Datenbank bietet Leistungstools, mit denen Sie Bereiche identifizieren können, in denen die aktuelle Abfrageleistung verbessert werden kann."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: a60b75ac-cf27-4d73-8322-ee4d4c448aa2
ms.service: sql-database
ms.custom: monitor and tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/19/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 38416deeb583c78bca9ded2e514bf3ddff716c52


---
# <a name="sql-database-performance-insight"></a>Einblicke in die SQL-Datenbankleistung
Die Azure SQL-Datenbank bietet Leistungstools, mit denen Sie die Leistung Ihrer Datenbanken ermitteln und anhand von intelligenten Optimierungsaktionen und Empfehlungen verbessern können. 

1. Navigieren Sie im [Azure-Portal](http://portal.azure.com) zu Ihrer Datenbank, und klicken Sie auf **Alle Einstellungen** > **Leistung** > **Übersicht**, um die Seite **Leistung** zu öffnen. 
2. Klicken Sie auf **Empfehlungen**, um den [SQL-Datenbankratgeber](#sql-database-advisor) zu öffnen, und klicken Sie auf **Abfragen**, um [Query Performance Insight](#query-performance-insight) zu öffnen.
   
    ![Anzeigen der Leistung](./media/sql-database-performance/entries.png)

## <a name="performance-overview"></a>Leistungsübersicht
Durch Klicken auf **Übersicht** oder auf die Kachel **Leistung** gelangen Sie zum Leistungsdashboard für Ihre Datenbank. Diese Ansicht bietet eine Übersicht über die Leistung der Datenbank und unterstützt Sie bei der Leistungsoptimierung und Fehlerbehebung. 

![Leistung](./media/sql-database-performance/performance.png)

* Die Kachel **Empfehlungen** bietet eine Aufschlüsselung der Optimierungsempfehlungen für Ihre Datenbank. (Bei mehreren Empfehlungen werden die drei wichtigsten angezeigt.) Wenn Sie auf diese Kachel klicken, gelangen Sie zum **SQL-Datenbankratgeber**. 
* Die Kachel **Optimierungsaktivität** bietet eine Zusammenfassung der laufenden und abgeschlossenen Optimierungsaktionen für die Datenbank, sodass Sie einen schnellen Überblick über den Verlauf der Optimierungsaktivitäten erhalten. Wenn Sie auf diese Kachel klicken, gelangen Sie zur Ansicht des vollständigen Optimierungsverlaufs für Ihre Datenbank.
* Die Kachel **Automatische Optimierung** zeigt die Konfiguration der automatischen Optimierung für Ihre Datenbank (welche Optimierungsaktionen automatisch auf die Datenbank angewendet werden). Durch Klicken auf diese Kachel öffnen Sie das Dialogfeld zur Automatisierungskonfiguration.
* Die Kachel **Datenbankabfragen** zeigt die Zusammenfassung der Abfrageleistung für Ihre Datenbank (DTU-Nutzung insgesamt und die Abfragen mit dem größten Ressourcenverbrauch). Durch Klicken auf diese Kachel gelangen Sie zu **Query Performance Insight**.

## <a name="sql-database-advisor"></a>SQL-Datenbankratgeber
[SQL-Datenbankratgeber](sql-database-advisor.md) bietet intelligente Optimierungsempfehlungen, mit deren Hilfe Sie die Datenbankleistung verbessern können. 

* Empfehlungen dazu, welche Indizes erstellt oder gelöscht werden sollten (und eine Option zum automatischen Anwenden von Indexempfehlungen ohne Eingreifen des Benutzers sowie zum automatischen Rollback von Empfehlungen, die sich negativ auf die Leistung auswirken).
* Empfehlungen bei Schemaproblemen in der Datenbank.
* Empfehlungen, wenn Abfragen von parametrisierten Abfragen profitieren können.

## <a name="query-performance-insight"></a>Query Performance Insight
Mithilfe von [Query Performance Insight](sql-database-query-performance.md) können Sie den Zeitaufwand für die Problembehandlung der Datenbankleistung reduzieren. Dieses Tool stellt Folgendes bereit:

* Tiefere Einblicke in den Verbrauch von Datenbankressourcen (DTU). 
* Informationen zu Abfragen mit der höchsten CPU-Auslastung, für die unter Umständen eine Leistungssteigerung erzielt werden kann. 
* Die Möglichkeit zum Durchführen eines Drilldowns in die Details einer Abfrage.​ 

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Leitfaden zur Azure SQL-Datenbankleistung für Einzeldatenbanken](sql-database-performance-guidance.md)
* [Wo sollte ein Pool für elastische Datenbanken verwendet werden?](sql-database-elastic-pool-guidance.md)




<!--HONumber=Nov16_HO3-->


