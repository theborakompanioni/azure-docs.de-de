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
   ms.date="05/02/2016"
   ms.author="sonyama;barbkess"/>

# Bewährte Methoden für SQL Data Warehouse

Dieser Artikel enthält eine Sammlung vieler bewährter Methoden, mit denen Sie das optimale Preis-Leistungs-Verhältnis für Azure SQL Data Warehouse erzielen. Einige Konzepte dieses Artikels sind sehr einfach und leicht zu erklären, während andere Konzepte etwas komplizierter sind, sodass in diesem Artikel nicht ausführlicher darauf eingegangen wird. Dieser Artikel soll Ihnen als einfache Anleitung dienen und Sie auf wichtige Punkte aufmerksam machen, die Sie beim Erstellen des Data Warehouse beachten sollten. In jedem Abschnitt wird ein Konzept vorgestellt, und es sind Links zu Artikeln mit ausführlicheren Informationen angegeben, in denen näher auf ein Konzept eingegangen wird.

Lassen Sie sich von diesem Artikel nicht entmutigen, wenn Sie sich gerade erst mit Azure SQL Data Warehouse vertraut machen. Die Reihenfolge der Themen entspricht größtenteils auch der Reihenfolge ihrer Wichtigkeit. Wenn Sie sich zunächst auf die ersten drei konzentrieren, ist dies ein guter Anfang. Nachdem Sie Erfahrung mit der Nutzung von SQL Data Warehouse gesammelt haben, können Sie sich einige weitere Themen dieses Artikels ansehen. Es wird nicht lange dauern, bis alles für Sie Sinn ergibt. Am Anfang fühlen Sie sich vielleicht überfordert, aber im Laufe der Zeit und mit mehr Kenntnissen werden Ihnen die Themen nicht mehr schwierig erscheinen.

## Reduzieren von Kosten durch das Pausieren und Skalieren
Ein wichtiges Feature von SQL Data Warehouse ist die Möglichkeit, die Anwendung zu pausieren, wenn Sie sie nicht verwenden. Es werden dann keine Computeressourcen berechnet. Ein weiteres wichtiges Feature ist die Möglichkeit zum Skalieren der Ressourcen. Das Pausieren und Skalieren kann über das Azure-Portal oder mit PowerShell-Befehlen durchgeführt werden. Machen Sie sich mit diesen Features vertraut, da sie erheblich zur Kostenreduzierung für Ihr Data Warehouse beitragen können, wenn Sie es nicht verwenden. Wenn das Data Warehouse immer verfügbar sein soll, können Sie die Skalierung auf die kleinste Größe (DW100) in Betracht ziehen, anstatt zu pausieren.

Weitere Informationen finden Sie unter [Anhalten von Computeressourcen][], [Fortsetzen von Computeressourcen][] und [Skalieren von Computeressourcen][].


## Beseitigen von Transaktionen vor dem Pausieren oder Skalieren 
Wenn Sie SQL Data Warehouse pausieren oder skalieren, wird die Datenbankinstanz im Hintergrund beendet. Dies bedeutet, dass alle aktiven Abfragen abgebrochen werden. Das Abbrechen einer einfachen SELECT-Abfrage ist ein schneller Vorgang und hat fast keinerlei Auswirkung auf den Zeitraum, der für das Pausieren oder Skalieren Ihrer Instanz anfällt. Dagegen können Transaktionsabfragen, bei denen die Daten oder die Struktur der Daten geändert wird, unter Umständen nicht so schnell beendet werden. **Transaktionsabfragen müssen entweder vollständig abgeschlossen sein, oder es muss ein Rollback der Änderungen durchgeführt werden.** Ein Rollback der Schritte, die von einer Transaktionsabfrage ausgeführt wurden, kann genauso lange oder sogar länger als die ursprüngliche Änderung dauern, die mit der Abfrage durchgeführt werden sollte. Wenn Sie beispielsweise eine Abfrage abbrechen, mit der Zeilen gelöscht werden, und die Abfrage bereits eine Stunde lang ausgeführt wurde, kann es eine Stunde dauern, bis die gelöschten Zeilen wieder eingefügt wurden. Wenn Sie das Pausieren oder Skalieren bei aktiven Transaktionen ausführen, kann das Pausieren oder Skalieren lange dauern, weil erst gewartet werden muss, bis das Rollback abgeschlossen ist.

Weitere Informationen finden Sie unter [Transaktionen in SQL Data Warehouse][] und [Optimieren von Transaktionen für SQL Data Warehouse][].

## Verwalten von Statistiken
Im Gegensatz zu SQL Server, wobei Statistiken zu Spalten, die profitieren würden, automatisch erkannt und erstellt oder aktualisiert werden, ist für SQL Data Warehouse eine manuelle Verwaltung der Statistiken erforderlich. Dies soll in Zukunft geändert werden, aber vorerst sollten Sie Ihre Statistiken selbst verwalten, um sicherzustellen, dass die SQL Data Warehouse-Pläne optimiert sind. Die vom Optimierer erstellten Pläne sind nur so gut wie die verfügbaren Statistiken. **Das Erstellen von erfassten Statistiken für jede Spalte ist eine einfache Möglichkeit zum Einsteigen in das Thema Statistiken.** Genauso wichtig ist es, Statistiken zu aktualisieren, da für Ihre Daten erhebliche Änderungen durchgeführt werden. Ein konservativer Ansatz ist das Aktualisieren von Statistiken jeden Tag oder nach jedem Ladevorgang. Es gibt immer Vor- und Nachteile, was die Leistung einerseits und die Kosten für die Erstellung und Aktualisierung von Statistiken andererseits betrifft. Falls die Verwaltung aller Statistiken zu lange dauert, können Sie versuchen, selektiver auszuwählen, welche Spalten über Statistiken verfügen sollen oder häufig aktualisiert werden müssen. Beispielsweise können Sie Datumsspalten aktualisieren, wenn täglich neue Werte hinzugefügt werden. **Die größten Vorteile ergeben sich, wenn Sie Statistiken zu Spalten in Verknüpfungen, in der WHERE-Klausel verwendete Spalten und Spalten in GROUP BY nutzen.**

Weitere Informationen finden Sie unter [Verwalten von Statistiken in SQL Data Warehouse][], [CREATE STATISTICS (Transact-SQL)][] und [UPDATE STATISTICS (Transact-SQL)][].

## Gruppieren von INSERT-Anweisungen in Batches
Ein einmaliger Ladevorgang für eine kleine Tabelle mit einer INSERT-Anweisung oder sogar das regelmäßige erneute Laden eines Suchvorgang kann für Ihre Zwecke eine gute Leistung bieten, wenn Sie eine Anweisung wie `INSERT INTO MyLookup VALUES (1, 'Type 1')` verwenden. Falls Sie im Laufe eines Tages aber Tausende oder Millionen von Zeilen laden müssen, werden Sie merken, dass einzelne INSERTS nicht mehr ausreichen. Entwickeln Sie Ihre Prozesse stattdessen so, dass die Daten in eine Datei geschrieben werden und diese Datei von einem anderen Prozess regelmäßig geladen wird.

Weitere Informationen finden Sie unter [Insert (Transact-SQL)][].
 
## Verwenden von PolyBase zum schnellen Laden und Exportieren von Daten
SQL Data Warehouse unterstützt das Laden und Exportieren von Daten mit mehreren Tools, z.B. Azure Data Factory, PolyBase und BCP. Für kleinere Datenmengen, bei denen die Leistung nicht entscheidend ist, können auch beliebige andere Tools für Ihre Zwecke ausreichen. PolyBase ist aber die beste Wahl, wenn Sie große Datenvolumen laden oder exportieren oder eine schnelle Leistung benötigt wird. PolyBase ist dafür ausgelegt, die MPP-Architektur (Massively Parallel Processing) von SQL Data Warehouse zu nutzen, sodass größere Datenmengen schneller als mit anderen Tools geladen und exportiert werden können. PolyBase-Ladevorgänge können per CTAS oder INSERT INTO ausgeführt werden. **Durch die Verwendung von CTAS wird die Transaktionsprotokollierung reduziert, und es handelt sich um den schnellsten Weg zum Laden Ihrer Daten.** Azure Data Factory unterstützt auch PolyBase-Ladevorgänge. PolyBase unterstützt eine Vielzahl von Dateiformaten, z.B. Gzip-Dateien. **Um den Durchsatz zu erhöhen, sollten Sie die Dateien beim Verwenden von Gzip-Textdateien in 60 oder mehr Dateien aufteilen, um die Parallelität des Ladevorgangs zu erhöhen.** Erwägen Sie das gleichzeitige Laden von Daten, um einen besseren Gesamtdurchsatz zu erzielen.

Weitere Informationen finden Sie unter [Laden von Daten in SQL Data Warehouse][], [Anleitung für die Verwendung von PolyBase in SQL Data Warehouse][], [Azure SQL Data Warehouse loading patterns and strategies][] (Azure SQL Data Warehouse – Lademuster und -strategien), [Laden von Daten mit Azure Data Factory][], [Verschieben von Daten in und aus Azure SQL Data Warehouse mithilfe von Azure Data Factory][], [CREATE EXTERNAL FILE FORMAT (Transact-SQL)][] und [Create Table As Select (CTAS) in SQL Data Warehouse][].

## Verwenden der Hashverteilung für große Tabellen
Für Tabellen wird standardmäßig die Roundrobin-Verteilung eingesetzt. Dies erleichtert Benutzern die ersten Schritte beim Erstellen von Tabellen, ohne entscheiden zu müssen, wie die Tabellen verteilt werden sollen. Roundrobin-Tabellen ermöglichen für einige Workloads eine gute Leistung, aber häufig führt die Auswahl einer Verteilungsspalte zu einer besseren Leistung. Das häufigste Beispiel für den Fall, in dem eine von einer Spalte verteilte Tabelle eine deutlich höhere Leistung als eine Roundrobin-Tabelle aufweist, ist die Verknüpfung von zwei großen Faktentabellen. Wenn Sie beispielsweise eine Tabelle mit Bestellungen verwenden, die nach „order\_id“ verteilt wird, und eine Tabelle mit Transaktionen, die ebenfalls nach „order\_id“ verteilt wird, gilt Folgendes: Wenn Sie die Tabelle mit den Bestellungen per „order\_id“ mit der Tabelle mit den Transaktionen verknüpfen, wird diese Abfrage zu einer Pass-Through-Abfrage. Dies bedeutet, dass wir Datenverschiebungsvorgänge beseitigen. Weniger Schritte führen zu einer schnelleren Abfrage. Außerdem führen weniger Datenverschiebungen zu schnelleren Abfragen. Diese Erklärung geht nicht in die Tiefe. Achten Sie beim Laden einer verteilten Tabelle darauf, dass Ihre eingehenden Daten nicht nach dem Verteilungsschlüssel sortiert sind, da dies mit einer Verlangsamung Ihrer Ladevorgänge verbunden ist. Unter den unten angegebenen Links finden Sie viele weitere Details dazu, wie die Auswahl einer Verteilungsspalte zu einer Leistungssteigerung führen kann und wie Sie eine verteilte Tabelle in der WITH-Klausel der CREATE TABLE-Anweisung definieren.

Weitere Informationen finden Sie unter [Hashverteilung und deren Auswirkung auf die Abfrageleistung in SQL Data Warehouse][], [Choosing hash distributed table vs. round-robin distributed table][] (Auswählen einer Tabelle mit Hashverteilung gegenüber einer Tabelle mit Roundrobin-Verteilung), [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][] und [CREATE TABLE AS SELECT (Azure SQL Data Warehouse)][].

## Vermeiden von übermäßiger Partitionierung
Das Partitionieren von Daten kann für die Verwaltung von Daten durch Partitionswechsel oder die Optimierung von Scans durch eine Partitionsbeseitigung zwar sehr effektiv sein, aber eine zu hohe Zahl von Partitionen kann Ihre Abfragen verlangsamen. Häufig funktioniert eine Partitionierungsstrategie mit zu hoher Granularität, die für SQL Server geeignet ist, für SQL Data Warehouse nicht gut. Die Verwendung von zu vielen Partitionen kann auch die Effektivität von gruppierten Columnstore-Indizes reduzieren, wenn jede Partition weniger als 1 Million Zeilen enthält. Beachten Sie, dass SQL Data Warehouse Ihre Daten im Hintergrund in 60 Datenbanken partitioniert. Wenn Sie eine Tabelle mit 100 Partitionen erstellen, ergibt dies also insgesamt 6.000 Partitionen. Jede Workload ist anders, sodass es ratsam ist, mit der Partitionierung zu experimentieren. So können Sie ermitteln, was für Ihre Workload am besten funktioniert. Erwägen Sie die Verwendung einer geringeren Granularität als für SQL Server. Versuchen Sie es beispielsweise anstelle von täglichen Partitionen mit wöchentlichen oder monatlichen Partitionen.

Weitere Informationen finden Sie unter [Tabellenpartitionen in SQL Data Warehouse][].

## Verringern von Transaktionsgrößen
INSERT-, UPDATE- und DELETE-Anweisungen werden in einer Transaktion ausgeführt, und wenn hierbei Fehler auftreten, muss ein Rollback durchgeführt werden. Es ist ratsam, die Transaktionsgrößen nach Möglichkeit zu verringern, um das Potenzial für einen langen Rollbackvorgang zu reduzieren. Hierfür können Sie INSERT-, UPDATE- und DELETE-Anweisungen in Teile unterteilen. Wenn Sie beispielsweise einen INSERT-Vorgang verwenden, der voraussichtlich eine Stunde dauert, können Sie den INSERT-Vorgang, falls möglich, in vier Teile unterteilen, die jeweils 15 Minuten dauern. Nutzen Sie spezielle Fälle mit minimaler Protokollierung, z.B. CTAS, TRUNCATE, DROP TABLE oder INSERT zum Leeren von Tabellen, um das Rollback-Risiko zu verringern. Eine weitere Möglichkeit zum Beseitigen von Rollbacks ist die Verwendung von Vorgängen vom Typ „Nur Metadaten“, z.B. ein Partitionswechsel für die Datenverwaltung. Anstatt z.B. eine DELETE-Anweisung zum Löschen aller Zeilen in einer Tabelle auszuführen, für die das Bestelldatum (order\_date) im Oktober 2001 liegt, können Sie Ihre Daten monatlich partitionieren und die Partition dann gegen Daten für eine leere Partition aus einer anderen Tabelle auswechseln (siehe Beispiele zu ALTER TABLE). Erwägen Sie für nicht partitionierte Tabellen die Verwendung eines CTAS-Vorgangs, um beizubehaltende Daten in eine Tabelle zu schreiben, anstatt DELETE zu verwenden. Wenn für einen CTAS-Vorgang die gleiche Zeit benötigt wird, ist dies der sicherere Vorgang, da er mit minimaler Transaktionsprotokollierung verbunden ist und bei Bedarf schnell abgebrochen werden kann.

Weitere Informationen finden Sie unter [Transaktionen in SQL Data Warehouse][], [Optimieren von Transaktionen für SQL Data Warehouse][], [Tabellenpartitionen in SQL Data Warehouse][], [TRUNCATE TABLE (Transact-SQL)][], [ALTER TABLE (Transact-SQL)][] und [Create Table As Select (CTAS) in SQL Data Warehouse][].

## Verwenden der kleinstmöglichen Spaltengröße
Beim Ihrer DDL verbessert sich die Abfrageleistung, wenn Sie den kleinsten Datentyp verwenden, der für Ihre Daten unterstützt wird. Dies ist besonders für CHAR- und VARCHAR-Spalten wichtig. Wenn der längste Wert in einer Spalte 25 Zeichen lang ist, sollten Sie die Spalte VARCHAR(25) definieren. Vermeiden Sie es, für alle Zeichenspalten eine hohe Standardlänge zu definieren. Definieren Sie darüber hinaus Spalten als VARCHAR, sofern dies ausreicht, anstatt NVARCHAR zu verwenden.

Weitere Informationen finden Sie unter [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][].

## Verwenden temporärer Heaptabellen für vorübergehende Daten
Wenn Sie Daten vorübergehend in SQL Data Warehouse anordnen, werden Sie wahrscheinlich merken, dass der Gesamtprozess durch die Nutzung einer Heaptabelle beschleunigt wird. Wenn Sie Daten nur laden, um sie vor dem Ausführen weiterer Transformationen bereitzustellen, ist das Laden der Tabelle in eine Heaptabelle deutlich schneller als das Laden der Daten in eine gruppierte Columnstore-Tabelle. Beim Laden von Daten in eine temporäre Tabelle wird der Ladevorgang außerdem viel schneller als beim Laden einer Tabelle in einen dauerhaften Speicher durchgeführt. Auf temporäre Tabellen mit dem Zeichen „#“ kann nur über die Sitzung zugegriffen werden, in der sie erstellt wurden, sodass sie unter Umständen nicht in allen Fällen funktionieren. Heaptabellen werden in der WITH-Klausel von CREATE TABLE definiert. Denken Sie beim Verwenden einer temporären Tabelle daran, auch für die temporäre Tabelle Statistiken zu erstellen.

Weitere Informationen finden Sie unter [Temporäre Tabellen in SQL Data Warehouse][], [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][] und [CREATE TABLE AS SELECT (Azure SQL Data Warehouse)][].

## Optimieren von gruppierten Columnstore-Tabellen
Gruppierte Columnstore-Indizes sind eine der effizientesten Möglichkeiten zum Speichern von Daten in Azure SQL Data Warehouse. Tabellen werden in SQL Data Warehouse standardmäßig als gruppierter Columnstore erstellt. Die Verwendung einer guten Segmentqualität ist wichtig, um für Abfragen in Columnstore-Tabellen die beste Leistung zu erzielen. Wenn Zeilen bei hohem Arbeitsspeicherdruck in Columnstore-Tabellen geschrieben werden, kann die Qualität von Columnstore-Segmenten leiden. Die Segmentqualität kann anhand der Anzahl an Zeilen in einer komprimierten Zeilengruppe gemessen werden. Eine Schritt-für-Schritt-Anleitung zur Erkennung und Verbesserung der Segmentqualität für gruppierte Columnstore-Tabellen finden Sie im Abschnitt **Qualität gruppierter Columnstore-Segmente** in der [Problembehandlung][]. Da eine gute Qualität der Columnstore-Segmente ziemlich wichtig ist, ist es im Allgemeinen ratsam, ausschließlich für das Laden spezielle Benutzer-IDs zu erstellen, für die eine mittelgroße oder große Ressourcenklasse verwendet wird. Je weniger DWUs Sie verwenden, desto größer sollte die Ressourcenklasse sein, die Sie Ihrem ladenden Benutzer zuweisen.

Da Columnstore-Tabellen Daten im Allgemeinen erst dann in ein komprimiertes Columnstore-Segment übertragen, wenn mehr als 1 Million Zeilen pro Tabelle vorhanden sind, und jede SQL Data Warehouse-Tabelle in 60 Tabellen partitioniert ist, gilt folgende Faustregel: Columnstore-Tabellen stellen für eine Abfrage nur dann einen Vorteil dar, wenn die Tabelle mehr als 60 Millionen Zeilen hat. Für Tabellen mit weniger als 60 Millionen Zeilen ist es unter Umständen nicht sinnvoll, einen Columnstore-Index zu nutzen. Es muss aber nicht unbedingt einen Nachteil bedeuten. Wenn Sie Ihre Daten partitionieren, können Sie auch darauf achten, dass jede Partition 1 Million Zeilen aufweisen muss, um von einem gruppierten Columnstore-Index zu profitieren. Falls eine Tabelle 100 Partitionen hat, muss sie mindestens 6 Milliarden Zeilen enthalten, um von einem gruppierten Columnstore zu profitieren (60 Verteilungen * 100 Partitionen * 1 Million Zeilen). Falls Ihre Tabelle für dieses Beispiel nicht 6 Milliarden Zeilen enthält, sollten Sie entweder die Anzahl der Partitionen reduzieren oder stattdessen eine Heaptabelle verwenden. Außerdem kann sich folgendes Experiment lohnen: Ermitteln Sie, ob eine bessere Leistung erzielt werden kann, indem eine Heaptabelle mit sekundären Indizes anstelle einer Columnstore-Tabelle verwendet wird. Columnstore-Tabellen unterstützen noch keine sekundären Indizes.

Beim Abfragen einer Columnstore-Tabelle werden die Abfragen schneller ausgeführt, wenn Sie nur die benötigten Spalten auswählen.

Weitere Informationen finden Sie unter [Problembehandlung][], [Verwalten von Columnstore-Indizes in Azure SQL Data Warehouse][] und [Beschreibung von Columnstore-Indizes][].

## Verwenden einer größeren Ressourcenklasse zum Verbessern der Abfrageleistung
SQL Data Warehouse nutzt Ressourcengruppen zum Zuordnen von Arbeitsspeicher zu Abfragen. Standardmäßig werden alle Benutzer der kleinen Ressourcenklasse zugewiesen, bei der pro Verteilung 100 MB Arbeitsspeicher gewährt werden. Da immer 60 Verteilungen vorhanden sind und jede Verteilung mindestens 100 MB erhält, beträgt die gesamte systemweite Arbeitsspeicherzuordnung 6.000 MB bzw. knapp 6 GB. Bestimmte Abfragen, z.B. große Verknüpfungen oder Ladevorgänge für gruppierte Columnstore-Tabellen, profitieren von größeren Arbeitsspeicherzuordnungen. Für einige Abfragen, z.B. reine Scans, ergibt sich kein Vorteil. Andererseits wirkt sich die Nutzung größerer Ressourcenklassen auf die Parallelität aus. Dies sollten Sie berücksichtigen, bevor Sie alle Benutzer in eine große Ressourcenklasse verschieben.
 
Weitere Informationen finden Sie unter [Parallelitäts- und Workloadverwaltung in SQL Data Warehouse][].

## Verwenden einer kleineren Ressourcenklasse zur Steigerung der Parallelität
Wenn Sie bemerken, dass Benutzerabfragen mit einer langen Verzögerung verbunden sind, werden Ihre Benutzer unter Umständen in größeren Ressourcenklassen ausgeführt und verbrauchen viele Parallelitätsslots, sodass für andere Abfragen eine Warteschlange entsteht. Führen Sie `SELECT * FROM sys.dm_pdw_waits` aus, um zu ermitteln, ob Benutzeranfragen in eine Warteschlange eingereiht werden. So können Sie testen, ob Zeilen zurückgegeben werden.

Weitere Informationen finden Sie unter [Parallelitäts- und Workloadverwaltung in SQL Data Warehouse][] und [sys.dm\_pdw\_waits (Transact-SQL)][].

## Verwenden von DMVs zum Überwachen und Optimieren von Abfragen
SQL Data Warehouse verfügt über mehrere dynamische Verwaltungssichten (DMVs), die zum Überwachen der Abfrageausführung verwendet werden können. Unten im Artikel zur Überwachung werden Sie Schritt für Schritt durch das Anzeigen einer ausgeführten Abfrage geführt. Die Verwendung der Option LABEL für Abfragen kann hilfreich sein, um Abfragen für diese DMVs schnell finden zu können.

Weitere Informationen finden Sie unter [Überwachen Ihres Workloads mit dynamischen Verwaltungssichten][], [Verwenden von Bezeichnungen zum Instrumentieren von Abfragen in SQL Data Warehouse][], [OPTION Clause (Transact-SQL)][], [sys.dm\_exec\_sessions (Transact-SQL)][], [sys.dm\_pdw\_exec\_requests (Transact-SQL)][], [sys.dm\_pdw\_request\_steps (Transact-SQL)][], [sys.dm\_pdw\_sql\_requests (Transact-SQL)][], [sys.dm\_pdw\_dms\_workers (Transact-SQL)], [DBCC PDW\_SHOWEXECUTIONPLAN (Transact-SQL)][] und [sys.dm\_pdw\_waits (Transact-SQL)][].

## Weitere Ressourcen
Es gibt viele Orte, an denen Sie nach Informationen zur Verwendung von Azure SQL Data Warehouse suchen können. Dieser Artikel ist Teil der Azure-Dokumentation und enthält viele Links zu anderen Azure-Artikeln und zu MSDN-Artikeln. Wir überwachen diese Artikel auf Ihr Feedback und arbeiten regelmäßig Aktualisierungen ein. Wenn Sie einen Artikel hilfreich finden, können Sie uns gern darüber informieren, indem Sie die Frage „War diese Seite hilfreich?“ beantworten. Sie können unabhängig davon, ob Sie mit „Ja“ oder „Nein“ antworten, Kommentare angeben. Falls ein Artikel für Sie hilfreich ist und Sie Feedback geben möchten, können Sie auf „Ja“ klicken und im Kommentarbereich angeben, wie wir den Artikel verbessern können. Falls diese Frage nicht angezeigt wird, finden Sie sie immer am Ende eines Azure-Artikels. Für MSDN-Artikel können Sie den Link für Vorschläge oben rechts auf jeder MSDN-Seite verwenden. Wir schätzen Ihr Feedback und ergreifen in den meisten Fällen entsprechende Maßnahmen.

Wenn Sie **Vorschläge für Funktionen** haben, können Sie die Seite [Azure SQL Data Warehouse Feedback][] verwenden. Indem Sie Ihre Anfragen hinzufügen oder für andere Anfragen stimmen, unterstützen Sie uns beim Priorisieren von Funktionen.

Das [MSDN-Forum für Azure SQL Data Warehouse][] ist ein Ort, an dem Sie Fragen an andere Benutzer und an die SQL Data Warehouse-Produktgruppe stellen können. Wir überwachen dieses Forum aktiv, um sicherzustellen, dass Ihre Frage entweder von einem anderen Benutzer oder einem Mitarbeiter beantwortet wird. Falls Sie Ihre Fragen lieber über Stack Overflow stellen möchten, können Sie auch das [Stack Overflow-Forum für Azure SQL Data Warehouse][] verwenden.

<!--Image references-->

<!--Article references-->
[create a support ticket]: sql-data-warehouse-get-started-create-support-ticket.md
[Parallelitäts- und Workloadverwaltung in SQL Data Warehouse]: sql-data-warehouse-develop-concurrency.md
[Create Table As Select (CTAS) in SQL Data Warehouse]: sql-data-warehouse-develop-ctas.md
[Anleitung für die Verwendung von PolyBase in SQL Data Warehouse]: sql-data-warehouse-load-polybase-guide.md
[Hashverteilung und deren Auswirkung auf die Abfrageleistung in SQL Data Warehouse]: sql-data-warehouse-develop-hash-distribution-key.md
[Laden von Daten in SQL Data Warehouse]: sql-data-warehouse-overview-load.md
[Laden von Daten mit Azure Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Load data with bcp]: sql-data-warehouse-load-with-bcp.md
[Load data with PolyBase in SQL Data Warehouse]: sql-data-warehouse-get-started-load-with-polybase.md
[Verwalten von Columnstore-Indizes in Azure SQL Data Warehouse]: sql-data-warehouse-manage-columnstore-indexes.md
[Verwalten von Statistiken in SQL Data Warehouse]: sql-data-warehouse-develop-statistics.md
[Überwachen Ihres Workloads mit dynamischen Verwaltungssichten]: sql-data-warehouse-manage-monitor.md
[Verschieben von Daten in und aus Azure SQL Data Warehouse mithilfe von Azure Data Factory]: data-factory-azure-sql-data-warehouse-connector.md
[Optimieren von Transaktionen für SQL Data Warehouse]: sql-data-warehouse-develop-best-practices-transactions.md
[Anhalten von Computeressourcen]: sql-data-warehouse-overview-scalability.md#pause-compute-bk
[Fortsetzen von Computeressourcen]: sql-data-warehouse-overview-scalability.md#resume-compute-bk
[Skalieren von Computeressourcen]: sql-data-warehouse-overview-scalability.md#scale-performance-bk
[Table design in SQL Data Warehouse]: sql-data-warehouse-develop-table-design.md
[Tabellenpartitionen in SQL Data Warehouse]: sql-data-warehouse-develop-table-partitions.md
[Temporäre Tabellen in SQL Data Warehouse]: sql-data-warehouse-develop-temporary-tables.md
[Transaktionen in SQL Data Warehouse]: sql-data-warehouse-develop-transactions.md
[Problembehandlung]: sql-data-warehouse-troubleshoot.md
[Verwenden von Bezeichnungen zum Instrumentieren von Abfragen in SQL Data Warehouse]: sql-data-warehouse-develop-label.md

<!--MSDN references-->
[ALTER TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/ms190273.aspx
[Beschreibung von Columnstore-Indizes]: https://msdn.microsoft.com/library/gg492088.aspx
[CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)]: https://msdn.microsoft.com/library/mt203953.aspx
[CREATE EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE TABLE AS SELECT (Azure SQL Data Warehouse)]: https://msdn.microsoft.com/library/mt204041.aspx
[CREATE STATISTICS (Transact-SQL)]: https://msdn.microsoft.com/library/ms188038.aspx
[DBCC PDW\_SHOWEXECUTIONPLAN (Transact-SQL)]: https://msdn.microsoft.com/library/mt204017.aspx
[Insert (Transact-SQL)]: https://msdn.microsoft.com/library/ms174335.aspx
[OPTION Clause (Transact-SQL)]: https://msdn.microsoft.com/library/ms190322.aspx
[sys.dm\_exec\_sessions (Transact-SQL)]: https://msdn.microsoft.com/library/ms176013.aspx
[sys.dm\_pdw\_exec\_requests (Transact-SQL)]: https://msdn.microsoft.com/library/mt203887.aspx
[sys.dm\_pdw\_request\_steps (Transact-SQL)]: https://msdn.microsoft.com/library/mt203913.aspx
[sys.dm\_pdw\_sql\_requests (Transact-SQL)]: https://msdn.microsoft.com/library/mt203889.aspx
[sys.dm\_pdw\_dms\_workers (Transact-SQL)]: https://msdn.microsoft.com/library/mt203878.aspx
[sys.dm\_pdw\_waits (Transact-SQL)]: https://msdn.microsoft.com/library/mt203893.aspx
[TRUNCATE TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/ms177570.aspx
[UPDATE STATISTICS (Transact-SQL)]: https://msdn.microsoft.com/library/ms187348.aspx

<!--Other Web references-->
[Choosing hash distributed table vs. round-robin distributed table]: https://blogs.msdn.microsoft.com/sqlcat/2015/08/11/choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/
[Azure SQL Data Warehouse Feedback]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-Forum für Azure SQL Data Warehouse]: https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=AzureSQLDataWarehouse
[Stack Overflow-Forum für Azure SQL Data Warehouse]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Azure SQL Data Warehouse loading patterns and strategies]: https://blogs.msdn.microsoft.com/sqlcat/2016/02/06/azure-sql-data-warehouse-loading-patterns-and-strategies

<!---HONumber=AcomDC_0504_2016-->