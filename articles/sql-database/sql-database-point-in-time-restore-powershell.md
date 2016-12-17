---
title: "Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt (PowerShell) | Microsoft Docs"
description: "Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 3d1ecd68-5e78-4e8d-9347-b37b5fa15f36
ms.service: sql-database
ms.custom: business continuity; how to
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: NA
ms.date: 07/17/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 4b70d51cbd4b245605173ee596fc0ea587668b92


---
# <a name="restore-an-azure-sql-database-to-a-previous-point-in-time-with-powershell"></a>Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt mit PowerShell
> [!div class="op_single_selector"]
> * [Übersicht](sql-database-recovery-using-backups.md)
> * [Point-in-Time-Wiederherstellung: im Azure-Portal](sql-database-point-in-time-restore-portal.md)
> 
> 

In diesem Artikel erfahren Sie, wie Sie aus [automatisierten SQL-Datenbanksicherungen](sql-database-automated-backups.md)einen früheren Zustand Ihrer Datenbank wiederherstellen. Hierfür können Sie PowerShell verwenden.

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="restore-your-database-to-a-point-in-time-as-a-standalone-database"></a>Wiederherstellen des Zustands der Datenbank zu einem bestimmten Zeitpunkt als eigenständige Datenbank
1. Rufen Sie die wiederherzustellende Datenbank mit dem Cmdlet [Get-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt603648\(v=azure.300\).aspx) ab.
   
        $Database = Get-AzureRmSqlDatabase -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"
2. Stellen Sie den Zustand der Datenbank zu einem bestimmten Zeitpunkt mit dem Cmdlet [Restore-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390\(v=azure.300\).aspx) wieder her.
   
        Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime UTCDateTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $Database.ResourceID -Edition "Standard" -ServiceObjectiveName "S2"

## <a name="restore-your-database-to-a-point-in-time-into-an-elastic-database-pool"></a>Wiederherstellen des Zustands der Datenbank zu einem bestimmten Zeitpunkt in einem Pool für elastische Datenbanken
1. Rufen Sie die wiederherzustellende Datenbank mit dem Cmdlet [Get-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt603648\(v=azure.300\).aspx) ab.
   
        $Database = Get-AzureRmSqlDatabase -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"
2. Stellen Sie den Zustand der Datenbank zu einem bestimmten Zeitpunkt mit dem Cmdlet [Restore-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390\(v=azure.300\).aspx) wieder her.
   
        Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime UTCDateTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $Database.ResourceID –ElasticPoolName "elasticpool01"

## <a name="next-steps"></a>Nächste Schritte
* Eine Übersicht und verschiedene Szenarien zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
* Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md)
* Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md)
* Informationen über schnellere Wiederherstellungsoptionen finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md)  
* Informationen zum Verwenden automatisierter Sicherungen für die Archivierung finden Sie unter [Datenbankkopie](sql-database-copy.md)




<!--HONumber=Nov16_HO3-->


