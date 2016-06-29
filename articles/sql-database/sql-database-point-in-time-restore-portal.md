<properties
	pageTitle="Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt (Azure-Portal) | Microsoft Azure"
	description="Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt"
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


# Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt über das Azure-Portal


> [AZURE.SELECTOR]
- [Übersicht](sql-database-point-in-time-restore.md)
- [Azure-Portal](sql-database-point-in-time-restore-portal.md)
- [PowerShell](sql-database-point-in-time-restore-powershell.md)

In diesem Artikel erfahren Sie, wie Sie auf der [Grundlage automatisierter SQL-Datenbanksicherungen](sql-database-automated-backups.md) über das Azure-Portal einen früheren Zustand Ihrer Datenbank wiederherstellen.

## Auswählen einer Datenbank für die Wiederherstellung des Zustands zu einem früheren Zeitpunkt

Gehen Sie zum Wiederherstellen einer Datenbank im Azure-Portal wie folgt vor:

1.	Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2.  Wählen Sie auf der linken Bildschirmseite **DURCHSUCHEN** > **SQL-Datenbanken** aus.
3.  Navigieren Sie zu der wiederherzustellenden Datenbank, und wählen Sie sie aus.
4.  Wählen Sie oben auf dem Blatt der Datenbank **Wiederherstellen** aus:

    ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-point-in-time-restore-portal/restore.png)

5.  Geben Sie einen Datenbanknamen und einen Zeitpunkt an, und klicken Sie dann auf „OK“:

    ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-point-in-time-restore-portal/restore-details.png)


## Nächste Schritte

- [Abschließen der wiederhergestellten Azure SQL-Datenbank](sql-database-recovered-finalize.md)
- [Point-in-Time-Wiederherstellung](sql-database-point-in-time-restore.md)
- [Point-in-time restore using the REST API](https://msdn.microsoft.com/library/azure/mt163685.aspx) (Point-in-Time-Wiederherstellung mithilfe der REST-API)
- [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md)

## Zusätzliche Ressourcen

- [Wiederherstellen einer gelöschten Datenbank](sql-database-restore-deleted-database.md)
- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [Geografische Wiederherstellung](sql-database-geo-restore.md)
- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md)

<!---HONumber=AcomDC_0615_2016-->