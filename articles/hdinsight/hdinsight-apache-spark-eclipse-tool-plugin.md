 <properties
	pageTitle="Erstellen von Spark Scala-Anwendungen mithilfe der HDInsight-Tools im Azure-Toolkit für Eclipse | Microsoft Azure"
	description="Hier erfahren Sie, wie Sie eine eigenständige Spark-Anwendung zur Ausführung in HDInsight Spark-Clustern erstellen."
	services="hdinsight"
	documentationCenter=""
	authors="nitinme"
	manager="jhubbard"
	editor="cgronlun"
	tags="azure-portal"/>

<tags
	ms.service="hdinsight"
	ms.workload="big-data"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/30/2016"
	ms.author="nitinme"/>


# Verwenden der HDInsight-Tools im Azure-Toolkit für Eclipse zum Erstellen von Spark-Anwendungen für HDInsight Spark-Cluster unter Linux

Dieser Artikel enthält eine detaillierte Anleitung zum Entwickeln von in Scala geschriebenen Spark-Anwendungen und zu deren Übermittlung an einen HDInsight Spark-Cluster mithilfe der HDInsight-Tools im Azure-Toolkit für Eclipse. Sie können die Tools auf verschiedene Arten verwenden:

* Zum Entwickeln und Übermitteln einer Scala Spark-Anwendung an einen HDInsight Spark-Cluster
* Zum Zugreifen auf Ihre Azure HDInsight Spark-Clusterressourcen
* Zum Entwickeln und lokalen Ausführen einer Scala Spark-Anwendung

>[AZURE.IMPORTANT] Dieses Tool kann nur zum Erstellen und Übermitteln von Anwendungen an einen HDInsight Spark-Cluster unter Linux verwendet werden.


##Voraussetzungen

* Ein Azure-Abonnement. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) (in englischer Sprache).

* Einen Apache Spark-Cluster unter HDInsight (Linux). Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

* Oracle Java Development-Kit Version 7 und Version 8.
	* **Java SDK 7** wird zum Kompilieren von Spark-Projekten verwendet, da die HDInsight-Cluster Java Version 7 unterstützen. Sie können Java SDK 7 [hier](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) herunterladen.
	* **Java SDK 8** wird für die Eclipse-IDE-Laufzeit verwendet. Sie können es [hier](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) herunterladen.

* Eclipse-IDE. Dieser Artikel verwendet Eclipse Neon. Das Installationspaket finden Sie [hier](https://www.eclipse.org/downloads/).

* Scala-IDE für Eclipse.
	* **Wenn Sie die Eclipse-IDE installiert haben**, können Sie das Scala-IDE-Plug-In hinzufügen, indem Sie unter **Help** (Hilfe) > **Install New Software** (Neue Software installieren) [http://download.scala-ide.org/sdk/lithium/e44/scala211/stable/site](http://download.scala-ide.org/sdk/lithium/e44/scala211/stable/site) als Quelle zum Herunterladen von Scala-Plug-Ins für Eclipse hinzufügen.
	* **Wenn Sie die Eclipse-IDE nicht installiert haben**, können Sie die Scala-IDE direkt von [hier](http://scala-ide.org/download/sdk.html) installieren. Sie können die ZIP-Datei über diesen Link herunterladen, sie extrahieren, zum Ordner **/eclipse** navigieren und von dort aus die Datei **eclipse.exe** ausführen.
	
	>[AZURE.NOTE] Die Schritte in diesem Dokument basieren auf der Verwendung der Eclipse-IDE mit installiertem Scala-Plug-In.

* Spark-SDK. Sie können es [hier](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409) herunterladen.

* Installieren Sie e(fx)clipse von [https://www.eclipse.org/efxclipse/install.html](https://www.eclipse.org/efxclipse/install.html).


## Installieren der HDInsight-Tools im Azure-Toolkit für Eclipse

Die HDInsight-Tools für Eclipse sind als Teil des Azure-Toolkits für Eclipse verfügbar. Anweisungen zum Installieren des Azure-Toolkits finden Sie unter [Installieren des Azure-Toolkits für Eclipse](../azure-toolkit-for-eclipse-installation.md).

## Anmelden bei Ihrem Azure-Abonnement

1. Starten Sie die Eclipse-IDE, und öffnen Sie den Azure Explorer. Klicken Sie in der IDE im Menü **Window** auf **Show View** dann auf **Other**. Erweitern Sie im daraufhin angezeigten Dialogfeld den Eintrag **Azure**, und klicken Sie auf **Azure Explorer** und dann auf **OK**.

	![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-1.png)

2. Klicken Sie mit der rechten Maustaste auf den Knoten **Azure** im **Azure Explorer**, und klicken Sie auf **Manage Subscriptions**.

3. Klicken Sie im Dialogfeld **Manage Subscriptions** auf **Sign in**, und geben Sie Ihre Azure-Anmeldeinformationen ein.

	![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-2.png)

4. Nachdem Sie sich angemeldet haben, werden im Dialogfeld **Manage Subscriptions** alle Azure-Abonnements aufgelistet, die den Anmeldeinformationen zugeordnet sind. Klicken Sie im Dialogfeld auf **Schließen**.

5. Erweitern Sie in der Registerkarte „Azure Explorer“ **HDInsight**, um die HDInsight Spark-Cluster in Ihrem Abonnement anzuzeigen.

	![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-3.png)

6. Sie können des Weiteren einen Clusternamenknoten erweitern, um die mit dem Cluster verbundenen Ressourcen (z.B. Speicherkonten) anzuzeigen.

	![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-4.png)

## Einrichten eines Spark Scala-Projekts für einen HDInsight Spark-Cluster

1. Wählen Sie im Eclipse-IDE-Arbeitsbereich **File**, klicken Sie auf **New**, und klicken Sie dann auf **Project**.

2. Erweitern Sie im Assistenten **New Project**die Option **HDInsight**, wählen Sie **Spark on HDInsight (Scala)**, und klicken Sie dann auf **Next**.

	![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-2.png)

3. Geben Sie im Dialogfeld **New HDInsight Scala Project** wie in der folgenden Abbildung dargestellt Werte ein, bzw. wählen Sie Werte aus, und klicken Sie dann auf **Next**.

	![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-3.png)

	* Geben Sie einen Namen für das Projekt ein.
	* Achten Sie darauf, dass im Feld **JRE** für **Use an execution environment JRE** die Option **JavaSE-1.7** festgelegt ist.
	* Stellen Sie sicher, dass für das Spark-SDK der Speicherort festgelegt ist, in den Sie das SDK heruntergeladen haben. Der Link zum Downloadspeicherort wird in [Voraussetzungen](#prerequisites) weiter oben in diesem Thema aufgeführt. Sie können das SDK auch über den Link in diesem Dialogfeld herunterladen, wie in der Abbildung oben dargestellt.

4. Klicken Sie im nächsten Dialogfeld auf die Registerkarte **Libraries**, und doppelklicken Sie dann auf **JRE System Library [JavaSE-1.7]**.

	![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-4.png)

5. Stellen Sie im Dialogfeld **Edit Library** sicher, dass für **Execution Environment** der Wert **JavaSE-1.7(jdk1.7.0\_79)** festgelegt ist. Wenn dies nicht als Option verfügbar ist, führen Sie die folgenden Schritte aus.

	1. Wählen Sie die Option **Alternate JRE** aus, und überprüfen Sie, ob **JavaSE-1.7(jdk1.7.0\_79)** verfügbar ist.
	2. Falls nicht, klicken Sie auf die Schaltfläche **Installed JREs**.

		  ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-5.png)

	3. Klicken Sie im Dialogfeld **Installed JREs** auf **Add**.

		  ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-6.png)

	4. Wählen Sie im Dialogfeld **JRE Type** die Option **Standard VM** aus, und klicken Sie auf **Next**.

		  ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-7.png)

	5. Klicken Sie im Dialogfeld **JRE Definition** auf „Directory“, navigieren Sie zum Speicherort für die JDK 7-Installation, und wählen Sie den Stammordner für **jdk1.7.0\_79** aus.

		  ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-8.png)

	6. Klicken Sie auf **Fertig stellen**. Wählen Sie im Dialogfeld **Installed JREs** die neu hinzugefügte JRE aus, und klicken Sie dann auf **OK**.

		   ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-9.png)

	7. Die neu hinzugefügte JRE sollte als **Execution Environment** aufgelistet werden. Klicken Sie auf **Fertig stellen**.

	  	   ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-10.png)

6. Doppelklicken Sie auf der Registerkarte **Libraries** auf **Scala Library Container[2.11.8]**. Wählen Sie im Dialogfeld **Edit Library** die Option **Fixed Scala Library container:2.10.6**.

	![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-11.png)

	Klicken Sie auf **Finish**, bis das Dialogfeld mit den Projekteinstellungen geschlossen wird.

## Erstellen einer Scala-Anwendung für einen HDInsight Spark-Cluster

1. Erweitern Sie in der bereits geöffneten Eclipse-IDE im **Package Explorer** das Projekt, das Sie zuvor erstellt haben, klicken Sie mit der rechten Maustaste auf **src**, zeigen Sie auf **New**, und klicken Sie auf **Other**.

2. Erweitern Sie im Dialogfeld **Select a wizard** den Eintrag **Scala Wizards**, klicken Sie auf **Scala Object**, und klicken Sie dann auf **Next**.

	![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-1.png)

3. Geben Sie im Dialogfeld **Create New File** einen Namen für das Objekt ein, und klicken Sie dann auf **Finish**.

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

	1. Klicken Sie im **Package Explorer** mit der rechten Maustaste auf den Projektnamen, und wählen Sie dann **Submit Spark Application to HDInsight** aus.

	2. Geben Sie im Dialogfeld **Spark Submission** die folgenden Werte ein.

		* Wählen Sie für **Cluster Name** den HDInsight Spark-Cluster aus, auf dem Sie Ihre Anwendung ausführen möchten.

		* Sie müssen entweder ein Artefakt aus dem Eclipse-Projekt oder von der Festplatte auswählen.

		* Geben Sie im Textfeld **Main class name** den Namen des Objekts ein, das Sie im Code angegeben haben (siehe Abbildung unten).

			![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-3.png)

		* Da der Anwendungscode in diesem Beispiel keine Befehlszeilenargumente oder Referenzdateien (z. B. JAR) erfordert, können Sie die restlichen Textfelder leer lassen.

		* Klicken Sie auf **Senden**.

	3. Auf der Registerkarte **Spark Submission** sollte nun der Fortschritt angezeigt werden. Sie können die Anwendung auch anhalten, indem Sie im Fenster „Spark Submission“ (Spark-Übermittlung) auf die rote Schaltfläche klicken. Sie können auch die Protokolle für diese bestimmte Anwendungsausführung anzeigen, indem Sie auf das Globussymbol klicken (gekennzeichnet durch das blaue Feld in der Abbildung).

        ![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-4.png)

    Im nächsten Abschnitt erfahren Sie, wie Sie mithilfe der HDInsight-Tools im Azure-Toolkit für Eclipse auf die Auftragsausgabe zugreifen.


## Zugreifen auf und Verwalten von HDInsight Spark-Clustern mithilfe der HDInsight-Tools im Azure-Toolkit für Eclipse

Sie können eine Vielzahl von Vorgängen mit den HDInsight-Tools ausführen.

### Zugreifen auf den Speichercontainer des Clusters

1. Erweitern Sie im Azure Explorer den Stammknoten **HDInsight**, um eine Liste der verfügbaren HDInsight Spark-Cluster anzuzeigen.

3. Erweitern Sie den Namen des Clusters, um das Speicherkonto und den Standardspeichercontainer des Clusters anzuzeigen.

	![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-5.png)

4. Klicken Sie auf den Namen des Speichercontainers, der dem Cluster zugeordnet ist. Im rechten Bereich sollten Sie einen Ordner namens **HVACOut** sehen. Doppelklicken Sie darauf, um den Ordner zu öffnen. Sie sehen Dateien vom Typ **part-***. Öffnen Sie eine dieser Dateien, um die Ausgabe der Anwendung anzuzeigen.

### Zugreifen auf den Spark-Verlaufsserver

1. Klicken Sie im **Azure Explorer** mit der rechten Maustaste auf den Namen Ihres Spark-Clusters, und wählen Sie dann **Open Spark History UI** aus. Geben Sie die Anmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden. Diese müssen Sie beim Bereitstellen des Clusters angegeben haben.

2. Im Dashboard des Spark-Verlaufsservers können Sie die Anwendung, deren Ausführung Sie gerade beendet haben, anhand des Anwendungsnamens suchen. Im obigen Code haben Sie den Anwendungsnamen mit `val conf = new SparkConf().setAppName("MyClusterApp")` festgelegt. Daher lautete der Name Ihrer Spark-Anwendung **MyClusterApp**.

### Öffnen des Ambari-Portals

Klicken Sie im **Azure Explorer** mit der rechten Maustaste auf den Namen Ihres Spark-Clusters, und wählen Sie dann **Open Cluster Management Portal (Ambari)** aus. Geben Sie die Anmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden. Diese müssen Sie beim Bereitstellen des Clusters angegeben haben.

### Verwalten von Azure-Abonnements

Standardmäßig führen die HDInsight-Tools im Azure-Toolkit für Eclipse die Spark-Cluster in all Ihren Azure-Abonnements auf. Bei Bedarf können Sie die Abonnements angeben, für die Sie auf den Cluster zugreifen möchten. Klicken Sie im **Azure Explorer** mit der rechten Maustaste auf den Stammknoten **Azure**, und klicken Sie auf **Manage Subscriptions**. Deaktivieren Sie im Dialogfeld die Kontrollkästchen für die Abonnements, auf die Sie nicht zugreifen möchten, und klicken Sie dann auf **Close**. Sie können auch auf **Sign Out** klicken, wenn Sie sich von Ihrem Azure-Abonnement abmelden möchten.


## Lokales Ausführen einer Spark Scala-Anwendung

Mithilfe der HDInsight-Tools-im Azure-Toolkit für Eclipse können Sie Spark Scala-Anwendungen lokal auf Ihrer Arbeitsstation ausführen. In der Regel müssen solche Anwendungen nicht auf Clusterressourcen wie den Speichercontainer zugreifen und können lokal ausgeführt und getestet werden.

### Voraussetzung

Beim lokalen Ausführen der Spark Scala-Anwendung auf einem Windows-Computer erhalten Sie ggf. eine Ausnahme wie unter [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356) beschrieben, die ausgelöst wird, weil im Windows-Betriebssystem die Datei **WinUtils.exe** fehlt. Um diesen Fehler zu umgehen, müssen Sie [die ausführbare Datei hier herunterladen](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) und an einem Speicherort wie **C:\\WinUtils\\bin** speichern. Fügen Sie anschließend die Umgebungsvariable **HADOOP\_HOME** hinzu, und legen Sie den Wert der Variablen auf **C\\WinUtils** fest.

### Ausführen einer lokalen Spark Scala-Anwendung	 

1. Starten Sie Eclipse und erstellen Sie ein neues Projekt. Wählen Sie im Dialogfeld für das neue Projekt Folgendes aus, und klicken Sie anschließend auf **Weiter**:

	![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run.png)

	* Wählen Sie im linken Bereich **HDInsight** aus.
	* Wählen Sie im rechten Bereich **Spark on HDInsight Local Run Sample (Scala)** (Spark auf HDInsight – Beispiel für lokale Ausführung [Scala]) aus.
	* Klicken Sie auf **Next**.

2. Um die Projektdetails bereitzustellen, führen Sie die Schritte 3 bis 6 wie im früheren Abschnitt [Einrichten eines Spark Scala-Projekts für einen HDInsight Spark-Cluster](#set-up-a-spark-scala-application-project-for-an-hdinsight-spark Cluster) gezeigt aus.

3. Die Vorlage fügt einen Beispielcode (**LogQuery**) unter dem Ordner **src** hinzu, der lokal auf Ihrem Computer ausgeführt werden kann.

	![Ergebnis der lokalen Ausführung der Spark-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/local-app.png)

4.  Klicken Sie mit der rechten Maustaste auf die Anwendung **LogQuery**, zeigen Sie auf **Run As**, und klicken Sie dann auf **1 Scala Application**. Unten auf der Registerkarte **Console** wird eine Ausgabe wie die folgende angezeigt.

	![Ergebnis der lokalen Ausführung der Spark-Anwendung](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run-result.png)

## Feedback und bekannte Probleme

Zurzeit wird die direkte Anzeige von Spark-Ausgaben nicht unterstützt. Wir arbeiten daran.

Falls Sie Vorschläge oder Feedback haben oder bei Verwendung dieses Tools Probleme auftreten, senden Sie eine E-Mail an „hdivstool@microsoft.com“.

## <a name="seealso"></a>Weitere Informationen


* [Übersicht: Apache Spark in Azure HDInsight](hdinsight-apache-spark-overview.md)

### Szenarios

* [Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](hdinsight-apache-spark-use-bi-tools.md)

* [Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Spark-Streaming: Erstellen von Echtzeitstreaminganwendungen mithilfe von Spark in HDInsight](hdinsight-apache-spark-eventhub-streaming.md)

* [Websiteprotokollanalyse mithilfe von Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### Erstellen und Ausführen von Anwendungen

* [Erstellen einer eigenständigen Anwendung mit Scala](hdinsight-apache-spark-create-standalone-application.md)

* [Remoteausführung von Aufträgen in einem Spark-Cluster mithilfe von Livy](hdinsight-apache-spark-livy-rest-interface.md)

### Tools und Erweiterungen

* [Verwenden der HDInsight-Tools im Azure-Toolkit für IntelliJ zum Erstellen und Übermitteln von Spark Scala-Anwendungen](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Verwenden der HDInsight-Tools im Azure-Toolkit für IntelliJ zum Remotedebuggen von Spark-Anwendungen](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Verwenden von Zeppelin-Notebooks mit einem Spark-Cluster in HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Verfügbare Kernels für Jupyter-Notebook im Spark-Cluster für HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Verwenden von externen Paketen mit Jupyter Notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### Verwalten von Ressourcen

* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Track and debug jobs running on an Apache Spark cluster in HDInsight (Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](hdinsight-apache-spark-job-debugging.md)

<!---HONumber=AcomDC_0914_2016-->