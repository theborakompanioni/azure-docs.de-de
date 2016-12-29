---
title: Migrieren zu SQL-Datenbank per Transaktionsreplikation | Microsoft Docs
description: Microsoft Azure SQL-Datenbank, Datenbankmigration, Datenbank importieren, Transaktionsreplikation
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: eebdd725-833d-4151-9b2b-a0303f39e30f
ms.service: sql-database
ms.custom: migrate and move
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: sqldb-migrate
ms.date: 11/08/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: e8bb9e5a02a7caf95dae0101c720abac1c2deff3
ms.openlocfilehash: 891c10b2f8e560a3c97f93198c742f657a92e927


---
# <a name="migrate-sql-server-database-to-azure-sql-database-using-transactional-replication"></a>Migrieren von SQL Server-Datenbank zu Azure SQL-Datenbank per Transaktionsreplikation
> [!div class="op_single_selector"]
> * [SSMS-Migrations-Assistent](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
> * [Exportieren in eine BACPAC-Datei](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
> * [Importieren aus einer BACPAC-Datei](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
> * [Transaktionsreplikation](sql-database-cloud-migrate-compatible-using-transactional-replication.md)
> 
> 

In diesem Artikel erfahren Sie, wie Sie eine kompatible SQL Server-Datenbank mithilfe der Transaktionsreplikation von SQL Server mit nur minimalen Ausfallzeiten zu Azure SQL-Datenbank migrieren.

## <a name="understanding-the-transactional-replication-architecture"></a>Verstehen der Architektur der Transaktionsreplikation
Wenn Sie es sich nicht leisten können, Ihre SQL Server-Datenbank während der Migration aus der Produktion herauszunehmen, können Sie die SQL Server-Transaktionsreplikation als Migrationslösung verwenden. Um diese Lösung zu verwenden, können Sie Ihre Azure SQL-Datenbank-Instanz als Abonnenten der lokalen SQL Server-Instanz konfigurieren, die Sie migrieren möchten. Der Verteiler der lokalen Transaktionsreplikation synchronisiert die Daten aus der zu synchronisierenden lokalen Datenbank (dem Herausgeber), während weiterhin neue Transaktionen ausgeführt werden. 

Sie können die Transaktionsreplikation auch zum Migrieren einer Teilmenge Ihrer lokalen Datenbank verwenden. Die Veröffentlichung, die Sie zu Azure SQL-Datenbank replizieren, kann auf eine Teilmenge der Tabellen in der replizierten Datenbank beschränkt werden. Für jede replizierte Tabelle können Sie die Daten auf eine Teilmenge der Zeilen bzw. eine Teilmenge der Spalten beschränken.

Bei der Transaktionsreplikation werden alle Änderungen an Daten oder Schema in Ihrer Azure SQL-Datenbank angezeigt. Wenn die Synchronisierung abgeschlossen ist, und Sie zur Migration bereit sind, ändern Sie die Verbindungszeichenfolgen Ihrer Anwendungen so, dass sie auf Ihre Azure SQL-Datenbank verweisen. Sobald mit der Transaktionsreplikation alle verbleibenden Änderungen Ihrer lokalen Datenbank abgearbeitet wurden, und alle Anwendungen auf Azure DB zeigen, können Sie die Transaktionsreplikation deinstallieren. Ihre Azure SQL-Datenbank ist jetzt Ihr Produktionssystem.

 ![SeedCloudTR-Diagramm](./media/sql-database-cloud-migrate/SeedCloudTR.png)

## <a name="transactional-replication-requirements"></a>Anforderungen der Transaktionsreplikation
Die Transaktionsreplikation ist eine Technologie, die seit SQL Server 6.5 in SQL Server integriert ist. Es handelt sich hierbei um eine ausgereifte und bewährte Technologie, mit der die meisten Datenbankadministratoren über Erfahrung verfügen. Mit [SQL Server 2016](https://www.microsoft.com/sql-server/sql-server-2016) kann Azure SQL-Datenbank als [Transaktionsreplikationsabonnent](https://msdn.microsoft.com/library/mt589530.aspx) für die lokale Veröffentlichung konfiguriert werden. Wenn Sie die Einrichtung über Management Studio durchführen, erhalten Sie das gleiche Ergebnis wie beim Einrichten eines Abonnenten der Transaktionsreplikation auf einem lokalen Server. Der Support für dieses Szenario ist gegeben, wenn der Herausgeber und der Verteiler mindestens über eine der folgenden SQL Server-Versionen verfügen:

* SQL Server 2016 und höher 
* SQL Server 2014 SP1 CU3 und höher
* SQL Server 2014 RTM CU10 und höher
* SQL Server 2012 SP2 CU8 und höher
* SQL Server 2012 SP3 und höher

> [!IMPORTANT]
> Verwenden Sie die neueste Version von SQL Server Management Studio, um immer den Updates von Microsoft Azure und SQL-Datenbank zu entsprechen. Ältere Versionen von SQL Server Management Studio können SQL-Datenbank nicht als Abonnenten einrichten. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

## <a name="next-steps"></a>Nächste Schritte
* [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
* [Neueste Version von SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
* [SQL Server 2016](https://www.microsoft.com/sql-server/sql-server-2016)

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Transaktionsreplikation](https://msdn.microsoft.com/library/mt589530.aspx)
* [Azure SQL Database features (Features der Azure SQL-Datenbank)](sql-database-features.md)
* [Teilweise oder vollständig unterstützte Transact-SQL-Funktionen](sql-database-transact-sql-information.md)
* [Migrate non-SQL Server databases using SQL Server Migration Assistant (Migrieren von Nicht-SQL Server-Datenbanken mithilfe des SQL Server-Migrations-Assistenten)](http://blogs.msdn.com/b/ssma/)




<!--HONumber=Nov16_HO4-->


