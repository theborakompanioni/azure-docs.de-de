---
title: Zuordnen von Datasetspalten in Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Quellspalten Zielspalten zuordnen.
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
ms.date: 03/21/2017
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: b4802009a8512cb4dcb49602545c7a31969e0a25
ms.openlocfilehash: 444ac59b0b21795228e08f79a25a638d72403399
ms.lasthandoff: 03/29/2017


---

# <a name="map-source-dataset-columns-to-destination-dataset-columns"></a>Zuordnen von Spalten im Quelldataset zu Spalten im Zieldataset
Die Spaltenzuordnung kann verwendet werden, um anzugeben, wie die in "structure" der Quelltabelle angegebenen Spalten den in "structure" der Senkentabelle angegebenen Spalten zugeordnet werden. Die **columnMapping**-Eigenschaft ist im Abschnitt **typeProperties** der Kopieraktivität verfügbar.

Spaltenzuordnungen unterstützen die folgenden Szenarien:

* Alle Spalten in der Struktur des Quelldatasets werden allen Spalten in der Struktur des Senkendatasets zugeordnet.
* Eine Teilmenge der Spalten in der Struktur des Quelldatasets wird allen Spalten in der Struktur des Senkendatasets zugeordnet.

Im Folgenden sind Fehlerbedingungen angegeben, die zu einer Ausnahme führen:

* Weniger Spalten oder mehr Spalten in "structure" der Senkentabelle als in der Zuordnung angegeben sind.
* Doppelte Zuordnung.
* Das Ergebnis der SQL-Abfrage enthält keinen Spaltennamen, der in der Zuordnung angegeben ist.

> [!NOTE]
> Die folgenden Beispiele beziehen sich auf Azure SQL und Azure Blob, sie gelten aber auch für beliebige Datenspeicher, die rechteckige Datasets unterstützen. Passen Sie die Definitionen der Datasets und verknüpften Dienste in den Beispielen an, damit sie auf Daten in der relevanten Datenquelle verweisen.

## <a name="sample-1--column-mapping-from-azure-sql-to-azure-blob"></a>Beispiel 1 – Spaltenzuordnung von Azure SQL zu Azure-Blob
In diesem Beispiel verfügt die Eingabetabelle über eine Struktur und diese verweist auf eine SQL-Tabelle in einer Azure SQL-Datenbank.

```json
{
    "name": "AzureSQLInput",
    "properties": {
        "structure": 
         [
           { "name": "userid"},
           { "name": "name"},
           { "name": "group"}
         ],
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
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

In diesem Beispiel verfügt die Ausgabetabelle über eine Struktur und diese verweist auf ein Blob in einem Azure-Blobspeicher.

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
         "structure": 
          [
                { "name": "myuserid"},
                { "name": "myname" },
                { "name": "mygroup"}
          ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
            "fileName":"myfile.csv",
            "format":
            {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

Der folgende JSON-Code definiert eine Kopieraktivität in einer Pipeline. Die Spalten der Quelle werden mithilfe der **Translator**-Eigenschaft den Spalten der Senke (**columnMappings**) zugeordnet.

```json
{
    "name": "CopyActivity",
    "description": "description", 
    "type": "Copy",
    "inputs":  [ { "name": "AzureSQLInput"  } ],
    "outputs":  [ { "name": "AzureBlobOutput" } ],
    "typeProperties":    {
        "source":
        {
            "type": "SqlSource"
        },
        "sink":
        {
            "type": "BlobSink"
        },
        "translator": 
        {
            "type": "TabularTranslator",
            "ColumnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"
        }
    },
   "scheduler": {
          "frequency": "Hour",
          "interval": 1
        }
}
```
**Ablauf der Spaltenzuordnung:**

![Ablauf der Spaltenzuordnung](./media/data-factory-map-columns/column-mapping-flow.png)

## <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-to-azure-blob"></a>Beispiel 2 – Spaltenzuordnung mit SQL-Abfrage von Azure SQL zu Azure-Blob
In diesem Beispiel wird eine SQL-Abfrage zum Extrahieren von Daten aus SQL Azure verwendet, statt einfach den Tabellennamen und die Spaltennamen im Abschnitt "structure" anzugeben. 

```json
{
    "name": "CopyActivity",
    "description": "description", 
    "type": "CopyActivity",
    "inputs":  [ { "name": " AzureSQLInput"  } ],
    "outputs":  [ { "name": " AzureBlobOutput" } ],
    "typeProperties":
    {
        "source":
        {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartDateTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
        },
        "sink":
        {
            "type": "BlobSink"
        },
        "Translator": 
        {
            "type": "TabularTranslator",
            "ColumnMappings": "UserId: MyUserId, Group: MyGroup,Name: MyName"
        }
    },
    "scheduler": {
          "frequency": "Hour",
          "interval": 1
        }
}
```
In diesem Fall werden die Abfrageergebnisse zuerst den in "structure" der Quelle angegebenen Spalten zugeordnet. Anschließend werden die Spalten aus "structure" der Quelle mit Regeln, die in "columnMappings" angegeben sind, Spalten in "structure" der Senke zugeordnet.  Wenn die Abfrage fünf Spalten zurückgibt, sind dies zwei Spalten mehr, als in der „structure“ der Quelle angegeben sind.

**Ablauf der Spaltenzuordnung**

![Ablauf der Spaltenzuordnung-2](./media/data-factory-map-columns/column-mapping-flow-2.png)

## <a name="next-steps"></a>Nächste Schritte
Im folgenden Artikel finden Sie ein Tutorial zur Verwendung der Kopieraktivität: 

- [Kopieren von Daten aus Blob Storage in SQL-Datenbank](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)

