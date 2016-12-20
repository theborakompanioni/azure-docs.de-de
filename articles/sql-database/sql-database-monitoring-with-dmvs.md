---
title: "Überwachen der Azure SQL-Datenbank mit dynamischen Verwaltungssichten | Microsoft Docs"
description: "Erfahren Sie, wie Sie allgemeine Leistungsprobleme mithilfe der dynamischen Verwaltungssichten zum Überwachen von Microsoft Azure SQL-Datenbank ermitteln und diagnostizieren."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: d08f505f-3c62-47d4-bab7-35c9a834b79b
ms.service: sql-database
ms.custom: monitor and tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 09/20/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: ae0054ac9d87562f6babbfaeaf440d653d60963a


---
# <a name="monitoring-azure-sql-database-using-dynamic-management-views"></a>Überwachen der Azure SQL-Datenbank mit dynamischen Verwaltungssichten
Die Microsoft Azure SQL-Datenbank unterstützt eine Teilmenge dynamischer Verwaltungssichten für die Diagnose von Leistungsproblemen, die auf blockierte Abfragen oder Abfragen mit langen Laufzeiten, fehlerhafte Abfragepläne usw. zurückzuführen sind. Dieses Thema enthält Informationen zum Erkennen häufiger Leistungsprobleme mithilfe von dynamischen Verwaltungssichten.

Die SQL-Datenbank unterstützt teilweise drei Kategorien von dynamischen Verwaltungssichten:

* Dynamische Verwaltungssichten für die Datenbank
* Dynamische Verwaltungssichten für die Ausführung
* Dynamische Verwaltungssichten für Transaktionen

Ausführliche Informationen zu dynamischen Verwaltungssichten finden Sie unter [Dynamische Verwaltungssichten und -funktionen (Transact-SQL)](https://msdn.microsoft.com/library/ms188754.aspx) in der SQL Server-Onlinedokumentation.

## <a name="permissions"></a>Berechtigungen
Die Abfrage einer dynamischen Verwaltungsansicht erfordert in der SQL-Datenbank die Berechtigung **VIEW DATABASE STATE** . Die Berechtigung **VIEW DATABASE STATE** gibt Informationen zu allen Objekten innerhalb der aktuellen Datenbank zurück.
Zum Erteilen der Berechtigung **VIEW DATABASE STATE** für einen bestimmten Benutzer führen Sie die folgende Abfrage aus:

```GRANT VIEW DATABASE STATE TO database_user; ```

Bei einer lokalen Instanz von SQL Server geben dynamische Verwaltungssichten Informationen zum Serverstatus zurück. In SQL-Datenbank geben sie lediglich Informationen zur aktuellen logischen Datenbank zurück.

## <a name="calculating-database-size"></a>Berechnen der Datenbankgröße
Die folgende Abfrage gibt die Größe Ihrer Datenbank (in Megabyte) zurück:

```
-- Calculates the size of the database.
SELECT SUM(reserved_page_count)*8.0/1024
FROM sys.dm_db_partition_stats;
GO
```

Die folgende Abfrage gibt die Größe der einzelnen Objekte in Ihrer Datenbank in Megabyte zurück:

```
-- Calculates the size of individual database objects.
SELECT sys.objects.name, SUM(reserved_page_count) * 8.0 / 1024
FROM sys.dm_db_partition_stats, sys.objects
WHERE sys.dm_db_partition_stats.object_id = sys.objects.object_id
GROUP BY sys.objects.name;
GO
```

## <a name="monitoring-connections"></a>Überwachen von Verbindungen
Sie können die Sicht [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) zum Abrufen von Informationen über die Verbindungen, die mit einem bestimmten Azure SQL-Datenbank-Server hergestellt wurden, sowie der Details zu den einzelnen Verbindungen verwenden. Darüber hinaus ist die Sicht [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) für das Abrufen von Informationen zu allen aktiven Benutzerverbindungen und internen Aufgaben nützlich.
Verwenden Sie die folgende Abfrage zum Abrufen der Informationen zur aktuellen Verbindung:

```
SELECT
    c.session_id, c.net_transport, c.encrypt_option,
    c.auth_scheme, s.host_name, s.program_name,
    s.client_interface_name, s.login_name, s.nt_domain,
    s.nt_user_name, s.original_login_name, c.connect_time,
    s.login_time
FROM sys.dm_exec_connections AS c
JOIN sys.dm_exec_sessions AS s
    ON c.session_id = s.session_id
WHERE c.session_id = @@SPID;
```

> [!NOTE]
> Wenn Sie **sys.dm_exec_requests** und **sys.dm_exec_sessions views** ausführen und die Berechtigung **VIEW DATABASE STATE** für die Datenbank besitzen, werden Ihnen alle zurzeit ausgeführten Sitzungen in der Datenbank angezeigt. Andernfalls wird Ihnen nur die aktuelle Sitzung angezeigt.
> 
> 

## <a name="monitoring-query-performance"></a>Überwachen der Abfrageleistung
Langsame Abfragen oder Abfragen mit langen Ausführungszeiten können beträchtliche Systemressourcen beanspruchen. In diesem Abschnitt wird veranschaulicht, wie mit dynamischen Verwaltungssichten häufig auftretende Leistungsprobleme bei Abfragen ermittelt werden können. Eine frühere, aber immer noch nützliche Referenz zur Problembehandlung ist der Microsoft TechNet-Artikel [Behandlung von Leistungsproblemen in SQL Server 2008](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) .

### <a name="finding-top-n-queries"></a>Suchen der ersten N Abfragen
Das folgende Beispiel gibt Informationen über die „Top-Fünf“-Abfragen gemessen an durchschnittlicher CPU-Zeit zurück. In diesem Beispiel werden die Abfragen entsprechend ihrem Abfragenhash zusammengefasst, sodass logisch äquivalente Abfragen nach ihrem kumulativen Ressourcenverbrauch gruppiert werden.

```
SELECT TOP 5 query_stats.query_hash AS "Query Hash",
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",
    MIN(query_stats.statement_text) AS "Statement Text"
FROM
    (SELECT QS.*,
    SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
    ((CASE statement_end_offset
        WHEN -1 THEN DATALENGTH(ST.text)
        ELSE QS.statement_end_offset END
            - QS.statement_start_offset)/2) + 1) AS statement_text
     FROM sys.dm_exec_query_stats AS QS
     CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
GROUP BY query_stats.query_hash
ORDER BY 2 DESC;
```

### <a name="monitoring-blocked-queries"></a>Überwachen blockierter Abfragen
Langsame Abfragen oder Abfragen mit langer Laufzeit können zu einer übermäßigen Ressourcennutzung beitragen und auf blockierte Abfragen zurückzuführen sein. Ursache für das Blockieren kann ein mangelhafter Anwendungsentwurf, fehlerhafte Abfragepläne oder ein Mangel an nützlichen Indizes usw. sein. Verwenden Sie die sys.dm_tran_locks-Sichten, um Informationen über die aktuellen Sperraktivitäten in der Azure SQL-Datenbank abzurufen. Beispielcode finden Sie unter [sys.dm_tran_locks (Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) in der SQL Server-Onlinedokumentation.

### <a name="monitoring-query-plans"></a>Überwachen von Abfrageplänen
Auch ein ineffizienter Abfrageplan kann die CPU-Auslastung erhöhen. Im folgenden Beispiel wird die Sicht [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) verwendet, um festzustellen, welche Abfrage die CPU kumulativ am meisten auslastet.

```
SELECT
    highest_cpu_queries.plan_handle,
    highest_cpu_queries.total_worker_time,
    q.dbid,
    q.objectid,
    q.number,
    q.encrypted,
    q.[text]
FROM
    (SELECT TOP 50
        qs.plan_handle,
        qs.total_worker_time
    FROM
        sys.dm_exec_query_stats qs
    ORDER BY qs.total_worker_time desc) AS highest_cpu_queries
    CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS q
ORDER BY highest_cpu_queries.total_worker_time DESC;
```

## <a name="see-also"></a>Siehe auch
[Einführung in SQL-Datenbank](sql-database-technical-overview.md)




<!--HONumber=Nov16_HO3-->


