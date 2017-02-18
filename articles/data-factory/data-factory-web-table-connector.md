---
title: Verschieben von Daten aus einer Webtabelle mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Daten aus einer Tabelle auf einer Webseite mithilfe von Azure Data Factory verschieben.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: f54a26a4-baa4-4255-9791-5a8f935898e2
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2016
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: dd8a68029449ad013c4df9a46c558efaefd20e96
ms.openlocfilehash: 88f746a4802e13e062f4414e5d2032dfcee565b7


---
# <a name="move-data-from-a-web-table-source-using-azure-data-factory"></a>Verschieben von Daten aus einer Webtabelle mithilfe von Azure Data Factory
Dieser Artikel beschreibt, wie Sie die Kopieraktivität in einer Azure Data Factory verwenden können, um Daten aus einer Tabelle in einer Webseite in einen anderen Datenspeicher zu verschieben. Dieser Artikel baut auf dem Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) auf, der eine allgemeine Übersicht zur Datenverschiebung mit Kopieraktivität und unterstützten Datenspeicherkombinationen bietet.

Data Factory unterstützt derzeit nur das Verschieben von Daten aus einer Webtabelle in andere Datenspeicher, aber nicht das Verschieben aus anderen Datenspeichern in eine Webtabelle.

> [!NOTE]
> Dieser Webconnector unterstützt derzeit nur das Extrahieren von Tabelleninhalten einer HTML-Seite.
>
>

## <a name="sample-copy-data-from-web-table-to-azure-blob"></a>Beispiel: Kopieren von Daten aus einer Webtabelle in ein Azure-Blob
Das nachstehende Beispiel zeigt Folgendes:

1. Einen verknüpften Dienst des Typs [Web](#web-linked-service-properties)
2. Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service)
3. Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [WebTable](#WebTable-dataset-properties)
4. Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)
5. Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [WebSource](#websource-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) verwendet

Im Beispiel werden stündlich Daten aus einer Webtabelle in ein Azure-Blob kopiert. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

In diesem Beispiel wird gezeigt, wie Sie Daten aus einer Webtabelle in ein Azure-Blob kopieren. Daten können jedoch mithilfe der Kopieraktivität in Azure Data Factory direkt in die im Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) aufgeführten Senken kopiert werden.

**Verknüpfter Webdienst** In diesem Beispiel wird der verknüpfte Webdienst mit anonymer Authentifizierung verwendet. Informationen zu den verschiedenen Authentifizierungstypen finden Sie im Abschnitt [Eigenschaften des verknüpften Webdiensts](#web-linked-service-properties) .

```JSON
{
    "name": "WebLinkedService",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

**Mit Azure Storage verknüpfter Dienst**

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

**WebTable-Eingabedataset** Durch Festlegen von **external** auf **true** wird dem Data Factory-Dienst mitgeteilt, dass das Dataset für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

> [!NOTE]
> Im Abschnitt [Abrufen des Indexes einer Tabelle auf einer HTML-Seite](#get-index-of-a-table-in-an-html-page) werden die Schritte zum Abrufen des Indexes einer Tabelle auf einer HTML-Seite beschrieben.  
>
>

```JSON
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```


**Azure-Blob-Ausgabedataset**

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



**Pipeline mit Kopieraktivität**

Die Pipeline enthält eine Kopieraktivität, die für das Verwenden der oben genannten Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **WebSource** und der Typ **sink** auf **BlobSink** festgelegt.

Unter [WebSource-Typeigenschaften](#websource-copy-activity-type-properties) finden Sie die Liste der Eigenschaften, die von „WebSource“ unterstützt werden.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "WebTableToAzureBlob",
        "description": "Copy from a Web table to an Azure blob",
        "type": "Copy",
        "inputs": [
          {
            "name": "WebTableInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "WebSource"
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

## <a name="web-linked-service-properties"></a>Eigenschaften des Verknüpften Webdiensts
Die folgende Tabelle enthält eine Beschreibung der JSON-Elemente, die für den verknüpften Webdienst spezifisch sind.

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| Typ |Die Typeigenschaft muss auf **Web** |Ja |
| Url |URL der Webquelle |Ja |
| authenticationType |Anonym oder Standard. |Ja |
| userName |Benutzername für die Standardauthentifizierung. |Ja (für die Standardauthentifizierung) |
| password |Kennwort für die Standardauthentifizierung. |Ja (für die Standardauthentifizierung) |

### <a name="using-anonymous-authentication"></a>Verwenden der anonymen Authentifizierung

```JSON
{
    "name": "web",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

### <a name="using-basic-authentication"></a>Verwenden der Standardauthentifizierung

```JSON
{
    "name": "web",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "authenticationType": "basic",
            "url" : "http://myit.mycompany.com/",
            "userName": "Administrator",
            "password": "password"
        }
    }
}
```

## <a name="webtable-dataset-properties"></a>WebTable-Dataset-Eigenschaften
Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie „structure“, „availability“ und „policy“ des JSON-Codes eines Datasets sind bei allen Dataset-Typen (Azure SQL, Azure-Blob, Azure-Tabelle usw.) ähnlich.

Der Abschnitt **typeProperties** unterscheidet sich bei jedem Typ von Dataset und bietet Informationen zum Speicherort der Daten im Datenspeicher. Der „typeProperties“-Abschnitt für ein Dataset des Typs **WebTable** hat die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| type |Die Art des Datasets. Muss auf **WebTable** festgelegt sein. |Ja |
| path |Eine relative URL zu der Ressource, die die Tabelle enthält. |Nein. Wenn der Pfad nicht angegeben ist, wird nur die URL verwendet, die in der Definition des verknüpften Diensts angegeben ist. |
| index |Der Index der Tabelle in der Ressource. Im Abschnitt [Abrufen des Indexes einer Tabelle auf einer HTML-Seite](#get-index-of-a-table-in-an-html-page) werden die Schritte zum Abrufen des Indexes einer Tabelle auf einer HTML-Seite beschrieben. |Ja |

**Beispiel:**

```JSON
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

## <a name="websource---copy-activity-type-properties"></a>WebSource – Typeigenschaften der Kopieraktivität
Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md). Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabetabellen und Richtlinie sind für alle Arten von Aktivitäten verfügbar.

Eigenschaften im Abschnitt „typeProperties“ der Aktivität können dagegen je nach Aktivitätstyp variieren. Für die Kopieraktivität variieren die Eigenschaften je nach Art der Quellen und Senken.

Wenn bei der Kopieraktivität die Quelle den Typ **WebSource**hat, werden derzeit keine zusätzlichen Eigenschaften unterstützt.

## <a name="get-index-of-a-table-in-an-html-page"></a>Abrufen des Indexes einer Tabelle auf einer HTML-Seite
1. Starten Sie **Excel 2016**, und wechseln Sie zur Registerkarte **Daten**.  
2. Klicken Sie auf der Symbolleiste auf **Neue Abfrage**, zeigen Sie auf **Aus anderen Quellen**, und klicken Sie auf **Aus dem Web**.

    ![Power Query-Menü](./media/data-factory-web-table-connector/PowerQuery-Menu.png)
3. Geben Sie im Dialogfeld **Aus dem Web** die **URL**, die Sie im JSON-Code für den verknüpften Dienst verwenden möchten (Beispiel: https://en.wikipedia.org/wiki/), sowie den Pfad ein, den Sie für das Dataset angeben möchten (Beispiel: AFI%27s_100_Years...&100;_Movies), und klicken Sie anschließend auf **OK**.

    ![Aus dem Web (Dialogfeld) ](./media/data-factory-web-table-connector/FromWeb-DialogBox.png)

    In diesem Beispiel verwendete URL: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies
4. Falls das Dialogfeld **Auf Webinhalt zugreifen** angezeigt wird, wählen Sie die richtige **URL** und **Authentifizierung** aus, und klicken Sie auf **Verbinden**.

   ![Webinhalt aufrufen (Dialogfeld)](./media/data-factory-web-table-connector/AccessWebContentDialog.png)
5. Klicken Sie in der Strukturansicht auf ein** **Tabellenelement, um Inhalte aus der Tabelle anzuzeigen, und klicken Sie dann am unteren Rand auf **Bearbeiten**.  

   ![Navigator (Dialogfeld)](./media/data-factory-web-table-connector/Navigator-DialogBox.png)
6. Klicken Sie im Fenster **Abfrage-Editor** auf der Symbolleiste auf die Schaltfläche **Erweiterter Editor**.

    ![Erweiterter Editor (Schaltfläche)](./media/data-factory-web-table-connector/QueryEditor-AdvancedEditorButton.png)
7. Im Dialogfeld „Erweiterter Editor“ ist die Zahl neben „Quelle“ der Index.

    ![Erweiterter Editor – Index](./media/data-factory-web-table-connector/AdvancedEditor-Index.png)

Rufen Sie den Index bei Verwendung von Excel 2013 mit [Microsoft Power Query für Excel](https://www.microsoft.com/download/details.aspx?id=39379) ab. Ausführlichere Informationen finden Sie im Artikel [Connect to a web page](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) (Verbinden mit einer Webseite). Bei Verwendung von [Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)werden ähnliche Schritte verwendet.

[!INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Leistung und Optimierung
Der Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) beschreibt wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.



<!--HONumber=Jan17_HO4-->


