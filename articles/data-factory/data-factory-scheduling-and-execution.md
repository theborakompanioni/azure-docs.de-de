---
title: Planung und Ausführung mit Data Factory | Microsoft Docs
description: Informationen zu den Planungs- und Ausführungsaspekten des Azure Data Factory-Anwendungsmodells.
services: data-factory
documentationcenter: ''
author: spelluru
manager: jhubbard
editor: monicar

ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2016
ms.author: spelluru

---
# <a name="data-factory-scheduling-and-execution"></a>Data Factory – Planung und Ausführung
In diesem Artikel werden die Planungs- und Ausführungsaspekte des Azure Data Factory-Anwendungsmodells erläutert. 

## <a name="prerequisites"></a>Voraussetzungen
In diesem Artikel wird vorausgesetzt, dass Sie mit den Grundbegriffen des Data Factory-Anwendungsmodells vertraut sind, z.B. Aktivität, Pipelines, verknüpfte Dienste und Datasets. Grundlegende Konzepte von Azure Data Factory finden Sie in den folgenden Artikeln:

* [Einführung in Data Factory](data-factory-introduction.md)
* [Pipelines](data-factory-create-pipelines.md)
* [Datasets](data-factory-create-datasets.md) 

## <a name="schedule-an-activity"></a>Planen einer Aktivität
Im Abschnitt „scheduler“ im JSON-Code der Aktivität können Sie eine Zeitplanserie für die Aktivität angeben. Beispielsweise können Sie wie folgt eine stündliche Ausführung einer Aktivität planen:

    "scheduler": {
        "frequency": "Hour",
        "interval": 1
    },  

![Beispiel für "scheduler"](./media/data-factory-scheduling-and-execution/scheduler-example.png)

Wie im obigen Diagramm gezeigt, wird durch Angabe eines Zeitplans für die Aktivität eine Reihe rollierender Fenster erstellt. Rollierende Fenster sind eine Reihe von sich nicht überlappenden zusammenhängenden Zeitintervallen mit fester Größe. Diese logischen rollierenden Fenster für die Aktivität heißen *Aktivitätsfenster*.

Für das derzeit ausgeführte Aktivitätsfenster können Sie im JSON-Code der Aktivität über die Systemvariablen [WindowStart](data-factory-functions-variables.md#data-factory-system-variables) und [WindowEnd](data-factory-functions-variables.md#data-factory-system-variables) auf das dem Aktivitätsfenster zugeordnete Zeitintervall zugreifen. Sie können diese Variablen für unterschiedliche Zwecke im JSON-Code der Aktivität verwenden. Beispielsweise können Sie sie zum Auswählen von Daten aus Eingabe- und Ausgabedatasets verwenden, die Zeitreihendaten darstellen.

Die **scheduler**-Eigenschaft unterstützt dieselben untergeordneten Eigenschaften wie die **availability**-Eigenschaft in einem Dataset. Details finden Sie unter [Dataset: Availability](data-factory-create-datasets.md#Availability) . Beispiele sind das Planen eines bestimmten Zeitoffsets oder das Festlegen des Modus zum Ausrichten der Verarbeitung am Anfang oder Ende des Intervalls für das Aktivitätsfenster.

Sie können optional **scheduler**-Eigenschaften für eine Aktivität angeben. Diese Eigenschaft ist allerdings **optional**. Wenn Sie eine Eigenschaft angeben, muss sie mit dem Rhythmus übereinstimmen, den Sie in der Definition des Ausgabedatasets angegeben haben. Derzeit steuert das Ausgabedataset den Zeitplan, sodass Sie auch dann ein Ausgabedataset erstellen müssen, wenn die Aktivität keine Ausgabe erzeugt. Wenn die Aktivität keine Eingabe akzeptiert, können Sie das Erstellen des Eingabedatasets überspringen.

## <a name="time-series-datasets-and-data-slices"></a>Datasets und Datenslices von Zeitreihen
Zeitreihendaten stellen eine fortlaufende Abfolge von Datenpunkten dar, die in der Regel aus aufeinander folgenden Messungen in einem Zeitintervall bestehen. Gängige Beispiele für Zeitreihendaten sind Sensordaten und Anwendungstelemetriedaten.

Mithilfe von Data Factory können Sie Zeitreihendaten in Form von Aktivitätsausführungen im Batchmodus verarbeiten. Es gibt in der Regel einen regelmäßigen Rhythmus, in dem Eingabedaten erfasst und Ausgabedaten erstellt werden müssen. Dieser Rhythmus wird durch das Festlegen der Verfügbarkeit ( **availability** ) für das Dataset wie folgt bestimmt:

    "availability": {
      "frequency": "Hour",
      "interval": 1
    },

Jede Einheit von Daten, die durch eine Aktivitätsausführung genutzt und erstellt wird, heißt Datenslice. Das folgende Diagramm zeigt das Beispiel einer Aktivität mit einem Eingabedataset und einem Ausgabedataset. Die **availability** dieser Datasets wurde auf eine stündliche Frequenz festgelegt.

![Scheduler für "availability"](./media/data-factory-scheduling-and-execution/availability-scheduler.png)

Im vorherigen Diagramm sind die stündlichen Datenslices für das Eingabe- und Ausgabedataset dargestellt. Das Diagramm enthält drei Eingabeslices, die bereit für die Verarbeitung sind. Die Aktivität von 10 bis 11 Uhr ist in Bearbeitung und produziert den Ausgabeslice für „10-11 AM“.

Sie können auf das Zeitintervall des aktuell erzeugten Slice im JSON-Code des Datasets über die Variablen [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) und [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables) zugreifen.

Derzeit ist es für Data Factory erforderlich, dass der in der Aktivität angegebene Zeitplan exakt mit dem Zeitplan übereinstimmt, der unter **availability** für das Ausgabedataset angegeben ist. Daher sind **WindowStart**, **WindowEnd**, **SliceStart** und **SliceEnd** immer dem gleichen Zeitraum und einem einzelnen Ausgabeslice zugeordnet.

Weitere Informationen zu verschiedenen Eigenschaften, die im Abschnitt „availability“ verfügbar sind, finden Sie unter [Datasets in Azure Data Factory](data-factory-create-datasets.md).

## <a name="move-data-from-sql-database-to-blob-storage"></a>Verschieben von Daten aus Azure SQL-Datenbank in Azure Blob Storage
Nun wollen wir uns diese Dinge in Aktion ansehen, indem wir eine Pipeline erstellen, die Daten stündlich aus einer Azure SQL-Datenbanktabelle in Azure-Blobspeicher kopiert.

**Eingabe: Azure SQL-Datenbankdataset**

    {
        "name": "AzureSqlInput",
        "properties": {
            "published": false,
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }


Im Abschnitt „availability“ ist **Frequency** auf **Hour** und **interval** auf **1** festgelegt.

**Ausgabe: Azure-Blobspeicher-Dataset**

    {
        "name": "AzureBlobOutput",
        "properties": {
            "published": false,
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mypath/{Year}/{Month}/{Day}/{Hour}",
                "format": {
                    "type": "TextFormat"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%M"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%d"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%H"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


Im Abschnitt „availability“ ist **Frequency** auf **Hour** und **interval** auf **1** festgelegt.

**Aktivität: Kopieraktivität**

    {
        "name": "SamplePipeline",
        "properties": {
            "description": "copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "name": "AzureSQLtoBlob",
                    "description": "copy activity",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource",
                            "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 100000,
                            "writeBatchTimeout": "00:05:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureSQLInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    }
                }
            ],
            "start": "2015-01-01T08:00:00Z",
            "end": "2015-01-01T11:00:00Z"
        }
    }


Das Beispiel zeigt die Abschnitte mit dem Aktivitätszeitplan und der Verfügbarkeit von Datasets, die auf eine stündliche Frequenz festgelegt sind. Das Beispiel veranschaulicht, wie Sie **WindowStart** und **WindowEnd** verwenden können, um relevante Daten für eine Aktivitätsausführung auszuwählen und in ein Blob mit entsprechendem **folderPath** zu kopieren. Der Ordnerpfad ( **folderPath** ) ist so parametrisiert, dass für jede Stunde ein separater Ordner angelegt wird.

Wenn drei der Slices zwischen 8 und 11 Uhr ausgeführt werden, lauten die Daten in Azure SQL-Datenbank wie folgt:

![Beispieleingabe](./media/data-factory-scheduling-and-execution/sample-input-data.png)

Nach der Bereitstellung der Pipeline wird das Azure-Blob wie folgt aufgefüllt:

* Datei „mypath/2015/1/1/8/Data.&lt;GUID&gt;.txt“ mit Daten
  
          10002345,334,2,2015-01-01 08:24:00.3130000
          10002345,347,15,2015-01-01 08:24:00.6570000
          10991568,2,7,2015-01-01 08:56:34.5300000
  
  > [!NOTE]
  > &lt;Guid&gt; wird durch eine tatsächliche GUID ersetzt. Beispieldateiname: Data.bcde1348-7620-4f93-bb89-0eed3455890b.txt
  > 
  > 
* Datei „mypath/2015/1/1/9/Data.&lt;GUID&gt;.txt“ mit Daten:
  
          10002345,334,1,2015-01-01 09:13:00.3900000
          24379245,569,23,2015-01-01 09:25:00.3130000
          16777799,21,115,2015-01-01 09:47:34.3130000
* Datei „mypath/2015/1/1/10/Data.&lt;GUID&gt;.txt“ ohne Daten:

## <a name="active-period-for-pipeline"></a>Aktiver Zeitraum für Pipeline
Unter [Pipelines und Aktivitäten in Azure Data Factory: Erstellen/Planen von Pipelines und Kettenaktivitäten](data-factory-create-pipelines.md) wurde das Konzept eines aktiven Zeitraums für eine Pipeline vorgestellt, der durch das Festlegen der Eigenschaften **start** und **end** angegeben wird.

Sie können das Startdatum für den aktiven Zeitraum der Pipeline auf ein Datum in der Vergangenheit festlegen. Data Factory führt automatisch eine Berechnung (einen Abgleich) aller Datenslices in der Vergangenheit aus und beginnt mit ihrer Verarbeitung.

## <a name="parallel-processing-of-data-slices"></a>Parallele Verarbeitung von Datenslices
Sie können abgeglichene Datenslices für eine parallele Ausführung konfigurieren, indem Sie die Eigenschaft **concurrency** im Abschnitt „policy“ des JSON-Codes der Aktivität festlegen. Weitere Informationen zu dieser Eigenschaft finden Sie unter [Erstellen von Pipelines](data-factory-create-pipelines.md).

## <a name="rerun-a-failed-data-slice"></a>Wiederholen eines fehlerhaften Datenslices
Sie können die Ausführung von Slices umfassend visuell überwachen. Ausführliche Informationen finden Sie unter [Überwachen und Verwalten von Pipelines mithilfe von Blättern im Azure-Portal](data-factory-monitor-manage-pipelines.md) oder [App „Überwachung und Verwaltung“](data-factory-monitor-manage-app.md).

Betrachten Sie das folgende Beispiel mit zwei Aktivitäten. Aktivität1 erstellt ein Zeitreihen-Dataset mit Slices als Ausgabe, die als Eingabe von Aktivität2 verwendet wird, um die endgültige Ausgabe des Zeitreihen-Datasets zu erstellen.

![Fehlerhafter Slice](./media/data-factory-scheduling-and-execution/failed-slice.png)

Das Diagramm zeigt, dass bei den letzten drei Slices ein Fehler beim Erstellen des Slice „9-10 AM“ für Dataset2 aufgetreten ist. Data Factory verfolgt die Abhängigkeit für das Zeitreihen-Dataset automatisch. Aus diesem Grund wird die Aktivitätsausführung für den nachgelagerten Slice für den Zeitraum von 9 bis 10 Uhr nicht gestartet.

Mit den Data Factory-Überwachungs- und Verwaltungstools können Sie die Diagnoseprotokolle detailliert nach dem fehlerhaften Slice durchsuchen, um die Ursache des Problems leicht zu finden und zu beseitigen. Nachdem Sie das Problem behoben haben, können Sie die Aktivitätsausführung ganz einfach starten, um den fehlerhaften Slice zu erstellen. Weitere Informationen zur erneuten Ausführung und Grundlegendes zu Statusübergängen für Datenslices finden Sie unter [Überwachen und Verwalten von Pipelines mithilfe von Blättern im Azure-Portal](data-factory-monitor-manage-pipelines.md) oder [App „Überwachung und Verwaltung“](data-factory-monitor-manage-app.md).

Nachdem Sie den Slice „9-10 AM“ für **Dataset2**erneut ausgeführt haben, startet Data Factory die Ausführung für den von „9-10 AM“ abhängigen Slice im endgültigen Dataset.

![Wiederholen eines fehlerhaften Slices](./media/data-factory-scheduling-and-execution/rerun-failed-slice.png)

## <a name="run-activities-in-a-sequence"></a>Ausführen von Aktivitäten in einer Sequenz
Sie können zwei Aktivitäten verketten (nacheinander ausführen), indem Sie das Ausgabedataset einer Aktivität als Eingabedataset der anderen Aktivität festlegen. Die Aktivitäten können sich in derselben Pipeline oder in verschiedenen Pipelines befinden. Die zweite Aktivität wird nur ausgeführt, wenn die erste erfolgreich abgeschlossen wurde.

Betrachten Sie beispielsweise den folgenden Fall:

1. Die Pipeline P1 enthält die Aktivität A1, für die das externe Eingabedataset D1 erforderlich ist und die das Ausgabedataset D2 generiert.
2. Die Pipeline P2 enthält die Aktivität A2, für die eine Eingabe aus dem Dataset D2 erforderlich ist und die das Ausgabedataset D3 generiert.

In diesem Szenario befinden sich die Aktivitäten A1 und A2 in verschiedenen Pipelines. Die Aktivität A1 wird ausgeführt, wenn die externen Daten verfügbar sind und die Häufigkeit für die geplante Verfügbarkeit erreicht ist. Die Aktivität A2 wird ausgeführt, wenn die geplanten Slices von D2 verfügbar werden und die Häufigkeit für die geplante Verfügbarkeit erreicht ist. Wenn ein Fehler in einem der Slices im Dataset D2 auftritt, wird A2 für diesen Slice nicht ausgeführt, bis er verfügbar wird.

Die Diagrammansicht sieht in diesem Fall wie im folgenden Diagramm dargestellt aus:

![Verketten von Aktivitäten in zwei Pipelines](./media/data-factory-scheduling-and-execution/chaining-two-pipelines.png)

Wie bereits erwähnt, können sich die Aktivitäten in derselben Pipeline befinden. Die Diagrammansicht mit beiden Aktivitäten in derselben Pipeline sieht wie im folgenden Diagramm dargestellt aus:

![Verketten von Aktivitäten in derselben Pipeline](./media/data-factory-scheduling-and-execution/chaining-one-pipeline.png)

### <a name="copy-sequentially"></a>Sequenzielles Kopieren
Es ist möglich, mehrere Kopiervorgänge nacheinander sequenziell/sortiert auszuführen. Sie können beispielsweise zwei Kopieraktivitäten in einer Pipeline („CopyActivity1“ und „CopyActivity2“) mit den folgenden Eingabe-/Ausgabedatasets verwenden:   

CopyActivity1

Eingabe: Dataset Ausgabe: Dataset2

CopyActivity2

Eingabe: Dataset2  Ausgabe: Dataset3

CopyActivity2 wird nur ausgeführt, wenn CopyActivity1 erfolgreich ausgeführt wurde und Dataset2 verfügbar ist.

Hier finden Sie das Beispiel für die Pipeline-JSON:

    {
        "name": "ChainActivities",
        "properties": {
            "description": "Run activities in sequence",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "copyBehavior": "PreserveHierarchy",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset1"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob1ToBlob2",
                    "description": "Copy data from a blob to another"
                },
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset3"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob2ToBlob3",
                    "description": "Copy data from a blob to another"
                }
            ],
            "start": "2016-08-25T01:00:00Z",
            "end": "2016-08-25T01:00:00Z",
            "isPaused": false
        }
    }

Beachten Sie, dass in diesem Beispiel das Ausgabedataset der ersten Kopieraktivität (Dataset2) als Eingabe für die zweite Aktivität festgelegt ist. Aus diesem Grund wird die zweite Aktivität nur ausgeführt, wenn das Ausgabedataset der ersten Aktivität bereit ist.  

Im Beispiel kann CopyActivity2 eine andere Eingabe haben, z.B. Dataset3. Sie legen aber Dataset2 als Eingabe für CopyActivity2 fest, damit die Aktivität nicht so lange ausgeführt wird, bis CopyActivity1 abgeschlossen ist. Beispiel:

CopyActivity1

Eingabe: Dataset1 Ausgabe: Dataset2

CopyActivity2

Eingaben: Dataset3, Dataset2 Ausgabe: Dataset4

    {
        "name": "ChainActivities",
        "properties": {
            "description": "Run activities in sequence",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "copyBehavior": "PreserveHierarchy",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset1"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlobToBlob",
                    "description": "Copy data from a blob to another"
                },
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset3"
                        },
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset4"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob3ToBlob4",
                    "description": "Copy data from a blob to another"
                }
            ],
            "start": "2017-04-25T01:00:00Z",
            "end": "2017-04-25T01:00:00Z",
            "isPaused": false
        }
    }


Beachten Sie, dass im Beispiel zwei Eingabedatasets für die zweite Kopieraktivität angegeben sind. Wenn mehrere Eingaben angegeben wurden, wird nur das erste Eingabedataset zum Kopieren der Daten verwendet, aber die anderen Datasets werden als Abhängigkeiten verwendet. CopyActivity2 wird erst gestartet, nachdem die folgenden Bedingungen erfüllt sind:

* CopyActivity1 wurde erfolgreich abgeschlossen und Dataset2 ist verfügbar. Dieses Dataset wird beim Kopieren von Daten in Dataset4 nicht verwendet. Es fungiert nur als Terminplanungs-Abhängigkeit für CopyActivity2.   
* Dataset3 ist verfügbar. Dieses Dataset stellt die Daten dar, die zum Ziel kopiert werden.  

## <a name="model-datasets-with-different-frequencies"></a>Modellieren von Datasets mit unterschiedlichen Frequenzen
In den Beispielen waren die Frequenzen für Eingabe- und Ausgabedatasets und das Aktivitätszeitfenster identisch. Einige Szenarien erfordern die Fähigkeit, eine Ausgabe mit einer Frequenz zu erzeugen, die sich von den Frequenzen einer oder mehrerer Eingaben unterscheidet. Data Factory unterstützt die Modellierung dieser Szenarien.

### <a name="sample-1:-produce-a-daily-output-report-for-input-data-that-is-available-every-hour"></a>Beispiel 1: Erzeugen eines täglichen Ausgabeberichts für Eingabedaten, die stündlich verfügbar sind
In diesem Szenario verwenden Sie Eingabemessdaten von Sensoren, die stündlich im Azure-Blobspeicher verfügbar sind. Sie möchten einen täglichen Aggregationsbericht mit Statistiken wie Mittel-, Höchst- und Mindestwert für den Tag mit der [Hive-Aktivität](data-factory-hive-activity.md)von Data Factory erstellen.

Dieses Szenario können Sie wie folgt mit Data Factory realisieren:

**Eingabedataset**

Die stündlichen Eingabedateien werden im Ordner für den jeweiligen Tag abgelegt. Für die Eingabe ist „availability“ auf „Stündlich“ festgelegt (frequency: **Hour** , interval: 1).

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Ausgabedataset**

Jeden Tag wird eine Ausgabedatei im Ordner des jeweiligen Tags erstellt. Für die Ausgabe ist „availability“ auf „Täglich“ festgelegt (frequency: **Day** , interval: 1).

    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Aktivität: Hive-Aktivität in einer Pipeline**

Das Hive-Skript empfängt wie im folgenden Codeausschnitt dargestellt durch Verwendung der Variablen *WindowStart* die entsprechenden **DateTime** -Informationen als Parameter. Das Hive-Skript verwendet diese Variable zum Laden der Daten aus dem richtigen Ordner für den jeweiligen Tag und zum Ausführen der Aggregation, um die Ausgabe zu generieren.

        {  
            "name":"SamplePipeline",
            "properties":{  
            "start":"2015-01-01T08:00:00",
            "end":"2015-01-01T11:00:00",
            "description":"hive activity",
            "activities": [
                {
                    "name": "SampleHiveActivity",
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adftutorial\\hivequery.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                            "Month": "$$Text.Format('{0:%M}',WindowStart)",
                            "Day": "$$Text.Format('{0:%d}',WindowStart)"
                        }
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },          
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "OldestFirst",
                        "retry": 2,
                        "timeout": "01:00:00"
                    }
                 }
             ]
           }
        }

Das folgende Diagramm zeigt das Szenario im Hinblick auf eine Datenabhängigkeit.

![Datenabhängigkeit](./media/data-factory-scheduling-and-execution/data-dependency.png)

Der Ausgabeslice für jeden Tag hängt von 24 stündlichen Slices aus einem Eingabedataset ab. Data Factory berechnet diese Abhängigkeiten automatisch, indem die Eingabedatenslices ermittelt werden, die in demselben Zeitraum wie der zu erzeugende Ausgabeslice liegen. Ist einer der 24 Eingabeslices nicht verfügbar, wartet Data Factory, bis der Eingabeslice bereit ist. Erst dann wird die tägliche Aktivitätsausführung gestartet.

### <a name="sample-2:-specify-dependency-with-expressions-and-data-factory-functions"></a>Beispiel 2: Angeben von Abhängigkeiten mit Ausdrücken und Data Factory-Funktionen
Lassen Sie uns ein weiteres Szenario betrachten. Angenommen, Sie verwenden eine Hive-Aktivität, die zwei Eingabedatasets verarbeitet. Eines davon verfügt über neue tägliche Daten, und eines erhält jede Woche neue Daten. Angenommen, Sie möchten einen Join-Vorgang auf zwei Eingaben anwenden und eine tägliche Ausgabe erzeugen.

Der einfache Ansatz, bei dem Data Factory die zu verarbeitenden Eingabeslices automatisch durch Abstimmung mit dem Zeitraum des Ausgabedatenslice bestimmt, funktioniert in diesem Fall nicht mehr.

Sie müssen angeben, dass die Data Factory für jede Aktivitätsausführung den Datenslice der letzten Woche als wöchentliches Eingabedataset verwenden soll. Mithilfe von im folgenden Codeausschnitt gezeigten Azure Data Factory-Funktionen können Sie dieses Verhalten implementieren.

**Eingabe1: Azure-Blob**

Die erste Eingabe ist das täglich aktualisierte Azure-Blob.

    {
      "name": "AzureBlobInputDaily",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Eingabe2: Azure-Blob**

Die zweite Eingabe ist das wöchentlich aktualisierte Azure-Blob.

    {
      "name": "AzureBlobInputWeekly",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 7
        }
      }
    }

**Ausgabe: Azure-Blob**

Jeden Tag wird eine Ausgabedatei im Ordner des jeweiligen Tags erstellt. Für die Ausgabe ist „availability“ auf „Täglich“ festgelegt (frequency: **Day** , interval: 1).

    {
      "name": "AzureBlobOutputDaily",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Aktivität: Hive-Aktivität in einer Pipeline**

Die Hive-Aktivität verwendet zwei Eingaben und erzeugt täglich einen Ausgabeslice. Sie können den täglichen Ausgabeslice wie folgt so angeben, dass dieser für die wöchentliche Eingabe vom Eingabeslice der vorherigen Woche abhängt.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2015-01-01T08:00:00",
        "end":"2015-01-01T11:00:00",
        "description":"hive activity",
        "activities": [
          {
            "name": "SampleHiveActivity",
            "inputs": [
              {
                "name": "AzureBlobInputDaily"
              },
              {
                "name": "AzureBlobInputWeekly",
                "startTime": "Date.AddDays(SliceStart, - Date.DayOfWeek(SliceStart))",
                "endTime": "Date.AddDays(SliceEnd,  -Date.DayOfWeek(SliceEnd))"  
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutputDaily"
              }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "type": "HDInsightHive",
            "typeProperties": {
              "scriptPath": "adftutorial\\hivequery.hql",
              "scriptLinkedService": "StorageLinkedService",
              "defines": {
                "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                "Month": "$$Text.Format('{0:%M}',WindowStart)",
                "Day": "$$Text.Format('{0:%d}',WindowStart)"
              }
            },
            "scheduler": {
              "frequency": "Day",
              "interval": 1
            },          
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 2,  
              "timeout": "01:00:00"
            }
           }
         ]
       }
    }


## <a name="data-factory-functions-and-system-variables"></a>Data Factory – Funktionen und Systemvariablen
Unter [Data Factory – Funktionen und Systemvariablen](data-factory-functions-variables.md) finden Sie eine Liste mit den Funktionen und Systemvariablen, die von Data Factory unterstützt werden.

## <a name="data-dependency-deep-dive"></a>Datenabhängigkeiten – Detaillierte Informationen
Zum Generieren eines Datasetslices mittels einer Aktivitätsausführung verwendet Data Factory das folgende *Abhängigkeitsmodell* , um die Beziehungen zwischen den von einer Aktivität genutzten und erzeugten Datasets zu bestimmen.

Der Zeitraum der Eingabedatasets, der zum Generieren des Slice des Ausgabedatasets erforderlich ist, wird als *Abhängigkeitszeitraum*bezeichnet.

Bei einer Aktivitätsausführung wird ein Datasetslice erst erzeugt, wenn die Datenslices in Eingabedatasets innerhalb des Abhängigkeitszeitraums verfügbar sind. Die bedeutet, dass alle Eingabeslices, die den Abhängigkeitszeitraum bilden, den Status **Bereit** aufweisen müssen, damit bei der Aktivitätsausführung ein Slice des Ausgabedatasets erzeugt werden kann.

Zum Generieren des Datasetslice [**start**, **end**] ist eine Funktion erforderlich, die den Datasetslice seinem Abhängigkeitszeitraum zuordnet. Diese Funktion ist im Wesentlichen eine Formel, die Anfang und Ende des Zeitraums des Datenslices in Anfang und Ende des Abhängigkeitszeitraums umwandelt. Formeller ausgedrückt:

    DatasetSlice = [start, end]
    DependecyPeriod = [f(start, end), g(start, end)]

**f** und **g** sind Zuordnungsfunktionen, die Anfang und Ende des Abhängigkeitszeitraums für jede Aktivitätseingabe berechnen.

Wie Sie in den Beispielen gesehen haben, entspricht der Abhängigkeitszeitraum dem Zeitraum des zu erstellenden Datenslices. In diesen Fällen berechnet Data Factory automatisch die Eingabeslices, die in den Abhängigkeitszeitraum fallen.  

Beispiel: Beim Aggregationsbeispiel, bei dem die Ausgabe täglich erzeugt wird und Eingabedaten stündlich verfügbar sind, beträgt der Zeitraum des Datenslice 24 Stunden. Data Factory sucht die relevanten stündlichen Eingabeslices für diesen Zeitraum und macht den Ausgabeslice vom Eingabeslice abhängig.

Wie im Beispiel, bei dem eine der Eingaben wöchentlich erfolgt und der Ausgabeslice täglich erzeugt wird, können Sie auch eine eigene Zuordnung für den Abhängigkeitszeitraum angeben.

## <a name="data-dependency-and-validation"></a>Datenabhängigkeit und -überprüfung
Für ein Dataset kann eine Überprüfungsrichtlinie definiert sein, die angibt, wie die von einer Sliceausführung generierten Daten überprüft werden können, ehe sie zur Nutzung bereit sind. Weitere Informationen finden Sie unter [Datasets in Azure Data Factory](data-factory-create-datasets.md) .

In solchen Fällen wird nach Beendigung der Sliceausführung der Status des Ausgabeslice in **Warten** mit dem Unterstatus **Überprüfung** geändert. Nach der Überprüfung der Slices ändert sich der Slicestatus in **Bereit**.

Wenn ein Datenslice erstellt wurde, aber die Überprüfung nicht bestanden hat, werden Aktivitätsausführungen für nachgelagerte, von diesem Slice abhängige Slices nicht verarbeitet.

[Überwachen und Verwalten von Pipelines](data-factory-monitor-manage-pipelines.md) behandelt.

## <a name="external-data"></a>Externe Daten
Ein Dataset kann (wie im folgenden JSON-Codeausschnitt gezeigt) als extern gekennzeichnet werden, um anzugeben, dass es nicht mit Data Factory erstellt wurde. In einem solchen Fall kann die Datasetrichtlinie eine Reihe zusätzliche Parameter zum Beschreiben der Überprüfung und eine Wiederholungsrichtlinie für das Dataset aufweisen. Eine Beschreibung aller Eigenschaften finden Sie unter [Pipelines und Aktivitäten in Azure Data Factory: Erstellen/Planen von Pipelines und Kettenaktivitäten](data-factory-create-pipelines.md) .

Ähnlich wie Datasets, die von Data Factory erstellt werden, müssen die Datenslices für externe Daten bereit sein, ehe abhängige Slices verarbeitet werden können.

    {
        "name": "AzureSqlInput",
        "properties":
        {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties":
            {
                "tableName": "MyTable"
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1     
            },
            "external": true,
            "policy":
            {
                "externalData":
                {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }  
        }
    }


## <a name="onetime-pipeline"></a>Pipeline mit einmaliger Ausführung
Sie können eine Pipeline erstellen und zur regelmäßigen Ausführung (z.B. stündlich oder täglich) innerhalb der in der Pipelinedefinition angegebenen Start- und Endzeiten planen. Weitere Informationen finden Sie unter [Planen von Aktivitäten](#scheduling-and-execution). Sie können auch eine Pipeline erstellen, die nur einmal ausgeführt wird. Legen Sie zu diesem Zweck wie im folgenden JSON-Beispiel gezeigt die **pipelineMode**-Eigenschaft in der Pipelinedefinition auf **onetime** (einmalig) fest. Der Standardwert für diese Eigenschaft lautet **scheduled** (geplant).

    {
        "name": "CopyPipeline",
        "properties": {
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource",
                            "recursive": false
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "InputDataset"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "OutputDataset"
                        }
                    ]
                    "name": "CopyActivity-0"
                }
            ]
            "pipelineMode": "OneTime"
        }
    }

Beachten Sie Folgendes:

* Es werden keine Start- und Endzeiten (**start** und **end**) für die Pipeline angegeben.
* Die Verfügbarkeit (**availability**) von Ein- und Ausgabedatasets (**frequency** und **interval**) wird angegeben, auch wenn die Werte von Data Factory nicht verwendet werden.  
* Die Diagrammansicht zeigt einmalig ausgeführte Pipelines nicht an. Dieses Verhalten ist beabsichtigt.
* Einmalige Pipelines können nicht aktualisiert werden. Sie können eine einmalige Pipeline klonen, umbenennen, deren Eigenschaften aktualisieren und sie bereitstellen, um eine andere Pipeline zu erstellen.

<!--HONumber=Oct16_HO2-->


