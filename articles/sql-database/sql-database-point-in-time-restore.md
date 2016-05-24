<properties
   pageTitle="Geschäftskontinuität in der Cloud – Point-in-Time-Wiederherstellung | Microsoft Azure"
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

Mit der Point-in-Time-Wiederherstellung können Sie einen früheren Datenbankzustand wiederherstellen.

Der Azure SQL-Datenbankdienst schützt alle Datenbanken mit einer automatisierten Sicherung, die sieben (Basic), 14 (Standard) bzw. 35 Tage (Premium) lang aufbewahrt wird. Die Point-in-Time-Wiederherstellung ermöglicht die Wiederherstellung Ihrer Datenbank auf der Grundlage dieser Sicherungen, falls Daten unbeabsichtigt beschädigt oder gelöscht wurden.

Die Datenbanksicherungen werden automatisch ohne vorherige Registrierung oder zusätzliche Kosten erstellt. Durch diese automatisierten Sicherungen und die Point-in-Time-Wiederherstellung lassen sich Datenbanken kostenlos und ohne Verwaltungsaufwand vor unbeabsichtigter Beschädigung oder Löschung schützen – ganz gleich, wodurch diese verursacht wurde.


|Aufgabe (Portal) | PowerShell | REST |
|:--|:--|:--|
| [Restore a SQL database to a previous point in time](sql-database-point-in-time-restore-portal.md) (Wiederherstellen eines früheren Zustands einer SQL-Datenbank) | [PowerShell](sql-database-point-in-time-restore-powershell.md) | [REST (createMode=PointInTimeRestore)](https://msdn.microsoft.com/library/azure/mt163685.aspx) |
| [Restore a deleted SQL database](sql-database-restore-deleted-database-portal.md) (Wiederherstellen einer gelöschten SQL-Datenbank) | [PowerShell](sql-database-restore-deleted-database-powershell.md) | [REST (createMode=Restore)](https://msdn.microsoft.com/library/azure/mt163685.aspx)|



## Grundlegendes zu automatischen Sicherungen

Alle Basic-, Standard- und Premium-Datenbanken werden durch automatische Sicherungen geschützt. Vollständige Sicherungen werden wöchentlich, differenzielle Sicherungen täglich und Protokollsicherungen alle fünf Minuten erstellt. Die erste vollständige Sicherung wird unmittelbar nach der Datenbankerstellung geplant. Sie wird üblicherweise innerhalb von 30 Minuten abgeschlossen, kann aber auch länger dauern. Falls die Datenbank bereits groß ist (etwa, weil sie durch Kopieren einer Datenbank oder durch Wiederherstellen einer großen Datenbank erstellt wurde), dauert die erste vollständige Sicherung unter Umständen länger. Nach der ersten vollständigen Sicherung werden alle weiteren Sicherungen automatisch geplant und im Hintergrund verwaltet. Der genaue Zeitpunkt für vollständige und differenzielle Sicherungen wird vom System unter Berücksichtigung allgemeiner Lastaspekte bestimmt. Die Sicherungsdateien werden in einem georedundanten Speicherkonto mit Lesezugriff (RA-GRS) gespeichert, um die Verfügbarkeit für eine Notfallwiederherstellung zu gewährleisten.


## Wiederherstellungsdauer einer Point-in-Time-Wiederherstellung

Die Zeit, die zum Wiederherstellen einer Datenbank benötigt wird, hängt von zahlreichen Faktoren ab. Hierzu zählen unter anderem die Größe der Datenbank, der ausgewählte Zeitpunkt und die Menge an Aktivitäten, die ausgeführt werden müssen, um den Zustand des ausgewählten Zeitpunkts wiederherzustellen. Bei einer sehr großen und/oder sehr aktiven Datenbank kann die Wiederherstellung mehrere Stunden dauern. Im Zuge der Wiederherstellung einer Datenbank wird auf dem Server mit der ursprünglichen Datenbank immer eine neue Datenbank erstellt. Die wiederhergestellte Datenbank muss daher einen neuen Namen erhalten.


Die Datenbank kann für eine beliebige Leistungsebene und einen beliebigen elastischen Pool wiederhergestellt werden. Vergewissern Sie sich, dass Sie für den Server oder Pool über ein ausreichendes DTU-Kontingent verfügen, und denken Sie daran, dass die Wiederherstellung eine neue Datenbank erstellt, deren Dienst- und Leistungsebene sich unter Umständen vom aktuellen Zustand der Livedatenbank unterscheiden. Nach Abschluss des Vorgangs verfügen Sie über eine wiederhergestellte, uneingeschränkt zugängliche Onlinedatenbank, für die reguläre Kosten anfallen (abhängig von Dienst- und Leistungsebene). Sie können die wiederhergestellte Datenbank entweder als Ersatz für die ursprüngliche Datenbank verwenden oder Daten daraus abrufen und die ursprüngliche Datenbank damit aktualisieren. Wenn die wiederhergestellte Datenbank als Ersatz für die ursprüngliche Datenbank fungieren soll, vergewissern Sie sich, dass Leistungs- und Dienstebene angemessen sind, und skalieren Sie die Datenbank bei Bedarf entsprechend. Sie können die ursprüngliche Datenbank umbenennen und dann die wiederhergestellte Datenbank mit dem Namen der ursprünglichen Datenbank versehen. Verwenden Sie hierzu den Befehl „ALTER DATABASE“ in T-SQL. Wenn Sie Daten aus der wiederhergestellten Datenbank abrufen möchten, müssen Sie die dazu erforderlichen Wiederherstellungsskripts separat erstellen und ausführen. Der Wiederherstellungsvorgang kann zwar eine ganze Weile dauern, die Datenbank wird jedoch während der gesamten Zeit in der Datenbankliste angezeigt. Wenn Sie die Datenbank während der Wiederherstellung löschen, wird der Vorgang abgebrochen, und es fallen keine Kosten an.



## Wiederherstellen einer kürzlich gelöschten Datenbank

Wenn Sie eine Basic-, Standard- oder Premium-Datenbank löschen, wird die letzte Sicherung für den normalen Aufbewahrungszeitraum aufbewahrt, sodass Sie die Datenbank in dem Zustand wiederherstellen können, der zum Zeitpunkt der Löschung vorlag.

Bei gelöschten Datenbanken ist als Wiederherstellungszeitpunkt der Zeitpunkt der Datenbanklöschung vorgegeben. Auch beim Wiederherstellen einer gelöschten Datenbank gilt: Die Wiederherstellung kann nur auf dem Server erfolgen, auf dem sich auch die ursprüngliche Datenbank befand. Berücksichtigen Sie diesen Punkt, wenn Sie einen Server löschen, da Datenbanken, die sich auf diesem Server befunden haben, nach dem Löschen des Servers nicht wiederhergestellt werden können.

## Sichern/Wiederherstellen im Vergleich zu Kopieren/Exportieren/Importieren

Die Point-in-Time-Wiederherstellung ist die empfohlene Vorgehensweise für die Wiederherstellung von Basic-, Standard- und Premium-Datenbanken nach unbeabsichtigtem Datenverlust oder unbeabsichtigter Beschädigung. Sicherung und Wiederherstellung bieten folgende Vorteile: Sie sind kostengünstiger. (Gebühren fallen nur bei übermäßigen Sicherungen an. Im Gegensatz dazu fallen Kosten für die Datenbankkopie, die zur Gewährleistung eines Exports mit konsistenten Transaktionen benötigt wird, sowie für die Speicherung der BACPAC-Datei an.) Sie erfordern keinerlei Verwaltung. (Die Sicherungen werden automatisch erstellt. Exporte müssen dagegen manuell verwaltet und geplant werden.) Sie bieten ein besseres RPO. (Die Wiederherstellung kann im Gegensatz zum Kopieren/Exportieren/Importieren minutengenau für einen bestimmten Zeitpunkt erfolgen.) Sie ermöglichen eine schnellere Wiederherstellung. (Das Wiederherstellen von Sicherungen ist in der Regel deutlich schneller als das Importieren, da beim Importieren das Schema erstellt, Indizes deaktiviert, Daten geladen und dann Indizes einzeln für jede Tabelle erstellt werden müssen.) Für eine langfristige, über die Aufbewahrungsdauer hinausgehende Archivierung wird allerdings weiterhin die Verwendung von Kopieren und Exportieren empfohlen.


## Zusammenfassung

Automatische Sicherungen und die Point-in-Time-Wiederherstellung schützen Ihre Datenbanken vor unbeabsichtigter Beschädigung oder Löschung von Daten. Diese kostenlose, verwaltungsfreie Lösung steht für alle SQL-Datenbanken zur Verfügung. Verglichen mit der Alternative (Kopieren/Exportieren/Importieren) stellen Sicherung und Wiederherstellung eine deutliche Verbesserung für kurzfristige Wiederherstellungen dar. Es empfiehlt sich, die Point-in-Time-Wiederherstellung in Ihre Geschäftskontinuitätsstrategie zu integrieren. Die Verwendung der Exportoption wird dagegen nur für die langfristige Archivierung oder für die Datenmigration empfohlen.



## Zusätzliche Ressourcen

- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [Geografische Wiederherstellung](sql-database-geo-restore.md)
- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- [Abschließen der wiederhergestellten Azure SQL-Datenbank](sql-database-recovered-finalize.md)

<!---HONumber=AcomDC_0511_2016-->