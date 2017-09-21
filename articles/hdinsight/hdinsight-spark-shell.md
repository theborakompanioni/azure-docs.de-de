---
title: Verwenden einer interaktiven Spark-Shell in Azure HDInsight | Microsoft-Dokumentation
description: "Eine interaktive Spark-Shell bietet einen „Lesen-Ausführen-Anzeigen“-Prozesses, um Spark-Befehle nacheinander auszuführen und die Ergebnisse anzuzeigen."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/01/2017
ms.author: nitinme
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: 108b2e25dee63a15ac703d2a9b617bfb155f95da
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---
# <a name="run-spark-from-the-spark-shell"></a>Ausführen von Spark aus der Spark-Shell

Eine interaktive Spark-Shell bietet eine REPL-Umgebung (Read-Execute-Print-Loop, „Lesen-Ausführen-Anzeigen“-Schleife), um Spark-Befehle nacheinander auszuführen und die Ergebnisse anzuzeigen. Dieser Vorgang ist nützlich für Entwicklung und Debuggen. Spark bietet für jede unterstützte Sprache eine Shell: Scala, Python und R.

## <a name="get-to-a-spark-shell-with-ssh"></a>Zugriff auf eine Spark-Shell mit SSH

Greifen Sie unter HDInsight durch Herstellen einer Verbindung mit dem primären Hauptknoten des Clusters mithilfe von SSH auf eine Shell Spark zu:

     ssh <sshusername>@<clustername>-ssh.azurehdinsight.net

Sie können den vollständigen SSH-Befehl für Ihren Cluster aus dem Azure-Portal abrufen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Navigieren Sie zu dem Bereich für Ihren HDInsight-Spark-Cluster.
3. Wählen Sie „Secure Shell (SSH)“.

    ![HDInsight-Bereich im Azure-Portal](./media/hdinsight-spark-shell/hdinsight-spark-blade.png)

4. Kopieren Sie den angezeigten SSH-Befehl, und führen Sie ihn auf Ihrem Terminal aus.

    ![HDInsight-SSH-Bereich im Azure-Portal](./media/hdinsight-spark-shell/hdinsight-spark-ssh-blade.png)

Details zum Verwenden von SSH zum Herstellen einer Verbindung mit HDInsight finden Sie unter [Herstellen einer Verbindung mit HDInsight (Hadoop) per SSH](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="run-a-spark-shell"></a>Ausführen einer Spark-Shell

Spark bietet Shells für Scala (spark-shell), Python (pyspark) und R (sparkR). Geben Sie in der SSH-Sitzung auf dem Hauptknoten des HDInsight-Clusters einen der folgenden Befehle ein:

    ./bin/spark-shell
    ./bin/pyspark
    ./bin/sparkR

Sie können jetzt Spark-Befehle in der entsprechenden Sprache eingeben.

## <a name="sparksession-and-sparkcontext-instances"></a>SparkSession- und SparkContext-Instanzen

Beim Ausführen der Spark-Shell werden standardmäßig automatisch Instanzen von SparkSession und SparkContext für Sie erzeugt.

Um auf die SparkSession-Instanz zuzugreifen, geben Sie `spark` ein. Um auf die SparkContext-Instanz zuzugreifen, geben Sie `sc` ein.

## <a name="important-shell-parameters"></a>Wichtige Shellparameter

Der Spark-Shell-Befehl (`spark-shell`, `pyspark` oder `sparkR`) unterstützt viele Befehlszeilenparameter. Um eine vollständige Liste der Parameter anzuzeigen, starten Sie die Spark-Shell mit dem Schalter `--help`. Beachten Sie, dass einige dieser Parameter möglicherweise nur für `spark-submit` gelten, den die Spark-Shell umhüllt.

| Schalter | Beschreibung | Beispiel |
| --- | --- | --- |
| --master MASTER-URL | Gibt die Master-URL an. In HDInsight ist dieser Wert immer `yarn`. | `--master yarn`|
| --jars JAR-LISTE | Durch Trennzeichen getrennte Liste der lokalen JAR-Dateien zum Einschließen der Treiber- und Executorklassenpfade. In HDInsight besteht diese Liste aus Pfaden zum Standarddateisystem in Azure Storage oder Data Lake Store. | `--jars /path/to/examples.jar` |
| --packages MAVEN-KOORDINATEN | Durch Trennzeichen getrennte Liste der Maven-Koordinaten von JAR-Dateien zum Einschließen der Treiber- und Executorklassenpfade. Durchsucht zuerst das lokale, anschließend das zentrale Maven-Repository und dann etwaige zusätzliche Remote-Repositorys, die mit `--repositories` angegeben werden. Das Format für die Koordinaten ist *groupId*:*artifactId*:*version*. | `--packages "com.microsoft.azure:azure-eventhubs:0.14.0"`|
| --py-files LISTE | Nur für Python eine durch Trennzeichen getrennte Liste der im PYTHONPATH zu platzierenden ZIP-, EGG- oder PY-Dateien. | `--pyfiles "samples.py"` |

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht finden Sie unter [Einführung in Spark in HDInsight](hdinsight-apache-spark-overview.md).
- Informationen zum Arbeiten mit Spark-Clustern und SparkSQL finden Sie unter [Erstellen eines Apache Spark-Clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
- Informationen zum Schreiben von Anwendungen, die Streamingdaten mit Spark verarbeiten, finden Sie unter [Overview of Spark Streaming](hdinsight-spark-streaming-overview.md) (Übersicht zum Spark-Streaming).


