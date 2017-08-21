---
title: "Python-UDF mit Apache Hive und Pig – Azure HDInsight | Microsoft-Dokumentation"
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
ms.date: 07/17/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.translationtype: HT
ms.sourcegitcommit: 94d1d4c243bede354ae3deba7fbf5da0652567cb
ms.openlocfilehash: ad96b5dcb3bce98abc7ab776880dfec4f4a91620
ms.contentlocale: de-de
ms.lasthandoff: 07/18/2017

---
# <a name="use-python-user-defined-functions-udf-with-hive-and-pig-in-hdinsight"></a>Verwenden benutzerdefinierter Python-Funktionen mit Hive und Pig in HDInsight

Erfahren Sie, wie Sie benutzerdefinierte Python-Funktionen (User-Defined Functions, UDFs) mit Apache Hive und Pig in Hadoop in Azure HDInsight verwenden.

## <a name="python"></a>Python in HDInsight

Python 2.7 wird in der Version HDInsight 3.0 und höher standardmäßig installiert. Apache Hive kann mit dieser Version von Python zur Streamverarbeitung verwendet werden. Die Streamverarbeitung nutzt STDOUT und STDIN, um Daten zwischen Hive und der benutzerdefinierten Funktion zu übergeben.

HDInsight enthält außerdem Jython, eine in Java geschriebene Python-Implementierung. Jython wird direkt auf der Java Virtual Machine ausgeführt und verwendet kein Streaming. Jython wird bei Verwendung von Python mit Pig als Python-Interpreter empfohlen.

> [!WARNING]
> Für die Schritte in diesem Dokument gelten die folgenden Annahmen: 
>
> * Sie erstellen die Python-Skripts in der lokalen Entwicklungsumgebung.
> * Sie laden die Skripts entweder mit dem `scp`-Befehl aus einer lokalen Bash-Sitzung oder dem bereitgestellten PowerShell-Skript in HDInsight hoch.
>
> Wenn Sie die [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview)-Vorschau (Bash) zur Arbeit mit HDInsight verwenden möchten, müssen Sie:
>
> * die Skripts in der Cloud Shell-Umgebung erstellen.
> * `scp` zum Hochladen der Dateien aus Cloud Shell in HDInsight verwenden.
> * `ssh` aus Cloud Shell zum Herstellen einer Verbindung mit HDInsight verwenden und die Beispiele ausführen.

## <a name="hivepython"></a>Hive-UDF

Python kann mittels der Hive QL-`TRANSFORM`-Anweisung als UDF von Hive aus verwendet werden. Beispielsweise ruft die folgende HiveQL-Anweisung die im standardmäßigen Azure Storage-Konto für den Cluster gespeicherte `hiveudf.py`-Datei auf.

**HDInsight (Linux-basiert)**

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'python hiveudf.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

**HDInsight (Windows-basiert)**

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'D:\Python27\python.exe hiveudf.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

> [!NOTE]
> In Windows-basierten HDInsight-Clustern muss mit der `USING`-Klausel der vollständige Pfad zu „python.exe“ angegeben werden.

Das Beispiel bewirkt Folgendes:

1. Die `add file`-Anweisung am Anfang der Datei fügt die `hiveudf.py`-Datei dem verteilten Cache hinzu, sodass sie von allen Knoten im Cluster aus zugänglich ist.
2. Die `SELECT TRANSFORM ... USING`-Anweisung wählt Daten aus der `hivesampletable` aus. Sie übergibt außerdem die Werte „clientid“, „devicemake“ und „devicemodel“ an das `hiveudf.py`-Skript .
3. Die `AS`-Klausel beschreibt die von `hiveudf.py` zurückgegebenen Felder.

<a name="streamingpy"></a>

### <a name="create-the-hiveudfpy-file"></a>Erstellen der Datei „hiveudf.py“


Erstellen Sie in Ihrer Entwicklungsumgebung eine Textdatei namens `hiveudf.py`. Fügen Sie der Datei den folgenden Code als Inhalt hinzu:

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
4. Wenn die Verarbeitung abgeschlossen ist, muss die Ausgabe als eine Zeile nach STDOUT geschrieben werden mit einem Tabulator zwischen jedem Feld. Beispiel: `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.
5. Die `while`-Schleife wird wiederholt, bis keine `line` mehr gelesen wird.

Die Skriptausgabe ist eine Verkettung der Eingabewerte für `devicemake` und `devicemodel` und ein Hash der verketteten Werte.

Wie Sie dieses Beispiel auf Ihrem HDInsight-Cluster ausführen, erfahren Sie unter [Ausführen der Beispiele](#running) .

## <a name="pigpython"></a>Pig-UDF

Ein Python-Skript kann mit der `GENERATE`-Anweisung von Pig aus als UDF verwendet werden. Sie können das Skript entweder mit Jython oder C-Python ausführen.

* Jython wird auf der JVM ausgeführt und kann nativ von Pig aufgerufen werden.
* C-Python ist ein externer Prozess, sodass die Daten aus Pig auf der JVM an das Skript gesendet werden, das in einem Python-Prozess ausgeführt wird. Die Ausgabe des Python-Skripts wird wieder an Pig gesendet.

Verwenden Sie zum Angeben des Python-Interpreters `register`, wenn Sie auf das Python-Skript verweisen. In den folgenden Beispielen werden Skripts mit Pig als `myfuncs` registriert:

* **Verwendung von Jython**: `register '/path/to/pigudf.py' using jython as myfuncs;`
* **Verwendung von C Python**: `register '/path/to/pigudf.py' using streaming_python as myfuncs;`

> [!IMPORTANT]
> Bei der Verwendung von Jython kann der Pfad zur Datei „pig_jython“ entweder ein lokaler oder ein „WASB://“-Pfad sein. Jedoch müssen Sie bei der Verwendung von C-Python auf eine Datei auf dem lokalen Dateisystem des Knotens verweisen, den Sie verwenden, um den Pig-Job zu übermitteln.

Nach der Registrierung ist Pig Latin in diesem Beispiel für beides identisch:

```pig
LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
LOG = FILTER LOGS by LINE is not null;
DETAILS = FOREACH LOG GENERATE myfuncs.create_structure(LINE);
DUMP DETAILS;
```

Das Beispiel bewirkt Folgendes:

1. Die erste Zeile lädt die Datei mit den Beispieldaten `sample.log` in `LOGS`. Sie definiert außerdem jeden Datensatz als ein `chararray`.
2. Die nächste Zeile filtert etwaige NULL-Werte heraus und speichert das Ergebnis des Vorgangs in `LOG`.
3. Dann folgt eine Iteration über die Datensätze in `LOG`, und mit `GENERATE` wird die Methode `create_structure` aufgerufen, die im Python/Jython-Skript enthalten ist, das als `myfuncs` geladen wird. `LINE` wird für die Übergabe des aktuellen Datensatzes an die Funktion verwendet.
4. Schließlich werden die ausgegebenen Daten mit dem Befehl `DUMP` in STDOUT geschrieben. Dieser Befehl zeigt die Ergebnisse nach Abschluss des Vorgangs.

### <a name="create-the-pigudfpy-file"></a>Erstellen der Datei „pigudf.py“

Erstellen Sie in Ihrer Entwicklungsumgebung eine Textdatei namens `pigudf.py`. Fügen Sie der Datei den folgenden Code als Inhalt hinzu:

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

Im Pig Latin-Beispiel definierten wir die `LINE`-Eingabe einfach als Chararray, denn es gibt kein gleichbleibendes Schema für die Eingabe. Das Python-Skript transformiert die Daten für die Ausgabe in ein gleichbleibendes Schema.

1. Die `@outputSchema`-Anweisung definiert das Format der Daten, die an Pig zurückgegeben werden. In diesem Fall ist das ein **Datenbehälter**, also ein Pig-Datentyp. Der Behälter enthält folgende Felder, die alle Chararray (Zeichenfolgen) sind:

   * date – das Datum, an dem der Protokolleintrag erstellt wurde
   * time – die Zeit, zu der der Protokolleintrag erstellt wurde
   * classname – der Klassenname, für den der Eintrag erstellt wurde
   * level – die Protokollierungsebene
   * detail – ausführliche Details des Protokolleintrags

2. Dann definiert `def create_structure(input)` die Funktion, an die Pig Positionen übergibt.

3. Die Beispieldatei `sample.log` entspricht weitgehend dem Schema für Datum, Uhrzeit, Klassenname, Ebene und Detail, das wir zurückgeben möchten. Sie enthält jedoch ein paar Zeilen, die mit `*java.lang.Exception*` beginnen. Diese Zeilen müssen geändert werden, um dem Schema zu entsprechen. Die Anweisung `if` überprüft deren Vorhandensein, weist dann die Eingabedaten an, die Zeichenfolge `*java.lang.Exception*` ans Ende zu stellen, sodass die Daten dem erwarteten Ausgabeschema entsprechen.

4. Als Nächstes wird der Befehl `split` zum Aufteilen der Daten bei den ersten vier Leerzeichen verwendet. Die Ausgabe wird in `date`, `time`, `classname`, `level` und `detail` zugewiesen.

5. Zuletzt werden die Werte an Pig zurückgegeben.

Wenn die Daten an Pig zurückgegeben werden, haben sie ein gleichbleibendes Schema gemäß Definition in der `@outputSchema`-Anweisung.

## <a name="running"></a>Hochladen und Ausführen der Beispiele

> [!IMPORTANT]
> Die **SSH**-Schritte sind nur für einen Linux-basierten HDInsight-Cluster geeignet. Die **PowerShell**-Schritte funktionieren entweder mit einem Linux- oder Windows-basierten HDInsight-Cluster, erfordern aber einen Windows-Client.

### <a name="ssh"></a>SSH

Weitere Informationen zur Verwendung von SSH finden Sie unter [Verwenden von SSH mit HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

1. Verwenden Sie `scp` , um die Dateien in Ihren HDInsight-Cluster zu kopieren. Beispielsweise wird die Datei mit folgendem Befehl in einen Cluster namens **mycluster**kopiert.

    ```bash
    scp hiveudf.py pigudf.py myuser@mycluster-ssh.azurehdinsight.net:
    ```

2. Verwenden Sie SSH zum Herstellen einer Verbindung mit dem Cluster.

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

3. Fügen Sie die Python-Dateien, die zuvor hochgeladen wurden, in der SSH-Sitzung dem WASB-Speicher für den Cluster hinzu.

    ```bash
    hdfs dfs -put hiveudf.py /hiveudf.py
    hdfs dfs -put pigudf.py /pigudf.py
    ```

Nach dem Hochladen der Dateien, gehen Sie folgendermaßen vor, um die Hive- und Pig-Aufträge auszuführen.

#### <a name="use-the-hive-udf"></a>Verwenden der Hive-UDF

1. Verwenden Sie den `hive` -Befehl, um die Hive-Shell starten. Nachdem die Shell geladen wurde, sollte eine `hive>` -Eingabeaufforderung angezeigt werden.

2. Geben Sie bei der Eingabeaufforderung `hive>` folgende Abfrage ein:

   ```hive
   add file wasb:///hiveudf.py;
   SELECT TRANSFORM (clientid, devicemake, devicemodel)
       USING 'python hiveudf.py' AS
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

#### <a name="use-the-pig-udf"></a>Verwenden der Pig-UDF

1. Verwenden Sie den `pig` -Befehl, um die Shell starten. Nachdem die Shell geladen wurde, wird eine `grunt>`-Eingabeaufforderung angezeigt.

2. Geben Sie in der `grunt>`-Befehlszeile die folgenden Anweisungen ein:

   ```pig
   Register wasb:///pigudf.py using jython as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

3. Nach Eingabe der folgenden Zeile sollte der Auftrag gestartet werden. Nach Abschluss des Auftrags wird eine Ausgabe zurückgegeben, die folgenden Daten ähnelt:

        ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
        ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
        ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
        ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
        ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

4. Verwenden Sie `quit` zum Beenden der Grunt-Shell und dann Folgendes zum Bearbeiten der Datei „pigudf.py“ auf dem lokalen Dateisystem:

    ```bash
    nano pigudf.py
    ```

5. Heben Sie die Kommentierung der folgenden Zeilen auf, indem Sie das Zeichen `#` vom Zeilenanfang entfernen:

    ```bash
    #from pig_util import outputSchema
    ```

    Nachdem die Änderung vorgenommen wurde, beenden Sie den Editor mit STRG+X. Wählen Sie Y und dann EINGABE, um die Änderungen zu speichern.

6. Verwenden Sie den `pig` -Befehl, um die Shell neu zu starten. Geben Sie Folgendes an der `grunt>` -Eingabeaufforderung ein, um das Python-Skript mit dem C-Python-Interpreter auszuführen.

   ```pig
   Register 'pigudf.py' using streaming_python as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

    Wenn dieser Auftrag abgeschlossen ist, sollte die Ausgabe derjenigen bei der vorherigen Skriptausführung mit Jython entsprechen.

### <a name="powershell-upload-the-files"></a>PowerShell: Hochladen der Dateien

Sie können PowerShell verwenden, um die Dateien auf den HDInsight-Server hochzuladen. Verwenden Sie das folgende Skript, um die Python-Dateien hochzuladen:

> [!IMPORTANT] 
> Für die Schritte in diesem Abschnitt wird Azure PowerShell verwendetet. Weitere Informationen zum Verwenden von Azure PowerShell finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview).

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
# Change the path to match the file location on your system
$pathToStreamingFile = "C:\path\to\hiveudf.py"
$pathToJythonFile = "C:\path\to\pigudf.py"

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
    -Blob "hiveudf.py" `
    -Container $container `
    -Context $context

Set-AzureStorageBlobContent `
    -File $pathToJythonFile `
    -Blob "pigudf.py" `
    -Container $container `
    -Context $context
```
> [!IMPORTANT]
> Ändern Sie den Wert `C:\path\to` in den Pfad zu den Dateien in Ihrer Entwicklungsumgebung.

Dieses Skript ruft Informationen für Ihren HDInsight-Cluster ab, extrahiert dann das Konto und den Schlüssel für das standardmäßigen Speicherkonto und lädt die Dateien in den Stamm des Containers.

> [!NOTE]
> Weitere Informationen zum Hochladen von Dateien finden Sie im Dokument [Hochladen von Daten für Hadoop-Aufträge in HDInsight](hdinsight-upload-data.md).

#### <a name="powershell-use-the-hive-udf"></a>PowerShell: Verwenden der Hive-UDF

PowerShell kann auch zur Remoteausführung von Hive-Abfragen verwendet werden. Verwenden Sie das folgende PowerShell-Skript zum Ausführen einer Hive-Abfrage, die das **hiveudf.py**-Skript verwendet:

> [!IMPORTANT]
> Vor dem Ausführen fordert das Skript Sie zur Eingabe der HTTPS-/Administratorkontoinformationen für Ihren HDInsight-Cluster auf.

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
# "USING 'D:\Python27\python.exe hiveudf.py' AS " +
$HiveQuery = "add file wasb:///hiveudf.py;" +
                "SELECT TRANSFORM (clientid, devicemake, devicemodel) " +
                "USING 'python hiveudf.py' AS " +
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

PowerShell kann auch zum Ausführen von Pig Latin-Aufträgen verwendet werden. Zur Ausführung eines Pig-Latin-Auftrags verwendet das **pigudf.py**-Skript das folgende PowerShell-Skript:

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

$PigQuery = "Register wasb:///pigudf.py using jython as myfuncs;" +
            "LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);" +
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

Die Ausgabe für den **Pig**-Job sollte ungefähr folgenden Daten entsprechen:

    ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
    ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
    ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
    ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
    ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

## <a name="troubleshooting"></a>Problembehandlung

### <a name="errors-when-running-jobs"></a>Fehler beim Ausführen von Aufträgen

Bei der Ausführung des Hive-Auftrags kann ein ähnlicher Fehler wie der folgende Text auftreten:

    Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: [Error 20001]: An error occurred while reading or writing to your custom script. It may have crashed with an error.

Dieses Problem kann durch die Zeilenenden in der Python-Datei verursacht werden. Viele Windows-Editoren verwenden als Zeilenende standardmäßig CRLF, Linux-Anwendung erwarten jedoch i. d. R. LF.

Sie können die folgenden PowerShell-Anweisungen verwenden, um die CR-Zeichen zu entfernen, bevor Sie die Datei in HDInsight hochladen:

```powershell
$original_file ='c:\path\to\hiveudf.py'
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

Die Fehlerinformationen (STDERR) und das Ergebnis des Auftrags (STDOUT) werden auch in Ihrem HDInsight-Speicher protokolliert.

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

