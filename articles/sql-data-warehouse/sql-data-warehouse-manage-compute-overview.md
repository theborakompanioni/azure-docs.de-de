---
title: "Verwalten von Computeleistung in Azure SQL Data Warehouse (Übersicht) | Microsoft Docs"
description: "Funktionen zur horizontalen Leistungshochskalierung in Azure SQL Data Warehouse. Führen Sie eine horizontale Skalierung durch, indem Sie DWUs anpassen oder Computeressourcen anhalten und fortsetzen, um Kosten zu sparen."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: e13a82b0-abfe-429f-ac3c-f2b6789a70c6
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 5d3bcc3c1434b16279778573ccf3034f9ac28a4d
ms.openlocfilehash: 6871ab3bc25ab3ec7b3c60852aa06bee047d8e9a
ms.lasthandoff: 02/16/2017


---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-overview"></a>Verwalten von Computeleistung in Azure SQL Data Warehouse (Übersicht)
> [!div class="op_single_selector"]
> * [Übersicht](sql-data-warehouse-manage-compute-overview.md)
> * [Portal](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>

In der Architektur von SQL Data Warehouse werden Speicher- und Computeressourcen voneinander getrennt, sodass diese unabhängig voneinander skaliert werden können. So können Sie Kosten sparen, indem Sie die Leistung horizontal skalieren und nur für die Leistung bezahlen, die Sie benötigen.

In dieser Übersicht werden die folgenden SQL Data Warehouse-Funktionen zum horizontalen Skalieren der Leistung beschrieben und Empfehlungen aufgeführt, wie und wann sie zu verwenden sind.

* Skalieren der Computeleistung durch Anpassen von [Data Warehouse-Einheiten (DWUs)][data warehouse units (DWUs)]
* Anhalten oder Fortsetzen von Computeressourcen

<a name="scale-performance-bk"></a>

## <a name="scale-performance"></a>Skalieren der Leistung
In SQL Data Warehouse können Sie die Leistung schnell horizontal hoch- und wieder herunterskalieren, indem Sie die Computeressourcen für die CPU, den Arbeitsspeicher oder die E/A-Bandbreite erhöhen bzw. verringern. Um die Leistung zu skalieren, müssen Sie lediglich die Anzahl der [DWUs (Data Warehouse-Einheiten)][data warehouse units (DWUs)] anpassen, die Ihrer Datenbank von SQL Data Warehouse zugewiesen werden. SQL Data Warehouse nimmt die Änderungen schnell vor und kümmert sich um alle zugrunde liegenden Änderungen an Hardware oder Software.

Vorbei sind die Zeiten, in denen Sie die Art der Prozessoren, die Menge des Arbeitsspeichers oder den Speichertyp ermitteln mussten, um eine hohe Leistung des Data Warehouse zu erreichen. Durch das Verlagern des Data Warehouse in die Cloud müssen Sie nicht mehr mit Low-Level-Hardwareprobleme befassen. Stattdessen stellt SQL Data Warehouse Ihnen diese Frage: Wie schnell möchten Sie Ihre Daten analysieren?

### <a name="how-do-i-scale-performance"></a>Wie skaliere ich die Leistung?
Um die Computeleistung flexibel zu erhöhen oder zu verringern, ändern Sie einfach die Einstellung für die [Data Warehouse-Einheiten (DWUs)][data warehouse units (DWUs)] für Ihre Datenbank. Die Leistung steigt linear an, wenn Sie weitere DWUs hinzufügen.  Auf höheren DWU-Ebenen müssen Sie mehr als 100 DWUs hinzufügen, um eine merkliche Verbesserung der Leistung zu erzielen. Damit Sie sinnvolle DWU-Steigerungen wählen können, bieten wir die DWU-Ebenen an, mit denen Sie die besten Ergebnisse erzielen.

Zum Anpassen der DWUs können Sie eine beliebige dieser individuellen Methoden verwenden.

* [Skalieren von Computeleistung mit dem Azure-Portal][Scale compute power with Azure portal]
* [Skalieren von Computeleistung mit PowerShell][Scale compute power with PowerShell]
* [Skalieren von Computeleistung mit REST-APIs][Scale compute power with REST APIs]
* [Skalieren von Computeleistung mit TSQL][Scale compute power with TSQL]

### <a name="how-many-dwus-should-i-use"></a>Wie viele DWUs sollte ich verwenden?
Die Leistung in SQL Data Warehouse wird linear skaliert, und das Ändern von einer Compute-Skalierung in eine andere (z. B. von 100 DWUs auf 2000 DWUs) erfolgt in Sekunden. So erhalten Sie die Flexibilität, mit verschiedenen DWU-Einstellungen zu experimentieren, bis Sie die beste Lösung für Ihr Szenario gefunden haben.

Damit Sie verstehen, wie Ihr idealer DWU-Wert lautet, sollten Sie versuchsweise hoch- und herunterskalieren und nach dem Laden der Daten einige Abfragen ausführen. Da die Skalierung schnell geht, können Sie es mit mehreren unterschiedlichen Leistungsebenen probieren, ohne dass dies länger als eine Stunde dauert. Beachten Sie, dass SQL Data Warehouse für die Verarbeitung von großen Datenmengen ausgelegt ist. Um die wahren Skalierungsmöglichkeiten zu nutzen, vor allem im Rahmen der von uns angebotenen größeren Skalierungen, sollten Sie ein großes Dataset von 1 TB oder darüber verwenden.

Empfehlungen für das Finden der besten DWU-Anzahl für Ihre Workload:

1. Beginnen Sie bei einem in der Entwicklung befindlichen Data Warehouse mit einer geringen Anzahl von DWUs.  Ein guter Ausgangspunkt ist DW400 oder DW200.
2. Überwachen Sie die Anwendungsleistung, und beobachten Sie dabei die Anzahl der ausgewählten DWUs im Vergleich zur beobachteten Leistung.
3. Bestimmen Sie durch Annahme einer linearen Skalierung, wie viel schneller oder langsamer die Leistung für Sie sein muss, um die optimale Leistungsstufe für Ihre Anforderungen zu erreichen.
4. Erhöhen oder verringern Sie die Anzahl von DWUs proportional dazu, wie viel schneller oder langsamer Ihre Workload ausgeführt werden soll. Der Dienst reagiert schnell und passt die Computeressourcen gemäß den neuen DWU-Anforderungen an.
5. Nehmen Sie weitere Anpassungen vor, bis Sie die optimale Leistungsstufe für Ihre geschäftlichen Anforderungen erreichen.

### <a name="when-should-i-scale-dwus"></a>Wann sollte ich DWUs skalieren?
Falls Sie schnellere Ergebnisse benötigen, sollten Sie Ihre DWUs erhöhen und für mehr Leistung zahlen.  Falls Sie weniger Computeleistung benötigen, sollten Sie Ihre DWUs verringern und nur für die verbrauchte Leistung bezahlen.

Empfehlungen für den Zeitpunkt für die Skalierung von DWUs:

1. Wenn Sie eine Anwendung mit wechselnder Workload haben, skalieren Sie die DWU-Anzahl nach oben oder unten, um Spitzen und Tiefpunkte zu berücksichtigen. Wenn eine Workload z.B. in der Regel am Ende des Monats einen Spitzenwert aufweist, fügen Sie während dieser Spitzenzeiten weitere DWUs hinzu, und entfernen Sie sie wieder, wenn diese Spitzenzeiten vorbei sind.
2. Bevor Sie einen umfangreichen Vorgang zum Laden oder Transformieren von Daten durchführen, skalieren Sie die DWUs zentral hoch, damit die Daten schneller verfügbar sind.

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Anhalten von Computeressourcen
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Verwenden Sie zum Anhalten einer Datenbank eine dieser individuellen Methoden.

* [Anhalten von Computeressourcen mit dem Azure-Portal][Pause compute with Azure portal]
* [Anhalten von Computeressourcen mit PowerShell][Pause compute with PowerShell]
* [Anhalten von Computeressourcen mit REST-APIs][Pause compute with REST APIs]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Fortsetzen von Computeressourcen
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Verwenden Sie zum Fortsetzen einer Datenbank eine dieser individuellen Methoden.

* [Fortsetzen von Computeressourcen mit dem Azure-Portal][Resume compute with Azure portal]
* [Fortsetzen von Computeressourcen mit PowerShell][Resume compute with PowerShell]
* [Fortsetzen von Computeressourcen mit REST-APIs][Resume compute with REST APIs]

## <a name="permissions"></a>Berechtigungen
Zum Skalieren der Datenbank sind die in [ALTER DATABASE][ALTER DATABASE] beschriebenen Berechtigungen erforderlich.  Zum Anhalten und Fortsetzen ist die Berechtigung [Mitwirkender von SQL DB][SQL DB Contributor] erforderlich, insbesondere „Microsoft.Sql/servers/databases/action“.

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Nächste Schritte
Informationen zu zusätzlichen zentralen Leistungskonzepten finden Sie in den folgenden Artikeln:

* [Parallelitäts- und Workloadverwaltung][Workload and concurrency managment]
* [Tabellenentwurf – Übersicht][Table design overview]
* [Tabellenverteilung][Table distribution]
* [Tabellenindizierung][Table indexing]
* [Tabellenpartitionierung][Table partitioning]
* [Tabellenstatistiken][Table statistics]
* [Best Practices][Best practices]

<!--Image reference-->

<!--Article references-->
[data warehouse units (DWUs)]: ./sql-data-warehouse-overview-what-is.md

[Scale compute power with Azure portal]: ./sql-data-warehouse-manage-compute-portal.md#scale-compute-power
[Scale compute power with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#scale-compute-bk
[Scale compute power with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#scale-compute-bk
[Scale compute power with TSQL]: ./sql-data-warehouse-manage-compute-tsql.md#scale-compute-bk

[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md

[Pause compute with Azure portal]:  ./sql-data-warehouse-manage-compute-portal.md#pause-compute-bk
[Pause compute with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#pause-compute-bk
[Pause compute with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#pause-compute-bk

[Resume compute with Azure portal]:  ./sql-data-warehouse-manage-compute-portal.md#resume-compute-bk
[Resume compute with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#resume-compute-bk
[Resume compute with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#resume-compute-bk

[Workload and concurrency managment]: ./sql-data-warehouse-develop-concurrency.md
[Table design overview]: ./sql-data-warehouse-tables-overview.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Table indexing]: ./sql-data-warehouse-tables-index.md
[Table partitioning]: ./sql-data-warehouse-tables-partition.md
[Table statistics]: ./sql-data-warehouse-tables-statistics.md
[Best practices]: ./sql-data-warehouse-best-practices.md
[development overview]: ./sql-data-warehouse-overview-develop.md

[SQL DB Contributor]: ../active-directory/role-based-access-built-in-roles.md#sql-db-contributor

<!--MSDN references-->
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Azure portal]: http://portal.azure.com/

