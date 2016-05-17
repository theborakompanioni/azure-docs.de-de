<properties
pageTitle="Verwalten von Skalierbarkeitstasks für Azure SQL Data Warehouse (TSQL) | Microsoft Azure"
   description="TSQL-Tasks zum horizontalen Hochskalieren der Leistung für Azure SQL Data Warehouse. Ändern Sie Computeressourcen durch Anpassen der DWUs mithilfe von TSQL."
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
   ms.date="04/28/2016"
   ms.author="barbkess;sonyama"/>

# Verwalten von Skalierbarkeitstasks für Azure SQL Data Warehouse (TSQL)

> [AZURE.SELECTOR]
- [Übersicht](sql-data-warehouse-overview-scalability.md)
- [Portal](sql-data-warehouse-manage-scale-out-tasks.md)
- [PowerShell](sql-data-warehouse-manage-scale-out-tasks-powershell.md)
- [REST](sql-data-warehouse-manage-scale-out-tasks-rest-api.md)
- [TSQL](sql-data-warehouse-manage-scale-out-tasks-tsql.md)


Skalieren Sie Computeressourcen und Arbeitsspeicher elastisch horizontal hoch, um die sich ändernden Anforderungen Ihrer Workload zu erfüllen, und sparen Sie Kosten, indem Sie in Zeiten geringerer Auslastung Ressourcen zurück skalieren.

Diese Sammlung von Tasks verwendet TSQL für folgende Aufgaben:

- Anzeigen der aktuellen DWU-Einstellungen
- Ändern von Computeressourcen durch Anpassen der DWUs

Wählen Sie eine der anderen Plattformoptionen am Anfang dieses Artikels, um eine Datenbank anzuhalten oder fortzusetzen.

Informationen hierzu finden Sie unter [Übersicht über die Skalierbarkeit der Leistung][].

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

<a name="scale-dwu-bk"></a>

## Skalieren von DWUs

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
[Übersicht über die Skalierbarkeit der Leistung]: ./sql-data-warehouse-overview-scalability.md

<!--MSDN references-->

[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/

<!---HONumber=AcomDC_0504_2016-->