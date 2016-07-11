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
- [Übersicht](sql-database-recovery-using-backups.md)
- [Geowiederherstellung: im Azure-Portal](sql-database-geo-restore-portal.md)

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

- Eine Übersicht zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).
- Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md).
- Informationen über Entwurfs- und Wiederherstellungsszenarien für die Geschäftskontinuität finden Sie unter [Geschäftskontinuitätsszenarien](sql-database-business-continuity-scenarios.md).
- Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md).
- Informationen über schnellere Wiederherstellungsoptionen finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md).
- Informationen zum Verwenden automatisierter Sicherungen für die Archivierung finden Sie unter [Datenbankkopie](sql-database-copy.md).

<!---HONumber=AcomDC_0629_2016-->