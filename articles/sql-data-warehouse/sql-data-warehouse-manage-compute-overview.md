<properties
   pageTitle="Verwalten von Computeleistung in Azure SQL Data Warehouse (Übersicht) | Microsoft Azure"
   description="Funktionen zur horizontalen Leistungshochskalierung in Azure SQL Data Warehouse. Führen Sie eine horizontale Skalierung durch, indem Sie DWUs anpassen oder Computeressourcen anhalten und fortsetzen, um Kosten zu sparen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/03/2016"
   ms.author="barbkess;sonyama"/>

# Verwalten von Computeleistung in Azure SQL Data Warehouse (Übersicht)

> [AZURE.SELECTOR]
- [Übersicht](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [REST](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)

In der Architektur von SQL Data Warehouse werden Speicher- und Computeressourcen voneinander getrennt, sodass diese unabhängig voneinander skaliert werden können. So können Sie Kosten sparen, indem Sie die Leistung horizontal skalieren und nur für die Leistung bezahlen, die Sie benötigen.

In dieser Übersicht werden die folgenden SQL Data Warehouse-Funktionen zum horizontalen Skalieren der Leistung beschrieben und Empfehlungen aufgeführt, wie und wann sie zu verwenden sind.

- Skalieren der Computeleistung durch Anpassen von [Data Warehouse-Einheiten (DWUs)][]
- Anhalten oder Fortsetzen von Computeressourcen

<a name="scale-performance-bk"></a>

## Skalieren der Leistung

In SQL Data Warehouse können Sie die Leistung schnell horizontal hoch- und wieder herunterskalieren, indem Sie die Computeressourcen für die CPU, den Arbeitsspeicher oder die E/A-Bandbreite erhöhen bzw. verringern. Um die Leistung zu skalieren, müssen Sie lediglich die Anzahl der [DWUs (Data Warehouse-Einheiten)][] anpassen, die Ihrer Datenbank von SQL Data Warehouse zugewiesen werden. SQL Data Warehouse nimmt die Änderungen schnell vor und kümmert sich um alle zugrunde liegenden Änderungen an Hardware oder Software.

Vorbei sind die Zeiten, in denen Sie die Art der Prozessoren, die Menge des Arbeitsspeichers oder den Speichertyp ermitteln mussten, um eine hohe Leistung des Data Warehouse zu erreichen. Durch das Verlagern des Data Warehouse in die Cloud müssen Sie nicht mehr mit Low-Level-Hardwareprobleme befassen. Stattdessen stellt SQL Data Warehouse Ihnen diese Frage: Wie schnell möchten Sie Ihre Daten analysieren?

### Wie skaliere ich die Leistung?

Um die Computeleistung flexibel zu erhöhen oder zu verringern, ändern Sie einfach die Einstellung für die [Data Warehouse-Einheiten (DWUs)][] für Ihre Datenbank. Die Leistung steigt linear an, wenn Sie weitere DWUs hinzufügen. Auf höheren DWU-Ebenen müssen Sie mehr als 100 DWUs hinzufügen, um eine merkliche Verbesserung der Leistung zu erzielen. Damit Sie sinnvolle DWU-Steigerungen wählen können, bieten wir die DWU-Ebenen an, mit denen Sie die besten Ergebnisse erzielen.
 
Zum Anpassen der DWUs können Sie eine beliebige dieser individuellen Methoden verwenden.

- [Skalierung von Computeleistung mit dem Azure-Portal][]
- [Skalierung von Computeleistung mit PowerShell][]
- [Skalierung von Computeleistung mit REST-APIs][]
- [Skalierung von Computeleistung mit TSQL][]

### Wie viele DWUs sollte ich verwenden?
 
Die Leistung in SQL Data Warehouse wird linear skaliert, und das Ändern von einer Compute-Skalierung in eine andere (z. B. von 100 DWUs auf 2000 DWUs) erfolgt in Sekunden. So erhalten Sie die Flexibilität, mit verschiedenen DWU-Einstellungen zu experimentieren, bis Sie die beste Lösung für Ihr Szenario gefunden haben.

Damit Sie verstehen, wie Ihr idealer DWU-Wert lautet, sollten Sie versuchsweise hoch- und herunterskalieren und nach dem Laden der Daten einige Abfragen ausführen. Da die Skalierung schnell geht, können Sie es mit mehreren unterschiedlichen Leistungsebenen probieren, ohne dass dies länger als eine Stunde dauert. Beachten Sie, dass SQL Data Warehouse für die Verarbeitung von großen Datenmengen ausgelegt ist. Um die wahren Skalierungsmöglichkeiten zu nutzen, vor allem im Rahmen der von uns angebotenen größeren Skalierungen, sollten Sie ein großes Dataset von 1 TB oder darüber verwenden.

Empfehlungen für das Finden der besten DWU-Anzahl für Ihre Workload:

1. Beginnen Sie bei einem in der Entwicklung befindlichen Data Warehouse mit einer geringen Anzahl von DWUs. Ein guter Ausgangspunkt ist DW400 oder DW200.
2. Überwachen Sie die Anwendungsleistung, und beobachten Sie dabei die Anzahl der ausgewählten DWUs im Vergleich zur beobachteten Leistung.
3. Bestimmen Sie durch Annahme einer linearen Skalierung, wie viel schneller oder langsamer die Leistung für Sie sein muss, um die optimale Leistungsstufe für Ihre Anforderungen zu erreichen.
4. Erhöhen oder verringern Sie die Anzahl von DWUs proportional dazu, wie viel schneller oder langsamer Ihre Workload ausgeführt werden soll. Der Dienst reagiert schnell und passt die Computeressourcen gemäß den neuen DWU-Anforderungen an.
5. Nehmen Sie weitere Anpassungen vor, bis Sie die optimale Leistungsstufe für Ihre geschäftlichen Anforderungen erreichen.

### Wann sollte ich DWUs skalieren?

Falls Sie schnellere Ergebnisse benötigen, sollten Sie Ihre DWUs erhöhen und für mehr Leistung zahlen. Falls Sie weniger Computeleistung benötigen, sollten Sie Ihre DWUs verringern und nur für die verbrauchte Leistung bezahlen.

Empfehlungen für den Zeitpunkt für die Skalierung von DWUs:

1. Wenn Sie eine Anwendung mit wechselnder Workload haben, skalieren Sie die DWU-Anzahl nach oben oder unten, um Spitzen und Tiefpunkte zu berücksichtigen. Wenn eine Workload z.B. in der Regel am Ende des Monats einen Spitzenwert aufweist, fügen Sie während dieser Spitzenzeiten weitere DWUs hinzu, und entfernen Sie sie wieder, wenn diese Spitzenzeiten vorbei sind.
2. Bevor Sie einen umfangreichen Vorgang zum Laden oder Transformieren von Daten durchführen, skalieren Sie die DWUs zentral hoch, damit die Daten schneller verfügbar sind.

<a name="pause-compute-bk"></a>

## Anhalten von Computeressourcen

[AZURE.INCLUDE [Beschreibung des Anhaltens in SQL Data Warehouse](../../includes/sql-data-warehouse-pause-description.md)]

Verwenden Sie zum Anhalten einer Datenbank eine dieser individuellen Methoden.

- [Anhalten von Computeressourcen mit dem Azure-Portal][]
- [Anhalten von Computeressourcen mit PowerShell][]
- [Anhalten von Computeressourcen mit REST-APIs][]

<a name="resume-compute-bk"></a>

## Fortsetzen von Computeressourcen

[AZURE.INCLUDE [Beschreibung des Fortsetzens in SQL Data Warehouse](../../includes/sql-data-warehouse-resume-description.md)]

Verwenden Sie zum Fortsetzen einer Datenbank eine dieser individuellen Methoden.

- [Fortsetzen von Computeressourcen mit dem Azure-Portal][]
- [Fortsetzen von Computeressourcen mit PowerShell][]
- [Fortsetzen von Computeressourcen mit REST-APIs][]

## Berechtigungen

Zum Skalieren der Datenbank sind die in [ALTER DATABASE][] beschriebenen Berechtigungen erforderlich. Zum Anhalten und Fortsetzen ist die [Mitwirkender von SQL DB][]-Berechtigung erforderlich, insbesondere „Microsoft.Sql/servers/databases/action“.

<a name="next-steps-bk"></a>

## Nächste Schritte
Informationen zu zusätzlichen zentralen Leistungskonzepten finden Sie in den folgenden Artikeln:

- [Parallelitäts- und Workloadverwaltung][]
- [Overview of tables in SQL Data Warehouse (Übersicht über Tabellen in SQL Data Warehouse)][]
- [Tabellenverteilung][]
- [Tabellenindizierung][]
- [Tabellenpartitionierung][]
- [Tabellenstatistiken][]
- [Bewährte Methoden][]

<!--Image reference-->

<!--Article references-->
[DWUs (Data Warehouse-Einheiten)]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[Data Warehouse-Einheiten (DWUs)]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

[Skalierung von Computeleistung mit dem Azure-Portal]: ./sql-data-warehouse-manage-compute-portal.md#scale-compute-bk
[Skalierung von Computeleistung mit PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#scale-compute-bk
[Skalierung von Computeleistung mit REST-APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#scale-compute-bk
[Skalierung von Computeleistung mit TSQL]: ./sql-data-warehouse-manage-compute-tsql.md#scale-compute-bk

[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md

[Anhalten von Computeressourcen mit dem Azure-Portal]: ./sql-data-warehouse-manage-compute-portal.md#pause-compute-bk
[Anhalten von Computeressourcen mit PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#pause-compute-bk
[Anhalten von Computeressourcen mit REST-APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#pause-compute-bk

[Fortsetzen von Computeressourcen mit dem Azure-Portal]: ./sql-data-warehouse-manage-compute-portal.md#resume-compute-bk
[Fortsetzen von Computeressourcen mit PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#resume-compute-bk
[Fortsetzen von Computeressourcen mit REST-APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#resume-compute-bk

[Parallelitäts- und Workloadverwaltung]: ./sql-data-warehouse-develop-concurrency.md
[Overview of tables in SQL Data Warehouse (Übersicht über Tabellen in SQL Data Warehouse)]: ./sql-data-warehouse-tables-overview.md
[Tabellenverteilung]: ./sql-data-warehouse-tables-distribute.md
[Tabellenindizierung]: ./sql-data-warehouse-tables-index.md
[Tabellenpartitionierung]: ./sql-data-warehouse-tables-partition.md
[Tabellenstatistiken]: ./sql-data-warehouse-tables-statistics.md
[Bewährte Methoden]: ./sql-data-warehouse-best-practices.md
[development overview]: ./sql-data-warehouse-overview-develop.md

[Mitwirkender von SQL DB]: ../active-directory/role-based-access-built-in-roles.md#sql-db-contributor

<!--MSDN references-->
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Azure portal]: http://portal.azure.com/

<!---HONumber=AcomDC_0907_2016-->