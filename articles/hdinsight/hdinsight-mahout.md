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
ms.date: 01/19/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 4ee75cac7fb4c8e6903b73150ec7b1acfc9cb9f9
ms.lasthandoff: 04/12/2017


---
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-hadoop-in-hdinsight-powershell"></a>Erstellen von Filmempfehlungen mithilfe von Apache Mahout mit Hadoop in HDInsight (PowerShell)
[!INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Erfahren Sie, wie Sie Filmempfehlungen mit der [Apache Mahout](http://mahout.apache.org) -Bibliothek für maschinelles Lernen und Azure HDInsight erstellen können. In diesem Dokument erfahren Sie, wie Sie Mahout remote über Azure PowerShell ausführen.

Mahout ist eine Bibliothek für [maschinelles Lernen][ml] für Apache Hadoop. Mahout enthält Algorithmen zur Verarbeitung von Daten wie etwa Filterung, Klassifizierung und Clustering. In diesem Artikel verwenden Sie ein Empfehlungsmodul zum Generieren von Filmempfehlungen auf der Grundlage von Filmen, die Ihre Freunde gesehen haben.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Linux-basierter HDInsight-Cluster. Informationen zu dessen Erstellung finden Sie unter [Erste Schritte mit Linux-basiertem Hadoop in HDInsight][getstarted].

> [!IMPORTANT]
> Linux ist das einzige Betriebssystem, das mit HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-33-nearing-deprecation-date).

* **Eine Arbeitsstation mit Azure PowerShell**.

    > [!IMPORTANT]
    > Die Azure PowerShell-Unterstützung zum Verwalten von HDInsight-Ressourcen mithilfe von Azure Service Manager ist **veraltet** und wird zum 1. Januar 2017 eingestellt. Die Schritte in diesem Dokument zeigen die neuen HDInsight-Cmdlets, die mit Azure Resource Manager genutzt werden können.
    >
    > Führen Sie zur Installation der neuesten Version von Azure PowerShell die unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs) beschriebenen Schritte aus. Wenn Sie über Skripts verfügen, die für die Verwendung der neuen Cmdlets für Azure Resource Manager geändert werden müssen, finden Sie unter [Migrieren zu Azure Resource Manager-basierten Entwicklungstools für HDInsight-Cluster](hdinsight-hadoop-development-using-azure-resource-manager.md) weitere Informationen.

## <a name="recommendations"></a>Generieren von Empfehlungen mithilfe von Azure PowerShell

> [!NOTE]
> Zwar kann der in diesem Abschnitt verwendete Auftrag mit Azure PowerShell ausgeführt werden, jedoch funktionieren viele der mit Mahout bereitgestellten Klassen derzeit nicht mit Azure PowerShell und müssen über die Hadoop-Befehlszeile ausgeführt werden. Eine Liste der Klassen, die nicht mit Azure PowerShell funktionieren, finden Sie im Abschnitt [Problembehandlung](#troubleshooting).
>
> Ein Beispiel für die Verwendung von SSH zum Herstellen einer Verbindung mit HDInsight und zum Ausführen von Mahout-Beispielen direkt im Cluster finden Sie unter [Erstellen von Filmempfehlungen mit Mahout und HDInsight (SSH)](hdinsight-hadoop-mahout-linux-mac.md).

Eine der von Mahout bereitgestellten Funktionen ist ein Empfehlungsmodul. Dieses Modul akzeptiert Daten im Format `userID`, `itemId` und `prefValue` (Benutzervoreinstellung für das Element). Mahout kann dann eine Analyse des gemeinsamen Vorkommens durchführen: *Benutzer, die eine Vorliebe für ein bestimmtes Element haben, haben auch eine Vorliebe für andere Elemente*. Mahout bestimmt dann Benutzer mit ähnlichen Vorlieben für Elemente, aus denen sich Empfehlungen erstellen lassen.

Im Folgenden sehen Sie ein extrem einfaches Beispiel mit Spielfilmen:

* **Gemeinsames Vorkommen:** Joe, Alice und Bob gefällt *Krieg der Sterne*, *Das Imperium schlägt zurück* und *Rückkehr der Jedi-Ritter*. Mahout stellt fest, dass Benutzer, denen einer dieser Filme gefällt, auch die beiden anderen mögen.

* **Gemeinsames Vorkommen:** Bob und Alice haben auch *Die dunkle Bedrohung*, *Angriff der Klonkrieger* und *Die Rache der Sith* gefallen. Mahout stellt fest, dass Benutzer, denen die vorherigen drei Filme gefallen, auch diese drei mögen.

* **Vergleichbare Empfehlung**: Da Joe die ersten drei Filme gefallen haben, sucht Mahout nach Filmen, die anderen Personen mit ähnlichen Vorlieben gefallen haben, die Joe aber noch nicht gesehen (mit "Gefällt mir" markiert oder bewertet) hat. In diesem Fall empfiehlt Mahout *Die dunkle Bedrohung*, *Angriff der Klonkrieger* und *Die Rache der Sith*.

### <a name="understanding-the-data"></a>Grundlegendes zu den Daten

Praktischerweise stellt [GroupLens Research][movielens] Bewertungsdaten für Filme in einem Mahout-kompatiblen Format zur Verfügung. Diese Daten sind im Standardspeicher Ihres Clusters unter `/HdiSamples//HdiSamples/MahoutMovieData` verfügbar.

Es existieren zwei Dateien, `moviedb.txt` (Informationen zu den Filmen) und `user-ratings.txt`. Die Datei „user-ratings.txt“ der Benutzerbewertung wird während der Analyse verwendet, während „moviedb.txt“ beim Anzeigen der Ergebnisse der Analyse benutzerfreundliche Textinformationen angibt.

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
$creds=Get-Credential -Message "Enter the login for the cluster (the default name is usually 'admin')"

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

#Use Hive to figure out the path to the mahout examples
#Because the file name/path has a version number in it that changes
$queryString = "!ls /usr/hdp/current/mahout-client"
$hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString
$hiveJob=Start-AzureRmHDInsightJob -ClusterName $clusterName -JobDefinition $hiveJobDefinition -HttpCredential $creds
$dummy = wait-azurermhdinsightjob -ClusterName $clusterName -JobId $hiveJob.JobId -HttpCredential $creds
#Get the files returned from Hive
$files=get-azurermhdinsightjoboutput -clustername $clusterName -JobId $hiveJob.JobId -DefaultContainer $container -DefaultStorageAccountName $storageAccountName -DefaultStorageAccountKey $storageAccountKey -HttpCredential $creds
#Find the file that starts with mahout-examples and ends in job.jar
$jarFile = $files | select-string "mahout-examples.+job\.jar" | % {$_.Matches.Value}
#Add the full path
$jarFile = "file:///usr/hdp/current/mahout-client/$jarFile"

# The arguments for the mahout job
# * input - the path to the data uploaded to HDInsight
# * output - the path to store output data
# * tempDir - the directory for temp files
$jobArguments = "-s", "SIMILARITY_COOCCURRENCE", `
                "--input", "/HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt",
                "--output", "/example/out",
                "--tempDir", "/example/temp"

# Create the job definition
$jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
    -JarFile $jarFile `
    -ClassName "org.apache.mahout.cf.taste.hadoop.item.RecommenderJob" `
    -Arguments $jobArguments

# Start the job
$job = Start-AzureRmHDInsightJob `
    -ClusterName $clusterName `
    -JobDefinition $jobDefinition `
    -HttpCredential $creds

# Wait on the job to complete
Write-Host "Wait for the job to complete ..." -ForegroundColor Green
Wait-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds

# Write out any error information
Write-Host "STDERR"
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError

# Download the output
Get-AzureStorageBlobContent `
        -Blob example/out/part-r-00000 `
        -Container $container `
        -Destination output.txt `
        -Context $context
#Download movie and user files for use in displaying results
Get-AzureStorageBlobContent -blob "HdiSamples/HdiSamples/MahoutMovieData/moviedb.txt" `
        -Container $container `
        -Destination moviedb.txt `
        -Context $context
Get-AzureStorageBlobContent -blob "HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt" `
        -Container $container `
        -Destination user-ratings.txt `
        -Context $context
```

> [!NOTE]
> Mahout-Aufträge entfernen keine temporären Daten, die bei der Verarbeitung des Auftrags erstellt wurden. Der `--tempDir` -Parameter wird im Beispielauftrag festgelegt, sodass die temporären Dateien in einem spezifischen Pfad isoliert abgelegt werden.

Der Mahout-Auftrag gibt keine Ausgabe an STDOUT zurück. Stattdessen wird diese im angegebenen Ausgabeverzeichnis unter **part-r-00000**gespeichert. Das Skript lädt diese Datei in **output.txt** in das aktuelle Verzeichnis auf Ihrer Arbeitsstation herunter.

Im Folgenden sehen Sie ein Beispiel für den Inhalt der Datei:

    1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

Die erste Spalte ist die `userID`. Die in "[" und "]" enthaltenen Werte sind `movieId`:`recommendationScore`.

Das Skript lädt auch die Dateien `moviedb.txt` und `user-ratings.txt` herunter, die zur Formatierung der Ausgabe für bessere Lesbarkeit erforderlich sind.

### <a name="view-the-output"></a>Anzeigen der Ausgabe

Obwohl die generierte Ausgabe in einer Anwendung verwendet werden kann, ist sie für Benutzer nicht gut lesbar. Die Datei `moviedb.txt` kann verwendet werden, um die `movieId` in den Namen eines Films aufzulösen. Verwenden Sie das folgende PowerShell-Skript, um Empfehlungen mit Filmnamen anzuzeigen:

```powershell
<#
.SYNOPSIS
    Displays recommendations for movies.
.DESCRIPTION
    Displays recommendations generated by Mahout
    with HDInsight example in a human readable format.
.EXAMPLE
    .\Show-Recommendation -userId 4
        -userDataFile "user-ratings.txt"
        -movieFile "moviedb.txt"
        -recommendationFile "output.txt"
#>

[CmdletBinding(SupportsShouldProcess = $true)]
param(
    #The user ID
    [Parameter(Mandatory = $true)]
    [String]$userId,

    [Parameter(Mandatory = $true)]
    [String]$userDataFile,

    [Parameter(Mandatory = $true)]
    [String]$movieFile,

    [Parameter(Mandatory = $true)]
    [String]$recommendationFile
)
# Read movie ID & description into hash table
Write-Host "Reading movies descriptions" -ForegroundColor Green
$movieById = @{}
foreach($line in Get-Content $movieFile)
{
    $tokens = $line.Split("|")
    $movieById[$tokens[0]] = $tokens[1]
}
# Load movies user has already seen (rated)
# into a hash table
Write-Host "Reading rated movies" -ForegroundColor Green
$ratedMovieIds = @{}
foreach($line in Get-Content $userDataFile)
{
    $tokens = $line.Split("`t")
    if($tokens[0] -eq $userId)
    {
        # Resolve the ID to the movie name
        $ratedMovieIds[$movieById[$tokens[1]]] = $tokens[2]
    }
}
# Read recommendations generated by Mahout
Write-Host "Reading recommendations" -ForegroundColor Green
$recommendations = @{}
foreach($line in get-content $recommendationFile)
{
    $tokens = $line.Split("`t")
    if($tokens[0] -eq $userId)
    {
        #Trim leading/treailing [] and split at ,
        $movieIdAndScores = $tokens[1].TrimStart("[").TrimEnd("]").Split(",")
        foreach($movieIdAndScore in $movieIdAndScores)
        {
            #Split at : and store title and score in a hash table
            $idAndScore = $movieIdAndScore.Split(":")
            $recommendations[$movieById[$idAndScore[0]]] = $idAndScore[1]
        }
        break
    }
}

Write-Host "Rated movies" -ForegroundColor Green
Write-Host "---------------------------" -ForegroundColor Green
$ratedFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                @{Expression={$_.Value};Label="Rating"}
$ratedMovieIds | format-table $ratedFormat
Write-Host "---------------------------" -ForegroundColor Green

write-host "Recommended movies" -ForegroundColor Green
Write-Host "---------------------------" -ForegroundColor Green
$recommendationFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                        @{Expression={$_.Value};Label="Score"}
$recommendations | format-table $recommendationFormat
```

Nachfolgend sehen Sie ein Beispiel für das Ausführen des Skripts:

    PS C:\> show-recommendation.ps1 -userId 4 -userDataFile .\user-ratings.txt -movieFile .\moviedb.txt -recommendationFile .\output.txt

Die Ausgabe sollte in etwa folgendermaßen aussehen:

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

Mahout-Aufträge bereinigen keine temporären Dateien, die während der Verarbeitung erstellt wurden. Außerdem überschreiben Aufträge keine vorhandenen Ausgabedateien.

Um Fehler beim Ausführen von Mahout-Aufträgen zu vermeiden, löschen Sie temporäre und Ausgabedateien zwischen dem Ausführen der Aufträge, oder verwenden Sie eindeutige Namen für temporäre und Ausgabeverzeichnisse. Verwenden Sie das folgende PowerShell-Skript, um die Dateien zu entfernen, die von den früheren Skripts in diesem Dokument erstellt wurden:

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

Mahout-Aufträge, die die folgenden Klassen verwenden, geben zahlreiche Fehlermeldungen zurück, wenn sie von Windows PowerShell aus verwendet werden.

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

