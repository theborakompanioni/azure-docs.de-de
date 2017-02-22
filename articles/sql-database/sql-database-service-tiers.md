---
title: 'SQL-Datenbankleistung: Dienstebenen | Microsoft Docs'
description: "Vergleichen Sie die Dienstebenen für SQL-Datenbank."
keywords: Datenbankoptionen, Datenbankleistung
services: sql-database
documentationcenter: 
author: janeng
manager: jhubbard
editor: 
ms.assetid: f5c5c596-cd1e-451f-92a7-b70d4916e974
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 02/09/2017
ms.author: janeng
translationtype: Human Translation
ms.sourcegitcommit: ae230c012a17eb73c8993a32197c844c6abaa2a4
ms.openlocfilehash: 920143756a0e0b369cf839681f9c4308f77a4af0


---
# <a name="sql-database-options-and-performance-understand-whats-available-in-each-service-tier"></a>SQL-Datenbankoptionen und -leistung: Grundlegendes zum Angebot in den einzelnen Tarifen
[Azure SQL-Datenbank](sql-database-technical-overview.md) bietet drei Dienstebenen (**Basic**, **Standard** und **Premium**) mit mehreren Leistungsebenen für unterschiedliche Workloads. Bei höheren Leistungsebenen stehen mehr Ressourcen zur Verfügung, um einen höheren Durchsatz zu ermöglichen. Sie können die [Dienstebenen und Leistungsebenen dynamisch](sql-database-service-tiers.md) und ohne Ausfallzeiten ändern. Die Dienstebenen „Basic“, „Standard“ und „Premium“ haben alle eine Betriebszeit-SLA von 99,99 Prozent und bieten flexible Optionen für Geschäftskontinuität, Sicherheitsfeatures und stündliche Abrechnung. 

Sie können Einzeldatenbanken mit dedizierten Ressourcen auf der gewählten [Leistungsebene](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) erstellen. Außerdem können Sie mehrere Datenbanken in einem [elastischen Pool](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) verwalten, in dem die Ressourcen gemeinsam von mehreren Datenbanken verwendet werden. Die für Einzeldatenbanken verfügbaren Ressourcen werden als Datenbanktransaktionseinheiten (Database Transaction Units, DTUs) und die für elastische Pools verfügbaren Ressourcen als elastische DTUs (eDTUs) bezeichnet. Weitere Informationen zu DTUs und eDTUs finden Sie unter [Was sind Datenbanktransaktionseinheiten (DTUs)?](sql-database-what-is-a-dtu.md). 

In beiden Fällen sind die Dienstebenen **Basic**, **Standard** und **Premium** verfügbar. 

## <a name="choosing-a-service-tier"></a>Auswählen einer Dienstebene
In der folgenden Tabelle sind Beispiele für Dienstebenen aufgeführt, die sich für unterschiedliche Anwendungsworkloads am besten eignen.

| Dienstebene | Zielworkloads |
| :--- | --- |
| **Basic** | Eignet sich am besten für eine kleine Datenbank, in der normalerweise ein einzelner aktiver Vorgang zu einem bestimmten Zeitpunkt unterstützt wird. Beispiele hierfür sind Datenbanken, die für die Entwicklung, für Tests oder für kleine, selten verwendete Anwendungen verwendet werden. |
| **Standard** |Optimale Option für Cloudanwendungen mit geringen bis mittleren E/A-Leistungsanforderungen. Unterstützt mehrere parallele Abfragen. Beispiele hierfür sind Arbeitsgruppen-oder Webanwendungen. |
| **Premium** | Konzipiert für hohes Transaktionsvolumen mit hohen E/A-Leistungsanforderungen. Unterstützt zahlreiche gleichzeitige Benutzer. Beispiele hierfür sind Datenbanken für unternehmenskritische Anwendungen. |

Überlegen Sie sich zunächst, ob Sie eine Einzeldatenbank ausführen oder Datenbanken mit gemeinsam genutzten Ressourcen gruppieren möchten. Informieren Sie sich über die [Überlegungen zu elastischen Pools](sql-database-elastic-pool-guidance.md). Bestimmen Sie zu Beginn die Datenbankfeatures, die Sie mindestens benötigen:

* Maximale Datenbankgröße für einzelne Datenbanken (2 GB bei Basic, 250 GB bei Standard und 500 GB bis 1 TB bei Premium in den höchsten Leistungsebenen)
* Maximaler Gesamtspeicher in einem Pool für elastische Datenbanken (117 GB bei Basic, 1200 GB bei Standard und 750 GB bei Premium)
* Maximale Anzahl von Datenbanken pro Pool (400 bei Basic, 400 bei Standard und 50 bei Premium)
* Datenbank-Aufbewahrungszeitraum (7 Tage bei Basic und 35 Tage bei Standard und Premium)

Nach der Ermittlung der Mindestdienstebene können Sie die Leistungsebene für die Datenbank (Anzahl von DTUs) bestimmen. Die standardmäßigen Leistungsebenen S2 und S3 sind meist ein guter Ausgangspunkt. Für Datenbanken mit hohen CPU- oder E/A-Anforderungen empfehlen sich dagegen eher die Premium-Leistungsebenen. Premium bietet im Vergleich zur höchsten Standard-Leistungsebene mehr CPU- und eine mindestens zehnmal höhere E/A-Leistung.

## <a name="single-database-service-tiers-and-performance-levels"></a>Tarife und Leistungsstufen für Einzeldatenbanken
Bei Einzeldatenbanken gibt es mehrere Leistungsstufen auf den einzelnen Dienstebenen. Sie können die Ebene auswählen, die Ihren Anforderungen an Workloads am besten entspricht. Wenn Sie zentral hoch- oder herunterskalieren müssen, können Sie die Ebenen Ihrer Datenbank leicht ändern. Ausführlichere Informationen finden Sie unter [Ändern der Dienstebenen und -Leistungsstufen von Datenbanken](sql-database-service-tiers.md) .

Unabhängig von der Anzahl von gehosteten Datenbanken erhält Ihre Datenbank einen garantierten Ressourcensatz, und die erwarteten Leistungsmerkmale Ihrer Datenbank werden nicht beeinträchtigt.

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

> [!NOTE]
> Eine ausführliche Beschreibung aller anderen Zeilen in dieser Dienstebenentabelle finden Sie unter [Service tier capabilities and limits](sql-database-performance-guidance.md#service-tier-capabilities-and-limits)(Funktionen und Limits von Dienstebenen; in englischer Sprache).
> 

## <a name="scaling-up-or-scaling-down-a-single-database"></a>Zentrales Hoch- oder Herunterskalieren einer einzelnen Datenbank

Nach dem Auswählen eines Tarifs und einer Leistungsebene können Sie eine einzelne Datenbank dynamisch und bedarfsgerecht zentral hoch- oder herunterskalieren. 

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

Wenn Sie die Dienst- und/oder die Leistungsstufe einer Datenbank ändern, wird ein Replikat der ursprünglichen Datenbank mit der neuen Leistungsebene erstellt und anschließend die Verbindung auf dieses Replikat umgestellt. Während dieses Vorgangs gehen keine Daten verloren. Allerdings sind die Verbindungen zur Datenbank inaktiv, während kurz auf das Replikat umgestellt wird. Daher werden möglicherweise einige aktive Transaktionen zurückgesetzt. Dieses Zeitfenster variiert, ist aber durchschnittlich kleiner als vier Sekunden und in mehr als 99 % der Fälle kürzer als 30 Sekunden. Falls im Moment der Verbindungstrennung viele Transaktionen stattfinden, ist das Zeitfenster unter Umständen größer.  

Die Dauer des gesamten zentralen Hochskalierungsvorgangs hängt sowohl von der Größe als auch vom Diensttarif der Datenbank vor und nach der Änderung ab. Beispielsweise sollte eine 250-GB-Datenbank beim Wechsel in, aus oder innerhalb eines Standarddiensttarifs innerhalb von sechs Stunden zentral hochskalieren. Eine Datenbank der gleichen Größe, die ihre Leistungsstufen innerhalb des Premium-Diensttarifs ändert, sollte dies innerhalb von drei Stunden vollziehen.

* Für ein Downgrade einer Datenbank sollte die Datenbank kleiner als die in der Zieldienstebene maximal zulässige Größe sein. 
* Beim Aktualisieren einer Datenbank, für die [Georeplikation](sql-database-geo-replication-portal.md) aktiviert ist, müssen Sie vor der Aktualisierung der primären Datenbank zunächst die zugehörigen sekundären Datenbanken auf die gewünschte Leistungsstufe aktualisieren.
* Beim Downgrade von einer Premium-Dienstebene müssen Sie zuerst alle geografischen Replikationsbeziehungen beenden. Sie können die im Thema [Wiederherstellen nach einem Ausfall](sql-database-disaster-recovery.md) beschriebenen Schritte verwenden, um den Replikationsprozess zwischen der primären und den aktiven sekundären Datenbanken zu beenden.
* Die Angebote des Wiederherstellungsdienstes variieren für die verschiedenen Dienstebenen. Wenn Sie ein Downgrade durchführen, verlieren Sie eventuell die Möglichkeit einer Zeitpunktwiederherstellung, oder der Aufbewahrungszeitraum für Sicherungen verkürzt sich. Weitere Informationen finden Sie unter [Sichern und Wiederherstellen der Azure SQL-Datenbank](sql-database-business-continuity.md).
* Die neuen Eigenschaften für die Datenbank werden erst angewendet, wenn die Änderungen abgeschlossen sind.

> [!IMPORTANT]
> Ausführliche Schritte finden Sie unter [Create and manage single Azure SQL databases with the Azure portal](sql-database-manage-single-databases-portal.md) (Erstellen und Verwalten einzelner Azure SQL-Datenbanken über das Azure-Portal), [Create and manage single Azure SQL databases with PowerShell](sql-database-manage-single-databases-powershell.md) (Erstellen und Verwalten einzelner Azure SQL-Datenbanken mithilfe von PowerShell) oder [Create and manage single Azure SQL databases with Transact-SQL](sql-database-manage-single-databases-tsql.md) (Erstellen und Verwalten einzelner Azure SQL-Datenbanken mithilfe von Transact-SQL).
>

## <a name="elastic-pool-service-tiers-and-performance-in-edtus"></a>Dienstebenen und Leistung für elastische Pools in eDTUs

Durch Pools können Datenbanken eDTU-Ressourcen freigeben und gemeinsam nutzen, ohne dass jeder Datenbank im Pool eine bestimmte Leistungsebene zugewiesen werden muss. Beispielsweise kann eine Einzeldatenbank in einem Pool der Dienstebene „Standard“ 0 eDTU bis hin zum maximalen eDTU-Wert für Datenbanken nutzen, den Sie beim Konfigurieren des Pools festlegen. Bei Pools können mehrere Datenbanken mit unterschiedlichen Workloads die für den gesamten Pool verfügbaren eDTU-Ressourcen effizient nutzen. Weitere Informationen finden Sie unter den [Überlegungen zum Preis und zur Leistung eines elastischen Pools](sql-database-elastic-pool-guidance.md) .

In der folgenden Tabelle sind die Merkmale der Dienstebenen eines Pools beschrieben.

[!INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

Für jede Datenbank in einem Pool gelten auch die Merkmale für Einzeldatenbanken für die entsprechende Dienstebene. Der Basic-Pool verfügt in Bezug auf die maximalen Sitzungen pro Pool beispielsweise über eine Beschränkung auf den Bereich von 4.800 bis 28.800. Eine eigenständige Datenbank in einem Basic-Pool verfügt dagegen über eine Datenbankbeschränkung von 300 Sitzungen.

## <a name="scaling-up-or-scaling-down-an-elastic-pool"></a>Zentrales Hoch- oder Herunterskalieren eines Pools für elastische Datenbanken

Nach dem Auswählen eines Tarifs und einer Leistungsebene können Sie den Pool für elastische Datenbanken dynamisch und bedarfsgerecht zentral hoch- oder herunterskalieren. 

* Änderungen der minimalen oder maximalen Anzahl von eDTUs pro Datenbank werden in der Regel in höchstens fünf Minuten durchgeführt.
* Wie lange es dauert, die Poolgröße (eDTUs) zu ändern, hängt von der kombinierten Größe aller Datenbanken im Pool ab. Änderungen dauern durchschnittlich 90 Minuten oder weniger pro 100 GB. Wenn beispielsweise der gesamte Speicherplatz für alle Datenbanken im Pool 200 GB beträgt, ist für die Änderung der eDTUs pro Pool eine Latenzzeit von drei Stunden oder weniger zu erwarten.

> [!IMPORTANT]
> Ausführliche Schritte finden Sie unter [Erstellen und Verwalten eines Pools für elastische Datenbanken über das Azure-Portal](sql-database-elastic-pool-manage-portal.md), [Erstellen und Verwalten eines Pools für elastische Datenbanken mit PowerShell](sql-database-elastic-pool-manage-powershell.md), [Überwachen und Verwalten eines Pools für elastische Datenbanken per Transact-SQL](sql-database-elastic-pool-manage-tsql.md) oder [Erstellen und Verwalten eines Pools für elastische Datenbanken mit C#](sql-database-elastic-pool-manage-csharp.md).
>

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [elastische Pools](sql-database-elastic-pool-guidance.md), und lesen Sie sich den Artikel [Wo sollte ein Pool für elastische Datenbanken verwendet werden?](sql-database-elastic-pool-guidance.md) durch.
* Informieren Sie sich darüber, wie Sie [einen elastischen Pool überwachen und verwalten und seine Größe ändern](sql-database-elastic-pool-manage-portal.md) und wie Sie [die Leistung von Einzeldatenbanken überwachen](sql-database-single-database-monitor.md).
* Nachdem Sie jetzt die Ebenen für SQL-Datenbank kennen, können Sie sie mit einem [kostenlosen Konto](https://azure.microsoft.com/pricing/free-trial/) ausprobieren und sich mit der [Erstellung Ihrer ersten SQL-Datenbank](sql-database-get-started.md) befassen.
* Verwenden Sie in Migrationsszenarien den [DTU Calculator](http://dtucalculator.azurewebsites.net/) (DTU-Rechner), um die voraussichtlich benötigte DTU-Anzahl zu ermitteln. 




<!--HONumber=Feb17_HO3-->


