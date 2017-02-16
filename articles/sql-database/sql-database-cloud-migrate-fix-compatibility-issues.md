---
title: "SAMW: Beheben von Kompatibilitätsproblemen bei der Migration zu Azure SQL-Datenbank | Microsoft Docs"
description: "In diesem Artikel lernen Sie Möglichkeiten kennen, wie Sie mit dem SQL Azure-Migrations-Assistenten vor der Migration zu Azure SQL-Datenbank Kompatibilitätsprobleme mit SQL Server-Datenbank ermitteln und beheben."
keywords: "Microsoft Azure SQL-Datenbank, Datenbankmigration, Kompatibilität, Migrations-Assistent für SQL Azure"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: bba0893a-3247-406c-b155-acb4b3ef479c
ms.service: sql-database
ms.custom: migrate and move
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: sqldb-migrate
ms.date: 08/24/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 2b55b6b4475abdbc1985d8ac370b3b612b77eb0e
ms.openlocfilehash: 006d2d5cd24bc42655bf0b3c0325531144743524


---
# <a name="use-sql-azure-migration-wizard-to-fix-sql-server-database-compatibility-issues-before-migration-to-azure-sql-database"></a>Verwenden Sie den SQL Azure-Migrations-Assistenten, um vor der Migration zu Azure SQL-Datenbank Kompatibilitätsprobleme mit SQL Server-Datenbank zu beheben.
> [!div class="op_single_selector"]
> * Mit [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)
> * Mit [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
> * Mit [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
>  

In diesem Artikel erfahren Sie, wie Sie mit dem SQL Azure-Migrations-Assistenten vor der Migration zu Azure SQL-Datenbank Kompatibilitätsprobleme mit SQL Server-Datenbank ermitteln und beheben.

## <a name="using-sql-azure-migration-wizard"></a>Verwenden des SQL Azure-Migrations-Assistenten
Verwenden Sie das CodePlex-Tool des [Azure SQL-Datenbankmigrations-Assistenten](http://sqlazuremw.codeplex.com/) zum Generieren eines T-SQL-Skripts aus einer nicht kompatiblen Quelldatenbank. Dieses Skript wird dann vom Assistenten transformiert, sodass es mit der SQL-Datenbank kompatibel ist. Sie stellen dann eine Verbindung mit Azure SQL-Datenbank her, um das Skript auszuführen. Dieses Tool analysiert auch Ablaufverfolgungsdateien, um Kompatibilitätsprobleme zu ermitteln. Das Skript kann mit der Option "Nur Schema" generiert werden oder Daten im BCP-Format enthalten. Weitere Dokumentation, einschließlich einer Schritt-für-Schritt-Anleitung, finden Sie bei CodePlex unter [SQL Azure-Migrations-Assistent](http://sqlazuremw.codeplex.com/).  

 ![Diagramm der SAMW-Migration](./media/sql-database-cloud-migrate/02SAMWDiagram.png)

> [!NOTE]
> Nicht jedes inkompatible Schema, das vom Assistenten erkannt wird, kann durch dessen integrierte Transformationen behoben werden. Ein inkompatibles Skript, das nicht verarbeitet werden kann, wird als Fehler gemeldet, wobei Kommentare in das generierte Skript eingefügt werden. Falls viele Fehler erkannt werden, sollten Sie entweder Visual Studio oder SQL Server Management Studio nutzen, um die Fehler durchzugehen und zu beheben, die mit dem SQL Server-Migrations-Assistenten nicht behoben werden konnten.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
* [Neueste Version von SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
* [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
* [Migrieren einer kompatiblen SQL Server-Datenbank zur SQL-Datenbank](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Azure SQL Database features (Features der Azure SQL-Datenbank)](sql-database-features.md)
* [Teilweise oder vollständig unterstützte Transact-SQL-Funktionen](sql-database-transact-sql-information.md)
* [Migrate non-SQL Server databases using SQL Server Migration Assistant (Migrieren von Nicht-SQL Server-Datenbanken mithilfe des SQL Server-Migrations-Assistenten)](http://blogs.msdn.com/b/ssma/)




<!--HONumber=Jan17_HO1-->


