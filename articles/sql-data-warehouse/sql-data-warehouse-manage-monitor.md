<properties
   pageTitle="Überwachen Ihres Workloads mit dynamischen Verwaltungssichten | Microsoft Azure"
   description="Informationen zum Überwachen Ihres Workloads mit dynamischen Verwaltungssichten."
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
   ms.date="05/03/2016"
   ms.author="sonyama;barbkess;sahajs"/>

# Überwachen Ihres Workloads mit dynamischen Verwaltungssichten

Dieser Artikel beschreibt, wie Sie mit dynamischen Verwaltungssichten Ihren Workload überwachen und die Ausführung von Abfragen in Azure SQL Data Warehouse untersuchen.

## Überwachen von Verbindungen

Mithilfe der Sicht [sys.dm\_pdw\_exec\_sessions][] können Sie Verbindungen mit Ihrer Azure SQL Data Warehouse-Datenbank überwachen. Diese Sicht enthält aktive Sitzungen sowie einen Verlauf der zuletzt getrennten Sitzungen. Die Sitzungs-ID ist der Primärschlüssel für diese Sicht. Sie wird bei jeder neuen Anmeldung sequenziell zugewiesen.

```sql
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed';
```

## Untersuchen der Ausführung von Abfragen
Beginnen Sie zum Überwachen der Abfrageausführung mit [sys.dm\_pdw\_exec\_requests][]. Diese Sicht enthält Abfragen in Bearbeitung sowie einen Verlauf der Abfragen, die vor Kurzem abgeschlossen wurden. Die Abfrage-ID identifiziert eindeutig jede Abfrage. Sie ist der Primärschlüssel für diese Sicht. Die Abfrage-ID wird für jede neue Abfrage sequenziell zugewiesen. Bei der Abfrage dieser Tabelle für eine bestimmte Sitzungs-ID werden alle Abfragen für eine bestimmte Anmeldung angezeigt.

Wenn Sie die Abfrageausführung für eine bestimmte Abfrage untersuchen möchten, finden Sie hier einige allgemeine Schritte, die ausgeführt werden sollten.

### SCHRITT 1: Suchen der zu untersuchenden Abfrage

```sql
-- Monitor running queries
SELECT * FROM sys.dm_pdw_exec_requests WHERE status = 'Running';

-- Find 10 queries which ran the longest
SELECT TOP 10 * FROM sys.dm_pdw_exec_requests ORDER BY total_elapsed_time DESC;
```

Notieren Sie die Anfrage-ID der Abfrage, die Sie untersuchen möchten.

### SCHRITT 2: Überprüfen, ob die Abfrage auf Ressourcen wartet

```sql
-- Find waiting tasks for your session.
-- Replace request_id with value from Step 1.

SELECT waits.session_id,
      waits.request_id,  
      requests.command,
      requests.status,
      requests.start_time,  
      waits.type,  
      waits.object_type,
      waits.object_name,  
      waits.state  
FROM   sys.dm_pdw_waits waits
   JOIN  sys.dm_pdw_exec_requests requests
   ON waits.request_id=requests.request_id
WHERE waits.request_id = 'QID33188'
ORDER BY waits.object_name, waits.object_type, waits.state;
```

Die Ergebnisse der obigen Abfrage liefern Ihnen den Wartezustand Ihrer Abfrage.

- Wenn die Abfrage auf Ressourcen einer anderen Abfrage wartet, lautet der Status **AcquireResources**.
- Wenn die Abfrage über alle erforderlichen Ressourcen verfügt und nicht wartet, ist der Status **Granted**. Untersuchen Sie in diesem Fall die Abfrageschritte.

### SCHRITT 3: Bestimmen des am längsten dauernden Schritts des Abfrageplans

Verwenden Sie die Anfrage-ID zum Abrufen einer Liste mit den Schritten des Abfrageplans aus [sys.dm\_pdw\_request\_steps][]. Untersuchen Sie die insgesamt verstrichene Zeit, um den am längsten dauernden Schritt zu finden.

```sql

-- Find the distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID33209'
ORDER BY step_index;
```

Speichern Sie den Schrittindex des lang dauernden Schritts.

Überprüfen Sie die Spalte *operation\_type* des Schritts für die Abfrage mit langer Laufzeit:

- Fahren Sie mit Schritt 4a für **SQL-Vorgänge** fort: OnOperation, RemoteOperation, ReturnOperation.
- Fahren Sie mit Schritt 4b für **Datenverschiebungsvorgänge** fort: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.

### SCHRITT 4a: Bestimmen des Ausführungsstatus eines SQL-Schritts

Verwenden Sie die Anforderungs-ID und den Schrittindex, um Informationen aus [sys.dm\_pdw\_sql\_requests][] abzurufen. Diese Sicht enthält Details zur Ausführung der Abfrage auf den verteilten SQL Server-Instanzen. Notieren Sie die Verteilungs-ID und SPID, wenn die Abfrage noch ausgeführt wird und Sie den Plan aus der SQL Server-Verteilung abrufen möchten.

```sql
-- Find the distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID33209' AND step_index = 2;
```


Wird die Abfrage gerade ausgeführt, können Sie mit [DBCC PDW\_SHOWEXECUTIONPLAN][] den SQL Server-Ausführungsplan für den derzeit ausgeführten SQL-Schritt für eine bestimmte Verteilung abrufen.

```sql
-- Find the SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);

```

### SCHRITT 4b: Bestimmen des Ausführungsstatus eines Datenverschiebungsschritts

Verwenden Sie die Anforderungs-ID und den Schrittindex zum Abrufen von Informationen zum Datenverschiebungsschritt, der für jede Verteilung ausgeführt wird, aus [sys.dm\_pdw\_dms\_workers][].

```sql
-- Find the information about all the workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID33209' AND step_index = 2;

```

- Überprüfen Sie die Spalte *total\_elapsed\_time*, um festzustellen, ob eine bestimmte Verteilung für das Verschieben von Daten erheblich länger als andere dauert.
- Überprüfen Sie für die Verteilung mit langer Laufzeit die Spalte *rows\_processed*, um festzustellen, ob die Anzahl der Zeilen, die von dieser Verteilung verschoben werden, beträchtlich größer als bei den anderen ist. Falls ja, kann dies auf eine Ungleichmäßigkeit der zugrunde liegenden Daten hinweisen.

Wird die Abfrage gerade ausgeführt, können Sie mit [DBCC PDW\_SHOWEXECUTIONPLAN][] den SQL Server-Ausführungsplan für den derzeit ausgeführten DMS-Schritt für eine bestimmte Verteilung abrufen.

```sql
-- Find the SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);

```

## Nächste Schritte
Weitere Informationen zu dynamischen Verwaltungssichten finden Sie unter [Systemsichten][]. Tipps zur Verwaltung von SQL Data Warehouse finden Sie unter [Verwaltungstools für SQL Data Warehouse][]. Bewährte Methoden finden Sie unter [Bewährte Methoden für SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[manage data skew for distributed tables]: sql-data-warehouse-manage-distributed-data-skew.md
[Verwaltungstools für SQL Data Warehouse]: sql-data-warehouse-overview-manage.md
[Bewährte Methoden für SQL Data Warehouse]: sql-data-warehouse-best-practices.md
[Systemsichten]: sql-data-warehouse-reference-tsql-system-views.md

<!--MSDN references-->
[sys.dm\_pdw\_dms\_workers]: http://msdn.microsoft.com/library/mt203878.aspx
[sys.dm\_pdw\_exec\_requests]: http://msdn.microsoft.com/library/mt203887.aspx
[sys.dm\_pdw\_exec\_sessions]: http://msdn.microsoft.com/library/mt203883.aspx
[sys.dm\_pdw\_request\_steps]: http://msdn.microsoft.com/library/mt203913.aspx
[sys.dm\_pdw\_sql\_requests]: http://msdn.microsoft.com/library/mt203889.aspx
[DBCC PDW\_SHOWEXECUTIONPLAN]: http://msdn.microsoft.com/library/mt204017.aspx
[DBCC PDW_SHOWSPACEUSED]: http://msdn.microsoft.com/library/mt204028.aspx

<!---HONumber=AcomDC_0504_2016-->