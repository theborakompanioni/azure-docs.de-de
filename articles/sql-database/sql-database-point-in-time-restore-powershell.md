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
    ms.date="05/10/2016"
    ms.author="sstein"/>

# Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt mit PowerShell

> [AZURE.SELECTOR]
- [Azure-Portal](sql-database-point-in-time-restore-portal.md)
- [PowerShell](sql-database-point-in-time-restore-powershell.md)

In diesem Artikel wird erläutert, wie Sie den Zustand Ihrer Datenbank zu einem früheren Zeitpunkt mithilfe von PowerShell wiederherstellen können.

Die [**Point-in-Time-Wiederherstellung**](sql-database-point-in-time-restore.md) ist eine Self-Service-Funktion zum Wiederherstellen einer Datenbank aus den automatischen Sicherungen, die wir für alle Datenbanken zu jedem Zeitpunkt innerhalb der Aufbewahrungsdauer Ihrer Datenbank erstellen. Weitere Informationen zu automatischen Sicherungen und zur Datenbankaufbewahrungsdauer finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).

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

- [Abschließen der wiederhergestellten Azure SQL-Datenbank](sql-database-recovered-finalize.md)
- [Herstellen einer Verbindung mit einer Azure SQL-Datenbank mit SQL Server Management Studio und Ausführen einer T-SQL-Beispielabfrage](sql-database-connect-query-ssms.md)


## Zusätzliche Ressourcen

- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [SQL-Datenbankdokumentation](https://azure.microsoft.com/documentation/services/sql-database/)

<!---HONumber=AcomDC_0511_2016-->