<properties
   pageTitle="Wiederherstellen einer Azure SQL Data Warehouse-Instanz (REST-API) | Microsoft Azure"
   description="REST-API-Aufgaben zum Wiederherstellen einer Azure SQL Data Warehouse-Instanz."
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
   ms.date="06/10/2016"
   ms.author="elfish;barbkess;sonyama"/>

# Wiederherstellen einer Azure SQL Data Warehouse-Instanz (REST-API)

> [AZURE.SELECTOR]
- [Übersicht][]
- [Portal][]
- [PowerShell][]
- [REST][]

In diesem Artikel erfahren Sie, wie Sie eine Azure SQL Data Warehouse-Instanz mithilfe der REST-API wiederherstellen.

## Voraussetzungen

**Überprüfen Sie Ihre DTU-Kapazität.** Jede SQL Data Warehouse-Instanz wird von einem logischen Server mit SQL Server gehostet. Dieser logische Server verfügt über ein in DTU gemessenes Kapazitätslimit. Vor dem Wiederherstellen einer SQL Data Warehouse-Instanz muss zunächst geprüft werden, ob die DTU-Kapazität des logischen Servers mit SQL Server, der als Host für Ihre Datenbank fungiert, für die wiederherzustellende Datenbank ausreicht. Weitere Informationen zum Anzeigen und Erhöhen des DTU-Kontingents finden Sie in [diesem Blogbeitrag][].

## Wiederherstellen einer aktiven oder angehaltenen Datenbank

So stellen Sie eine Datenbank wieder her:

1. Rufen Sie die Liste mit den Wiederherstellungspunkten für die Datenbank ab, indem Sie den Get Database Restore Points-Vorgang verwenden.
2. Beginnen Sie die Wiederherstellung mit dem Vorgang [Datenbankwiederherstellungsanforderung erstellen][].
3. Verfolgen Sie den Status der Wiederherstellung mithilfe des Vorgangs [Datenbank-Betriebsstatus][] nach.

>[AZURE.NOTE] Nach Abschluss der Wiederherstellung können Sie Ihre wiederhergestellte Datenbank konfigurieren. Befolgen Sie hierzu die Anleitung [Abschließen der wiederhergestellten Azure SQL-Datenbank][].

## Wiederherstellen einer gelöschten Datenbank

So stellen Sie eine gelöschte Datenbank wieder her

1.	Listen Sie alle gelöschten wiederherstellbaren Datenbanken mithilfe des Vorgangs [Wiederherstellbare gelöschte Datenbanken auflisten][] auf.
2.	Rufen Sie die Details der gelöschten Datenbank, die wiederhergestellt werden soll, mithilfe des Vorgangs [Wiederherstellbare gelöschte Datenbank abrufen][] ab.
3.	Beginnen Sie die Wiederherstellung mit dem Vorgang [Datenbankwiederherstellungsanforderung erstellen][].
4.	Verfolgen Sie den Status der Wiederherstellung mithilfe des Vorgangs [Datenbank-Betriebsstatus][] nach.

>[AZURE.NOTE] Nach Abschluss der Wiederherstellung können Sie Ihre wiederhergestellte Datenbank konfigurieren. Befolgen Sie hierzu die Anleitung [Abschließen der wiederhergestellten Azure SQL-Datenbank][].


## Nächste Schritte
Informationen zu den Geschäftskontinuitätsfeatures von Azure SQL-Datenbank-Editionen finden Sie in der [Übersicht über die Geschäftskontinuität der Azure SQL-Datenbank][].

<!--Image references-->

<!--Article references-->
[Übersicht über die Geschäftskontinuität der Azure SQL-Datenbank]: ./sql-database-business-continuity.md
[Abschließen der wiederhergestellten Azure SQL-Datenbank]: ./sql-database-recovered-finalize.md
[How to install and configure Azure PowerShell]: ./powershell-install-configure.md
[Übersicht]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->
[Datenbankwiederherstellungsanforderung erstellen]: https://msdn.microsoft.com/library/azure/dn509571.aspx
[Datenbank-Betriebsstatus]: https://msdn.microsoft.com/library/azure/dn720371.aspx
[Wiederherstellbare gelöschte Datenbank abrufen]: https://msdn.microsoft.com/library/azure/dn509574.aspx
[Wiederherstellbare gelöschte Datenbanken auflisten]: https://msdn.microsoft.com/library/azure/dn509562.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx


<!--Blog references-->
[diesem Blogbeitrag]: https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps

<!---HONumber=AcomDC_0615_2016-->