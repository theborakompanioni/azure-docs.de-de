<properties
   pageTitle="Sicherungen für SQL-Datenbank | Microsoft Azure"
   description="Informieren Sie sich über integrierte Datenbanksicherungen der SQL-Datenbank, mit denen Sie einen früheren Zustand einer Azure SQL-Datenbank wiederherstellen oder eine Datenbank in eine neue Datenbank in einer geografischen Region kopieren können (bis zu 35 Tage)."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="carlrab"/>

# Sicherungen für SQL-Datenbank

Mit SQL-Datenbank werden Datenbanksicherungen automatisch als Teil des Dienstangebots erstellt, ohne dass eine spezielle Auswahl erfolgen muss, und es fallen auch keine zusätzlichen Kosten an. Verwenden Sie Datenbanksicherungen, um den Stand einer Datenbank zu einem früheren Zeitpunkt wiederherzustellen. In diesem Artikel werden die Details der Funktion zum Sichern von Datenbanken in SQL-Datenbank beschrieben.

## Was ist eine Datenbanksicherung?  

Eine Datenbanksicherung ist eine Datei, in der Informationen zum Zustand der Datenbank zu einem bestimmten Zeitpunkt gespeichert werden. SQL-Datenbank erstellt [vollständige](https://msdn.microsoft.com/library/ms186289.aspx), [differenzielle](https://msdn.microsoft.com/library/ms175526.aspx) und [Transaktionsprotokoll](https://msdn.microsoft.com/library/ms191429.aspx)sicherungen. Wenn Sie den Zustand einer Datenbank zu einem bestimmten Zeitpunkt wiederherstellen, ermittelt der Dienst, welche Sicherungen wiederhergestellt werden müssen.

Datenbanksicherungen sind ein wesentlicher Bestandteil jeder Strategie für Geschäftskontinuität und Notfallwiederherstellung, da Ihre Daten vor versehentlichen Beschädigungen und Löschungen geschützt werden. Weitere Informationen finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).

## Georedundanter Speicher

Der SQL-Datenbank-Dienst speichert Datenbanksicherungsdateien in einem georedundanten Speicherkonto mit Lesezugriff (RA-GRS). Die RA-GRS-Funktion von Azure Storage repliziert die Sicherungsdateien in einem [gekoppelten Rechenzentrum](../best-practices-availability-paired-regions.md). Mit dieser Georeplikation wird sichergestellt, dass Sie eine Datenbank wiederherstellen können, falls Sie aus Ihrer primären Datenbankregion nicht auf die Datenbanksicherung zugreifen können. Im folgenden Beispiel erstellt SQL-Datenbank Datenbanksicherungen in der Region „USA, Osten“ und speichert sie unter einem RA-GRS-Konto. Anschließend führt Azure Storage die Georeplikation für die Sicherungen in ein gekoppeltes Rechenzentrum in der Region „USA, Westen“ durch.

![Geowiederherstellung](./media/sql-database-geo-restore/geo-restore-1.png)

>[AZURE.NOTE] In Azure Storage bezieht sich der Begriff *Replikation* auf das Kopieren von Dateien von einem Speicherort zum anderen. Die *Datenbankreplikation* der SQL bezieht sich auf das Beibehalten von mehreren sekundären Datenbanken, die mit einer primären Datenbank synchronisiert werden.

Weitere Informationen:
- Informationen zum georedundanten Speichern finden Sie unter [Azure Storage-Replikation](../storage/storage-redundancy.md).
- Informationen zum RA-GRS-Speicher finden Sie unter [Georedundanter Speicher mit Lesezugriff](../storage/storage-redundancy.md#read-access-geo-redundant-storage).

## Kosten für Datenbanksicherungen

Mit Microsoft Azure SQL-Datenbank werden bis zu 200 Prozent Ihres maximal bereitgestellten Datenbankspeichers ohne zusätzliche Kosten als Sicherungsspeicher zur Verfügung gestellt. Wenn Sie beispielsweise über eine Standard-Datenbankinstanz mit einer bereitgestellten Größe von 250 GB verfügen, können Sie ohne zusätzliche Kosten 500 GB Sicherungsspeicher nutzen. Falls Ihre Datenbank den bereitgestellten Sicherungsspeicher überschreitet, können Sie die Aufbewahrungsdauer reduzieren lassen, indem Sie sich an den Azure-Support wenden. Eine weitere Option ist die Bezahlung für zusätzlichen Sicherungsspeicher, für den die RA-GRS-Standardrate (Read-Access Geographically Redundant Storage, Geografisch redundanter Speicher mit Lesezugriff) berechnet wird.

## Zeitplan für Datenbanksicherung

Alle Basic-, Standard- und Premium-Datenbanken werden durch automatische Sicherungen geschützt. Vollständige Datenbanksicherungen werden wöchentlich, differenzielle Datenbanksicherungen stündlich und Transaktionsprotokollsicherungen alle fünf Minuten ausgeführt. Die erste vollständige Sicherung wird unmittelbar nach der Datenbankerstellung geplant. Sie wird normalerweise innerhalb von 30 Minuten abgeschlossen, aber der Vorgang kann auch länger dauern, wenn es sich um eine sehr große Datenbank handelt. Die erste Sicherung kann bei einer wiederhergestellten Datenbank oder einer Datenbankkopie beispielsweise länger dauern. Nach der ersten vollständigen Sicherung werden alle weiteren Sicherungen automatisch geplant und im Hintergrund verwaltet. Die genaue Zeitplanung für vollständige und [differenzielle](https://msdn.microsoft.com/library/ms175526.aspx) Datenbanksicherungen wird je nach der gesamten Systemworkload bestimmt.

## Aufbewahrungszeitraum von Datenbanksicherungen

Jede SQL-Datenbanksicherung wird sieben Tage (Basic) bzw. 35 Tage (Standard und Premium) aufbewahrt. Weitere Informationen zu den Funktionen, die für die einzelnen Tarife verfügbar sind, finden Sie unter [SQL-Datenbankoptionen und -leistung: Grundlegendes zum Angebot in den einzelnen Tarifen](sql-database-service-tiers.md).

### Was geschieht mit meiner Aufbewahrungsdauer für den Wiederherstellungspunkt, wenn ich ein Downgrade oder Upgrade des Tarifs durchführe?

Nach einem Downgrade auf einen niedrigeren Tarif (verbunden mit einer niedrigeren Leistungsstufe) wird die Aufbewahrungsdauer für den Wiederherstellungspunkt unverzüglich auf die Aufbewahrungsdauer des aktuellen Datenbanktarifs verkürzt. Wenn ein Tarifupgrade durchgeführt wird, beginnt die Verlängerung der Aufbewahrungsdauer erst nach dem Upgrade der Datenbank. Wenn eine Datenbank beispielsweise auf „Basic“ heruntergestuft wird, ändert sich der Aufbewahrungszeitraum von 35 Tagen in 7 Tage. Alle Wiederherstellungspunkte, die älter als sieben Tage sind, sind somit sofort nicht mehr verfügbar. Wenn Sie eine Datenbank auf „Standard“ oder „Premium“ heraufstufen, beginnt die Aufbewahrungsdauer mit 7 Tagen und erreicht dann 35 Tage.

### Wie lang ist die Aufbewahrungsdauer für eine gelöschte Datenbank? 

Die Aufbewahrungsdauer wird anhand der Dienstebene der Datenbank, während sie vorhanden war, oder der Anzahl der Tage, die sie vorhanden war, berechnet, je nachdem, welcher Wert kleiner ist.

> [AZURE.IMPORTANT] Wenn Sie eine Azure SQL-Datenbankserverinstanz löschen, werden auch alle Datenbanken der Instanz gelöscht und können nicht wiederhergestellt werden. Es ist nicht möglich, einen gelöschten Server wiederherzustellen.


## Häufige Verwendungen für Datenbanksicherungen

Die hauptsächliche Verwendung für Datenbanksicherungen ist die Möglichkeit, für eine Datenbank den Zustand zu einem bestimmten Zeitpunkt während der Aufbewahrungsdauer wiederherzustellen. Mit einer Datenbanksicherung können Sie für eine Datenbank einen bestimmten Zeitpunkt wiederherstellen, für eine gelöschte Datenbank den Zustand zum Löschzeitpunkt wiederherstellen oder eine Datenbank in einer anderen geografischen Region wiederherstellen.

- Informationen zur Datenbankwiederherstellung finden Sie unter [Wiederherstellen einer Azure SQL-Datenbank mit automatisierten Datenbanksicherungen](sql-database-recovery-using-backups.md).

Sie können eine Datenbanksicherung verwenden, um eine Datenbank auf einen logischen SQL-Server in einer beliebigen geografischen Region zu kopieren. Die Kopie ist in Bezug auf die Transaktion mit der aktuellen SQL-Datenbank konsistent.

- Informationen zum Kopieren einer Datenbank finden Sie unter [Datenbankkopie](sql-database-copy.md).

Außerdem können Sie automatisierte Sicherungen über den Aufbewahrungszeitraum hinaus archivieren, indem Sie eine Datenbankkopie erstellen, die Sie in eine [BACPAC-Datei exportieren](sql-database-export.md). Nachdem Sie über die BACPAC-Datei verfügen, können Sie sie in einem langfristigen Speicher archivieren und über den Aufbewahrungszeitraum hinaus beibehalten. Sie können auch die BACPAC-Datei zum Übertragen einer Kopie der Datenbank an SQL Server verwenden, entweder lokal oder auf einem virtuellen Azure-Computer (VM).

- Informationen zur Archivierung einer Datenbanksicherung finden Sie unter [Datenbankkopie](sql-database-copy.md).


## Verwandte Themen

### Szenarien

- Eine Übersicht zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).

### Features

Sie erhalten Informationen zu folgenden Themen:

- Informationen zur Wiederherstellung einer Datenbanksicherung finden Sie unter [Wiederherstellen einer Azure SQL-Datenbank mit automatisierten Datenbanksicherungen](sql-database-recovery-using-backups.md).
- Informationen zur Archivierung einer Datenbanksicherung finden Sie unter [Kopieren einer Azure SQL-Datenbank](sql-database-copy.md).
- Informationen zu schnelleren Wiederherstellungsoptionen finden Sie unter [Übersicht: Aktive Georeplikation in Azure SQL-Datenbank](sql-database-geo-replication-overview.md).

<!-- ### Tasks -->

<!-- ### Tutorials -->

<!---HONumber=AcomDC_0928_2016-->