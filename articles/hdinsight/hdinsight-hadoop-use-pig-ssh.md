---
title: "Verwenden von Hadoop Pig mit SSH in einem HDInsight-Cluster – Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie eine Verbindung zu einem Linux-basierten Hadoop-Cluster mit SSH aufbauen und dann den Pig-Befehl zum Ausführen von Pig Latin-Anweisungen interaktiv oder als Stapelverarbeitungsauftrag verwenden."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b646a93b-4c51-4ba4-84da-3275d9124ebe
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.translationtype: HT
ms.sourcegitcommit: 54454e98a2c37736407bdac953fdfe74e9e24d37
ms.openlocfilehash: e4c893ef4bfa573dd9fbc9c9b0ae296720769842
ms.contentlocale: de-de
ms.lasthandoff: 07/13/2017

---
# <a name="run-pig-jobs-on-a-linux-based-cluster-with-the-pig-command-ssh"></a>Ausführen von Pig-Jobs auf einem Linux-basierten Cluster mit dem Pig-Befehl (SSH)

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Erfahren Sie, wie Sie Pig-Aufträge interaktiv über eine SSH-Verbindung in Ihrem HDInsight-Cluster ausführen. Mit der Programmiersprache Pig Latin können Sie Transformationen beschreiben, die auf die Eingabedaten angewendet werden, um die gewünschte Ausgabe zu generieren.

> [!IMPORTANT]
> Die Schritte in diesem Dokument erfordern einen Linux-basierten HDInsight-Cluster. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a id="ssh"></a>Verbinden mit SSH

Stellen Sie mithilfe von SSH eine Verbindung mit dem HDInsight-Cluster her. Im folgenden Beispiel stellt das Konto namens **sshuser** eine Verbindung mit einem Cluster namens **myhdinsight** her:

    ssh sshuser@myhdinsight-ssh.azurehdinsight.net

**Wenn Sie beim Erstellen des HDInsight-Clusters einen Zertifikatschlüssel für die SSH-Authentifizierung** angegeben haben, müssen Sie möglicherweise den Speicherort des privaten Schlüssels im Clientsystem angeben.

    ssh sshuser@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

Wenn Sie beim Erstellen des HDInsight-Clusters **ein Kennwort für die SSH-Authentifizierung angegeben haben**, geben Sie das Kennwort nach der entsprechenden Aufforderung an.

Weitere Informationen zum Verwenden von SSH mit HDInsight finden Sie unter [Verwenden von SSH mit HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="pig"></a>Verwenden des Pig-Befehls

1. Nachdem die Verbindung hergestellt ist, starten Sie die Pig-Befehlszeilenschnittstelle (CLI) mit dem folgenden Befehl:

        pig

    Nach einem kurzen Moment sollte eine-Eingabeaufforderung `grunt>` angezeigt werden.

2. Geben Sie die folgende Anweisung ein:

        LOGS = LOAD '/example/data/sample.log';

    Dieser Befehl lädt die Inhalte der Datei "sample.log" in LOGS. Sie können den Inhalt der Datei mit der folgenden Anweisung anzeigen:

        DUMP LOGS;

3. Als Nächstes transformieren Sie die Daten durch Anwendung eines regulären Ausdrucks, um nur den Protokolliergrad aus jedem Datensatz unter Verwendung der folgenden Anweisung zu extrahieren:

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    Sie können **ABBILDSICHERUNG** zum Anzeigen der Daten nach der Transformation verwenden. Verwenden Sie in diesem Fall `DUMP LEVELS;`.

4. Fahren Sie mit der Anwendung der Transformationen mithilfe der Anweisungen in der folgenden Tabelle fort:

    | Pig Latin-Anweisung | Wirkung der Anweisung |
    | ---- | ---- |
    | `FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;` | Entfernt Zeilen mit NULL-Werten für die Protokollebene und speichert die Ergebnisse in `FILTEREDLEVELS`. |
    | `GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;` | Gruppiert die Zeilen nach Protokollebene und speichert die Ergebnisse in `GROUPEDLEVELS`. |
    | `FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;` | Erstellt eine Gruppe von Daten, die jeden eindeutigen Protokollebenenwert und die Häufigkeit seines Auftretens enthält. Das Dataset wird unter `FREQUENCIES` gespeichert. |
    | `RESULT = order FREQUENCIES by COUNT desc;` | Ordnet die Protokollebenen nach Anzahl (absteigend) und speichert sie in `RESULT`. |

    > [!TIP]
    > Verwenden Sie `DUMP`, um das Ergebnis der Transformation nach jedem Schritt anzuzeigen.

5. Sie können die Ergebnisse einer Transformation auch mithilfe der Anweisung `STORE` speichern. Die folgende Anweisung speichert `RESULT` z.B im Verzeichnis `/example/data/pigout` des Standardspeichers für Ihren Cluster:

        STORE RESULT into '/example/data/pigout';

   > [!NOTE]
   > Die Daten werden im angegebenen Verzeichnis in Dateien namens `part-nnnnn` gespeichert. Wenn das Verzeichnis bereits vorhanden ist, erhalten Sie einen Fehler.

6. Geben Sie die folgende Anweisung ein, um die grunt-Eingabeaufforderung zu beenden:

        QUIT;

### <a name="pig-latin-batch-files"></a>Pig Latin-Batchdateien

Den Pig-Befehl können Sie auch zum Ausführen von Pig Latin verwenden, das in einer Datei enthalten ist.

1. Verwenden Sie nach dem Beenden der grunt-Eingabeaufforderung den folgenden Befehl, um STDIN in eine Datei namens `pigbatch.pig` zu leiten. Diese Datei wird im Basisverzeichnis des SSH-Benutzerkontos erstellt.

        cat > ~/pigbatch.pig

2. Geben oder fügen Sie die folgenden Zeilen ein, und drücken Sie STRG+D, wenn Sie fertig sind.

        LOGS = LOAD '/example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

3. Verwenden Sie den folgenden Befehl, um die Datei `pigbatch.pig` mit dem Pig-Befehl auszuführen.

        pig ~/pigbatch.pig

    Sobald der Batchauftrag abgeschlossen ist, wird die folgende Ausgabe angezeigt:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)


## <a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu Pig in HDInsight finden Sie im folgenden Artikel:

* [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md)

Weitere Informationen zu anderen Methoden zur Verwendung von Hadoop in HDInsight finden Sie in den folgenden Artikeln:

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)
* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)

