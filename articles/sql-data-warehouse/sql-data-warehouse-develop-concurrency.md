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
   ms.date="08/02/2016"
   ms.author="jrj;barbkess;sonyama"/>

# Parallelitäts- und Workloadverwaltung in SQL Data Warehouse

Mit SQL Data Warehouse können Benutzer die Parallelitätsebenen und Ressourcenzuordnungen steuern, z.B. die Priorisierung von Arbeitsspeicher und CPU, um für eine bedarfsorientierte vorhersagbare Leistung zu sorgen. In diesem Artikel werden die Konzepte der Parallelitäts- und Workloadverwaltung vorgestellt. Es wird beschrieben, wie beide Features implementiert wurden und wie Sie diese in Ihrem Data Warehouse steuern können. Die Workloadverwaltung von SQL Data Warehouse ist für die Unterstützung von Mehrbenutzerumgebungen ausgelegt. Sie ist nicht für Workloads mit mehreren Mandanten ausgelegt.

## Parallelitätslimits

In SQL Data Warehouse sind bis zu 1.024 gleichzeitige Verbindungen zulässig. Alle 1.024 Verbindungen können gleichzeitig Abfragen übermitteln. Um den Durchsatz zu optimieren, reiht SQL Data Warehouse einige Abfragen unter Umständen in die Warteschlange ein, um sicherzustellen, dass für jede Abfrage ein Mindestmaß an Arbeitsspeicher gewährt wird. Queuing erfolgt während der Abfrageausführung. Durch das Einreihen von Abfragen in die Warteschlange beim Erreichen von Parellelitätslimits kann SQL Data Warehouse den Gesamtdurchsatz erhöhen, indem sichergestellt wird, dass aktive Abfragen Zugriff auf dringend benötigte Arbeitsspeicherressourcen erhalten.

Für Parallelitätslimits gelten zwei Konzepte: **gleichzeitige Abfragen** und **Parallelitätsslots**. Damit eine Abfrage ausgeführt wird, muss dies sowohl innerhalb des Abfrageparallelitätslimits als auch innerhalb der Parallelitätsslotzuordnung erfolgen.

- Bei **gleichzeitigen Abfragen** geht es einfach um die Anzahl von gleichzeitig ausgeführten Abfragen. SQL Data Warehouse unterstützt bis zu 32 gleichzeitige Abfragen.
- **Parallelitätsslots** werden basierend auf DWUs zugeordnet. Für jeweils 100 DWUs werden vier Parallelitätsslots bereitgestellt. DW100 beispielsweise ordnet vier Parallelitätsslots zu, DW1000 dagegen 40. Jede Abfrage nutzt mindestens einen Parallelitätsslot in der [Ressourcenklasse](#resource-classes) der Abfrage. Abfragen, die in der Ressourcenklasse „smallrc“ ausgeführt werden, nutzen einen Parallelitätsslot. Abfragen, die in einer höheren Ressourcenklasse ausgeführt werden, benötigen mehr Parallelitätsslots.

In der folgenden Tabelle sind die Limits für gleichzeitige Abfragen und Parallelitätsslots der verschiedenen DWU-Größen beschrieben.

### Parallelitätslimits

| DWU | Max. Anzahl gleichzeitiger Abfragen | Zugeordnete Parallelitätsslots |
| :----  | :---------------------: | :-------------------------: |
| DW100 | 32 | 4 |
| DW200 | 32 | 8 |
| DW300 | 32 | 12 |
| DW400 | 32 | 16 |
| DW500 | 32 | 20 |
| DW600 | 32 | 24 |
| DW1000 | 32 | 40 |
| DW1200 | 32 | 48 |
| DW1500 | 32 | 60 |
| DW2000 | 32 | 80 |
| DW3000 | 32 | 120 |
| DW6000 | 32 | 240 |

Wenn einer dieser beiden Schwellenwerte erreicht wird, werden neue Abfragen in die Warteschlange eingereiht. In der Warteschlange befindliche Abfragen werden nach dem Prinzip „First in, First out“ ausgeführt, wenn andere Abfragen abgeschlossen werden und die Anzahl von Abfragen und Slots unter die Limits fällt.

> [AZURE.NOTE]  SELECT-Abfragen, die exklusiv in dynamischen Verwaltungssichten (DMVs) oder Katalogsichten ausgeführt werden, werden **nicht** von den Parallelitätslimits gesteuert. So können Benutzer das System unabhängig von der Anzahl der Abfragen überwachen, die auf dem System ausgeführt werden.

## Ressourcenklassen

Mithilfe von Ressourcenklassen steuern Sie die Speicherzuweisung und die zugewiesenen CPU-Zyklen einer Abfrage. Es gibt vier Ressourcenklassen, die einem Benutzer in Form einer **Datenbankrolle** zugewiesen werden können. Diese vier Ressourcenklassen sind **smallrc, mediumrc, largerc und xlargerc**. Benutzer in „smallrc“ erhalten eine geringere Menge an Arbeitsspeicher, sodass eine höhere Parallelität ermöglicht wird. Im Gegensatz dazu werden Benutzern, die „xlargerc“ zugewiesen sind, mehr Arbeitsspeicheranteile gewährt. Dies bedeutet, dass ein geringerer Anteil dieser Abfragen gleichzeitig ausgeführt werden kann.

Standardmäßig ist jeder Benutzer Mitglied der kleinen Ressourcenklasse (smallrc). Die Prozedur `sp_addrolemember` wird verwendet, um die Ressourcenklasse heraufzusetzen, und `sp_droprolemember` wird verwendet, um die Ressourcenklasse herabzusetzen. Mit dem folgenden Befehl wird die Ressourcenklasse beispielsweise von „loaduser“ auf „largerc“ heraufgesetzt:

```sql
EXEC sp_addrolemember 'largerc', 'loaduser'
```

Eine gute Vorgehensweise besteht darin, Benutzer zu erstellen, die einer Ressourcenklasse dauerhaft zugewiesen sind, anstatt die Ressourcenklasse eines Benutzers zu ändern. Für Ladevorgänge in gruppierte Columnstore-Tabellen werden beispielsweise Indizes von höherer Qualität erstellt, wenn mehr Arbeitsspeicher zugeordnet wird. Erstellen Sie einen Benutzer speziell für das Laden von Daten, und weisen Sie diesen Benutzer dauerhaft einer höheren Ressourcenklasse zu, um sicherzustellen, dass bei Ladevorgängen Zugriff auf eine höhere Arbeitsspeicherebene besteht.

Es gibt einige Arten von Abfragen, die nicht von einer höheren Arbeitsspeicherzuordnung profitieren. Das System ignoriert ihre Ressourcenklassenzuordnung und führt diese Abfragen stattdessen immer in der kleinen Ressourcenklasse aus. Indem erzwungen wird, dass diese Abfragen immer in der kleinen Ressourcenklasse ausgeführt werden, wird Folgendes ermöglicht: Diese Abfragen können ausgeführt werden, wenn der Druck für die Parallelitätsslots hoch ist, und es wird verhindert, dass diese Abfragen mehr Slots als erforderlich belegen. Diese [Ressourcenklassenausnahmen](#query-exceptions-to-concurrency-limits) werden später in diesem Artikel behandelt.

Weitere Details zu Ressourcenklassen:

- Die Berechtigung `ALTER ROLE` ist erforderlich, um die Ressourcenklasse eines Benutzers zu ändern.
- Ein Benutzer kann zwar einer oder mehreren höheren Ressourcenklassen hinzugefügt werden, aber Benutzer nutzen die Attribute der höchsten Ressourcenklasse, der sie zugewiesen sind. Dies bedeutet Folgendes: Wenn ein Benutzer sowohl „mediumrc“ als auch „largerc“ zugewiesen ist, wird die höhere Ressourcenklasse (largerc) verwendet.
- Die Ressourcenklasse des Systemadministratorbenutzers kann nicht geändert werden.
 
Ein ausführliches Beispiel finden Sie unter [Beispiel: Ändern der Ressourcenklasse eines Benutzers](#changing-user-resource-class-example) am Ende dieses Artikels.

## Speicherzuweisung

Das Erhöhen der Ressourcenklasse eines Benutzers hat Vor- und Nachteile. Einerseits kann das Erhöhen einer Ressourcenklasse für einen Benutzer bedeuten, dass für seine Abfragen mehr Arbeitsspeicher zugänglich ist und dass sie schneller ausgeführt werden. Andererseits verringert eine höhere Ressourcenklasse aber auch die Anzahl gleichzeitig ausführbarer Abfragen. Dies ist der Kompromiss zwischen dem Zuweisen großer Mengen von Arbeitsspeicher für eine einzelne Abfrage und dem Zulassen der gleichzeitigen Ausführung anderer Abfragen, denen ebenfalls Arbeitsspeicher zugewiesen werden muss. Wenn ein Benutzer hohe Arbeitsspeicherzuordnungen für eine Abfrage erhält, haben andere Benutzer nicht Zugriff auf den gleichen Speicher zum Ausführen einer Abfrage.

In der folgenden Tabelle ist der Arbeitsspeicher angegeben, der jeder Verteilung nach DWU und Ressourcenklasse zugeordnet wird. In SQL Data Warehouse gibt es 60 Verteilungen. Beispielsweise hat eine Abfrage, die in einem DW2000 in der Ressourcenklasse „xlargerc“ ausgeführt wird, jeweils Zugriff auf 6.400 MB Arbeitsspeicher in jeder der 60 verteilten Datenbanken.

### Speicherbelegungen pro Verteilung (MB)

| DWU | smallrc | mediumrc | largerc | xlargerc |
| :----- | :-----: | :------: | :-----: | :------: |
| DW100 | 100 | 100 | 200 | 400 |
| DW200 | 100 | 200 | 400 | 800 |
| DW300 | 100 | 200 | 400 | 800 |
| DW400 | 100 | 400 | 800 | 1\.600 |
| DW500 | 100 | 400 | 800 | 1\.600 |
| DW600 | 100 | 400 | 800 | 1\.600 |
| DW1000 | 100 | 800 | 1\.600 | 3\.200 |
| DW1200 | 100 | 800 | 1\.600 | 3\.200 |
| DW1500 | 100 | 800 | 1\.600 | 3\.200 |
| DW2000 | 100 | 1\.600 | 3\.200 | 6\.400 |
| DW3000 | 100 | 1\.600 | 3\.200 | 6\.400 |
| DW6000 | 100 | 3\.200 | 6\.400 | 12\.800 |

Für das obige Beispiel gilt Folgendes: Einer Abfrage, die in einem DW2000 in der Ressourcenklasse „xlargerc“ ausgeführt wird, werden im gesamten SQL Data Warehouse insgesamt 375 GB Arbeitsspeicher zugewiesen (6.400 MB * 60 Verteilungen / 1.024 zur Umrechnung in GB).

### Systemweite Speicherbelegungen (GB)

| DWU | smallrc | mediumrc | largerc | xlargerc |
| :----- | :-----: | :------: | :-----: | :------: |
| DW100 | 6 | 6 | 12 | 23 |
| DW200 | 6 | 12 | 23 | 47 |
| DW300 | 6 | 12 | 23 | 47 |
| DW400 | 6 | 23 | 47 | 94 |
| DW500 | 6 | 23 | 47 | 94 |
| DW600 | 6 | 23 | 47 | 94 |
| DW1000 | 6 | 47 | 94 | 188 |
| DW1200 | 6 | 47 | 94 | 188 |
| DW1500 | 6 | 47 | 94 | 188 |
| DW2000 | 6 | 94 | 188 | 375 |
| DW3000 | 6 | 94 | 188 | 375 |
| DW6000 | 6 | 188 | 375 | 750 |


## Verbrauch von Parallelitätsslots

Abfragen in höheren Ressourcenklassen wird mehr Arbeitsspeicher zugewiesen. Da Arbeitsspeicher eine feststehende Ressource ist, kann umso weniger Parallelität unterstützt werden, je mehr Arbeitsspeicher pro Abfrage zugeordnet ist. Die folgende Tabelle gibt in einer einzigen Ansicht alle obigen Konzepte wieder – sowohl die Anzahl verfügbarer Parallelitätsslots pro DWU als auch die Slots, die von den einzelnen Ressourcenklassen verbraucht werden.

### Zuordnung und Verbrauch von Parallelitätsslots

| DWU | Max. Anzahl gleichzeitiger Abfragen | Zugeordnete Parallelitätsslots | Durch „smallrc“ verwendete Slots | Durch „mediumrc“ verwendete Slots | Durch „largerc“ verwendete Slots | Durch „xlargerc“ verwendete Slots |
| :----  | :---------------------: | :-------------------------: | :-----: | :------: | :-----: | :------: |
| DW100 | 32 | 4 | 1 | 1 | 2 | 4 |
| DW200 | 32 | 8 | 1 | 2 | 4 | 8 |
| DW300 | 32 | 12 | 1 | 2 | 4 | 8 |
| DW400 | 32 | 16 | 1 | 4 | 8 | 16 |
| DW500 | 32 | 20 | 1 | 4 | 8 | 16 |
| DW600 | 32 | 24 | 1 | 4 | 8 | 16 |
| DW1000 | 32 | 40 | 1 | 8 | 16 | 32 |
| DW1200 | 32 | 48 | 1 | 8 | 16 | 32 |
| DW1500 | 32 | 60 | 1 | 8 | 16 | 32 |
| DW2000 | 32 | 80 | 1 | 16 | 32 | 64 |
| DW3000 | 32 | 120 | 1 | 16 | 32 | 64 |
| DW6000 | 32 | 240 | 1 | 32 | 64 | 128 |


In dieser Tabelle sehen Sie, dass ein als DW1000 ausgeführtes SQL Data Warehouse maximal 32 gleichzeitige Abfragen und insgesamt 40 Parallelitätsslots zuordnet. Wenn alle Benutzer Abfragen in der kleinen Ressourcenklasse ausführen, sind 32 gleichzeitige Abfragen zulässig, da jede Abfrage 1 Parallelitätsslot belegt. Wenn alle Benutzer in einem DW1000 Abfragen in der mittleren Ressourcenklasse ausführen, werden jeder Abfrage 800 MB pro Verteilung zugewiesen, was eine Gesamtzuweisung an Arbeitsspeicher von 47 GB ergibt. Die Parallelität ist in diesem Fall auf 5 Benutzer begrenzt (40 Parallelitätsslots / 8 Slots pro mediumrc-Benutzer).

## Wichtigkeit von Abfragen

Hinter den Kulissen werden Ressourcenklassen mithilfe von Workloadgruppen implementiert. Es gibt insgesamt acht Workloadgruppen vorhanden, die das Verhalten der Ressourcenklassen über die verschiedenen DWU-Größen hinweg steuern. Für jede DWU werden nur jeweils vier der acht Workloadgruppen genutzt. Dies ergibt Sinn, da jede Workloadgruppe einer der vier Ressourcenklassen „smallrc“, „mediumrc“, „largerc“ oder „xlargerc“ zugewiesen ist. Sie müssen diese Workloadgruppen verstehen, da für einige dieser Workloadgruppen eine höhere **WICHTIGKEIT** festgelegt ist. Die Wichtigkeit wird für die CPU-Zeitplanung verwendet. Abfragen, die mit **hoher** Wichtigkeit ausgeführt werden, erhalten dreimal mehr CPU-Zyklen als Abfragen mit **mittlerer** Wichtigkeit. Daher wird anhand von Parallelitätsslotzuordnungen auch die CPU-Priorität bestimmt. Wenn für eine Abfrage 16 oder mehr Slots verwendet werden, ist die Wichtigkeit hoch.

Unten sind die Wichtigkeitszuordnungen für die einzelnen Workloadgruppen angegeben.

### Zuordnungen von Workloadgruppen zu Parallelitätsslots und Wichtigkeit

| Workloadgruppen | Zuordnung von Parallelitätsslots | Wichtigkeitszuordnung |
| :-------------- | :----------------------: | :----------------- |
| SloDWGroupC00 | 1 | Mittel |
| SloDWGroupC01 | 2 | Mittel |
| SloDWGroupC02 | 4 | Mittel |
| SloDWGroupC03 | 8 | Mittel |
| SloDWGroupC04 | 16 | Hoch |
| SloDWGroupC05 | 32 | Hoch |
| SloDWGroupC06 | 64 | Hoch |
| SloDWGroupC07 | 128 | Hoch |

In der Tabelle **Zuordnung und Verbrauch von Parallelitätsslots** sehen Sie, dass ein DW500 jeweils 1, 4, 8 bzw. 16 Parallelitätsslots für smallrc-, mediumrc-, largerc- und xlargerc-Ressourcenklassen verwendet. Sie können diese Werte in der oben stehenden Tabelle nachschlagen, um Informationen zur Wichtigkeit jeder Ressourcenklasse zu erhalten.

### DW500-Zuordnung zwischen Ressourcenklassen und Wichtigkeit

| Ressourcenklasse | Workloadgruppe | Verwendete Parallelitätsslots | Priorität |
| :------------- | :------------- | :--------------------: | :--------- |
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

## Abfragen mit Berücksichtigung von Parallelitätslimits

Die meisten Abfragen werden über Ressourcenklassen gesteuert. Diese Abfragen müssen die Schwellenwerte sowohl für gleichzeitige Abfragen als auch für Parallelität einhalten. Ein Endbenutzer kann eine Abfrage nicht aus dem Parallelitätsslotmodell ausschließen.

Um es noch einmal zu wiederholen: Die folgenden Anweisungen **berücksichtigen** Ressourcenklassen:

- INSERT-SELECT
- UPDATE
- DELETE
- SELECT (wenn Benutzertabellen abgefragt werden)
- ALTER INDEX REBUILD
- ALTER INDEX REORGANIZE
- ALTER TABLE REBUILD
- CREATE INDEX
- CREATE CLUSTERED COLUMNSTORE INDEX
- CREATE TABLE AS SELECT (CTAS)
- Laden von Daten
- Datenverschiebungsvorgänge, die vom DMS (Data Movement Service) ausgeführt werden

## Ausnahmen von Parallelitätslimits für Abfragen

Einige Abfragen berücksichtigen die Ressourcenklasse nicht, der der Benutzer zugeordnet ist. Diese Ausnahmen von den Parallelitätslimits werden zugelassen, wenn für einen bestimmten Befehl nicht viele Arbeitsspeicherressourcen zur Verfügung stehen. Dies ist häufig der Fall, wenn es sich bei dem Befehl um einen Metadatenvorgang handelt. Mit solchen Ausnahmen wird vermieden, dass Abfragen eine größere Menge an Speicher zugewiesen wird, die diese Abfragen niemals benötigen. In diesen Fällen wird immer die Standardressourcenklasse (die kleine Ressourcenklasse, „smallrc“) verwendet, unabhängig von der Ressourcenklasse, die dem Benutzer eigentlich zugewiesen ist. Beispielsweise wird `CREATE LOGIN` immer in „smallrc“ ausgeführt. Die zum Ausführen dieses Vorgangs erforderlichen Ressourcen sind sehr gering, und daher wäre es nicht sinnvoll, die Abfrage in das Parallelitätsslot-Modell aufzunehmen. Es wäre ineffizient, vorab große Mengen an Arbeitsspeicher für diese Aktion zuzuweisen. Durch Ausschließen von `CREATE LOGIN` aus dem Parallelitätsslot-Modell kann SQL Data Warehouse sehr viel effizienter arbeiten.

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

## Beispiel: Ändern der Ressourcenklasse eines Benutzers

1. **Anmeldung erstellen:** Öffnen Sie eine Verbindung mit der **Masterdatenbank** für Ihr SQL Data Warehouse, und führen Sie die folgenden Befehle aus.
	
	```sql
	CREATE LOGIN newperson WITH PASSWORD = 'mypassword';
	CREATE USER newperson for LOGIN newperson;
	```

	> [AZURE.NOTE] Es empfiehlt sich, Benutzer für Anmeldungen bei der Masterdatenbank sowohl in Azure SQL-Datenbank als auch in Azure SQL Data Warehouse zu erstellen. Auf dieser Ebene sind zwei Serverrollen verfügbar, die eine Anmeldung dafür erfordern, dass ein Benutzer in der Masterdatenbank ist, um die Mitgliedschaft erteilen zu können. Die Rollen lauten `Loginmanager` und `dbmanager`. Sowohl in der Azure SQL-Datenbank als auch im SQL Data Warehouse erteilen diese Rollen Berechtigungen zum Verwalten von Anmeldungen und zum Erstellen von Datenbanken. Dies unterscheidet sich bei SQL Server. Weitere Informationen finden Sie im Artikel [SQL-Datenbank-Authentifizierung und -Autorisierung: Gewähren von Zugriff][].

2. **Benutzerkonto erstellen:** Öffnen Sie eine Verbindung mit der **SQL Data Warehouse-Datenbank**, und führen Sie den folgenden Befehl aus.

	```sql
	CREATE USER newperson FOR LOGIN newperson;
	```

3. **Berechtigungen gewähren:** Im Beispiel unten wird die Berechtigung `CONTROL` in der SQL Data Warehouse-Datenbank gewährt. `CONTROL` auf Ebene der Datenbank entspricht „db\_owner“ in SQL Server.

	```sql
	GRANT CONTROL ON DATABASE::MySQLDW to newperson;
	```

4. **Ressourcenklasse heraufsetzen:** Verwenden Sie die folgende Abfrage, um einen Benutzer einer Rolle mit erhöhten Berechtigungen für die Workloadverwaltung hinzuzufügen.

	```sql
	EXEC sp_addrolemember 'largerc', 'newperson'
	```

5. **Ressourcenklasse herabsetzen:** Verwenden Sie die folgende Abfrage, um einen Benutzer aus einer Workloadverwaltungsrolle zu entfernen.

	```sql
	EXEC sp_droprolemember 'largerc', 'newperson';
	```

	> [AZURE.NOTE] Es ist nicht möglich, einen Benutzer aus „smallrc“ zu entfernen.

## Erkennung von Abfragen in der Warteschlange und andere DMVs

Die DMV `sys.dm_pdw_exec_requests` kann verwendet werden, um Abfragen zu identifizieren, die in eine Parallelitätswarteschlange eingereiht wurden. Abfragen, die auf einen Parallelitätsslot warten, weisen den Status **Angehalten** auf.

```sql
SELECT 	 r.[request_id]				 AS Request_ID
	,r.[status]				 AS Request_Status
	,r.[submit_time]			 AS Request_SubmitTime
	,r.[start_time]				 AS Request_StartTime
        ,DATEDIFF(ms,[submit_time],[start_time]) AS Request_InitiateDuration_ms
        ,r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r;
```

Workloadverwaltungsrollen können mit `sys.database_principals` angezeigt werden.

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

Mit der DMV `sys.dm_pdw_resource_waits` werden nur die Ressourcenwartezeiten einer bestimmten Abfrage angezeigt. Mit der Ressourcenwartezeit wird nur gemessen, wie lange auf die Bereitstellung von Ressourcen gewartet wird. Bei der Signalwartezeit (Signal Wait Time) wird dagegen gemessen, wie lange die zugrunde liegende SQL Server-Komponente zum Planen der Abfrage in der CPU benötigt.

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

Die DMV `sys.dm_pdw_wait_stats` kann für Verlaufsanalysen von Wartezeiten verwendet werden.

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

<!---HONumber=AcomDC_0803_2016-->