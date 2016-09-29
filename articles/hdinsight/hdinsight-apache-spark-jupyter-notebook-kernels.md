<properties 
	pageTitle="Verfügbare Kernel für Jupyter Notebooks in HDInsight Spark-Clustern unter Linux | Microsoft Azure" 
	description="Hier erhalten Sie Informationen zu zusätzlichen Jupyter Notebook-Kerneln, die in Spark-Clustern unter HDInsight (Linux) zur Verfügung stehen." 
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
	ms.date="08/25/2016" 
	ms.author="nitinme"/>


# Verfügbare Kernels für Jupyter Notebooks mit Apache Spark-Clustern unter HDInsight (Linux)

Apache Spark-Cluster unter HDInsight (Linux) beinhalten Jupyter Notebooks, die Sie zum Testen Ihrer Anwendungen verwenden können. Ein Kernel ist ein Programm, das ausgeführt wird und Ihren Code interpretiert. HDInsight Spark-Cluster bieten zwei Kernel, die Sie für Jupyter Notebook verwenden können. Diese sind wie folgt:

1. **PySpark** (für in Python geschriebene Anwendungen)
2. **Spark** (für in Scala geschriebene Anwendungen)

In diesem Artikel erfahren Sie, wie Sie diese Kernel verwenden und welche Vorteile sie Ihnen bringen.

**Voraussetzungen:**

Sie benötigen Folgendes:

- Ein Azure-Abonnement. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) (in englischer Sprache).
- Einen Apache Spark-Cluster unter HDInsight (Linux). Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## Vorgehensweise zum Verwenden der Kernel 

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) im Startmenü auf die Kachel für Ihren Spark-Cluster (sofern Sie die Kachel ans Startmenü angeheftet haben). Sie können auch unter **Alle durchsuchen** > **HDInsight-Cluster** zu Ihrem Cluster navigieren.

2. Klicken Sie auf dem Blatt für den Spark-Cluster auf **Quicklinks** und anschließend auf dem Blatt **Cluster Dashboard** auf **Jupyter Notebook**. Geben Sie die Administratoranmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden.

	> [AZURE.NOTE] Sie können auch das Jupyter Notebook für Ihren Cluster aufrufen, indem Sie in Ihrem Browser die folgende URL öffnen. Ersetzen Sie __CLUSTERNAME__ durch den Namen Ihres Clusters:
	>
	> `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Erstellen Sie ein neues Notebook mit dem neuen Kernel. Klicken Sie auf **Neu** und dann auf **Pyspark** oder **Spark**. Der Spark-Kernel ist für Scala-Anwendungen vorgesehen, der PySpark-Kernel für Python-Anwendung.

	![Erstellen eines neuen Jupyter Notebooks](./media/hdinsight-apache-spark-jupyter-notebook-kernels/jupyter-kernels.png "Erstellen eines neuen Jupyter Notebooks")

3. Dadurch öffnet sich ein neues Notebook mit dem ausgewählten Kernel.

## Warum sollte ich die PySpark- oder Spark-Kernel verwenden?

Die Verwendung der neuen Kernel bringt einige Vorteile mit sich.

1. **Voreingestellte Kontexte**. Bei Verwendung der **PySpark**- oder **Spark**-Kernel, die mit Jupyter Notebooks verfügbar sind, müssen Sie die Spark- oder Hive-Kontexte nicht mehr explizit festlegen, um mit der Anwendung, die Sie entwickeln, arbeiten zu können. Diese Kontexte stehen standardmäßig zur Verfügung. Diese Kontexte sind:

	* **sc** (Spark-Kontext)
	* **sqlContext** – für Hive-Kontext


	Sie müssen also keine Anweisungen wie die folgenden ausführen, um die Kontexte festzulegen:

		###################################################
		# YOU DO NOT NEED TO RUN THIS WITH THE NEW KERNELS
		###################################################
		sc = SparkContext('yarn-client')
		sqlContext = HiveContext(sc)

	Stattdessen können Sie in Ihrer Anwendung direkt die vordefinierten Kontexte verwenden.
	
2. **Zellen-Magics**. Der PySpark-Kernel bietet einige vordefinierte „Magics“, spezielle Befehle, die Sie mit `%%` (z.B. `%%MAGIC` <args>) aufrufen können. Der Magic-Befehl muss das erste Wort in einer Codezelle sein und ermöglicht mehrere Inhaltszeilen. Das Magic-Wort sollte das erste Wort in der Zelle sein. Beliebige Hinzufügungen vor dem Magic, auch Kommentare, verursachen einen Fehler. Weitere Informationen zu Magics finden Sie [hier](http://ipython.readthedocs.org/en/stable/interactive/magics.html).

	In der folgenden Tabelle sind die verschiedenen über den Kernel verfügbaren Magics aufgeführt.

	| Magic | Beispiel | Beschreibung |
	|-----------|---------------------------------|--------------|
	| help | `%%help` | Generiert eine Tabelle mit allen verfügbaren Magics mit Beispiel und Beschreibung. |
	| info | `%%info` | Gibt Sitzungsinformationen für den aktuellen Livy-Endpunkt heraus. |
	| Konfigurieren | `%%configure -f`<br>`{"executorMemory": "1000M"`,<br>`"executorCores": 4`} | Konfiguriert die Parameter für das Erstellen einer neuen Sitzung. Das Force-Flag (-f) ist obligatorisch, wenn bereits eine Sitzung erstellt wurde, und die Sitzung gelöscht und neu erstellt wird. Unter [„Request Body“ in „POST /sessions“ für Livy](https://github.com/cloudera/livy#request-body) finden Sie eine Liste der gültigen Parameter. Parameter müssen als JSON-Zeichenfolge übergeben werden und in der nächsten Zeile nach dem Magic-Befehl stehen, wie in der Beispielspalte gezeigt. |
	| sql | `%%sql -o <variable name>`<br> `SHOW TABLES` | Führt eine Hive-Abfrage für sqlContext aus. Wenn der Parameter `-o` übergeben wird, wird das Ergebnis der Abfrage im %%local-Python-Kontext als [Pandas](http://pandas.pydata.org/)-Dataframe beibehalten. |
	| local | `%%local`<br>`a=1` | Der gesamte Code in den folgenden Zeilen wird lokal ausgeführt. Der Code muss gültiger Python-Code sein. |
	| Protokolle | `%%logs` | Gibt die Protokolle für die aktuelle Livy-Sitzung aus. |
	| delete | `%%delete -f -s <session number>` | Löscht eine bestimmte Sitzung des aktuellen Livy-Endpunkts. Beachten Sie, dass Sie die Sitzung, die für den Kernel selbst initiiert wird, nicht löschen können. |
	| cleanup | `%%cleanup -f` | Löscht alle Sitzungen für den aktuellen Livy-Endpunkt, einschließlich dieser Notebook-Sitzung. Das Force-Flag „-f“ ist obligatorisch |

	>[AZURE.NOTE] Zusätzlich zu den Magics, die durch den PySpark-Kernel hinzugefügt werden, können Sie auch die [integrierten IPython-Magics](https://ipython.org/ipython-doc/3/interactive/magics.html#cell-magics) verwenden, einschließlich `%%sh`. Sie können das Magic `%%sh` verwenden, um Skripts und Codeblöcke auf dem Clusterhauptknoten auszuführen.

3. **Automatische Visualisierung**. Der **Pyspark**-Kernel visualisiert automatisch die Ausgabe von Hive- und SQL-Abfragen. Sie können zwischen verschiedenen Arten von Visualisierungen wählen, inklusive Tabelle, Kreis-, Linie-, Flächen- und Balkendiagramm.

## Mit %%sql-Magic unterstützte Parameter

%% Sql-Magic unterstützt verschiedene Parameter, mit denen Sie steuern können, welche Art der Ausgabe Sie erhalten, wenn Sie Abfragen ausführen. In der folgenden Tabelle werden die Ausgaben aufgeführt.

| Parameter | Beispiel | Beschreibung |
|-----------|---------------------------------|--------------|
| -o | `-o <VARIABLE NAME>` | Verwenden Sie diesen Parameter, um das Ergebnis der Abfrage im %%local-Python-Kontext als [Pandas](http://pandas.pydata.org/)-Dataframe beizubehalten. Der Name der Datenrahmenvariablen ist der Variablenname, den Sie angeben. |
| -q | `-q` | Verwenden Sie diese Option, um die Visualisierungen für die Zelle auszuschalten. Wenn Sie den Inhalt einer Zelle nicht automatisch visualisieren, sondern einfach als Dataframe erfassen möchten, verwenden Sie `-q -o <VARIABLE>`. Wenn Sie Visualisierungen deaktivieren möchten, ohne die Ergebnisse zu erfassen (z.B. zum Ausführen einer SQL-Abfrage mit Nebeneffekten, wie etwa einer `CREATE TABLE`-Anweisung), verwenden Sie einfach `-q` ohne Angabe eines `-o`-Arguments. |
| -m | `-m <METHOD>` | Dabei ist **METHOD** entweder **take** oder **sample** (der Standardwert ist **take**). Wenn die Methode **take** ist, wählt der Kernel Elemente von der obersten Position des Resultdatasets gemäß MAXROWS (weiter unten in dieser Tabelle beschrieben). Wenn die Methode **sample** ist, wählt der Kernel nach dem Zufallsprinzip Elemente des Datasets gemäß des `-r`-Parameters, der in dieser Tabelle beschrieben wird. |
| -r | `-r <FRACTION>` | Hier ist **FRACTION** eine Gleitkommazahl zwischen 0,0 und 1,0. Wenn die Methode für die SQL-Abfrage `sample` ist, dann wählt der Kernel nach dem Zufallsprinzip den angegebenen Bruchteil der Elemente des Resultsets aus. Wenn Sie z.B. eine SQL-Abfrage mit den Argumenten `-m sample -r 0.01` ausführen, wird 1% der Ergebniszeilen nach dem Zufallsprinzip entnommen. |
| -n | `-n <MAXROWS>` | **MAXROWS** ist ein Ganzzahlwert. Der Kernel schränkt die Anzahl der Ausgabezeilen auf **MAXROWS** ein. Wenn **MAXROWS** eine negative Zahl ist, z.B. **-1**, dann ist die Anzahl der Zeilen im Resultset nicht begrenzt. |

**Beispiel:**

	%%sql -q -m sample -r 0.1 -n 500 -o query2 
	SELECT * FROM hivesampletable

Die obige Anweisung führt Folgendes aus:

* Wählt alle Datensätze aus **hivesampletable** aus.
* Da wir -q verwenden, wird die automatische Visualisierung deaktiviert.
* Da wir `-m sample -r 0.1 -n 500` verwenden, werden nach dem Zufallsprinzip 10% der Zeilen in „hivesampletable“ ausgewählt, und die Größe des Resultsets wird auf 500 Zeilen beschränkt.
* Da wir `-o query2` verwendet haben, wird schließlich auch die Ausgabe in einem Dataframe namens **query2** gespeichert.
	

## Überlegungen bei der Verwendung der neuen Kernel

Unabhängig vom verwendeten Kernel (PySpark oder Spark) werden von ausgeführten Notebooks immer Clusterressourcen beansprucht. Da die Kontexte für diese Kernel voreingestellt sind, werden durch ein einfaches Beenden der Notebooks die Kontexte nicht ebenfalls beendet, sondern beanspruchen weiterhin Clusterressourcen. Bei PySpark- und Spark-Kernels empfiehlt sich daher die Verwendung der Option zum Schließen und Anhalten aus dem Menü **Datei** des Notebooks. Dadurch wird die Beendigung des Kontexts erzwungen und das Notebook beendet.


## Beispiele

Wenn Sie ein Jupyter Notebook öffnen, sind auf der Stammebene zwei Ordner verfügbar.

* Der Ordner **PySpark** enthält Beispiele für Notebooks, die den neuen Kernel vom Typ **Python** verwenden.
* Der Ordner **Scala** enthält Beispiele für Notebooks, die den neuen Kernel vom Typ **Spark** verwenden.

Sie können das Notebook **00 - [READ ME FIRST] Spark Magic Kernel Features** im Ordner **PySpark** oder **Spark** öffnen, um Informationen zu den verschiedenen verfügbaren Magics zu erhalten. Sie können auch anhand der anderen in den beiden Ordnern verfügbaren Beispiel-Notebooks erfahren, wie Sie verschiedene Szenarien der Verwendung von Jupyter-Notebooks mit HDInsight Spark-Clustern realisieren können.

## Wo werden die Notebooks gespeichert?

Jupyter Notebooks werden in dem Speicherkonto gespeichert, das mit dem Cluster unter dem Ordner **/HdiNotebooks** verknüpft ist. Der Zugriff auf Notebooks, Textdateien und Ordner, die Sie in Jupyter erstellen, erfolgt von WASB aus. Wenn Sie z.B. Jupyter verwenden, um einen Ordner **myfolder** und ein Notebook **myfolder/mynotebook.ipynb** zu erstellen, können Sie auf dieses Notebook unter `wasbs:///HdiNotebooks/myfolder/mynotebook.ipynb` zugreifen. Das gilt auch umgekehrt, d.h. wenn Sie ein Notebook direkt in Ihr Speicherkonto unter `/HdiNotebooks/mynotebook1.ipynb` hochladen, ist das Notebook auch von Jupyter aus sichtbar. Die Notebooks verbleiben im Speicherkonto, auch nachdem der Cluster gelöscht wurde.

Die Art, in der Notebooks im Speicherkonto gespeichert werden, ist mit HDFS kompatibel. Wenn Sie also eine SSH-Verbindung mit dem Cluster herstellen, können Sie Dateiverwaltungsbefehle wie die folgenden verwenden:

	hdfs dfs -ls /HdiNotebooks             				  # List everything at the root directory – everything in this directory is visible to Jupyter from the home page
	hdfs dfs –copyToLocal /HdiNotebooks    				# Download the contents of the HdiNotebooks folder
	hdfs dfs –copyFromLocal example.ipynb /HdiNotebooks   # Upload a notebook example.ipynb to the root folder so it’s visible from Jupyter


Falls Probleme beim Zugriff auf das Speicherkonto für den Cluster auftreten, werden die Notebooks ebenfalls auf dem Hauptknoten `/var/lib/jupyter` gespeichert.

## Unterstützte Browser
Für HDInsight Spark-Cluster ausgeführte Jupyter Notebooks werden nur von Google Chrome unterstützt.

## Feedback

Die neuen Kernels befinden sich in der Entwicklungsphase und werden mit der Zeit ausreifen. Im Zuge dieser Entwicklung ändern sich unter Umständen auch APIs. Wir freuen uns über Ihr Feedback zur Verwendung der neuen Kernel. Dies hilft uns bei der Gestaltung der endgültigen Kernelversion. Kommentare/Feedback können Sie im Kommentarbereich am Ende dieses Artikels hinterlassen.


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

* [Verwenden des HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Erstellen und Übermitteln von Spark Scala-Anwendungen](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely (Verwenden von HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Remotedebuggen von Spark-Anwendungen)](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Verwenden von Zeppelin-Notebooks mit einem Spark-Cluster in HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Verwenden von externen Paketen mit Jupyter Notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### Verwalten von Ressourcen

* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Track and debug jobs running on an Apache Spark cluster in HDInsight (Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](hdinsight-apache-spark-job-debugging.md)

<!---HONumber=AcomDC_0914_2016-->