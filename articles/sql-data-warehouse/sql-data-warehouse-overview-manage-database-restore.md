<properties
   pageTitle="Datenbankwiederherstellung in Azure SQL Data Warehouse (Übersicht) | Microsoft Azure"
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
   ms.date="06/02/2016"
   ms.author="elfish;barbkess;sonyama"/>


# Datenbankwiederherstellung in Azure SQL Data Warehouse (Übersicht)

> [AZURE.SELECTOR]
- [Übersicht](sql-data-warehouse-overview-manage-database-restore.md)
- [Portal](sql-data-warehouse-manage-database-restore-portal.md)
- [PowerShell](sql-data-warehouse-manage-database-restore-powershell.md)
- [REST](sql-data-warehouse-manage-database-restore-rest-api.md)

In diesem Artikel werden die Optionen zum Wiederherstellen einer Datenbank in Azure SQL Data Warehouse beschrieben. Dazu gehören das Wiederherstellen eines Live- und gelöschten Data Warehouse. Live- und gelöschte Data Warehouses werden aus den Momentaufnahmen wiederhergestellt, die für alle Data Warehouses automatisch erstellt werden.

## Wiederherstellungsszenarien

**Wiederherstellung bei Fehlern in der Infrastruktur**: Dieses Szenario bezieht sich auf das Wiederherstellen nach Infrastrukturproblemen wie Datenträgerfehlern usw. Ein Kunde möchte Geschäftskontinuität mit einer fehlertoleranten und hoch verfügbaren Infrastruktur gewährleisten.

**Wiederherstellung nach Benutzerfehlern**: Dieses Szenario bezieht sich auf die Wiederherstellung nach unbeabsichtigten oder beiläufigen Datenbeschädigungen oder -löschungen. Ein Kunde möchte für den Fall, dass ein Benutzer versehentlich oder beiläufig Daten ändert oder löscht, Geschäftskontinuität durch Wiederherstellen der Datenbank zu einem früheren Zeitpunkt sicherstellen.

## Momentaufnahmen-Richtlinien

[AZURE.INCLUDE [SQL Data Warehouse-Aufbewahrungsrichtlinie für die Sicherung](../../includes/sql-data-warehouse-backup-retention-policy.md)]


## Wiederherstellungsfunktionen für Datenbanken

Wir untersuchen nun, wie SQL Data Warehouse in den oben genannten Szenarios die Zuverlässigkeit Ihrer Datenbank erhöht und die Wiederherstellbarkeit und die fortlaufende Ausführung des Systems ermöglicht.


### Datenredundanz

SQL Data Warehouse speichert drei Kopien aller Ihrer Daten in [lokal redundantem Speicher (LRS)](../storage/storage-redundancy.md) des Typs Azure Storage Premium.

### Datenbankwiederherstellung

Mit der Datenbankwiederherstellung können Sie die Datenbank in den Zustand zu einem früheren Zeitpunkt wiederherstellen. Der Azure SQL Data Warehouse-Dienst schützt alle Datenbanken durch automatische Speichermomentaufnahmen mindestens alle acht Stunden, die sieben Tage lang aufbewahrt werden und einen diskreten Satz von Wiederherstellungspunkten für Sie bereitstellen. Die Funktionen für die automatische Momentaufnahme und Wiederherstellung bieten eine verwaltungsfreie Möglichkeit, Datenbanken vor versehentlicher Beschädigung oder Löschung zu schützen. Weitere Informationen zur Datenbankwiederherstellung finden Sie unter [Database restore tasks][] (Aufgaben zur Datenbankwiederherstellung).

## Nächste Schritte
Andere wichtige Verwaltungsaufgaben finden Sie in der [Verwaltungsübersicht][].

<!--Image references-->

<!--Article references-->
[Azure storage redundancy options]: ../storage/storage-redundancy.md#read-access-geo-redundant-storage
[Backup and restore tasks]: sql-data-warehouse-database-restore-portal.md
[Verwaltungsübersicht]: sql-data-warehouse-overview-management.md
[Database restore tasks]: sql-data-warehouse-manage-database-restore-portal.md

<!--MSDN references-->


<!--Other Web references-->

<!---HONumber=AcomDC_0608_2016-->