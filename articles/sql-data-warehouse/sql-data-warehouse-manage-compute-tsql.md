<properties
   pageTitle="Verwalten von Computeleistung in Azure SQL Data Warehouse (REST) | Microsoft Azure"
   description="Transact-SQL-Aufgaben (T-SQL) zur horizontalen Hochskalierung durch Anpassen der DWUs Sparen Sie Kosten, indem Sie während Nicht-Spitzenzeiten herunterskalieren."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/06/2016"
   ms.author="barbkess;sonyama"/>

# Verwalten von Computeleistung in Azure SQL Data Warehouse (T-SQL)

> [AZURE.SELECTOR]
- [Übersicht](sql-data-warehouse-overview-manage-compute.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [REST](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Passen Sie die Leistung durch horizontales Hochskalieren der Computeressourcen und des Arbeitsspeichers an, um die wechselhaften Anforderungen Ihres Workloads zu erfüllen. Sparen Sie außerhalb von Spitzenzeiten Kosten, indem Sie die zur Verfügung stehenden Ressourcen verringern oder den Computevorgang vollständig pausieren.

Diese Sammlung von Tasks verwendet T-SQL für folgende Aufgaben:

- Anzeigen der aktuellen DWU-Einstellungen
- Ändern von Computeressourcen durch Anpassen der DWUs

Wählen Sie eine der anderen Plattformoptionen am Anfang dieses Artikels, um eine Datenbank anzuhalten oder fortzusetzen.

Informationen hierzu finden Sie in der [Übersicht zum Verwalten von Computeleistung][].

<a name="current-dwu-bk"></a>

## Anzeigen der aktuellen DWU-Einstellungen

So zeigen Sie die aktuellen DWU-Einstellungen für Ihre Datenbanken an:

1. Öffnen Sie den Objekt-Explorer von SQL Server in Visual Studio 2015.
2. Stellen Sie eine Verbindung mit der Masterdatenbank mit dem logischen SQL-Datenbankserver her.
2. Treffen Sie eine Auswahl aus der dynamischen Verwaltungssicht „sys.database\_service\_objectives“. Beispiel: 

```
SELECT
 db.name [Database],
 ds.edition [Edition],
 ds.service_objective [Service Objective]
FROM
 sys.database_service_objectives ds
 JOIN sys.databases db ON ds.database_id = db.database_id
```

<a name="scale-dwu-bk"></a> <a name="scale-compute-bk"></a>

## Skalieren von Computeressourcen

[AZURE.INCLUDE [Beschreibung des Skalierens der DWUs in SQL Data Warehouse](../../includes/sql-data-warehouse-scale-dwus-description.md)]

So ändern Sie die DWUs


1. Stellen Sie eine Verbindung mit der Masterdatenbank mit Ihrem logischen SQL-Datenbankserver her.
2. Verwenden Sie die TSQL-Anweisung [ALTER DATABASE][]. Im folgenden Beispiel wird das Servicelevelziel für die Datenbank „MySQLDW“ auf „DW1000“ gesetzt. 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

<a name="next-steps-bk"></a>

## Nächste Schritte

Andere Verwaltungsaufgaben finden Sie in der [Verwaltungsübersicht][].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Verwaltungsübersicht]: ./sql-data-warehouse-overview-manage.md
[Übersicht zum Verwalten von Computeleistung]: ./sql-data-warehouse-overview-manage-compute-power.md

<!--MSDN references-->

[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/

<!---HONumber=AcomDC_0518_2016-->