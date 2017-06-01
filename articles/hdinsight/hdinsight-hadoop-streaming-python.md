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
ms.date: 05/03/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: 7c4d5e161c9f7af33609be53e7b82f156bb0e33f
ms.openlocfilehash: a33a2b64bba917f186437f071cb0ee2326154879
ms.contentlocale: de-de
ms.lasthandoff: 05/04/2017


---
# <a name="develop-python-streaming-programs-for-hdinsight"></a>Entwickeln von Python-Streamingprogrammen für HDInsight

Erfahren Sie, wie Sie Python in MapReduce-Vorgängen verwenden. Hadoop stellt eine Streaming-API für MapReduce zur Verfügung, mit der Sie Map- und Reduce-Funktionen in anderen Sprache als Java schreiben können. Die Schritte in diesem Dokument haben die Komponenten für Mapper und Reducer in Python implementiert.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Linux-basierter Hadoop-Cluster in HDInsight

  > [!IMPORTANT]
  > Die Schritte in diesem Dokument erfordern einen HDInsight-Cluster mit Linux. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [HDInsight-Komponentenversionen](hdinsight-component-versioning.md#hdi-version-33-nearing-deprecation-date).

* Ein Texteditor

  > [!IMPORTANT]
  > Im Text-Editor muss LF als Zeilenende verwendet werden. Wenn ein Zeilenende von CRLF verwendet wird, verursacht dies beim Ausführen des MapReduce-Auftrags auf Linux-basierten HDInsight-Clustern Fehler.

* Die Befehle `ssh` und `scp` oder [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-3.8.0)

## <a name="word-count"></a>Wortzählung

Dieses Beispiel ist eine einfache Wortzahlermittlung, das in eine Mapper- und Reducer-Funktion von Python implementiert wird. Die Mapper-Funktion teilt Sätze in einzelne Wörter auf und die Reducer-Funktion aggregiert die Wörter und Zähler, um die Ausgabe zu erzeugen.

Im folgenden Flußdiagramm ist dargestellt, was in den Mapper- und Reducer-Funktionen geschieht.

![Darstellung des MapReduce-Prozesses](./media/hdinsight-hadoop-streaming-python/HDI.WordCountDiagram.png)

## <a name="streaming-mapreduce"></a>Streaming-MapReduce

Mit Hadoop können Sie eine Datei angeben, die die von einem Auftrag verwendete Logik für den Mapper und Reducer enthält. Bestimmte Anforderungen an die Logik für Mapper und Reducer:

* **Eingabe**: Die Komponenten für Mapper und Reducer müssen Eingabedaten von STDIN lesen.
* **Ausgabe**: Die Komponenten für Mapper und Reducer müssen Ausgabedaten an STDOUT schreiben.
* **Datenformat**: Die verwendeten und erzeugten Daten müssen ein Schlüssel-/Wertpaar darstellen, das durch ein Tabulatorzeichen getrennt werden muss.

Python kann diese Anforderungen einfach mithilfe des Moduls `sys` zum Lesen von STDIN und mithilfe des Moduls `print` für die Ausgabe an STDOUT behandeln. Der Rest besteht einfach aus der Formatierung der Daten mit einem Tabulatorzeichen (`\t`) zwischen Schlüssel und Wert.

## <a name="create-the-mapper-and-reducer"></a>Erstellen von Mapper und Reducer

1. Erstellen Sie eine Datei mit dem Namen `mapper.py`, und verwenden Sie den folgenden Inhalt für die Datei:

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

2. Erstellen Sie eine Datei mit dem Namen **reducer.py**, und verwenden Sie den folgenden Inhalt für die Datei:

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

## <a name="run-using-powershell"></a>Ausführen mithilfe von PowerShell

Stellen Sie mithilfe des folgenden PowerShell-Skripts sicher, dass Ihre Dateien über die richtigen Zeilenenden verfügen:

```powershell
# Set $original_file to the Python file name
$text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
[IO.File]::WriteAllText($original_file, $text)
```

Verwenden Sie das folgende PowerShell-Skript, um Dateien hochzuladen, den Auftrag auszuführen und die Ausgabe anzuzeigen:

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
# Get the login (HTTPS) credentials for the cluster
$creds=Get-Credential -Message "Enter the login for the cluster" -UserName "admin"
$clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$storageInfo = $clusterInfo.DefaultStorageAccount.split('.')
$defaultStoreageType = $storageInfo[1]
$defaultStorageName = $storageInfo[0]

# Progress indicator
$activity="Python MapReduce"
Write-Progress -Activity $activity -Status "Uploading mapper and reducer..."

# Upload the files
switch ($defaultStoreageType)
{
    "blob" {
        # Get the blob storage information for the cluster
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageContainer=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $defaultStorageName `
            -ResourceGroupName $resourceGroup)[0].Value
        # Create a storage context and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $defaultStorageName `
            -StorageAccountKey $storageAccountKey
        # Upload the mapper.py file
        Set-AzureStorageBlobContent `
            -File .\mapper.py `
            -Blob "mapper.py" `
            -Container $storageContainer `
            -Context $context
        # Upload the reducer.py file
        Set-AzureStorageBlobContent `
            -File .\reducer.py `
            -Blob "reducer.py" `
            -Container $storageContainer `
            -Context $context `
    }
    "azuredatalakestore" {
        # Get the Data Lake Store name
        # Get the root of the HDInsight cluster azuredatalakestore
        $clusterRoot=$clusterInfo.DefaultStorageRootPath
        # Upload the files. Prepend the destination with the cluster root
        Import-AzureRmDataLakeStoreItem -AccountName $defaultStorageName `
            -Path .\mapper.py `
            -Destination "$clusterRoot/mapper.py" `
            -Force
        Import-AzureRmDataLakeStoreItem -AccountName $defaultStorageName `
            -Path .\reducer.py `
            -Destination "$clusterRoot/reducer.py" `
            -Force
    }
    default {
        Throw "Unknown storage type: $defaultStoreageType"
    }
}

# Create the streaming job definition
# Note: This assumes that the mapper.py and reducer.py
#       are in the root of default storage. If you put them in a
#       subdirectory, change the -Files parameter to the correct path.
$jobDefinition = New-AzureRmHDInsightStreamingMapReduceJobDefinition `
    -Files "/mapper.py", "/reducer.py" `
    -Mapper "mapper.py" `
    -Reducer "reducer.py" `
    -InputPath "/example/data/gutenberg/davinci.txt" `
    -OutputPath "/example/wordcountout"

# Start the job
Write-Progress -Activity $activity -Status "Starting the MapReduce job..."
$job = Start-AzureRmHDInsightJob `
    -ClusterName $clusterName `
    -JobDefinition $jobDefinition `
    -HttpCredential $creds

# Wait for the job to complete
Write-Progress -Activity $activity -Status "Waiting for the job to complete..."
Wait-AzureRmHDInsightJob `
    -JobId $job.JobId `
    -ClusterName $clusterName `
    -HttpCredential $creds

# Display the results of the job
Write-Progress -Activity $activity -Status "Downloading job output..."
switch ($defaultStoreageType)
{
    "blob" {
        # Get the blob storage information for the cluster
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageContainer=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $defaultStorageName `
            -ResourceGroupName $resourceGroup)[0].Value
        # Create a storage context and download the file
        $context = New-AzureStorageContext `
            -StorageAccountName $defaultStorageName `
            -StorageAccountKey $storageAccountKey
        # Download the file
        Get-AzureStorageBlobContent `
            -Container $storageContainer `
            -Blob "example/wordcountout/part-00000" `
            -Context $context `
            -Destination "./output.txt"
        # Display the output
        Get-Content "./output.txt"
    }
    "azuredatalakestore" {
        # Get the Data Lake Store name
        # Get the root of the HDInsight cluster azuredatalakestore
        $clusterRoot=$clusterInfo.DefaultStorageRootPath
        # Download the file. Prepend the destination with the cluster root
        # NOTE: Unlike getting a blob, this just gets the content and no
        #       file is created locally.
        $sourcePath=$clusterRoot + "example/wordcountout/part-00000"
        Get-AzureRmDataLakeStoreItemContent -Account $defaultStorageName -Path $sourcePath -Confirm
    }
    default {
        Throw "Unknown storage type: $defaultStoreageType"
    }
}
```

## <a name="run-from-an-ssh-session"></a>Ausführen in einer SSH-Sitzung

1. Verwenden Sie in Ihrer Entwicklungsumgebung den folgenden Befehl im gleichen Verzeichnis, in dem sich auch `mapper.py` und `reducer.py` befinden:

    ```bash
    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:
    ```

    Ersetzen Sie `username` durch den SSH-Benutzernamen für Ihren Cluster und `clustername` durch den Namen Ihres Clusters.

    Dieser Befehl kopiert die Dateien aus dem lokalen System auf den Stammknoten.

    > [!NOTE]
    > Wenn Sie zum Schutz Ihres SSH-Kontos ein Kennwort verwendet haben, werden Sie zur Eingabe dieses Kennworts aufgefordert. Wenn Sie einen SSH-Schlüssel verwendet haben, müssen Sie möglicherweise den Parameter `-i` und den Pfad zum privaten Schlüssel, z. B. `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`, angeben.

2. Herstellen einer Verbindung zum Cluster mithilfe von SSH:

    ```bash
    ssh username@clustername-ssh.azurehdinsight.net`
    ```

    Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

3. Um sicherzustellen, dass „mapper.py“ und „reducer.py“ über die richtigen Zeilenenden verfügen, verwenden Sie folgende Befehle:

    ```bash
    perl -pi -e 's/\r\n/\n/g' mappery.py
    perl -pi -e 's/\r\n/\n/g' reducer.py
    ```

4. Verwenden Sie zum Starten des MapReduce-Auftrags den folgenden Befehl.

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
    ```

    Dieser Befehl besteht aus den folgenden Komponenten:

   * **hadoop-streaming.jar**: Wird verwendet, wenn Streaming-MapReduce-Vorgänge ausgeführt werden. Es verbindet Hadoop mit dem von Ihnen bereitgestellten externen MapReduce-Code.

   * **-files**: Fügt die bestimmten Dateien dem MapReduce-Auftrag hinzu.

   * **-mapper**: Teilt Hadoop mit, welche Datei als Mapper verwendet werden soll.

   * **-reducer**: Teilt Hadoop mit, welche Datei als Reducer verwendet werden soll.

   * **-input**: Die Eingabedatei, deren Wörter gezählt werden sollen.

   * **-output**: Das Verzeichnis, in das die Ausgabe geschrieben wird.

    Sobald der MapReduce-Auftrag ausgeführt wird, wird der Prozess als Prozentsatz dargestellt.

        15/02/05 19:01:04 INFO mapreduce.Job:  map 0% reduce 0%    15/02/05 19:01:16 INFO mapreduce.Job:  map 100% reduce 0%    15/02/05 19:01:27 INFO mapreduce.Job:  map 100% reduce 100%


5. Verwenden Sie den folgenden Befehl, um die Ausgabe anzuzeigen:

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    Dieser Befehl zeigt eine Liste mit Wörtern und der Häufigkeit ihres Auftretens.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie Sie Streaming-MapReduce-Aufträge mit HDInsight verwenden, können Sie mithilfe der nachfolgenden Links andere Möglichkeiten für die Arbeit mit Azure HDInsight untersuchen.

* [Verwenden von Hive mit HDInsight](hdinsight-use-hive.md)
* [Verwenden von Pig mit HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce-Aufträgen mit HDInsight](hdinsight-use-mapreduce.md)

