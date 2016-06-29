<properties
   pageTitle="Migrieren zur SQL-Datenbank per Transaktionsreplikation | Microsoft Azure"
   description="Microsoft Azure SQL-Datenbank, Datenbankmigration, Datenbank importieren, Transaktionsreplikation"
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
   ms.workload="sqldb-migrate"
   ms.date="06/07/2016"
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

Sie können die Transaktionsreplikation auch zum Migrieren einer Teilmenge Ihrer lokalen Datenbank verwenden. Die Veröffentlichung, die Sie zu Azure SQL-Datenbank replizieren, kann auf eine Teilmenge der Tabellen in der replizierten Datenbank beschränkt werden. Für jede replizierte Tabelle können Sie die Daten außerdem auf eine Teilmenge der Zeilen bzw. eine Teilmenge der Spalten beschränken.

Bei der Transaktionsreplikation werden alle Änderungen an Ihren Daten oder Schemas, die zwischen dem Beginn der Migration und dem Abschluss der Migration erfolgen, in Ihrer Azure SQL-Datenbank angezeigt.

Wenn die Synchronisierung abgeschlossen ist und Sie zur Migration bereit sind, müssen Sie nur die Verbindungszeichenfolgen Ihrer Anwendungen so ändern, dass sie auf Ihre Azure SQL-Datenbank anstatt auf Ihre lokale Datenbank verweisen. Sobald mit der Transaktionsreplikation alle verbleibenden Änderungen Ihrer lokalen Datenbank abgearbeitet wurden und alle Anwendungen auf Azure DB zeigen, können Sie die Replikation sicher deinstallieren und Azure SQL-Datenbank als Produktionssystem nutzen.

 ![SeedCloudTR-Diagramm](./media/sql-database-cloud-migrate/SeedCloudTR.png)

## Anforderungen der Transaktionsreplikation

Die Transaktionsreplikation ist eine Technologie, die seit SQL Server 6.5 in SQL Server integriert ist. Es handelt sich hierbei um eine sehr ausgereifte und bewährte Technologie, für die die meisten Datenbankadministratoren über Erfahrung verfügen. Mit der [SQL Server 2016-Vorschau](http://www.microsoft.com/server-cloud/products/sql-server-2016/) kann die Azure SQL-Datenbank als [Transaktionsreplikationsabonnent](https://msdn.microsoft.com/library/mt589530.aspx) für die lokale Veröffentlichung konfiguriert werden. Wenn Sie die Einrichtung über Management Studio durchführen, erhalten Sie genau das gleiche Ergebnis wie beim Einrichten eines Abonnenten der Transaktionsreplikation auf einem lokalen Server. Der Support für dieses Szenario ist gegeben, wenn der Herausgeber und der Verteiler mindestens über eine der folgenden SQL Server-Versionen verfügen:

 - SQL Server 2016 CTP3 (Vorschau) und höher 
 - SQL Server 2014 SP1 CU3 und höher
 - SQL Server 2014 RTM CU10 und höher
 - SQL Server 2012 SP2 CU8 und höher
 - SQL Server 2013 SP3 (nach der Veröffentlichung)


> [AZURE.IMPORTANT] Sie müssen die neueste Version von SQL Server Management Studio verwenden, um immer auf dem gleichen Updatestand wie Microsoft Azure und SQL-Datenbank zu sein. Ältere Versionen von SQL Server Management Studio können SQL-Datenbank nicht als Abonnent einrichten. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## Nächste Schritte

- [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Neueste Version von SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server 2016 – Vorschau](http://www.microsoft.com/server-cloud/products/sql-server-2016/)

## Weitere Ressourcen

- [Transaktionsreplikation](https://msdn.microsoft.com/library/mt589530.aspx)
- [SQL-Datenbank V12](sql-database-v12-whats-new.md)
- [Teilweise oder vollständig unterstützte Transact-SQL-Funktionen](sql-database-transact-sql-information.md)
- [Migrate non-SQL Server databases using SQL Server Migration Assistant (Migrieren von Nicht-SQL Server-Datenbanken mithilfe des SQL Server-Migrations-Assistenten)](http://blogs.msdn.com/b/ssma/)

<!---HONumber=AcomDC_0615_2016-->