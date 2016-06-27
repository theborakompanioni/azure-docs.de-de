<properties
   pageTitle="Geschäftskontinuität in der Cloud – Geowiederherstellung | Microsoft Azure"
   description="Erfahren Sie, wie Azure SQL-Datenbank die Geschäftskontinuität in der Cloud sowie die Datenbankwiederherstellung unterstützt und dafür sorgt, dass geschäftskritische Cloudanwendungen in Betrieb gehalten werden."
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

# Übersicht: SQL-Datenbank – Geowiederherstellung

> [AZURE.SELECTOR]
- [Übersicht](sql-database-geo-restore.md)
- [Azure-Portal](sql-database-geo-restore-portal.md)
- [PowerShell](sql-database-geo-restore-powershell.md)

Mit der Geowiederherstellung können Sie eine SQL-Datenbank aus der letzten täglichen Sicherung wiederherstellen. Sie wird ohne zusätzliche Kosten automatisch für alle Diensttarife aktiviert. Die Geowiederherstellung verwendet eine georedundante Sicherung als Quelle und kann selbst dann zum Wiederherstellen einer Datenbank verwendet werden, wenn die Datenbank oder das Rechenzentrum aufgrund eines Ausfalls nicht mehr verfügbar ist.

Durch die Einleitung der Geowiederherstellung können Sie eine neue SQL-Datenbank auf einem beliebigen Server in einer beliebigen Azure-Region erstellen.

> [AZURE.NOTE] Sie können auch [REST (createMode=Restore)](https://msdn.microsoft.com/library/azure/mt163685.aspx) verwenden.


Die Geowiederherstellung ist die Standardoption für die Wiederherstellung, wenn eine Datenbank aufgrund eines Incidents in der Region, in der die Datenbank gehostet wird, nicht verfügbar ist. Die Datenbank kann auf einem beliebigen Server in jeder Azure-Region erstellt werden. Die Geowiederherstellung nutzt [automatisierte Datenbanksicherungen](sql-database-automated-backups.md) in geografisch redundantem Azure-Speicher, und Daten werden auf der Grundlage der geografisch replizierten Sicherungskopie wiederhergestellt, wodurch sie gegen Speicherausfälle in der primären Region abgesichert ist.



## Geowiederherstellung im Detail

Die Geowiederherstellung verwendet die gleiche Technologie wie die Point-in-Time-Wiederherstellung, allerdings mit einem wichtigen Unterschied. Sie stellt die Datenbank aus einer Kopie der neuesten täglichen Sicherung im georeplizierten Blobspeicher (RA-GRS) wieder her. Für jede aktive Datenbank behält der Dienst eine Sicherungskette bei, die eine vollständige Sicherung pro Woche, mehrere differenzielle Sicherungen pro Tag sowie Transaktionsprotokolle umfasst, die alle fünf Minuten gespeichert werden. Diese Blobs sind georepliziert, was gewährleistet, dass tägliche Sicherungen auch nach einem massiven Ausfall in der primären Region verfügbar sind. Im Folgenden wird die Georeplikation von wöchentlichen und täglichen Sicherungen gezeigt, die in die Speichercontainer kopiert werden.

![Geowiederherstellung](./media/sql-database-geo-restore/geo-restore-1.png)


Wenn Ihre Datenbankanwendung durch einen umfangreichen Incident in einer Region nicht mehr verfügbar ist, können Sie die Geowiederherstellung einsetzen, um eine Datenbank aus der letzten Sicherung auf einem Server in einer beliebigen anderen Region wiederherzustellen. Alle Sicherungen sind georepliziert, und es kann eine Verzögerung zwischen der Sicherungserstellung und der Georeplikation auf dem Azure-Blob in einer anderen Region auftreten. Diese Verzögerung kann bis zu einer Stunde betragen. Folglich kann bei einem Notfall ein Datenverlust von bis zu einer Stunde auftreten, was einem RPO-Wert von bis zu einer Stunde entspricht. Das folgende Beispiel zeigt die Wiederherstellung der Datenbank aus der letzten täglichen Sicherung.


![Geowiederherstellung](./media/sql-database-geo-restore/geo-restore-2.png)



## Wiederherstellungszeit für eine Geowiederherstellung

Die Wiederherstellungszeit wird durch verschiedene Faktoren beeinflusst: die Größe und Leistungsebene der Datenbank und die Anzahl der gleichzeitigen Wiederherstellungsanforderungen, die in der Zielregion verarbeitet werden. Wenn es sich um einen längeren Ausfall in einer Region handelt, müssen möglicherweise andere Regionen eine große Anzahl von Geowiederherstellungsanforderungen verarbeiten. Wenn eine große Anzahl von Anforderungen vorliegt, kann dies die Wiederherstellungszeit für Datenbanken in dieser Region erhöhen.


## Zusammenfassung

Auch wenn die Geowiederherstellung in allen Diensttarifen verfügbar ist, ist sie die elementarste Notfallwiederherstellungslösung, die in der SQL-Datenbank verfügbar ist. Sie weist die längste RPO und die längste geschätzte Wiederherstellungszeit (ERT) auf. Für Basic-Datenbanken mit einer maximalen Größe von 2 GB bietet die Geowiederherstellung eine angemessene Notfallwiederherstellungslösung mit einer ERT von 12 Stunden. Wenn bei größeren Standard- oder Premium-Datenbanken erheblich kürzere Wiederherstellungszeiten gewünscht werden, oder wenn Sie die Wahrscheinlichkeit eines Datenverlusts reduzieren möchten, sollten sie die aktive Georeplikation in Erwägung ziehen. Die aktive Georeplikation bietet eine niedrigere RPO und ERT, da sie nur das Initiieren eines Failovers auf eine kontinuierlich replizierte sekundäre Datenbank erfordert. Weitere Informationen finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md).

## Nächste Schritte

- [Abschließen der wiederhergestellten Azure SQL-Datenbank](sql-database-recovered-finalize.md)
- [Geowiederherstellung einer Azure SQL-Datenbank aus einer georedundanten Sicherung über das Azure-Portal](sql-database-geo-restore-portal.md)
- [Wiederherstellen einer Azure SQL-Datenbank aus einer georedundanten Sicherung mit PowerShell](sql-database-geo-restore-powershell.md)

## Weitere Ressourcen

- [BCDR in SQL-Datenbank – Häufig gestellte Fragen](sql-database-bcdr-faq.md)
- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [Point-in-Time-Wiederherstellung](sql-database-point-in-time-restore.md)
- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md)

<!---HONumber=AcomDC_0615_2016-->