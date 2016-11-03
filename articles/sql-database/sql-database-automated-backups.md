<properties
   pageTitle="Weitere Informationen zu Sicherungen für SQL-Datenbank | Microsoft Azure" 
   description="Informieren Sie sich über integrierte Datenbanksicherungen von SQL-Datenbank, mit denen Sie eine Azure SQL-Datenbank-Instanz auf einen früheren Zeitpunkt wiederherstellen oder eine Datenbank in eine neue Datenbank in einer geografischen Region kopieren können (bis zu 35 Tage)."
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




<!-- # H1 Title

 H1 title should answer the question "What is in this topic?" Write the title in conversational language and use search key words as much as possible. Since this is a learning topic, make sure the title indicates that and doesn't mislead people to think this will tell them how to do tasks.  
-->

# <a name="learn-about-sql-database-backups"></a>Weitere Informationen zu Sicherungen für SQL-Datenbank

<!-- Introduction

1. Sentence #1 begins with "Learn about ..." and gives the scope of what the article will cover.
2. Sentence #2 explains the key capability or selling point of the feature.
3. Sentence #3 begins with "Use this feature to ..." and gives a common use case.

-->

SQL-Datenbank erstellt alle fünf Minuten eine lokale Datenbanksicherung und verwendet georedundanten Azure-Speicher mit Lesezugriff (Read-Access Geographically Redundant Storage, RA-GRS), um einen Teil der Datenbanksicherungen in eine andere geografische Region zu kopieren. Diese Sicherungen erfolgen **automatisch und ohne zusätzliche Kosten**. Verwenden Sie lokale Datenbanksicherungen, um eine [Datenbank auf dem gleichen Server auf einen früheren Zeitpunkt wiederherzustellen](sql-database-point-in-time-restore-portal.md). Mithilfe von georedundanten Sicherungen können Sie die [Datenbank in einer anderen geografischen Region wiederherstellen](sql-database-geo-restore-portal.md).  

>[AZURE.NOTE] Sowohl lokale als auch georedundante Sicherungen erfolgen automatisch. Sie müssen keinerlei Aktionen durchführen, und es fallen keine zusätzlichen Gebühren an. 

Im folgenden Diagramm wird SQL-Datenbank in der Region „USA, Osten“ ausgeführt. Alle fünf Minuten wird eine Datenbanksicherung ausgeführt und lokal in georedundantem Azure-Speicher mit Lesezugriff gespeichert. Anschließend kopiert Azure die Datenbanksicherungen in ein gekoppeltes Rechenzentrum in der Region „USA, Westen“.

![Geowiederherstellung](./media/sql-database-geo-restore/geo-restore-1.png)

<!--## What is <feature>?" -->

## <a name="what-is-a-sql-database-backup"></a>Was ist die Sicherung einer SQL-Datenbank-Instanz?  

<!-- 
First sentence begins with "The <feature> is ..." followed by a definition of the feature. Provide a 1-2 paragraph intro to explain what the feature is, how it works, and the importance of the feature for solving business problems.
-->
Eine Sicherung einer SQL-Datenbank-Instanz ist eine Datei, in der Informationen zum Zustand der Datenbank zu einem bestimmten Zeitpunkt gespeichert werden. SQL-Datenbank verwendet die SQL Server-Technologie, um [vollständige Sicherungen](https://msdn.microsoft.com/library/ms186289.aspx), [differenzielle Sicherungen](https://msdn.microsoft.com/library/ms175526.aspx ) und [Transaktionsprotokollsicherungen](https://msdn.microsoft.com/library/ms191429.aspx) zu erstellen. Transaktionsprotokollsicherungen erfolgen alle fünf Minuten, sodass Sie eine Zeitpunktwiederherstellung auf dem gleichen Server durchführen können, auf dem die Datenbank gehostet wird. Wenn Sie eine Datenbank wiederherstellen, ermittelt der Dienst, welche vollständigen und differenziellen Sicherungen bzw. Transaktionsprotokollsicherungen wiederhergestellt werden müssen.

>[AZURE.NOTE] SQL-Datenbank erstellt automatisch sowohl lokale als auch georedundante Sicherungen. Sie müssen keinerlei Aktionen durchführen. Es fallen keine zusätzlichen Gebühren an.

Verwenden Sie eine Datenbanksicherung für Folgendes:

- Stellen Sie eine Datenbank innerhalb der Aufbewahrungsdauer auf einen Zeitpunkt wieder her. Mit einer Datenbanksicherung können Sie für eine Datenbank einen bestimmten Zeitpunkt wiederherstellen, für eine gelöschte Datenbank den Zustand zum Löschzeitpunkt wiederherstellen oder eine Datenbank in einer anderen geografischen Region wiederherstellen. Informationen zum Durchführen einer Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus einer Datenbanksicherung](sql-database-recovery-using-backups.md).

- Kopieren Sie eine Datenbank auf eine SQL Server-Instanz in der gleichen oder einer anderen Region. Die Kopie ist in Bezug auf die Transaktion mit der aktuellen SQL-Datenbank konsistent. Informationen zum Erstellen einer Kopie finden Sie unter [Datenbankkopie](sql-database-copy.md).

- Archivieren Sie eine Datenbanksicherung über den Aufbewahrungszeitraum der Sicherung hinaus. Informationen zum Archivieren einer Datenbank finden Sie unter [Exportieren einer SQL-Datenbank in eine BACPAC-Datei](sql-database-export.md). Sie können diese BACPAC-Datei in einem langfristigen Speicher archivieren und über den Aufbewahrungszeitraum hinaus speichern. Sie können auch die BACPAC-Datei zum Übertragen einer Kopie der Datenbank an SQL Server verwenden, entweder lokal oder auf einem virtuellen Azure-Computer (VM).

## <a name="backups-have-geographical-redundancy"></a>Sicherungen sind geografisch redundant

SQL-Datenbank verwendet die [Azure Storage-Replikation](../storage/storage-redundancy.md), um Ihre Datenbank an einem anderen geografischen Standort zu sichern. Um georedundanten Speicher bereitzustellen, speichert SQL-Datenbank lokale Datenbanksicherungsdateien in einem Konto für [georedundanten Speicher mit Lesezugriff (RA-GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage). Azure repliziert die Sicherungsdateien in einem [gekoppelten Rechenzentrum](../best-practices-availability-paired-regions.md). Mit dieser Georeplikation wird sichergestellt, dass Sie eine Datenbank wiederherstellen können, falls Sie aus Ihrer primären Datenbankregion nicht auf die Datenbanksicherung zugreifen können. 

>[AZURE.NOTE] In Azure Storage bezieht sich der Begriff *Replikation* auf das Kopieren von Dateien von einem Speicherort zum anderen. Die *Datenbankreplikation* der SQL bezieht sich auf das Beibehalten von mehreren sekundären Datenbanken, die mit einer primären Datenbank synchronisiert werden. 


## <a name="how-much-backup-storage-is-included-at-no-cost"></a>Wie viel Sicherungsspeicher ist kostenlos enthalten?

SQL-Datenbank stellt bis zu 200 Prozent Ihres maximal bereitgestellten Datenbankspeichers ohne zusätzliche Kosten als Sicherungsspeicher zur Verfügung. Wenn Sie beispielsweise über eine Standard-Datenbankinstanz mit einer bereitgestellten Größe von 250 GB verfügen, können Sie ohne zusätzliche Kosten 500 GB Sicherungsspeicher nutzen. Falls Ihre Datenbank den bereitgestellten Sicherungsspeicher überschreitet, können Sie die Aufbewahrungsdauer reduzieren lassen, indem Sie sich an den Azure-Support wenden. Eine weitere Option ist die Bezahlung für zusätzlichen Sicherungsspeicher, für den die RA-GRS-Standardrate (Read-Access Geographically Redundant Storage, Geografisch redundanter Speicher mit Lesezugriff) berechnet wird. 

## <a name="how-often-do-backups-happen"></a>Wie oft erfolgen Sicherungen?

Vollständige Datenbanksicherungen werden wöchentlich, differenzielle Datenbanksicherungen stündlich und Transaktionsprotokollsicherungen alle fünf Minuten ausgeführt. Die erste vollständige Sicherung wird unmittelbar nach der Datenbankerstellung geplant. Sie wird normalerweise innerhalb von 30 Minuten abgeschlossen, aber der Vorgang kann auch länger dauern, wenn es sich um eine sehr große Datenbank handelt. Die erste Sicherung kann bei einer wiederhergestellten Datenbank oder einer Datenbankkopie beispielsweise länger dauern. Nach der ersten vollständigen Sicherung werden alle weiteren Sicherungen automatisch geplant und im Hintergrund verwaltet. Die genaue Zeitplanung für vollständige und [differenzielle](https://msdn.microsoft.com/library/ms175526.aspx) Datenbanksicherungen wird je nach der gesamten Systemworkload bestimmt. 

## <a name="how-long-do-you-keep-my-backups"></a>Wie lang werden meine Sicherungen aufbewahrt?

Jede SQL-Datenbank-Sicherung verfügt über einen Aufbewahrungszeitraum, der auf der [Dienstebene](sql-database-service-tiers.md) der Datenbank basiert. Folgende Aufbewahrungszeiträume gelten:

- Tarif „Basic“: 7 Tage
- Tarif „Standard“: 35 Tage
- Tarif „Premium“: 35 Tage


Wenn Sie Ihre Datenbank von den Tarifen „Standard“ oder „Premium“ auf „Basic“ herabstufen, werden die Sicherungen sieben Tage lang gespeichert. Alle vorhandenen Sicherungen, die älter sind als sieben Tage, sind nicht mehr verfügbar. 

Wenn Sie Ihre Datenbank vom Tarif „Basic“ auf „Standard“ oder „Premium“ heraufstufen, bewahrt SQL-Datenbank vorhandene Sicherungen so lange auf, bis sie 35 Tage alt sind. Neue Sicherungen werden 35 Tage lang aufbewahrt.
 
Wenn Sie eine Datenbank löschen, bewahrt SQL-Datenbank die Sicherungen auf die gleiche Weise auf wie für eine Onlinedatenbank. Ein Beispiel: Sie löschen eine Basic-Datenbank, die einen Aufbewahrungszeitraum von sieben Tagen aufweist. Eine vier Tage alte Sicherung wird drei weitere Tage gespeichert.

> [AZURE.IMPORTANT] Wenn Sie die Azure SQL Server-Instanz löschen, auf der die SQL-Datenbanken gehostet werden, werden alle Datenbanken, die zum Server gehören, gelöscht und können nicht wiederhergestellt werden. Es ist nicht möglich, einen gelöschten Server wiederherzustellen.

## <a name="next-steps"></a>Nächste Schritte

Datenbanksicherungen sind ein wesentlicher Bestandteil jeder Strategie für Geschäftskontinuität und Notfallwiederherstellung, da Ihre Daten vor versehentlichen Beschädigungen und Löschungen geschützt werden. Um herauszufinden, wie Datenbanksicherungen in Ihre Geschäftsstrategie passen, lesen Sie die [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).





<!--HONumber=Oct16_HO2-->


