<properties
   pageTitle="Migrieren zur SQL-Datenbank per Transaktionsreplikation | Microsoft Azure"
   description="Microsoft Azure SQL-Datenbank, Datenbankmigration, Datenbank importieren, Transaktionsreplikation"
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
   ms.date="08/23/2016"
   ms.author="carlrab"/>

# Migrieren von SQL Server-Datenbank zu Azure SQL-Datenbank per Transaktionsreplikation

> [AZURE.SELECTOR]
- [SSMS-Migrations-Assistent](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [Exportieren in eine BACPAC-Datei](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Importieren aus einer BACPAC-Datei](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Transaktionsreplikation](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

In diesem Artikel erfahren Sie, wie Sie eine kompatible SQL Server-Datenbank mithilfe der Transaktionsreplikation von SQL Server mit nur minimalen Ausfallzeiten zu Azure SQL-Datenbank migrieren.

## Verstehen der Architektur der Transaktionsreplikation

Wenn Sie es sich nicht leisten können, Ihre SQL Server-Datenbank während der Migration aus der Produktion herauszunehmen, können Sie die SQL Server-Transaktionsreplikation als Migrationslösung verwenden. Um diese Lösung zu verwenden, können Sie Ihre Azure SQL-Datenbank-Instanz als Abonnenten der lokalen SQL Server-Instanz konfigurieren, die Sie migrieren möchten. Der Verteiler der lokalen Transaktionsreplikation synchronisiert die Daten aus der zu synchronisierenden lokalen Datenbank (dem Herausgeber), während weiterhin neue Transaktionen ausgeführt werden.

Sie können die Transaktionsreplikation auch zum Migrieren einer Teilmenge Ihrer lokalen Datenbank verwenden. Die Veröffentlichung, die Sie zu Azure SQL-Datenbank replizieren, kann auf eine Teilmenge der Tabellen in der replizierten Datenbank beschränkt werden. Für jede replizierte Tabelle können Sie die Daten auf eine Teilmenge der Zeilen bzw. eine Teilmenge der Spalten beschränken.

Bei der Transaktionsreplikation werden alle Änderungen an Daten oder Schema in Ihrer Azure SQL-Datenbank angezeigt. Wenn die Synchronisierung abgeschlossen ist, und Sie zur Migration bereit sind, ändern Sie die Verbindungszeichenfolgen Ihrer Anwendungen so, dass sie auf Ihre Azure SQL-Datenbank verweisen. Sobald mit der Transaktionsreplikation alle verbleibenden Änderungen Ihrer lokalen Datenbank abgearbeitet wurden, und alle Anwendungen auf Azure DB zeigen, können Sie die Transaktionsreplikation deinstallieren. Ihre Azure SQL-Datenbank ist jetzt Ihr Produktionssystem.

 ![SeedCloudTR-Diagramm](./media/sql-database-cloud-migrate/SeedCloudTR.png)

## Anforderungen der Transaktionsreplikation

Die Transaktionsreplikation ist eine Technologie, die seit SQL Server 6.5 in SQL Server integriert ist. Es handelt sich hierbei um eine ausgereifte und bewährte Technologie, mit der die meisten Datenbankadministratoren über Erfahrung verfügen. Mit [SQL Server 2016](https://www.microsoft.com/de-DE/cloud-platform/sql-server) kann die Azure SQL-Datenbank als [Transaktionsreplikationsabonnent](https://msdn.microsoft.com/library/mt589530.aspx) für die lokale Veröffentlichung konfiguriert werden. Wenn Sie die Einrichtung über Management Studio durchführen, erhalten Sie das gleiche Ergebnis wie beim Einrichten eines Abonnenten der Transaktionsreplikation auf einem lokalen Server. Der Support für dieses Szenario ist gegeben, wenn der Herausgeber und der Verteiler mindestens über eine der folgenden SQL Server-Versionen verfügen:

 - SQL Server 2016 und höher
 - SQL Server 2014 SP1 CU3 und höher
 - SQL Server 2014 RTM CU10 und höher
 - SQL Server 2012 SP2 CU8 und höher
 - SQL Server 2012 SP3 und höher


> [AZURE.IMPORTANT] Verwenden Sie die neueste Version von SQL Server Management Studio, um immer den Updates von Microsoft Azure und SQL-Datenbank zu entsprechen. Ältere Versionen von SQL Server Management Studio können SQL-Datenbank nicht als Abonnenten einrichten. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## Nächste Schritte

- [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Neueste Version von SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server 2016](https://www.microsoft.com/de-DE/cloud-platform/sql-server)

## Zusätzliche Ressourcen

- [Transaktionsreplikation](https://msdn.microsoft.com/library/mt589530.aspx)
- [SQL-Datenbank V12](sql-database-v12-whats-new.md)
- [Teilweise oder vollständig unterstützte Transact-SQL-Funktionen](sql-database-transact-sql-information.md)
- [Migrate non-SQL Server databases using SQL Server Migration Assistant (Migrieren von Nicht-SQL Server-Datenbanken mithilfe des SQL Server-Migrations-Assistenten)](http://blogs.msdn.com/b/ssma/)

<!---HONumber=AcomDC_0824_2016-->