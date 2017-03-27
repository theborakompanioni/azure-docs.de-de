---
title: "Entwickeln von Python MapReduce-Aufträgen mit HDInsight | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Python MapReduce-Aufträge auf Linux-basierten HDInsight-Clustern erstellen und ausführen."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7631d8d9-98ae-42ec-b9ec-ee3cf7e57fb3
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/06/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: cf233279c0a26c3d2970596b1bb515508da20b89
ms.openlocfilehash: ad696f14d48452840805bc413d890309e523ce34
ms.lasthandoff: 02/07/2017


---
# <a name="develop-python-streaming-programs-for-hdinsight"></a>Entwickeln von Python-Streamingprogrammen für HDInsight

Hadoop stellt eine Streaming-API für MapReduce zur Verfügung, mit der Sie Map- und Reduce-Funktionen in anderen Sprache als Java schreiben können. In diesem Dokument erfahren Sie, wie Sie mit Python MapReduce-Vorgänge ausführen.

Dieser Artikel basiert auf Informationen und Beispielen, die von Michael Noll unter [Writing an Hadoop MapReduce Program in Python](http://www.michael-noll.com/tutorials/writing-an-hadoop-mapreduce-program-in-python/)(in englischer Sprache) veröffentlicht wurden.

## <a name="prerequisites"></a>Voraussetzungen

Um die in diesem Artikel aufgeführten Schritte auszuführen, benötigen Sie Folgendes:

* Ein Linux-basierter Hadoop-Cluster in HDInsight

  > [!IMPORTANT]
  > Die Schritte in diesem Dokument erfordern einen HDInsight-Cluster mit Linux. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

* Ein Texteditor
  
  > [!IMPORTANT]
  > Im Text-Editor muss LF als Zeilenende verwendet werden. Wenn CRLF verwendet wird, verursacht dies beim Ausführen des MapReduce-Auftrags auf Linux-basierten HDInsight-Clustern Fehler. Wenn Sie nicht sicher sind, führen Sie die optionalen Schritte im Abschnitt [Ausführen von MapReduce](#run-mapreduce) aus, um CRLF-Vorkommen in LF zu konvertieren.

* **Erfahrung mit SSH und SCP**. Weitere Informationen zur Verwendung von SSH und SCP mit HDInsight finden Sie in den folgenden Artikeln:
  
  * **Linux-, Unix- oder OS X-Clients**: Siehe [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, OS X oder Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

  * **Windows-Clients**: Siehe [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

## <a name="word-count"></a>Wortzählung

In diesem Beispiel implementieren Sie eine einfache Wortanzahlermittlung mithilfe einer Mapper- und Reducer-Funktion. Die Mapper-Funktion teilt Sätze in einzelne Wörter auf und die Reducer-Funktion aggregiert die Wörter und Zähler, um die Ausgabe zu erzeugen.

Im folgenden Flußdiagramm ist dargestellt, was in den Mapper- und Reducer-Funktionen geschieht.

![Abbildung von MapReduce](./media/hdinsight-hadoop-streaming-python/HDI.WordCountDiagram.png)

## <a name="why-python"></a>Gründe für Python

Python ist eine allgemeine Programmiersprache, mit der Sie Konzepte in weniger Codezeilen als in vielen anderen Sprachen ausdrücken können. Sie wurde kürzlich bei Datenwissenschaftlern als Prototyping-Sprache beliebt, da sie durch ihre interpretierende Art, dynamische Typisierung und elegante Syntax für die schnelle Anwendungsentwicklung geeignet ist.

Python wird auf allen HDInsight-Clustern installiert.

## <a name="streaming-mapreduce"></a>Streaming-MapReduce

Mit Hadoop können Sie eine Datei angeben, die die von einem Auftrag verwendete Logik für den Mapper und Reducer enthält. Bestimmte Anforderungen an die Logik für Mapper und Reducer:

* **Eingabe**: Die Komponenten für Mapper und Reducer müssen Eingabedaten von STDIN lesen.
* **Ausgabe**: Die Komponenten für Mapper und Reducer müssen Ausgabedaten an STDOUT schreiben.
* **Datenformat**: Die verwendeten und erzeugten Daten müssen ein Schlüssel-/Wertpaar darstellen, das durch ein Tabulatorzeichen getrennt werden muss.

Python kann diese Anforderungen einfach mithilfe des Moduls **sys** zum Lesen von STDIN und mithilfe des Moduls **print** für die Ausgabe an STDOUT behandeln. Der Rest besteht einfach aus der Formatierung der Daten mit einem Tabulatorzeichen (`\t`) zwischen Schlüssel und Wert.

## <a name="create-the-mapper-and-reducer"></a>Erstellen von Mapper und Reducer

Mapper und Reducer sind reine Textdateien, in diesem Fall **mapper.py** und **reducer.py**, daher ist eindeutig, welche Datei wofür zuständig ist. Sie können diese mit einem Editor Ihrer Wahl erstellen.

### <a name="mapperpy"></a>Mapper.py

Erstellen Sie eine neue Datei namens **mapper.py** , und verwenden Sie den folgenden Inhalt für die Datei.

```python
#!/usr/bin/env python

# Use the sys module
import sys

# 'file' in this case is STDIN
def read_input(file):
    # Split each line into words
    for line in file:
        yield line.split()

def main(separator='\t'):
    # Read the data using read_input
    data = read_input(sys.stdin)
    # Process each words returned from read_input
    for words in data:
        # Process each word
        for word in words:
            # Write to STDOUT
            print '%s%s%d' % (word, separator, 1)

if __name__ == "__main__":
    main()
```

Nehmen Sie sich einen Moment Zeit, um den Code zu lesen und seine Funktionsweise zu verstehen.

### <a name="reducerpy"></a>reducer.py

Erstellen Sie eine neue Datei namens **mapper.py** , und verwenden Sie den folgenden Inhalt für die Datei:

```python
#!/usr/bin/env python

# import modules
from itertools import groupby
from operator import itemgetter
import sys

# 'file' in this case is STDIN
def read_mapper_output(file, separator='\t'):
    # Go through each line
    for line in file:
        # Strip out the separator character
        yield line.rstrip().split(separator, 1)

def main(separator='\t'):
    # Read the data using read_mapper_output
    data = read_mapper_output(sys.stdin, separator=separator)
    # Group words and counts into 'group'
    #   Since MapReduce is a distributed process, each word
    #   may have multiple counts. 'group' will have all counts
    #   which can be retrieved using the word as the key.
    for current_word, group in groupby(data, itemgetter(0)):
        try:
            # For each word, pull the count(s) for the word
            #   from 'group' and create a total count
            total_count = sum(int(count) for current_word, count in group)
            # Write to stdout
            print "%s%s%d" % (current_word, separator, total_count)
        except ValueError:
            # Count was not a number, so do nothing
            pass

if __name__ == "__main__":
    main()
```

## <a name="upload-the-files"></a>Hochladen der Dateien

Sowohl **mapper.py** als auch **reducer.py** müssen sich auf dem Stammknoten des Clusters befinden, bevor sie ausgeführt werden können. Dieser Abschnitt enthält ein Beispiel für den Befehl `scp` und ein Azure PowerShell-Skript, das zum Hochladen der Dateien in den Cluster verwendet werden kann.

> [!IMPORTANT]
> Es gibt einen wichtigen Unterschied zwischen der Verwendung von `scp` und von PowerShell zum Hochladen der Dateien:
>
> * Bei Verwenden von `scp` werden die Dateien auf dem primären Hauptknoten des Clusters abgelegt. Dies setzt voraus, dass Sie später eine Verbindung mit dem Hauptknoten herstellen und den Auftrag in einer SSH-Sitzung ausführen.
> * Bei Verwenden des PowerShell-Skripts werden die Dateien im Standardspeicher des Clusters abgelegt. Dies setzt voraus, dass Sie später ein PowerShell-Skript verwenden, um den Auftrag von einem Remoteclient aus auszuführen.

### <a name="upload-using-scp"></a>Hochladen mit SCP

Verwenden Sie in Ihrer Entwicklungsumgebung den folgenden Befehl im gleichen Verzeichnis, in dem sich auch **mapper.py** und **reducer.py** befinden. Ersetzen Sie **username** durch den SSH-Benutzernamen für Ihren Cluster und **clustername** durch den Namen Ihres Clusters.

`scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`

Dadurch werden die Dateien aus dem lokalen System auf den Stammknoten kopiert.

> [!NOTE]
> Wenn Sie zum Schutz Ihres SSH-Kontos ein Kennwort verwendet haben, werden Sie zur Eingabe dieses Kennworts aufgefordert. Wenn Sie einen SSH-Schlüssel verwendet haben, müssen Sie möglicherweise den Parameter `-i` und den Pfad zum privaten Schlüssel, z. B. `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`, angeben.

### <a name="upload-using-powershell"></a>Hochladen mithilfe von PowerShell

1. Erstellen Sie in Ihrer Entwicklungsumgebung eine neue Datei mit dem Namen `Put-FilesInHDInsight.ps1`, und verwenden Sie für die Datei den folgenden Inhalt:

    ```PowerShell
    param(
        [Parameter(Mandatory = $true)]
        [String]$clusterName,
        [Parameter(Mandatory = $true)]
        [String]$source,
        [Parameter(Mandatory = $true)]
        [String]$destination
    )
    # Login to your Azure subscription
    # Is there an active Azure subscription?
    $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
    if(-not($sub))
    {
        Add-AzureRmAccount
    }

    # Get the cluster info
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $storageInfo = $clusterInfo.DefaultStorageAccount.split('.')

    # What type of default storage are we using?
    switch ($storageInfo[1])
    {
        "blob" {
            # Get the blob storage information for the cluster
            $resourceGroup = $clusterInfo.ResourceGroup
            $storageAccountName=$storageInfo[0]
            $storageContainer=$clusterInfo.DefaultStorageContainer
            $storageAccountKey=(Get-AzureRmStorageAccountKey `
                -Name $storageAccountName `
                -ResourceGroupName $resourceGroup)[0].Value
            # Create a storage context and upload the file
            $context = New-AzureStorageContext `
                -StorageAccountName $storageAccountName `
                -StorageAccountKey $storageAccountKey
            # Upload the file
            Set-AzureStorageBlobContent `
                -File $source `
                -Blob $destination `
                -Container $storageContainer `
                -Context $context
        }
        "azuredatalakestore" {
            # Get the Data Lake Store name
            $dataLakeStoreName=$storageInfo[0]
            # Get the root of the HDInsight cluster azuredatalakestore
            $clusterRoot=$clusterInfo.DefaultStorageRootPath
            # Upload the file. Prepend the destination with the cluster root
            Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $source -Destination "$clusterRoot$destination"
        }
        default {
            Throw "Unknown storage type: $storageInfo[1]"
        }
    }
    ```

2. Um dieses Skript zum Hochladen einer Datei zu verwenden, geben Sie an einer Azure-PowerShell-Eingabeaufforderung Folgendes an:

    `.\Put-FilesInHDInsight.ps1 -clusterName <your HDInsight cluster name> -source mapper.py -destination mapper.py`

    Wenn Sie dazu aufgefordert werden, geben Sie die HTTPS-Anmeldeinformationen für Ihren Cluster ein.

3. Wiederholen Sie den Befehl, und ersetzen Sie dabei `mapper.py` durch `reducer.py`. Dadurch werden die Mapper- und Reducer-Dateien in den Standardspeicher für den Cluster hochgeladen.

## <a name="run-mapreduce-ssh"></a>Ausführen von MapReduce (SSH)

Führen Sie die folgenden Schritte aus, um eine Verbindung mit dem Cluster herzustellen, und führen Sie den MapReduce-Streamingauftrag in einer SSH-Sitzung aus.

1. Herstellen einer Verbindung zum Cluster mithilfe von SSH:
   
   `ssh username@clustername-ssh.azurehdinsight.net`
   
   > [!NOTE]
   > Wenn Sie zum Schutz Ihres SSH-Kontos ein Kennwort verwendet haben, werden Sie zur Eingabe dieses Kennworts aufgefordert. Wenn Sie einen SSH-Schlüssel verwendet haben, müssen Sie möglicherweise den Parameter `-i` und den Pfad zum privaten Schlüssel, z. B. `ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`, angeben.

2. (Optional) Falls Sie einen Text-Editor verwendet haben, der beim Erstellen der Dateien „mapper.py“ und „reducer.py“ CRLF als Zeilenende verwendet, oder nicht wissen, welches Zeilenende Ihr Editor einsetzt, konvertieren Sie CRLF-Vorkommen in „mapper.py“ und „reducer.py“ mithilfe der folgenden Befehle in LF.
   
    `perl -pi -e 's/\r\n/\n/g' mappery.py`
    `perl -pi -e 's/\r\n/\n/g' reducer.py`

3. Verwenden Sie zum Starten des MapReduce-Auftrags den folgenden Befehl.
   
    `yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input /example/data/gutenberg/davinci.txt -output /example/wordcountout`
   
    Dieser Befehl besteht aus den folgenden Komponenten:
   
   * **hadoop-streaming.jar**: Wird verwendet, wenn Streaming-MapReduce-Vorgänge ausgeführt werden. Es verbindet Hadoop mit dem von Ihnen bereitgestellten externen MapReduce-Code.

   * **-files**: Teilt Hadoop mit, dass die angegebenen Dateien für diesen MapReduce-Auftrag erforderlich sind und auf alle Arbeitsknoten kopiert werden müssen

   * **-mapper**: Teilt Hadoop mit, welche Datei als Mapper verwendet werden soll.

   * **-reducer**: Teilt Hadoop mit, welche Datei als Reducer verwendet werden soll.

   * **-input**: Die Eingabedatei, deren Wörter gezählt werden sollen.

   * **-output**: Das Verzeichnis, in das die Ausgabe geschrieben wird.
     
     > [!NOTE]
     > Dieses Verzeichnis wird durch den Auftrag erstellt.

Beim Start des Auftrags sollte eine Reihe von **INFO**-Anweisungen angezeigt werden. Abschließend werden die Vorgänge **map** und **reduce** als Prozentsätze angezeigt.

    15/02/05 19:01:04 INFO mapreduce.Job:  map 0% reduce 0%
    15/02/05 19:01:16 INFO mapreduce.Job:  map 100% reduce 0%
    15/02/05 19:01:27 INFO mapreduce.Job:  map 100% reduce 100%

Zuletzt erhalten Sie Statusinformationen zum Auftrag, wenn dieser abgeschlossen ist.

## <a name="run-mapreduce-powershell"></a>Ausführen von MapReduce (PowerShell)

Führen Sie die folgenden Schritte aus, um den MapReduce-Streamingauftrag in PowerShell in Ihrer Entwicklungsumgebung auszuführen. Das PowerShell-Skript führt den Auftrag durch Herstellen einer Verbindung mit dem HDInsight-Cluster mit WebHCat aus.

1. Erstellen Sie eine neue Datei mit dem Namen `Start-HDInsightStreamingPythonJob`, und verwenden Sie für die Datei den folgenden Inhalt:

    ```PowerShell
    param(
        [Parameter(Mandatory = $true)]
        [String]$clusterName,
        [Parameter(Mandatory = $true)]
        [String]$inputPath,
        [Parameter(Mandatory = $true)]
        [String]$outputPath
    )
    # Login to your Azure subscription
    # Is there an active Azure subscription?
    $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
    if(-not($sub))
    {
        Add-AzureRmAccount
    }

    # Get the login (HTTPS) credentials for the cluster
    $creds=Get-Credential -Message "Enter the login for the cluster" -UserName "admin"

    # Create the streaming job definition
    # Note: This assumes that the mapper.py and reducer.py
    #       are in the root of default storage. If you put them in a 
    #       subdirectory, change the -Files parameter to the correct path.
    $jobDefinition = New-AzureRmHDInsightStreamingMapReduceJobDefinition `
        -Files "/mapper.py", "/reducer.py" `
        -Mapper "mapper.py" `
        -Reducer "reducer.py" `
        -InputPath $inputPath `
        -OutputPath $outputPath

    # Start the job
    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds

    # Wait for the job to complete
    Wait-AzureRmHDInsightJob `
        -JobId $job.JobId `
        -ClusterName $clusterName `
        -HttpCredential $creds
    ```

2. Geben Sie an einer Azure-PowerShell-Eingabeaufforderung Folgendes zum Ausführen des Auftrags ein:

    `.\Start-HDInsightStreamingPythonJob.ps1 -clusterName <your HDInsight cluster name> -inputPath "/example/data/gutenberg/davinci.txt" -outputPath "/example/wordcountout"`

    Dabei werden die Dateien `mapper.py` und `reducer.py` verwendet, die zuvor in den Cluster hochgeladen wurden, um die Wörter in der Datei `davinci.txt` zu zählen. Die Ausgabe wird im Ordner `/example/wordcount` im Standardspeicher des Clusters gespeichert.

    Nachdem der Auftrag abgeschlossen ist, werden Informationen wie die folgenden zurückgegeben:

    ```
    Cluster         : myhdicluster
    HttpEndpoint    : myhdicluster.azurehdinsight.net
    State           : SUCCEEDED
    JobId           : job_1486046226168_0004
    ParentId        :
    PercentComplete : map 100% reduce 100%
    ExitValue       : 0
    User            : admin
    Callback        :
    Completed       : done
    StatusFolder    : 2017-02-06T19-14-28-a3dda3ca-f489-4287-afc9-b5e16e2e7c7a
    ```

## <a name="view-the-output"></a>Anzeigen der Ausgabe

Die Ausgabe des Auftrags wird im Verzeichnis `/example/wordcountout` gespeichert. Diese können Sie entweder in einer SSH-Sitzung anzeigen oder lokal herunterladen und in PowerShell anzeigen.

Führen Sie zum Anzeigen der Daten in einer SSH-Sitzung mit dem Cluster den folgenden Befehl aus:

`hdfs dfs -text /example/wordcountout/part-00000`

Es wird eine Liste mit Wörtern und der Häufigkeit ihres Auftretens angezeigt. Nachfolgend sehen Sie ein Beispiel für die Ausgabedaten:

```
wrenching       1
wretched        6
wriggling       1
wrinkled,       1
wrinkles        2
wrinkling       2
```

Um die Ausgabe in Ihrer Entwicklungsumgebung mithilfe von PowerShell anzuzeigen, führen Sie die folgenden Schritte aus:

1. Erstellen Sie eine neue Datei mit dem Namen `Get-FilesInHDInsight.ps1`, und verwenden Sie für die Datei den folgenden Inhalt:

    ```PowerShell
    param(
        [Parameter(Mandatory = $true)]
        [String]$clusterName,
        [Parameter(Mandatory = $true)]
        [String]$source
    )
    # Login to your Azure subscription
    # Is there an active Azure subscription?
    $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
    if(-not($sub))
    {
        Add-AzureRmAccount
    }

    # Get the cluster info
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $storageInfo = $clusterInfo.DefaultStorageAccount.split('.')

    # What type of default storage are we using?
    switch ($storageInfo[1])
    {
        "blob" {
            # Get the blob storage information for the cluster
            $resourceGroup = $clusterInfo.ResourceGroup
            $storageAccountName=$storageInfo[0]
            $storageContainer=$clusterInfo.DefaultStorageContainer
            $storageAccountKey=(Get-AzureRmStorageAccountKey `
                -Name $storageAccountName `
                -ResourceGroupName $resourceGroup)[0].Value
            # Create a storage context and download the file
            $context = New-AzureStorageContext `
                -StorageAccountName $storageAccountName `
                -StorageAccountKey $storageAccountKey
            # Download the file
            Get-AzureStorageBlobContent `
                -Container $storageContainer `
                -Blob $source `
                -Context $context `
                -Destination "./output.txt"
            # Display the output
            Get-Content "./output.txt"
        }
        "azuredatalakestore" {
            # Get the Data Lake Store name
            $dataLakeStoreName=$storageInfo[0]
            # Get the root of the HDInsight cluster azuredatalakestore
            $clusterRoot=$clusterInfo.DefaultStorageRootPath
            # Download the file. Prepend the destination with the cluster root
            # NOTE: Unlike getting a blob, this just gets the content and no
            #       file is created locally.
            Get-AzureRmDataLakeStoreItemContent -Account $dataLakeStoreName -Path $clusterRoot$source -Confirm
        }
        default {
            Throw "Unknown storage type: $storageInfo[1]"
        }
    }
    ```

2. Geben Sie an einer Azure PowerShell-Eingabeaufforderung Folgendes an, um das Skript auszuführen und die Ausgabe anzuzeigen:

    `Get-FilesInHDInsight.ps1 -clusterName <your HDInsight cluster name> -source "example/wordcountout/part-00000"`

    Es wird eine Liste mit Wörtern und der Häufigkeit ihres Auftretens angezeigt. Nachfolgend sehen Sie ein Beispiel für die Ausgabedaten:

    ```
    wrenching       1
    wretched        6
    wriggling       1
    wrinkled,       1
    wrinkles        2
    wrinkling       2
    ```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie Sie Streaming-MapReduce-Aufträge mit HDInsight verwenden, können Sie mithilfe der nachfolgenden Links andere Möglichkeiten für die Arbeit mit Azure HDInsight untersuchen.

* [Verwenden von Hive mit HDInsight](hdinsight-use-hive.md)
* [Verwenden von Pig mit HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce-Aufträgen mit HDInsight](hdinsight-use-mapreduce.md)


