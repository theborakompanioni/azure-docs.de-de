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
ms.date: 03/17/2017
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: a27ec9e1ebfde3493e41c493b85c0dc7f0ada2a0
ms.lasthandoff: 03/18/2017


---
# <a name="move-data-tofrom-on-premises-oracle-using-azure-data-factory"></a>Verschieben von Daten in lokales/aus lokalem Oracle mithilfe von Azure Data Factory
Dieser Artikel beschreibt, wie Sie die Data Factory-Kopieraktivität zum Verschieben von Daten in/aus Oracle in einen/aus einem anderen Datenspeicher verwenden können. Dieser Artikel baut auf dem Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) auf, der eine allgemeine Übersicht zur Datenverschiebung mit Kopieraktivität und unterstützten Datenspeicherkombinationen bietet.

Data Factory unterstützt das Herstellen einer Verbindung mit lokalen Oracle-Datenquellen mithilfe des Datenverwaltungsgateways. Im Artikel [Datenverwaltungsgateway](data-factory-data-management-gateway.md) erfahren Sie etwas über das Datenverwaltungsgateway, und unter [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit dem Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md) finden Sie schrittweise Anleitungen für das Einrichten des Gateways für eine Datenpipeline zum Verschieben von Daten.

> [!NOTE]
> Das Gateway ist auch erforderlich, wenn Oracle auf einem virtuellen Azure IaaS-Computer gehostet wird. Sie können das Gateway auf dem gleichen virtuellen IaaS-Computer installieren wie den Datenspeicher oder auch auf einem anderen virtuellen Computer, solange das Gateway eine Verbindung mit der Datenbank herstellen kann.
>

## <a name="supported-versions-and-installation"></a>Unterstützte Versionen und Installation
Connector für Oracle unterstützen zwei Treiberversionen:

- **Microsoft-Treiber für Oracle**, der ab Version 2.7 als Bündel mit dem Datenverwaltungsgateway bereitgestellt wird. Die Verwendung dieses Treibers wird **empfohlen**. Sie müssen neben dem Gateway für die Verbindung mit Oracle nichts weiter installieren, und das Kopieren funktioniert ebenfalls besser. Oracle Database Version 10g Release 2 und höher werden unterstützt.

    > [!NOTE]
    > Derzeit unterstützt der Microsoft-Treiber für Oracle nur das Kopieren von Daten aus Oracle, nicht jedoch das Schreiben in Oracle. Beachten Sie, dass die Funktion zum Testen der Verbindung auf der Registerkarte für die Datenverwaltungsgateway-Diagnose diesen Treiber nicht unterstützt. Alternativ können Sie den Assistenten zum Kopieren verwenden, um die Konnektivität zu überprüfen.
    >

- **Oracle-Datenanbieter für .NET:** Datenverwaltungsgateway, Version 2.7 oder höher, enthält diese Komponente, sodass Sie keine separate Installation durchführen müssen. Wenn Sie ein Gateway mit einer niedrigeren Version als 2.7 verwenden, sollten Sie die neueste Version des Gateways von [hier](https://www.microsoft.com/download/details.aspx?id=39717) installieren. Sie finden die Version des Gateways auf der Hilfeseite des Datenverwaltungsgateway-Konfigurations-Managers (Suchen Sie nach „Datenverwaltungsgateway“).

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Die einfachste Art, eine Pipeline zu erstellen, mit der Daten aus einer Oracle-Datenbank in einen der unterstützten Senkendatenspeicher kopiert werden (oder umgekehrt), ist die Verwendung des Assistenten zum Kopieren von Daten. Unter [Tutorial: Erstellen einer Pipeline mit dem Assistenten zum Kopieren](data-factory-copy-data-wizard-tutorial.md) finden Sie eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten.

Das folgende Beispiel stellt JSON-Beispieldefinitionen bereit, die Sie zum Erstellen einer Pipeline mit dem [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md), mit [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder mit [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) verwenden können. Darin wird veranschaulicht, wie Sie Daten aus einer Oracle-Datenbank in Azure Blob Storage kopieren. Daten können jedoch auch mithilfe der Kopieraktivität in Azure Data Factory in eine beliebige der [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) aufgeführten Senken kopiert werden.   

## <a name="sample-copy-data-from-oracle-to-azure-blob"></a>Beispiel: Kopieren von Daten aus Oracle in Azure-Blob 
In diesem Beispiel wird gezeigt, wie Sie Daten aus einer lokalen Oracle-Datenbank in einen Azure-BLOB-Speicher kopieren. Daten können jedoch mithilfe der Kopieraktivität in Azure Data Factory **direkt** in die [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) aufgeführten Senken kopiert werden.  

Das Beispiel enthält die folgenden Data Factory-Entitäten:

1. Einen verknüpften Dienst des Typs [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties)
2. Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service)
3. Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties)
4. Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)
5. Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [OracleSource](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) als Quelle und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) als Senke verwendet

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

## <a name="sample-copy-data-from-azure-blob-to-oracle"></a>Beispiel: Kopieren von Daten aus einem Azure-Blob in Oracle
In diesem Beispiel wird gezeigt, wie Sie Daten aus Azure Blog Storage in eine lokale Oracle-Datenbank kopieren. Daten können jedoch auch mithilfe der Kopieraktivität in Azure Data Factory **direkt** aus den [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) aufgeführten Quellen kopiert werden.  

Das Beispiel enthält die folgenden Data Factory-Entitäten:

1. Einen verknüpften Dienst des Typs [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties)
2. Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service)
3. Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)
4. Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties)
5. Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) als Quelle und [OracleSink](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) als Senke verwendet

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

## <a name="oracle-linked-service-properties"></a>Eigenschaften des mit Oracle verknüpften Diensts
Die folgende Tabelle enthält eine Beschreibung der JSON-Elemente, die für den mit Oracle verknüpften Dienst spezifisch sind.

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| type |Die "type"-Eigenschaft muss auf **OnPremisesOracle** |Ja |
| driverType | Legen Sie fest, welcher Treiber für das Kopieren von Daten aus/in Oracle Database verwendet wird. Zulässige Werte sind **Microsoft** oder **ODP** (Standard). Details zu den Treibern finden Sie unter [Unterstützte Versionen und Installation](#supported-versions-and-installation). | Nein |
| connectionString | Geben Sie Informationen, die zur Verbindung mit der Oracle Databaseinstanz erforderlich sind, für die Eigenschaft "connectionString" an. | Ja |
| gatewayName | Der Name des Gateways, das zum Herstellen einer Verbindung mit dem lokalen Oracle-Server verwendet wird. |Ja |

Ausführliche Informationen zum Festlegen von Anmeldeinformationen für eine lokale Oracle-Datenquelle finden Sie unter [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit dem Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md).

**Beispiel: Verwendung des Microsoft-Treibers**
```JSON
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
```JSON
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

## <a name="oracle-dataset-type-properties"></a>Eigenschaften des Dataset-Typs „Oracle“
Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie Struktur, Verfügbarkeit und Richtlinie einer Dataset-JSON sind bei allen Dataset-Typen (Oracle, Azure-Blob, Azure-Tabelle usw.) ähnlich.

Der Abschnitt "typeProperties" unterscheidet sich bei jedem Typ von Dataset und bietet Informationen zum Speicherort der Daten im Datenspeicher. Der Abschnitt „typeProperties“ für ein Dataset vom Typ „OracleTable“ hat die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| tableName |Name der Tabelle in der Oracle Database, auf die der verknüpfte Dienst verweist. |Nein (wenn **oracleReaderQuery** von **OracleSource** angegeben ist) |

## <a name="oracle-copy-activity-type-properties"></a>Eigenschaften des Oracle-Kopieraktivitätstyps
Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md). Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabetabellen und Richtlinie sind für alle Arten von Aktivitäten verfügbar.

> [!NOTE]
> Die Kopieraktivität verwendet nur eine Eingabe und erzeugt nur eine Ausgabe.
>
>

Eigenschaften im Abschnitt typeProperties der Aktivität können dagegen je nach Aktivitätstyp variieren. Für die Kopieraktivität variieren die Eigenschaften je nach Art der Quellen und Senken.

### <a name="oraclesource"></a>OracleSource
Wenn bei der Kopieraktivität eine Quelle vom Typ **OracleSource** verwendet wird, sind im Abschnitt **typeProperties** folgende Eigenschaften verfügbar:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| --- | --- | --- | --- |
| oracleReaderQuery |Verwendet die benutzerdefinierte Abfrage zum Lesen von Daten. |SQL-Abfragezeichenfolge. Beispiel: select *from MyTable <br/><br/>Falls nicht angegeben, die SQL-Anweisung, die ausgeführt wird: select* from MyTable |Nein (wenn **tableName** von **Dataset** angegeben ist) |

### <a name="oraclesink"></a>OracleSink
**OracleSink** unterstützt die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| --- | --- | --- | --- |
| writeBatchTimeout |Die Wartezeit für den Abschluss der Batcheinfügung, bis das Timeout wirksam wird. |Zeitraum<br/><br/> Beispiel: 00:30:00 (30 Minuten) |Nein |
| writeBatchSize |Fügt Daten in die SQL-Tabelle ein, wenn die Puffergröße "writeBatchSize" erreicht. |Integer (Gesamtanzahl von Zeilen) |Nein (Standard = 100) |
| sqlWriterCleanupScript |Geben Sie eine Abfrage für die Kopieraktivität an, bei deren Ausführung die Daten eines bestimmten Slice bereinigt werden. |Eine Abfrageanweisung. |Nein |
| sliceIdentifierColumnName |Geben Sie einen Spaltennamen an, den die Kopieraktivität mit einem automatisch generierten Slicebezeichner füllen soll, der bei erneuter Ausführung zum Bereinigen der Daten eines bestimmten Slices verwendet wird. |Spaltenname einer Spalte mit binärem Datentyp (32). |Nein |

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


[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

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
>

[!INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>Leistung und Optimierung
Der Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) beschreibt wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.

