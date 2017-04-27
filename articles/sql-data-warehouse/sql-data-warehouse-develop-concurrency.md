---
title: "Parallelitäts- und Workloadverwaltung in SQL Data Warehouse | Microsoft Docs"
description: "Grundlagen der Parallelitäts- und Workloadverwaltung in Azure SQL Data Warehouse zum Entwickeln von Lösungen"
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: ef170f39-ae24-4b04-af76-53bb4c4d16d3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 10/31/2016
ms.author: jrj;barbkess
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: dd8c1b07262fc375678262a7617699c1f69c6090
ms.lasthandoff: 04/03/2017


---
# <a name="concurrency-and-workload-management-in-sql-data-warehouse"></a>Parallelitäts- und Workloadverwaltung in SQL Data Warehouse
Mit Microsoft Azure SQL Data Warehouse können Sie die Parallelitätsebenen und Ressourcenzuordnungen steuern, z.B. die Priorisierung von Arbeitsspeicher und CPU, um für eine vorhersagbare und skalierbare Leistung zu sorgen. In diesem Artikel werden die Konzepte der Parallelitäts- und Workloadverwaltung vorgestellt. Es wird beschrieben, wie beide Features implementiert wurden und wie Sie diese in Ihrem Data Warehouse steuern können. Die Workloadverwaltung von SQL Data Warehouse ist für die Unterstützung von Mehrbenutzerumgebungen ausgelegt. Sie ist nicht für Workloads mit mehreren Mandanten ausgelegt.

## <a name="concurrency-limits"></a>Parallelitätslimits
In SQL Data Warehouse sind bis zu 1.024 gleichzeitige Verbindungen zulässig. Alle 1.024 Verbindungen können gleichzeitig Abfragen übermitteln. Um den Durchsatz zu optimieren, reiht SQL Data Warehouse einige Abfragen unter Umständen in die Warteschlange ein, um sicherzustellen, dass für jede Abfrage ein Mindestmaß an Arbeitsspeicher gewährt wird. Queuing erfolgt während der Abfrageausführung. Beim Erreichen von Parallelitätslimits kann SQL Data Warehouse den Gesamtdurchsatz erhöhen, indem Abfragen in Warteschlangen eingereiht werden und so sichergestellt wird, dass aktive Abfragen Zugriff auf dringend benötigte Arbeitsspeicherressourcen erhalten.  

Für Parallelitätslimits gelten zwei Konzepte: *gleichzeitige Abfragen* und *Parallelitätsslots*. Damit eine Abfrage ausgeführt wird, muss dies sowohl innerhalb des Abfrageparallelitätslimits als auch innerhalb der Parallelitätsslotzuordnung erfolgen.

* Bei gleichzeitigen Abfragen geht es um die Anzahl von gleichzeitig ausgeführten Abfragen. SQL Data Warehouse unterstützt bis zu 32 gleichzeitige Abfragen für höhere DWU-Größen.
* Parallelitätsslots werden basierend auf DWUs zugeordnet. Für jeweils 100 DWUs werden vier Parallelitätsslots bereitgestellt. DW100 beispielsweise ordnet vier Parallelitätsslots zu, DW1000 dagegen 40. Jede Abfrage nutzt mindestens einen Parallelitätsslot, je nach [Ressourcenklasse](#resource-classes) der Abfrage. Abfragen, die in der Ressourcenklasse „smallrc“ ausgeführt werden, nutzen einen Parallelitätsslot. Abfragen, die in einer höheren Ressourcenklasse ausgeführt werden, benötigen mehr Parallelitätsslots.

In der folgenden Tabelle werden die Limits für gleichzeitige Abfragen und Parallelitätsslots der verschiedenen DWU-Größen beschrieben.

### <a name="concurrency-limits"></a>Parallelitätslimits
| DWU | Max. Anzahl gleichzeitiger Abfragen | Zugeordnete Parallelitätsslots |
|:--- |:---:|:---:|
| DW100 |4 |4 |
| DW200 |8 |8 |
| DW300 |12 |12 |
| DW400 |16 |16 |
| DW500 |20 |20 |
| DW600 |24 |24 |
| DW1000 |32 |40 |
| DW1200 |32 |48 |
| DW1500 |32 |60 |
| DW2000 |32 |80 |
| DW3000 |32 |120 |
| DW6000 |32 |240 |

Wenn einer dieser Grenzwerte erreicht wird, werden neue Abfragen in die Warteschlange eingereiht und nach dem Prinzip „First In, First Out“ ausgeführt.  Wenn eine Abfrage abgeschlossen wird und die Anzahl von Abfragen und Slots unter die Grenzwerte sinkt, werden Abfragen in der Warteschlange freigegeben. 

> [!NOTE]
> *SELECT* -Abfragen, die exklusiv in dynamischen Verwaltungssichten (DMVs) oder Katalogsichten ausgeführt werden, werden nicht von den Parallelitätslimits gesteuert. Sie können das System unabhängig von der Anzahl der darin ausgeführten Abfragen überwachen.
> 
> 

## <a name="resource-classes"></a>Ressourcenklassen
Mithilfe von Ressourcenklassen steuern Sie die Speicherzuweisung und die zugewiesenen CPU-Zyklen einer Abfrage. Sie können einem Benutzer vier Ressourcenklassen in Form von *Datenbankrollen* zuweisen. Diese vier Ressourcenklassen sind **smallrc**, **mediumrc**, **largerc** und **xlargerc**. Benutzer in „smallrc“ erhalten eine geringere Menge an Arbeitsspeicher und können von höherer Parallelität profitieren. Im Gegensatz dazu wird Benutzern, die „xlargerc“ zugewiesen sind, eine große Menge an Arbeitsspeicher gewährt. Dies bedeutet, dass ein geringerer Anteil ihrer Abfragen gleichzeitig ausgeführt werden kann.

Standardmäßig ist jeder Benutzer Mitglied der kleinen Ressourcenklasse (smallrc). Die Prozedur `sp_addrolemember` wird verwendet, um die Ressourcenklasse heraufzusetzen, und `sp_droprolemember` wird verwendet, um die Ressourcenklasse herabzusetzen. Mit dem folgenden Befehl wird die Ressourcenklasse beispielsweise von „loaduser“ auf „largerc“ heraufgesetzt:

```sql
EXEC sp_addrolemember 'largerc', 'loaduser'
```

Eine gute Vorgehensweise besteht darin, Benutzer dauerhaft zu einer Ressourcenklasse zuzuweisen, anstatt die Ressourcenklasse der Benutzer zu ändern. Für Ladevorgänge in gruppierte Columnstore-Tabellen werden beispielsweise Indizes von höherer Qualität erstellt, wenn mehr Arbeitsspeicher zugeordnet wird. Erstellen Sie einen Benutzer speziell für das Laden von Daten, und weisen Sie diesen Benutzer dauerhaft einer höheren Ressourcenklasse zu, um sicherzustellen, dass bei Ladevorgängen Zugriff auf eine höhere Arbeitsspeicherebene besteht.

Es gibt einige Arten von Abfragen, die nicht von einem größeren Arbeitsspeicher profitieren. Das System ignoriert die Ressourcenklassenzuweisung dieser Abfragen und führt sie stattdessen immer in der kleinen Ressourcenklasse aus. Wenn diese Abfragen immer in der kleinen Ressourcenklasse ausgeführt werden, können sie auch dann ausgeführt werden, wenn nicht viele Parallelitätsslots zur Verfügung stehen, und sie verbrauchen nicht mehr Slots als notwendig. Weitere Informationen finden Sie unter [Ressourcenklassenausnahmen](#query-exceptions-to-concurrency-limits) .

Weitere Details zu Ressourcenklassen:

* *Rolle ändern* ist erforderlich, um die Ressourcenklasse eines Benutzers zu ändern.  
* Sie können zwar einen Benutzer zu mehreren der höheren Ressourcenklassen hinzufügen, Benutzer nutzen aber die Attribute der höchsten Ressourcenklasse, der sie zugewiesen sind. Dies bedeutet Folgendes: Wenn ein Benutzer sowohl „mediumrc“ als auch „largerc“ zugewiesen ist, wird die höhere Ressourcenklasse (largerc) berücksichtigt.  
* Die Ressourcenklasse des Systemadministratorbenutzers kann nicht geändert werden.

Ein ausführliches Beispiel finden Sie unter [Beispiel: Ändern der Ressourcenklasse eines Benutzers](#changing-user-resource-class-example).

## <a name="memory-allocation"></a>Speicherzuweisung
Das Erhöhen der Ressourcenklasse eines Benutzers hat Vor- und Nachteile. Durch das Erhöhen einer Ressourcenklasse für einen Benutzer ist für die Abfragen mehr Arbeitsspeicher zugänglich. Das kann bedeuten, dass Abfragen schneller ausgeführt werden.  Durch höhere Ressourcenklassen verringert sich jedoch auch die Anzahl von gleichzeitigen Abfragen, die ausgeführt werden können. Dies ist der Kompromiss zwischen dem Zuweisen großer Mengen von Arbeitsspeicher für eine einzelne Abfrage und dem Zulassen der gleichzeitigen Ausführung anderer Abfragen, denen ebenfalls Arbeitsspeicher zugewiesen werden muss. Wenn einem Benutzer eine große Menge Arbeitsspeicher für eine Abfrage zugewiesen wurde, können andere Benutzer nicht auf diesen Arbeitsspeicher zugreifen, um eine Abfrage auszuführen.

In der folgenden Tabelle ist der Arbeitsspeicher angegeben, der jeder Verteilung nach DWU und Ressourcenklasse zugeordnet wird.

### <a name="memory-allocations-per-distribution-mb"></a>Speicherbelegungen pro Verteilung (MB)
| DWU | smallrc | mediumrc | largerc | xlargerc |
|:--- |:---:|:---:|:---:|:---:|
| DW100 |100 |100 |200 |400 |
| DW200 |100 |200 |400 |800 |
| DW300 |100 |200 |400 |800 |
| DW400 |100 |400 |800 |1.600 |
| DW500 |100 |400 |800 |1.600 |
| DW600 |100 |400 |800 |1.600 |
| DW1000 |100 |800 |1.600 |3.200 |
| DW1200 |100 |800 |1.600 |3.200 |
| DW1500 |100 |800 |1.600 |3.200 |
| DW2000 |100 |1.600 |3.200 |6.400 |
| DW3000 |100 |1.600 |3.200 |6.400 |
| DW6000 |100 |3.200 |6.400 |12.800 |

In der obigen Tabelle sehen Sie, dass eine Abfrage, die in einem DW2000 in der Ressourcenklasse „xlargerc“ ausgeführt wird, jeweils Zugriff auf 6.400 MB Arbeitsspeicher in jeder der 60 verteilten Datenbanken hat.  In SQL Data Warehouse gibt es 60 Verteilungen. Daher sollten zur Berechnung der gesamten Arbeitsspeicherbelegung für eine Abfrage in einer bestimmten Ressourcenklasse die oben genannten Werte mit 60 multipliziert werden.

### <a name="memory-allocations-system-wide-gb"></a>Systemweite Speicherbelegungen (GB)
| DWU | smallrc | mediumrc | largerc | xlargerc |
|:--- |:---:|:---:|:---:|:---:|
| DW100 |6 |6 |12 |23 |
| DW200 |6 |12 |23 |47 |
| DW300 |6 |12 |23 |47 |
| DW400 |6 |23 |47 |94 |
| DW500 |6 |23 |47 |94 |
| DW600 |6 |23 |47 |94 |
| DW1000 |6 |47 |94 |188 |
| DW1200 |6 |47 |94 |188 |
| DW1500 |6 |47 |94 |188 |
| DW2000 |6 |94 |188 |375 |
| DW3000 |6 |94 |188 |375 |
| DW6000 |6 |188 |375 |750 |

In dieser Tabelle mit systemweiten Speicherbelegungen sehen Sie, dass einer Abfrage, die in einem DW2000 in der Ressourcenklasse „xlargerc“ ausgeführt wird, im gesamten SQL Data Warehouse insgesamt 375 GB Arbeitsspeicher zugewiesen werden (6.400 MB × 60 Verteilungen ÷ 1.024 zur Umrechnung in GB).

## <a name="concurrency-slot-consumption"></a>Verbrauch von Parallelitätsslots
SQL Data Warehouse weist Abfragen, die in höheren Ressourcenklassen ausgeführt werden, mehr Arbeitsspeicher zu. Der Arbeitsspeicher ist eine feststehende Ressource.  Daher gilt Folgendes: Je mehr Arbeitsspeicher pro Abfrage zugeordnet ist, desto weniger parallele Abfragen können unterstützt werden. Die folgende Tabelle gibt in einer einzigen Ansicht alle oben stehenden Konzepte wieder – sowohl die Anzahl verfügbarer Parallelitätsslots pro DWU als auch die Slots, die von den einzelnen Ressourcenklassen verbraucht werden.

### <a name="allocation-and-consumption-of-concurrency-slots"></a>Zuordnung und Verbrauch von Parallelitätsslots
| DWU | Maximale Anzahl gleichzeitiger Abfragen | Zugeordnete Parallelitätsslots | Durch „smallrc“ verwendete Slots | Durch „mediumrc“ verwendete Slots | Durch „largerc“ verwendete Slots | Durch „xlargerc“ verwendete Slots |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|
| DW100 |4 |4 |1 |1 |2 |4 |
| DW200 |8 |8 |1 |2 |4 |8 |
| DW300 |12 |12 |1 |2 |4 |8 |
| DW400 |16 |16 |1 |4 |8 |16 |
| DW500 |20 |20 |1 |4 |8 |16 |
| DW600 |24 |24 |1 |4 |8 |16 |
| DW1000 |32 |40 |1 |8 |16 |32 |
| DW1200 |32 |48 |1 |8 |16 |32 |
| DW1500 |32 |60 |1 |8 |16 |32 |
| DW2000 |32 |80 |1 |16 |32 |64 |
| DW3000 |32 |120 |1 |16 |32 |64 |
| DW6000 |32 |240 |1 |32 |64 |128 |

In dieser Tabelle sehen Sie, dass ein als DW1000 ausgeführtes SQL Data Warehouse maximal 32 gleichzeitige Abfragen und insgesamt 40 Parallelitätsslots zuordnet. Wenn alle Benutzer Abfragen in „smallrc“ ausführen, sind 32 gleichzeitige Abfragen zulässig, da jede Abfrage 1 Parallelitätsslot belegt. Wenn alle Benutzer in einem DW1000 Abfragen in „mediumrc“ ausführen, werden jeder Abfrage 800 MB pro Verteilung zugewiesen, was eine Gesamtzuweisung an Arbeitsspeicher von 47 GB ergibt. Die Parallelität ist in diesem Fall auf 5 Benutzer begrenzt (40 Parallelitätsslots / 8 Slots pro mediumrc-Benutzer).

## <a name="query-importance"></a>Wichtigkeit von Abfragen
SQL Data Warehouse implementiert Ressourcenklassen anhand von Workloadgruppen. Es gibt insgesamt acht Workloadgruppen, die das Verhalten der Ressourcenklassen über die verschiedenen DWU-Größen hinweg steuern. In jeder DWU verwendet SQL Data Warehouse nur vier der acht Workloadgruppen. Dies ergibt Sinn, da jede Workloadgruppe einer der vier Ressourcenklassen „smallrc“, „mediumrc“, „largerc“ oder „xlargerc“ zugewiesen ist. Sie müssen diese Workloadgruppen verstehen, da für einige dieser Workloadgruppen eine höhere *Wichtigkeit*festgelegt ist. Die Wichtigkeit wird für die CPU-Zeitplanung verwendet. Abfragen, die mit hoher Wichtigkeit ausgeführt werden, erhalten dreimal mehr CPU-Zyklen als Abfragen mit mittlerer Wichtigkeit. Daher wird anhand von Parallelitätsslotzuordnungen auch die CPU-Priorität bestimmt. Wenn für eine Abfrage 16 oder mehr Slots verwendet werden, ist die Wichtigkeit hoch.

In der folgenden Tabelle sind die Wichtigkeitszuordnungen für die einzelnen Workloadgruppen angegeben.

### <a name="workload-group-mappings-to-concurrency-slots-and-importance"></a>Zuordnungen von Workloadgruppen zu Parallelitätsslots und Wichtigkeit
| Workloadgruppen | Zuordnung von Parallelitätsslots | MB/Verteilung | Wichtigkeitszuordnung |
|:--- |:---:|:---:|:--- |
| SloDWGroupC00 |1 |100 |Mittel |
| SloDWGroupC01 |2 |200 |Mittel |
| SloDWGroupC02 |4 |400 |Mittel |
| SloDWGroupC03 |8 |800 |Mittel |
| SloDWGroupC04 |16 |1.600 |Hoch |
| SloDWGroupC05 |32 |3.200 |Hoch |
| SloDWGroupC06 |64 |6.400 |Hoch |
| SloDWGroupC07 |128 |12.800 |Hoch |

In der Tabelle **Zuordnung und Verbrauch von Parallelitätsslots** sehen Sie, dass ein DW500 jeweils 1, 4, 8 bzw. 16 Parallelitätsslots für smallrc-, mediumrc-, largerc- und xlargerc-Ressourcenklassen verwendet. Schlagen Sie diese Werte in der vorherigen Tabelle nach, um die Wichtigkeit für jede Ressourcenklasse zu ermitteln.

### <a name="dw500-mapping-of-resource-classes-to-importance"></a>DW500-Zuordnung zwischen Ressourcenklassen und Wichtigkeit
| Ressourcenklasse | Workloadgruppe | Verwendete Parallelitätsslots | MB/Verteilung | Wichtigkeit |
|:--- |:--- |:---:|:---:|:--- |
| smallrc |SloDWGroupC00 |1 |100 |Mittel |
| mediumrc |SloDWGroupC02 |4 |400 |Mittel |
| largerc |SloDWGroupC03 |8 |800 |Mittel |
| xlargerc |SloDWGroupC04 |16 |1.600 |Hoch |

Sie können die folgende DMV-Abfrage verwenden, um die Unterschiede bei der Arbeitsspeicherressourcen-Zuweisung detailliert aus Sicht des Resource Governors anzuzeigen. Außerdem können Sie die derzeitige und zurückliegende Nutzung der Workloadgruppen bei der Problembehandlung analysieren.

```sql
WITH rg
AS
(   SELECT  
     pn.name                        AS node_name
    ,pn.[type]                        AS node_type
    ,pn.pdw_node_id                    AS node_id
    ,rp.name                        AS pool_name
    ,rp.max_memory_kb*1.0/1024                AS pool_max_mem_MB
    ,wg.name                        AS group_name
    ,wg.importance                    AS group_importance
    ,wg.request_max_memory_grant_percent        AS group_request_max_memory_grant_pcnt
    ,wg.max_dop                        AS group_max_dop
    ,wg.effective_max_dop                AS group_effective_max_dop
    ,wg.total_request_count                AS group_total_request_count
    ,wg.total_queued_request_count            AS group_total_queued_request_count
    ,wg.active_request_count                AS group_active_request_count
    ,wg.queued_request_count                AS group_queued_request_count
    FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
    JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    
            ON  wg.pdw_node_id  = rp.pdw_node_id
            AND wg.pool_id      = rp.pool_id
    JOIN    sys.dm_pdw_nodes pn
            ON    wg.pdw_node_id    = pn.pdw_node_id
    WHERE   wg.name like 'SloDWGroup%'
        AND     rp.name = 'SloDWPool'
)
SELECT    pool_name
,        pool_max_mem_MB
,        group_name
,        group_importance
,        (pool_max_mem_MB/100)*group_request_max_memory_grant_pcnt AS max_memory_grant_MB
,        node_name
,        node_type
,       group_total_request_count
,       group_total_queued_request_count
,       group_active_request_count
,       group_queued_request_count
FROM    rg
ORDER BY
    node_name
,    group_request_max_memory_grant_pcnt
,    group_importance
;
```

## <a name="queries-that-honor-concurrency-limits"></a>Abfragen mit Berücksichtigung von Parallelitätslimits
Die meisten Abfragen werden über Ressourcenklassen gesteuert. Diese Abfragen müssen die Schwellenwerte sowohl für gleichzeitige Abfragen als auch für Parallelität einhalten. Ein Benutzer kann eine Abfrage nicht aus dem Parallelitätsslotmodell ausschließen.

Um es noch einmal zu wiederholen: Die folgenden Anweisungen berücksichtigen Ressourcenklassen:

* INSERT-SELECT
* UPDATE
* DELETE
* SELECT (wenn Benutzertabellen abgefragt werden)
* ALTER INDEX REBUILD
* ALTER INDEX REORGANIZE
* ALTER TABLE REBUILD
* CREATE INDEX
* CREATE CLUSTERED COLUMNSTORE INDEX
* CREATE TABLE AS SELECT (CTAS)
* Laden von Daten
* Datenverschiebungsvorgänge, die vom DMS (Data Movement Service) ausgeführt werden

## <a name="query-exceptions-to-concurrency-limits"></a>Ausnahmen von Parallelitätslimits für Abfragen
Einige Abfragen berücksichtigen die Ressourcenklasse nicht, der der Benutzer zugeordnet ist. Diese Ausnahmen von den Parallelitätslimits werden zugelassen, wenn für einen bestimmten Befehl nicht viele Arbeitsspeicherressourcen zur Verfügung stehen. Dies ist häufig der Fall, wenn es sich bei dem Befehl um einen Metadatenvorgang handelt. Ziel dieser Ausnahmen ist es, zu vermeiden, dass Abfragen eine größere Menge an Arbeitsspeicher zugewiesen wird, die diese niemals benötigen. In diesen Fällen wird immer die Standardressourcenklasse (die kleine Ressourcenklasse, „smallrc“) verwendet, unabhängig von der Ressourcenklasse, die dem Benutzer eigentlich zugewiesen ist. Beispielsweise wird `CREATE LOGIN` immer in „smallrc“ ausgeführt. Da zum Ausführen dieses Vorgangs nur sehr wenig Ressourcen erforderlich sind, ist es nicht sinnvoll, die Abfrage in das Parallelitätsslotmodell aufzunehmen.  Diese Abfragen sind auch nicht durch das Parallelitätslimit von 32 Benutzern beschränkt. Eine unbegrenzte Anzahl dieser Abfragen kann bis zum Sitzungslimit von 1.024 Sitzungen ausgeführt werden.

Die folgenden Anweisungen berücksichtigen Ressourcenklassen nicht:

* CREATE oder DROP TABLE
* ALTER TABLE ... SWITCH, SPLIT oder MERGE PARTITION
* ALTER INDEX DISABLE
* DROP INDEX
* CREATE, UPDATE oder DROP STATISTICS
* TRUNCATE TABLE
* ALTER AUTHORIZATION
* CREATE LOGIN
* CREATE, ALTER oder DROP USER
* CREATE, ALTER oder DROP PROCEDURE
* CREATE oder DROP VIEW
* INSERT VALUES
* SELECT aus Systemsichten und DMVs
* EXPLAIN
* DBCC

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

## <a name="change-a-user-resource-class-example"></a>Beispiel: Ändern der Ressourcenklasse eines Benutzers
1. **Anmeldung erstellen**: Öffnen Sie eine Verbindung mit Ihrer **master**-Datenbank auf der SQL Server-Instanz, die Ihre SQL Data Warehouse-Datenbank hostet, und führen Sie die folgenden Befehle aus.
   
    ```sql
    CREATE LOGIN newperson WITH PASSWORD = 'mypassword';
    CREATE USER newperson for LOGIN newperson;
    ```
   
   > [!NOTE]
   > Es ist eine gute Idee, einen Benutzer in der Masterdatenbank für Azure SQL Data Warehouse-Benutzer zu erstellen. Das Erstellen eines Benutzers in der Masterdatenbank ermöglicht einem Benutzer, sich mit Tools wie SSMS ohne Angabe eines Datenbanknamens anzumelden.  Außerdem kann er mit dem Objekt-Explorer alle Datenbanken auf einer SQL Server-Instanz anzeigen.  Weitere Informationen zum Erstellen und Verwalten von Benutzern finden Sie unter [Sichern einer Datenbank in SQL Data Warehouse][Secure a database in SQL Data Warehouse].
   > 
   > 
2. **SQL Data Warehouse-Benutzer erstellen**: Öffnen Sie eine Verbindung mit der **SQL Data Warehouse**-Datenbank, und führen Sie den folgenden Befehl aus.
   
    ```sql
    CREATE USER newperson FOR LOGIN newperson;
    ```
3. **Berechtigungen erteilen**: Das folgende Beispiel gewährt die Berechtigung `CONTROL` für die **SQL Data Warehouse**-Datenbank. `CONTROL` auf Datenbankebene entspricht „db_owner“ in SQL Server.
   
    ```sql
    GRANT CONTROL ON DATABASE::MySQLDW to newperson;
    ```
4. **Ressourcenklasse heraufsetzen:** Verwenden Sie die folgende Abfrage, um einen Benutzer zu einer Rolle mit höheren Berechtigungen für die Workloadverwaltung hinzuzufügen.
   
    ```sql
    EXEC sp_addrolemember 'largerc', 'newperson'
    ```
5. **Ressourcenklasse herabsetzen:** Verwenden Sie die folgende Abfrage, um einen Benutzer aus einer Workloadverwaltungsrolle zu entfernen.
   
    ```sql
    EXEC sp_droprolemember 'largerc', 'newperson';
    ```
   
   > [!NOTE]
   > Es ist nicht möglich, einen Benutzer aus „smallrc“ zu entfernen.
   > 
   > 

## <a name="queued-query-detection-and-other-dmvs"></a>Erkennung von Abfragen in der Warteschlange und andere DMVs
Sie können die DMV `sys.dm_pdw_exec_requests` verwenden, um Abfragen zu identifizieren, die in eine Parallelitätswarteschlange eingereiht wurden. Abfragen, die auf einen Parallelitätsslot warten, weisen den Status **Angehalten**auf.

```sql
SELECT      r.[request_id]                 AS Request_ID
        ,r.[status]                 AS Request_Status
        ,r.[submit_time]             AS Request_SubmitTime
        ,r.[start_time]                 AS Request_StartTime
        ,DATEDIFF(ms,[submit_time],[start_time]) AS Request_InitiateDuration_ms
        ,r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r;
```

Workloadverwaltungsrollen können mit `sys.database_principals`angezeigt werden.

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0;
```

Mit der folgenden Abfrage wird angezeigt, welcher Rolle die einzelnen Benutzer zugewiesen sind.

```sql
SELECT     r.name AS role_principal_name
        ,m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id        = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id    = m.principal_id
WHERE    r.name IN ('mediumrc','largerc', 'xlargerc');
```

SQL Data Warehouse verfügt über die folgenden Wartetypen:

* **LocalQueriesConcurrencyResourceType**: Bezieht sich auf Abfragen, die sich außerhalb des Parallelitätsslot-Frameworks befinden. DMV-Abfragen und Systemfunktionen wie `SELECT @@VERSION` sind Beispiele für lokale Abfragen.
* **UserConcurrencyResourceType**: Bezieht sich auf Abfragen, die sich innerhalb des Parallelitätsslot-Frameworks befinden. Abfragen für Endbenutzertabellen sind ein Beispiel für die Verwendung dieses Ressourcentyps.
* **DmsConcurrencyResourceType**: Bezieht sich auf Wartezeiten, die sich aufgrund von Datenverschiebungen ergeben.
* **BackupConcurrencyResourceType**: Diese Wartezeit gibt an, dass eine Datenbank gesichert wird. Der maximale Wert für diesen Ressourcentyp ist 1. Wenn mehrere Sicherungen gleichzeitig angefordert wurden, werden die restlichen Sicherungen in die Warteschlange eingereiht.

Mit der DMV `sys.dm_pdw_waits` kann angezeigt werden, auf welche Ressourcen eine Abfrage wartet.

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]            AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,    SESSION_ID()            AS Current_session
,    s.[status]            AS Session_status
,    s.[login_name]
,    s.[query_count]
,    s.[client_id]
,    s.[sql_spid]
,    r.[command]            AS Request_command
,    r.[label]
,    r.[status]            AS Request_status
,    r.[submit_time]
,    r.[start_time]
,    r.[end_compile_time]
,    r.[end_time]
,    DATEDIFF(ms,r.[submit_time],r.[start_time])        AS Request_queue_time_ms
,    DATEDIFF(ms,r.[start_time],r.[end_compile_time])    AS Request_compile_time_ms
,    DATEDIFF(ms,r.[end_compile_time],r.[end_time])        AS Request_execution_time_ms
,    r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE    w.[session_id] <> SESSION_ID();
```

Mit der DMV `sys.dm_pdw_resource_waits` werden nur die Ressourcenwartezeiten einer bestimmten Abfrage angezeigt. Mit der Ressourcenwartezeit wird nur gemessen, wie lange auf die Bereitstellung von Ressourcen gewartet wird. Bei der Signalwartezeit (Signal Wait Time) wird dagegen gemessen, wie lange die zugrunde liegenden SQL Server-Instanzen zum Planen der Abfrage in der CPU benötigen.

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
WHERE    [session_id] <> SESSION_ID();
```

Die DMV `sys.dm_pdw_wait_stats` kann für Verlaufsanalysen von Wartezeiten verwendet werden.

```sql
SELECT    w.[pdw_node_id]
,        w.[wait_name]
,        w.[max_wait_time]
,        w.[request_count]
,        w.[signal_time]
,        w.[completed_count]
,        w.[wait_time]
FROM    sys.dm_pdw_wait_stats w;
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Verwalten von Datenbankbenutzern und der Sicherheit finden Sie unter [Sichern einer Datenbank in SQL Data Warehouse][Secure a database in SQL Data Warehouse]. Weitere Informationen dazu, wie größere Ressourcenklassen die Qualität von gruppierten Columnstore-Indizes verbessern können, finden Sie unter [Neuerstellen von Indizes zur Verbesserung der Segmentqualität].

<!--Image references-->

<!--Article references-->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Neuerstellen von Indizes zur Verbesserung der Segmentqualität]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->

