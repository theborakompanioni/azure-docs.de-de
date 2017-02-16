---
title: 'SSMS: Migrieren einer SQL Server-Datenbank zu Azure SQL-Datenbank | Microsoft Docs'
description: In diesem Artikel wird beschrieben, wie Sie mit dem Assistenten zum Bereitstellen einer Datenbank unter Microsoft Azure in SQL Server Management Studio eine kompatible SQL Server-Datenbank direkt zu Ihrem Azure SQL-Datenbank-Server migrieren.
keywords: "Microsoft Azure SQL-Datenbank, Datenbankmigration, Assistent für Microsoft Azure-Datenbank"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 91d3fd5e-d035-4c55-b1ea-a7ccc8e0f543
ms.service: sql-database
ms.custom: migrate and move
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: sqldb-migrate
ms.date: 08/24/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 86bc7d89bb5725add8ba05b6f0978467147fd3ca
ms.openlocfilehash: f4cbfc1a277cbe80edbfbb4dc651adb1972f41b4


---
# <a name="migrate-sql-server-database-to-sql-database-using-deploy-database-to-microsoft-azure-database-wizard"></a>Migrieren von SQL Server-Datenbank auf SQL-Datenbank mit dem Assistenten zum Bereitstellen einer Datenbank unter Microsoft Azure-Datenbank
> [!div class="op_single_selector"]
> * [SSMS-Migrations-Assistent](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
> * [Exportieren in eine BACPAC-Datei](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
> * [Importieren aus einer BACPAC-Datei](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
> * [Transaktionsreplikation](sql-database-cloud-migrate-compatible-using-transactional-replication.md)
> 
> 

In diesem Artikel wird beschrieben, wie Sie mit dem Assistenten zum Bereitstellen einer Datenbank unter Microsoft Azure in SQL Server Management Studio eine [kompatible SQL Server-Datenbank](sql-database-cloud-migrate.md) direkt zu Ihrem Azure SQL-Datenbank-Server migrieren.

## <a name="use-the-deploy-database-to-microsoft-azure-database-wizard"></a>Verwenden des Assistenten zum Bereitstellen einer Datenbank unter Microsoft Azure-Datenbank
> [!NOTE]
> Die folgenden Schritte setzen voraus, dass Sie über einen [bereitgestellten SQL-Datenbankserver](https://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database/)verfügen.
> 
> 

1. Stellen Sie sicher, dass Sie über die neueste Version von SQL Server Management Studio verfügen. Neue Versionen von Management Studio werden monatlich aktualisiert, damit sie mit Updates des Azure-Portals synchron sind.
   
   > [!IMPORTANT]
   > Es wird empfohlen, immer die neueste Version von Management Studio zu verwenden, damit Sie mit Updates von Microsoft Azure und SQL-Datenbank synchron sind. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
   > 
   > 
2. Öffnen Sie Management Studio und stellen Sie eine Verbindung mit Ihrer SQL Server-Datenbank her, die in den Objekt-Explorer migriert werden soll.
3. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf die Datenbank, zeigen Sie auf **Aufgaben**, und klicken Sie auf **Datenbank für Microsoft Azure SQL-Datenbank bereitstellen…**
   
    !["Bereitstellen in Azure" aus dem Menü "Aufgaben"](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard01.png)
4. Klicken Sie im Bereitstellungs-Assistenten auf **Weiter**, und klicken Sie dann auf **Verbinden**, um die Verbindung mit Ihrem SQL-Datenbank-Server zu konfigurieren.
   
   !["Bereitstellen in Azure" aus dem Menü "Aufgaben"](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard002.png)
5. Geben Sie im Dialogfeld „Verbindung mit Server herstellen“ Ihre Verbindungsinformationen ein, um die Verbindung mit Ihrem SQL-Datenbankserver herzustellen. Um den Servernamen für einen vorhandenen Server abzurufen, navigieren Sie zu [Anzeigen und Aktualisieren eines SQL Server-Datenbankservers und seiner Einstellungen](sql-database-view-update-server-settings.md).
   
    !["Bereitstellen in Azure" aus dem Menü "Aufgaben"](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard00.png)
6. Geben Sie Folgendes für die [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) -Datei an, die dieser Assistent während der Migration erstellt:
   
   * Den **neuen Datenbanknamen** 
   * Die **Edition von Microsoft Azure SQL-Datenbank** ([Dienstebene](sql-database-service-tiers.md))
   * Die **maximale Datenbankgröße**
   * Das **Dienstziel** (Leistungsstufe)
   * Den **temporären Dateinamen**  
   
   ![Exporteinstellungen](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard02.png)
7. Schließen Sie den Assistenten ab. Je nach Größe und Komplexität der Datenbank kann die Bereitstellung wenige Minuten bis viele Stunden dauern. Wenn dieser Assistent Kompatibilitätsprobleme erkennt, werden auf dem Bildschirm Fehler angezeigt, und die Migration wird nicht fortgesetzt. Eine Anleitung zum Beheben von Problemen mit der Datenbankkompatibilität finden Sie unter [Beheben von Problemen mit der Datenbankkompatibilität](sql-database-cloud-migrate-fix-compatibility-issues.md).
8. Stellen Sie mit dem Objekt-Explorer eine Verbindung mit der migrierten Datenbank auf Ihrem Azure SQL-Datenbankserver her.
9. Zeigen Sie über das Azure-Portal Ihre Datenbank und deren Eigenschaften an.

## <a name="next-steps"></a>Nächste Schritte
* [Neueste Version von SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
* [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Azure SQL Database features (Features der Azure SQL-Datenbank)](sql-database-features.md)
* [Teilweise oder vollständig unterstützte Transact-SQL-Funktionen](sql-database-transact-sql-information.md)
* [Migrate non-SQL Server databases using SQL Server Migration Assistant (Migrieren von Nicht-SQL Server-Datenbanken mithilfe des SQL Server-Migrations-Assistenten)](http://blogs.msdn.com/b/ssma/)




<!--HONumber=Jan17_HO1-->


