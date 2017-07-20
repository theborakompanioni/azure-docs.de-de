---
title: "Leitfaden für Datentypen – Azure SQL Data Warehouse | Microsoft-Dokumentation"
description: Empfehlungen zum Definieren von Datentypen, die mit SQL Data Warehouse kompatibel sind.
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: barbkess
editor: 
ms.assetid: d4a1f0a3-ba9f-44b9-95f6-16a4f30746d6
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 06/02/2017
ms.author: shigu;barbkess
ms.translationtype: Human Translation
ms.sourcegitcommit: 532ff423ff53567b6ce40c0ea7ec09a689cee1e7
ms.openlocfilehash: 5c24c71af16bd9851d9caf15fecfa4bb76f5f77e
ms.contentlocale: de-de
ms.lasthandoff: 06/05/2017


---
# <a name="guidance-for-defining-data-types-for-tables-in-sql-data-warehouse"></a>Leitfaden zum Definieren von Datentypen für Tabellen in SQL Data Warehouse
Verwenden Sie diese Empfehlungen zum Definieren von Datentypen für die Tabelle, die mit SQL Data Warehouse kompatibel sind. Neben der Kompatibilität wird durch die Größe der Datentypen die Abfrageleistung verbessert.

SQL Data Warehouse unterstützt die gängigsten Datentypen. Eine Liste der unterstützten Datentypen finden Sie in der CREATE TABLE-Anweisung unter [Datentypen](/sql/docs/t-sql/statements/create-table-azure-sql-data-warehouse.md#datatypes). 


## <a name="minimize-row-length"></a>Minimieren der Zeilenlänge
Durch das Minimieren der Größe von Datentypen wird die Zeilenlänge verkürzt. Dies führt zu einer besseren Abfrageleistung. Verwenden Sie den kleinsten Datentyp, der für Ihre Daten funktioniert. 

- Vermeiden Sie es, für Zeichenspalten eine hohe Standardlänge zu definieren. Wenn der längste Wert 25 Zeichen umfasst, sollten Sie die Spalte VARCHAR(25) definieren. 
- Vermeiden Sie die Verwendung von [NVARCHAR][NVARCHAR], wenn Sie nur VARCHAR benötigen.
- Verwenden Sie anstelle von NVARCHAR(MAX) oder VARCHAR(MAX) nach Möglichkeit NVARCHAR(4000) oder VARCHAR(8000).

Wenn Sie die Tabellen mit Polybase laden, darf die definierte Länge der Tabellenzeile 1 MB nicht überschreiten. Wenn eine Zeile mit Daten variabler Länge 1 MB überschreitet, können Sie die Zeile mit BCP, jedoch nicht mit PolyBase laden.

## <a name="identify-unsupported-data-types"></a>Identifizieren nicht unterstützter Datentypen
Wenn Sie die Datenbank aus einer anderen SQL­Datenbank migrieren,treten ggf. Datentypen auf, die in SQL Data Warehouse nicht unterstützt werden. Verwenden Sie diese Abfrage, um nicht unterstützte Datentypen in Ihrem vorhandenen SQL-Schema zu ermitteln.

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','timestamp','xml')
 AND  y.[is_user_defined] = 1;
```


## <a name="unsupported-data-types"></a>Verwenden von Problemumgehungen für nicht unterstützte Datentypen

Die folgende Liste zeigt die Datentypen, die nicht von SQL Data Warehouse unterstützt werden, und bietet Alternativen, die Sie anstelle der nicht unterstützten Datentypen verwenden können.

| Nicht unterstützte Datentypen | Problemumgehung |
| --- | --- |
| [geometry][geometry] |[varbinary][varbinary] |
| [geography][geography] |[varbinary][varbinary] |
| [hierarchyid][hierarchyid] |[nvarchar][nvarchar](4000) |
| [image][ntext,text,image] |[varbinary][varbinary] |
| [text][ntext,text,image] |[varchar][varchar] |
| [ntext][ntext,text,image] |[nvarchar][nvarchar] |
| [sql_variant][sql_variant] |Spalte in mehrere Spalten mit starker Typisierung unterteilen. |
| [table][table] |In temporäre Tabellen konvertieren. |
| [timestamp][timestamp] |Passen Sie den Code an, damit [datetime2][datetime2] und die `CURRENT_TIMESTAMP`-Funktion verwendet werden.  Nur Konstanten werden als Standardwerte unterstützt. Daher kann „current_timestamp“ nicht als Standardeinschränkung definiert werden. Wenn Sie Zeilenversionswerte aus einer Spalte vom Typ timestamp migrieren müssen, sollten Sie [BINARY][BINARY](8) oder [VARBINARY][BINARY](8) für NOT NULL- oder NULL-Zeilenversionswerte verwenden. |
| [xml][xml] |[varchar][varchar] |
| [Benutzerdefinierter Typ][user defined types] |Führen Sie, wenn möglich, eine Rückkonvertierung in den nativen Datentyp durch. |
| Standardwerte | Standardwerte unterstützen nur Literale und Konstanten.  Nicht deterministische Ausdrücke oder Funktionen, z. B. `GETDATE()` oder `CURRENT_TIMESTAMP`, werden nicht unterstützt. |


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter:

- [Bewährte Methoden für SQL Data Warehouse][SQL Data Warehouse Best Practices]
- [Tabellenübersicht][Overview]
- [Verteilen einer Tabelle][Distribute]
- [Indizieren einer Tabelle][Index]
- [Partitionieren einer Tabelle][Partition]
- [Verwalten von Tabellenstatistiken][Statistics]
- [Temporäre Tabellen][Temporary]

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->

<!--Other Web references-->
[create table]: https://msdn.microsoft.com/library/mt203953.aspx
[bigint]: https://msdn.microsoft.com/library/ms187745.aspx
[binary]: https://msdn.microsoft.com/library/ms188362.aspx
[bit]: https://msdn.microsoft.com/library/ms177603.aspx
[char]: https://msdn.microsoft.com/library/ms176089.aspx
[date]: https://msdn.microsoft.com/library/bb630352.aspx
[datetime]: https://msdn.microsoft.com/library/ms187819.aspx
[datetime2]: https://msdn.microsoft.com/library/bb677335.aspx
[datetimeoffset]: https://msdn.microsoft.com/library/bb630289.aspx
[decimal]: https://msdn.microsoft.com/library/ms187746.aspx
[float]: https://msdn.microsoft.com/library/ms173773.aspx
[geometry]: https://msdn.microsoft.com/library/cc280487.aspx
[geography]: https://msdn.microsoft.com/library/cc280766.aspx
[hierarchyid]: https://msdn.microsoft.com/library/bb677290.aspx
[int]: https://msdn.microsoft.com/library/ms187745.aspx
[money]: https://msdn.microsoft.com/library/ms179882.aspx
[nchar]: https://msdn.microsoft.com/library/ms186939.aspx
[nvarchar]: https://msdn.microsoft.com/library/ms186939.aspx
[ntext,text,image]: https://msdn.microsoft.com/library/ms187993.aspx
[real]: https://msdn.microsoft.com/library/ms173773.aspx
[smalldatetime]: https://msdn.microsoft.com/library/ms182418.aspx
[smallint]: https://msdn.microsoft.com/library/ms187745.aspx
[smallmoney]: https://msdn.microsoft.com/library/ms179882.aspx
[sql_variant]: https://msdn.microsoft.com/library/ms173829.aspx
[sysname]: https://msdn.microsoft.com/library/ms186939.aspx
[table]: https://msdn.microsoft.com/library/ms175010.aspx
[time]: https://msdn.microsoft.com/library/bb677243.aspx
[timestamp]: https://msdn.microsoft.com/library/ms182776.aspx
[tinyint]: https://msdn.microsoft.com/library/ms187745.aspx
[uniqueidentifier]: https://msdn.microsoft.com/library/ms187942.aspx
[varbinary]: https://msdn.microsoft.com/library/ms188362.aspx
[varchar]: https://msdn.microsoft.com/library/ms186939.aspx
[xml]: https://msdn.microsoft.com/library/ms187339.aspx
[user defined types]: https://msdn.microsoft.com/library/ms131694.aspx

