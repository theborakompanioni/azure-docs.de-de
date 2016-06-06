 <properties
	pageTitle="Erstellen von Spark Scala-Anwendungen mithilfe des HDInsight-Plug-Ins für IntelliJ IDEA | Microsoft Azure"
	description="Hier erfahren Sie, wie Sie eine eigenständige Spark-Anwendung zur Ausführung in HDInsight Spark-Clustern erstellen."
	services="hdinsight"
	documentationCenter=""
	authors="nitinme"
	manager="paulettm"
	editor="cgronlun"
	tags="azure-portal"/>

<tags
	ms.service="hdinsight"
	ms.workload="big-data"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/18/2016"
	ms.author="nitinme"/>


# Verwenden des HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Erstellen von Spark Scala-Anwendungen für HDInsight Spark-Cluster unter Linux (Vorschau)

Dieser Artikel enthält eine detaillierte Anleitung zum Entwickeln von in Scala geschriebenen Spark-Anwendungen und zu deren Übermittlung an einen HDInsight Spark-Cluster mithilfe des HDInsight-Plug-Ins für IntelliJ IDEA. Sie können das Plug-In auf verschiedene Weisen verwenden:

* Zum Entwickeln und Übermitteln einer Scala Spark-Anwendung an einen HDInsight Spark-Cluster
* Zum Zugreifen auf Ihre Azure HDInsight Spark-Clusterressourcen
* Zum Entwickeln und lokalen Ausführen einer Scala Spark-Anwendung

Informationen zu den ersten Schritten finden Sie auch in diesem [Video](https://mix.office.com/watch/1nqkqjt5xonza).

>[AZURE.IMPORTANT] Dieses Tool kann nur zum Erstellen und Übermitteln von Anwendungen an einen HDInsight Spark-Cluster unter Linux verwendet werden.


**Voraussetzungen**

* Ein Azure-Abonnement. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) (in englischer Sprache).
* Einen Apache Spark-Cluster unter HDInsight (Linux). Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
* Oracle Java Development Kit. Das Installationspaket finden Sie [hier](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* IntelliJ IDEA. In diesem Artikel wird die Version 15.0.1 verwendet. Das Installationspaket finden Sie [hier](https://www.jetbrains.com/idea/download/).

## Installieren des HDInsight-Tools-Plug-Ins für IntelliJ IDEA

1. Klicken Sie auf der Begrüßungsseite von IntelliJ IDEA auf **Configure** und anschließend auf **Plugins**.

2. Klicken Sie auf dem nächsten Bildschirm links unten auf **Browse Repositories**. Suchen Sie im daraufhin eingeblendeten Dialogfeld **Browse Repositories** nach **HDInsight**, wählen Sie **Microsoft Azure HDInsight Tools for IntelliJ (Preview)** aus, und klicken Sie dann auf **Install**. Dieses Plug-In weist eine Abhängigkeit zum Scala-Plug-In auf. Wenn also das Scala-Plug-In nicht installiert ist, werden Sie aufgefordert, dieses ebenfalls zu installieren.

	![HDInsight-Plug-In installieren](./media/hdinsight-apache-spark-intellij-tool-plugin/install-hdinsight-plugin.png)

3. Klicken Sie bei Aufforderung auf die Schaltfläche **Restart IntelliJ IDEA**, um die IDE neu zu starten.


## Ausführen einer Spark Scala-Anwendung in einem HDInsight Spark-Cluster

1. Starten Sie IntelliJ IDEA, und erstellen Sie ein neues Projekt. Wählen Sie im Dialogfeld für das neue Projekt Folgendes aus, und klicken Sie anschließend auf **Weiter**:

	![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)

	* Wählen Sie im linken Bereich **HDInsight** aus.
	* Wählen Sie im rechten Bereich **Spark on HDInsight (Scala)** aus.
	* Klicken Sie auf **Weiter**.

2. Geben Sie im nächsten Fenster die Projektdetails an.

	* Geben Sie einen Projektnamen und den Projektspeicherort an.
	* Geben Sie für **Project SDK** eine Java-Version größer als 7 an.
	* Klicken Sie für **Scala SDK** auf **Create**, dann auf **Download**, und wählen Sie die zu verwendende Version von Scala aus. **Verwenden Sie nicht die Version 2.11.x**. In diesem Beispiel wird Version **2.10.6** verwendet.

		![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-scala-version.png)

	* Für **Spark SDK**: Laden Sie das zu verwendende SDK [hier](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409) herunter. Sie können stattdessen auch das [Spark Maven Repository](http://mvnrepository.com/search?q=spark) verwenden. Stellen Sie jedoch sicher, dass die richtige Maven-Repository-Version für die Entwicklung Ihrer Spark-Anwendungen installiert ist. (Wenn Sie z. B. Spark Streaming verwenden, müssen Sie sicherstellen, dass Sie den Teil für Spark Streaming installiert haben. Vergewissern Sie sich auch, dass Sie das Repository für Scala 2.10 verwenden. Verwenden Sie nicht das Repository für Scala 2.11.)

		![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-scala-project-details.png)

	* Klicken Sie auf **Fertig stellen**.

3. Das Spark-Projekt erstellt automatisch ein Artefakt für Sie. Sie können auch selbst ein Artefakt erstellen, indem Sie folgende Schritte ausführen:

	1. Klicken Sie im Menü **Datei** auf **Projektstruktur**.
	2. Klicken Sie im Dialogfeld **Project Structure** auf **Artifacts** und anschließend auf das Pluszeichen. Klicken Sie im Popupdialogfeld auf **JAR** und anschließend auf **Empty**.

		![JAR-Datei erstellen](./media/hdinsight-apache-spark-intellij-tool-plugin/create-jar-1.png)

	3. Geben Sie einen Namen für die JAR-Datei ein (z. B. **MyClusterApp**). Klicken Sie im Bereich „Available Elements“ mit der rechten Maustaste auf **'MyClusterApp' compile output**, und klicken Sie dann auf **Put into Output Root**.

		![JAR-Datei erstellen](./media/hdinsight-apache-spark-intellij-tool-plugin/create-jar-2.png)

	4. Klicken Sie auf **Apply** und anschließend auf **OK**.

4. Fügen Sie den Quellcode der Anwendung hinzu.

	1. Klicken Sie in **Project Explorer** mit der rechten Maustaste auf **src**, zeigen Sie auf **New**, und klicken Sie dann auf **Scala class**.

		![Hinzufügen von Quellcode](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)

	2. Geben Sie im Dialogfeld **Create New Scala Class** einen Namen ein. Wählen Sie **Object** für **Kind** aus, und klicken Sie dann auf **OK**.

		![Hinzufügen von Quellcode](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)

	3. Fügen Sie den folgenden Code in die Datei **MyClusterApp.scala** ein. Dieser Code liest die Daten aus der Datei „HVAC.csv“ (die für alle HDInsight Spark-Cluster verfügbar ist), ruft die Zeilen ab, die nur eine Ziffer in der siebten Spalte der CSV-Datei enthalten, und schreibt die Ausgabe in **/HVACOut** unter dem Standardspeichercontainer für den Cluster.


			import org.apache.spark.SparkConf
			import org.apache.spark.SparkContext

			object MyClusterApp{
			  def main (arg: Array[String]): Unit = {
			    val conf = new SparkConf().setAppName("MyClusterApp")
			    val sc = new SparkContext(conf)

			    val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

			    //find the rows which have only one digit in the 7th column in the CSV
				val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)

			    rdd1.saveAsTextFile("wasb:///HVACOut")
			  }

			}

5. Führen Sie die Anwendung in einem HDInsight Spark-Cluster aus.

	1. Klicken Sie in **Project Explorer** mit der rechten Maustaste auf den Projektnamen, und wählen Sie dann **Submit Spark Application to HDInsight** aus.

		![Übermitteln der Spark-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)

	2. Sie werden zur Eingabe der Anmeldeinformationen Ihres Azure-Abonnements aufgefordert. Geben Sie in das Dialogfeld **Spark Submission** die folgenden Werte ein.

		![Übermitteln der Spark-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-2.png)

		* Wählen Sie für **Spark clusters (Linux only)** den HDInsight Spark-Cluster aus, in dem Sie Ihre Anwendung ausführen möchten.

		* Sie müssen ein Artefakt aus dem IntelliJ-Projekt oder von der Festplatte auswählen.

		* Klicken Sie neben dem Textfeld **Main class name** auf die Auslassungspunkte (![Auslassungspunkte](./media/hdinsight-apache-spark-intellij-tool-plugin/ellipsis.png)), wählen Sie die Hauptklasse in Ihrem Anwendungsquellcode aus, und klicken Sie dann auf **OK**.

			![Übermitteln der Spark-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-3.png)

		* Da der Anwendungscode in diesem Beispiel keine Befehlszeilenargumente oder Referenzdateien (z. B. JAR) erfordert, können Sie die restlichen Textfelder leer lassen.

		* Klicken Sie auf **Senden**.

	3. Auf der Registerkarte **Spark Submission** unten im Fenster sollte nun der Prozess angezeigt werden. Sie können die Anwendung auch anhalten, indem Sie im Fenster „Spark Submission“ auf die rote Schaltfläche klicken.

        ![Ergebnis der Spark-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)

    Im nächsten Abschnitt erfahren Sie, wie Sie mithilfe des HDInsight-Plug-Ins für IntelliJ IDEA auf die Auftragsausgabe zugreifen.


## Zugreifen auf und Verwalten von HDInsight Spark-Clustern mithilfe des HDInsight-Plug-Ins für IntelliJ

Sie können eine Vielzahl von Vorgängen mit dem HDInsight-Plug-In ausführen.

### Zugreifen auf den Speichercontainer des Clusters

1. Zeigen Sie im Menü **View** auf **Tool Windows**, und klicken Sie dann auf **HDInsight Explorer**. Wenn Sie dazu aufgefordert werden, geben Sie die Anmeldeinformationen für Ihr Azure-Abonnement ein.

2. Erweitern Sie den Stammknoten **HDInsight**, um eine Liste der verfügbaren HDInsight Spark-Cluster anzuzeigen.

3. Erweitern Sie den Namen des Clusters, um das Speicherkonto und den Standardspeichercontainer des Clusters anzuzeigen.

	![Zugreifen auf Clusterspeicher](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-access-storage.png)

4. Klicken Sie auf den Namen des Speichercontainers, der dem Cluster zugeordnet ist. Im rechten Bereich sollten Sie einen Ordner namens **HVACOut** sehen. Doppelklicken Sie darauf, um den Ordner zu öffnen. Sie sehen Dateien vom Typ **part-***. Öffnen Sie eine dieser Dateien, um die Ausgabe der Anwendung anzuzeigen.

### Zugreifen auf den Spark-Verlaufsserver

1. Klicken Sie in **HDInsight Explorer** mit der rechten Maustaste auf den Namen Ihres Spark-Clusters, und wählen Sie dann **Open Spark History UI** aus. Geben Sie die Anmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden. Diese müssen Sie beim Bereitstellen des Clusters angegeben haben.

2. Im Dashboard des Spark-Verlaufsservers können Sie die Anwendung, deren Ausführung Sie gerade beendet haben, anhand des Anwendungsnamens suchen. Im obigen Code haben Sie den Anwendungsnamen mit `val conf = new SparkConf().setAppName("MyClusterApp")` festgelegt. Daher lautete der Name Ihrer Spark-Anwendung **MyClusterApp**.

### Öffnen des Ambari-Portals

Klicken Sie in **HDInsight Explorer** mit der rechten Maustaste auf den Namen Ihres Spark-Clusters, und wählen Sie dann **Open Cluster Management Portal (Ambari)** aus. Geben Sie die Anmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden. Diese müssen Sie beim Bereitstellen des Clusters angegeben haben.

### Verwalten von Azure-Abonnements

Standardmäßig listet das HDInsight-Plug-In die Spark-Cluster in allen Ihren Azure-Abonnements auf. Bei Bedarf können Sie die Abonnements angeben, für die Sie auf den Cluster zugreifen möchten. Klicken Sie in **HDInsight Explorer** mit der rechten Maustaste auf den Stammknoten **HDInsight**, und wählen Sie im Kontextmenü **Manage Subscriptions** aus. Deaktivieren Sie im Dialogfeld die Kontrollkästchen für das Abonnement, auf das Sie nicht zugreifen möchten, und klicken Sie dann auf **Close**. Sie können auch auf **Sign Out** klicken, wenn Sie sich von Ihrem Azure-Abonnement abmelden möchten.


## Lokales Ausführen einer Spark Scala-Anwendung

Mithilfe des HDInsight-Tools-Plug-Ins für IntelliJ IDEA können Sie Spark Scala-Anwendungen lokal auf Ihrer Arbeitsstation ausführen. In der Regel müssen solche Anwendungen nicht auf Clusterressourcen wie den Speichercontainer zugreifen und können lokal ausgeführt und getestet werden.

### Voraussetzung

Beim lokalen Ausführen der Spark Scala-Anwendung auf einem Windows-Computer erhalten Sie ggf. eine Ausnahme wie unter [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356) beschrieben, die aufgrund der unter Windows fehlenden Datei „WinUtils.exe“ ausgelöst wird. Um diesen Fehler zu umgehen, müssen Sie [die ausführbare Datei hier herunterladen](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) und an einem Speicherort wie **C:\\WinUtils\\bin** speichern. Fügen Sie anschließend die Umgebungsvariable **HADOOP\_HOME** hinzu, und setzen Sie den Wert der Variablen auf **C\\WinUtils**.

### Ausführen einer lokalen Spark Scala-Anwendung	 

1. Starten Sie IntelliJ IDEA, und erstellen Sie ein neues Projekt. Wählen Sie im Dialogfeld für das neue Projekt Folgendes aus, und klicken Sie anschließend auf **Weiter**:

	![Erstellen einer Spark Scala-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run.png)

	* Wählen Sie im linken Bereich **HDInsight** aus.
	* Wählen Sie im rechten Bereich **Spark on HDInsight Local Run Sample (Scala)** aus.
	* Klicken Sie auf **Weiter**.

2. Führen Sie die Schritte im Abschnitt „Ausführen einer Spark Scala-Anwendung in einem HDInsight Spark-Cluster“ aus, um die erforderlichen Einstellungen zu konfigurieren.

3. Führen Sie die Anwendung lokal auf Ihrer Arbeitsstation aus. Klicken Sie mit der rechten Maustaste auf die Scala-Anwendung, und klicken Sie auf **Run ‚LogQuery‘**. Unten auf der Registerkarte **Run** wird eine Ausgabe wie die folgende angezeigt.

	![Ergebnis der lokalen Ausführung der Spark-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run-result.png)

## Konvertieren vorhandener IntelliJ IDEA-Anwendungen für die Verwendung des HDInsight-Tools-Plug-Ins

Sie können auch Ihre vorhandenen in IntelliJ IDEA erstellten Spark Scala-Anwendungen so umwandeln, dass sie mit dem HDInsight-Tools-Plug-in kompatibel sind. Dadurch können Sie das Tool verwenden, um die Anwendungen an einen HDInsight Spark-Cluster zu übermitteln. Führen Sie hierzu die folgenden Schritte aus:

1. Öffnen Sie für eine vorhandene Spark Scala-Anwendung, die mit IntelliJ IDEA erstellt wurde, die zugehörige IML-Datei.
2. Auf Stammebene sehen Sie ein **module**-Element wie das folgende:

		<module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">

3. Fügen Sie dem Element `UniqueKey="HDInsightTool"` hinzu, damit das **module**-Element wie folgt aussieht:

		<module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">

4. Speichern Sie die Änderungen. Ihre Anwendung sollte jetzt mit dem HDInsight-Tools-Plug-In kompatibel sein. Sie können dies testen, indem Sie im Project Explorer mit der rechten Maustaste auf den Projektnamen klicken. Im Popupmenü sollte jetzt die Option **Submit Spark Application to HDInsight** enthalten sein.


## Problembehandlung

### Fehler „Bitte verwenden Sie einen größeren Heap“ bei der lokalen Ausführung

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

Die Fehler treten auf, weil der Heap nicht groß genug für die Ausführung von Spark ist, da Spark mindestens 471 MB benötigt (weitere Details finden Sie bei Bedarf unter [SPARK-12081](https://issues.apache.org/jira/browse/SPARK-12081)). Eine einfache Lösung ist die Verwendung eines 64-Bit-Java SDK. Sie können auch die JVM-Einstellungen in IntelliJ ändern, indem Sie die folgenden Optionen hinzufügen:

    -Xms128m -Xmx512m -XX:MaxPermSize=300m -ea

![Ergebnis der lokalen Ausführung der Spark-Anwendung](./media/hdinsight-apache-spark-intellij-tool-plugin/change-heap-size.png)

## Feedbacks und bekannte Probleme

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

* [Remoteausführung von Aufträgen in einem Spark-Cluster mithilfe von Livy](hdinsight-apache-spark-livy-rest-interface.md)

### Tools und Erweiterungen

* [Verwenden von Zeppelin-Notebooks mit einem Spark-Cluster in HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Verfügbare Kernels für Jupyter-Notebooks im Spark-Cluster für HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)

### Verwalten von Ressourcen

* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

<!---HONumber=AcomDC_0525_2016-->