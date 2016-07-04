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
	ms.date="06/17/2016"
	ms.author="sstein"
	ms.workload="sqldb-bcdr"
	ms.topic="article"
	ms.tgt_pltfrm="NA"/>


# Geowiederherstellung einer Azure SQL-Datenbank aus einer georedundanten Sicherung über das Azure-Portal


> [AZURE.SELECTOR]
- [Übersicht](sql-database-geo-restore.md)
- [Azure-Portal](sql-database-geo-restore-portal.md)
- [PowerShell](sql-database-geo-restore-powershell.md)

In diesem Artikel wird erläutert, wie Sie Ihre Datenbank mit der Geowiederherstellung über das Azure-Portal auf einem neuen Server wiederherstellen können.

## Auswählen der wiederherzustellenden Datenbank

Gehen Sie zum Wiederherstellen einer Datenbank im Azure-Portal wie folgt vor:

1.	Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2.  Wählen Sie auf der linken Bildschirmseite **Neu** > **Daten und Speicher** > **SQL-Datenbank** aus.
3.  Wählen Sie als Quelle die Option **Sicherung** und anschließend die georedundante Sicherung aus, die zur Wiederherstellung verwendet werden soll.

    ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-geo-restore-portal/geo-restore.png)

4.  Geben Sie einen Datenbanknamen und einen Server ein, auf dem die Datenbank wiederhergestellt werden soll, und klicken Sie dann auf „Erstellen“.

## Nächste Schritte

- Ausführliche Schritte zum Wiederherstellen einer Azure SQL-Datenbank aus einer georedundanten Sicherung mit dem Azure-Portal finden Sie unter [Geowiederherstellung über das Azure-Portal](sql-database-geo-restore-portal.md).
- Ausführliche Informationen zum Wiederherstellen einer Azure SQL-Datenbank aus einer georedundanten Sicherung finden Sie unter [Geowiederherstellung mithilfe von PowerShell](sql-database-geo-restore.md).
- Eine vollständige Erläuterung der Wiederherstellung nach einem Ausfall finden Sie unter [Wiederherstellen nach einem Ausfall](sql-database-disaster-recovery.md).

## Weitere Ressourcen

- [Szenarien für die Geschäftskontinuität](sql-database-business-continuity-scenarios.md)

<!---HONumber=AcomDC_0622_2016-->