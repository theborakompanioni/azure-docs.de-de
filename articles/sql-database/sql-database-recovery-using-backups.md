---
title: Geschäftskontinuität in der Cloud – Wiederherstellen einer gelöschten Datenbank – SQL-Datenbank | Microsoft Docs
description: Hier finden Sie Informationen zur Point-in-Time-Wiederherstellung, mit der Sie ein Rollback für eine Azure SQL-Datenbank durchführen können (bis zu 35 Tage).
services: sql-database
documentationcenter: ''
author: stevestein
manager: jhubbard
editor: monicar

ms.service: sql-database
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/01/2016
ms.author: sstein

---
# Wiederherstellen einer Azure SQL-Datenbank mit automatisierten Datenbanksicherungen
SQL-Datenbank bietet drei Optionen für die Datenbankwiederherstellung mit [automatisierten SQL-Datenbanksicherungen](sql-database-automated-backups.md). Während der [Aufbewahrungsdauer](sql-database-service-tiers.md) können Sie eine Datenbank aus vom Dienst initiierten Sicherungen wiederherstellen. Diese Wiederherstellung kann erfolgen als:

* Eine neue Datenbank auf dem gleichen logischen Server, die auf den Zustand zu einem angegebenen Zeitpunkt innerhalb der Aufbewahrungsdauer wiederhergestellt wird.
* Eine Datenbank auf dem gleichen Server, die auf den Zustand zum Zeitpunkt des Löschens einer gelöschten Datenbank wiederhergestellt wird.
* Eine neue Datenbank auf einem beliebigen logischen Server in einer beliebigen Region, die auf den Zustand der neuesten täglichen Sicherung im georeplizierten Blobspeicher (RA-GRS) wiederhergestellt wird.

Sie können die [automatisierten SQL-Datenbanksicherungen](sql-database-automated-backups.md) auch verwenden, um auf einem beliebigen logischen Server in einer beliebigen Region eine [Datenbankkopie](sql-database-copy.md) zu erstellen, die im Hinblick auf Transaktionen mit der aktuellen SQL-Datenbank konsistent ist. Sie können die Datenbankkopie [in eine BACPAC-Datei exportieren](sql-database-export.md), um eine transaktional konsistente Kopie der Datenbank für die langfristige Archivierung über die für Sie gültige Aufbewahrungsdauer hinaus zu erhalten oder um eine Kopie der Datenbank an eine lokale Instanz oder eine Azure VM-Instanz von SQL Server zu übertragen.

## Wiederherstellungszeit
Die Zeit, die zum Wiederherstellen einer Datenbank mit automatisierten Sicherungen benötigt wird, hängt von verschiedenen Faktoren ab. Dies sind:

* Größe der Datenbank
* Leistungsstufe der Datenbank
* Anzahl der beteiligten Transaktionsprotokolle
* Menge der erneut auszuführenden Aktivitäten, um den Wiederherstellungspunkt wiederherzustellen
* Netzwerkbandbreite, sofern die Wiederherstellung in einer anderen Region erfolgt
* Anzahl der gleichzeitigen Wiederherstellungsanforderungen, die aktuell in der Zielregion verarbeitet werden
  
  Bei einer sehr großen und/oder sehr aktiven Datenbank kann die Wiederherstellung mehrere Stunden dauern. Wenn es sich um einen längeren Ausfall in einer Region handelt, müssen möglicherweise andere Regionen eine große Anzahl von Geowiederherstellungsanforderungen verarbeiten. Wenn eine große Anzahl von Anforderungen vorliegt, kann dies die Wiederherstellungszeit für Datenbanken in dieser Region erhöhen. Der Großteil der Datenbankwiederherstellungen erfolgt innerhalb von 12 Stunden.
  
  Es gibt keine integrierte Funktion für Massenwiederherstellungen. Das Skript [Azure SQL-Datenbank: Vollständige Serverwiederherstellung](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) ist ein Beispiel für eine Möglichkeit, diese Aufgabe auszuführen.

> [!IMPORTANT]
> Um eine Wiederherstellung mithilfe von automatisierten Sicherungen durchzuführen, müssen Sie Mitglied der Rolle „SQL Server-Mitwirkender“ im Abonnement oder Besitzer des Abonnements sein. Sie können das Azure-Portal, PowerShell oder die REST-API zur Wiederherstellung verwenden. Die Nutzung von Transact-SQL ist nicht möglich.
> 
> 

## Point-in-Time-Wiederherstellung
Mit der Point-in-Time-Wiederherstellung können Sie mithilfe [automatisierter SQL-Datenbanksicherungen](sql-database-automated-backups.md) auf dem gleichen logischen Server eine vorhandene Datenbank als neue Datenbank mit einem früheren Datenbankzustand wiederherstellen. Sie können die vorhandene Datenbank nicht überschreiben. Die Wiederherstellung eines früheren Zustands kann im [Azure-Portal](sql-database-point-in-time-restore-portal.md), mit [PowerShell](sql-database-point-in-time-restore-powershell.md) oder mit der [REST-API](https://msdn.microsoft.com/library/azure/mt163685.aspx) durchgeführt werden.

> [!div class="op_single_selector"]
> * [Point-in-Time-Wiederherstellung: im Azure-Portal](sql-database-point-in-time-restore-portal.md)
> * [Point-in-Time-Wiederherstellung: mit PowerShell](sql-database-point-in-time-restore-powershell.md)
> 
> 

Die Datenbank kann für eine beliebige Leistungsstufe und einen beliebigen elastischen Pool wiederhergestellt werden. Sie müssen über ein ausreichend großes DTU-Kontingent (Database Throughput Unit) auf dem logischen Server oder im elastischen Pool verfügen. Denken Sie daran, dass bei der Wiederherstellung eine neue Datenbank erstellt wird, deren Tarif- und Leistungsstufe sich unter Umständen vom aktuellen Zustand der Livedatenbank unterscheidet. Nach Abschluss des Vorgangs verfügen Sie über eine wiederhergestellte, uneingeschränkt zugängliche Onlinedatenbank, für die reguläre Kosten anfallen (abhängig von Tarif- und Leistungsstufe). Kosten entstehen erst, wenn die Datenbankwiederherstellung abgeschlossen ist.

Im Allgemeinen wird beim Wiederherstellen der Datenbank der Zustand zu einem früheren Zeitpunkt wiederhergestellt. Sie können die wiederhergestellte Datenbank entweder als Ersatz für die ursprüngliche Datenbank verwenden oder Daten daraus abrufen und die ursprüngliche Datenbank damit aktualisieren.

* ***Ersetzung der Datenbank:*** Wenn die wiederhergestellte Datenbank als Ersatz für die ursprüngliche Datenbank fungieren soll, vergewissern Sie sich, dass Leistungs- und Tarifebene angemessen sind, und skalieren Sie die Datenbank bei Bedarf entsprechend. Sie können die ursprüngliche Datenbank umbenennen und dann die wiederhergestellte Datenbank mit dem Namen der ursprünglichen Datenbank versehen. Verwenden Sie hierzu den Befehl „ALTER DATABASE“ in T-SQL.
* ***Wiederherstellung der Daten:*** Wenn Sie Daten aus der wiederhergestellten Datenbank abrufen möchten, um einen Benutzer- oder Anwendungsfehler zu korrigieren, müssen Sie die entsprechende Datenwiederherstellungsskripts separat schreiben und ausführen, um die Daten aus der wiederhergestellten Datenbank zu extrahieren und sie in der ursprünglichen Datenbank wiederherzustellen. Der Wiederherstellungsvorgang kann zwar eine ganze Weile dauern, die wiederherstellende Datenbank wird jedoch während der gesamten Zeit in der Datenbankliste angezeigt. Wenn Sie diese Datenbank während der Wiederherstellung löschen, wird der Vorgang abgebrochen, und es fallen keine Kosten für die Datenbank an, die die Wiederherstellung nicht abgeschlossen hat.

Ausführliche Informationen zum Verwenden von Point-in-Time-Wiederherstellungen zur Behebung von Benutzer- und Anwendungsfehlern finden Sie unter [Point-in-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore).

## Wiederherstellen einer gelöschten Datenbank
Mit dem Verfahren zum Wiederherstellen einer gelöschten Datenbank können Sie eine gelöschte Datenbank mithilfe der [automatisierten SQL-Datenbanksicherung](sql-database-automated-backups.md) in dem Zustand wiederherstellen, den sie zum Zeitpunkt des Löschens hatte. Die Wiederherstellung erfolgt dabei auf dem gleichen logischen Server.

> [!IMPORTANT]
> Wenn Sie eine Azure SQL-Datenbankserverinstanz löschen, werden auch alle dazugehörigen Datenbanken gelöscht und können nicht wiederhergestellt werden. Derzeit wird das Wiederherstellen gelöschter Server nicht unterstützt.
> 
> 

Für die wiederhergestellte Datenbank können Sie den gleichen oder einen neuen Namen verwenden. Sie können dazu das [Azure-Portal](sql-database-restore-deleted-database-portal.md), [PowerShell](sql-database-restore-deleted-database-powershell.md) oder [REST (createMode=Restore)](https://msdn.microsoft.com/library/azure/mt163685.aspx) verwenden.

> [!div class="op_single_selector"]
> * [Wiederherstellen einer gelöschten Datenbank: im Azure-Portal](sql-database-restore-deleted-database-portal.md)
> * [Wiederherstellen einer gelöschten Datenbank: mit PowerShell](sql-database-restore-deleted-database-powershell.md)
> 
> 

## Geowiederherstellung
Mit der Geowiederherstellung können Sie eine SQL-Datenbank auf einem beliebigen Server in einer beliebigen Azure-Region aus der letzten georeplizierten [automatisierten täglichen Sicherung](sql-database-automated-backups.md) wiederherstellen. Die Geowiederherstellung verwendet eine georedundante Sicherung als Quelle und kann selbst dann zum Wiederherstellen einer Datenbank verwendet werden, wenn die Datenbank oder das Rechenzentrum aufgrund eines Ausfalls nicht mehr verfügbar ist. Sie können dazu das [Azure-Portal](sql-database-geo-restore-portal.md), [PowerShell](sql-database-geo-restore-powershell.md) oder [REST (createMode=Recovery)](https://msdn.microsoft.com/library/azure/mt163685.aspx) verwenden.

> [!div class="op_single_selector"]
> * [Geowiederherstellung: im Azure-Portal](sql-database-geo-restore-portal.md)
> * [Geowiederherstellung: mit PowerShell](sql-database-geo-restore-powershell.md)
> 
> 

Die Geowiederherstellung ist die Standardoption für die Wiederherstellung, wenn eine Datenbank aufgrund eines Incidents in der Region, in der die Datenbank gehostet wird, nicht verfügbar ist. Wenn Ihre Datenbankanwendung durch einen umfangreichen Incident in einer Region nicht mehr verfügbar ist, können Sie die Geowiederherstellung einsetzen, um eine Datenbank aus der letzten Sicherung auf einem Server in einer beliebigen anderen Region wiederherzustellen. Alle Sicherungen sind georepliziert, und es kann eine Verzögerung zwischen der Sicherungserstellung und der Georeplikation auf dem Azure-Blob in einer anderen Region auftreten. Diese Verzögerung kann bis zu einer Stunde betragen. Folglich kann bei einem Notfall ein Datenverlust von bis zu einer Stunde auftreten, was einem RPO-Wert von bis zu einer Stunde entspricht. Das folgende Beispiel zeigt die Wiederherstellung der Datenbank aus der letzten täglichen Sicherung.

![Geowiederherstellung](./media/sql-database-geo-restore/geo-restore-2.png)

Ausführliche Informationen zum Verwenden der Geowiederherstellung nach einem Ausfall finden Sie unter [Wiederherstellen nach einem Ausfall](sql-database-disaster-recovery.md).

> [!IMPORTANT]
> Auch wenn die Geowiederherstellung in allen Dienstebenen verfügbar ist, ist sie die elementarste Notfallwiederherstellungslösung, die in der SQL-Datenbank verfügbar ist. Sie weist die längste RPO und die längste geschätzte Wiederherstellungszeit (ERT) auf. Für Basic-Datenbanken mit einer maximalen Größe von 2 GB bietet die Geowiederherstellung eine angemessene Notfallwiederherstellungslösung mit einer ERT von 12 Stunden. Wenn bei größeren Standard- oder Premium-Datenbanken erheblich kürzere Wiederherstellungszeiten gewünscht werden, oder wenn Sie die Wahrscheinlichkeit eines Datenverlusts reduzieren möchten, sollten sie die aktive Georeplikation in Erwägung ziehen. Die aktive Georeplikation bietet eine niedrigere RPO und ERT, da sie nur das Initiieren eines Failovers auf eine kontinuierlich replizierte sekundäre Datenbank erfordert. Weitere Informationen finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md).
> 
> 

## Programmgesteuerte Wiederherstellung mit automatisierten Sicherungen
Wie bereits zuvor erwähnt, kann die Datenbankwiederherstellung nicht nur über das Azure-Portal, sondern auch programmgesteuert mit Azure PowerShell und mit der REST-API ausgeführt werden. In den folgenden Tabellen werden die verfügbaren Befehle beschrieben.

### PowerShell
| Cmdlet | Beschreibung |
| --- | --- |
| [Get-AzureRmSqlDatabase](https://msdn.microsoft.com/de-DE/library/azure/mt603648.aspx) |Ruft mindestens eine Datenbank ab. |
| [Get-AzureRMSqlDeletedDatabaseBackup](https://msdn.microsoft.com/de-DE/library/azure/mt693387.aspx) |Ruft eine gelöschte Datenbank ab, die Sie wiederherstellen können. |
| [Get-AzureRmSqlDatabaseGeoBackup](https://msdn.microsoft.com/library/azure/mt693388.aspx) |Ruft eine georedundante Sicherung einer Datenbank ab. |
| [Restore-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390.aspx) |Stellt eine SQL-Datenbank wieder her. |
|  | |

### REST-API
| API | Beschreibung |
| --- | --- |
| [REST (createMode=Recovery)](https://msdn.microsoft.com/library/azure/mt163685.aspx) |Stellt eine Datenbank wieder her. |
| [Get Create or Update Database Status](https://msdn.microsoft.com/library/azure/mt643934.aspx) |Ruft den Status während eines Wiederherstellungsvorgangs ab. |
|  | |

## Zusammenfassung
Automatische Sicherungen schützen Ihre Datenbanken vor Benutzer- und Anwendungsfehlern, versehentlichen Datenbanklöschungen und längeren Ausfällen. Diese kostenlose, verwaltungsfreie Lösung steht für alle SQL-Datenbanken zur Verfügung.

## Nächste Schritte
* Eine Übersicht und verschiedene Szenarien zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).
* Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md).
* Informationen über schnellere Wiederherstellungsoptionen finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md).
* Informationen zum Verwenden automatisierter Sicherungen für die Archivierung finden Sie unter [Datenbankkopie](sql-database-copy.md).

<!---HONumber=AcomDC_0803_2016-->