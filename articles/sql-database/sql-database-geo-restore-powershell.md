<properties 
    pageTitle="Wiederherstellen einer Azure SQL-Datenbank aus einer georedundanten Sicherung (PowerShell) | Microsoft Azure" 
    description="Wiederherstellen einer Azure SQL-Datenbank aus einer georedundanten Sicherung auf einem neuen Server" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="sqldb-bcdr" 
    ms.date="06/17/2016"
    ms.author="sstein"/>

# Wiederherstellen einer Azure SQL-Datenbank aus einer georedundanten Sicherung mit PowerShell


> [AZURE.SELECTOR]
- [Übersicht](sql-database-geo-restore.md)
- [Azure-Portal](sql-database-geo-restore-portal.md)
- [PowerShell](sql-database-geo-restore-powershell.md)

In diesem Artikel wird erläutert, wie Sie Ihre Datenbank mit der Geowiederherstellung mithilfe von PowerShell auf einem neuen Server wiederherstellen können.

[AZURE.INCLUDE [Starten der PowerShell-Sitzung](../../includes/sql-database-powershell.md)]

## Geowiederherstellung der Datenbank in einer eigenständigen Datenbank

1. Rufen Sie mit dem Cmdlet [Get-AzureRmSqlDatabaseGeoBackup](https://msdn.microsoft.com/library/azure/mt693388.aspx) die georedundante Sicherung der wiederherzustellenden Datenbank ab.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Starten Sie mit dem Cmdlet [Restore-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390.aspx) die Wiederherstellung auf der Grundlage der georedundanten Sicherung.
    
        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID -Edition "Standard" -RequestedServiceObjectiveName "S2"


## Geowiederherstellung der Datenbank in einem Pool für elastische Datenbanken

1. Rufen Sie mit dem Cmdlet [Get-AzureRmSqlDatabaseGeoBackup](https://msdn.microsoft.com/library/azure/mt693388.aspx) die georedundante Sicherung der wiederherzustellenden Datenbank ab.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Starten Sie mit dem Cmdlet [Restore-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390.aspx) die Wiederherstellung auf der Grundlage der georedundanten Sicherung. Geben Sie den Namen des Pools an, in dem Sie die Datenbank wiederherstellen möchten.
    
        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID –ElasticPoolName "elasticpool01"  

## Nächste Schritte

- Ausführliche Schritte zum Wiederherstellen einer Azure SQL-Datenbank aus einer georedundanten Sicherung mit dem Azure-Portal finden Sie unter [Geowiederherstellung über das Azure-Portal](sql-database-geo-restore-portal.md).
- Ausführliche Informationen zum Wiederherstellen einer Azure SQL-Datenbank aus einer georedundanten Sicherung finden Sie unter [Geowiederherstellung mithilfe von PowerShell](sql-database-geo-restore.md).
- Eine vollständige Erläuterung der Wiederherstellung nach einem Ausfall finden Sie unter [Wiederherstellen nach einem Ausfall](sql-database-disaster-recovery.md).


## Zusätzliche Ressourcen

- [Szenarien für die Geschäftskontinuität](sql-database-business-continuity-scenarios.md)

<!---HONumber=AcomDC_0622_2016-->