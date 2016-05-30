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
   ms.date="05/05/2016"
   ms.author="elfish;barbkess;sonyama"/>


# Datenbankwiederherstellung in Azure SQL Data Warehouse (Übersicht)

> [AZURE.SELECTOR]
- [Übersicht](sql-data-warehouse-overview-manage-database-restore.md)
- [Portal](sql-data-warehouse-manage-database-restore-portal.md)
- [PowerShell](sql-data-warehouse-manage-database-restore-powershell.md)
- [REST](sql-data-warehouse-manage-database-restore-rest-api.md)

Beschreibt die Optionen zum Wiederherstellen einer Datenbank in Azure SQL Data Warehouse. Dazu gehören die Wiederherstellung einer Livedatenbank, einer gelöschte Datenbank, und die Wiederherstellung einer Datenbank, auf die nicht zugegriffen werden kann. Die Livedatenbank und die gelöschte Datenbank werden mit einer Momentaufnahme aus demselben Rechenzentrum wiederhergestellt. Die Datenbank, auf die nicht zugegriffen werden kann, kann aufgrund eines Ausfalls möglicherweise offline sein. In diesem Fall erfolgt die Wiederherstellung von einem Rechenzentrum an einem andern geografischen Standort.


## Wiederherstellungsszenarien

**Wiederherstellung bei Fehlern in der Infrastruktur**: Dieses Szenario bezieht sich auf das Wiederherstellen nach Infrastrukturproblemen wie Datenträgerfehlern usw. Ein Kunde möchte Geschäftskontinuität mit einer fehlertoleranten und hoch verfügbaren Infrastruktur gewährleisten.

**Wiederherstellung nach Benutzerfehlern**: Dieses Szenario bezieht sich auf die Wiederherstellung nach unbeabsichtigten oder beiläufigen Datenbeschädigungen oder -löschungen. Ein Kunde möchte für den Fall, dass ein Benutzer versehentlich oder beiläufig Daten ändert oder löscht, Geschäftskontinuität durch Wiederherstellen der Datenbank zu einem früheren Zeitpunkt sicherstellen.

**Wiederherstellung nach Datenverlusten (DR)**: Dieses Szenario bezieht sich auf das Wiederherstellen nach einer Katastrophe. In einem Szenario, in dem ein Ereignis wie z. B. eine Naturkatastrophe oder ein regionaler Ausfalls bewirkt, dass die Datenbank nicht mehr verfügbar ist, möchte ein Kunde die Datenbank in einer anderen Region wiederherstellen, um den Geschäftsbetrieb aufrecht erhalten zu können.

## Momentaufnahmen-Richtlinien

[AZURE.INCLUDE [SQL Data Warehouse-Aufbewahrungsrichtlinie für die Sicherung](../../includes/sql-data-warehouse-backup-retention-policy.md)]


## Wiederherstellungsfunktionen für Datenbanken

Wir untersuchen nun, wie SQL Data Warehouse in den oben genannten Szenarios die Zuverlässigkeit Ihrer Datenbank erhöht und die Wiederherstellbarkeit und die fortlaufende Ausführung des Systems ermöglicht.


### Datenredundanz

SQL Data Warehouse speichert alle Daten in georedundanten Azure Storage (RA-GRS). Georedundante Speicher repliziert Ihre Daten in eine sekundäre Region, die Hunderte von Kilometern von der primären Region entfernt ist. In primären und sekundären Regionen werden Ihre Daten jeweils dreimal in separaten Fehler- und Upgradedomänen repliziert. Dadurch wird sichergestellt, dass Ihre Daten selbst bei vollständigen regionalen Ausfällen oder bei Notfällen, in denen eine Region nicht zur Verfügung steht, erhalten bleiben. Weitere Informationen zu georedundantem Speicher mit Lesezugriff finden Sie unter [Redundanzoptionen für Azure-Speicher][].

### Datenbankwiederherstellung

Mit der Datenbankwiederherstellung können Sie die Datenbank in den Zustand zu einem früheren Zeitpunkt wiederherstellen. Der Azure SQL Data Warehouse-Dienst schützt alle Datenbanken durch automatische Speichermomentaufnahmen mindestens alle acht Stunden, die sieben Tage lang aufbewahrt werden und einen diskreten Satz von Wiederherstellungspunkten für Sie bereitstellen. Diese Momentaufnahmen werden in RA-GRS Azure Storage gespeichert und sind daher standardmäßig georedundant. Die Funktionen für die automatische Momentaufnahme und Wiederherstellung bieten eine verwaltungsfreie Möglichkeit, Datenbanken vor versehentlicher Beschädigung oder Löschung zu schützen. Weitere Informationen zur Datenbankwiederherstellung finden Sie unter [Database restore tasks][] \(Aufgaben zur Datenbankwiederherstellung).

### Geografische Wiederherstellung

Die geografische Wiederherstellung soll die Datenbank wiederherstellen, wenn sie aufgrund eines Ausfalls nicht mehr zur Verfügung steht. Da die Sicherung georedundant ist, kann mit ihr eine Datenbank selbst dann wiederhergestellt werden, wenn sie aufgrund eines Ausfalls nicht mehr verfügbar ist. Die wiederhergestellte Datenbank kann auf einem beliebigen Server in jeder Azure-Region erstellt werden. Neben der Wiederherstellung nach einem Ausfall kann die Geowiederherstellung auch für andere Szenarien verwendet werden, beispielsweise zum Migrieren oder Kopieren der Datenbank auf einen anderen Server oder in eine andere Region.

**Wann die Wiederherstellung gestartet werden sollte** Der Wiederherstellungsvorgang erfordert eine Änderung der SQL-Verbindungszeichenfolge nach der Wiederherstellung und kann zu dauerhaftem Datenverlust führen. Er sollte daher nur ausgeführt werden, wenn der Ausfall wahrscheinlich länger als die Anwendungs-RTO dauert. Verwenden Sie die folgenden Datenpunkte, um zu bestätigen, dass die Wiederherstellung erforderlich ist:

- Permanente Verbindungsfehler mit der Datenbank.
- Das Azure-Portal zeigt eine Warnung zu einem Vorfall in der Region mit weit reichenden Auswirkungen.


## Nächste Schritte
Andere wichtige Verwaltungsaufgaben finden Sie in der [Verwaltungsübersicht][].

<!--Image references-->

<!--Article references-->
[Redundanzoptionen für Azure-Speicher]: ../storage/storage-redundancy.md#read-access-geo-redundant-storage
[Backup and restore tasks]: sql-data-warehouse-database-restore-portal.md
[Finalize a recovered database]: ../sql-database/sql-database-recovered-finalize.md
[Verwaltungsübersicht]: sql-data-warehouse-overview-management.md

<!--MSDN references-->


<!--Other Web references-->

<!---HONumber=AcomDC_0518_2016-->