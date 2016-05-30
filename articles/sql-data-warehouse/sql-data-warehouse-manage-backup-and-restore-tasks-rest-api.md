<properties
   pageTitle="Sichern und Wiederherstellen in Azure SQL Data Warehouse (REST) | Microsoft Azure"
   description="Aufgaben von REST API zum Wiederherstellen einer Livedatenbank, einer gelöschten Datenbank oder einer Datenbank, auf die nicht zugegriffen werden kann, in Azure SQL Data Warehouse"
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

# Sichern und Wiederherstellen einer Datenbank in Azure SQL Data Warehouse (REST)

> [AZURE.SELECTOR]
- [Übersicht](sql-data-warehouse-overview-manage-backup-and-restore.md)
- [Portal](sql-data-warehouse-manage-backup-and-restore-tasks-portal.md)
- [PowerShell](sql-data-warehouse-manage-backup-and-restore-tasks-powershell.md)
- [REST](sql-data-warehouse-manage-backup-and-restore-tasks-rest-api.md)

Eine Datenbank in SQL Data Warehouse mithilfe von REST-APIs sichern und wiederherstellen.

Aufgaben in diesem Thema:

- Wiederherstellen einer Livedatenbank
- Wiederherstellen einer gelöschten Datenbank
- Wiederherstellen einer Datenbank, auf die nicht zugegriffen werden kann, von einer anderen geografischen Azure-Region aus

[AZURE.INCLUDE [SQL Data Warehouse-Aufbewahrungsrichtlinie für die Sicherung](../../includes/sql-data-warehouse-backup-retention-policy.md)]

## Voraussetzungen

**Überprüfen Sie Ihre Kapazität der SQL-Datenbank-DTU.** SQL Data Warehouse führt die Wiederherstellung so aus, dass eine neue Datenbank auf dem logischen Server erstellt wird. Daher sollten Sie sicherstellen, dass der SQL-Server, auf den Sie wiederherstellen, über ausreichend DTU-Kapazität für die neue Datenbank verfügt. Weitere Informationen zum Anzeigen und Erhöhen des DTU-Kontingents finden Sie in [diesem Blogbeitrag][].

## Wiederherstellen einer Livedatenbank

So stellen Sie eine Datenbank wieder her:

1. Rufen Sie die Liste mit den Wiederherstellungspunkten für die Datenbank ab, indem Sie den Get Database Restore Points-Vorgang verwenden.
2. Beginnen Sie die Wiederherstellung mit dem Vorgang [Datenbankwiederherstellungsanforderung erstellen][].
3. Verfolgen Sie den Status der Wiederherstellung mithilfe des Vorgangs [Datenbank-Betriebsstatus][] nach.

>[AZURE.NOTE] Nachdem die Wiederherstellung abgeschlossen ist, können Sie Ihre wiederhergestellte Datenbank konfigurieren. Befolgen Sie hierzu die Anleitung [Abschließen der wiederhergestellten Azure SQL-Datenbank][].

## Wiederherstellen einer gelöschten Datenbank

So stellen Sie eine gelöschte Datenbank wieder her

1.	Listen Sie alle gelöschten wiederherstellbaren Datenbanken mithilfe des Vorgangs [Wiederherstellbare gelöschte Datenbanken auflisten][] auf.
2.	Rufen Sie die Details der gelöschten Datenbank, die wiederhergestellt werden soll, mithilfe des Vorgangs [Wiederherstellbare gelöschte Datenbank abrufen][] ab.
3.	Beginnen Sie die Wiederherstellung mit dem Vorgang [Datenbankwiederherstellungsanforderung erstellen][].
4.	Verfolgen Sie den Status der Wiederherstellung mithilfe des Vorgangs [Datenbank-Betriebsstatus][] nach.

>[AZURE.NOTE] Nachdem die Wiederherstellung abgeschlossen ist, können Sie Ihre wiederhergestellte Datenbank konfigurieren. Befolgen Sie hierzu die Anleitung [Abschließen der wiederhergestellten Azure SQL-Datenbank][].

## Ausführen einer Geowiederherstellung für ein Data Warehouse aus einer georedundanten Sicherung

So führen Sie eine Geowiederherstellung aus:

1. Rufen Sie die Liste der wiederherstellbaren Datenbanken mithilfe des Vorgangs [List Recoverable Databases][] ab.
2. Rufen Sie die Datenbank, die wiederhergestellt werden soll, mithilfe des Vorgangs [Get Recoverable Database][] ab.
3. Erstellen Sie die Wiederherstellungsanforderung mithilfe des Vorgangs [Create Database Recovery Request][].
4. Verfolgen Sie den Status der Wiederherstellung mithilfe des Vorgangs [Database Operation Status][] nach.

### Konfigurieren der Datenbank nach der Durchführung einer Geowiederherstellung
Dies ist eine Prüfliste, mit der Sie die wiederhergestellte Datenbank für die Produktion vorbereiten können.

1. **Aktualisieren von Verbindungszeichenfolgen**: Stellen Sie sicher, dass die Verbindungszeichenfolgen Ihrer Clienttools auf die neu wiederhergestellte Datenbank verweisen.
2. **Ändern von Firewallregeln**: Überprüfen Sie die Firewallregeln auf dem Zielserver, und stellen Sie sicher, dass die Verbindungen von Ihren Clientcomputern oder Azure zum Server und der neu wiederhergestellten Datenbank aktiviert sind.
3. **Überprüfen von Serveranmeldungen und Datenbankbenutzern**: Überprüfen Sie, ob alle von der Anwendung verwendeten Anmeldungen auf dem Server vorhanden sind, der die wiederhergestellte Datenbank hostet. Erstellen Sie die fehlenden Anmeldungen erneut, und gewähren Sie ihnen entsprechende Berechtigungen auf der wiederhergestellten Datenbank. 
4. **Aktivieren der Überwachung**: Wenn die Überwachung für den Zugriff auf die Datenbank benötigt wird, müssen Sie nach der Wiederherstellung der Datenbank die Überwachung aktivieren.

Für die wiederhergestellte Datenbank ist TDE aktiviert, wenn für die Quelldatenbank TDE aktiviert ist.


## Nächste Schritte
Informationen zu den Geschäftskontinuitätsfunktionen von Azure SQL-Datenbank-Editionen finden Sie in der [Azure SQL-Datenbank-Übersicht zur Geschäftskontinuität][].

<!--Image references-->

<!--Article references-->
[Azure SQL-Datenbank-Übersicht zur Geschäftskontinuität]: sql-database-business-continuity.md
[Abschließen der wiederhergestellten Azure SQL-Datenbank]: sql-database-recovered-finalize.md
[How to install and configure Azure PowerShell]: powershell-install-configure.md

<!--MSDN references-->
[Datenbankwiederherstellungsanforderung erstellen]: https://msdn.microsoft.com/library/azure/dn509571.aspx
[Database operation status]: https://msdn.microsoft.com/library/azure/dn720371.aspx
[Datenbank-Betriebsstatus]: https://msdn.microsoft.com/library/azure/dn720371.aspx
[Wiederherstellbare gelöschte Datenbank abrufen]: https://msdn.microsoft.com/library/azure/dn509574.aspx
[Wiederherstellbare gelöschte Datenbanken auflisten]: https://msdn.microsoft.com/library/azure/dn509562.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Blog references-->
[diesem Blogbeitrag]: https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps

<!---HONumber=AcomDC_0518_2016-->