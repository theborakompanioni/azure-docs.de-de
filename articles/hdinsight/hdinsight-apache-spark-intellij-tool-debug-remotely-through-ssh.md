---
title: "Azure-Toolkit für IntelliJ – Remotedebuggen von Spark-Anwendungen per SSH | Microsoft-Dokumentation"
description: "Hier finden Sie eine ausführliche Anleitung zur Verwendung der HDInsight-Tools im Azure-Toolkit für IntelliJ zum Remotedebuggen von Anwendungen in HDInsight-Clustern per SSH."
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
ms.date: 06/05/2017
ms.author: Jenny Jiang
ms.translationtype: HT
ms.sourcegitcommit: c999eb5d6b8e191d4268f44d10fb23ab951804e7
ms.openlocfilehash: 5ee6504cd4bf69c8f2c14a3623ff537e66030ce1
ms.contentlocale: de-de
ms.lasthandoff: 07/17/2017

---
# <a name="remotely-debug-spark-applications-on-an-hdinsight-cluster-with-azure-toolkit-for-intellij-through-ssh"></a>Remotedebuggen von Spark-Anwendungen in einem HDInsight-Cluster mit dem Azure-Toolkit für IntelliJ per SSH

Dieser Artikel enthält eine ausführliche Anleitung zur Verwendung der HDInsight-Tools im Azure-Toolkit für IntelliJ zum Remotedebuggen von Anwendungen in HDInsight-Clustern. Sie können sich auch ein [Video](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) zum Debuggen des Projekts ansehen.

**Voraussetzungen:**

* **HDInsight-Tools im Azure-Toolkit für IntelliJ.** Dieser Teil des Azure-Toolkits für IntelliJ. Weitere Informationen finden Sie unter [Installieren des Azure-Toolkits für IntelliJ](https://docs.microsoft.com/en-us/azure/azure-toolkit-for-intellij-installation).
* **Azure-Toolkit für IntelliJ.** Verwenden Sie es zum Erstellen von Spark-Anwendungen für HDInsight-Cluster. Weitere Informationen finden Sie in den Anweisungen unter [Erstellen von Spark-Anwendungen für HDInsight-Cluster mit dem Azure-Toolkit für IntelliJ](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin).
* **HDInsight SSH-Dienst mit Verwaltung von Benutzername und Kennwort.** Weitere Informationen finden Sie unter [Herstellen von Verbindungen mit HDInsight (Hadoop)](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) und [Verwenden von SSH-Tunneln für den Zugriff auf die Ambari-Webbenutzeroberfläche, JobHistory, NameNode, Oozie und andere Webbenutzeroberflächen](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-linux-ambari-ssh-tunnel). 
 

## <a name="create-a-spark-scala-application-and-configure-it-for-remote-debugging"></a>Erstellen einer Spark Scala-Anwendung und Konfigurieren dieser für das Remotedebuggen
1. Starten Sie IntelliJ IDEA, und erstellen Sie ein Projekt. Wählen Sie im Dialogfeld für das neue Projekt Folgendes aus, und klicken Sie anschließend auf **Weiter**: In diesem Artikel wird ein **Beispiel für die Ausführung von Spark in HDInsight-Clustern (Scala)** verwendet.

     ![Erstellen eines Debugprojekts](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-create-projectfor-debug-remotely.png)
   - Wählen Sie im linken Bereich **HDInsight** aus.
   - Wählen Sie im rechten Bereich eine Java- oder Scala-Vorlage nach Ihren Anforderungen aus. Es stehen die folgenden Optionen zur Auswahl: 
      - **Spark auf HDInsight (Scala)**
      - **Spark auf HDInsight (Java)**
      - **Beispiel für die Ausführung von Spark auf HDInsight-Clustern (Scala)**
   - Buildtool: Scala-Projekterstellungs-Assistent unterstützt Maven oder SBT bei Verwaltung der Abhängigkeiten und Erstellen für Scala-Projekte. Treffen Sie Ihre Auswahl entsprechend Ihren Anforderungen.
2. Geben Sie im nächsten Fenster die Projektdetails an.

   ![Auswählen des Spark-SDK](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-new-project.png)
   - Geben Sie einen Projektnamen und den Projektspeicherort an.
   - Als **Projekt-SDK** verwenden Sie **Java 1.8** für **Spark 2.x**-Cluster oder **Java 1.7** für **Spark 1.x**-Cluster.
   - Als **Spark-Version** integriert der Scala-Projekterstellungs-Assistent die richtige Version von Spark-SDK und Scala-SDK. Wenn die Spark-Clusterversion niedriger als 2.0 ist, wählen Sie **Spark 1.x** aus. Wählen Sie andernfalls **Spark 2.x** aus. In diesem Artikel wird als Beispiel **Spark 2.0.2 (Scala 2.11.8)** verwendet.
3. Wählen Sie **src** > **main** > **scala** aus, um Ihren Code im Projekt zu öffnen. In diesem Artikel wird als Beispiel das Skript **SparkCore_wasbloTest** verwendet.
4. Für den Zugriff auf das Menü **Edit Configurations** (Konfigurationen bearbeiten) wählen Sie das Symbol in der rechten oberen Ecke aus. Über dieses Menü können Sie die Konfigurationen für das Remotedebuggen erstellen und bearbeiten.

   ![Konfigurationen bearbeiten](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-edit-configurations.png) 
5. Klicken Sie im Fenster **Run/Debug Configurations** (Ausführen/Debugkonfigurationen) auf das Pluszeichen (**+**). Wählen Sie dann die Option **Submit Spark Job** (Spark-Auftrag übermitteln) aus.

   ![Konfigurationen bearbeiten](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-add-new-Configuration.png)
6. Geben Sie Informationen für **Name**, **Spark cluster** und **Main class name** (Name der main-Klasse) ein. Wählen Sie anschließend **Advanced configuration** (Erweiterte Konfiguration) aus. 

   ![Debugkonfigurationen ausführen](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-run-debug-configurations.png)
7. Wählen Sie im Dialogfeld **Spark Submission Advanced Configuration** (Erweiterte Konfiguration für Spark Übermittlung) die Option **Enable Spark remote debug** (Remotedebuggen für Spark aktivieren) aus. Geben Sie den SSH-Benutzernamen oder das Kennwort ein, oder verwenden Sie eine Datei mit einem privaten Schlüssel. Wählen Sie zum Speichern **Ok** aus.

   ![Remotedebuggen für Spark aktivieren](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-enable-spark-remote-debug.png)
8. Die Konfiguration wird jetzt unter dem von Ihnen angegebenen Namen gespeichert. Um die Konfigurationsdetails anzuzeigen, wählen Sie den Konfigurationsnamen aus. Um Änderungen vorzunehmen, wählen Sie **Edit Configurations** (Konfigurationen bearbeiten) aus. 
9. Nachdem Sie die Konfigurationseinstellungen abgeschlossen haben, können Sie das Projekt für den Remotecluster ausführen oder das Remotedebuggen durchführen.

## <a name="learn-how-to-perform-remote-debugging-and-remote-run"></a>Informationen Sie zum Remotedebuggen und zur Remoteausführung
### <a name="scenario-1-perform-remote-run"></a>Szenario 1: Remoteausführung
1. Wenn Sie das Projekt remote ausführen möchten, wählen Sie das Symbol **Ausführen** aus.

   ![Klicken Sie auf das Symbol „Ausführen“.](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-run-icon.png)

2. Das Fenster **HDInsight Spark Submission** (HDInsight Spark-Übermittlung) zeigt den Ausführungsstatus der Anwendung an. Sie können den Fortschritt des Scala-Auftrags basierend auf den hier gezeigten Informationen überwachen.

   ![Klicken Sie auf das Symbol „Ausführen“.](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-run-result.png)

### <a name="scenario-2-perform-remote-debugging"></a>Szenario 2: Remotedebuggen
1. Richten Sie einen Haltepunkt ein, und wählen Sie dann das Symbol **Debuggen** aus.

    ![Klicken Sie auf das Symbol „Debuggen“.](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debug-icon.png)
2. Wenn die Programmausführung den Haltepunkt erreicht, wird im unteren Bereich die Registerkarte **Debugger** angezeigt. Im Fenster **Variable** werden auch Informationen zu Parametern und Variablen angezeigt. Klicken Sie auf das Symbol **Überspringen**, um zur nächsten Codezeile zu wechseln. Sie können den Code schrittweise durchlaufen. Wählen Sie das Symbol **Programm fortsetzen** aus, um die Codeausführung fortzusetzen. Sie können den Ausführungsstatus im Fenster **HDInsight Spark Submission** (HDInsight Spark-Übermittlung) überprüfen. 

   ![Registerkarte „Debugging“](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debugger-tab.png)

### <a name="scenario-3-perform-remote-debugging-and-bug-fixing"></a>Szenario 3: Remotedebuggen und Beheben von Fehlern
In diesem Abschnitt erfahren Sie, wie Sie den Wert den Variable dynamisch mit den Debugfunktionen von IntelliJ für eine schnelle Fehlerbehebung aktualisieren. Für das folgenden Codebeispiel wird eine Ausnahme ausgelöst, da die Zieldatei bereits vorhanden ist.
  
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
1. Richten Sie zwei Haltepunkte ein, und klicken Sie dann auf das Symbol **Debuggen**, um das Remotedebuggen zu starten.

2. Der Code wird am ersten Haltepunkt unterbrochen, und die Informationen zu Parametern und Variablen werden im Fenster **Variable** angezeigt. 

3. Klicken Sie auf das Symbol **Programm fortsetzen**, um die Ausführung fortzusetzen. Der Code wird am zweiten Punkt beendet. Die Ausnahme wird wie erwartet abgefangen.

  ![Fehler auslösen](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-throw-error.png) 

4. Klicken Sie erneut auf das Symbol **Programm fortsetzen**. Das Fenster **HDInsight Spark Submission** (HDInsight Spark-Übermittlung) zeigt einen Fehler bei der Auftragsausführung an.

  ![Fehler beim Übermitteln](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-error-submission.png) 

5. Um den Wert der Variable mithilfe der Debugfunktionen von IntelliJ dynamisch zu aktualisieren, wählen Sie erneut **Debug** aus. Das Variablenfenster wird erneut angezeigt. 

6. Klicken Sie mit der Maustaste auf die Registerkarte **Debug**, und wählen Sie dann **Set Value** (Wert festlegen) aus. Als Nächstes geben Sie einen neuen Wert für die Variable ein. Wählen Sie dann **Enter** (Eingabe) aus, um den Wert zu speichern. 

  ![Wert festlegen](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-set-value.png) 

7. Klicken Sie auf das Symbol **Programm fortsetzen**, um die Programmausführung fortzusetzen. Dieses Mal wird die Ausnahme nicht abgefangen. Sie können sehen, dass das Projekt erfolgreich und ohne Ausnahmen ausgeführt wird.

  ![Debuggen ohne Ausnahme](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debug-without-exception.png)

## <a name="seealso"></a>Weitere Informationen
* [Übersicht: Apache Spark in Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="demo"></a>Demo
* Erstellen von Scala-Projekten (Video): [Create Spark Scala Applications](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) (Erstellen von Spark Scala-Anwendungen)
* Remotedebuggen (Video): [Use Azure Toolkit for IntelliJ to debug Spark applications remotely on HDInsight Cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) (Verwenden des Azure-Toolkits für IntelliJ zum Remotedebuggen von Spark-Anwendungen in HDInsight-Clustern)

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
* [Verwenden der HDInsight-Tools im Azure-Toolkit für IntelliJ zum Erstellen und Übermitteln von Spark Scala-Anwendungen](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Verwenden des Azure-Toolkits für IntelliJ zum Remotedebuggen von Anwendungen in HDInsight Spark über VPN](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verwenden der HDInsight-Tools für IntelliJ mit Hortonworks Sandbox](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Verwenden der HDInsight-Tools im Azure-Toolkit für Eclipse zum Erstellen von Spark-Anwendungen](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Verwenden von Zeppelin-Notebooks mit einem Spark-Cluster in HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Verfügbare Kernels für Jupyter-Notebook im Spark-Cluster für HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Verwenden von externen Paketen mit Jupyter Notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen
* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight(Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](hdinsight-apache-spark-job-debugging.md)
 
