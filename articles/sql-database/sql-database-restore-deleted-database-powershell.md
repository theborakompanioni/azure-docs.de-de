<properties
	pageTitle="Wiederherstellen einer gelöschten Azure SQL-Datenbank (PowerShell) | Microsoft Azure"
	description="Wiederherstellen einer gelöschten Azure SQL-Datenbank (PowerShell)"
	services="sql-database"
	documentationCenter=""
	authors="stevestein"
	manager="jhubbard"
	editor=""/>

<tags
	ms.service="sql-database"
	ms.devlang="NA"
	ms.date="06/09/2016"
	ms.author="sstein"
	ms.workload="data-management"
	ms.topic="article"
	ms.tgt_pltfrm="NA"/>


# Wiederherstellen einer gelöschten Azure SQL-Datenbank mit PowerShell

> [AZURE.SELECTOR]
- [Übersicht](sql-database-restore-deleted-database.md)
- [Azure-Portal](sql-database-restore-deleted-database-portal.md)
- [PowerShell](sql-database-restore-deleted-database-powershell.md)

[AZURE.INCLUDE [Starten der PowerShell-Sitzung](../../includes/sql-database-powershell.md)]


## Wiederherstellen der gelöschten Datenbank in einer eigenständigen Datenbank

1. Rufen Sie mit dem Cmdlet [Get-AzureRmSqlDeletedDatabaseBackup](https://msdn.microsoft.com/library/azure/mt693387.aspx) die Sicherung der gelöschten Datenbank ab, die Sie wiederherstellen möchten.

        $DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Starten Sie mit dem Cmdlet [Restore-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390.aspx) die Wiederherstellung auf der Grundlage der Sicherung der gelöschten Datenbank.
    
        Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $DeletedDatabase.ResourceID -Edition "Standard" -ServiceObjectiveName "S2"

## Wiederherstellen der gelöschten Datenbank in einem Pool für elastische Datenbanken

1. Rufen Sie mit dem Cmdlet [Get-AzureRmSqlDeletedDatabaseBackup](https://msdn.microsoft.com/library/azure/mt693387.aspx) die Sicherung der gelöschten Datenbank ab, die Sie wiederherstellen möchten.

        $DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Starten Sie mit dem Cmdlet [Restore-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390.aspx) die Wiederherstellung auf der Grundlage der Sicherung der gelöschten Datenbank.
    
        Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $DeletedDatabase.ResourceID –ElasticPoolName "elasticpool01" 

## Nächste Schritte

- [Abschließen der wiederhergestellten Azure SQL-Datenbank](sql-database-recovered-finalize.md)
- [Wiederherstellen einer gelöschten Datenbank](sql-database-restore-deleted-database.md)
- [Wiederherstellen einer gelöschten Azure SQL-Datenbank im Azure-Portal](sql-database-restore-deleted-database-portal.md)
- [Restore a deleted database using the REST API](https://msdn.microsoft.com/library/azure/mt163685.aspx) (Wiederherstellen einer gelöschten Datenbank mithilfe der REST-API)
- [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md)

## Zusätzliche Ressourcen

- [Point-in-Time-Wiederherstellung](sql-database-point-in-time-restore.md)
- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [Geografische Wiederherstellung](sql-database-geo-restore.md)
- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md)

<!---HONumber=AcomDC_0615_2016-->