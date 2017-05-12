---
title: "Erstellen von Scala-Anwendungen für Spark mithilfe des Azure-Toolkits für Eclipse | Microsoft-Dokumentation"
description: "Verwenden Sie die HDInsight-Tools im Azure Toolkit für Eclipse, um in Scala geschriebene Spark-Anwendungen zu entwickeln und diese direkt aus der Eclipse-IDE an einen HDInsight Spark-Cluster zu senden."
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
ms.date: 05/10/2017
ms.author: nitinme
ms.translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 51cef9d1fc41d8ece2c5ec82df5f6a889d4c57dc
ms.contentlocale: de-de
ms.lasthandoff: 04/27/2017


---
# <a name="use-azure-toolkit-for-eclipse-to-create-spark-applications-for-hdinsight-cluster"></a>Erstellen von Spark-Anwendungen für HDInsight-Cluster mit dem Azure Toolkit für Eclipse

Verwenden Sie die HDInsight-Tools im Azure Toolkit für Eclipse, um in Scala geschriebene Spark-Anwendungen zu entwickeln und diese direkt aus der Eclipse-IDE an einen HDInsight Spark-Cluster zu senden. Sie verfügen über verschiedene Möglichkeiten, das HDInsight-Tools-Plug-In zu verwenden:

* Zum Entwickeln und Übermitteln einer Scala Spark-Anwendung an einen HDInsight Spark-Cluster
* Zum Zugreifen auf Ihre Azure HDInsight Spark-Clusterressourcen
* Zum Entwickeln und lokalen Ausführen einer Scala Spark-Anwendung

> [!IMPORTANT]
> Dieses Tool kann nur zum Erstellen und Übermitteln von Anwendungen an einen HDInsight Spark-Cluster unter Linux verwendet werden.
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Abonnement. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)(in englischer Sprache).
* Ein Apache Spark-Cluster unter HDInsight. Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
* Oracle Java Development Kit, Version 8. 
  * **Java SDK 8** wird für die Eclipse-IDE-Laufzeit verwendet. Sie können es [hier](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)herunterladen.
* Eclipse-IDE. Dieser Artikel verwendet Eclipse Neon. Das Installationspaket finden Sie [hier](https://www.eclipse.org/downloads/).
* Scala-IDE für Eclipse. 
  
  * **Wenn Sie die Eclipse-IDE installiert haben**, können Sie das Scala-IDE-Plug-In hinzufügen, indem Sie zu **Help** -> **Install New Software** wechseln und [http://download.scala-ide.org/sdk/lithium/e46/scala211/stable/site](http://download.scala-ide.org/sdk/lithium/e46/scala211/stable/site) als Quelle zum Herunterladen von Scala-Plug-Ins für Eclipse hinzufügen. 
  * **Wenn Sie die Eclipse-IDE nicht installiert haben**, können Sie die Scala-IDE direkt von [hier](http://scala-ide.org/download/sdk.html)installieren. Sie können die ZIP-Datei über diesen Link herunterladen, sie extrahieren, zum Ordner **/eclipse** navigieren und von dort aus die Datei **eclipse.exe** ausführen.
    
    > [!NOTE]
    > Die Schritte in diesem Dokument basieren auf der Verwendung der Eclipse-IDE mit installiertem Scala-Plug-In.
    > 
    > 
* Spark-SDK. Sie können es [hier](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409)herunterladen.
* Installieren Sie e(fx)clipse von [https://www.eclipse.org/efxclipse/install.html](https://www.eclipse.org/efxclipse/install.html).

## <a name="install-hdinsight-tools-in-azure-toolkit-for-eclipse"></a>Installieren der HDInsight-Tools im Azure-Toolkit für Eclipse
Die HDInsight-Tools für Eclipse sind als Teil des Azure-Toolkits für Eclipse verfügbar. Anweisungen zum Installieren des Azure-Toolkits finden Sie unter [Installieren des Azure-Toolkits für Eclipse](../azure-toolkit-for-eclipse-installation.md).

## <a name="log-into-your-azure-subscription"></a>Anmelden bei Ihrem Azure-Abonnement
1. Starten Sie die Eclipse-IDE, und öffnen Sie den Azure Explorer. Klicken Sie in der IDE im Menü **Window** (Fenster) auf **Show View** (Ansicht anzeigen) und dann auf **Other** (Sonstiges). Erweitern Sie im daraufhin angezeigten Dialogfeld den Eintrag **Azure**, und klicken Sie auf **Azure Explorer** und dann auf **OK**.

    ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-1.png)
2. Klicken Sie im **Azure Explorer** mit der rechten Maustaste auf den Knoten **Azure**, und klicken Sie dann auf **Anmelden**.
3. Wählen Sie im Dialogfeld **Azure-Anmeldung** die Option „Authentifizierungsmodus“ aus, klicken Sie auf **Anmelden**, und geben Sie Ihre Azure-Anmeldeinformationen ein.
   
    ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-2.png)
4. Nachdem Sie sich angemeldet haben, werden im Dialogfeld **Abonnements auswählen** alle Azure-Abonnements aufgelistet, die den Anmeldeinformationen zugeordnet sind. Klicken Sie im Dialogfeld auf **Auswählen**, um es zu schließen.

    ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/Select-Subscriptions.png)
5. Erweitern Sie in der Registerkarte „Azure Explorer“ **HDInsight** , um die HDInsight Spark-Cluster in Ihrem Abonnement anzuzeigen.
   
    ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-3.png)
6. Sie können des Weiteren einen Clusternamenknoten erweitern, um die mit dem Cluster verbundenen Ressourcen (z.B. Speicherkonten) anzuzeigen.
   
    ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-4.png)

## <a name="set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster"></a>Einrichten eines Spark Scala-Projekts für einen HDInsight Spark-Cluster

1. Wählen Sie im Eclipse-IDE-Arbeitsbereich **Datei**, klicken Sie auf **Neu**, und klicken Sie dann auf **Projekt**. 
2. Erweitern Sie im Assistenten **Neues Projekt** die Option **HDInsight**, wählen Sie **Spark für HDInsight (Scala)**, und klicken Sie dann auf **Weiter**.

    ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-2.png)
3. Geben Sie im Dialogfeld **New HDInsight Scala Project** (Neues HDInsight Scala-Projekt) wie in der folgenden Abbildung dargestellt Werte ein, bzw. wählen Sie sie aus, und klicken Sie dann auf **Weiter**.
   
    ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-3.png)
   
   * Geben Sie einen Namen für das Projekt ein.
   * Achten Sie darauf, dass im Feld **JRE** für **Use an execution environment JRE** (Ausführungsumgebungs-JRE verwenden) **JavaSE-1.7** festgelegt ist.
   * Stellen Sie sicher, dass für das Spark-SDK der Speicherort festgelegt ist, in den Sie das SDK heruntergeladen haben. Der Link zum Downloadspeicherort wird in [Voraussetzungen](#prerequisites) weiter oben in diesem Thema aufgeführt. Sie können das SDK auch über den Link in diesem Dialogfeld herunterladen, wie in der Abbildung oben dargestellt.    
4.    Klicken Sie im nächsten Dialogfeld auf die Registerkarte **Bibliotheken**, und übernehmen Sie die Standardeinstellung. 
   
    ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-4.png)

    Klicken Sie auf **Finish** , bis das Dialogfeld mit den Projekteinstellungen geschlossen wird.

    **Ausführen einer Spark Scala-Anwendung in einem ADLS-Cluster** Wenn Sie eine Anwendung an ADLS übermitteln möchten, müssen Sie den Modus **Interaktiv** für die Protokollierung auswählen. 

    ![Erstellen einer Spark Scala-Anwendungsauthentifizierung](./media/hdinsight-apache-spark-eclipse-tool-plugin/Interactive-Authentication.png)

## <a name="create-a-scala-application-for-hdinsight-spark-cluster"></a>Erstellen einer Scala-Anwendung für einen HDInsight Spark-Cluster

1. Erweitern Sie in der bereits geöffneten Eclipse-IDE im **Paket-Explorer** das Projekt, das Sie zuvor erstellt haben, klicken Sie mit der rechten Maustaste auf **src**, zeigen Sie auf **Neu** und dann auf **Sonstige**.
2. Erweitern Sie im Dialogfeld **Select a wizard** (Assistenten auswählen) **Scala Wizards** (Scala-Assistenten), klicken Sie auf **Scala Object** (Scala-Objekt), und klicken Sie dann auf **Weiter**.
   
    ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-1.png)
3. Geben Sie im Dialogfeld **Create New File** (Neue Datei erstellen) einen Namen für das Objekt ein, und klicken Sie dann auf **Fertig stellen**.
   
    ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-2.png)
4. Fügen Sie folgenden Code in den Text-Editor ein.
   
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
5. Führen Sie die Anwendung in einem HDInsight Spark-Cluster aus.
   
   1. Klicken Sie im **Paket-Explorer** mit der rechten Maustaste auf den Projektnamen, und wählen Sie dann **Submit Spark Application to HDInsight** (Spark-Anwendung an HDInsight senden) aus.        
   2. Geben Sie in das Dialogfeld **Spark Submission** (Spark-Übermittlung) die folgenden Werte ein.
      
      * Wählen Sie für **Cluster Name**den HDInsight Spark-Cluster aus, auf dem Sie Ihre Anwendung ausführen möchten.
      * Sie müssen entweder ein Artefakt aus dem Eclipse-Projekt oder von der Festplatte auswählen.
      * Geben Sie im Textfeld **Main class name** den Namen des Objekts ein, das Sie im Code angegeben haben (siehe Abbildung unten).
        
          ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-3.png)
      * Da der Anwendungscode in diesem Beispiel keine Befehlszeilenargumente oder Referenzdateien (z. B. JAR) erfordert, können Sie die restlichen Textfelder leer lassen.
      * Klicken Sie auf **Submit**.
   3. Auf der Registerkarte **Spark Submission** sollte nun der Fortschritt angezeigt werden. Sie können die Anwendung auch anhalten, indem Sie im Fenster „Spark Submission“ (Spark-Übermittlung) auf die rote Schaltfläche klicken. Sie können auch die Protokolle für diese bestimmte Anwendungsausführung anzeigen, indem Sie auf das Globussymbol klicken (gekennzeichnet durch das blaue Feld in der Abbildung).
      
       ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-4.png)
      
      Im nächsten Abschnitt erfahren Sie, wie Sie mithilfe der HDInsight-Tools im Azure-Toolkit für Eclipse auf die Auftragsausgabe zugreifen.

## <a name="access-and-manage-hdinsight-spark-clusters-using-the-hdinsight-tools-in-azure-toolkit-for-eclipse"></a>Zugreifen auf und Verwalten von HDInsight Spark-Clustern mithilfe der HDInsight-Tools im Azure-Toolkit für Eclipse
Mit den HDInsight-Tools können Sie verschiedene Vorgänge ausführen.

### <a name="access-the-storage-container-for-the-cluster"></a>Zugreifen auf den Speichercontainer des Clusters
1. Erweitern Sie im Azure Explorer den Stammknoten **HDInsight** , um eine Liste der verfügbaren HDInsight Spark-Cluster anzuzeigen.
2. Erweitern Sie den Namen des Clusters, um das Speicherkonto und den Standardspeichercontainer des Clusters anzuzeigen.
   
    ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-5.png)
3. Klicken Sie auf den Namen des Speichercontainers, der dem Cluster zugeordnet ist. Im rechten Bereich sollten Sie einen Ordner namens **HVACOut** sehen. Doppelklicken Sie darauf, um den Ordner zu öffnen. Sie sehen Dateien vom Typ **part-***. Öffnen Sie eine dieser Dateien, um die Ausgabe der Anwendung anzuzeigen.

### <a name="access-the-spark-history-server"></a>Zugreifen auf den Spark-Verlaufsserver
1. Klicken Sie im **Azure Explorer** mit der rechten Maustaste auf den Namen Ihres Spark-Clusters, und wählen Sie dann **Open Spark History UI** (Spark-Verlaufsbenutzeroberfläche öffnen) aus. Geben Sie die Anmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden. Diese müssen Sie beim Bereitstellen des Clusters angegeben haben.
2. Im Dashboard des Spark-Verlaufsservers können Sie die Anwendung, deren Ausführung Sie gerade beendet haben, anhand des Anwendungsnamens suchen. Im obigen Code haben Sie den Anwendungsnamen mit `val conf = new SparkConf().setAppName("MyClusterApp")`festgelegt. Daher lautete der Name Ihrer Spark-Anwendung **MyClusterApp**.

### <a name="launch-the-ambari-portal"></a>Öffnen des Ambari-Portals
Klicken Sie im **Azure Explorer** mit der rechten Maustaste auf den Namen Ihres Spark-Clusters, und wählen Sie dann **Open Cluster Management Portal (Ambari)** (Clusterverwaltungsportal [Ambari] öffnen) aus. Geben Sie die Anmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden. Diese müssen Sie beim Bereitstellen des Clusters angegeben haben.

### <a name="manage-azure-subscriptions"></a>Verwalten von Azure-Abonnements
Standardmäßig führen die HDInsight-Tools im Azure-Toolkit für Eclipse die Spark-Cluster in all Ihren Azure-Abonnements auf. Bei Bedarf können Sie die Abonnements angeben, für die Sie auf den Cluster zugreifen möchten. Klicken Sie im **Azure Explorer** mit der rechten Maustaste auf den Stammknoten **Azure**, und klicken Sie auf **Abonnements verwalten**. Deaktivieren Sie im Dialogfeld die Kontrollkästchen für die Abonnements, auf die Sie nicht zugreifen möchten, und klicken Sie dann auf **Close**. Sie können auch auf **Sign Out** klicken, wenn Sie sich von Ihrem Azure-Abonnement abmelden möchten.

## <a name="run-a-spark-scala-application-locally"></a>Lokales Ausführen einer Spark Scala-Anwendung
Mithilfe der HDInsight-Tools-im Azure-Toolkit für Eclipse können Sie Spark Scala-Anwendungen lokal auf Ihrer Arbeitsstation ausführen. In der Regel müssen solche Anwendungen nicht auf Clusterressourcen wie den Speichercontainer zugreifen und können lokal ausgeführt und getestet werden.

### <a name="prerequisite"></a>Voraussetzung
Beim lokalen Ausführen der Spark Scala-Anwendung auf einem Windows-Computer erhalten Sie ggf. eine Ausnahme wie unter [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356)beschrieben, die ausgelöst wird, weil im Windows-Betriebssystem die Datei **WinUtils.exe** fehlt. Um diesen Fehler zu umgehen, müssen Sie [die ausführbare Datei hier herunterladen](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) und an einem Speicherort wie **C:\WinUtils\bin** speichern. Fügen Sie anschließend die Umgebungsvariable **HADOOP_HOME** hinzu, und setzen Sie den Wert der Variablen auf **C\WinUtils**.

### <a name="run-a-local-spark-scala-application"></a>Ausführen einer lokalen Spark Scala-Anwendung
1. Starten Sie Eclipse und erstellen Sie ein neues Projekt. Wählen Sie im Dialogfeld für das neue Projekt Folgendes aus, und klicken Sie anschließend auf **Weiter**:
   
    ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run.png)
   
   * Wählen Sie im linken Bereich **HDInsight**aus.
   * Wählen Sie im rechten Bereich **Spark on HDInsight Local Run Sample (Scala)**(Spark auf HDInsight – Beispiel für lokale Ausführung [Scala]) aus.
   * Klicken Sie auf **Weiter**.
2. Um die Projektdetails bereitzustellen, führen Sie die Schritte 3 bis 6 wie oben im Abschnitt [Einrichten eines Spark Scala-Projekts für einen HDInsight Spark-Cluster](#set-up-a-spark-scala-application-project-for-an-hdinsight-spark cluster) gezeigt aus.
3. Die Vorlage fügt einen Beispielcode (**LogQuery**) unter dem Ordner **src** hinzu, der lokal auf Ihrem Computer ausgeführt werden kann.
   
    ![Ergebnis der lokalen Ausführung der Spark-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/local-app.png)
4. Klicken Sie mit der rechten Maustaste auf die Anwendung **LogQuery**, zeigen Sie auf **Ausführen als**, und klicken Sie dann auf **1 Scala Application** (1 Scala-Anwendung). Unten auf der Registerkarte **Console** wird eine Ausgabe wie die folgende angezeigt.
   
   ![Ergebnis der lokalen Ausführung der Spark-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run-result.png)

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
* [Verwenden der HDInsight-Tools im Azure-Toolkit für IntelliJ zum Erstellen und Übermitteln von Spark Scala-Anwendungen](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Verwenden der HDInsight-Tools im Azure-Toolkit für IntelliJ zum Remotedebuggen von Spark-Anwendungen](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verwenden von Zeppelin-Notebooks mit einem Spark-Cluster in HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)
* [Verfügbare Kernels für Jupyter-Notebook im Spark-Cluster für HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Verwenden von externen Paketen mit Jupyter Notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen
* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight(Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](hdinsight-apache-spark-job-debugging.md)


