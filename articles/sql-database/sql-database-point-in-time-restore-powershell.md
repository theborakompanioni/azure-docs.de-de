<properties 
    pageTitle="Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt (PowerShell) | Microsoft Azure" 
    description="Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt" 
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
    ms.workload="data-management" 
    ms.date="06/09/2016"
    ms.author="sstein"/>

# Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt mit PowerShell

In diesem Artikel erfahren Sie, wie Sie auf der [Grundlage automatisierter SQL-Datenbanksicherungen](sql-database-automated-backups.md) mithilfe von PowerShell einen früheren Zustand Ihrer Datenbank wiederherstellen.

[AZURE.INCLUDE [Starten der PowerShell-Sitzung](../../includes/sql-database-powershell.md)]

## Wiederherstellen des Zustands der Datenbank zu einem bestimmten Zeitpunkt als eigenständige Datenbank

1. Rufen Sie mit dem Cmdlet [Get-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt603648.aspx) die wiederherzustellende Datenbank ab.

        $Database = Get-AzureRmSqlDatabase -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Stellen Sie mit dem Cmdlet [Restore-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390.aspx) den Zustand der Datenbank zu einem bestimmten Zeitpunkt wieder her.
    
        Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime UTCDateTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $Database.ResourceID -Edition "Standard" -ServiceObjectiveName "S2"


## Wiederherstellen des Zustands der Datenbank zu einem bestimmten Zeitpunkt in einem Pool für elastische Datenbanken
   
1. Rufen Sie mit dem Cmdlet [Get-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt603648.aspx) die wiederherzustellende Datenbank ab.

        $Database = Get-AzureRmSqlDatabase -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Stellen Sie mit dem Cmdlet [Restore-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390.aspx) den Zustand der Datenbank zu einem bestimmten Zeitpunkt wieder her.
    
        Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime UTCDateTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $Database.ResourceID –ElasticPoolName "elasticpool01"

## Nächste Schritte

- [Abschließen der wiederhergestellten Azure SQL-Datenbank](sql-database-recovered-finalize.md)
- [Point-in-Time-Wiederherstellung](sql-database-point-in-time-restore.md)
- [Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt über das Azure-Portal](sql-database-point-in-time-restore-portal.md)
- [Point-in-time restore using the REST API](https://msdn.microsoft.com/library/azure/mt163685.aspx) (Point-in-Time-Wiederherstellung mithilfe der REST-API)
- [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md)

## Weitere Ressourcen

- [Wiederherstellen einer gelöschten Datenbank](sql-database-restore-deleted-database.md)
- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [Geografische Wiederherstellung](sql-database-geo-restore.md)
- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md)

<!---HONumber=AcomDC_0615_2016-->