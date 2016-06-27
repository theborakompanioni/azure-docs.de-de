<properties
   pageTitle="Exportieren einer SQL Server-Datenbank in eine BACPAC-Datei per SqlPackage | Microsoft Azure"
   description="Microsoft Azure SQL-Datenbank, Datenbankmigration, Datenbank exportieren, BACPAC-Datei exportieren, sqlpackage"
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

# Exportieren einer SQL Server-Datenbank in eine BACPAC-Datei per SqlPackage

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)

Dieser Artikel zeigt, wie Sie eine SQL-Serverdatenbank in eine [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4)-Datei mithilfe des [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx)-Hilfsprogramms für die Eingabeaufforderung exportieren. Dieses Hilfsprogramm wird mit Visual Studio und SQL Server bereitgestellt. Sie können auch die aktuelle Version der SQL Server Data Tools [herunterladen](https://msdn.microsoft.com/library/mt204009.aspx), um dieses Hilfsprogramm zu erhalten.

1. Öffnen Sie ein Eingabeaufforderungsfenster, und wechseln Sie in ein Verzeichnis mit dem Befehlszeilen-Hilfsprogramm „sqlpackage.exe“. Dieses Hilfsprogramm ist im Lieferumfang von Visual Studio und SQL Server enthalten. Verwenden Sie die Suche auf dem Computer, um den Pfad in Ihrer Umgebung zu finden.
2. Führen Sie den folgenden sqlpackage.exe-Befehl mit den unten angegebenen Argumenten für Ihre Umgebung aus:

	'sqlpackage.exe /Action:Export /ssn:< server_name > /sdn:< database_name > /tf:< target_file >

	| Argument | Beschreibung |
	|---|---|
	| < server_name > | Name des Quellservers |
	| < database_name > | Name der Quelldatenbank |
	| < target_file > | Dateiname und Speicherort für die BACPAC-Datei |

	![Exportieren von Datenebenenanwendungen im Menü "Aufgaben"](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01b.png)

## Nächste Schritte

- [Neueste Version von SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Importieren einer BACPAC-Datei in Azure SQL-Datenbank mithilfe von SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Importieren einer BACPAC-Datei in Azure SQL-Datenbank mithilfe von SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Importieren einer BACPAC-Datei in Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-import.md)
- [Importieren einer BACPAC-Datei in Azure SQL-Datenbank mithilfe von PowerShell](sql-database-import-powershell.md)

## Zusätzliche Ressourcen

- [SQL-Datenbank V12](sql-database-v12-whats-new.md)
- [Teilweise oder vollständig unterstützte Transact-SQL-Funktionen](sql-database-transact-sql-information.md)
- [Migrate non-SQL Server databases using SQL Server Migration Assistant (Migrieren von Nicht-SQL Server-Datenbanken mithilfe des SQL Server-Migrations-Assistenten)](http://blogs.msdn.com/b/ssma/)

<!---HONumber=AcomDC_0615_2016-->