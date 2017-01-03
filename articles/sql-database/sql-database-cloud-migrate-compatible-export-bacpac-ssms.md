---
title: Exportieren einer SQL Server-Datenbank in eine BACPAC-Datei mit SQL Server Management Studio | Microsoft Docs
description: "Microsoft Azure SQL-Datenbank, Datenbankmigration, Datenbank exportieren, BACPAC-Datei exportieren, Assistent „Datenebenenanwendung exportieren“"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 19c2dab4-81a6-411d-b08a-0ef79b90fbce
ms.service: sql-database
ms.custom: migrate and move
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 11/08/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: e8bb9e5a02a7caf95dae0101c720abac1c2deff3
ms.openlocfilehash: f6f64479f6ddfda4badb8e826e03369c476d6d1d


---
# <a name="export-a-sql-server-database-to-a-bacpac-file-using-sql-server-management-studio"></a>Exportieren einer SQL Server-Datenbank in eine BACPAC-Datei mit SQL Server Management Studio
> [!div class="op_single_selector"]
> * [Azure-Portal](sql-database-export.md)
> * [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
> * [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)
> * [PowerShell](sql-database-export-powershell.md)
> 

In diesem Artikel wird beschrieben, wie Sie mithilfe des Assistenten „Datenebenenanwendung exportieren“ in SQL Server Management Studio eine SQL Server-Datenbank in eine [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) -Datei exportieren. 

1. Stellen Sie sicher, dass Sie über die neueste Version von SQL Server Management Studio verfügen. Neue Versionen von Management Studio werden monatlich aktualisiert, damit sie mit Updates des Azure-Portals synchron sind.
   
   > [!IMPORTANT]
   > Es wird empfohlen, immer die neueste Version von Management Studio zu verwenden, damit Sie mit Updates von Microsoft Azure und SQL-Datenbank synchron sind. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
   > 
   > 
2. Öffnen Sie Management Studio, und stellen Sie eine Verbindung mit Ihrer Quelldatenbank im Objekt-Explorer her.
   
    ![Exportieren von Datenebenenanwendungen im Menü "Aufgaben"](./media/sql-database-cloud-migrate/MigrateUsingBACPAC01.png)
3. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf die Quelldatenbank, zeigen Sie auf **Aufgaben**, und klicken Sie auf **Datenebenenanwendung exportieren...**.
   
    ![Exportieren von Datenebenenanwendungen im Menü "Aufgaben"](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS01.png)
4. Konfigurieren Sie im Export-Assistenten den Export für das Speichern der BACPAC-Datei entweder an einem lokalen Speicherort auf dem Datenträger oder in einem Azure-Blob. Die exportierte BACPAC-Datei enthält immer das vollständige Datenbankschema und standardmäßig die Daten aus allen Tabellen. Verwenden Sie die Registerkarte „Erweitert“, wenn Sie Daten aus einigen oder allen Tabellen ausschließen möchten. Sie können z. B. nur die Daten für Verweistabellen exportieren, anstatt die Daten aller Tabellen.

    ![Exporteinstellungen](./media/sql-database-cloud-migrate/MigrateUsingBACPAC02.png)

   > [!IMPORTANT]
   > Verwenden Sie beim Exportieren einer BACPAC-Datei in Azure Blob Storage den Standardspeicher. Das Importieren einer BACPAC-Datei aus dem Premiumspeicher wird nicht unterstützt.
   >
   
## <a name="next-steps"></a>Nächste Schritte
* [Neueste Version von SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
* [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
* [Importieren einer BACPAC-Datei in Azure SQL-Datenbank mithilfe von SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
* [Importieren einer BACPAC-Datei in Azure SQL-Datenbank mithilfe von SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
* [Importieren einer BACPAC-Datei in Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-import.md)
* [Importieren einer BACPAC-Datei in Azure SQL-Datenbank mithilfe von PowerShell](sql-database-import-powershell.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Azure SQL Database features (Features der Azure SQL-Datenbank)](sql-database-features.md)
* [Teilweise oder vollständig unterstützte Transact-SQL-Funktionen](sql-database-transact-sql-information.md)
* [Migrate non-SQL Server databases using SQL Server Migration Assistant (Migrieren von Nicht-SQL Server-Datenbanken mithilfe des SQL Server-Migrations-Assistenten)](http://blogs.msdn.com/b/ssma/)




<!--HONumber=Nov16_HO4-->


