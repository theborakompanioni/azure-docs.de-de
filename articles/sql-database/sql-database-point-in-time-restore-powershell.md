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
    ms.workload="NA"
    ms.date="07/17/2016"
    ms.author="sstein"/>

# Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt mit PowerShell

> [AZURE.SELECTOR]
- [Übersicht](sql-database-recovery-using-backups.md)
- [Point-in-Time-Wiederherstellung: im Azure-Portal](sql-database-point-in-time-restore-portal.md)

In diesem Artikel erfahren Sie, wie Sie aus [automatisierten SQL-Datenbanksicherungen](sql-database-automated-backups.md) einen früheren Zustand Ihrer Datenbank wiederherstellen. Hierfür können Sie PowerShell verwenden.

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

- Eine Übersicht und verschiedene Szenarien zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).
- Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md).
- Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md).
- Informationen über schnellere Wiederherstellungsoptionen finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md).
- Informationen zum Verwenden automatisierter Sicherungen für die Archivierung finden Sie unter [Datenbankkopie](sql-database-copy.md).

<!---HONumber=AcomDC_0803_2016-->