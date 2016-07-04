<properties
   pageTitle="Geschäftskontinuität in der Cloud – Point-in-Time-Wiederherstellung – SQL-Datenbank | Microsoft Azure"
   description="Hier finden Sie Informationen zur Point-in-Time-Wiederherstellung, mit der Sie ein Rollback für eine Azure SQL-Datenbank durchführen können (bis zu 35 Tage)."
   services="sql-database"
   documentationCenter=""
   authors="stevestein"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-bcdr"
   ms.date="06/17/2016"
   ms.author="sstein"/>

# Übersicht: Point-in-Time-Wiederherstellung der SQL-Datenbank

> [AZURE.SELECTOR]
- [Übersicht über die Geschäftskontinuität](sql-datbase-business-continuity.md)
- [Point-in-Time-Wiederherstellung](sql-database-point-in-time-restore.md)
- [Wiederherstellen einer gelöschten Datenbank](sql-database-restore-deleted-database.md)
- [Geografische Wiederherstellung](sql-database-geo-restore.md)
- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Szenarios für die Geschäftskontinuität](sql-database-business-continuity-scenarios.md)

Mit der Point-in-Time-Wiederherstellung können Sie mithilfe [automatisierter SQL-Datenbanksicherungen](sql-database-automated-backups.md) auf dem gleichen logischen Server eine vorhandene Datenbank als neue Datenbank mit einem früheren Datenbankzustand wiederherstellen. Sie können die vorhandene Datenbank nicht überschreiben. Die Wiederherstellung eines früheren Zustands kann über das [Azure-Portal](sql-database-point-in-time-restore-portal.md), mithilfe von [PowerShell](sql-database-point-in-time-restore-powershell.md) oder mit der [REST-API](https://msdn.microsoft.com/library/azure/mt163685.aspx) durchgeführt werden.

> [AZURE.SELECTOR]
- [Azure-Portal](sql-database-point-in-time-restore-portal.md)
- [PowerShell](sql-database-point-in-time-restore-powershell.md)

Die Datenbank kann für eine beliebige Leistungsebene und einen beliebigen elastischen Pool wiederhergestellt werden. Vergewissern Sie sich, dass Sie für den Server oder Pool über ein ausreichendes DTU-Kontingent verfügen, und denken Sie daran, dass die Wiederherstellung eine neue Datenbank erstellt, deren Dienst- und Leistungsebene sich unter Umständen vom aktuellen Zustand der Livedatenbank unterscheiden. Nach Abschluss des Vorgangs verfügen Sie über eine wiederhergestellte, uneingeschränkt zugängliche Onlinedatenbank, für die reguläre Kosten anfallen (abhängig von Dienst- und Leistungsebene).

Um den ältesten verfügbaren Wiederherstellungspunkt herauszufinden, rufen Sie mit [Get Database](https://msdn.microsoft.com/library/dn505708.aspx) (*RecoveryPeriodStartDate*) den ältesten Wiederherstellungspunkt (nicht geografisch replizierter Wiederherstellungspunkt) ab.

Sie können die wiederhergestellte Datenbank entweder als Ersatz für die ursprüngliche Datenbank verwenden oder Daten daraus abrufen und die ursprüngliche Datenbank damit aktualisieren. Wenn die wiederhergestellte Datenbank als Ersatz für die ursprüngliche Datenbank fungieren soll, vergewissern Sie sich, dass Leistungs- und Dienstebene angemessen sind, und skalieren Sie die Datenbank bei Bedarf entsprechend. Sie können die ursprüngliche Datenbank umbenennen und dann die wiederhergestellte Datenbank mit dem Namen der ursprünglichen Datenbank versehen. Verwenden Sie hierzu den Befehl „ALTER DATABASE“ in T-SQL.

Wenn Sie Daten aus der wiederhergestellten Datenbank abrufen möchten, müssen Sie die dazu erforderlichen Wiederherstellungsskripts separat erstellen und ausführen. Der Wiederherstellungsvorgang kann zwar eine ganze Weile dauern, die Datenbank wird jedoch während der gesamten Zeit in der Datenbankliste angezeigt. Wenn Sie die Datenbank während der Wiederherstellung löschen, wird der Vorgang abgebrochen, und es fallen keine Kosten an.

## Wiederherstellungsdauer einer Point-in-Time-Wiederherstellung

Die Zeit, die zum Wiederherstellen einer Datenbank benötigt wird, hängt von zahlreichen Faktoren ab. Hierzu zählen unter anderem die Größe der Datenbank, die Anzahl der Transaktionsprotokolle, der ausgewählte Zeitpunkt und die Menge an Aktivitäten, die ausgeführt werden müssen, um den Zustand des ausgewählten Zeitpunkts wiederherzustellen. Bei einer sehr großen und/oder sehr aktiven Datenbank kann die Wiederherstellung mehrere Stunden dauern. Der Großteil der Datenbankwiederherstellungen erfolgt innerhalb von 12 Stunden.

## Sichern/Wiederherstellen im Vergleich zu Kopieren/Exportieren/Importieren

Die Point-in-Time-Wiederherstellung ist die empfohlene Vorgehensweise für die Wiederherstellung von Basic-, Standard- und Premium-Datenbanken nach unbeabsichtigtem Datenverlust oder unbeabsichtigter Beschädigung. Sicherung und Wiederherstellung bieten folgende Vorteile: Sie sind kostengünstiger. (Gebühren fallen nur bei übermäßigen Sicherungen an. Im Gegensatz dazu fallen Kosten für die Datenbankkopie, die zur Gewährleistung eines Exports mit konsistenten Transaktionen benötigt wird, sowie für die Speicherung der BACPAC-Datei an.) Sie erfordern keinerlei Verwaltung. (Die Sicherungen werden automatisch erstellt. Exporte müssen dagegen manuell verwaltet und geplant werden.) Sie bieten ein besseres RPO. (Die Wiederherstellung kann im Gegensatz zum Kopieren/Exportieren/Importieren minutengenau für einen bestimmten Zeitpunkt erfolgen.) Sie ermöglichen eine schnellere Wiederherstellung. (Das Wiederherstellen von Sicherungen ist in der Regel deutlich schneller als das Importieren, da beim Importieren das Schema erstellt, Indizes deaktiviert, Daten geladen und dann Indizes einzeln für jede Tabelle erstellt werden müssen.) Für eine langfristige, über die Aufbewahrungsdauer hinausgehende Archivierung wird allerdings weiterhin die Verwendung von Kopieren und Exportieren empfohlen.


## Zusammenfassung

Automatische Sicherungen und die Point-in-Time-Wiederherstellung schützen Ihre Datenbanken vor unbeabsichtigter Beschädigung oder Löschung von Daten. Diese kostenlose, verwaltungsfreie Lösung steht für alle SQL-Datenbanken zur Verfügung. Verglichen mit der Alternative (Kopieren/Exportieren/Importieren) stellen Sicherung und Wiederherstellung eine deutliche Verbesserung für kurzfristige Wiederherstellungen dar. Es empfiehlt sich, die Point-in-Time-Wiederherstellung in Ihre Geschäftskontinuitätsstrategie zu integrieren. Die Verwendung der Exportoption wird dagegen nur für die langfristige Archivierung oder für die Datenmigration empfohlen.


## Nächste Schritte

- Ausführliche Schritte zur Wiederherstellung des Zustands zu einem bestimmten Zeitpunkt mithilfe des Azure-Portals finden Sie unter [Point-in-Time-Wiederherstellung mithilfe des Azure-Portals](sql-database-point-in-time-restore-portal.md).
- Ausführliche Schritte zur Wiederherstellung des Zustands zu einem bestimmten Zeitpunkt mithilfe von PowerShell finden Sie unter [Point-in-Time-Wiederherstellung mithilfe von PowerShell](sql-database-point-in-time-restore-powershell.md).
- Informationen zur Wiederherstellung des Zustands zu einem bestimmten Zeitpunkt mithilfe der REST-API finden Sie unter [Point-In-Time-Wiederherstellung mithilfe der REST-API](https://msdn.microsoft.com/library/azure/mt163685.aspx).
- Eine vollständige Erläuterung der Wiederherstellung nach einem Benutzer- oder Anwendungsfehler finden Sie unter [Wiederherstellen einer Azure-SQL-Datenbank nach einem Benutzerfehler](sql-database-user-error-recovery.md).

## Zusätzliche Ressourcen

- [Szenarios für die Geschäftskontinuität](sql-database-business-continuity-scenarios.md)

<!---HONumber=AcomDC_0622_2016-->