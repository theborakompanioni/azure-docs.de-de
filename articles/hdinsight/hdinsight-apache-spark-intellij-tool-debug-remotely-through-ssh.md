---
title: "Azure-Toolkit für IntelliJ: Remotedebuggen von Spark-Anwendungen per SSH | Microsoft-Dokumentation"
description: "Hier finden Sie eine ausführliche Anleitung zur Verwendung der HDInsight Tools im Azure-Toolkit für IntelliJ zum Remotedebuggen von Anwendungen in HDInsight-Clustern per SSH."
keywords: Remotedebuggen von IntelliJ, Remotedebugging IntelliJ, SSH IntelliJ, HDInsight, Debuggen von Intellij, Debuggen
services: hdinsight
documentationcenter: 
author: jejiang
manager: DJ
editor: Jenny Jiang
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 08/24/2017
ms.author: Jenny Jiang
ms.translationtype: HT
ms.sourcegitcommit: 7456da29aa07372156f2b9c08ab83626dab7cc45
ms.openlocfilehash: 19053e31d6eb097bc91a04ef9c6af5772aaa16da
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---
# <a name="debug-spark-applications-on-an-hdinsight-cluster-with-azure-toolkit-for-intellij-through-ssh"></a>Debuggen von Spark-Anwendungen in einem HDInsight-Cluster mit dem Azure-Toolkit für IntelliJ per SSH

Dieser Artikel enthält eine ausführliche Anleitung zur Verwendung der HDInsight Tools im Azure-Toolkit für IntelliJ zum Remotedebuggen von Anwendungen in einem HDInsight-Cluster. Informationen zum Debuggen Ihres Projekts finden Sie auch im Video [Debug HDInsight Spark applications with Azure Toolkit for IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) (Debuggen von HDInsight Spark-Anwendungen mit dem Azure-Toolkit für IntelliJ).

**Voraussetzungen**

* **HDInsight-Tools im Azure-Toolkit für IntelliJ.** Dieses Tool ist Teil des Azure-Toolkits für IntelliJ. Weitere Informationen finden Sie unter [Installieren des Azure-Toolkits für IntelliJ](https://docs.microsoft.com/en-us/azure/azure-toolkit-for-intellij-installation).
* **Azure-Toolkit für IntelliJ.** Verwenden Sie dieses Toolkit zum Erstellen von Spark-Anwendungen für einen HDInsight-Cluster. Weitere Informationen finden Sie in den Anweisungen unter [Erstellen von Spark-Anwendungen für einen HDInsight-Cluster mit dem Azure-Toolkit für IntelliJ](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin).
* **HDInsight SSH-Dienst mit Benutzernamen- und Kennwortverwaltung**. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit HDInsight (Hadoop) per SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) und [Verwenden von SSH-Tunneling zum Zugriff auf die Ambari-Webbenutzeroberfläche, JobHistory, NameNode, Oozie und andere Webbenutzeroberflächen](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-linux-ambari-ssh-tunnel). 
 

## <a name="create-a-spark-scala-application-and-configure-it-for-remote-debugging"></a>Erstellen einer Spark Scala-Anwendung und Konfigurieren dieser für das Remotedebuggen

1. Starten Sie IntelliJ IDEA, und erstellen Sie ein Projekt. Führen Sie im Dialogfeld **Neues Projekt** folgende Schritte aus:

   a. Wählen Sie **HDInsight** aus. 

   b. Wählen Sie entsprechend Ihren Anforderungen eine Java- oder Scala-Vorlage aus. Folgende Optionen stehen zur Auswahl:

      - **Spark auf HDInsight (Scala)**

      - **Spark auf HDInsight (Java)**

      - **Beispiel für die Ausführung von Spark auf HDInsight-Clustern (Scala)**

      In diesem Beispiel wird eine Vorlage vom Typ **Beispiel für die Ausführung von Spark auf HDInsight-Clustern (Scala)** verwendet.

   c. Wählen Sie in der Liste **Build-Tool** je nach Ihren Anforderungen einen der folgenden Einträge aus:

      - **Maven** für die Unterstützung des Scala-Projekterstellungs-Assistenten

      -  **SBT** zum Verwalten von Abhängigkeiten und Erstellen für das Scala-Projekt 

      ![Erstellen eines Debugprojekts](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-create-projectfor-debug-remotely.png)

   d. Klicken Sie auf **Weiter**.     
 
3. Führen Sie im nächsten Fenster **New Project** (Neues Projekt) folgende Schritte aus:

   ![Auswählen des Spark-SDK](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-new-project.png)

   a. Geben Sie einen Projektnamen und -speicherort an.

   b. Wählen Sie in der Dropdownliste **Project SDK** (Projekt-SDK) die Option **Java 1.8** für **Spark 2.x**-Cluster oder die Option **Java 1.7** für **Spark 1.x**-Cluster aus.

   c. In der Dropdownliste **Spark Version** (Spark-Version) fügt der Scala-Projekterstellungs-Assistent die richtige Version für das Spark-SDK und das Scala-SDK ein. Wenn die Spark-Clusterversion niedriger als 2.0 ist, wählen Sie **Spark 1.x** aus. Wählen Sie andernfalls **Spark 2.x** aus. In diesem Beispiel wird **Spark 2.0.2 (Scala 2.11.8)** verwendet.

   d. Wählen Sie **Fertig stellen** aus.

4. Wählen Sie **src** > **main** > **scala** aus, um Ihren Code im Projekt zu öffnen. In diesem Beispiel wird das Skript **SparkCore_wasbloTest** verwendet.

5. Um das Menü **Edit Configurations** (Konfigurationen bearbeiten) zu öffnen, klicken Sie auf das Symbol in der rechten oberen Ecke. Über dieses Menü können Sie die Konfigurationen für das Remotedebuggen erstellen und bearbeiten.

   ![Konfigurationen bearbeiten](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-edit-configurations.png) 

6. Klicken Sie im Dialogfeld **Run/Debug Configurations** (Konfigurationen ausführen/debuggen) auf das Plussymbol (**+**). Wählen Sie dann die Option **Submit Spark Job** (Spark-Auftrag übermitteln) aus.

   ![Hinzufügen einer neuen Konfiguration](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-add-new-Configuration.png)
7. Geben Sie Informationen für **Name**, **Spark cluster** und **Main class name** (Name der main-Klasse) ein. Wählen Sie anschließend **Advanced configuration** (Erweiterte Konfiguration) aus. 

   ![Debugkonfigurationen ausführen](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-run-debug-configurations.png)

8. Wählen Sie im Dialogfeld **Spark Submission Advanced Configuration** (Erweiterte Konfiguration für Spark Übermittlung) die Option **Enable Spark remote debug** (Remotedebuggen für Spark aktivieren) aus. Geben Sie den SSH-Benutzernamen und anschließend ein Kennwort ein, oder verwenden Sie eine private Schlüsseldatei. Klicken Sie auf **OK**, um die Konfiguration zu speichern.

   ![Remotedebuggen für Spark aktivieren](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-enable-spark-remote-debug.png)

9. Die Konfiguration wird jetzt unter dem von Ihnen angegebenen Namen gespeichert. Um die Konfigurationsdetails anzuzeigen, wählen Sie den Konfigurationsnamen aus. Um Änderungen vorzunehmen, wählen Sie **Edit Configurations** (Konfigurationen bearbeiten) aus. 

10. Nachdem Sie die Konfigurationseinstellungen abgeschlossen haben, können Sie das Projekt für den Remotecluster ausführen oder das Remotedebuggen durchführen.

## <a name="learn-how-to-perform-remote-debugging"></a>Informationen Sie zum Remotedebuggen
### <a name="scenario-1-perform-remote-run"></a>Szenario 1: Remoteausführung

In diesem Abschnitt wird erläutert, wie Sie Treiber und Executors debuggen.

    import org.apache.spark.{SparkConf, SparkContext}

    object LogQuery {
      val exampleApacheLogs = List(
        """10.10.10.10 - "FRED" [18/Jan/2013:17:56:07 +1100] "GET http://images.com/2013/Generic.jpg
          | HTTP/1.1" 304 315 "http://referall.com/" "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1;
          | GTB7.4; .NET CLR 2.0.50727; .NET CLR 3.0.04506.30; .NET CLR 3.0.04506.648; .NET CLR
          | 3.5.21022; .NET CLR 3.0.4506.2152; .NET CLR 1.0.3705; .NET CLR 1.1.4322; .NET CLR
          | 3.5.30729; Release=ARP)" "UD-1" - "image/jpeg" "whatever" 0.350 "-" - "" 265 923 934 ""
          | 62.24.11.25 images.com 1358492167 - Whatup""".stripMargin.lines.mkString,
        """10.10.10.10 - "FRED" [18/Jan/2013:18:02:37 +1100] "GET http://images.com/2013/Generic.jpg
          | HTTP/1.1" 304 306 "http:/referall.com" "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1;
          | GTB7.4; .NET CLR 2.0.50727; .NET CLR 3.0.04506.30; .NET CLR 3.0.04506.648; .NET CLR
          | 3.5.21022; .NET CLR 3.0.4506.2152; .NET CLR 1.0.3705; .NET CLR 1.1.4322; .NET CLR
          | 3.5.30729; Release=ARP)" "UD-1" - "image/jpeg" "whatever" 0.352 "-" - "" 256 977 988 ""
          | 0 73.23.2.15 images.com 1358492557 - Whatup""".stripMargin.lines.mkString
      )
      def main(args: Array[String]) {
        val sparkconf = new SparkConf().setAppName("Log Query")
        val sc = new SparkContext(sparkconf)
        val dataSet = sc.parallelize(exampleApacheLogs)
        // scalastyle:off
        val apacheLogRegex =
          """^([\d.]+) (\S+) (\S+) \[([\w\d:/]+\s[+\-]\d{4})\] "(.+?)" (\d{3}) ([\d\-]+) "([^"]+)" "([^"]+)".*""".r
        // scalastyle:on
        /** Tracks the total query count and number of aggregate bytes for a particular group. */
        class Stats(val count: Int, val numBytes: Int) extends Serializable {
          def merge(other: Stats): Stats = new Stats(count + other.count, numBytes + other.numBytes)
          override def toString: String = "bytes=%s\tn=%s".format(numBytes, count)
        }
        def extractKey(line: String): (String, String, String) = {
          apacheLogRegex.findFirstIn(line) match {
            case Some(apacheLogRegex(ip, _, user, dateTime, query, status, bytes, referer, ua)) =>
              if (user != "\"-\"") (ip, user, query)
              else (null, null, null)
            case _ => (null, null, null)
          }
        }
        def extractStats(line: String): Stats = {
          apacheLogRegex.findFirstIn(line) match {
            case Some(apacheLogRegex(ip, _, user, dateTime, query, status, bytes, referer, ua)) =>
              new Stats(1, bytes.toInt)
            case _ => new Stats(1, 0)
          }
        }
        
        dataSet.map(line => (extractKey(line), extractStats(line)))
          .reduceByKey((a, b) => a.merge(b))
          .collect().foreach{
          case (user, query) => println("%s\t%s".format(user, query))}

        sc.stop()
      }
    }


1. Richten Sie Breakpoints ein, und klicken Sie dann auf das Symbol **Debug** (Debuggen).

   ![Klicken auf das Symbol „Debug“ (Debuggen)](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debug-icon.png)

2. Wenn die Programmausführung den Breakpoint erreicht, werden eine Registerkarte **Driver** (Treiber) und zwei Registerkarten **Executor** im Bereich **Debugger** angezeigt. Klicken Sie auf das Symbol **Resume Program** (Programm fortsetzen), um die Ausführung des Codes fortzusetzen. Die Ausführung erreicht dann den nächsten Breakpoint, und die entsprechende Registerkarte **Executor** wird angezeigt. Sie können die Ausführungsprotokolle auf der entsprechenden Registerkarte **Console** (Konsole) überprüfen.

   ![Registerkarte „Debugging“](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debugger-tab.png)

### <a name="scenario-2-perform-remote-debugging-and-bug-fixing"></a>Szenario 2: Remotedebuggen und Beheben von Fehlern
In diesem Abschnitt erfahren Sie, wie Sie den Wert den Variable dynamisch mit den Debugfunktionen von IntelliJ für eine schnelle Fehlerbehebung aktualisieren. Im folgenden Codebeispiel wird eine Ausnahme ausgelöst, weil die Zieldatei bereits vorhanden ist.
  
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext

        object SparkCore_WasbIOTest {
          def main(arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("SparkCore_WasbIOTest")
            val sc = new SparkContext(conf)
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

            // Find the rows that have only one digit in the sixth column.
            val rdd1 = rdd.filter(s => s.split(",")(6).length() == 1)

            try {
              var target = "wasb:///HVACout2_testdebug1";
              rdd1.saveAsTextFile(target);
            } catch {
              case ex: Exception => {
                throw ex;
              }
            }
          }
        }


#### <a name="to-perform-remote-debugging-and-bug-fixing"></a>So führen Sie Remotedebuggen und Fehlerbehebung durch
1. Richten Sie zwei Breakpoints ein, und klicken Sie dann auf das Symbol **Debug** (Debuggen), um das Remotedebuggen zu starten.

2. Der Code wird am ersten Breakpoint angehalten, und die Informationen zu Parametern und Variablen werden im Bereich **Variables** (Variablen) angezeigt. 

3. Klicken Sie auf das Symbol **Resume Program** (Programm fortsetzen), um die Ausführung fortzusetzen. Der Code wird am zweiten Punkt beendet. Die Ausnahme wird wie erwartet abgefangen.

  ![Fehler auslösen](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-throw-error.png) 

4. Klicken Sie erneut auf das Symbol **Resume Program** (Programm fortsetzen). Im Fenster **HDInsight Spark Submission** (HDInsight Spark-Übermittlung) wird ein Fehler bei der Auftragsausführung angezeigt.

  ![Fehler beim Übermitteln](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-error-submission.png) 

5. Um den Wert der Variablen mithilfe der Debugfunktion von IntelliJ dynamisch zu aktualisieren, wählen Sie erneut **Debug** (Debuggen) aus. Der Bereich **Variables** (Variablen) wird wieder angezeigt. 

6. Klicken Sie mit der Maustaste auf die Registerkarte **Debug**, und wählen Sie dann **Set Value** (Wert festlegen) aus. Als Nächstes geben Sie einen neuen Wert für die Variable ein. Wählen Sie dann **Enter** (Eingabe) aus, um den Wert zu speichern. 

  ![Wert festlegen](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-set-value.png) 

7. Klicken Sie auf das Symbol **Resume Program** (Programm fortsetzen), um die Programmausführung fortzusetzen. Dieses Mal wird keine Ausnahme abgefangen. Sie können sehen, dass das Projekt erfolgreich und ohne Ausnahmen ausgeführt wird.

  ![Debuggen ohne Ausnahme](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debug-without-exception.png)

## <a name="seealso"></a>Nächste Schritte
* [Übersicht: Apache Spark in Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="demo"></a>Demo
* Erstellen von Scala-Projekten (Video): [Create Spark Scala Applications](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) (Erstellen von Spark Scala-Anwendungen)
* Remotedebuggen (Video): [Use Azure Toolkit for IntelliJ to debug Spark applications remotely on an HDInsight cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) (Remotedebuggen von Spark-Anwendungen in einem HDInsight-Cluster mit dem Azure-Toolkit für IntelliJ)

### <a name="scenarios"></a>Szenarien
* [Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](hdinsight-apache-spark-use-bi-tools.md)
* [Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark-Streaming: Erstellen von Echtzeitstreaminganwendungen mithilfe von Spark in HDInsight](hdinsight-apache-spark-eventhub-streaming.md)
* [Websiteprotokollanalyse mithilfe von Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Erstellen und Ausführen von Anwendungen
* [Erstellen einer eigenständigen Anwendung mit Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Remoteausführung von Aufträgen in einem Spark-Cluster mithilfe von Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools und Erweiterungen
* [Erstellen von Spark-Anwendungen für einen HDInsight-Cluster mit dem Azure-Toolkit für IntelliJ](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Verwenden des Azure-Toolkits für IntelliJ zum Remotedebuggen von Spark-Anwendungen über VPN](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verwenden der HDInsight-Tools für IntelliJ mit Hortonworks Sandbox](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Verwenden der HDInsight-Tools im Azure-Toolkit für Eclipse zum Erstellen von Spark-Anwendungen](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Verwenden von Zeppelin-Notebooks mit einem Spark-Cluster in HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Kernel für Jupyter-Notebook in Spark-Clustern in Azure HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Verwenden von externen Paketen mit Jupyter Notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen
* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight(Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](hdinsight-apache-spark-job-debugging.md)

