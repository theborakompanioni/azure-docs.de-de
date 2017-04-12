---
title: Verwenden von Bezeichnungen zum Instrumentieren von Abfragen in SQL Data Warehouse | Microsoft Docs
description: "Tipps zum Verwenden von Bezeichnungen zum Instrumentieren von Abfragen in Azure SQL Data Warehouse für die Entwicklung von Lösungen"
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 44988de8-04c1-4fed-92be-e1935661a4e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 10/31/2016
ms.author: jrj;barbkess
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 9e75bbe528a427724a623305fbd45e2277e9d0af
ms.lasthandoff: 04/03/2017


---
# <a name="use-labels-to-instrument-queries-in-sql-data-warehouse"></a>Verwenden von Bezeichnungen zum Instrumentieren von Abfragen in SQL Data Warehouse
In SQL Data Warehouse wird das Konzept der „Abfragebezeichnungen“ unterstützt. Bevor wir darauf näher eingehen, sehen wir uns ein Beispiel an:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Mit dieser letzten Zeile wird die Abfrage mit der Zeichenfolge „My Query Label“ versehen. Dies ist besonders hilfreich, da die Bezeichnung über die DMVs abgefragt werden kann. So erhalten wir ein Verfahren zum Ermitteln von Problemabfragen und können während einer ETL-Ausführung außerdem den Status abfragen.

Eine gute Benennungskonvention ist hierbei wirklich hilfreich. Beispielsweise ist etwas wie „PROJEKT : PROZEDUR : ANWEISUNG : KOMMENTAR“ hilfreich, um die Abfrage im Code der Quellcodeverwaltung eindeutig identifizieren zu können.

Um nach der Bezeichnung zu suchen, können Sie die folgende Abfrage nutzen, bei der die dynamischen Verwaltungssichten verwendet werden:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> Es ist wichtig, dass Sie das Wort „label“ bei Abfragen in eckige Klammern oder doppelte Anführungszeichen setzen. „label“ ist ein reserviertes Wort, das zu einem Fehler führen kann, wenn es nicht entsprechend markiert wird.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->

