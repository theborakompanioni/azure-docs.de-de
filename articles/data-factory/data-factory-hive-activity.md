<properties 
	pageTitle="Hive-Aktivität" 
	description="Erfahren Sie, wie Sie die Hive-Aktivität in Azure Data Factory verwenden können, um Hive-Abfragen in einem bedarfsgesteuerten/eigenen HDInsight-Cluster auszuführen." 
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="09/20/2016" 
	ms.author="spelluru"/>

# Hive-Aktivität

Die HDInsight-Hive-Aktivität in einer Data Factory-[Pipeline](data-factory-create-pipelines.md) führt Hive-Abfragen in [Ihren eigenen](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) oder [bedarfsgesteuerten](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-basierten HDInsight-Clustern aus. Dieser Artikel baut auf dem Artikel zu [Datentransformationsaktivitäten](data-factory-data-transformation-activities.md) auf, der eine allgemeine Übersicht über die Datentransformation und die unterstützten Transformationsaktivitäten bietet.

## Syntax

	{
		"name": "Hive Activity",
	    "description": "description",
	    "type": "HDInsightHive",
	    "inputs": [
	      {
	        "name": "input tables"
	      }
	    ],
	    "outputs": [
	      {
	        "name": "output tables"
	      }
	    ],
	    "linkedServiceName": "MyHDInsightLinkedService",
	    "typeProperties": {
	      "script": "Hive script",
	      "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
	      "defines": {
	        "param1": "param1Value"
	      }
	    },
       "scheduler": {
          "frequency": "Day",
          "interval": 1
        }
	}
	
## Syntaxdetails

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Name | Der Name der Aktivität | Ja
description | Ein Text, der beschreibt, wofür die Aktivität verwendet wird. | Nein
Typ | HDInsightHive | Ja
inputs | Von der Hive-Aktivität genutzte Eingaben | Nein
outputs | Von der Hive-Aktivität erzeugte Ausgaben | Ja 
linkedServiceName | Verweis auf den HDInsight-Cluster, der als verknüpfter Dienst in Data Factory registriert ist. | Ja 
script | Angabe des Hive-Skripts inline | Nein
scriptPath | Speichern Sie das Hive-Skript in einem Azure-Blobspeicher, und geben Sie den Pfad zur Datei an. Verwenden Sie die Eigenschaft "script" oder "scriptPath". Beide können nicht zusammen verwendet werden. Beim Dateinamen muss die Groß-/Kleinschreibung beachtet werden. | Nein 
defines | Geben Sie Parameter als Schlüssel-Wert-Paare für Verweise innerhalb des Hive-Skripts mit "hiveconf" an. | Nein

## Beispiel

Betrachten wir ein Beispiel mit Analysen von Spielprotokollen, in dem Sie die Zeit ermitteln möchten, die Benutzern mit den Spielen Ihres Unternehmens verbringen.

Im Folgenden finden Sie ein Beispielspielprotokoll mit Kommas (`,`) als Trennzeichen und den folgenden Feldern: ProfileID, SessionStart, Duration, SrcIPAddress und GameType.

	1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
	1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
	1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
	1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
	.....

Das **Hive-Skript** zur Verarbeitung dieser Daten sieht folgendermaßen aus:

	DROP TABLE IF EXISTS HiveSampleIn; 
	CREATE EXTERNAL TABLE HiveSampleIn 
	(
		ProfileID		string, 
	    SessionStart 	string, 
	    Duration 		int, 
	    SrcIPAddress 	string, 
	    GameType 		string
	) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/samplein/'; 
	
	DROP TABLE IF EXISTS HiveSampleOut; 
	CREATE EXTERNAL TABLE HiveSampleOut 
	(	
		ProfileID 	string, 
	    Duration 	int
	) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/sampleout/';
	
	INSERT OVERWRITE TABLE HiveSampleOut
	Select 
		ProfileID,
		SUM(Duration)
	FROM HiveSampleIn Group by ProfileID

Um dieses Hive-Skript in einer Data Factory-Pipeline auszuführen, müssen Sie folgende Schritte ausführen:

1. Erstellen Sie einen verknüpften Dienst, um [Ihren eigenen HDInsight-Compute-Cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) zu registrieren oder einen [bedarfsgesteuerten HDInsight-Compute-Cluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) zu konfigurieren. Wir nennen diesen verknüpften Dienst "HDInsightLinkedService".
2. Erstellen Sie einen [verknüpften Dienst](data-factory-azure-blob-connector.md), um die Verbindung mit dem Azure-Blobspeicher zu konfigurieren, in dem die Daten gehostet werden. Wir nennen diesen verknüpften Dienst "StorageLinkedService".
3. Erstellen Sie [Datasets](data-factory-create-datasets.md), die auf die Eingabe- und die Ausgabedaten verweisen. Wir nennen das Eingabedataset "HiveSampleIn" und das Ausgabedataset "HiveSampleOut".
4. Kopieren Sie die Hive-Abfrage als Datei in den Azure-Blobspeicher, den Sie in Schritt 2 konfiguriert haben. Wenn sich der Speicher zum Hosten der Daten von dem Speicher zum Hosten dieser Abfragedatei unterscheidet, erstellen Sie einen separaten verknüpften Azure Storage-Dienst, und verweisen Sie in der Aktivitätskonfiguration darauf. Geben Sie mit **scriptPath** den Pfad zur Hive-Abfragedatei und mit **scriptLinkedService** den Azure-Speicher mit der Skriptdatei an.

	> [AZURE.NOTE] Sie können das Hive-Skript auch inline in der Aktivitätsdefinition bereitstellen, indem Sie die **script**-Eigenschaft verwenden. Dieser Ansatz wird jedoch nicht empfohlen, da alle Sonderzeichen im Skript innerhalb des JSON-Dokuments mit Escapezeichen versehen werden müssen und zu Debuggingproblemen führen können. Die bewährte Methode ist, Schritt 4 auszuführen.
5.	Erstellen Sie eine Pipeline mit der HDInsightHive-Aktivität. Die Aktivität verarbeitet/transformiert die Daten.

		{
		  "name": "HiveActivitySamplePipeline",
		  "properties": {
		    "activities": [
		      {
		        "name": "HiveActivitySample",
		        "type": "HDInsightHive",
		        "inputs": [
		          {
		            "name": "HiveSampleIn"
		          }
		        ],
		        "outputs": [
		          {
		            "name": "HiveSampleOut"
		          }
		        ],
		        "linkedServiceName": "HDInsightLinkedService",
		        "typeproperties": {
		          "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
		          "scriptLinkedService": "StorageLinkedService"
		        },
       			"scheduler": {
          			"frequency": "Hour",
          			"interval": 1
        		}
		      }
		    ]
		  }
		}

6.	Stellen Sie die Pipeline bereit. Weitere Informationen finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md).
7.	Überwachen Sie die Pipeline mithilfe der Überwachungs- und Verwaltungsansichten von Data Factory. Weitere Informationen finden Sie im Artikel [Überwachen und Verwalten von Data Factory-Pipelines](data-factory-monitor-manage-pipelines.md).


## Angeben der Parameter für ein Hive-Skript  
In diesem Beispiel werden die Spielprotokolle täglich im Azure-Blobspeicher erfasst und in einem mit Datum und Uhrzeit partitionierten Ordner gespeichert. Sie möchten das Hive-Skript parametrisieren, den Eingabeordnerpfad dynamisch während der Laufzeit übergeben und zudem die Ausgabe partitioniert mit Datum und Uhrzeit erzeugen.

Gehen Sie folgendermaßen vor, um parametrisierte Hive-Skripts zu verwenden:

- Legen Sie die Parameter in **defines** fest.

		{
			"name": "HiveActivitySamplePipeline",
		  	"properties": {
		    "activities": [
		     	{
		        	"name": "HiveActivitySample",
		        	"type": "HDInsightHive",
			        "inputs": [
			          	{
				            "name": "HiveSampleIn"
				          }
		        	],
		        	"outputs": [
		          		{
				            "name": "HiveSampleOut"
				        }
		        	],
		        	"linkedServiceName": "HDInsightLinkedService",
		        	"typeproperties": {
		          		"scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
		          		"scriptLinkedService": "StorageLinkedService",
		          		"defines": {
		            		"Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)",
		            		"Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
		          		},
       					"scheduler": {
          					"frequency": "Hour",
          					"interval": 1
        				}
		        	}
		      	}
		    ]
		  }
		}

- Verweisen Sie im Hive-Skript mit **${hiveconf:parameterName}** auf den Parameter.

		DROP TABLE IF EXISTS HiveSampleIn; 
		CREATE EXTERNAL TABLE HiveSampleIn 
		(
			ProfileID 	string, 
		    SessionStart 	string, 
		    Duration 	int, 
		    SrcIPAddress 	string, 
		    GameType 	string
		) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Input}'; 
		
		DROP TABLE IF EXISTS HiveSampleOut; 
		CREATE EXTERNAL TABLE HiveSampleOut 
		(
		    ProfileID 	string, 
		    Duration 	int
		) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Output}';
		
		INSERT OVERWRITE TABLE HiveSampleOut
		Select 
			ProfileID,
			SUM(Duration)
		FROM HiveSampleIn Group by ProfileID


## Weitere Informationen
- [Pig-Aktivität](data-factory-pig-activity.md)
- [MapReduce-Aktivität](data-factory-map-reduce.md)
- [Hadoop-Streamingaktivität](data-factory-hadoop-streaming-activity.md)
- [Invoke Spark programs (Aufrufen von Spark-Programmen)](data-factory-spark.md)
- [Invoke R scripts (Aufrufen von R-Skripts)](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

<!---HONumber=AcomDC_0921_2016-->