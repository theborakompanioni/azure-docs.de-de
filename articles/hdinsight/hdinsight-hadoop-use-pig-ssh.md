---
title: Verwenden von Hadoop Pig mit SSH in einem HDInsight-Cluster | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie eine Verbindung zu einem Linux-basierten Hadoop-Cluster mit SSH aufbauen und dann den Pig-Befehl zum Ausführen von Pig Latin-Anweisungen interaktiv oder als Stapelverarbeitungsauftrag verwenden."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b646a93b-4c51-4ba4-84da-3275d9124ebe
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 93990e342f6bd8fcfe9781bcb021aabfd33e8572
ms.openlocfilehash: 271a15d258177c98ad72be007c20ae278df6236b


---
# <a name="run-pig-jobs-on-a-linux-based-cluster-with-the-pig-command-ssh"></a>Ausführen von Pig-Jobs auf einem Linux-basierten Cluster mit dem Pig-Befehl (SSH)
[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

In diesem Dokument werden Sie durch den Prozess der Verbindungsherstellung zu einem Linux-basierten Azure HDInsight-Cluster mithilfe von Secure Shell (SSH) geführt. Anschließend werden mit dem Pig-Befehl Pig-Latin-Anweisungen interaktiv oder als Stapelverarbeitungsauftrag ausgeführt.

Mit der Programmiersprache Pig Latin können Sie Transformationen beschreiben, die auf die Eingabedaten angewendet werden, um die gewünschte Ausgabe zu generieren.

> [!NOTE]
> Wenn Sie bereits mit der Verwendung von Linux-basierten Hadoop-Servern vertraut sind, Ihnen HDInsight jedoch neu ist, finden Sie weitere Informationen unter [Tipps zu Linux-basiertem HDInsight](hdinsight-hadoop-linux-information.md).


## <a name="a-idprereqaprerequisites"></a><a id="prereq"></a>Voraussetzungen
Damit Sie die in dieser Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

* Einen Linux-basierten HDInsight-Cluster (Hadoop in HDInsight)

  > [!IMPORTANT]
  > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

* Einen SSH-Client Linux, Unix und Mac OS sollten über einen SSH-Client verfügbar sein. Windows-Benutzer müssen einen Client herunterladen, z. B [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

## <a name="a-idsshaconnect-with-ssh"></a><a id="ssh"></a>Verbinden mit SSH
Stellen Sie mithilfe des SSH-Befehls eine Verbindung zum vollqualifizierten Domänennamen (FQDN) des HDInsight-Clusters her. Der FQDN entspricht dem Namen, den Sie dem Cluster zugewiesen haben, gefolgt von **.azurehdinsight.net**. Mithilfe des folgenden Befehls wird z. B. eine Verbindung mit einem Cluster namens **myhdinsight** hergestellt.

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Wenn Sie beim Erstellen des HDInsight-Clusters einen Zertifikatschlüssel für die SSH-Authentifizierung** angegeben haben, müssen Sie möglicherweise den Speicherort des privaten Schlüssels im Clientsystem angeben.

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Wenn Sie beim Erstellen des HDInsight-Clusters ein Kennwort für die SSH-Authentifizierung** angegeben haben, müssen Sie nach der entsprechenden Aufforderung das Kennwort angeben.

Weitere Informationen zur Verwendung von SSH mit HDInsight finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, OS X und Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

### <a name="putty-windows-based-clients"></a>PUTTY (Windows-basierte Clients)
Windows bietet keinen integrierten SSH-Client. Wir empfehlen die Verwendung von **PuTTY**. Sie können das Programm unter [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) herunterladen.

Weitere Informationen zum Verwenden von PuTTY finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

## <a name="a-idpigause-the-pig-command"></a><a id="pig"></a>Verwenden des Pig-Befehls
1. Nachdem die Verbindung hergestellt ist, starten Sie die Pig-Befehlszeilenschnittstelle (CLI) mit dem folgenden Befehl.
   
        pig
   
    Nach einem kurzen Moment sollte eine-Eingabeaufforderung `grunt>` angezeigt werden.
2. Geben Sie die folgende Anweisung ein.
   
        LOGS = LOAD 'wasbs:///example/data/sample.log';
   
    Dieser Befehl lädt die Inhalte der Datei "sample.log" in LOGS. Sie können den Inhalt der Datei wie folgt anzeigen.
   
        DUMP LOGS;
3. Als Nächstes transformieren Sie die Daten durch Anwendung eines regulären Ausdrucks, um nur den Protokolliergrad aus jedem Datensatz wie folgt zu extrahieren.
   
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
   
    Sie können **ABBILDSICHERUNG** zum Anzeigen der Daten nach der Transformation verwenden. Verwenden Sie in diesem Fall `DUMP LEVELS;`.
4. Fahren Sie mit der Anwendung der Transformationen mithilfe der folgenden Anweisungen fort. Verwenden Sie `DUMP`, um das Ergebnis der Transformation nach jedem Schritt anzuzeigen.
   
    <table>
    <tr>
    <th>Anweisung</th><th>Funktionsbeschreibung</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;</td><td>Entfernt Zeilen mit NULL-Wert für die Protokollebene und speichert die Ergebnisse in FILTEREDLEVELS.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;</td><td>Gruppiert die Zeilen nach Protokollebene und speichert die Ergebnisse in GROUPEDLEVELS.</td>
    </tr>
    <tr>
    <td>FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;</td><td>Erstellt eine neue Gruppe von Daten, die jeden eindeutigen Protokollebenenwert und die Häufigkeit seines Auftretens enthält. Die Ergebnisse werden in HÄUFIGKEITEN gespeichert.</td>
    </tr>
    <tr>
    <td>RESULT = order FREQUENCIES by COUNT desc;</td><td>Ordnet die Protokollebenen nach Anzahl (absteigend) und speichert sie in ERGEBNIS.</td>
    </tr>
    </table>
5. Sie können die Ergebnisse einer Transformation auch mithilfe der Anweisung `STORE` speichern. Das folgende Beispiel speichert `RESULT` im Verzeichnis **/example/data/pigout** des Standardspeichercontainers für Ihren Cluster.
   
        STORE RESULT into 'wasbs:///example/data/pigout';
   
   > [!NOTE]
   > Die Daten werden im angegebenen Verzeichnis in Dateien namens **part-nnnnn**gespeichert. Wenn das Verzeichnis bereits vorhanden ist, erhalten Sie einen Fehler.
   > 
   > 
6. Geben Sie die folgende Anweisung ein, um die grunt-Eingabeaufforderung zu beenden.
   
        QUIT;

### <a name="pig-latin-batch-files"></a>Pig Latin-Batchdateien
Den Pig-Befehl können Sie auch zum Ausführen von Pig Latin verwenden, das in einer Datei enthalten ist.

1. Verwenden Sie nach dem Beenden der grunt-Eingabeaufforderung den folgenden Befehl, um STDIN in eine Datei namens **pigbatch.pig**zu leiten. Diese Datei wird im Stammverzeichnis des Kontos erstellt, bei dem Sie für die SSH-Sitzung angemeldet sind.
   
        cat > ~/pigbatch.pig
2. Geben oder fügen Sie die folgenden Zeilen ein, und drücken Sie STRG+D, wenn Sie fertig sind.
   
        LOGS = LOAD 'wasbs:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;
3. Gehen Sie wie folgt vor, um die Datei **pigbatch.pig** mit dem Pig-Befehl auszuführen.
   
        pig ~/pigbatch.pig
   
    Wenn der Stapelverarbeitungsauftrag abgeschlossen ist, sollte die folgende Ausgabe angezeigt werden. Diese sollte mit der Ausgabe übereinstimmen, die Sie bei Verwendung von `DUMP RESULT;` in den vorherigen Schritten erhalten haben.
   
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <a name="a-idsummaryasummary"></a><a id="summary"></a>Zusammenfassung
Wie Sie sehen, können Sie mit dem Pig-Befehl MapReduce-Vorgänge mithilfe von Pig Latin interaktiv ausführen sowie die in einer Batchdatei gespeicherten Anweisungen ausführen.

## <a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Nächste Schritte
Allgemeine Informationen zu Pig in HDInsight:

* [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md)

Informationen zu anderen Möglichkeiten, wie Sie mit Hadoop in HDInsight arbeiten können:

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)
* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)




<!--HONumber=Jan17_HO3-->


