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
	ms.date="06/17/2016"
	ms.author="sstein"
	ms.workload="sqldb-bcdr"
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

- Ausführliche Schritte zur Wiederherstellung des Zustands zu einem bestimmten Zeitpunkt mithilfe von PowerShell finden Sie unter [Point-in-Time-Wiederherstellung mithilfe von PowerShell](sql-database-point-in-time-restore-powershell.md).
- Informationen zur Wiederherstellung des Zustands zu einem bestimmten Zeitpunkt mithilfe der REST-API finden Sie unter [Point-In-Time-Wiederherstellung mithilfe der REST-API](https://msdn.microsoft.com/library/azure/mt163685.aspx).
- Einen Überblick über die Point-in-Time-Wiederherstellung finden Sie unter [Point-In-Time-Wiederherstellung](sql-database-point-in-time-restore.md).
- Eine vollständige Erläuterung der Wiederherstellung nach einem Benutzer- oder Anwendungsfehler finden Sie unter [Wiederherstellen einer Azure-SQL-Datenbank nach einem Benutzerfehler](sql-database-user-error-recovery.md).

## Zusätzliche Ressourcen

- [Szenarios für die Geschäftskontinuität](sql-database-business-continuity-scenarios.md)

<!---HONumber=AcomDC_0622_2016-->