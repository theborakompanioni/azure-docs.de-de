<properties
   pageTitle="Importieren aus einer BACPAC-Datei in SQL-Datenbank per SqlPackage"
   description="Microsoft Azure SQL-Datenbank, Datenbankmigration, Datenbank importieren, BACPAC-Datei importieren, sqlpackage"
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

# Importieren aus einer BACPAC-Datei in SQL-Datenbank per SqlPackage

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Azure-Portal](sql-database-import.md)
- [PowerShell](sql-database-import-powershell.md)

Dieser Artikel zeigt, wie Sie aus einer [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4)-Datei mithilfe des [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx)-Befehlszeilen-Hilfsprogramms für die Eingabeaufforderung in die SQL-Datenbank importieren. Dieses Dienstprogramm wird mit den neuesten Versionen von [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) und [SQL Server Data Tools für Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx) ausgeliefert. Alternativ dazu können Sie die neueste Version von [SqlPackage](https://www.microsoft.com/de-DE/download/details.aspx?id=53876) direkt aus dem Microsoft Download Center herunterladen.


> [AZURE.NOTE] Die folgenden Schritten setzen voraus, dass Sie bereits einen SQL-Datenbankserver bereitgestellt haben, Ihnen die Verbindungsinformationen vorliegen und Sie die Kompatibilität der Quelldatenbank überprüft haben.

## Importieren aus einer BACPAC-Datei in Azure SQL-Datenbank per SqlPackage

Führen Sie die folgenden Schritte aus, um das Befehlszeilen-Hilfsprogramm [SqlPackage.exe](https://msdn.microsoft.com/library/hh550080.aspx) zum Importieren einer kompatiblen SQL Server-Datenbank (oder Azure SQL-Datenbank) aus einer BACPAC-Datei zu verwenden.

> [AZURE.NOTE] Die folgenden Schritten setzen voraus, dass Sie bereits einen Azure SQL-Datenbank-Server bereitgestellt haben und Ihnen die Verbindungsinformationen vorliegen.

1. Öffnen Sie ein Eingabeaufforderungsfenster, und wechseln Sie in ein Verzeichnis mit dem Befehlszeilen-Hilfsprogramm „sqlpackage.exe“. Dieses Hilfsprogramm ist im Lieferumfang von Visual Studio und SQL Server enthalten.
2. Führen Sie den folgenden sqlpackage.exe-Befehl mit den unten angegebenen Argumenten für Ihre Umgebung aus:

	`sqlpackage.exe /Action:Import /tsn:< server_name > /tdn:< database_name > /tu:< user_name > /tp:< password > /sf:< source_file >`

	| Argument | Beschreibung |
	|---|---|
	| < Servername > | Zielservername |
	| < Datenbankname > | Zieldatenbankname |
	| < Benutzername > | Benutzername auf dem Zielserver |
	| < Kennwort > | Kennwort des Benutzers |
	| < Quelldatei > | Dateiname und Speicherort für die zu importierende BACPAC-Datei |

	![Exportieren von Datenebenenanwendungen im Menü "Aufgaben"](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01c.png)

## Nächste Schritte

- [Neueste Version von SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## Zusätzliche Ressourcen

- [SQL-Datenbank V12](sql-database-v12-whats-new.md)
- [Teilweise oder vollständig unterstützte Transact-SQL-Funktionen](sql-database-transact-sql-information.md)
- [Migrate non-SQL Server databases using SQL Server Migration Assistant (Migrieren von Nicht-SQL Server-Datenbanken mithilfe des SQL Server-Migrations-Assistenten)](http://blogs.msdn.com/b/ssma/)

<!---HONumber=AcomDC_0921_2016-->