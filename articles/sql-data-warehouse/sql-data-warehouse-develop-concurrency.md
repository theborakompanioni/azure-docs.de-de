<properties
   pageTitle="Parallelitäts- und Workloadverwaltung in SQL Data Warehouse | Microsoft Azure"
   description="Grundlagen der Parallelitäts- und Workloadverwaltung in Azure SQL Data Warehouse zum Entwickeln von Lösungen"
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

# Parallelitäts- und Workloadverwaltung in SQL Data Warehouse

Mit SQL Data Warehouse können Benutzer die Parallelitätsebenen und Ressourcenzuordnungen steuern, z.B. die Priorisierung von Arbeitsspeicher und CPU, um für eine bedarfsorientierte vorhersagbare Leistung zu sorgen. In diesem Artikel werden die Konzepte der Parallelitäts- und Workloadverwaltung vorgestellt. Es wird beschrieben, wie beide Features implementiert wurden und wie Sie diese in Ihrem Data Warehouse steuern können. Die Workloadverwaltung von SQL Data Warehouse ist für die Unterstützung von Mehrbenutzerumgebungen ausgelegt. Sie ist nicht für Workloads mit mehreren Mandanten ausgelegt.

## Parallelitätslimits

In SQL Data Warehouse sind bis zu 1.024 gleichzeitige Verbindungen zulässig. Alle 1.024 Verbindungen können gleichzeitig Abfragen übermitteln. Um den Durchsatz zu optimieren, reiht SQL Data Warehouse einige Abfragen unter Umständen in die Warteschlange ein, um sicherzustellen, dass für jede Abfrage ein Mindestmaß an Arbeitsspeicher gewährt wird. Das Einreihen in die Warteschlange erfolgt während der Abfrageausführung, wenn Parallelitätslimits erreicht werden. Durch das Einreihen von Abfragen in die Warteschlange beim Erreichen von Parellelitätslimits kann SQL Data Warehouse den Gesamtdurchsatz erhöhen, indem sichergestellt wird, dass aktive Abfragen Zugriff auf dringend benötigte Arbeitsspeicherressourcen erhalten.

Für Parallelitätslimits gelten zwei Konzepte: **gleichzeitige Abfragen** und **Parallelitätsslots**. Damit eine Abfrage ausgeführt wird, muss dies sowohl unter Einhaltung des Parallelitätslimits als auch der Parallelitätsslotzuordnung erfolgen.

- Bei **gleichzeitigen Abfragen** geht es einfach um die Anzahl von gleichzeitig ausgeführten Abfragen. SQL Data Warehouse unterstützt bis zu 32 **gleichzeitige Abfragen** für höhere DW-Größen wie DW1000 und darüber. Da die Anzahl von gleichzeitigen Abfragen je nach der Anzahl von DWUs variiert, ist unten eine Tabelle mit den Beschränkungen nach DWU angegeben.
- Bei **Parallelitätsslots** geht es um ein dynamischeres Konzept. Jede gleichzeitig ausgeführte Abfrage verbraucht einen oder mehrere Parallelitätsslots. Die genaue Anzahl von Slots, die von einer Abfrage verbraucht werden, richtet sich nach der Größe Ihrer SQL Data Warehouse-Instanz und der [Ressourcenklasse](#resource-classes) der Abfrage.

In der folgenden Tabelle sind die Limits für gleichzeitige Abfragen und Parallelitätsslots beschrieben.

### Parallelitätslimits

| | DW100 | DW200 | DW300 | DW400 | DW500 | DW600 | DW1000 | DW1200 | DW1500 | DW2000 | DW3000 | DW6000 |
| :--------------------------- | ----: | ----: | ----: | ----: | ----: | ----: | -----: | -----: | -----: | -----: | -----: | -----: |
| Max. Anzahl gleichzeitiger Abfragen | 32 | 32 | 32 | 32 | 32 | 32 | 32 | 32 | 32 | 32 | 32 | 32 |
| Max. Parallelitätsslots | 4 | 8 | 12 | 16 | 20 | 24 | 40 | 48 | 60 | 80 | 120 | 240 |

Wenn einer dieser beiden Schwellenwerte erreicht wird, werden neue Abfragen in die Warteschlange eingereiht. In der Warteschlange befindliche Abfragen werden nach dem Prinzip „First in, First out“ ausgeführt, wenn Abfragen abgeschlossen werden und die Anzahl von Abfragen und Slots unter die Limits fällt.

> [AZURE.NOTE]  SELECT-Abfragen, die exklusiv in dynamischen Verwaltungssichten (DMVs) oder Katalogsichten ausgeführt werden, werden **nicht** von Ressourcenklassen gesteuert. So können Benutzer das System auch dann überwachen, wenn alle Parallelitätsslots verwendet werden.

## Ressourcenklassen

Ressourcenklassen sind ein wesentlicher Bestandteil der SQL Data Warehouse-Workloadverwaltung. Sie ermöglichen es, dass Abfragen, die von einem Benutzer durchgeführt werden, mehr Arbeitsspeicheranteile bzw. CPU-Zyklen zugeordnet werden. Es gibt vier Ressourcenklassen, bei denen es sich jeweils um eine **Datenbankrolle** handelt. Diese vier Ressourcenklassen sind **smallrc, mediumrc, largerc und xlargerc**. Benutzer in „smallrc“ erhalten eine geringere Menge an Arbeitsspeicher, sodass eine höhere Parallelität ermöglicht wird. Im Gegensatz dazu werden Benutzern, die „xlargerc“ zugewiesen sind, mehr Arbeitsspeicheranteile gewährt. Dies bedeutet, dass ein geringerer Anteil dieser Abfragen gleichzeitig ausgeführt werden kann.

Es gibt einige Arten von Abfragen, die von der Zuordnung von mehr Arbeitsspeicher nicht profitieren und deshalb die Ressourcenklassenzuordnung ignorieren und weiterhin mit der kleinen Ressourcenklasse ausgeführt werden. Indem erzwungen wird, dass diese Abfragen immer in der kleinen Ressourcenklasse ausgeführt werden, wird Folgendes ermöglicht: Diese Abfragen können ausgeführt werden, wenn der Druck für die Parallelitätsslots hoch ist, und es wird verhindert, dass diese Abfragen mehr Slots als erforderlich belegen. Diese [Ressourcenklassenausnahmen](#resource-class-exceptions) werden später in diesem Artikel behandelt.

Standardmäßig ist jeder Benutzer Mitglied der kleinen Ressourcenklasse (smallrc). Die Prozedur `sp_addrolemember` wird verwendet, um die Ressourcenklasse heraufzusetzen, und `sp_droprolemember` wird verwendet, um die Ressourcenklasse herabzusetzen. Mit dem folgenden Befehl wird die Ressourcenklasse beispielsweise von „loaduser“ auf „largerc“ heraufgesetzt:

```sql
EXEC sp_addrolemember 'largerc', 'loaduser'
```

Eine gute Vorgehensweise besteht darin, Benutzer zu erstellen, die einer Ressourcenklasse dauerhaft zugewiesen sind, anstatt die Ressourcenklasse eines Benutzers zu ändern. Für Ladevorgänge in gruppierte Columnstore-Tabellen werden beispielsweise Indizes von höherer Qualität erstellt, wenn mehr Arbeitsspeicher zugeordnet wird. Erstellen Sie einen Benutzer speziell für das Laden von Daten, und weisen Sie diesen Benutzer dauerhaft einer höheren Ressourcenklasse zu, um sicherzustellen, dass bei Ladevorgängen Zugriff auf eine höhere Arbeitsspeicherebene besteht.

Weitere Details zu Ressourcenklassen:

- Die Berechtigung `ALTER ROLE` ist erforderlich, um die Ressourcenklasse eines Benutzers zu ändern.
- Ein Benutzer kann zwar einer oder mehreren höheren Ressourcenklassen hinzugefügt werden, aber Benutzer nutzen die Attribute der höchsten Ressourcenklasse, der sie zugewiesen sind. Dies bedeutet Folgendes: Wenn ein Benutzer sowohl „mediumrc“ als auch „largerc“ zugewiesen ist, wird die höhere Ressourcenklasse (largerc) verwendet.
- Die Ressourcenklasse des Systemadministratorbenutzers kann nicht geändert werden.
 
Weitere Informationen und Beispiele für das Erstellen von Benutzern und das Zuweisen zu Ressourcenklassen finden Sie im Abschnitt [Verwalten von Benutzern](#Managing-users) am Ende dieses Artikels.

## Speicherzuweisung

Das Erhöhen der Ressourcenklasse eines Benutzers hat Vor- und Nachteile. Einerseits kann das Erhöhen einer Ressourcenklasse für einen Benutzer bedeuten, dass für seine Abfragen mehr Arbeitsspeicher zugänglich ist und dass sie schneller ausgeführt werden. Andererseits verringert sich aber auch die Anzahl von gleichzeitigen Abfragen, die ausgeführt werden können. Dies ist der Kompromiss zwischen dem Zuordnen großer Mengen von Arbeitsspeicher für eine einzelne Abfrage und dem Zulassen der Ausführung von anderen gleichzeitigen Abfragen, für die ebenfalls Arbeitsspeicher zugeordnet werden muss. Wenn ein Benutzer mehr Arbeitsspeicher für eine Abfrage erhält, steht anderen Benutzern kein Arbeitsspeicher für die Ausführung einer Abfrage zur Verfügung.

In der folgenden Tabelle ist der Arbeitsspeicher angegeben, der jeder Verteilung nach DWU und Ressourcenklasse zugeordnet wird. In SQL Data Warehouse gibt es 60 Verteilungen pro Datenbank. Eine Abfrage, die unter DW2000 in der Ressourcenklasse „xlarge“ ausgeführt wird, hat daher jeweils Zugriff auf 6.400 MB in allen 60 Datenbanken.

### Speicherbelegungen pro Verteilung (MB)

| | DW100 | DW200 | DW300 | DW400 | DW500 | DW600 | DW1000 | DW1200 | DW1500 | DW2000 | DW3000 | DW6000 |
| :------------- | ----: | ----: | ----: | ----: | ----: | ----: | -----: | -----: | -----: | -----: | -----: | -----: |
| smallrc | 100 | 100 | 100 | 100 | 100 | 100 | 100 | 100 | 100 | 100 | 100 | 100 |
| mediumrc | 100 | 200 | 200 | 400 | 400 | 400 | 800 | 800 | 800 | 1\.600 | 1\.600 | 3\.200 |
| largerc | 200 | 400 | 400 | 800 | 800 | 800 | 1\.600 | 1\.600 | 1\.600 | 3\.200 | 3\.200 | 6\.400 |
| xlargerc | 400 | 800 | 800 | 1\.600 | 1\.600 | 1\.600 | 3\.200 | 3\.200 | 3\.200 | 6\.400 | 6\.400 | 12\.800 |


Für das obige Beispiel gilt Folgendes: Einer Abfrage, die unter DW2000 in der Ressourcenklasse „xlarge“ ausgeführt wird, werden systemweit insgesamt 375 GB Arbeitsspeicher zugeordnet (6.400 MB * 60 Verteilungen / 1.024 für die Umrechnung in GB).

### Systemweite Speicherbelegungen (GB)

| | DW100 | DW200 | DW300 | DW400 | DW500 | DW600 | DW1000 | DW1200 | DW1500 | DW2000 | DW3000 | DW6000 |
| :------------- | ----: | ----: | ----: | ----: | ----: | ----: | -----: | -----: | -----: | -----: | -----: | -----: |
|smallrc | 6 | 6 | 6 | 6 | 6 | 6 | 6 | 6 | 6 | 6 | 6 | 6 |
|mediumrc | 6 | 12 | 12 | 23 | 23 | 23 | 47 | 47 | 47 | 94 | 94 | 188 |
|largerc | 12 | 23 | 23 | 47 | 47 | 47 | 94 | 94 | 94 | 188 | 188 | 375 |
|xlargerc | 23 | 47 | 47 | 94 | 94 | 94 | 188 | 188 | 188 | 375 | 375 | 750 |

## Verbrauch von Parallelitätsslots

Es wurde bereits erwähnt, dass umso mehr Arbeitsspeicher gewährt wird, je höher die Ressourcenklasse ist. Da Arbeitsspeicher eine feststehende Ressource ist, kann umso weniger Parallelität unterstützt werden, je mehr Arbeitsspeicher pro Abfrage zugeordnet ist. In der folgenden Tabelle sind die Anzahl von Parallelitätsslots pro DWU und die Slots angegeben, die von den einzelnen Ressourcenklassen verbraucht werden.

### Zuordnung und Verbrauch von Parallelitätsslots

| | DW100 | DW200 | DW300 | DW400 | DW500 | DW600 | DW1000 | DW1200 | DW1500 | DW2000 | DW3000 | DW6000 |
| :---------------------- | ----: | ----: | ----: | ----: | ----: | ----: | -----: | -----: | -----: | -----: | -----: | -----: |
| **Zuordnung** | | | | | | | | | | | | |
| Max. Anzahl gleichzeitiger Abfragen | 32 | 32 | 32 | 32 | 32 | 32 | 32 | 32 | 32 | 32 | 32 | 32 |
| Max. Parallelitätsslots | 4 | 8 | 12 | 16 | 20 | 24 | 40 | 48 | 60 | 80 | 80 | 80 |
| **Slotverbrauch** | | | | | | | | | | | | |
| smallrc | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |
| mediumrc | 1 | 2 | 2 | 4 | 4 | 4 | 8 | 8 | 8 | 16 | 16 | 32 |
| largerc | 2 | 4 | 4 | 8 | 8 | 8 | 16 | 16 | 16 | 32 | 32 | 64 |
| xlargerc | 4 | 8 | 8 | 16 | 16 | 16 | 32 | 32 | 32 | 64 | 64 | 128 |

In dieser Tabelle sehen Sie, dass eine mit DW100 ausgeführte SQL Data Warehouse-Instanz entweder vier gleichzeitige Abfragen vom Typ „smallrc“ oder zwei gleichzeitige Abfragen vom Typ „largerc“ zulässt.

## Wichtigkeit von Abfragen

Im Hintergrund sind insgesamt acht Workloadgruppen vorhanden, die das Verhalten von Ressourcenklassen steuern. Für eine DWU werden aber nur jeweils vier der acht Workloadgruppen genutzt. Dies ergibt Sinn, da jede Workloadgruppe entweder „smallrc“, „mediumrc“, „largerc“ oder „xlargerc“ zugewiesen ist. Sie müssen diese im Hintergrund befindlichen Workloadgruppen verstehen, da für einige dieser Workloadgruppen eine höhere **WICHTIGKEIT** (bzw. Priorität) festgelegt ist. Die Wichtigkeit wird für die CPU-Zeitplanung verwendet. Abfragen, die mit hoher Wichtigkeit ausgeführt werden, erhalten dreimal mehr CPU-Zyklen als Abfragen mit mittlerer Wichtigkeit. Daher wird anhand von Parallelitätsslotzuordnungen auch die CPU-Wichtigkeit bestimmt. Wenn für eine Abfrage 16 oder mehr Slots verwendet werden, ist die Wichtigkeit hoch.

Unten sind die Wichtigkeitszuordnungen für die einzelnen Workloadgruppen angegeben.

| Workloadgruppen | Zuordnung von Parallelitätsslots | Wichtigkeitszuordnung |
| :------------------  | :----------------------: | :----------------- |
| SloDWGroupC00 | 1 | Mittel |
| SloDWGroupC01 | 2 | Mittel |
| SloDWGroupC02 | 4 | Mittel |
| SloDWGroupC03 | 8 | Mittel |
| SloDWGroupC04 | 16 | Hoch |
| SloDWGroupC05 | 32 | Hoch |
| SloDWGroupC06 | 64 | Hoch |
| SloDWGroupC07 | 128 | Hoch |

Für eine SQL Data Warehouse-Instanz mit DW500 werden die aktiven Workloadgruppen den Ressourcenklassen beispielsweise wie folgt zugeordnet:

| Ressourcenklasse | Workloadgruppe | Verwendete Parallelitätsslots | Priorität |
| :--------------- | :------------- | :--------------------:   | :--------- |
| smallrc | SloDWGroupC00 | 1 | Mittel |
| mediumrc | SloDWGroupC02 | 4 | Mittel |
| largerc | SloDWGroupC03 | 8 | Mittel |
| xlargerc | SloDWGroupC04 | 16 | Hoch |


Die folgende DMV-Abfrage kann verwendet werden, um die Unterschiede bei der Arbeitsspeicherressourcen-Zuordnung ausführlich aus Sicht der Ressourcenkontrolle anzuzeigen. Außerdem kann die derzeitige und zurückliegende Nutzung der Workloadgruppen bei der Problembehandlung analysiert werden:

```sql
WITH rg
AS
(   SELECT  
     pn.name						AS node_name
    ,pn.[type]						AS node_type
    ,pn.pdw_node_id					AS node_id
    ,rp.name						AS pool_name
    ,rp.max_memory_kb*1.0/1024				AS pool_max_mem_MB
    ,wg.name						AS group_name
    ,wg.importance					AS group_importance
    ,wg.request_max_memory_grant_percent		AS group_request_max_memory_grant_pcnt
    ,wg.max_dop						AS group_max_dop
    ,wg.effective_max_dop				AS group_effective_max_dop
    ,wg.total_request_count				AS group_total_request_count
    ,wg.total_queued_request_count			AS group_total_queued_request_count
    ,wg.active_request_count				AS group_active_request_count
    ,wg.queued_request_count				AS group_queued_request_count
    FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
    JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    
            ON  wg.pdw_node_id  = rp.pdw_node_id
    	    AND wg.pool_id      = rp.pool_id
    JOIN    sys.dm_pdw_nodes pn	
            ON	wg.pdw_node_id	= pn.pdw_node_id
    WHERE   wg.name like 'SloDWGroup%'
        AND     rp.name = 'SloDWPool'
)
SELECT	pool_name
,		pool_max_mem_MB
,		group_name
,		group_importance
,		(pool_max_mem_MB/100)*group_request_max_memory_grant_pcnt AS max_memory_grant_MB
,		node_name
,		node_type
,       group_total_request_count
,       group_total_queued_request_count
,       group_active_request_count
,       group_queued_request_count
FROM	rg
ORDER BY
	node_name
,	group_request_max_memory_grant_pcnt
,	group_importance
;
```

## Ressourcenklassenausnahmen

Bei den meisten Abfragen werden die Ressourcenklassen beachtet, aber es gibt auch einige Ausnahmen. Dies ist in der Regel der Fall, wenn die zum Ausführen der Aktion erforderlichen Ressourcen gering sind. Bei den Ausnahmen handelt es sich meist um Fälle, in denen eine Abfrage die höhere Arbeitsspeicherebene, die von höheren Ressourcenklassen zugeordnet wird, niemals nutzt. Es wird dann immer die Standardressourcenklasse oder kleine Ressourcenklasse (smallrc) verwendet. Dies gilt unabhängig von der Ressourcenklasse, die dem Benutzer zugewiesen ist. Beispielsweise wird `CREATE LOGIN` immer in „smallrc“ ausgeführt. Die zum Ausführen dieses Vorgangs erforderlichen Ressourcen sind sehr gering, und daher wäre es nicht sinnvoll, die Abfrage in das Parallelitätsslot-Modell aufzunehmen. Es wäre ineffizient, vorab große Mengen an Arbeitsspeicher für diese Aktion zuzuweisen. Durch Ausschließen von `CREATE LOGIN` aus dem Parallelitätsslot-Modell kann SQL Data Warehouse sehr viel effizienter arbeiten.

Die folgenden Anweisungen berücksichtigen Ressourcenklassen **nicht**:

- CREATE oder DROP TABLE
- ALTER TABLE ... SWITCH, SPLIT oder MERGE PARTITION
- ALTER INDEX DISABLE
- DROP INDEX
- CREATE, UPDATE oder DROP STATISTICS
- TRUNCATE TABLE
- ALTER AUTHORIZATION
- CREATE LOGIN
- CREATE, ALTER oder DROP USER
- CREATE, ALTER oder DROP PROCEDURE
- CREATE oder DROP VIEW
- INSERT VALUES
- SELECT aus Systemsichten und DMVs
- EXPLAIN
- DBCC

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

### Abfragen mit Berücksichtigung von Parallelitätslimits

Es ist wichtig zu beachten, dass die Mehrzahl der Endbenutzerabfragen wahrscheinlich durch Ressourcenklassen gesteuert werden. Als allgemeine Regel gilt, dass die aktive Abfrageworkload sowohl den Schwellenwert für gleichzeitige Abfragen als auch den Schwellenwert für Parallelitätsslots einhalten muss, sofern sie nicht ausdrücklich von der Plattform ausgeschlossen wurde. Als Endbenutzer können Sie eine Abfrage nicht aus dem Parallelitätsslot-Modell ausschließen. Falls einer der Schwellenwerte überschritten wird, werden Abfragen in die Warteschlange eingereiht. Abfragen in der Warteschlange werden anhand der Prioritätsreihenfolge gefolgt vom Übermittlungszeitpunkt abgearbeitet.

Zur Wiederholung: Die folgenden Anweisungen **berücksichtigen** Ressourcenklassen:

- INSERT-SELECT
- UPDATE
- DELETE
- SELECT (wenn Benutzertabellen abgefragt werden)
- ALTER INDEX REBUILD
- ALTER INDEX REORGANIZE
- ALTER TABLE REBUILD
- CREATE INDEX
- CREATE CLUSTERED COLUMNSTORE INDEX
- CREATE TABLE AS SELECT
- Laden von Daten
- Datenverschiebungsvorgänge, die vom DMS (Data Movement Service) ausgeführt werden


## Verwalten von Benutzern

1. **Anmeldung erstellen:** Öffnen Sie eine Verbindung mit der **Masterdatenbank** für Ihre SQL Data Warehouse-Instanz, und führen Sie die folgenden Befehle aus:
	
	```sql
	CREATE LOGIN newperson WITH PASSWORD = 'mypassword';
	CREATE USER newperson for LOGIN newperson;
	```

	> [AZURE.NOTE] Es empfiehlt sich, Benutzer für Anmeldungen bei der Masterdatenbank sowohl in Azure SQL-Datenbank als auch in Azure SQL Data Warehouse zu erstellen. Auf dieser Ebene sind zwei Serverrollen verfügbar, die eine Anmeldung dafür erfordern, dass ein Benutzer in der Masterdatenbank ist, um die Mitgliedschaft erteilen zu können. Die Rollen lauten `Loginmanager` und `dbmanager`. Sowohl in der Azure SQL-Datenbank als auch im SQL Data Warehouse erteilen diese Rollen Berechtigungen zum Verwalten von Anmeldungen und zum Erstellen von Datenbanken. Dies unterscheidet sich bei SQL Server. Weitere Informationen finden Sie im Artikel [SQL-Datenbank-Authentifizierung und -Autorisierung: Gewähren von Zugriff][].

2. **Benutzerkonto erstellen:** Öffnen Sie eine Verbindung mit der **SQL Data Warehouse-Datenbank**, und führen Sie den folgenden Befehl aus:

	```sql
	CREATE USER newperson FOR LOGIN newperson;
	```

3. **Berechtigungen gewähren:** Im Beispiel unten erteilt `CONTROL` Berechtigungen in der SQL Data Warehouse-Datenbank. `CONTROL` auf Ebene der Datenbank entspricht „db\_owner“ in SQL Server.

	```sql
	GRANT CONTROL ON DATABASE::MySQLDW to newperson;
	```

4. **Ressourcenklasse heraufsetzen:** Verwenden Sie die folgende Abfrage, um einen Benutzer einer erhöhten Workload-Verwaltungsrolle hinzuzufügen:

	```sql
	EXEC sp_addrolemember 'largerc', 'newperson'
	```

5. **Ressourcenklasse herabsetzen:** Verwenden Sie die folgende Abfrage, um einen Benutzer aus einer Workload-Verwaltungsrolle zu entfernen.

	```sql
	EXEC sp_droprolemember 'largerc', 'newperson';
	```

	> [AZURE.NOTE] Es ist nicht möglich, einen Benutzer aus „smallrc“ zu entfernen.

## Erkennung von Abfragen in der Warteschlange und andere DMVs

Die DMV `sys.dm_pdw_exec_requests` kann verwendet werden, um Abfragen zu identifizieren, die in eine Parallelitätswarteschlange eingereiht wurden.

```sql
SELECT 	 r.[request_id]				 AS Request_ID
	,r.[status]				 AS Request_Status
	,r.[submit_time]			 AS Request_SubmitTime
	,r.[start_time]				 AS Request_StartTime
        ,DATEDIFF(ms,[submit_time],[start_time]) AS Request_InitiateDuration_ms
        ,r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r;
```

Workload-Verwaltungsrollen können mit `sys.database_principals` angezeigt werden.

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0;
```

Mit der folgenden Abfrage wird angezeigt, welcher Rolle die einzelnen Benutzer zugewiesen sind.

```sql
SELECT	r.name AS role_principal_name
,		m.name AS member_principal_name
FROM	sys.database_role_members rm
JOIN	sys.database_principals AS r			ON rm.role_principal_id		= r.principal_id
JOIN	sys.database_principals AS m			ON rm.member_principal_id	= m.principal_id
WHERE	r.name IN ('mediumrc','largerc', 'xlargerc');
```

SQL Data Warehouse verfügt über die folgenden Wartetypen:

- LocalQueriesConcurrencyResourceType: Bezieht sich auf Abfragen, die sich außerhalb des Parallelitätsslot-Frameworks befinden. DMV-Abfragen und Systemfunktionen wie `SELECT @@VERSION` sind Beispiele für lokale Abfragen.
- UserConcurrencyResourceType: Bezieht sich auf Abfragen, die sich innerhalb des Parallelitätsslot-Frameworks befinden. Abfragen für Endbenutzertabellen sind ein Beispiel für die Verwendung dieses Ressourcentyps.
- DmsConcurrencyResourceType: Bezieht sich auf Wartezeiten, die sich aufgrund von Datenverschiebungen ergeben.
- BackupConcurrencyResourceType: Diese Wartezeit gibt an, dass eine Datenbank gesichert wird. Der maximale Wert für diesen Ressourcentyp ist 1. Wenn mehrere Sicherungen gleichzeitig angefordert werden, werden die restlichen in die Warteschlange eingereiht.

Mit der DMV `sys.dm_pdw_waits` kann angezeigt werden, auf welche Ressourcen eine Abfrage wartet.

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]			AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,	SESSION_ID()			AS Current_session
,	s.[status]			AS Session_status
,	s.[login_name]
,	s.[query_count]
,	s.[client_id]
,	s.[sql_spid]
,	r.[command]			AS Request_command
,	r.[label]
,	r.[status]			AS Request_status
,	r.[submit_time]
,	r.[start_time]
,	r.[end_compile_time]
,	r.[end_time]
,	DATEDIFF(ms,r.[submit_time],r.[start_time])		AS Request_queue_time_ms
,	DATEDIFF(ms,r.[start_time],r.[end_compile_time])	AS Request_compile_time_ms
,	DATEDIFF(ms,r.[end_compile_time],r.[end_time])		AS Request_execution_time_ms
,	r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE	w.[session_id] <> SESSION_ID();
```

Mit der DMV `sys.dm_pdw_resource_waits` werden nur die Ressourcenwartezeiten angezeigt, die von einer bestimmten Abfrage verbraucht werden. Mit der Ressourcenwartezeit wird nur gemessen, wie lange auf die Bereitstellung von Ressourcen gewartet wird. Bei der Signalwartezeit (Signal Wait Time) wird dagegen gemessen, wie lange die zugrunde liegende SQL Server-Komponente zum Planen der Abfrage in der CPU benötigt.

```sql
SELECT  [session_id]
,       [type]
,       [object_type]
,       [object_name]
,       [request_id]
,       [request_time]
,       [acquire_time]
,       DATEDIFF(ms,[request_time],[acquire_time])  AS acquire_duration_ms
,       [concurrency_slots_used]                    AS concurrency_slots_reserved
,       [resource_class]
,       [wait_id]                                   AS queue_position
FROM    sys.dm_pdw_resource_waits
WHERE	[session_id] <> SESSION_ID();
```

Die DMV `sys.dm_pdw_wait_stats` kann für die Verlaufsanalysen von Wartezeiten verwendet werden.

```sql
SELECT	w.[pdw_node_id]
,		w.[wait_name]
,		w.[max_wait_time]
,		w.[request_count]
,		w.[signal_time]
,		w.[completed_count]
,		w.[wait_time]
FROM	sys.dm_pdw_wait_stats w;
```

## Nächste Schritte

Weitere Informationen zum Verwalten von Datenbankbenutzern und der Sicherheit finden Sie unter [Sichern einer Datenbank in SQL Data Warehouse][]. Weitere Informationen dazu, wie größere Ressourcenklassen die Qualität von gruppierten Columnstore-Indizes verbessern können, finden Sie unter [Neuerstellen von Indizes zur Verbesserung der Segmentqualität].

<!--Image references-->

<!--Article references-->
[Sichern einer Datenbank in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Neuerstellen von Indizes zur Verbesserung der Segmentqualität]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality

<!--MSDN references-->
[SQL-Datenbank-Authentifizierung und -Autorisierung: Gewähren von Zugriff]: https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->

<!---HONumber=AcomDC_0713_2016-->