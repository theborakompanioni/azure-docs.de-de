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
- [Übersicht](sql-database-recovery-using-backups.md)
- [Wiederherstellen einer gelöschten Datenbank: mit PowerShell](sql-database-restore-deleted-database-powershell.md)

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

- Eine Übersicht zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).
- Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md).
- Informationen über Entwurfs- und Wiederherstellungsszenarien für die Geschäftskontinuität finden Sie unter [Geschäftskontinuitätsszenarien](sql-database-business-continuity-scenarios.md).
- Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md).
- Informationen über schnellere Wiederherstellungsoptionen finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md).
- Informationen zum Verwenden automatisierter Sicherungen für die Archivierung finden Sie unter [Datenbankkopie](sql-database-copy.md).

<!---HONumber=AcomDC_0629_2016-->