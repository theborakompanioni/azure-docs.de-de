---
title: Verschieben von Daten in ein und aus einem Dateisystem | Microsoft Docs
description: Erfahren Sie, wie Daten mit Azure Data Factory in ein und aus ein lokales Dateisystem verschoben werden.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: ce19f1ae-358e-4ffc-8a80-d802505c9c84
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2016
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: 6ec8ac288a4daf6fddd6d135655e62fad7ae17c2
ms.openlocfilehash: e715588edb60dbe19449474e89da841e8b375878


---
# <a name="move-data-to-and-from-an-on-premises-file-system-by-using-azure-data-factory"></a>Verschieben von Daten in ein und aus ein lokales Dateisystem mit Azure Data Factory
Dieser Artikel beschreibt, wie Sie die Data Factory-Kopieraktivität zum Verschieben von Daten in ein und aus einem lokales Dateisystem verwenden können. Eine Liste mit Datenspeichern, die als Quellen oder Senken mit dem lokalen Dateisystem verwendet werden können, finden Sie unter [Unterstützte Datenspeicher](data-factory-data-movement-activities.md#supported-data-stores-and-formats) . Dieser Artikel baut auf dem Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) auf, der eine allgemeine Übersicht zur Datenverschiebung mit Kopieraktivität und unterstützten Datenspeicherkombinationen bietet.

Data Factory unterstützt das Herstellen einer Verbindung mit dem lokalen Dateisystem über das Datenverwaltungsgateway. Weitere Informationen zum Datenverwaltungsgateway und eine schrittweise Anleitung zum Einrichten des Gateways finden Sie im Artikel [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit dem Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md).

> [!NOTE]
> Abgesehen vom Datenverwaltungsgateway müssen keine anderen Binärdateien installiert werden, um die Kommunikation mit dem lokalen Dateisystem zu ermöglichen.
>
> Unter [Problembehandlung bei Gateways](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) finden Sie Tipps zur Behandlung von Verbindungs- bzw. Gatewayproblemen.
>
>

## <a name="linux-file-share"></a>Linux-Dateifreigabe
Führen Sie die folgenden beiden Schritte aus, um eine Linux-Dateifreigabe mit dem Dienst zu verwenden, der mit dem Dateiserver verknüpft ist:

* Installieren Sie [Samba](https://www.samba.org/) auf dem Linux-Server.
* Installieren und konfigurieren Sie das Datenverwaltungsgateway auf einem Windows-Server. Das Installieren des Datenverwaltungsgateways auf einem Linux-Server wird nicht unterstützt.

## <a name="copy-wizard"></a>Kopier-Assistent
Die einfachste Möglichkeit zum Erstellen einer Pipeline, die Daten aus einem und in ein lokales Dateisystem kopiert, ist die Verwendung des Kopier-Assistenten. Eine kurze exemplarische Vorgehensweise finden Sie im [Tutorial: Erstellen einer Pipeline mit dem Kopier-Assistenten](data-factory-copy-data-wizard-tutorial.md).

Die folgenden Beispiele zeigen JSON-Beispieldefinitionen, die Sie zum Erstellen einer Pipeline mit dem [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md), mit [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) verwenden können. Sie zeigen, wie Sie Daten in und aus einem lokalen Dateisystem und Azure Blob Storage kopieren. Allerdings können Sie mit der Kopier-Aktivität in Azure Data Factory.Daten *direkt* aus den Quellen in alle unter [Unterstützte Datenquellen und Senken](data-factory-data-movement-activities.md#supported-data-stores-and-formats) aufgeführten Senken kopieren.

## <a name="sample-copy-data-from-an-on-premises-file-system-to-azure-blob-storage"></a>Beispiel: Kopieren von Daten aus einem lokalen Dateisystem in Azure Blog Storage
In diesem Beispiel wird gezeigt, wie Sie Daten aus einem lokalen Dateisystem in Azure Blob Storage kopieren.

Das Beispiel enthält die folgenden Data Factory-Entitäten:

* Einen verknüpften Dienst des Typs [OnPremisesFileServer](data-factory-onprem-file-system-connector.md#on-premises-file-server-linked-service-properties)
* Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service)
* Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [FileShare](data-factory-onprem-file-system-connector.md#on-premises-file-system-dataset-type-properties)
* Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)
* Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [FileSystemSource](data-factory-onprem-file-system-connector.md#file-share-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) verwendet

Im folgenden Beispiel werden Zeitreihendaten aus dem lokalen Dateisystem stündlich in ein Azure Blob Storage kopiert. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten nach den Beispielen beschrieben.

Richten Sie zuerst das Datenverwaltungsgateway wie unter [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit dem Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md) beschrieben ein.

**Mit lokalem Dateiserver verknüpfter Dienst:**

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia.<region>.corp.<company>.com",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

Es empfiehlt sich, anstelle der Eigenschaften **userid** und **password** die Eigenschaft **encryptedCredential** zu verwenden. Weitere Informationen zu diesem verknüpften Dienst finden Sie unter [Mit lokalem Dateiserver verknüpfter Dienst](#onpremisesfileserver-linked-service-properties).

**Mit Azure Storage verknüpfter Dienst:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

**Eingabedataset aus dem lokalem Dateisystem:**

Daten werden stündlich aus einer neuen Datei abgerufen. Die Eigenschaften „folderPath“ und „fileName“ werden auf der Grundlage der Slice-Startzeit bestimmt.  

Die Festlegung von `"external": "true"` teilt dem Data Factory-Dienst mit, dass das Dataset für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

```JSON
{
  "name": "OnpremisesFileSystemInput",
  "properties": {
    "type": " FileShare",
    "linkedServiceName": " OnPremisesFileServerLinkedService ",
    "typeProperties": {
      "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
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
    "external": true,
    "availability": {
      "frequency": "Hour",
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

**Azure Blob Storage-Ausgabedataset:**

Daten werden stündlich in ein neues Blob geschrieben ("frequency": "hour", "interval": 1). Der Ordnerpfad des Blobs wird basierend auf der Startzeit des Slices, der verarbeitet wird, dynamisch ausgewertet. Im Ordnerpfad werden Jahr, Monat, Tag und die Stundenteile der Startzeit verwendet.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Kopieraktivität:**

Die Pipeline enthält eine Kopieraktivität, die für die Verwendung der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **FileSystemSource** und der Typ **sink** auf **BlobSink** festgelegt.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-06-01T18:00:00",
    "end":"2015-06-01T19:00:00",
    "description":"Pipeline for copy activity",
    "activities":[  
      {
        "name": "OnpremisesFileSystemtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "OnpremisesFileSystemInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "FileSystemSource"
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

## <a name="sample-copy-data-from-azure-sql-database-to-an-on-premises-file-system"></a>Beispiel: Kopieren von Daten aus Azure SQL-Datenbank in ein lokales Dateisystem
Dieses Beispiel zeigt Folgendes:

* Einen verknüpften Dienst des Typs "AzureSqlDatabase"
* Einen verknüpften Dienst des Typs "OnPremisesFileServer"
* Ein Eingabedataset des Typs "AzureSqlTable"
* Ein Ausgabedataset des Typs "FileShare"
* Eine Pipeline mit Kopieraktivität, die „SqlSource“ und „FileSystemSink“ verwendet.

Im Beispiel werden Zeitreihendaten aus einer Azure SQL-Tabelle stündlich in ein lokales Dateisystem kopiert. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten nach den Beispielen beschrieben.

**Mit Azure SQL verknüpfter Dienst:**

```JSON
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```

**Mit lokalem Dateiserver verknüpfter Dienst:**

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia.<region>.corp.<company>.com",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

Es empfiehlt sich, anstelle der Eigenschaften **userid** und **password** die Eigenschaft **encryptedCredential** zu verwenden. Weitere Informationen zu diesem verknüpften Dienst finden Sie unter [Eigenschaften des verknüpften Diensts „OnPremisesFileServer“](#onpremisesfileserver-linked-service-properties) .

**Azure SQL-Eingabedataset:**

Im Beispiel wird vorausgesetzt, dass Sie die Tabelle „MyTable“ in Azure SQL erstellt haben, die für Zeitreihendaten eine Spalte namens „timestampcolumn“ enthält.

Die Festlegung von ``“external”: ”true”`` teilt dem Data Factory-Dienst mit, dass das Dataset für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

```JSON
{
  "name": "AzureSqlInput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
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

**Ausgabedataset aus dem lokalem Dateisystem:**

Daten werden stündlich in eine neue Datei kopiert. Ordnerpfad (folderPath) und Dateiname (fileName) für das Blob werden auf der Grundlage der Slice-Startzeit bestimmt.

```JSON
{
  "name": "OnpremisesFileSystemOutput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": " OnPremisesFileServerLinkedService ",
    "typeProperties": {
      "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
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
    "external": true,
    "availability": {
      "frequency": "Hour",
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

**Pipeline mit Kopieraktivität:**

Die Pipeline enthält eine Kopieraktivität, die für die Verwendung der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **SqlSource** und der Typ **sink** auf **FileSystemSink** festgelegt. Die für die **SqlReaderQuery** -Eigenschaft angegebene SQL-Abfrage wählt die Daten der letzten Stunde zum Kopieren aus.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-06-01T18:00:00",
    "end":"2015-06-01T20:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "AzureSQLtoOnPremisesFile",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSQLInput"
          }
        ],
        "outputs": [
          {
            "name": "OnpremisesFileSystemOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "FileSystemSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 3,
          "timeout": "01:00:00"
        }
      }
     ]
   }
}
```

## <a name="on-premises-file-server-linked-service-properties"></a>Eigenschaften des mit dem lokalen Dateiserver verknüpften Diensts
Sie können ein lokales Dateisystem mithilfe eines verknüpften Dienst des Typs „lokaler Dateiserver“ mit einer Azure Data Factory verknüpfen. Die folgende Tabelle enthält Beschreibungen der JSON-Elemente, die für den mit dem lokalen Dateiserver verknüpften Dienst spezifisch sind.

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| type |Stellen Sie sicher, dass die Eigenschaft „type“ auf **OnPremisesFileServer** festgelegt ist. |Ja |
| host |Gibt den Stammpfad des Ordners an, den Sie kopieren möchten. Verwenden Sie für Sonderzeichen in der Zeichenfolge das Escapezeichen „\“. Beispiele finden Sie unter [Beispieldefinitionen für verknüpfte Dienste und Datasets](#sample-linked-service-and-dataset-definitions) . |Ja |
| userid |Geben Sie die ID des Benutzers an, der auf dem Server zugreifen darf. |Nein (wenn Sie "encryptedCredential" auswählen) |
| password |Geben Sie das Kennwort für das Benutzerkonto (userid) an. |Nein (wenn Sie "encryptedCredential" auswählen) |
| encryptedCredential |Geben Sie die verschlüsselten Anmeldeinformationen an. Diese können Sie durch Ausführen des Cmdlets „New-AzureRmDataFactoryEncryptValue“ abrufen. |Nein (wenn Sie "userid" und "password" unverschlüsselt angeben) |
| gatewayName |Gibt den Name des Gateways an, das der Data Factory-Dienst zum Verbinden mit dem lokalen Dateiserver verwenden soll. |Ja |

Ausführliche Informationen zum Festlegen von Anmeldeinformationen für eine lokale Dateisystem-Datenquelle finden Sie unter [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit dem Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md).

### <a name="sample-linked-service-and-dataset-definitions"></a>Beispieldefinitionen für verknüpfte Dienste und Datasets
| Szenario | Host in der Definition des verknüpften Diensts | folderPath in der Datasetdefinition |
| --- | --- | --- |
| Lokaler Ordner auf dem Datenverwaltungsgateway-Computer:  <br/><br/>Beispiele: D:\\\* oder D:\Ordner\Unterordner\\* |D:\\\\ (für Datenverwaltungsgateway 2.0 und neuere Versionen) <br/><br/> localhost (für ältere Versionen als Datenverwaltungsgateway 2.0) |.\\\\ oder Ordner\\\\Unterordner (für Datenverwaltungsgateway 2.0 und neuere Versionen) <br/><br/>D:\\\\ oder D:\\\\Ordner\\\\Unterordner (für Gatewayversionen unter 2.0) |
| Freigegebener Remoteordner:  <br/><br/>Beispiele: \\\\MeinServer\\Freigabe\\\* oder \\\\MeinServer\\Freigabe\\Ordner\\Unterordner\\* |\\\\\\\\MeinServer\\\\Freigabe |.\\\\ oder Ordner\\\\Unterordner |

**So ermitteln Sie die Version des Gateways:**

1. Starten Sie auf Ihrem Computer den [Datenverwaltungsgateway-Konfigurations-Manager](data-factory-data-management-gateway.md#configuration-manager) .
2. Wechseln Sie zur Registerkarte **Hilfe** .

> [!NOTE]
> Führen Sie ein [Upgrade Ihres Gateways auf Datenverwaltungsgateway 2.0 oder höher](data-factory-data-management-gateway.md#update) durchzuführen, um die neuesten Features und Korrekturen nutzen zu können.
>
>

**Beispiel: Mit "username" und "password" im Nur-Text-Format**

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

**Beispiel: Verwenden von "encryptedcredential"**

```JSON
{
  "Name": " OnPremisesFileServerLinkedService ",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "D:\\",
      "encryptedCredential": "WFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5xxxxxxxxxxxxxxxxx",
      "gatewayName": "mygateway"
    }
  }
}
```

## <a name="on-premises-file-system-dataset-type-properties"></a>Eigenschaften des Dataset-Typs „Lokales Dateisystem“
Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie unter [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie „structure“, „availability“ und „policy“ des JSON-Codes eines Datasets sind bei allen Dataset-Typen ähnlich.

Der Abschnitt „typeproperties“ ist bei jeder Art von Dataset unterschiedlich. Es enthält Informationen wie den Speicherort und das Format der Daten im Datenspeicher. Der Abschnitt typeProperties für ein Dataset des Typs **FileShare** hat die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| folderPath |Gibt den Unterpfad zum Ordner an. Verwenden Sie für Sonderzeichen in der Zeichenfolge das Escapezeichen „\“. Beispiele finden Sie unter [Beispieldefinitionen für verknüpfte Dienste und Datasets](#sample-linked-service-and-dataset-definitions) .<br/><br/>Sie können diese Eigenschaft mit **partitionBy** kombinieren, um Ordnerpfade auf der Grundlage von Datum und Uhrzeit für Start und Ende des Slices zu erhalten. |Ja |
| fileName |Geben Sie den Namen der Datei in **folderPath** an, wenn die Tabelle auf eine bestimmte Datei im Ordner verweisen soll. Wenn Sie keine Werte für diese Eigenschaft angeben, verweist die Tabelle auf alle Dateien im Ordner.<br/><br/>Wenn der Dateiname für ein Ausgabedataset nicht angegeben ist, weist der Name der generierten Datei das folgenden Format auf: <br/><br/>`Data.<Guid>.txt` (Beispiel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Nein |
| partitionedBy |Sie können mit „partitionedBy“ für Zeitreihendaten einen dynamischen Wert für „folderPath“ und „filename“ angeben. Beispiel: Parametrisierung von „folderPath“ für Daten nach Stunde. |Nein |
| Format |Die folgenden Formattypen werden unterstützt: **TextFormat**, **AvroFormat**, **JsonFormat**, **OrcFormat** und **ParquetFormat**. Sie müssen die **type** -Eigenschaft unter „format“ auf einen dieser Werte festlegen. Weitere Informationen finden Sie unter [Angeben von „TextFormat“](#specifying-textformat), [Angeben von „AvroFormat“](#specifying-avroformat), [Angeben von „JsonFormat“](#specifying-jsonformat), [Angeben von „OrcFormat“](#specifying-orcformat) und [Angeben von „ParquetFormat“](#specifying-parquetformat). Wenn Sie Dateien unverändert zwischen dateibasierten Speichern kopieren möchten (binäre Kopie), können Sie den Formatabschnitt bei den Definitionen von Eingabe- und Ausgabedatasets überspringen. |Nein |
| fileFilter |Geben Sie einen Filter zur Auswahl einer Teilmenge der Dateien in "folderPath" statt alle Dateien an. <br/><br/>Zulässige Werte sind: `*` (mehrere Zeichen) und `?` (einzelnes Zeichen).<br/><br/>Beispiel 1: „fileFilter“: „*.log“<br/>Beispiel 2: „fileFilter“: 2014 - 1-?. txt“<br/><br/>Beachten Sie, dass sich „fileFilter“ für das Eingabedataset „FileShare“ eignet. |Nein |
| Komprimierung |Geben Sie den Typ und den Grad der Komprimierung für die Daten an. Unterstützte Typen sind **GZip**, **Deflate** und **BZip2**. Unterstützte Grade sind **Optimal** und **Schnellste**. Für Daten mit **AvroFormat** oder **OrcFormat** werden derzeit keine Komprimierungseinstellungen unterstützt. Weitere Informationen finden Sie im Abschnitt [Komprimierungsunterstützung](#compression-support). |Nein |

> [!NOTE]
> „fileName“ und „fileFilter“ können nicht gleichzeitig verwendet werden.
>
>

### <a name="using-partitionedby-property"></a>Nutzen der Eigenschaft „partitionedBy“
Wie im vorherigen Abschnitt erwähnt, kann „partitionedBy“ verwendet werden, um für Zeitreihendaten einen dynamischen Wert für „folderPath“ und „filename“ anzugeben. Sie können dazu die Data Factory-Makros und Systemvariablen „SliceStart“ und „SliceEnd“ verwenden, die den logischen Zeitraum für einen bestimmten Datenslice angeben.

Weitere Informationen zu Zeitreihen-Datasets, Planung und Slices finden Sie unter [Erstellen von Datasets](data-factory-create-datasets.md), [Planung und Ausführung](data-factory-scheduling-and-execution.md) und [Erstellen von Pipelines](data-factory-create-pipelines.md).

#### <a name="sample-1"></a>Beispiel 1:

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

Im obigen Beispiel wird {Slice} durch den Wert der Data Factory-Systemvariablen SliceStart im Format (JJJJMMTTHH) ersetzt. „SliceStart“ bezieht sich auf die Startzeit des Slices. "folderPath" unterscheidet sich für jeden Slice. Beispiel: "wikidatagateway/wikisampledataout/2014100103" oder "wikidatagateway/wikisampledataout/2014100104".

#### <a name="sample-2"></a>Beispiel 2:

```JSON
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

Im Beispiel oben werden Jahr, Monat, Tag und Uhrzeit von SliceStart in separate Variablen extrahiert, die von den Eigenschaften „folderPath“ und „fileName“ verwendet werden.

[!INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]

[!INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="file-share-copy-activity-type-properties"></a>Eigenschaften des Kopieraktivitätstyps „Dateifreigabe“
**FileSystemSource** unterstützt die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| --- | --- | --- | --- |
| recursive |Gibt an, ob die Daten rekursiv aus den Unterordnern oder nur aus dem angegebenen Ordner gelesen werden. |True/False (Standardwert) |Nein |

**FileSystemSink** unterstützt die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| --- | --- | --- | --- |
| copyBehavior |Definiert das Verhalten beim Kopieren, wenn die Quelle "BlobSource" oder "FileSystem" ist. |**PreserveHierarchy:** Behält die Dateihierarchie im Zielordner bei. Der relative Pfad der Quelldatei zum Quellordner entspricht dem relativen Pfad der Zieldatei zum Zielordner.<br/><br/>**FlattenHierarchy** : Alle Dateien aus dem Quellordner werden auf der ersten Ebene des Zielordners erstellt. Die Namen der Zieldateien werden automatisch generiert.<br/><br/>**MergeFiles**: Alle Dateien aus dem Quellordner werden in einer Datei zusammengeführt. Wenn der Datei-/Blob-Name angegeben wurde, entspricht der Name dem angegebenen Namen, andernfalls dem automatisch generierten Dateinamen. |Nein |

### <a name="recursive-and-copybehavior-examples"></a>Beispiele für "recursive" und "copyBehavior"
Dieser Abschnitt beschreibt das resultierende Verhalten des Kopiervorgangs für verschiedene Kombinationen von Werten für recursive- und copyBehavior-Eigenschaften.

| recursive-Wert | copyBehavior-Wert | Resultierendes Verhalten |
| --- | --- | --- |
| true |preserveHierarchy |Für den Quellordner „Ordner1“ mit der folgenden Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5<br/><br/>wird der Zielordner „Ordner1“ mit der gleichen Struktur erstellt wie die Quelle:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5 |
| true |flattenHierarchy |Für den Quellordner „Ordner1“ mit der folgenden Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5<br/><br/>wird der Zielordner „Ordner1“ mit der folgenden Struktur erstellt: <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei5 |
| true |mergeFiles |Für den Quellordner „Ordner1“ mit der folgenden Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5<br/><br/>wird der Zielordner „Ordner1“ mit der folgenden Struktur erstellt: <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp; Inhalte von Datei1 + Datei2 + Datei3 + Datei4 + Datei5 werden in einer Datei mit einem automatisch generierten Namen zusammengeführt. |
| false |preserveHierarchy |Für den Quellordner „Ordner1“ mit der folgenden Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5<br/><br/>wird der Zielordner „Ordner1“ mit der folgenden Struktur erstellt:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/><br/>Unterordner1 mit Datei3, Datei4 und Datei5 wird nicht übernommen. |
| false |flattenHierarchy |Für den Quellordner „Ordner1“ mit der folgenden Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5<br/><br/>wird der Zielordner „Ordner1“ mit der folgenden Struktur erstellt:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei2<br/><br/>Unterordner1 mit Datei3, Datei4 und Datei5 wird nicht übernommen. |
| false |mergeFiles |Für den Quellordner „Ordner1“ mit der folgenden Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5<br/><br/>wird der Zielordner „Ordner1“ mit der folgenden Struktur erstellt:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Inhalte von Datei1 + Datei2 werden zu einer Datei mit einem automatisch generierten Namen zusammengeführt.<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei1<br/><br/>Unterordner1 mit Datei3, Datei4 und Datei5 wird nicht übernommen. |

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[!INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>Leistung und Optimierung
 Im [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) werden wichtige Faktoren beschrieben, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.



<!--HONumber=Nov16_HO3-->


