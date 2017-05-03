---
title: Erstellen von Datasets in Azure Data Factory | Microsoft Docs
description: "Anhand von Beispielen, in denen Eigenschaften wie „offset“ und „anchorDateTime“ verwendet werden, wird beschrieben, wie Sie Datasets in Azure Data Factory erstellen."
keywords: "Dataset erstellen, Datasetbeispiel, „offset“-Beispiel"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 0614cd24-2ff0-49d3-9301-06052fd4f92a
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: shlo
translationtype: Human Translation
ms.sourcegitcommit: e0c999b2bf1dd38d8a0c99c6cdd4976cc896dd99
ms.openlocfilehash: 88e653f6e46f3e8eb72e620b495d1769f17bdfbf
ms.lasthandoff: 04/20/2017


---
# <a name="datasets-in-azure-data-factory"></a>Datasets in Azure Data Factory
In diesem Artikel werden die Datasets in Azure Data Factory beschrieben. Außerdem enthält der Artikel Beispiele, z.B. zu Datenbanken vom Typ „offset“, „anchorDateTime“ und „offset/style“.

> [!NOTE]
> Wenn Sie noch nicht viel Erfahrung mit Azure Data Factory haben, nutzen Sie [Einführung in Azure Data Factory](data-factory-introduction.md) als Übersicht zum Azure Data Factory-Dienst. Wenn Sie noch nicht über praktische Erfahrung mit dem Erstellen von Data Factorys verfügen, erleichtert Ihnen das Tutorial [Erstellen Ihrer ersten Pipeline zur Transformierung von Daten mithilfe eines Hadoop-Clusters](data-factory-build-your-first-pipeline.md) oder [Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) das Verständnis des vorliegenden Artikels. 

## <a name="overview"></a>Übersicht
Eine Data Factory kann eine oder mehrere Aktivitäten aufweisen. Bei einer **Pipeline** handelt es sich um eine logische Gruppierung von **Aktivitäten**, die zusammen eine Aufgabe bilden. Die Aktivitäten in einer Pipeline definieren Aktionen, die Sie auf Ihre Daten anwenden. Sie können z.B. mit einer Kopieraktivität Daten aus einer lokalen SQL Server-Instanz in eine Instanz von Azure Blob Storage kopieren. Verwenden Sie dann eine Hive-Aktivität, die ein Hive-Skript auf einen Azure HDInsight-Cluster anwendet, um Daten aus dem Blob Storage zu verarbeiten/transformieren, um Ausgabedaten zu produzieren. Kopieren Sie die Ausgabedaten schließlich mit einer zweiten Kopieraktivität in ein Azure SQL Data Warehouse, auf Basis dessen Business Intelligence-Berichtslösungen (BI) erstellt werden. Weitere Informationen zu Pipelines und Aktivitäten finden Sie im Artikel [Pipelines und Aktivitäten in Azure Data Factory](data-factory-create-pipelines.md).

Eine Aktivität kann über null oder mehr **Eingabedatasets** verfügen und ein oder mehrere Ausgabedatasets erstellen. Ein Eingabedataset stellt die Eingabe für eine Aktivität in der Pipeline dar, und ein Ausgabedataset stellt die Ausgabe für die Aktivität dar. Datasets bestimmen Daten in verschiedenen Datenspeichern, z.B. Tabellen, Dateien, Ordnern und Dokumenten. Ein Azure-Blob-Dataset kann beispielsweise den Blobcontainer und -ordner in Azure Blob Storage angeben, aus dem die Pipeline die Daten lesen soll. 

Bevor Sie ein Dataset erstellen, müssen Sie einen **verknüpften Dienst** erstellen, um Ihren Datenspeicher mit der Data Factory zu verknüpfen. Verknüpfte Dienste ähneln Verbindungszeichenfolgen, mit denen die Verbindungsinformationen definiert werden, die für Data Factory zum Herstellen einer Verbindung mit externen Ressourcen erforderlich sind. Datasets bestimmen Daten in den verknüpften Datenspeichern, z.B. SQL-Tabellen, Dateien, Ordnern und Dokumenten. Ein mit Azure Storage verknüpfter Dienst verbindet z.B. ein Azure-Speicherkonto mit der Data Factory. Ein Azure-Blob-Dataset stellt den Blobcontainer und den Ordner dar, der die zu verarbeitenden Eingabeblobs enthält. 

Hier ist ein Beispielszenario: Um Daten aus einer Azure Blob Storage-Instanz in eine Azure SQL-Datenbank zu kopieren, erstellen Sie zwei verknüpfte Dienste: Azure Storage und Azure SQL-Datenbank. Erstellen Sie dann zwei Datasets: Azure-Blob-Dataset (bezieht sich auf den mit Azure Storage verknüpften Dienst) und Azure SQL-Tabellendataset (bezieht sich auf den mit Azure SQL-Datenbank verknüpften Dienst). Die mit Azure Storage und Azure SQL-Datenbank verknüpften Dienste enthalten Verbindungszeichenfolgen, die Data Factory zur Laufzeit verwendet, um die Verbindung mit Ihrem Azure-Speicher bzw. mit Ihrer Instanz von Azure SQL-Datenbank herzustellen. Das Azure-Blob-Dataset gibt den Blobcontainer und Blobordner an, der die Eingabeblobs in Ihrer Azure Blob Storage-Instanz enthält. Das Azure SQL-Tabellendataset gibt die SQL-Tabelle in Ihrer Azure SQL-Datenbank an, in die die Daten kopiert werden sollen.

Das folgende Diagramm zeigt die Beziehung zwischen Pipeline, Aktivität, Dataset und verknüpftem Dienst in der Data Factory an: 

![Beziehung zwischen Pipeline, Aktivität, Dataset und verknüpften Diensten](media/data-factory-create-datasets/relationship-between-data-factory-entities.png)

## <a name="dataset-json"></a>JSON-DataSet
Ein Dataset in Azure Data Factory wird wie folgt im JSON-Format definiert:

```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "external": <boolean flag to indicate external data. only for input datasets>,
        "linkedServiceName": "<Name of the linked service that refers to a data store.>",
        "structure": [
            {
                "name": "<Name of the column>",
                "type": "<Name of the type>"
            }
        ],
        "typeProperties": {
            "<type specific property>": "<value>",
            "<type specific property 2>": "<value 2>",
        },
        "availability": {
            "frequency": "<Specifies the time unit for data slice production. Supported frequency: Minute, Hour, Day, Week, Month>",
            "interval": "<Specifies the interval within the defined frequency. For example, frequency set to 'Hour' and interval set to 1 indicates that new data slices should be produced hourly>"
        },
       "policy":
        {      
        }
    }
}
```

In der folgenden Tabelle werden die Eigenschaften im obigen JSON-Code beschrieben:   

| Eigenschaft | Beschreibung | Erforderlich | Standard |
| --- | --- | --- | --- |
| Name |Name des Datasets. Unter [Azure Data Factory – Benennungsregeln](data-factory-naming-rules.md) finden Sie die Benennungsregeln. |Ja |NA |
| type |Typ des Datasets. Geben Sie einen der von Azure Data Factory unterstützten Typen ein (z. B.: „azureblob“, „azuresqltable“). <br/><br/>Weitere Informationen finden Sie unter [Dataset: type](#Type). |Ja |NA |
| structure |Schema des Datasets<br/><br/>Unter [Dataset: Structure](#Structure) finden Sie weitere Details. |Nein. |NA |
| typeProperties | Die Typeigenschaften der einzelnen Typen (z.B. Azure-Blob, Azure SQL-Tabelle) sind unterschiedlich. Ausführliche Informationen über die unterstützten Typen und deren Eigenschaften finden Sie unter [Dataset: type](#Type). |Ja |NA |
| external | Boolesches Flag, das angibt, ob ein Dataset explizit durch eine Data Factory-Pipeline erstellt wird oder nicht. Wenn das Eingabedataset für eine Aktivität nicht durch die aktuelle Pipeline erstellt wird, legen Sie für dieses Flag „true“ fest. Legen Sie für das Eingabedataset der ersten Aktivität in der Pipeline für dieses Flag „true“ fest.  |Nein |false |
| availability | Definiert das Verarbeitungsfenster (stündlich, täglich etc.) oder das Modell für das Aufteilen in Slices für die Datasetproduktion. Jede Einheit von Daten, die durch eine Aktivitätsausführung genutzt und erstellt wird, heißt Datenslice. Wenn die Verfügbarkeit des Ausgabedatasets auf täglich (frequency - Day, interval - 1) festgelegt ist, wird täglich ein Slice erstellt. <br/><br/>Details finden Sie unter [Dataset: availability](#Availability). <br/><br/>Weitere Informationen zum Modell für das Aufteilen von Datasets in Slices finden Sie im Artikel [Planung und Ausführung](data-factory-scheduling-and-execution.md) . |Ja |NA |
| policy |Definiert die Kriterien oder die Bedingung, die die Dataset-Slices erfüllen müssen. <br/><br/>Weitere Informationen finden Sie im Abschnitt [Dataset-Richtlinie](#Policy) . |Nein |NA |

## <a name="dataset-example"></a>Datasetbeispiel
Im folgenden Beispiel stellt das Dataset eine Tabelle mit dem Namen **MyTable** in einer **Azure SQL-Datenbank** dar.

```json
{
    "name": "DatasetSample",
    "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties":
        {
            "tableName": "MyTable"
        },
        "availability":
        {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

Beachten Sie folgende Punkte:

* „type“ ist auf „AzuresqlTable“ festgelegt.
* Als tableName-Typeigenschaft (spezifisch für AzureSplTable-Typ) ist „MyTable“ festgelegt.
* linkedServiceName verweist auf einen verknüpften Dienst vom Typ AzureSqlDatabase, der im nächsten JSON-Codeausschnitt definiert ist. 
* Als „availability frequency“ ist „day“ festgelegt, und „interval“ ist auf „1“ festgelegt. Das bedeutet, dass der Datasetslice täglich erzeugt wird.  

AzureSqlLinkedService wird wie folgt definiert:

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
        }
    }
}
```

Im obigen JSON-Code:

* „type“ ist auf „AzureStorage“ festgelegt.
* Die „connectionString“-Typeigenschaft gibt Informationen zur Verbindung mit einer Azure SQL-Datenbank.  

Wie Sie sehen, definiert der verknüpfte Dienst das Herstellen einer Verbindung mit einer Azure SQL-Datenbank. Das Dataset definiert, welche Tabelle als Eingabe/Ausgabe für die Aktivität in einer Pipeline verwendet wird.   

> [!IMPORTANT]
> Falls ein Dataset nicht von der Pipeline erzeugt wird, sollte es als **external** markiert werden. Diese Einstellung gilt im Allgemeinen für Eingaben der ersten Aktivität in einer Pipeline.   


## <a name="Type"></a> Dataset: type
Der Typ des Datasets hängt von dem Datenspeicher ab, den Sie verwenden. In der folgenden Tabelle finden Sie eine Liste der von Data Factory unterstützten Datenspeicher. Klicken Sie auf einen Datenspeicher, um Informationen zum Erstellen eines verknüpften Diensts und eines Datasets für diesen Datenspeicher zu erhalten.

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> Datenspeicher mit * können lokal oder in Azure IaaS verfügbar sein. Für ihre Verwendung müssen Sie das [Datenverwaltungsgateway](data-factory-data-management-gateway.md) auf einem lokalen oder einem Azure IaaS-Computer installieren.

Im vorherigen Abschnitt ist im Beispiel der Typ des Datasets auf **AzureSqlTable** festgelegt. Ebenso ist für ein Azure-Blob-Dataset der Typ des Datasets auf **AzureBlob** festgelegt, wie im folgenden JSON-Code dargestellt:

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "input.log",
            "folderPath": "adfgetstarted/inputdata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```

## <a name="Structure"></a>Dataset: structure
Bei dem Abschnitt **structure** handelt es sich um einen **optionalen** Abschnitt, der das Schema des Datasets definiert. Er enthält eine Auflistung der Namen und Datentypen der Spalten. Im Abschnitt „structure“ legen Sie Typinformationen fest, die zum Konvertieren von Typen und Zuordnen von Spalten von der Quelle zum Ziel bereitgestellt werden. Im folgenden Beispiel hat das Dataset die drei Spalten `slicetimestamp`, `projectname` und `pageviews`. Sie weisen die Typen „String“, „String“ und „Decimal“ auf.

```json
structure:  
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

Jede Spalte im Abschnitt „structure“ enthält die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| name |Name der Spalte. |Ja |
| type |Datentyp der Spalte.  |Nein |
| culture |Zu verwendendes .NET-basiertes Gebietsschema, wenn der Typ ein .NET-Typ ist: `Datetime` oder `Datetimeoffset`. Der Standardwert ist `en-us`. |Nein |
| format |Zu verwendende Formatzeichenfolge, wenn der Typ ein .NET-Typ ist: `Datetime` oder `Datetimeoffset`. |Nein |

Befolgen Sie die folgenden Angaben dazu, wann der Abschnitt **structure** mit welchen Informationen verwendet werden sollte.

* Geben Sie **bei strukturierten Datenquellen**, die Datenschema- und Typinformationen neben den Daten selbst speichern (Quellen wie SQL Server, Oracle, Azure-Tabelle), den Abschnitt „structure“ nur dann an, wenn Sie bestimmte Quellspalten Senkenspalten zuordnen möchten, und deren Namen nicht identisch sind. 
  
    Da Typinformationen für strukturierte Datenquellen bereits verfügbar sind, sollten Sie keine Typinformationen einschließen, wenn Sie sich für die Verwendung des Abschnitts „structure“ entscheiden.
* **Für das Schema von Lesedatenquellen (insbesondere Azure-Blob)** können Sie Daten speichern, ohne Schema- oder Typinformationen mit den Daten zu speichern. Schließen Sie für diese Arten von Datenquellen „structure“ ein, wenn Sie Quellspalten Senkenspalten zuordnen möchten (oder/,) wenn das Dataset ein Eingabedataset für eine Kopieraktivität darstellt und Datentypen des Quelldatasets in native Typen der Senke konvertiert werden müssen. 
    
    Data Factory unterstützt die folgenden CLS-kompatiblen auf .NET basierenden Typwerte für die Bereitstellung von Typinformationen in „structure“ für das Schema von Lesedatenquellen wie Azure-Blob: Int16, Int32, Int64, Single, Double, Decimal, Byte[], Bool, String, Guid, Datetime, Datetimeoffset, Timespan.

Data Factory führt beim Verschieben von Daten aus einem Quelldatenspeicher in einen Senkendatenspeicher automatisch Typkonvertierungen durch. 
  

## <a name="Availability"></a> Dataset: availability
Der Abschnitt **availability** in einem Dataset definiert das Verarbeitungsfenster (stündlich, täglich, wöchentlich usw.) für das Dataset. Ausführliche Informationen zum Aktivitätsfenster finden Sie im Artikel [Data Factory – Planung und Ausführung](data-factory-scheduling-and-execution.md).

Der folgende Abschnitt „availability“ definiert, dass das Ausgabedataset entweder stündlich erstellt wird oder das Eingabedataset stündlich verfügbar ist:

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 1    
}
```

Wenn die Pipeline die folgenden Start- und Endzeiten hat:  

```json
    "start": "2016-08-25T00:00:00Z",
    "end": "2016-08-25T05:00:00Z",
```

Das Ausgabedataset wird zwischen Start- und Endzeit der Pipeline stündlich erzeugt. Aus diesem Grund werden fünf Datasetslices von dieser Pipeline erzeugt, einer für jedes Aktivitätsfenster (12:00 Uhr - 1:00 Uhr, 1:00 Uhr - 2:00 Uhr, 2:00 Uhr - 3:00 Uhr, 3:00 Uhr - 4:00 Uhr, 4:00 Uhr - 5: 00 Uhr). 

In der folgenden Tabelle werden die Eigenschaften beschrieben, die Sie im Abschnitt „availability“ verwenden können:

| Eigenschaft | Beschreibung | Erforderlich | Standard |
| --- | --- | --- | --- |
| frequency |Gibt die Zeiteinheit für die Erstellung der Datasetslices an.<br/><br/><b>Unterstützte Häufigkeit</b>: „Minute“, „Hour“, „Day“, „Week“, „Month“ |Ja |NA |
| interval |Gibt einen Multiplikator für die Häufigkeit an<br/><br/>„Frequency x interval“ bestimmt, wie oft der Slice erzeugt wird.<br/><br/>Wenn Sie das Dataset auf Stundenbasis in Slices aufteilen möchten, legen Sie <b>Frequency</b> auf <b>Hour</b> und <b>interval</b> auf <b>1</b> fest.<br/><br/><b>Hinweis:</b> Wenn Sie „Frequency“ auf „Minute“ festlegen, sollten Sie „interval“ mindestens auf „15“ festlegen. |Ja |NA |
| style |Gibt an, ob der Slice am Anfang/Ende des Intervalls erzeugt werden soll.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>Wenn „Frequency“ auf „Month“ und „style“ auf „EndOfInterval“ festgelegt ist, wird der Slice am letzten Tag des Monats erstellt. Wenn „style“ auf „StartOfInterval“ festgelegt ist, wird der Slice am ersten Tag des Monats erstellt.<br/><br/>Wenn „Frequency“ auf „Day“ und „style“ auf „EndOfInterval“ festgelegt ist, wird der Slice in der letzten Stunde des Tages erstellt.<br/><br/>Wenn „Frequency“ auf „Hour“ und „style“ auf „EndOfInterval“ festgelegt ist, wird der Slice am Ende der Stunde erstellt. Ein Slice für den Zeitraum 13:00 bis 14:00 Uhr wird z. B. um 14.00 Uhr erstellt. |Nein |EndOfInterval |
| anchorDateTime |Definiert die absolute Position in der Zeit, die der Scheduler benötigt, um Dataset-Slicegrenzen zu berechnen. <br/><br/><b>Hinweis</b>: Wenn AnchorDateTime Datumsteile aufweist, die präziser als die Häufigkeit sind, werden die präziseren Teile ignoriert. <br/><br/>Wenn <b>interval</b> z.B. auf <b>hourly</b> festgelegt ist („frequency: hour“ und „interval: 1“) und <b>AnchorDateTime</b> Angaben für <b>Minuten und Sekunden</b> enthält, werden die <b>Minuten- und Sekundenteile</b> von AnchorDateTime ignoriert. |Nein |01/01/0001 |
| offset |Zeitspanne, um die Anfang und Ende aller Datasetslices verschoben werden. <br/><br/><b>Hinweis:</b> Wenn sowohl „anchorDateTime“ als auch „offset“ angegeben werden, ist das Ergebnis die kombinierte Verschiebung. |Nein |NA |

### <a name="offset-example"></a>Beispiel zu Offset
Standardmäßig beginnen Slices täglich (`"frequency": "Day", "interval": 1`) um 00:00 Uhr UTC-Zeit (Mitternacht). Wenn die Startzeit 6:00 Uhr UTC-Zeit sein soll, legen Sie den Versatz wie im folgenden Codeausschnitt gezeigt fest: 

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a>Beispiel zu „anchorDateTime“
Im folgenden Beispiel wird das Dataset einmal alle 23 Stunden erstellt. Der erste Slice beginnt zu dem durch anchorDateTime vorgegebenen Zeitpunkt, d.h. um `2017-04-19T08:00:00` (UTC-Zeit).

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 23,    
    "anchorDateTime":"2017-04-19T08:00:00"    
}
```

### <a name="offsetstyle-example"></a>Beispiel zu „offset“/„style“
Das folgende Dataset ist ein monatliches Dataset und wird am Dritten jedes Monats um 8:00 Uhr erstellt (`3.08:00:00`):

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00",    
    "style": "StartOfInterval"
}
```

## <a name="Policy"></a>Dataset: policy
Der Abschnitt **policy** in der Datasetdefinition definiert die Kriterien oder die Bedingung, die die Datasetslices erfüllen müssen.

### <a name="validation-policies"></a>Überprüfungsrichtlinien
| Richtlinienname | Beschreibung | Angewendet auf | Erforderlich | Standard |
| --- | --- | --- | --- | --- |
| minimumSizeMB |Überprüft, ob die Daten in einem **Azure-Blob** die minimalen Größenanforderungen (in MB) erfüllen. |Azure-Blob |Nein |NA |
| minimumRows |Überprüft, ob die Daten in einer **Azure SQL-Datenbank** oder einer **Azure-Tabelle** die minimale Anzahl von Zeilen enthalten. |<ul><li>Azure SQL-Datenbank</li><li>Azure-Tabelle</li></ul> |Nein |NA |

#### <a name="examples"></a>Beispiele
**minimumSizeMB:**

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

**minimumRows**

```json
"policy":
{
    "validation":
    {
        "minimumRows": 100
    }
}
```

### <a name="external-datasets"></a>Externe Datasets
Externe Datasets werden nicht durch eine Pipeline erstellt, die in der Data Factory ausgeführt wird. Wenn das Dataset als **extern** gekennzeichnet ist, kann die **ExternalData**-Richtlinie definiert werden, um das Verhalten der Dataset-Sliceverfügbarkeit zu beeinflussen.

Falls von Azure Data Factory kein Dataset erzeugt wird, sollte es als **extern**markiert werden. Diese Einstellung gilt im Allgemeinen für die Eingaben der ersten Aktivität in einer Pipeline, wenn nicht die Aktivität oder Pipeline-Verkettung genutzt wird.

| Name | Beschreibung | Erforderlich | Standardwert |
| --- | --- | --- | --- |
| dataDelay |Zeit, um die die Prüfung der Verfügbarkeit der externen Daten für den angegebenen Slice verzögert wird. Wenn die Daten beispielsweise stündlich verfügbar sind, kann die Überprüfung, ob die externen Daten verfügbar sind und der entsprechende Slice bereit ist, mithilfe von dataDelay verzögert werden.<br/><br/>Dies gilt nur für die aktuelle Zeit.  Beispiel: Wenn es gerade 13:00 Uhr ist und dieser Wert 10 Minuten beträgt, beginnt die Überprüfung um 13:10 Uhr.<br/><br/>Diese Einstellung wirkt sich nicht auf Slices in der Vergangenheit aus (Slices, für die gilt: Sliceendzeit + dataDelay < jetzt), sie werden ohne Verzögerung verarbeitet.<br/><br/>Zeiträume, die größer als 23 Stunden 59 Minuten sind, müssen im Format `day.hours:minutes:seconds` angegeben werden. Um beispielsweise 24 Stunden anzugeben, verwenden Sie nicht 24:00:00, sondern stattdessen 1.00:00:00. Wenn Sie 24:00:00 verwenden, wird dies als 24 Tage (24.00:00:00) gewertet. Für 1 Tag und 4 Stunden geben Sie „1:04:00:00“ an. |Nein |0 |
| retryInterval |Die Wartezeit zwischen einem Fehler und dem nächsten Wiederholungsversuch. Gilt für die aktuelle Zeit. Wenn beim vorherigen Versuch ein Fehler auftrat, wird der nächste Versuch nach Verstreichen des retryInterval-Zeitraums durchgeführt. <br/><br/>Wenn es gerade 13:00 Uhr ist, beginnt der erste Versuch. Wenn die Ausführung der ersten Überprüfung 1 Minute gedauert hat und ein Fehler aufgetreten ist, findet die nächste Wiederholung um 13:00 + 1 Min. (Dauer) + 1 Min. (Wiederholungsintervall) = 13:02 Uhr statt. <br/><br/>Für Slices in der Vergangenheit gibt es keine Verzögerung. Der erneute Versuch erfolgt sofort. |Nein |00:01:00 (1 Minute) |
| retryTimeout |Das Timeout für die einzelnen Wiederholungsversuche.<br/><br/>Wenn diese Eigenschaft auf 10 Minuten festgelegt ist, muss die Überprüfung innerhalb von 10 Minuten abgeschlossen werden. Wenn die Ausführung der Überprüfung länger als 10 Minuten dauert, wird das Timeout für die Wiederholung wirksam.<br/><br/>Wenn für alle Überprüfungsversuche ein Timeout wirksam wird, wird der Slice als TimedOut gekennzeichnet. |Nein |00:10:00 (10 Minuten) |
| maximumRetry |Gibt an, wie oft die Verfügbarkeit der externen Daten überprüft werden soll. Der zulässige Höchstwert ist 10. |Nein |3 |


## <a name="create-datasets"></a>Erstellen von Datasets
Sie können Datasets mit einem dieser Tools oder SDKs erstellen. 

- Kopier-Assistent. 
- Azure-Portal
- Visual Studio
- Azure PowerShell
- Azure Resource Manager-Vorlage
- REST-API
- .NET API

In den folgenden Tutorials finden Sie schrittweise Anleitungen zum Erstellen von Pipelines und Datasets mit einem dieser Tools oder SDKs.
 
- [Tutorial: Erstellen Ihrer ersten Pipeline zur Transformierung von Daten mithilfe eines Hadoop-Clusters](data-factory-build-your-first-pipeline.md)
- [Tutorial: Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)

Sobald eine Pipeline erstellt/bereitgestellt ist, können Sie sie mit den Blättern des Azure-Portals oder der App „Überwachung und Verwaltung“ überwachen und verwalten. In den nächsten Themen finden Sie schrittweise Anweisungen. 

- [Überwachen und Verwalten von Azure Data Factory-Pipelines mit dem Azure-Portal und PowerShell](data-factory-monitor-manage-pipelines.md).
- [Überwachen und Verwalten von Azure Data Factory-Pipelines mit der App „Überwachung und Verwaltung“](data-factory-monitor-manage-app.md)


## <a name="scoped-datasets"></a>Zugeordnete Datasets
Mit der Eigenschaft **datasets** können Sie Datasets erstellen, die einer Pipeline zugeordnet sind. Diese Datasets können nur von Aktivitäten innerhalb dieser Pipeline, aber nicht von Aktivitäten in anderen Pipelines verwendet werden. Das folgende Beispiel definiert eine Pipeline mit zwei Datasets (InputDataset-rdc und OutputDataset-rdc), die in der Pipeline verwendet werden sollen:  

> [!IMPORTANT]
> Zugeordnete Datasets werden nur mit einmalig ausgeführten Pipelines unterstützt (pipelineMode ist auf OneTime festgelegt). Weitere Informationen finden Sie unter [Pipeline mit einmaliger Ausführung](data-factory-scheduling-and-execution.md#onetime-pipeline) .
>
>

```json
{
    "name": "CopyPipeline-rdc",
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
                        "name": "InputDataset-rdc"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataset-rdc"
                    }
                ],
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1,
                    "style": "StartOfInterval"
                },
                "name": "CopyActivity-0"
            }
        ],
        "start": "2016-02-28T00:00:00Z",
        "end": "2016-02-28T00:00:00Z",
        "isPaused": false,
        "pipelineMode": "OneTime",
        "expirationTime": "15.00:00:00",
        "datasets": [
            {
                "name": "InputDataset-rdc",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "InputLinkedService-rdc",
                    "typeProperties": {
                        "fileName": "emp.txt",
                        "folderPath": "adftutorial/input",
                        "format": {
                            "type": "TextFormat",
                            "rowDelimiter": "\n",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "external": true,
                    "policy": {}
                }
            },
            {
                "name": "OutputDataset-rdc",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "OutputLinkedService-rdc",
                    "typeProperties": {
                        "fileName": "emp.txt",
                        "folderPath": "adftutorial/output",
                        "format": {
                            "type": "TextFormat",
                            "rowDelimiter": "\n",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "external": false,
                    "policy": {}
                }
            }
        ]
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu Pipelines finden Sie im Artikel [Pipelines und Aktivitäten in Azure Data Factory](data-factory-create-pipelines.md). 
- Weitere Informationen über die Planung und Ausführung von Pipelines finden Sie im Artikel [Data Factory – Planung und Ausführung](data-factory-scheduling-and-execution.md). 
