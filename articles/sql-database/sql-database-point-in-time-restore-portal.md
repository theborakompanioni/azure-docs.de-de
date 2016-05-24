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
	ms.date="05/10/2016"
	ms.author="sstein"
	ms.workload="data-management"
	ms.topic="article"
	ms.tgt_pltfrm="NA"/>


# Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt über das Azure-Portal


> [AZURE.SELECTOR]
- [Azure-Portal](sql-database-point-in-time-restore-portal.md)
- [PowerShell](sql-database-point-in-time-restore-powershell.md)

In diesem Artikel wird erläutert, wie Sie den Zustand Ihrer Datenbank zu einem früheren Zeitpunkt über das Azure-Portal wiederherstellen können.

Die [**Point-in-Time-Wiederherstellung**](sql-database-point-in-time-restore.md) ist eine Self-Service-Funktion zum Wiederherstellen einer Datenbank aus den automatischen Sicherungen, die wir für alle Datenbanken zu jedem Zeitpunkt innerhalb der Aufbewahrungsdauer Ihrer Datenbank erstellen. Weitere Informationen zu automatischen Sicherungen und zur Datenbankaufbewahrungsdauer finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).

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
- [Herstellen einer Verbindung mit einer Azure SQL-Datenbank mit SQL Server Management Studio und Ausführen einer T-SQL-Beispielabfrage](sql-database-connect-query-ssms.md)



## Zusätzliche Ressourcen

- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [SQL-Datenbankdokumentation](https://azure.microsoft.com/documentation/services/sql-database/)

<!---HONumber=AcomDC_0511_2016-->