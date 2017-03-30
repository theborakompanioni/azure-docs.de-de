---
title: Verschieben von Daten aus SAP HANA mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Daten mithilfe von Azure Data Factory aus SAP HANA verschieben.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2017
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: afe143848fae473d08dd33a3df4ab4ed92b731fa
ms.openlocfilehash: 415e91ca2f1e47094fc8b64569b5cc29a706999a
ms.lasthandoff: 03/17/2017


---
# <a name="move-data-from-sap-hana-using-azure-data-factory"></a>Verschieben von Daten aus SAP HANA mithilfe von Azure Data Factory
Dieser Artikel beschreibt, wie Sie die Kopieraktivität in einer Azure Data Factory-Pipeline verwenden können, um Daten aus SAP HANA in einen anderen Datenspeicher zu verschieben. Dieser Artikel baut auf dem Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) auf, der eine allgemeine Übersicht zur Datenverschiebung mit Kopieraktivität und unterstützten Datenspeicherkombinationen bietet. Data Factory unterstützt derzeit nur das Verschieben von Daten aus SAP HANA in andere Datenspeicher, aber nicht das Verschieben aus anderen Datenspeichern in SAP HANA.

## <a name="supported-versions-and-installation"></a>Unterstützte Versionen und Installation
Dieser Connector unterstützt jede Version der SAP HANA-Datenbank. Er unterstützt das Kopieren von Daten aus HANA-Informationsmodellen (z. B. Analyse- und Berechnungssichten) und aus Zeile/Spalte-Tabellen mit SQL-Abfragen.

Um Konnektivität zur SAP HANA-Instanz zu aktivieren, installieren Sie die folgenden Komponenten:
- **Datenverwaltungsgateway**: Der Data Factory-Dienst unterstützt das Herstellen einer Verbindung mit lokalen Datenspeichern (einschließlich SAP HANA) über eine Komponente, die als Datenverwaltungsgateway bezeichnet wird. Wenn Sie mehr zum Datenverwaltungsgateway erfahren möchten und eine schrittweise Anleitung zum Einrichten des Gateways wünschen, lesen Sie den Artikel [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit dem Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md). Das Gateway ist selbst dann erforderlich, wenn SAP HANA auf einem virtuellen Azure IaaS-Computer gehostet wird. Das Gateway kann auf dem gleichen virtuellen Computer wie der Datenspeicher oder auf einem anderen virtuellen Computer installiert werden, solange das Gateway eine Verbindung mit der Datenbank herstellen kann.
- **SAP HANA-ODBC-Treiber** auf dem Gatewaycomputer. Sie können den SAP HANA-ODBC-Treiber aus dem [SAP Software Download Center](https://support.sap.com/swdc) herunterladen. Suche Sie mit dem Schlüsselwort **SAP HANA CLIENT for Windows**. 

## <a name="supported-sinks"></a>Unterstützte Senken
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Sie können Daten aus SAP HANA in jeden unterstützten Senkendatenspeicher verschieben. 

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Die einfachste Art, eine Pipeline zu erstellen, die Daten aus SAP HANA in einen der unterstützten Senkendatenspeicher kopiert, ist die Verwendung des Assistenten zum Kopieren von Daten. Unter [Tutorial: Erstellen einer Pipeline mit dem Assistenten zum Kopieren](data-factory-copy-data-wizard-tutorial.md) finden Sie eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten.

Das folgende Beispiel stellt JSON-Beispieldefinitionen bereit, die Sie zum Erstellen einer Pipeline mit dem [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md), mit [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder mit [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) verwenden können. In diesem Beispiel wird gezeigt, wie Sie Daten aus einem lokalen SAP HANA-System in einen Azure-BLOB-Speicher kopieren. Daten können jedoch mithilfe der Kopieraktivität in Azure Data Factory **direkt** in die [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) aufgeführten Senken kopiert werden.  

> [!IMPORTANT]
> Dieses Beispiel enthält JSON-Codeausschnitte. Eine schrittweise Anleitung zum Erstellen der Data Factory ist nicht enthalten. Einen Artikel mit schrittweisen Anleitungen finden Sie unter [Verschieben von Daten zwischen lokalen Standorten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) .

## <a name="sample-copy-data-from-sap-hana-to-azure-blob"></a>Beispiel: Kopieren von Daten aus SAP HANA in Azure-Blob
Das Beispiel enthält die folgenden Data Factory-Entitäten:

1. Einen verknüpften Dienst des Typs [SapHana](#sap-hana-linked-service).
2. Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service)
3. Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [RelationalTable](#sap-hana-dataset)
4. Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)
5. Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [RelationalSource](#sap-hana-source-in-copy-activity) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) verwendet

Im Beispiel werden stündlich Daten aus einer SAP HANA-Instanz in ein Azure-Blob kopiert. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

Als Erstes richten Sie das Datenverwaltungsgateway ein. Anweisungen dazu finden Sie im Artikel [Verschieben von Daten zwischen lokalen Standorten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) .

### <a name="sap-hana-linked-service"></a>Mit SAP HANA verknüpfter Dienst
Dieser verknüpfte Dienst verbindet Ihre SAP HANA-Instanz mit der Data Factory. Die „type“-Eigenschaft wird auf **SapHana** festgelegt. Im Abschnitt „typeProperties“ werden Verbindungsinformationen für die SAP HANA-Instanz bereitgestellt.

```json
{
    "name": "SapHanaLinkedService",
    "properties":
    {
        "type": "SapHana",
        "typeProperties":
        {
            "server": "<server name>",
            "authenticationType": "<Basic, or Windows>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}

```

### <a name="azure-storage-linked-service"></a>Mit Azure Storage verknüpfter Dienst
Dieser verknüpfte Dienst verbindet Ihr Azure Storage-Konto mit der Data Factory. Die „type“-Eigenschaft wird auf **AzureStorage** festgelegt. Im Abschnitt „typeProperties“ werden Verbindungsinformationen für das Azure Storage-Konto bereitgestellt.

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

### <a name="sap-hana-input-dataset"></a>SAP HANA-Eingabedataset

Dieses Dataset definiert das SAP HANA-Dataset. Sie legen Sie den Typ des Data Factory-Datasets auf **RelationalTable** fest. Aktuell geben Sie keine typspezifischen Eigenschaften für ein SAP HANA-Dataset an. Die Abfrage in der Definition der Kopieraktivität gibt an, welche Daten aus der SAP HANA-Instanz gelesen werden sollen. 

Durch Festlegen der „external“-Eigenschaft auf „true“ wird dem Data Factory-Dienst mitgeteilt, dass die Tabelle für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

Die Eigenschaften „frequency“ und „interval“ definieren den Zeitplan. In diesem Fall werden die Daten stündlich aus der SAP HANA-Instanz gelesen. 

```json
{
    "name": "SapHanaDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapHanaLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

### <a name="azure-blob-output-dataset"></a>Azure-Blob-Ausgabedataset
Dieses Dataset definiert das Azure-Blob-Ausgabedatase. Die „type“-Eigenschaft wird auf „AzureBlob“ festgelegt. Im Abschnitt „typeProperties“ ist angegeben, wo die Daten, die aus der SAP HANA-Instanz kopiert werden, gespeichert werden. Die Daten werden stündlich in ein neues Blob geschrieben (frequency: Hour, interval: 1). Der Ordnerpfad des Blobs wird basierend auf der Startzeit des Slices, der verarbeitet wird, dynamisch ausgewertet. Im Ordnerpfad werden Jahr, Monat, Tag und die Stundenteile der Startzeit verwendet.

```json
{
    "name": "AzureBlobDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/saphana/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


### <a name="pipeline-with-copy-activity"></a>Pipeline mit Kopieraktivität

Die Pipeline enthält eine Kopieraktivität, die für die Verwendung der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **RelationalSource** (für SAP HANA-Quelle) und der Typ **sink** auf **BlobSink** festgelegt. Die für die **query** -Eigenschaft angegebene SQL-Abfrage wählt die aus der letzten Stunde zu kopierenden Daten aus.

```json
{
    "name": "CopySapHanaToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "<SQL Query for HANA>"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "SapHanaDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobDataSet"
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
                "name": "SapHanaToBlob"
            }
        ],
        "start": "2017-03-01T18:00:00Z",
        "end": "2017-03-01T19:00:00Z"
    }
}
```


## <a name="sap-hana-linked-service"></a>Mit SAP HANA verknüpfter Dienst
Die folgende Tabelle enthält eine Beschreibung der JSON-Elemente, die für den mit SAP HANA verknüpften Dienst spezifisch sind.

Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich
-------- | ----------- | -------------- | --------
server | Der Name des Servers, auf dem sich die SAP HANA-Instanz befindet. Wenn Ihr Server einen benutzerdefinierten Port verwendet, geben Sie `server:port` an. | string | Ja
authenticationType | Die Art der Authentifizierung. | string. „Basic“ oder „Windows“ | Ja 
username | Der Name des Benutzers, der Zugriff auf den SAP-Server hat | string | Ja
password | Kennwort für den Benutzer | string | Ja
gatewayName | Der Name des Gateways, das der Data Factory-Dienst zum Herstellen einer Verbindung mit der lokalen SAP HANA-Instanz verwenden soll | string | Ja
encryptedCredential | Die verschlüsselte Zeichenfolge mit Anmeldeinformationen | string | Nein

## <a name="sap-hana-dataset"></a>SAP HANA-Dataset
Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie „structure“, „availability“ und „policy“ des JSON-Codes eines Datasets sind bei allen Dataset-Typen (Azure SQL, Azure-Blob, Azure-Tabelle usw.) ähnlich.

Der Abschnitt **typeProperties** unterscheidet sich bei jedem Typ von Dataset und bietet Informationen zum Speicherort der Daten im Datenspeicher. Es sind keine typspezifischen Eigenschaften, die für das SAP HANA-Dataset des Typs **RelationalTable** unterstützt werden. 


## <a name="sap-hana-source-in-copy-activity"></a>SAP HANA-Quelle in Kopieraktivität
Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md). Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabetabellen und Richtlinien sind für alle Arten von Aktivitäten verfügbar.

Eigenschaften im Abschnitt **typeProperties** der Aktivität können dagegen je nach Aktivitätstyp variieren. Für die Kopieraktivität variieren die Eigenschaften je nach Art der Quellen und Senken.

Wenn die Quelle bei der Kopieraktivität den Typ **RelationalSource** hat (zu dem SAP HANA gehört), sind im Abschnitt „typeProperties“ die folgenden Eigenschaften verfügbar:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| --- | --- | --- | --- |
| query | Gibt die SQL-Abfrage an, mit der Daten aus der SAP HANA-Instanz gelesen werden. | SQL-Abfrage | Ja |

### <a name="type-mapping-for-sap-hana"></a>Typzuordnung für SAP HANA
Wie im Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) beschrieben, führt die Kopieraktivität automatische Typkonvertierungen von Quelltypen in Senkentypen mithilfe des folgenden aus zwei Schritten bestehenden Ansatzes durch:

1. Konvertieren von systemeigenen Quelltypen in den .NET-Typ
2. Konvertieren vom .NET-Typ in systemeigenen Senkentyp

Beim Verschieben von Daten aus SAP HANA werden die folgenden Zuordnungen zwischen den SAP HANA-Typen und den .NET-Typen verwendet.

SAP HANA-Typ | .NET-basierter Typ
------------- | ---------------
TINYINT | Byte
SMALLINT | Int16
INT | Int32
BIGINT | Int64
REAL | Single
DOUBLE | Single
DECIMAL | Decimal
BOOLEAN | Byte
VARCHAR | string
NVARCHAR | String
CLOB | Byte[]
ALPHANUM | string
BLOB | Byte[]
DATE | DateTime
TIME | Zeitraum
TIMESTAMP | DateTime
SECONDDATE | DateTime

## <a name="known-limitations"></a>Bekannte Einschränkungen
Es gibt einige bekannte Einschränkungen, wenn Daten aus SAP HANA kopiert werden:

- NVARCHAR-Zeichenfolgen werden auf maximal 4000 Unicode-Zeichen abgeschnitten.
- SMALLDECIMAL wird nicht unterstützt.
- VARBINARY wird nicht unterstützt.
- Gültige Datumsangaben liegen zwischen 1899/12/30 und 9999/12/31.

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]
[!INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]
[!INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Leistung und Optimierung
Der Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) beschreibt wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.

