---
title: Verschieben von Daten aus einem lokalen HDFS | Microsoft Docs
description: Erfahren Sie, wie Sie Daten aus einem lokalem HDFS mithilfe von Azure Data Factory verschieben.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 3215b82d-291a-46db-8478-eac1a3219614
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2016
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: 6ec8ac288a4daf6fddd6d135655e62fad7ae17c2
ms.openlocfilehash: 0f0eaaa927ea73cec845dbb369dc2c4a7a8466ba


---
# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a>Verschieben von Daten aus einem lokalem HDFS mithilfe von Azure Data Factory
Dieser Artikel beschreibt, wie Sie die Kopieraktivität in einer Azure Data Factory verwenden können, um Daten aus einem lokalen HDFS (Hadoop Distributed File System) in einen anderen Datenspeicher zu verschieben. Dieser Artikel baut auf dem Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) auf, der eine allgemeine Übersicht zur Datenverschiebung mit der Kopieraktivität und unterstützten Datenspeicherkombinationen enthält.

Data Factory unterstützt derzeit nur das Verschieben von Daten aus einem lokalen HDFS in andere Datenspeicher, aber nicht das Verschieben aus anderen Datenspeichern in ein lokales HDFS.

## <a name="enabling-connectivity"></a>Herstellen der Verbindung
Der Data Factory-Dienst unterstützt das Herstellen einer Verbindung mit einem lokalen HDFS über das Datenverwaltungsgateway. Im Artikel [Verschieben von Daten zwischen lokalen Standorten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) erfahren mehr zum Datenverwaltungsgateway und erhalten eine schrittweise Anleitung zum Einrichten des Gateways. Verwenden Sie das Gateway, um eine Verbindung mit dem HDFS herzustellen, auch wenn es auf einem virtuellen Azure-IaaS-Computer gehostet wird.

Grundsätzlich ist es zwar möglich, das Gateway auf dem gleichen lokalen Computer bzw. auf dem virtuellen Azure-Computer zu installieren, auf dem sich auch das HDFS befindet, es empfiehlt sich jedoch, das Gateway auf einem separaten Computer bzw. auf einem separaten virtuellen Azure-IaaS-Computer zu installieren. Diese Vorgehensweise dient zur Vermeidung von Ressourcenkonflikten und verbessert die Leistung. Wenn Sie das Gateway auf einem separaten Computer installieren, muss dieser Computer auf den Computer mit dem HDFS zugreifen können.

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Die einfachste Möglichkeit zum Erstellen einer Pipeline, die Daten aus einem lokalen HDFS kopiert, ist die Verwendung des Assistenten zum Kopieren von Daten. Unter [Tutorial: Erstellen einer Pipeline mit dem Assistenten zum Kopieren](data-factory-copy-data-wizard-tutorial.md) finden Sie eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten.

Die folgenden Beispiele zeigen JSON-Beispieldefinitionen, die Sie zum Erstellen einer Pipeline mit dem [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md), mit [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder mit [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) verwenden können. Darin wird veranschaulicht, wie Sie Daten aus einem lokalen HDFS in Azure Blob Storage kopieren. Daten können jedoch auch mithilfe der Kopieraktivität in Azure Data Factory in eine beliebige der [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) aufgeführten Senken kopiert werden.

## <a name="sample-copy-data-from-on-premises-hdfs-to-azure-blob"></a>Beispiel: Kopieren von Daten aus einem lokalen HDFS in ein Azure-Blob
In diesem Beispiel wird gezeigt, wie Sie Daten aus einem lokalen HDFS in Azure Blob Storage kopieren. Daten können jedoch mithilfe der Kopieraktivität in Azure Data Factory **direkt** in die [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) aufgeführten Senken kopiert werden.  

Das Beispiel enthält die folgenden Data Factory-Entitäten:

1. Einen verknüpften Dienst des Typs [OnPremisesHdfs](#hdfs-linked-service-properties)
2. Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service)
3. Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [FileShare](#hdfs-dataset-type-properties)
4. Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)
5. Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [FileSystemSource](#hdfs-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) verwendet

Das Beispiel kopiert stündlich Daten aus einem lokalen HDFS in ein Azure-Blob. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

Als Erstes richten Sie das Datenverwaltungsgateway ein. Anweisungen dazu finden Sie im Artikel [Verschieben von Daten zwischen lokalen Standorten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) .

**Mit HDFS verknüpfter Dienst**: In diesem Beispiel wird die Windows-Authentifizierung verwendet. Informationen zu den verwendbaren Authentifizierungstypen finden Sie im Abschnitt [Mit HDFS verknüpfter Dienst](#hdfs-linked-service-properties).

    {
        "name": "HDFSLinkedService",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }

**Mit Azure Storage verknüpfter Dienst**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**HDFS-Eingabedataset**: Dieses Dataset verweist auf den HDFS-Ordner „DataTransfer/UnitTest/“. Die Pipeline kopiert alle Dateien in diesem Ordner an das Ziel.

Durch Festlegen von „external“ auf „true“ wird dem Data Factory-Dienst mitgeteilt, dass das Dataset für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

    {
        "name": "InputDataset",
        "properties": {
            "type": "FileShare",
            "linkedServiceName": "HDFSLinkedService",
            "typeProperties": {
                "folderPath": "DataTransfer/UnitTest/"
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval":  1
            }
        }
    }




**Azure-Blob-Ausgabedataset**

Daten werden stündlich in ein neues Blob geschrieben ("frequency": "hour", "interval": 1). Der Ordnerpfad des Blobs wird basierend auf der Startzeit des Slices, der verarbeitet wird, dynamisch ausgewertet. Im Ordnerpfad werden Jahr, Monat, Tag und die Stundenteile der Startzeit verwendet.

    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/hdfs/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
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
                            "format": "MM"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "dd"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "HH"
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



**Pipeline mit Kopieraktivität**

Die Pipeline enthält eine Kopieraktivität, die für das Verwenden der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **FileSystemSource** und der Typ **sink** auf **BlobSink** festgelegt. Die für die **query** -Eigenschaft angegebene SQL-Abfrage wählt die aus der letzten Stunde zu kopierenden Daten aus.

    {
        "name": "pipeline",
        "properties":
        {
            "activities":
            [
                {
                    "name": "HdfsToBlobCopy",
                    "inputs": [ {"name": "InputDataset"} ],
                    "outputs": [ {"name": "OutputDataset"} ],
                    "type": "Copy",
                    "typeProperties":
                    {
                        "source":
                        {
                            "type": "FileSystemSource"
                        },
                        "sink":
                        {
                            "type": "BlobSink"
                        }
                    },
                    "policy":
                    {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1,
                        "timeout": "00:05:00"
                    }
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="hdfs-linked-service-properties"></a>Eigenschaften des mit HDFS verknüpften Diensts
Die folgende Tabelle enthält eine Beschreibung der JSON-Elemente, die für den mit HDFS verknüpften Dienst spezifisch sind.

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| Typ |Die type-Eigenschaft muss auf **Hdfs** |Ja |
| Url |Die URL für das HDFS. |Ja |
| encryptedCredential |[New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) für den Zugriff. |Nein |
| userName |Der Benutzername für die Windows-Authentifizierung. |Ja (für die Windows-Authentifizierung) |
| password |Das Kennwort für die Windows-Authentifizierung. |Ja (für die Windows-Authentifizierung) |
| authenticationType |Windows oder Anonymous. |Ja |
| gatewayName |Der Name des Gateways, das der Data Factory-Dienst zum Verbinden mit dem HDFS verwenden soll. |Ja |

Ausführliche Informationen zum Festlegen von Anmeldeinformationen für ein lokales HDFS finden Sie unter [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit dem Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md).

### <a name="using-anonymous-authentication"></a>Verwenden der anonymen Authentifizierung
    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Anonymous",
                "userName": "hadoop",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }


### <a name="using-windows-authentication"></a>Verwenden der Windows-Authentifizierung
    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }



## <a name="hdfs-dataset-type-properties"></a>Eigenschaften des Dataset-Typs „HDFS“
Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie „structure“, „availability“ und „policy“ des JSON-Codes eines Datasets sind bei allen Dataset-Typen (Azure SQL, Azure-Blob, Azure-Tabelle usw.) ähnlich.

Der Abschnitt **typeProperties** unterscheidet sich bei jedem Typ von Dataset und bietet Informationen zum Speicherort der Daten im Datenspeicher. Der Abschnitt „typeProperties“ für ein Dataset vom Typ **FileShare** (mit HDFS-Dataset) hat die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| folderPath |Pfad zum Ordner. Beispiel: `myfolder`<br/><br/>Verwenden Sie für Sonderzeichen in der Zeichenfolge das Escapezeichen „\“. Geben Sie beispielsweise für „Ordner\Unterordner“ die Zeichenfolge „Ordner\\\\Unterordner“ und für „d:\Beispielordner“ die Zeichenfolge „d:\\\\Beispielordner“ an.<br/><br/>Sie können diese Eigenschaft mit **partitionBy** kombinieren, um Ordnerpfade auf der Grundlage von Datum und Uhrzeit für Start und Ende des Slices zu erhalten. |Ja |
| fileName |Geben Sie den Namen der Datei in **folderPath** an, wenn die Tabelle auf eine bestimmte Datei im Ordner verweisen soll. Wenn Sie keine Werte für diese Eigenschaft angeben, verweist die Tabelle auf alle Dateien im Ordner.<br/><br/>Wenn „fileName“ für ein Ausgabedataset nicht angegeben ist, hat der Name der generierten Datei folgendes Format: <br/><br/>Data.<Guid>.txt (Beispiel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Nein |
| partitionedBy |Mit „partitionedBy“ kann für Zeitreihendaten ein dynamischer Wert für „folderPath“ und „filename“ angegeben werden. Beispiel: Parametrisierung von „folderPath“ für Daten nach Stunde. |Nein |
| fileFilter |Geben Sie einen Filter zur Auswahl einer Teilmenge der Dateien in "folderPath" statt alle Dateien an. <br/><br/>Zulässige Werte: `*` (mehrere Zeichen) und `?` (einzelnes Zeichen).<br/><br/>Beispiel 1: `"fileFilter": "*.log"`<br/>Beispiel 2: `"fileFilter": 2014-1-?.txt"`<br/><br/>**Hinweis**: „fileFilter“ eignet sich für das Eingabedataset „FileShare“. |Nein |
| format |Folgende Formattypen werden unterstützt: **TextFormat**, **AvroFormat**, **JsonFormat**, **OrcFormat** und **ParquetFormat**. Sie müssen die **type** -Eigenschaft unter „format“ auf einen dieser Werte festlegen. Weitere Informationen finden Sie in den Abschnitten [Angeben von „TextFormat“](#specifying-textformat), [Angeben von „AvroFormat“](#specifying-avroformat), [Angeben von „JsonFormat“](#specifying-jsonformat), [Angeben von „OrcFormat“](#specifying-orcformat) und [Angeben von „ParquetFormat“](#specifying-parquetformat). Wenn Sie Dateien unverändert zwischen dateibasierten Speichern kopieren möchten (binäre Kopie), können Sie den Formatabschnitt bei den Definitionen von Eingabe- und Ausgabedatasets überspringen. |Nein |
| Komprimierung |Geben Sie den Typ und den Grad der Komprimierung für die Daten an. Folgende Typen werden unterstützt: **GZip**, **Deflate** und **BZip2**. Folgende Komprimierungsstufen werden unterstützt: **Optimal** und **Schnellste**. Für Daten mit **AvroFormat** oder **OrcFormat** werden derzeit keine Komprimierungseinstellungen unterstützt. Weitere Informationen finden Sie im Abschnitt [Komprimierungsunterstützung](#compression-support). |Nein |

> [!NOTE]
> "filename" und "fileFilter" können nicht gleichzeitig verwendet werden.
>
>

### <a name="using-partionedby-property"></a>Verwenden der partionedBy-Eigenschaft
Wie im vorherigen Abschnitt erwähnt, kann „partitionedBy“ verwendet werden, um für Zeitreihendaten einen dynamischen Wert für „folderPath“ und „filename“ anzugeben. Sie können dazu die Data Factory-Makros und Systemvariablen "SliceStart" und "SliceEnd" verwenden, die den logischen Zeitraum für einen bestimmten Datenslice angeben.

In den Artikeln [Erstellen von Datasets](data-factory-create-datasets.md), [Planung und Ausführung](data-factory-scheduling-and-execution.md) und [Erstellen von Pipelines](data-factory-create-pipelines.md) finden Sie weitere Details zu Zeitreihen-Datasets, Planung und Slices.

#### <a name="sample-1"></a>Beispiel 1:
    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy":
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

Im obigen Beispiel wird „{Slice}“ durch den Wert der Data Factory-Systemvariablen „SliceStart“ ersetzt, die im Format „JJJJMMTTHH“ angegeben ist. "SliceStart" bezieht sich auf die Startzeit des Slices. "folderPath" unterscheidet sich für jeden Slice. Beispiel: "wikidatagateway/wikisampledataout/2014100103" oder "wikidatagateway/wikisampledataout/2014100104".

#### <a name="sample-2"></a>Beispiel 2:
    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy":
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
    ],

Im Beispiel oben werden Jahr, Monat, Tag und Uhrzeit von SliceStart in separate Variablen extrahiert, die von den Eigenschaften „folderPath“ und „fileName“ verwendet werden.

[!INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]

[!INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="hdfs-copy-activity-type-properties"></a>Eigenschaften von HDFS-Kopieraktivitätstyp
Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md). Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabetabellen und Richtlinien sind für alle Arten von Aktivitäten verfügbar.

Eigenschaften im Abschnitt „typeProperties“ der Aktivität können dagegen je nach Aktivitätstyp variieren. Für die Kopieraktivität variieren die Eigenschaften je nach Art der Quellen und Senken.

Wenn die Quelle der Kopieraktivität den Typ **FileSystemSource** hat, sind im Abschnitt „typeProperties“ die folgenden Eigenschaften verfügbar:

**FileSystemSource** unterstützt die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| --- | --- | --- | --- |
| recursive |Gibt an, ob die Daten rekursiv aus den Unterordnern oder nur aus dem angegebenen Ordner gelesen werden. |True/False (Standardwert) |Nein |

[!INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Leistung und Optimierung
Der Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) beschreibt wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.



<!--HONumber=Nov16_HO3-->


