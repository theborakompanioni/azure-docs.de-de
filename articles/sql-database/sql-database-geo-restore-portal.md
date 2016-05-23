<properties
	pageTitle="Wiederherstellen einer Azure SQL-Datenbank aus einer georedundanten Sicherung (Azure-Portal) | Microsoft Azure"
	description="Geowiederherstellung einer Azure SQL-Datenbank aus einer georedundanten Sicherung (Azure-Portal)"
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


# Geowiederherstellung einer Azure SQL-Datenbank aus einer georedundanten Sicherung über das Azure-Portal


> [AZURE.SELECTOR]
- [Azure-Portal](sql-database-geo-restore-portal.md)
- [PowerShell](sql-database-geo-restore-powershell.md)

In diesem Artikel wird erläutert, wie Sie Ihre Datenbank mit der Geowiederherstellung über das Azure-Portal auf einem neuen Server wiederherstellen können.

Die [Geowiederherstellung](sql-database-geo-restore.md) bietet die Möglichkeit der Wiederherstellung einer Datenbank aus einer georedundanten Sicherung, um eine neue Datenbank zu erstellen. Die Datenbank kann auf einem beliebigen Server in jeder Azure-Region erstellt werden. Da die Quelle eine georedundante Sicherung ist, kann mit ihr eine Datenbank selbst dann wiederhergestellt werden, wenn sie aufgrund eines Ausfalls nicht mehr verfügbar ist. Die Geowiederherstellung ist für alle Diensttarife ohne zusätzliche Kosten automatisch aktiviert.

## Auswählen der wiederherzustellenden Datenbank

Gehen Sie zum Wiederherstellen einer Datenbank im Azure-Portal wie folgt vor:

1.	Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2.  Wählen Sie auf der linken Bildschirmseite **Neu** > **Daten und Speicher** > **SQL-Datenbank** aus.
3.  Wählen Sie **Sicherung** als Quelle und dann die georedundante Sicherung aus, die wiederhergestellt werden soll.

    ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-geo-restore-portal/geo-restore.png)

4.  Geben Sie einen Datenbanknamen und einen Server ein, auf dem die Datenbank wiederhergestellt werden soll, und klicken Sie dann auf „Erstellen“.

## Nächste Schritte

- [Herstellen einer Verbindung mit einer Azure SQL-Datenbank mit SQL Server Management Studio und Ausführen einer T-SQL-Beispielabfrage](sql-database-connect-query-ssms.md)
- [Warnungen zur Notfallwiederherstellung](sql-database-disaster-recovery-drills.md)


## Zusätzliche Ressourcen

- [Geografische Wiederherstellung](sql-database-geo-restore.md)
- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [SQL-Datenbankdokumentation](https://azure.microsoft.com/documentation/services/sql-database/)

<!---HONumber=AcomDC_0511_2016-->