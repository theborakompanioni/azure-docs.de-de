---
title: Verwenden von Python mit Hive und Pig in HDInsight | Microsoft Docs
description: "Erfahren Sie, wie Sie benutzerdefinierte Python-Funktionen mit Hive und Pig in HDInsight, dem Hadoop-Technologiestapel in Azure, verwenden können."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c44d6606-28cd-429b-b535-235e8f34a664
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 09/07/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: ecd33fdad27cb5bb13931075f4c603f9bd479207


---
# <a name="use-python-with-hive-and-pig-in-hdinsight"></a>Verwenden von Python mit Hive und Pig in HDInsight
Hive und Pig sind großartig für die Arbeit mit Daten in HDInsight. Manchmal benötigt man aber eine allgemeinere Sprache. Sowohl Hive als auch Pig ermöglichen Ihnen die Erstellung benutzerdefinierter Funktionen (User Defined Functions, UDF) mithilfe einer Vielzahl von Programmiersprachen. In diesem Artikel erfahren Sie, wie Sie eine Python-UDF von Hive und Pig aus verwenden.

## <a name="requirements"></a>Anforderungen
* Ein HDInsight-Cluster (Windows- oder Linux-basiert)
* Ein Texteditor
  
  > [!IMPORTANT]
  > Wenn Sie einen Linux-basierten HDInsight-Server verwenden, aber die Python-Dateien auf einem Windows-Client erstellen, müssen Sie einen Editor verwenden, der als Zeilenende LF verwendet. Wenn Sie nicht sicher sind, ob der Editor LF oder CRLF verwendet, finden Sie im Abschnitt [Problembehandlung](#troubleshooting) Schritte für das Entfernen des CR-Zeichens mithilfe der Hilfsprogramme im HDInsight-Cluster.
  > 
  > 

## <a name="a-namepythonapython-on-hdinsight"></a><a name="python"></a>Python in HDInsight
Python 2.7 wird auf Clustern der Version HDInsight 3.0 und höher standardmäßig installiert. Hive kann mit dieser Version von Python für die Streamverarbeitung verwendet werden (Daten werden zwischen Hive und Python mithilfe von STDOUT/STDIN weitergegeben).

HDInsight enthält außerdem Jython, eine in Java geschriebene Python-Implementierung. Pig kann ohne Streaming mit Jython kommunizieren, weshalb es bei der Arbeit mit Pig vorzuziehen ist. Allerdings können Sie auch normales Python (C-Python) verwenden, auch mit Pig.

## <a name="a-namehivepythonahive-and-python"></a><a name="hivepython"></a>Hive und Python
Python kann als UDF von Hive aus mittels der Hive QL **TRANSFORM** -Anweisung verwendet werden. Das folgende HiveQL ruft beispielsweise ein Python-Skript auf, das in der Datei **streaming.py** gespeichert ist.

**HDInsight (Linux-basiert)**

    add file wasbs:///streaming.py;

    SELECT TRANSFORM (clientid, devicemake, devicemodel)
      USING 'python streaming.py' AS
      (clientid string, phoneLable string, phoneHash string)
    FROM hivesampletable
    ORDER BY clientid LIMIT 50;

**HDInsight (Windows-basiert)**

    add file wasbs:///streaming.py;

    SELECT TRANSFORM (clientid, devicemake, devicemodel)
      USING 'D:\Python27\python.exe streaming.py' AS
      (clientid string, phoneLable string, phoneHash string)
    FROM hivesampletable
    ORDER BY clientid LIMIT 50;

> [!NOTE]
> In Windows-basierten HDInsight-Clustern muss mit der **USING** -Klausel der vollständige Pfad zu "python.exe" angegeben werden. Dies ist immer `D:\Python27\python.exe`.
> 
> 

Das Beispiel bewirkt Folgendes:

1. Die Anweisung **add file** am Anfang der Datei fügt die Datei **streaming.py** zum verteilten Cache hinzu, sodass sie von allen Knoten im Cluster aus zugänglich ist.
2. Die Anweisung **SELECT TRANSFORM ... Mit der USING**-Anweisung werden Daten aus **hivesampletable** ausgewählt und „clientid“, „devicemake“ und „devicemodel“ an das Skript **streaming.py** übergeben.
3. Die **AS**-Klausel beschreibt die Felder, die von **streaming.py** zurückgegeben werden.

<a name="streamingpy"></a> Dies ist die Datei **streaming.py** , die im HiveQL-Beispiel verwendet wird.

    #!/usr/bin/env python

    import sys
    import string
    import hashlib

    while True:
      line = sys.stdin.readline()
      if not line:
        break

      line = string.strip(line, "\n ")
      clientid, devicemake, devicemodel = string.split(line, "\t")
      phone_label = devicemake + ' ' + devicemodel
      print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])

Da wir Streaming verwenden, muss das Skript Folgendes tun:

1. Lesen der Daten von STDIN. Das wird durch Verwendung von `sys.stdin.readline()` in diesem Beispiel erreicht.
2. Das Zeilenumbruchzeichen am Ende wird mit `string.strip(line, "\n ")`entfernt, da wir nur die Textdaten und keinen Indikator für das Zeilenende benötigen.
3. Bei der Streamverarbeitung enthält eine einzelne Zeile alle Werte, jeweils getrennt durch ein Tabulatorzeichen. Deshalb kann `string.split(line, "\t")` zum Unterteilen der Eingabe bei jedem Tabstopp verwendet werden, sodass nur die Felder zurückgeben werden.
4. Wenn die Verarbeitung abgeschlossen ist, muss die Ausgabe als eine Zeile nach STDOUT geschrieben werden mit einem Tabulator zwischen jedem Feld. Dies wird mithilfe von `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])` erreicht.
5. All das ereignet sich in einer `while`-Schleife, die sich wiederholt, bis keine `line` mehr gelesen werden; dann verlässt `break` die Schleife, und das Skript wird beendet.

Danach verkettet das Skript einfach die Eingabewerte für `devicemake` und `devicemodel` und berechnet einen Hash der verketteten Werte. Das ist ziemlich einfach, beschreibt aber das Grundkonzept, nach dem jedes von Hive aufgerufene Python-Skript funktionieren sollte: Schleife ausführen, Eingabe gelesen, bis keine weiteren Daten mehr vorhanden sind, jede Zeile der Eingabe an den Tabstopps unterteilen, Eingabe verarbeiten, eine einzelne Ausgabezeile mit Tabstopptrennzeichen schreiben.

Wie Sie dieses Beispiel auf Ihrem HDInsight-Cluster ausführen, erfahren Sie unter [Ausführen der Beispiele](#running) .

## <a name="a-namepigpythonapig-and-python"></a><a name="pigpython"></a>Pig und Python
Ein Python-Skript kann als UDF von Pig aus mittels der **GENERATE** -Anweisung verwendet werden. Es gibt zwei Möglichkeiten, dies zu erreichen – mithilfe von Jython (Python auf der Java Virtual Machine implementiert) und C Python (reguläres Python).

Der Hauptunterschied liegt darin, dass Jython auf der JVM ausgeführt wird und nativ von Pig (auch auf der JVM ausgeführt) aufgerufen werden kann. C-Python ist ein (in C geschriebener) externer Prozess. Die Daten aus Pig auf der JVM werden an das Skript gesendet, das in einem Python-Prozess ausgeführt wird, und dann wird die Ausgabe davon in Pig zurückgesendet.

Um zu bestimmen, ob Pig Jython oder C-Python verwendet, um das Skript auszuführen, verwenden Sie beim Verweis auf das Python-Skript aus Pig Latin **register**. So erfährt Pig, welcher Interpreter verwendet und welcher Alias für das Skript erstellt werden muss. In den folgenden Beispielen werden Skripts mit Pig als **myfuncs** registriert:

* **Verwendung von Jython**: `register '/path/to/pig_python.py' using jython as myfuncs;`
* **Verwendung von C Python**: `register '/path/to/pig_python.py' using streaming_python as myfuncs;`

> [!IMPORTANT]
> Bei der Verwendung von Jython kann der Pfad zur Datei „pig_jython“ entweder ein lokaler oder ein „WASB://“-Pfad sein. Jedoch müssen Sie bei der Verwendung von C-Python auf eine Datei auf dem lokalen Dateisystem des Knotens verweisen, den Sie verwenden, um den Pig-Job zu übermitteln.
> 
> 

Nach der Registrierung ist Pig Latin in diesem Beispiel für beides identisch:

    LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
    LOG = FILTER LOGS by LINE is not null;
    DETAILS = FOREACH LOG GENERATE myfuncs.create_structure(LINE);
    DUMP DETAILS;

Das Beispiel bewirkt Folgendes:

1. Die erste Zeile lädt die Datei mit den Beispieldaten, **sample.log**, in **LOGS**. Da diese Protokolldatei kein gleichbleibendes Schema hat, definiert sie außerdem jeden Datensatz (in diesem Fall **LINE**) als ein **chararray**. Chararray ist im Grunde genommen eine Zeichenfolge.
2. Die nächste Zeile filtert etwaige NULL-Werte heraus und speichert die Ergebnisse des Vorgangs in **LOG**.
3. Dann folgt eine Iteration über die Datensätze in **LOG** und **GENERATE** zum Aufruf der Methode **create_structure**, die im Python/Jython-Skript enthalten ist und als **myfuncs** geladen wird.  **LINE** wird für die Übergabe des aktuellen Datensatzes an die Funktion verwendet.
4. Schließlich werden die ausgegebenen Daten mit dem Befehl **DUMP** in STDOUT geschrieben. Das geschieht nur, damit die Ergebnisse direkt nach dem Ende des Vorgangs angezeigt werden; in einem echten Skript würde man die Daten normalerweise in einer neuen Datei speichern ( **STORE** ).

Die eigentlichen Python-Skriptdateien bei CPython und Jython sind sich recht ähnlich. Der einzige wirkliche Unterschied besteht darin, dass bei Verwendung von CPython ein Import aus **pig\_util** durchgeführt werden muss. Hier ist das **pig\_python.py**-Skript:

<a name="streamingpy"></a>

    # Uncomment the following if using C Python
    #from pig_util import outputSchema

    @outputSchema("log: {(date:chararray, time:chararray, classname:chararray, level:chararray, detail:chararray)}")
    def create_structure(input):
    if (input.startswith('java.lang.Exception')):
        input = input[21:len(input)] + ' - java.lang.Exception'
    date, time, classname, level, detail = input.split(' ', 4)
    return date, time, classname, level, detail

> [!NOTE]
> Über die Installation von „pig_util“ müssen Sie sich keine Gedanken machen; es ist automatisch für das Skript verfügbar.
> 
> 

Sie erinnern sich daran, dass wir vorhin die **LINE** -Eingabe einfach als Chararray definiert haben, denn es gab kein gleichbleibendes Schema für die Eingabe. Das Python-Skript transformiert die Daten für die Ausgabe in ein gleichbleibendes Schema. Das funktioniert folgendermaßen:

1. Die Anweisung **@outputSchema** definiert das Format der Daten, die an Pig zurückgegeben werden. In diesem Fall ist das ein **Datenbehälter**, also ein Pig-Datentyp. Der Behälter enthält folgende Felder, die alle Chararray (Zeichenfolgen) sind:
   
   * date – das Datum, an dem der Protokolleintrag erstellt wurde
   * time – die Zeit, zu der der Protokolleintrag erstellt wurde
   * classname – der Klassenname, für den der Eintrag erstellt wurde
   * level – die Protokollierungsebene
   * detail – ausführliche Details des Protokolleintrags
2. Dann definiert **def create_structure(input)** die Funktion, an die Pig Positionen übergibt.
3. Die Beispieldatei **sample.log**entspricht weitgehend dem Schema für Datum, Uhrzeit, Klassenname, Ebene und Detail, das wir zurückgeben möchten. Sie enthält aber auch ein paar Zeilen, die mit der Zeichenfolge '*java.lang.Exception*' beginnen, die geändert werden muss, damit sie dem Schema entspricht. Die Anweisung **if** überprüft deren Vorhandensein, weist dann die Eingabedaten an, die Zeichenfolge '*java.lang.Exception*' ans Ende zu stellen, sodass die Daten dem erwarteten Ausgabeschema entsprechen.
4. Als Nächstes wird der Befehl **split** zum Aufteilen der Daten bei den ersten vier Leerzeichen verwendet. Das führt zu fünf Werten, die **date**, **time**, **classname**, **level** und **detail** zugewiesen werden.
5. Zuletzt werden die Werte an Pig zurückgegeben.

Wenn die Daten an Pig zurückgegeben werden, haben sie ein gleichbleibendes Schema wie in der Anweisung **@outputSchema** -Anweisung verwendet werden.

## <a name="a-namerunningarunning-the-examples"></a><a name="running"></a>Ausführen der Beispiele
Wenn Sie einen Linux-basierten HDInsight-Cluster verwenden, führen Sie die unten im Abschnitt **SSH** beschriebenen Schritte aus. Wenn Sie einen Windows-basierten HDInsight-Cluster und einen Windows-Client verwenden, führen Sie die im Abschnitt **PowerShell** beschriebenen Schritte aus.

### <a name="ssh"></a>SSH
Weitere Informationen zur Verwendung von SSH finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix und OS X](hdinsight-hadoop-linux-use-ssh-unix.md) oder [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md).

1. Erstellen Sie mithilfe der Python-Beispiele [streaming.py](#streamingpy) und [pig_python.py](#jythonpy) lokale Kopien der Dateien auf Ihrem Entwicklungscomputer.
2. Verwenden Sie `scp` , um die Dateien in Ihren HDInsight-Cluster zu kopieren. Beispielsweise wird die Datei wie folgt in einen Cluster namens **mycluster**kopiert:
   
        scp streaming.py pig_python.py myuser@mycluster-ssh.azurehdinsight.net:
3. Verwenden Sie SSH zum Herstellen einer Verbindung mit dem Cluster. Beispielsweise wird über den folgenden Code der Benutzer **myuser** mit einem Cluster namens **mycluster** verbunden.
   
        ssh myuser@mycluster-ssh.azurehdinsight.net
4. Fügen Sie die Python-Dateien, die zuvor hochgeladen wurden, in der SSH-Sitzung dem WASB-Speicher für den Cluster hinzu.
   
        hdfs dfs -put streaming.py /streaming.py
        hdfs dfs -put pig_python.py /pig_python.py

Nach dem Hochladen der Dateien, gehen Sie folgendermaßen vor, um die Hive- und Pig-Aufträge auszuführen.

#### <a name="hive"></a>Hive
1. Verwenden Sie den `hive` -Befehl, um die Hive-Shell starten. Nachdem die Shell geladen wurde, sollte eine `hive>` -Eingabeaufforderung angezeigt werden.
2. Geben Sie in der `hive>` -Befehlszeile Folgendes ein:
   
        add file wasbs:///streaming.py;
        SELECT TRANSFORM (clientid, devicemake, devicemodel)
          USING 'python streaming.py' AS
          (clientid string, phoneLabel string, phoneHash string)
        FROM hivesampletable
        ORDER BY clientid LIMIT 50;
3. Nach Eingabe der letzten Zeile sollte der Auftrag gestartet werden. Die Ausgabe sollte ähnliche Ergebnisse wie diese liefern.
   
        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="pig"></a>Pig
1. Verwenden Sie den `pig` -Befehl, um die Shell starten. Nachdem die Shell geladen wurde, sollte eine `grunt>` -Eingabeaufforderung angezeigt werden.
2. Geben Sie die folgenden Anweisungen an der `grunt>` -Eingabeaufforderung ein, um das Python-Skript mit dem Jython-Interpreter auszuführen.
   
        Register wasbs:///pig_python.py using jython as myfuncs;
        LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
        DUMP DETAILS;
3. Nach Eingabe der folgenden Zeile sollte der Auftrag gestartet werden. Die Ausgabe sollte ähnliche Ergebnisse wie diese liefern.
   
        ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
        ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
        ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
        ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
        ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))
4. Verwenden Sie `quit` zum Beenden der Grunt-Shell und dann Folgendes zum Bearbeiten der Datei „pig_python.py“ auf dem lokalen Dateisystem:
   
    nano pig_python.py
5. Heben Sie die Kommentierung der folgenden Zeilen auf, indem Sie das Zeichen `#` vom Zeilenanfang entfernen:
   
        #from pig_util import outputSchema
   
    Nachdem die Änderung vorgenommen wurde, beenden Sie den Editor mit STRG+X. Wählen Sie Y und dann EINGABE, um die Änderungen zu speichern.
6. Verwenden Sie den `pig` -Befehl, um die Shell neu zu starten. Geben Sie Folgendes an der `grunt>` -Eingabeaufforderung ein, um das Python-Skript mit dem C-Python-Interpreter auszuführen.
   
        Register 'pig_python.py' using streaming_python as myfuncs;
        LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
        DUMP DETAILS;
   
    Wenn dieser Auftrag abgeschlossen ist, sollte die Ausgabe derjenigen bei der vorherigen Skriptausführung mit Jython entsprechen.

### <a name="powershell"></a>PowerShell
In diesen Schritten wird Microsoft Azure PowerShell verwendet. Wen es nicht bereits auf Ihrem Entwicklungsrechner installiert und konfiguriert ist, sehen Sie sich bitte [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) an und folgen Sie den Schritten dort.

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

1. Erstellen Sie mithilfe der Python-Beispiele [streaming.py](#streamingpy) und [pig_python.py](#jythonpy) lokale Kopien der Dateien auf Ihrem Entwicklungscomputer.
2. Verwenden Sie das folgende PowerShell-Skript zum Hochladen der Dateien **streaming.py** und **pig\_python.py** auf den Server. Ersetzen Sie den Namen Ihres Azure HDInsight-Clusters und den Pfad zu den Dateien **streaming.py** und **pig\_python.py** in den ersten drei Skriptzeilen.
   
        $clusterName = YourHDIClusterName
        $pathToStreamingFile = "C:\path\to\streaming.py"
        $pathToJythonFile = "C:\path\to\pig_python.py"
   
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
   
        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
   
        Set-AzureStorageBlobContent `
            -File $pathToStreamingFile `
            -Blob "streaming.py" `
            -Container $container `
            -Context $context
   
        Set-AzureStorageBlobContent `
            -File $pathToJythonFile `
            -Blob "pig_python.py" `
            -Container $container `
            -Context $context
   
    Dieses Skript ruft Informationen für Ihren HDInsight-Cluster ab, extrahiert dann das Konto und den Schlüssel für das standardmäßigen Speicherkonto und lädt die Dateien in den Stamm des Containers.
   
   > [!NOTE]
   > Andere Methoden zum Hochladen der Skripts finden sich im Dokument [Hochladen von Daten für Hadoop-Aufträge in HDInsight](hdinsight-upload-data.md) .
   > 
   > 

Verwenden Sie nach dem Hochladen der Dateien die folgenden PowerShell-Skripts zum Starten der Jobs. Wenn der Job abgeschlossen ist, sollte die Ausgabe zur PowerShell-Konsole geschrieben werden.

#### <a name="hive"></a>Hive
Das folgende Skript führt das Skript **streaming.py** aus. Vor dem Ausführen werden Sie nach den HTTPS-/Administratorkontoinformationen für Ihren HDInsight-Cluster gefragt.

    # Replace 'YourHDIClusterName' with the name of your cluster
    $clusterName = YourHDIClusterName
    $creds=Get-Credential
    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    $storageAccountKey=(Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
    #Create a storage content and upload the file
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey

    # If using a Windows-based HDInsight cluster, change the USING statement to:
    # "USING 'D:\Python27\python.exe streaming.py' AS " +
    $HiveQuery = "add file wasbs:///streaming.py;" +
                 "SELECT TRANSFORM (clientid, devicemake, devicemodel) " +
                   "USING 'python streaming.py' AS " +
                   "(clientid string, phoneLabel string, phoneHash string) " +
                 "FROM hivesampletable " +
                 "ORDER BY clientid LIMIT 50;"

    $jobDefinition = New-AzureRmHDInsightHiveJobDefinition `
        -Query $HiveQuery

    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds
    Write-Host "Wait for the Hive job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
        -JobId $job.JobId `
        -ClusterName $clusterName `
        -HttpCredential $creds
    # Uncomment the following to see stderr output
    # Get-AzureRmHDInsightJobOutput `
    #   -Clustername $clusterName `
    #   -JobId $job.JobId `
    #   -DefaultContainer $container `
    #   -DefaultStorageAccountName $storageAccountName `
    #   -DefaultStorageAccountKey $storageAccountKey `
    #   -HttpCredential $creds `
    #   -DisplayOutputType StandardError
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -DefaultContainer $container `
        -DefaultStorageAccountName $storageAccountName `
        -DefaultStorageAccountKey $storageAccountKey `
        -HttpCredential $creds

Die Ausgabe für den **Hive** -Job sollte ungefähr wie folgt aussehen:

    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="pig-jython"></a>Pig (Jython)
Im Folgenden wird das Skript **pig_python.py** mit dem Jython-Interpreter verwendet. Vor dem Ausführen werden Sie nach den HTTPS-/Administratorinformationen für den HDInsight-Cluster gefragt.

> [!NOTE]
> Wenn Sie einen Auftrag mithilfe von PowerShell remote übermitteln, können Sie C-Python nicht als Interpreter verwenden.
> 
> 

    # Replace 'YourHDIClusterName' with the name of your cluster
    $clusterName = YourHDIClusterName

    $creds = Get-Credential
    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    $storageAccountKey=(Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value

    #Create a storage content and upload the file
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey

    $PigQuery = "Register wasbs:///jython.py using jython as myfuncs;" +
                "LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);" +
                "LOG = FILTER LOGS by LINE is not null;" +
                "DETAILS = foreach LOG generate myfuncs.create_structure(LINE);" +
                "DUMP DETAILS;"

    $jobDefinition = New-AzureRmHDInsightPigJobDefinition -Query $PigQuery

    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds

    Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
        -Job $job.JobId `
        -ClusterName $clusterName `
        -HttpCredential $creds
    # Uncomment the following to see stderr output
    # Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -DefaultContainer $container `
        -DefaultStorageAccountName $storageAccountName `
        -DefaultStorageAccountKey $storageAccountKey `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -DefaultContainer $container `
        -DefaultStorageAccountName $storageAccountName `
        -DefaultStorageAccountKey $storageAccountKey `
        -HttpCredential $creds

Die Ausgabe für den **Pig** -Job sollte ungefähr wie folgt aussehen:

    ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
    ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
    ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
    ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
    ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

## <a name="a-nametroubleshootingatroubleshooting"></a><a name="troubleshooting"></a>Problembehandlung
### <a name="errors-when-running-jobs"></a>Fehler beim Ausführen von Aufträgen
Bei der Ausführung des Hive-Auftrags können ähnliche Fehler wie die folgenden auftreten:

    Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: [Error 20001]: An error occurred while reading or writing to your custom script. It may have crashed with an error.

Dieses Problem kann durch die Zeilenenden in der Datei „streaming.py“ verursacht werden. Viele Windows-Editoren verwenden als Zeilenende standardmäßig CRLF, Linux-Anwendung erwarten jedoch i. d. R. LF.

Wenn Sie einen Editor verwenden, der keine Zeilenenden mit LF erstellen kann, oder wenn Sie sich nicht sicher sind, welche Zeilenenden verwendet werden, können Sie mit den folgenden PowerShell-Anweisungen die CR-Zeichen entfernen, bevor Sie die Datei in HDInsight hochladen:

    $original_file ='c:\path\to\streaming.py'
    $text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
    [IO.File]::WriteAllText($original_file, $text)

### <a name="powershell-scripts"></a>PowerShell-Skripts
Beide für das Ausführen der Beispiele verwendete PowerShell-Beispielskripte enthalten eine Kommentarzeile, die Fehler bei der Ausgabe des Jobs anzeigt. Wenn Sie nicht die erwartete Ausgabe für den Job sehen, kommentieren Sie die folgende Zeile aus und sehen Sie nach, ob die Fehlerinformation auf ein Problem hinweist.

    # Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Die Fehlermeldung (STDERR) und das Ergebnis des Jobs (STDERR) werden ebenfalls im standardmäßigen BLOB-Container an folgenden Orten für Ihren Cluster protokolliert.

| Für diesen Job... | Sehen Sie sich diese Dateien im BLOB-Container an |
| --- | --- |
| Hive |/HivePython/stderr<p>/HivePython/stdout |
| Pig |/PigPython/stderr<p>/PigPython/stdout |

## <a name="a-namenextanext-steps"></a><a name="next"></a>Nächste Schritte
Wenn Sie Python-Module laden müssen, die standardmäßig nicht bereitgestellt werden, sehen Sie sich [Bereitstellen eines Moduls für Azure HDInsight](http://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx) , wo Sie ein Beispiel finden, wie das geht.

Informationen zu anderen Möglichkeiten der Verwendung von Pig und Hive sowie Informationen zur Verwendung von MapReduce finden Sie in diesen Themen:

* [Verwenden von Hive mit HDInsight](hdinsight-use-hive.md)
* [Verwenden von Pig mit HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce mit HDInsight](hdinsight-use-mapreduce.md)




<!--HONumber=Nov16_HO3-->


