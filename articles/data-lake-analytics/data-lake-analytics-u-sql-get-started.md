---
title: Erste Schritte mit der U-SQL-Sprache | Microsoft-Dokumentation
description: "Enthält eine grundlegende Beschreibung der U-SQL-Sprache."
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: 57143396-ab86-47dd-b6f8-613ba28c28d2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 4884d96e8126337f62af23316935978cfe219ec8
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017


---
# <a name="get-started-with-u-sql"></a>Erste Schritte mit U-SQL
U-SQL ist eine Sprache, bei der deklarative SQL mit imperativen C#-Elementen kombiniert wird, damit Sie Daten jedweden Umfangs verarbeiten können. Mit der skalierbaren Funktion für verteilte Abfragen von U-SQL können Sie Daten über relationale Speicher, z.B. Azure SQL-Datenbank, hinweg effizient analysieren. Mit U-SQL können Sie unstrukturierte Daten verarbeiten, indem Sie ein Schema zum Lesen anwenden und benutzerdefinierte Logik und UDFs einfügen. Darüber hinaus bietet U-SQL eine Erweiterbarkeit, die Ihnen eine präzisere Steuerung der Ausführung in großem Umfang ermöglicht. 

## <a name="learning-resources"></a>Schulungsressourcen

Ausführliche Informationen zur **Syntax der U-SQL-Sprache** finden Sie unter [U-SQL Language Reference](http://go.microsoft.com/fwlink/p/?LinkId=691348) (Referenz zur U-SQL-Sprache).

Informationen zur **Entwurfsphilosophie von U-SQL** finden Sie im Visual Studio-Blogbeitrag [Introducing U-SQL – A Language that Makes Big Data Processing Easy](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/) (Einführung in U-SQL: eine Sprache, die die Verarbeitung von Big Data vereinfacht).

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie die U-SQL-Beispiele in diesem Dokument durcharbeiten, ist es ratsam, das [Tutorial: Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md) zu lesen und zu absolvieren. In diesem Tutorial wird veranschaulicht, wie Sie U-SQL mit Azure Data Lake Tools für Visual Studio verwenden.

## <a name="your-first-u-sql-script"></a>Ihr erstes U-SQL-Skript

Das folgende U-SQL-Skript ist sehr einfach aufgebaut und ermöglicht es uns, viele Aspekte der U-SQL-Sprache zu untersuchen.

```
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();

OUTPUT @searchlog   
    TO "/output/SearchLog-first-u-sql.csv"
    USING Outputters.Csv();
```

Dieses Skript weist keine Transformationsschritte auf. Es liest Daten aus der Quelldatei `SearchLog.tsv` und schematisiert sie. Anschließend schreibt es das Rowset wieder in eine Datei namens „SearchLog-first-u-sql.csv“.

Beachten Sie im Feld `Duration` das Fragezeichen neben dem Datentyp. Dies bedeutet, dass das Feld `Duration` NULL sein kann.

### <a name="key-concepts"></a>Wichtige Begriffe
* **Rowsetvariablen**: Jeder Abfrageausdruck, der ein Rowset produziert, kann einer Variablen zugewiesen werden. U-SQL basiert im Skript auf dem Benennungsmuster für T-SQL-Variablen (z.B. `@searchlog`).
* Mit dem Schlüsselwort **EXTRACT** werden Daten aus einer Datei gelesen, und das Schema wird beim Lesen definiert. `Extractors.Tsv` ist ein integrierter U-SQL-Extractor für Dateien mit durch Tabstopp getrennten Werten. Sie können benutzerdefinierte Extractors entwickeln.
* Mit **OUTPUT** werden Daten aus einem Rowset in eine Datei geschrieben. `Outputters.Csv()` ist ein integrierter U-SQL-Outputter zum Erstellen einer Datei mit kommagetrennten Werten. Sie haben die Möglichkeit, benutzerdefinierte Outputters zu entwickeln.

### <a name="file-paths"></a>Dateipfade

Für die Anweisungen EXTRACT und OUTPUT werden Dateipfade verwendet. Dateipfade können absolut oder relativ sein:

Dieser absolute Dateipfad verweist auf eine Datei in einer Data Lake Store-Instanz mit dem Namen `mystore`:

    adl://mystore.azuredatalakestore.net/Samples/Data/SearchLog.tsv

Dieser absolute Pfad bezieht sich auf eine Datei in einem Azure Blob Storage-Konto mit dem Namen `myblobaccount` und in einem Container mit dem Namen `mycontainer`:

    wasb://mycontainer@myblobaccount.blob.core.windows.net/Samples/Data/SearchLog.tsv

 >[!NOTE]
 >Azure-Blobspeichercontainer mit Zugriffsberechtigungen für öffentliche Blobs oder öffentliche Container werden derzeit nicht unterstützt.

Dieser relative Dateipfad beginnt mit `"/"`. Er verweist auf eine Datei unter dem Data Lake Store-Standardkonto, das dem Data Lake Analytics-Konto zugeordnet ist:

    TO "/output/SearchLog-first-u-sql.csv"

## <a name="use-scalar-variables"></a>Verwenden von Skalarvariablen

Sie können auch Skalarvariablen verwenden, um die Skriptverwaltung zu vereinfachen. Das vorherige U-SQL-Skript kann auch wie folgt geschrieben werden:

    DECLARE @in  string = "/Samples/Data/SearchLog.tsv";
    DECLARE @out string = "/output/SearchLog-scalar-variables.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO @out
        USING Outputters.Csv();

## <a name="transform-rowsets"></a>Transformieren von Rowsets

Verwenden Sie **SELECT** , um Rowsets zu transformieren:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-rowsets.csv"
        USING Outputters.Csv();

Die WHERE-Klausel verwendet einen [booleschen C#-Ausdruck](https://msdn.microsoft.com/library/6a71f45d.aspx). Sie können die C#-Ausdruckssprache nutzen, um Ihre eigenen Ausdrücke und Funktionen zu erstellen. Sie können sogar komplexere Filtervorgänge durchführen, indem Sie die Ausdrücke und Funktionen mit logischen Konjunktionen (AND) und Disjunktionen (OR) kombinieren.

Im folgenden Skript werden die DateTime.Parse()-Methode und eine Konjunktion verwendet.

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start >= DateTime.Parse("2012/02/16") AND Start <= DateTime.Parse("2012/02/17");

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-datetime.csv"
        USING Outputters.Csv();

 >[!NOTE]
 >Die zweite Abfrage verwendet das Ergebnis des ersten Rowsets, wodurch ein Verbund der beiden Filter entsteht. Sie können auch einen Variablennamen wiederverwenden, und die Namen werden lexikalisch begrenzt.

## <a name="aggregate-rowsets"></a>Aggregieren von Rowsets
Bei U-SQL können Sie die vertrauten Elemente ORDER BY und GROUP BY sowie Aggregationen verwenden.

Die folgende Abfrage sucht nach der Gesamtdauer pro Region und zeigt dann die ersten fünf Zeiträume in der Reihenfolge ihrer Länge an.

In U-SQL-Rowsets wird die Reihenfolge für die nächste Abfrage nicht beibehalten. Zum Sortieren einer Ausgabe müssen Sie der OUTPUT-Anweisung also das ORDER BY-Element hinzufügen:

    DECLARE @outpref string = "/output/Searchlog-aggregation";
    DECLARE @out1    string = @outpref+"_agg.csv";
    DECLARE @out2    string = @outpref+"_top5agg.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region;

    @res =
    SELECT *
    FROM @rs1
    ORDER BY TotalDuration DESC
    FETCH 5 ROWS;

    OUTPUT @rs1
        TO @out1
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();
    OUTPUT @res
        TO @out2
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

Für die ORDER BY-Klausel von U-SQL ist die Verwendung der FETCH-Klausel in einem SELECT-Ausdruck erforderlich.

Die U-SQL-Klausel HAVING kann verwendet werden, um die Ausgabe auf Gruppen zu beschränken, die die HAVING-Bedingung erfüllen:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-having.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

## <a name="see-also"></a>Weitere Informationen
* [Übersicht über Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [Verwenden von U-SQL-Funktionen für Azure Data Lake Analytics-Aufträge](data-lake-analytics-use-window-functions.md)

## <a name="let-us-know-what-you-think"></a>Teilen Sie uns Ihre Meinung mit
* [Feature anfordern](http://aka.ms/adlafeedback)
* [Hilfe in Foren](http://aka.ms/adlaforums)
* [Feedback zu U-SQL](http://aka.ms/usqldiscuss)

