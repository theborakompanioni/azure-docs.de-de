---
title: Anhalten, Fortsetzen und Skalieren mit T-SQL in Azure SQL Data Warehouse | Microsoft-Dokumentation
description: "Transact-SQL-Aufgaben (T-SQL) zur horizontalen Hochskalierung durch Anpassen der DWUs Sparen Sie Kosten, indem Sie während Nicht-Spitzenzeiten herunterskalieren."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: a970d939-2adf-4856-8a78-d4fe8ab2cceb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 03/30/2017
ms.author: elbutter;barbkess
ms.translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: e37a943c775261a6817169c95a931f1b268305d9
ms.contentlocale: de-de
ms.lasthandoff: 03/31/2017


---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-t-sql"></a>Verwalten von Computeleistung in Azure SQL Data Warehouse (T-SQL)
> [!div class="op_single_selector"]
> * [Übersicht](sql-data-warehouse-manage-compute-overview.md)
> * [Portal](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>

<a name="current-dwu-bk"></a>

## <a name="view-current-dwu-settings"></a>Anzeigen der aktuellen DWU-Einstellungen
So zeigen Sie die aktuellen DWU-Einstellungen für Ihre Datenbanken an:

1. Öffnen Sie den Objekt-Explorer von SQL Server in Visual Studio.
2. Stellen Sie eine Verbindung mit der Masterdatenbank mit dem logischen SQL-Datenbankserver her.
3. Treffen Sie eine Auswahl aus der dynamischen Verwaltungssicht „sys.database_service_objectives“. Beispiel: 

```sql
SELECT
    db.name [Database]
,    ds.edition [Edition]
,    ds.service_objective [Service Objective]
FROM
     sys.database_service_objectives ds
JOIN
    sys.databases db ON ds.database_id = db.database_id
```

<a name="scale-dwu-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a>Skalieren von Computeressourcen
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

So ändern Sie die DWUs

1. Stellen Sie eine Verbindung mit der Masterdatenbank mit Ihrem logischen SQL-Datenbankserver her.
2. Verwenden Sie die T-SQL-Anweisung [ALTER DATABASE][ALTER DATABASE]. Im folgenden Beispiel wird das Servicelevelziel für die Datenbank „MySQLDW“ auf „DW1000“ gesetzt. 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

<a name="check-database-state-bk"></a>

## <a name="check-database-state-and-operation-progress"></a>Überprüfen von Datenbank- und Vorgangsstatus

1. Stellen Sie eine Verbindung mit der Masterdatenbank mit Ihrem logischen SQL-Datenbankserver her.
2. Übermitteln einer Abfrage zum Überprüfen des Datenbankstatus

```sql
SELECT *
FROM
sys.databases
```

3. Übermitteln einer Abfrage zum Überprüfen des Vorgangsstatus

```sql
SELECT *
FROM
    sys.dm_operation_status
WHERE
    resource_type_desc = 'Database'
AND 
    major_resource_id = 'MySQLDW'
```

Diese DMV gibt Informationen zu verschiedenen Verwaltungsvorgängen in Ihrer SQL Data Warehouse-Instanz zurück. Dazu gehören u.a. der Vorgang und der Status des Vorgangs, der entweder IN_PROGRESS oder COMPLETED lauten kann.



<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Nächste Schritte
Andere Verwaltungsaufgaben finden Sie in der [Verwaltungsübersicht][Management overview].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Management overview]: ./sql-data-warehouse-overview-manage.md
[Manage compute power overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->

[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/

