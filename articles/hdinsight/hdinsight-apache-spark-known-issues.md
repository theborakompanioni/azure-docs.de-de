---
title: Bekannte Probleme mit Apache Spark-Clustern in Azure HDInsight | Microsoft-Dokumentation
description: Bekannte Probleme mit Apache Spark-Clustern in Azure HDInsight
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 610c4103-ffc8-4ec0-ad06-fdaf3c4d7c10
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 2ba5f280b38622b6a0c966d76617cd5698420b92
ms.lasthandoff: 03/25/2017


---
# <a name="known-issues-for-apache-spark-cluster-on-hdinsight"></a>Bekannte Probleme bei Apache Spark-Clustern unter HDInsight

In diesem Dokument werden sämtliche bekannte Probleme für die öffentliche Vorschauversion von HDInsight Spark erfasst.  

## <a name="livy-leaks-interactive-session"></a>Verlust einer interaktiven Sitzung durch Livy
Wenn Livy neu gestartet wird (von Ambari oder aufgrund eines VM-Neustarts mit Hauptknoten 0), während noch eine interaktive Sitzung aktiv ist, geht eine interaktive Auftragssitzung verloren. Dadurch bleiben neue Aufträge unter Umständen im Zustand „Akzeptiert“ hängen und können nicht gestartet werden.

**Lösung:**

Gehen Sie wie folgt vor, um das Problem zu umgehen:

1. Greifen Sie per SSH auf den Stammknoten zu. Informationen hierzu finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Führen Sie den folgenden Befehl aus, um die Anwendungs-IDs der interaktiven Aufträge zu ermitteln, die über Livy gestartet wurden. 
   
        yarn application –list
   
    Wenn die Aufträge mit einer interaktiven Livy-Sitzung ohne explizite Namensangabe gestartet wurden, lauten die Auftragsnamen standardmäßig „Livy“. Bei der von Jupyter Notebook gestarteten Livy-Sitzung beginnt der Auftragsname mit „remotesparkmagics_*“. 
3. Führen Sie den folgenden Befehl aus, um die Beendigung dieser Aufträge zu erzwingen. 
   
        yarn application –kill <Application ID>

Neue Aufträge werden gestartet. 

## <a name="spark-history-server-not-started"></a>Spark-Verlaufsserver startet nicht
Der Spark-Verlaufsserver wird nach der Clustererstellung nicht automatisch gestartet.  

**Lösung:** 

Starten Sie den Verlaufsserver in Ambari manuell.

## <a name="permission-issue-in-spark-log-directory"></a>Berechtigungsproblem im Spark-Protokollverzeichnis
Wenn „hdiuser“ einen Auftrag mit „spark-submit“ übermittelt, tritt der Fehler „java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log“ (Zugriff verweigert) auf, und das Treiberprotokoll wird nicht geschrieben. 

**Lösung:**

1. Fügen Sie „hdiuser“ der Hadoop-Gruppe hinzu. 
2. Erteilen Sie nach der Clustererstellung 777-Berechtigungen für „/var/log/spark“. 
3. Aktualisieren Sie den Spark-Protokollspeicherort mit Ambari auf ein Verzeichnis mit 777-Berechtigungen.  
4. Führen Sie „spark-submit“ als sudo aus.  

## <a name="spark-phoenix-connector-is-not-supported"></a>Keine Unterstützung für den Spark-Phoenix-Connector

Derzeit wird der Spark-Phoenix-Connector für HDInsight Spark-Cluster nicht unterstützt.

**Lösung:**

Sie müssen stattdessen den Spark-HBase-Connector verwenden. Anweisungen finden Sie unter [How to use Spark-HBase connector](https://blogs.msdn.microsoft.com/azuredatalake/2016/07/25/hdinsight-how-to-use-spark-hbase-connector/) (Verwenden des Spark-HBase-Connectors).

## <a name="issues-related-to-jupyter-notebooks"></a>Probleme im Zusammenhang mit Jupyter Notebooks
Im Folgenden sind einige Probleme im Zusammenhang mit Jupyter Notebooks genannt.

### <a name="notebooks-with-non-ascii-characters-in-filenames"></a>Notebooks mit Nicht-ASCII-Zeichen in Dateinamen
Jupyter Notebooks, die in Spark HDInsight-Clustern verwendet werden können, sollten keine Nicht-ASCII-Zeichen in den Dateinamen haben. Wenn Sie versuchen, eine Datei, die einen Nicht-ASCII-Dateinamen besitzt, über die Jupyter-Benutzeroberfläche hochzuladen, tritt ein „stiller“ Fehler auf (d.h. Jupyter lässt Sie die Datei nicht hochladen, löst aber auch keinen sichtbaren Fehler aus). 

### <a name="error-while-loading-notebooks-of-larger-sizes"></a>Fehler beim Laden von größeren Notebooks
Möglicherweise wird beim Laden von größeren Notebooks der Fehler **`Error loading notebook`** angezeigt.  

**Lösung:**

Wenn Sie diesen Fehler erhalten, bedeutet dies nicht, dass Ihre Daten beschädigt oder verloren sind.  Ihre Notebooks befinden sich weiterhin auf der Festplatte in `/var/lib/jupyter`, und Sie können über SSH eine Verbindung mit dem Cluster herstellen, um darauf zuzugreifen. Informationen hierzu finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

Sobald Sie mithilfe von SSH eine Verbindung mit dem Cluster hergestellt haben, können Sie Ihre Notebooks zur Sicherung aus Ihrem Cluster auf den lokalen Computer kopieren (mit SCP oder WinSCP), um den Verlust wichtiger Daten im Notebook zu vermeiden. Anschließend können Sie über einen SSH-Tunnel an Port 8001 eine Verbindung mit Ihrem Hauptknoten herstellen, um ohne Umweg über das Gateway auf Jupyter zuzugreifen.  Dort können die Ausgabe Ihres Notebooks löschen und es erneut speichern, um die Größe des Notebooks zu minimieren.

Um zu verhindern, dass dieser Fehler in Zukunft auftritt, müssen Sie einige bewährten Methoden befolgen:

* Es ist wichtig, die Größe von Notebooks niedrig zu halten. Alle Ausgaben Ihrer Spark-Aufträge, die an Jupyter zurückgesendet werden, werden beständig im Notebook gespeichert.  Für Jupyter wird allgemein empfohlen, das Ausführen von `.collect()` auf großen RDDs (Resilient Distributed Datasets) oder Dataframes zu vermeiden. Wenn Sie einen Blick auf den Inhalt eines RDD werfen möchten, erwägen Sie stattdessen das Ausführen von `.take()` oder `.sample()`, damit die Ausgabe nicht zu groß wird.
* Löschen Sie außerdem beim Speichern eines Notebooks alle Ausgabezellen, um die Größe zu verringern.

### <a name="notebook-initial-startup-takes-longer-than-expected"></a>Erster Notebook-Start dauert länger als erwartet
Die Verarbeitung der ersten Codeanweisung in Jupyter Notebook mit Spark Magic kann über eine Minute dauern.  

**Erklärung:**

Dies geschieht, wenn die erste Codezelle ausgeführt wird. Im Hintergrund werden dadurch die Sitzungskonfiguration initiiert und Spark-, SQL- sowie Hive-Kontexte festgelegt. Nachdem diese Kontexte festgelegt wurden, wird die erste Anweisung ausgeführt, die den Eindruck entstehen lässt, dass sie lange Zeit in Anspruch nimmt.

### <a name="jupyter-notebook-timeout-in-creating-the-session"></a>Jupyter Notebook-Timeout bei der Sitzungserstellung
Wenn dem Spark-Cluster nicht genügend Ressourcen zur Verfügung stehen, tritt bei der Sitzungserstellung für die Spark- und Pyspark-Kernel in Jupyter Notebook ein Timeout auf. 

**Lösung:** 

1. Führen Sie folgende Schritte aus, um Ressourcen in Ihrem Spark-Cluster freizugeben:
   
   * Beenden Sie andere Spark-Notebooks über das entsprechende Menü, oder klicken Sie im Notebook-Explorer auf „Herunterfahren“.
   * Beenden Sie andere Spark-Anwendungen über YARN.
2. Starten Sie das Notebook, das Sie starten wollten, neu. Nun sollten genügend Ressourcen für die Sitzungserstellung verfügbar sein.

## <a name="see-also"></a>Siehe auch
* [Übersicht: Apache Spark in Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Szenarios
* [Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](hdinsight-apache-spark-use-bi-tools.md)
* [Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark-Streaming: Erstellen von Echtzeitstreaminganwendungen mithilfe von Spark in HDInsight](hdinsight-apache-spark-eventhub-streaming.md)
* [Websiteprotokollanalyse mithilfe von Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Erstellen und Ausführen von Anwendungen
* [Erstellen einer eigenständigen Anwendung mit Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Remoteausführung von Aufträgen in einem Spark-Cluster mithilfe von Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools und Erweiterungen
* [Verwenden des HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Erstellen und Übermitteln von Spark Scala-Anwendungen](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely (Verwenden von HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Remotedebuggen von Spark-Anwendungen)](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verwenden von Zeppelin-Notebooks mit einem Spark-Cluster in HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)
* [Verfügbare Kernels für Jupyter-Notebook im Spark-Cluster für HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Verwenden von externen Paketen mit Jupyter Notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen
* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight(Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](hdinsight-apache-spark-job-debugging.md)


