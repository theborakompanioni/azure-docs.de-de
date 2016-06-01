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
   ms.date="05/06/2016"
   ms.author="barbkess;sonyama"/>

# Verwalten von Computeleistung in Azure SQL Data Warehouse (Übersicht)

> [AZURE.SELECTOR]
- [Übersicht](sql-data-warehouse-overview-manage-compute.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [REST](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)

In der Architektur von SQL Data Warehouse werden Speicher- und Computeressourcen voneinander getrennt, sodass diese unabhängig voneinander skaliert werden können. So können Sie Kosten sparen, indem Sie die Leistung horizontal skalieren und nur für die Leistung bezahlen, die Sie benötigen.

In dieser Übersicht werden die folgenden SQL Data Warehouse-Funktionen zum horizontalen Skalieren der Leistung beschrieben und Empfehlungen aufgeführt, wie und wann sie zu verwenden sind.

- Skalieren der Computeleistung durch Anpassen von Data Warehouse-Einheiten (DWUs)
- Anhalten oder Fortsetzen von Computeressourcen

<a name="scale-performance-bk"></a>

## Skalieren der Leistung

In SQL Data Warehouse können Sie die Leistung schnell horizontal hoch- und wieder herunterskalieren, indem Sie die Computeressourcen für die CPU, den Arbeitsspeicher oder die E/A-Bandbreite erhöhen bzw. verringern. Um die Leistung zu skalieren, müssen Sie lediglich die Anzahl der DWUs (Data Warehouse-Einheiten) anpassen, die Ihrer Datenbank von SQL Data Warehouse zugewiesen werden. SQL Data Warehouse nimmt die Änderungen schnell vor und kümmert sich um alle zugrunde liegenden Änderungen an Hardware oder Software.

>[AZURE.NOTE] Vorbei sind die Zeiten, in denen Sie die Art der Prozessoren, die Menge des Arbeitsspeichers oder den Speichertyp ermitteln mussten, um eine hohe Leistung des Data Warehouse zu erreichen. Durch das Verlagern des Data Warehouse in die Cloud müssen Sie nicht mehr mit Low-Level-Hardwareprobleme befassen. Stattdessen stellt SQL Data Warehouse Ihnen diese Frage: Wie schnell möchten Sie Ihre Daten analysieren?

### Was ist eine DWU?

Eine *Data Warehouse-Einheit* (DWU) ist ein Maß für die zugrunde liegenden Computefunktionen für Ihre Datenbank zu einem bestimmten Zeitpunkt. Mit zunehmender Anzahl von DWUs führt SQL Data Warehouse Vorgänge (z.B. das Laden oder Abfragen von Daten) parallel über stärker verteilte CPU- und Speicherressourcen aus. Dadurch verringert sich die Latenz, und die Leistung verbessert sich.

DWUs basieren auf Auslastungs- und Scanraten. Wenn Sie die Anzahl von DWUs erhöhen, werden damit die Auslastungsrate und die Scanrate erhöht.

- **Auslastungsrate**. Die Anzahl von Datensätzen, die SQL Data Warehouse pro Sekunde erfassen kann. Insbesondere ist dies die Anzahl von Datensätzen, die SQL Data Warehouse mithilfe von PolyBase aus Azure-Blobspeicher in einen gruppierten Columnstore-Index importieren kann. 

- **Scanrate**. Die Anzahl von Datensätzen, die eine Abfrage pro Sekunde aus SQL Data Warehouse abrufen kann. Insbesondere ist dies die Anzahl von Datensätzen, die SQL Data Warehouse durch Ausführen einer Data Warehousing-Abfrage für einen gruppierten Columnstore-Index zurückgeben kann. Zum Testen der Scanrate wird eine Data Warehousing-Standardabfrage verwendet, mit der eine große Zahl von Zeilen gescannt und anschließend eine komplexe Aggregation durchgeführt wird. Dies ist ein E/A- und CPU-intensiver Vorgang.

>[AZURE.NOTE] Wir haben einige wichtige Leistungsverbesserungen gemessen und vorgenommen und werden die voraussichtlichen Raten in Kürze mitteilen. Während der Preview nehmen wir fortlaufend Verbesserungen (z. B. an Komprimierung und Caching) vor, um diese Raten zu erhöhen und eine vorhersagbare Skalierung sicherzustellen.

Eine Liste der DWUs finden Sie im Artikel [Kapazitätsgrenzen][] unter den SLOs.

### Wie skaliere ich die Leistung?

Um die Computeleistung flexibel zu erhöhen oder zu verringern, ändern Sie einfach die Einstellung für die Data Warehouse-Einheiten (DWUs) für Ihre Datenbank. Hinter den Kulissen ändert SQL Data Warehouse die Zuteilung von CPU- und Speicherressourcen mithilfe der schnellen und einfachen Bereitstellungsfunktionen der SQL-Datenbank.

DWUs werden in Blöcken von je 100 zugeordnet, aber nicht alle Blöcke sind verfügbar. Mit den DWUs steigt die Leistung linear an. Auf höheren DWU-Ebenen müssen Sie mehr als 100 DWUs hinzufügen, um eine merkliche Verbesserung der Leistung zu erzielen. Damit Sie sinnvolle DWU-Steigerungen wählen können, bieten wir die DWU-Ebenen an, mit denen Sie die besten Ergebnisse erzielen.
 
Zum Anpassen der DWUs können Sie eine beliebige dieser individuellen Methoden verwenden.

- [Skalierung von Computeleistung mit dem Azure-Portal][]
- [Skalierung von Computeleistung mit PowerShell][]
- [Skalierung von Computeleistung mit REST-APIs][]
- [Skalierung von Computeleistung mit TSQL][]

### Wie viele DWUs sollte ich verwenden?
 
Die Leistung in SQL Data Warehouse wird linear skaliert, und das Ändern von einer Compute-Skalierung in eine andere (z. B. von 100 DWUs auf 2000 DWUs) erfolgt in Sekunden. So erhalten Sie die Flexibilität, mit verschiedenen DWU-Einstellungen zu experimentieren, bis Sie die beste Lösung für Ihr Szenario gefunden haben.

> [AZURE.NOTE] Bei niedrigeren Datenmengen werden Sie die erwartete Leistungsskalierung möglicherweise nicht feststellen. Wir empfehlen Ihnen, mit Datenvolumen von 1 TB oder höher zu beginnen, um bei Leistungstests genaue Ergebnisse zu erzielen.

Empfehlungen für das Finden der besten DWU-Anzahl für Ihre Workload:

1. Beginnen Sie bei einem in der Entwicklung befindlichen Data Warehouse mit einer geringen Anzahl von DWUs.
2. Überwachen Sie die Anwendungsleistung, und beobachten Sie dabei die Anzahl der ausgewählten DWUs im Vergleich zur beobachteten Leistung.
3. Bestimmen Sie durch Annahme einer linearen Skalierung, wie viel schneller oder langsamer die Leistung für Sie sein muss, um die optimale Leistungsstufe für Ihre Anforderungen zu erreichen.
4. Erhöhen oder verringern Sie die Anzahl von DWUs proportional dazu, wie viel schneller oder langsamer Ihre Workload ausgeführt werden soll. Der Dienst reagiert schnell und passt die Computeressourcen gemäß den neuen DWU-Anforderungen an.
5. Nehmen Sie weitere Anpassungen vor, bis Sie die optimale Leistungsstufe für Ihre geschäftlichen Anforderungen erreichen.

### Wann sollte ich DWUs skalieren?

Generell möchten wir DWUs einfach halten. Falls Sie schnellere Ergebnisse benötigen, sollten Sie Ihre DWUs erhöhen und für mehr Leistung zahlen. Falls Sie weniger Computeleistung benötigen, sollten Sie Ihre DWUs verringern und nur für die verbrauchte Leistung bezahlen.

Empfehlungen für den Zeitpunkt für die Skalierung von DWUs:

1. Wenn Sie eine Anwendung mit wechselnder Workload haben, skalieren Sie die DWU-Anzahl nach oben oder unten, um Spitzen und Tiefpunkte zu berücksichtigen. Wenn eine Workload z.B. in der Regel am Ende des Monats einen Spitzenwert aufweist, fügen Sie während dieser Spitzenzeiten weitere DWUs hinzu, und entfernen Sie sie wieder, wenn diese Spitzenzeiten vorbei sind.
1. Bevor Sie einen umfangreichen Vorgang zum Laden oder Transformieren von Daten durchführen, skalieren Sie die DWUs zentral hoch, damit die Daten schneller verfügbar sind.

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

<a name="next-steps-bk"></a>

## Nächste Schritte
Informationen zu zusätzlichen zentralen Leistung- und Skalierbarkeitskonzepten finden Sie in den folgenden Artikeln:

- [Parallelitätsmodell][]
- [Entwerfen von Tabellen][]
- [Auswählen eines Hashverteilungsschlüssels für die Tabelle][]
- [Statistiken zum Verbessern der Leistung][]

<!--Image reference-->

<!--Article references-->

[Skalierung von Computeleistung mit dem Azure-Portal]: ./sql-data-warehouse-manage-compute-tasks-portal.md#task-1-scale-performance
[Skalierung von Computeleistung mit PowerShell]: ./sql-data-warehouse-manage-compute-tasks-powershell.md#task-1-scale-performance
[Skalierung von Computeleistung mit REST-APIs]: ./sql-data-warehouse-manage-compute-tasks-rest-api.md#task-1-scale-performance
[Skalierung von Computeleistung mit TSQL]: ./sql-data-warehouse-manage-compute-tasks-tsql.md

[Kapazitätsgrenzen]: ./sql-data-warehouse-service-capacity-limits.md

[Anhalten von Computeressourcen mit dem Azure-Portal]: ./sql-data-warehouse-manage-scale-out-tasks.md#task-2-pause-compute
[Anhalten von Computeressourcen mit PowerShell]: ./sql-data-warehouse-manage-scale-out-tasks-powershell.md#task-2-pause-compute
[Anhalten von Computeressourcen mit REST-APIs]: ./sql-data-warehouse-manage-scale-out-tasks-rest-api.md#task-2-pause-compute
[Fortsetzen von Computeressourcen mit dem Azure-Portal]: ./sql-data-warehouse-manage-scale-out-tasks.md#task-3-resume-compute
[Fortsetzen von Computeressourcen mit PowerShell]: ./sql-data-warehouse-manage-scale-out-tasks-powershell.md#task-3-resume-compute
[Fortsetzen von Computeressourcen mit REST-APIs]: ./sql-data-warehouse-manage-scale-out-tasks-rest-api.md#task-3-resume-compute

[Parallelitätsmodell]: sql-data-warehouse-develop-concurrency.md
[Entwerfen von Tabellen]: sql-data-warehouse-develop-table-design.md
[Auswählen eines Hashverteilungsschlüssels für die Tabelle]: sql-data-warehouse-develop-hash-distribution-key.md
[Statistiken zum Verbessern der Leistung]: sql-data-warehouse-develop-statistics.md
[development overview]: sql-data-warehouse-overview-develop.md



<!--MSDN references-->


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/

<!---HONumber=AcomDC_0518_2016-->