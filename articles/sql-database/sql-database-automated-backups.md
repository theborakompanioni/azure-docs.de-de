<properties
   pageTitle="Geschäftskontinuität für die Cloud – integrierte Sicherung – SQL-Datenbank | Microsoft Azure"
   description="Informieren Sie sich über integrierte Sicherungen der SQL-Datenbank, mit denen Sie einen früheren Zustand einer Azure SQL-Datenbank wiederherstellen oder eine Datenbank in eine neue Datenbank in einer geografischen Region kopieren können (bis zu 35 Tage)."
   services="sql-database"
   documentationCenter=""
   authors="carlrabeler"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-bcdr"
   ms.date="06/16/2016"
   ms.author="carlrab"/>

# Übersicht: Automatisierte SQL-Datenbanksicherungen

Der Azure SQL-Datenbankdienst schützt alle Datenbanken mit einer automatisierten Sicherung, die sieben Tage (Basic) bzw. 35 Tage (Standard und Premium) lang aufbewahrt wird. Weitere Informationen über die in den einzelnen Tarifen verfügbaren Features finden Sie unter [Tarife](sql-database-service-tiers.md).

Die Datenbanksicherungen werden automatisch ohne vorherige Registrierung oder zusätzliche Kosten erstellt. Durch diese automatisierten Sicherungen und die Point-in-Time-Wiederherstellung lassen sich Datenbanken kostenlos und ohne Verwaltungsaufwand vor unbeabsichtigter Beschädigung oder Löschung schützen – ganz gleich, wodurch diese verursacht wurde. Mit diesen automatisierten Sicherungen können Sie Point-in-Time-Wiederherstellungen durchführen und eine Datenbank nach versehentlicher Löschung oder Beschädigung von Daten wiederherstellen.

## Kosten der automatisierten Sicherung

Microsoft Azure SQL-Datenbanken bieten bis zu 200 Prozent Ihres maximal bereitgestellten Sicherungsdatenbankspeichers ohne zusätzliche Kosten. Wenn Sie z. B. über eine Standard-DB-Instanz mit einer bereitgestellten Größe von 250 GB verfügen, werden Ihnen ohne zusätzliche Kosten 500 GB Sicherungsspeicher bereitgestellt. Wenn die maximale Größe Ihres bereitgestellten Sicherungsspeichers überschritten wird, können Sie sich entweder an den Azure-Support wenden, um den Aufbewahrungszeitraum zu verkürzen, oder zusätzlichen Sicherungsspeicher erwerben, für den die standardmäßigen Gebühren für geografisch redundanten Speicher mit Lesezugriff (Read-Access Geographically Redundant Storage, RA-GRS) anfallen.

## Details zur automatischen Sicherung

Alle Basic-, Standard- und Premium-Datenbanken werden durch automatische Sicherungen geschützt. Vollständige Datenbanksicherungen werden wöchentlich, differenzielle Datenbanksicherungen stündlich und Transaktionsprotokollsicherungen alle 5 Minuten ausgeführt. Die erste vollständige Sicherung wird unmittelbar nach der Datenbankerstellung geplant. Sie wird üblicherweise innerhalb von 30 Minuten abgeschlossen, kann aber auch länger dauern. Falls die Datenbank bereits groß ist (etwa, weil sie durch Kopieren einer Datenbank oder durch Wiederherstellen einer großen Datenbank erstellt wurde), dauert die erste vollständige Sicherung unter Umständen länger. Nach der ersten vollständigen Sicherung werden alle weiteren Sicherungen automatisch geplant und im Hintergrund verwaltet. Der genaue Zeitpunkt für vollständige und differenzielle Sicherungen wird vom System unter Berücksichtigung allgemeiner Lastaspekte bestimmt.

## Georedundanz

Die Sicherungsdateien werden in einem georedundanten Speicherkonto mit Lesezugriff (RA-GRS) gespeichert, um die Verfügbarkeit für eine Notfallwiederherstellung zu gewährleisten. Die folgende Abbildung veranschaulicht die Georeplikation von wöchentlichen und täglichen Sicherungen in einem georedundanten Speicherkonto mit Lesezugriff (RA-GRS), um die Verfügbarkeit für eine Notfallwiederherstellung zu gewährleisten.

![Geowiederherstellung](./media/sql-database-geo-restore/geo-restore-1.png)

## Verwenden automatisierter Sicherungen

Während der [Aufbewahrungsdauer](sql-database-service-tiers.md) können Sie [eine Datenbank aus vom Dienst initiierten Sicherungen wiederherstellen](sql-database-recovery-using-backups.md). Diese Wiederherstellung kann erfolgen als:

- Eine neue Datenbank auf dem gleichen logischen Server, die auf den Zustand zu einem angegebenen Zeitpunkt innerhalb der Aufbewahrungsdauer wiederhergestellt wird.
- Eine Datenbank auf dem gleichen Server, die auf den Zustand zum Zeitpunkt des Löschens einer gelöschten Datenbank wiederhergestellt wird.
- Eine neue Datenbank auf einem beliebigen logischen Server in einer beliebigen Region, die auf den Zustand der neuesten täglichen Sicherung im georeplizierten Blobspeicher (RA-GRS) wiederhergestellt wird.

Sie können die [automatisierten SQL-Datenbanksicherungen](sql-database-automated-backups.md) auch verwenden, um auf einem beliebigen logischen Server in einer beliebigen Region eine [Datenbankkopie](sql-database-copy.md) zu erstellen, die transaktional mit der aktuellen SQL-Datenbank konsistent ist. Sie können die Datenbankkopie [in eine BACPAC-Datei exportieren](sql-database-export.md), um eine transaktional konsistente Kopie der Datenbank für die langfristige Archivierung über die für Sie gültige Aufbewahrungsdauer hinaus zu erhalten oder um eine Kopie der Datenbank an eine lokale Instanz oder eine Azure VM-Instanz von SQL Server zu übertragen.

## Was geschieht mit meiner Aufbewahrungsdauer für den Wiederherstellungspunkt, wenn ich ein Downgrade oder Upgrade des Tarifs durchführe?

Nach einem Downgrade auf einen niedrigeren Tarif (verbunden mit einer niedrigeren Leistungsstufe) wird die Aufbewahrungsdauer für den Wiederherstellungspunkt unverzüglich auf die Aufbewahrungsdauer des aktuellen Datenbanktarifs verkürzt. Wenn ein Tarifupgrade durchgeführt wird, beginnt die Verlängerung der Aufbewahrungsdauer erst nach dem Upgrade der Datenbank. Wenn die Datenbank beispielsweise auf Basic herabgestuft wird, wird die Aufbewahrungsdauer sofort von 35 Tagen auf 7 Tage verkürzt, und alle Wiederherstellungspunkte mit einem Alter von mehr als 35 Tagen sind nicht mehr verfügbar. Wenn in der Folge wieder ein Upgrade auf Standard oder Premium erfolgt, beginnt die Aufbewahrungsdauer bei 7 Tagen und erweitert sich nach und nach auf 35 Tage.

## Wie lang ist die Aufbewahrungsdauer für eine gelöschte Datenbank? 
Die Aufbewahrungsdauer wird anhand der Dienstebene der Datenbank, während sie vorhanden war, oder der Anzahl der Tage, die sie vorhanden war, berechnet, je nachdem, welcher Wert kleiner ist.

> [AZURE.IMPORTANT] Wenn Sie eine Azure SQL-Datenbankserverinstanz löschen, werden auch alle dazugehörigen Datenbanken gelöscht und können nicht wiederhergestellt werden. Derzeit wird das Wiederherstellen gelöschter Server nicht unterstützt.

## Nächste Schritte

- Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md).
- Informationen über schnellere Wiederherstellungsoptionen finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md).
- Informationen zum Verwenden automatisierter Sicherungen für die Archivierung finden Sie unter [Datenbankkopie](sql-database-copy.md).
- Eine Übersicht zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).

<!---HONumber=AcomDC_0720_2016-->