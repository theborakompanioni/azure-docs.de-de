---
title: "Verschieben von Daten aus einer HTTP-Quelle – Azure | Microsoft-Dokumentation"
description: Hier finden Sie Informationen zum Verschieben von Daten aus einer lokalen oder in der Cloud gehosteten HTTP-Quelle mithilfe von Azure Data Factory.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.translationtype: Human Translation
ms.sourcegitcommit: 757d6f778774e4439f2c290ef78cbffd2c5cf35e
ms.openlocfilehash: 4870d2a0bbe35f3980864d8b4f3d011a189b650e
ms.contentlocale: de-de
ms.lasthandoff: 04/10/2017


---
# <a name="move-data-from-an-http-source-using-azure-data-factory"></a>Verschieben von Daten aus einer HTTP-Quelle mithilfe von Azure Data Factory
Dieser Artikel beschreibt, wie die Kopieraktivität in Azure Data Factory verwendet wird, um Daten von einem lokalen bzw. einem in der Cloud gehosteten HTTP-Endpunkt in einen unterstützten Senkendatenspeicher zu verschieben. Dieser Artikel baut auf dem Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) auf, der eine allgemeine Übersicht zur Datenverschiebung mit der Kopieraktivität und der als Quellen/Senken unterstützten Datenspeichern darstellt.

Data Factory unterstützt derzeit nur das Verschieben von Daten aus einer HTTP-Quelle in andere Datenspeicher, aber nicht das Verschieben aus anderen Datenspeichern an ein HTTP-Ziel.

## <a name="supported-scenarios-and-authentication-types"></a>Unterstützte Szenarien und Authentifizierungsarten
Sie können mit diesem HTTP-Connector Daten **sowohl aus in der Cloud gehosteten als auch lokalen HTTP(S)-Endpunkten** abrufen. Verwenden Sie dazu die HTTP-Methode **GET** oder **POST**. Die folgenden Authentifizierungstypen werden unterstützt: **Anonymous**, **Basic**, **Digest**, **Windows** und **ClientCertificate**. Beachten Sie den Unterschied zwischen diesem Connector und dem [Webtabellenconnector](data-factory-web-table-connector.md): Letzterer wird dazu verwendet, Tabelleninhalte aus HTML-Webseiten zu extrahieren.

Beim Kopieren von Daten von einem lokalen HTTP-Endpunkt müssen Sie in der lokalen Umgebung bzw. auf dem virtuellen Azure-Computer ein Datenverwaltungsgateway installieren. Im Artikel [Verschieben von Daten zwischen lokalen Standorten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) erfahren mehr zum Datenverwaltungsgateway und erhalten eine schrittweise Anleitung zum Einrichten des Gateways.

## <a name="getting-started"></a>Erste Schritte
Sie können eine Pipeline mit einer Kopieraktivität erstellen, die Daten mithilfe verschiedener Tools/APIs aus einer HTTP-Quelle verschiebt.

- Am einfachsten erstellen Sie eine Pipeline mit dem **Kopier-Assistenten**. Unter [Tutorial: Erstellen einer Pipeline mit dem Assistenten zum Kopieren](data-factory-copy-data-wizard-tutorial.md) finden Sie eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten.

- Sie können auch die folgenden Tools für das Erstellen einer Pipeline verwenden: **Azure-Portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-Vorlagen**, **.NET-API** und **REST-API**. Im [Tutorial zur Kopieraktivität](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) finden Sie detaillierte Anweisungen, wie Sie eine Pipeline mit einer Kopieraktivität erstellen können. JSON-Beispiele zum Kopieren von Daten aus einer HTTP-Quelle in Azure Blob Storage finden Sie in diesem Artikel im Abschnitt [JSON-Beispiele](#json-examples).

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts
Die folgende Tabelle enthält eine Beschreibung der JSON-Elemente, die für den verknüpften HTTP-Dienst spezifisch sind.

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| Typ | Die Typeigenschaft muss auf `Http` festgelegt sein. | Ja |
| url | Basis-URL zum Webserver | Ja |
| authenticationType | Gibt den Authentifizierungstyp an. Zulässige Werte: **Anonymous**, **Basic**, **Digest**, **Windows**, **ClientCertificate**. <br><br> Weitere Eigenschaften und JSON-Beispiele für diese Authentifizierungstypen finden Sie in den Abschnitten nach dieser Tabelle. | Ja |
| enableServerCertificateValidation | Angeben, ob die Überprüfung des SSL-Serverzertifikats aktiviert werden soll, wenn die Quelle ein HTTPS-Webserver ist | Nein. Der Standardwert ist TRUE. |
| gatewayName | Name des Datenverwaltungsgateways für die Verbindung mit einer lokalen HTTP-Quelle | Ja beim Kopieren von Daten von einer lokalen HTTP-Quelle |
| encryptedCredential | Verschlüsselte Anmeldeinformation für den Zugriff auf den HTTP-Endpunkt. Werden automatisch generiert, wenn Sie die Authentifizierungsinformationen im Kopier-Assistenten oder im ClickOnce-Popupdialogfeld konfigurieren. | Nein. Betrifft nur das Kopieren von Daten von einem lokalen HTTP-Server. |

Ausführliche Informationen zum Festlegen von Anmeldeinformationen für eine lokale HTTP-Connectordatenquelle finden Sie unter [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit dem Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md).

### <a name="using-basic-digest-or-windows-authentication"></a>Verwenden der Authentifizierung des Typs „Basic“, „Digest“ oder „Windows“

Legen Sie für `authenticationType` die Option `Basic`, `Digest` oder `Windows` fest, und geben Sie zusätzlich zu den oben vorgestellten allgemeinen HTTP-Connectoreigenschaften die folgenden Eigenschaften an:

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| username | Benutzername zum Zugreifen auf den HTTP-Endpunkt | Ja |
| password | Kennwort für den Benutzer (username) | Ja |

#### <a name="example-using-basic-digest-or-windows-authentication"></a>Beispiel: Verwenden der Authentifizierung des Typs „Basic“, „Digest“ oder „Windows“

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "basic",
            "url" : "https://en.wikipedia.org/wiki/",
            "userName": "user name",
            "password": "password"
        }
    }
}
```

### <a name="using-clientcertificate-authentication"></a>Verwenden der ClientCertificate-Authentifizierung

Legen Sie zum Verwenden der Standardauthentifizierung `authenticationType` auf `ClientCertificate` fest, und geben Sie zusätzlich zu den oben beschriebenen allgemeinen Eigenschaften des HTTP-Connectors die folgenden Eigenschaften an:

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| embeddedCertData | Der Base64-codierte Inhalt der Binärdaten der Personal Information Exchange-Datei (PFX) | Geben Sie entweder `embeddedCertData` oder `certThumbprint` an. |
| certThumbprint | Der Fingerabdruck des Zertifikats, das im Zertifikatspeicher des Gatewaycomputers installiert wurde. Betrifft nur das Kopieren von Daten von einer lokalen HTTP-Quelle. | Geben Sie entweder `embeddedCertData` oder `certThumbprint` an. |
| password | Das Kennwort des Zertifikats | Nein |

Wenn Sie `certThumbprint` für die Authentifizierung verwenden und das Zertifikat im persönlichen Speicher des lokalen Computers installiert wird, müssen Sie dem Gatewaydienst Leseberechtigungen gewähren:

1. Starten Sie die Microsoft Management Console (MMC). Fügen Sie das für **Lokaler Computer** vorgesehene Snap-In **Zertifikate** hinzu.
2. Erweitern Sie **Zertifikate** > **Personal** (Persönlich), und klicken Sie auf **Zertifikate**.
3. Klicken Sie im persönlichen Speicher mit der rechten Maustaste auf das Zertifikat, und wählen Sie **Alle Aufgaben**->**Private Schlüssel verwalten...**.
3. Fügen Sie auf der Registerkarte **Sicherheit** das Benutzerkonto hinzu, unter dem der Datenverwaltungsgateway-Hostdienst mit dem Lesezugriff auf das Zertifikat ausgeführt wird.  

#### <a name="example-using-client-certificate"></a>Beispiel: Verwenden eines Clientzertifikats
Dieser verknüpfte Dienst verbindet Ihre Data Factory mit einem lokalen HTTP-Server. Er verwendet ein Clientzertifikat, das auf dem Computer mit dem Datenverwaltungsgateway installiert ist.

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "certThumbprint": "thumbprint of certificate",
            "gatewayName": "gateway name"

        }
    }
}
```

#### <a name="example-using-client-certificate-in-a-file"></a>Beispiel: Verwenden eines Clientzertifikats in einer Datei
Dieser verknüpfte Dienst verbindet Ihre Data Factory mit einem lokalen HTTP-Server. Er verwendet eine Clientzertifikatdatei auf dem Computer, auf dem das Datenverwaltungsgateway installiert ist.

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "embeddedCertData": "base64 encoded cert data",
            "password": "password of cert"
        }
    }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften
Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie „structure“, „availability“ und „policy“ des JSON-Codes eines Datasets sind bei allen Dataset-Typen (Azure SQL, Azure-Blob, Azure-Tabelle usw.) ähnlich.

Der Abschnitt **typeProperties** unterscheidet sich bei jedem Typ von Dataset und bietet Informationen zum Speicherort der Daten im Datenspeicher. Der Abschnitt „typeProperties“ für ein Dataset des Typs **Http** hat die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Gibt an, dass der Datasettyp auf `Http` festgelegt werden muss. | Ja |
| relativeUrl | Eine relative URL zu der Ressource, die die Daten enthält. Wenn der Pfad nicht angegeben ist, wird nur die URL verwendet, die in der Definition des verknüpften Diensts angegeben ist. <br><br> Zum Erstellen einer dynamischen URL können Sie [Data Factory – Funktionen und Systemvariablen](data-factory-functions-variables.md) verwenden, z.B. „relativeUrl“: „$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)“. | Nein |
| requestMethod | HTTP-Methode. Zulässige Werte: **GET** oder **POST** | Nein. Der Standardwert ist `GET`. |
| additionalHeaders | Zusätzliche HTTP-Anforderungsheader | Nein |
| requestBody | Text für die HTTP-Anforderung | Nein |
| format | Wenn Sie einfach **die Daten ohne Änderung von einem HTTP-Endpunkt abrufen** möchten, ohne sie zu analysieren, überspringen Sie diese Formateinstellungen. <br><br> Wenn der HTTP-Antwortinhalt während des Kopierens analysiert werden soll, werden die folgenden Formattypen unterstützt: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Weitere Informationen finden Sie in den Abschnitten [Textformat](data-factory-supported-file-and-compression-formats.md#text-format), [JSON-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc-Format](data-factory-supported-file-and-compression-formats.md#orc-format) und [Parquet-Format](data-factory-supported-file-and-compression-formats.md#parquet-format). |Nein |
| Komprimierung | Geben Sie den Typ und den Grad der Komprimierung für die Daten an. Unterstützte Typen sind: **Gzip**, **Deflate**, **bzip2** und **ZipDeflate**. Unterstützte Grade sind: **Optimal** und **Schnellste**. Weitere Informationen finden Sie unter [Datei- und Komprimierungsformate in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nein |

### <a name="example-using-the-get-default-method"></a>Beispiel: Verwenden der GET-Methode (Standard)

```JSON
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "XXX/test.xml",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

### <a name="example-using-the-post-method"></a>Beispiel: Verwenden der POST-Methode

```JSON
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "/XXX/test.xml",
           "requestMethod": "Post",
            "requestBody": "body for POST HTTP request"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität
Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md). Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabetabellen und Richtlinie sind für alle Arten von Aktivitäten verfügbar.

Eigenschaften im Abschnitt **typeProperties** der Aktivität können dagegen je nach Aktivitätstyp variieren. Für die Kopieraktivität variieren die Eigenschaften je nach Art der Quellen und Senken.

Wenn bei der Kopieraktivität die Quelle den Typ **HttpSource** aufweist, werden derzeit die folgenden Eigenschaften unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| httpRequestTimeout | Das Timeout (TimeSpan) für die HTTP-Anforderung, um eine Antwort zu empfangen. Dabei handelt es sich um das Timeout zum Empfangen einer Antwort, nicht das Timeout zum Lesen von Antwortdaten. | Nein. Standardwert: 00:01:40 |

## <a name="supported-file-and-compression-formats"></a>Unterstützte Datei- und Komprimierungsformate
Einzelheiten finden Sie im Artikel [Datei- und Komprimierungsformate in Azure Data Factory](data-factory-supported-file-and-compression-formats.md).

## <a name="json-examples"></a>JSON-Beispiele
Die folgenden Beispiele zeigen JSON-Beispieldefinitionen, die Sie zum Erstellen einer Pipeline mit dem [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md), mit [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) verwenden können. Darin wird veranschaulicht, wie Sie Daten aus einer HTTP-Quelle in Azure Blob Storage kopieren. Daten können jedoch mithilfe der Kopieraktivität in Azure Data Factory **direkt** aus beliebigen Quellen in die [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) aufgeführten Senken kopiert werden.

### <a name="example-copy-data-from-http-source-to-azure-blob-storage"></a>Beispiel: Kopieren von Daten aus einer HTTP-Quelle in Azure Blob Storage
Die Data Factory-Lösung für dieses Beispiel enthält die folgenden Data Factory-Entitäten:

1. Einen verknüpften Dienst des Typs [HTTP](#linked-service-properties)
2. Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
3. Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [Http](#dataset-properties)
4. Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
5. Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [HttpSource](#copy-activity-properties) und [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) verwendet

Im Beispiel werden stündlich Daten aus einer HTTP-Quelle in ein Azure-Blob kopiert. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

### <a name="http-linked-service"></a>Verknüpfter HTTP-Dienst
Bei diesem Beispiel wird der verknüpfte HTTP-Dienst mit anonymer Authentifizierung verwendet. Informationen zu den verschiedenen Authentifizierungstypen finden Sie im Abschnitt [Verknüpfter HTTP-Dienst](#linked-service-properties).

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a>Mit Azure Storage verknüpfter Dienst

```JSON
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

### <a name="http-input-dataset"></a>HTTP-Eingabedataset
Durch Festlegen von **external** auf **true** wird dem Data Factory-Dienst mitgeteilt, dass das Dataset für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

```JSON
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}

```

### <a name="azure-blob-output-dataset"></a>Azure-Blob-Ausgabedataset

Daten werden stündlich in ein neues Blob geschrieben ("frequency": "hour", "interval": 1).

```JSON
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/Movies"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

### <a name="pipeline-with-copy-activity"></a>Pipeline mit Kopieraktivität

Die Pipeline enthält eine Kopieraktivität, die für die Verwendung der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **HttpSource** und der Typ **sink** auf **BlobSink** festgelegt.

Eine Liste der von „HttpSource“ unterstützten Eigenschaften finden Sie unter [HttpSource](#httpsource-in-copy-activity).

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "HttpSourceToAzureBlob",
        "description": "Copy from an HTTP source to an Azure blob",
        "type": "Copy",
        "inputs": [
          {
            "name": "HttpSourceDataInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "HttpSource"
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

> [!NOTE]
> Weitere Informationen zum Zuordnen von Spalten im Quelldataset zu Spalten im Senkendataset finden Sie unter [Zuordnen von Datasetspalten in Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Leistung und Optimierung
Der Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) beschreibt wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.

