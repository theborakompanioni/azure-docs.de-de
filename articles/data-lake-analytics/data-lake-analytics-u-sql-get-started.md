---
title: 'Tutorial: Erste Schritte mit der Azure Data Lake Analytics-U-SQL-Sprache | Microsoft-Dokumentation'
description: In diesem Tutorial erhalten Sie Informationen zur U-SQL-Sprache in Azure Data Lake Analytics.
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
translationtype: Human Translation
ms.sourcegitcommit: 2f03ba60d81e97c7da9a9fe61ecd419096248763
ms.openlocfilehash: dc8c7beaf5b8e8d4f5467ffe22390c41f446d787
ms.lasthandoff: 03/04/2017


---
# <a name="tutorial-get-started-with-azure-data-lake-analytics-u-sql-language"></a>Tutorial: Erste Schritte mit der Azure Data Lake Analytics-U-SQL-Sprache
U-SQL ist eine Sprache, bei der die Vorteile von SQL mit der Ausdruckskraft Ihres eigenen Codes zum Verarbeiten von Daten jedweden Umfangs kombiniert werden. Mit der skalierbaren Funktion für verteilte Abfragen von U-SQL können Sie Daten über relationale Speicher, z.B. Azure SQL-Datenbank, hinweg effizient analysieren. Mit U-SQL können Sie unstrukturierte Daten verarbeiten, indem Sie ein Schema zum Lesen anwenden und benutzerdefinierte Logik und UDFs einfügen. Darüber hinaus bietet U-SQL eine Erweiterbarkeit, die Ihnen eine präzisere Steuerung der Ausführung in großem Umfang ermöglicht. Informationen zur Entwurfsphilosophie hinter U-SQL finden Sie im Visual Studio-Blogbeitrag [Introducing U-SQL – A Language that makes Big Data Processing Easy](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/) (Einführung in U-SQL: eine Sprache, die die Verarbeitung von Big Data vereinfacht).

U-SQL unterscheidet sich in einigen Aspekten von ANSI SQL oder T-SQL. Schlüsselwörter wie beispielsweise SELECT müssen in Großbuchstaben geschrieben werden.

 Typsystem und Ausdruckssprache in SELECT-Klauseln und WHERE-Prädikaten sind C#. Dies bedeutet, dass die Datentypen C#-Typen sind und C#-NULL-Semantik verwenden. Die Vergleichsvorgänge innerhalb eines Prädikats folgen der C#-Syntax (z.B. a == "foo"). Es bedeutet auch, dass die Werte vollständige .NET-Objekte sind und Sie problemlos alle Methoden für Objektvorgänge verwenden können (z.B. "f o o o".Split(' ')).

Weitere Informationen zu U-SQL finden Sie in der [U-SQL Language Reference](http://go.microsoft.com/fwlink/p/?LinkId=691348) (U-SQL-Sprachreferenz).

### <a name="prerequisites"></a>Voraussetzungen
Wenn noch nicht bereits erfolgt, arbeiten Sie das [Tutorial: Entwickeln von U-SQL-Skripts mit Data Lake Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md) durch. Nachdem Sie das Tutorial abgeschlossen haben, kehren Sie zu diesem Artikel zurück.

Im Tutorial haben Sie einen Azure Data Lake Analytics-Auftrag mit dem folgenden U-SQL-Skript ausgeführt:

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

Dieses Skript weist keine Transformationsschritte auf. Es liest Daten aus der Quelldatei „SearchLog.tsv“ und schematisiert sie. Anschließend schreibt es das Rowset wieder in eine Datei namens „SearchLog-first-u-sql.csv“.

Beachten Sie im Feld **Dauer** das Fragezeichen neben dem Datentyp. Dies bedeutet, dass das Feld **Dauer** null sein kann.

Im Skript finden Sie folgende Konzepte und Schlüsselwörter:

* Rowsetvariablen: Jeder Abfrageausdruck, der ein Rowset produziert, kann einer Variablen zugewiesen werden. U-SQL basiert im Skript auf dem Benennungsmuster für T-SQL-Variablen (z.B. @searchlog).

 >[!NOTE]
 >Die Zuweisung erzwingt keine Ausführung. Der Ausdruck wird lediglich benannt, sodass Sie komplexere Ausdrücke erstellen können.
* EXTRACT: Mit diesem Schlüsselwort können Sie ein Schema zum Lesen definieren. Das Schema wird mit einem Spaltennamen und einem C#-Typnamenpaar pro Spalte angegeben. Das Schema verwendet einen so genannten „Extractor“, z.B. „Extractors.Tsv()“, um TSV-Dateien zu extrahieren. Sie können benutzerdefinierte Extractors entwickeln.
* OUTPUT: Dieses Schlüsselwort serialisiert ein Rowset. „Outputters.Csv()“ gibt eine durch Trennzeichen getrennte Datei an einem bestimmten Speicherort aus. Sie können auch benutzerdefinierte Outputters entwickeln.

 >[!NOTE]
 >Die beiden Pfade sind relative Pfade. Sie können aber auch absolute Pfade verwenden. Beispiel:    
 >     adl://\<ADLStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
 >
 >Sie müssen einen absoluten Pfad verwenden, um auf die Dateien in den verknüpften Speicherkonten zuzugreifen.  Die Syntax für im verknüpften Azure-Speicherkonto gespeicherte Dateien lautet: wasb://\<BlobContainerName>@\<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

 >[!NOTE]
 >Azure-Blobspeichercontainer mit Zugriffsberechtigungen für öffentliche Blobs oder öffentliche Container werden derzeit nicht unterstützt.

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

Die U-SQL-Klausel ORDER BY muss mit der FETCH-Klausel in einem SELECT-Ausdruck kombiniert werden.

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

## <a name="join-data"></a>Verknüpfen von Daten
U-SQL enthält häufig verwendete Verknüpfungsoperatoren, z. B. INNER JOIN, LEFT/RIGHT/FULL OUTER JOIN, SEMI JOIN. Damit können nicht nur Tabellen verknüpft werden, sondern auch beliebige Rowsets (auch aus Dateien erzeugte Rowsets).

Im folgenden Skript wird das searchlog-Element mit einem Ankündigungsaufrufprotokoll verknüpft und gibt die Ankündigungen für die Abfragezeichenfolge für ein bestimmtes Datum aus.

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


U-SQL unterstützt nur die ANSI-kompatible Verknüpfungssyntax „Rowset1 JOIN Rowset2 ON predicate“. Die alte Syntax „FROM Rowset1, Rowset2 WHERE predicate“ wird _nicht_ unterstützt.
Das Prädikat in einem JOIN-Element muss eine Gleichheitsverknüpfung (kein Ausdruck) sein. Wenn Sie einen Ausdruck verwenden möchten, können Sie ihn der SELECT-Klausel eines vorherigen Rowsets hinzufügen. Wenn Sie einen anderen Vergleich durchführen möchten, können Sie ihn in die WHERE-Klausel verschieben.

## <a name="create-databases-table-valued-functions-views-and-tables"></a>Erstellen von Datenbanken, Tabellenwertfunktionen, Ansichten und Tabellen
In U-SQL können Sie Daten im Kontext einer Datenbank und eines Schemas verwenden, und Sie müssen nicht immer aus Dateien lesen oder in Dateien schreiben.

Alle U-SQL-Skripts werden mit einer Standarddatenbank (master) und einem Standardschema (DBO) als Standardkontext ausgeführt. Sie können Ihre eigene Datenbank bzw. Ihr eigenes Schema erstellen. Verwenden Sie zum Ändern des Kontexts die USE-Anweisung.

### <a name="create-a-tvf"></a>Erstellen einer Tabellenwertfunktion
Im vorherigen U-SQL-Skript haben Sie die Verwendung von EXTRACT zum Lesen aus derselben Quelldatei wiederholt. Mithilfe der Tabellenwertfunktion von U-SQL können Sie die Daten zur späteren Wiederverwendung kapseln.  

Im folgenden Skript wird eine Tabellenwertfunktion mit dem Namen *Searchlog()* in der Standarddatenbank und im Standardschema erstellt:

    DROP FUNCTION IF EXISTS Searchlog;

    CREATE FUNCTION Searchlog()
    RETURNS @searchlog TABLE
    (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
    )
    AS BEGIN
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
    RETURN;
    END;

Das folgende Skript veranschaulicht, wie Sie die im obigen Skript definierte Tabellenwertfunktion verwenden:

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM Searchlog() AS S
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/SerachLog-use-tvf.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-views"></a>Erstellen von Ansichten
Falls Sie nur über einen Abfrageausdruck verfügen, der abstrahiert werden soll, und keinen Parameter aus diesem Ausdruck erstellen möchten, können Sie anstelle einer Tabellenwertfunktion eine Ansicht erstellen.

Im folgenden Skript wird die Ansicht *SearchlogView* in der Standarddatenbank und im Standardschema erstellt:

    DROP VIEW IF EXISTS SearchlogView;

    CREATE VIEW SearchlogView AS  
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();

Das folgende Skript veranschaulicht die Verwendung der definierten Ansicht:

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchlogView
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-use-view.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-tables"></a>Erstellen von Tabellen.
Ähnlich wie bei relationalen Datenbanktabellen ermöglicht U-SQL Ihnen die Erstellung einer Tabelle mit einem vordefinierten Schema. Sie können auch eine Tabelle erstellen und das Schema aus der Abfrage ableiten, mit der die Tabelle aufgefüllt wird (auch als CREATE TABLE AS SELECT oder CTAS bezeichnet).

Erstellen Sie mithilfe des folgenden Skripts eine Datenbank und zwei Tabellen:

    DROP DATABASE IF EXISTS SearchLogDb;
    CREATE DATABASE SearchLogDb;
    USE DATABASE SearchLogDb;

    DROP TABLE IF EXISTS SearchLog1;
    DROP TABLE IF EXISTS SearchLog2;

    CREATE TABLE SearchLog1 (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string,

                INDEX sl_idx CLUSTERED (UserId ASC)
                    DISTRIBUTED BY HASH (UserId)
    );

    INSERT INTO SearchLog1 SELECT * FROM master.dbo.Searchlog() AS s;

    CREATE TABLE SearchLog2(
        INDEX sl_idx CLUSTERED (UserId ASC)
                DISTRIBUTED BY HASH (UserId)
    ) AS SELECT * FROM master.dbo.Searchlog() AS S; // You can use EXTRACT or SELECT here


### <a name="query-tables"></a>Abfragen von Tabellen
Sie können Tabellen wie die im obigen Skript erstellten auf die gleiche Weise wie Datendateien abfragen. Anstatt mit EXTRACT ein Rowset zu erstellen, können Sie jetzt auf den Tabellennamen verweisen.

Um aus den Tabellen zu lesen, ändern Sie das zuvor verwendete Transformationsskript:

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchLogDb.dbo.SearchLog2
    GROUP BY Region;

    @res =
        SELECT *
        FROM @rs1
        ORDER BY TotalDuration DESC
        FETCH 5 ROWS;

    OUTPUT @res
        TO "/output/Searchlog-query-table.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

 >[!NOTE]
 >Derzeit können Sie eine SELECT-Anweisung für eine Tabelle nicht im gleichen Skript ausführen, in dem Sie die Tabelle erstellt haben.

## <a name="conclusion"></a>Zusammenfassung
Dieses Tutorial deckt nur einen kleinen Teil von U-SQL ab. Viele weitere Vorteile von U-SQL können aufgrund des begrenzten Umfangs nicht erläutert werden. Dazu zählen z. B.:

* Verwenden von CROSS APPLY, um Teile von Zeichenfolgen, Arrays und Zuordnungen in Zeilen zu entpacken
* Verwenden von partitionierten Datasets (Dateisätze und partitionierte Tabellen)
* Entwickeln von benutzerdefinierten Operatoren, z.B. Extractors, Outputters, Prozessoren und benutzerdefinierte Aggregatoren in C#
* Verwenden von U-SQL-Windowing-Funktionen
* Verwalten von U-SQL-Code mit Ansichten, Tabellenwertfunktionen und gespeicherten Prozeduren
* Ausführen von beliebigem benutzerdefiniertem Code auf Verarbeitungsknoten
* Herstellen einer Verbindung mit SQL-Datenbanken und Durchführen von Partnerverbundabfragen in diesen Datenbanken und Ihren U-SQL- und Azure Data Lake-Daten

## <a name="see-also"></a>Siehe auch
* [Übersicht über Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [Verwenden von U-SQL-Funktionen für Azure Data Lake Analytics-Aufträge](data-lake-analytics-use-window-functions.md)
* [Überwachung und Problembehandlung von Azure Data Lake Analytics-Aufträgen mithilfe des Azure-Portals](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

## <a name="let-us-know-what-you-think"></a>Teilen Sie uns Ihre Meinung mit
* [Feature anfordern](http://aka.ms/adlafeedback)
* [Hilfe in Foren](http://aka.ms/adlaforums)
* [Feedback zu U-SQL](http://aka.ms/usqldiscuss)

