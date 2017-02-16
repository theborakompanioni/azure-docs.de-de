---
title: "Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt | Microsoft Docs"
description: "Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: NA
ms.date: 12/08/2016
ms.author: sstein; carlrab
translationtype: Human Translation
ms.sourcegitcommit: 75bf523679c8d8ad6fbe4a8aa8a561d03008e59b
ms.openlocfilehash: 4a3c7ae453ead16aec68d362676aa0a000b647ba


---
# <a name="restore-an-azure-sql-database-to-a-previous-point-in-time"></a>Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt 

In diesem Artikel erfahren Sie, wie Sie aus [automatisierten SQL-Datenbanksicherungen](sql-database-automated-backups.md) einen früheren Zustand Ihrer Datenbank wiederherstellen. 

## <a name="restore-to-a-previous-point-in-time-using-the-azure-portal"></a>Wiederherstellung des Zustands zu einem früheren Zeitpunkt über das Azure-Portal

> [!TIP]
> Ein Tutorial finden Sie unter [Erste Schritte mit der Sicherung und Wiederherstellung für Datenschutz und Wiederherstellung](sql-database-get-started-backup-recovery.md).
>

Auswählen einer Datenbank für die Wiederherstellung im Azure-Portal:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Wählen Sie auf der linken Bildschirmseite **Weitere Dienste** > **SQL-Datenbanken**.
3. Klicken Sie auf die Datenbank, die Sie wiederherstellen möchten.
4. Wählen Sie oben auf der Seite der Datenbank **Wiederherstellen**aus:
   
   ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-point-in-time-restore-portal/restore.png)
5. Wählen Sie auf der Seite **Wiederherstellen** das Datum und die Uhrzeit (im UTC-Zeitformat) für die Wiederherstellung der Datenbank, und klicken Sie dann auf **OK**:
   
   ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-point-in-time-restore-portal/restore-details.png)

6. Nachdem Sie im vorherigen Schritt auf **OK** geklickt haben, klicken auf der Seite oben rechts auf das Benachrichtigungssymbol und dann auf die Benachrichtigung **SQL-Datenbank wird wiederhergestellt**, um Details anzuzeigen.
   
    ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-point-in-time-restore-portal/notification-icon.png)
7. Die Seite „SQL-Datenbank wird wiederhergestellt“ mit Informationen zum Status der Wiederherstellung wird geöffnet. Sie können auf die Position klicken, um weitere Details anzuzeigen:
   
    ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-point-in-time-restore-portal/inprogress.png)

## <a name="restore-to-a-previous-point-in-time-using-powershell"></a>Wiederherstellung des Zustands zu einem früheren Zeitpunkt mithilfe von PowerShell

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell-h3.md)]

### <a name="restore-your-database-to-a-point-in-time-as-a-single-database"></a>Wiederherstellen des Zustands der Datenbank zu einem bestimmten Zeitpunkt als Einzeldatenbank
1. Rufen Sie die wiederherzustellende Datenbank mit dem Cmdlet [Get-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt603648\(v=azure.300\).aspx) ab.
   
        $Database = Get-AzureRmSqlDatabase -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"
2. Stellen Sie den Zustand der Datenbank zu einem bestimmten Zeitpunkt mit dem Cmdlet [Restore-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390\(v=azure.300\).aspx) wieder her.
   
        Restore-AzureRmSqlDatabase -FromPointInTimeBackup -PointInTime UTCDateTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName "RestoredDatabase" -ResourceId $Database.ResourceID -Edition "Standard" -ServiceObjectiveName "S2"

### <a name="restore-your-database-to-a-point-in-time-into-an-elastic-pool"></a>Wiederherstellen des Zustands der Datenbank zu einem bestimmten Zeitpunkt in einem elastischen Pool
1. Rufen Sie die wiederherzustellende Datenbank mit dem Cmdlet [Get-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt603648\(v=azure.300\).aspx) ab.
   
        $Database = Get-AzureRmSqlDatabase -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"
2. Stellen Sie den Zustand der Datenbank zu einem bestimmten Zeitpunkt mit dem Cmdlet [Restore-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390\(v=azure.300\).aspx) wieder her.
   
        Restore-AzureRmSqlDatabase -FromPointInTimeBackup -PointInTime UTCDateTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName "RestoredDatabase" -ResourceId $Database.ResourceID -ElasticPoolName "elasticpool01"

## <a name="next-steps"></a>Nächste Schritte
* Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md)
* Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md)
* Informationen zum Anzeigen des ältesten Wiederherstellungspunkts aus den vom Dienst generierten Sicherungen einer Datenbank finden Sie unter [Anzeigen des ältesten Wiederherstellungspunkts aus den vom Dienst generierten Sicherungen einer Azure SQL-Datenbank](sql-database-view-oldest-restore-point.md).
* Eine Übersicht und verschiedene Szenarien zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)




<!--HONumber=Dec16_HO3-->


