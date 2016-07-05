<properties
   pageTitle="Wiederherstellen einer Azure SQL Data Warehouse-Instanz (Übersicht) | Microsoft Azure"
   description="Übersicht über die Wiederherstellungsoptionen zur Wiederherstellung einer Datenbank in Azure SQL Data Warehouse."
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
   ms.date="06/15/2016"
   ms.author="elfish;barbkess;sonyama"/>


# Wiederherstellen einer Azure SQL Data Warehouse-Instanz (Übersicht)

> [AZURE.SELECTOR]
- [Übersicht][]
- [Portal][]
- [PowerShell][]
- [REST][] Azure SQL Data Warehouse schützt Ihre Daten sowohl durch lokal redundanten Speicher als auch durch automatisierte Sicherungen. Mit automatisierte Sicherungen können Sie Datenbanken ohne Verwaltungsaufwand vor versehentlicher Beschädigung oder Löschung schützen. Falls ein Benutzer versehentlich oder nebenbei Daten ändert oder löscht, können Sie einen früheren Zustand der Datenbank wiederherstellen und so die Geschäftskontinuität gewährleisten. SQL Data Warehouse verwendet Azure Storage-Momentaufnahmen, um Ihre Datenbank nahtlos und ohne Ausfallzeiten zu sichern.

## Automatisierte Sicherungen

Ihre **aktiven** Datenbanken werden spätestens alle acht Stunden automatisch gesichert und die Sicherungen sieben Tage lang aufbewahrt. Dadurch können Sie Ihre aktive Datenbank auf einen von mehreren Wiederherstellungspunkten der letzten sieben Tage wiederherstellen.

Wenn eine Datenbank angehalten wird, werden keine neuen Sicherungen erstellt, und vorherige Sicherungen laufen aus, wenn sie ein Alter von sieben Tagen erreichen. Wird eine Datenbank länger als sieben Tage angehalten, wird die letzte Sicherung gespeichert, sodass Ihnen immer mindestens eine Sicherung zur Verfügung steht.

Wird eine Datenbank gelöscht, wird die letzte Sicherung sieben Tage lang gespeichert.

Führen Sie die folgende Abfrage in Ihrer aktiven SQL Data Warehouse-Instanz aus, um zu ermitteln, wann die letzte Sicherung erstellt wurde:

```sql
select top 1 *
from sys.pdw_loader_backup_runs 
order by run_id desc;
```

Wenn eine Sicherung für mehr als sieben Tage aufbewahrt werden soll, können Sie einfach einen Ihrer Wiederherstellungspunkte in einer neuen Datenbank wiederherstellen und diese Datenbank ggf. anhalten, damit Sie nur für den Speicherplatz dieser Sicherung bezahlen.

## Datenredundanz

Zusätzlich zu Sicherungen schützt SQL Data Warehouse Ihre Daten auch durch [lokal redundanten (LRS)][] Azure Premium-Speicher. Mehrere synchrone Kopien der Daten werden im lokalen Rechenzentrum behalten, um transparenten Datenschutz bei lokalen Ausfällen sicherzustellen. Die Datenredundanz gewährleistet, dass Ihre Daten gegen Infrastrukturprobleme wie Datenträgerausfälle oder Ähnliches geschützt sind. Außerdem gewährleistet die Datenredundanz Geschäftskontinuität mit einer fehlertoleranten und hoch verfügbaren Infrastruktur.

## Wiederherstellen einer Datenbank

Die Wiederherstellung einer SQL Data Warehouse-Instanz ist ganz einfach und kann entweder über das Azure-Portal durchgeführt oder mithilfe von PowerShell oder REST-APIs automatisiert werden.


## Nächste Schritte
Informationen zu den Geschäftskontinuitätsfunktionen von Azure SQL-Datenbank-Editionen finden Sie in der [Azure SQL-Datenbank-Übersicht zur Geschäftskontinuität][].

<!--Image references-->

<!--Article references-->
[Azure SQL-Datenbank-Übersicht zur Geschäftskontinuität]: ./sql-database-business-continuity.md
[lokal redundanten (LRS)]: ../storage/storage-redundancy.md
[Übersicht]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->


<!--Other Web references-->

<!---HONumber=AcomDC_0622_2016-->