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
ms.date: 02/27/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
translationtype: Human Translation
ms.sourcegitcommit: cfaade8249a643b77f3d7fdf466eb5ba38143f18
ms.openlocfilehash: b39c913367928e8e98dfb1d6bfdca75fcded13c3
ms.lasthandoff: 03/01/2017

---
# <a name="use-python-user-defined-functions-udf-with-hive-and-pig-in-hdinsight"></a>Verwenden benutzerdefinierter Python-Funktionen mit Hive und Pig in HDInsight

Hive und Pig sind großartig für die Arbeit mit Daten in HDInsight. Manchmal benötigt man aber eine allgemeinere Sprache. Sowohl Hive als auch Pig ermöglichen Ihnen die Erstellung benutzerdefinierter Funktionen (User Defined Functions, UDF) mithilfe vieler Programmiersprachen. In diesem Artikel erfahren Sie, wie Sie eine Python-UDF von Hive und Pig aus verwenden.

## <a name="requirements"></a>Anforderungen

* Ein HDInsight-Cluster

  > [!IMPORTANT]
  > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

* Ein Texteditor

## <a name="python"></a>Python in HDInsight

Python 2.7 wird auf Clustern der Version HDInsight 3.0 und höher standardmäßig installiert. Hive kann mit dieser Version von Python für die Streamverarbeitung verwendet werden (Daten werden zwischen Hive und Python mithilfe von STDOUT/STDIN weitergegeben).

HDInsight enthält außerdem Jython, eine in Java geschriebene Python-Implementierung. Pig kann ohne Streaming mit Jython kommunizieren, weshalb es bei der Arbeit mit Pig vorzuziehen ist. Sie können auch normales Python (C-Python) mit Pig verwenden.

## <a name="hivepython"></a>Hive und Python

Python kann als UDF von Hive aus mittels der Hive QL **TRANSFORM** -Anweisung verwendet werden. Das folgende HiveQL ruft beispielsweise ein Python-Skript auf, das in der Datei **streaming.py** gespeichert ist.

**HDInsight (Linux-basiert)**

```hiveql
add file wasbs:///streaming.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'python streaming.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

**HDInsight (Windows-basiert)**

```hiveql
add file wasbs:///streaming.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'D:\Python27\python.exe streaming.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

> [!NOTE]
> In Windows-basierten HDInsight-Clustern muss mit der **USING** -Klausel der vollständige Pfad zu "python.exe" angegeben werden.

Das Beispiel bewirkt Folgendes:

1. Die Anweisung **add file** am Anfang der Datei fügt die Datei **streaming.py** zum verteilten Cache hinzu, sodass sie von allen Knoten im Cluster aus zugänglich ist.
2. Die Anweisung **SELECT TRANSFORM ... USING** wählt Daten aus der **hivesampletable** aus. Sie übergibt außerdem die Werte „clientid“, „devicemake“ und „devicemodel“ an das Skript **streaming.py**.
3. Die **AS**-Klausel beschreibt die Felder, die von **streaming.py** zurückgegeben werden.

<a name="streamingpy"></a> Dies ist die Datei **streaming.py** , die im HiveQL-Beispiel verwendet wird.

```python
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
```

Dieses Skript führt folgende Aktionen aus:

1. Lesen einer Datenzeile aus STDIN.
2. Das nachfolgende Zeilenumbruchzeichen wird mit `string.strip(line, "\n ")` entfernt.
3. Bei der Streamverarbeitung enthält eine einzelne Zeile alle Werte, jeweils getrennt durch ein Tabulatorzeichen. Deshalb kann `string.split(line, "\t")` zum Unterteilen der Eingabe bei jedem Tabstopp verwendet werden, sodass nur die Felder zurückgeben werden.
4. Wenn die Verarbeitung abgeschlossen ist, muss die Ausgabe als eine Zeile nach STDOUT geschrieben werden mit einem Tabulator zwischen jedem Feld. Dies wird mithilfe von `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])` erreicht.
5. Die `while`-Schleife wird wiederholt, bis keine `line` mehr gelesen wird.

Die Skriptausgabe ist eine Verkettung der Eingabewerte für `devicemake` und `devicemodel` und ein Hash der verketteten Werte.

Wie Sie dieses Beispiel auf Ihrem HDInsight-Cluster ausführen, erfahren Sie unter [Ausführen der Beispiele](#running) .

## <a name="pigpython"></a>Pig und Python

Ein Python-Skript kann als UDF von Pig aus mittels der **GENERATE** -Anweisung verwendet werden. Sie können das Skript entweder mit Jython oder C-Python ausführen.

Der Unterschied liegt darin, dass Jython auf der JVM ausgeführt wird und nativ von Pig aufgerufen werden kann. C-Python ist ein externer Prozess, sodass die Daten aus Pig auf der JVM an das Skript gesendet werden, das in einem Python-Prozess ausgeführt wird. Die Ausgabe des Python-Skripts wird wieder an Pig gesendet.

Um zu bestimmen, ob Pig Jython oder C-Python verwendet, um das Skript auszuführen, verwenden Sie beim Verweis auf das Python-Skript aus Pig Latin **register**. In den folgenden Beispielen werden Skripts mit Pig als **myfuncs** registriert:

* **Verwendung von Jython**: `register '/path/to/pig_python.py' using jython as myfuncs;`
* **Verwendung von C Python**: `register '/path/to/pig_python.py' using streaming_python as myfuncs;`

> [!IMPORTANT]
> Bei der Verwendung von Jython kann der Pfad zur Datei „pig_jython“ entweder ein lokaler oder ein „WASB://“-Pfad sein. Jedoch müssen Sie bei der Verwendung von C-Python auf eine Datei auf dem lokalen Dateisystem des Knotens verweisen, den Sie verwenden, um den Pig-Job zu übermitteln.

Nach der Registrierung ist Pig Latin in diesem Beispiel für beides identisch:

```pig
LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
LOG = FILTER LOGS by LINE is not null;
DETAILS = FOREACH LOG GENERATE myfuncs.create_structure(LINE);
DUMP DETAILS;
```

Das Beispiel bewirkt Folgendes:

1. Die erste Zeile lädt die Datei mit den Beispieldaten, **sample.log**, in **LOGS**. Sie definiert außerdem jeden Datensatz als ein **Chararray**.
2. Die nächste Zeile filtert etwaige NULL-Werte heraus und speichert die Ergebnisse des Vorgangs in **LOG**.
3. Dann folgt eine Iteration über die Datensätze in **LOG** und **GENERATE** zum Aufruf der Methode **create_structure**, die im Python/Jython-Skript enthalten ist und als **myfuncs** geladen wird.  **LINE** wird für die Übergabe des aktuellen Datensatzes an die Funktion verwendet.
4. Schließlich werden die ausgegebenen Daten mit dem Befehl **DUMP** in STDOUT geschrieben. Dies zeigt die Ergebnisse nach Abschluss des Vorgangs.

Die Python-Skriptdateien bei C-Python und Jython sind sich recht ähnlich. Der einzige Unterschied besteht darin, dass bei Verwendung von C-Python ein Import aus **pig\_util** durchgeführt werden muss. Hier ist das **pig\_python.py**-Skript:

<a name="streamingpy"></a>

```python
# Uncomment the following if using C Python
#from pig_util import outputSchema

@outputSchema("log: {(date:chararray, time:chararray, classname:chararray, level:chararray, detail:chararray)}")
def create_structure(input):
    if (input.startswith('java.lang.Exception')):
        input = input[21:len(input)] + ' - java.lang.Exception'
    date, time, classname, level, detail = input.split(' ', 4)
    return date, time, classname, level, detail
```

> [!NOTE]
> Über die Installation von „pig_util“ müssen Sie sich keine Gedanken machen; es ist automatisch für das Skript verfügbar.

Sie erinnern sich daran, dass wir die **LINE**-Eingabe einfach als Chararray definiert haben, denn es gab kein gleichbleibendes Schema für die Eingabe. Das Python-Skript transformiert die Daten für die Ausgabe in ein gleichbleibendes Schema.

1. Die Anweisung **@outputSchema** definiert das Format der Daten, die an Pig zurückgegeben werden. In diesem Fall ist das ein **Datenbehälter**, also ein Pig-Datentyp. Der Behälter enthält folgende Felder, die alle Chararray (Zeichenfolgen) sind:
   
   * date – das Datum, an dem der Protokolleintrag erstellt wurde
   * time – die Zeit, zu der der Protokolleintrag erstellt wurde
   * classname – der Klassenname, für den der Eintrag erstellt wurde
   * level – die Protokollierungsebene
   * detail – ausführliche Details des Protokolleintrags

2. Dann definiert **def create_structure(input)** die Funktion, an die Pig Positionen übergibt.

3. Die Beispieldatei **sample.log**entspricht weitgehend dem Schema für Datum, Uhrzeit, Klassenname, Ebene und Detail, das wir zurückgeben möchten. Sie enthält aber auch ein paar Zeilen, die mit der Zeichenfolge '*java.lang.Exception*' beginnen, die geändert werden muss, damit sie dem Schema entspricht. Die Anweisung **if** überprüft deren Vorhandensein, weist dann die Eingabedaten an, die Zeichenfolge '*java.lang.Exception*' ans Ende zu stellen, sodass die Daten dem erwarteten Ausgabeschema entsprechen.

4. Als Nächstes wird der Befehl **split** zum Aufteilen der Daten bei den ersten vier Leerzeichen verwendet. Die Ausgabe wird in **date**, **time**, **classname**, **level** und **detail** zugewiesen.

5. Zuletzt werden die Werte an Pig zurückgegeben.

Wenn die Daten an Pig zurückgegeben werden, haben sie ein gleichbleibendes Schema gemäß Definition in der **@outputSchema**-Anweisung.

## <a name="running"></a>Ausführen der Beispiele
Wenn Sie einen Linux-basierten HDInsight-Cluster verwenden, führen Sie die **SSH**-Schritte aus. Wenn Sie einen Windows-basierten HDInsight-Cluster und einen Windows-Client verwenden, führen Sie die im Abschnitt **PowerShell** beschriebenen Schritte aus.

### <a name="ssh"></a>SSH
Weitere Informationen zur Verwendung von SSH finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix und OS X](hdinsight-hadoop-linux-use-ssh-unix.md) oder [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md).

1. Erstellen Sie mithilfe der Python-Beispiele [streaming.py](#streamingpy) und [pig_python.py](#jythonpy) lokale Kopien der Dateien auf Ihrem Entwicklungscomputer.

2. Verwenden Sie `scp` , um die Dateien in Ihren HDInsight-Cluster zu kopieren. Beispielsweise wird die Datei mit folgendem Befehl in einen Cluster namens **mycluster**kopiert.
   
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
   
   ```hive
   add file wasbs:///streaming.py;
   SELECT TRANSFORM (clientid, devicemake, devicemodel)
       USING 'python streaming.py' AS
       (clientid string, phoneLabel string, phoneHash string)
   FROM hivesampletable
   ORDER BY clientid LIMIT 50;
   ```
3. Nach Eingabe der letzten Zeile sollte der Auftrag gestartet werden. Nach Abschluss des Auftrags wird eine Ausgabe ähnlich der folgenden zurückgegeben:
   
        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="pig"></a>Pig

1. Verwenden Sie den `pig` -Befehl, um die Shell starten. Nachdem die Shell geladen wurde, sollte eine `grunt>` -Eingabeaufforderung angezeigt werden.

2. Geben Sie in der `grunt>`-Befehlszeile die folgenden Anweisungen ein:
   
   ```pig
   Register wasbs:///pig_python.py using jython as myfuncs;
   LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

3. Nach Eingabe der folgenden Zeile sollte der Auftrag gestartet werden. Nach Abschluss des Auftrags wird eine Ausgabe ähnlich der folgenden zurückgegeben.
   
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
   
   ```pig
   Register 'pig_python.py' using streaming_python as myfuncs;
   LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```
   
    Wenn dieser Auftrag abgeschlossen ist, sollte die Ausgabe derjenigen bei der vorherigen Skriptausführung mit Jython entsprechen.

### <a name="powershell"></a>PowerShell

In diesen Schritten wird Microsoft Azure PowerShell verwendet. Weitere Informationen zum Verwenden von Azure PowerShell finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).

1. Erstellen Sie mithilfe der Python-Beispiele [streaming.py](#streamingpy) und [pig_python.py](#jythonpy) lokale Kopien der Dateien auf Ihrem Entwicklungscomputer.
2. Verwenden Sie das folgende PowerShell-Skript zum Hochladen der Dateien **streaming.py** und **pig\_python.py** auf den Server. Ersetzen Sie den Namen Ihres Azure HDInsight-Clusters und den Pfad zu den Dateien **streaming.py** und **pig\_python.py** in den ersten drei Skriptzeilen.
   
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
   ```

    Dieses Skript ruft Informationen für Ihren HDInsight-Cluster ab, extrahiert dann das Konto und den Schlüssel für das standardmäßigen Speicherkonto und lädt die Dateien in den Stamm des Containers.
   
   > [!NOTE]
   > Andere Methoden zum Hochladen der Skripts finden sich im Dokument [Hochladen von Daten für Hadoop-Aufträge in HDInsight](hdinsight-upload-data.md) .

Verwenden Sie nach dem Hochladen der Dateien die folgenden PowerShell-Skripts zum Starten der Jobs. Wenn der Job abgeschlossen ist, sollte die Ausgabe zur PowerShell-Konsole geschrieben werden.

#### <a name="hive"></a>Hive

Das folgende Skript führt das Skript **streaming.py** aus. Vor dem Ausführen werden Sie nach den HTTPS-/Administratorkontoinformationen für Ihren HDInsight-Cluster gefragt.

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
#   -HttpCredential $creds `
#   -DisplayOutputType StandardError
Write-Host "Display the standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
    -Clustername $clusterName `
    -JobId $job.JobId `
    -HttpCredential $creds
```

Die Ausgabe für den **Hive**-Auftrag sollte ungefähr folgendem Beispiel entsprechen:

    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="pig-jython"></a>Pig (Jython)

Im Folgenden wird das Skript **pig_python.py** mit dem Jython-Interpreter verwendet. Vor dem Ausführen werden Sie nach den HTTPS-/Administratorinformationen für den HDInsight-Cluster gefragt.

> [!NOTE]
> Wenn Sie einen Auftrag mithilfe von PowerShell remote übermitteln, können Sie C-Python nicht als Interpreter verwenden.

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

$PigQuery = "Register wasbs:///pig_python.py using jython as myfuncs;" +
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
#    -Clustername $clusterName `
#    -JobId $job.JobId `
#    -HttpCredential $creds `
#    -DisplayOutputType StandardError
Write-Host "Display the standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
    -Clustername $clusterName `
    -JobId $job.JobId `
    -HttpCredential $creds
```

Die Ausgabe für den **Pig** -Job sollte ungefähr wie folgt aussehen:

    ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
    ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
    ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
    ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
    ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

## <a name="troubleshooting"></a>Problembehandlung

### <a name="errors-when-running-jobs"></a>Fehler beim Ausführen von Aufträgen

Bei der Ausführung des Hive-Auftrags können ähnliche Fehler wie die folgenden auftreten:

    Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: [Error 20001]: An error occurred while reading or writing to your custom script. It may have crashed with an error.

Dieses Problem kann durch die Zeilenenden in der Datei „streaming.py“ verursacht werden. Viele Windows-Editoren verwenden als Zeilenende standardmäßig CRLF, Linux-Anwendung erwarten jedoch i. d. R. LF.

Sie können die folgenden PowerShell-Anweisungen verwenden, um die CR-Zeichen zu entfernen, bevor Sie die Datei in HDInsight hochladen:

```powershell
$original_file ='c:\path\to\streaming.py'
$text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
[IO.File]::WriteAllText($original_file, $text)
```

### <a name="powershell-scripts"></a>PowerShell-Skripts

Beide für das Ausführen der Beispiele verwendeten PowerShell-Beispielskripts enthalten eine Kommentarzeile, die Fehler bei der Ausgabe des Jobs anzeigt. Wenn Sie nicht die erwartete Ausgabe für den Job sehen, kommentieren Sie die folgende Zeile aus und sehen Sie nach, ob die Fehlerinformation auf ein Problem hinweist.

```powershell
# Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Die Fehlerinformationen (STDERR,) und das Ergebnis des Auftrags (STDOUT,) werden auch in Ihrem HDInsight-Speicher protokolliert.

| Für diesen Job... | Sehen Sie sich diese Dateien im BLOB-Container an |
| --- | --- |
| Hive |/HivePython/stderr<p>/HivePython/stdout |
| Pig |/PigPython/stderr<p>/PigPython/stdout |

## <a name="next"></a>Nächste Schritte

Wenn Sie Python-Module laden müssen, die standardmäßig nicht bereitgestellt werden, lesen Sie [Bereitstellen eines Moduls für Azure HDInsight](http://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx).

Informationen zu anderen Möglichkeiten der Verwendung von Pig und Hive sowie Informationen zur Verwendung von MapReduce finden Sie in diesen Dokumenten:

* [Verwenden von Hive mit HDInsight](hdinsight-use-hive.md)
* [Verwenden von Pig mit HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce mit HDInsight](hdinsight-use-mapreduce.md)


