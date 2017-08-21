---
title: "Problembehandlung bei Spark – Azure HDInsight | Microsoft-Dokumentation"
description: "Verwenden Sie die Spark-FAQ für Antworten auf häufig gestellte Fragen zu Spark auf der Azure HDInsight-Plattform."
keywords: Azure HDInsight, Spark, FAQ, Problembehandlungshandbuch, allgemeine Probleme, Anwendungskonfiguration, Ambari
services: Azure HDInsight
documentationcenter: na
author: arijitt
manager: 
editor: 
ms.assetid: 25D89586-DE5B-4268-B5D5-CC2CE12207ED
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: arijitt
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: 7655acd689c3f5b869eb0ddb5f186cff032ca8b6
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017


---

# <a name="spark-troubleshooting"></a>Problembehandlung bei Spark

Dieser Artikel beschreibt die wichtigsten Probleme und ihre Lösungen für die Arbeit mit Spark-Nutzlasten in Apache Ambari.

## <a name="how-do-i-configure-a-spark-application-through-ambari-on-clusters"></a>Wie konfiguriere ich eine Spark-Anwendung über Ambari auf Clustern?

### <a name="issue"></a>Problem:

Ich muss in Ambari die Menge an Arbeitsspeicher und die Anzahl von Kernen konfigurieren, die eine Spark-Anwendung verwenden kann.  

### <a name="resolution-steps"></a>Lösungsschritte: 

Diese Prozedurkonfigurationswerte werden zuvor in HDInsight Spark-Clustern festgelegt. Bestimmen Sie mithilfe von [Why did my Spark application fail with an OutOfMemoryError?](#why-did-my-spark-application-fail-with-an-outofmemoryerror) (Warum ist bei meiner Spark-Anwendung ein OutOfMemoryError-Fehler aufgetreten?), welche Spark-Konfigurationen mit welchen Werten festgelegt werden müssen.

1. Wählen Sie **Spark2** aus der Liste der Cluster.

    ![Cluster aus der Liste wählen](media/hdinsight-troubleshoot-spark/update-config-1.png)

1. Klicken Sie auf die Registerkarte **Configs** .

    ![Registerkarte „Configs“ wählen](media/hdinsight-troubleshoot-spark/update-config-2.png)

1. Wählen Sie in der Liste der Konfigurationen **Custom-spark2-defaults**.

    ![„custom-spark-defaults“ wählen](media/hdinsight-troubleshoot-spark/update-config-3.png)

1. Suchen Sie nach der Werteinstellung, die Sie anpassen müssen, z.B. **spark.executor.memory**. In diesem Fall ist der Wert „4608m“ zu hoch.

    ![Feld „spark.executor.memory“ wählen](media/hdinsight-troubleshoot-spark/update-config-4.png)

1. Legen Sie den Wert wie empfohlen fest. In diesem Fall wird „2048“ für diese Einstellung empfohlen.

    ![Wert in „2048 m“ ändern](media/hdinsight-troubleshoot-spark/update-config-5.png)

1. Speichern Sie den Einstellungswert und dann die Konfiguration. 

    Klicken Sie in der Symbolleiste auf **Speichern** .

    ![Einstellung und Konfiguration speichern](media/hdinsight-troubleshoot-spark/update-config-6a.png)

    Sie werden benachrichtigt, wenn eine Konfiguration Ihre Aufmerksamkeit erfordert. Notieren Sie sie, und klicken Sie auf **Proceed Anyway**. 

    ![Auf „Proceed Anyway“ klicken](media/hdinsight-troubleshoot-spark/update-config-6b.png)

    Schreiben Sie eine Notiz zu den Konfigurationsänderungen, und klicken Sie auf **Save**.

    ![Notiz zu den von Ihnen vorgenommenen Änderungen eingeben](media/hdinsight-troubleshoot-spark/update-config-6c.png)

1. Wenn eine Konfiguration gespeichert ist, werden Sie aufgefordert, den Dienst neu starten. Klicken Sie auf **Restart**.

    ![Auf „Restart“ klicken](media/hdinsight-troubleshoot-spark/update-config-7a.png)

    Bestätigen Sie den Neustart.

    ![Auf „Confirm Restart All“ klicken](media/hdinsight-troubleshoot-spark/update-config-7b.png)

    Sie können die ausgeführten Prozesse überprüfen.

    ![Ausgeführte Prozesse überprüfen](media/hdinsight-troubleshoot-spark/update-config-7c.png)

1. Sie können auch Konfigurationen hinzufügen. Wählen Sie in der Liste der Konfigurationen **Custom-spark2-defaults** wie in Schritt 3 und dann **Add Property**.

    ![Auf „Add Property“ klicken](media/hdinsight-troubleshoot-spark/update-config-8.png)

1. Definieren Sie eine neue Eigenschaft. Sie können entweder eine einzelne Eigenschaft definieren – in einem Dialogfeld für spezifische Einstellungen wie z.B. den Datentyp – oder Sie können mehrere Eigenschaften mit einer Definition pro Zeile definieren. 

    In diesem Beispiel wird die Eigenschaft **spark.driver.memory** mit einem Wert von „4g“ definiert.

    ![Neue Eigenschaft definieren](media/hdinsight-troubleshoot-spark/update-config-9.png)

1. Speichern Sie die Konfiguration, und starten Sie den Dienst wie in Schritt 6 und 7 beschrieben neu.

Diese Änderungen betreffen den gesamten Cluster, können jedoch zum Zeitpunkt der tatsächlichen Übermittlung eines Spark-Auftrags überschrieben werden.

### <a name="further-reading"></a>Weitere nützliche Informationen:

[Spark job submission on HDInsight clusters](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/) (Übermitteln von Spark-Aufträgen auf HDInsight-Clustern)


## <a name="how-do-i-configure-a-spark-application-through-a-jupyter-notebook-on-clusters"></a>Wie konfiguriere ich eine Spark-Anwendung über ein Jupyter-Notebook auf Clustern?

### <a name="issue"></a>Problem:

Ich muss die Menge an Arbeitsspeicher und die Anzahl von Kernen konfigurieren, die eine Spark-Anwendung bei Nutzung des Jupyter-Notebooks auf HDInsight-Clustern verwenden kann. 

1. Bestimmen Sie mithilfe von [Why did my Spark application fail with an OutOfMemoryError?](#spark-application-failure-outofmemory) (Warum ist bei meiner Spark-Anwendung ein OutOfMemoryError-Fehler aufgetreten?), welche Spark-Konfigurationen mit welchen Werten festgelegt werden müssen.
1.  Geben Sie die Spark-Konfigurationen im gültigen JSON-Format in der ersten Zelle des Jupyter-Notebooks nach der %%configure-Anweisung an (ändern Sie die Istwerte entsprechend): 

>![Konfiguration hinzufügen](media/hdinsight-troubleshoot-spark/add-configuration-cell.png)

### <a name="further-reading"></a>Weitere nützliche Informationen:

[Spark job submission on HDInsight clusters](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/) (Übermitteln von Spark-Aufträgen auf HDInsight-Clustern)


## <a name="how-do-i-configure-a-spark-application-through-livy-on-clusters"></a>Wie konfiguriere ich eine Spark-Anwendung über LIVY auf Clustern?

### <a name="issue"></a>Problem:

Ich muss die Menge an Arbeitsspeicher und die Anzahl von Kernen zum Zeitpunkt der Übermittlung über LIVY konfigurieren, die eine Spark-Anwendung auf HDInsight-Clustern verwenden kann. 

1. Bestimmen Sie mithilfe von [Why did my Spark application fail with an OutOfMemoryError?](#spark-application-failure-outofmemory) (Warum ist bei meiner Spark-Anwendung ein OutOfMemoryError-Fehler aufgetreten?), welche Spark-Konfigurationen mit welchen Werten festgelegt werden müssen.
1. Übermitteln Sie die Spark-Anwendung an LIVY mit einem REST-Client wie CURL mit einem Befehl ähnlich dem folgenden (ändern Sie die Istwerte entsprechend):

```apache
curl -k --user 'username:password' -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://container@storageaccountname.blob.core.windows.net/example/jars/sparkapplication.jar", "className":"com.microsoft.spark.application", "numExecutors":4, "executorMemory":"4g", "executorCores":2, "driverMemory":"8g", "driverCores":4}'  
```

### <a name="further-reading"></a>Weitere nützliche Informationen:

[Spark job submission on HDInsight clusters](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/) (Übermitteln von Spark-Aufträgen auf HDInsight-Clustern)


## <a name="how-do-i-configure-a-spark-application-through-spark-submit-on-clusters"></a>Wie konfiguriere ich eine Spark-Anwendung über spark-submit auf Clustern?

### <a name="issue"></a>Problem: 

Ich muss die Menge an Arbeitsspeicher und die Anzahl von Kernen zum Zeitpunkt der Übermittlung über spark-submit konfigurieren, die eine Spark-Anwendung auf HDInsight-Clustern verwenden kann.

1. Bestimmen Sie mithilfe von [Why did my Spark application fail with an OutOfMemoryError?](#spark-application-failure-outofmemory) (Warum ist bei meiner Spark-Anwendung ein OutOfMemoryError-Fehler aufgetreten?), welche Spark-Konfigurationen mit welchen Werten festgelegt werden müssen.
1. Starten Sie die Spark-Shell mit einem Befehl ähnlich dem folgenden (ändern Sie den Istwert der Konfiguration entsprechend): 

```apache
spark-submit --master yarn-cluster --class com.microsoft.spark.application --num-executors 4 --executor-memory 4g --executor-cores 2 --driver-memory 8g --driver-cores 4 /home/user/spark/sparkapplication.jar
```

### <a name="further-reading"></a>Weitere nützliche Informationen:

[Spark job submission on HDInsight clusters](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/) (Übermitteln von Spark-Aufträgen auf HDInsight-Clustern)



## <a name="what-causes-a-spark-application-outofmemoryerror-exception"></a>Wodurch wird eine OutOfMemoryError-Ausnahme in einer Spark-Anwendung verursacht?

### <a name="error"></a>Fehler

Bei der Spark-Anwendung ist der Ausnahmefehler OutOfMemoryError aufgetreten.

### <a name="detailed-description"></a>Detaillierte Beschreibung:

Bei der Spark-Anwendung sind die folgenden Typen nicht abgefangener Ausnahmefehler aufgetreten.  
```apache
ERROR Executor: Exception in task 7.0 in stage 6.0 (TID 439) 

java.lang.OutOfMemoryError 
    at java.io.ByteArrayOutputStream.hugeCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.grow(Unknown Source) 
    at java.io.ByteArrayOutputStream.ensureCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.write(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.drain(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.setBlockDataMode(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject0(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject(Unknown Source) 
    at org.apache.spark.serializer.JavaSerializationStream.writeObject(JavaSerializer.scala:44) 
    at org.apache.spark.serializer.JavaSerializerInstance.serialize(JavaSerializer.scala:101) 
    at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:239) 
    at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source) 
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source) 
    at java.lang.Thread.run(Unknown Source) 
```

```apache
ERROR SparkUncaughtExceptionHandler: Uncaught exception in thread Thread[Executor task launch worker-0,5,main] 

java.lang.OutOfMemoryError 
    at java.io.ByteArrayOutputStream.hugeCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.grow(Unknown Source) 
    at java.io.ByteArrayOutputStream.ensureCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.write(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.drain(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.setBlockDataMode(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject0(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject(Unknown Source) 
    at org.apache.spark.serializer.JavaSerializationStream.writeObject(JavaSerializer.scala:44) 
    at org.apache.spark.serializer.JavaSerializerInstance.serialize(JavaSerializer.scala:101) 
    at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:239) 
    at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source) 
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source) 
    at java.lang.Thread.run(Unknown Source) 
```

### <a name="probable-cause"></a>Wahrscheinliche Ursache:

Die wahrscheinlichste Ursache dieser Ausnahme ist, dass den Java Virtual Machine (JVM)-Instanzen, die als Executor oder Treiber als Teil der Spark-Anwendung gestartet werden, nicht genügend Heap-Arbeitsspeicher zugeordnet ist. 

### <a name="resolution-steps"></a>Lösungsschritte:

1. Ermitteln Sie die maximale Größe der Daten, die von die Spark-Anwendung verarbeitet werden. Basierend auf der maximalen Größe der Eingabedaten, den bei der Transformation der Eingabedaten erzeugten Zwischendateien und den erzeugten Ausgabedaten, die die Zwischendateien weiter transformieren, kann eine Schätzung vorgenommen werden. Dies kann auch ein iterativer Prozess sein, wenn eine formale anfängliche Schätzung nicht möglich ist. 
1. Stellen Sie sicher, dass der zu verwendende HDInsight-Cluster über genügend Ressourcen im Hinblick auf Arbeitsspeicher und Kerne verfügt, um die Spark-Anwendung auszuführen. Dazu zeigen Sie den Abschnitt mit Clustermetriken der YARN-Benutzeroberfläche des Clusters an und vergleichen die Werte für den verwendeten Arbeitsspeicher und den Gesamtarbeitsspeicher sowie für die verwendeten VCores und VCores insgesamt.

1. Legen Sie die folgenden Spark-Konfigurationen auf geeignete Werte von bis zu 90% des verfügbaren Arbeitsspeichers und eine Anzahl von Kernen fest, die für YARN noch gut innerhalb der Arbeitsspeicheranforderung der Spark-Anwendung liegen: 

```apache
spark.executor.instances (Example: 8 for 8 executor count) 
spark.executor.memory (Example: 4g for 4 GB) 
spark.yarn.executor.memoryOverhead (Example: 384m for 384 MB) 
spark.executor.cores (Example: 2 for 2 cores per executor) 
spark.driver.memory (Example: 8g for 8GB) 
spark.driver.cores (Example: 4 for 4 cores)   
spark.yarn.driver.memoryOverhead (Example: 384m for 384MB) 
```

Gesamter von allen Executors verwendeter Arbeitsspeicher = 
```apache
spark.executor.instances * (spark.executor.memory + spark.yarn.executor.memoryOverhead) 
```
Gesamter vom Treiber verwendeter Arbeitsspeicher = 
```apache
spark.driver.memory + spark.yarn.driver.memoryOverhead
```

### <a name="further-reading"></a>Weitere nützliche Informationen:

- [Spark memory management overview](http://spark.apache.org/docs/latest/tuning.html#memory-management-overview) (Übersicht über die Spark-Speicherverwaltung)
- [Debugging Spark application on HDInsight clusters](https://blogs.msdn.microsoft.com/azuredatalake/2016/12/19/spark-debugging-101/) (Debuggen der Spark-Anwendung auf HDInsight-Clustern)










