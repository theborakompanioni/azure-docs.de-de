---
title: Wiederherstellen einer Azure SQL Data Warehouse-Instanz (REST-API) | Microsoft Docs
description: REST-API-Aufgaben zum Wiederherstellen einer Azure SQL Data Warehouse-Instanz.
services: sql-data-warehouse
documentationcenter: NA
author: Lakshmi1812
manager: jhubbard
editor: ''

ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: lakshmir;barbkess

---
# <a name="restore-an-azure-sql-data-warehouse-rest-api"></a>Wiederherstellen einer Azure SQL Data Warehouse-Instanz (REST-API)
> [!div class="op_single_selector"]
> * [Übersicht][Übersicht]
> * [Portal][Portal]
> * [PowerShell][PowerShell]
> * [REST][REST]
> 
> 

In diesem Artikel erfahren Sie, wie Sie eine Azure SQL Data Warehouse-Instanz mithilfe der REST-API wiederherstellen.

## <a name="before-you-begin"></a>Voraussetzungen
**Überprüfen Sie Ihre DTU-Kapazität.** Jedes SQL Data Warehouse wird von einer SQL Server-Instanz gehostet (z.B. myserver.database.windows.net), die über ein Standard-DTU-Kontingent verfügt.  Bevor Sie ein SQL Data Warehouse wiederherstellen können, überprüfen Sie, ob Ihre SQL Server-Instanz über genügend verbleibendes DTU-Kontingent für die Datenbank-Wiederherstellung verfügt. Informationen zum Berechnen des DTU-Bedarfs bzw. zur Anforderung weiterer DTUs finden Sie unter [Anfordern einer DTU-Kontingentänderung][Anfordern einer DTU-Kontingentänderung].

## <a name="restore-an-active-or-paused-database"></a>Wiederherstellen einer aktiven oder angehaltenen Datenbank
So stellen Sie eine Datenbank wieder her:

1. Rufen Sie die Liste mit den Wiederherstellungspunkten für die Datenbank ab, indem Sie den Get Database Restore Points-Vorgang verwenden.
2. Beginnen Sie die Wiederherstellung mit dem Vorgang [Datenbankwiederherstellungsanforderung erstellen][Datenbankwiederherstellungsanforderung erstellen] .
3. Verfolgen Sie den Status der Wiederherstellung mithilfe des Vorgangs [Datenbank-Betriebsstatus][Datenbank-Betriebsstatus] nach.

> [!NOTE]
> Nach Abschluss der Wiederherstellung können Sie Ihre wiederhergestellte Datenbank konfigurieren. Befolgen Sie hierzu die Anleitung [Konfigurieren der Datenbank nach der Wiederherstellung][Konfigurieren der Datenbank nach der Wiederherstellung].
> 
> 

## <a name="restore-a-deleted-database"></a>Wiederherstellen einer gelöschten Datenbank
So stellen Sie eine gelöschte Datenbank wieder her

1. Listen Sie alle gelöschten wiederherstellbaren Datenbanken mithilfe des Vorgangs [Wiederherstellbare gelöschte Datenbanken auflisten][Wiederherstellbare gelöschte Datenbanken auflisten] auf.
2. Rufen Sie die Details der gelöschten Datenbank, die wiederhergestellt werden soll, mithilfe des Vorgangs [Wiederherstellbare gelöschte Datenbank abrufen][Wiederherstellbare gelöschte Datenbank abrufen] ab.
3. Beginnen Sie die Wiederherstellung mit dem Vorgang [Datenbankwiederherstellungsanforderung erstellen][Datenbankwiederherstellungsanforderung erstellen] .
4. Verfolgen Sie den Status der Wiederherstellung mithilfe des Vorgangs [Datenbank-Betriebsstatus][Datenbank-Betriebsstatus] nach.

> [!NOTE]
> Informationen zum Konfigurieren der Datenbank nach Abschluss der Wiederherstellung finden Sie unter [Konfigurieren der Datenbank nach der Wiederherstellung][Konfigurieren der Datenbank nach der Wiederherstellung]. 
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Informationen zu den Geschäftskontinuitätsfunktionen von Azure SQL-Datenbank-Editionen finden Sie in der [Azure SQL-Datenbank-Übersicht zur Geschäftskontinuität][Azure SQL-Datenbank-Übersicht zur Geschäftskontinuität].

<!--Image references-->

<!--Article references-->
[Azure SQL-Datenbank-Übersicht zur Geschäftskontinuität]: ./sql-database-business-continuity.md
[Anfordern einer DTU-Kontingentänderung]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Konfigurieren der Datenbank nach der Wiederherstellung]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Installieren und Konfigurieren von Azure PowerShell]: ./powershell-install-configure.md
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

<!--Other Web references-->
[Azure-Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps



<!--HONumber=Oct16_HO2-->


