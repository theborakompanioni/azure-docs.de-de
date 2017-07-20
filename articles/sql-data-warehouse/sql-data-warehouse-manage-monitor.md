---
title: "Überwachen Ihrer Workload mit dynamischen Verwaltungssichten | Microsoft Docs"
description: "Informationen zum Überwachen Ihrer Workload mit dynamischen Verwaltungssichten."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 69ecd479-0941-48df-b3d0-cf54c79e6549
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 10/31/2016
ms.author: joeyong;barbkess
ms.translationtype: Human Translation
ms.sourcegitcommit: fc27849f3309f8a780925e3ceec12f318971872c
ms.openlocfilehash: 7ce6c2cdf1e28852da536414533ccdcdaeb437e5
ms.contentlocale: de-de
ms.lasthandoff: 06/14/2017


---
# <a name="monitor-your-workload-using-dmvs"></a>Überwachen Ihrer Workload mit dynamischen Verwaltungssichten
Dieser Artikel beschreibt, wie Sie mit dynamischen Verwaltungssichten Ihre Workload überwachen und die Ausführung von Abfragen in Azure SQL Data Warehouse untersuchen.

## <a name="permissions"></a>Berechtigungen
Um die DMVs in diesem Artikel abzufragen, benötigen Sie die Berechtigung VIEW DATABASE STATE oder CONTROL. Üblicherweise ist VIEW DATABASE STATE die bevorzugte Berechtigung, da sie wesentlich restriktiver ist.

```sql
GRANT VIEW DATABASE STATE TO myuser;
```

## <a name="monitor-connections"></a>Überwachen von Verbindungen
Alle Anmeldungen bei SQL Data Warehouse werden in [sys.dm_pdw_exec_sessions][sys.dm_pdw_exec_sessions] protokolliert.  Diese DMV enthält die letzten 10.000 Anmeldungen.  Die Sitzungs-ID ist der Primärschlüssel und wird bei jeder neuen Anmeldung sequenziell zugewiesen.

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a>Überwachen der Abfrageausführung
Alle in SQL Data Warehouse ausgeführten Abfragen werden in [sys.dm_pdw_exec_requests][sys.dm_pdw_exec_requests] protokolliert.  Diese DMV enthält die letzten 10.000 ausgeführten Abfragen.  Die Anforderungs-ID identifiziert jede Abfrage eindeutig. Sie ist der Primärschlüssel für diese DMV.  Die Anforderungs-ID wird für jede neue Abfrage sequenziell zugewiesen und erhält das Präfix QID für Abfrage-ID.  Bei der Abfrage dieser DMV für eine bestimmte Sitzungs-ID werden alle Abfragen für eine bestimmte Anmeldung angezeigt.

> [!NOTE]
> Gespeicherte Prozeduren verwenden mehrere Anforderungs-IDs.  Anforderungs-IDs werden in sequenzieller Reihenfolge zugewiesen. 
> 
> 

Führen Sie folgende Schritte aus, um Abfrageausführungspläne und -zeiten für eine bestimmte Abfrage zu untersuchen.

### <a name="step-1-identify-the-query-you-wish-to-investigate"></a>Schritt 1: Ermitteln der Abfrage, die Sie untersuchen möchten
```sql
-- Monitor active queries
SELECT * 
FROM sys.dm_pdw_exec_requests 
WHERE status not in ('Completed','Failed','Cancelled')
  AND session_id <> session_id()
ORDER BY submit_time DESC;

-- Find top 10 queries longest running queries
SELECT TOP 10 * 
FROM sys.dm_pdw_exec_requests 
ORDER BY total_elapsed_time DESC;

-- Find a query with the Label 'My Query'
-- Use brackets when querying the label column, as it it a key word
SELECT  *
FROM    sys.dm_pdw_exec_requests
WHERE   [label] = 'My Query';
```

Notieren Sie sich aus den oben stehenden Abfrageergebnissen **die Anforderungs-ID** der Abfrage, die Sie untersuchen möchten.

Abfragen im Status **Angehalten** werden aufgrund von Parallelitätslimits in die Warteschlange gestellt. Diese Abfragen werden auch in der Abfrage „sys.dm_pdw_waits“ mit dem Typ UserConcurrencyResourceType angezeigt. Weitere Informationen zu Parallelitätslimits finden Sie unter [Parallelitäts- und Workloadverwaltung][Concurrency and workload management]. Abfragen können auch aus anderen Gründen warten, beispielsweise wegen Objektsperren.  Wenn Ihre Abfrage auf eine Ressource wartet, finden Sie nähere Informationen unter [Untersuchen von Abfragen, die auf Ressourcen warten][Investigating queries waiting for resources] weiter unten in diesem Artikel.

Vereinfachen Sie die Suche nach einer Abfrage in der Tabelle „sys.dm_pdw_exec_requests“ mithilfe von [LABEL][LABEL], um Ihrer Abfrage einen Kommentar hinzuzufügen, der in der Ansicht „sys.dm_pdw_exec_requests“ gesucht werden kann.

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;
```

### <a name="step-2-investigate-the-query-plan"></a>Schritt 2: Untersuchen des Abfrageplans
Rufen Sie mit der Anforderungs-ID den DSQL-Plan (Distributed SQL, verteiltes SQL) der Abfrage aus [sys.dm_pdw_request_steps][sys.dm_pdw_request_steps] ab.

```sql
-- Find the distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

Wenn ein DSQL-Plan mehr Zeit in Anspruch nimmt als erwartet, kann die Ursache ein komplexer Plan mit vielen DSQL-Schritten oder nur ein einziger Schritt sein, der einen langen Zeitraum benötigt.  Wenn der Plan viele Schritte mit mehreren Verschiebungen aufweist, erwägen Sie die Optimierung Ihrer Tabellenverteilungen, um Datenverschiebungen zu reduzieren. Der Artikel [Verteilen von Tabellen][Table distribution] erläutert, warum Daten verschoben werden müssen, um eine Abfrage zu lösen, und erläutert einige Verteilungsstrategien zum Minimieren von Datenverschiebungen.

Um weitere Informationen zu einem Einzelschritt zu erhalten, beachten Sie die Spalte *operation_type* des Abfrageschritts mit langer Laufzeit, und beachten Sie den **Schrittindex**:

* Fahren Sie für **SQL-Vorgänge**mit Schritt 3a fort: OnOperation, RemoteOperation, ReturnOperation.
* Fahren Sie für **Datenverschiebungsvorgänge**mit Schritt 3b fort: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.

### <a name="step-3a-investigate-sql-on-the-distributed-databases"></a>Schritt 3a: Untersuchen von SQL auf verteilten Datenbanken
Verwenden Sie die Anforderungs-ID und den Schrittindex, um Informationen aus [sys.dm_pdw_sql_requests][sys.dm_pdw_sql_requests] abzurufen. Das Ergebnis enthält Informationen zur Ausführung des Abfrageschritts in allen verteilten Datenbanken.

```sql
-- Find the distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

Wenn der Abfrageschritt ausgeführt wird, können Sie mit [DBCC PDW_SHOWEXECUTIONPLAN][DBCC PDW_SHOWEXECUTIONPLAN] aus dem Cache des SQL Server-Plans den berechneten SQL Server-Ausführungsplan für den in einer bestimmten Verteilung ausgeführten Schritt abrufen.

```sql
-- Find the SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-3b-investigate-data-movement-on-the-distributed-databases"></a>Schritt 3 b: Untersuchen der Datenverschiebung auf den verteilten Datenbanken
Verwenden Sie die Anforderungs-ID und den Schrittindex, um Informationen zu einem Datenverschiebungsschritt, der für jede Verteilung ausgeführt wird, aus [sys.dm_pdw_dms_workers][sys.dm_pdw_dms_workers] abzurufen.

```sql
-- Find the information about all the workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

* Überprüfen Sie die Spalte *total_elapsed_time*, um festzustellen, ob das Verschieben von Daten in einer bestimmten Verteilung erheblich länger dauert als in anderen Verteilungen.
* Überprüfen Sie für die Verteilung mit langer Laufzeit die Spalte *rows_processed*, um festzustellen, ob die Anzahl der Zeilen, die von dieser Verteilung verschoben werden, beträchtlich größer als bei den anderen ist. Falls ja, kann dies auf eine Ungleichmäßigkeit der zugrunde liegenden Daten hinweisen.

Wird die Abfrage gerade ausgeführt, können Sie mit [DBCC PDW_SHOWEXECUTIONPLAN][DBCC PDW_SHOWEXECUTIONPLAN] aus dem Cache des SQL Server-Plans den berechneten SQL Server-Ausführungsplan für den derzeit ausgeführten SQL-Schritt innerhalb einer bestimmten Verteilung abrufen.

```sql
-- Find the SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>

## <a name="monitor-waiting-queries"></a>Überwachen von wartenden Abfragen
Wenn Sie feststellen, dass Ihre Abfrage keine Fortschritte erzielt, weil sie auf eine Ressource wartet, können Sie mit folgender Abfrage alle Ressourcen anzeigen, auf die eine Abfrage wartet.

```sql
-- Find queries 
-- Replace request_id with value from Step 1.

SELECT waits.session_id,
      waits.request_id,  
      requests.command,
      requests.status,
      requests.start_time,  
      waits.type,
      waits.state,
      waits.object_type,
      waits.object_name
FROM   sys.dm_pdw_waits waits
   JOIN  sys.dm_pdw_exec_requests requests
   ON waits.request_id=requests.request_id
WHERE waits.request_id = 'QID####'
ORDER BY waits.object_name, waits.object_type, waits.state;
```

Wenn die Abfrage aktiv auf Ressourcen einer anderen Abfrage wartet, lautet der Status **AcquireResources**.  Wenn die Abfrage über alle erforderlichen Ressourcen verfügt, ist der Status **Granted**.

## <a name="monitor-tempdb"></a>Überwachen von tempdb
Eine hohe tempdb-Auslastung kann die Hauptursache für Probleme in Verbindung mit geringer Leistung und unzureichendem Arbeitsspeicher sein. Überprüfen Sie zuerst, ob Datenschiefe oder Zeilengruppen schlechter Qualität vorhanden sind, und führen Sie die entsprechenden Aktionen durch. Ziehen Sie die Skalierung Ihres Data Warehouse in Betracht, wenn Sie feststellen, dass tempdb beim Ausführen der Abfrage vollständig ausgelastet ist. Im Folgenden wird beschrieben, wie zu jedem Knoten die tempdb-Auslastung pro Abfrage ermittelt wird. 

Erstellen Sie die folgende Ansicht, um die entsprechende Knoten-ID für „sys.dm_pdw_sql_requests“ zuzuordnen. Dadurch können Sie andere Pass-Through-DMVs nutzen und diese Tabellen mit „sys.dm_pdw_sql_requests“ verknüpfen.

```sql
-- sys.dm_pdw_sql_requests with the correct node id
CREATE VIEW sql_requests AS
(SELECT
       sr.request_id,
       sr.step_index,
       (CASE 
              WHEN (sr.distribution_id = -1 ) THEN 
              (SELECT pdw_node_id FROM sys.dm_pdw_nodes WHERE type = 'CONTROL') 
              ELSE d.pdw_node_id END) AS pdw_node_id,
       sr.distribution_id,
       sr.status,
       sr.error_id,
       sr.start_time,
       sr.end_time,
       sr.total_elapsed_time,
       sr.row_count,
       sr.spid,
       sr.command
FROM sys.pdw_distributions AS d
RIGHT JOIN sys.dm_pdw_sql_requests AS sr ON d.distribution_id = sr.distribution_id)
```
Führen Sie die folgende Abfrage aus, um tempdb zu überwachen:

```sql
-- Monitor tempdb
SELECT
    sr.request_id,
    ssu.session_id,
    ssu.pdw_node_id,
    sr.command,
    sr.total_elapsed_time,
    es.login_name AS 'LoginName',
    DB_NAME(ssu.database_id) AS 'DatabaseName',
    (es.memory_usage * 8) AS 'MemoryUsage (in KB)',
    (ssu.user_objects_alloc_page_count * 8) AS 'Space Allocated For User Objects (in KB)',
    (ssu.user_objects_dealloc_page_count * 8) AS 'Space Deallocated For User Objects (in KB)',
    (ssu.internal_objects_alloc_page_count * 8) AS 'Space Allocated For Internal Objects (in KB)',
    (ssu.internal_objects_dealloc_page_count * 8) AS 'Space Deallocated For Internal Objects (in KB)',
    CASE es.is_user_process
    WHEN 1 THEN 'User Session'
    WHEN 0 THEN 'System Session'
    END AS 'SessionType',
    es.row_count AS 'RowCount'
FROM sys.dm_pdw_nodes_db_session_space_usage AS ssu
    INNER JOIN sys.dm_pdw_nodes_exec_sessions AS es ON ssu.session_id = es.session_id AND ssu.pdw_node_id = es.pdw_node_id
    INNER JOIN sys.dm_pdw_nodes_exec_connections AS er ON ssu.session_id = er.session_id AND ssu.pdw_node_id = er.pdw_node_id
    INNER JOIN sql_requests AS sr ON ssu.session_id = sr.spid AND ssu.pdw_node_id = sr.pdw_node_id
WHERE DB_NAME(ssu.database_id) = 'tempdb'
    AND es.session_id <> @@SPID
    AND es.login_name <> 'sa' 
ORDER BY sr.request_id;
```
## <a name="monitor-memory"></a>Überwachen des Arbeitsspeichers

Der Arbeitsspeicher kann die Hauptursache für Probleme in Verbindung mit geringer Leistung und unzureichendem Arbeitsspeicher sein. Überprüfen Sie zuerst, ob Datenschiefe oder Zeilengruppen schlechter Qualität vorhanden sind, und führen Sie die entsprechenden Aktionen durch. Ziehen Sie die Skalierung Ihres Data Warehouse in Betracht, wenn Sie feststellen, dass die Speicherauslastung von SQL Server beim Ausführen der Abfrage die Grenzwerte erreicht.

Die folgende Abfrage gibt die Speicherauslastung von SQL Server und die Speicherauslastung pro Knoten zurück:   
```sql
-- Memory consumption
SELECT
  pc1.cntr_value as Curr_Mem_KB, 
  pc1.cntr_value/1024.0 as Curr_Mem_MB,
  (pc1.cntr_value/1048576.0) as Curr_Mem_GB,
  pc2.cntr_value as Max_Mem_KB,
  pc2.cntr_value/1024.0 as Max_Mem_MB,
  (pc2.cntr_value/1048576.0) as Max_Mem_GB,
  pc1.cntr_value * 100.0/pc2.cntr_value AS Memory_Utilization_Percentage,
  pc1.pdw_node_id
FROM
-- pc1: current memory
sys.dm_pdw_nodes_os_performance_counters AS pc1
-- pc2: total memory allowed for this SQL instance
JOIN sys.dm_pdw_nodes_os_performance_counters AS pc2 
ON pc1.object_name = pc2.object_name AND pc1.pdw_node_id = pc2.pdw_node_id
WHERE
pc1.counter_name = 'Total Server Memory (KB)'
AND pc2.counter_name = 'Target Server Memory (KB)'
```
## <a name="monitor-transaction-log-size"></a>Überwachen der Größe von Transaktionsprotokollen
Die folgende Abfrage gibt die Größe von Transaktionsprotokollen für jede Verteilung zurück. Überprüfen Sie, ob Datenschiefe oder Zeilengruppen schlechter Qualität vorhanden sind, und führen Sie die entsprechenden Aktionen durch. Wenn eine der Protokolldateien 160 GB erreicht, sollten Sie Ihre Instanz eventuell zentral hochskalieren oder die Transaktionsgröße beschränken. 
```sql
-- Transaction log size
SELECT
  instance_name as distribution_db,
  cntr_value*1.0/1048576 as log_file_size_used_GB,
  pdw_node_id 
FROM sys.dm_pdw_nodes_os_performance_counters 
WHERE 
instance_name like 'Distribution_%' 
AND counter_name = 'Log File(s) Used Size (KB)'
AND counter_name = 'Target Server Memory (KB)'
```
## <a name="monitor-transaction-log-rollback"></a>Überwachen des Rollbacks von Transaktionsprotokollen
Wenn bei Ihren Abfragen Fehler auftreten oder deren Verarbeitung sehr lange dauert, können Sie überprüfen und überwachen, ob Sie über Rollbacks von Transaktionen verfügen.
```sql
-- Monitor rollback
SELECT 
    SUM(CASE WHEN t.database_transaction_next_undo_lsn IS NOT NULL THEN 1 ELSE 0 END),
    t.pdw_node_id,
    nod.[type]
FROM sys.dm_pdw_nodes_tran_database_transactions t
JOIN sys.dm_pdw_nodes nod ON t.pdw_node_id = nod.pdw_node_id
GROUP BY t.pdw_node_id, nod.[type]
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu DMVs finden Sie unter [Systemsichten][System views].
Weitere Informationen zu bewährten Methoden finden Sie unter [Bewährte Methoden für SQL Data Warehouse][SQL Data Warehouse best practices].

<!--Image references-->

<!--Article references-->
[Manage overview]: ./sql-data-warehouse-overview-manage.md
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[System views]: ./sql-data-warehouse-reference-tsql-system-views.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Concurrency and workload management]: ./sql-data-warehouse-develop-concurrency.md
[Investigating queries waiting for resources]: ./sql-data-warehouse-manage-monitor.md#waiting

<!--MSDN references-->
[sys.dm_pdw_dms_workers]: http://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_exec_requests]: http://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_exec_sessions]: http://msdn.microsoft.com/library/mt203883.aspx
[sys.dm_pdw_request_steps]: http://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: http://msdn.microsoft.com/library/mt203889.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: http://msdn.microsoft.com/library/mt204017.aspx
[DBCC PDW_SHOWSPACEUSED]: http://msdn.microsoft.com/library/mt204028.aspx
[LABEL]: https://msdn.microsoft.com/library/ms190322.aspx

