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
   ms.date="07/22/2016"
   ms.author="sonyama;barbkess;sahajs"/>

# Überwachen Ihres Workloads mit dynamischen Verwaltungssichten

Dieser Artikel beschreibt, wie Sie mit dynamischen Verwaltungssichten Ihren Workload überwachen und die Ausführung von Abfragen in Azure SQL Data Warehouse untersuchen.

## Überwachen von Verbindungen

Mithilfe der Sicht [sys.dm\_pdw\_exec\_sessions][] können Sie Verbindungen mit Ihrer Azure SQL Data Warehouse-Datenbank überwachen. Diese Sicht enthält aktive Sitzungen sowie einen Verlauf der zuletzt getrennten Sitzungen. Die Sitzungs-ID ist der Primärschlüssel für diese Sicht. Sie wird bei jeder neuen Anmeldung sequenziell zugewiesen.

```sql
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed';
```

## Überwachen der Abfrageausführung

Beginnen Sie zum Überwachen der Abfrageausführung mit [sys.dm\_pdw\_exec\_requests][]. Diese Sicht enthält Abfragen in Bearbeitung sowie einen Verlauf der Abfragen, die vor Kurzem abgeschlossen wurden. Die Anforderungs-ID identifiziert eindeutig jede Abfrage. Sie ist der Primärschlüssel für diese Sicht. Die Anforderungs-ID wird für jede neue Abfrage sequenziell zugewiesen. Bei der Abfrage dieser Tabelle für eine bestimmte Sitzungs-ID werden alle Abfragen für eine bestimmte Anmeldung angezeigt.

>[AZURE.NOTE] Gespeicherte Prozeduren verwenden mehrere „request\_ids“ (Anforderungs-IDs). Die Anforderungs-IDs werden in sequenzieller Reihenfolge zugewiesen.

Führen Sie folgende Schritte aus, um Abfrageausführungspläne und -zeiten für eine bestimmte Abfrage zu untersuchen.

### Schritt 1: Ermitteln der Abfrage, die Sie untersuchen möchten

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
```

Notieren Sie sich aus den oben stehenden Abfrageergebnissen **die Anforderungs-ID** der Abfrage, die Sie untersuchen möchten.

Abfragen im Zustand „Angehalten“ werden aufgrund von Parallelitätsgrenzen in eine Warteschlange eingereiht. Ausführliche Informationen zu diesen Grenzen finden Sie unter [Parallelitäts- und Workloadverwaltung][]. Diese Abfragen werden auch in der Abfrage „sys.dm\_pdw\_waits“ mit dem Typ UserConcurrencyResourceType angezeigt. Abfragen können auch aus anderen Gründen warten, beispielsweise wegen Sperren. Wenn Ihre Abfrage auf eine Ressource wartet, finden Sie nähere Informationen unter [Untersuchen von Anfragen, die auf Ressourcen warten][].

### Schritt 2: Ermitteln des am längsten dauernden Schritts des Abfrageplans

Verwenden Sie die Anforderungs-ID, um eine Liste mit den Schritten des Abfrageplans aus [sys.dm\_pdw\_request\_steps][] abzurufen. Untersuchen Sie die insgesamt verstrichene Zeit, um den am längsten dauernden Schritt zu finden.

```sql
-- Find the distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

Überprüfen Sie die Spalte *operation\_type* des Abfrageschritts mit langer Laufzeit, und beachten Sie den **Schrittindex**:

- Fahren Sie für **SQL-Vorgänge** mit Schritt 3a fort: OnOperation, RemoteOperation, ReturnOperation.
- Fahren Sie für **Datenverschiebungsvorgänge** mit Schritt 3b fort: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.

### Schritt 3a: Ermitteln des Ausführungsfortschritts eines SQL-Schritts

Verwenden Sie die Anforderungs-ID und den Schrittindex, um Informationen aus [sys.dm\_pdw\_sql\_requests][] abzurufen. Das Ergebnis enthält Details zur Ausführung der Abfrage in jeder der verteilten SQL Server-Instanzen.

```sql
-- Find the distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

Wird die Abfrage gerade ausgeführt, können Sie mit [DBCC PDW\_SHOWEXECUTIONPLAN][] aus dem Cache des SQL Server-Plans den berechneten SQL Server-Ausführungsplan für den derzeit ausgeführten SQL-Schritt innerhalb einer bestimmten Verteilung abrufen.

```sql
-- Find the SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### Schritt 3b: Ermitteln des Ausführungsfortschritts eines Datenverschiebungsschritts

Verwenden Sie die Anforderungs-ID und den Schrittindex, um Informationen zum Datenverschiebungsschritt, der für jede Verteilung ausgeführt wird, aus [sys.dm\_pdw\_dms\_workers][] abzurufen.

```sql
-- Find the information about all the workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

- Überprüfen Sie die Spalte *total\_elapsed\_time*, um festzustellen, ob eine bestimmte Verteilung für das Verschieben von Daten erheblich länger als andere dauert.
- Überprüfen Sie für die Verteilung mit langer Laufzeit die Spalte *rows\_processed*, um festzustellen, ob die Anzahl der Zeilen, die von dieser Verteilung verschoben werden, beträchtlich größer als bei den anderen ist. Falls ja, kann dies auf eine Ungleichmäßigkeit der zugrunde liegenden Daten hinweisen.

Wird die Abfrage gerade ausgeführt, können Sie mit [DBCC PDW\_SHOWEXECUTIONPLAN][] aus dem Cache des SQL Server-Plans den berechneten SQL Server-Ausführungsplan für den derzeit ausgeführten SQL-Schritt innerhalb einer bestimmten Verteilung abrufen.

```sql
-- Find the SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>
## Überwachen von wartenden Abfragen

Wenn Sie feststellen, dass Ihre Abfrage keine Fortschritte erzielt, weil sie auf eine Ressource wartet, können Sie mit folgender Abfrage alle Ressourcen anzeigen, auf die die eigentliche Abfrage wartet.

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

Wenn die Abfrage aktiv auf Ressourcen einer anderen Abfrage wartet, lautet der Status **AcquireResources**. Wenn die Abfrage über alle erforderlichen Ressourcen verfügt, ist der Status **Granted**.

## Nächste Schritte
Weitere Informationen zu dynamischen Verwaltungssichten finden Sie unter [Systemsichten][]. Tipps zur Verwaltung von SQL Data Warehouse finden Sie unter [Übersicht über die Verwaltung][]. Bewährte Methoden finden Sie unter [Bewährte Methoden für SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[Übersicht über die Verwaltung]: ./sql-data-warehouse-overview-manage.md
[Bewährte Methoden für SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md
[Systemsichten]: ./sql-data-warehouse-reference-tsql-system-views.md
[Parallelitäts- und Workloadverwaltung]: ./sql-data-warehouse-develop-concurrency.md
[Untersuchen von Anfragen, die auf Ressourcen warten]: ./sql-data-warehouse-manage-monitor.md#waiting

<!--MSDN references-->
[sys.dm\_pdw\_dms\_workers]: http://msdn.microsoft.com/library/mt203878.aspx
[sys.dm\_pdw\_exec\_requests]: http://msdn.microsoft.com/library/mt203887.aspx
[sys.dm\_pdw\_exec\_sessions]: http://msdn.microsoft.com/library/mt203883.aspx
[sys.dm\_pdw\_request\_steps]: http://msdn.microsoft.com/library/mt203913.aspx
[sys.dm\_pdw\_sql\_requests]: http://msdn.microsoft.com/library/mt203889.aspx
[DBCC PDW\_SHOWEXECUTIONPLAN]: http://msdn.microsoft.com/library/mt204017.aspx
[DBCC PDW_SHOWSPACEUSED]: http://msdn.microsoft.com/library/mt204028.aspx

<!---HONumber=AcomDC_0810_2016-->