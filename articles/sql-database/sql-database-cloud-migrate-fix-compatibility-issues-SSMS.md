<properties
   pageTitle="Beheben von Kompatibilitätsproblemen der SQL Server-Datenbank vor der Migration zu SQL-Datenbank"
   description="Microsoft Azure SQL-Datenbank, Datenbankmigration, Kompatibilität, Migrations-Assistent für SQL Azure"
   services="sql-database"
   documentationCenter=""
   authors="carlrabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="05/31/2016"
   ms.author="carlrab"/>

# Beheben von Kompatibilitätsproblemen der SQL Server-Datenbank mit SQL Server Management Studio vor der Migration zu einer SQL-Datenbank

> [AZURE.SELECTOR]
- Mit dem [SQL Azure-Migrations-Assistenten](sql-database-cloud-migrate-fix-compatibility-issues.md)
- Mit [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- Mit [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)

Fortgeschrittene Benutzer können Kompatibilitätsprobleme von SQL Server-Datenbank mithilfe von SQL Server Management Studio vor der Migration zu Azure SQL-Datenbank beheben.

## Verwenden von SQL Server Management Studio

Verwenden Sie SQL Server Management Studio zum Beheben von Komptatibilitätsproblemen mithilfe von verschiedenen Transact-SQL-Befehlen, z. B. **ALTER DATABASE**. Diese Methode richtet sich primär an fortgeschrittene Benutzer, die mit der Arbeit mit Transact-SQL in einer Livedatenbank vertraut sind. Andernfalls empfiehlt sich die Verwendung von SSDT.



## Nächste Schritte

- [Neueste Version von SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Migrieren einer kompatiblen SQL Server-Datenbank zur SQL-Datenbank](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## Weitere Informationen

- [SQL-Datenbank V12](sql-database-v12-whats-new.md)
- [Teilweise oder vollständig unterstützte Transact-SQL-Funktionen](sql-database-transact-sql-information.md)
- [Migrate non-SQL Server databases using SQL Server Migration Assistant](http://blogs.msdn.com/b/ssma/) (Migrieren von Nicht-SQL Server-Datenbanken mithilfe des SQL Server-Migrations-Assistenten)

<!---HONumber=AcomDC_0601_2016-->