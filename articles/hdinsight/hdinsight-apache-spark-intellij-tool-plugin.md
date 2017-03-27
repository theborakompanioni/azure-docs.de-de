---
title: "Erstellen von Scala-Anwendungen für Spark mithilfe des Azure-Toolkits für IntelliJ | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie eine eigenständige Spark-Anwendung zur Ausführung in HDInsight Spark-Clustern erstellen."
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
ms.date: 02/06/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: a939a0845d7577185ff32edd542bcb2082543a26
ms.openlocfilehash: 6c513def2abc8c99b84b2eb48848de76e12e69c7
ms.lasthandoff: 01/24/2017


---
# <a name="use-hdinsight-tools-in-azure-toolkit-for-intellij-to-create-spark-applications-for-hdinsight-spark-linux-cluster"></a>Verwenden der HDInsight-Tools im Azure-Toolkit für IntelliJ zum Erstellen von Spark-Anwendungen für HDInsight Spark-Cluster unter Linux
Dieser Artikel enthält eine detaillierte Anleitung zum Entwickeln von in Scala geschriebenen Spark-Anwendungen und zu deren Übermittlung an einen HDInsight Spark-Cluster mithilfe der HDInsight-Tools im Azure-Toolkit für IntelliJ.  Sie können die Tools auf verschiedene Arten verwenden:

* Zum Entwickeln und Übermitteln einer Scala Spark-Anwendung an einen HDInsight Spark-Cluster
* Zum Zugreifen auf Ihre Azure HDInsight Spark-Clusterressourcen
* Zum Entwickeln und lokalen Ausführen einer Scala Spark-Anwendung

Informationen zu den ersten Schritten finden Sie auch in diesem [Video](https://mix.office.com/watch/1nqkqjt5xonza) .

> [!IMPORTANT]
> Dieses Tool kann nur zum Erstellen und Übermitteln von Anwendungen an einen HDInsight Spark-Cluster unter Linux verwendet werden.
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Abonnement. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)(in englischer Sprache).
* Einen Apache Spark-Cluster unter HDInsight (Linux). Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
* Oracle Java Development Kit. Das Installationspaket finden Sie [hier](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* IntelliJ IDEA. In diesem Artikel wird die Version 15.0.1 verwendet. Das Installationspaket finden Sie [hier](https://www.jetbrains.com/idea/download/).

## <a name="install-hdinsight-tools-in-azure-toolkit-for-intellij"></a>Installieren der HDInsight-Tools im Azure-Toolkit für IntelliJ
Die HDInsight-Tools für IntelliJ sind als Teil des Azure-Toolkits für IntelliJ verfügbar. Anweisungen zum Installieren des Azure-Toolkits finden Sie unter [Installieren des Azure Toolkit für IntelliJ](../azure-toolkit-for-intellij-installation.md).

## <a name="log-into-your-azure-subscription"></a>Anmelden bei Ihrem Azure-Abonnement
1. Starten Sie die IntelliJ-IDE, und öffnen Sie den Azure Explorer. Klicken Sie im Menü **Ansicht** in IDE auf **Toolfenster**, und klicken Sie dann auf **Azure Explorer**.
   
    ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/show-azure-explorer.png)
2. Klicken Sie im **Azure Explorer** mit der rechten Maustaste auf den Knoten **Azure**, und klicken Sie auf **Abonnements verwalten**.
3. Klicken Sie im Dialogfeld **Abonnements verwalten** auf **Anmelden**, und geben Sie Ihre Azure-Anmeldeinformationen ein.
   
    ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-2.png)
4. Nachdem Sie sich angemeldet haben, werden im Dialogfeld **Manage Subscriptions** alle Azure-Abonnements aufgelistet, die den Anmeldeinformationen zugeordnet sind. Klicken Sie im Dialogfeld auf **Schließen** .
5. Erweitern Sie auf der Registerkarte **Azure Explorer** **HDInsight**, um die HDInsight Spark-Cluster in Ihrem Abonnement anzuzeigen.
   
    ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-3.png)
6. Sie können des Weiteren einen Clusternamenknoten erweitern, um die mit dem Cluster verbundenen Ressourcen (z.B. Speicherkonten) anzuzeigen.
   
    ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-4.png)

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>Ausführen einer Spark Scala-Anwendung in einem HDInsight Spark-Cluster
1. Starten Sie IntelliJ IDEA, und erstellen Sie ein neues Projekt. Wählen Sie im Dialogfeld für das neue Projekt Folgendes aus, und klicken Sie anschließend auf **Weiter**:
   
    ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)
   
   * Wählen Sie im linken Bereich **HDInsight**aus.
   * Wählen Sie im rechten Bereich **Spark on HDInsight (Scala)**(Spark auf HDInsight [Scala]) aus.
   * Klicken Sie auf **Weiter**.
2. Geben Sie im nächsten Fenster die Projektdetails an.
   
   * Geben Sie einen Projektnamen und den Projektspeicherort an.
   * Für **Project SDK**: mindestens Java 1.7 für Spark 1.6-Cluster und Java 1.8 für Spark 2.0-Cluster.
   * Klicken Sie für **Scala SDK** auf **Erstellen** und dann auf **Herunterladen**, und wählen Sie die zu verwendende Version von Scala aus.
   * * Wählen Sie **JDK 1.8 und Scala 2.11.x** aus, wenn Sie Aufträge an den Spark 2.0-Cluster übermitteln möchten.
   * * Wählen Sie **JDK 1.7 oder höher und Scala 2.10.x** aus, wenn Sie Aufträge an den Spark 1.6-Cluster übermitteln möchten.

        ![](./media/hdinsight-apache-spark-intellij-tool-plugin/show-scala2.11.x-select.png)
   * Laden Sie das **Spark SDK** herunter, und verwenden Sie es. Sie finden das SDK [hier](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409) („spark-assembly-2.0.0-hadoop2.7.0-SNAPSHOT.jar“ für den Spark 2.0-Cluster und „spark-assembly-x.jar“ für den Spark 1.6-Cluster). Sie können stattdessen auch das [Spark Maven Repository](http://mvnrepository.com/search?q=spark) verwenden. Stellen Sie jedoch sicher, dass die richtige Maven-Repository-Version für die Entwicklung Ihrer Spark-Anwendungen installiert ist. (Wenn Sie beispielsweise Spark Streaming verwenden, müssen Sie sicherstellen, dass Sie den Teil für Spark Streaming installiert haben. Vergewissern Sie sich auch, dass Sie für den Spark 1.6-Cluster das mit „Scala 2.10“ markierte Repository und für den Spark 2.0-Cluster das mit „Scala 2.11“ markierte Repository verwenden.)
     
       ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-scala-project-details.png)
   * Klicken Sie auf **Fertig stellen**.
3. Das Spark-Projekt erstellt automatisch ein Artefakt für Sie. Gehen Sie wie folgt vor, um das Artefakt anzuzeigen:
   
   1. Klicken Sie im Menü **Datei** auf **Projektstruktur**.
   2. Klicken Sie im Dialogfeld **Projektstruktur** auf **Artefakte**, um das Standardartefakt anzuzeigen, das erstellt wird.
      
       ![Erstellen einer JAR-Datei](./media/hdinsight-apache-spark-intellij-tool-plugin/default-artifact.png)
      
      Sie können auch eigene Artefakte erstellen, indem Sie auf das in der Abbildung oben hervorgehobene **+** -Symbol klicken.
4. Klicken Sie im Dialogfeld **Projektstruktur** auf **Projekt**. Wenn **Project SDK** auf 1.8 festgelegt ist, stellen Sie sicher, dass die **Projektsprachebene** auf **7 - Diamonds, ARM, multi-catch, etc.** festgelegt ist. (Optional für den Spark 2.0-Cluster.)
   
    ![Festlegen der Sprache auf Projektebene](./media/hdinsight-apache-spark-intellij-tool-plugin/set-project-language-level.png)
5. Fügen Sie den Quellcode der Anwendung hinzu.
   
   1. Klicken Sie im **Projektexplorer** mit der rechten Maustaste auf **src**, zeigen Sie auf **Neu**, und klicken Sie dann auf **Scala class** (Scala-Klasse).
      
       ![Hinzufügen von Quellcode](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)
   2. Geben Sie im Dialogfeld **Create New Scala Class** (Neue Scala-Klasse erstellen) einen Namen ein. Wählen Sie **Object** (Objekt) für **Kind** (Variante) aus, und klicken Sie dann auf **OK**.
      
       ![Hinzufügen von Quellcode](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)
   3. Fügen Sie den folgenden Code in die Datei **MyClusterApp.scala** ein. Dieser Code liest die Daten aus der Datei „HVAC.csv“ (die für alle HDInsight Spark-Cluster verfügbar ist), ruft die Zeilen ab, die nur eine Ziffer in der siebten Spalte der CSV-Datei enthalten, und schreibt die Ausgabe in **/HVACOut** unter dem Standardspeichercontainer für den Cluster.

            import org.apache.spark.SparkConf
            import org.apache.spark.SparkContext

            object MyClusterApp{
              def main (arg: Array[String]): Unit = {
                val conf = new SparkConf().setAppName("MyClusterApp")
                val sc = new SparkContext(conf)

                val rdd = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

                //find the rows which have only one digit in the 7th column in the CSV
                val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)

                rdd1.saveAsTextFile("wasbs:///HVACOut")
              }

            }

1. Führen Sie die Anwendung in einem HDInsight Spark-Cluster aus.
   
   1. Klicken Sie im **Projektexplorer** mit der rechten Maustaste auf den Projektnamen, und wählen Sie dann **Submit Spark Application to HDInsight** (Spark-Anwendung an HDInsight senden) aus.
      
       ![Übermitteln der Spark-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)
   2. Sie werden zur Eingabe der Anmeldeinformationen Ihres Azure-Abonnements aufgefordert. Geben Sie in das Dialogfeld **Spark Submission** (Spark-Übermittlung) die folgenden Werte ein.
      
      * Wählen Sie für **Spark clusters (Linux only)**(Spark-Cluster [nur Linux]) den HDInsight Spark-Cluster aus, in dem Sie Ihre Anwendung ausführen möchten.
      * Sie müssen ein Artefakt aus dem IntelliJ-Projekt oder von der Festplatte auswählen.
      * Klicken Sie neben dem Textfeld **Main class name** (Hauptklassenname) auf die Auslassungspunkte (![ellipsis](./media/hdinsight-apache-spark-intellij-tool-plugin/ellipsis.png)), wählen Sie die Hauptklasse in Ihrem Anwendungsquellcode aus, und klicken Sie dann auf **OK**.
        
          ![Übermitteln der Spark-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-3.png)
      * Da der Anwendungscode in diesem Beispiel keine Befehlszeilenargumente oder Referenzdateien (z. B. JAR) erfordert, können Sie die restlichen Textfelder leer lassen.
      * Nachdem Sie alle Eingaben vorgenommen haben, sollte das Dialogfeld wie folgt aussehen.
        
          ![Übermitteln der Spark-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-2.png)
      * Klicken Sie auf **Senden**.
   3. Auf der Registerkarte **Spark Submission** (Spark-Übermittlung) unten im Fenster sollte nun der Status angezeigt werden. Sie können die Anwendung auch anhalten, indem Sie im Fenster „Spark Submission“ auf die rote Schaltfläche klicken.
      
       ![Ergebnis der Spark-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)
      
      Im nächsten Abschnitt erfahren Sie, wie Sie mithilfe der HDInsight-Tools im Azure-Toolkit für IntelliJ auf die Auftragsausgabe zugreifen.

## <a name="access-and-manage-hdinsight-spark-clusters-using-the-hdinsight-tools-in-azure-toolkit-for-intellij"></a>Zugreifen auf und Verwalten von HDInsight Spark-Clustern mithilfe der HDInsight-Tools im Azure-Toolkit für IntelliJ
Mit den HDInsight-Tools, die zum Azure-Toolkit für IntelliJ gehören, können Sie eine Vielzahl von Vorgängen ausführen.

### <a name="access-the-job-view-directly-from-the-hdinsight-tools"></a>Zugriff auf die Auftragsansicht direkt über die HDInsight-Tools
1. Erweitern Sie im **Azure Explorer** **HDInsight**, den Namen des Spark-Clusters, und klicken Sie dann auf **Jobs** (Aufträge).
2. Im rechten Bereich werden auf der Registerkarte **Spark Job View** (Spark-Auftragsansicht) alle Anwendungen angezeigt, die in dem Cluster ausgeführt wurden. Klicken Sie auf den Namen der Anwendung, zu der Sie weitere Details sehen möchten.
   
    ![Zugreifen auf Auftragsansicht](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)
3. Die Felder für **Error Message** (Fehlermeldung), **Job Output** (Auftragsausgabe), **Livy Job Logs** (Liveauftragsprotokolle) und **Spark Driver Logs** (Spark-Treiberprotokolle) werden gemäß der Anwendung aufgefüllt, die Sie auswählen.
4. Sie können auch die **Spark History UI** (Spark-Verlaufsbenutzeroberfläche) und **YARN UI** (YARN-Benutzeroberfläche) (auf Anwendungsebene) öffnen, indem Sie auf die entsprechenden Schaltflächen am oberen Bildschirmrand klicken.

### <a name="access-the-spark-history-server"></a>Zugreifen auf den Spark-Verlaufsserver
1. Erweitern Sie im **Azure Explorer** **HDInsight**, klicken Sie mit der rechten Maustaste auf den Namen Ihres Spark-Clusters, und wählen Sie dann **Open Spark History UI** (Spark-Verlaufsbenutzeroberfläche öffnen) aus. Geben Sie die Anmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden. Diese müssen Sie beim Bereitstellen des Clusters angegeben haben.
2. Im Dashboard des Spark-Verlaufsservers können Sie die Anwendung, deren Ausführung Sie gerade beendet haben, anhand des Anwendungsnamens suchen. Im obigen Code haben Sie den Anwendungsnamen mit `val conf = new SparkConf().setAppName("MyClusterApp")`festgelegt. Daher lautete der Name Ihrer Spark-Anwendung **MyClusterApp**.

### <a name="launch-the-ambari-portal"></a>Öffnen des Ambari-Portals
Erweitern Sie im **Azure Explorer** **HDInsight**, klicken Sie mit der rechten Maustaste auf den Namen Ihres Spark-Clusters, und wählen Sie dann **Open Cluster Management Portal (Ambari)** (Clusterverwaltungsportal [Ambari] öffnen) aus. Geben Sie die Anmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden. Diese müssen Sie beim Bereitstellen des Clusters angegeben haben.

### <a name="manage-azure-subscriptions"></a>Verwalten von Azure-Abonnements
Standardmäßig führen die HDInsight-Tools die Spark-Cluster in all Ihren Azure-Abonnements auf. Bei Bedarf können Sie die Abonnements angeben, für die Sie auf den Cluster zugreifen möchten. Klicken Sie im **Azure Explorer** mit der rechten Maustaste auf den Stammknoten **Azure**, und klicken Sie auf **Abonnements verwalten**. Deaktivieren Sie im Dialogfeld die Kontrollkästchen für die Abonnements, auf die Sie nicht zugreifen möchten, und klicken Sie dann auf **Close**. Sie können auch auf **Sign Out** klicken, wenn Sie sich von Ihrem Azure-Abonnement abmelden möchten.

## <a name="run-a-spark-scala-application-locally"></a>Lokales Ausführen einer Spark Scala-Anwendung
Mithilfe der HDInsight-Tools-im Azure-Toolkit für IntelliJ können Sie Spark Scala-Anwendungen lokal auf Ihrer Arbeitsstation ausführen. In der Regel müssen solche Anwendungen nicht auf Clusterressourcen wie den Speichercontainer zugreifen und können lokal ausgeführt und getestet werden.

### <a name="prerequisite"></a>Voraussetzung
Beim lokalen Ausführen der Spark Scala-Anwendung auf einem Windows-Computer erhalten Sie ggf. eine Ausnahme wie unter [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356) beschrieben, die ausgelöst wird, weil im Windows-Betriebssystem die Datei „WinUtils.exe“ fehlt. Um diesen Fehler zu umgehen, müssen Sie [die ausführbare Datei hier herunterladen](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) und an einem Speicherort wie **C:\WinUtils\bin** speichern. Fügen Sie anschließend die Umgebungsvariable **HADOOP_HOME** hinzu, und setzen Sie den Wert der Variablen auf **C\WinUtils**.

### <a name="run-a-local-spark-scala-application"></a>Ausführen einer lokalen Spark Scala-Anwendung
1. Starten Sie IntelliJ IDEA, und erstellen Sie ein neues Projekt. Wählen Sie im Dialogfeld für das neue Projekt Folgendes aus, und klicken Sie anschließend auf **Weiter**:
   
    ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run.png)
   
   * Wählen Sie im linken Bereich **HDInsight**aus.
   * Wählen Sie im rechten Bereich **Spark on HDInsight Local Run Sample (Scala)**(Spark auf HDInsight – Beispiel für lokale Ausführung [Scala]) aus.
   * Klicken Sie auf **Weiter**.
2. Geben Sie im nächsten Fenster die Projektdetails an.
   
   * Geben Sie einen Projektnamen und den Projektspeicherort an.
   * Geben Sie für **Project SDK**(Projekt-SDK) eine Java-Version höher als 7 an.
   * Klicken Sie für **Scala SDK** auf **Erstellen** und dann auf **Herunterladen**, und wählen Sie die zu verwendende Version von Scala aus. (**Scala 2.11.x für Spark 2.0 und Scala 2.10.x für Spark 1.6**)
     
       ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-scala-version.png)
   * Für **Spark SDK**: Laden Sie das zu verwendende SDK [hier](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409) herunter. Sie können stattdessen auch das [Spark Maven Repository](http://mvnrepository.com/search?q=spark) verwenden. Stellen Sie jedoch sicher, dass die richtige Maven-Repository-Version für die Entwicklung Ihrer Spark-Anwendungen installiert ist. (Wenn Sie beispielsweise Spark Streaming verwenden, müssen Sie sicherstellen, dass Sie den Teil für Spark Streaming installiert haben. Vergewissern Sie sich auch, dass Sie für den Spark 1.6-Cluster das mit „Scala 2.10“ markierte Repository und für den Spark 2.0-Cluster das mit „Scala 2.11“ markierte Repository verwenden.)
     
       ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-create-project.png)
   * Klicken Sie auf **Fertig stellen**.
3. Die Vorlage fügt einen Beispielcode (**LogQuery**) unter dem Ordner **src** hinzu, der lokal auf Ihrem Computer ausgeführt werden kann.
   
    ![Lokale Scala-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/local-app.png)
4. Klicken Sie mit der rechten Maustaste auf die Anwendung **LogQuery**, und klicken Sie dann auf **„Run ‚LogQuery‘“** (LogQuery ausführen). Unten auf der Registerkarte **Run** (Ausführen) wird eine Ausgabe wie die folgende angezeigt.
   
   ![Ergebnis der lokalen Ausführung der Spark-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="convert-existing-intellij-idea-applications-to-use-the-hdinsight-tools-in-azure-toolkit-for-intellij"></a>Konvertieren vorhandener IntelliJ IDEA-Anwendungen für die Verwendung der HDInsight-Tools im Azure-Toolkit für IntelliJ
Sie können auch Ihre vorhandenen in IntelliJ IDEA erstellten Spark Scala-Anwendungen so umwandeln, dass sie mit den HDInsight-Tools im Azure-Toolkit für IntelliJ kompatibel sind. Dadurch können Sie das Tool verwenden, um die Anwendungen an einen HDInsight Spark-Cluster zu übermitteln. Führen Sie hierzu die folgenden Schritte aus:

1. Öffnen Sie für eine vorhandene Spark Scala-Anwendung, die mit IntelliJ IDEA erstellt wurde, die zugehörige IML-Datei.
2. Auf Stammebene sehen Sie ein **module** -Element wie das folgende:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">
3. Fügen Sie dem Element `UniqueKey="HDInsightTool"` hinzu, damit das **module** -Element wie folgt aussieht:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">
4. Speichern Sie die Änderungen. Ihre Anwendung sollte jetzt mit den HDInsight-Tools im Azure-Toolkit für IntelliJ kompatibel sein. Sie können dies testen, indem Sie im Project Explorer mit der rechten Maustaste auf den Projektnamen klicken. Im Popupmenü sollte jetzt die Option **Submit Spark Application to HDInsight**(Spark-Anwendung an HDInsight senden) enthalten sein.

## <a name="troubleshooting"></a>Problembehandlung
### <a name="please-use-a-larger-heap-size-error-in-local-run"></a>Fehler „Bitte verwenden Sie einen größeren Heap“ bei der lokalen Ausführung
Sie erhalten möglicherweise die folgenden Fehlermeldungen, wenn Sie bei der lokalen Ausführung in Spark 1.6 ein 32-Bit-Java SDK verwenden:

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

Die Fehler treten auf, weil der Heap nicht groß genug für die Ausführung von Spark ist, da Spark mindestens 471 MB benötigt (weitere Details finden Sie bei Bedarf unter [SPARK-12081](https://issues.apache.org/jira/browse/SPARK-12081) ). Eine einfache Lösung ist die Verwendung eines 64-Bit-Java SDK. Sie können auch die JVM-Einstellungen in IntelliJ ändern, indem Sie die folgenden Optionen hinzufügen:

    -Xms128m -Xmx512m -XX:MaxPermSize=300m -ea

![Ergebnis der lokalen Ausführung der Spark-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/change-heap-size.png)

## <a name="feedback--known-issues"></a>Feedback und bekannte Probleme
Zurzeit wird die direkte Anzeige von Spark-Ausgaben nicht unterstützt. Wir arbeiten daran.

Falls Sie Vorschläge oder Feedback haben oder bei Verwendung dieses Tools Probleme auftreten, senden Sie eine E-Mail an „hdivstool@microsoft.com“.

## <a name="seealso"></a>Weitere Informationen
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
* [Verwenden der HDInsight-Tools im Azure-Toolkit für IntelliJ zum Remotedebuggen von Spark-Anwendungen](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verwenden der HDInsight-Tools im Azure-Toolkit für Eclipse zum Erstellen von Spark-Anwendungen](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Verwenden von Zeppelin-Notebooks mit einem Spark-Cluster in HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)
* [Verfügbare Kernels für Jupyter-Notebook im Spark-Cluster für HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Verwenden von externen Paketen mit Jupyter Notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen
* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight(Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](hdinsight-apache-spark-job-debugging.md)


