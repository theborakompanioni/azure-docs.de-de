---
title: Verschieben von Daten in/aus Oracle mit Data Factory | Microsoft Dokumentation
description: Informationen zum Verschieben von Daten in und aus einer Oracle-Datenbank, die lokal oder mithilfe von Azure Data Factory gehostet wird.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 3c20aa95-a8a1-4aae-9180-a6a16d64a109
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: jingwang
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: 18fffb6cae9107b9301ff702d483b598836ac180
ms.contentlocale: de-de
ms.lasthandoff: 05/15/2017


---
# <a name="move-data-tofrom-on-premises-oracle-using-azure-data-factory"></a>Verschieben von Daten in lokales/aus lokalem Oracle mithilfe von Azure Data Factory
Dieser Artikel beschreibt, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten in und aus einer lokalen Oracle-Datenbank zu verschieben. Dieser Artikel baut auf dem Artikel zu [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) auf, der eine allgemeine Übersicht zur Datenverschiebung mit der Kopieraktivität bietet.

## <a name="supported-scenarios"></a>Unterstützte Szenarien
Sie können Daten **aus Oracle-Datenbank** in die folgenden Datenspeicher kopieren:

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

Sie können Daten aus den folgenden Datenspeichern **in Oracle-Datenbank** kopieren:

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="prerequisites"></a>Voraussetzungen
Data Factory unterstützt das Herstellen einer Verbindung mit lokalen Oracle-Datenquellen mithilfe des Datenverwaltungsgateways. Im Artikel [Datenverwaltungsgateway](data-factory-data-management-gateway.md) erfahren Sie etwas über das Datenverwaltungsgateway, und unter [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit dem Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md) finden Sie schrittweise Anleitungen für das Einrichten des Gateways für eine Datenpipeline zum Verschieben von Daten.

Das Gateway ist auch erforderlich, wenn Oracle auf einem virtuellen Azure IaaS-Computer gehostet wird. Sie können das Gateway auf dem gleichen virtuellen IaaS-Computer installieren wie den Datenspeicher oder auch auf einem anderen virtuellen Computer, solange das Gateway eine Verbindung mit der Datenbank herstellen kann.

> [!NOTE]
> Unter [Problembehandlung bei Gateways](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) finden Sie Tipps zur Behandlung von Verbindungs- bzw. Gatewayproblemen.

## <a name="supported-versions-and-installation"></a>Unterstützte Versionen und Installation
Dieser Connector für Oracle unterstützt zwei Treiberversionen:

- **Microsoft-Treiber für Oracle (empfohlen)**: Ab Datenverwaltungsgateway, Version 2.7, wird zusammen mit dem Gateway automatisch ein Microsoft-Treiber für Oracle installiert. Sie müssen also nicht zusätzlich den Treiber behandeln, um eine Verbindung mit Oracle herzustellen, und Sie erhalten mit diesem Treiber eine bessere Kopierleistung. Folgende Versionen von Oracle-Datenbanken werden unterstützt:
    - Oracle 12c R1 (12.1)
    - Oracle 11g R1, R2 (11.1, 11.2)
    - Oracle 10g R1, R2 (10.1, 10.2)
    - Oracle 9i R1, R2 (9.0.1, 9.2)
    - Oracle 8i R3 (8.1.7)

> [!IMPORTANT]
> Derzeit unterstützt der Microsoft-Treiber für Oracle nur das Kopieren von Daten aus Oracle, nicht jedoch das Schreiben in Oracle. Beachten Sie, dass die Funktion zum Testen der Verbindung auf der Registerkarte für die Datenverwaltungsgateway-Diagnose diesen Treiber nicht unterstützt. Alternativ können Sie den Assistenten zum Kopieren verwenden, um die Konnektivität zu überprüfen.
>

- **Oracle Data Provider für .NET:** Sie können auch Oracle Data Provider zum Kopieren von Daten aus/in Oracle verwenden. Diese Komponente ist in [Oracle Data Access Components (ODAC) für Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/)enthalten. Installieren Sie die geeignete Version (32/64 Bit) auf dem Computer, auf dem das Gateway installiert ist. [Oracle Data Provider .NET 12.1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) kann auf Oracle Database 10g, Version 2 oder höher, zugreifen.

    Wenn Sie „XCopy Installation“ auswählen, führen Sie die Schritte in der Datei „Readme.htm“ aus. Es wird empfohlen, das Installationsprogramm mit Benutzeroberfläche (also nicht die XCopy-Variante) auszuwählen.

    Starten Sie nach der Installation des Anbieters den Hostdienst des Datenverwaltungsgateways auf Ihrem Computer mithilfe des Applets „Dienste“ oder Datenverwaltungsgateway-Konfigurations-Managers **neu**.  

Wenn Sie zum Kopieren der Pipeline den Kopier-Assistenten verwenden, wird der Treibertyp automatisch bestimmt. Standardmäßig wird der Microsoft-Treiber verwendet, sofern die Gatewayversion nicht niedriger als 2.7 ist oder Sie Oracle als Senke wählen.

## <a name="getting-started"></a>Erste Schritte
Sie können eine Pipeline mit einer Kopieraktivität erstellen, die Daten mithilfe verschiedener Tools/APIs in und aus einer lokalen Oracle-Datenbank verschiebt.

Am einfachsten erstellen Sie eine Pipeline mit dem **Kopier-Assistenten**. Unter [Tutorial: Erstellen einer Pipeline mit dem Assistenten zum Kopieren](data-factory-copy-data-wizard-tutorial.md) finden Sie eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten.

Sie können auch die folgenden Tools für das Erstellen einer Pipeline verwenden: **Azure-Portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-Vorlagen**, **.NET-API** und **REST-API**. Im [Tutorial zur Kopieraktivität](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) finden Sie detaillierte Anweisungen, wie Sie eine Pipeline mit einer Kopieraktivität erstellen können.

Unabhängig davon, ob Sie Tools oder APIs verwenden, führen Sie die folgenden Schritte aus, um eine Pipeline zu erstellen, die Daten aus einem Quelldatenspeicher in einen Senkendatenspeicher verschiebt:

1. Eine **Data Factory**. Eine Data Factory kann eine oder mehrere Pipelines enthalten. 
2. Erstellen **verknüpfter Dienste** zum Verknüpfen von Eingabe- und Ausgabedatenspeichern mit Ihrer Data Factory. Wenn Sie beispielsweise Daten aus Oracle-Datenbank in Azure Blob Storage kopieren, erstellen Sie zwei verknüpfte Dienste, um Oracle-Datenbank und Ihr Azure Storage-Konto mit Ihrer Data Factory zu verknüpfen. Informationen zu Eigenschaften von verknüpften Diensten, die spezifisch für Oracle sind, finden Sie im Abschnitt [Eigenschaften von verknüpften Diensten](#linked-service-properties).
3. Erstellen von **Datasets** zur Darstellung von Eingabe- und Ausgabedaten für den Kopiervorgang. Im Beispiel, das im letzten Schritt erwähnt wurde, erstellen Sie ein Dataset, um die Tabelle mit den Eingabedaten in Oracle-Datenbank anzugeben. Außerdem erstellen Sie ein weiteres Dataset zum Angeben eines Blobcontainers und des Ordners, in dem die aus Oracle-Datenbank kopierten Daten enthalten sind. Informationen zu Dataseteigenschaften, die spezifisch für Oracle sind, finden Sie im Abschnitt [Dataseteigenschaften](#dataset-properties).
4. Erstellen einer **Pipeline** mit einer Kopieraktivität, die ein Dataset als Eingabe und ein Dataset als Ausgabe akzeptiert. Im oben erwähnten Beispiel verwenden Sie „OracleSource“ als Quelle und „BlobSink“ als Senke für die Kopieraktivität. Wenn Sie einen Kopiervorgang von Azure Blob Storage zu Oracle-Datenbank durchführen, verwenden Sie entsprechend „BlobSource“ und „OracleSink“ in der Kopieraktivität. Informationen zu den Eigenschaften von Kopieraktivitäten, die spezifisch für Oracle-Datenbank sind, finden Sie im Abschnitt [Eigenschaften der Kopieraktivität](#copy-activity-properties). Ausführliche Informationen zur Verwendung eines Datenspeichers als Quelle oder Senke erhalten Sie, indem Sie im vorherigen Abschnitt auf den Link für Ihren Datenspeicher klicken. 

Wenn Sie den Assistenten verwenden, werden automatisch JSON-Definitionen für diese Data Factory-Entitäten (verknüpfte Diensten, Datasets und die Pipeline) erstellt. Bei Verwendung von Tools und APIs (mit Ausnahme der .NET-API) definieren Sie diese Data Factory-Entitäten im JSON-Format.  Beispiele mit JSON-Definitionen für Data Factory-Entitäten für das Kopieren von Daten in und aus einer lokalen Oracle-Datenbank finden Sie in diesem Artikel im Abschnitt [JSON-Beispiele](#json-examples-for-copying-data-to-and-from-oracle-database).

Die folgenden Abschnitte enthalten Details zu JSON-Eigenschaften, die zum Definieren von Data Factory-Entitäten verwendet werden:

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts
Die folgende Tabelle enthält eine Beschreibung der JSON-Elemente, die für den mit Oracle verknüpften Dienst spezifisch sind.

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| type |Die "type"-Eigenschaft muss auf **OnPremisesOracle** |Ja |
| driverType | Legen Sie fest, welcher Treiber für das Kopieren von Daten aus/in Oracle Database verwendet wird. Zulässige Werte sind **Microsoft** oder **ODP** (Standard). Details zu den Treibern finden Sie unter [Unterstützte Versionen und Installation](#supported-versions-and-installation). | Nein |
| connectionString | Geben Sie Informationen, die zur Verbindung mit der Oracle Databaseinstanz erforderlich sind, für die Eigenschaft "connectionString" an. | Ja |
| gatewayName | Der Name des Gateways, das zum Herstellen einer Verbindung mit dem lokalen Oracle-Server verwendet wird. |Ja |

**Beispiel: Verwenden des Microsoft-Treibers**
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Beispiel: Verwendung des ODP-Treibers**

Weitere zulässige Formate finden Sie auf [dieser Website](https://www.connectionstrings.com/oracle-data-provider-for-net-odp-net/).

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<hostname>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<SID>)));
User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften
Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie Struktur, Verfügbarkeit und Richtlinie einer Dataset-JSON sind bei allen Dataset-Typen (Oracle, Azure-Blob, Azure-Tabelle usw.) ähnlich.

Der Abschnitt "typeProperties" unterscheidet sich bei jedem Typ von Dataset und bietet Informationen zum Speicherort der Daten im Datenspeicher. Der Abschnitt „typeProperties“ für ein Dataset vom Typ „OracleTable“ hat die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| tableName |Name der Tabelle in der Oracle Database, auf die der verknüpfte Dienst verweist. |Nein (wenn **oracleReaderQuery** von **OracleSource** angegeben ist) |

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität
Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md). Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabetabellen und Richtlinie sind für alle Arten von Aktivitäten verfügbar.

> [!NOTE]
> Die Kopieraktivität verwendet nur eine Eingabe und erzeugt nur eine Ausgabe.

Eigenschaften im Abschnitt typeProperties der Aktivität können dagegen je nach Aktivitätstyp variieren. Für die Kopieraktivität variieren die Eigenschaften je nach Art der Quellen und Senken.

### <a name="oraclesource"></a>OracleSource
Wenn bei der Kopieraktivität eine Quelle vom Typ **OracleSource** verwendet wird, sind im Abschnitt **typeProperties** folgende Eigenschaften verfügbar:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| --- | --- | --- | --- |
| oracleReaderQuery |Verwendet die benutzerdefinierte Abfrage zum Lesen von Daten. |SQL-Abfragezeichenfolge. Beispiel: select * from MyTable <br/><br/>Falls nicht angegeben, wird folgende SQL-Anweisung ausgeführt: select * from MyTable. |Nein (wenn **tableName** von **Dataset** angegeben ist) |

### <a name="oraclesink"></a>OracleSink
**OracleSink** unterstützt die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| --- | --- | --- | --- |
| writeBatchTimeout |Die Wartezeit für den Abschluss der Batcheinfügung, bis das Timeout wirksam wird. |Zeitraum<br/><br/> Beispiel: 00:30:00 (30 Minuten) |Nein |
| writeBatchSize |Fügt Daten in die SQL-Tabelle ein, wenn die Puffergröße "writeBatchSize" erreicht. |Integer (Gesamtanzahl von Zeilen) |Nein (Standard = 100) |
| sqlWriterCleanupScript |Geben Sie eine Abfrage für die Kopieraktivität an, bei deren Ausführung die Daten eines bestimmten Slice bereinigt werden. |Eine Abfrageanweisung. |Nein |
| sliceIdentifierColumnName |Geben Sie einen Spaltennamen an, den die Kopieraktivität mit einem automatisch generierten Slicebezeichner füllen soll, der bei erneuter Ausführung zum Bereinigen der Daten eines bestimmten Slices verwendet wird. |Spaltenname einer Spalte mit binärem Datentyp (32). |Nein |

## <a name="json-examples-for-copying-data-to-and-from-oracle-database"></a>JSON-Beispiele zum Kopieren von Daten in bzw. aus Oracle-Datenbank
Das folgende Beispiel stellt JSON-Beispieldefinitionen bereit, die Sie zum Erstellen einer Pipeline mit dem [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md), mit [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder mit [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) verwenden können. Darin wird veranschaulicht, wie Sie Daten aus einer Oracle-Datenbank in Azure Blob Storage kopieren. Daten können jedoch auch mithilfe der Kopieraktivität in Azure Data Factory in eine beliebige der [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) aufgeführten Senken kopiert werden.   

## <a name="example-copy-data-from-oracle-to-azure-blob"></a>Beispiel: Kopieren von Daten aus Oracle in ein Azure-Blob

Das Beispiel enthält die folgenden Data Factory-Entitäten:

1. Einen verknüpften Dienst des Typs [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties)
2. Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
3. Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties)
4. Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
5. Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [OracleSource](data-factory-onprem-oracle-connector.md#copy-activity-properties) als Quelle und [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) als Senke verwendet

Das Beispiel kopiert Daten stündlich aus einer Tabelle einer lokalen Oracle-Datenbank in ein Blob. Weitere Informationen zu den verschiedenen Eigenschaften, die in dem Beispiel verwendet werden, finden Sie in den Dokumentationsabschnitten nach den Beispielen.

**Mit Oracle verknüpfter Dienst:**

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Mit Azure-Blobspeicher verknüpfter Dienst:**

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
    }
}
```

**Oracle-Eingabedataset:**

Im Beispiel wird vorausgesetzt, dass Sie die Tabelle "MyTable" in Oracle erstellt haben, die für Zeitreihendaten eine Spalte namens "timestampcolumn" enthält.

Durch Festlegen von „external“ auf „true“ wird dem Data Factory-Dienst mitgeteilt, dass das Dataset für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

```json
{
    "name": "OracleInput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "offset": "01:00:00",
            "interval": "1",
            "anchorDateTime": "2014-02-27T12:00:00",
            "frequency": "Hour"
        },
        "policy": {     
            "externalData": {        
                "retryInterval": "00:01:00",    
                "retryTimeout": "00:10:00",       
                "maximumRetry": 3       
            }     
        }
    }
}
```

**Azure-Blob-Ausgabedataset:**

Daten werden stündlich in ein neues Blob geschrieben ("frequency": "hour", "interval": 1). Ordnerpfad und Dateiname des Blobs werden basierend auf der Startzeit des Slices, der verarbeitet wird, dynamisch ausgewertet. Im Ordnerpfad werden Jahr, Monat, Tag und die Stundenteile der Startzeit verwendet.

```json
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
            ],
            "format": {
                "type": "TextFormat",
                "columnDelimiter": "\t",
                "rowDelimiter": "\n"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Pipeline mit Kopieraktivität:**

Die Pipeline enthält eine Kopieraktivität, die für die Verwendung der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **OracleSource** und der Typ **sink** auf **BlobSink** festgelegt.  Die für die **oracleReaderQuery** -Eigenschaft angegebene SQL-Abfrage wählt die aus der letzten Stunde zu kopierenden Daten aus.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
            {
                "name": "OracletoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": " OracleInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "OracleSource",
                        "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 0,
                    "timeout": "01:00:00"
                }
            }
        ]
    }
}
```

## <a name="example-copy-data-from-azure-blob-to-oracle"></a>Beispiel: Kopieren von Daten aus einem Azure-Blob in Oracle
In diesem Beispiel wird gezeigt, wie Sie Daten aus Azure Blog Storage in eine lokale Oracle-Datenbank kopieren. Daten können jedoch auch mithilfe der Kopieraktivität in Azure Data Factory **direkt** aus den [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) aufgeführten Quellen kopiert werden.  

Das Beispiel enthält die folgenden Data Factory-Entitäten:

1. Einen verknüpften Dienst des Typs [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties)
2. Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
3. Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
4. Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties)
5. Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) als Quelle und [OracleSink](data-factory-onprem-oracle-connector.md#copy-activity-properties) als Senke verwendet

Das Beispiel kopiert Daten stündlich aus einem Blob in eine Tabelle in einer lokalen Oracle-Datenbank. Weitere Informationen zu den verschiedenen Eigenschaften, die in dem Beispiel verwendet werden, finden Sie in den Dokumentationsabschnitten nach den Beispielen.

**Mit Oracle verknüpfter Dienst:**
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<hostname>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<SID>)));
            User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Mit Azure-Blobspeicher verknüpfter Dienst:**
```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
    }
}
```

**Azure-Blob-Eingabedataset**

Daten werden stündlich aus einem neuen Blob übernommen ("frequency": "hour", "interval": 1). Ordnerpfad und Dateiname des Blobs werden basierend auf der Startzeit des Slices, der verarbeitet wird, dynamisch ausgewertet. Der Ordnerpfad enthält das Jahr, den Monat und den Tag der Startzeit, der Dateiname enthält die Stunde der Startzeit. Die Festlegung von „external“ auf „true“ teilt dem Data Factory-Dienst mit, dass diese Tabelle für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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
                }
            ],
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

**Oracle-Ausgabedataset:**

Im Beispiel wird davon ausgegangen, dass Sie die Tabelle „MyTable“ in Oracle erstellt haben. Erstellen Sie die Tabelle in Oracle mit der gleichen Anzahl von Spalten, die Sie in der Blob-CSV-Datei erwarten. Neue Zeilen werden der Tabelle stündlich hinzugefügt.

```json
{
    "name": "OracleOutput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Day",
            "interval": "1"
        }
    }
}
```

**Pipeline mit Kopieraktivität:**

Die Pipeline enthält eine Kopieraktivität, die für die Verwendung der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **BlobSource** und der Typ **sink** auf **OracleSink** festgelegt.  

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-05T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
            {
                "name": "AzureBlobtoOracle",
                "description": "Copy Activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "OracleOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "OracleSink"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 0,
                    "timeout": "01:00:00"
                }
            }
        ]
    }
}
```


## <a name="troubleshooting-tips"></a>Tipps zur Problembehandlung
### <a name="problem-1-net-framework-data-provider"></a>Problem 1: .NET Framework-Datenanbieter

Folgende **Fehlermeldung** wird angezeigt:

    Copy activity met invalid parameters: 'UnknownParameterName', Detailed message: Unable to find the requested .Net Framework Data Provider. It may not be installed”.  

**Mögliche Ursachen:**

1. Der .NET Framework-Datenanbieter für Oracle wurde nicht installiert.
2. Der .NET Framework-Datenanbieter für Oracle wurde für .NET Framework 2.0 installiert und in den Ordnern für .NET Framework 4.0 nicht gefunden.

**Lösung/Problemumgehung:**

1. Falls Sie den .NET Framework-Datenanbieter für Oracle nicht installiert haben, [installieren Sie ihn](http://www.oracle.com/technetwork/topics/dotnet/downloads/) , und wiederholen Sie anschließend das Szenario.
2. Falls Sie die Fehlermeldung auch nach der Installation des Anbieters erhalten, führen Sie folgende Schritte aus:
   1. Öffnen Sie die Computerkonfiguration von .NET 2.0 im folgenden Ordner: <system disk>:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.
   2. Wenn Sie nach **Oracle Data Provider für .NET** suchen, sollten Sie unter **system.data** -> **DbProviderFactories** einen Eintrag wie im folgenden Beispiel finden: „<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Oracle Data Provider für .NET" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />”
3. Kopieren Sie diesen Eintrag in die Datei „machine.config“ im v4.0-Ordner „<system disk>:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config“, und ändern Sie die Version in „4.xxx.x.x“.
4. Führen Sie `gacutil /i [provider path]` aus, um „<ODP.NET-Installationspfad>\11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll“ im globalen Assemblycache (GAC) zu installieren.## Tipps zur Problembehandlung

### <a name="problem-2-datetime-formatting"></a>Problem 2: DateTime-Formatierung

Folgende **Fehlermeldung** wird angezeigt:

    Message=Operation failed in Oracle Database with the following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

**Lösung/Problemumgehung:**

Möglicherweise müssen Sie die Abfragezeichenfolge in Ihrer Kopieraktivität basierend darauf anpassen, wie Daten in Ihrer Oracle-Datenbank konfiguriert sind. Siehe folgendes Beispiel (mithilfe der to_date-Funktion):

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"


### <a name="type-mapping-for-oracle"></a>Typzuordnung für Oracle
Wie im Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) beschrieben, führt die Kopieraktivität mithilfe der folgenden beiden Schritte automatische Typkonvertierungen von Quelltypen in Senkentypen durch:

1. Konvertieren von systemeigenen Quelltypen in den .NET-Typ
2. Konvertieren vom .NET-Typ in systemeigenen Senkentyp

Beim Verschieben von Daten aus Oracle werden die folgenden Zuordnungen zwischen dem Datentyp „Oracle“ und dem Typ „.NET“ verwendet (und umgekehrt):

| Datentyp "Oracle" | Datentyp ".NET Framework" |
| --- | --- |
| BFILE |Byte[] |
| BLOB |Byte[] |
| CHAR |String |
| CLOB |String |
| DATE |DateTime |
| FLOAT |Decimal |
| INTEGER |Decimal |
| INTERVAL YEAR TO MONTH |Int32 |
| INTERVAL DAY TO SECOND |Zeitraum |
| LONG |String |
| LONG RAW |Byte[] |
| NCHAR |String |
| NCLOB |String |
| NUMBER |Decimal |
| NVARCHAR2 |String |
| RAW |Byte[] |
| ROWID |String |
| TIMESTAMP |DateTime |
| TIMESTAMP WITH LOCAL TIME ZONE |DateTime |
| TIMESTAMP WITH TIME ZONE |DateTime |
| UNSIGNED INTEGER |NUMBER |
| VARCHAR2 |String |
| XML |String |

> [!NOTE]
> Die Datentypen **INTERVAL YEAR TO MONTH** und **INTERVAL DAY TO SECOND** werden bei Verwendung des Microsoft-Treibers nicht unterstützt.

## <a name="map-source-to-sink-columns"></a>Zuordnen von Quell- zur Senkenspalten
Weitere Informationen zum Zuordnen von Spalten im Quelldataset zu Spalten im Senkendataset finden Sie unter [Zuordnen von Datasetspalten in Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Wiederholbare Lesevorgänge aus relationalen Quellen
Beim Kopieren von Daten aus relationalen Datenspeichern müssen Sie die Wiederholbarkeit berücksichtigen, um unbeabsichtigte Ergebnisse zu vermeiden. Sie können einen Slice in Azure Data Factory manuell erneut ausführen. Sie können auch eine Wiederholungsrichtlinie für ein Dataset konfigurieren, sodass ein Slice erneut ausgeführt wird, wenn ein Fehler auftritt. Wenn ein Slice erneut ausgeführt wird, müssen Sie sicherstellen, dass dieselben Daten gelesen werden – egal wie oft ein Slice ausgeführt wird. Weitere Informationen finden Sie unter [Wiederholbare Lesevorgänge aus relationalen Quellen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Leistung und Optimierung
Der Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) beschreibt wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.

