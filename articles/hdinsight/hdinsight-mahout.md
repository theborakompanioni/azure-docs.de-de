---
title: "Generieren von Empfehlungen mit Mahout HDInsight über PowerShell | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Filmempfehlungen mit der Apache Mahout-Bibliothek für Machine Learning und HDInsight (Hadoop) über ein PowerShell-Skript erstellen können, das auf Ihrem Client ausgeführt wird."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 07b57208-32aa-4e59-900a-6c934fa1b7a7
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/14/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: 125f05f5dce5a0e4127348de5b280f06c3491d84
ms.openlocfilehash: 102cd55a0b55e18ec5240bf6ddf94a1909c08ebb
ms.contentlocale: de-de
ms.lasthandoff: 05/22/2017


---
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-hadoop-in-hdinsight-powershell"></a>Erstellen von Filmempfehlungen mithilfe von Apache Mahout mit Hadoop in HDInsight (PowerShell)

[!INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Erfahren Sie, wie Sie Filmempfehlungen mit der [Apache Mahout](http://mahout.apache.org) -Bibliothek für maschinelles Lernen und Azure HDInsight erstellen können. Das Beispiel in diesem Artikel verwendet Azure PowerShell zum Ausführen von Mahout-Aufträgen.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Linux-basierter HDInsight-Cluster. Informationen zu dessen Erstellung finden Sie unter [Erste Schritte mit Linux-basiertem Hadoop in HDInsight][getstarted].

> [!IMPORTANT]
> Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](hdinsight-component-versioning.md#hdi-version-33-nearing-retirement-date).

* [Azure PowerShell](/powershell/azure/overview)

## <a name="recommendations"></a>Generieren von Empfehlungen mithilfe von Azure PowerShell

> [!WARNING]
> Der Auftrag in diesem Abschnitt verwendet Azure PowerShell. Viele der mit Mahout bereitgestellten Klassen funktionieren derzeit mit Azure PowerShell nicht. Eine Liste der Klassen, die nicht mit Azure PowerShell funktionieren, finden Sie im Abschnitt [Problembehandlung](#troubleshooting).
>
> Ein Beispiel für die Verwendung von SSH zum Herstellen einer Verbindung mit HDInsight und zum Ausführen von Mahout-Beispielen direkt im Cluster finden Sie unter [Erstellen von Filmempfehlungen mit Mahout und HDInsight (SSH)](hdinsight-hadoop-mahout-linux-mac.md).

Eine der von Mahout bereitgestellten Funktionen ist ein Empfehlungsmodul. Dieses Modul akzeptiert Daten im Format `userID`, `itemId` und `prefValue` (Benutzervoreinstellung für das Element). Mahout verwendet die Daten, um die Benutzer mit ähnlichen Vorlieben für Elemente zu ermitteln, aus denen sich Empfehlungen erstellen lassen.

Das folgende Beispiel ist eine vereinfachte exemplarische Vorgehensweise der Funktionsweise des Empfehlungsprozesses:

* **Gemeinsames Vorkommen:** Joe, Alice und Bob gefällt *Krieg der Sterne*, *Das Imperium schlägt zurück* und *Rückkehr der Jedi-Ritter*. Mahout stellt fest, dass Benutzer, denen einer dieser Filme gefällt, auch die beiden anderen mögen.

* **Gemeinsames Vorkommen:** Bob und Alice haben auch *Die dunkle Bedrohung*, *Angriff der Klonkrieger* und *Die Rache der Sith* gefallen. Mahout stellt fest, dass Benutzer, denen die vorherigen drei Filme gefallen, auch diese Filme mögen.

* **Vergleichbare Empfehlung**: Da Joe die ersten drei Filme gefallen haben, sucht Mahout nach Filmen, die anderen Personen mit ähnlichen Vorlieben gefallen haben, die Joe aber noch nicht gesehen (mit "Gefällt mir" markiert oder bewertet) hat. In diesem Fall empfiehlt Mahout *Die dunkle Bedrohung*, *Angriff der Klonkrieger* und *Die Rache der Sith*.

### <a name="understanding-the-data"></a>Grundlegendes zu den Daten

[GroupLens Research][movielens] stellt Bewertungsdaten für Filme in einem Mahout-kompatiblen Format zur Verfügung. Diese Daten sind im Standardspeicher Ihres Clusters unter `/HdiSamples//HdiSamples/MahoutMovieData` verfügbar.

Es existieren zwei Dateien, `moviedb.txt` (Informationen zu den Filmen) und `user-ratings.txt`. Die Datei `user-ratings.txt` wird während der Analyse verwendet. Die Datei `moviedb.txt` wird verwendet, um beim Anzeigen der Ergebnisse der Analyse benutzerfreundliche Texte bereitzustellen.

Die Daten in der Datei „user-ratings.txt“ der Benutzerbewertung haben die Struktur von `userID`, `movieID`, `userRating` und `timestamp`, anhand der die Bewertung eines Films durch die einzelnen Benutzer ersichtlich wird. Hier sehen Sie ein Beispiel für die Daten:

    196    242    3    881250949
    186    302    3    891717742
    22    377    1    878887116
    244    51    2    880606923
    166    346    1    886397596

### <a name="run-the-job"></a>Ausführen des Auftrags

Verwenden Sie das folgende Windows PowerShell-Skript zum Ausführen eines Auftrags, der das Mahout-Empfehlungsmodul mit den Filmdaten verwendet.

> [!NOTE]
> Diese Datei fordert Sie zur Eingabe von Informationen auf, die zum Herstellen der Verbindung mit Ihrem HDInsight-Cluster und zum Ausführen von Aufträgen verwendet werden. Es kann einige Minuten dauern, bis die Aufträge abgeschlossen sind und die TXT-Ausgabedatei heruntergeladen wird.

[!code-powershell[main](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=5-98)]

> [!NOTE]
> Mahout-Aufträge entfernen keine temporären Daten, die bei der Verarbeitung des Auftrags erstellt wurden. Der `--tempDir` -Parameter wird im Beispielauftrag festgelegt, sodass die temporären Dateien in einem spezifischen Pfad isoliert abgelegt werden.

Der Mahout-Auftrag gibt keine Ausgabe an STDOUT zurück. Stattdessen wird diese im angegebenen Ausgabeverzeichnis unter **part-r-00000**gespeichert. Das Skript lädt diese Datei in **output.txt** in das aktuelle Verzeichnis auf Ihrer Arbeitsstation herunter.

Der folgende Text ist ein Beispiel für den Inhalt der Datei:

    1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

Die erste Spalte ist die `userID`. Die in "[" und "]" enthaltenen Werte sind `movieId`:`recommendationScore`.

Das Skript lädt auch die Dateien `moviedb.txt` und `user-ratings.txt` herunter, die zur Formatierung der Ausgabe für bessere Lesbarkeit erforderlich sind.

### <a name="view-the-output"></a>Anzeigen der Ausgabe

Obwohl die generierte Ausgabe in einer Anwendung verwendet werden kann, ist sie nicht benutzerfreundlich. Die Datei `moviedb.txt` kann verwendet werden, um die `movieId` in den Namen eines Films aufzulösen. Verwenden Sie das folgende PowerShell-Skript, um Empfehlungen mit Filmnamen anzuzeigen:

[!code-powershell[main](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=106-180)]

Verwenden Sie den folgenden Befehl, um die Empfehlungen in einem benutzerfreundlichen Format anzuzeigen: 

```powershell
.\show-recommendation.ps1 -userId 4 -userDataFile .\user-ratings.txt -movieFile .\moviedb.txt -recommendationFile .\output.txt
```

Die Ausgabe sieht in etwa wie folgender Text aus:

    Reading movies descriptions
    Reading rated movies
    Reading recommendations
    Rated movies
    ---------------------------
    Movie                                    Rating
    -----                                    ------
    Devil's Own, The (1997)                  1
    Alien: Resurrection (1997)               3
    187 (1997)                               2
    (lines ommitted)

    ---------------------------
    Recommended movies
    ---------------------------

    Movie                                    Score
    -----                                    -----
    Good Will Hunting (1997)                 4.6504064
    Swingers (1996)                          4.6862745
    Wings of the Dove, The (1997)            4.6666665
    People vs. Larry Flynt, The (1996)       4.834559
    Everyone Says I Love You (1996)          4.707071
    Secrets & Lies (1996)                    4.818182
    That Thing You Do! (1996)                4.75
    Grosse Pointe Blank (1997)               4.8235292
    Donnie Brasco (1997)                     4.6792455
    Lone Star (1996)                         4.7099237

## <a name="troubleshooting"></a>Problembehandlung

### <a name="cannot-overwrite-files"></a>Dateien können nicht überschrieben werden

Mahout-Aufträge bereinigen keine temporären Dateien, die während der Verarbeitung erstellt wurden. Außerdem überschreiben die Aufträge keine vorhandenen Ausgabedateien.

Um Fehler beim Ausführen von Mahout-Aufträgen zu vermeiden, löschen Sie temporäre und Ausgabedateien zwischen dem Ausführen der Aufträge. Verwenden Sie das folgende PowerShell-Skript, um die Dateien zu entfernen, die von den früheren Skripts in diesem Dokument erstellt wurden:

```powershell
# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
$creds=Get-Credential -Message "Enter the login for the cluster"

#Get the cluster info so we can get the resource group, storage, etc.
$clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroup = $clusterInfo.ResourceGroup
$storageAccountName = $clusterInfo.DefaultStorageAccount.split('.')[0]
$container = $clusterInfo.DefaultStorageContainer
$storageAccountKey = (Get-AzureRmStorageAccountKey `
    -Name $storageAccountName `
-ResourceGroupName $resourceGroup)[0].Value

#Create a storage context and upload the file
$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey $storageAccountKey

#Azure PowerShell can't delete blobs using wildcard,
#so have to get a list and delete one at a time
# Start with the output
$blobs = Get-AzureStorageBlob -Container $container -Context $context -Prefix "example/out"
foreach($blob in $blobs)
{
    Remove-AzureStorageBlob -Blob $blob.Name -Container $container -context $context
}
# Next the temp files
$blobs = Get-AzureStorageBlob -Container $container -Context $context -Prefix "example/temp"
foreach($blob in $blobs)
{
    Remove-AzureStorageBlob -Blob $blob.Name -Container $container -context $context
}
```

### <a name="nopowershell"></a>Klassen, die nicht mit Azure PowerShell funktionieren

Mahout-Aufträge, die die folgenden Klassen verwenden, geben zahlreiche Fehlermeldungen zurück, wenn sie von Windows PowerShell aus verwendet werden:

* org.apache.mahout.utils.clustering.ClusterDumper
* org.apache.mahout.utils.SequenceFileDumper
* org.apache.mahout.utils.vectors.lucene.Driver
* org.apache.mahout.utils.vectors.arff.Driver
* org.apache.mahout.text.WikipediaToSequenceFile
* org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles
* org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer
* org.apache.mahout.classifier.sgd.TrainLogistic
* org.apache.mahout.classifier.sgd.RunLogistic
* org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic
* org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic
* org.apache.mahout.classifier.sgd.RunAdaptiveLogistic
* org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer
* org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator
* org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator
* org.apache.mahout.classifier.df.tools.Describe

Um Aufträge auszuführen, die diese Klassen verwenden, stellen Sie über SSH eine Verbindung mit dem HDInsight-Cluster her, und führen Sie die Aufträge über die Befehlszeile aus. Ein Beispiel für die Ausführung von Mahout-Aufträgen über SSH finden Sie unter [Erstellen von Filmempfehlungen mit Mahout und HDInsight (SSH)](hdinsight-hadoop-mahout-linux-mac.md).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich mit Mahout vertraut gemacht haben, können Sie sich anderen Methoden der Datenverarbeitung in HDInsight zuwenden:

* [Hive mit HDInsight](hdinsight-use-hive.md)
* [Pig mit HDInsight](hdinsight-use-pig.md)
* [MapReduce mit HDInsight](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[aps]: /powershell/azureps-cmdlets-docs
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

