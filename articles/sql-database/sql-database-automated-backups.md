<properties
   pageTitle="Geschäftskontinuität für die Cloud – integrierte Sicherung – SQL-Datenbank | Microsoft Azure"
   description="Informieren Sie sich über integrierte Sicherungen der SQL-Datenbank, mit denen Sie einen früheren Zustand einer Azure SQL-Datenbank wiederherstellen oder eine Datenbank in eine neue Datenbank in einer geografischen Region kopieren können (bis zu 35 Tage)."
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
   ms.workload="sqldb-features"
   ms.date="06/09/2016"
   ms.author="sstein"/>

# Übersicht: Automatisierte SQL-Datenbanksicherungen

Der Azure SQL-Datenbankdienst schützt alle Datenbanken mit einer automatisierten Sicherung, die sieben (Basic), 14 (Standard) bzw. 35 Tage (Premium) lang aufbewahrt wird. Mit diesen automatisierten Sicherungen können Sie Point-in-Time-Wiederherstellungen durchführen und eine Datenbank nach versehentlicher Löschung oder Beschädigung von Daten wiederherstellen.

Die Datenbanksicherungen werden automatisch ohne vorherige Registrierung oder zusätzliche Kosten erstellt. Durch diese automatisierten Sicherungen und die Point-in-Time-Wiederherstellung lassen sich Datenbanken kostenlos und ohne Verwaltungsaufwand vor unbeabsichtigter Beschädigung oder Löschung schützen – ganz gleich, wodurch diese verursacht wurde.

## Kosten der automatisierten Sicherung

Microsoft Azure SQL-Datenbanken bieten bis zu 200 Prozent Ihres maximal bereitgestellten Sicherungsdatenbankspeichers ohne zusätzliche Kosten. Wenn Sie z. B. über eine Standard-DB-Instanz mit einer bereitgestellten Größe von 250 GB verfügen, werden Ihnen ohne zusätzliche Kosten 500 GB Sicherungsspeicher bereitgestellt. Wenn die maximale Größe Ihres bereitgestellten Sicherungsspeichers überschritten wird, können Sie sich entweder an den Azure-Support wenden, um den Aufbewahrungszeitraum zu verkürzen, oder zusätzlichen Sicherungsspeicher erwerben, für den die standardmäßigen Gebühren für geografisch redundanten Speicher mit Lesezugriff (Read-Access Geographically Redundant Storage, RA-GRS) anfallen.

## Details zur automatischen Sicherung

Alle Basic-, Standard- und Premium-Datenbanken werden durch automatische Sicherungen geschützt. Vollständige Sicherungen werden wöchentlich, differenzielle Sicherungen täglich und Protokollsicherungen alle fünf Minuten erstellt. Die erste vollständige Sicherung wird unmittelbar nach der Datenbankerstellung geplant. Sie wird üblicherweise innerhalb von 30 Minuten abgeschlossen, kann aber auch länger dauern. Falls die Datenbank bereits groß ist (etwa, weil sie durch Kopieren einer Datenbank oder durch Wiederherstellen einer großen Datenbank erstellt wurde), dauert die erste vollständige Sicherung unter Umständen länger. Nach der ersten vollständigen Sicherung werden alle weiteren Sicherungen automatisch geplant und im Hintergrund verwaltet. Der genaue Zeitpunkt für vollständige und differenzielle Sicherungen wird vom System unter Berücksichtigung allgemeiner Lastaspekte bestimmt. Die Sicherungsdateien werden in einem georedundanten Speicherkonto mit Lesezugriff (RA-GRS) gespeichert, um die Verfügbarkeit für eine Notfallwiederherstellung zu gewährleisten.

## Weitere Ressourcen

- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [Geografische Wiederherstellung](sql-database-geo-restore.md)
- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md)

<!---HONumber=AcomDC_0615_2016-->