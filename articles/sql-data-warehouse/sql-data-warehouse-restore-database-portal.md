<properties
   pageTitle="Wiederherstellen einer Datenbank in Azure SQL Data Warehouse (Portal) | Microsoft Azure"
   description="Aufgaben im Azure-Portal zum Wiederherstellen einer Live- oder gelöschten Datenbank in Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="elfisher"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/04/2016"
   ms.author="elfish;barbkess;sonyama"/>

# Wiederherstellen einer Datenbank in Azure SQL Data Warehouse (Portal)

> [AZURE.SELECTOR]
- [Übersicht](sql-data-warehouse-restore-database-overview.md)
- [Portal](sql-data-warehouse-restore-database-portal.md)
- [PowerShell](sql-data-warehouse-restore-database-powershell.md)
- [REST](sql-data-warehouse-manage-restore-database-rest-api.md)

Aufgaben in PowerShellzum Wiederherstellen einer Live- oder gelöschten Datenbank in Azure SQL Data Warehouse

Aufgaben in diesem Thema:

- Wiederherstellen einer Livedatenbank
- Wiederherstellen einer gelöschten Datenbank

## Voraussetzungen

Überprüfen Sie Ihre Kapazität der SQL-Datenbank-DTU. SQL Data Warehouse führt die Wiederherstellung so aus, dass eine neue Datenbank auf dem logischen Server erstellt wird. Daher sollten Sie sicherstellen, dass der SQL-Server, auf den Sie wiederherstellen, über ausreichend DTU-Kapazität für die neue Datenbank verfügt. Weitere Informationen zum [Anzeigen und Erhöhen des DTU-Kontingents][] finden Sie in diesem Blogbeitrag.


## Wiederherstellen einer Livedatenbank

So stellen Sie eine Datenbank wieder her:

1. Melden Sie sich beim [Azure-Portal][] an.
2. Wählen Sie auf der linken Bildschirmseite **DURCHSUCHEN** und dann **SQL-Datenbanken** aus.
3. Navigieren Sie zu Ihrer Datenbank, und wählen Sie sie aus.
4. Klicken Sie oben im Blatt für die Datenbank auf **Wiederherstellen**.
5. Geben Sie einen neuen **Datenbanknamen** an, wählen Sie einen **Wiederherstellungspunkt**, und klicken Sie anschließend auf **Erstellen**.
6. Der Datenbank-Wiederherstellungsvorgang wird gestartet und kann mithilfe von **BENACHRICHTIGUNGEN** überwacht werden.


## Wiederherstellen einer gelöschten Datenbank

So stellen Sie eine gelöschte Datenbank wieder her

1. Melden Sie sich beim [Azure-Portal][] an.
2. Wählen Sie auf der linken Bildschirmseite **DURCHSUCHEN** und dann **SQL Server** aus.
3. Navigieren Sie zu Ihrem Server, und wählen Sie ihn aus.
4. Scrollen Sie auf dem Blatt für Ihren Server nach unten zu „Vorgänge“, und klicken Sie auf die Kachel **Gelöschte Datenbanken**.
5. Klicken Sie auf die Datenbank, die Sie wiederherstellen möchten.
5. Geben Sie einen neuen **Datenbanknamen** an, und klicken Sie auf **Erstellen**.
6. Der Datenbank-Wiederherstellungsvorgang wird gestartet und kann mithilfe von **BENACHRICHTIGUNGEN** überwacht werden.


## Nächste Schritte
Weitere Informationen finden Sie unter [Übersicht über die Geschäftskontinuität in Azure SQL-Datenbank][] und [Verwaltungsübersicht][].

<!--Image references-->

<!--Article references-->
[Übersicht über die Geschäftskontinuität in Azure SQL-Datenbank]: sql-database-business-continuity.md
[How to install and configure Azure PowerShell]: powershell-install-configure.md
[Verwaltungsübersicht]: sql-data-warehouse-overview-manage.md

<!--MSDN references-->

<!--Blog references-->
[Anzeigen und Erhöhen des DTU-Kontingents]: https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/

<!--Other Web references-->
[Azure-Portal]: https://portal.azure.com/

<!---HONumber=AcomDC_0608_2016-->