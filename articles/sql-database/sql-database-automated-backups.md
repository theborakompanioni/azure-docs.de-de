---
title: Automatische, georedundante SQL-Datenbank-Sicherungen | Microsoft Docs
description: "SQL-Datenbank erstellt alle paar Minuten automatisch eine lokale Datenbanksicherung und verwendet georedundanten Azure-Speicher mit Lesezugriff (Read-Access Geographically Redundant Storage, RA-GRS), um für Georedundanz zu sorgen."
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: 
ms.assetid: 3ee3d49d-16fa-47cf-a3ab-7b22aa491a8d
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2016
ms.author: sashan;carlrab;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 145cdc5b686692b44d2c3593a128689a56812610
ms.openlocfilehash: f6bb6e1c81cafe5f0e5c43c99ab15a0483742868


---
# <a name="learn-about-sql-database-backups"></a>Weitere Informationen zu Sicherungen für SQL-Datenbank
<!------------------
This topic is annotated with TEMPLATE guidelines for FEATURE TOPICS.

Metadata guidelines

pageTitle
    60 characters or less. Includes name of the feature - primary benefit. Not the same as H1. Its 60 characters or fewer including all characters between the quotes and the Microsoft Azure site identifier.

description
    115-145 characters. Duplicate of the first sentence in the introduction. This is the abstract of the article that displays under the title when searching in Bing or Google. 

    Example: "SQL Database automatically creates a local database backup every few minutes and uses Azure read-access geo-redundant storage for geo-redundancy."

TEMPLATE GUIDELINES for feature topics

The Feature Topic is a one-pager (ok, sometimes longer) that explains a capability of the product or service. It explains what the capability is and characteristics of the capability.  

It is a "learning" topic, not an action topic.

DO explain this:
    � Definition of the feature terminology.  i.e., What is a database backup?
    � Characteristics and capabilities of the feature. (How the feature works)
    � Common uses with links to overview topics that recommend when to use the feature.
    � Reference specifications (Limitations and Restrictions, Permissions, General Remarks, etc.)
    � Next Steps with links to related overviews, features, and tasks.

DON'T explain this:
    � How to steps for using the feature (Tasks)
    � How to solve business problems that incorporate the feature (Overviews)

GUIDELINES for the H1 

    The H1 should answer the question "What is in this topic?" Write the H1 heading in conversational language and use search key words as much as possible. Since this is a learning topic, make sure the title indicates that and doesn't mislead people to think this will tell them how to do tasks.  

    To help people understand this is a learning topic and not an action topic, start the title with "Learn about ... "

    Heading must use an industry standard term. If your feature is a proprietary name like "elastic pools", use a synonym. For example:    "Learn about elastic pools for multi-tenant databases". In this case multi-tenant database is the industry-standard term that will be an anchor for finding the topic.

GUIDELINES for introduction

    The introduction is 1-2 sentences.  It is optimized for search and sets proper expectations about what to expect in the article. It should contain the top key words that you are using throughout the article.The introduction should be brief and to the point of what the feature is, what it is used for, and what's in the article. 

    If the introduction is short enough, your article can pop to the top in Google Instant Answers.

    In this example:

Sentence #1 Explains what the article will cover, which is what the feature is or does. This is also the metadata description. 
    SQL Database automatically creates a database backup every five minutes and uses Azure read-access geo-redundant storage (RA-GRS) to provide geo-redundancy. 

Sentence #2 Explains why I should care about this.  
    Database backups are an essential part of any business continuity and disaster recovery strategy because they protect your data from accidental corruption or deletion.

-------------------->

SQL-Datenbank erstellt automatisch eine Datenbanksicherung und verwendet georedundanten Azure-Speicher mit Lesezugriff (Read-Access Geographically Redundant Storage, RA-GRS), um für Georedundanz zu sorgen. Diese Sicherungen werden automatisch und ohne zusätzliche Kosten erstellt. Sie müssen keinerlei Aktionen durchführen. Datenbanksicherungen sind ein wesentlicher Bestandteil jeder Strategie für Geschäftskontinuität und Notfallwiederherstellung, da Ihre Daten vor versehentlichen Beschädigungen und Löschungen geschützt werden. Wenn Sie Sicherungen in Ihrem eigenen Speichercontainer beibehalten möchten, können Sie eine Richtlinie für die langfristige Aufbewahrung der Sicherung konfigurieren. Weitere Informationen finden Sie unter [Langfristige Aufbewahrung](sql-database-long-term-retention.md).

<!-- This image needs work, so not putting it in right now.

This diagram shows SQL Database running in the US East region. It creates a database backup every five minutes, which it stores locally to Azure Read Access Geo-redundant Storage (RA-GRS). Azure uses geo-replication to copy the database backups to a paired data center in the US West region.

![geo-restore](./media/sql-database-geo-restore/geo-restore-1.png)

-->

<!---------------
GUIDELINES for the first ## H2.

    The first ## describes what the feature encompasses and how it is used. It points to related task articles.

    For consistency, being the heading with "What is ... "
----------------->

## <a name="what-is-a-sql-database-backup"></a>Was ist die Sicherung einer SQL-Datenbank-Instanz?
<!-- 
    Explains what a SQL Database backup is and answers an important question that people want to know.
-->


<!----------------- 
    Explains first component of the backup feature
------------------>

SQL-Datenbank verwendet die SQL Server-Technologie, um [vollständige Sicherungen](https://msdn.microsoft.com/library/ms186289.aspx), [differenzielle Sicherungen](https://msdn.microsoft.com/library/ms175526.aspx) und [Transaktionsprotokollsicherungen](https://msdn.microsoft.com/library/ms191429.aspx) zu erstellen. Die Transaktionsprotokollsicherungen erfolgen im Allgemeinen alle 5-10 Minuten, wobei die Häufigkeit auf der Leistungsebene und der Menge an Datenbankaktivität basiert. Transaktionsprotokollsicherungen mit vollständigen und differenziellen Sicherungen ermöglichen die Wiederherstellung einer Datenbank zu einem bestimmten Zeitpunkt auf dem gleichen Server, der die Datenbank hostet. Wenn Sie eine Datenbank wiederherstellen, ermittelt der Dienst, welche vollständigen und differenziellen Sicherungen bzw. Transaktionsprotokollsicherungen wiederhergestellt werden müssen.

<!--------------- 
    Explicit list of what to do with a local backup. "Use a ..." helps people to scan the topic and find the uses quickly.
---------------->

Sie können diese Sicherungen für Folgendes verwenden:

* Stellen Sie eine Datenbank innerhalb der Aufbewahrungsdauer auf einen Zeitpunkt wieder her. Dieser Vorgang wird eine neue Datenbank auf dem gleichen Server wie die ursprüngliche Datenbank erstellen.
* Wiederherstellen einer gelöschten Datenbank auf den Zeitpunkt, zu dem sie gelöscht wurde, oder auf einen beliebigen Zeitpunkt innerhalb des Aufbewahrungszeitraums. Die gelöschte Datenbank kann nur auf dem Server wiederhergestellt werden, auf dem die ursprüngliche Datenbank erstellt wurde.
* Wiederherstellen einer Datenbank in einer anderen geografischen Region. Dies ermöglicht die Wiederherstellung nach dem Ausfall einer geografischen Region, wenn Sie keinen Zugriff auf Ihren Server und Ihre Datenbank haben. Dabei wird eine neue Datenbank auf einem beliebigen Server an einem beliebigen Ort der Welt erstellt. 
* Wiederherstellen einer Datenbank aus einer bestimmten Sicherung, die in Ihrem Azure Recovery Services-Tresor gespeichert ist. Dadurch können Sie eine alte Version der Datenbank wiederherstellen, um eine Konformitätsanforderung zu erfüllen oder eine alte Version der Anwendung auszuführen. Weitere Informationen hierzu finden Sie unter [Langfristige Aufbewahrung](sql-database-long-term-retention.md).
* Informationen zum Durchführen einer Wiederherstellung finden Sie unter [Wiederherstellen einer Azure SQL-Datenbank mit automatisierten Datenbanksicherungen](sql-database-recovery-using-backups.md).

> [!TIP]
> Ein Tutorial finden Sie unter [Erste Schritte mit der Sicherung und Wiederherstellung für Datenschutz und Wiederherstellung](sql-database-get-started-backup-recovery.md).
>

<!----------------- 
    Explains first component of the backup feature
------------------>

<!--------------- 
    Explicit list of what to do with a geo-redundant backup. "Use a ..." helps people to scan the topic and find the uses quickly.
---------------->

> [!NOTE]
> In Azure Storage bezieht sich der Begriff *Replikation* auf das Kopieren von Dateien von einem Speicherort zum anderen. Die *Datenbankreplikation* der SQL bezieht sich auf das Beibehalten von mehreren sekundären Datenbanken, die mit einer primären Datenbank synchronisiert werden. 
> 
> 

<!----------------
    The next ## H2's discuss key characteristics of how the feature works. The title is in conversational language and asks the question that will be answered.
------------------->
## <a name="how-much-backup-storage-is-included-at-no-cost"></a>Wie viel Sicherungsspeicher ist kostenlos enthalten?
SQL-Datenbank stellt bis zu 200 Prozent Ihres maximal bereitgestellten Datenbankspeichers ohne zusätzliche Kosten als Sicherungsspeicher zur Verfügung. Wenn Sie beispielsweise über eine Standard-Datenbankinstanz mit einer bereitgestellten Größe von 250 GB verfügen, können Sie ohne zusätzliche Kosten 500 GB Sicherungsspeicher nutzen. Falls Ihre Datenbank den bereitgestellten Sicherungsspeicher überschreitet, können Sie die Aufbewahrungsdauer reduzieren lassen, indem Sie sich an den Azure-Support wenden. Eine weitere Option ist die Bezahlung für zusätzlichen Sicherungsspeicher, für den die RA-GRS-Standardrate (Read-Access Geographically Redundant Storage, Geografisch redundanter Speicher mit Lesezugriff) berechnet wird. 

## <a name="how-often-do-backups-happen"></a>Wie oft erfolgen Sicherungen?
Vollständige Datenbanksicherungen werden wöchentlich, differenzielle Datenbanksicherungen im Allgemeinen alle paar Stunden und Transaktionsprotokollsicherungen alle 5-10 Minuten ausgeführt. Die erste vollständige Sicherung wird unmittelbar nach der Datenbankerstellung geplant. Sie wird normalerweise innerhalb von 30 Minuten abgeschlossen, aber der Vorgang kann auch länger dauern, wenn es sich um eine sehr große Datenbank handelt. Die erste Sicherung kann bei einer wiederhergestellten Datenbank oder einer Datenbankkopie beispielsweise länger dauern. Nach der ersten vollständigen Sicherung werden alle weiteren Sicherungen automatisch geplant und im Hintergrund verwaltet. Die genaue Zeitplanung für alle Datenbanksicherungen wird vom SQL-Datenbank-Dienst je nach der gesamten Systemworkload bestimmt. 

Die Georeplikation des Sicherungsspeichers erfolgt basierend auf dem Azure Storage-Replikationszeitplan.

## <a name="how-long-do-you-keep-my-backups"></a>Wie lang werden meine Sicherungen aufbewahrt?
Jede SQL-Datenbank-Sicherung verfügt über einen Aufbewahrungszeitraum, der auf der [Dienstebene](sql-database-service-tiers.md) der Datenbank basiert. Folgende Aufbewahrungszeiträume gelten:

<!------------------

    Using a list so the information is easy to find when scanning.
------------------->

* Tarif „Basic“: 7 Tage
* Tarif „Standard“: 35 Tage
* Tarif „Premium“: 35 Tage

Wenn Sie Ihre Datenbank von den Tarifen „Standard“ oder „Premium“ auf „Basic“ herabstufen, werden die Sicherungen sieben Tage lang gespeichert. Alle vorhandenen Sicherungen, die älter sind als sieben Tage, sind nicht mehr verfügbar. 

Wenn Sie Ihre Datenbank vom Tarif „Basic“ auf „Standard“ oder „Premium“ heraufstufen, bewahrt SQL-Datenbank vorhandene Sicherungen so lange auf, bis sie 35 Tage alt sind. Neue Sicherungen werden 35 Tage lang aufbewahrt.

Wenn Sie eine Datenbank löschen, bewahrt SQL-Datenbank die Sicherungen auf die gleiche Weise auf wie für eine Onlinedatenbank. Ein Beispiel: Sie löschen eine Basic-Datenbank, die einen Aufbewahrungszeitraum von sieben Tagen aufweist. Eine vier Tage alte Sicherung wird drei weitere Tage gespeichert.

> [!IMPORTANT]
> Wenn Sie die Azure SQL Server-Instanz löschen, auf der die SQL-Datenbanken gehostet werden, werden alle Datenbanken, die zum Server gehören, gelöscht und können nicht wiederhergestellt werden. Es ist nicht möglich, einen gelöschten Server wiederherzustellen.
> 
> 

## <a name="how-to-extend-the-backup-retention-period"></a>Wie kann der Aufbewahrungszeitraum für Sicherungen erweitert werden?
Wenn Ihre Anwendung erfordert, dass die Sicherungen für einen längeren Zeitraum verfügbar sind, können Sie den integrierten Aufbewahrungszeitraum erweitern, indem Sie die Richtlinie für die langfristige Aufbewahrung von Sicherungen (LTR-Richtlinie, Long-term backup retention) konfigurieren. Dadurch können Sie den integrierten Aufbewahrungszeitraum von 35 Tagen auf bis zu 10 Jahre erweitern. Weitere Informationen finden Sie unter [Langfristige Aufbewahrung](sql-database-long-term-retention.md).

Sobald Sie die LTR-Richtlinie einer Datenbank mithilfe des Azure-Portals oder der API hinzufügen, werden die wöchentlichen vollständigen Datenbanksicherungen automatisch in Ihren Azure Backup-Diensttresor kopiert. Wenn Ihre Datenbank mit TDE verschlüsselt ist, werden die Sicherungen automatisch im Ruhezustand verschlüsselt.  Der Diensttresor löscht Ihre abgelaufenen Sicherungen basierend auf ihrem Zeitstempel und der LTR-Richtlinie automatisch.  Daher müssen Sie den Sicherungszeitplan nicht verwalten und sich keine Sorgen über die Bereinigung der alten Dateien machen. Die Wiederherstellungs-API unterstützt im Tresor gespeicherte Sicherungen, solange sich der Tresor im gleichen Abonnement wie Ihre SQL-Datenbank befindet. Sie können das Azure-Portal oder PowerShell verwenden, um auf diese Sicherungen zuzugreifen.

> [!TIP]
> Ein Tutorial finden Sie unter [Erste Schritte mit der Sicherung und Wiederherstellung für Datenschutz und Wiederherstellung](sql-database-get-started-backup-recovery.md).
>

<!-------------------
OPTIONAL section
## Best practices 
--------------------->

<!-------------------
OPTIONAL section
## General remarks
--------------------->

<!-------------------
OPTIONAL section
## Limitations and restrictions
--------------------->

<!-------------------
OPTIONAL section
## Metadata
--------------------->

<!-------------------
OPTIONAL section
## Performance
--------------------->

<!-------------------
OPTIONAL section
## Permissions
--------------------->

<!-------------------
OPTIONAL section
## Security
--------------------->

<!-------------------
GUIDELINES for Next Steps

    The last section is Next Steps. Give a next step that would be relevant to the customer after they have learned about the feature and the tasks associated with it.  Perhaps point them to one or two key scenarios that use this feature.

    You don't need to repeat links you have already given them.
--------------------->

## <a name="next-steps"></a>Nächste Schritte

- Datenbanksicherungen sind ein wesentlicher Bestandteil jeder Strategie für Geschäftskontinuität und Notfallwiederherstellung, da Ihre Daten vor versehentlichen Beschädigungen und Löschungen geschützt werden. Weitere Informationen zu den anderen Geschäftskontinuitätslösungen von Azure SQL-Datenbank finden Sie unter [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md).
- Informationen zum Wiederherstellen eines bestimmten Zeitpunkts finden Sie unter [Restore an Azure SQL Database to a previous point in time](sql-database-point-in-time-restore.md) (Wiederherstellen eines bestimmten Zeitpunkts einer Azure SQL-Datenbank).
- Informationen zum Anzeigen des ältesten Wiederherstellungspunkts aus den vom Dienst generierten Sicherungen einer Datenbank finden Sie unter [View the oldest restore point from the service-generated backups of an Azure SQL database](sql-database-view-oldest-restore-point.md) (Anzeigen des ältesten Wiederherstellungspunkts aus den vom Dienst generierten Sicherungen einer Azure SQL-Datenbank).
- Informationen zum Konfigurieren der langfristigen Beibehaltung von automatisierten Sicherungen in einem Azure Recovery Services-Tresor finden Sie unter [Configure long-term retention of automated backups in an Azure Recovery Services vault](sql-database-configure-long-term-retention.md) (Konfigurieren der langfristigen Beibehaltung von automatisierten Sicherungen in einem Azure Recovery Services-Tresor).



<!--HONumber=Dec16_HO2-->


