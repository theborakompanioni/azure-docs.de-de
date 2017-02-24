---
title: 'SSMS: Exportieren einer Datenbank in eine BACPAC-Datei (Azure) | Microsoft-Dokumentation'
description: "In diesem Artikel wird beschrieben, wie Sie mithilfe des Assistenten „Datenebenenanwendung exportieren“ in SQL Server Management Studio eine SQL Server-Datenbank in eine BACPAC-Datei exportieren."
keywords: "Microsoft Azure SQL-Datenbank, Datenbankmigration, Datenbank exportieren, BACPAC-Datei exportieren, Assistent „Datenebenenanwendung exportieren“"
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
ms.workload: sqldb-migrate
ms.date: 02/07/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 3f0af43c103e34535fd114e33c40da010ea69d69
ms.openlocfilehash: 87d9ae3a5c9c8b8edb02e212f652de5f2f796188


---
# <a name="export-an-azure-sql-database-or-a-sql-server-database-to-a-bacpac-file-using-sql-server-management-studio"></a>Exportieren einer Azure SQL- oder SQL Server-Datenbank in eine BACPAC-Datei mithilfe von SQL Server Management Studio        
        
In diesem Artikel wird beschrieben, wie Sie mithilfe des Assistenten „Datenebenenanwendung exportieren“ in SQL Server Management Studio eine Azure SQL- oder SQL Server-Datenbank in eine [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4)-Datei exportieren. Einen Überblick über den Export in eine BACPAC-Datei finden Sie unter [Exportieren in eine BACPAC-Datei](sql-database-export.md).    
        
1. Stellen Sie sicher, dass Sie über die neueste Version von SQL Server Management Studio verfügen. Neue Versionen von Management Studio werden monatlich aktualisiert, damit sie mit Updates des Azure-Portals synchron sind.        
           
   > [!IMPORTANT]        
   > Es wird empfohlen, immer die neueste Version von Management Studio zu verwenden, damit Sie mit Updates von Microsoft Azure und SQL-Datenbank synchron sind. [Neueste Version von SSMS](https://msdn.microsoft.com/library/mt238290.aspx).        
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
* [Neueste Version von SSMS](https://msdn.microsoft.com/library/mt238290.aspx)
* Informationen zum Importieren einer BACPAC-Datei in eine SQL Server-Datenbank finden Sie unter [Importieren einer BACPAC-Datei in eine SQL Server-Datenbank](https://msdn.microsoft.com/library/hh710052.aspx)
* Eine Erläuterung des gesamten Migrationsprozesses von SQL Server-Datenbanken, einschließlich Empfehlungen zur Leistung, finden Sie unter [Migrieren einer SQL Server-Datenbank zu Azure SQL-Datenbank](sql-database-cloud-migrate.md).
* Informationen zur langfristigen Archivierung von Azure SQL-Datenbanksicherungen als Alternative zum Exportieren einer Datenbank für Archivzwecke finden Sie unter [Langfristige Archivierung von Sicherungen](sql-database-long-term-retention.md).


    



<!--HONumber=Feb17_HO2-->


