<properties
   pageTitle="Ermitteln der SQL-Datenbankkompatibilität mithilfe von SqlPackage.exe | Microsoft Azure"
   description="Microsoft Azure SQL-Datenbank, Datenbankmigration, SQL-Datenbankkompatibilität, SqlPackage"
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
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# Ermitteln der SQL-Datenbankkompatibilität mithilfe von SqlPackage.exe

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Aktualisierungsratgeber](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

In diesem Artikel erfahren Sie, wie Sie ermitteln können, ob eine SQL Server-Datenbank für die Migration zu SQL-Datenbank geeignet ist, indem Sie das [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx)-Befehlszeilen-Hilfsprogramm für die Eingabeaufforderung verwenden.

## Verwenden von SqlPackage.exe

1. Öffnen Sie ein Eingabeaufforderungsfenster, und wechseln Sie in ein Verzeichnis mit der neuesten Version von „sqlpackage.exe“. Dieses Dienstprogramm wird mit den neuesten Versionen von [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) und [SQL Server Data Tools für Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx) ausgeliefert. Alternativ dazu können Sie die neueste Version von [SqlPackage](https://www.microsoft.com/de-DE/download/details.aspx?id=53876) direkt aus dem Microsoft Download Center herunterladen.
2. Führen Sie den folgenden SqlPackage-Befehl mit den unten angegebenen Argumenten für Ihre Umgebung aus:

	'sqlpackage.exe /Action:Export /ssn:< Servername > /sdn:< Datenbankname > /tf:< Zieldatei > /p:TableData=< Schemaname.Tabellenname > > < Ausgabedatei > 2>&1'

	| Argument | Beschreibung |
	|---|---|
	| < Servername > | Name des Quellservers |
	| < Datenbankname > | Name der Quelldatenbank |
	| < Zieldatei > | Dateiname und Speicherort für die BACPAC-Datei |
	| < Schemaname.Tabellenname > | Tabellen, für die Daten in die Zieldatei ausgegeben werden |
	| < Ausgabedatei > | Dateiname und Speicherort für die Ausgabedatei mit Fehlern, falls vorhanden |

	Der Grund für das Argument „/p:TableName“ ist, dass wir nur die Datenbankkompatibilität in Bezug auf das Exportieren nach Azure SQL-Datenbank V12 testen und nicht die Daten aus allen Tabellen exportieren möchten. Das Exportargument für „sqlpackage.exe“ wird leider nicht unterstützt, wenn keine Tabellen extrahiert werden. Sie müssen mindestens eine Tabelle angeben, z.B. eine einzelne kleine Tabelle. Die < Ausgabedatei > enthält den Bericht zu etwaigen Fehlern. Mit der Zeichenfolge „> 2>&1“ werden sowohl die Standardausgabe als auch der Standardfehler, die bzw. der sich aus der Befehlsausführung ergibt, an die angegebene Ausgabedatei geleitet.

	![Exportieren von Datenebenenanwendungen im Menü "Aufgaben"](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01.png)

3. Öffnen Sie die Ausgabedatei, und sehen Sie sich die Kompatibilitätsfehler an, falls vorhanden.

	![Exportieren von Datenebenenanwendungen im Menü "Aufgaben"](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage02.png)

## Nächste Schritte

- [Neueste Version von SSDT](https://msdn.microsoft.com/library/mt204009.aspx) [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Beheben von Kompatibilitätsproblemen in Bezug auf die Datenbankmigration](sql-database-cloud-migrate.md#fix-database-migration-compatibility-issues)
- [Migrieren einer kompatiblen SQL Server-Datenbank zur SQL-Datenbank](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## Zusätzliche Ressourcen

- [SQL-Datenbank V12](sql-database-v12-whats-new.md)
- [Teilweise oder vollständig unterstützte Transact-SQL-Funktionen](sql-database-transact-sql-information.md)
- [Migrate non-SQL Server databases using SQL Server Migration Assistant (Migrieren von Nicht-SQL Server-Datenbanken mithilfe des SQL Server-Migrations-Assistenten)](http://blogs.msdn.com/b/ssma/)

<!---HONumber=AcomDC_0921_2016-->