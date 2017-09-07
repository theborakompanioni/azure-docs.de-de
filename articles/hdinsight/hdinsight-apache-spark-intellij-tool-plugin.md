---
title: "Azure Toolkit für IntelliJ: Erstellen von Spark-Anwendungen für einen HDInsight-Cluster | Microsoft-Dokumentation"
description: "Verwenden Sie das Azure Toolkit für IntelliJ, um in Scala geschriebene Spark-Anwendungen zu entwickeln und diese an einen HDInsight Spark-Cluster zu senden."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 73304272-6c8b-482e-af7c-cd25d95dab4d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: nitinme
ms.translationtype: HT
ms.sourcegitcommit: 5b6c261c3439e33f4d16750e73618c72db4bcd7d
ms.openlocfilehash: 19cb8f436fa4d86f323013a5d4b3b50bf6c80a1a
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---
# <a name="use-azure-toolkit-for-intellij-to-create-spark-applications-for-an-hdinsight-cluster"></a>Erstellen von Spark-Anwendungen für einen HDInsight-Cluster mit dem Azure Toolkit für IntelliJ

Verwenden Sie das Plug-In „Azure-Toolkit für IntelliJ“, um in Scala geschriebene Spark-Anwendungen zu entwickeln und diese dann direkt aus der IntelliJ-IDE (Integrated Development Environment, integrierte Entwicklungsumgebung) an einen HDInsight Spark-Cluster zu senden. Sie können das Plug-In auf mehrere Arten verwenden:

* Entwickeln und Übermitteln einer Scala Spark-Anwendung an einen HDInsight Spark-Cluster
* Zugreifen auf Ihre Azure HDInsight Spark-Clusterressourcen
* Entwickeln und lokales Ausführen einer Scala Spark-Anwendung

Um das Projekt zu erstellen, sehen Sie sich das Video [Erstellen von Spark-Anwendungen mit dem Azure-Toolkit für IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) an.

> [!IMPORTANT]
> Sie können dieses Plug-In nur zum Erstellen von Anwendungen und Übermitteln dieser an einen HDInsight Spark-Cluster unter Linux verwenden.
> 

## <a name="prerequisites"></a>Voraussetzungen

- Einen Apache Spark-Cluster unter HDInsight (Linux). Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
- Oracle Java Development Kit. Sie können das Kit über die [Oracle-Website](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) installieren.
- IntelliJ IDEA. In diesem Artikel wird die Version 2017.1 verwendet. Sie können die Komponente über die [JetBrains-Website](https://www.jetbrains.com/idea/download/) installieren.

## <a name="install-azure-toolkit-for-intellij"></a>Installieren des Azure-Toolkits für IntelliJ
Installationsanweisungen finden Sie unter [Installieren des Azure-Toolkits für IntelliJ](../azure-toolkit-for-intellij-installation.md).

## <a name="sign-in-to-your-azure-subscription"></a>Melden Sie sich bei Ihrem Azure-Abonnement an.

1. Starten Sie die IntelliJ-IDE, und öffnen Sie den Azure Explorer. Wählen Sie im Menü **Ansicht** die Option **Toolfenster** und dann **Azure Explorer** aus.
       
   ![Link „Azure Explorer“](./media/hdinsight-apache-spark-intellij-tool-plugin/show-azure-explorer.png)

2. Klicken Sie mit der rechten Maustaste auf den Knoten **Azure**, und wählen Sie dann **Anmelden** aus.

3. Wählen Sie im Dialogfeld **Azure-Anmeldung** die Option **Anmelden** aus, und geben Sie Ihre Azure-Anmeldeinformationen ein.

    ![Dialogfeld „Azure-Anmeldung“](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-2.png)

4. Nachdem Sie sich angemeldet haben, werden im Dialogfeld **Abonnements auswählen** alle Azure-Abonnements aufgelistet, die den Anmeldeinformationen zugeordnet sind. Wählen Sie die Schaltfläche **Auswählen** aus.

    ![Dialogfeld zum Auswählen von Abonnements](./media/hdinsight-apache-spark-intellij-tool-plugin/Select-Subscriptions.png)

5. Erweitern Sie auf der Registerkarte **Azure Explorer** die Option **HDInsight**, um die HDInsight Spark-Cluster in Ihrem Abonnement anzuzeigen.
   
    ![HDInsight Spark-Cluster im Azure Explorer](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-3.png)

6. Um die dem Cluster zugeordneten Ressourcen (z.B. Speicherkonten) anzuzeigen, können Sie einen Clusternamenknoten noch einmal erweitern.
   
    ![Ein erweiterter Clusternamenknoten](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-4.png)

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>Ausführen einer Spark Scala-Anwendung in einem HDInsight Spark-Cluster

1. Starten Sie IntelliJ IDEA, und erstellen Sie ein Projekt. Führen Sie im Dialogfeld **Neues Projekt** folgende Schritte aus: 

   a. Wählen Sie **HDInsight** > **Spark auf HDInsight (Scala)** aus.

   b. Wählen Sie in der Liste **Build-Tool** je nach Ihren Anforderungen einen der folgenden Einträge aus:

      * **Maven** für die Unterstützung des Scala-Projekterstellungs-Assistenten
      * **SBT** zum Verwalten von Abhängigkeiten und Erstellen für das Scala-Projekt

    ![Dialogfeld „Neues Projekt“](./media/hdinsight-apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)

2. Klicken Sie auf **Weiter**.

3. Der Scala-Projekterstellungs-Assistent erkennt automatisch, ob das Scala-Plug-In installiert ist. Wählen Sie **Installieren** aus.

   ![Scala-Plug-In-Prüfung](./media/hdinsight-apache-spark-intellij-tool-plugin/Scala-Plugin-check-Reminder.PNG) 

4. Wählen Sie **OK** aus, um das Scala-Plug-In herunterzuladen. Befolgen Sie die Anweisungen zum Neustart von IntelliJ. 

   ![Das Installationsdialogfeld für das Scala Plug-In](./media/hdinsight-apache-spark-intellij-tool-plugin/Choose-Scala-Plugin.PNG)

5. Führen Sie im Fenster **Neues Projekt** folgende Schritte aus:  

    ![Auswählen des Spark-SDK](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-new-project.png)

   a. Geben Sie einen Projektnamen und einen Speicherort ein.

   b. Wählen Sie in der Dropdownliste **Projekt-SDK** eine **Java 1.8** für Spark 2.x-Cluster bzw. **Java 1.7** für Spark 1.x-Cluster aus.

   c. In der Dropdownliste **Spark-Version** fügt der Scala-Projekterstellungs-Assistent die richtige Version von Spark-SDK und Scala-SDK ein. Wenn die Spark-Clusterversion niedriger als 2.0 ist, wählen Sie **Spark 1.x** aus. Wählen Sie andernfalls **Spark 2.x** aus. In diesem Beispiel wird **Spark 2.0.2 (Scala 2.11.8)** verwendet.

6. Wählen Sie **Fertig stellen** aus.

7. Das Spark-Projekt erstellt automatisch ein Artefakt für Sie. Um das Artefakt anzuzeigen, gehen Sie folgendermaßen vor:

   a. Wählen Sie im Menü **File** (Datei) die Option **Project Structure** (Projektstruktur) aus.

   b. Wählen Sie im Dialogfeld **Project Structure** (Projektstruktur) die Option **Artifacts** (Artefakte) aus, um das Standardartefakt anzuzeigen, das erstellt wird. Sie können auch ein eigenes Artefakt erstellen, indem Sie das Plus-Symbol (**+**) auswählen.

      ![Informationen zum Artefakt im Dialogfeld](./media/hdinsight-apache-spark-intellij-tool-plugin/default-artifact.png)
      
8. Fügen Sie den Quellcode der Anwendung wie folgt hinzu:

   a. Klicken Sie im Projekt-Explorer mit der rechten Maustaste auf **src**, zeigen Sie auf **Neu**, und wählen Sie dann **Scala Class** (Scala-Klasse) aus.
      
      ![Befehle zum Erstellen einer Scala-Klasse im Projektexplorer](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)

   b. Geben Sie im Dialogfeld **Create New Scala Class** (Neue Scala-Klasse erstellen) einen Namen ein. Wählen Sie unter **Kind** (Art) die Option **Object** aus, und wählen Sie dann **OK** aus.
      
      ![Dialogfeld „Create New Scala Class“ (Neue Scala-Klasse erstellen)](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)

   c. Fügen Sie den folgenden Code in die Datei **MyClusterApp.scala** ein. Der Code liest die Daten aus der Datei „HVAC.csv“ (die für alle HDInsight Spark-Cluster verfügbar ist), ruft die Zeilen ab, die nur eine Ziffer in der siebten Spalte der CSV-Datei enthalten, und schreibt die Ausgabe in **/HVACOut** unter dem Standardspeichercontainer für den Cluster.

        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
    
        object MyClusterApp{
            def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
    
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    
            //find the rows that have only one digit in the seventh column in the CSV file
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
    
            rdd1.saveAsTextFile("wasb:///HVACOut")
            }
    
        }

9. Führen Sie die Anwendung folgendermaßen in einem HDInsight Spark-Cluster aus:

   a. Klicken Sie im Projektexplorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie dann **Submit Spark Application to HDInsight** (Spark-Anwendung an HDInsight senden).
      
      ![Befehl „Submit Spark Application to HDInsight“](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)

   b. Sie werden zur Eingabe der Anmeldeinformationen Ihres Azure-Abonnements aufgefordert. Geben Sie in das Dialogfeld **Spark Submission** (Spark-Übermittlung) die folgenden Werte ein, und wählen Sie dann **Submit** (Übermitteln) aus.
      
      * Wählen Sie für **Spark clusters (Linux only)**(Spark-Cluster [nur Linux]) den HDInsight Spark-Cluster aus, in dem Sie Ihre Anwendung ausführen möchten.

      * Wählen Sie ein Artefakt aus dem IntelliJ-Projekt oder von der Festplatte aus.

      * Wählen Sie im Feld **Main class name** (Hauptklassenname) die Auslassungspunkte (**...**) aus, wählen Sie die Hauptklasse in Ihrem Anwendungsquellcode aus, und wählen Sie dann **OK** aus.

        ![Dialogfeld „Select Main Class“ (Hauptklasse auswählen)](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-3.png)

      * Da für den Anwendungscode in diesem Beispiel keine Befehlszeilenargumente oder Referenzdateien (JAR) erforderlich sind, können Sie die restlichen Felder leer lassen. Nachdem Sie alle Informationen angegeben haben, sollte das Dialogfeld in etwa wie in der folgenden Abbildung aussehen.
        
        ![Dialogfeld für die Spark-Übermittlung](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-2.png)

   c. Auf der Registerkarte **Spark Submission** (Spark-Übermittlung) unten im Fenster sollte nun der Status angezeigt werden. Sie können die Anwendung auch anhalten, indem Sie im Fenster **Spark Submission** (Spark-Übermittlung) die rote Schaltfläche auswählen.
      
      ![Fenster für die Spark-Übermittlung](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)
      
      Weiter unten in diesem Artikel im Abschnitt „Zugreifen auf und Verwalten von HDInsight Spark-Clustern mit dem Azure-Toolkit für IntelliJ“ wird beschrieben, wie Sie auf die Auftragsausgabe zugreifen.

## <a name="run-or-debug-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>Ausführen oder Debuggen einer Spark Scala-Anwendung in einem HDInsight Spark-Cluster
Es wird noch eine andere Möglichkeit der Übermittlung von Spark Anwendungen an den Cluster empfohlen. Dazu werden die Parameter in der IDE **Run/Debug Configurations** (Ausführen/Debugkonfigurationen) festgelegt. Weitere Informationen finden Sie unter [Remotedebuggen von Spark-Anwendungen in einem HDInsight-Cluster mit dem Azure-Toolkit für IntelliJ über SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-azure-toolkit-for-intellij"></a>Zugreifen auf und Verwalten von HDInsight Spark-Clustern mit dem Azure-Toolkit für IntelliJ
Sie können mit dem Azure-Toolkit für IntelliJ verschiedene Vorgänge durchführen.

### <a name="access-the-job-view"></a>Zugreifen auf die Auftragsansicht
1. Erweitern Sie im Azure Explorer die Option **HDInsight** und den Namen des Spark-Clusters, und wählen Sie dann **Jobs** (Aufträge) aus.  

    ![Knoten „Auftragsansicht“](./media/hdinsight-apache-spark-intellij-tool-plugin/job-view-node.png)

2. Im rechten Bereich werden auf der Registerkarte **Spark Job View** (Spark-Auftragsansicht) alle Anwendungen angezeigt, die in dem Cluster ausgeführt wurden. Wählen Sie den Namen der Anwendung aus, zu der Sie weitere Details anzeigen möchten.

    ![Anwendungsdetails](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)

3. Wenn Sie grundlegende Informationen zum ausgeführten Auftrag anzeigen möchten, zeigen Sie auf das Auftragsdiagramm. Um das Phasendiagramm und von den einzelnen Aufträgen generierte Informationen anzuzeigen, wählen Sie einen Knoten im Auftragsdiagramm aus.

    ![Details zur Auftragsphase](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

4. Um häufig verwendete Protokolle anzuzeigen, z.B. *Driver Stderr*, *Driver Stdout* oder *Directory Info*, wählen Sie die Registerkarte **Log** (Protokoll) aus.

    ![Protokolldetails](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-log-info.png)

5. Sie können auch die Spark History UI (Spark-Verlaufsbenutzeroberfläche) und die YARN UI (YARN-Benutzeroberfläche) (auf Anwendungsebene) anzeigen, indem Sie im oberen Bereich des Fensters einen Link auswählen.

### <a name="access-the-spark-history-server"></a>Zugreifen auf den Spark-Verlaufsserver
1. Erweitern Sie im Azure Explorer die Option **HDInsight**, klicken Sie mit der rechten Maustaste auf den Namen Ihres Spark-Clusters, und wählen Sie dann **Open Spark History UI** (Spark-Verlaufsbenutzeroberfläche öffnen). 

2. Wenn Sie dazu aufgefordert werden, geben Sie die Administratoranmeldeinformationen für den Cluster ein, die Sie beim Einrichten einer Verbindung mit dem Cluster angegeben haben.

3. Im Dashboard des Spark-Verlaufsservers können Sie anhand des Anwendungsnamens nach der Anwendung suchen, deren Ausführung Sie gerade beendet haben. Im obigen Code legen Sie den Namen der Anwendung mit `val conf = new SparkConf().setAppName("MyClusterApp")` fest. Daher lautet der Name Ihrer Spark-Anwendung **MyClusterApp**.

### <a name="start-the-ambari-portal"></a>Starten des Ambari-Portals
1. Erweitern Sie im Azure Explorer die Option **HDInsight**, klicken Sie mit der rechten Maustaste auf den Namen Ihres Spark-Clusters, und wählen Sie dann **Open Cluster Management Portal (Ambari)** (Clusterverwaltungsportal (Ambari) öffnen). 

2. Geben Sie die Anmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden. Sie haben diese Anmeldeinformationen während der Einrichtung des Clusters angegeben.

### <a name="manage-azure-subscriptions"></a>Verwalten von Azure-Abonnements
Standardmäßig werden mit dem Azure-Toolkit für IntelliJ die Spark-Cluster Ihrer gesamten Azure-Abonnements aufgelistet. Bei Bedarf können Sie die Abonnements angeben, auf die Sie zugreifen möchten. 

1. Klicken Sie im Azure Explorer mit der rechten Maustaste auf den Hauptknoten **Azure**, und wählen Sie dann **Abonnements verwalten** aus. 

2. Deaktivieren Sie im Dialogfeld die Kontrollkästchen neben den Abonnements, auf die Sie nicht zugreifen möchten, und wählen Sie dann **Schließen** aus. Sie können auch **Abmelden** auswählen, wenn Sie sich von Ihrem Azure-Abonnement abmelden möchten.

## <a name="run-a-spark-scala-application-locally"></a>Lokales Ausführen einer Spark Scala-Anwendung
Mit dem Azure-Toolkit für IntelliJ können Sie Spark Scala-Anwendungen lokal auf Ihrer Arbeitsstation ausführen. Die Anwendungen müssen normalerweise nicht auf Clusterressourcen wie Speichercontainer zugreifen und können lokal ausgeführt und getestet werden.

### <a name="prerequisite"></a>Voraussetzung
Beim Ausführen der lokalen Spark Scala-Anwendung auf einem Windows-Computer wird unter Umständen eine Ausnahme wie unter [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356) beschrieben ausgelöst. Diese Ausnahme tritt auf, weil die Datei „WinUtils.exe“ in Windows fehlt. 

Um diesen Fehler zu beheben, müssen Sie [die ausführbare Datei herunterladen](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) und an einem Speicherort wie **C:\WinUtils\bin** speichern. Fügen Sie anschließend die Umgebungsvariable **HADOOP_HOME** hinzu, und legen Sie den Wert der Variable auf **C\WinUtils** fest.

### <a name="run-a-local-spark-scala-application"></a>Ausführen einer lokalen Spark Scala-Anwendung
1. Starten Sie IntelliJ IDEA, und erstellen Sie ein Projekt. 

2. Führen Sie im Dialogfeld **Neues Projekt** folgende Schritte aus:
   
    a. Wählen Sie **HDInsight** > **Spark on HDInsight Local Run Sample (Scala)** (Spark auf HDInsight – Beispiel für lokale Ausführung [Scala]) aus.

    b. Wählen Sie in der Liste **Build-Tool** je nach Ihren Anforderungen einen der folgenden Einträge aus:

      * **Maven** für die Unterstützung des Scala-Projekterstellungs-Assistenten
      * **SBT** zum Verwalten von Abhängigkeiten und Erstellen für das Scala-Projekt

    ![Dialogfeld „Neues Projekt“](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run.png)

3. Klicken Sie auf **Weiter**.
 
4. Gehen Sie im nächsten Fenster folgendermaßen vor:
   
    a. Geben Sie einen Projektnamen und einen Speicherort ein.

    b. Wählen Sie in der Dropdownliste **Project SDK** eine Java-Version nach Version 1.7 aus.

    c. Wählen Sie in der Dropdownliste **Spark Version** die Version von Scala aus, die Sie verwenden möchten: Scala 2.11.x für Spark 2.0 oder Scala 2.10.x für Spark 1.6.

    ![Dialogfeld „Neues Projekt“](./media/hdinsight-apache-spark-intellij-tool-plugin/Create-local-project.PNG)

5. Wählen Sie **Fertig stellen** aus.

6. Die Vorlage fügt einen Beispielcode (**LogQuery**) unter dem Ordner **src** hinzu, der lokal auf Ihrem Computer ausgeführt werden kann.
   
    ![Speicherort von LogQuery](./media/hdinsight-apache-spark-intellij-tool-plugin/local-app.png)

7. Klicken Sie mit der rechten Maustaste auf die Anwendung **LogQuery**, und wählen Sie dann **Run 'LogQuery'** („LogQuery“ ausführen) aus. Unten auf der Registerkarte **Run** (Ausführen) wird eine Ausgabe angezeigt, die in etwa wie folgt aussieht:
   
   ![Ergebnis der lokalen Ausführung der Spark-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="convert-existing-intellij-idea-applications-to-use-azure-toolkit-for-intellij"></a>Konvertieren vorhandener IntelliJ IDEA-Anwendungen für die Verwendung des Azure-Toolkits für IntelliJ
Sie können die vorhandenen Spark Scala-Anwendungen, die Sie in IntelliJ IDEA erstellt haben, so umwandeln, dass sie mit dem Azure-Toolkit für IntelliJ kompatibel sind. Anschließend können Sie das Plug-In verwenden, um die Anwendungen an einen HDInsight Spark-Cluster zu übermitteln.

1. Öffnen Sie die zugehörige IML-Datei einer vorhandenen Spark Scala-Anwendung, die mit IntelliJ IDEA erstellt wurde.

2. Auf der Stammebene befindet sich ein **module**-Element wie das folgende:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">

   Fügen Sie dem Element `UniqueKey="HDInsightTool"` hinzu, damit das **module** -Element wie folgt aussieht:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">

3. Speichern Sie die Änderungen. Ihre Anwendung sollte jetzt mit dem Azure-Toolkit für IntelliJ kompatibel sein. Sie können dies testen, indem Sie im Projektexplorer mit der rechten Maustaste auf den Projektnamen klicken. Im Popupmenü ist jetzt die Option **Submit Spark Application to HDInsight** (Spark-Anwendung an HDInsight übermitteln) verfügbar.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="error-in-local-run-please-use-a-larger-heap-size"></a>Fehler bei lokaler Ausführung: *Bitte verwenden Sie einen größeren Heap*
Sie erhalten unter Umständen die folgenden Fehlermeldungen, wenn Sie bei der lokalen Ausführung in Spark 1.6 ein 32-Bit-Java SDK verwenden:

    Exception in thread "main" java.lang.IllegalArgumentException: System memory 259522560 must be at least 4.718592E8. Please use a larger heap size.
        at org.apache.spark.memory.UnifiedMemoryManager$.getMaxMemory(UnifiedMemoryManager.scala:193)
        at org.apache.spark.memory.UnifiedMemoryManager$.apply(UnifiedMemoryManager.scala:175)
        at org.apache.spark.SparkEnv$.create(SparkEnv.scala:354)
        at org.apache.spark.SparkEnv$.createDriverEnv(SparkEnv.scala:193)
        at org.apache.spark.SparkContext.createSparkEnv(SparkContext.scala:288)
        at org.apache.spark.SparkContext.<init>(SparkContext.scala:457)
        at LogQuery$.main(LogQuery.scala:53)
        at LogQuery.main(LogQuery.scala)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:606)
        at com.intellij.rt.execution.application.AppMain.main(AppMain.java:144)

Diese Fehler treten auf, weil die Heapgröße für die Ausführung von Spark nicht groß genug ist. Für Spark sind mindestens 471 MB erforderlich. (Weitere Informationen finden Sie unter [SPARK-12081](https://issues.apache.org/jira/browse/SPARK-12081).) Eine einfache Lösung ist die Verwendung eines 64-Bit-Java SDK. Sie können auch die JVM-Einstellungen in IntelliJ ändern, indem Sie die folgenden Optionen hinzufügen:

    -Xms128m -Xmx512m -XX:MaxPermSize=300m -ea

![Hinzufügen von Optionen zum Feld „VM options“ (VM-Optionen) in IntelliJ](./media/hdinsight-apache-spark-intellij-tool-plugin/change-heap-size.png)

## <a name="faq"></a>Häufig gestellte Fragen
Um die Anwendung an Azure Data Lake Store zu übermitteln, wählen Sie während des Azure-Anmeldeprozesses den Modus **Interaktiv** aus. Bei Auswahl von **Automatisiert** tritt unter Umständen ein Fehler auf.

![Interaktiv (Anmeldung)](./media/hdinsight-apache-spark-intellij-tool-plugin/interative-signin.png)

Nun wurde er behoben. Sie können einen Azure Data Lake-Cluster zum Übermitteln Ihrer Anwendung mit jeder Anmeldemethode auswählen.

## <a name="feedback-and-known-issues"></a>Feedback und bekannte Probleme
Derzeit wird die direkte Anzeige von Spark-Ausgaben nicht unterstützt.

Falls Sie Vorschläge oder Feedback haben oder bei Verwendung dieses Plug-Ins Probleme auftreten, senden Sie eine E-Mail an hdivstool@microsoft.com.

## <a name="seealso"></a>Nächste Schritte
* [Übersicht: Apache Spark in Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="demo"></a>Demo
* Erstellen von Scala-Projekten (Video): [Create Spark Scala Applications](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) (Erstellen von Spark Scala-Anwendungen)
* Remotedebuggen (Video): [Use Azure Toolkit for IntelliJ to debug Spark applications remotely on HDInsight Cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) (Verwenden des Azure Toolkits für IntelliJ zum Remotedebuggen von Spark-Anwendungen in HDInsight-Clustern)

### <a name="scenarios"></a>Szenarien
* [Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](hdinsight-apache-spark-use-bi-tools.md)
* [Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark-Streaming: Erstellen von Echtzeitstreaminganwendungen mithilfe von Spark in HDInsight](hdinsight-apache-spark-eventhub-streaming.md)
* [Websiteprotokollanalyse mithilfe von Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Erstellen und Ausführen von Anwendungen
* [Erstellen einer eigenständigen Anwendung mit Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Remoteausführung von Aufträgen in einem Spark-Cluster mithilfe von Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools und Erweiterungen
* [Verwenden des Azure-Toolkits für IntelliJ zum Remotedebuggen von Spark-Anwendungen über VPN](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verwenden des Azure-Toolkits für IntelliJ zum Remotedebuggen von Spark-Anwendungen per SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Verwenden der HDInsight-Tools für IntelliJ mit Hortonworks Sandbox](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Verwenden der HDInsight-Tools im Azure-Toolkit für Eclipse zum Erstellen von Spark-Anwendungen](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Verwenden von Zeppelin-Notebooks mit einem Spark-Cluster in HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Verfügbare Kernels für Jupyter-Notebook im Spark-Cluster für HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Verwenden von externen Paketen mit Jupyter Notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Verwalten von Ressourcen
* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight(Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](hdinsight-apache-spark-job-debugging.md)


