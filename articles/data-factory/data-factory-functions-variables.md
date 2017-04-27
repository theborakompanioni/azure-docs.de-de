---
title: "Data Factory – Funktionen und Systemvariablen | Microsoft Docs"
description: "Enthält eine Liste der Funktionen und Systemvariablen von Azure Data Factory."
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
services: data-factory
ms.assetid: b6b3c2ae-b0e8-4e28-90d8-daf20421660d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2017
ms.author: shlo
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 74f2fafdf7355bbce37cf2bf98a6e709ebb7986e
ms.lasthandoff: 04/12/2017


---
# <a name="azure-data-factory---functions-and-system-variables"></a>Azure Data Factory – Funktionen und Systemvariablen
Dieser Artikel enthält Informationen zu Funktionen und Variablen, die von Azure Data Factory unterstützt werden.

## <a name="data-factory-system-variables"></a>Data Factory-Systemvariablen
| Variablenname | Beschreibung | Objektbereich | JSON-Bereich und Anwendungsfälle |
| --- | --- | --- | --- |
| WindowStart |Anfang des Zeitfensters der aktuellen Aktivitätsausführung |Aktivität |<ol><li>Geben Sie Abfragen zur Datenauswahl an. Informationen finden Sie in den Artikeln zu Connectors, auf die im Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) verwiesen wird).</li> |
| WindowEnd |Ende des Zeitfensters der aktuellen Aktivitätsausführung |Aktivität |Wie oben |
| SliceStart |Anfang des Zeitfensters für den zu erstellenden Datenslice |Aktivität<br/>Dataset |<ol><li>Geben Sie bei der Arbeit mit [Azure-Blob](data-factory-azure-blob-connector.md) und [Dateisystem-Datasets](data-factory-onprem-file-system-connector.md) dynamische Pfade und Dateinamen an.</li><li>Geben Sie Eingabeabhängigkeiten mit Data Factory-Funktionen in der Auflistung der Aktivitätseingaben an.</li></ol> |
| SliceEnd |Ende des Zeitfensters für den zu erstellenden Datenslice |Aktivität<br/>dataset |Das Gleiche wie „SliceStart“ |

> [!NOTE]
> Derzeit erfordert Data Factory, dass der in der Aktivität angegebene Zeitplan exakt mit dem Zeitplan übereinstimmt, der in "availability" für das Ausgabedataset angegeben ist. Daher sind „WindowStart“, „WindowEnd“, „SliceStart“ und „SliceEnd“ immer dem gleichen Zeitraum und einem einzelnen Ausgabeslice zugeordnet.
> 

### <a name="example-for-using-a-system-variable"></a>Beispiel für die Verwendung einer Systemvariablen
Im folgenden Beispiel werden Jahr, Monat, Tag und Uhrzeit von **SliceStart** in separate Variablen extrahiert, die von den Eigenschaften **folderPath** und **fileName** verwendet werden.

```json
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```

## <a name="data-factory-functions"></a>Data Factory-Funktionen
Sie können Funktionen in Data Factory zusammen mit Systemvariablen für folgende Zwecke verwenden:

1. Angeben von Abfragen zur Datenauswahl (siehe die Artikel zu Connectors, auf die im Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) verwiesen wird).
   
   Die Syntax zum Aufrufen einer Data Factory-Funktion ist **$$<function>** für Abfragen zur Datenauswahl und andere Eigenschaften in der Aktivität und den Datasets.  
2. Angeben von Eingabeabhängigkeiten mit Data Factory-Funktionen in der Sammlung der Aktivitätseingaben.
   
    $$ ist nicht erforderlich, um Eingabeabhängigkeitsausdrücke anzugeben.     

Im folgenden Beispiel wird die **sqlReaderQuery**-Eigenschaft in einer JSON-Datei einem Wert zugewiesen, der von der `Text.Format`-Funktion zurückgegeben wird. Dieses Beispiel verwendet ebenfalls die Systemvariable **WindowStart**, die die Startzeit des Zeitfensters für die Aktivitätsausführung darstellt.

```json
{
    "Type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
}
```

Informationen zu verschiedenen verfügbaren Formatierungsoptionen (beispielsweise „yy“ oder „yyyy“) finden Sie im Thema [Benutzerdefinierte Formatzeichenfolgen für Datum und Uhrzeit](https://msdn.microsoft.com/library/8kb3ddd4.aspx). 

### <a name="functions"></a>Functions
In den folgenden Tabellen werden alle Funktionen in Azure Data Factory aufgelistet:

| Kategorie | Funktion | Parameter | Beschreibung |
| --- | --- | --- | --- |
| Time |AddHours(X,Y) |X: DateTime  <br/><br/>Y: int |Fügt Y Stunden der angegebenen Uhrzeit X hinzu. <br/><br/>Beispiel: 05.09.2013 12:00:00 Uhr + 2 Stunden = 05.09.2013 14:00:00 Uhr |
| Time |AddMinutes(X,Y) |X: DateTime  <br/><br/>Y: int |Fügt Y Minuten zu X hinzu.<br/><br/>Beispiel: 15.09.2013 12:00:00 Uhr + 15 Minuten = 15.09.2013 12:15:00 Uhr |
| Time |StartOfHour(X) |X: DateTime  |Ruft die Startzeit der Stunde ab, die von der Stundenkomponente X dargestellt wird. <br/><br/>Beispiel: StartOfHour von 15.09.2013 05:10:23 Uhr ist 15.09.2013 05:00:00 Uhr |
| Datum |AddDays(X,Y) |X: DateTime <br/><br/>Y: int |Addiert Y Tage zu X. <br/><br/>Beispiel: 15.09.2013 12:00:00 Uhr + 2 Tage = 17.09.2013 12:00:00 Uhr<br/><br/>Sie können auch Tage subtrahieren, indem Sie Y als negative Zahl angeben.<br/><br/>Beispiel: 15.09.2013 12:00:00 Uhr - 2 Tage = 13.09.2013 12:00:00 Uhr |
| Date |AddMonths(X,Y) |X: DateTime <br/><br/>Y: int |Fügt Y Monate zu X hinzu.<br/><br/>Beispiel: 15.09.2013 12:00:00 Uhr + 1 Monat = 15.10.2013 12:00:00 Uhr<br/><br/>Sie können auch Monate subtrahieren, indem Sie Y als negative Zahl angeben.<br/><br/>Beispiel: 15.09.2013 12:00:00 Uhr - 1 Monat = 15.8.2013 12:00:00 Uhr|
| Date |AddQuarters(X,Y) |X: DateTime  <br/><br/>Y: int |Fügt Y * 3 Monate zu X hinzu<br/><br/>Beispiel: 15.09.2013 12:00:00 Uhr + 1 Quartal = 15.12.2013 12:00:00 Uhr |
| Date |AddWeeks(X,Y) |X: DateTime <br/><br/>Y: int |Addiert Y * 7 Tage zu X<br/><br/>Beispiel: 15.09.2013 12:00:00 Uhr + eine Woche = 22.09.2013 12:00:00 Uhr<br/><br/>Sie können auch Wochen subtrahieren, indem Sie Y als negative Zahl angeben.<br/><br/>Beispiel: 15.09.2013 12:00:00 Uhr - 1 Woche = 07.09.2013 12:00:00 Uhr |
| Date |AddYears(X,Y) |X: DateTime <br/><br/>Y: int |Fügt Y Jahre zu X hinzu.<br/><br/>Beispiel: 15.09.2013 12:00:00 Uhr + 1 Jahr = 15.09.2014 12:00:00 Uhr<br/><br/>Sie können auch Jahre subtrahieren, indem Sie Y als negative Zahl angeben.<br/><br/>Beispiel: 15.09.2013 12:00:00 Uhr - 1 Jahr = 15.09.2012 12:00:00 Uhr |
| Date |Day(X) |X: DateTime  |Ruft die Komponente "Tag" von X ab.<br/><br/>Beispiel: Tag von 15.09.2013 12:00:00 Uhr ist der 9. |
| Date |DayOfWeek(X) |X: DateTime  |Ruft den Tag der Komponente "Woche" von X ab.<br/><br/>Beispiel: DayOfWeek von 15.09.2013 12:00:00 Uhr ist Sonntag. |
| Date |DayOfYear(X) |X: DateTime  |Ruft den Tag des Jahres ab, der von der Komponente "Jahr" von X dargestellt wird.<br/><br/>Beispiele:<br/>1.12.2015: Tag 335 von 2015<br/>31.12.2015: Tag 365 von 2015<br/>31.12.2016: Tag 366 von 2016 (Schaltjahr) |
| Date |DaysInMonth(X) |X: DateTime  |Ruft die Tage des Monats ab, die von der Komponente "Monat" des Parameters X dargestellt werden.<br/><br/>Beispiel: DaysInMonth von 15.09.2013 sind 30, da der Monat September 30 Tage hat. |
| Date |EndOfDay(X) |X: DateTime  |Ruft die Datum/Uhrzeit-Angabe ab, die das Ende des Tages (Komponente "Tag") von X darstellt.<br/><br/>Beispiel: EndOfDay 15.09.2013 17:10:23 Uhr ist 15.09.2013 23:59:59 Uhr. |
| Date |EndOfMonth(X) |X: DateTime  |Ruft das Ende des Monats ab, das von der Komponente "Monat" des Parameters X dargestellt wird. <br/><br/>Beispiel: EndOfMonth 15.09.2013 17:10:23 Uhr ist 30.09.2013 23:59:59 Uhr (Datum/Uhrzeit-Angabe, die das Ende des Monats September darstellt) |
| Date |StartOfDay(X) |X: DateTime  |Ruft den Beginn des Tages ab, der von der Komponente "Tag" des Parameters X dargestellt wird.<br/><br/>Beispiel: StartOfDay 15.09.2013 17:10:23 Uhr ist 15.09.2013 12:00:00 Uhr. |
| DateTime |From(X) |X: String |Analysieren der Zeichenfolge X in einen Datum/Uhrzeit-Wert. |
| DateTime |Ticks(X) |X: DateTime  |Ruft die Zeiteinheitseigenschaft des Parameters X ab. Eine Zeiteinheit entspricht 100 Nanosekunden. Der Wert dieser Eigenschaft stellt die Anzahl der Zeiteinheiten dar, die seit Mitternacht am 1. Januar 0001 verstrichen sind. |
| Text |Format(X) |X: Stringvariable |Formatiert den Text. |

> [!IMPORTANT]
> Bei Verwenden einer Funktion in einer anderen Funktion müssen Sie für die innere Funktion nicht das Präfix **$$** verwenden. Beispiel: $$Text.Format('PartitionKey eq \\'my_pkey_filter_value\\' and RowKey ge \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(SliceStart, -6)). Wie Sie sehen, wird das Präfix **$$** bei diesem Beispiel nicht für die **Time.AddHours**-Funktion verwendet wird. 

#### <a name="example"></a>Beispiel
Im folgenden Beispiel werden die Eingabe- und Ausgabeparameter für die Hive-Aktivität mithilfe der Text.Format-Funktion und der SliceStart-Systemvariablen bestimmt. 

```json  
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
```

### <a name="example-2"></a>Beispiel 2

Im folgenden Beispiel wird der DateTime-Parameter für die Aktivität „Gespeicherte Prozedur“ mithilfe der Text.Format-Funktion und der SliceStart-Systemvariablen bestimmt. 

```json
{
    "name": "SprocActivitySamplePipeline",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_sample",
                    "storedProcedureParameters": {
                        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                    }
                },
                "outputs": [
                    {
                        "name": "sprocsampleout"
                    }
                ],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "SprocActivitySample"
            }
        ],
            "start": "2016-08-02T00:00:00Z",
            "end": "2016-08-02T05:00:00Z",
        "isPaused": false
    }
}
```

### <a name="example-3"></a>Beispiel 3
Um Daten vom vorherigen Tag statt von dem durch „SliceStart“ dargestellten Tag zu lesen, verwenden Sie die AddDays-Funktion, wie im folgenden Beispiel gezeigt: 

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-01-01T08:00:00",
        "end": "2017-01-01T11:00:00",
        "description": "hive activity",
        "activities": [
            {
                "name": "SampleHiveActivity",
                "inputs": [
                    {
                        "name": "MyAzureBlobInput",
                        "startTime": "Date.AddDays(SliceStart, -1)",
                        "endTime": "Date.AddDays(SliceEnd, -1)"
                    }
                ],
                "outputs": [
                    {
                        "name": "MyAzureBlobOutput"
                    }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adftutorial\\hivequery.hql",
                    "scriptLinkedService": "StorageLinkedService",
                    "defines": {
                        "Year": "$$Text.Format('{0:yyyy}',WindowsStart)",
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
```

Informationen zu verschiedenen verfügbaren Formatierungsoptionen (beispielsweise „yy“ oder „yyyy“) finden Sie im Thema [Benutzerdefinierte Formatzeichenfolgen für Datum und Uhrzeit](https://msdn.microsoft.com/library/8kb3ddd4.aspx). 


