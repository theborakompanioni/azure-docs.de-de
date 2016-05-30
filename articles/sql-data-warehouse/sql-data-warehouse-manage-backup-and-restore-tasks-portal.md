<properties
   pageTitle="Sichern und Wiederherstellen in Azure SQL Data Warehouse (Portal) | Microsoft Azure"
   description="Aufgaben von Azure-Portal zum Wiederherstellen einer Livedatenbank, einer gelöschten Datenbank oder einer Datenbank, auf die nicht zugegriffen werden kann, in Azure SQL Data Warehouse"
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
   ms.date="05/05/2016"
   ms.author="elfish;barbkess;sonyama"/>

# Sichern und Wiederherstellen einer Datenbank in Azure SQL Data Warehouse (Portal)

> [AZURE.SELECTOR]
- [Übersicht](sql-data-warehouse-overview-manage-backup-and-restore.md)
- [Portal](sql-data-warehouse-manage-backup-and-restore-tasks-portal.md)
- [PowerShell](sql-data-warehouse-manage-backup-and-restore-tasks-powershell.md)
- [REST](sql-data-warehouse-manage-backup-and-restore-tasks-rest-api.md)

Eine Datenbank in SQL Data Warehouse mithilfe des Azure-Portals sichern und wiederherstellen.

Aufgaben in diesem Thema:

- Wiederherstellen einer Livedatenbank
- Wiederherstellen einer gelöschten Datenbank
- Wiederherstellen einer Datenbank, auf die nicht zugegriffen werden kann, von einer anderen geografischen Azure-Region aus

[AZURE.INCLUDE [SQL Data Warehouse-Aufbewahrungsrichtlinie für die Sicherung](../../includes/sql-data-warehouse-backup-retention-policy.md)]


## Voraussetzungen

Überprüfen Sie Ihre Kapazität der SQL-Datenbank-DTU. SQL Data Warehouse führt die Wiederherstellung so aus, dass eine neue Datenbank auf dem logischen Server erstellt wird. Daher sollten Sie sicherstellen, dass der SQL-Server, auf den Sie wiederherstellen, über ausreichend DTU-Kapazität für die neue Datenbank verfügt. Weitere Informationen zum Anzeigen und Erhöhen des DTU-Kontingents finden Sie in [diesem Blogbeitrag][].

## Wiederherstellen einer Livedatenbank

So stellen Sie eine Datenbank aus einer Momentaufnahme wieder her:

1. Melden Sie sich beim [Azure-Portal][] an.
2. Wählen Sie auf der linken Bildschirmseite **DURCHSUCHEN** und dann **SQL-Datenbanken** aus.
3. Navigieren Sie zu Ihrer Datenbank, und wählen Sie sie aus.
4. Klicken Sie oben im Blatt für die Datenbank auf **Wiederherstellen**.
5. Geben Sie einen neuen **Datenbanknamen** an, wählen Sie einen **Wiederherstellungspunkt** aus, und klicken Sie anschließend auf **Erstellen**.
6. Der Datenbank-Wiederherstellungsvorgang beginnt und kann mithilfe von **BENACHRICHTIGUNGEN** überwacht werden.


## Wiederherstellen einer gelöschten Datenbank

So stellen Sie eine gelöschte Datenbank wieder her

1. Melden Sie sich beim [Azure-Portal][] an.
2. Wählen Sie auf der linken Bildschirmseite **DURCHSUCHEN** und dann **SQL Server** aus.
3. Navigieren Sie zu Ihrem Server, und wählen Sie ihn aus.
4. Scrollen Sie auf dem Blatt für Ihren Server nach unten zu „Vorgänge“, und klicken Sie auf die Kachel **Gelöschte Datenbanken**.
5. Klicken Sie auf die Datenbank, die Sie wiederherstellen möchten.
5. Geben Sie einen neuen **Datenbanknamen** an, und klicken Sie auf **Erstellen**.
6. Der Datenbank-Wiederherstellungsvorgang beginnt und kann mithilfe von **BENACHRICHTIGUNGEN** überwacht werden.


## Ausführen einer Geowiederherstellung für ein Data Warehouse aus einer georedundanten Sicherung

So führen Sie eine Geowiederherstellung aus:

1. Melden Sie sich beim [Azure-Portal][] an.
2. Wählen Sie auf der linken Bildschirmseite **+NEU**, dann **Daten und Speicher** und anschließend **SQL Data Warehouse** aus.
3. Wählen Sie **SICHERUNG** als Quelle und anschließend die georedundante Sicherung aus, von der aus wiederhergestellt werden soll.
4. Geben Sie die restlichen Datenbankeigenschaften an, und klicken Sie auf **Erstellen**.
5. Der Datenbank-Wiederherstellungsvorgang beginnt und kann mithilfe von **BENACHRICHTIGUNGEN** überwacht werden.

## Nächste Schritte
Informationen zu den Geschäftskontinuitätsfunktionen von Azure SQL-Datenbank-Editionen finden Sie in der [Azure SQL-Datenbank-Übersicht zur Geschäftskontinuität][].

<!--Image references-->

<!--Article references-->
[Azure SQL-Datenbank-Übersicht zur Geschäftskontinuität]: sql-database-business-continuity.md
[Finalize a recovered database]: sql-database-recovered-finalize.md
[How to install and configure Azure PowerShell]: powershell-install-configure.md

<!--MSDN references-->

<!--Blog references-->
[diesem Blogbeitrag]: https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/

<!--Other Web references-->
[Azure-Portal]: https://portal.azure.com/

<!---HONumber=AcomDC_0518_2016-->