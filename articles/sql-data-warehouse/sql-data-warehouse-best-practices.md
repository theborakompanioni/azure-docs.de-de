<properties
   pageTitle="Bewährte Methoden für Azure SQL Data Warehouse | Microsoft Azure"
   description="Enthält Empfehlungen und bewährte Methoden, die Sie kennen sollten, wenn Sie Lösungen für Azure SQL Data Warehouse entwickeln. Sie tragen dazu bei, dass Sie erfolgreich sind."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/04/2016"
   ms.author="sonyama;barbkess"/>

# Bewährte Methoden für Azure SQL Data Warehouse

Dieser Artikel enthält eine Sammlung vieler bewährter Methoden, mit denen Sie die optimale Leistung mit Ihrem Azure SQL Data Warehouse erzielen. Einige Konzepte dieses Artikels sind einfach und leicht zu erklären, während andere Konzepte etwas komplizierter sind, sodass in diesem Artikel nicht ausführlicher darauf eingegangen wird. Dieser Artikel soll Ihnen als einfache Anleitung dienen und Sie auf wichtige Punkte aufmerksam machen, die Sie beim Erstellen des Data Warehouse beachten sollten. In jedem Abschnitt wird ein Konzept vorgestellt, und es sind Links zu Artikeln mit ausführlicheren Informationen angegeben, in denen näher auf ein Konzept eingegangen wird.

Lassen Sie sich von diesem Artikel nicht entmutigen, wenn Sie sich gerade erst mit Azure SQL Data Warehouse vertraut machen. Die Reihenfolge der Themen entspricht größtenteils auch der Reihenfolge ihrer Wichtigkeit. Wenn Sie sich zu Beginn auf die ersten Konzepte konzentrieren, sind Sie auf dem richtigen Weg. Nachdem Sie Erfahrung mit der Nutzung von SQL Data Warehouse gesammelt haben, machen Sie sich mit einigen weiteren Konzepte vertraut. Es wird nicht lange dauern, bis alles für Sie Sinn ergibt.

## Reduzieren von Kosten durch das Pausieren und Skalieren

Ein wichtiges Feature von SQL Data Warehouse ist die Möglichkeit, die Anwendung zu pausieren, wenn Sie sie nicht verwenden. Es werden dann keine Computeressourcen berechnet. Ein weiteres wichtiges Feature ist die Möglichkeit zum Skalieren der Ressourcen. Das Pausieren und Skalieren kann über das Azure-Portal oder mit PowerShell-Befehlen durchgeführt werden. Machen Sie sich mit diesen Features vertraut, da sie erheblich zur Kostenreduzierung für Ihr Data Warehouse beitragen können, wenn Sie es nicht verwenden. Wenn das Data Warehouse immer verfügbar sein soll, können Sie die Skalierung auf die kleinste Größe (DW100) in Betracht ziehen, anstatt zu pausieren.

Weitere Informationen finden Sie unter [Anhalten von Computeressourcen][], [Fortsetzen von Computeressourcen][] und [Skalieren von Computeressourcen][].


## Beseitigen von Transaktionen vor dem Pausieren oder Skalieren 

Beim Anhalten oder Skalieren Ihres SQL Data Warehouse werden Ihre Abfragen hinter den Kulissen abgebrochen, wenn Sie die Anforderung zum Anhalten oder Skalieren initiieren. Das Abbrechen einer einfachen SELECT-Abfrage ist ein schneller Vorgang und hat fast keinerlei Auswirkung auf den Zeitraum, der für das Pausieren oder Skalieren Ihrer Instanz anfällt. Dagegen können Transaktionsabfragen, bei denen die Daten oder die Struktur der Daten geändert wird, unter Umständen nicht so schnell beendet werden. **Transaktionsabfragen müssen laut Definition entweder vollständig abgeschlossen sein, oder es muss ein Rollback der Änderungen durchgeführt werden.** Ein Rollback der Schritte, die von einer Transaktionsabfrage ausgeführt wurden, kann genauso lange oder sogar länger als die ursprüngliche Änderung dauern, die mit der Abfrage durchgeführt werden sollte. Wenn Sie beispielsweise eine Abfrage abbrechen, mit der Zeilen gelöscht werden, und die Abfrage bereits eine Stunde lang ausgeführt wurde, kann es eine Stunde dauern, bis die gelöschten Zeilen wieder eingefügt wurden. Wenn Sie das Pausieren oder Skalieren bei aktiven Transaktionen ausführen, kann das Pausieren oder Skalieren lange dauern, weil erst gewartet werden muss, bis das Rollback abgeschlossen ist.

Siehe auch [Grundlagen von Transaktionen][], [Optimieren von Transaktionen][].

## Verwalten von Statistiken

Im Gegensatz zu SQL Server, wobei Statistiken zu Spalten, automatisch erkannt und erstellt oder aktualisiert werden, ist für SQL Data Warehouse eine manuelle Verwaltung der Statistiken erforderlich. Dies soll in Zukunft geändert werden, aber vorerst sollten Sie Ihre Statistiken selbst verwalten, um sicherzustellen, dass die SQL Data Warehouse-Pläne optimiert sind. Die vom Optimierer erstellten Pläne sind nur so gut wie die verfügbaren Statistiken. **Das Erstellen von erfassten Statistiken für jede Spalte ist eine einfache Möglichkeit zum Einsteigen in das Thema Statistiken.** Genauso wichtig ist es, Statistiken zu aktualisieren, da für Ihre Daten erhebliche Änderungen durchgeführt werden. Ein konservativer Ansatz ist das Aktualisieren von Statistiken jeden Tag oder nach jedem Ladevorgang. Es gibt immer Vor- und Nachteile, was die Leistung einerseits und die Kosten für die Erstellung und Aktualisierung von Statistiken andererseits betrifft. Falls die Verwaltung aller Statistiken zu lange dauert, können Sie versuchen, selektiver auszuwählen, welche Spalten über Statistiken verfügen sollen oder häufig aktualisiert werden müssen. Beispielsweise können Sie Datumsspalten aktualisieren, wenn täglich neue Werte hinzugefügt werden. **Die größten Vorteile ergeben sich, wenn Sie Statistiken zu Spalten in Verknüpfungen, in der WHERE-Klausel verwendete Spalten und Spalten in GROUP BY nutzen.**

Siehe auch [Verwalten von Tabellenstatistiken][], [CREATE STATISTICS][], [UPDATE STATISTICS][].

## Gruppieren von INSERT-Anweisungen in Batches

Ein einmaliger Ladevorgang für eine kleine Tabelle mit einer INSERT-Anweisung oder sogar das regelmäßige erneute Laden eines Suchvorgang kann für Ihre Zwecke eine gute Leistung bieten, wenn Sie eine Anweisung wie `INSERT INTO MyLookup VALUES (1, 'Type 1')` verwenden. Falls Sie im Laufe eines Tages aber Tausende oder Millionen von Zeilen laden müssen, werden Sie merken, dass einzelne INSERTS nicht mehr ausreichen. Entwickeln Sie Ihre Prozesse stattdessen so, dass die Daten in eine Datei geschrieben werden und diese Datei von einem anderen Prozess regelmäßig geladen wird.

Siehe auch [INSERT][].
 
## Verwenden von PolyBase zum schnellen Laden und Exportieren von Daten

SQL Data Warehouse unterstützt das Laden und Exportieren von Daten mit mehreren Tools, z.B. Azure Data Factory, PolyBase und BCP. Für kleinere Datenmengen, bei denen die Leistung nicht entscheidend ist, können auch beliebige andere Tools für Ihre Zwecke ausreichen. PolyBase ist aber die beste Wahl, wenn Sie große Datenvolumen laden oder exportieren oder eine schnelle Leistung benötigt wird. PolyBase ist dafür ausgelegt, die MPP-Architektur (Massively Parallel Processing) von SQL Data Warehouse zu nutzen, sodass größere Datenmengen schneller als mit anderen Tools geladen und exportiert werden können. PolyBase-Ladevorgänge können per CTAS oder INSERT INTO ausgeführt werden. **Durch die Verwendung von CTAS wird die Transaktionsprotokollierung reduziert, und es handelt sich um den schnellsten Weg zum Laden Ihrer Daten.** Azure Data Factory unterstützt auch PolyBase-Ladevorgänge. PolyBase unterstützt eine Vielzahl von Dateiformaten, z.B. Gzip-Dateien. **Um den Durchsatz zu erhöhen, sollten Sie die Dateien beim Verwenden von Gzip-Textdateien in 60 oder mehr Dateien aufteilen, um die Parallelität des Ladevorgangs zu erhöhen.** Erwägen Sie das gleichzeitige Laden von Daten, um einen besseren Gesamtdurchsatz zu erzielen.

Siehe auch [Laden von Daten][], [Anleitung für die Verwendung von PolyBase][], [Azure SQL Data Warehouse loading patterns and strategies][] (Azure SQL Data Warehouse – Lademuster und -strategien), [Laden von Daten mit Azure Data Factory][], [Verschieben von Daten mit Azure Data Factory][], [CREATE EXTERNAL FILE FORMAT][], [Create Table As Select (CTAS)][].

## Laden und Abfragen von externen Tabellen

Polybase, auch bekannt als externe Tabellen, kann zwar die schnellste Möglichkeit zum Laden von Daten sein, ist aber nicht optimal für Abfragen. In SQL Data Warehouse unterstützen Polybase-Tabellen derzeit nur Azure-Blobdateien. Diese Dateien werden nicht durch Computeressourcen gestützt. Daher kann SQL Data Warehouse diese Arbeit nicht auslagern und muss darum die gesamte Datei in „tempdb“ laden, um die Daten zu lesen. Wenn mehrere auf diese Daten gerichtete Abfragen vorliegen, sollten Sie also besser diese Daten einmal laden, und die Abfragen sollten in der lokalen Tabelle durchgeführt werden.

Siehe auch [Anleitung für die Verwendung von PolyBase in SQL Data Warehouse][].


## Verwenden der Hashverteilung für große Tabellen

Für Tabellen wird standardmäßig die Roundrobin-Verteilung eingesetzt. Dies erleichtert Benutzern die ersten Schritte beim Erstellen von Tabellen, ohne entscheiden zu müssen, wie die Tabellen verteilt werden sollen. Roundrobin-Tabellen ermöglichen für einige Workloads eine ausreichende Leistung, aber in den meisten Fällen führt die Auswahl einer Verteilungsspalte zu einer besseren Leistung. Das häufigste Beispiel für den Fall, in dem eine von einer Spalte verteilte Tabelle eine deutlich höhere Leistung als eine Roundrobin-Tabelle aufweist, ist die Verknüpfung von zwei großen Faktentabellen. Wenn Sie beispielsweise eine Tabelle mit Bestellungen verwenden, die nach „order\_id“ verteilt wird, und eine Tabelle mit Transaktionen, die ebenfalls nach „order\_id“ verteilt wird, gilt Folgendes: Wenn Sie die Tabelle mit den Bestellungen per „order\_id“ mit der Tabelle mit den Transaktionen verknüpfen, wird diese Abfrage zu einer Pass-Through-Abfrage. Dies bedeutet, dass wir Datenverschiebungsvorgänge beseitigen. Weniger Schritte führen zu einer schnelleren Abfrage. Außerdem führen weniger Datenverschiebungen zu schnelleren Abfragen. Diese Erklärung geht nicht in die Tiefe. Achten Sie beim Laden einer verteilten Tabelle darauf, dass Ihre eingehenden Daten nicht nach dem Verteilungsschlüssel sortiert sind, da dies mit einer Verlangsamung Ihrer Ladevorgänge verbunden ist. Unter den unten angegebenen Links finden Sie viele weitere Details dazu, wie die Auswahl einer Verteilungsspalte zu einer Leistungssteigerung führen kann und wie Sie eine verteilte Tabelle in der WITH-Klausel der CREATE TABLE-Anweisung definieren.

Siehe auch [Übersicht über Tabellen][], [Tabellenverteilung][], [Selecting table distribution][] (Auswählen der Tabellenverteilung), [CREATE TABLE][], [CREATE TABLE AS SELECT][].

## Vermeiden von übermäßiger Partitionierung

Das Partitionieren von Daten kann für die Verwaltung von Daten durch Partitionswechsel oder die Optimierung von Scans durch eine Partitionsbeseitigung zwar sehr effektiv sein, aber eine zu hohe Zahl von Partitionen kann Ihre Abfragen verlangsamen. Häufig funktioniert eine Partitionierungsstrategie mit zu hoher Granularität, die für SQL Server geeignet ist, für SQL Data Warehouse nicht gut. Die Verwendung von zu vielen Partitionen kann auch die Effektivität von gruppierten Columnstore-Indizes reduzieren, wenn jede Partition weniger als 1 Million Zeilen enthält. Beachten Sie, dass SQL Data Warehouse Ihre Daten im Hintergrund in 60 Datenbanken partitioniert. Wenn Sie eine Tabelle mit 100 Partitionen erstellen, ergibt dies also insgesamt 6.000 Partitionen. Jede Workload ist anders, sodass es ratsam ist, mit der Partitionierung zu experimentieren. So können Sie ermitteln, was für Ihre Workload am besten funktioniert. Erwägen Sie die Verwendung einer geringeren Granularität als für SQL Server. Versuchen Sie es beispielsweise anstelle von täglichen Partitionen mit wöchentlichen oder monatlichen Partitionen.

Siehe auch [Tabellenpartitionierung][].

## Verringern von Transaktionsgrößen

INSERT-, UPDATE- und DELETE-Anweisungen werden in einer Transaktion ausgeführt, und wenn hierbei Fehler auftreten, muss ein Rollback durchgeführt werden. Es ist ratsam, die Transaktionsgrößen nach Möglichkeit zu verringern, um das Potenzial für einen langen Rollbackvorgang zu reduzieren. Hierfür können Sie INSERT-, UPDATE- und DELETE-Anweisungen in Teile unterteilen. Wenn Sie beispielsweise einen INSERT-Vorgang verwenden, der voraussichtlich eine Stunde dauert, können Sie den INSERT-Vorgang, falls möglich, in vier Teile unterteilen, die jeweils 15 Minuten dauern. Nutzen Sie spezielle Fälle mit minimaler Protokollierung, z.B. CTAS, TRUNCATE, DROP TABLE oder INSERT zum Leeren von Tabellen, um das Rollback-Risiko zu verringern. Eine weitere Möglichkeit zum Beseitigen von Rollbacks ist die Verwendung von Vorgängen vom Typ „Nur Metadaten“, z.B. ein Partitionswechsel für die Datenverwaltung. Anstatt z.B. eine DELETE-Anweisung zum Löschen aller Zeilen in einer Tabelle auszuführen, für die das Bestelldatum (order\_date) im Oktober 2001 liegt, können Sie Ihre Daten monatlich partitionieren und die Partition dann gegen Daten für eine leere Partition aus einer anderen Tabelle auswechseln (siehe Beispiele zu ALTER TABLE). Erwägen Sie für nicht partitionierte Tabellen die Verwendung eines CTAS-Vorgangs, um beizubehaltende Daten in eine Tabelle zu schreiben, anstatt DELETE zu verwenden. Wenn für einen CTAS-Vorgang die gleiche Zeit benötigt wird, ist dies der sicherere Vorgang, da er mit minimaler Transaktionsprotokollierung verbunden ist und bei Bedarf schnell abgebrochen werden kann.

Siehe auch [Grundlagen von Transaktionen][], [Optimieren von Transaktionen][], [Tabellenpartitionierung][], [TRUNCATE TABLE][], [ALTER TABLE][], [Create Table As Select (CTAS)][].

## Verwenden der kleinstmöglichen Spaltengröße

Beim Ihrer DDL verbessert sich die Abfrageleistung, wenn Sie den kleinsten Datentyp verwenden, der für Ihre Daten unterstützt wird. Dies ist besonders für CHAR- und VARCHAR-Spalten wichtig. Wenn der längste Wert in einer Spalte 25 Zeichen lang ist, sollten Sie die Spalte VARCHAR(25) definieren. Vermeiden Sie es, für alle Zeichenspalten eine hohe Standardlänge zu definieren. Definieren Sie darüber hinaus Spalten als VARCHAR, sofern dies ausreicht, anstatt NVARCHAR zu verwenden.

Siehe auch [Übersicht über Tabellen][], [Tabellendatentypen][], [CREATE TABLE][].

## Verwenden temporärer Heaptabellen für vorübergehende Daten

Wenn Sie Daten vorübergehend in SQL Data Warehouse anordnen, werden Sie wahrscheinlich merken, dass der Gesamtprozess durch die Nutzung einer Heaptabelle beschleunigt wird. Wenn Sie Daten nur laden, um sie vor dem Ausführen weiterer Transformationen bereitzustellen, ist das Laden der Tabelle in eine Heaptabelle deutlich schneller als das Laden der Daten in eine gruppierte Columnstore-Tabelle. Beim Laden von Daten in eine temporäre Tabelle wird der Ladevorgang außerdem viel schneller als beim Laden einer Tabelle in einen dauerhaften Speicher durchgeführt. Auf temporäre Tabellen mit dem Zeichen „#“ kann nur über die Sitzung zugegriffen werden, in der sie erstellt wurden, sodass sie unter Umständen nur in einer begrenzten Zahl von Fällen funktionieren. Heaptabellen werden in der WITH-Klausel von CREATE TABLE definiert. Denken Sie beim Verwenden einer temporären Tabelle daran, auch für die temporäre Tabelle Statistiken zu erstellen.

Siehe auch [Temporäre Tabellen][], [CREATE TABLE][], [CREATE TABLE AS SELECT][].

## Optimieren von gruppierten Columnstore-Tabellen

Gruppierte Columnstore-Indizes sind eine der effizientesten Möglichkeiten zum Speichern von Daten in Azure SQL Data Warehouse. Tabellen werden in SQL Data Warehouse standardmäßig als gruppierter Columnstore erstellt. Die Verwendung einer guten Segmentqualität ist wichtig, um für Abfragen in Columnstore-Tabellen die beste Leistung zu erzielen. Wenn Zeilen bei hohem Arbeitsspeicherdruck in Columnstore-Tabellen geschrieben werden, kann die Qualität von Columnstore-Segmenten leiden. Die Segmentqualität kann anhand der Anzahl an Zeilen in einer komprimierten Zeilengruppe gemessen werden. Eine Schritt-für-Schritt-Anleitung zur Erkennung und Verbesserung der Segmentqualität für gruppierte Columnstore-Tabellen finden Sie im Artikel [Indizieren von Tabellen in SQL Data Warehouse][] unter [Ursachen für eine schlechte Qualität des Columnstore-Index][]. Da qualitativ hochwertige Columnstore-Segmente wichtig sind, ist es eine gute Idee, Benutzer-IDs, die zur mittleren oder großen Ressourcenklasse gehören, zum Laden von Daten zu verwenden. Je weniger DWUs Sie verwenden, desto größer sollte die Ressourcenklasse sein, die Sie dem ladenden Benutzer zuweisen.

Da Columnstore-Tabellen Daten im Allgemeinen erst dann in ein komprimiertes Columnstore-Segment übertragen, wenn mehr als 1 Million Zeilen pro Tabelle vorhanden sind, und jede SQL Data Warehouse-Tabelle in 60 Tabellen partitioniert ist, gilt folgende Faustregel: Columnstore-Tabellen stellen für eine Abfrage nur dann einen Vorteil dar, wenn die Tabelle mehr als 60 Millionen Zeilen hat. Für Tabellen mit weniger als 60 Millionen Zeilen ist es unter Umständen nicht sinnvoll, einen Columnstore-Index zu nutzen. Es muss aber nicht unbedingt einen Nachteil bedeuten. Wenn Sie Ihre Daten partitionieren, können Sie auch darauf achten, dass jede Partition 1 Million Zeilen aufweisen muss, um von einem gruppierten Columnstore-Index zu profitieren. Falls eine Tabelle 100 Partitionen hat, muss sie mindestens 6 Milliarden Zeilen enthalten, um von einem gruppierten Columnstore zu profitieren (60 Verteilungen * 100 Partitionen * 1 Million Zeilen). Falls Ihre Tabelle für dieses Beispiel nicht 6 Milliarden Zeilen enthält, sollten Sie entweder die Anzahl der Partitionen reduzieren oder stattdessen eine Heaptabelle verwenden. Außerdem kann sich folgendes Experiment lohnen: Ermitteln Sie, ob eine bessere Leistung erzielt werden kann, indem eine Heaptabelle mit sekundären Indizes anstelle einer Columnstore-Tabelle verwendet wird. Columnstore-Tabellen unterstützen noch keine sekundären Indizes.

Beim Abfragen einer Columnstore-Tabelle werden die Abfragen schneller ausgeführt, wenn Sie nur die benötigten Spalten auswählen.

Siehe auch [Tabellenindizes][], [Beschreibung von Columnstore-Indizes][] und [Neuerstellen von Columnstore-Indizes][].

## Verwenden einer größeren Ressourcenklasse zum Verbessern der Abfrageleistung

SQL Data Warehouse nutzt Ressourcengruppen zum Zuordnen von Arbeitsspeicher zu Abfragen. Standardmäßig werden alle Benutzer der kleinen Ressourcenklasse zugewiesen, bei der pro Verteilung 100 MB Arbeitsspeicher gewährt werden. Da immer 60 Verteilungen vorhanden sind, und jede Verteilung mindestens 100MB erhält, beträgt die gesamte systemweite Arbeitsspeicherzuordnung 6.000MB bzw. knapp 6GB. Bestimmte Abfragen, z.B. große Verknüpfungen oder Ladevorgänge für gruppierte Columnstore-Tabellen, profitieren von größeren Arbeitsspeicherzuordnungen. Für einige Abfragen, z.B. reine Scans, ergibt sich kein Vorteil. Andererseits wirkt sich die Nutzung größerer Ressourcenklassen auf die Parallelität aus. Dies sollten Sie berücksichtigen, bevor Sie alle Benutzer in eine große Ressourcenklasse verschieben.
 
Siehe auch [Parallelitäts- und Workloadverwaltung][].

## Verwenden einer kleineren Ressourcenklasse zur Steigerung der Parallelität

Wenn Sie bemerken, dass Benutzerabfragen mit einer langen Verzögerung verbunden sind, werden Ihre Benutzer unter Umständen in größeren Ressourcenklassen ausgeführt und verbrauchen viele Parallelitätsslots, sodass für andere Abfragen eine Warteschlange entsteht. Führen Sie `SELECT * FROM sys.dm_pdw_waits` aus, um zu ermitteln, ob Zeilen zurückgegeben werden. So können Sie feststellen, ob Benutzeranfragen in eine Warteschlange eingereiht werden.

Siehe auch [Parallelitäts- und Workloadverwaltung][] und [sys.dm\_pdw\_waits][].

## Verwenden von DMVs zum Überwachen und Optimieren von Abfragen

SQL Data Warehouse verfügt über mehrere dynamische Verwaltungssichten (DMVs), die zum Überwachen der Abfrageausführung verwendet werden können. Unten im Artikel zur Überwachung werden Sie Schritt für Schritt durch das Anzeigen einer ausgeführten Abfrage geführt. Die Verwendung der Option LABEL für Abfragen kann hilfreich sein, um Abfragen für diese DMVs schnell finden zu können.

Siehe auch [Überwachen Ihrer Workload mit dynamischen Verwaltungssichten][], [LABEL][], [OPTION][], [sys.dm\_exec\_sessions][], [sys.dm\_pdw\_exec\_requests][], [sys.dm\_pdw\_request\_steps][], [sys.dm\_pdw\_sql\_requests][], [sys.dm\_pdw\_dms\_workers], [DBCC PDW\_SHOWEXECUTIONPLAN][] und [sys.dm\_pdw\_waits][].

## Weitere Ressourcen

Informationen zu häufigen Problemen und den entsprechenden Lösungen finden Sie im Artikel [Problembehandlung][].

Wenn Sie die gewünschten Informationen in diesem Artikel nicht gefunden haben, können Sie links auf dieser Seite die Option zum Suchen nach Dokumenten (Search for docs) verwenden, um alle Dokumente zu Azure SQL Data Warehouse zu durchsuchen. Das [MSDN-Forum für Azure SQL Data Warehouse][] ist ein Ort, an dem Sie anderen Benutzern und dem SQL Data Warehouse-Produktteam Fragen stellen können. Wir überwachen dieses Forum aktiv, um sicherzustellen, dass Ihre Frage entweder von einem anderen Benutzer oder einem Mitarbeiter beantwortet wird. Falls Sie Ihre Fragen lieber über Stack Overflow stellen möchten, können Sie auch das [Stack Overflow-Forum für Azure SQL Data Warehouse][] verwenden.

Verwenden Sie die Seite für [Azure SQL Data Warehouse-Feedback][], um Anfragen zu Features zu stellen. Indem Sie Ihre Anfragen hinzufügen oder für andere Anfragen stimmen, leisten Sie wertvolle Unterstützung beim Priorisieren von Funktionen.

<!--Image references-->

<!--Article references-->
[Create a support ticket]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Parallelitäts- und Workloadverwaltung]: ./sql-data-warehouse-develop-concurrency.md
[Create Table As Select (CTAS)]: ./sql-data-warehouse-develop-ctas.md
[Übersicht über Tabellen]: ./sql-data-warehouse-tables-overview.md
[Tabellendatentypen]: ./sql-data-warehouse-tables-data-types.md
[Tabellenverteilung]: ./sql-data-warehouse-tables-distribute.md
[Indizieren von Tabellen in SQL Data Warehouse]: ./sql-data-warehouse-tables-index.md
[Tabellenindizes]: ./sql-data-warehouse-tables-index.md
[Ursachen für eine schlechte Qualität des Columnstore-Index]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Neuerstellen von Columnstore-Indizes]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Tabellenpartitionierung]: ./sql-data-warehouse-tables-partition.md
[Verwalten von Tabellenstatistiken]: ./sql-data-warehouse-tables-statistics.md
[Temporäre Tabellen]: ./sql-data-warehouse-tables-temporary.md
[Anleitung für die Verwendung von PolyBase]: ./sql-data-warehouse-load-polybase-guide.md
[Anleitung für die Verwendung von PolyBase in SQL Data Warehouse]: ./sql-data-warehouse-load-polybase-guide.md
[Laden von Daten]: ./sql-data-warehouse-overview-load.md
[Verschieben von Daten mit Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[Laden von Daten mit Azure Data Factory]: ./sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Load data with PolyBase]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Überwachen Ihrer Workload mit dynamischen Verwaltungssichten]: ./sql-data-warehouse-manage-monitor.md
[Anhalten von Computeressourcen]: ./sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Fortsetzen von Computeressourcen]: ./sql-data-warehouse-manage-compute-overview.md#resume-compute-bk
[Skalieren von Computeressourcen]: ./sql-data-warehouse-manage-compute-overview.md#scale-performance-bk
[Grundlagen von Transaktionen]: ./sql-data-warehouse-develop-transactions.md
[Optimieren von Transaktionen]: ./sql-data-warehouse-develop-best-practices-transactions.md
[Problembehandlung]: ./sql-data-warehouse-troubleshoot.md
[LABEL]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->
[ALTER TABLE]: https://msdn.microsoft.com/library/ms190273.aspx
[CREATE EXTERNAL FILE FORMAT]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE STATISTICS]: https://msdn.microsoft.com/library/ms188038.aspx
[CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx
[CREATE TABLE AS SELECT]: https://msdn.microsoft.com/library/mt204041.aspx
[DBCC PDW\_SHOWEXECUTIONPLAN]: https://msdn.microsoft.com/library/mt204017.aspx
[INSERT]: https://msdn.microsoft.com/library/ms174335.aspx
[OPTION]: https://msdn.microsoft.com/library/ms190322.aspx
[TRUNCATE TABLE]: https://msdn.microsoft.com/library/ms177570.aspx
[UPDATE STATISTICS]: https://msdn.microsoft.com/library/ms187348.aspx
[sys.dm\_exec\_sessions]: https://msdn.microsoft.com/library/ms176013.aspx
[sys.dm\_pdw\_exec\_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[sys.dm\_pdw\_request\_steps]: https://msdn.microsoft.com/library/mt203913.aspx
[sys.dm\_pdw\_sql\_requests]: https://msdn.microsoft.com/library/mt203889.aspx
[sys.dm\_pdw\_dms\_workers]: https://msdn.microsoft.com/library/mt203878.aspx
[sys.dm\_pdw\_waits]: https://msdn.microsoft.com/library/mt203893.aspx
[Beschreibung von Columnstore-Indizes]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->
[Selecting table distribution]: https://blogs.msdn.microsoft.com/sqlcat/2015/08/11/choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/
[Azure SQL Data Warehouse-Feedback]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-Forum für Azure SQL Data Warehouse]: https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=AzureSQLDataWarehouse
[Stack Overflow-Forum für Azure SQL Data Warehouse]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Azure SQL Data Warehouse loading patterns and strategies]: https://blogs.msdn.microsoft.com/sqlcat/2016/02/06/azure-sql-data-warehouse-loading-patterns-and-strategies

<!---HONumber=AcomDC_0907_2016-->