---
title: "Pig-Aktivität"
description: "Erfahren Sie, wie Sie die Pig-Aktivität in Azure Data Factory verwenden können, um Pig-Abfragen in einem bedarfsgesteuerten/eigenen HDInsight-Cluster auszuführen."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 5af07a1a-2087-455e-a67b-a79841b4ada5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2016
ms.author: shlo
translationtype: Human Translation
ms.sourcegitcommit: ab5923ffeac5e86eaf643ba79c56fb57a34f9f87
ms.openlocfilehash: 3ee61c0a1bb3ff72017768337773373df50ef927


---
# <a name="pig-activity"></a>Pig-Aktivität
> [!div class="op_single_selector"]
> * [Hive](data-factory-hive-activity.md)  
> * [Pig](data-factory-pig-activity.md)  
> * [MapReduce](data-factory-map-reduce.md)  
> * [Hadoop-Datenströme](data-factory-hadoop-streaming-activity.md)
> * [Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
> * [Gespeicherte Prozedur](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL](data-factory-usql-activity.md)
> * [Benutzerdefinierte .NET-Aktivität](data-factory-use-custom-activities.md)

Die HDInsight Pig-Aktivität in einer Data Factory-[Pipeline](data-factory-create-pipelines.md) wendet Pig-Abfragen auf [Ihren eigenen](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) oder [bedarfsgesteuerten](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows-/Linux-basierten HDInsight-Cluster an. Dieser Artikel baut auf dem Artikel zu [Datentransformationsaktivitäten](data-factory-data-transformation-activities.md) auf, der eine allgemeine Übersicht über die Datentransformation und die unterstützten Transformationsaktivitäten bietet.

## <a name="syntax"></a>Syntax
    {
        "name": "HiveActivitySamplePipeline",
          "properties": {
        "activities": [
            {
                "name": "Pig Activity",
                "description": "description",
                "type": "HDInsightPig",
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
                      "script": "Pig script",
                      "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
                      "defines": {
                        "param1": "param1Value"
                      }
                },
                   "scheduler": {
                      "frequency": "Day",
                      "interval": 1
                }
              }
        ]
      }
    }

## <a name="syntax-details"></a>Syntaxdetails
| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| Name |Der Name der Aktivität |Ja |
| Beschreibung |Ein Text, der beschreibt, wofür die Aktivität verwendet wird. |Nein |
| Typ |HDInsightPig |Ja |
| inputs |Mindestens eine von der Pig-Aktivität genutzte Eingabe |Nein |
| outputs |Mindestens eine von der Pig-Aktivität erzeugte Ausgabe |Ja |
| linkedServiceName |Verweis auf den HDInsight-Cluster, der als verknüpfter Dienst in Data Factory registriert ist. |Ja |
| script |Angabe des Pig-Skripts inline |Nein |
| Skriptpfad |Speichern Sie das Pig-Skript in einem Azure-Blobspeicher, und geben Sie den Pfad zur Datei an. Verwenden Sie die Eigenschaft "script" oder "scriptPath". Beide können nicht zusammen verwendet werden. Beim Dateinamen muss die Groß-/Kleinschreibung beachtet werden. |Nein |
| defines |Geben Sie Parameter als Schlüssel-Wert-Paare für Verweise innerhalb des Pig-Skripts an. |Nein |

## <a name="example"></a>Beispiel
Betrachten wir ein Beispiel mit Analysen von Spielprotokollen, in dem Sie die Zeit ermitteln möchten, die Benutzer mit den Spielen Ihres Unternehmens verbringen.

Das folgende Beispielspielprotokoll ist eine durch Komma (,) getrennte Datei. Es enthält die folgenden Felder: ProfileID, SessionStart, Duration, SrcIPAddress und GameType.

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

Das **Pig-Skript** zur Verarbeitung dieser Daten sieht folgendermaßen aus:

    PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);

    GroupProfile = Group PigSampleIn all;

    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);

    Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');

Um dieses Pig-Skript in einer Data Factory-Pipeline auszuführen, führen Sie folgende Schritte aus:

1. Erstellen Sie einen verknüpften Dienst, um [Ihren eigenen HDInsight-Computecluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) zu registrieren oder einen [bedarfsgesteuerten HDInsight-Computecluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) zu konfigurieren. Wir nennen diesen verknüpften Dienst **HDInsightLinkedService**.
2. Erstellen Sie einen [verknüpften Dienst](data-factory-azure-blob-connector.md) , um die Verbindung mit dem Azure-Blobspeicher zu konfigurieren, in dem die Daten gehostet werden. Wir nennen diesen verknüpften Dienst **StorageLinkedService**.
3. Erstellen Sie [Datasets](data-factory-create-datasets.md) , die auf die Eingabe- und die Ausgabedaten verweisen. Wir nennen das Eingabedataset **PigSampleIn** und das Ausgabedataset **PigSampleOut**.
4. Kopieren Sie die Pig-Abfrage in eine Datei in dem in Schritt 2 konfigurierten Azure-Blobspeicher. Wenn der Azure-Speicher, der die Daten hostet, sich von dem Speicher unterscheidet, der die Abfragedatei hostet, erstellen Sie einen separaten mit verknüpften Azure Storage-Dienst. Informationen dazu finden Sie im verknüpften Dienst in der Aktivitätskonfiguration. Verwenden Sie **scriptPath**, um den Pfad zur Pig-Skriptdatei und zum **scriptLinkedService** anzugeben. 
   
   > [!NOTE]
   > Sie können das Pig-Skript auch inline in der Aktivitätsdefinition bereitstellen, indem Sie die **script** -Eigenschaft verwenden. Dieser Ansatz wird jedoch nicht empfohlen, da alle Sonderzeichen im Skript mit Escapezeichen versehen werden müssen und zu Debuggingproblemen führen können. Die bewährte Methode ist, Schritt 4 auszuführen.
   > 
   > 
5. Erstellen Sie die Pipeline mit der HDInsightPig-Aktivität. Diese Aktivität verarbeitet die Eingabedaten durch Ausführen des Pig-Skripts im HDInsight-Cluster.
   
        {
          "name": "PigActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "PigActivitySample",
                "type": "HDInsightPig",
                "inputs": [
                  {
                    "name": "PigSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "PigSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\enrichlogs.pig",
                  "scriptLinkedService": "StorageLinkedService"
                },
                   "scheduler": {
                      "frequency": "Day",
                      "interval": 1
                }
              }
            ]
          }
        } 
6. Stellen Sie die Pipeline bereit. Weitere Informationen finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md) . 
7. Überwachen Sie die Pipeline mithilfe der Überwachungs- und Verwaltungsansichten von Data Factory. Weitere Informationen finden Sie im Artikel [Überwachen und Verwalten von Data Factory-Pipelines](data-factory-monitor-manage-pipelines.md) .

## <a name="specifying-parameters-for-a-pig-script"></a>Angeben der Parameter für ein Pig-Skript
Betrachten Sie das folgende Beispiel: Spielprotokolle werden täglich im Azure-Blobspeicher erfasst und in einem basierend auf Datum und Uhrzeit partitionierten Ordner gespeichert. Sie möchten das Pig-Skript parametrisieren, den Eingabeordnerpfad dynamisch während der Laufzeit übergeben und zudem die Ausgabe partitioniert mit Datum und Uhrzeit erzeugen.

Gehen Sie folgendermaßen vor, um das parametrisierte Pig-Skript zu verwenden:

* Legen Sie die Parameter in **defines**fest.
  
        {
            "name": "PigActivitySamplePipeline",
              "properties": {
            "activities": [
                {
                    "name": "PigActivitySample",
                    "type": "HDInsightPig",
                    "inputs": [
                          {
                            "name": "PigSampleIn"
                          }
                    ],
                    "outputs": [
                          {
                            "name": "PigSampleOut"
                          }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                          "scriptPath": "adfwalkthrough\\scripts\\samplepig.hql",
                          "scriptLinkedService": "StorageLinkedService",
                          "defines": {
                            "Input": "$$Text.Format('wasb: //adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0: yyyy}/monthno={0: %M}/dayno={0: %d}/',SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                          }
                    },
                       "scheduler": {
                          "frequency": "Day",
                          "interval": 1
                    }
                  }
            ]
          }
        }  
* Verweisen Sie im Pig-Skript mit "**$parameterName**" wie im folgenden Beispiel gezeigt auf die Parameter:
  
        PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);    
        GroupProfile = Group PigSampleIn all;        
        PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);        
        Store PigSampleOut into '$Output' USING PigStorage (','); 

## <a name="see-also"></a>Weitere Informationen
* [Hive-Aktivität](data-factory-hive-activity.md)
* [MapReduce-Aktivität](data-factory-map-reduce.md)
* [Hadoop-Streamingaktivität](data-factory-hadoop-streaming-activity.md)
* [Invoke Spark programs (Aufrufen von Spark-Programmen)](data-factory-spark.md)
* [Invoke R scripts (Aufrufen von R-Skripts)](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)




<!--HONumber=Nov16_HO3-->


