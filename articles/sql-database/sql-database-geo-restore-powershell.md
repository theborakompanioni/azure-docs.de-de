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
    ms.workload="data-management" 
    ms.date="05/10/2016"
    ms.author="sstein"/>

# Wiederherstellen einer Azure SQL-Datenbank aus einer georedundanten Sicherung mit PowerShell

> [AZURE.SELECTOR]
- [Azure-Portal](sql-database-geo-restore-portal.md)
- [PowerShell](sql-database-geo-restore-powershell.md)

In diesem Artikel wird erläutert, wie Sie Ihre Datenbank mit der Geowiederherstellung mithilfe von PowerShell auf einem neuen Server wiederherstellen können.

Die [Geowiederherstellung](sql-database-geo-restore.md) bietet die Möglichkeit der Wiederherstellung einer Datenbank aus einer georedundanten Sicherung, um eine neue Datenbank zu erstellen. Die Datenbank kann auf einem beliebigen Server in jeder Azure-Region erstellt werden. Da die Quelle eine georedundante Sicherung ist, kann mit ihr eine Datenbank selbst dann wiederhergestellt werden, wenn sie aufgrund eines Ausfalls nicht mehr verfügbar ist. Die Geowiederherstellung ist für alle Diensttarife ohne zusätzliche Kosten automatisch aktiviert.

[AZURE.INCLUDE [Starten der PowerShell-Sitzung](../../includes/sql-database-powershell.md)]

## Geowiederherstellung der Datenbank in einer eigenständigen Datenbank

1. Rufen Sie die georedundante Sicherung der wiederherzustellenden Datenbank mit dem Cmdlet [Get-AzureRmSqlDatabaseGeoBackup](https://msdn.microsoft.com/library/azure/mt693388.aspx) ab.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Starten Sie die Wiederherstellung aus der georedundanten Sicherung mit dem Cmdlet [Restore-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390.aspx).
    
        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID -Edition "Standard" -RequestedServiceObjectiveName "S2"


## Geowiederherstellung der Datenbank in einem Pool für elastische Datenbanken

1. Rufen Sie die georedundante Sicherung der wiederherzustellenden Datenbank mit dem Cmdlet [Get-AzureRmSqlDatabaseGeoBackup](https://msdn.microsoft.com/library/azure/mt693388.aspx) ab.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Starten Sie die Wiederherstellung aus der georedundanten Sicherung mit dem Cmdlet [Restore-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390.aspx). Geben Sie den Namen des Pools an, in dem Sie die Datenbank wiederherstellen möchten.
    
        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID –ElasticPoolName "elasticpool01"  

## Nächste Schritte

- [Abschließen der wiederhergestellten Azure SQL-Datenbank](sql-database-recovered-finalize.md)
- [Herstellen einer Verbindung mit einer Azure SQL-Datenbank mit SQL Server Management Studio und Ausführen einer T-SQL-Beispielabfrage](sql-database-connect-query-ssms.md)


## Zusätzliche Ressourcen

- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [SQL-Datenbankdokumentation](https://azure.microsoft.com/documentation/services/sql-database/)

<!---HONumber=AcomDC_0511_2016-->