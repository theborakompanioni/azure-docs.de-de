<properties
   pageTitle="Beheben von Kompatibilitätsproblemen der SQL Server-Datenbank vor der Migration zu SQL-Datenbank | Microsoft Azure"
   description="Microsoft Azure SQL-Datenbank, Datenbankmigration, Kompatibilität, Migrations-Assistent für SQL Azure"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-migrate"
   ms.date="06/07/2016"
   ms.author="carlrab"/>

# Verwenden Sie den SQL Azure-Migrations-Assistenten, um vor der Migration zu Azure SQL-Datenbank Kompatibilitätsprobleme mit SQL Server-Datenbank zu beheben.

> [AZURE.SELECTOR]
- Mit dem [SQL Azure-Migrations-Assistenten](sql-database-cloud-migrate-fix-compatibility-issues.md)
- Mit [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- Mit [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)

In diesem Artikel erfahren Sie, wie Sie mit dem SQL Azure-Migrations-Assistenten vor der Migration zu Azure SQL-Datenbank Kompatibilitätsprobleme mit SQL Server-Datenbank ermitteln und beheben.

## Verwenden des SQL Azure-Migrations-Assistenten

Verwenden Sie den [SQL Azure-Migrations-Assistenten](http://sqlazuremw.codeplex.com/) für das CodePlex-Tool zum Generieren eines T-SQL-Skripts aus einer inkompatiblen Quelldatenbank verwenden, das vom Assistenten dann transformiert wird, um es mit der SQL-Datenbank kompatibel zu machen. Anschließend wird eine Verbindung mit Azure SQL-Datenbank hergestellt, um das Skript auszuführen. Mit diesem Tool werden außerdem Ablaufverfolgungsdateien analysiert, um Kompatibilitätsprobleme zu ermitteln. Das Skript kann mit der Option "Nur Schema" generiert werden oder Daten im BCP-Format enthalten. Weitere Dokumentation, einschließlich einer Schritt-für-Schritt-Anleitung, finden Sie bei CodePlex unter [SQL Azure-Migrations-Assistent](http://sqlazuremw.codeplex.com/).

 ![Diagramm der SAMW-Migration](./media/sql-database-cloud-migrate/02SAMWDiagram.png)

  > [AZURE.NOTE] Beachten Sie, dass nicht jedes inkompatible Schema, das vom Assistenten erkannt werden kann, durch dessen integrierte Transformationen behoben werden kann. Ein inkompatibles Skript, das nicht verarbeitet werden kann, wird als Fehler gemeldet, wobei Kommentare in das generierte Skript eingefügt werden. Falls viele Fehler erkannt werden, sollten Sie entweder Visual Studio oder SQL Server Management Studio nutzen, um die Fehler durchzugehen und zu beheben, die mit dem SQL Server-Migrations-Assistenten nicht behoben werden konnten.

## Nächste Schritte

- [Neueste Version von SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Migrieren einer kompatiblen SQL Server-Datenbank zur SQL-Datenbank](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## Weitere Ressourcen

- [SQL-Datenbank V12](sql-database-v12-whats-new.md)
- [Teilweise oder vollständig unterstützte Transact-SQL-Funktionen](sql-database-transact-sql-information.md)
- [Migrate non-SQL Server databases using SQL Server Migration Assistant (Migrieren von Nicht-SQL Server-Datenbanken mithilfe des SQL Server-Migrations-Assistenten)](http://blogs.msdn.com/b/ssma/)

<!---HONumber=AcomDC_0803_2016-->