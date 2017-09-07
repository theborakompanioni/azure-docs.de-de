---
title: "Azure-Toolkit für Eclipse: Erstellen von Scala-Anwendungen für HDInsight Spark | Microsoft-Dokumentation"
description: "Verwenden Sie die HDInsight Tools im Azure-Toolkit für Eclipse, um in Scala geschriebene Spark-Anwendungen zu entwickeln und direkt in der integrierten Eclipse-Entwicklungsumgebung (IDE) an einen HDInsight Spark-Cluster zu übermitteln."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: f6c79550-5803-4e13-b541-e86c4abb420b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: nitinme
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: 79b3183171e3c28276c8e4e6d4fe3998e0109643
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="use-azure-toolkit-for-eclipse-to-create-spark-applications-for-an-hdinsight-cluster"></a>Erstellen von Spark-Anwendungen für HDInsight-Cluster mit dem Azure-Toolkit für Eclipse

Verwenden Sie die HDInsight-Tools im Azure-Toolkit für Eclipse, um in Scala geschriebene Spark-Anwendungen zu entwickeln und diese direkt aus der Eclipse-IDE an einen Azure HDInsight Spark-Cluster zu senden. Sie können das HDInsight-Tools-Plug-In auf verschiedene Weise verwenden:

* Zum Entwickeln und Übermitteln einer Scala Spark-Anwendung an einen HDInsight Spark-Cluster
* Zum Zugreifen auf Ihre Azure HDInsight Spark-Clusterressourcen
* Zum Entwickeln und lokalen Ausführen einer Scala Spark-Anwendung

> [!IMPORTANT]
> Sie können dieses Tool nur zum Erstellen und Übermitteln von Anwendungen für einen HDInsight Spark-Cluster unter Linux verwenden.
> 
> 

## <a name="prerequisites"></a>Voraussetzungen

* Apache Spark-Cluster in HDInsight. Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
* Oracle Java Development Kit, Version 8, das für die Runtime der Eclipse-IDE verwendet wird. Sie können es von der [Oracle-Website](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) herunterladen.
* Eclipse-IDE. Dieser Artikel verwendet Eclipse Neon. Sie können es von der [Eclipse-Website](https://www.eclipse.org/downloads/) aus installieren.
* Spark-SDK. Sie können es von [GitHub](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409) herunterladen.


## <a name="install-hdinsight-tools-in-azure-toolkit-for-eclipse-and-the-scala-plug-in"></a>Installieren der HDInsight Tools im Azure-Toolkit für Eclipse und des Scala-Plug-Ins
### <a name="install-hdinsight-tools"></a>Installieren der HDInsight Tools
Die HDInsight-Tools für Eclipse sind als Teil des Azure-Toolkits für Eclipse verfügbar. Installationsanweisungen finden Sie unter [Installieren des Azure-Toolkits für Eclipse](../azure-toolkit-for-eclipse-installation.md).
### <a name="install-the-scala-plug-in"></a>Installieren des Scala-Plug-Ins
Wenn Sie Eclipse öffnen, erkennen die HDInsight Tools automatisch, ob das Scala-Plug-In installiert ist. Klicken Sie auf **OK** um fortzufahren, und folgen Sie dann den Anweisungen zum Installieren des Plug-Ins vom Eclipse-Marketplace.

![Automatische Installation des Scala-Plug-Ins](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala.png)

## <a name="sign-in-to-your-azure-subscription"></a>Melden Sie sich bei Ihrem Azure-Abonnement an.
1. Starten Sie die Eclipse-IDE, und öffnen Sie den Azure Explorer. Wählen Sie im Menü **Window** (Fenster) die Option **Show View** (Ansicht anzeigen) und dann **Other** (Sonstiges) aus. Erweitern Sie im daraufhin angezeigten Dialogfeld den Eintrag **Azure**, klicken Sie auf **Azure Explorer** und dann auf **OK**.

   ![Dialogfeld „Show View“ (Ansicht anzeigen)](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-1.png)
2. Klicken Sie mit der rechten Maustaste auf den Knoten **Azure**, und wählen Sie dann **Sign in** (Anmelden) aus.
3. Wählen Sie im Dialogfeld **Azure Sign In** (Azure-Anmeldung) die Authentifizierungsmethode aus, klicken Sie auf **Sign in** (Anmelden), und geben Sie Ihre Azure-Anmeldeinformationen ein.
   
   ![Azure-Anmeldedialogfeld](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-2.png)
4. Nachdem Sie sich angemeldet haben, werden im Dialogfeld **Abonnements auswählen** alle Azure-Abonnements aufgelistet, die den Anmeldeinformationen zugeordnet sind. Klicken Sie im Dialogfeld auf **Auswählen**, um es zu schließen.

   ![Dialogfeld zum Auswählen von Abonnements](./media/hdinsight-apache-spark-eclipse-tool-plugin/Select-Subscriptions.png)
5. Erweitern Sie auf der Registerkarte **Azure Explorer** die Option **HDInsight**, um die HDInsight Spark-Cluster in Ihrem Abonnement anzuzeigen.
   
   ![HDInsight Spark-Cluster im Azure Explorer](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-3.png)
6. Sie können einen Clusternamenknoten noch einmal erweitern, um die dem Cluster zugeordneten Ressourcen (z.B. Speicherkonten) anzuzeigen.
   
   ![Erweitern eines Clusternamens zum Anzeigen der Ressourcen](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-4.png)



## <a name="set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster"></a>Einrichten eines Spark Scala-Projekts für einen HDInsight Spark-Cluster

1. Wählen Sie im Arbeitsbereich der Eclipse-IDE **File** (Datei), **New** (Neu) und dann **Project** (Projekt) aus. 
2. Erweitern Sie im Assistenten zum Erstellen eines neuen Projekts die Option **HDInsight**, wählen Sie **Spark on HDInsight (Scala)** (Spark für HDInsight [Scala]) aus, und klicken Sie dann auf **Next** (Weiter).

   ![Auswählen des Projekts „Spark für HDInsight (Scala)“](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-2.png)
3. Der Scala-Projekterstellungs-Assistent erkennt automatisch, ob das Scala-Plug-In installiert ist. Klicken Sie auf **OK**, um den Download des Scala-Plug-Ins fortzusetzen, und folgen Sie dann den Anweisungen, um Eclipse neu zu starten.

   ![Scala-Überprüfung](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala-2.png)
4. Geben Sie im Dialogfeld **New HDInsight Scala Project** (Neues HDInsight Scala-Projekt) die folgenden Werte an, und klicken Sie dann auf **Next** (Weiter):
   * Geben Sie einen Namen für das Projekt ein.
   * Achten Sie darauf, dass im Bereich **JRE** für **Ausführungsumgebungs-JRE verwenden** die Option **JavaSE-1.7** oder höher festgelegt ist.
   * Stellen Sie sicher, dass für das Spark-SDK der Speicherort festgelegt ist, an den Sie das SDK heruntergeladen haben. Der Link zum Downloadspeicherort wird in [Voraussetzungen](#prerequisites) weiter oben in diesem Artikel aufgeführt. Sie können das SDK auch über den Link in diesem Dialogfeld herunterladen.

   ![Dialogfeld für ein neues HDInsight Scala-Projekt](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-3.png)
5. Klicken Sie im nächsten Dialogfeld auf die Registerkarte **Libraries** (Bibliotheken), übernehmen Sie die Standardeinstellungen, und klicken Sie dann auf **Finish** (Fertig stellen). 
   
   ![Registerkarte „Bibliotheken“](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-4.png)
  
## <a name="create-a-scala-application-for-an-hdinsight-spark-cluster"></a>Erstellen einer Scala-Anwendung für einen HDInsight Spark-Cluster

1. Erweitern Sie in der Eclipse-IDE im Paket-Explorer das Projekt, das Sie zuvor erstellt haben, klicken Sie mit der rechten Maustaste auf **src**, zeigen Sie auf **New** (Neu) und dann auf **Other** (Sonstiges).
2. Erweitern Sie im Dialogfeld **Select a wizard** (Assistenten auswählen) die Option **Scala Wizards** (Scala-Assistenten), wählen Sie **Scala Object** (Scala-Objekt) aus, und klicken Sie dann auf **Next** (Weiter).
   
   ![Dialogfeld zum Auswählen eines Assistenten](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-1.png)
3. Geben Sie im Dialogfeld **Create New File** (Neue Datei erstellen) einen Namen für das Objekt ein, und klicken Sie dann auf **Finish** (Fertig stellen).
   
   ![Dialogfeld zum Erstellen einer neuen Datei](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-2.png)
4. Fügen Sie folgenden Code in den Text-Editor ein:
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        object MyClusterApp{
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find the rows that have only one digit in the seventh column in the CSV
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACOut")
          }        
        }
5. Führen Sie die Anwendung in einem HDInsight Spark-Cluster aus:
   
   a. Klicken Sie im Paket-Explorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie dann **Submit Spark Application to HDInsight** (Spark-Anwendung an HDInsight senden) aus.        
   b. Geben Sie im Dialogfeld **Spark Submission** (Spark-Übermittlung) die folgenden Werte ein, und wählen Sie dann **Submit** (Übermitteln) aus:
      
      * Wählen Sie für **Cluster Name**den HDInsight Spark-Cluster aus, auf dem Sie Ihre Anwendung ausführen möchten.
      * Wählen Sie ein Artefakt aus dem Eclipse-Projekt oder von der Festplatte aus. Der Standardwert hängt von dem Element ab, auf das Sie im Paket-Explorer mit der rechten Maustaste klicken.
      * In der Dropdownliste **Main class name** (Hauptklassenname) zeigt der Übermittlungs-Assistent alle Objektnamen aus Ihrem Projekt an. Wählen Sie ein Objekt aus, oder geben Sie den Namen des Objekts ein, das Sie ausführen möchten. Wenn Sie ein Artefakt von einer Festplatte ausgewählt haben, müssen Sie den Namen der Hauptklasse manuell eingeben. 
      * Weil der Anwendungscode in diesem Beispiel keine Befehlszeilenargumente, Referenz-JARs oder Referenzdateien erfordert, können Sie die restlichen Textfelder leer lassen.
        
      ![Dialogfeld für die Spark-Übermittlung](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-3.png)
6. Auf der Registerkarte **Spark Submission** sollte nun der Fortschritt angezeigt werden. Sie können die Anwendung anhalten, indem Sie im Fenster **Spark Submission** (Spark-Übermittlung) auf die rote Schaltfläche klicken. Sie können auch die Protokolle für die Anwendungsausführung anzeigen, indem Sie auf das Globussymbol klicken (in der Abbildung gekennzeichnet durch das blaue Feld).
      
   ![Fenster für die Spark-Übermittlung](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-4.png)

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-hdinsight-tools-in-azure-toolkit-for-eclipse"></a>Zugreifen auf und Verwalten von HDInsight Spark-Clustern mithilfe der HDInsight-Tools im Azure-Toolkit für Eclipse
Sie können mithilfe der HDInsight-Tools verschiedene Vorgänge durchführen, z.B. auf die Auftragsausgabe zugreifen.

### <a name="access-the-job-view"></a>Zugreifen auf die Auftragsansicht
1. Erweitern Sie im Azure Explorer die Option **HDInsight** und den Namen des Spark-Clusters, und wählen Sie dann **Jobs** (Aufträge) aus. 

   ![Knoten „Auftragsansicht“](./media/hdinsight-apache-spark-intellij-tool-plugin/job-view-node.png)

2. Wählen Sie den Knoten **Jobs** (Aufträge) aus. Die HDInsight Tools erkennen automatisch, ob das E(fx)clipse-Plug-In installiert ist. Klicken Sie auf **OK**, um fortzufahren, folgen Sie dann den Anweisungen zum Installieren des Eclipse-Marketplace, und starten Sie Eclipse neu.

   ![Installieren von E(fx)clipse](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-efxclipse.png)

3. Öffnen Sie in der Auftragsansicht den Knoten **Jobs** (Aufträge). Im rechten Bereich werden auf der Registerkarte **Spark Job View** (Spark-Auftragsansicht) alle Anwendungen angezeigt, die in dem Cluster ausgeführt wurden. Wählen Sie den Namen der Anwendung aus, zu der Sie weitere Details anzeigen möchten.

   ![Anwendungsdetails](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)

   Sie können dann eine der folgenden Aktionen ausführen:

   * Zeigen Sie auf den Auftragsgraphen. Dadurch werden grundlegende Informationen zum ausgeführten Auftrag angezeigt. Wenn Sie den Auftragsgraphen auswählen, werden die Phasen und die von den einzelnen Aufträgen generierten Informationen angezeigt.

     ![Details zur Auftragsphase](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

   * Klicken Sie auf die Registerkarte **Log** (Protokoll), um häufig verwendete Protokolle anzuzeigen, z. B. **Driver Stderr**, **Driver Stdout** und **Directory Info**.

     ![Protokolldetails](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-log-info.png)

   * Öffnen Sie die Spark-Verlaufsbenutzeroberfläche und die YARN-Benutzeroberfläche (auf Anwendungsebene), indem Sie auf die Links im oberen Bereich des Fensters klicken.

### <a name="access-the-storage-container-for-the-cluster"></a>Zugreifen auf den Speichercontainer des Clusters
1. Erweitern Sie im Azure Explorer den Stammknoten **HDInsight**, um eine Liste der verfügbaren HDInsight Spark-Cluster anzuzeigen.
2. Erweitern Sie den Namen des Clusters, um das Speicherkonto und den Standardspeichercontainer des Clusters anzuzeigen.
   
   ![Standardspeicherkonto und -container](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-5.png)
3. Wählen Sie den Namen des Speichercontainers aus, der dem Cluster zugeordnet ist. Doppelklicken Sie im rechten Bereich auf den Ordner **HVACOut**. Öffnen Sie eine der **part**-Dateien, um die Ausgabe der Anwendung anzuzeigen.

### <a name="access-the-spark-history-server"></a>Zugreifen auf den Spark-Verlaufsserver
1. Klicken Sie im Azure Explorer mit der rechten Maustaste auf den Namen Ihres Spark-Clusters, und wählen Sie dann **Open Spark History UI** (Spark-Verlaufsbenutzeroberfläche öffnen) aus. Geben Sie die Anmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden. Diese Anmeldeinformationen haben Sie bei der Bereitstellung des Clusters angegeben.
2. Im Dashboard des Spark-Verlaufsservers können Sie die Anwendung, deren Ausführung Sie gerade beendet haben, anhand des Anwendungsnamens suchen. Im obigen Code legen Sie den Namen der Anwendung mit `val conf = new SparkConf().setAppName("MyClusterApp")` fest. Daher lautete der Name Ihrer Spark-Anwendung **MyClusterApp**.

### <a name="start-the-ambari-portal"></a>Starten des Ambari-Portals
1. Klicken Sie im Azure Explorer mit der rechten Maustaste auf den Namen Ihres Spark-Clusters, und wählen Sie dann **Open Cluster Management Portal (Ambari)** (Clusterverwaltungsportal [Ambari] öffnen) aus. 
2. Geben Sie die Anmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden. Diese Anmeldeinformationen haben Sie bei der Bereitstellung des Clusters angegeben.

### <a name="manage-azure-subscriptions"></a>Verwalten von Azure-Abonnements
Standardmäßig führen die HDInsight-Tools im Azure-Toolkit für Eclipse die Spark-Cluster in all Ihren Azure-Abonnements auf. Bei Bedarf können Sie die Abonnements angeben, für die Sie auf den Cluster zugreifen möchten. 

1. Klicken Sie im Azure Explorer mit der rechten Maustaste auf den Hauptknoten **Azure**, und wählen Sie dann **Abonnements verwalten** aus. 
2. Deaktivieren Sie im Dialogfeld die Kontrollkästchen für die Abonnements, auf die Sie nicht zugreifen möchten, und klicken Sie dann auf **Close** (Schließen). Sie können auch **Abmelden** auswählen, wenn Sie sich von Ihrem Azure-Abonnement abmelden möchten.

## <a name="run-a-spark-scala-application-locally"></a>Lokales Ausführen einer Spark Scala-Anwendung
Mithilfe der HDInsight-Tools-im Azure-Toolkit für Eclipse können Sie Spark Scala-Anwendungen lokal auf Ihrer Arbeitsstation ausführen. In der Regel müssen solche Anwendungen nicht auf Clusterressourcen wie den Speichercontainer zugreifen und können lokal ausgeführt und getestet werden.

### <a name="prerequisite"></a>Voraussetzung
Beim Ausführen der lokalen Spark Scala-Anwendung auf einem Windows-Computer wird möglicherweise eine Ausnahme wie unter [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356) beschrieben ausgelöst. Diese Ausnahme tritt auf, weil **WinUtils.exe** in Windows fehlt. 

Um diesen Fehler zu beheben, müssen Sie [die ausführbare Datei herunterladen](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) und an einem Speicherort wie **C:\WinUtils\bin** speichern. Fügen Sie anschließend die Umgebungsvariable **HADOOP_HOME** hinzu, und legen Sie den Wert der Variablen auf **C\WinUtils** fest.

### <a name="run-a-local-spark-scala-application"></a>Ausführen einer lokalen Spark Scala-Anwendung
1. Starten Sie Eclipse, und erstellen Sie ein Projekt. Nehmen Sie im Dialogfeld **New Project** (Neues Projekt) die folgenden Einstellungen vor, und klicken Sie anschließend auf **Next** (Weiter).
   
   * Wählen Sie im linken Bereich **HDInsight** aus.
   * Wählen Sie im rechten Bereich **Spark on HDInsight Local Run Sample (Scala)** (Spark auf HDInsight – Beispiel für lokale Ausführung [Scala]) aus.

   ![Dialogfeld "Neues Projekt"](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run.png)
   
2. Um die Projektdetails bereitzustellen, führen Sie die Schritte 3 bis 6 wie oben im Abschnitt [Einrichten eines Spark Scala-Projekts für einen HDInsight Spark-Cluster](#set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster) gezeigt aus.

3. Die Vorlage fügt einen Beispielcode (**LogQuery**) unter dem Ordner **src** hinzu, der lokal auf Ihrem Computer ausgeführt werden kann.
   
   ![Speicherort von LogQuery](./media/hdinsight-apache-spark-eclipse-tool-plugin/local-app.png)
   
4. Klicken Sie mit der rechten Maustaste auf die Anwendung **LogQuery**, zeigen Sie auf **Run As** (Ausführen als), und klicken Sie dann auf **1 Scala Application** (1 Scala-Anwendung). Auf der Registerkarte **Console** (Konsole) wird eine Ausgabe der folgenden Art angezeigt:
   
   ![Ergebnis der lokalen Ausführung der Spark-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="known-problems"></a>Bekannte Probleme
Um eine Anwendung an Azure Data Lake Store zu übermitteln, wählen Sie während des Azure-Anmeldeprozesses den Modus **Interactive** (Interaktiv) aus. Bei Auswahl von **Automated** (Automatisiert) tritt unter Umständen ein Fehler auf.

![Interaktive Anmeldung](./media/hdinsight-apache-spark-eclipse-tool-plugin/interactive-authentication.png)

Sie können einen Azure Data Lake-Cluster auswählen, um Ihre Anwendung mit einer beliebigen Anmeldemethode zu übermitteln.

Derzeit wird die direkte Anzeige von Spark-Ausgaben nicht unterstützt.

## <a name="feedback"></a>Feedback
Falls Sie uns Feedback geben möchten oder bei der Verwendung des Tools andere Probleme auftreten, können Sie uns eine E-Mail an hdivstool@microsoft.com senden.

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
* [Verwenden des Azure-Toolkits für IntelliJ zum Erstellen und Übermitteln von Spark Scala-Anwendungen](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Verwenden des Azure-Toolkits für IntelliJ zum Remotedebuggen von Spark-Anwendungen über VPN](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verwenden des Azure-Toolkits für IntelliJ zum Remotedebuggen von Spark-Anwendungen per SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Verwenden der HDInsight-Tools für IntelliJ mit Hortonworks Sandbox](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Verwenden von Zeppelin-Notebooks mit einem Spark-Cluster in HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Verfügbare Kernels für Jupyter-Notebook im Spark-Cluster für HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Verwenden von externen Paketen mit Jupyter Notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Verwalten von Ressourcen
* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight(Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](hdinsight-apache-spark-job-debugging.md)


