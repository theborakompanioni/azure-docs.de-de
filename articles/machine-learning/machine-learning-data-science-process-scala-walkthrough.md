<properties
	pageTitle="Data Science unter Verwendung von Scala mit Spark in Azure | Microsoft Azure"
	description="Die Verwendung von Scala für überwachte Machine Learning-Aufgaben mit der skalierbaren Machine Learning-Bibliothek (MLlib) von Spark und Spark ML-Paketen auf einem Azure HDInsight Spark-Cluster."  
	services="machine-learning"
	documentationCenter=""
	authors="bradsev,deguhath"
	manager="paulettm"
	editor="cgronlun" />

<tags
	ms.service="machine-learning"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/01/2016"
	ms.author="bradsev;"/>


# Data Science unter Verwendung von Scala mit Spark in Azure

Dieses Thema zeigt die Verwendung von Scala für überwachte Machine Learning-Aufgaben mit der skalierbaren Machine Learning-Bibliothek (MLlib) von Spark und Spark ML-Paketen auf einem Azure HDInsight Spark-Cluster. Es führt Sie durch die Aufgaben, aus denen der [Data Science-Process](http://aka.ms/datascienceprocess) besteht: Erfassen und Durchsuchen von Daten , Visualisierung, Feature Engineering, Modellierung und Modellnutzung. Die erstellten Modelle umfassen logistische und lineare Regression, zufällige Gesamtstrukturen und Gradient-Boosted-Strukturen. Wir behandeln zwei allgemeine überwachte Machine Learning-Aufgaben:

- Regressionsproblem: Vorhersage des Trinkgeldbetrags ($)
- Binäre Klassifizierung: Vorhersage für eine Taxifahrt, ob Trinkgeld gegeben wird oder nicht (1/0)

Die Modellierung erfordert Training und Auswertung von Testdatasets mit relevanten Genauigkeitsmetriken. Wir zeigen auch, wie diese Modelle in Azure-Blobspeicher (WASB) gespeichert werden, und wie ihre Vorhersageleistung bewertet und ausgewertet wird. Ein fortgeschritteneres behandeltes Thema ist das Optimieren von Modellen mithilfe übergreifender Validierung und Hyperparameter-Sweeping. Die verwendeten Daten sind eine Stichprobe des Datasets für Taxifahrten und Fahrpreise in New York aus dem Jahr 2013.

[Scala](http://www.scala-lang.org/) ist eine Sprache auf der Basis von Java Virtual Machine, die objektorientierte und funktionale Sprachkonzepte integriert. Es ist eine skalierbare Sprache, die sich gut für die verteilte Verarbeitung in der Cloud eignet und auf Azure Spark-Clustern ausgeführt wird.

[Spark](http://spark.apache.org/) ist ein Open-Source-Framework für die Parallelverarbeitung, das die In-Memory-Verarbeitung unterstützt, um die Leistung von Anwendungen zur Analyse von Big Data zu steigern. Das Spark-Verarbeitungsmodul ist auf Geschwindigkeit, einfache Nutzung und anspruchsvolle Analysen ausgelegt. Dank seiner verteilten In-Memory-Datenverarbeitungsfunktionen eignet sich Spark besonders für iterative Algorithmen bei Machine Learning und für Graphberechnungen. Das [spark.ml](http://spark.apache.org/docs/latest/ml-guide.html)-Paket bietet eine einheitliche Gruppe von auf Dataframes aufgesetzten High-Level-APIs, mit deren Hilfe Benutzer praktische Machine Learning-Pipelines erstellen und optimieren können. [MLlib](http://spark.apache.org/mllib/) ist die skalierbare Machine Learning-Bibliothek von Spark, die Modellierungsfunktionen in diese verteilte Umgebung einbringt.

[HDInsight Spark](../hdinsight/hdinsight-apache-spark-overview.md) ist das in Azure gehostete Angebot für Open-Source-Spark. Darüber hinaus unterstützt der Spark-Cluster, der interaktive Spark-SQL-Abfragen zum Transformieren, Filtern und Visualisieren von Daten, die in Azure-Blobs (WASB) gespeichert sind, ausführen kann, auch **Jupyter Scala-Notebooks**. Die Scala-Codeausschnitte, die die Lösungen bereitstellen und die relevanten Plots zum Visualisieren der Daten zeigen, werden in Jupyter-Notebooks in den Spark-Clustern ausgeführt. Die Modellierungsschritte in diesen Themen enthalten auch Code zum Trainieren, Evaluieren, Speichern und Nutzen jedes Modelltyps.

Installationsschritte und Code in dieser exemplarischen Vorgehensweise beziehen sich auf HDInsight 3.4 Spark 1.6. Der hier und im [Jupyter Scala-Notebook](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration%20Modeling%20and%20Scoring%20using%20Scala.ipynb) für diese exemplarische Vorgehensweise vorhandene Code ist jedoch generisch und sollte auf jedem Spark-Cluster funktionieren. Clustereinrichtung und Verwaltungsschritte weichen möglicherweise geringfügig von dem ab, was hier gezeigt wird, wenn Sie nicht HDInsight Spark verwenden.

> [AZURE.NOTE] Ein Thema, das die Verwendung von Python statt Scala zur Ausführung von Aufgaben für einen End-to-End-Data Science-Process veranschaulicht, ist [Übersicht über Data Science mit Spark in Azure HDInsight](machine-learning-data-science-spark-overview.md).


## Voraussetzungen

1\. Bevor Sie mit diesen Themen beginnen, müssen Sie über ein Azure-Abonnement verfügen. Wenn Sie noch keines haben, sehen Sie sich [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) (Erhalten einer kostenlosen Azure-Testversion zum Testen von Hadoop in HDInsight) an.

2\. Sie benötigen einen HDInsight 3.4 Spark 1.6-Cluster zum Durchführen dieser exemplarischen Vorgehensweise. Anweisungen zum Erstellen eines solchen Clusters finden Sie unter [Erste Schritte: Erstellen von Apache Spark in Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Clustertyp und Version werden im Menü **Clustertyp auswählen** angegeben.

![](./media/machine-learning-data-science-process-scala-walkthrough/spark-cluster-on-portal.png)


>[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Eine Beschreibung der NYC-Taxidaten und Anweisungen zum Ausführen von Code aus einem Jupyter-Notebook im Spark-Cluster finden Sie in den entsprechenden Abschnitten von [Übersicht zu Data Science unter Verwendung von Spark unter Azure HDInsight](machine-learning-data-science-spark-overview.md).


## Ausführen von Scala-Code über ein Jupyter-Notebook auf dem Spark-Cluster 

Sie können das Jupyter-Notebook über das Azure-Portal starten. Suchen Sie Ihren Spark-Cluster auf dem Dashboard, und klicken Sie darauf, um zur Verwaltungsseite für Ihren Cluster zu gelangen. Klicken Sie dann auf **Cluster-Dashboards** > **Jupyter-Notebook**, um das dem Spark-Cluster zugeordnete Notebook zu öffnen.

![](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-on-portal.png)

Sie können auch zu ***https://CLUSTERNAME.azurehdinsight.net/jupyter*** navigieren, um auf die Jupyter-Notebooks zuzugreifen. Ersetzen Sie einfach das Element „CLUSTERNAME“ in dieser URL durch den Namen Ihres eigenen Clusters. Sie benötigen das Kennwort für Ihr Administratorkonto, um auf die Notebooks zuzugreifen.

![](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-notebook.png)

Wählen Sie Scala aus, um ein Verzeichnis mit einigen Beispielen für vorkonfigurierte Notebooks anzuzeigen, die die PySpark-API verwenden. Das Notebook **Exploration Modeling and Scoring using Scala.ipynb** mit den Codebeispielen für diese Suite von Spark-Themen finden Sie bei [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Scala).


Sie können das Notebook direkt von Github auf den Jupyter-Notebook-Server in Ihrem Spark-Cluster hochladen. Klicken Sie auf der Homepage Ihres Jupyter im rechten Teil des Bildschirms auf die Schaltfläche **Hochladen**. Ein Datei-Explorer wird geöffnet. Hier können Sie die GitHub-URL (Rohdateninhalt) des Scala-Notebooks einfügen und auf **Öffnen** klicken. Das Scala-Notebook ist unter folgender URL verfügbar:

[Exploration-Modeling-and-Scoring-using-Scala.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration-Modeling-and-Scoring-using-Scala.ipynb)


## Setup: voreingestellte Kontexte, Spark-Magics und Bibliotheken

### Voreingestellter Spark- und Hive-Kontext

	# SET START TIME
	import java.util.Calendar
	val beginningTime = Calendar.getInstance().getTime()


Die Spark-Kernel, die mit Jupyter Notebooks bereitgestellt werden, verfügen über einen vorab festgelegten Kontext. Sie müssen die Spark- oder Hive-Kontexte also nicht mehr explizit festlegen, um mit der Anwendung, die Sie entwickeln, arbeiten zu können. Diese Kontexte stehen standardmäßig zur Verfügung. Diese Kontexte sind:

- sc – für Spark-Kontext
- sqlContext – für Hive-Kontext


### Spark-Magics

Der Spark-Kernel bietet einige so genannte „Magic-Befehle“, die vordefiniert sind. Dies sind spezielle Befehle, die Sie mit %% aufrufen können. Es gibt zwei Befehle dieser Art, die in den Codebeispielen verwendet werden.

- **%%local** gibt an, dass der Code in den nachfolgenden Zeilen lokal ausgeführt wird. Der Code muss gültiger Scala-Code sein.
- **%%sql -o <Variablenname>** führt eine Hive-Abfrage für den sqlContext aus. Wenn der Parameter -o übergeben wird, wird das Ergebnis der Abfrage im %%local-Scala-Kontext als Spark-Dataframe beibehalten.

Weitere Informationen zu den Kernels für Jupyter-Notebooks und den zugehörigen vordefinierten „Magics“, die mit %% aufgerufen werden (z.B. „%%local“), finden Sie unter [Verfügbare Kernels für Jupyter-Notebooks mit HDInsight Spark-Linux-Clustern in HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


### Importieren von Bibliotheken

Importieren Sie die Spark-, MLlib- und anderen erforderlichen Bibliotheken mit folgendem Code.

	# IMPORT SPARK AND JAVA LIBRARIES 
	import org.apache.spark.sql.SQLContext
	import org.apache.spark.sql.functions._
	import java.text.SimpleDateFormat
	import java.util.Calendar
	import sqlContext.implicits._
	import org.apache.spark.sql.Row
	
	# SPARK SQL FUNCTIONS
	import org.apache.spark.sql.types.{StructType, StructField, StringType, IntegerType, FloatType, DoubleType}
	import org.apache.spark.sql.functions.rand
	
	# SPARK ML FUNCTIONS
	import org.apache.spark.ml.Pipeline
	import org.apache.spark.ml.feature.{StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer, Binarizer}
	import org.apache.spark.ml.tuning.{ParamGridBuilder, TrainValidationSplit, CrossValidator}
	import org.apache.spark.ml.regression.{LinearRegression, LinearRegressionModel, RandomForestRegressor, RandomForestRegressionModel, GBTRegressor, GBTRegressionModel}
	import org.apache.spark.ml.classification.{LogisticRegression, LogisticRegressionModel, RandomForestClassifier, RandomForestClassificationModel, GBTClassifier, GBTClassificationModel}
	import org.apache.spark.ml.evaluation.{BinaryClassificationEvaluator, RegressionEvaluator, MulticlassClassificationEvaluator}
	
	# SPARK MLLIB FUNCTIONS
	import org.apache.spark.mllib.linalg.{Vector, Vectors}
	import org.apache.spark.mllib.util.MLUtils
	import org.apache.spark.mllib.classification.{LogisticRegressionWithLBFGS, LogisticRegressionModel}
	import org.apache.spark.mllib.regression.{LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel}
	import org.apache.spark.mllib.tree.{GradientBoostedTrees, RandomForest}
	import org.apache.spark.mllib.tree.configuration.BoostingStrategy
	import org.apache.spark.mllib.tree.model.{GradientBoostedTreesModel, RandomForestModel, Predict}
	import org.apache.spark.mllib.evaluation.{BinaryClassificationMetrics, MulticlassMetrics, RegressionMetrics}
	
	# SPECIFY SQL CONTEXT
	val sqlContext = new SQLContext(sc)


## Datenerfassung
 
Der erste Schritt im Data Science-Prozess ist das Erfassen der zu analysierenden Daten aus externen Quellen oder Systemen in die Umgebung, die Sie zum Durchsuchen und Modellieren von Daten nutzen. In dieser exemplarischen Vorgehensweise lesen wir in einem verknüpften 0,1%-Beispiel der Datei für Taxifahrten- und Fahrpreise (gespeichert als TSV-Datei). Die Umgebung zum Durchsuchen und Modellieren der Daten ist Spark. Dieser Abschnitt enthält den Code zur Ausführung der folgenden Reihe von Aufgaben:

- Festlegen der Verzeichnispfade für Daten- und Modellspeicherung
- Einlesen des Eingabedatasets (gespeichert als TSV-Datei)
- Definieren eines Schemas für die Daten und Bereinigen der Daten
- Erstellen eines bereinigten Dataframes, dessen Zwischenspeicherung im Arbeitsspeicher
- dessen Registrierung als temporäre Tabelle im SQL-Kontext
- Abfragen der Tabelle und Importieren der Ergebnisse in einen Dataframe.


### Festlegen von Verzeichnispfaden für Speicherorte im WASB

Spark kann aus dem Azure Storage-Blob (auch bekannt als WASB) lesen und darin schreiben. Es können also alle Ihre vorhandenen Daten mithilfe von Spark verarbeitet und die Ergebnisse wieder im WASB gespeichert werden.

Um Modelle oder Dateien im WASB zu speichern, muss der Pfad korrekt angegeben werden. Auf den an den Spark-Cluster angefügten Standardcontainer können Sie mit einem Pfad verweisen, der mit „wasb:///“ beginnt. Auf andere Speicherorte wird mit „wasb://“ verwiesen.

Das folgende Codebeispiel gibt den Speicherort der zu lesenden Eingabedaten und den Pfad zum Azure-Blob an, das dem Spark-Cluster zugeordnet ist, in dem das Modell gespeichert wird.

	# SET PATHS TO DATA AND MODEL FILE LOCATIONS: 
	# INGEST DATA DATA AND SPECIFY HEADERS FOR COLUMNS
	val taxi_train_file = sc.textFile("wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv")
	val header = taxi_train_file.first;
	
	# SET THE MODEL STORAGE DIRECTORY PATH 
	# NOTE THAT THE FINAL BACKSLASH IN THE PATH IS NEEDED.
	val modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";


### Importieren von Daten, Erstellen von RDDs (Resilient Distributed Datasets) und Definieren eines Dataframes gemäß Schema 

	# RECORD START TIME
	val starttime = Calendar.getInstance().getTime()
	
	# DEFINE SCHEMA BASED ON HEADER OF THE FILE
	val sqlContext = new SQLContext(sc)
	val taxi_schema = StructType(
	    Array(
	        StructField("medallion", StringType, true), 
	        StructField("hack_license", StringType, true),
	        StructField("vendor_id", StringType, true), 
	        StructField("rate_code", DoubleType, true),
	        StructField("store_and_fwd_flag", StringType, true),
	        StructField("pickup_datetime", StringType, true),
	        StructField("dropoff_datetime", StringType, true),
	        StructField("pickup_hour", DoubleType, true),
	        StructField("pickup_week", DoubleType, true),
	        StructField("weekday", DoubleType, true),
	        StructField("passenger_count", DoubleType, true),
	        StructField("trip_time_in_secs", DoubleType, true),
	        StructField("trip_distance", DoubleType, true),
	        StructField("pickup_longitude", DoubleType, true),
	        StructField("pickup_latitude", DoubleType, true),
	        StructField("dropoff_longitude", DoubleType, true),
	        StructField("dropoff_latitude", DoubleType, true),
	        StructField("direct_distance", StringType, true),
	        StructField("payment_type", StringType, true),
	        StructField("fare_amount", DoubleType, true),
	        StructField("surcharge", DoubleType, true),
	        StructField("mta_tax", DoubleType, true),
	        StructField("tip_amount", DoubleType, true),
	        StructField("tolls_amount", DoubleType, true),
	        StructField("total_amount", DoubleType, true),
	        StructField("tipped", DoubleType, true),
	        StructField("tip_class", DoubleType, true)
	        )
	    )
	
	# CAST VARIABLES ACCORDING TO SCHEMA
	val taxi_temp = (taxi_train_file.map(_.split("\t"))
	                        .filter((r) => r(0) != "medallion")
	                        .map(p => Row(p(0), p(1), p(2),
	                            p(3).toDouble, p(4), p(5), p(6), p(7).toDouble, p(8).toDouble, p(9).toDouble, p(10).toDouble,
	                            p(11).toDouble, p(12).toDouble, p(13).toDouble, p(14).toDouble, p(15).toDouble, p(16).toDouble,
	                            p(17), p(18), p(19).toDouble, p(20).toDouble, p(21).toDouble, p(22).toDouble,
	                            p(23).toDouble, p(24).toDouble, p(25).toDouble, p(26).toDouble)))
	
	
	# CREATE INITIAL DATA-FRAME, DROP COLUMNS, AND CREATE CLEANED DATA-FRAME BY FILTERING FOR UNDESIRED VALUES OR OUTLIERS
	val taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)
	
	val taxi_df_train_cleaned = (taxi_train_df.drop(taxi_train_df.col("medallion"))
	        .drop(taxi_train_df.col("hack_license")).drop(taxi_train_df.col("store_and_fwd_flag"))
	        .drop(taxi_train_df.col("pickup_datetime")).drop(taxi_train_df.col("dropoff_datetime"))
	        .drop(taxi_train_df.col("pickup_longitude")).drop(taxi_train_df.col("pickup_latitude"))
	        .drop(taxi_train_df.col("dropoff_longitude")).drop(taxi_train_df.col("dropoff_latitude"))
	        .drop(taxi_train_df.col("surcharge")).drop(taxi_train_df.col("mta_tax"))
	        .drop(taxi_train_df.col("direct_distance")).drop(taxi_train_df.col("tolls_amount"))
	        .drop(taxi_train_df.col("total_amount")).drop(taxi_train_df.col("tip_class"))
	        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200"));
	
	# CACHE AND MATERIALIZE CLEANED DATA-FRAME IN MEMORY
	taxi_df_train_cleaned.cache()
	taxi_df_train_cleaned.count()
	
	# REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
	taxi_df_train_cleaned.registerTempTable("taxi_train")
	
	# GET TIME TO RUN THE CELL
	val endtime = Calendar.getInstance().getTime()
	val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
	println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**AUSGABE:**

Für die Ausführung der obigen Zelle benötigte Zeit: 8 Sekunden.


### Tabelle abfragen und Ergebnisse in einen Dataframe importieren. 

Tabelle nach Fahrpreis-, Fahrgast- und Trinkgelddaten abfragen, beschädigte und außerhalb liegende Daten filtern und mehrere Zeilen drucken.

	# QUERY THE DATA
	val sqlStatement = """
	    SELECT fare_amount, passenger_count, tip_amount, tipped
	    FROM taxi_train 
	    WHERE passenger_count > 0 AND passenger_count < 7
	    AND fare_amount > 0 AND fare_amount < 200
	    AND payment_type in ('CSH', 'CRD')
	    AND tip_amount > 0 AND tip_amount < 25
	"""
	val sqlResultsDF = sqlContext.sql(sqlStatement)
	
	# SHOW ONLY THE TOP THREE ROWS
	sqlResultsDF.show(3)

**AUSGABE:**

fare\_amount|passenger\_count|tip\_amount|tipped
-----------|---------------|----------|------
 13,5| 1,0| 2,9| 1,0
 16,0| 2,0| 3\.4| 1,0
 10,5| 2,0| 1,0| 1,0


## Durchsuchen und Visualisieren von Daten 

Nachdem die Daten in Spark eingegeben wurden, besteht der nächste Schritt im Data Science-Prozess darin, durch Durchsuchen und Visualisieren eine tiefer gehende Einsicht in die Daten zu erhalten. In diesem Abschnitt untersuchen wir die Taxidaten, verwenden SQL-Abfragen und importieren dann die Ergebnisse in einen Dataframe, um die Zielvariablen und künftigen Features für die Sichtprüfung mithilfe des Jupyter-Features zur automatischen Visualisierung zu zeichnen.

### Verwenden von lokalen und SQL-Magic-Befehlen, um Daten zu zeichnen

Standardmäßig ist die Ausgabe von allen Codeausschnitten, die Sie über ein Jupyter-Notebook ausführen, innerhalb des Kontexts der Sitzung verfügbar, die auf Workerknoten beibehalten wird. Wenn Sie eine Fahrt für jede Berechnung auf den Workerknoten speichern möchten, und wenn alle Daten, die Sie zur Berechnung benötigen, lokal auf dem Jupyter-Serverknoten (dem Hauptknoten) verfügbar sind, können Sie den Codeausschnitt mit dem Magic-Befehl %%local auf dem Jupyter-Server ausführen.

- **SQL-Magic (`%%sql`)** Der HDInsight Spark-Kernel unterstützt einfache HiveQL-Inlineabfragen für den sqlContext. Mit dem Argument (-o VARIABLE\_NAME) wird die Ausgabe der SQL-Abfrage als Pandas-Dataframe auf dem Jupyter-Server beibehalten. Das bedeutet, dass die Daten im lokalen Modus verfügbar sind.
- Der **Magic-Befehl `%%local`** wird genutzt, um Code lokal auf dem Jupyter-Server auszuführen. Dieser ist der Hauptknoten des HDInsight-Clusters. Normalerweise verwenden Sie den Magic-Befehl `%%local` zusammen mit dem Magic-Befehl `%%sql` mit dem Parameter -o. Mit dem Parameter -o wird die Ausgabe der SQL-Abfrage lokal beibehalten. Anschließend löst der Magic-Befehl %%local die nächste Gruppe von Codeausschnitten aus, damit diese lokal für die Ausgabe der lokal gespeicherten SQL-Abfragen ausgeführt werden können.

### Abfragen der Daten mit SQL
Diese Abfrage ruft die Fahrten nach Höhe des Fahrpreises, Anzahl der Fahrgäste und Trinkgeldbetrag ab.

	# SQL QUERY
	%%sql -q -o sqlResults
	SELECT fare_amount, passenger_count, tip_amount, tipped FROM taxi_train WHERE passenger_count > 0 AND passenger_count < 7 AND fare_amount > 0 AND fare_amount < 200 AND payment_type in ('CSH', 'CRD') AND tip_amount > 0 AND tip_amount < 25

Im folgenden Code erstellt der Magic-Befehl %%local einen lokalen Dataframe, sqlResults, der zum Zeichnen mit matplotlib verwendet werden kann.

> [AZURE.TIP] Der Magic-Befehl %%local wird in dieser exemplarischen Vorgehensweise mehrfach genutzt. Bei einer großen Datenmenge sollten Sie Stichproben verwenden, um einen Dataframe zu erstellen, der nicht zu groß für den lokalen Arbeitsspeicher ist.

### Zeichnen der Daten

Zeichnen mit Python-Code ist möglich, sobald der Dataframe im lokalen Kontext als Pandas-Dataframe vorhanden ist.

	# RUN THE CODE LOCALLY ON THE JUPYTER SERVER
	%%local
	
	# USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
	# CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
	sqlResults


 Der Spark-Kernel visualisiert automatisch die Ausgabe der SQL-Abfragen (HiveQL), nachdem Sie den Code ausgeführt haben. Sie können zwischen verschiedenen Arten von Visualisierungen wählen:

- Table
- Kreisdiagramm
- Liniendiagramm
- Bereich
- Balkendiagramm

Hier ist der Code zum Zeichnen der Daten:

	# RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
	%%local
	import matplotlib.pyplot as plt
	%matplotlib inline
	
	# TIP BY PAYMENT TYPE AND PASSENGER COUNT
	ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
	ax1.set_title('Tip amount distribution')
	ax1.set_xlabel('Tip Amount ($)')
	ax1.set_ylabel('Counts')
	plt.suptitle('')
	plt.show()
	
	# TIP BY PASSENGER COUNT
	ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
	ax2.set_title('Tip amount by Passenger count')
	ax2.set_xlabel('Passenger count')
	ax2.set_ylabel('Tip Amount ($)')
	plt.suptitle('')
	plt.show()
	
	# TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
	ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
	ax.set_title('Tip amount by Fare amount')
	ax.set_xlabel('Fare Amount ($)')
	ax.set_ylabel('Tip Amount ($)')
	plt.axis([-2, 80, -2, 20])
	plt.show()


**AUSGABE:**

![Trinkgeldbetrag-Histogramm](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-histogram.png)

![Trinkgeldbetrag nach Anzahl der Fahrgäste](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-passenger-count.png)

![Trinkgeldbetrag nach Höhe des Fahrpreises](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-fare-amount.png)



## Erstellen von Features und Datenvorbereitung für Modellierungsfunktionen 

Für die Verwendung strukturbasierter Modellierungsfunktionen von Spark ML und MLlib müssen Ziel und Features mithilfe einer Vielzahl von geeigneten Techniken wie Diskretisierung, Indizierung, One-Hot-Codierung und Vektorisierung vorbereitet werden. Diese Verfahren durchlaufen wir in diesem Abschnitt:

- Erstellen eines neuen Features durch **Diskretisieren** von Stunden in Verkehrszeitbuckets
- **Indizieren und One-Hot-Codieren** von kategorischen Features
- **Stichprobenerfassung und Teilen von Datasets** in Trainings- und Testteile
- **Festlegen von Trainingsvariablen und Features** und Erstellen indizierter oder One-Hot-codierter RDDs mit bezeichnetem Punkt oder Dataframes für Training und Testeingabe
- Automatisches **Kategorisieren und Vektorisieren von Features und Zielen** für die Verwendung als Eingaben für ML-Modelle


### Erstellen eines neuen Features durch Diskretisieren von Stunden in Verkehrszeitbuckets 

Dieser Code zeigt, wie Sie ein neues Feature durch Diskretisieren von Stunden in Verkehrszeitbuckets erstellen und dann den resultierenden Datenrahmen im Arbeitsspeicher zwischenspeichern. Wo robuste verteilte Datasets (Resilient Distributed Datasets, RDDs) und Datenrahmen wiederholt verwendet werden, führt das Zwischenspeichern zu verbesserten Ausführungszeiten. Folglich werden wir RDDs und Datenrahmen in mehreren Phasen der exemplarischen Vorgehensweise zwischenspeichern.

	# CREATE FOUR BUCKETS FOR TRAFFIC TIMES
	val sqlStatement = """
	    SELECT *,
	    CASE
	     WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
	     WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
	     WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
	     WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
	    END as TrafficTimeBins
	    FROM taxi_train 
	"""
	val taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)
	
	# CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY 
	taxi_df_train_with_newFeatures.cache()
	taxi_df_train_with_newFeatures.count()


### Indizieren und One-Hot-Codieren von kategorischen Features 

Dieser Abschnitt beschreibt das Indizieren oder Codieren von kategorischen Features für die Eingabe in die Modellierungsfunktionen. Die Modellierungs- und Vorhersagefunktionen von MLlib erfordern, dass Features mit kategorischen Eingabedaten vor der Verwendung indiziert oder codiert werden.

Je nach Modell müssen Sie sie auf unterschiedliche Weise indizieren oder codieren. Logistische und lineare Regressionsmodelle erfordern beispielsweise One-Hot-Codierung, wobei z.B. ein Feature mit 3 Kategorien auf 3 Featurespalten erweitert werden kann, wobei jede abhängig von der Kategorie einer Beobachtung 0 oder 1 enthalten kann. MLlib bietet die Funktion [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) zur One-Hot-Codierung. Dieser Encoder ordnet eine Spalte mit Bezeichnungsindizes einer Spalte mit binären Vektoren mit höchstens einem einzigen Wert zu. Diese Codierung ermöglicht die Anwendung von Algorithmen, die Nummernwertfeatures erwarten, z.B. logistische Regression, auf kategorische Features.

Hier wandeln wir nur vier Variablen um, um Beispiele zu zeigen, die Zeichenfolgen sind. Andere Variablen, z.B. Wochentag, die durch numerische Werte dargestellt werden, können auch als kategorische Variablen indiziert werden.

Für die Indizierung verwendeten wir `StringIndexer()`, und für die One-Hot-Codierung verwendeten wir `OneHotEncoder()`-Funktionen von MLlib. Hier ist der Code zum Indizieren und Codieren von kategorischen Features:


	# HERE WE CREATE INDEXES, AND ONE-HOT ENCODED VECTORS FOR SEVERAL CATEGORICAL FEATURES

	# RECORD START TIME
	val starttime = Calendar.getInstance().getTime()
	
	# INDEX AND ENCODE VENDOR_ID
	val stringIndexer = new StringIndexer().setInputCol("vendor_id").setOutputCol("vendorIndex").fit(taxi_df_train_with_newFeatures)
	val indexed = stringIndexer.transform(taxi_df_train_with_newFeatures)
	val encoder = new OneHotEncoder().setInputCol("vendorIndex").setOutputCol("vendorVec")
	val encoded1 = encoder.transform(indexed)
	
	# INDEX AND ENCODE RATE_CODE
	val stringIndexer = new StringIndexer().setInputCol("rate_code").setOutputCol("rateIndex").fit(encoded1)
	val indexed = stringIndexer.transform(encoded1)
	val encoder = new OneHotEncoder().setInputCol("rateIndex").setOutputCol("rateVec")
	val encoded2 = encoder.transform(indexed)
	
	# INDEX AND ENCODE PAYMENT_TYPE
	val stringIndexer = new StringIndexer().setInputCol("payment_type").setOutputCol("paymentIndex").fit(encoded2)
	val indexed = stringIndexer.transform(encoded2)
	val encoder = new OneHotEncoder().setInputCol("paymentIndex").setOutputCol("paymentVec")
	val encoded3 = encoder.transform(indexed)
	
	# INDEX AND TRAFFIC TIME BINS
	val stringIndexer = new StringIndexer().setInputCol("TrafficTimeBins").setOutputCol("TrafficTimeBinsIndex").fit(encoded3)
	val indexed = stringIndexer.transform(encoded3)
	val encoder = new OneHotEncoder().setInputCol("TrafficTimeBinsIndex").setOutputCol("TrafficTimeBinsVec")
	val encodedFinal = encoder.transform(indexed)
	
	# GET TIME TO RUN THE CELL 
	val endtime = Calendar.getInstance().getTime()
	val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
	println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**AUSGABE:**

Für die Ausführung der obigen Zelle benötigte Zeit: 4 Sekunden.



### Stichprobenerfassung und Teilen von Datasets in Trainings- und Testteile

Dieser Code erstellt eine zufällige Stichprobe der Daten (25 % werden hier verwendet). Obwohl es aufgrund der Größe des Datasets in diesem Beispiel nicht erforderlich ist, zeigen wir Ihnen, wie Sie hier Stichproben erfassen können, damit Sie dieses Verfahren bei Bedarf zur Lösung eigener Probleme verwenden können. Bei großen Stichproben können Sie so beim Trainieren von Modellen deutlich Zeit sparen. Als Nächstes teilen wir die Stichprobe in einen Trainingsteil (hier 75 %) und einen Testteil (hier 25 %) zur Klassifizierung und Regressionsmodellierung.

Wir fügen eine Zufallszahl (zwischen 0 und 1) hinzu, um die Zeile (in einer „rand“-Spalte) zu erreichen, die zur Auswahl der Kreuzvalidierungsteilmengen während des Trainings verwendet werden kann.


	# RECORD START TIME
	val starttime = Calendar.getInstance().getTime()
	
	# SPECIFY SAMPLING AND SPLITTING FRACTIONS
	val samplingFraction = 0.25;
	val trainingFraction = 0.75; 
	val testingFraction = (1-trainingFraction);
	val seed = 1234;
	val encodedFinalSampledTmp = encodedFinal.sample(withReplacement = false, fraction = samplingFraction, seed = seed)
	val sampledDFcount = encodedFinalSampledTmp.count().toInt
	
	val generateRandomDouble = udf(() => {
	    scala.util.Random.nextDouble
	})
	
	# ADD RANDOM NUMBER FOR CV
	val encodedFinalSampled = encodedFinalSampledTmp.withColumn("rand", generateRandomDouble());
	
	# SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST, WITH A RANDOM COLUMN ADDED FOR DOING CV (SHOWN LATER)
	# INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS
	val splits = encodedFinalSampled.randomSplit(Array(trainingFraction, testingFraction), seed = seed)
	val trainData = splits(0)
	val testData = splits(1)
	
	# GET TIME TO RUN THE CELL 
	val endtime = Calendar.getInstance().getTime()
	val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
	println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**AUSGABE:**

Für die Ausführung der obigen Zelle benötigte Zeit: 2 Sekunden.


### Festlegen von Trainingsvariablen und Features und Erstellen indizierter oder One-Hot-codierter RDDs mit bezeichnetem Punkt oder Dataframes für Training und Testeingabe 

Dieser Abschnitt enthält Code, der zeigt, wie Sie kategorische Textdaten als bezeichneten Punktdatentyp und so codieren, dass sie zum Trainieren und Testen logistischer Regression gemäß MLlib und anderer Klassifizierungsmodelle verwendet werden können. Bezeichnete Punktobjekte sind robuste verteilte Datasets (RDD), die in einer Weise formatiert sind, die von den meisten ML-Algorithmen in MLlib als Eingabedaten benötigt wird. Ein [bezeichneter Punkt](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) ist ein lokaler Vektor, entweder dicht oder platzsparend, der mit einer Bezeichnung/Antwort verknüpft ist.

In diesem Code legen wir (zielabhängige) Variablen und Features fest, die zum Training verwendet werden sollen, und erstellen indizierte oder One-Hot-codierte RDDs mit bezeichnetem Punkt oder Dataframes für Training und Testeingabe.


	# RECORD START TIME
	val starttime = Calendar.getInstance().getTime()
	
	# MAP NAMES OF FEATURES AND TARGETS FOR CLASSIFICATION AND REGRESSION PROBLEMS.
	val featuresIndOneHot = List("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))
	val featuresIndIndex = List("paymentIndex", "vendorIndex", "rateIndex", "TrafficTimeBinsIndex", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))
	
	# SPECIFY THE TARGET FOR CLASSIFICATION ('tipped') AND REGRESSION ('tip_amount') PROBLEMS
	val targetIndBinary = List("tipped").map(encodedFinalSampled.columns.indexOf(_))
	val targetIndRegression = List("tip_amount").map(encodedFinalSampled.columns.indexOf(_))
	
	# INDEXED LAELEDPOINT RDD OBJECTS
	val indexedTRAINbinary = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
	val indexedTESTbinary = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
	val indexedTRAINreg = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
	val indexedTESTreg = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
	
	# Indexed DFs that can be used for training using Spark ML functions
	val indexedTRAINbinaryDF = indexedTRAINbinary.toDF()
	val indexedTESTbinaryDF = indexedTESTbinary.toDF()
	val indexedTRAINregDF = indexedTRAINreg.toDF()
	val indexedTESTregDF = indexedTESTreg.toDF()
	
	# One-hot encoded (vectorized) DFs that can be used for training using Spark ML functions
	val assemblerOneHot = new VectorAssembler().setInputCols(Array("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount")).setOutputCol("features")
	val OneHotTRAIN = assemblerOneHot.transform(trainData) 
	val OneHotTEST = assemblerOneHot.transform(testData)
	
	# GET TIME TO RUN THE CELL
	val endtime = Calendar.getInstance().getTime()
	val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
	println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**AUSGABE:**

Für die Ausführung der obigen Zelle benötigte Zeit: 4 Sekunden.


### Automatisches Kategorisieren und Vektorisieren von Features und Zielen für die Verwendung als Eingaben für ML-Modelle

Kategorisieren Sie ordnungsgemäß Ziel und Features für die Verwendung in strukturbasierten Modellierungsfunktionen in Spark ML. Im Code werden zwei Aufgaben ausgeführt:

1. Erstellen eines binären Ziels für die Klassifizierung durch Zuweisung eines Werts von 0 oder 1 zu jedem Datenpunkt zwischen 0 und 1 unter Verwendung eines Schwellenwerts von 0,5.
2. Automatisches Kategorisieren von Features. Wenn die Anzahl von unterschiedlichen numerischen Werten für beliebige Features < 32 ist, dann ist dieses Feature kategorisiert.

Hier ist der Code für diese beiden Aufgaben.

	# CATEGORIZE FEATURES AND BINARIZE TARGET FOR BINARY CLASSIFICATION PROBLEM

	# TRAIN DATA
	val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
	val indexerModel = indexer.fit(indexedTRAINbinaryDF)
	val indexedTrainwithCatFeat = indexerModel.transform(indexedTRAINbinaryDF)
	val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
	val indexedTRAINwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)
	
	# TEST DATA
	val indexerModel = indexer.fit(indexedTESTbinaryDF)
	val indexedTrainwithCatFeat = indexerModel.transform(indexedTESTbinaryDF)
	val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
	val indexedTESTwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)
	
	# CATEGORIZE FEATURES FOR REGRESSION PROBLEM
	# CREATE PROPERLY INDEXED AND CATEGORIED DFS FOR TREE-BASED MODELS

	# TRAIN DATA
	val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
	val indexerModel = indexer.fit(indexedTRAINregDF)
	val indexedTRAINwithCatFeat = indexerModel.transform(indexedTRAINregDF)
	
	# TEST DATA
	val indexerModel = indexer.fit(indexedTESTbinaryDF)
	val indexedTESTwithCatFeat = indexerModel.transform(indexedTESTregDF)


## Binäre Klassifizierung: Vorhersage, ob ein Trinkgeld gezahlt wird oder nicht

In diesem Abschnitt erstellen wir drei Arten von Modellen der binären Klassifizierung, um vorherzusagen, ob ein Trinkgeld gezahlt wird oder nicht:

- ein **logistisches Regressionsmodell** mithilfe der `LogisticRession()`-Funktion von Spark ML
- ein **zufälliges Gesamtstrukturmodell** mithilfe der `RandomForestClassifier()`-Funktion von Spark ML
- ein **Gradient-Boosted-Struktur-Klassifizierungsmodell** mithilfe der `GradientBoostedTrees()`-Funktion von MLlib

### Erstellen eines logistischen Regressionsmodells

Hier erstellen wir ein logistisches Regressionsmodell mithilfe der `LogisticRession()`-Funktion von Spark ML. Der Modellerstellungscode in diesem Abschnitt besteht aus einer Reihe von Schritten:

1. **Modelltrainingsdaten** mit einem Parametersatz
2. **Modellevaluierung** an einem Testdataset mit Metriken
3. **Speichern des Modells** im Blob für die zukünftige Nutzung
4. **Abstimmen des Modells anhand von Testdaten**
5. **Zeichnen der Ergebnisse** – die ROC-Kurven

Hier ist der Code für diese beiden Verfahren.

	# CREATE LOGISTIC REGRESSION MODEL 
	val lr = new LogisticRegression().setLabelCol("tipped").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)
	val lrModel = lr.fit(OneHotTRAIN)
	
	# PREDICT ON TEST DATA SET
	val predictions = lrModel.transform(OneHotTEST)
	
	# SELECT BinaryClassificationEvaluator() TO COMPUTE TEST ERROR
	val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
	val ROC = evaluator.evaluate(predictions)
	println("ROC on test data = " + ROC)
	
	# SAVE THE MODEL
	val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
	val modelName = "LogisticRegression__"
	val filename = modelDir.concat(modelName).concat(datestamp)
	lrModel.save(filename);

Dann laden, bewerten und speichern Sie die Ergebnisse.

	# RECORD START TIME 
	val starttime = Calendar.getInstance().getTime()

	# LOAD SAVED MODEL AND SCORE THE TEST DATA SET
	val savedModel = org.apache.spark.ml.classification.LogisticRegressionModel.load(filename)
	println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")
	
	# SCORE THE MODEL ON THE TEST DATA
	val predictions = savedModel.transform(OneHotTEST).select("tipped","probability","rawPrediction")
	predictions.registerTempTable("testResults")
	
	# SELECT BinaryClassificationEvaluator() TO COMPUTE TEST ERROR
	val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
	val ROC = evaluator.evaluate(predictions)
	
	# GET TIME TO RUN THE CELL
	val endtime = Calendar.getInstance().getTime()
	val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
	println("Time taken to run the above cell: " + elapsedtime + " seconds.");
	
	# PRINT THE ROC RESULTS
	println("ROC on test data = " + ROC)


**AUSGABE:**

ROC für Testdaten = 0,9827381497557599


Verwenden Sie Python auf lokalen Pandas-Dataframes, um ROC-Kurven zu zeichnen.


	# QUERY RESULTS
	%%sql -q -o sqlResults
	SELECT tipped, probability from testResults


	# RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
	%%local
	%matplotlib inline
	from sklearn.metrics import roc_curve,auc
	
	sqlResults['probFloat'] = sqlResults.apply(lambda row: row['probability'].values()[0][1], axis=1)
	predictions_pddf = sqlResults[["tipped","probFloat"]]
	
	# ROC CURVE
	# predictions_pddf = sqlResults.rename(columns={'_1': 'probability', 'tipped': 'label'})
	prob = predictions_pddf["probFloat"] 
	fpr, tpr, thresholds = roc_curve(predictions_pddf['tipped'], prob, pos_label=1);
	roc_auc = auc(fpr, tpr)
	
	#PLOT
	plt.figure(figsize=(5,5))
	plt.plot(fpr, tpr, label='ROC curve (area = %0.2f)' % roc_auc)
	plt.plot([0, 1], [0, 1], 'k--')
	plt.xlim([0.0, 1.0])
	plt.ylim([0.0, 1.05])
	plt.xlabel('False Positive Rate')
	plt.ylabel('True Positive Rate')
	plt.title('ROC Curve')
	plt.legend(loc="lower right")
	plt.show()


**AUSGABE:**

![„Trinkgeld-oder-nicht“-ROC-Kurve](./media/machine-learning-data-science-process-scala-walkthrough/plot-roc-curve-tip-or-not.png)


### Erstellen eines zufälligen Gesamtstrukturmodells

Hier erstellen wir ein zufälliges Gesamtstrukturmodell mit der `RandomForestClassifier()`-Funktion von Spark ML und werten das Modell anhand von Testdaten aus.


	# RECORD START TIME 
	val starttime = Calendar.getInstance().getTime()
	
	# CREATE THE RANDOM FOREST CLASSIFIER MODEL
	val rf = new RandomForestClassifier().setLabelCol("labelBin").setFeaturesCol("featuresCat").setNumTrees(10).setSeed(1234)
	
	# FIT THE MODEL
	val rfModel = rf.fit(indexedTRAINwithCatFeatBinTarget)
	val predictions = rfModel.transform(indexedTESTwithCatFeatBinTarget)
	
	# EVALUATE THE MODEL
	val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
	val Test_f1Score = evaluator.evaluate(predictions)
	println("F1 score on test data: " + Test_f1Score);
	
	# GET TIME TO RUN THE CELL
	val endtime = Calendar.getInstance().getTime()
	val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
	println("Time taken to run the above cell: " + elapsedtime + " seconds.");
	
	# CALCULATE BINARY CLASSIFICATION EVALUATION METRICSS
	val evaluator = new BinaryClassificationEvaluator().setLabelCol("label").setRawPredictionCol("probability").setMetricName("areaUnderROC")
	val ROC = evaluator.evaluate(predictions)
	println("ROC on test data = " + ROC)


**AUSGABE:**

ROC für Testdaten = 0,9847103571552683


### Erstellen eines Gradient-Boosted-Struktur-Klassifizierungsmodells

Hier erstellen wir ein Gradient-Boosted-Struktur-Klassifizierungsmodell unter Verwendung der `GradientBoostedTrees()`-Funktion von MLlib und werten das Modell anhand von Testdaten aus.


	# TRAIN A GBT CLASSIFICATION MODEL USING MLIB AND LABELEDPOINT

	# RECORD START TIME
	val starttime = Calendar.getInstance().getTime()
	
	# DEFINE THE GBT CLASSIFICATION MODEL
	val boostingStrategy = BoostingStrategy.defaultParams("Classification")
	boostingStrategy.numIterations = 20
	boostingStrategy.treeStrategy.numClasses = 2
	boostingStrategy.treeStrategy.maxDepth = 5
	boostingStrategy.treeStrategy.categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))
	
	# TRAIN THE MODEL
	val gbtModel = GradientBoostedTrees.train(indexedTRAINbinary, boostingStrategy)
	
	# SAVE THE MODEL IN BLOB STORAGE
	val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
	val modelName = "GBT_Classification__"
	val filename = modelDir.concat(modelName).concat(datestamp)
	gbtModel.save(sc, filename);
	
	# EVALUATE THE MODEL ON TEST INSTANCES AND THE COMPUTE TEST ERROR
	val labelAndPreds = indexedTESTbinary.map { point =>
	  val prediction = gbtModel.predict(point.features)
	  (point.label, prediction)
	}
	val testErr = labelAndPreds.filter(r => r._1 != r._2).count.toDouble / indexedTRAINbinary.count()
	//println("Learned classification GBT model:\n" + gbtModel.toDebugString)
	println("Test Error = " + testErr)
	
	# USE BINARY AND MULTICLASS METRICS TO EVALUATE THE MODEL ON THE TEST DATA
	val metrics = new MulticlassMetrics(labelAndPreds)
	println(s"Precision: ${metrics.precision}")
	println(s"Recall: ${metrics.recall}")
	println(s"F1 Score: ${metrics.fMeasure}")
	
	val metrics = new BinaryClassificationMetrics(labelAndPreds)
	println(s"Area under PR curve: ${metrics.areaUnderPR}")
	println(s"Area under ROC curve: ${metrics.areaUnderROC}")
	
	# GET TIME TO RUN THE CELL
	val endtime = Calendar.getInstance().getTime()
	val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
	println("Time taken to run the above cell: " + elapsedtime + " seconds.");
	
	# PRINT THE ROC METRIC
	println(s"Area under ROC curve: ${metrics.areaUnderROC}")


**AUSGABE:**

Bereich unter ROC-Kurve = 0,9846895479241554


## Regression: Vorhersage des Trinkgeldbetrags 

In diesem Abschnitt erstellen wir zwei Arten von Regressionsmodellen, um den Trinkgeldbetrag vorherzusagen:

- ein **normalisiertes lineares Regressionsmodell** mithilfe der `LinearRegression()`-Funktion von Spark ML, um das Modell zu speichern und anhand von Testdaten auszuwerten.
- ein **Gradient-Boosted-Struktur-Regressionsmodells** mit der `GBTRegressor() `-Funktion von Spark ML.


### Erstellen eines normalisierten linearen Regressionsmodells

	# RECORD START TIME
	val starttime = Calendar.getInstance().getTime()
	
	# CREATE REGULARIZED LINEAR REGRESSION MODEL USING SPARK ML FUNCTION AND DATA-FRAME
	val lr = new LinearRegression().setLabelCol("tip_amount").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)
	
	# FIT THE MODEL USING DATA-FRAME
	val lrModel = lr.fit(OneHotTRAIN)
	println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")
	
	# SUMMARIZE THE MODEL OVER THE TRAIINNG SET AND PRINT OUT SOME METRICS
	val trainingSummary = lrModel.summary
	println(s"numIterations: ${trainingSummary.totalIterations}")
	println(s"objectiveHistory: ${trainingSummary.objectiveHistory.toList}")
	trainingSummary.residuals.show()
	println(s"RMSE: ${trainingSummary.rootMeanSquaredError}")
	println(s"r2: ${trainingSummary.r2}")
	
	# SAVE THE MODEL IN BLOB STORAGE
	val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
	val modelName = "LinearRegression__"
	val filename = modelDir.concat(modelName).concat(datestamp)
	lrModel.save(filename);
	
	# PRINT COEFFICIENTS
	println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")
	
	# SCORE THE MODEL ON TEST DATA
	val predictions = lrModel.transform(OneHotTEST)
	
	# EVALUATE THE MODEL ON TEST DATA
	val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
	val r2 = evaluator.evaluate(predictions)
	println("R-sqr on test data = " + r2)
	
	# GET TIME TO RUN THE CELL
	val endtime = Calendar.getInstance().getTime()
	val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
	println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**AUSGABE:**

Für die Ausführung der obigen Zelle benötigte Zeit: 13 Sekunden.


	# LOAD A SAVED LINEARREGRESSION MODEL FROM BLOB STORAGE AND SCORE A TEST DATA SET. 

	# RECORD START TIME
	val starttime = Calendar.getInstance().getTime()
	
	# LOAD A SAVED LINEARREGRESSION MODEL FROM BLOB STORAGE
	val savedModel = org.apache.spark.ml.regression.LinearRegressionModel.load(filename)
	println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")
	
	# SCORE THE MODEL ON TEST DATA
	val predictions = savedModel.transform(OneHotTEST).select("tip_amount","prediction")
	predictions.registerTempTable("testResults")
	
	# EVALUATE THE MODEL ON TEST DATA
	val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
	val r2 = evaluator.evaluate(predictions)
	println("R-sqr on test data = " + r2)
	
	# GET TIME TO RUN THE CELL
	val endtime = Calendar.getInstance().getTime()
	val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
	println("Time taken to run the above cell: " + elapsedtime + " seconds.");
	
	# PRINT RESULTS
	println("R-sqr on test data = " + r2)


**AUSGABE:**

R-sqr. für Testdaten = 0,5960320470835743


Fragen Sie dann die Testergebnisse als Dataframe ab, und visualisieren Sie die Ergebnisse mit Jupyter autoviz und Python matplotlib.


	# SQL QUERY
	%%sql -q -o sqlResults
	select * from testResults

	# RUN THE CODE LOCALLY ON THE JUPYTER SERVER
	%%local

	# USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
	# CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
	sqlResults

Mit dem Code wird aus der Ausgabe der Abfrage ein lokaler Dataframe erstellt, und die Daten werden in einem Diagramm dargestellt („geplottet“). Der Magic-Befehl `%%local` erstellt einen lokalen Dataframe `sqlResults`, der für das Plotten mit matplotlib verwendet werden kann.

>[AZURE.NOTE] Dieser Spark-Magic-Befehl wird in dieser exemplarischen Vorgehensweise mehrfach genutzt. Bei einer großen Datenmenge sollten Sie Stichproben verwenden, um einen Dataframe zu erstellen, der nicht zu groß für den lokalen Arbeitsspeicher ist.

Erstellen Sie Plots mit Python matplotlib.

	# RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
	%%local
	sqlResults
	%matplotlib inline
	import numpy as np
	
	# PLOT THE RESULTS
	ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='tip_amount', y='prediction', color='blue', alpha = 0.25, label='Actual vs. predicted');
	fit = np.polyfit(sqlResults['tip_amount'], sqlResults['prediction'], deg=1)
	ax.set_title('Actual vs. Predicted Tip Amounts ($)')
	ax.set_xlabel("Actual")
	ax.set_ylabel("Predicted")
	#ax.plot(sqlResults['tip_amount'], fit[0] * sqlResults['prediction'] + fit[1], color='magenta')
	plt.axis([-1, 15, -1, 8])
	plt.show(ax)

**AUSGABE:**

![Trinkgeldbetrag: „tatsächlich“ verglichen mit „vorhergesagt“](./media/machine-learning-data-science-process-scala-walkthrough/plot-actual-vs-predicted-tip-amount.png)


### Erstellen eines Gradient-Boosted-Struktur-Regressionsmodells

Hier erstellen wir ein Gradient-Boosted-Struktur-Regressionsmodell unter Verwendung der `ML GBTRegressor()`-Funktion von Spark und werten das Modell anhand von Testdaten aus.

[Gradient-Boosted-Strukturen](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) sind Gruppen von Entscheidungsstrukturen. GBTs trainieren Entscheidungsstrukturen iterativ, um einen Funktionsverlust zu minimieren. GBTs werden für die Regression und Klassifizierung verwendet und können kategorische Features behandeln, erfordern keine Featureskalierung und können Nichtlinearitäten und Featureinteraktionen erfassen. Sie können auch in einer Mehrklassenklassifizierung verwendet werden.


	# RECORD START TIME
	val starttime = Calendar.getInstance().getTime()
	
	# TRAIN A GBT REGRESSION MODEL
	val gbt = new GBTRegressor().setLabelCol("label").setFeaturesCol("featuresCat").setMaxIter(10)
	val gbtModel = gbt.fit(indexedTRAINwithCatFeat)
	
	# MAKE PREDICTIONS
	val predictions = gbtModel.transform(indexedTESTwithCatFeat)
	
	# COMPUTE TEST SET R2
	val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
	val Test_R2 = evaluator.evaluate(predictions)
	
	
	# GET TIME TO RUN THE CELL
	val endtime = Calendar.getInstance().getTime()
	val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
	println("Time taken to run the above cell: " + elapsedtime + " seconds.");
	
	# PRINT RESULTS
	println("Test R-sqr is: " + Test_R2);


**AUSGABE:**

Test-R-sqr. ist: 0,7655383534596654



## Erweiterte Modellierungshilfsprogramme zur Optimierung

In diesem Abschnitt wird beschrieben, wie häufig eingesetzte ML-Hilfsprogramme zur Modelloptimierung verwendet werden. Insbesondere zeigen wir drei verschiedene Arten der Optimierung von ML-Modellen mit Parameter-Sweeping und Kreuzvalidierung:

1\. Aufteilen der Daten in Trainings- und Validierungssets, Optimieren des Modells mit Hyperparameter-Sweeping eines Trainingssatzes und Auswertung eines Validierungssets (lineare Regression)

2\. Optimieren des Modells mit Kreuzvalidierung und Hyperparameter-Sweeping mithilfe der CrossValidator-Funktion von Spark ML (binäre Klassifizierung)

3\. Optimieren des Modells mit benutzerdefinierter Kreuzvalidierung und Parameter-Sweepingcode zur Nutzung beliebiger ML-Funktionen und Parametersätzen (lineare Regression)


Die **Kreuzvalidierung** (Cross-Validation, CV) ist eine Technik, mit der bewertet wird, wie gut ein Modell, das mit einem bekannten Satz von Daten trainiert wurde, verallgemeinern kann, um die Features von Datasets, mit denen es nicht trainiert wurde, vorherzusagen. Der Grundgedanke hinter dieser Technik ist, dass ein Modell mit bekannten Daten trainiert wird und die Genauigkeit seiner Vorhersagen dann mit einem unabhängigen Datensatz geprüft wird. Eine gängige Implementierung besteht darin, den Datensatz k-fach aufzuteilen und das Modell daraufhin im Roundrobin-Verfahren mit allen Teilmengen außer einer zu trainieren.

Die **Hyperparameteroptimierung** besteht in dem Problem, einen Hyperparametersatz für einen Lernalgorithmus auszuwählen, in der Regel um eine Leistungskennzahl des Algorithmus in Bezug auf einen unabhängigen Datensatz zu optimieren. **Hyperparameter** sind Werte, die außerhalb des Trainingsverfahrens des Modells festgelegt werden müssen. Annahmen über diese Werte können sich auf die Flexibilität und Genauigkeit der Modelle auswirken. Entscheidungsstrukturen umfassen Hyperparameter, wie z.B. die gewünschte Tiefe und Anzahl der Blätter in der Struktur. Support Vector Machines (SVMs) erfordern es, Bedingungen für Abzüge bei Fehlklassifikationen festzulegen.

Eine hier häufig verwendete Methode für die Hyperparameteroptimierung besteht in der Rastersuche bzw. im **Parameter-Sweeping**. Dabei werden die Werte einer bestimmten Teilmenge des Hyperparameterraums für einen Lernalgorithmus umfassend durchsucht. Die Kreuzvalidierung kann eine Leistungsmetrik liefern, um die besten Ergebnisse des Rastersuchalgorithmus auszusortieren. Zusammen mit dem Hyperparameter-Sweeping ermöglicht es die Kreuzvalidierung, Probleme wie eine Überanpassung eines Modells an die Trainingsdaten einzuschränken, damit das Modell weiterhin auf den allgemeinen Datensatz, aus dem die Trainingsdaten extrahiert wurden, angewandt werden kann.

### Optimieren des linearen Regressionsmodells mit Hyperparameter-Sweeping

Aufteilen der Daten in Trainings- und Validierungssets, Optimieren des Modells mit Hyperparameter-Sweeping eines Trainingssatzes und Auswertung eines Validierungssets (lineare Regression)

	# RECORD START TIME
	val starttime = Calendar.getInstance().getTime()
	
	# RENAME tip_amount AS LABEL
	val OneHotTRAINLabeled = OneHotTRAIN.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label") 
	val OneHotTESTLabeled = OneHotTEST.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
	OneHotTRAINLabeled.cache()
	OneHotTESTLabeled.cache()
	
	# DEFINE THE ESTIMATOR FUNCTION: THE LinearRegression() FUNCTION
	val lr = new LinearRegression().setLabelCol("label").setFeaturesCol("features").setMaxIter(10)
	
	# DEFINE THE PARAMETER GRID
	val paramGrid = new ParamGridBuilder().addGrid(lr.regParam, Array(0.1, 0.01, 0.001)).addGrid(lr.fitIntercept).addGrid(lr.elasticNetParam, Array(0.1, 0.5, 0.9)).build()
	
	# DEFINE PIPELINE WITH TRAIN-TEST VALIDATION SPLIT, WITH 75% IN THE TRAIING SET, SPECIFY ESTIMATOR, EVALUATOR, AND PARAMETER GRID
	val trainPct = 0.75
	val trainValidationSplit = new TrainValidationSplit().setEstimator(lr).setEvaluator(new RegressionEvaluator).setEstimatorParamMaps(paramGrid).setTrainRatio(trainPct)
	
	# RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
	val model = trainValidationSplit.fit(OneHotTRAINLabeled)
	
	# MAKE PREDICTIONS ON THE TEST DATA USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORM THE BEST
	val testResults = model.transform(OneHotTESTLabeled).select("label", "prediction")
	
	# COMPUTE TEST SET R2
	val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
	val Test_R2 = evaluator.evaluate(testResults)
	
	# GET TIME TO RUN THE CELL
	val endtime = Calendar.getInstance().getTime()
	val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
	println("Time taken to run the above cell: " + elapsedtime + " seconds.");
	
	println("Test R-sqr is: " + Test_R2);


**AUSGABE:**

Test-R-sqr. ist: 0,6226484708501209


### Optimieren des binären Klassifizierungsmodells mit Kreuzvalidierung und Hyperparameter-Sweeping

Hier zeigen wir Ihnen das Optimieren des binären Klassifizierungsmodells mit Kreuzvalidierung und Hyperparameter-Sweeping. Hier wird die CrossValidator-Funktion von Spark ML verwendet.

	# RECORD START TIME
	val starttime = Calendar.getInstance().getTime()
	
	# CREATE DATA-FRAMES WITH PROPERLY LABELED COLUMNS FOR USE WITH TRAIN/TEST SPLIT
	val indexedTRAINwithCatFeatBinTargetRF = indexedTRAINwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
	val indexedTESTwithCatFeatBinTargetRF = indexedTESTwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
	indexedTRAINwithCatFeatBinTargetRF.cache()
	indexedTESTwithCatFeatBinTargetRF.cache()
	
	# DEFINE THE ESTIMATOR FUNCTION
	val rf = new RandomForestClassifier().setLabelCol("label").setFeaturesCol("features").setImpurity("gini").setSeed(1234).setFeatureSubsetStrategy("auto").setMaxBins(32)
	
	# DEFINE THE PARAMETER GRID
	val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(4,8)).addGrid(rf.numTrees, Array(5,10)).addGrid(rf.minInstancesPerNode, Array(100,300)).build()
	
	# SPECIFY THE NUMBER OF FOLDS
	val numFolds = 3
	
	# DEFINE THE TRAIN-TEST VALIDATION SPLIT WITH 75% IN THE TRAIING SET
	val CrossValidator = new CrossValidator().setEstimator(rf).setEvaluator(new BinaryClassificationEvaluator).setEstimatorParamMaps(paramGrid).setNumFolds(numFolds)
	
	# RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
	val model = CrossValidator.fit(indexedTRAINwithCatFeatBinTargetRF)
	
	# MAKE PREDICTIONS ON THE TEST DATA USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORM THE BEST
	val testResults = model.transform(indexedTESTwithCatFeatBinTargetRF).select("label", "prediction")
	
	# COMPUTE TEST F1 SCORE
	val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
	val Test_f1Score = evaluator.evaluate(testResults)
	
	# GET TIME TO RUN THE CELL
	val endtime = Calendar.getInstance().getTime()
	val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
	println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**AUSGABE:**

Für die Ausführung der obigen Zelle benötigte Zeit: 33 Sekunden.


### Optimieren des linearen Regressionsmodells mithilfe benutzerdefinierter Kreuzvalidierung und Parameter-Sweepingcode

Hier optimieren wir das Modell mit benutzerdefiniertem Code und identifizieren die besten Modellparameter mit dem Kriterium der höchste Genauigkeit. Dann erstellen wir das finale Modell, werten es anhand von Testdaten aus und speichern es im Blobspeicher. Schließlich laden wir das Modell, bewerten Testdaten und werten seine Genauigkeit aus.

	# RECORD START TIME
	val starttime = Calendar.getInstance().getTime()
	
	# DEFINE PARAMETER GRID AND THE NUMBER OF FOLDS
	val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(5,10)).addGrid(rf.numTrees, Array(10,25,50)).build()
	
	val nFolds = 3
	val numModels = paramGrid.size
	val numParamsinGrid = 2
	
	# SPECIFY THE NUMBER OF CATEGORIES FOR THE CATEGORIAL VARIBLES
	val categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))
	
	var maxDepth = -1
	var numTrees = -1
	var param = ""
	var paramval = -1
	var validateLB = -1.0
	var validateUB = -1.0
	val h = 1.0 / nFolds;
	val RMSE  = Array.fill(numModels)(0.0)
	
	# CREATE K FOLDS
	val splits = MLUtils.kFold(indexedTRAINbinary, numFolds = nFolds, seed=1234)
	
	
	# LOOP THROUGH K-FOLDS AND THE PARAMETER GRID TO GET AND IDENTIFY THE BEST PARAMETER SET BY ACCURACY
	for (i <- 0 to (nFolds-1)) {
	    validateLB = i * h
	    validateUB = (i + 1) * h
	    val validationCV = trainData.filter($"rand" >= validateLB  && $"rand" < validateUB)
	    val trainCV = trainData.filter($"rand" < validateLB  || $"rand" >= validateUB)
	    val validationLabPt = validationCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
	    val trainCVLabPt = trainCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
	    validationLabPt.cache()
	    trainCVLabPt.cache()
	
	    for (nParamSets <- 0 to (numModels-1)) {
	        for (nParams <- 0 to (numParamsinGrid-1)) {
	            param = paramGrid(nParamSets).toSeq(nParams).param.toString.split("__")(1)
	            paramval = paramGrid(nParamSets).toSeq(nParams).value.toString.toInt
	            if (param == "maxDepth") {maxDepth = paramval}
	            if (param == "numTrees") {numTrees = paramval}
	        }
	        val rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
	                                                  numTrees=numTrees, maxDepth=maxDepth,
	                                                  featureSubsetStrategy="auto",impurity="variance", maxBins=32)
	        val labelAndPreds = validationLabPt.map { point =>
	                                                 val prediction = rfModel.predict(point.features)
	                                                 ( prediction, point.label )
	                                                }
	        val validMetrics = new RegressionMetrics(labelAndPreds)
	        val rmse = validMetrics.rootMeanSquaredError
	        RMSE(nParamSets) += rmse
	    }
	    validationLabPt.unpersist();
	    trainCVLabPt.unpersist();
	}
	val minRMSEindex = RMSE.indexOf(RMSE.min)
	
	# GET BEST PARAMETERS FROM CV AND PARAMETER SWEEP 
	var best_maxDepth = -1
	var best_numTrees = -1
	for (nParams <- 0 to (numParamsinGrid-1)) {
	    param = paramGrid(minRMSEindex).toSeq(nParams).param.toString.split("__")(1)
	    paramval = paramGrid(minRMSEindex).toSeq(nParams).value.toString.toInt
	    if (param == "maxDepth") {best_maxDepth = paramval}
	    if (param == "numTrees") {best_numTrees = paramval}
	}
	
	# CREATE BEST MODEL WITH BEST PARAMETERS AND FULL TRAIING DATASET
	val best_rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
	                                                  numTrees=best_numTrees, maxDepth=best_maxDepth,
	                                                  featureSubsetStrategy="auto",impurity="variance", maxBins=32)
	
	# SAVE BEST RF MODEL IN BLOB STORAGE
	val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
	val modelName = "BestCV_RF_Regression__"
	val filename = modelDir.concat(modelName).concat(datestamp)
	best_rfModel.save(sc, filename);
	
	# PREDICT ON TRAINING SET WITH BEST MODEL AND EVALUATE
	val labelAndPreds = indexedTESTreg.map { point =>
	                                        val prediction = best_rfModel.predict(point.features)
	                                        ( prediction, point.label )
	                                       }
	
	val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
	val test_rsqr = new RegressionMetrics(labelAndPreds).r2
	
	# GET TIME TO RUN THE CELL
	val endtime = Calendar.getInstance().getTime()
	val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
	println("Time taken to run the above cell: " + elapsedtime + " seconds.");


	# LOAD THE MODEL
	val savedRFModel = RandomForestModel.load(sc, filename)
	
	val labelAndPreds = indexedTESTreg.map { point =>
	                                        val prediction = savedRFModel.predict(point.features)
	                                        ( prediction, point.label )
	                                       }
	# TEST THE MODEL
	val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
	val test_rsqr = new RegressionMetrics(labelAndPreds).r2


**AUSGABE:**

Für die Ausführung der obigen Zelle benötigte Zeit: 61 Sekunden.


## Automatische Nutzung mit Spark erstellter ML-Modelle mit Scala

Das Verfahren, Scala-Code zu verwenden, um mit in Spark erstellten und in Azure-Blobs gespeicherten ML-Modellen neue Datasets automatisch zu laden und zu bewerten, wurde im Thema [Bewerten von Machine Learning-Modellen, die mit Spark erstellt wurden](machine-learning-data-science-spark-model-consumption.md) vorgestellt. Benutzer können die dortigen Anweisungen befolgen und für eine automatisierte Nutzung einfach den Python-Code durch den oben aufgeführten Scala-Code ersetzen.

## Nächste Schritte

Eine Übersicht zu Themen, die Sie durch die Aufgaben führen, die den Data Science-Prozess in Azure umfassen, finden Sie unter [Team Data Science-Prozess (TDSP)](http://aka.ms/datascienceprocess).

Eine Beschreibung der sonstigen umfassenden exemplarischen Vorgehensweisen, die jeweils die Schritte im Team Data Science-Prozess für **bestimmte Szenarien** sowie das Kombinieren von cloudbasierten und lokalen Tools und Diensten in einem Workflow oder einer Pipeline, um eine intelligente Anwendung zu erstellen, veranschaulichen, finden Sie unter [Exemplarische Vorgehensweisen für den Team Data Science-Prozess](data-science-process-walkthroughs.md).

<!---HONumber=AcomDC_0803_2016-->