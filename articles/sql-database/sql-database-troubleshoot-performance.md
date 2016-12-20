---
title: Tipps zur Optimierung der SQL-Datenbankleistung | Microsoft Azure
description: "Tipps für die Leistungsoptimierung in Azure SQL-Datenbank durch Auswertung und Verbesserung."
services: sql-database
documentationcenter: 
author: v-shysun
manager: felixwu
editor: 
keywords: SQL-Leistungsoptimierung, Datenbankleistungsoptimierung, Tipps zur SQL-Leistungsoptimierung, Optimierung der SQL-Datenbankleistung
ms.assetid: eb7b3f66-3b33-4e1b-84fb-424a928a6672
ms.service: sql-database
ms.custom: monitor and tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2016
ms.author: v-shysun
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: df8b72c18dfaf85de07d0cf575fad0a7ef1935bf


---
# <a name="sql-database-performance-tuning-tips"></a>Tipps zur Optimierung der SQL-Datenbankleistung
Sie können jederzeit den [Tarif](sql-database-service-tiers.md) einer Einzeldatenbank ändern oder die Anzahl der eDTUs eines Pools für elastische Datenbanken erhöhen, um die Leistung zu verbessern. Doch zuvor sollten Sie zunächst Möglichkeiten zum Verbessern und Optimieren der Abfrageleistung bestimmen. Fehlende Indizes und falsch optimierte Abfragen sind häufige Ursachen für eine schlechte Datenbankleistung. Dieser Artikel enthält Richtlinien für die Leistungsoptimierung in SQL-Datenbank.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="steps-to-evaluate-and-tune-database-performance"></a>Schritte zum Bewerten und Optimieren der Datenbankleistung
1. Klicken Sie im [Azure-Portal](https://portal.azure.com)auf **SQL-Datenbanken**, wählen Sie die Datenbank aus, und suchen Sie dann mithilfe des Diagramms „Überwachung“ Ressourcen, die sich ihrem Maximum annähern. „DTU-Verbrauch“ wird standardmäßig angezeigt. Klicken Sie auf **Bearbeiten** , um den Zeitraum und die angezeigten Werte zu ändern.
2. Verwenden Sie [Query Performance Insight](sql-database-query-performance.md) zum Auswerten der Abfragen mittels DTUs und dann den [SQL-Datenbankratgeber](sql-database-advisor.md), um Empfehlungen zum Erstellen und Löschen von Indizes, zum Parametrisieren von Abfragen und zum Beheben von Schemaproblemen anzuzeigen.
3. Mithilfe von dynamischen Verwaltungsansichten (DMVs), erweiterten Ereignissen (Xevents) und des Abfragespeichers in SSMS können Sie Leistungsparameter in Echtzeit abrufen. Im [Leistungsleitfaden](sql-database-performance-guidance.md) finden Sie detaillierte Tipps zur Überwachung und Optimierung.

    > [AZURE.IMPORTANT] Es wird empfohlen, immer die neueste Version von Management Studio zu verwenden, damit Sie mit Updates von Microsoft Azure und SQL-Datenbank synchron sind. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="steps-to-improve-database-performance-with-more-resources"></a>Schritte zur Verbesserung der Datenbankleistung mit mehr Ressourcen
1. Für Einzeldatenbanken können Sie die [Diensttarife bedarfsgesteuert ändern](sql-database-scale-up.md) , um die Datenbankleistung zu steigern.
2. Ziehen Sie bei mehreren Datenbanken [Pool für elastische Datenbanken](sql-database-elastic-pool-guidance.md) in Betracht, um Ressourcen automatisch zu skalieren.




<!--HONumber=Nov16_HO3-->


