---
title: "Azure-Toolkit für IntelliJ – Erstellen von Scala-Anwendungen für HDInsight Spark | Microsoft-Dokumentation"
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
ms.date: 05/10/2017
ms.author: nitinme
ms.translationtype: Human Translation
ms.sourcegitcommit: 44eac1ae8676912bc0eb461e7e38569432ad3393
ms.openlocfilehash: e6e005e07b35bbaffebf1efa216feebafc99df8b
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017


---
# <a name="use-azure-toolkit-for-intellij-to-create-spark-applications-for-hdinsight-cluster"></a>Erstellen von Spark-Anwendungen für HDInsight-Cluster mit dem Azure Toolkit für IntelliJ

Verwenden Sie das Plug-In „Azure Toolkit für IntelliJ“, um in Scala geschriebene Spark-Anwendungen zu entwickeln und diese direkt aus der IntelliJ-IDE an einen HDInsight Spark-Cluster zu senden. Sie können das Plug-In auf verschiedene Arten verwenden:

* Zum Entwickeln und Übermitteln einer Scala Spark-Anwendung an einen HDInsight Spark-Cluster
* Zum Zugreifen auf Ihre Azure HDInsight Spark-Clusterressourcen
* Zum Entwickeln und lokalen Ausführen einer Scala Spark-Anwendung

Informationen zu den ersten Schritten finden Sie auch in [diesem Video](https://mix.office.com/watch/1nqkqjt5xonza).

> [!IMPORTANT]
> Dieses Plug-In kann nur zum Erstellen und Übermitteln von Anwendungen an einen HDInsight Spark-Cluster unter Linux verwendet werden.
> 
> 

## <a name="prerequisites"></a>Voraussetzungen

* Einen Apache Spark-Cluster unter HDInsight (Linux). Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

* Oracle Java Development Kit. Sie können das Kit über die [Oracle-Website](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) installieren.

* IntelliJ IDEA. In diesem Artikel wird die Version 15.0.1 verwendet. Sie können die Komponente über die [JetBrains-Website](https://www.jetbrains.com/idea/download/) installieren.

## <a name="install-azure-toolkit-for-intellij"></a>Installieren des Azure-Toolkits für IntelliJ
Installationsanweisungen finden Sie unter [Installieren des Azure-Toolkits für IntelliJ](../azure-toolkit-for-intellij-installation.md).

## <a name="sign-in-to-your-azure-subscription"></a>Melden Sie sich bei Ihrem Azure-Abonnement an.
1. Starten Sie die IntelliJ-IDE, und öffnen Sie den Azure Explorer. Klicken Sie im Menü **Ansicht** auf **Toolfenster** und dann auf **Azure Explorer**.
   
    ![Ausgewählte Befehle im Menü „Ansicht“](./media/hdinsight-apache-spark-intellij-tool-plugin/show-azure-explorer.png)
2. Klicken Sie mit der rechten Maustaste auf den Knoten **Azure**, und klicken Sie dann auf **Anmelden**.
3. Klicken Sie im Dialogfeld **Azure-Anmeldung** auf **Anmelden**, und geben Sie Ihre Azure-Anmeldeinformationen ein.
   
    ![Azure-Anmeldedialogfeld](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-2.png)
4. Nachdem Sie sich angemeldet haben, werden im Dialogfeld **Abonnements auswählen** alle Azure-Abonnements aufgelistet, die den Anmeldeinformationen zugeordnet sind. Klicken Sie im Dialogfeld auf **Auswählen**, um es zu schließen.

    ![Dialogfeld zum Auswählen von Abonnements](./media/hdinsight-apache-spark-intellij-tool-plugin/Select-Subscriptions.png)
5. Erweitern Sie auf der Registerkarte **Azure Explorer** die Option **HDInsight**, um die HDInsight Spark-Cluster in Ihrem Abonnement anzuzeigen.
   
    ![HDInsight Spark-Cluster im Azure Explorer](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-3.png)
6. Sie können einen Clusternamenknoten noch einmal erweitern, um die dem Cluster zugeordneten Ressourcen (z.B. Speicherkonten) anzuzeigen.
   
    ![Erweitern eines Clusternamens zum Anzeigen der Ressourcen](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-4.png)

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>Ausführen einer Spark Scala-Anwendung in einem HDInsight Spark-Cluster
1. Starten Sie IntelliJ IDEA, und erstellen Sie ein Projekt. Wählen Sie im Dialogfeld **Neues Projekt** Folgendes aus, und klicken Sie anschließend auf **Weiter**:
   
   * Wählen Sie im linken Bereich **HDInsight** aus.
   * Wählen Sie im rechten Bereich **Spark on HDInsight (Scala)** (Spark auf HDInsight (Scala)) aus.
   
    ![Dialogfeld "Neues Projekt"](./media/hdinsight-apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)
2. Geben Sie im nächsten Fenster die folgenden Projektdetails an, und klicken Sie auf **Fertig stellen**.
   
   * Geben Sie einen Projektnamen und den Projektspeicherort an.
   * Verwenden Sie für **Projekt-SDK** die Option „Java 1.8“ für einen Spark 1.6- oder Spark 2.0-Cluster.
   * Klicken Sie für **Scala SDK** auf **Erstellen** und dann auf **Herunterladen**, und wählen Sie die zu verwendende Version von Scala aus.
     * Wählen Sie **JDK 1.8 und Scala 2.11.x** aus, wenn Sie den Auftrag an den Spark 2.0-Cluster übermitteln möchten.
     * Wählen Sie **JDK 1.8 (Sprachebene 7) und Scala 2.10.x** aus, wenn Sie den Auftrag an den Spark 1.6-Cluster übermitteln möchten.

        ![Auswählen der Scala-Version](./media/hdinsight-apache-spark-intellij-tool-plugin/show-scala2.11.x-select.png)
   * Laden Sie für **Spark-SDK** das SDK von [GitHub] (http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409) herunter, und verwenden Sie es. Beachten Sie, dass „spark-assembly-2.0.0-hadoop2.7.0-SNAPSHOT.jar“ für einen Spark 2.0-Cluster und „spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar“ für einen Spark 1.6-Cluster bestimmt ist. 

     Sie können stattdessen auch das [Spark Maven-Repository](http://mvnrepository.com/search?q=spark) verwenden. Stellen Sie hierbei aber sicher, dass die richtige Maven-Repository-Version für die Entwicklung Ihrer Spark-Anwendungen installiert ist. Wenn Sie beispielsweise Spark Streaming verwenden, müssen Sie sicherstellen, dass der Teil für Spark Streaming installiert ist. Stellen Sie außerdem sicher, dass Sie für einen Spark 1.6-Cluster das mit „Scala 2.10“ markierte Repository und für einen Spark 2.0-Cluster das mit „Scala 2.11“ markierte Repository verwenden.
     
       ![Auswählen des Spark-SDK](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-scala-project-details.png)
3. Das Spark-Projekt erstellt automatisch ein Artefakt für Sie. Gehen Sie wie folgt vor, um das Artefakt anzuzeigen:
   
   1. Klicken Sie im Menü **Datei** auf **Projektstruktur**.
   2. Klicken Sie im Dialogfeld **Projektstruktur** auf **Artefakte**, um das Standardartefakt anzuzeigen, das erstellt wird. Sie können auch ein eigenes Artefakt erstellen, indem Sie auf das Symbol **+** klicken.
      
       ![Informationen zum Artefakt im Dialogfeld](./media/hdinsight-apache-spark-intellij-tool-plugin/default-artifact.png)
      
4. Klicken Sie im Dialogfeld **Projektstruktur** auf **Projekt**. Wenn das **Projekt-SDK** auf 1.8 festgelegt ist, sollten Sie sicherstellen, dass die **Projektsprachebene** auf **7 - Diamonds, ARM, multi-catch, etc** festgelegt ist. (Dies ist für einen Spark 2.0-Cluster optional.)
   
    ![Festlegen der Projektsprachebene](./media/hdinsight-apache-spark-intellij-tool-plugin/set-project-language-level.png)
5. Fügen Sie den Quellcode der Anwendung hinzu.
   
   1. Klicken Sie im Projektexplorer mit der rechten Maustaste auf **src**, zeigen Sie auf **Neu**, und klicken Sie dann auf **Scala class** (Scala-Klasse).
      
       ![Befehle zum Erstellen einer Scala-Klasse im Projektexplorer](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)
   2. Geben Sie im Dialogfeld **Create New Scala Class** (Neue Scala-Klasse erstellen) einen Namen ein. Wählen Sie unter **Kind** (Variante) die Option **Object** (Objekt), und klicken Sie auf **OK**.
      
       ![Dialogfeld „Create New Scala Class“ (Neue Scala-Klasse erstellen)](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)
   3. Fügen Sie den folgenden Code in die Datei **MyClusterApp.scala** ein. Dieser Code liest die Daten aus der Datei „HVAC.csv“ (die für alle HDInsight Spark-Cluster verfügbar ist), ruft die Zeilen ab, die nur eine Ziffer in der siebten Spalte der CSV-Datei enthalten, und schreibt die Ausgabe in **/HVACOut** unter dem Standardspeichercontainer für den Cluster.

            import org.apache.spark.SparkConf
            import org.apache.spark.SparkContext

            object MyClusterApp{
              def main (arg: Array[String]): Unit = {
                val conf = new SparkConf().setAppName("MyClusterApp")
                val sc = new SparkContext(conf)

                val rdd = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

                //find the rows that have only one digit in the seventh column in the CSV file
                val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)

                rdd1.saveAsTextFile("wasbs:///HVACOut")
              }

            }

6. Führen Sie die Anwendung in einem HDInsight Spark-Cluster aus.
   
   1. Klicken Sie im Projektexplorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie dann **Submit Spark Application to HDInsight** (Spark-Anwendung an HDInsight senden).
      
       ![Auswählen von „Submit Spark Application to HDInsight“ (Spark-Anwendung an HDInsight senden)](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)
   2. Sie werden zur Eingabe der Anmeldeinformationen Ihres Azure-Abonnements aufgefordert. Geben Sie in das Dialogfeld **Spark Submission** (Spark-Übermittlung) die folgenden Werte ein, und klicken Sie dann auf **Submit** (Übermitteln).
      
      * Wählen Sie für **Spark clusters (Linux only)**(Spark-Cluster [nur Linux]) den HDInsight Spark-Cluster aus, in dem Sie Ihre Anwendung ausführen möchten.
      * Wählen Sie ein Artefakt aus dem IntelliJ-Projekt oder von der Festplatte aus.
      * Klicken Sie im Feld **Main class name** (Hauptklassenname) auf die Auslassungspunkte (![Auslassungspunkte](./media/hdinsight-apache-spark-intellij-tool-plugin/ellipsis.png)), wählen Sie die Hauptklasse in Ihrem Anwendungsquellcode aus, und klicken Sie dann auf **OK**.
        
          ![Dialogfeld zum Auswählen der Hauptklasse](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-3.png)
      * Da für den Anwendungscode in diesem Beispiel keine Befehlszeilenargumente oder Referenzdateien (z.B. JAR) erforderlich sind, können Sie die restlichen Felder leer lassen.
        Nachdem Sie alle Eingaben vorgenommen haben, sollte das Dialogfeld in etwa wie in der folgenden Abbildung aussehen:
        
          ![Dialogfeld für die Spark-Übermittlung](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-2.png)
   3. Auf der Registerkarte **Spark Submission** (Spark-Übermittlung) unten im Fenster sollte nun der Status angezeigt werden. Sie können die Anwendung auch anhalten, indem Sie im Fenster **Spark Submission** (Spark-Übermittlung) auf die rote Schaltfläche klicken.
      
       ![Fenster für die Spark-Übermittlung](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)
      
      Weiter unten in diesem Artikel im Abschnitt „Zugreifen auf und Verwalten von HDInsight Spark-Clustern mit dem Azure-Toolkit für IntelliJ“ wird beschrieben, wie Sie auf die Auftragsausgabe zugreifen.

## <a name="choose-azure-data-lake-store-as-spark-scala-application-storage"></a>Auswählen von Azure Data Lake Store als Spark Scala-Anwendungsspeicher
Wenn Sie die Anwendung an Azure Data Lake Store übermitteln möchten, müssen Sie während des Azure-Anmeldeprozesses den Modus **Interaktiv** auswählen. 

   ![Option „Interaktiv“ bei der Anmeldung](./media/hdinsight-apache-spark-intellij-tool-plugin/authentication-interactive.png)

Bei Auswahl von **Automatisiert** erhalten Sie den folgenden Fehler:

   ![Anmeldefehler](./media/hdinsight-apache-spark-intellij-tool-plugin/authentication-error.png)

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-azure-toolkit-for-intellij"></a>Zugreifen auf und Verwalten von HDInsight Spark-Clustern mit dem Azure-Toolkit für IntelliJ
Sie können mit dem Azure-Toolkit für IntelliJ verschiedene Vorgänge durchführen.

### <a name="access-the-job-view"></a>Zugreifen auf die Auftragsansicht
1. Erweitern Sie im Azure Explorer die Option **HDInsight** und den Namen des Spark-Clusters, und klicken Sie dann auf **Jobs** (Aufträge).
2. Im rechten Bereich werden auf der Registerkarte **Spark Job View** (Spark-Auftragsansicht) alle Anwendungen angezeigt, die in dem Cluster ausgeführt wurden. Klicken Sie auf den Namen der Anwendung, zu der Sie weitere Details anzeigen möchten.
   
    ![Anwendungsdetails](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)

Die Felder für **Fehler**, **Treiberprotokoll**, **Ausgabe** und **Livy Log** (Livy-Protokoll) werden basierend auf der von Ihnen ausgewählten Anwendung mit Werten gefüllt.

Sie können auch die Spark History UI (Spark-Verlaufsbenutzeroberfläche) und YARN UI (YARN-Benutzeroberfläche) (auf Anwendungsebene) öffnen, indem Sie auf die entsprechenden Schaltflächen am oberen Rand des Fensters klicken.

### <a name="access-the-spark-history-server"></a>Zugreifen auf den Spark-Verlaufsserver
1. Erweitern Sie im Azure Explorer die Option **HDInsight**, klicken Sie mit der rechten Maustaste auf den Namen Ihres Spark-Clusters, und wählen Sie dann **Open Spark History UI** (Spark-Verlaufsbenutzeroberfläche öffnen). Geben Sie die Anmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden. Diese müssen Sie beim Bereitstellen des Clusters angegeben haben.
2. Im Dashboard des Spark-Verlaufsservers können Sie anhand des Anwendungsnamens nach der Anwendung suchen, deren Ausführung Sie gerade beendet haben. Im obigen Code legen Sie den Namen der Anwendung mit `val conf = new SparkConf().setAppName("MyClusterApp")` fest. Daher lautete der Name Ihrer Spark-Anwendung **MyClusterApp**.

### <a name="start-the-ambari-portal"></a>Starten des Ambari-Portals
1. Erweitern Sie im Azure Explorer die Option **HDInsight**, klicken Sie mit der rechten Maustaste auf den Namen Ihres Spark-Clusters, und wählen Sie dann **Open Cluster Management Portal (Ambari)** (Clusterverwaltungsportal (Ambari) öffnen). 
2. Geben Sie die Anmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden. Sie haben diese Anmeldeinformationen während der Bereitstellung des Clusters angegeben.

### <a name="manage-azure-subscriptions"></a>Verwalten von Azure-Abonnements
Standardmäßig werden mit dem Azure-Toolkit für IntelliJ die Spark-Cluster Ihrer gesamten Azure-Abonnements aufgelistet. Bei Bedarf können Sie die Abonnements angeben, für die Sie auf den Cluster zugreifen möchten. 

1. Klicken Sie im Azure Explorer mit der rechten Maustaste auf den Stammknoten **Azure**, und klicken Sie dann auf **Abonnements verwalten**. 
2. Deaktivieren Sie im Dialogfeld die Kontrollkästchen für die Abonnements, auf die Sie nicht zugreifen möchten, und klicken Sie dann auf **Schließen**. Sie können auch auf **Abmelden** klicken, wenn Sie sich von Ihrem Azure-Abonnement abmelden möchten.

## <a name="run-a-spark-scala-application-locally"></a>Lokales Ausführen einer Spark Scala-Anwendung
Mit dem Azure-Toolkit für IntelliJ können Sie Spark Scala-Anwendungen lokal auf Ihrer Arbeitsstation ausführen. In der Regel müssen solche Anwendungen nicht auf Clusterressourcen wie den Speichercontainer zugreifen und können lokal ausgeführt und getestet werden.

### <a name="prerequisite"></a>Voraussetzung
Beim Ausführen der lokalen Spark Scala-Anwendung auf einem Windows-Computer wird unter Umständen eine Ausnahme wie unter [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356) beschrieben ausgelöst. Diese Ausnahme tritt auf, weil die Datei „WinUtils.exe“ in Windows fehlt. 

Um diesen Fehler zu beheben, müssen Sie [die ausführbare Datei herunterladen](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) und an einem Speicherort wie **C:\WinUtils\bin** speichern. Fügen Sie anschließend die Umgebungsvariable **HADOOP_HOME** hinzu, und legen Sie den Wert der Variablen auf **C\WinUtils** fest.

### <a name="run-a-local-spark-scala-application"></a>Ausführen einer lokalen Spark Scala-Anwendung
1. Starten Sie IntelliJ IDEA, und erstellen Sie ein Projekt. Wählen Sie im Dialogfeld **Neues Projekt** Folgendes aus, und klicken Sie anschließend auf **Weiter**:
   
   * Wählen Sie im linken Bereich **HDInsight** aus.
   * Wählen Sie im rechten Bereich **Spark on HDInsight Local Run Sample (Scala)** (Spark auf HDInsight – Beispiel für lokale Ausführung (Scala)) aus.
   
    ![Auswahl im Dialogfeld „Neues Projekt“](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run.png)
2. Geben Sie im nächsten Fenster die folgenden Projektdetails an, und klicken Sie auf **Fertig stellen**.
   
   * Geben Sie einen Projektnamen und den Projektspeicherort an.
   * Geben Sie für **Project SDK** (Projekt-SDK) eine Java-Version höher als 7 an.
   * Klicken Sie für **Scala SDK** auf **Erstellen** und dann auf **Herunterladen**, und wählen Sie die zu verwendende Version von Scala aus: Scala 2.11.x für Spark 2.0 und Scala 2.10.x für Spark 1.6.
     
       ![Auswählen der Scala-Version](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-scala-version.png)
   * Für **Spark SDK**: Laden Sie das zu verwendende SDK von [GitHub](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409) herunter. Sie können stattdessen auch das [Spark Maven-Repository](http://mvnrepository.com/search?q=spark) verwenden. Stellen Sie hierbei aber sicher, dass die richtige Maven-Repository-Version für die Entwicklung Ihrer Spark-Anwendungen installiert ist. Wenn Sie beispielsweise Spark Streaming verwenden, müssen Sie sicherstellen, dass der Teil für Spark Streaming installiert ist. Stellen Sie außerdem sicher, dass Sie für einen Spark 1.6-Cluster das mit „Scala 2.10“ gekennzeichnete Repository und für einen Spark 2.0-Cluster das mit „Scala 2.11“ gekennzeichnete Repository verwenden.
     
       ![Auswählen des Spark-SDK](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-create-project.png)
3. Die Vorlage fügt einen Beispielcode (**LogQuery**) unter dem Ordner **src** hinzu, der lokal auf Ihrem Computer ausgeführt werden kann.
   
    ![Speicherort von LogQuery](./media/hdinsight-apache-spark-intellij-tool-plugin/local-app.png)
4. Klicken Sie mit der rechten Maustaste auf die Anwendung **LogQuery**, und klicken Sie dann auf **Run „LogQuery“** („LogQuery“ ausführen). Unten auf der Registerkarte **Run** (Ausführen) wird eine Ausgabe angezeigt, die in etwa wie folgt aussieht:
   
   ![Ergebnis der lokalen Ausführung der Spark-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="convert-existing-intellij-idea-applications-to-use-azure-toolkit-for-intellij"></a>Konvertieren vorhandener IntelliJ IDEA-Anwendungen für die Verwendung des Azure-Toolkits für IntelliJ
Sie können Ihre vorhandenen Spark Scala-Anwendungen, die in IntelliJ IDEA erstellt wurden, so umwandeln, dass sie mit dem Azure-Toolkit für IntelliJ kompatibel sind. Anschließend können Sie das Plug-In verwenden, um die Anwendungen an einen HDInsight Spark-Cluster zu übermitteln.

1. Öffnen Sie für eine vorhandene Spark Scala-Anwendung, die mit IntelliJ IDEA erstellt wurde, die zugehörige IML-Datei.
2. Auf der Stammebene wird ein **module**-Element wie das folgende angezeigt:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">
   Fügen Sie dem Element `UniqueKey="HDInsightTool"` hinzu, damit das **module** -Element wie folgt aussieht:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">
3. Speichern Sie die Änderungen. Ihre Anwendung sollte jetzt mit dem Azure-Toolkit für IntelliJ kompatibel sein. Sie können dies testen, indem Sie im Projektexplorer mit der rechten Maustaste auf den Projektnamen klicken. Im Popupmenü ist jetzt die Option **Submit Spark Application to HDInsight** (Spark-Anwendung an HDInsight übermitteln) verfügbar.

## <a name="troubleshooting"></a>Problembehandlung
### <a name="please-use-a-larger-heap-size-error-in-local-run"></a>Fehler „Bitte verwenden Sie einen größeren Heap“ bei der lokalen Ausführung
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

Diese Fehler treten auf, weil die Heapgröße für die Ausführung von Spark nicht groß genug ist. (Für Spark sind mindestens 471 MB erforderlich. Weitere Details finden Sie unter [SPARK-12081](https://issues.apache.org/jira/browse/SPARK-12081).) Eine einfache Lösung ist die Verwendung eines 64-Bit-Java SDK. Sie können auch die JVM-Einstellungen in IntelliJ ändern, indem Sie die folgenden Optionen hinzufügen:

    -Xms128m -Xmx512m -XX:MaxPermSize=300m -ea

![Hinzufügen von Optionen zum Feld „VM options“ (VM-Optionen) in IntelliJ](./media/hdinsight-apache-spark-intellij-tool-plugin/change-heap-size.png)

## <a name="feedback-and-known-issues"></a>Feedback und bekannte Probleme
Derzeit wird die direkte Anzeige von Spark-Ausgaben nicht unterstützt.

Falls Sie Vorschläge oder Feedback haben oder bei Verwendung dieses Plug-Ins Probleme auftreten, können Sie eine E-Mail an hdivstool@microsoft.com senden.

## <a name="seealso"></a>Weitere Informationen
* [Übersicht: Apache Spark in Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Szenarios
* [Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](hdinsight-apache-spark-use-bi-tools.md)
* [Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark-Streaming: Erstellen von Echtzeitstreaminganwendungen mithilfe von Spark in HDInsight](hdinsight-apache-spark-eventhub-streaming.md)
* [Websiteprotokollanalyse mithilfe von Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Erstellen und Ausführen von Anwendungen
* [Erstellen einer eigenständigen Anwendung mit Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Remoteausführung von Aufträgen in einem Spark-Cluster mithilfe von Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools und Erweiterungen
* [Verwenden des Azure-Toolkits für IntelliJ zum Remotedebuggen von Spark-Anwendungen](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verwenden der HDInsight-Tools im Azure-Toolkit für Eclipse zum Erstellen von Spark-Anwendungen](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Verwenden von Zeppelin-Notebooks mit einem Spark-Cluster in HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)
* [Verfügbare Kernels für Jupyter-Notebook im Spark-Cluster für HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Verwenden von externen Paketen mit Jupyter Notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Verwalten von Ressourcen
* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight(Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](hdinsight-apache-spark-job-debugging.md)


