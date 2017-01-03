---
title: Importieren aus einer BACPAC-Datei in SQL-Datenbank per SqlPackage
description: Microsoft Azure SQL-Datenbank, Datenbankmigration, Datenbank importieren, BACPAC-Datei importieren, sqlpackage
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 424afa27-5f13-4ec3-98f6-99a511a6a2df
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
ms.openlocfilehash: d1f701bc5e4028db4d97ac2e7097afedf46b1f1f


---
# <a name="import-to-sql-database-from-a-bacpac-file-using-sqlpackage"></a>Importieren aus einer BACPAC-Datei in SQL-Datenbank per SqlPackage
> [!div class="op_single_selector"]
> * [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
> * [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
> * [Azure-Portal](sql-database-import.md)
> * [PowerShell](sql-database-import-powershell.md)
> 
> 

Dieser Artikel zeigt, wie Sie aus einer [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4)-Datei mithilfe des [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx)-Befehlszeilen-Hilfsprogramms für die Eingabeaufforderung in SQL-Datenbank importieren. Dieses Hilfsprogramm wird mit den neuesten Versionen von [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) und [SQL Server Data Tools für Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx) ausgeliefert. Alternativ dazu können Sie die neueste Version von [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) direkt aus dem Microsoft Download Center herunterladen.

> [!NOTE]
> Die folgenden Schritten setzen voraus, dass Sie bereits einen SQL-Datenbankserver bereitgestellt haben, Ihnen die Verbindungsinformationen vorliegen und Sie die Kompatibilität der Quelldatenbank überprüft haben.
> 
> 

## <a name="import-from-a-bacpac-file-into-azure-sql-database-using-sqlpackage"></a>Importieren aus einer BACPAC-Datei in Azure SQL-Datenbank per SqlPackage
Führen Sie die folgenden Schritte aus, um das Befehlszeilen-Hilfsprogramm [SqlPackage.exe](https://msdn.microsoft.com/library/hh550080.aspx) zum Importieren einer kompatiblen SQL Server-Datenbank (oder Azure SQL-Datenbank) aus einer BACPAC-Datei zu verwenden.

> [!NOTE]
> Die folgenden Schritten setzen voraus, dass Sie bereits einen Azure SQL-Datenbank-Server bereitgestellt haben und Ihnen die Verbindungsinformationen vorliegen.
> 
> 

1. Öffnen Sie ein Eingabeaufforderungsfenster, und wechseln Sie in ein Verzeichnis mit dem Befehlszeilen-Hilfsprogramm „sqlpackage.exe“. Dieses Hilfsprogramm ist im Lieferumfang von Visual Studio und SQL Server enthalten.
2. Führen Sie den folgenden sqlpackage.exe-Befehl mit den unten angegebenen Argumenten für Ihre Umgebung aus:
   
    `sqlpackage.exe /Action:Import /tsn:< server_name > /tdn:< database_name > /tu:< user_name > /tp:< password > /sf:< source_file >`
   
   | Argument | Beschreibung |
   | --- | --- |
   | < Servername > |Zielservername |
   | < Datenbankname > |Zieldatenbankname |
   | < Benutzername > |Benutzername auf dem Zielserver |
   | < Kennwort > |Kennwort des Benutzers |
   | < Quelldatei > |Dateiname und Speicherort für die zu importierende BACPAC-Datei |
   
    ![Exportieren von Datenebenenanwendungen im Menü "Aufgaben"](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01c.png)

## <a name="next-steps"></a>Nächste Schritte
* [Neueste Version von SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
* [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Azure SQL Database features (Features der Azure SQL-Datenbank)](sql-database-features.md)
* [Teilweise oder vollständig unterstützte Transact-SQL-Funktionen](sql-database-transact-sql-information.md)
* [Migrate non-SQL Server databases using SQL Server Migration Assistant (Migrieren von Nicht-SQL Server-Datenbanken mithilfe des SQL Server-Migrations-Assistenten)](http://blogs.msdn.com/b/ssma/)




<!--HONumber=Nov16_HO4-->


