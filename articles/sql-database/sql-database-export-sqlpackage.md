---
title: 'SqlPackage: Exportieren einer SQL Server-Datenbank in eine BACPAC-Datei (Azure) | Microsoft Docs'
description: Dieser Artikel zeigt, wie Sie eine SQL Server-Datenbank mithilfe des Befehlszeilen-Hilfsprogramms SqlPackage in eine BACPAC-Datei exportieren.
keywords: Microsoft Azure SQL-Datenbank, Datenbankmigration, Datenbank exportieren, BACPAC-Datei exportieren, sqlpackage
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 7b9541c5-5590-4c70-ad36-73007389f6dc
ms.service: sql-database
ms.custom: migrate and move
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: sqldb-migrate
ms.date: 02/07/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 3d04be3d2427bc59d24bfaad227730991b61265b
ms.openlocfilehash: a17bfa6868ac581dc5ae562b3174f28571d52698
ms.lasthandoff: 02/11/2017


---
# <a name="export-an-azure-sql-database-or-a-sql-server-database-to-a-bacpac-file-using-sqlpackage"></a>Exportieren einer Azure SQL- oder SQL Server-Datenbank in eine BACPAC-Datei mithilfe von SqlPackage

Dieser Artikel zeigt, wie Sie eine Azure SQL- oder SQL-Server-Datenbank mithilfe des Befehlszeilenhilfsprogramms [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) in eine BACPAC-Datei exportieren. Dieses Hilfsprogramm wird mit den neuesten Versionen von [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) und [SQL Server Data Tools für Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx) ausgeliefert. Alternativ dazu können Sie die neueste Version von [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) direkt aus dem Microsoft Download Center herunterladen.

Einen Überblick über den Export in eine BACPAC-Datei finden Sie unter [Exportieren in eine BACPAC-Datei](sql-database-export.md).

> [!NOTE]
> Sie können Ihre Azure SQL-Datenbankdatei auch über das [Azure-Portal](sql-database-export-portal.md), [SQL Server Management Studio](sql-database-export-ssms.md) oder [PowerShell](sql-database-export-powershell.md) in eine BACPAC-Datei exportieren.
>

## <a name="sqlpackage-utility"></a>SqlPackage-Hilfsprogramm

1. Öffnen Sie ein Eingabeaufforderungsfenster, und wechseln Sie in ein Verzeichnis mit dem Befehlszeilen-Hilfsprogramm „sqlpackage.exe“. Dieses Hilfsprogramm ist im Lieferumfang von Visual Studio und SQL Server enthalten. Verwenden Sie die Suche auf dem Computer, um den Pfad in Ihrer Umgebung zu finden.
2. Führen Sie den folgenden sqlpackage.exe-Befehl mit den unten angegebenen Argumenten für Ihre Umgebung aus:
   
```    sqlpackage.exe /Action:Export /ssn:< server_name > /sdn:< database_name > /tf:< target_file >
```
   
   | Argument | Description |
   | --- | --- |
   | < server_name > |source server name |
   | < database_name > |source database name |
   | < target_file > |file name and location for BACPAC file |
   
   ![Export a data-tier application from the Tasks menu](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01b.png)

## Next steps

* To learn about importing a BACPAC using SQLPackage, see [Import a BACPAC to Azure SQL Database using SqlPackage](sql-database-import-sqlpackage.md)
* To learn about importing a BACPAC using the Azure portal, see [Import a BACPAC to Azure SQL Database using the Azure portal](sql-database-import-portal.md)
* To learn about importing a BACPAC using PowerShell, see [Import a BACPAC to Azure SQL Database using PowerShell](scripts/sql-database-import-from-bacpac-powershell.md)
* For a discussion of the entire SQL Server database migration process, including performance recommendations, see [Migrate a SQL Server database to Azure SQL Database](sql-database-cloud-migrate.md).
* To learn about long-term backup retention of an Azure SQL database backup as an alternative to exported a database for archive purposes, see [Long term backup retention](sql-database-long-term-retention.md)
* To learn about importing a BACPAC to a SQL Server database, see [Import a BACPCAC to a SQL Server database](https://msdn.microsoft.com/library/hh710052.aspx)



