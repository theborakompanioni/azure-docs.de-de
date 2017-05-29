---
title: "Kombinieren von Rowsets mit U-SQL-Verknüpfungen in Azure Data Lake Analytics | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie für U-SQL Join-Operatoren zum Kombinieren von Rowsets in Azure Data Lake Analytics verwendet werden."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 57143396-ab86-47dd-b6f8-613ba28c28d2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/09/2017
ms.author: edmaca
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 5ffc7116812e9f18f972eac2daff7935b00891f1
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017


---
# <a name="combine-rowsets-with-u-sql-joins"></a>Kombinieren von Rowsets mit U-SQL-Verknüpfungen

U-SQL enthält häufig verwendete Verknüpfungsoperatoren, z. B. INNER JOIN, LEFT/RIGHT/FULL OUTER JOIN, SEMI JOIN. Damit können nicht nur Tabellen verknüpft werden, sondern auch beliebige Rowsets (auch aus Dateien erzeugte Rowsets).

Im folgenden Skript wird das ``@searchlog``-Element mit einem Ankündigungsaufrufprotokoll verknüpft, und die Ankündigungen für die Abfragezeichenfolge für ein bestimmtes Datum werden ausgegeben.

```
@adlog =
    EXTRACT UserId int,
            Ad string,
            Clicked int
    FROM "/Samples/Data/AdsLog.tsv"
    USING Extractors.Tsv();

@join =
    SELECT a.Ad, s.Query, s.Start AS Date
    FROM @adlog AS a JOIN <insert your DB name>.dbo.SearchLog1 AS s
                    ON a.UserId == s.UserId
    WHERE a.Clicked == 1;

OUTPUT @join   
    TO "/output/Searchlog-join.csv"
    USING Outputters.Csv();
```

U-SQL unterstützt nur die ANSI-kompatible Verknüpfungssyntax „Rowset1 JOIN Rowset2 ON predicate“. Die alte Syntax „FROM Rowset1, Rowset2 WHERE predicate“ wird _nicht_ unterstützt.
Das Prädikat in einem JOIN-Element muss eine Gleichheitsverknüpfung (kein Ausdruck) sein. Wenn Sie einen Ausdruck verwenden möchten, können Sie ihn der SELECT-Klausel eines vorherigen Rowsets hinzufügen. Wenn Sie einen anderen Vergleich durchführen möchten, können Sie ihn in die WHERE-Klausel verschieben.

### <a name="next-steps"></a>Nächste Schritte
* [Übersicht über Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [Verwenden von U-SQL-Funktionen für Azure Data Lake Analytics-Aufträge](data-lake-analytics-use-window-functions.md)


