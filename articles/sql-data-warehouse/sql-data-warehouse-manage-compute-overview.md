---
title: "Verwalten von Computeleistung in Azure SQL Data Warehouse (Übersicht) | Microsoft Docs"
description: "Funktionen zur horizontalen Leistungshochskalierung in Azure SQL Data Warehouse. Führen Sie eine horizontale Skalierung durch, indem Sie DWUs anpassen oder Computeressourcen anhalten und fortsetzen, um Kosten zu sparen."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: 
ms.assetid: e13a82b0-abfe-429f-ac3c-f2b6789a70c6
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 03/22/2017
ms.author: elbutter
translationtype: Human Translation
ms.sourcegitcommit: b4802009a8512cb4dcb49602545c7a31969e0a25
ms.openlocfilehash: f4a79413bc5e660504b4b6b48fcf496fb0f08ade
ms.lasthandoff: 03/29/2017


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

In der Architektur von SQL Data Warehouse werden Speicher- und Computeressourcen voneinander getrennt, sodass diese unabhängig voneinander skaliert werden können. Daher können die Computeressourcen skaliert werden, um Leistungsanforderungen unabhängig von der Datenmenge zu erfüllen. Eine logische Konsequenz dieser Architektur ist es, dass die [Abrechnung][billed] für Compute- und Speicherressourcen unabhängig voneinander erfolgt. 

Diese Übersicht erläutert, wie die horizontale Hochskalierung bei SQL Data Warehouse funktioniert und wie Sie die SQL Data Warehouse-Funktionen zum Anhalten, Fortsetzen und Skalieren verwenden. Informationen zur Beziehung zwischen DWUs und Leistung finden Sie auf der Seite [Data Warehouse-Einheiten (DWUs)][data warehouse units (DWUs)]. 

## <a name="how-compute-management-operations-work-in-sql-data-warehouse"></a>Funktionsweise der Computeverwaltung in SQL Data Warehouse
Die Architektur für SQL Data Warehouse besteht aus einem Steuerknoten, mehreren Computeknoten und der Speicherebene, die sich über 60 Verteilungen erstreckt. 

Während einer normalen aktiven Sitzung in SQL Data Warehouse verwaltet der Hauptknoten Ihres Systems die Metadaten und enthält den Optimierer für verteilte Abfragen. Unterhalb dieses Hauptknotens befinden sich die Computeknoten und die Speicherebene. Bei einer DWU 400 besteht Ihr System aus einem Hauptknoten, vier Computeknoten und der Speicherebene, die sich über 60 Verteilungen erstreckt. 

Wenn Sie einen Vorgang zum Skalieren oder Anhalten starten, beendet das System zunächst alle eingehenden Abfragen und führt dann ein Rollback der Transaktionen durch, um einen konsistenten Zustand zu gewährleisten. Bei Skalierungsvorgängen erfolgt die Skalierung nur einmal, sobald das Rollback der Transaktionen abgeschlossen ist. Bei einem Vorgang zum zentralen Hochskalieren stellt das System die gewünschte zusätzliche Anzahl von Computeknoten bereit und beginnt dann damit, die Computeknoten erneut an die Speicherebene anzufügen. Bei einem Vorgang zum zentralen Herunterskalieren werden die nicht benötigten Knoten freigegeben, und die verbleibenden Computeknoten fügen sich selbst erneut an die geeignete Anzahl von Verteilungen an. Bei einem Anhaltevorgang werden alle Computeknoten freigegeben, und das System führt verschiedene Metadatenvorgänge durch, um das finale System in einen stabilen Zustand zu versetzen.

| DWU  | \# Computeknoten | \# Verteilungen pro Knoten |
| ---- | ------------------ | ---------------------------- |
| 100  | 1                  | 60                           |
| 200  | 2                  | 30                           |
| 300  | 3                  | 20                           |
| 400  | 4                  | 15                           |
| 500  | 5                  | 12                           |
| 600  | 6                  | 10                           |
| 1000 | 10                 | 6                            |
| 1200 | 12                 | 5                            |
| 1500 | 15                 | 4                            |
| 2000 | 20                 | 3                            |
| 3000 | 30                 | 2                            |
| 6000 | 60                 | 1                            |

Zum Verwalten der Computeleistung stehen die drei folgenden primären Funktionen zur Verfügung:

1. Anhalten
2. Fortfahren
3. Skalieren

Jeder dieser Vorgänge kann mehrere Minuten in Anspruch nehmen. Wenn Sie das Skalieren/Anhalten/Fortsetzen automatisch durchführen, empfiehlt es sich, eine Logik zu implementieren, die sicherstellt, dass bestimmte Vorgänge abgeschlossen wurden, bevor mit einer andern Aktion fortgefahren wird. 

Überprüfen Sie den Datenbankzustand auf verschiedenen Endpunkten, um sicherzugehen, dass die Automatisierung dieser Vorgänge ordnungsgemäß implementiert werden kann. Über das Portal erhalten Sie bei Abschluss eines Vorgangs eine Benachrichtigung und Informationen zum aktuellen Zustand der Datenbanken. Das Portal ermöglicht aber keine programmgesteuerte Überprüfung des Zustands. 

>  [!NOTE]
>
>  Funktionen für die Computeverwaltung stehen nicht auf allen Endpunkten zur Verfügung.
>
>  

|              | Fortsetzen | Skalieren | Überprüfen des Datenbankzustands |
| ------------ | ------------ | ----- | -------------------- |
| Azure-Portal | Ja          | Ja   | **Nein**               |
| PowerShell   | Ja          | Ja   | Ja                  |
| REST-API     | Ja          | Ja   | Ja                  |
| T-SQL        | **Nein**       | Ja   | Ja                  |



<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a>Skalieren von Computeressourcen

Die Leistung in SQL Data Warehouse wird in [Data Warehouse-Einheiten (Data Warehouse Units, DWUs)][data warehouse units (DWUs)] gemessen. Dies ist eine abstrahierte Maßeinheit für Computeressourcen wie z.B. CPU, Arbeitsspeicher und E/A-Bandbreite. Um die Systemleistung zu skalieren, stehen Benutzern verschiedene Möglichkeiten zur Verfügung: das Portal, T-SQL und REST-APIs. 

### <a name="how-do-i-scale-compute"></a>Wie skaliere ich Computeressourcen?
Die Computeleistung für SQL Data Warehouse wird durch Ändern der DWU-Einstellung verwaltet. Die Leistung steigt [linear][linearly], wenn Sie für bestimmte Vorgänge weitere DWUs hinzufügen.  Wir bieten DWU-Pakete, die sicherstellen, dass sich die Leistung merklich ändert, wenn Sie Ihr System zentral hoch- oder herunterskalieren. 

Zum Anpassen der DWUs können Sie eine beliebige dieser individuellen Methoden verwenden.

* [Skalieren von Computeleistung mit dem Azure-Portal][Scale compute power with Azure portal]
* [Skalieren von Computeleistung mit PowerShell][Scale compute power with PowerShell]
* [Skalieren von Computeleistung mit REST-APIs][Scale compute power with REST APIs]
* [Skalieren von Computeleistung mit TSQL][Scale compute power with TSQL]

### <a name="how-many-dwus-should-i-use"></a>Wie viele DWUs sollte ich verwenden?

Damit Sie verstehen, wie Ihr idealer DWU-Wert lautet, sollten Sie versuchsweise hoch- und herunterskalieren und nach dem Laden der Daten einige Abfragen ausführen. Da die Skalierung schnell erfolgt, können Sie innerhalb einer Stunde verschiedene Leistungsebenen ausprobieren. 

> [!Note] 
> SQL Data Warehouse ist für die Verarbeitung großer Datenmengen konzipiert. Um die Skalierungsleistung mit eigenen Augen zu sehen – insbesondere bei größeren DWUs –, sollten Sie ein großes Dataset mit 1 TB oder mehr verwenden.

Empfehlungen für das Finden der besten DWU-Anzahl für Ihre Workload:

1. Beginnen Sie bei einem in der Entwicklung befindlichen Data Warehouse mit einer niedrigeren DWU-Leistungsebene.  Ein guter Ausgangspunkt ist DW400 oder DW200.
2. Überwachen Sie die Anwendungsleistung, und beobachten Sie dabei die Anzahl der ausgewählten DWUs im Vergleich zur beobachteten Leistung.
3. Bestimmen Sie durch Annahme einer linearen Skalierung, wie viel schneller oder langsamer die Leistung für Sie sein muss, um die optimale Leistungsstufe für Ihre Anforderungen zu erreichen.
4. Erhöhen oder verringern Sie die Anzahl von DWUs proportional dazu, wie viel schneller oder langsamer Ihre Workload ausgeführt werden soll. 
5. Nehmen Sie weitere Anpassungen vor, bis Sie die optimale Leistungsstufe für Ihre geschäftlichen Anforderungen erreichen.

> [!NOTE]
>
> Die Abfrageleistung steigt bei höherem Parallelisierungsgrad nur, wenn die Verarbeitung auf mehrere Computeknoten aufgeteilt werden kann. Wenn Sie feststellen, dass sich die Systemleistung durch Skalierung nicht ändert, lesen Sie unsere Artikel zum Thema Leistungsoptimierung, um zu ermitteln, ob möglicherweise Ihre Daten ungleichmäßig verteilt sind oder sehr viele Datenverschiebungen stattfinden. 

### <a name="when-should-i-scale-dwus"></a>Wann sollte ich DWUs skalieren?
Die Skalierung von DWUs ändert die folgenden wichtigen Szenarien:

1. Lineare Änderung der Systemleistung bei Scans, Aggregationen und CTAS-Anweisungen
2. Erhöhen der Anzahl von Readern und Writern beim Laden mit PolyBase
3. Maximale Anzahl von gleichzeitigen Abfragen und Parallelitätsslots

Empfehlungen für den Zeitpunkt für die Skalierung von DWUs:

1. Bevor Sie einen umfangreichen Vorgang zum Laden oder Transformieren von Daten durchführen, skalieren Sie die DWUs zentral hoch, damit die Daten schneller verfügbar sind.
2. Skalieren Sie die DWUs während der Hauptgeschäftszeiten, um eine größere Anzahl gleichzeitiger Abfragen verarbeiten zu können. 

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

<a name="check-compute-bk"></a>

## <a name="check-database-state"></a>Überprüfen des Datenbankzustands 

Verwenden Sie zum Fortsetzen einer Datenbank eine dieser individuellen Methoden.

- [Überprüfen des Datenbankzustands mit T-SQL][Check database state with T-SQL]
- [Überprüfen des Datenbankzustands mit PowerShell][Check database state with PowerShell]
- [Überprüfen des Datenbankzustands mit REST-APIs][Check database state with REST APIs]

## <a name="permissions"></a>Berechtigungen

Zum Skalieren der Datenbank sind die in [ALTER DATABASE][ALTER DATABASE] beschriebenen Berechtigungen erforderlich.  Zum Anhalten und Fortsetzen ist die Berechtigung [Mitwirkender von SQL DB][SQL DB Contributor] erforderlich, insbesondere „Microsoft.Sql/servers/databases/action“.

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Nächste Schritte
Informationen zu zusätzlichen zentralen Leistungskonzepten finden Sie in den folgenden Artikeln:

* [Parallelitäts- und Workloadverwaltung][Workload and concurrency management]
* [Tabellenentwurf – Übersicht][Table design overview]
* [Tabellenverteilung][Table distribution]
* [Tabellenindizierung][Table indexing]
* [Tabellenpartitionierung][Table partitioning]
* [Tabellenstatistiken][Table statistics]
* [Best Practices][Best practices]

<!--Image reference-->

<!--Article references-->
[data warehouse units (DWUs)]: ./sql-data-warehouse-overview-what-is.md#predictable-and-scalable-performance-with-data-warehouse-units
[billed]: https://azure.microsoft.com/en-us/pricing/details/sql-data-warehouse/
[linearly]: ./sql-data-warehouse-overview-what-is.md#predictable-and-scalable-performance-with-data-warehouse-units
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

[Check database state with T-SQL]: ./sql-data-warehouse-manage-compute-tsql.md#check-database-state-and-operation-progress
[Check database state with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#check-database-state
[Check database state with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#check-database-state

[Workload and concurrency management]: ./sql-data-warehouse-develop-concurrency.md
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

