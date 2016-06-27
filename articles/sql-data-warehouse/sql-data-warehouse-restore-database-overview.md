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
   ms.date="06/14/2016"
   ms.author="elfish;barbkess;sonyama"/>


# Wiederherstellen einer Azure SQL Data Warehouse-Instanz (Übersicht)

> [AZURE.SELECTOR]
- [Übersicht][]
- [Portal][]
- [PowerShell][]
- [REST][]

Azure SQL Data Warehouse schützt Ihre Daten sowohl durch lokal redundanten Speicher als auch durch automatisierte Sicherungen. Mit automatisierte Sicherungen können Sie Datenbanken ohne Verwaltungsaufwand vor versehentlicher Beschädigung oder Löschung schützen. Falls ein Benutzer versehentlich oder nebenbei Daten ändert oder löscht, können Sie einen früheren Zustand der Datenbank wiederherstellen und so die Geschäftskontinuität gewährleisten. SQL Data Warehouse verwendet Azure Storage-Momentaufnahmen, um Ihre Datenbank nahtlos und ohne Ausfallzeiten zu sichern.

## Automatisierte Sicherungen

Ihre **aktiven** Datenbanken werden spätestens alle acht Stunden automatisch gesichert und die Sicherungen sieben Tage lang aufbewahrt. Dadurch können Sie Ihre aktive Datenbank auf einen von mehreren Wiederherstellungspunkten der letzten sieben Tage wiederherstellen.

Wenn eine Datenbank angehalten wird, werden keine neuen Momentaufnahmen erstellt, und vorherige Momentaufnahmen laufen aus, wenn sie ein Alter von sieben Tagen erreichen. Wird eine Datenbank länger als sieben Tage angehalten, wird die letzte Momentaufnahme gespeichert, sodass Ihnen immer mindestens eine Sicherung zur Verfügung steht.

Wird eine Datenbank gelöscht, wird die letzte Momentaufnahme sieben Tage lang gespeichert.

Mit der folgenden Abfrage können Sie ermitteln, wann die letzte Sicherung für Ihre Instanz erstellt wurde:

```sql
select top 1 *
from sys.pdw_loader_backup_runs 
order by run_id desc;
```

Wenn eine Sicherung für mehr als sieben Tage aufbewahrt werden soll, können Sie einfach einen Ihrer Wiederherstellungspunkte in einer neuen Datenbank wiederherstellen und diese Datenbank ggf. anhalten, damit Sie nur für den Speicherplatz dieser Sicherung bezahlen.

## Datenredundanz

Zusätzlich zu Sicherungen schützt SQL Data Warehouse Ihre Daten auch durch [lokal redundanten][] Azure Premium-Speicher. Mehrere synchrone Kopien der Daten werden im lokalen Rechenzentrum behalten, um transparenten Datenschutz bei lokalen Ausfällen sicherzustellen. Die Datenredundanz gewährleistet, dass Ihre Daten gegen Infrastrukturprobleme wie Datenträgerausfälle oder Ähnliches geschützt sind. Außerdem gewährleistet die Datenredundanz Geschäftskontinuität mit einer fehlertoleranten und hoch verfügbaren Infrastruktur.

## Wiederherstellen einer Datenbank

Die Wiederherstellung einer SQL Data Warehouse-Instanz ist ganz einfach und kann entweder über das Azure-Portal durchgeführt oder mithilfe von PowerShell oder REST-APIs automatisiert werden.


## Nächste Schritte
Informationen zu den Geschäftskontinuitätsfeatures von Azure SQL-Datenbank-Editionen finden Sie in der [Übersicht über die Geschäftskontinuität der Azure SQL-Datenbank][].

<!--Image references-->

<!--Article references-->
[Übersicht über die Geschäftskontinuität der Azure SQL-Datenbank]: ./sql-database-business-continuity.md
[lokal redundanten]: ../storage/storage-redundancy.md
[Übersicht]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->


<!--Other Web references-->

<!---HONumber=AcomDC_0615_2016-->