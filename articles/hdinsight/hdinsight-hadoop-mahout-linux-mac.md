---
title: Generieren von Empfehlungen mit Mahout und HDInsight (SSH) | Microsoft Docs
description: "Erfahren Sie, wie Sie Filmempfehlungen mit der Apache Mahout-Bibliothek für maschinelles Lernen und HDInsight (Hadoop) erstellen können."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c78ec37c-9a8c-4bb6-9e38-0bdb9e89fbd7
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 9662ff49e38d65fcba307bc3fc7a8ec699b13202
ms.lasthandoff: 04/12/2017


---
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-linux-based-hadoop-in-hdinsight-ssh"></a>Erstellen von Filmempfehlungen mithilfe von Apache Mahout mit Linux-basiertem Hadoop in HDInsight (SSH)

[!INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Erfahren Sie, wie Sie Filmempfehlungen mit der [Apache Mahout](http://mahout.apache.org) -Bibliothek für maschinelles Lernen und Azure HDInsight erstellen können.

Mahout ist eine Bibliothek für [maschinelles Lernen][ml] für Apache Hadoop. Mahout enthält Algorithmen zur Verarbeitung von Daten wie etwa Filterung, Klassifizierung und Clustering. In diesem Artikel verwenden Sie ein Empfehlungsmodul zum Generieren von Filmempfehlungen auf der Grundlage von Filmen, die Ihre Freunde gesehen haben.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Linux-basierter HDInsight-Cluster. Informationen zu dessen Erstellung finden Sie unter [Erste Schritte mit Linux-basiertem Hadoop in HDInsight][getstarted].

> [!IMPORTANT]
> Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight 3.2 und 3.4](hdinsight-component-versioning.md#hdi-version-33-nearing-deprecation-date).

## <a name="mahout-versioning"></a>Mahout-Versionsverwaltung

Weitere Informationen zu der in Ihrem HDInsight-Cluster enthaltenen Version von Mahout finden Sie unter [HDInsight-Versionen und Hadoop-Komponenten](hdinsight-component-versioning.md).

## <a name="recommendations"></a>Grundlegendes zu Empfehlungen

Eine der von Mahout bereitgestellten Funktionen ist ein Empfehlungsmodul. Dieses Modul akzeptiert Daten im Format `userID`, `itemId` und `prefValue` (Einstellung für das Element). Mahout kann dann eine Analyse des gemeinsamen Vorkommens durchführen: *Benutzer, die eine Vorliebe für ein bestimmtes Element haben, haben auch eine Vorliebe für andere Elemente*. Mahout bestimmt dann Benutzer mit ähnlichen Vorlieben für Elemente, aus denen sich Empfehlungen erstellen lassen.

Im folgenden Workflow sehen Sie ein vereinfachtes Beispiel mit Spielfilmdaten:

* **Gemeinsames Vorkommen**: Joe, Alice und Bob gefällt *Krieg der Sterne*, *Das Imperium schlägt zurück* und *Rückkehr der Jedi-Ritter*. Mahout stellt fest, dass Benutzer, denen einer dieser Filme gefällt, auch die beiden anderen mögen.

* **Gemeinsames Vorkommen**: Bob und Alice haben auch *Die dunkle Bedrohung*, *Angriff der Klonkrieger* und *Die Rache der Sith* gefallen. Mahout stellt fest, dass Benutzer, denen die vorherigen drei Filme gefallen, auch diese drei mögen.

* **Vergleichbare Empfehlung**: Da Joe die ersten drei Filme gefallen haben, sucht Mahout nach Filmen, die anderen Personen mit ähnlichen Vorlieben gefallen haben, die Joe aber noch nicht gesehen (mit „Gefällt mir“ markiert oder bewertet) hat. In diesem Fall empfiehlt Mahout *Die dunkle Bedrohung*, *Angriff der Klonkrieger* und *Die Rache der Sith*.

### <a name="understanding-the-data"></a>Grundlegendes zu den Daten

Praktischerweise stellt [GroupLens Research][movielens] Bewertungsdaten für Filme in einem Mahout-kompatiblen Format zur Verfügung. Diese Daten sind im Standardspeicher Ihres Clusters unter `/HdiSamples/HdiSamples/MahoutMovieData` verfügbar.

Zwei Dateien sind vorhanden: `moviedb.txt` und `user-ratings.txt`. Die Datei „user-ratings.txt“ der Benutzerbewertung wird während der Analyse verwendet, während „moviedb.txt“ beim Anzeigen der Ergebnisse der Analyse benutzerfreundliche Textinformationen angibt.

Die Daten in der Datei „user-ratings.txt“ der Benutzerbewertung haben die Struktur von `userID`, `movieID`, `userRating` und `timestamp`, anhand der die Bewertung eines Films durch die einzelnen Benutzer ersichtlich wird. Hier sehen Sie ein Beispiel für die Daten:

    196    242    3    881250949
    186    302    3    891717742
    22    377    1    878887116
    244    51    2    880606923
    166    346    1    886397596

## <a name="run-the-analysis"></a>Ausführen der Analyse

Verwenden Sie bei einer SSH-Verbindung mit dem Cluster den folgenden Befehl, um den Empfehlungsauftrag auszuführen:

```bash
mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp
```

> [!NOTE]
> Der Auftrag kann mehrere Minuten dauern und mehrere MapReduce-Jobs einschließen.

## <a name="view-the-output"></a>Anzeigen der Ausgabe

1. Sobald der Auftrag abgeschlossen ist, verwenden Sie den folgenden Befehl zum Anzeigen der Ausgabe.

    ```bash
    hdfs dfs -text /example/data/mahoutout/part-r-00000
    ```

    Die Ausgabe sieht nun wie folgt aus:

        1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    Die erste Spalte ist die `userID`. Die in "[" und "]" enthaltenen Werte sind `movieId`:`recommendationScore`.

2. Die Ausgabedaten können zusammen mit „moviedb.txt“ verwendet werden, um weitere Informationen zu den Empfehlungen anzuzeigen. Zunächst müssen wir die Dateien lokal mithilfe folgender Befehle kopieren:

    ```bash
    hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
    hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .
    ```

    Mit diesem Befehl werden die Ausgabedaten zusammen mit den Filmdatendateien in eine Datei namens **recommendations.txt** im aktuellen Verzeichnis kopiert.

3. Verwenden Sie den folgenden Befehl, um ein Python-Skript zu erstellen, mit dem Filmnamen für die Daten in der Empfehlungsausgabe gesucht werden:

    ```bash
    nano show_recommendations.py
    ```

    Verwenden Sie, sobald der Editor geöffnet wird, den folgenden Text als Inhalt für die Datei:

   ```python
   #!/usr/bin/env python

   import sys

   if len(sys.argv) != 5:
        print "Arguments: userId userDataFilename movieFilename recommendationFilename"
        sys.exit(1)

   userId, userDataFilename, movieFilename, recommendationFilename = sys.argv[1:]

   print "Reading Movies Descriptions"
   movieFile = open(movieFilename)
   movieById = {}
   for line in movieFile:
       tokens = line.split("|")
       movieById[tokens[0]] = tokens[1:]
   movieFile.close()

   print "Reading Rated Movies"
   userDataFile = open(userDataFilename)
   ratedMovieIds = []
   for line in userDataFile:
       tokens = line.split("\t")
       if tokens[0] == userId:
           ratedMovieIds.append((tokens[1],tokens[2]))
   userDataFile.close()

   print "Reading Recommendations"
   recommendationFile = open(recommendationFilename)
   recommendations = []
   for line in recommendationFile:
       tokens = line.split("\t")
       if tokens[0] == userId:
           movieIdAndScores = tokens[1].strip("[]\n").split(",")
           recommendations = [ movieIdAndScore.split(":") for movieIdAndScore in movieIdAndScores ]
           break
   recommendationFile.close()

   print "Rated Movies"
   print "------------------------"
   for movieId, rating in ratedMovieIds:
       print "%s, rating=%s" % (movieById[movieId][0], rating)
   print "------------------------"

   print "Recommended Movies"
   print "------------------------"
   for movieId, score in recommendations:
       print "%s, score=%s" % (movieById[movieId][0], score)
   print "------------------------"
   ```

    Drücken Sie **STRG + X**, **STRG + Y** und schließlich die **EINGABETASTE**, um die Daten zu speichern.

4. Verwenden Sie folgenden Befehl, um die Datei ausführbar zu machen:

    ```bash
    chmod +x show_recommendations.py
    ```

5. Führen Sie das Python-Skript aus. Im folgenden Befehl wird davon ausgegangen, dass Sie sich im Verzeichnis befinden, in das alle Dateien heruntergeladen wurden:

    ```bash
    ./show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt
    ```

    Dieser Befehl sucht nach den für die Benutzer-ID 4 generierten Empfehlungen.

    * Mit der Datei **user-ratings.txt** werden Filme abgerufen, die bewertet wurden.

    * Mit der Datei **moviedb.txt** werden die Namen der Filme abgerufen.

    * Mit der Datei **recommendations.txt** werden die Filmempfehlungen für diesen Benutzer abgerufen.

     Die Ausgabe dieses Befehls ähnelt dem folgenden Text:

        Sieben Jahre in Tibet (1997), score=5.0   Indiana Jones und der letzte Kreuzzug (1989), score=5.0   Der weiße Hai (1975), score=5.0   Sinn und Sinnlichkeit (1995), score=5.0   Independence Day (ID4) (1996), score=5.0   Die Hochzeit meines besten Freundes (1997), score=5.0   Jerry Maguire (1996), score=5.0   Scream 2 (1997), score=5.0   Die Jury (1996), score=5.0

## <a name="delete-temporary-data"></a>Löschen temporärer Daten

Mahout-Aufträge entfernen keine temporären Daten, die bei der Verarbeitung des Auftrags erstellt wurden. Der `--tempDir`-Parameter wird im Beispielauftrag festgelegt, sodass die temporären Dateien zum einfachen Löschen in einem spezifischen Pfad isoliert abgelegt werden. Verwenden Sie zum Entfernen temporärer Dateien den folgenden Befehl:

```bash
hdfs dfs -rm -f -r /temp/mahouttemp
```

> [!WARNING]
> Wenn Sie den Befehl erneut ausführen möchten, müssen Sie auch das Ausgabeverzeichnis löschen. Verwenden Sie zum Löschen des Verzeichnisses Folgendes:
>
> `hdfs dfs -rm -f -r /example/data/mahoutout`


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich mit Mahout vertraut gemacht haben, können Sie sich anderen Methoden der Datenverarbeitung in HDInsight zuwenden:

* [Hive mit HDInsight](hdinsight-use-hive.md)
* [Pig mit HDInsight](hdinsight-use-pig.md)
* [MapReduce mit HDInsight](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[management]: https://manage.windowsazure.com/
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools

