---
title: Migrieren von SQL-Code nach SQL Data Warehouse | Microsoft Docs
description: "Tipps für die Migration des SQL-Codes in Azure SQL Data Warehouse zum Entwickeln von Lösungen."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 19c252a3-0e41-4eec-9d3e-09a68c7e7add
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 01/30/2017
ms.author: joeyong;barbkess
ms.translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 3fd5224983c723faefb8001888ae20e78acdb8ce
ms.contentlocale: de-de
ms.lasthandoff: 04/03/2017


---
# <a name="migrate-your-sql-code-to-sql-data-warehouse"></a>Migrieren von SQL-Code nach SQL Data Warehouse
Wenn Sie den Ihren Code von einer anderen Datenbank zu SQL Data Warehouse migrieren, müssen Sie höchstwahrscheinlich Änderungen an der Codebasis vornehmen. Mit einigen SQL Data Warehouse-Funktionen kann die Leistung erheblich verbessert werden, da sie speziell für eine verteilte Ausführung entwickelt wurden. Um jedoch eine gleichbleibende Leistung und Skalierung zu gewährleisten, sind einige Funktionen auch nicht verfügbar.

## <a name="common-t-sql-limitations"></a>Allgemeine T-SQL-Einschränkungen
Die folgende Aufstellung enthält die wichtigsten Funktionen, die in Azure SQL Data Warehouse nicht unterstützt werden. Über die Links gelangen Sie zu Problemumgehungen für die nicht unterstützten Funktionen:

* [ANSI-Joins bei Aktualisierungen][ANSI joins on updates]
* [ANSI-Joins bei Löschvorgängen][ANSI joins on deletes]
* [MERGE-Anweisung][merge statement]
* Datenbankübergreifende Verknüpfungen
* [Cursor][cursors]
* <seg>
  [INSERT..EXEC][INSERT..EXEC]</seg>
* OUTPUT-Klausel
* Benutzerdefinierte Inlinefunktionen
* Funktionen mit mehreren Anweisungen
* [Allgemeine Tabellenausdrücke](#Common-table-expressions)
* [Rekursive allgemeine Tabellenausdrücke (CTE)] (#Recursive-common-table-expressions-(CTE)
* CLR-Funktionen und -Prozeduren
* $partition-Funktion
* Tabellenvariablen
* Parameter für Tabellenwerte
* Verteilte Transaktionen
* Commit- oder Rollback-Vorgänge
* SAVE TRANSACTION
* Ausführungskontexte (EXECUTE AS)
* [GROUP BY-Klausel mit ROLLUP-, CUBE- oder GROUPING SETS-Option][group by clause with rollup / cube / grouping sets options]
* [Über 8 Schachtelungsebenen][nesting levels beyond 8]
* [Aktualisieren über Sichten][updating through views]
* [Verwenden der SELECT-Anweisung zur Variablenzuweisung][use of select for variable assignment]
* [Kein MAX-Datentyp für dynamische SQL-Zeichenfolgen][no MAX data type for dynamic SQL strings]

Glücklicherweise können die meisten dieser Einschränkungen umgangen werden. Die entsprechenden Erläuterungen finden Sie in den jeweiligen oben referenzierten Artikeln.

## <a name="supported-cte-features"></a>Unterstützte CTE-Funktionen
Allgemeine Tabellenausdrücke (CTEs) werden in SQL Data Warehouse teilweise unterstützt.  Die folgenden CTE-Funktionen werden derzeit unterstützt:

* Ein allgemeiner Tabellenausdruck kann in einer SELECT-Anweisung angegeben werden.
* Ein allgemeiner Tabellenausdruck kann in einer CREATE VIEW-Anweisung angegeben werden.
* Ein allgemeiner Tabellenausdruck kann in einer CREATE TABLE AS SELECT (CTAS)-Anweisung angegeben werden.
* Ein allgemeiner Tabellenausdruck kann in einer CREATE REMOTE TABLE AS SELECT (CRTAS)-Anweisung angegeben werden.
* Ein allgemeiner Tabellenausdruck kann in einer CREATE EXTERNAL TABLE AS SELECT (CETAS)-Anweisung angegeben werden.
* Eine Remotetabelle kann über einen allgemeinen Tabellenausdruck referenziert werden.
* Eine externe Tabelle kann über einen allgemeinen Tabellenausdruck referenziert werden.
* In einem allgemeinen Tabellenausdruck können mehrere Abfragedefinitionen definiert werden.

## <a name="cte-limitations"></a>Einschränkungen bei allgemeinen Tabellenausdrücken (CTE)
Allgemeine Tabellenausdrücke weisen einige der folgenden Einschränkungen in SQL Data Warehouse auf:

* Auf einen allgemeinen Tabellenausdruck muss eine einzelne SELECT-Anweisung folgen. INSERT-, UPDATE-, DELETE- und MERGE-Anweisungen werden nicht unterstützt.
* Ein allgemeiner Tabellenausdruck, der Verweise auf sich selbst (einen rekursiven allgemeinen Tabellenausdrucks) enthält, wird nicht unterstützt (siehe nachfolgenden Abschnitt).
* Es ist maximal eine WITH-Klausel in einem allgemeinen Tabellenausdruck zulässig. Beispiel: Wenn eine Abfragedefinition für einen allgemeinen Tabellenausdruck eine Unterabfrage enthält, kann nicht diese Unterabfrage keine geschachtelte WITH-Klausel enthalten, die einen anderen allgemeinen Tabellenausdruck definiert.
* Eine ORDER BY-Klausel kann in der Abfragedefinition für einen allgemeinen Tabellenausdruck nur verwendet werden, wenn eine TOP-Klausel angegeben wurde.
* Wenn ein allgemeiner Tabellenausdruck in einer Anweisung verwendet wird, die Teil einer Batchinstanz ist, muss der Anweisung davor ein Semikolon folgen.
* Bei Verwendung in Anweisungen, die mit sp_prepare vorbereitet wurden, verhalten sich allgemeine Tabellenausdrücke genauso, wie andere SELECT-Anweisungen in PDW. Wenn CTEs jedoch als Teil von CETAS verwendet werden, die mit „sp_prepare“ vorbereitet werden, kann das Verhalten von SQL Server- und anderen PDW-Anweisungen aufgrund der Art, wie die Bindung für „sp_prepare“ implementiert wird, abweichen. Falls die das CTE referenzierende SELECT-Anweisung eine falsche Spalte verwendet, die im CTE nicht vorhanden ist, wird „sp_prepare“ übergeben, ohne dass der Fehler erkannt wird. Der Fehler wird dann aber während „sp_execute“ ausgegeben.

## <a name="recursive-ctes"></a>Rekursive CTEs
Rekursive CTEs werden in SQL Data Warehouse nicht unterstützt.  Die Migration rekursiver CTEs kann mehr oder weniger komplex sein, und die beste Vorgehensweise besteht darin, diese in mehrere Schritte aufzuteilen. In der Regel können Sie eine Schleife verwenden und eine temporäre Tabelle auffüllen, während Sie die rekursiven Zwischenabfragen durchlaufen. Sobald die temporäre Tabelle aufgefüllt ist, können Sie die Daten als ein einzelnes Resultset zurückgeben. Ein ähnlicher Ansatz wurde als Lösung für `GROUP BY WITH CUBE` im Artikel zur [GROUP BY-Klausel mit ROLLUP-, CUBE- oder GROUPING SETS-Option][group by clause with rollup / cube / grouping sets options] verwendet.

## <a name="unsupported-system-functions"></a>Nicht unterstützte Systemfunktionen
Es werden auch einige Systemfunktionen nicht unterstützt. Zu den wichtigsten Funktionen, die normalerweise in Data Warehousing verwendet, gehören u. a.:

* NEWSEQUENTIALID()
* @@NESTLEVEL()
* @@IDENTITY()
* @@ROWCOUNT()
* ROWCOUNT_BIG
* ERROR_LINE()

Einige dieser Probleme können umgangen werden.

## <a name="rowcount-workaround"></a>@@ROWCOUNT – Problemumgehung
Um die fehlende Unterstützung für @@ROWCOUNT zu umgehen, erstellen Sie eine gespeicherte Prozedur, die die letzte Zeilenanzahl aus „sys.dm_pdw_request_steps“ abruft, und führen Sie dann `EXEC LastRowCount` nach einer DML-Anweisung aus.

```sql
CREATE PROCEDURE LastRowCount AS
WITH LastRequest as 
(   SELECT TOP 1    request_id
    FROM            sys.dm_pdw_exec_requests
    WHERE           session_id = SESSION_ID()
    AND             resource_class IS NOT NULL
    ORDER BY end_time DESC
),
LastRequestRowCounts as
(
    SELECT  step_index, row_count
    FROM    sys.dm_pdw_request_steps
    WHERE   row_count >= 0
    AND     request_id IN (SELECT request_id from LastRequest)
)
SELECT TOP 1 row_count FROM LastRequestRowCounts ORDER BY step_index DESC
;
```

## <a name="next-steps"></a>Nächste Schritte
Eine vollständige Liste aller unterstützten T-SQL-Anweisungen finden Sie in den [Themen zu Transact-SQL][Transact-SQL topics].

<!--Image references-->

<!--Article references-->
[ANSI joins on updates]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[ANSI joins on deletes]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[merge statement]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[INSERT..EXEC]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[Transact-SQL topics]: ./sql-data-warehouse-reference-tsql-statements.md

[cursors]: ./sql-data-warehouse-develop-loops.md
[group by clause with rollup / cube / grouping sets options]: ./sql-data-warehouse-develop-group-by-options.md
[nesting levels beyond 8]: ./sql-data-warehouse-develop-transactions.md
[updating through views]: ./sql-data-warehouse-develop-views.md
[use of select for variable assignment]: ./sql-data-warehouse-develop-variable-assignment.md
[no MAX data type for dynamic SQL strings]: ./sql-data-warehouse-develop-dynamic-sql.md

<!--MSDN references-->

<!--Other Web references-->

