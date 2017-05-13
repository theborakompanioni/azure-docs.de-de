---
title: Erweitern von U-SQL-Skripts um R in Azure Data Lake Analytics | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie R-Code in U-SQL-Skripts ausführen."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: sukvg
editor: cgronlun
ms.assetid: c1c74e5e-3e4a-41ab-9e3f-e9085da1d315
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/01/2017
ms.author: saveenr
ms.translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: ea837a735404d11b087cd552d1fce64184e88e00
ms.contentlocale: de-de
ms.lasthandoff: 05/02/2017


---

# <a name="tutorial-get-started-with-extending-u-sql-with-r"></a>Tutorial: Erste Schritte beim Erweitern von U-SQL um R

Im folgenden Beispiel werden die grundlegenden Schritte für eine Bereitstellung von R-Code veranschaulicht:
* Verwenden Sie die REFERENCE ASSEMBLY-Anweisung, um R-Erweiterungen für das U-SQL-Skript zu aktivieren.
* Verwenden Sie den REDUCE-Vorgang, um die Eingabedaten für einen Schlüssel zu partitionieren.
* Die R-Erweiterungen für U-SQL enthalten einen integrierten Reducer (Extension.R.Reducer), mit dem R-Code auf jedem Scheitelpunkt ausgeführt wird, der dem Reducer zugewiesen ist. 
* Verwenden von dedizierten benannten Datenrahmen („inputFromUSQL“ und „outputToUSQL“), um Daten zwischen USQL und R zu übergeben. Die Namen von Eingabe- und Ausgabedatenrahmen-IDs sind festgelegt (das heißt, Benutzer können diese vordefinierten Namen von Eingabe- und Ausgabedatenrahmen-IDs nicht ändern).


--

    REFERENCE ASSEMBLY [ExtPython];

    DECLARE @myScript = @"
    def get_mentions(tweet):
        return ';'.join( ( w[1:] for w in tweet.split() if w[0]=='@' ) )

    def usqlml_main(df):
        del df['time']
        del df['author']
        df['mentions'] = df.tweet.apply(get_mentions)
        del df['tweet']
        return df
    ";

    @t  = 
        SELECT * FROM 
           (VALUES
               ("D1","T1","A1","@foo Hello World @bar"),
               ("D2","T2","A2","@baz Hello World @beer")
           ) AS 
               D( date, time, author, tweet );

    @m  =
        REDUCE @t ON date
        PRODUCE date string, mentions string
        USING new Extension.Python.Reducer(pyScript:@myScript);

    OUTPUT @m
        TO "/tweetmentions.csv"
        USING Outputters.Csv();

## <a name="how-r-integrates-with-u-sql"></a>Integration von R in U-SQL

### <a name="datatypes"></a>Datentypen
* Zeichenfolgen- und numerische Spalten von U-SQL werden unmittelbar zwischen R-Datenrahmen (DataFrame) und U-SQL konvertiert [unterstützte Typen: double, string, bool, integer, byte].
* Der Datentyp „factor“ wird in U-SQL nicht unterstützt.
* „byte[]“ muss als Base64-codierte Zeichenfolge serialisiert werden.
* U-SQL-Zeichenfolgen können in Faktoren (factors) in R-Code konvertiert werden, nachdem U-SQL R-Eingabedatenrahmen erstellt oder indem Sie den Reducer-Parameter „stringsAsFactors“ auf „true“ festlegen.

### <a name="schemas"></a>Schemas
* U-SQL-Datasets dürfen keine doppelten Spaltennamen haben.
* U-SQL-Dataset-Spaltennamen müssen Zeichenfolgen sein.
* Spaltennamen müssen in U-SQL und R-Skripts identisch sein.
* Eine schreibgeschützte Spalte darf nicht Bestandteil des Ausgabedatenrahmens sein. Ursache hierfür ist, dass eine schreibgeschützte Spalte automatisch wieder in die U-SQL-Tabelle zurückgeschrieben wird, wenn sie Bestandteil des Ausgabeschemas von UDO ist.

## <a name="next-steps"></a>Nächste Schritte
* [Übersicht über Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [Verwenden von U-SQL-Funktionen für Azure Data Lake Analytics-Aufträge](data-lake-analytics-use-window-functions.md)

