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
    ms.workload="sqldb-bcdr" 
    ms.date="06/17/2016"
    ms.author="sstein"/>

# Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt mit PowerShell

> [AZURE.SELECTOR]
- [Übersicht](sql-database-point-in-time-restore.md)
- [Azure-Portal](sql-database-point-in-time-restore-portal.md)
- [PowerShell](sql-database-point-in-time-restore-powershell.md)

In diesem Artikel erfahren Sie, wie Sie auf der [Grundlage automatisierter SQL-Datenbanksicherungen](sql-database-automated-backups.md) mithilfe von PowerShell einen früheren Zustand Ihrer Datenbank wiederherstellen.

[AZURE.INCLUDE [Starten der PowerShell-Sitzung](../../includes/sql-database-powershell.md)]

## Wiederherstellen des Zustands der Datenbank zu einem bestimmten Zeitpunkt als eigenständige Datenbank

1. Rufen Sie die wiederherzustellende Datenbank mit dem Cmdlet [Get-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt603648.aspx) ab.

        $Database = Get-AzureRmSqlDatabase -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Stellen Sie den Zustand der Datenbank zu einem bestimmten Zeitpunkt mit dem Cmdlet [Restore-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390.aspx) wieder her.
    
        Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime UTCDateTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $Database.ResourceID -Edition "Standard" -ServiceObjectiveName "S2"


## Wiederherstellen des Zustands der Datenbank zu einem bestimmten Zeitpunkt in einem Pool für elastische Datenbanken
   
1. Rufen Sie die wiederherzustellende Datenbank mit dem Cmdlet [Get-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt603648.aspx) ab.

        $Database = Get-AzureRmSqlDatabase -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Stellen Sie den Zustand der Datenbank zu einem bestimmten Zeitpunkt mit dem Cmdlet [Restore-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390.aspx) wieder her.
    
        Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime UTCDateTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $Database.ResourceID –ElasticPoolName "elasticpool01"

## Nächste Schritte

- Ausführliche Schritte zur Wiederherstellung des Zustands zu einem bestimmten Zeitpunkt mithilfe des Azure-Portals finden Sie unter [Point-in-Time-Wiederherstellung mithilfe des Azure-Portals](sql-database-point-in-time-restore-portal.md).
- Informationen zur Wiederherstellung des Zustands zu einem bestimmten Zeitpunkt mithilfe der REST-API finden Sie unter [Point-In-Time-Wiederherstellung mithilfe der REST-API](https://msdn.microsoft.com/library/azure/mt163685.aspx).
- Einen Überblick über die Point-in-Time-Wiederherstellung finden Sie unter [Point-In-Time-Wiederherstellung](sql-database-point-in-time-restore.md).
- Eine vollständige Erläuterung der Wiederherstellung nach einem Benutzer- oder Anwendungsfehler finden Sie unter [Wiederherstellen einer Azure-SQL-Datenbank nach einem Benutzerfehler](sql-database-user-error-recovery.md).

## Zusätzliche Ressourcen

- [Szenarios für die Geschäftskontinuität](sql-database-business-continuity-scenarios.md)

<!---HONumber=AcomDC_0622_2016-->