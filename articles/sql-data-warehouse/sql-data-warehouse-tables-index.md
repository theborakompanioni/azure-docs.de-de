<properties
   pageTitle="Indizieren von Tabellen in SQL Data Warehouse | Microsoft Azure"
   description="Enthält Informationen zu den ersten Schritten bei der Tabellenindizierung in Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/12/2016"
   ms.author="jrj;barbkess;sonyama"/>

# Indizieren von Tabellen in SQL Data Warehouse

> [AZURE.SELECTOR]
- [Übersicht][]
- [Datentypen][]
- [Verteilen][]
- [Index][]
- [Partition][]
- [Statistiken][]
- [Temporär][]

SQL Data Warehouse bietet mehrere Indizierungsoptionen, z.B. [gruppierte Columnstore-Indizes][], [gruppierte Indizes und nicht gruppierte Indizes][]. Außerdem ist eine Nichtindizierungsoption vorhanden, die auch als [Heap][] bezeichnet wird. In diesem Artikel werden die Vorteile der einzelnen Indextypen und Tipps zur Erzielung der besten Leistung für die Indizes behandelt. Ausführlichere Informationen zur Erstellung einer Tabelle in SQL Data Warehouse finden Sie unter [CREATE TABLE][].

## Gruppierte Columnstore-Indizes

SQL Data Warehouse erstellt standardmäßig einen gruppierten Columnstore-Index, wenn in einer Tabelle keine Indizierungsoptionen angegeben werden. Gruppierte Columnstore-Tabellen bieten sowohl den höchsten Grad an Datenkomprimierung als auch die beste Gesamtabfrageleistung. Gruppierte Columnstore-Tabellen weisen normalerweise eine höhere Leistung als gruppierte Indizes oder Heaptabellen auf und stellen für große Tabellen meist die beste Wahl dar. Aus diesen Gründen ist der gruppierte Columnstore der beste Einstieg, wenn Sie nicht sicher sind, wie Sie Ihre Tabelle indizieren sollen.

Geben Sie zum Erstellen einer gruppierten Columnstore-Tabelle in der WITH-Klausel einfach CLUSTERED COLUMNSTORE INDEX an, oder lassen Sie die WITH-Klausel deaktiviert:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX );
```

Es gibt einige Szenarien, in denen der gruppierte Columnstore keine gute Option ist:

- Columnstore-Tabellen weisen keine Unterstützung für sekundäre nicht gruppierte Indizes auf. Erwägen Sie stattdessen die Verwendung von Heap oder gruppierten Indextabellen.
- Columnstore-Tabellen weisen keine Unterstützung für „varchar(max)“, „nvarchar(max)“ und „varbinary(max)“ auf. Erwägen Sie stattdessen die Verwendung von Heap oder gruppiertem Index.
- Unter Umständen sind Columnstore-Tabellen für vorübergehende Daten weniger effizient. Erwägen Sie die Verwendung von Heap und ggf. temporären Tabellen.
- Kleine Tabellen mit weniger als 100 Millionen Zeilen. Erwägen Sie die Verwendung von Heaptabellen.

## Heaptabellen

Wenn Sie Daten vorübergehend in SQL Data Warehouse anordnen, werden Sie wahrscheinlich merken, dass der Gesamtprozess durch die Nutzung einer Heaptabelle beschleunigt wird. Dies liegt daran, dass Ladevorgänge für Heaps schneller als das Indizieren von Tabellen sind und der nachfolgende Lesevorgang in einigen Fällen aus dem Cache erfolgen kann. Wenn Sie Daten nur laden, um sie vor dem Ausführen weiterer Transformationen bereitzustellen, ist das Laden der Tabelle in eine Heaptabelle deutlich schneller als das Laden der Daten in eine gruppierte Columnstore-Tabelle. Beim Laden von Daten in eine [temporäre Tabelle][Temporary] wird der Ladevorgang außerdem viel schneller als beim Laden einer Tabelle in einen dauerhaften Speicher durchgeführt.

Für kleine Nachschlagetabellen mit weniger als 100 Millionen Zeilen ist häufig die Nutzung von Heaptabellen sinnvoll. Für gruppierte Columnstore-Tabellen wird die optimale Komprimierung erst erreicht, wenn mehr als 100 Millionen Zeilen vorhanden sind.

Geben Sie zum Erstellen einer Heaptabelle in der WITH-Klausel einfach HEAP an:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( HEAP );
```

## Gruppierte und nicht gruppierte Indizes

Gruppierte Indizes können eine höhere Leistung als gruppierte Columnstore-Tabellen erzielen, wenn eine einzelne Zeile schnell abgerufen werden muss. Damit für Abfragen, bei denen nur eine oder sehr wenige Zeilensuchen erforderlich sind, eine sehr hohe Geschwindigkeit erzielt werden kann, können Sie die Verwendung eines gruppierten Index oder eines nicht gruppierten sekundären Index erwägen. Der Nachteil bei der Verwendung eines gruppierten Index ist, dass nur Abfragen profitieren, bei denen für die Spalte mit dem gruppierten Index ein hoch selektiver Filter eingesetzt wird. Um die Filterung für andere Spalten zu verbessern, kann den anderen Spalten ein nicht gruppierter Index hinzugefügt werden. Für jeden Index, der einer Tabelle hinzugefügt wird, wird bei Ladevorgängen sowohl der Speicherplatzbedarf als auch die Verarbeitungszeit erhöht.

Geben Sie zum Erstellen einer Tabelle mit gruppiertem Index in der WITH-Klausel einfach CLUSTERED INDEX an:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED INDEX (id) );
```

Um einer Tabelle einen nicht gruppierten Index hinzuzufügen, verwenden Sie einfach die folgende Syntax:

```SQL
CREATE INDEX zipCodeIndex ON t1 (zipCode);
```

> [AZURE.NOTE] Bei Verwendung von CREATE INDEX wird standardmäßig ein nicht gruppierter Index erstellt. Darüber hinaus ist ein nicht gruppierter Index nur in einer Zeilenspeichertabelle (HEAP oder CLUSTERED INDEX) zulässig. Nicht gruppierte Indizes zusätzlich zu einem CLUSTERED COLUMNSTORE INDEX sind zurzeit nicht zulässig.


## Optimieren von gruppierten Columnstore-Indizes

Gruppierte Columnstore-Tabellen sind in Daten in Segmenten angeordnet. Eine hohe Segmentqualität ist entscheidend, um für eine Columnstore-Tabelle eine optimale Abfrageleistung zu erzielen. Die Segmentqualität kann anhand der Anzahl von Zeilen in einer komprimierten Zeilengruppe gemessen werden. Die Segmentqualität ist am besten, wenn mindestens 100.000 Zeilen pro komprimierter Zeilengruppe vorhanden sind. Die Leistung verbessert sich, je näher die Anzahl von Zeilen pro Zeilengruppe an 1.048.576 heranreicht. Dies ist der Höchstwert für Zeilen in einer Zeilengruppe.

Die unten angegebene Sicht kann erstellt und auf Ihrem System verwendet werden, um die durchschnittlichen Zeilen pro Zeilengruppe zu berechnen und alle suboptimalen gruppierten Columnstore-Indizes zu identifizieren. Die letzte Spalte dieser Sicht wird als SQL-Anweisung generiert, die zum Neuerstellen Ihrer Indizes verwendet werden kann.

```sql
CREATE VIEW dbo.vColumnstoreDensity
AS
SELECT
        GETDATE()                                                               AS [execution_date]
,       DB_Name()                                                               AS [database_name]
,       s.name                                                                  AS [schema_name]
,       t.name                                                                  AS [table_name]
,	COUNT(DISTINCT rg.[partition_number])					AS [table_partition_count]
,       SUM(rg.[total_rows])                                                    AS [row_count_total]
,       SUM(rg.[total_rows])/COUNT(DISTINCT rg.[distribution_id])               AS [row_count_per_distribution_MAX]
,	CEILING	((SUM(rg.[total_rows])*1.0/COUNT(DISTINCT rg.[distribution_id]))/1048576) AS [rowgroup_per_distribution_MAX]
,       SUM(CASE WHEN rg.[State] = 0 THEN 1                   ELSE 0    END)    AS [INVISIBLE_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE 0    END)    AS [INVISIBLE_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 1 THEN 1                   ELSE 0    END)    AS [OPEN_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE 0    END)    AS [OPEN_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 2 THEN 1                   ELSE 0    END)    AS [CLOSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE 0    END)    AS [CLOSED_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 3 THEN 1                   ELSE 0    END)    AS [COMPRESSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE 0    END)    AS [COMPRESSED_rowgroup_rows]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[deleted_rows]   ELSE 0    END)    AS [COMPRESSED_rowgroup_rows_DELETED]
,       MIN(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_AVG]
,       'ALTER INDEX ALL ON ' + s.name + '.' + t.NAME + ' REBUILD;'             AS [Rebuild_Index_SQL]
FROM    sys.[pdw_nodes_column_store_row_groups] rg
JOIN    sys.[pdw_nodes_tables] nt                   ON  rg.[object_id]          = nt.[object_id]
                                                    AND rg.[pdw_node_id]        = nt.[pdw_node_id]
                                                    AND rg.[distribution_id]    = nt.[distribution_id]
JOIN    sys.[pdw_table_mappings] mp                 ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[tables] t                              ON  mp.[object_id]          = t.[object_id]
JOIN    sys.[schemas] s                             ON t.[schema_id]            = s.[schema_id]
GROUP BY
        s.[name]
,       t.[name]
;
```

Führen Sie nach dem Erstellen der Sicht diese Abfrage aus, um Tabellen mit Zeilengruppen mit weniger als 100.000 Zeilen zu identifizieren. Sie können den Schwellenwert von 100.000 natürlich erhöhen, wenn Sie eine noch bessere Segmentqualität erzielen möchten.

```sql
SELECT	*
FROM	[dbo].[vColumnstoreDensity]
WHERE	COMPRESSED_rowgroup_rows_AVG < 100000
        OR INVISIBLE_rowgroup_rows_AVG < 100000
```

Nachdem Sie die Abfrage ausgeführt haben, können Sie die Daten anzeigen und die Ergebnisse analysieren. In dieser Tabelle wird erläutert, auf was Sie bei der Zeilengruppenanalyse achten sollten.


| Column | Verwendung dieser Daten |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [table\_partition\_count] | Wenn die Tabelle partitioniert ist, können Sie einen höheren Wert für die Anzahl der offenen Zeilengruppen erwarten. Jeder Partition in der Verteilung kann theoretisch eine offene Zeilengruppe zugeordnet sein. Berücksichtigen Sie dies in der Analyse. Eine kleine Tabelle, die partitioniert wurde, kann ggf. durch das vollständige Entfernen der Partitionierung optimiert werden, da dies die Komprimierung verbessern würde. |
| [row\_count\_total] | Gesamtanzahl der Zeilen für die Tabelle. Mit diesem Wert können Sie z. B. den Prozentsatz der Zeilen im komprimierten Zustand berechnen. |
| [row\_count\_per\_distribution\_MAX] | Wenn alle Zeilen gleichmäßig verteilt sind, wäre dieser Wert die Zielanzahl von Zeilen pro Verteilung. Vergleichen Sie diesen Wert mit „compressed\_rowgroup\_count“. |
| [COMPRESSED\_rowgroup\_rows] | Gesamtanzahl der Zeilen im Columnstore-Format für die Tabelle. |
| [COMPRESSED\_rowgroup\_rows\_AVG] | Wenn für eine Zeilengruppe die durchschnittliche Anzahl von Zeilen erheblich kleiner als die maximale Anzahl von Zeilen ist, sollten Sie CTAS oder ALTER INDEX REBUILD zum erneuten Komprimieren der Daten verwenden. |
| [COMPRESSED\_rowgroup\_count] | Die Anzahl von Zeilengruppen im Columnstore-Format. Wenn diese Anzahl in Relation zur Tabelle sehr hoch ist, ist dies ein Indikator dafür, dass die Columnstore-Dichte gering ist. |
| [COMPRESSED\_rowgroup\_rows\_DELETED] | Zeilen werden im Columnstore-Format logisch gelöscht. Wenn die Anzahl in Relation zur Tabellengröße hoch ist, sollten Sie die Partition oder den Index neu erstellen, da sie dadurch physisch entfernt werden. |
| [COMPRESSED\_rowgroup\_rows\_MIN] | Verwenden Sie diese Spalte zusammen mit den Spalten AVG und MAX, um den Wertebereich für die Zeilengruppen im Columnstore zu verstehen. Eine geringe Anzahl über dem Schwellenwert für das Laden (102.400 pro nach Verteilung ausgerichteter Partition) zeigt an, dass Optimierungen beim Laden von Daten möglich sind. |
| [COMPRESSED\_rowgroup\_rows\_MAX] | Wie oben. |
| [OPEN\_rowgroup\_count] | Offene Zeilengruppen sind normal. In der Regel ist eine OPEN-Zeilengruppe pro Tabellenverteilung (60) zu erwarten. Eine sehr große Anzahl deutet auf das partitionsübergreifende Laden von Daten hin. Überprüfen Sie die Partitionierungsstrategie, um sicherzustellen, dass sie fehlerfrei ist. |
| [OPEN\_rowgroup\_rows] | Jede Zeilengruppe kann maximal 1.048.576 Zeilen enthalten. Verwenden Sie diesen Wert, um festzustellen, wie voll die offenen Zeilengruppen derzeit sind. |
| [OPEN\_rowgroup\_rows\_MIN] | Offene Gruppen geben an, dass Daten langsam in die Tabelle geladen werden oder dass beim vorherigen Laden verbleibende Zeilen in diese Zeilengruppe aufgenommen wurden. Verwenden Sie die Spalten MIN, MAX und AVG, um festzustellen, wie viele Daten sich in OPEN-Zeilengruppen befinden. Bei kleinen Tabellen können es 100 % aller Daten sein! Verwenden Sie in diesem Fall ALTER INDEX REBUILD, um die Datenaufnahme in einen Columnstore zu erzwingen. |
| [OPEN\_rowgroup\_rows\_MAX] | Wie oben. |
| [OPEN\_rowgroup\_rows\_AVG] | Wie oben. |
| [CLOSED\_rowgroup\_rows] | Betrachten Sie die Zeilen in geschlossenen Zeilengruppen als Integritätsprüfung. |
| [CLOSED\_rowgroup\_count] | Die Anzahl der geschlossenen Zeilengruppen sollte niedrig sein, wenn überhaupt welche vorhanden sind. Geschlossene Zeilengruppen können mit dem Befehl ALTER INDEX ... REORGANISE in komprimierte Zeilengruppen konvertiert werden. Dies ist jedoch normalerweise nicht erforderlich. Geschlossene Gruppen werden automatisch durch den Tupelverschiebungsvorgang im Hintergrund in Columnstore-Zeilengruppen konvertiert. |
| [CLOSED\_rowgroup\_rows\_MIN] | Geschlossene Zeilengruppen sollten eine sehr hohe Füllrate aufweisen. Wenn die Füllrate für eine geschlossene Zeilengruppe niedrig ist, ist eine weitere Analyse des Columnstore erforderlich. |
| [CLOSED\_rowgroup\_rows\_MAX] | Wie oben. |
| [CLOSED\_rowgroup\_rows\_AVG] | Wie oben. |
| [Rebuild\_Index\_SQL] | SQL-Code zum Neuerstellen des Columnstore-Index für eine Tabelle |

## Ursachen für eine schlechte Qualität des Columnstore-Index

Wenn Sie Tabellen mit schlechter Segmentqualität identifiziert haben, sollten Sie die Hauptursache ermitteln. Unten sind einige andere häufige Ursachen für eine schlechte Segmentqualität aufgeführt:

1. Hohe Speicherauslastung bei der Erstellung des Index
2. Hohes Volumen von DML-Vorgängen
3. Kleine oder langsame Ladevorgänge
4. Zu viele Partitionen

Diese Faktoren können dazu führen, dass ein Columnstore-Index über deutlich weniger als die optimalen 1 Million Zeilen pro Zeilengruppe verfügt. Sie können auch verursachen, dass Zeilen in die Deltazeilengruppe anstatt in eine komprimierte Zeilengruppe aufgenommen werden.

### Hohe Speicherauslastung bei der Erstellung des Index

Die Anzahl von Zeilen pro komprimierter Zeilengruppe hängt direkt mit der Breite der Zeile und der Speichermenge zusammen, die zum Verarbeiten der Zeilengruppe verfügbar ist. Wenn Zeilen bei hohem Arbeitsspeicherdruck in Columnstore-Tabellen geschrieben werden, kann die Qualität von Columnstore-Segmenten leiden. Die bewährte Methode besteht deshalb darin, der Sitzung, in der in Ihre Columnstore-Indextabellen geschrieben wird, Zugriff auf so viel Arbeitsspeicher wie möglich gewährt wird. Da ein Kompromiss zwischen Arbeitsspeicher und Parallelität eingegangen werden muss, richtet sich die Empfehlung zur richtigen Speicherbelegung nach den folgenden Aspekten: den Daten in jeder Zeile der Tabelle, der DWU-Menge, die Sie dem System zugeordnet haben, und der Anzahl von Parallelitätsslots, die Sie für eine Sitzung vergeben können, von der Daten in Ihre Tabelle geschrieben werden. Als bewährte Methode empfehlen wir zu Beginn die Verwendung von „xlargerc“ wenn Sie DW300 oder weniger verwenden, „largerc“, wenn Sie DW400 bis DW600 verwenden, und „mediumrc“, wenn Sie DW1000 oder mehr verwenden.

### Hohes Volumen von DML-Vorgängen

Ein hohes Volumen von DML-Vorgängen, mit denen Zeilen aktualisiert und gelöscht werden, sorgen für Ineffizienz im Columnstore. Dies gilt insbesondere, wenn der Großteil der Zeilen in einer Zeilengruppe geändert wird.

- Das Löschen einer Zeile aus einer komprimierten Zeilengruppe kennzeichnet die Zeile nur logisch als gelöscht. Die Zeile verbleibt in der komprimierten Zeilengruppe, bis die Partition oder Tabelle erneut erstellt wird.
- Beim Einfügen einer Zeile wird die Zeile einer internen Rowstore-Tabelle hinzugefügt, die als Deltazeilengruppe bezeichnet wird. Die eingefügte Zeile wird erst in das Columnstore-Format konvertiert, wenn die Deltazeilengruppe voll und als geschlossen markiert ist. Zeilengruppen werden geschlossen, sobald sie die maximale Kapazität von 1.048.576 Zeilen erreichen.
- Das Aktualisieren einer Zeile im Columnstore-Format wird als logisches Löschen und anschließendes Einfügen verarbeitet. Die eingefügte Zeile kann im Deltaspeicher gespeichert werden.

Als Batch ausgeführte Aktualisierungs- und Einfügevorgänge, die den Massenschwellenwert von 102.400 Zeilen pro nach Partition ausgerichteter Verteilung überschreiten, werden direkt in das Columnstore-Format geschrieben. Bei einer gleichmäßigen Verteilung müssten Sie allerdings 6,144 Millionen Zeilen in einem einzigen Vorgang ändern, damit dies eintritt. Wenn die Anzahl der Zeilen für eine bestimmte nach Partition ausgerichtete Verteilung geringer als 102.400 Zeilen ist, werden die Zeilen in den Deltaspeicher aufgenommen, bis genügend Zeilen eingefügt oder geändert wurden, um die Zeilengruppe zu schließen, oder bis der Index neu erstellt wird.

### Kleine oder langsame Ladevorgänge

Kleine Ladevorgänge in SQL Data Warehouse werden manchmal auch als langsame Ladevorgänge bezeichnet. Sie stellen in der Regel einen annähernd konstanten Datenstrom dar, der vom System erfasst wird. Dieser Datenstrom ist zwar fast kontinuierlich, die Anzahl der Zeilen ist jedoch nicht besonders groß. In den meisten Fällen liegt die Datenmenge deutlich unter dem Schwellenwert, der für ein direktes Laden in das Columnstore-Format erforderlich ist.

In diesen Situationen ist es oft besser, die Daten zunächst in den Azure-Blobspeicher zu laden, damit sie sich vor dem Laden ansammeln können. Diese Technik wird auch als *Verarbeitung von Mikrobatches* bezeichnet.

### Zu viele Partitionen

Ein weiterer zu berücksichtigender Aspekt ist die Auswirkung der Partitionierung auf Ihre gruppierten Columnstore-Tabellen. Vor dem Partitionieren teilt SQL Data Warehouse Ihre Daten bereits auf 60 Datenbanken auf. Durch die Partitionierung werden die Daten dann noch weiter aufgeteilt. Beim Partitionieren Ihrer Daten sollten Sie darauf achten, dass **jede** Partition mindestens 1 Million Zeilen aufweisen muss, um von einem gruppierten Columnstore-Index zu profitieren. Wenn Sie Ihre Tabelle in 100 Partitionen unterteilen, muss die Tabelle mindestens 6 Milliarden Zeilen aufweisen, um von einem gruppierten Columnstore-Index zu profitieren (60 Verteilungen * 100 Partitionen * 1 Million Zeilen). Falls Ihre Tabelle mit 100 Partitionen nicht 6 Milliarden Zeilen enthält, sollten Sie entweder die Anzahl der Partitionen reduzieren oder stattdessen eine Heaptabelle verwenden.

Führen Sie nach dem Laden der Tabellen mit einigen Daten die unten angegebenen Schritte aus, um Tabellen mit suboptimalen gruppierten Columnstore-Indizes zu identifizieren und neu zu erstellen.

## Neuerstellen von Indizes zur Verbesserung der Segmentqualität

### Schritt 1: Identifizieren oder Erstellen eines Benutzers, der die richtige Ressourcenklasse verwendet

Eine schnelle Möglichkeit zur sofortigen Verbesserung der Seqmentqualität besteht darin, den Index neu zu erstellen. Mit dem von der obigen Sicht zurückgegebenen SQL-Code wird eine ALTER INDEX REBUILD-Anweisung zurückgegeben, die zum Neuerstellen der Indizes verwendet werden kann. Stellen Sie beim Neuerstellen der Indizes sicher, dass Sie der Sitzung, mit der der Index neu erstellt wird, genügend Arbeitsspeicher zuordnen. Erhöhen Sie hierzu die Ressourcenklasse eines Benutzers, der über die Berechtigungen zum Neuerstellen des Index für diese Tabelle verfügt, bis auf das empfohlene Minimum. Die Ressourcenklasse des Datenbankbesitzer-Benutzers kann nicht geändert werden. Wenn Sie im System noch keinen Benutzer erstellt haben, müssen Sie dies also zuerst durchführen. Als Minimum empfehlen wir die Verwendung von „xlargerc“ wenn Sie DW300 oder weniger verwenden, „largerc“, wenn Sie DW400 bis DW600 verwenden, und „mediumrc“, wenn Sie DW1000 oder mehr verwenden.

Unten ist ein Beispiel dafür angegeben, wie Sie einem Benutzer mehr Arbeitsspeicher zuordnen, indem Sie seine Ressourcenklasse erhöhen. Weitere Informationen zu Ressourcenklassen und zur Erstellung eines neuen Benutzers finden Sie im Artikel [Parallelitäts- und Workloadverwaltung][Concurrency].

```sql
EXEC sp_addrolemember 'xlargerc', 'LoadUser'
```

### Schritt 2: Neuerstellen von gruppierten Columnstore-Indizes mit einem Benutzer mit einer höheren Ressourcenklasse
Melden Sie sich als der Benutzer aus Schritt 1 an (z.B. LoadUser), für den jetzt eine höhere Ressourcenklasse verwendet wird, und führen Sie die ALTER INDEX-Anweisungen aus. Stellen Sie sicher, dass dieser Benutzer über die ALTER-Berechtigung für die Tabellen verfügt, in denen der Index neu erstellt wird. Diese Beispiele zeigen, wie Sie den gesamten Columnstore-Index bzw. eine einzelne Partition neu erstellen. Bei großen Tabellen ist es praktischer, die Indizes Partition für Partition neu zu erstellen.

Anstatt den Index neu zu erstellen, können Sie alternativ dazu die Tabelle per [CTAS][] in eine neue Tabelle kopieren. Welche Methode ist am besten geeignet? Für große Datenmengen ist [CTAS][] in der Regel schneller als [ALTER INDEX][]. Für kleinere Datenmengen ist [ALTER INDEX][] einfacher zu verwenden, und das Auslagern der Tabelle ist nicht erforderlich. Weitere Details zur Neuerstellung von Indizes per CTAS finden Sie unten im Abschnitt **Neuerstellen von Indizes per CTAS und Partitionswechsel**.

```sql
-- Rebuild the entire clustered index
ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD
```

```sql
-- Rebuild a single partition
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5
```

```sql
-- Rebuild a single partition with archival compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE)
```

```sql
-- Rebuild a single partition with columnstore compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE)
```

Die Neuerstellung eines Index in SQL Data Warehouse ist ein Offlinevorgang. Weitere Informationen zur Neuerstellung von Indizes finden Sie im Abschnitt zu ALTER INDEX REBUILD in [Columnstore Indexes Defragmentation][] \(Columnstore-Indexdefragmentierung) und im Syntaxthema [ALTER INDEX][].
 
### Schritt 3: Sicherstellen, dass sich die Qualität gruppierter Columnstore-Segmente verbessert hat
Führen Sie die Abfrage, mit der die Tabelle mit schlechter Segmentqualität identifiziert wurde, erneut aus, und vergewissern Sie sich, dass sich die Segmentqualität verbessert hat. Falls sich die Segmentqualität nicht verbessert hat, kann dies daran liegen, dass die Zeilen in der Tabelle besonders breit sind. Erwägen Sie, beim Neuerstellen der Indizes eine höhere Ressourcenklasse oder DWU-Anzahl zu verwenden.

 
## Neuerstellen von Indizes per CTAS und Partitionswechsel

In diesem Beispiel werden [CTAS][] und Partitionswechsel verwendet, um eine Tabellenpartition neu zu erstellen.

```sql
-- Step 1: Select the partition of data and write it out to a new table using CTAS
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

-- Step 2: Create a SWITCH out table
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE   1=2 -- Note this table will be empty

-- Step 3: Switch OUT the data 
ALTER TABLE [dbo].[FactInternetSales] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales_20000101] PARTITION 2;

-- Step 4: Switch IN the rebuilt data
ALTER TABLE [dbo].[FactInternetSales_20000101_20010101] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales] PARTITION 2;
```

Weitere Informationen zum Neuerstellen von Partitionen mit `CTAS` finden Sie im Artikel [Partitionieren][].

## Nächste Schritte

Weitere Informationen finden Sie in den Artikeln [Übersicht über Tabellen][Overview], [Tabellendatentypen][Data Types], [Verteilen einer Tabelle][Distribute], [Partitionieren einer Tabelle][Partition], [Verwalten von Tabellenstatistiken][Statistics] und [Temporäre Tabellen][Temporary]. Weitere Informationen zu bewährten Methoden finden Sie unter [Bewährte Methoden für SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Übersicht]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Datentypen]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Verteilen]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Partitionieren]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Statistiken]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Temporär]: ./sql-data-warehouse-tables-temporary.md
[Concurrency]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[Bewährte Methoden für SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[ALTER INDEX]: https://msdn.microsoft.com/library/ms188388.aspx
[heap]: https://msdn.microsoft.com/library/hh213609.aspx
[gruppierte Indizes und nicht gruppierte Indizes]: https://msdn.microsoft.com/library/ms190457.aspx
[CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx
[Columnstore Indexes Defragmentation]: https://msdn.microsoft.com/library/dn935013.aspx#Anchor_1
[gruppierte Columnstore-Indizes]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->

<!---HONumber=AcomDC_0810_2016-->