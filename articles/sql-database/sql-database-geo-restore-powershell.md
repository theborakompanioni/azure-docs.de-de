---
title: Wiederherstellen einer Azure SQL-Datenbank aus einer georedundanten Sicherung (PowerShell) | Microsoft Docs
description: Wiederherstellen einer Azure SQL-Datenbank aus einer georedundanten Sicherung auf einem neuen Server
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 4b42bffa-f98c-406a-9a96-51721cc423d4
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
ms.openlocfilehash: 4fe8bc103856f891d3b29fa77f5c43dd0c403e8f


---
# <a name="restore-an-azure-sql-database-from-a-geo-redundant-backup-by-using-powershell"></a>Wiederherstellen einer Azure SQL-Datenbank aus einer georedundanten Sicherung mit PowerShell
> [!div class="op_single_selector"]
> * [Übersicht](sql-database-recovery-using-backups.md)
> * [Geowiederherstellung: im Azure-Portal](sql-database-geo-restore-portal.md)
> 
> 

In diesem Artikel wird erläutert, wie Sie Ihre Datenbank mithilfe der Geowiederherstellung auf einem neuen Server wiederherstellen können. Dies kann über PowerShell erfolgen.

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="geo-restore-your-database-into-a-standalone-database"></a>Geowiederherstellung der Datenbank in einer eigenständigen Datenbank
1. Rufen Sie mit dem Cmdlet [Get-AzureRmSqlDatabaseGeoBackup](https://msdn.microsoft.com/library/azure/mt693388\(v=azure.300\).aspx) die georedundante Sicherung der wiederherzustellenden Datenbank ab.
   
        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"
2. Starten Sie mit dem Cmdlet [Restore-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390\(v=azure.300\).aspx) die Wiederherstellung aus der georedundanten Sicherung.
   
        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID -Edition "Standard" -RequestedServiceObjectiveName "S2"

## <a name="geo-restore-your-database-into-an-elastic-database-pool"></a>Geowiederherstellung der Datenbank in einem Pool für elastische Datenbanken
1. Rufen Sie mit dem Cmdlet [Get-AzureRmSqlDatabaseGeoBackup](https://msdn.microsoft.com/library/azure/mt693388\(v=azure.300\).aspx) die georedundante Sicherung der wiederherzustellenden Datenbank ab.
   
        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"
2. Starten Sie mit dem Cmdlet [Restore-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390\(v=azure.300\).aspx) die Wiederherstellung aus der georedundanten Sicherung. Geben Sie den Namen des Pools an, in dem Sie die Datenbank wiederherstellen möchten.
   
        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID –ElasticPoolName "elasticpool01"  

## <a name="next-steps"></a>Nächste Schritte
* Eine Übersicht und verschiedene Szenarien zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).
* Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md).
* Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md).
* Informationen über schnellere Wiederherstellungsoptionen finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md).  
* Informationen zum Verwenden automatisierter Sicherungen für die Archivierung finden Sie unter [Datenbankkopie](sql-database-copy.md).




<!--HONumber=Nov16_HO3-->


