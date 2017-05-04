---
title: Verschieben von Daten aus Amazon Redshift mit Data Factory | Microsoft Docs
description: Erfahren Sie, wie Sie Daten aus Amazon Redshift mithilfe von Azure Data Factory verschieben.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 01d15078-58dc-455c-9d9d-98fbdf4ea51e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: b4802009a8512cb4dcb49602545c7a31969e0a25
ms.openlocfilehash: 524b87d95d3060c780b296350797501847c80638
ms.lasthandoff: 03/29/2017


---
# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a>Verschieben von Daten mithilfe von Azure Data Factory
Dieser Artikel beschreibt, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus Amazon Redshift zu verschieben. Dieser Artikel baut auf dem Artikel zu [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) auf, der eine allgemeine Übersicht zur Datenverschiebung mit der Kopieraktivität bietet. 

Sie können Daten von Amazon Redshift in beliebige unterstützte Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Data Factory unterstützt derzeit nur das Verschieben von Daten aus Amazon Redshift in andere Datenspeicher und nicht das Verschieben aus anderen Datenspeichern in Amazon Redshift.

## <a name="prerequisites"></a>Voraussetzungen
* Wenn Sie Daten in einen einem lokalen Datenspeicher verschieben möchten, installieren Sie das [Datenverwaltungsgateway](data-factory-data-management-gateway.md) auf dem lokalen Computer. Gewähren Sie anschließend dem Datenverwaltungsgateway (verwenden Sie die IP-Adresse des Computers) den Zugriff auf Amazon Redshift-Cluster. Anweisungen finden Sie unter [Authorize access to the cluster](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) (Autorisieren des Zugriffs auf den Cluster).
* Wenn Sie Daten in einen Azure-Datenspeicher verschieben, nutzen Sie die Auflistung der Compute-IP-Adressbereiche unter [Azure Data Center IP Ranges](https://www.microsoft.com/download/details.aspx?id=41653) (IP-Adressbereiche der Microsoft Azure-Rechenzentren einschließlich SQL-Bereiche).

## <a name="getting-started"></a>Erste Schritte
Sie können eine Pipeline mit einer Kopieraktivität erstellen, die Daten mithilfe verschiedener Tools/APIs aus einer Amazon Redshift-Quelle verschiebt.

Am einfachsten erstellen Sie eine Pipeline mit dem **Kopier-Assistenten**. Unter [Tutorial: Erstellen einer Pipeline mit dem Assistenten zum Kopieren](data-factory-copy-data-wizard-tutorial.md) finden Sie eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten.

Sie können auch die folgenden Tools für das Erstellen einer Pipeline verwenden: **Azure-Portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-Vorlagen**, **.NET-API** und **REST-API**. Im [Tutorial zur Kopieraktivität](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) finden Sie detaillierte Anweisungen, wie Sie eine Pipeline mit einer Kopieraktivität erstellen können. 

Unabhängig davon, ob Sie Tools oder APIs verwenden, führen Sie die folgenden Schritte aus, um eine Pipeline zu erstellen, die Daten aus einem Quelldatenspeicher in einen Senkendatenspeicher verschiebt: 

1. Erstellen **verknüpfter Dienste** zum Verknüpfen von Eingabe- und Ausgabedatenspeichern mit Ihrer Data Factory
2. Erstellen von **Datasets** zur Darstellung von Eingabe- und Ausgabedaten für den Kopiervorgang 
3. Erstellen einer **Pipeline** mit einer Kopieraktivität, die ein Dataset als Eingabe und ein Dataset als Ausgabe akzeptiert 

Wenn Sie den Assistenten verwenden, werden automatisch JSON-Definitionen für diese Data Factory-Entitäten (verknüpfte Diensten, Datasets und die Pipeline) erstellt. Bei Verwendung von Tools und APIs (mit Ausnahme der .NET-API) definieren Sie diese Data Factory-Entitäten im JSON-Format.  Ein Beispiel mit JSON-Definitionen für Data Factory-Entitäten, die zum Kopieren von Daten aus einem Amazon Redshift-Datenspeicher verwendet werden, finden Sie in diesem Artikel im Abschnitt [JSON-Beispiel: Kopieren von Daten aus Amazon Redshift in ein Azure-Blob](#json-example-copy-data-from-amazon-redshift-to-azure-blob). 

Die folgenden Abschnitte enthalten Details zu JSON-Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für Amazon Redshift verwendet werden: 

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts
Die folgende Tabelle enthält eine Beschreibung der JSON-Elemente, die für den mit Amazon Redshift verknüpften Dienst spezifisch sind.

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| type |Die type-Eigenschaft muss auf **AmazonRedshift**festgelegt sein. |Ja |
| server |IP-Adresse oder Hostname des Amazon Redshift-Servers. |Ja |
| port |Die Nummer des TCP-Ports, den der Amazon Redshift-Server verwendet, um auf Clientverbindungen zu lauschen. |Nein, Standardwert: 5439 |
| database |Der Name der Amazon Redshift-Datenbank. |Ja |
| username |Der Name des Benutzers, der Zugriff auf die Datenbank hat. |Ja |
| password |Kennwort für das Benutzerkonto. |Ja |

## <a name="dataset-properties"></a>Dataset-Eigenschaften
Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie „structure“, „availability“ und „policy“ sind bei allen Dataset-Typen (Azure SQL, Azure-Blob, Azure-Tabelle usw.) ähnlich.

Der Abschnitt **typeProperties** unterscheidet sich bei jedem Typ von Dataset und bietet Informationen zum Speicherort der Daten im Datenspeicher. Der Abschnitt typeProperties für ein Dataset vom Typ **RelationalTable** (wozu ein Amazon Redshift-Dataset gehört) hat die folgenden Eigenschaften.

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| tableName |Name der Tabelle in der Amazon Redshift-Datenbank, auf die der verknüpfte Dienst verweist. |Nein (wenn **query** von **RelationalSource** angegeben ist) |

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität
Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md). Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabetabellen und Richtlinien sind für alle Arten von Aktivitäten verfügbar.

Eigenschaften im Abschnitt **typeProperties** der Aktivität können dagegen je nach Aktivitätstyp variieren. Für die Kopieraktivität variieren die Eigenschaften je nach Art der Quellen und Senken.

Wenn die Quelle der Kopieraktivität vom Typ **RelationalSource** ist (zu dem Amazon Redshift gehört), sind im Abschnitt typeProperties die folgenden Eigenschaften verfügbar:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| --- | --- | --- | --- |
| query |Verwendet die benutzerdefinierte Abfrage zum Lesen von Daten. |SQL-Abfragezeichenfolge. Beispiel: select * from MyTable. |Nein (wenn **tableName** von **Dataset** angegeben ist) |

## <a name="json-example-copy-data-from-amazon-redshift-to-azure-blob"></a>JSON-Beispiel: Kopieren von Daten aus Amazon Redshift in ein Azure-Blob
In diesem Beispiel wird gezeigt, wie Sie Daten aus Amazon Redshift in einen Azure-Blobspeicher kopieren. Daten können jedoch mithilfe der Kopieraktivität in Azure Data Factory **direkt** in die [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) aufgeführten Senken kopiert werden.  

Das Beispiel enthält die folgenden Data Factory-Entitäten:

* Ein verknüpfter Dienst des Typs [AmazonRedshift](#linked-service-properties).
* Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
* Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [RelationalTable](#dataset-properties)
* Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
* Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [RelationalSource](#copy-activity-properties) und [BlobSink](data-factory-azure-blob-connector.md##copy-activity-properties) verwendet

Das Beispiel kopiert stündlich Daten aus einem Abfrageergebnis in Amazon Redshift in ein Blob. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

**Mit Amazon Redshift verknüpfter Dienst:**

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties":
    {
        "type": "AmazonRedshift",
        "typeProperties":
        {
            "server": "< The IP address or host name of the Amazon Redshift server >",
            "port": <The number of the TCP port that the Amazon Redshift server uses to listen for client connections.>,
            "database": "<The database name of the Amazon Redshift database>",
            "username": "<username>",
            "password": "<password>"
        }
    }
}
```

**Mit Azure Storage verknüpfter Dienst:**

```json
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Amazon Redshift-Eingabedataset:**

Mit der Festlegung **"external": true** wird dem Data Factory-Dienst mitgeteilt, dass das Dataset für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird. Legen Sie diese Eigenschaft für ein Eingabedataset, das nicht durch eine Aktivität in der Pipeline erstellt wird, auf „true“ fest.

```json
{
    "name": "AmazonRedshiftInputDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "AmazonRedshiftLinkedService",
        "typeProperties": {
            "tableName": "<Table name>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

**Azure-Blob-Ausgabedataset:**

Daten werden stündlich in ein neues Blob geschrieben ("frequency": "hour", "interval": 1). Der Ordnerpfad des Blobs wird basierend auf der Startzeit des Slices, der verarbeitet wird, dynamisch ausgewertet. Im Ordnerpfad werden Jahr, Monat, Tag und die Stundenteile der Startzeit verwendet.

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/fromamazonredshift/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
```

**Kopieraktivität in einer Pipeline mit Azure Redshift-Quelle (RelationalSource) und Blobsenke:**

Die Pipeline enthält eine Kopieraktivität, die für die Verwendung der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **RelationalSource** und der Typ **sink** auf **BlobSink** festgelegt. Die für die **query** -Eigenschaft angegebene SQL-Abfrage wählt die aus der letzten Stunde zu kopierenden Daten aus.

```json
{
    "name": "CopyAmazonRedshiftToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "AmazonRedshiftInputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "AmazonRedshiftToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```
### <a name="type-mapping-for-amazon-redshift"></a>Typzuordnung für Amazon Redshift
Wie im Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) beschrieben, führt die Kopieraktivität automatische Typkonvertierungen von Quelltypen in Senkentypen mithilfe des folgenden aus zwei Schritten bestehenden Ansatzes durch:

1. Konvertieren von systemeigenen Quelltypen in den .NET-Typ
2. Konvertieren vom .NET-Typ in systemeigenen Senkentyp

Beim Verschieben von Daten in Amazon Redshift werden die folgenden Zuordnungen zwischen Amazon Redshift-Typ und .NET-Typ verwendet.

| Amazon Redshift-Typ | .NET-basierter Typ |
| --- | --- |
| SMALLINT |Int16 |
| INTEGER |Int32 |
| BIGINT |Int64 |
| DECIMAL |DECIMAL |
| REAL |Single |
| DOUBLE PRECISION |Double |
| BOOLEAN |String |
| CHAR |String |
| VARCHAR |String |
| DATE |DateTime |
| TIMESTAMP |DateTime |
| TEXT |String |

## <a name="map-source-to-sink-columns"></a>Zuordnen von Quell- zur Senkenspalten
Weitere Informationen zum Zuordnen von Spalten im Quelldataset zu Spalten im Senkendataset finden Sie unter [Zuordnen von Datasetspalten in Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Wiederholbare Lesevorgänge aus relationalen Quellen
Beim Kopieren von Daten aus relationalen Datenspeichern müssen Sie die Wiederholbarkeit berücksichtigen, um unbeabsichtigte Ergebnisse zu vermeiden. Sie können einen Slice in Azure Data Factory manuell erneut ausführen. Sie können auch eine Wiederholungsrichtlinie für ein Dataset konfigurieren, sodass ein Slice erneut ausgeführt wird, wenn ein Fehler auftritt. Wenn ein Slice erneut ausgeführt wird, müssen Sie sicherstellen, dass dieselben Daten gelesen werden – egal wie oft ein Slice ausgeführt wird. Weitere Informationen finden Sie unter [Wiederholbare Lesevorgänge aus relationalen Quellen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Leistung und Optimierung
Der Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) beschreibt wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.

## <a name="next-steps"></a>Nächste Schritte
Entsprechende Informationen finden Sie in den folgenden Artikeln:

* [Kopieraktivität-Tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) für schrittweise Anleitungen zum Erstellen einer Pipeline mit einer Kopieraktivität.

