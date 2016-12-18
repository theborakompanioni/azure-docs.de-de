---
title: Verwalten von Computeleistung in Azure SQL Data Warehouse (REST) | Microsoft Docs
description: "Transact-SQL-Aufgaben (T-SQL) zur horizontalen Hochskalierung durch Anpassen der DWUs Sparen Sie Kosten, indem Sie während Nicht-Spitzenzeiten herunterskalieren."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: a970d939-2adf-4856-8a78-d4fe8ab2cceb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 41ab1c4b2709c2ea6890ca526db1dea177b7da1b


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

Passen Sie die Leistung durch horizontales Hochskalieren der Computeressourcen und des Arbeitsspeichers an, um die wechselnden Anforderungen Ihrer Workload zu erfüllen. Sparen Sie außerhalb von Spitzenzeiten Kosten, indem Sie die zur Verfügung stehenden Ressourcen verringern oder den Computevorgang vollständig pausieren. 

Diese Sammlung von Tasks verwendet T-SQL für folgende Aufgaben:

* Anzeigen der aktuellen DWU-Einstellungen
* Ändern von Computeressourcen durch Anpassen der DWUs

Wählen Sie eine der anderen Plattformoptionen am Anfang dieses Artikels, um eine Datenbank anzuhalten oder fortzusetzen.

Informationen hierzu finden Sie unter [Übersicht zum Verwalten von Computeleistung][Übersicht zum Verwalten von Computeleistung].

<a name="current-dwu-bk"></a>

## <a name="view-current-dwu-settings"></a>Anzeigen der aktuellen DWU-Einstellungen
So zeigen Sie die aktuellen DWU-Einstellungen für Ihre Datenbanken an:

1. Öffnen Sie den Objekt-Explorer von SQL Server in Visual Studio 2015.
2. Stellen Sie eine Verbindung mit der Masterdatenbank mit dem logischen SQL-Datenbankserver her.
3. Treffen Sie eine Auswahl aus der dynamischen Verwaltungssicht „sys.database_service_objectives“. Beispiel: 

```
SELECT
 db.name [Database],
 ds.edition [Edition],
 ds.service_objective [Service Objective]
FROM
 sys.database_service_objectives ds
 JOIN sys.databases db ON ds.database_id = db.database_id
```

<a name="scale-dwu-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a>Skalieren von Computeressourcen
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

So ändern Sie die DWUs

1. Stellen Sie eine Verbindung mit der Masterdatenbank mit Ihrem logischen SQL-Datenbankserver her.
2. Verwenden Sie die TSQL-Anweisung [ALTER DATABASE][ALTER DATABASE]. Im folgenden Beispiel wird das Servicelevelziel für die Datenbank „MySQLDW“ auf „DW1000“ gesetzt. 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Nächste Schritte
Andere Verwaltungsaufgaben finden Sie in der [Verwaltungsübersicht][Verwaltungsübersicht].

<!--Image references-->

<!--Article references-->
[Dienstkapazitätsgrenzen]: ./sql-data-warehouse-service-capacity-limits.md
[Verwaltungsübersicht]: ./sql-data-warehouse-overview-manage.md
[Übersicht zum Verwalten von Computeleistung]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->

[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx


<!--Other Web references-->

[Azure-Portal]: http://portal.azure.com/



<!--HONumber=Nov16_HO3-->


