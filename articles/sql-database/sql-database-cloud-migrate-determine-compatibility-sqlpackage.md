<properties
   pageTitle="Ermitteln der SQL-Datenbankkompatibilität mithilfe von SqlPackage.exe"
   description="Microsoft Azure SQL-Datenbank, Datenbankmigration, SQL-Datenbankkompatibilität, SqlPackage"
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

# Ermitteln der SQL-Datenbankkompatibilität mithilfe von SqlPackage.exe

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Aktualisierungsratgeber](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

In diesem Artikel erfahren Sie, wie Sie ermitteln können, ob eine SQL Server-Datenbank für die Migration zur SQL-Datenbank geeignet ist, indem Sie das [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx)-Hilfsprogramm für die Eingabeaufforderung verwenden.

## Verwenden von SqlPackage.exe

1. Öffnen Sie ein Eingabeaufforderungsfenster, und wechseln Sie in ein Verzeichnis mit der neuesten Version von „sqlpackage.exe“. Dieses Hilfsprogramm wird mit Visual Studio und SQL Server bereitgestellt. Laden Sie die [neueste Version von SQL Server Data Tools für Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx) herunter, um die neueste Version des SqlPackage-Hilfsprogramms zu erhalten.
2. Führen Sie den folgenden SqlPackage-Befehl mit den unten angegebenen Argumenten für Ihre Umgebung aus:

	'sqlpackage.exe /Action:Export /ssn:< server_name > /sdn:< database_name > /tf:< target_file > /p:TableData=< schema_name.table_name > > < output_file > 2>&1'

	| Argument | Beschreibung |
	|---|---|
	| < server_name > | Name des Quellservers |
	| < database_name > | Name der Quelldatenbank |
	| < target_file > | Dateiname und Speicherort für die BACPAC-Datei |
	| < schema_name.table_name > | Tabellen, für die Daten in die Zieldatei ausgegeben werden |
	| < output_file > | Dateiname und Speicherort für die Ausgabedatei mit Fehlern, falls vorhanden |

	Der Grund für das Argument „/p:TableName“ ist, dass wir nur die Datenbankkompatibilität in Bezug auf das Exportieren nach Azure SQL-Datenbank V12 testen möchten und nicht die Daten aus allen Tabellen exportieren möchten. Leider unterstützt das export-Argument für „sqlpackage.exe“ das Extrahieren von 0 Tabellen nicht. Sie müssen also eine kleine Tabelle angeben. < output_file > enthält den Bericht zu etwaigen Fehlern. Mit der Zeichenfolge „> 2>&1“ werden sowohl die Standardausgabe als auch der Standardfehler, die bzw. der sich aus der Befehlsausführung ergibt, an die angegebene Ausgabedatei geleitet.

	![Exportieren von Datenebenenanwendungen im Menü "Aufgaben"](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01.png)

3. Öffnen Sie die Ausgabedatei, und sehen Sie sich die Kompatibilitätsfehler an, falls vorhanden.

	![Exportieren von Datenebenenanwendungen im Menü "Aufgaben"](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage02.png)

## Nächste Schritte

- [Neueste Version von SSDT](https://msdn.microsoft.com/library/mt204009.aspx) [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Beheben von Kompatibilitätsproblemen in Bezug auf die Datenbankmigration](sql-database-cloud-migrate.md#fix-database-migration-compatibility-issues)
- [Migrieren einer kompatiblen SQL Server-Datenbank zur SQL-Datenbank](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## Weitere Informationen

- [SQL-Datenbank V12](sql-database-v12-whats-new.md)
- [Teilweise oder vollständig unterstützte Transact-SQL-Funktionen](sql-database-transact-sql-information.md)
- [Migrate non-SQL Server databases using SQL Server Migration Assistant](http://blogs.msdn.com/b/ssma/) (Migrieren von Nicht-SQL Server-Datenbanken mithilfe des SQL Server-Migrations-Assistenten)

<!---HONumber=AcomDC_0601_2016-->