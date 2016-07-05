<properties
	pageTitle="Wiederherstellen einer gelöschten Azure SQL-Datenbank (Azure-Portal) | Microsoft Azure"
	description="Wiederherstellen einer gelöschten Azure SQL-Datenbank (Azure-Portal)"
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
	ms.workload="sqldb-bcdr"
	ms.topic="article"
	ms.tgt_pltfrm="NA"/>


# Wiederherstellen einer gelöschten Azure SQL-Datenbank im Azure-Portal

> [AZURE.SELECTOR]
- [Übersicht](sql-database-restore-deleted-database.md)
- [Azure-Portal](sql-database-restore-deleted-database-portal.md)
- [PowerShell](sql-database-restore-deleted-database-powershell.md)


## Auswählen der wiederherzustellenden Datenbank 

Gehen Sie zum Wiederherstellen einer Datenbank im Azure-Portal wie folgt vor:

1.	Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2.  Wählen Sie auf der linken Bildschirmseite **DURCHSUCHEN** > **SQL Server** aus.
3.  Navigieren Sie zu dem Server mit der gelöschten Datenbank, die Sie wiederherstellen möchten, und wählen Sie den Server aus.
4.  Scrollen Sie auf dem Serverblatt nach unten zum Bereich **Vorgänge**, und wählen Sie **Gelöschte Datenbanken** aus: ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-restore-deleted-database-portal/restore-deleted-trashbin.png)
5.  Klicken Sie auf die Datenbank, die Sie wiederherstellen möchten.
6.  Geben Sie einen Datenbanknamen an, und klicken Sie auf „OK“.

    ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-restore-deleted-database-portal/restore-deleted.png)

## Nächste Schritte

- Ausführliche Schritte zum Wiederherstellen einer gelöschten Datenbank mithilfe von PowerShell finden Sie unter [Wiederherstellen einer gelöschten Azure SQL-Datenbank mit PowerShell](sql-database-restore-deleted-database-powershell.md).
- Informationen zum Wiederherstellen einer gelöschten Datenbank finden Sie unter [Wiederherstellen einer gelöschten Datenbank mithilfe der REST-API](https://msdn.microsoft.com/library/azure/mt163685.aspx).
- Detaillierte Informationen zum Wiederherstellen einer gelöschten Datenbank finden Sie unter [Wiederherstellen einer gelöschten Datenbank](sql-database-restore-deleted-database.md).
- Detaillierte Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md).

## Zusätzliche Ressourcen

- [Point-in-Time-Wiederherstellung](sql-database-point-in-time-restore.md)
- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [Geografische Wiederherstellung](sql-database-geo-restore.md)
- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md)

<!---HONumber=AcomDC_0622_2016-->