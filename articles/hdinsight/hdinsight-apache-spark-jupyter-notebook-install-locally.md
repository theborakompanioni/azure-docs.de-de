<properties 
	pageTitle="Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zu einem HDInsight-Spark-Cluster (Vorschau) | Microsoft Azure" 
	description="Erfahren Sie, wie Sie Jupyter Notebook auf Ihrem Computer installieren und eine Verbindung zum Apache Spark-Cluster in Azure HDInsight herstellen." 
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


# Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung mit dem Apache Spark-Cluster in HDInsight (Linux)

In diesem Artikel erfahren Sie, wie Sie Jupyter Notebook mit den benutzerdefinierten Kerneln PySpark (für Python) und Spark (für Scala) mit Spark Magic installieren und das Notebook mit einem HDInsight-Cluster verbinden. Es kann eine Reihe von Gründen geben, Jupyter auf dem lokalen Computer zu installieren, und es auch können auch einige Probleme auftreten. Eine Liste der Gründe und Probleme finden Sie im Abschnitt [Warum sollte ich Jupyter auf meinem Computer installieren](#why-should-i-install-jupyter-on-my-computer) am Ende dieses Artikels.

Die Installation von Jupyter und Spark Magic auf Ihrem Computer umfasst drei Hauptschritte.

* Installieren von Jupyter Notebook
* Installieren der PySpark- und Spark-Kernel mit Spark Magic
* Konfigurieren von Spark Magic für den Zugriff auf den Spark-Cluster in HDInsight

Weitere Informationen zu den benutzerdefinierten Kerneln und Spark Magic für Jupyter-Notebooks mit HDInsight-Clustern finden Sie unter [Verfügbare Kernels für Jupyter-Notebooks mit Apache Spark-Linux-Clustern in HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).

##Voraussetzungen

Die hier aufgeführten Voraussetzungen gelten nicht für die Installation von Jupyter. Sie gelten für die Herstellung einer Verbindung von einem Jupyter Notebook zu einem HDInsight-Cluster, sobald das Notebook installiert wurde.

- Ein Azure-Abonnement. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) (in englischer Sprache).
- Einen Apache Spark-Cluster unter HDInsight (Linux). Anleitungen finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## Installieren von Jupyter Notebook auf Ihrem Computer

Sie müssen Python installieren, bevor Sie Jupyter Notebooks installieren können. Python und Jupyter stehen als Bestandteile der [Anaconda-Verteilung](https://www.continuum.io/downloads) zur Verfügung. Wenn Sie Anaconda installieren, installieren Sie eigentlich eine Python-Verteilung. Nachdem Sie Anaconda installiert haben, können Sie die Jupyter-Installation über einen Befehl hinzufügen. In diesem Abschnitt werden die Schritte erläutert, die Sie befolgen müssen.

1. Laden Sie das [Anaconda-Installationsprogramm](https://www.continuum.io/downloads) für Ihre Plattform herunter, und führen Sie das Setup aus. Stellen Sie sicher, dass während der Ausführung des Setup-Assistenten die Option für das Hinzufügen von Anaconda zu Ihrer PATH-Variablen aktiviert ist.

2. Führen Sie den folgenden Befehl aus, um Jupyter zu installieren.

		conda install jupyter

	Weitere Informationen zur Installation von Jupyter finden Sie unter [Installing Jupyter using Anaconda](http://jupyter.readthedocs.io/en/latest/install.html) (Installieren von Jupyter mit Anaconda).

## Installieren der Kernel und Spark Magic

In diesem Abschnitt lernen Sie, die Kernel für Spark Magic, PySpark und Spark zu installieren. Anschließend konfigurieren Sie die Kernel für die Verbindung mit einem Apache Spark-Cluster, der in Azure HDInsight ausgeführt wird.

1. Laden Sie die aktuelle öffentliche Vorschau von Spark Magic von [GitHub](https://github.com/jupyter-incubator/sparkmagic/archive/publicpreview0.5.zip) herunter.

2. Entpacken Sie die heruntergeladene Datei an einem Speicherort auf dem Datenträger. In dieser Anleitung nennen wir diesen Pfad `$SPARKMAGIC_PATH`.

2. Führen Sie den folgenden Befehl aus

		pip install -r $SPARKMAGIC_PATH/requirements.txt  

3. Führen Sie den folgenden Befehl aus, um Spark Magic zu installieren.

		pip install -e $SPARKMAGIC_PATH

4. Installieren der PySpark- und Spark-Kernel Führen Sie die folgenden Befehle aus:

		jupyter-kernelspec install $SPARKMAGIC_PATH/remotespark/kernels/sparkkernel
		jupyter-kernelspec install $SPARKMAGIC_PATH/remotespark/kernels/pysparkkernel

## Konfigurieren von Spark Magic für den Zugriff auf den Spark-Cluster in HDInsight

In diesem Abschnitt lernen Sie, Spark Magic zu konfigurieren, nachdem Sie es installiert haben, um eine Verbindung mit einem Apache Spark-Cluster herzustellen. Dieser muss bereits in Azure HDInsight erstellt worden sein.

1. Die Konfigurationsinformationen für Jupyter werden in der Regel im Basisverzeichnis des Benutzers gespeichert. Geben Sie die folgenden Befehle ein, um das Basisverzeichnis auf allen Plattformen des Betriebssystems ausfindig zu machen.

	Starten Sie die Python-Shell. Geben Sie in einem Befehlsfenster den folgenden Befehl ein:

		python

	Geben Sie in der Python-Shell den folgenden Befehl ein, um das Basisverzeichnis ausfindig zu machen.

		import os
		print os.path.expanduser('~')

2. Navigieren Sie zum Stammverzeichnis, und erstellen Sie einen Ordner namens **.sparkmagic**, sofern er nicht bereits vorhanden ist.

3. Erstellen Sie im Ordner eine Datei namens **config.json**, und fügen Sie den folgenden JSON-Codeausschnitt in den Ordner ein.

		{
		  "kernel_python_credentials" : {
		    "username": "{USERNAME}",
		    "base64_password": "{BASE64ENCODEDPASSWORD}",
		    "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
		  },
		  "kernel_scala_credentials" : {
		    "username": "{USERNAME}",
		    " base64_password ": "{BASE64ENCODEDPASSWORD}",
		    "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
		  }
		}

4. Ersetzen Sie **{USERNAME}**, **{CLUSTERDNSNAME}**, und **{BASE64ENCODEDPASSWORD}** durch die entsprechenden Werte. Sie können eine Reihe von Dienstprogrammen in Ihrer bevorzugten Programmiersprache oder online nutzen, um ein base64-codiertes Kennwort für Ihr eigentliches Kennwort zu generieren. Dies wäre ein einfacher Python-Ausschnitt, den Sie über die Befehlszeile ausführen können:

		python -c "import base64; print(base64.b64encode('{YOURPASSWORD}'))"

5. Starten Sie Jupyter. Geben Sie in der Eingabeaufforderung folgenden Befehl ein:

		jupyter notebook

6. Stellen Sie sicher, dass Sie mit dem Jupyter Notebook eine Verbindung zu dem Cluster herstellen können und dass Sie die verfügbare Spark Magic mit den Kerneln verwenden können. Führen Sie die folgenden Schritte aus:

	1. Erstellen Sie ein neues Notebook. Klicken Sie in der rechten Ecke auf **Neu**. Daraufhin sollten der Standardkernel **Python2** und die zwei neu installierten Kernel **PySpark** und **Spark** angezeigt werden.

		![Erstellen eines neuen Jupyter Notebooks](./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Erstellen eines neuen Jupyter Notebooks")

	
		Klicken Sie auf **PySpark**.


	2. Führen Sie den folgenden Codeausschnitt aus.

			%%sql
			SELECT * FROM hivesampletable LIMIT 5

		Wenn Sie die Ausgabe erfolgreich abrufen können, wird Ihre Verbindung zu dem HDInsight-Cluster getestet.

	>[AZURE.TIP] Wenn Sie die Notebookkonfiguration für die Verbindung zu einem anderen Cluster aktualisieren möchten, aktualisieren Sie die Datei „config.json“ mit einem neuen Satz von Werten, wie in Schritt 3 oben dargestellt.

## Warum sollte ich Jupyter auf meinem Computer installieren?

Es kann zahlreiche Gründe geben, warum Sie Jupyter auf dem Computer installieren und mit einem Spark-Cluster in HDInsight verbinden möchten.

* Obwohl die Jupyter Notebooks bereits im Spark-Cluster in Azure HDInsight verfügbar sind, bietet Ihnen die Installation von Jupyter auf Ihrem Computer die Option, Ihre Notebooks lokal zu erstellen, Ihre Anwendung in einem aktiven Cluster zu testen und die Notebooks anschließend in den Cluster hochzuladen. Sie können die Notebooks entweder mithilfe des im Cluster ausgeführten Jupyter Notebooks hochladen oder sie im Ordner „/HdiNotebooks“ in dem dem Cluster zugeordneten Speicherkonto speichern. Weitere Informationen zum Speichern von Notebooks im Cluster finden Sie unter [Wo werden die Notebooks gespeichert?](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)
* Wenn Notebooks lokal verfügbar sind, können Sie basierend auf den Anwendungsanforderungen Verbindungen mit unterschiedlichen Spark-Clustern herstellen.
* Sie können GitHub verwenden, um ein Quellcodeverwaltungssystem zu implementieren und Versionskontrolle für die Notebooks zu ermöglichen. Sie können auch eine Umgebung für Zusammenarbeit einrichten, in der mehrere Benutzer mit dem gleichen Notebook arbeiten können.
* Sie können mit Notebooks lokal ohne einen Cluster arbeiten. Sie benötigen einen Cluster nur zum Testen der Notebooks, nicht für das manuelle Verwalten von Notebooks oder einer Entwicklungsumgebung.
* Es ist möglicherweise einfacher, Ihre eigene lokale Entwicklungsumgebung zu konfigurieren, statt die Jupyter-Installation im Cluster zu konfigurieren. Sie können sämtliche Software nutzen, die Sie lokal installiert haben, ohne Remotecluster zu konfigurieren.

>[AZURE.WARNING] Wenn Jupyter auf dem lokalen Computer installiert ist, können mehrere Benutzer gleichzeitig das gleiche Notebook im gleichen Spark-Cluster ausführen. In diesem Fall werden mehrere Livy-Sitzungen erstellt. Wenn ein Problem auftritt, das Sie debuggen möchten, ist es eine komplexe Aufgabe nachzuverfolgen, welche Livy-Sitzung welchem Benutzer gehört.




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

* [Verfügbare Kernels für Jupyter-Notebook im Spark-Cluster für HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Verwenden von externen Paketen mit Jupyter Notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

### Verwalten von Ressourcen

* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Track and debug jobs running on an Apache Spark cluster in HDInsight (Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](hdinsight-apache-spark-job-debugging.md)

<!---HONumber=AcomDC_0914_2016-->