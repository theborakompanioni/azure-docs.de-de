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
	ms.date="05/10/2016"
	ms.author="sstein"
	ms.workload="data-management"
	ms.topic="article"
	ms.tgt_pltfrm="NA"/>


# Wiederherstellen einer gelöschten Azure SQL-Datenbank im Azure-Portal


> [AZURE.SELECTOR]
- [Azure-Portal](sql-database-restore-deleted-database-portal.md)
- [PowerShell](sql-database-restore-deleted-database-powershell.md)

In diesem Artikel wird erläutert, wie Sie eine gelöschte Azure SQL-Datenbank wiederherstellen.

Falls eine Datenbank gelöscht wird, bietet Ihnen die Azure-SQL-Datenbank die Möglichkeit, die gelöschte Datenbank zum Zeitpunkt des Löschens wiederherzustellen. Die Azure-SQL-Datenbank speichert die gelöschte Datenbanksicherung während der Aufbewahrungsdauer der Datenbank.

Die Aufbewahrungsdauer einer gelöschten Datenbank wird anhand der Dienstebene der Datenbank bestimmt, während sie vorhanden war, oder der Anzahl der Tage, während denen die Datenbank vorhanden ist, je nachdem, welcher Wert kleiner ist. Weitere Informationen zur Datenbankaufbewahrungsdauer finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).

## Auswählen der wiederherzustellenden Datenbank 

Gehen Sie zum Wiederherstellen einer Datenbank im Azure-Portal wie folgt vor:

1.	Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2.  Wählen Sie auf der linken Bildschirmseite **DURCHSUCHEN** > **SQL Server** aus.
3.  Navigieren Sie zu dem Server mit der gelöschten Datenbank, die Sie wiederherstellen möchten, und wählen Sie den Server aus.
4.  Scrollen Sie nach unten zum Bereich **Vorgänge** des Serverblatts, und wählen Sie **Gelöschte Datenbanken** aus: ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-restore-deleted-database-portal/restore-deleted-trashbin.png)
5.  Klicken Sie auf die Datenbank, die Sie wiederherstellen möchten.
6.  Geben Sie einen Datenbanknamen an, und klicken Sie auf „OK“.

    ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-restore-deleted-database-portal/restore-deleted.png)

## Nächste Schritte

- [Abschließen der wiederhergestellten Azure SQL-Datenbank](sql-database-recovered-finalize.md)
- [Herstellen einer Verbindung mit einer Azure SQL-Datenbank mit SQL Server Management Studio und Ausführen einer T-SQL-Beispielabfrage](sql-database-connect-query-ssms.md)



## Zusätzliche Ressourcen

- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [SQL-Datenbankdokumentation](https://azure.microsoft.com/documentation/services/sql-database/)

<!---HONumber=AcomDC_0511_2016-->