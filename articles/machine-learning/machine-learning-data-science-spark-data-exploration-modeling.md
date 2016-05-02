<properties
	pageTitle="Durchsuchen von Daten und Modellierung mit Spark | Microsoft Azure"
	description="Veranschaulicht die Funktionen der zum Durchsuchen und Modellieren von Daten des Spark MLlib-Toolkits."
	services="machine-learning"
	documentationCenter=""
	authors="bradsev"
	manager="paulettm"
	editor="cgronlun" />

<tags
	ms.service="machine-learning"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/18/2016"
	ms.author="deguhath;bradsev" />

# Durchsuchen von Daten und Modellierung mit Spark

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

## Einführung 

In dieser exemplarischen Vorgehensweise wird HDInsight Spark für das Durchsuchen von Daten sowie für Aufgaben zur binären Klassifizierung und Regressionsmodellierung anhand einer Stichprobe des Datasets der NYC-Taxifahrten und Fahrpreise von 2013 verwendet. Sie werden schrittweise unter Verwendung eines HDInsight Spark-Clusters für die Verarbeitung und Azure-Blobs zum Speichern der Daten und Modelle durch den gesamten [Data Science-Prozess](http://aka.ms/datascienceprocess) geführt. In diesem Prozess werden Daten aus einem Azure Storage-Blob untersucht sowie visualisiert und anschließend für das Erstellen von Vorhersagemodellen vorbereitet. Diese Modelle werden mithilfe des Spark MLlib-Toolkits erstellt, um Aufgaben zur binären Klassifizierung und Regressionsmodellierung ausführen.

- Die Aufgabe zur **binären Klassifizierung** besteht darin, vorherzusagen, ob ein Trinkgeld für eine Fahrt bezahlt wird. 
- Die Aufgabe zur **Regression** besteht darin, die Höhe des Trinkgelds basierend auf anderen Trinkgeldfunktionen vorherzusagen. 

Die Modellierungsschritte enthalten auch Code zum Trainieren, Evaluieren und Speichern jedes Modelltyps. Python wurde verwendet, um die Lösung zu codieren und die entsprechenden Diagramme anzuzeigen.

Die Modelle, die wir verwenden, umfassen logistische und lineare Regression, zufällige Gesamtstrukturen und Gradient-Boosted-Strukturen:

- [Lineare Regression mit SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) ist ein lineares Regressionsmodell, das eine SGD-Methode (Stochastic Gradient Descent, stochastisches Gradientenverfahren) zur Optimierung und Featureskalierung verwendet, um die gezahlten Trinkgeldbeträge vorherzusagen. 
- [Logistische Regression mit LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) oder „Logit“-Regression ist ein Regressionsmodell, das verwendet werden kann, wenn die abhängige Variable zur Datenklassifizierung kategorisch ist. LBFGS ist ein quasi-Newtonscher Optimierungsalgorithmus, der sich dem Broyden-Fletcher-Goldfarb-Shanno-Algorithmus (BFGS) unter Verwendung einer begrenzten Menge an Arbeitsspeicher annähert und häufig im Machine Learning verwendet wird.
- [Zufällige Gesamtstrukturen](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) sind Ensembles von Entscheidungsstrukturen. In ihnen sind viele Entscheidungsstrukturen kombiniert, um das Risiko der Überanpassung zu verringern. Zufällige Gesamtstrukturen werden für die Regression und Klassifizierung verwendet und können kategorische Features behandeln, auf die Mehrklassenklassifizierung ausgedehnt werden, erfordern keine Featureskalierung und können Nichtlinearitäten und Featureinteraktionen erfassen. Zufällige Gesamtstrukturen zählen zu den erfolgreichsten Machine Learning-Modelle für Klassifizierung und Regression.
- [Gradient-Boosted-Strukturen](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) sind Ensembles von Entscheidungsstrukturen. GBTs trainieren Entscheidungsstrukturen iterativ, um einen Funktionsverlust zu minimieren. GBTs werden für die Regression und Klassifizierung verwendet und können kategorische Features behandeln, erfordern keine Featureskalierung und können Nichtlinearitäten und Featureinteraktionen erfassen. Sie können auch in einer Mehrklassenklassifizierung verwendet werden.


>AZURE.HINWEIS: Obwohl das Spark MLlib-Toolkit für die Arbeit mit großen Datasets entworfen wurde, wird zur Demonstration seiner Modellierungsfunktionen der Einfachheit halber ein relativ kleines Beispiel (ca. ~30 Mb mit 170 K Zeilen, etwa 0,1 % des ursprünglichen NYC-Datasets) verwendet. Die hier angegebene Übung wird effizient auf einen HDInsight-Cluster mit 2 Workerknoten (in ungefähr 10 Minuten) ausgeführt. Der gleiche Code kann mit geringfügigen Änderungen zur Verarbeitung größerer Datasets verwendet werden – mit entsprechenden Änderungen zum Zwischenspeichern von Daten im Arbeitsspeicher oder Änderung der Clustergröße.

## Voraussetzungen

Sie benötigen ein Azure-Konto und einen HDInsight Spark-Cluster, um mit dieser exemplarischen Vorgehensweise zu beginnen. Unter [Overview of Data Science using Spark on Azure HDInsight](machine-learning-data-science-spark-overview.md) (Übersicht zu Data Science unter Verwendung von Spark unter Azure HDInsight) finden Sie diese Anforderungen, eine Beschreibung der hier verwendeten NYC-Taxi-Daten von 2013 und Anweisungen zum Ausführen von Code aus einem Jupyter-Notebook auf dem Spark-Cluster. Das **machine-learning-data-science-spark-data-exploration-modeling.ipynb**-Notebook, das die Codebeispiele in diesem Thema enthält, ist in [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Python) verfügbar.


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## Setup: Speicherorte, Bibliotheken und Spark-Kontext

Spark kann aus dem Azure Storage-Blob (auch bekannt als WASB) lesen und darin schreiben. Es können also alle Ihre vorhandenen Daten mithilfe von Spark verarbeitet und die Ergebnisse wieder im WASB gespeichert werden.

Um Modelle oder Dateien im WASB zu speichern, muss der Pfad korrekt angegeben werden. Auf den an den Spark-Cluster angefügten Standardcontainer können Sie mit einem Pfad verweisen, der mit „wasb///“ beginnt. Auf andere Speicherorte wird mit „wasb://“ verwiesen.

Zum Setup gehört auch das Importieren erforderlicher Bibliotheken und das Festlegen des Spark-Kontexts.

### Festlegen von Verzeichnispfaden für Speicherorte im WASB

Das folgende Codebeispiel gibt den Speicherort der zu lesenden Daten und den Pfad für das Modellspeicherverzeichnis an, in dem die Ausgabe des Modells gespeichert wird.


	# SET PATHS TO FILE LOCATIONS: DATA AND MODEL STORAGE

	# LOCATION OF TRAINING DATA
	taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";

	# SET THE MODEL STORAGE DIRECTORY PATH 
	# Note that the final backslash in the path is needed.
	modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 


### Importieren benötigter Bibliotheken und Festlegen des Spark-Kontexts 

Legen Sie den Spark-Kontext fest, und importieren Sie die erforderlichen Bibliotheken mit dem folgenden Code.


	# IMPORT LIBRARIES
	import pyspark
	from pyspark import SparkConf
	from pyspark import SparkContext
	from pyspark.sql import SQLContext
	%matplotlib inline
	import matplotlib
	import matplotlib.pyplot as plt
	from pyspark.sql import Row
	from pyspark.sql.functions import UserDefinedFunction
	from pyspark.sql.types import *
	import atexit
	from numpy import array
	import numpy as np
	import datetime
	datetime.datetime.now()
	
	# SET SPARK CONTEXT
	sc = SparkContext(conf=SparkConf().setMaster('yarn-client'))
	sqlContext = SQLContext(sc)
	atexit.register(lambda: sc.stop())
	
	sc.defaultParallelism

**Ausgabe:**

4


## Datenerfassung

Dieser Abschnitt enthält den Code für eine Reihe erforderlicher Aufgaben zum Erfassen des Datenbeispiels, das modelliert werden soll. Lesen Sie eine verknüpfte 0,1 %-Stichprobe der Datei für Taxifahrten und Fahrpreise (gespeichert als TSV-Datei) ein, formatieren und bereinigen Sie die Daten, erstellen Sie einen Datenrahmen im Arbeitsspeicher, zwischenspeichern Sie ihn dort, und registrieren Sie ihn dann als temporäre Tabelle im SQL-Kontext.

Der erste Schritt im Data Science-Prozess ist das Erfassen der zu analysierenden Daten aus externen Quellen oder Systemen in die Umgebung, die Sie zum Durchsuchen und Modellieren von Daten nutzen. In dieser exemplarischen Vorgehensweise ist Spark diese Umgebung. Dieser Abschnitt enthält den Code zur Ausführung einer Reihe von Aufgaben:

- Erfassen des zu modellierenden Datenbeispiels
- Einlesen des Eingabedatasets (gespeichert als TSV-Datei)
- Formatieren und Bereinigen der Daten
- Erstellen und Zwischenspeichern von Objekten (RDDs oder Datenrahmen) im Arbeitsspeicher
- deren Registrierung als temporäre Tabelle im SQL-Kontext.

Hier ist der Code für die Datenerfassung.

	# INGEST DATA

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
	
	    
	# CREATE DATA FRAME
	taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)
	
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

**Ausgabe:**

Für die Ausführung der obigen Zelle benötigte Zeit: 19,8 Sekunden


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
	
	#CONVERT TO PANDAS DATA FRAME FOR PLOTTING IN PYTHON
	resultsPDDF = sqlResults.toPandas()
	
	# PLOT PASSENGER NUMBER VS. TRIP COUNTS
	x_labels = resultsPDDF['passenger_count'].values
	fig = resultsPDDF[['trip_counts']].plot(kind='bar', facecolor='lightblue')
	fig.set_xticklabels(x_labels)
	fig.set_title('Frequency of trips by passenger count')
	plt.show()

**Ausgabe:**

![Fahrtenhäufigkeit nach Anzahl der Fahrgäste](./media/machine-learning-data-science-spark-data-exploration-modeling/trip-freqency-by-passenger-count.png)

	
### Zeichnen eines Histogramms des Trinkgeldbetrags und der Variation des Trinkgeldbetrags nach Zahlungsbetrag und -art

Der Code sammelt die Daten in einer SQL-Abfrage und konvertiert die Ergebnisse in einen zu zeichnenden Pandas-Datenrahmen.

	#PLOT HISTOGRAM OF TIP AMOUNTS AND VARIATION BY PASSENGER COUNT AND PAYMENT TYPE

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
	
	#CONVERT TO PANDAS DATA FRAME FOR PLOTTING IN PYTHON
	resultsPDDF= sqlResults.toPandas()
	
	# HISTOGRAM OF TIP AMOUNTS
	ax1 = resultsPDDF[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
	ax1.set_title('Tip amount distribution')
	ax1.set_xlabel('Tip Amount ($)')
	ax1.set_ylabel('Counts')
	plt.suptitle('')
	plt.show()
	
	# TIP AMOUNT BY PASSENGER COUNT
	ax2 = resultsPDDF.boxplot(column=['tip_amount'], by=['passenger_count'])
	ax2.set_title('Tip amount by Passenger count')
	ax2.set_xlabel('Passenger count')
	ax2.set_ylabel('Tip Amount ($)')
	plt.suptitle('')
	plt.show()
	
	# TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
	ax = resultsPDDF.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(resultsPDDF.passenger_count))
	ax.set_title('Tip amount by Fare amount')
	ax.set_xlabel('Fare Amount ($)')
	ax.set_ylabel('Tip Amount ($)')
	plt.axis([-2, 120, -2, 30])
	plt.show()
	
	# PRINT ELAPSED TIME
	timeend = datetime.datetime.now()
	timedelta = round((timeend-timestart).total_seconds(), 2) 
	print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**Ausgabe:**

![Trinkgeldbetragverteilung](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-distribution.png)

![Trinkgeldbetrag nach Anzahl der Fahrgäste](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Trinkgeldbetrag nach Höhe des Fahrpreises](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-fare-amount.png)

Für die Ausführung der obigen Zelle benötigte Zeit: 10,61 Sekunden


## Feature Engineering, Transformation und Datenvorbereitung für die Modellierung
Dieser Abschnitt enthält den Code für die Prozeduren zum Vorbereiten von Daten für die Verwendung in der ML-Modellierung und deren Beschreibung. Hier erfahren Sie, wie Sie die folgenden Aufgaben ausführen:

- Erstellen eines neuen Features durch Diskretisieren von Stunden in Verkehrszeitbuckets
- Indizieren und Codieren von kategorischen Features
- Erstellen von bezeichneten Punktobjekten für die Eingabe in ML-Funktionen
- Erstellen einer Zufallsunterauswahl der Daten und Aufteilung in Trainings- und Testsätze
- Featureskalierung
- Zwischenspeichern von Objekten im Arbeitsspeicher


### Erstellen eines neuen Features durch Diskretisieren von Stunden in Verkehrszeitbuckets

Dieser Code zeigt, wie Sie ein neues Feature durch Diskretisieren von Stunden in Verkehrszeitbuckets erstellen und dann den resultierenden Datenrahmen im Arbeitsspeicher zwischenspeichern. Wo robuste verteilte Datasets (Resilient Distributed Datasets, RDDs) und Datenrahmen wiederholt verwendet werden, führt das Zwischenspeichern zu verbesserten Ausführungszeiten. Folglich werden wir RDDs und Datenrahmen in mehreren Phasen der exemplarischen Vorgehensweise zwischenspeichern.

	## CREATE FOUR BUCKETS FOR TRAFFIC TIMES
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
	
	## CACHE DATA FRAME IN MEMORY & MATERIALIZE IT IN MEMORY
	taxi_df_train_with_newFeatures.cache()
	taxi_df_train_with_newFeatures.count()

**Ausgabe:**

126050

### Indizieren und Codieren von kategorischen Features für die Eingabe in Modellierungsfunktionen

Dieser Abschnitt beschreibt das Indizieren oder Codieren von kategorischen Features für die Eingabe in die Modellierungsfunktionen. Die Modellierungs- und Vorhersagefunktionen von MLlib erfordern, dass Features mit kategorischen Eingabedaten vor der Verwendung indiziert oder codiert werden. Je nach Modell müssen Sie auf unterschiedliche Weise indizieren oder codieren:

- **Strukturbasierte Modellierung** erfordert, dass Kategorien als numerische Werte codiert werden (z. B. kann ein Feature mit 3 Kategorien möglicherweise mit 0, 1 und 2 codiert werden). Dies erfolgt über die MLlib-Funktion [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer). Diese Funktion codiert eine Zeichenfolgenspalte von Bezeichnungen in eine Spalte mit der Bezeichnungsindizes, die nach der Häufigkeit der Bezeichnungen angeordnet sind. Beachten Sie, dass der strukturbasierte Algorithmus, obwohl sie mit numerischen Werte für die Behandlung von Eingabe und Daten indiziert sind, so angegeben werden kann, dass sie entsprechend als Kategorien behandelt werden. 

- **Logistische und lineare Regressionsmodelle** erfordern One-Hot-Codierung, wobei z. B. ein Feature mit 3 Kategorien auf 3 Featurespalten erweitert werden kann, wobei jede abhängig von der Kategorie einer Beobachtung 0 oder 1 enthalten kann. MLlib bietet die Funktion [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) zur One-Hot-Codierung. Dieser Encoder ordnet eine Spalte mit Bezeichnungsindizes einer Spalte mit binären Vektoren mit höchstens einem einzigen Wert zu. Diese Codierung ermöglicht die Anwendung von Algorithmen, die kontinuierliche wertige Features erwarten, z. B. logistische Regression, auf kategorische Features.

Hier ist der Code zum Indizieren und Codieren von kategorischen Features:


	# INDEX AND ENCODE CATEGORICAL FEATURES

	# RECORD START TIME
	timestart = datetime.datetime.now()

	# LOAD PYSPARK LIBRARIES	
	from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer
	
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

**Ausgabe:**

Für die Ausführung der obigen Zelle benötigte Zeit: 1,42 Sekunden

### Erstellen von bezeichneten Punktobjekten für die Eingabe in ML-Funktionen

Dieser Abschnitt enthält Code, der zeigt, wie Sie kategorische Textdaten als bezeichneten Punktdatentyp und so codieren, dass sie zum Trainieren und Testen logistischer Regression gemäß MLlib und anderer Klassifizierungsmodelle verwendet werden können. Bezeichnete Punktobjekte sind robuste verteilte Datasets (RDD), die in einer Weise formatiert sind, die von den meisten ML-Algorithmen in MLlib als Eingabedaten benötigt wird. Ein [bezeichneter Punkt](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) ist ein lokaler Vektor, entweder dicht oder platzsparend, der mit einer Bezeichnung/Antwort verknüpft ist.

Dieser Abschnitt enthält Code, der zeigt, wie Sie kategorische Textdaten als [bezeichneten Punktdatentyp](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) und so codieren, dass sie zum Trainieren und Testen logistischer Regression gemäß MLlib und anderer Klassifizierungsmodelle verwendet werden können. Bezeichnete Punktobjekte sind robuste verteilte Datasets (RDD), die aus Bezeichnung (Ziel/Antwortvariable) und Featurevektor bestehen. Dieses Format wird von vielen ML-Algorithmen in MLlib als Eingabe benötigt.


	# FUNCTIONS FOR BINARY CLASSIFICATION

	# LOAD PYSPARK LIBRARIES
	from pyspark.mllib.regression import LabeledPoint
	from numpy import array

	# INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
	def parseRowIndexingBinary(line):
	    features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
	                         line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
	                         line.trip_distance, line.fare_amount])
	    labPt = LabeledPoint(line.tipped, features)
	    return  labPt
	
	# ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
	def parseRowOneHotBinary(line):
	    features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
	                                        line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
	                                        line.vendorVec.toArray(), line.rateVec.toArray(), 
	                                        line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
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
	
	# LOAD PYSPARK LIBRARIES
	from pyspark.sql.functions import rand

	# SPECIFY SAMPLING AND SPLITTING FRACTIONS
	samplingFraction = 0.25;
	trainingFraction = 0.75; testingFraction = (1-trainingFraction);
	seed = 1234;
	encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)
	
	# SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST
	# INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS (FOR USE LATER IN AN ADVANCED TOPIC)
	dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
	trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);
	
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

**Ausgabe:**

Für die Ausführung der obigen Zelle benötigte Zeit: 0,26 Sekunden


### Featureskalierung

Featureskalierung, auch bekannt als Datennormalisierung, stellt sicher, dass Features mit weit verteilten Werten keine übermäßige Gewichtung in der Zielfunktion erhalten. Der Code für die Featureskalierung verwendet [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) zum Skalieren der Features auf Einheitenvarianz. Er wird von MLlib für die Verwendung bei der linearen Regression mit dem stochastischen Gradientenverfahren (SGD), einem beliebten Algorithmus für das Training einer Vielzahl anderer Machine Learning-Modelle, z. B. normalisierter Regressionen oder Support Vector Machines (SVM), bereitgestellt.

>AZURE.HINWEIS: Wir haben festgestellt, dass der LinearRegressionWithSGD-Algorithmus für die Featureskalierung zu empfindlich ist.


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

**Ausgabe:**

Für die Ausführung der obigen Zelle benötigte Zeit: 6,63 Sekunden


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

- Normalisierte logistische Regression 
- Zufälliges Gesamtstrukturmodell
- Gradient-Boosted-Strukturen

Jeder Codeabschnitt zur Modellerstellung ist in Schritte unterteilt:

1. **Modelltrainingsdaten** mit einem Parametersatz
2. **Modellevaluierung** an einem Testdataset mit Metriken
3. **Speichern des Modells** im Blob für die zukünftige Nutzung

### Klassifizierung mittels logistischer Regression

Der Code in diesem Abschnitt zeigt, wie ein logistisches Regressionsmodell, das vorhersagt, ob ein Trinkgeld für eine Fahrt im NYC-Taxifahrten- und Fahrpreisedataset gezahlt wird, mit [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) trainiert, evaluiert und gespeichert werden kann.


	#PREDICT WHETHER A TIP IS PAID OR NOT USING LOGISTIC REGRESSION WITH LBFGS

	# RECORD START TIME
	timestart = datetime.datetime.now()
	
	# LOAD PYSPARK LIBRARIES
	from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
	from sklearn.metrics import roc_curve,auc
	from pyspark.mllib.evaluation import BinaryClassificationMetrics
	from pyspark.mllib.evaluation import MulticlassMetrics
	
	
	# CREATE MODEL WITH ONE SET OF PARAMETERS
	logitModel = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, iterations=20, initialWeights=None, 
	                                               regParam=0.01, regType='l2', intercept=True, corrections=10, 
	                                               tolerance=0.0001, validateData=True, numClasses=2)
	
	# PREDICT ON TEST DATA WITH MODEL
	predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitModel.predict(lp.features)), lp.label))
	
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
	logitModel.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
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
	
	## SAVE MODEL WITH DATE-STAMP
	datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
	logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
	dirfilename = modelDir + logisticregressionfilename;
	
	logitModel.save(sc, dirfilename);
	
	# PRINT ELAPSED TIME
	timeend = datetime.datetime.now()
	timedelta = round((timeend-timestart).total_seconds(), 2) 
	print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**Ausgabe:**

Area under PR = 0.985319161941 Area under ROC = 0.983511076103 Summary Stats Precision = 0.984187223276 Recall = 0.984187223276 F1 Score = 0.984187223276

![Logistic regression ROC curve.png](./media/machine-learning-data-science-spark-data-exploration-modeling/logistic-regression-roc-curve.png)

Für die Ausführung der obigen Zelle benötigte Zeit: 26,63 Sekunden

### Klassifizierung zufälliger Gesamtstrukturmodelle

Der Code in diesem Abschnitt zeigt, wie ein zufälliges Gesamtstrukturmodell, das vorhersagt, ob ein Trinkgeld für eine Fahrt im NYC-Taxifahrten- und Fahrpreisedataset gezahlt wird, trainiert, evaluiert und gespeichert werden kann.
	
	#PREDICT WHETHER A TIP IS PAID OR NOT USING RANDOM FOREST

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
	## UN-COMMENT IF YOU WANT TO PRINT TREES
	#print('Learned classification forest model:')
	#print(rfModel.toDebugString())
	
	# PREDICT ON TEST DATA AND EVALUATE
	predictions = rfModel.predict(indexedTESTbinary.map(lambda x: x.features))
	predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
	
	# AREA UNDER ROC CURVE
	metrics = BinaryClassificationMetrics(predictionAndLabels)

	# PRINT TEST METRICS
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

**Ausgabe:**

Area under ROC = 0.985240932843 Für die Ausführung der obigen Zelle benötigte Zeit: 25,62 Sekunden


### Klassifizierung von Gradient-Boosted-Strukturen

Der Code in diesem Abschnitt zeigt, wie ein Gradient-Boosted-Strukturmodell, das vorhersagt, ob ein Trinkgeld für eine Fahrt im NYC-Taxifahrten- und Fahrpreisedataset gezahlt wird, trainiert, evaluiert und gespeichert werden kann.

	#PREDICT WHETHER A TIP IS PAID OR NOT USING GRADIENT BOOSTING TREES

	# RECORD START TIME
	timestart = datetime.datetime.now()
	
	# LOAD PYSPARK LIBRARIES
	from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
	
	# SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
	categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
	
	gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo, numIterations=5)
	## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
	#print('Learned classification GBT model:')
	#print(bgtModel.toDebugString())
	
	# PREDICT ON TEST DATA AND EVALUATE
	predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
	predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
	
	# AREA UNDER ROC CURVE
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


## Vorhersage von Trinkgeldbeträgen für Taxifahrten mit Regressionsmodellen

Dieser Abschnitt zeigt Ihnen die Verwendung von drei Modellen für die Regressionsaufgabe der Vorhersage des Trinkgeldbetrags, der für eine Taxifahrt gezahlt wird, auf der Basis anderer Trinkgeldfeatures. Die präsentierten Modelle sind:

- Normalisierte lineare Regression
- Zufälliges Gesamtstrukturmodell
- Gradient-Boosted-Strukturen

Diese Modelle wurden in der Einführung beschrieben. Jeder Codeabschnitt zur Modellerstellung ist in Schritte unterteilt:

1. **Modelltrainingsdaten** mit einem Parametersatz
2. **Modellevaluierung** an einem Testdataset mit Metriken
3. **Speichern des Modells** im Blob für die zukünftige Nutzung

### Lineare Regression mit SGD 

Der Code in diesem Abschnitt zeigt, wie skalierte Features zum Trainieren einer linearen Regression verwendet werden, die das stochastische Gradientenverfahren (SGD) für die Optimierung verwendet, und wie das Modell bewertet, evaluiert und in Azure-Blobspeicher (WASB) gespeichert wird.

>AZURE.HINWEIS: Nach unserer Erfahrung können häufig Probleme mit der Konvergenz von LinearRegressionWithSGD-Modellen auftreten, und Parameter müssen sorgfältig geändert/optimiert werden, um ein gültiges Modell zu erhalten. Skalieren von Variablen ist eine deutliche Hilfe (siehe unten).

	#PREDICT TIP AMOUNTS USING LINEAR REGRESSION WITH SGD

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
	
	# PRINT TEST METRICS
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


### Regression der zufälligen Gesamtstruktur

Der Code in diesem Abschnitt zeigt, wie ein zufälliges Gesamtstrukturmodell, das den Trinkgeldbetrag für die NYC-Taxifahrtendaten vorhersagt, trainiert, evaluiert und gespeichert werden kann.


	#PREDICT TIP AMOUNTS USING RANDOM FOREST

	# RECORD START TIME
	timestart= datetime.datetime.now()

	# LOAD PYSPARK LIBRARIES
	from pyspark.mllib.tree import RandomForest, RandomForestModel
	from pyspark.mllib.util import MLUtils
	from pyspark.mllib.evaluation import RegressionMetrics
	
	
	## TRAIN MODEL
	categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
	rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
	                                    numTrees=25, featureSubsetStrategy="auto",
	                                    impurity='variance', maxDepth=10, maxBins=32)
	## UN-COMMENT IF YOU WANT TO PRING TREES
	#print('Learned classification forest model:')
	#print(rfModel.toDebugString())
	
	## PREDICT AND EVALUATE ON TEST DATA-SET
	predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
	predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)
	testMetrics = RegressionMetrics(predictionAndLabels)

	# PRINT TEST METRICS
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


### Gradient-Boosted-Strukturenregression

Der Code in diesem Abschnitt zeigt, wie ein Gradient-Boosted-Strukturmodell, das den Trinkgeldbetrag für die NYC-Taxifahrtendaten vorhersagt, trainiert, evaluiert und gespeichert werden kann.


	#PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

	# RECORD START TIME
	timestart= datetime.datetime.now()
	
	# LOAD PYSPARK LIBRARIES
	from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
	from pyspark.mllib.util import MLUtils
	
	## TRAIN MODEL
	categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
	gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
	                                                numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)
	
	## EVALUATE A TEST DATA-SET
	predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
	predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)
	testMetrics = RegressionMetrics(predictionAndLabels)

	# PRINT TEST METRICS
	print("RMSE = %s" % testMetrics.rootMeanSquaredError)
	print("R-sqr = %s" % testMetrics.r2)
	
	# PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES
	test_predictions = sqlContext.createDataFrame(predictionAndLabels)
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

**OUTLOOK**

RMSE = 0.962160568829

R-Sqr = 0.717354800581

![Actual-vs-predicted-tip-amounts](./media/machine-learning-data-science-spark-data-exploration-modeling/actual-vs-predicted-tips.png)

	
## Bereinigung von Objekten aus dem Arbeitsspeicher

Löschen Sie mit `unpersist()` zwischengespeicherte Objekte aus dem Arbeitsspeicher.
		
	# REMOVE ORIGINAL DFs
	taxi_df_train_cleaned.unpersist()
	taxi_df_train_with_newFeatures.unpersist()
	
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


## Datensatzspeicherorte der Modelle für die Nutzung und Bewertung

Für die Nutzung und Bewertung eines unabhängigen Datasets, das im Thema [Score Spark-built machine learning models](machine-learning-data-science-spark-model-consumption.md) (Bewerten mit Spark erstellter Machine Learning-Modelle) beschrieben wird, müssen Sie diese Namen der Dateien, die die hier erstellten gespeicherten Modelle enthalten, kopieren und in das Consumption-Jupyter-Notebook einfügen. Hier ist der Code zum Drucken der Pfade zu Modelldateien, die Sie benötigen.

	# MODEL FILE LOCATIONS FOR CONSUMPTION
	print "logisticRegFileLoc = modelDir + "" + logisticregressionfilename + """;
	print "linearRegFileLoc = modelDir + "" + linearregressionfilename + """;
	print "randomForestClassificationFileLoc = modelDir + "" + rfclassificationfilename + """;
	print "randomForestRegFileLoc = modelDir + "" + rfregressionfilename + """;
	print "BoostedTreeClassificationFileLoc = modelDir + "" + btclassificationfilename + """;
	print "BoostedTreeRegressionFileLoc = modelDir + "" + btregressionfilename + """;


## Wie geht es weiter?

Da Sie nun Regressions- und Klassifizierungsmodelle mit der Spark MlLib erstellt haben, können Sie jetzt lernen, wie diese Modelle bewertet und evaluiert werden.

**Modellnutzung:** Informationen zum Bewerten und Evaluieren der in diesem Thema erstellten Klassifizierungs- und Regressionsmodelle finden Sie unter [Score Spark-built machine learning models](machine-learning-data-science-spark-model-consumption.md) (Bewerten mit Spark erstellter Machine Learning-Modelle).

<!---HONumber=AcomDC_0420_2016-->