<properties
	pageTitle="Übersicht über Data Science mit Spark in Azure HDInsight | Microsoft Azure"
	description="Das Spark MLlib-Toolkit bringt wesentliche Machine Learning-Modellierungsfunktionen in die verteilte HDInsight-Umgebung ein."
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
	ms.author="bradsev" />

# Übersicht über Data Science mit Spark in Azure HDInsight

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

## Einführung

[Spark](http://spark.apache.org/) ist ein Open-Source-Framework für die Parallelverarbeitung, das die In-Memory-Verarbeitung unterstützt, um die Leistung von Anwendungen zur Analyse von Big Data zu steigern. Das Spark-Verarbeitungsmodul ist auf Geschwindigkeit, einfache Nutzung und anspruchsvolle Analysen ausgelegt. Dank seiner verteilten In-Memory-Datenverarbeitungsfunktionen eignet sich Spark besonders für iterative Algorithmen bei Machine Learning und für Graphberechnungen. [MLlib](http://spark.apache.org/mllib/) ist die skalierbare Machine Learning-Bibliothek von Spark, die Modellierungsfunktionen in diese verteilte Umgebung einbringt.

HDInsight Spark ist das von Azure gehostete Angebot von Open-Source-Spark. Darüber hinaus unterstützt der Spark-Cluster, der interaktive Spark-SQL-Abfragen zum Transformieren, Filtern und Visualisieren von Daten, die in Azure-Blobs (WASB) gespeichert sind, ausführen kann, auch Jupyter Notebooks.

Die Auflistung der im Menü verknüpften Themen veranschaulicht dies mit dem Durcharbeiten von Aufgaben zur binären Klassifizierung und Regression anhand einer Stichprobe des Datasets der NYC-Taxifahrten und Fahrpreise von 2013 und anschließendem Speichern des Modells im WASB. Die erstellten Modelle umfassen logistische und lineare Regression, zufällige Gesamtstrukturen und Gradient-Boosted-Strukturen. Sie zeigen außerdem die Nutzung dieser Modelle zum Bewerten und Evaluieren der Vorhersageleistung der Modelle. Fortgeschrittenere Themen behandeln, wie Modelle mithilfe übergreifender Validierung und Hyper-Parameter-Sweeping trainiert werden können.

Die Modellierungsschritte in diesen Themen enthalten auch Code zum Trainieren, Evaluieren, Speichern und Nutzen jedes Modelltyps. Mit Python (PySpark) – ausgeführt auf im Spark-Cluster installierten Jupyter-Notebooks – wurden die Lösung codiert und die entsprechenden Diagramme zum Visualisieren der Daten gezeigt.

Installationsschritte und Code in dieser exemplarischen Vorgehensweise beziehen sich auf HDInsight Spark. Allerdings ist der Code generisch und sollte auf jedem Spark-Cluster funktionieren. Clustereinrichtung und Verwaltungsschritte weichen möglicherweise geringfügig von dem ab, was hier gezeigt wird, wenn Sie nicht HDInsight Spark verwenden.

## Voraussetzungen

1\. Bevor Sie mit diesen Themen beginnen, müssen Sie über ein Azure-Abonnement verfügen. Wenn Sie noch keins besitzen, lesen Sie den Artikel [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) (So erhalten Sie eine Azure-Testversion zum Testen von Hadoop in HDInsight).

2\. Informationen zum Erstellen Ihres HDInsight Spark-Clusters, Version Spark 1.5.2 (HDI 3.3), finden Sie unter [Erste Schritte: Erstellen eines Apache Spark-Clusters für HDInsight (Linux) und Ausführen von interaktiven Abfragen per Spark-SQL (Vorschau)](../hdinsight/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql.md).

>[AZURE.NOTE] Der von den Notebooks und dem Code in dieser exemplarischen Vorgehensweise verwendete Kernel python2 muss die Version Spark (Vorschau) -> Spark 1.5.2 (HDI 3.3) verwenden.

![](./media/machine-learning-data-science-spark-overview/spark-cluster-on-portal.png)


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## Die NYC-2013-Taxidaten

Die NYC Taxi Trips-Daten umfassen ca. 20 GB komprimierter CSV-Dateien (~48 GB unkomprimiert) mit mehr als 173 Millionen einzelnen Fahrten mit den zugehörigen Preisen. Jeder Fahrtendatensatz enthält den Start- und Zielort mit der Uhrzeit, die anonymisierte Lizenznummer des Fahrers (Hack) und die eindeutige ID des Taxis (Medallion). Die Daten umfassen alle Fahrten im Jahr 2013. Sie werden für jeden Monat in den folgenden beiden Datasets bereitgestellt:

1. Die CSV-Dateien "trip\_data" enthalten Fahrtendetails, z. B. die Anzahl der Fahrgäste, Start- und Zielort, Fahrtdauer und Fahrtlänge. Es folgen einige Beispieleinträge:

		medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
		89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
		0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
		0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
		DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
		DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. Die CSV-Dateien "trip\_fare" enthalten Details zu den Kosten für jede Fahrt, beispielsweise Zahlungsart, Fahrpreis, Zuschläge und Steuern, Trinkgelder und Mautgebühren sowie den entrichteten Gesamtbetrag. Es folgen einige Beispieleinträge:

		medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
		89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
		0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
		0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
		DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
		DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5


Der eindeutige Schlüssel für die Zusammenführung von "trip\_data" und "trip\_fare" besteht aus den Feldern: "medallion", "hack\_licence" und "pickup\_datetime".


Wir haben eine Stichprobe von 0,1 % dieser Dateien entnommen und diese in einem einzelnen Dataset verknüpft, das für diese exemplarische Vorgehensweise als Eingabedataset verwendet wird. Der eindeutige Schlüssel für die Zusammenführung von "trip\_data" und "trip\_fare" besteht aus den Feldern: "medallion", "hack\_licence" und "pickup\_datetime". Jeder Datensatz des Datasets enthält die folgenden Attribute, die eine NYC-Taxifahrt darstellen:


|Feld| Kurzbeschreibung
|------|---------------------------------
| medallion |Anonymisierte Taxi-Medallion (eindeutige Taxi-ID)
| hack\_license |	Anonymisierte Hackney Carriage-Lizenznummer
| vendor\_id |	Taxiunternehmer-ID
| rate\_code | NYC-Taxitarif
| store\_and\_fwd\_flag | Speicherungs- und Weiterleitungsflag
| pickup\_datetime |	Datum und Uhrzeit der Fahrtgastaufnahme
| dropoff\_datetime | Datum und Uhrzeit des Fahrtgastabsetzens
| pickup\_hour |	Stunde der Aufnahme
| pickup\_week |	Kalenderwoche der Aufnahme
| weekday |	Wochentag (Bereich 1 bis 7)
| passenger\_count |	Anzahl der Fahrgäste bei einer Taxifahrt
| trip\_time\_in\_secs | Fahrtzeit in Sekunden
| trip\_distance | Zurückgelegte Fahrstrecke in Meilen
| pickup\_longitude | Längengrad der Aufnahme
| pickup\_latitude |	Breitengrad der Aufnahme
| dropoff\_longitude | Längengrad des Absetzens
| dropoff\_latitude | Breitengrad des Absetzens
| direct\_distance |	Direkte Entfernung zwischen Start- und Zielort
| payment\_type | Zahlungsart (bar, Kreditkarte usw.)
| fare\_amount | Fahrpreis in
| surcharge | Zuschlag
| mta\_tax |	MTA-Steuer
| tip\_amount | Trinkgeldbetrag
| tolls\_amount | Mautgebührenbetrag
| total\_amount | Gesamtbetrag
| tipped | Trinkgeld gezahlt (0/1 für „Nein“ oder „Ja“)
| tip\_class | Trinkgeldklasse (0: 0 $, 1: 0-5 $, 2: 6 bis 10 $, 3: 11 bis 20 $, 4: > 20$)


## Ausführen von Code über ein Jupyter-Notebook auf dem Spark-Cluster 

Sie können das Jupyter-Notebook über das Azure-Portal starten: Suchen Sie Ihren Spark-Cluster, und klicken Sie darauf, um die detaillierte Clusterverwaltungsseite einzugeben. Dort können Sie auf die **Clusterdashboards** klicken, wo das Symbol für das Jupyter Notebook mit dem Spark-Cluster verknüpft ist.

![](./media/machine-learning-data-science-spark-overview/spark-jupyter-on-portal.png)

Sie können auch zu ******https://CLUSTERNAME.azurehdinsight.net/jupyter*** navigieren, um auf die Jupyter Notebooks zuzugreifen. Sie benötigen das Kennwort für Ihr Administratorkonto, um auf die Notebooks zuzugreifen.

![](./media/machine-learning-data-science-spark-overview/spark-jupyter-notebook.png)

Navigieren Sie zu Python, um vorhandene Notebooks anzeigen, die Python-Skripts ausführen. Sie sehen ein Verzeichnis, das ein paar Beispiele für vorkonfigurierte Notebooks enthält. Das Notebook, das die Codebeispiele in diesem Thema enthält, ist in [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Python) verfügbar.

Sie können die Notebooks direkt von Github auf den Jupyter-Notebook-Server in Ihrem Spark-Cluster hochladen. Klicken Sie auf der Homepage Ihres Jupyter im rechten Teil des Bildschirms auf die Schaltfläche **Hochladen**. Ein Datei-Explorer wird geöffnet. Hier können Sie die Github-URL (Rohdateninhalt) des Notebooks einfügen und auf **Öffnen** klicken. Sie sehen den Dateinamen in der Jupyter-Dateiliste, wieder mit einer **Hochladen**-Schaltfläche. Klicken Sie auf diese **Hochladen**-Schaltfläche. Sie haben nun das Notebook importiert. Wiederholen Sie diese Schritte, um die folgenden Notebooks dieser exemplarischen Vorgehensweise hochzuladen. (Hinweis: Sie können mit der rechten Maustaste auf die Links unten in Ihrem Browser klicken und **Link kopieren** wählen, um die Github-URL für den unformatierten Inhalt abzurufen, die Sie in das Jupyter-Datei-Explorer-Dialogfeld zum Hochladen einfügen können.)

1.	[machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/machine-learning-data-science-spark-data-exploration-modeling.ipynb)
2.	[machine-learning-data-science-spark-model-consumption.ipynb](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/machine-learning-data-science-spark-model-consumption.ipynb)
3.	[machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb)

Sie können jetzt:

- Auf das Notebook klicken, um den Code zu sehen
- Jede Zelle über **UMSCHALT+EINGABE-Taste** ausführen.
- Das gesamte Notebook ausführen, indem Sie auf **Cell** -> **Run** klicken.


## Wie geht es weiter?

Da Sie jetzt einen HDInsight Spark-Cluster eingerichtet und die Jupyter-Notebooks hochgeladen haben, können Sie nun die Themen durcharbeiten, die diesen drei Notebooks entsprechen, die zeigen, wie Sie Ihre Daten durchsuchen, Modelle erstellen und nutzen. Das Notebook zum erweiterten Durchsuchen von Daten und Modellieren dringt tiefer in übergreifende Validierung, Hyper-Parameter-Sweeping und Auswertung von Modellen ein.

**Durchsuchen von Daten und Modellieren mit Spark:** Durchsuchen Sie das Dataset, erstellen Sie die hier zu bewertenden und evaluierenden Machine Learning-Modelle durch Durcharbeiten des Themas [Create binary classification and regression models for data with the Spark MLlib toolkit](machine-learning-data-science-spark-data-exploration-modeling.md) (Durchsuchen und Modellieren von Daten mit Spark).

**Modellnutzung:** Informationen zum Bewerten der in diesem Thema erstellten Klassifizierungs- und Regressionsmodelle finden Sie unter [Score Spark-built machine learning models](machine-learning-data-science-spark-model-consumption.md) (Bewerten mit Spark erstellter Machine Learning-Modelle).

**Übergreifende Validierung und Hyper-Parameter-Sweeping**: Unter [Advanced data exploration and modeling with Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) (Erweiterte Datendurchsuchung und Modellierung mit Spark) erfahren Sie, wie Modelle mit übergreifender Validierung und Hyper-Parameter-Sweeping trainiert werden können.

<!---HONumber=AcomDC_0427_2016-->