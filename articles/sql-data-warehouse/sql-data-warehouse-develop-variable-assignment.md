---
title: Zuweisen von Variablen in SQL Data Warehouse | Microsoft Docs
description: "Tipps zum Zuweisen von Transact-SQL-Variablen in Azure SQL Data Warehouse für die Entwicklung von Lösungen."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 81ddc7cf-a6ba-4585-91a3-b6ea50f49227
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 045d5148cd3f12dac63c961ccf7c953d355ed725
ms.lasthandoff: 04/03/2017


---
# <a name="assign-variables-in-sql-data-warehouse"></a>Zuweisen von Variablen in SQL Data Warehouse
Variablen werden in SQL Data Warehouse mit der `DECLARE`-Anweisung oder der `SET`-Anweisung festgelegt.

Im Folgenden sind Möglichkeiten zum Festlegen eines Variablenwerts aufgeführt:

## <a name="setting-variables-with-declare"></a>Festlegen von Variablen mit DECLARE
Das Initialisieren von Variablen mit DECLARE ist eine der flexibelsten Möglichkeiten zum Festlegen eines Variablenwerts in SQL Data Warehouse.

```sql
DECLARE @v  int = 0
;
```

Sie können mit DECLARE auch mehrere Variablen gleichzeitig festlegen. Sie können `SELECT` und `UPDATE` nicht für Folgendes verwenden:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

Eine Variable kann nicht in der gleichen DECLARE-Anweisung initialisiert und verwendet werden. Zur Veranschaulichung: Im folgenden Beispiel, das **nicht** zulässig ist, wird @p1 in der gleichen DECLARE-Anweisung sowohl initialisiert als auch verwendet. Dies führt zu einem Fehler.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>Festlegen von Werten mit SET
SET ist eine sehr allgemeine Methode zum Festlegen einer einzelnen Variable.

Alle folgenden Beispiele sind gültige Möglichkeiten zum Festlegen einer Variable mit SET:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

Sie können mit SET nur jeweils eine einzige Variable festlegen. Wie oben zu sehen ist, sind zusammengesetzte Operatoren jedoch zulässig.

## <a name="limitations"></a>Einschränkungen
Sie können nicht SELECT oder UPDATE für die Zuweisung von Variablen verwenden.

## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->

