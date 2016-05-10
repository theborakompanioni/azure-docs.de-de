<properties
	pageTitle="Erweitertes Untersuchen von Daten und Modellierung mit Spark | Microsoft Azure"
	description="TBD."
	services="machine-learning"
	documentationCenter=""
	authors="bradsev,deguhath,gokuma"
	manager="paulettm"
	editor="cgronlun"  />

<tags
	ms.service="machine-learning"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/26/2016"
	ms.author="deguhath;bradsev" />

# Erweitertes Untersuchen und Modellieren von Daten mit Spark 

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

In dieser exemplarischen Vorgehensweise wird HDInsight Spark für das Durchsuchen von Daten sowie für Aufgaben zur binären Klassifizierung und Regressionsmodellierung mithilfe der Kreuzvalidierung und der Hyperparameteroptimierung zum Trainieren der Modelle für eine Stichprobe des Datasets der NYC-Taxifahrten und Fahrpreise von 2013 verwendet. Sie werden schrittweise unter Verwendung eines HDInsight Spark-Clusters für die Verarbeitung und von Azure-Blobs zum Speichern der Daten und Modelle durch den gesamten [Data Science-Prozess](http://aka.ms/datascienceprocess) geführt. In diesem Prozess werden Daten aus einem Azure Storage-Blob untersucht sowie visualisiert und anschließend für das Erstellen von Vorhersagemodellen vorbereitet. Python wurde verwendet, um die Lösung zu codieren und die entsprechenden Diagramme anzuzeigen. Diese Modelle werden mithilfe des Spark MLlib-Toolkits erstellt, um Aufgaben zur binären Klassifizierung und Regressionsmodellierung ausführen.

- Die Aufgabe zur **binären Klassifizierung** besteht darin, vorherzusagen, ob ein Trinkgeld für eine Fahrt bezahlt wird. 
- Die Aufgabe zur **Regression** besteht darin, die Höhe des Trinkgelds basierend auf anderen Trinkgeldfunktionen vorherzusagen. 

Die Modellierungsschritte enthalten auch Code zum Trainieren, Evaluieren und Speichern jedes Modelltyps. Das Thema überschneidet sich teilweise mit [Data exploration and modeling with Spark](machine-learning-data-science-spark-data-exploration-modeling.md) (Durchsuchen von Daten und Modellierung mit Spark), umfasst jedoch auch die Kreuzvalidierung in Verbindung mit Hyperparameter-Sweeping zum Trainieren optimal präziser Klassifizierungs- und Regressionsmodelle verwendet.

Die **Kreuzvalidierung** ist eine Technik, mit der bewertet wird, wie gut ein Modell, das mit einem bekannten Datensatz trainiert wurde, verallgemeinern kann, um die Features von Datensätzen, mit denen es nicht trainiert wurde, vorherzusagen. Der Grundgedanke hinter dieser Technik ist, dass ein Modell mit bekannten Daten trainiert wird und die Genauigkeit seiner Vorhersagen dann mit einem unabhängigen Datensatz geprüft wird. Eine gängige Implementierung besteht darin, den Datensatz k-fach aufzuteilen und das Modell daraufhin im Roundrobin-Verfahren mit allen Teilmengen außer einer zu trainieren.

Die **Hyperparameteroptimierung** besteht in dem Problem, einen Hyperparametersatz für einen Lernalgorithmus auszuwählen, in der Regel um eine Leistungskennzahl des Algorithmus in Bezug auf einen unabhängigen Datensatz zu optimieren. **Hyperparameter** sind Werte, die außerhalb des Trainingsverfahrens des Modells festgelegt werden müssen. Annahmen über diese Werte können sich auf die Flexibilität und Genauigkeit der Modelle auswirken. Entscheidungsstrukturen umfassen Hyperparameter, wie z.B. die gewünschte Tiefe und Anzahl der Blätter in der Struktur. Support Vector Machines (SVMs) erfordern es, Bedingungen für Abzüge bei Fehlklassifikationen festzulegen.

Eine hier häufig verwendete Methode für die Hyperparameteroptimierung besteht in der Rastersuche bzw. im **Parameter-Sweeping**. Dabei werden die Werte einer bestimmten Teilmenge des Hyperparameterraums für einen Lernalgorithmus umfassend durchsucht. Die Kreuzvalidierung kann eine Leistungsmetrik liefern, um die besten Ergebnisse des Rastersuchalgorithmus auszusortieren. Zusammen mit dem Hyperparameter-Sweeping ermöglicht es die Kreuzvalidierung, Probleme wie eine Überanpassung eines Modells an die Trainingsdaten einzuschränken, damit das Modell weiterhin auf den allgemeinen Datensatz, aus dem die Trainingsdaten extrahiert wurden, angewandt werden kann.

Die Modelle, die wir verwenden, umfassen logistische und lineare Regression, zufällige Gesamtstrukturen und Gradient-Boosted-Strukturen:

- [Lineare Regression mit SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) ist ein lineares Regressionsmodell, das eine SGD-Methode (Stochastic Gradient Descent, stochastisches Gradientenverfahren) zur Optimierung und Featureskalierung verwendet, um die gezahlten Trinkgeldbeträge vorherzusagen. 
- [Logistische Regression mit LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) oder „Logit“-Regression ist ein Regressionsmodell, das verwendet werden kann, wenn die abhängige Variable zur Datenklassifizierung kategorisch ist. LBFGS ist ein quasi-Newtonscher Optimierungsalgorithmus, der sich dem Broyden-Fletcher-Goldfarb-Shanno-Algorithmus (BFGS) unter Verwendung einer begrenzten Menge an Arbeitsspeicher annähert und häufig im Machine Learning verwendet wird.
- [Zufällige Gesamtstrukturen](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) sind Ensembles von Entscheidungsstrukturen. In ihnen sind viele Entscheidungsstrukturen kombiniert, um das Risiko der Überanpassung zu verringern. Zufällige Gesamtstrukturen werden für die Regression und Klassifizierung verwendet und können kategorische Features behandeln, auf die Mehrklassenklassifizierung ausgedehnt werden, erfordern keine Featureskalierung und können Nichtlinearitäten und Featureinteraktionen erfassen. Zufällige Gesamtstrukturen zählen zu den erfolgreichsten Machine Learning-Modelle für Klassifizierung und Regression.
- [Gradient-Boosted-Strukturen](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) sind Ensembles von Entscheidungsstrukturen. GBTs trainieren Entscheidungsstrukturen iterativ, um einen Funktionsverlust zu minimieren. GBTs werden für die Regression und Klassifizierung verwendet und können kategorische Features behandeln, erfordern keine Featureskalierung und können Nichtlinearitäten und Featureinteraktionen erfassen. Sie können auch in einer Mehrklassenklassifizierung verwendet werden.

Beispiele für die Modellierung mit Kreuzvalidierung und Hyperparameter-Sweeping werden für das binäre Klassifizierungsproblem gezeigt. Einfachere Beispiele (ohne Parameter-Sweeping) finden sich im Hauptthema zu Regressionsaufgaben. Im Anhang werden jedoch auch die Validierung mit Elastic Net für die lineare Regression und die Kreuzvalidierung mit Parameter-Sweeping für eine Regression der zufälligen Gesamtstruktur veranschaulicht. **Elastic Net** ist eine regularisierte Regressionsmethode zum Anpassen linearer Regressionsmodelle, die die Metriken L1 und L2 als Abzüge für die Methoden [Lasso](https://en.wikipedia.org/wiki/Lasso%20%28statistics%29) und [Ridge](https://en.wikipedia.org/wiki/Tikhonov_regularization) kombiniert.



>[AZURE.NOTE] Obwohl das Spark MLlib-Toolkit für die Arbeit mit großen Datasets entworfen wurde, wird zur Demonstration seiner Modellierungsfunktionen der Einfachheit halber ein relativ kleines Beispiel (ca. 30 MB mit 170.000 Zeilen, etwa 0,1 % des ursprünglichen NYC-Datasets) verwendet. Diese Übung wird auf einen HDInsight-Cluster mit 2 Workerknoten (in ungefähr 10 Minuten) ausgeführt. Der gleiche Code kann mit geringfügigen Änderungen zur Verarbeitung größerer Datasets verwendet werden – mit entsprechenden Änderungen zum Zwischenspeichern von Daten im Arbeitsspeicher oder Änderung der Clustergröße.


## Voraussetzungen

Sie benötigen ein Azure-Konto und einen HDInsight Spark-Cluster der Version Spark 1.5.2 (HDI 3.3), um mit dieser exemplarischen Vorgehensweise zu beginnen. Unter [Overview of Data Science using Spark on Azure HDInsight](machine-learning-data-science-spark-overview.md) (Übersicht zu Data Science unter Verwendung von Spark unter Azure HDInsight) finden Sie diese Anforderungen, eine Beschreibung der hier verwendeten NYC-Taxi-Daten von 2013 und Anweisungen zum Ausführen von Code aus einem Jupyter-Notebook auf dem Spark-Cluster. Das **machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb**-Notebook, das die Codebeispiele in diesem Thema enthält, ist in [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Python) verfügbar.


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## Setup: Speicherorte, Bibliotheken und Spark-Kontext

Spark kann aus dem Azure Storage-Blob (auch bekannt als WASB) lesen und darin schreiben. Es können also alle Ihre vorhandenen Daten mithilfe von Spark verarbeitet und die Ergebnisse wieder im WASB gespeichert werden.

Um Modelle oder Dateien im WASB zu speichern, muss der Pfad korrekt angegeben werden. Auf den an den Spark-Cluster angefügten Standardcontainer können Sie mit einem Pfad verweisen, der mit „wasb///“ beginnt. Auf andere Speicherorte wird mit „wasb://“ verwiesen.

### Festlegen von Verzeichnispfaden für Speicherorte im WASB

Das folgende Codebeispiel gibt den Speicherort der zu lesenden Daten und den Pfad für das Modellspeicherverzeichnis an, in dem die Ausgabe des Modells gespeichert wird.

	# SET PATHS TO FILE LOCATIONS: DATA AND MODEL STORAGE

	# LOCATION OF TRAINING DATA
	taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";

	
	# SET THE MODEL STORAGE DIRECTORY PATH 
	# NOTE THAT THE FINAL BACKSLASH IN THE PATH IS NEEDED.
	modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";

	# PRINT START TIME
	import datetime
	datetime.datetime.now()

**AUSGABE**

datetime.datetime(2016, 4, 18, 17, 36, 27, 832799)


### Importieren benötigter Bibliotheken und Festlegen des Spark-Kontexts 

Legen Sie den Spark-Kontext fest, und importieren Sie die erforderlichen Bibliotheken mit dem folgenden Code.

	# LOAD PYSPARK LIBRARIES
	import pyspark
	from pyspark import SparkConf
	from pyspark import SparkContext
	from pyspark.sql import SQLContext
	%matplotlib inline
	import matplotlib
	import matplotlib.pyplot as plt
	#matplotlib.style.use('ggplot')
	from pyspark.sql import Row
	from pyspark.sql.functions import UserDefinedFunction
	from pyspark.sql.types import *
	import atexit
	from numpy import array
	import numpy as np
	import datetime
	
	# SET SPARK CONTEXT
	sc = SparkContext(conf=SparkConf().setMaster('yarn-client'))
	sqlContext = SQLContext(sc)
	atexit.register(lambda: sc.stop())
	
	sc.defaultParallelism

**AUSGABE**

4

## Datenerfassung: 

Dieser Abschnitt enthält den Code für eine Reihe erforderlicher Aufgaben zum Erfassen des Datenbeispiels, das modelliert werden soll. Lesen Sie eine verknüpfte 0,1 %-Stichprobe der Datei für Taxifahrten und Fahrpreise (gespeichert als TSV-Datei) ein, formatieren und bereinigen Sie die Daten, erstellen Sie einen Datenrahmen im Arbeitsspeicher, zwischenspeichern Sie ihn dort, und registrieren Sie ihn dann als temporäre Tabelle im SQL-Kontext.

Der erste Schritt im Data Science-Prozess ist das Erfassen der zu analysierenden Daten aus externen Quellen oder Systemen in die Umgebung, die Sie zum Durchsuchen und Modellieren von Daten nutzen. In dieser exemplarischen Vorgehensweise ist Spark diese Umgebung. Dieser Abschnitt enthält den Code zur Ausführung einer Reihe von Aufgaben:

- Erfassen des zu modellierenden Datenbeispiels
- Einlesen des Eingabedatasets (gespeichert als TSV-Datei)
- Formatieren und Bereinigen der Daten
- Erstellen und Zwischenspeichern von Objekten (RDDs oder Datenrahmen) im Arbeitsspeicher
- deren Registrierung als temporäre Tabelle im SQL-Kontext.

Hier ist der Code für die Datenerfassung.


	# RECORD START TIME
	timestart = datetime.datetime.now()
	
	# IMPORT FILE FROM PUBLIC BLOB
	taxi_train_file = sc.textFile(taxi_train_file_loc)
	
	# GET SCHEMA OF THE FILE FROM HEADER
	schema_string = taxi_train_file.first()
	fields = [StructField(field_name, StringType(), True) for field_name in schema_string.split('\t')]
	fields[7].dataType = IntegerType() #Pickup hour
	fields[8].dataType = IntegerType() # Pickup week
	fields[9].dataType = IntegerType() # Weekday
	fields[10].dataType = IntegerType() # Passenger count
	fields[11].dataType = FloatType() # Trip time in secs
	fields[12].dataType = FloatType() # Trip distance
	fields[19].dataType = FloatType() # Fare amount
	fields[20].dataType = FloatType() # Surcharge
	fields[21].dataType = FloatType() # Mta_tax
	fields[22].dataType = FloatType() # Tip amount
	fields[23].dataType = FloatType() # Tolls amount
	fields[24].dataType = FloatType() # Total amount
	fields[25].dataType = IntegerType() # Tipped or not
	fields[26].dataType = IntegerType() # Tip class
	taxi_schema = StructType(fields)
	
	# PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
	taxi_header = taxi_train_file.filter(lambda l: "medallion" in l)
	taxi_temp = taxi_train_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
	        .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
	                        float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
	                        float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))
	
	    
	# CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
	taxi_df_train_cleaned = taxi_train_df.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
	    .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
	    .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
	    .drop('direct_distance').drop('surcharge')\
	    .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )
	
	# CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
	taxi_df_train_cleaned.cache()
	taxi_df_train_cleaned.count()
	
	# REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
	taxi_df_train_cleaned.registerTempTable("taxi_train")
	
	# PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
	timeend = datetime.datetime.now()
	timedelta = round((timeend-timestart).total_seconds(), 2) 
	print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**AUSGABE**

Für die Ausführung der obigen Zelle benötigte Zeit: 17,73 Sekunden


## Durchsuchen und Visualisierung von Daten 

Nachdem die Daten in Spark eingegeben wurden, besteht der nächste Schritt im Data Science-Prozess darin, durch Durchsuchen und Visualisieren eine tiefer gehende Einsicht in die Daten zu erhalten. In diesem Abschnitt untersuchen wir die Taxidaten mit SQL-Abfragen und zeichnen Diagramme der Zielvariablen und künftigen Merkmale zur Sichtprüfung. Insbesondere stellen wir die Häufigkeit der Fahrgastzahlen bei Taxifahrten, die Häufigkeit der Trinkgeldbeträge und die Variation der Trinkgelder nach Zahlungsbetrag und -art im Diagramm dar.

### Zeichnen eines Histogramms der Häufigkeit der Fahrgastzahlen im Taxifahrtenbeispiel

Der Code sammelt die Daten in einer SQL-Abfrage und konvertiert die Ergebnisse in einen zu zeichnenden Pandas-Datenrahmen.

	# PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

	# SQL SQUERY
	sqlStatement = """
	    SELECT passenger_count, COUNT(*) as trip_counts 
	    FROM taxi_train 
	    WHERE passenger_count > 0 and passenger_count < 7
	    GROUP BY passenger_count 
	"""
	sqlResults = sqlContext.sql(sqlStatement)
	
	#CONVERT TO PANDAS DATA-FRAMES FOR PLOTTING IN PYTHON
	resultsPDDF = sqlResults.toPandas()
	
	# PLOT PASSENGER NUMBER VS. TRIP COUNTS
	x_labels = resultsPDDF['passenger_count'].values
	fig = resultsPDDF[['trip_counts']].plot(kind='bar', facecolor='lightblue')
	fig.set_xticklabels(x_labels)
	fig.set_title('Counts of trips by passenger count')
	fig.set_xlabel('Passenger counts')
	fig.set_ylabel('Trip counts')
	plt.show()

**AUSGABE**

![Fahrtenhäufigkeit nach Anzahl der Fahrgäste](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/frequency-of-trips-by-passenger-count.png)


### Zeichnen eines Histogramms des Trinkgeldbetrags und der Variation des Trinkgeldbetrags nach Anzahl der Fahrgäste und Höhe des Fahrpreises

Der Code sammelt die Daten in einer SQL-Abfrage und konvertiert die Ergebnisse in einen zu zeichnenden Pandas-Datenrahmen.

	# RECORD START TIME
	timestart = datetime.datetime.now()
	
	# SQL SQUERY
	sqlStatement = """
	    SELECT fare_amount, passenger_count, tip_amount, tipped
	    FROM taxi_train 
	    WHERE passenger_count > 0 AND passenger_count < 7
	    AND fare_amount > 0 AND fare_amount < 200
	    AND payment_type in ('CSH', 'CRD')
	    AND tip_amount > 0 AND tip_amount < 25
	"""
	sqlResults = sqlContext.sql(sqlStatement)
	
	# CONVERT TO PANDAS DATA-FRAME FOR PLOTTING IN PYTHON
	resultsPDDF= sqlResults.toPandas()
	
	# TIP BY PAYMENT TYPE AND PASSENGER COUNT
	ax1 = resultsPDDF[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
	ax1.set_title('Tip amount distribution')
	ax1.set_xlabel('Tip Amount ($)')
	ax1.set_ylabel('Counts')
	plt.suptitle('')
	plt.show()
	
	# TIP BY PASSENGER COUNT
	ax2 = resultsPDDF.boxplot(column=['tip_amount'], by=['passenger_count'])
	ax2.set_title('Tip amount ($) by Passenger count')
	ax2.set_xlabel('Passenger count')
	ax2.set_ylabel('Tip Amount ($)')
	plt.suptitle('')
	plt.show()
	
	# TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
	ax = resultsPDDF.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(resultsPDDF.passenger_count))
	ax.set_title('Tip amount by Fare amount ($)')
	ax.set_xlabel('Fare Amount')
	ax.set_ylabel('Tip Amount')
	plt.axis([-2, 120, -2, 30])
	plt.show()
	
	# PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
	timeend = datetime.datetime.now()
	timedelta = round((timeend-timestart).total_seconds(), 2) 
	print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**Ausgabe:**

![Trinkgeldbetragverteilung](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-distribution.png)

![Trinkgeldbetrag nach Anzahl der Fahrgäste](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Trinkgeldbetrag nach Höhe des Fahrpreises](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-fare-amount.png)

Für die Ausführung der obigen Zelle benötigte Zeit: 10,42 Sekunden


## Feature Engineering, Transformation und Datenvorbereitung für die Modellierung

Dieser Abschnitt enthält den Code für die Prozeduren zum Vorbereiten von Daten für die Verwendung in der ML-Modellierung und deren Beschreibung. Hier erfahren Sie, wie Sie die folgenden Aufgaben ausführen:

- Erstellen eines neuen Features durch Diskretisieren von Stunden in Verkehrszeitbuckets
- Indizieren und One-Hot-Codieren von kategorischen Features
- Erstellen von bezeichneten Punktobjekten für die Eingabe in ML-Funktionen
- Erstellen einer Zufallsunterauswahl der Daten und Aufteilung in Trainings- und Testsätze
- Featureskalierung
- Zwischenspeichern von Objekten im Arbeitsspeicher


### Erstellen eines neuen Features durch Diskretisieren von Stunden in Verkehrszeitbuckets

Dieser Code zeigt, wie Sie ein neues Feature durch Diskretisieren von Stunden in Verkehrszeitbuckets erstellen und dann den resultierenden Datenrahmen im Arbeitsspeicher zwischenspeichern. Wo robuste verteilte Datasets (Resilient Distributed Datasets, RDDs) und Datenrahmen wiederholt verwendet werden, führt das Zwischenspeichern zu verbesserten Ausführungszeiten. Folglich werden wir RDDs und Datenrahmen in mehreren Phasen der exemplarischen Vorgehensweise zwischenspeichern.

	# CREATE FOUR BUCKETS FOR TRAFFIC TIMES
	sqlStatement = """
	    SELECT *,
	    CASE
	     WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
	     WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
	     WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
	     WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
	    END as TrafficTimeBins
	    FROM taxi_train 
	"""
	taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)
	
	# CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
	taxi_df_train_with_newFeatures.cache()
	taxi_df_train_with_newFeatures.count()

**AUSGABE**

126050


### Indizieren und One-Hot-Codieren von kategorischen Features

Dieser Abschnitt beschreibt das Indizieren oder Codieren von kategorischen Features für die Eingabe in die Modellierungsfunktionen. Die Modellierungs- und Vorhersagefunktionen von MLlib erfordern, dass Features mit kategorischen Eingabedaten vor der Verwendung indiziert oder codiert werden.

Je nach Modell müssen Sie sie auf unterschiedliche Weise indizieren oder codieren. Logistische und lineare Regressionsmodelle erfordern beispielsweise One-Hot-Codierung, wobei z.B. ein Feature mit 3 Kategorien auf 3 Featurespalten erweitert werden kann, wobei jede abhängig von der Kategorie einer Beobachtung 0 oder 1 enthalten kann. MLlib bietet die Funktion [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) zur One-Hot-Codierung. Dieser Encoder ordnet eine Spalte mit Bezeichnungsindizes einer Spalte mit binären Vektoren mit höchstens einem einzigen Wert zu. Diese Codierung ermöglicht die Anwendung von Algorithmen, die Nummernwertfeatures erwarten, z.B. logistische Regression, auf kategorische Features.

Hier ist der Code zum Indizieren und Codieren von kategorischen Features:


	# RECORD START TIME
	timestart = datetime.datetime.now()
	
	# LOAD PYSPARK LIBRARIES
	from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer
	
	# INDEX AND ENCODE VENDOR_ID
	stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
	model = stringIndexer.fit(taxi_df_train_with_newFeatures) # Input data-frame is the cleaned one from above
	indexed = model.transform(taxi_df_train_with_newFeatures)
	encoder = OneHotEncoder(dropLast=False, inputCol="vendorIndex", outputCol="vendorVec")
	encoded1 = encoder.transform(indexed)
	
	# INDEX AND ENCODE RATE_CODE
	stringIndexer = StringIndexer(inputCol="rate_code", outputCol="rateIndex")
	model = stringIndexer.fit(encoded1)
	indexed = model.transform(encoded1)
	encoder = OneHotEncoder(dropLast=False, inputCol="rateIndex", outputCol="rateVec")
	encoded2 = encoder.transform(indexed)
	
	# INDEX AND ENCODE PAYMENT_TYPE
	stringIndexer = StringIndexer(inputCol="payment_type", outputCol="paymentIndex")
	model = stringIndexer.fit(encoded2)
	indexed = model.transform(encoded2)
	encoder = OneHotEncoder(dropLast=False, inputCol="paymentIndex", outputCol="paymentVec")
	encoded3 = encoder.transform(indexed)
	
	# INDEX AND TRAFFIC TIME BINS
	stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
	model = stringIndexer.fit(encoded3)
	indexed = model.transform(encoded3)
	encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
	encodedFinal = encoder.transform(indexed)
	
	# PRINT ELAPSED TIME
	timeend = datetime.datetime.now()
	timedelta = round((timeend-timestart).total_seconds(), 2) 
	print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**AUSGABE**

Für die Ausführung der obigen Zelle benötigte Zeit: 1,22 Sekunden


### Erstellen von bezeichneten Punktobjekten für die Eingabe in ML-Funktionen

Dieser Abschnitt enthält Code, der zeigt, wie Sie kategorische Textdaten als bezeichneten Punktdatentyp und so codieren, dass sie zum Trainieren und Testen logistischer Regression gemäß MLlib und anderer Klassifizierungsmodelle verwendet werden können. Bezeichnete Punktobjekte sind robuste verteilte Datasets (RDD), die in einer Weise formatiert sind, die von den meisten ML-Algorithmen in MLlib als Eingabedaten benötigt wird. Ein [bezeichneter Punkt](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) ist ein lokaler Vektor, entweder dicht oder platzsparend, der mit einer Bezeichnung/Antwort verknüpft ist.


	# FUNCTIONS FOR BINARY CLASSIFICATION

	# LOAD PYSPARK LIBRARIES
	from pyspark.mllib.regression import LabeledPoint
	from numpy import array

	# INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
	def parseRowIndexingBinary(line):
	    features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.pickup_hour, line.weekday,
	                         line.passenger_count, line.trip_time_in_secs, line.trip_distance, line.fare_amount])
	    labPt = LabeledPoint(line.tipped, features)
	    return  labPt
	
	# ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
	def parseRowOneHotBinary(line):
	    features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
	                                        line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
	                               line.vendorVec.toArray(), line.rateVec.toArray(), line.paymentVec.toArray()), axis=0)
	    labPt = LabeledPoint(line.tipped, features)
	    return  labPt


	# FUNCTIONS FOR REGRESSION WITH TIP AMOUNT AS TARGET VARIABLE

	# ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
	def parseRowIndexingRegression(line):
	    features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
	                         line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
	                         line.trip_distance, line.fare_amount])
	    labPt = LabeledPoint(line.tip_amount, features)
	    return  labPt
	
	# INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
	def parseRowOneHotRegression(line):
	    features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
	                                        line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
	                                        line.vendorVec.toArray(), line.rateVec.toArray(), 
	                                        line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
	    labPt = LabeledPoint(line.tip_amount, features)
	    return  labPt



### Erstellen einer Zufallsunterauswahl der Daten und Aufteilung in Trainings- und Testsätze

Dieser Code erstellt eine zufällige Stichprobe der Daten (25 % werden hier verwendet). Obwohl es aufgrund der Größe des Datasets in diesem Beispiel nicht erforderlich ist, zeigen wir Ihnen, wie Sie hier Stichproben erfassen können, damit Sie dieses Verfahren bei Bedarf zur Lösung eigener Probleme verwenden können. Bei großen Stichproben können Sie so beim Trainieren von Modellen deutlich Zeit sparen. Als Nächstes teilen wir die Stichprobe in einen Trainingsteil (hier 75 %) und einen Testteil (hier 25 %) zur Klassifizierung und Regressionsmodellierung.


	# RECORD START TIME
	timestart = datetime.datetime.now()
	
	# SPECIFY SAMPLING AND SPLITTING FRACTIONS
	from pyspark.sql.functions import rand
	
	samplingFraction = 0.25;
	trainingFraction = 0.75; testingFraction = (1-trainingFraction);
	seed = 1234;
	encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)
	
	# SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST, WITH A RANDOM COLUMN ADDED FOR DOING CV (SHOWN LATER)
	# INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS
	dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
	trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);
	
	# CACHE TRAIN AND TEST DATA
	trainData.cache()
	testData.cache()
	
	# FOR BINARY CLASSIFICATION TRAINING AND TESTING
	indexedTRAINbinary = trainData.map(parseRowIndexingBinary)
	indexedTESTbinary = testData.map(parseRowIndexingBinary)
	oneHotTRAINbinary = trainData.map(parseRowOneHotBinary)
	oneHotTESTbinary = testData.map(parseRowOneHotBinary)
	
	# FOR REGRESSION TRAINING AND TESTING
	indexedTRAINreg = trainData.map(parseRowIndexingRegression)
	indexedTESTreg = testData.map(parseRowIndexingRegression)
	oneHotTRAINreg = trainData.map(parseRowOneHotRegression)
	oneHotTESTreg = testData.map(parseRowOneHotRegression)
	
	# PRINT ELAPSED TIME
	timeend = datetime.datetime.now()
	timedelta = round((timeend-timestart).total_seconds(), 2) 
	print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**AUSGABE**

Für die Ausführung der obigen Zelle benötigte Zeit: 0,4 Sekunden


### Featureskalierung

Featureskalierung, auch bekannt als Datennormalisierung, stellt sicher, dass Features mit weit verteilten Werten keine übermäßige Gewichtung in der Zielfunktion erhalten. Der Code für die Featureskalierung verwendet [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) zum Skalieren der Features auf Einheitenvarianz. Er wird von MLlib für die Verwendung bei der linearen Regression mit dem stochastischen Gradientenverfahren (SGD), einem beliebten Algorithmus für das Training einer Vielzahl anderer Machine Learning-Modelle, z. B. normalisierter Regressionen oder Support Vector Machines (SVM), bereitgestellt.


>[AZURE.NOTE] Wir haben festgestellt, dass der LinearRegressionWithSGD-Algorithmus für die Featureskalierung zu empfindlich ist.


Im Folgenden finden Sie den Code zum Skalieren von Variablen für die Verwendung mit dem regularisierten linearen SGD-Algorithmus.

	# RECORD START TIME
	timestart = datetime.datetime.now()
	
	# LOAD PYSPARK LIBRARIES
	from pyspark.mllib.regression import LabeledPoint
	from pyspark.mllib.linalg import Vectors
	from pyspark.mllib.feature import StandardScaler, StandardScalerModel
	from pyspark.mllib.util import MLUtils
	
	# SCALE VARIABLES FOR REGULARIZED LINEAR SGD ALGORITHM
	label = oneHotTRAINreg.map(lambda x: x.label)
	features = oneHotTRAINreg.map(lambda x: x.features)
	scaler = StandardScaler(withMean=False, withStd=True).fit(features)
	dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
	oneHotTRAINregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))
	
	label = oneHotTESTreg.map(lambda x: x.label)
	features = oneHotTESTreg.map(lambda x: x.features)
	scaler = StandardScaler(withMean=False, withStd=True).fit(features)
	dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
	oneHotTESTregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))
	
	# PRINT ELAPSED TIME
	timeend = datetime.datetime.now()
	timedelta = round((timeend-timestart).total_seconds(), 2) 
	print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**AUSGABE**

Für die Ausführung der obigen Zelle benötigte Zeit: 7,33 Sekunden

### Zwischenspeichern von Objekten im Arbeitsspeicher

Die Zeit zum Trainieren und Testen von ML-Algorithmen kann durch Zwischenspeichern der Eingabedatenrahmen-Objekte für Klassifizierung, Regression und skalierte Features verwendet werden.

	# RECORD START TIME
	timestart = datetime.datetime.now()
	
	# FOR BINARY CLASSIFICATION TRAINING AND TESTING
	indexedTRAINbinary.cache()
	indexedTESTbinary.cache()
	oneHotTRAINbinary.cache()
	oneHotTESTbinary.cache()
	
	# FOR REGRESSION TRAINING AND TESTING
	indexedTRAINreg.cache()
	indexedTESTreg.cache()
	oneHotTRAINreg.cache()
	oneHotTESTreg.cache()
	
	# SCALED FEATURES
	oneHotTRAINregScaled.cache()
	oneHotTESTregScaled.cache()
	
	# PRINT ELAPSED TIME
	timeend = datetime.datetime.now()
	timedelta = round((timeend-timestart).total_seconds(), 2) 
	print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**Ausgabe:**

Für die Ausführung der obigen Zelle benötigte Zeit: 0,11 Sekunden


## Vorhersage, ob ein Trinkgeld bezahlt wird, mit Modellen zur binären Klassifizierung

Dieser Abschnitt beschreibt die Verwendung von drei Modellen für die Aufgabe der binären Klassifizierung der Vorhersage, ob für eine Taxifahrt ein Trinkgeld bezahlt wird. Die präsentierten Modelle sind:

- Logistische Regression 
- Zufällige Gesamtstruktur
- Gradient-Boosted-Strukturen

Jeder Codeabschnitt zur Modellerstellung ist in Schritte unterteilt:

1. **Modelltrainingsdaten** mit einem Parametersatz
2. **Modellevaluierung** an einem Testdataset mit Metriken
3. **Speichern des Modells** im Blob für die zukünftige Nutzung

Die Kreuzvalidierung mit Parameter-Sweeping kann auf zweierlei Weise ausgeführt werden:

1. Mithilfe von **allgemeinem** benutzerdefinierten Code, der auf jeden Algorithmus in MLlib und auf all Parametersätze in einem Algorithmus angewandt werden kann. 
1. Mithilfe der **PySpark CrossValidator-Pipelinefunktion**. CrossValidator ist zwar praktisch, weist erfahrungsgemäß aber auch einige Beschränkungen für Spark 1.5.0 auf: 

	- Pipelinemodelle können für die zukünftige Verwendung nicht gespeichert/beibehalten werden.
	- Kann nicht für jeden Parameter in einem Modell verwendet werden.
	- Kann nicht für jeden MLlib-Algorithmus verwendet werden.


### Allgemeine Kreuzvalidierung und Hyperparameter-Sweeping mit dem logistischen Regressionsalgorithmus für die binäre Klassifizierung

Der Code in diesem Abschnitt zeigt, wie ein logistisches Regressionsmodell, das im NYC-Taxifahrten- und Fahrpreisedataset vorhersagt, ob ein Trinkgeld für eine Fahrt gezahlt wird, mit [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) trainiert, evaluiert und gespeichert werden kann. Das Modell wird mithilfe der Kreuzvalidierung und des Hyperparameter-Sweeping trainiert. Dies wurde mit benutzerdefiniertem Code implementiert, der auf alle Lernalgorithmen in MLlib angewandt werden kann.


>[AZURE.NOTE] Die Ausführung dieser benutzerdefinierten Kreuzvalidierung kann einige Minuten in Anspruch nehmen.


	# LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

	# RECORD START TIME
	timestart = datetime.datetime.now()
	
	from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
	from pyspark.mllib.evaluation import BinaryClassificationMetrics
	
	# CREATE PARAMETER GRID FOR LOGISTIC REGRESSION PARAMETER SWEEP
	from sklearn.grid_search import ParameterGrid
	grid = [{'regParam': [0.01, 0.1], 'iterations': [5, 10], 'regType': ["l1", "l2"], 'tolerance': [1e-3, 1e-4]}]
	paramGrid = list(ParameterGrid(grid))
	numModels = len(paramGrid)
	
	# SET NUM FOLDS AND NUM PARAMETER SETS TO SWEEP ON
	nFolds = 3;
	h = 1.0 / nFolds;
	metricSum = np.zeros(numModels);
	
	# BEGIN CV WITH PARAMETER SWEEP
	for i in range(nFolds):
	    # Create training and x-validation sets
	    validateLB = i * h
	    validateUB = (i + 1) * h
	    condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
	    validation = trainData.filter(condition)
	    # Create labeled points from data-frames
	    if i > 0:
	        trainCVLabPt.unpersist()
	        validationLabPt.unpersist()
	    trainCV = trainData.filter(~condition)
	    trainCVLabPt = trainCV.map(parseRowOneHotBinary)
	    trainCVLabPt.cache()
	    validationLabPt = validation.map(parseRowOneHotBinary)
	    validationLabPt.cache()
	    # For parameter sets compute metrics from x-validation
	    for j in range(numModels):
	        regt = paramGrid[j]['regType']
	        regp = paramGrid[j]['regParam']
	        iters = paramGrid[j]['iterations']
	        tol = paramGrid[j]['tolerance']
	        # Train logistic regression model with hypermarameter set
	        model = LogisticRegressionWithLBFGS.train(trainCVLabPt, regType=regt, iterations=iters,  
	                                                  regParam=regp, tolerance = tol, intercept=True)
	        predictionAndLabels = validationLabPt.map(lambda lp: (float(model.predict(lp.features)), lp.label))
	        # Use ROC-AUC as accuracy metrics
	        validMetrics = BinaryClassificationMetrics(predictionAndLabels)
	        metric = validMetrics.areaUnderROC
	        metricSum[j] += metric
	
	avgAcc = metricSum / nFolds;
	bestParam = paramGrid[np.argmax(avgAcc)];
	
	# UNPERSIST OBJECTS
	trainCVLabPt.unpersist()
	validationLabPt.unpersist()
	    
	# TRAIN ON FULL TRAIING SET USING BEST PARAMETERS FROM CV/PARAMETER SWEEP
	logitBest = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, regType=bestParam['regType'], 
	                                              iterations=bestParam['iterations'], 
	                                              regParam=bestParam['regParam'], tolerance = bestParam['tolerance'], 
	                                              intercept=True)
	
	timeend = datetime.datetime.now()
	timedelta = round((timeend-timestart).total_seconds(), 2) 
	print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**AUSGABE**

Für die Ausführung der obigen Zelle benötigte Zeit: 160,47 Sekunden


**Evaluieren des binären Klassifizierungsmodells mit Standardmetriken und Ausgabe der ROC-Kurve**

Der Code in diesem Abschnitt zeigt, wie ein logistisches Regressionsmodell mit einem Testdataset evaluiert wird, einschließlich Ausgabe der ROC-Kurve.


	# RECORD START TIME
	timestart = datetime.datetime.now()
	
	# LOAD PYSPARK LIBRARIES
	from sklearn.metrics import roc_curve,auc
	from pyspark.mllib.evaluation import BinaryClassificationMetrics
	from pyspark.mllib.evaluation import MulticlassMetrics
	
	# PREDICT ON TEST DATA WITH BEST/FINAL MODEL
	predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitBest.predict(lp.features)), lp.label))
	
	# INSTANTIATE METRICS OBJECT
	metrics = BinaryClassificationMetrics(predictionAndLabels)

	# AREA UNDER PRECISION-RECALL CURVE
	print("Area under PR = %s" % metrics.areaUnderPR)

	# AREA UNDER ROC CURVE
	print("Area under ROC = %s" % metrics.areaUnderROC)
	metrics = MulticlassMetrics(predictionAndLabels)

	# OVERALL STATISTICS
	precision = metrics.precision()
	recall = metrics.recall()
	f1Score = metrics.fMeasure()
	print("Summary Stats")
	print("Precision = %s" % precision)
	print("Recall = %s" % recall)
	print("F1 Score = %s" % f1Score)
	
	
	# CREATE A PANDAS DATA-FRAME AND PLOT ROC-CURVE, FROM PREDICTED PROBS AND LABELS                                     
	logitBest.clearThreshold() # This clears threshold for classification (0.5) and outputs probabilities
	predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitBest.predict(lp.features)), lp.label))
	predictionAndLabelsDF = predictionAndLabels.toDF()
	test_predictions = predictionAndLabelsDF.toPandas()
	predictions_pddf = test_predictions.rename(columns={'_1': 'probability', '_2': 'label'})
	
	prob = predictions_pddf["probability"] 
	fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
	roc_auc = auc(fpr, tpr)
	
	# PLOT ROC CURVE
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
	
	# PRINT ELAPSED TIME
	timeend = datetime.datetime.now()
	timedelta = round((timeend-timestart).total_seconds(), 2) 
	print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**AUSGABE**

Area under PR = 0.985319161941

Area under ROC = 0.983511076103

Summary Stats

Precision = 0.984187223276

Recall = 0.984187223276

F1 Score = 0.984187223276

![ROC-Kurve der logistischen Regression für die allgemeine Methode](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/logistic-regression-roc-curve.png)

Für die Ausführung der obigen Zelle benötigte Zeit: 5,02 Sekunden


**Beibehalten des Modells im Blob für die zukünftige Nutzung**

Der Code in diesem Abschnitt zeigt, wie das logistische Regressionsmodell für die Nutzung gespeichert wird.

	# RECORD START TIME
	timestart = datetime.datetime.now()
	
	# LOAD PYSPARK LIBRARIES
	from pyspark.mllib.classification import LogisticRegressionModel
	
	# PERSIST MODEL
	datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
	logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
	dirfilename = modelDir + logisticregressionfilename;
	
	logitBest.save(sc, dirfilename);
	
	# PRINT ELAPSED TIME
	timeend = datetime.datetime.now()
	timedelta = round((timeend-timestart).total_seconds(), 2) 
	print "Time taken to execute above cell: " + str(timedelta) + " seconds";


**AUSGABE**

Für die Ausführung der obigen Zelle benötigte Zeit: 9,96 Sekunden


### Verwenden der CrossValidator-Pipeline-Funktion von MLlib mit dem LogisticRegression-Modell (elastische Regression)

Der Code in diesem Abschnitt zeigt, wie ein logistisches Regressionsmodell, das im NYC-Taxifahrten- und Fahrpreisedataset vorhersagt, ob ein Trinkgeld für eine Fahrt gezahlt wird, mit [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) trainiert, evaluiert und gespeichert werden kann. Das Modell wird mithilfe der Kreuzvalidierung und des Hyperparameter-Sweeping trainiert. Dies wurde mit der MLlib CrossValidator-Pipeline-Funktion für die Kreuzvalidierung mit Hyperparameter-Sweeping implementiert.


>[AZURE.NOTE] Die Ausführung dieses MLlib-Kreuzvalidierungscode kann einige Minuten in Anspruch nehmen.


	# RECORD START TIME
	timestart = datetime.datetime.now()
	
	# LOAD PYSPARK LIBRARIES
	from pyspark.ml.classification import LogisticRegression
	from pyspark.ml import Pipeline
	from pyspark.ml.evaluation import BinaryClassificationEvaluator
	from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
	from sklearn.metrics import roc_curve,auc
	
	# DEFINE ALGORITHM / MODEL
	lr = LogisticRegression()
	
	# DEFINE GRID PARAMETERS
	paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
	                              .addGrid(lr.maxIter, (5, 10))\
	                              .addGrid(lr.tol, (1e-4, 1e-5))\
	                              .addGrid(lr.elasticNetParam, (0.25,0.75))\
	                              .build()
	
	# DEFINE CV WITH PARAMETER SWEEP
	cv = CrossValidator(estimator= lr,
	                    estimatorParamMaps=paramGrid,
	                    evaluator=BinaryClassificationEvaluator(),
	                    numFolds=3)
	
	# CONVERT TO DATA-FRAME: THIS WILL NOT RUN ON RDDs
	trainDataFrame = sqlContext.createDataFrame(oneHotTRAINbinary, ["features", "label"])
	
	# TRAIN WITH CROSS-VALIDATION
	cv_model = cv.fit(trainDataFrame)
	

	## PREDICT AND EVALUATE ON TEST DATA-SET

	# USE TEST DATASET FOR PREDICTION
	testDataFrame = sqlContext.createDataFrame(oneHotTESTbinary, ["features", "label"])
	test_predictions = cv_model.transform(testDataFrame)
	
	# CONVERT RTO PANDAS DATA-FRAME FOR CALCULATING AND PLOTTING ROC CURVE
	predictions_pddf = test_predictions.toPandas()
	predictions_pddf.dtypes
	prob = [x[1] for x in predictions_pddf["probability"]]
	fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
	roc_auc = auc(fpr, tpr)
	
	# PLOT ROC CURVE
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
	
	# PRINT ELAPSED TIME
	timeend = datetime.datetime.now()
	timedelta = round((timeend-timestart).total_seconds(), 2) 
	print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**AUSGABE**

![ROC-Kurve der logistischen Regression mithilfe des CrossValidator von MLlib](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/mllib-crossvalidator-roc-curve.png)

Für die Ausführung der obigen Zelle benötigte Zeit: 118,25 Sekunden


### Klassifizierung der zufälligen Gesamtstruktur

Der Code in diesem Abschnitt zeigt, wie eine Regression der zufälligen Gesamtstruktur, die im NYC-Taxifahrten- und Fahrpreisedataset vorhersagt, ob ein Trinkgeld für eine Fahrt gezahlt wird, trainiert, evaluiert und gespeichert werden kann.


	# RECORD START TIME
	timestart = datetime.datetime.now()
	
	# LOAD PYSPARK LIBRARIES
	from pyspark.mllib.tree import RandomForest, RandomForestModel
	from pyspark.mllib.util import MLUtils
	from pyspark.mllib.evaluation import BinaryClassificationMetrics
	from pyspark.mllib.evaluation import MulticlassMetrics
	
	# SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
	categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
	
	# TRAIN RANDOMFOREST MODEL
	rfModel = RandomForest.trainClassifier(indexedTRAINbinary, numClasses=2, 
	                                       categoricalFeaturesInfo=categoricalFeaturesInfo,
	                                       numTrees=25, featureSubsetStrategy="auto",
	                                       impurity='gini', maxDepth=5, maxBins=32)
	## UN-COMMENT IF YOU WANT TO PRING TREES
	#print('Learned classification forest model:')
	#print(rfModel.toDebugString())
	
	# PREDICT ON TEST DATA AND EVALUATE
	predictions = rfModel.predict(indexedTESTbinary.map(lambda x: x.features))
	predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
	
	# AREA UNDER ROC CURVE
	metrics = BinaryClassificationMetrics(predictionAndLabels)
	print("Area under ROC = %s" % metrics.areaUnderROC)
	
	# PERSIST MODEL IN BLOB
	datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
	rfclassificationfilename = "RandomForestClassification_" + datestamp;
	dirfilename = modelDir + rfclassificationfilename;
	
	rfModel.save(sc, dirfilename);
	
	# PRINT ELAPSED TIME
	timeend = datetime.datetime.now()
	timedelta = round((timeend-timestart).total_seconds(), 2) 
	print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**AUSGABE**

Area under ROC = 0.985240932843

Für die Ausführung der obigen Zelle benötigte Zeit: 22,9 Sekunden


### Klassifizierung von Gradient-Boosted-Strukturen

Der Code in diesem Abschnitt zeigt, wie ein Gradient-Boosted-Strukturmodell, das vorhersagt, ob ein Trinkgeld für eine Fahrt im NYC-Taxifahrten- und Fahrpreisedataset gezahlt wird, trainiert, evaluiert und gespeichert werden kann.


	# RECORD START TIME
	timestart = datetime.datetime.now()
	
	# LOAD PYSPARK LIBRARIES
	from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
	
	# SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
	categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
	
	gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo,
	                                                numIterations=10)
	## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
	#print('Learned classification GBT model:')
	#print(bgtModel.toDebugString())
	
	# PREDICT ON TEST DATA AND EVALUATE
	predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
	predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
	
	# Area under ROC curve
	metrics = BinaryClassificationMetrics(predictionAndLabels)
	print("Area under ROC = %s" % metrics.areaUnderROC)
	
	# PERSIST MODEL IN A BLOB
	datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
	btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp;
	dirfilename = modelDir + btclassificationfilename;
	
	gbtModel.save(sc, dirfilename)
	
	# PRINT ELAPSED TIME
	timeend = datetime.datetime.now()
	timedelta = round((timeend-timestart).total_seconds(), 2) 
	print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**AUSGABE**

Area under ROC = 0.985240932843

Für die Ausführung der obigen Zelle benötigte Zeit: 22,41 Sekunden


## Vorhersage von Trinkgeldbeträgen mit Regressionsmodellen (ohne Kreuzvalidierung)

Dieser Abschnitt zeigt Ihnen die Verwendung von drei Modellen für die Regressionsaufgabe der Vorhersage des Trinkgeldbetrags, der für eine Taxifahrt gezahlt wird, auf der Basis anderer Trinkgeldfeatures. Die präsentierten Modelle sind:

- Normalisierte lineare Regression
- Zufällige Gesamtstruktur
- Gradient-Boosted-Strukturen

Diese Modelle wurden in der Einführung beschrieben. Jeder Codeabschnitt zur Modellerstellung ist in Schritte unterteilt:

1. **Modelltrainingsdaten** mit einem Parametersatz
2. **Modellevaluierung** an einem Testdataset mit Metriken
3. **Speichern des Modells** im Blob für die zukünftige Nutzung   


>AZURE-HINWEIS: Die Kreuzvalidierung wird nicht mit den drei Regressionsmodellen in diesem Abschnitt verwendet. Ein Beispiel dazu, wie die Kreuzvalidierung mit Elastic Net für die lineare Regression verwendet wird, finden Sie im Anhang zu diesem Thema.


>AZURE-HINWEIS: Nach unserer Erfahrung können Probleme mit der Konvergenz von LinearRegressionWithSGD-Modellen auftreten, und Parameter müssen sorgfältig geändert/optimiert werden, um ein gültiges Modell zu erhalten. Das Skalieren von Variablen ist bei der Konvergenz hilfreich. Auch Elastic Net-Regression (s. Anhang zu diesem Thema) kann für eine verbesserte Konvergenz eingesetzt werden.


### Lineare Regression mit SGD

Der Code in diesem Abschnitt zeigt, wie skalierte Features zum Trainieren einer linearen Regression verwendet werden, die das stochastische Gradientenverfahren (SGD) für die Optimierung verwendet, und wie das Modell bewertet, evaluiert und in Azure-Blobspeicher (WASB) gespeichert wird.


	# LINEAR REGRESSION WITH SGD 

	# RECORD START TIME
	timestart = datetime.datetime.now()
	
	# LOAD PYSPARK LIBRARIES
	from pyspark.mllib.regression import LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel
	from pyspark.mllib.evaluation import RegressionMetrics
	from scipy import stats
	
	# USE SCALED FEATURES TO TRAIN MODEL
	linearModel = LinearRegressionWithSGD.train(oneHotTRAINregScaled, iterations=100, step = 0.1, regType='l2', regParam=0.1, intercept = True)
	
	# SCORE ON SCALED TEST DATA-SET & EVALUATE
	predictionAndLabels = oneHotTESTregScaled.map(lambda lp: (float(linearModel.predict(lp.features)), lp.label))
	testMetrics = RegressionMetrics(predictionAndLabels)
	
	print("RMSE = %s" % testMetrics.rootMeanSquaredError)
	print("R-sqr = %s" % testMetrics.r2)
	
	# SAVE MODEL IN BLOB
	datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
	linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
	dirfilename = modelDir + linearregressionfilename;
	
	linearModel.save(sc, dirfilename)
	
	# PRINT ELAPSED TIME
	timeend = datetime.datetime.now()
	timedelta = round((timeend-timestart).total_seconds(), 2) 
	print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**AUSGABE**

RMSE = 1,29395294535

R-sqr = 0,588405443258

Für die Ausführung der obigen Zelle benötigte Zeit: 36,14 Sekunden


### Regression der zufälligen Gesamtstruktur

Der Code in diesem Abschnitt zeigt, wie ein zufälliges Gesamtstrukturmodell, das den Trinkgeldbetrag für die NYC-Taxifahrtendaten vorhersagt, trainiert, evaluiert und gespeichert werden kann.


>[AZURE.NOTE] Kreuzvalidierung mit Parameter-Sweeping mithilfe von benutzerdefinierten Code finden Sie im Anhang.


	#PREDICT TIP AMOUNTS USING RANDOM FOREST

	# RECORD START TIME
	timestart= datetime.datetime.now()
	
	# LOAD PYSPARK LIBRARIES
	from pyspark.mllib.tree import RandomForest, RandomForestModel
	from pyspark.mllib.util import MLUtils
	from pyspark.mllib.evaluation import RegressionMetrics
	
	
	# TRAIN MODEL
	categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
	rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
	                                    numTrees=25, featureSubsetStrategy="auto",
	                                    impurity='variance', maxDepth=10, maxBins=32)
	# UN-COMMENT IF YOU WANT TO PRING TREES
	#print('Learned classification forest model:')
	#print(rfModel.toDebugString())
	
	# PREDICT AND EVALUATE ON TEST DATA-SET
	predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
	predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)
	
	testMetrics = RegressionMetrics(predictionAndLabels)
	print("RMSE = %s" % testMetrics.rootMeanSquaredError)
	print("R-sqr = %s" % testMetrics.r2)
	
	# SAVE MODEL IN BLOB
	datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
	rfregressionfilename = "RandomForestRegression_" + datestamp;
	dirfilename = modelDir + rfregressionfilename;
	
	rfModel.save(sc, dirfilename);
	
	# PRINT ELAPSED TIME
	timeend = datetime.datetime.now()
	timedelta = round((timeend-timestart).total_seconds(), 2) 
	print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**AUSGABE**

RMSE = 0,962262172157

R-sqr = 0,69142848223

Für die Ausführung der obigen Zelle benötigte Zeit: 29,3 Sekunden


### Gradient-Boosted-Strukturenregression

Der Code in diesem Abschnitt zeigt, wie ein Gradient-Boosted-Strukturmodell, das den Trinkgeldbetrag für die NYC-Taxifahrtendaten vorhersagt, trainiert, evaluiert und gespeichert werden kann.


	#PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

	# RECORD START TIME
	timestart= datetime.datetime.now()
	
	# LOAD PYSPARK LIBRARIES
	from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
	from pyspark.mllib.util import MLUtils
	
	# TRAIN MODEL
	categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
	gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
	                                                numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)
	
	# EVALUATE A TEST DATA-SET
	predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
	predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)
	
	testMetrics = RegressionMetrics(predictionAndLabels)
	print("RMSE = %s" % testMetrics.rootMeanSquaredError)
	print("R-sqr = %s" % testMetrics.r2)
	
	# PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES
	test_predictions= sqlContext.createDataFrame(predictionAndLabels)
	test_predictions_pddf = test_predictions.toPandas()
	
	ax = test_predictions_pddf.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
	fit = np.polyfit(test_predictions_pddf['_1'], test_predictions_pddf['_2'], deg=1)
	ax.set_title('Actual vs. Predicted Tip Amounts ($)')
	ax.set_xlabel("Actual")
	ax.set_ylabel("Predicted")
	ax.plot(test_predictions_pddf['_1'], fit[0] * test_predictions_pddf['_1'] + fit[1], color='magenta')
	plt.axis([-1, 20, -1, 20])
	plt.show(ax)
	
	# SAVE MODEL IN BLOB
	datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
	btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
	dirfilename = modelDir + btregressionfilename;
	
	gbtModel.save(sc, dirfilename)
	
	# PRINT ELAPSED TIME
	timeend = datetime.datetime.now()
	timedelta = round((timeend-timestart).total_seconds(), 2) 
	print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**AUSGABE**

RMSE = 0.962160568829

R-Sqr = 0.717354800581

![Actual-vs-predicted-tip-amounts](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/actual-vs-predicted-tips.png)


## Anhang: Zusätzliche Regressionsaufgaben mithilfe der Kreuzvalidierung mit Parameter-Sweeping

Dieser Anhang enthält Code, der zeigt, wie die Kreuzvalidierung mithilfe von Elastic Net für die lineare Regression und mit Parameter-Sweeping mithilfe von benutzerdefiniertem Code für die Regression der zufälligen Gesamtstruktur verwendet wird.


### Kreuzvalidierung mithilfe von Elastic Net für die lineare Regression

Der Code in diesem Abschnitt zeigt wie die Kreuzvalidierung mithilfe von Elastic Net für die lineare Regression durchgeführt wird und wie das Modell in Bezug auf Testdaten evaluiert wird.

	###  CV USING ELASTIC NET FOR LINEAR REGRESSION

	# RECORD START TIME
	timestart = datetime.datetime.now()
	
	# LOAD PYSPARK LIBRARIES
	from pyspark.ml.regression import LinearRegression
	from pyspark.ml import Pipeline
	from pyspark.ml.evaluation import RegressionEvaluator
	from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
	
	# Define algo / model
	lr = LinearRegression()
	
	# Define grid parameters
	paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
	                              .addGrid(lr.maxIter, (5, 10))\
	                              .addGrid(lr.tol, (1e-4, 1e-5))\
	                              .addGrid(lr.elasticNetParam, (0.25,0.75))\
	                              .build() 
	
	# Define pipeline, in this case, simply the modeling (without any transformations etc.)
	pipeline = Pipeline(stages=[lr])
	
	# Define CV with parameter sweep
	cv = CrossValidator(estimator= lr,
	                    estimatorParamMaps=paramGrid,
	                    evaluator=RegressionEvaluator(),
	                    numFolds=3)
	
	# Convert to data-frame, as CrossValidator won't run on RDDs
	trainDataFrame = sqlContext.createDataFrame(oneHotTRAINreg, ["features", "label"])
	
	# Train with cross-validation
	cv_model = cv.fit(trainDataFrame)
	
	timeend = datetime.datetime.now()
	timeend-timestart
	

	# EVALUATE MODEL ON TEST SET
	testDataFrame = sqlContext.createDataFrame(oneHotTESTreg, ["features", "label"])
	
	# MAKE PREDICTIONS ON TEST DOCUMENTS 
	#THE cvModel USES THE BEST MODEL FOUND (lrModel).
	test_predictions = cv_model.transform(testDataFrame)
	predictions_pddf = test_predictions.toPandas()
	
	corstats = stats.linregress(predictions_pddf['label'],predictions_pddf['prediction'])
	r2 = (corstats[2]*corstats[2])
	print("R-sqr = %s" % r2)
	
	# PRINT ELAPSED TIME
	timeend = datetime.datetime.now()
	timedelta = round((timeend-timestart).total_seconds(), 2) 
	print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**AUSGABE**

R-sqr = 0,594830601664

Für die Ausführung der obigen Zelle benötigte Zeit: 129,51 Sekunden


### Kreuzvalidierung mit Parameter-Sweeping mithilfe von benutzerdefiniertem Code für die Regression der zufälligen Gesamtstruktur

Der Code in diesem Abschnitt zeigt wie die Kreuzvalidierung mit Parameter-Sweeping mithilfe von benutzerdefiniertem Code für die Regression der zufälligen Gesamtstruktur durchgeführt wird und wie das Modell in Bezug auf Testdaten evaluiert wird.


	# RECORD START TIME
	timestart= datetime.datetime.now()

	# LOAD PYSPARK LIBRARIES
	# GET ACCURARY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET
	from pyspark.mllib.tree import RandomForest, RandomForestModel
	from pyspark.mllib.util import MLUtils
	from pyspark.mllib.evaluation import RegressionMetrics
	from sklearn.grid_search import ParameterGrid
	
	## CREATE PARAMETER GRID
	grid = [{'maxDepth': [5,10], 'numTrees': [25,50]}]
	paramGrid = list(ParameterGrid(grid))
	
	## SPECIFY LEVELS OF CATEGORICAL VARIBLES
	categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
	
	# SPECIFY NUMFOLDS AND ARRAY TO HOLD METRICS
	nFolds = 3;
	numModels = len(paramGrid)
	h = 1.0 / nFolds;
	metricSum = np.zeros(numModels);
	
	for i in range(nFolds):
	    # Create training and x-validation sets
	    validateLB = i * h
	    validateUB = (i + 1) * h
	    condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
	    validation = trainData.filter(condition)
	    # Create labeled points from data-frames
	    if i > 0:
	        trainCVLabPt.unpersist()
	        validationLabPt.unpersist()
	    trainCV = trainData.filter(~condition)
	    trainCVLabPt = trainCV.map(parseRowIndexingRegression)
	    trainCVLabPt.cache()
	    validationLabPt = validation.map(parseRowIndexingRegression)
	    validationLabPt.cache()
	    # For parameter sets compute metrics from x-validation
	    for j in range(numModels):
	        maxD = paramGrid[j]['maxDepth']
	        numT = paramGrid[j]['numTrees']
	        # Train logistic regression model with hypermarameter set
	        rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
	                                    numTrees=numT, featureSubsetStrategy="auto",
	                                    impurity='variance', maxDepth=maxD, maxBins=32)
	        predictions = rfModel.predict(validationLabPt.map(lambda x: x.features))
	        predictionAndLabels = validationLabPt.map(lambda lp: lp.label).zip(predictions)
	        # Use ROC-AUC as accuracy metrics
	        validMetrics = RegressionMetrics(predictionAndLabels)
	        metric = validMetrics.rootMeanSquaredError
	        metricSum[j] += metric
	
	avgAcc = metricSum/nFolds;
	bestParam = paramGrid[np.argmin(avgAcc)];
	
	# UNPERSIST OBJECTS
	trainCVLabPt.unpersist()
	validationLabPt.unpersist()
	        
	## TRAIN FINAL MODL WIHT BEST PARAMETERS
	rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
	                                    numTrees=bestParam['numTrees'], featureSubsetStrategy="auto",
	                                    impurity='variance', maxDepth=bestParam['maxDepth'], maxBins=32)

	# EVALUATE MODEL ON TEST DATA
	predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
	predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)
	
	#PRINT TEST METRICS
	testMetrics = RegressionMetrics(predictionAndLabels)
	print("RMSE = %s" % testMetrics.rootMeanSquaredError)
	print("R-sqr = %s" % testMetrics.r2)
	
	# PRINT ELAPSED TIME
	timeend = datetime.datetime.now()
	timedelta = round((timeend-timestart).total_seconds(), 2) 
	print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**AUSGABE**

RMSE = 0,990182456723

R-sqr = 0,609523627251

Für die Ausführung der obigen Zelle benötigte Zeit: 72,5 Sekunden


### Bereinigen von Objekten aus dem Arbeitsspeicher und Drucken von Modellspeicherorten

Löschen Sie mit `unpersist()` zwischengespeicherte Objekte aus dem Arbeitsspeicher.

	# UNPERSIST OBJECTS CACHED IN MEMORY

	# REMOVE ORIGINAL DFs
	taxi_df_train_cleaned.unpersist()
	taxi_df_train_with_newFeatures.unpersist()
	trainData.unpersist()
	trainData.unpersist()
	
	# FOR BINARY CLASSIFICATION TRAINING AND TESTING
	indexedTRAINbinary.unpersist()
	indexedTESTbinary.unpersist()
	oneHotTRAINbinary.unpersist()
	oneHotTESTbinary.unpersist()
	
	# FOR REGRESSION TRAINING AND TESTING
	indexedTRAINreg.unpersist()
	indexedTESTreg.unpersist()
	oneHotTRAINreg.unpersist()
	oneHotTESTreg.unpersist()
	
	# SCALED FEATURES
	oneHotTRAINregScaled.unpersist()
	oneHotTESTregScaled.unpersist()

	# PRINT MODEL FILE LOCATIONS FOR CONSUMPTION
	print "logisticRegFileLoc = modelDir + "" + logisticregressionfilename + """;
	print "linearRegFileLoc = modelDir + "" + linearregressionfilename + """;
	print "randomForestClassificationFileLoc = modelDir + "" + rfclassificationfilename + """;
	print "randomForestRegFileLoc = modelDir + "" + rfregressionfilename + """;
	print "BoostedTreeClassificationFileLoc = modelDir + "" + btclassificationfilename + """;
	print "BoostedTreeRegressionFileLoc = modelDir + "" + btregressionfilename + """;


**AUSGABE**

PythonRDD[119] at RDD at PythonRDD.scala:43

logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS\_2016-04-1817\_40\_35.796789"

linearRegFileLoc = modelDir + "LinearRegressionWithSGD\_2016-04-1817\_44\_00.993832"

randomForestClassificationFileLoc = modelDir + "RandomForestClassification\_2016-04-1817\_42\_58.899412"

randomForestRegFileLoc = modelDir + "RandomForestRegression\_2016-04-1817\_44\_27.204734"

BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification\_2016-04-1817\_43\_16.354770"

BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression\_2016-04-1817\_44\_46.206262"

## Wie geht es weiter?

Da Sie nun Regressions- und Klassifizierungsmodelle mit der Spark MlLib erstellt haben, können Sie jetzt lernen, wie diese Modelle bewertet und evaluiert werden.

**Modellnutzung**: Informationen zum Bewerten und Evaluieren der in diesem Thema erstellten Klassifizierungs- und Regressionsmodelle finden Sie unter [Score Spark-built machine learning models](machine-learning-data-science-spark-model-consumption.md) (Bewerten mit Spark erstellter Machine Learning-Modelle).

<!---HONumber=AcomDC_0427_2016-->