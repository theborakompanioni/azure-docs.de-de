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
   ms.workload="data-management"
   ms.date="05/10/2016"
   ms.author="sstein"/>

# Übersicht: Point-in-Time-Wiederherstellung der SQL-Datenbank

> [AZURE.SELECTOR]
- [Übersicht](sql-database-point-in-time-restore.md)
- [Azure-Portal](sql-database-point-in-time-restore-portal.md)
- [PowerShell](sql-database-point-in-time-restore-powershell.md)

Mit der Point-in-Time-Wiederherstellung können Sie mithilfe [automatisierter SQL-Datenbanksicherungen](sql-database-automated-backups.md) einen früheren Datenbankzustand wiederherstellen. Die Wiederherstellung eines früheren Zustands kann über das [Azure-Portal](sql-database-point-in-time-restore-portal.md), mithilfe von [PowerShell](sql-database-point-in-time-restore-powershell.md) oder mit der [REST-API](https://msdn.microsoft.com/library/azure/mt163685.aspx) durchgeführt werden.

Die Datenbank kann für eine beliebige Leistungsebene und einen beliebigen elastischen Pool wiederhergestellt werden. Vergewissern Sie sich, dass Sie für den Server oder Pool über ein ausreichendes DTU-Kontingent verfügen, und denken Sie daran, dass die Wiederherstellung eine neue Datenbank erstellt, deren Dienst- und Leistungsebene sich unter Umständen vom aktuellen Zustand der Livedatenbank unterscheiden. Nach Abschluss des Vorgangs verfügen Sie über eine wiederhergestellte, uneingeschränkt zugängliche Onlinedatenbank, für die reguläre Kosten anfallen (abhängig von Dienst- und Leistungsebene). Sie können die wiederhergestellte Datenbank entweder als Ersatz für die ursprüngliche Datenbank verwenden oder Daten daraus abrufen und die ursprüngliche Datenbank damit aktualisieren. Wenn die wiederhergestellte Datenbank als Ersatz für die ursprüngliche Datenbank fungieren soll, vergewissern Sie sich, dass Leistungs- und Dienstebene angemessen sind, und skalieren Sie die Datenbank bei Bedarf entsprechend. Sie können die ursprüngliche Datenbank umbenennen und dann die wiederhergestellte Datenbank mit dem Namen der ursprünglichen Datenbank versehen. Verwenden Sie hierzu den Befehl „ALTER DATABASE“ in T-SQL. Wenn Sie Daten aus der wiederhergestellten Datenbank abrufen möchten, müssen Sie die dazu erforderlichen Wiederherstellungsskripts separat erstellen und ausführen. Der Wiederherstellungsvorgang kann zwar eine ganze Weile dauern, die Datenbank wird jedoch während der gesamten Zeit in der Datenbankliste angezeigt. Wenn Sie die Datenbank während der Wiederherstellung löschen, wird der Vorgang abgebrochen, und es fallen keine Kosten an.

## Wiederherstellungsdauer einer Point-in-Time-Wiederherstellung

Die Zeit, die zum Wiederherstellen einer Datenbank benötigt wird, hängt von zahlreichen Faktoren ab. Hierzu zählen unter anderem die Größe der Datenbank, der ausgewählte Zeitpunkt und die Menge an Aktivitäten, die ausgeführt werden müssen, um den Zustand des ausgewählten Zeitpunkts wiederherzustellen. Bei einer sehr großen und/oder sehr aktiven Datenbank kann die Wiederherstellung mehrere Stunden dauern. Im Zuge der Wiederherstellung einer Datenbank wird auf dem Server mit der ursprünglichen Datenbank immer eine neue Datenbank erstellt. Die wiederhergestellte Datenbank muss daher einen neuen Namen erhalten.

## Sichern/Wiederherstellen im Vergleich zu Kopieren/Exportieren/Importieren

Die Point-in-Time-Wiederherstellung ist die empfohlene Vorgehensweise für die Wiederherstellung von Basic-, Standard- und Premium-Datenbanken nach unbeabsichtigtem Datenverlust oder unbeabsichtigter Beschädigung. Sicherung und Wiederherstellung bieten folgende Vorteile: Sie sind kostengünstiger. (Gebühren fallen nur bei übermäßigen Sicherungen an. Im Gegensatz dazu fallen Kosten für die Datenbankkopie, die zur Gewährleistung eines Exports mit konsistenten Transaktionen benötigt wird, sowie für die Speicherung der BACPAC-Datei an.) Sie erfordern keinerlei Verwaltung. (Die Sicherungen werden automatisch erstellt. Exporte müssen dagegen manuell verwaltet und geplant werden.) Sie bieten ein besseres RPO. (Die Wiederherstellung kann im Gegensatz zum Kopieren/Exportieren/Importieren minutengenau für einen bestimmten Zeitpunkt erfolgen.) Sie ermöglichen eine schnellere Wiederherstellung. (Das Wiederherstellen von Sicherungen ist in der Regel deutlich schneller als das Importieren, da beim Importieren das Schema erstellt, Indizes deaktiviert, Daten geladen und dann Indizes einzeln für jede Tabelle erstellt werden müssen.) Für eine langfristige, über die Aufbewahrungsdauer hinausgehende Archivierung wird allerdings weiterhin die Verwendung von Kopieren und Exportieren empfohlen.


## Zusammenfassung

Automatische Sicherungen und die Point-in-Time-Wiederherstellung schützen Ihre Datenbanken vor unbeabsichtigter Beschädigung oder Löschung von Daten. Diese kostenlose, verwaltungsfreie Lösung steht für alle SQL-Datenbanken zur Verfügung. Verglichen mit der Alternative (Kopieren/Exportieren/Importieren) stellen Sicherung und Wiederherstellung eine deutliche Verbesserung für kurzfristige Wiederherstellungen dar. Es empfiehlt sich, die Point-in-Time-Wiederherstellung in Ihre Geschäftskontinuitätsstrategie zu integrieren. Die Verwendung der Exportoption wird dagegen nur für die langfristige Archivierung oder für die Datenmigration empfohlen.


## Nächste Schritte

- [Abschließen der wiederhergestellten Azure SQL-Datenbank](sql-database-recovered-finalize.md)
- [Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt über das Azure-Portal](sql-database-point-in-time-restore-portal.md)
- [Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt mit PowerShell](sql-database-point-in-time-restore-powershell.md)
- [Point-in-time restore using the REST API](https://msdn.microsoft.com/library/azure/mt163685.aspx) (Point-in-Time-Wiederherstellung mithilfe der REST-API)
- [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md)


## Zusätzliche Ressourcen

- [Wiederherstellen einer gelöschten Datenbank](sql-database-restore-deleted-database.md)
- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [Geografische Wiederherstellung](sql-database-geo-restore.md)
- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md)

<!---HONumber=AcomDC_0615_2016-->