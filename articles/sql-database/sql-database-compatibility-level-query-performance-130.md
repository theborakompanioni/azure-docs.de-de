---
title: "Datenbankkompatibilitätsgrad 130 – Azure-SQL-Datenbank | Microsoft-Dokumentation"
description: "In diesem Artikel untersuchen wir die Vorteile der Ausführung von Azure SQL-Datenbank mit dem Kompatibilitätsgrad 130 sowie die Vorteile des neuen Abfrageoptimierers und des Abfrageprozessors. Wir sehen uns zudem die möglichen Auswirkungen auf die Abfrageleistung für die vorhandenen SQL-Anwendungen an."
services: sql-database
documentationcenter: 
author: alainlissoir
manager: jhubbard
editor: 
ms.assetid: 8619f90b-7516-46dc-9885-98429add0053
ms.service: sql-database
ms.custom: monitor and tune
ms.workload: data-management
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.topic: article
ms.date: 03/03/2017
ms.author: alainl
translationtype: Human Translation
ms.sourcegitcommit: 2f03ba60d81e97c7da9a9fe61ecd419096248763
ms.openlocfilehash: 5649895d1ae39d9a7fa863407b5341f1cdf567ee
ms.lasthandoff: 03/04/2017


---
# <a name="improved-query-performance-with-compatibility-level-130-in-azure-sql-database"></a>Verbesserte Abfrageleistung mit Kompatibilitätsgrad 130 in Azure SQL-Datenbank
Azure SQL-Datenbank führt auf transparente Weise Hunderttausende Datenbanken mit vielen verschiedenen Kompatibilitätsgraden aus und sorgt so für alle Kunden für die Aufrechterhaltung und Sicherstellung der Abwärtskompatibilität mit der entsprechenden Version von Microsoft SQL Server.

In diesem Artikel untersuchen wir die Vorteile der Ausführung von Azure SQL-Datenbank mit dem Kompatibilitätsgrad 130 sowie die Vorteile des neuen Abfrageoptimierers und des Abfrageprozessors. Wir sehen uns zudem die möglichen Auswirkungen auf die Abfrageleistung für die vorhandenen SQL-Anwendungen an.

Zur Erinnerung sind hier noch einmal die SQL-Versionen mit den standardmäßigen Kompatibilitätsgraden angegeben:

* 100: in SQL Server 2008 und Azure SQL-Datenbank V11.
* 110: in SQL Server 2012 und Azure SQL-Datenbank V11.
* 120: in SQL Server 2014 und Azure SQL-Datenbank V12.
* 130: in SQL Server 2016 und Azure SQL-Datenbank V12.

> [!IMPORTANT]
> Der standardmäßige Kompatibilitätsgrad für **neu erstellte** Datenbanken ist 130.
> 
> Datenbanken, die vor Mitte Juni 2016 erstellt wurden, haben den zur Zeit ihrer Erstellung aktuellen Kompatibilitätsgrad. Für Datenbanken, die von einer früheren Version von Azure SQL-Datenbank migriert wurden, gilt ein Kompatibilitätsgrad von 100 oder 110. Der Kompatibilitätsgrad der Masterdatenbank eines Servers, der vor Mitte Juni 2016 erstellt wurde, entspricht dem zum Zeitpunkt der Erstellung aktuellen Kompatibilitätsgrad und kann nicht geändert werden. Ein unterschiedlicher Kompatibilitätsgrad zwischen Master- und Benutzerdatenbank beeinträchtigt die Funktionalität oder die Leistung von Benutzerdatenbanken nicht.
> 

## <a name="about-compatibility-level-130"></a>Informationen zum Kompatibilitätsgrad 130
Führen Sie die folgende Transact-SQL-Anweisung aus, wenn Sie den aktuellen Kompatibilitätsgrad Ihrer Datenbank ermitteln möchten.

```
SELECT compatibility_level
    FROM sys.databases
    WHERE name = '<YOUR DATABASE_NAME>’;
```


Bevor diese Änderung auf Kompatibilitätsgrad 130 für **neu** erstellte Datenbanken durchgeführt wird, sehen wir uns anhand von einfachen Abfragebeispielen an, worum es bei dieser Änderung geht und wie alle Beteiligten davon profitieren können.

Die Verarbeitung von Abfragen in relationalen Datenbanken kann sehr komplex sein und setzt einen fundierten Hintergrund in Computerwissenschaft und Mathematik voraus, um die getroffene Entwurfsauswahl und das jeweilige Verhalten verstehen zu können. In diesem Dokument wurde der Inhalt absichtlich vereinfacht, um sicherzustellen, dass auch Leser mit weniger umfangreichem technischem Wissen nachvollziehen können, welche Auswirkungen die Änderung des Kompatibilitätsgrads hat und wie sich daraus Vorteile für Anwendungen ergeben können.

Wir sehen uns nun kurz an, was der Kompatibilitätsgrad 130 zu bieten hat.  Im Folgenden finden Sie eine kurze Zusammenfassung – weitere Details können Sie unter [ALTER DATABASE-Kompatibilitätsgrad (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx) lesen:

* Der Insert-Vorgang einer Insert-select-Anweisung kann mehrere Threads umfassen oder über einen parallelen Plan verfügen. Bisher konnte für diesen Vorgang nur ein Thread verwendet werden.
* Abfragen für speicheroptimierte Tabellen und Tabellenvariablen können jetzt über parallele Pläne verfügen, während für diesen Vorgang bisher ebenfalls nur ein Thread verwendet werden konnte.
* Statistiken für speicheroptimierte Tabellen können jetzt durchsucht werden (per Stichprobe), und sie werden automatisch aktualisiert. Weitere Informationen finden Sie unter [Neuigkeiten (Datenbankmodul)](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) .
* Batchmodus/Zeilenmodus-Änderungen mit Columnstore-Indizes
  * Sortierungen für eine Tabelle mit Columnstore-Index werden jetzt im Batchmodus durchgeführt.
  * Für Windowing-Aggregate wird jetzt der Batchmodus verwendet, z.B. T-SQL-LAG/LEAD-Anweisungen.
  * Abfragen in Columnstore-Tabellen mit mehreren einzelnen Klauseln nutzen ebenfalls den Batchmodus.
  * Auch für Abfragen, die unter DOP=1 oder mit einem seriellen Plan ausgeführt werden, wird der Batchmodus verwendet.
* Mit Kompatibilitätsgrad 120 werden Verbesserungen bei der Kardinalitätsschätzung eingeführt. Benutzer mit einem niedrigeren Kompatibilitätsgrad (100 oder 110) erhalten mit der Umstellung auf Kompatibilitätsgrad 130 aber auch diese Verbesserungen. Dies kann sich positiv auf die Abfrageleistung der Anwendungen auswirken.

## <a name="practicing-compatibility-level-130"></a>Übungen zum Kompatibilitätsgrad 130
Zuerst nutzen wir einige Tabellen, Indizes und Zufallsdaten, die erstellt wurden, um die Verwendung von einigen dieser neuen Funktionen zu üben. Die T-SQL-Skriptbeispiele können unter SQL Server 2016 oder unter Azure SQL-Datenbank ausgeführt werden. Stellen Sie beim Erstellen einer Azure SQL-Datenbank-Instanz aber sicher, dass Sie mindestens eine P2-Datenbank auswählen. Sie benötigen mindestens zwei Prozessorkerne, um das Multithreading zu ermöglichen und von diesen Funktionen zu profitieren.

```
-- Create a Premium P2 Database in Azure SQL Database

CREATE DATABASE MyTestDB
    (EDITION=’Premium’, SERVICE_OBJECTIVE=’P2′);
GO

-- Create 2 tables with a column store index on
-- the second one (only available on Premium databases)

CREATE TABLE T_source
    (Color varchar(10), c1 bigint, c2 bigint);

CREATE TABLE T_target
    (c1 bigint, c2 bigint);

CREATE CLUSTERED COLUMNSTORE INDEX CCI ON T_target;
GO

-- Insert few rows.

INSERT T_source VALUES
    (‘Blue’, RAND() * 100000, RAND() * 100000),
    (‘Yellow’, RAND() * 100000, RAND() * 100000),
    (‘Red’, RAND() * 100000, RAND() * 100000),
    (‘Green’, RAND() * 100000, RAND() * 100000),
    (‘Black’, RAND() * 100000, RAND() * 100000);

GO 200

INSERT T_source SELECT * FROM T_source;

GO 10
```


Als Nächstes sehen wir uns einige Funktionen der Abfrageverarbeitung an, die mit Kompatibilitätsgrad 130 verbunden sind.

## <a name="parallel-insert"></a>Paralleler INSERT-Vorgang
Beim Ausführen der folgenden T-SQL-Anweisungen wird der INSERT-Vorgang unter Kompatibilitätsgrad 120 und 130 ausgeführt. Das bedeutet, dass die Ausführung des INSERT-Vorgangs in einem Singlethread-Modell (120) und einem Multithread-Modell (130) erfolgt.

```
-- Parallel INSERT … SELECT … in heap or CCI
-- is available under 130 only

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO 

-- The INSERT part is in serial

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130
GO

-- The INSERT part is in parallel

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

SET STATISTICS XML OFF;
```


Wenn Sie den tatsächlichen Abfrageplan anfordern, können Sie ermitteln, welche Funktion für die Kardinalitätsschätzung verwendet wird, indem Sie sich die grafische Darstellung oder den XML-Inhalt ansehen. Wenn die Pläne in Abbildung 1 nebeneinander dargestellt werden, ist klar erkennbar, dass die Ausführung des ColumnStore-Vorgangs INSERT von seriell (120) zu parallel (130) wechselt. Beachten Sie auch, dass für die Änderung des Iteratorsymbols für den Plan von 130 zwei parallele Pfeile angezeigt werden. So wird verdeutlicht, dass die Iteratorausführung jetzt parallel erfolgt. Wenn Sie umfangreiche INSERT-Vorgänge durchführen müssen, wird für die parallele Ausführung eine bessere Leistung erzielt (in Abhängigkeit der Anzahl von Kernen, die für die Datenbank verfügbar sind). Die Ausführung kann je nach Situation um das bis zu Hundertfache beschleunigt werden!

*Abbildung 1: INSERT-Vorgang ändert sich von seriell in parallel mit Kompatibilitätsgrad 130*

![Abbildung 1](./media/sql-database-compatibility-level-query-performance-130/figure-1.jpg)

## <a name="serial-batch-mode"></a>Serieller Batchmodus
Auf ähnliche Weise ermöglicht die Umstellung auf Kompatibilitätsgrad 130 beim Verarbeiten von Zeilen mit Daten die Verarbeitung im Batchmodus. Erstens sind Vorgänge im Batchmodus nur verfügbar, wenn Sie über einen Columnstore-Index verfügen. Zweitens umfasst ein Batch normalerweise etwa&900; Zeilen, und es werden eine für CPUs mit mehreren Kernen optimierte Codelogik und ein höherer Speicherdurchsatz verwendet. Außerdem werden die komprimierten Daten des Columnstore nach Möglichkeit direkt genutzt. Unter diesen Bedingungen kann SQL Server 2016 etwa 900 Zeilen auf einmal verarbeiten, anstatt nur jeweils eine Zeile. Dies führt dazu, dass die Gesamtkosten des Vorgangs auf den gesamten Batch verteilt und die Gesamtkosten pro Zeile reduziert werden. Diese gleichzeitigen Vorgänge in Kombination mit der ColumnStore-Komprimierung sorgen für eine Reduzierung der Latenz eines SELECT-Batchmodusvorgangs. Weitere Informationen zu Columnstore und zum Batchmodus finden Sie unter [Beschreibung von Columnstore-Indizes](https://msdn.microsoft.com/library/gg492088.aspx).

```
-- Serial batch mode execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT (C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO 

-- The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Beim Betrachten der Abfragepläne nebeneinander in Abbildung 2 sehen wir, dass sich der Verarbeitungsmodus mit dem Kompatibilitätsgrad geändert hat. Beim Ausführen der Abfragen bei beiden Kompatibilitätsgraden zusammen sehen wir dann, dass der Großteil der Verarbeitungszeit für den Zeilenmodus anfällt (86%) – im Vergleich zu dem Batchmodus (14%), in dem zwei Batches verarbeitet wurden. Je größer das Dataset, desto größer der Vorteil.

*Abbildung 2: SELECT-Vorgang ändert sich vom seriellen Modus in den Batchmodus mit Kompatibilitätsgrad 130*

![Abbildung 2](./media/sql-database-compatibility-level-query-performance-130/figure-2.jpg)

## <a name="batch-mode-on-sort-execution"></a>Batchmodus bei der Ausführung einer Sortierung
Ähnlich wie im vorherigen Beispiel – hier nun aber für einen Sortiervorgang – führt die Umstellung vom Zeilenmodus (Kompatibilitätsgrad 120) auf den Batchmodus (Kompatibilitätsgrad 130) aus denselben Gründen zu einer Verbesserung der Leistung des SORT-Vorgangs.

```
-- Batch mode on sort execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

-- The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


In der Darstellung nebeneinander in Abbildung 3 sehen wir, dass für den Sortiervorgang im Zeilenmodus 81% der Kosten anfallen, während für den Batchmodus nur 19% der Kosten anfallen (81% bzw. 56% für die eigentliche Sortierung).

*Abbildung 3: SORT-Vorgang ändert sich vom Zeilenmodus in den Batchmodus mit Kompatibilitätsgrad 130*

![Abbildung 3](./media/sql-database-compatibility-level-query-performance-130/figure-3.png)

Diese Beispiele enthalten natürlich nur Zeilen in einer Größenordnung von mehreren Zehntausend. Verglichen mit den Daten, die heutzutage unter den meisten SQL Server-Instanzen verfügbar sind, ist dies eine sehr geringe Menge. Wenn Sie diesen Fall für eine Größenordnung von Millionen von Zeilen hochrechnen, kann sich je nach Art der Workload eine tägliche Einsparung von mehreren Minuten ergeben.

## <a name="cardinality-estimation-ce-improvements"></a>Verbesserungen bei der Kardinalitätsschätzung
Die neue Funktion für die Kardinalitätsschätzung wurde mit SQL Server 2014 eingeführt, und diese Funktion wird für alle Datenbanken, die mit Kompatibilitätsgrad 120 oder höher ausgeführt werden, genutzt. Im Wesentlichen ist die Kardinalitätsschätzung die Logik, mit der bestimmt wird, wie SQL Server eine Abfrage basierend auf den geschätzten Kosten ausführt. Die Schätzung wird anhand der Eingabe aus statistischen Daten berechnet, die den an der Abfrage beteiligten Objekten zugeordnet sind. Bei Funktionen für die Kardinalitätsschätzung handelt es sich grob gesagt um Schätzungen der Zeilenanzahl mit Informationen zur Verteilung der Werte, Anzahl der einzelnen Werte und Duplikatanzahl in den Tabellen und Objekten, auf die in der Abfrage verwiesen wird. Wenn diese Schätzungen nicht stimmen, kann dies zu unnötigem Datenträger-E/A-Aufwand aufgrund von unzureichender Speichergewährung (TempDB-Überläufe) oder zu einer Auswahl der Ausführung eines seriellen Plans gegenüber der Ausführung eines parallelen Plans führen. Dies sind nur zwei Beispiele dafür, was passieren kann. Schlussfolgerung: Falsche Schätzungen können eine allgemeine Leistungsverschlechterung der Abfrageausführung nach sich ziehen. Bessere bzw. genauere Schätzungen führen dagegen zu einer besseren Ausführung von Abfragen.

Wie bereits erwähnt, sind Abfrageoptimierungen und Schätzungen komplexe Aufgaben. Falls Sie sich weiter über Abfragepläne und die Kardinalitätsschätzung informieren möchten, helfen Ihnen die ausführlicheren Informationen unter [Optimizing Your Query Plans with the SQL Server 2014 Cardinality Estimator](https://msdn.microsoft.com/library/dn673537.aspx) (Optimieren von Abfrageplänen mit der Kardinalitätsschätzung von SQL Server 2014) weiter.

## <a name="which-cardinality-estimation-do-you-currently-use"></a>Welche Kardinalitätsschätzung verwenden Sie derzeit?
Wir verwenden die folgenden Abfragebeispiele, um zu ermitteln, welche Kardinalitätsschätzung für Ihre Abfragen ausgeführt wird. Dieses erste Beispiel wird unter Kompatibilitätsgrad 110 ausgeführt, sodass davon auszugehen ist, dass die alten Funktionen für die Kardinalitätsschätzung verwendet werden.

```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 110;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


Klicken Sie nach Abschluss der Ausführung auf den XML-Link, und sehen Sie sich die Eigenschaften des ersten Iterators an. Achten Sie auf den Eigenschaftennamen CardinalityEstimationModelVersion, der derzeit auf 70 festgelegt ist. Das bedeutet nicht, dass der Kompatibilitätsgrad der Datenbank auf Version SQL Server 7.0 festgelegt ist (wie in den vorherigen T-SQL-Anweisungen zu sehen, ist er auf 110 festgelegt), sondern der Wert 70 steht für die alte Funktion für die Kardinalitätsschätzung, die seit SQL Server 7.0 verfügbar ist. Sie wurde bis zur Einführung von SQL Server 2014 (mit Kompatibilitätsgrad 120) nicht nennenswert überarbeitet.

*Abbildung 4: CardinalityEstimationModelVersion ist auf 70 festgelegt, wenn ein Kompatibilitätsgrad von 110 oder niedriger verwendet wird*

![Abbildung 4](./media/sql-database-compatibility-level-query-performance-130/figure-4.png)

Alternativ dazu können Sie den Kompatibilitätsgrad in 130 ändern und die Verwendung der neuen Funktion für die Kardinalitätsschätzung deaktivieren, indem Sie LEGACY_CARDINALITY_ESTIMATION mit [ALTER DATABASE SCOPED CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx) auf ON festlegen. Dies entspricht aus Sicht der Funktion genau der Verwendung von 110 für die Kardinalitätsschätzung mit dem aktuellen Kompatibilitätsgrad für die Abfrageverarbeitung. Wenn Sie so vorgehen, profitieren Sie von den neuen Funktionen für die Abfrageverarbeitung, die mit dem aktuellen Kompatibilitätsgrad bereitgestellt werden (Batchmodus). Falls erforderlich, sind die alten Funktionen für die Kardinalitätsschätzung aber immer noch vorhanden.

```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


Durch die Umstellung auf Kompatibilitätsgrad 120 oder 130 wird die neue Funktion für die Kardinalitätsschätzung ermöglicht. In diesem Fall wird der Standardwert für CardinalityEstimationModelVersion auf 120 oder 130 festgelegt.

```
-- New CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


*Abbildung 5: CardinalityEstimationModelVersion ist auf 130 festgelegt, wenn ein Kompatibilitätsgrad von 130 verwendet wird*

![Abbildung 5](./media/sql-database-compatibility-level-query-performance-130/figure-5.jpg)

## <a name="witnessing-the-cardinality-estimation-differences"></a>Unterschiede bei der Kardinalitätsschätzung
Wir führen jetzt eine etwas komplexere Abfrage aus, die über ein INNER JOIN-Element mit einer WHERE-Klausel mit einigen Prädikaten verfügt. Zuerst sehen wir uns aber die Schätzung der Zeilenanzahl der alten Funktion für die Kardinalitätsschätzung an.

```
-- Old CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Beim Ausführen dieser Abfrage werden 200.704 Zeilen zurückgegeben, während sich mit der alten Funktion für die Kardinalitätsschätzung 194.284 Zeilen ergeben. Wie bereits erwähnt, hängen diese Zeilenanzahlen natürlich auch davon ab, wie oft Sie die vorherigen Beispiele ausgeführt haben. Die Beispieltabellen werden bei jeder Ausführung immer neu gefüllt. Zudem haben die Prädikate in Ihrer Abfrage auch Einfluss auf die eigentliche Schätzung, nicht nur auf die Tabellenform, die Dateninhalte und die Korrelation der Daten untereinander.

*Abbildung 6: Schätzung der Zeilenanzahl lautet 194.284 (ca. 6.000 Zeilen Unterschied gegenüber den erwarteten 200.704 Zeilen)*

![Abbildung 6](./media/sql-database-compatibility-level-query-performance-130/figure-6.jpg)

Wir führen die gleiche Abfrage jetzt mit der neuen Funktion für die Kardinalitätsschätzung aus.

```
-- New CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Wir sehen jetzt, dass die Zeilenschätzung 202.877 lautet und somit höher und genauer als die alte Kardinalitätsschätzung ist.

*Abbildung 7: Geschätzte Zeilenanzahl beträgt nun 202.877, anstatt 194.284*

![Abbildung 7](./media/sql-database-compatibility-level-query-performance-130/figure-7.jpg)

In Wirklichkeit umfasst das Resultset 200.704 Zeilen. (Alles hängt aber davon ab, wie oft Sie die Abfragen der vorherigen Beispiele ausgeführt haben. Wichtiger ist aber der Hinweis, dass die tatsächlich zurückgegebenen Werte von einer Ausführung zur nächsten variieren können, weil für T-SQL die RAND()-Anweisung verwendet wird). In diesem Beispiel wird mit der neuen Kardinalitätsschätzung daher eine bessere Schätzung der Zeilenanzahl erzielt (202.877 liegt näher an 200.704 als 194.284). Wenn Sie für die Prädikate der WHERE-Klausel nun auch noch Gleichheit festlegen (z.B. anstelle von „>“), kann dies dazu führen, dass sich die Schätzungen der alten und der neuen Funktion für die Kardinalitätsschätzung noch stärker unterscheiden. Dies hängt davon ab, wie viele Übereinstimmungen erzielt werden.

In diesem Fall scheint der Unterschied von ca.&6;.000 Zeilen gegenüber der tatsächlichen Anzahl nicht sonderlich viele Daten zu betreffen. Wenn Sie dies aber auf Millionen von Zeilen in mehreren Tabellen und mit komplexeren Abfragen umrechnen, wird klar, dass die Schätzung unter Umständen einen Fehler in der Größenordnung von Millionen Zeilen aufweisen kann. Das Risiko der Entscheidung für einen falschen Ausführungsplan oder der Anforderung einer unzureichenden Speichergewährung mit nachfolgenden TempDB-Überläufen und höherem E/A-Aufwand ist also deutlich höher.

Wenn die Möglichkeit besteht, können Sie diesen Vergleich mit Ihren typischen Abfragen und Datasets durchführen und selbst ermitteln, inwieweit die alten oder neuen Schätzungen betroffen sind. Der Fehler der Schätzung kann entweder noch gravierender sein, oder der Wert kann näher an der tatsächlichen Anzahl von Zeilen liegen, die in den Resultsets zurückgegeben werden. Alles richtet sich nach der Form Ihrer Abfragen, den Merkmalen von Azure SQL-Datenbank, der Art und Größe Ihrer Datasets und den dazu verfügbaren Statistiken. Wenn Sie Ihre SQL-Datenbankinstanz gerade erstellt haben, muss der Abfrageoptimierer sein Wissen von Grund auf neu aufbauen und kann keine Statistiken nutzen, die bei vorherigen Ausführungen der Abfragen erstellt wurden. Die Schätzungen sind also sehr kontextbezogen und gelten nahezu speziell für jeden Server und jede Anwendungssituation. Dies ist ein wichtiger Punkt, den Sie beachten sollten.

## <a name="some-considerations-to-take-into-account"></a>Wichtige Aspekte
Auch wenn die meisten Workloads in der Regel von Kompatibilitätsgrad 130 profitieren, haben Sie drei Optionen, bevor Sie den Kompatibilitätsgrad für Ihre Produktionsumgebung übernehmen:

1. Sie stellen auf Kompatibilitätsgrad 130 um und sehen sich an, welche Leistung sich ergibt. Falls es zu Regressionen kommt, legen Sie den Kompatibilitätsgrad wieder auf die ursprüngliche Einstellung fest oder behalten Sie die Einstellung 130 bei und setzen nur die Kardinalitätsschätzung auf den Legacymodus zurück. (Wie vorher beschrieben, kann dies bereits die Lösung des Problems bewirken).
2. Sie testen Ihre vorhandenen Anwendungen unter einer ähnlichen Produktionslast, führen eine Optimierung durch und überprüfen die Leistung, bevor auf die Produktion umgestellt wird. Bei Problemen können Sie (wie vorher auch) immer zum ursprünglichen Kompatibilitätsgrad zurückkehren oder die Kardinalitätsschätzung auf den Legacymodus zurücksetzen.
3. Die dritte und neueste Möglichkeit zur Lösung dieses Problems ist die Verwendung des Abfragespeichers. Dies ist unsere heutige Empfehlung! Wenn es um die Unterstützung der Analyse Ihrer Abfragen unter Kompatibilitätsgrad 120 oder niedriger im Gegensatz zu Kompatibilitätsgrad 130 geht, können wir Ihnen die Verwendung des Abfragespeichers wärmstens empfehlen. Der Abfragespeicher ist mit der aktuellen Version von Azure SQL-Datenbank V12 erhältlich und soll Ihnen als Hilfe bei der Problembehandlung der Abfrageleistung dienen. Sie können sich den Abfragespeicher wie einen Flugdatenschreiber für Ihre Datenbank vorstellen, der ausführliche Verlaufsinformationen zu allen Abfragen sammelt und darstellt. Auf diese Weise wird die Leistungsforensik stark vereinfacht, indem die zum Diagnostizieren und Lösen von Problemen benötigte Zeit reduziert wird. Weitere Informationen finden Sie unter [Query Store: A flight data recorder for your database](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/)(Abfragespeicher: Ein Flugdatenschreiber für Ihre Datenbank).

Beachten Sie die folgenden allgemeinen Hinweise, wenn Sie bereits eine Gruppe von Datenbanken mit Kompatibilitätsgrad 120 oder niedriger ausführen und einige davon auf 130 umstellen möchten, oder wenn für Ihre Workload automatisch neue Datenbanken bereitgestellt werden, die standardmäßig auf 130 festgelegt sind:

* Aktivieren Sie den Abfragespeicher, bevor Sie die Umstellung auf den neuen Kompatibilitätsgrad in der Produktion durchführen. Weitere Informationen finden Sie unter [Change the Database Compatibility Mode and Use the Query Store](https://msdn.microsoft.com/library/bb895281.aspx) (Ändern des Datenbank-Kompatibilitätsmodus und Verwenden des Abfragespeichers).
* Testen Sie als Nächstes alle kritischen Workloads, indem Sie repräsentative Daten und Abfragen einer produktionsähnlichen Umgebung verwenden, und vergleichen Sie die erzielte Leistung mit den gemeldeten Werten des Abfragespeichers. Wenn Regressionen auftreten, können Sie die betroffenen Abfragen mit dem Abfragespeicher identifizieren und die Planerzwingungsoption des Abfragespeichers („Plan Pinning“) verwenden. In diesem Fall behalten Sie Kompatibilitätsgrad 130 definitiv bei und verwenden den vorherigen Abfrageplan, der vom Abfragespeicher vorgeschlagen wird.
* Wenn Sie neue Features und Funktionen von Azure SQL-Datenbank (mit Ausführung von SQL Server 2016) verwenden möchten, die mit Kompatibilitätsgrad 130 verbundenen Änderungen aber nicht unproblematisch sind, können Sie als letztes Mittel den Kompatibilitätsgrad auf den für Ihre Workload geeigneten Grad zurücksetzen, indem Sie eine ALTER DATABASE-Anweisung verwenden. Machen Sie sich aber zuerst bewusst, dass die Planerzwingungsoption des Abfragespeichers die beste Möglichkeit für Sie darstellt. Wenn Sie 130 nicht verwenden, bedeutet dies quasi, dass der Funktionsgrad einer älteren SQL Server-Version beibehalten wird.
* Wenn Sie über mehrinstanzenfähige Anwendungen mit mehreren Datenbanken verfügen, kann es erforderlich sein, die Bereitstellungslogik der Datenbanken zu aktualisieren. So sorgen Sie für einen einheitlichen Kompatibilitätsgrad über alle Datenbanken hinweg, also alte und neu bereitgestellte Datenbanken. Die Workloadleistung Ihrer Anwendung kann dadurch beeinträchtigt werden, dass einige Datenbanken mit unterschiedlichen Kompatibilitätsgraden ausgeführt werden. Aus diesem Grund ist ggf. ein einheitlicher Kompatibilitätsgrad über alle Datenbanken hinweg erforderlich, damit Ihre Kunden immer die gleiche Leistung erhalten. Es handelt sich hierbei nicht um eine zwingende Vorgabe. Es hängt jeweils davon ab, wie sich der Kompatibilitätsgrad auf Ihre Anwendung auswirkt.
* Wie auch bei der Änderung des Kompatibilitätsgrads wird empfohlen, die Produktionsworkload unter den neuen Bedingungen zu testen. So wird ermittelt, ob die Anwendung von der verbesserten Kardinalitätsschätzung profitiert.

## <a name="conclusion"></a>Zusammenfassung
Durch die Verwendung von Azure SQL-Datenbank mit dem Ziel, in den Genuss aller SQL Server 2016-Verbesserungen zu kommen, kann die Leistung der Abfrageausführungen eindeutig erhöht werden. Natürlich sollte wie bei jedem neuen Feature eine richtige Auswertung durchgeführt werden, um die genauen Bedingungen zu ermitteln, unter denen die Datenbankworkload am besten abgearbeitet werden kann. Die Erfahrung hat gezeigt, dass die meisten Workloads unter Kompatibilitätsgrad 130 zumindest transparent ausgeführt werden, während neue Funktionen der Abfrageverarbeitung und eine neue Kardinalitätsschätzung genutzt werden. Realistischerweise gibt es immer ein paar Ausnahmen, sodass stets sorgfältig geprüft werden sollte, wie stark Sie von diesen Verbesserungen profitieren können. Auch hierbei stellt der Abfragespeicher eine große Hilfe dar.

## <a name="references"></a>Referenzen
* [Neuigkeiten (Datenbankmodul)](https://msdn.microsoft.com/library/bb510411.aspx#InMemory)
* [Blog: Query Store: A flight data recorder for your database](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/) (Blog: Abfragespeicher: Ein Flugdatenschreiber für Ihre Datenbank) von Borko Novakovic, 8. Juni 2016
* [ALTER DATABASE-Kompatibilitätsgrad (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx)
* [ALTER DATABASE SCOPED CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx)
* [Compatibility Level 130 for Azure SQL Database V12](https://azure.microsoft.com/updates/compatibility-level-130-for-azure-sql-database-v12/)
* [Optimizing Your Query Plans with the SQL Server 2014 Cardinality Estimator](https://msdn.microsoft.com/library/dn673537.aspx)
* [Beschreibung von Columnstore-Indizes](https://msdn.microsoft.com/library/gg492088.aspx)
* [Blog: Improved Query Performance with Compatibility Level 130 in Azure SQL Database](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/) (Blog: Verbesserte Abfrageleistung mit Kompatibilitätsgrad 130 in Azure SQL-Datenbank) von Alain Lissoir, 6. Mai 2016


