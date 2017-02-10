---
redirect_url: /azure/sql-data-warehouse/sql-data-warehouse-load-with-data-factory
title: Laden von Daten aus Azure Blob Storage in Azure SQL Data Warehouse (Azure Data Factory) | Microsoft Docs
description: Informationen zum Laden von Daten mit Azure Data Factory
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse
ms.assetid: 689fb02e-eb98-4f7c-81e6-6c1d22d53901
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 11/22/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: bf9ea4bd9fde1cc534c58e32706b659e5d89e4f7


---
# <a name="load-data-from-azure-blob-storage-into-azure-sql-data-warehouse-azure-data-factory"></a>Laden von Daten aus Azure Blob Storage in Azure SQL Data Warehouse (Azure Data Factory)
> [!div class="op_single_selector"]
> * [Data Factory](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
> * [PolyBase](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)
> 
> 

 In diesem Tutorial wird gezeigt, wie Sie eine Pipeline in Azure Data Factory erstellen, mit deren Hilfe Daten aus Azure Storage-Blobs in ein SQL Data Warehouse verschoben werden. Mit den folgenden Schritten können Sie folgende Aktionen ausführen:

* Einrichten der Beispieldaten in einem Azure Storage-Blob.
* Verbinden von Ressourcen mit Azure Data Factory.
* Erstellen einer Pipeline zum Verschieben von Daten aus Storage-Blobs in SQL Data Warehouse.

> [!VIDEO https://channel9.msdn.com/Blogs/Windows-Azure/Loading-Azure-SQL-Data-Warehouse-with-Azure-Data-Factory/player]
> 
> 

## <a name="before-you-begin"></a>Voraussetzungen
Machen Sie sich unter [Einführung in den Azure Data Factory-Dienst][Einführung in den Azure Data Factory-Dienst] mit Azure Data Factory vertraut.

### <a name="create-or-identify-resources"></a>Erstellen oder Identifizieren von Ressourcen
Bevor Sie mit diesem Tutorial beginnen, müssen Sie über die folgenden Ressourcen verfügen.

* **Azure Storage-Blob**: In diesem Tutorial wird Azure Storage-Blob als Datenquelle für die Azure Data Factory-Pipeline verwendet und ist daher zum Speichern der Beispieldaten erforderlich. [Erstellen Sie ein Speicherkonto][Erstellen Sie ein Speicherkonto], sofern Sie über keines verfügen.
* **SQL Data Warehouse**: In diesem Tutorial werden die Daten aus dem Azure Storage-Blob in SQL Data Warehouse verschoben. Daher muss ein Data Warehouse online sein, in das die AdventureWorksDW-Beispieldaten geladen wurden. Wenn Sie noch kein Data Warehouse haben, helfen Ihnen die Informationen zur [Bereitstellung][Erstellen eines SQL Data Warehouse] weiter. Wenn Sie ein Data Warehouse besitzen, es aber nicht mit Beispieldaten bereitgestellt haben, können Sie sie [manuell laden][Laden von Beispieldaten in SQL Data Warehouse].
* **Azure Data Factory**: Azure Data Factory schließt die tatsächliche Last ab und ist daher zum Erstellen der Datenverschiebungspipeline erforderlich. Das Erstellen (sofern erforderlich) ist in Schritt 1 von [Erste Schritte mit Azure Data Factory (Data Factory Edition)][Erste Schritte mit Azure Data Factory (Data Factory Edition)] beschrieben.
* **AZCopy**: Sie benötigen AZCopy, um die Beispieldaten von Ihrem lokalen Client in Ihr Azure Storage-Blob zu kopieren. Anweisungen zur Installation finden Sie in der [AZCopy-Dokumentation][AZCopy-Dokumentation].

## <a name="step-1-copy-sample-data-to-azure-storage-blob"></a>Schritt 1: Kopieren von Beispieldaten in Azure Storage-Blob
Sobald alle Bestandteile bereitstehen, können Sie Beispieldaten in Ihr Azure Storage-Blob kopieren.

1. [Laden Sie Beispieldaten herunter][Laden Sie Beispieldaten herunter]. Mit diesen Daten werden Ihren AdventureWorksDW-Beispieldaten Umsatzdaten aus drei weiteren Jahren hinzugefügt.
2. Verwenden Sie diesen AZCopy-Befehl, um die Daten aus drei Jahren in Ihr Azure Storage-Blob zu kopieren.

````
AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
````


## <a name="step-2-connect-resources-to-azure-data-factory"></a>Schritt 2: Verbinden von Ressourcen mit Azure Data Factory
Da die Daten jetzt vorhanden ist, können wir die Azure Data Factory-Pipeline zum Verschieben der Daten aus dem Azure-Blobspeicher in SQL Data Warehouse erstellen.

Öffnen Sie zuerst das [Azure-Portal][Azure-Portal], und wählen Sie im Menü auf der linken Seite Ihre Data Factory aus.

### <a name="step-21-create-linked-service"></a>Schritt 2.1: Erstellen eines verknüpften Dienstes
Verknüpfen Sie Ihr Azure-Speicherkonto und SQL Data Warehouse mit Ihrer Data Factory.  

1. Starten Sie zunächst den Registrierungsvorgang, indem Sie auf den Abschnitt „Verknüpfte Dienste“ Ihrer Data Factory und dann auf „Neuer Datenspeicher“ klicken. Wählen Sie einen Namen aus, unter dem Sie Ihren Azure-Speicher registrieren möchten, wählen Sie Azure Storage als Typ aus, und geben Sie dann Ihren Kontonamen und Kontoschlüssel ein.
2. Zum Registrieren von SQL Data Warehouse wechseln Sie zum Abschnitt „Erstellen und Bereitstellen“ und wählen dort „Neuer Datenspeicher“ und dann „SQL Azure Data Warehouse“ aus. Kopieren Sie die Informationen, und fügen Sie sie in diese Vorlage ein. Geben Sie anschließend Ihre jeweiligen Informationen ein.

```JSON
{
    "name": "<Linked Service Name>",
    "properties": {
        "description": "",
        "type": "AzureSqlDW",
        "typeProperties": {
             "connectionString": "Data Source=tcp:<server name>.database.windows.net,1433;Initial Catalog=<server name>;Integrated Security=False;User ID=<user>@<servername>;Password=<password>;Connect Timeout=30;Encrypt=True"
         }
    }
}
```

### <a name="step-22-define-the-dataset"></a>Schritt 2.2: Definieren des Datasets
Nach dem Erstellen der verknüpften Dienste müssen Sie die Datasets definieren.  Dies bedeutet in diesem Fall, dass die Struktur der Daten definiert wird, die aus dem Speicher in das Data Warehouse verschoben werden.  Informationen zur Erstellung

1. Um diesen Prozess zu starten, wechseln Sie zum Abschnitt „Erstellen und Bereitstellen“ Ihrer Data Factory.
2. Klicken Sie auf „Neues Dataset“ und dann auf „Azure-Blobspeicher“, um Ihren Speicher mit Ihrer Data Factory zu verknüpfen.  Mit dem unten aufgeführten Skript können Sie Daten im Azure-Blobspeicher definieren:

```JSON
{
    "name": "<Dataset Name>",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "<linked storage name>",
        "typeProperties": {
            "folderPath": "<containter name>",
            "fileName": "FactInternetSales.csv",
            "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": "\n"
            }
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


1. Jetzt definieren wir auch unser Dataset für SQL Data Warehouse.  Wir beginnen auf die gleiche Weise, indem wir auf „Neues Dataset“ und dann auf „Azure SQL Data Warehouse“ klicken.

```JSON
{
    "name": "DWDataset",
    "properties": {
        "type": "AzureSqlDWTable",
        "linkedServiceName": "AzureSqlDWLinkedService",
        "typeProperties": {
            "tableName": "FactInternetSales"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

## <a name="step-3-create-and-run-your-pipeline"></a>Schritt 3: Erstellen und Ausführen der Pipeline
Zuletzt richten wir die Pipeline in Azure Data Factory ein und führen sie aus.  Dies ist der Vorgang, durch den die eigentliche Datenverschiebung abgeschlossen wird.  Eine vollständige Übersicht über die Vorgänge, die Sie mit SQL Data Warehouse und Azure Data Factory durchführen können, finden Sie [hier][Verschieben von Daten in und aus Azure SQL Data Warehouse mithilfe von Azure Data Factory].

Klicken Sie im Abschnitt „Erstellen und Bereitstellen“ nun auf „Weitere Befehle“ und dann auf „Neue Pipeline“.  Nachdem Sie die Pipeline erstellt haben, können Sie die Daten mit dem folgenden Code in das Data Warehouse übertragen:

```JSON
{
    "name": "<Pipeline Name>",
    "properties": {
        "description": "<Description>",
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "skipHeaderLineCount": 1
                },
                "sink": {
                    "type": "SqlDWSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:10"
                }
            },
            "inputs": [
              {
                "name": "<Storage Dataset>"
              }
            ],
            "outputs": [
              {
                "name": "<Data Warehouse Dataset>"
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
            "name": "Sample Copy",
            "description": "Copy Activity"
          }
        ],
        "start": "<Date YYYY-MM-DD>",
        "end": "<Date YYYY-MM-DD>",
        "isPaused": false
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter:

* [Lernpfad für Azure Data Factory][Lernpfad für Azure Data Factory].
* [Azure SQL Data Warehouse-Connector][Azure SQL Data Warehouse-Connector]. Dies ist das wichtigste Referenzthema zur Verwendung von Azure Data Factory mit Azure SQL Data Warehouse.

Die Themen enthalten ausführliche Informationen zu Azure Data Factory. Darin werden Azure SQL-Datenbank oder HDinsight behandelt, die Informationen gelten jedoch auch für Azure SQL Data Warehouse.

* [Tutorial: Erstellen der ersten Data Factory (Übersicht)][Tutorial: Erstellen der ersten Data Factory (Übersicht)]. Dies ist das wichtigste Tutorial zur Datenverarbeitung mit Azure Data Factory. In diesem Tutorial erstellen Sie Ihre erste Pipeline, die mithilfe von HDInsight monatlich Webprotokolle transformiert und analysiert. Beachten Sie, dass in diesem Tutorial keine Kopieraktivität erfolgt.
* [Tutorial: Kopieren von Daten aus Azure Blob Storage in Azure SQL-Datenbank][Tutorial: Kopieren von Daten aus Azure Blob Storage in Azure SQL-Datenbank]. In diesem Tutorial erstellen Sie in Azure Data Factory eine Pipeline, um Daten aus einem Azure Storage-Blob in Azure SQL-Datenbank zu kopieren.

<!--Image references-->

<!--Article references-->
[AZCopy-Dokumentation]: ../storage/storage-use-azcopy.md
[Azure SQL Data Warehouse-Connector]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[BCP]: sql-data-warehouse-load-with-bcp.md
[Erstellen eines SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Erstellen Sie ein Speicherkonto]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Erste Schritte mit Azure Data Factory (Data Factory Edition)]: ../data-factory/data-factory-build-your-first-pipeline-using-editor.md
[Einführung in den Azure Data Factory-Dienst]: ../data-factory/data-factory-introduction.md
[Laden von Beispieldaten in SQL Data Warehouse]: sql-data-warehouse-load-sample-databases.md
[Verschieben von Daten in und aus Azure SQL Data Warehouse mithilfe von Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md
[Tutorial: Kopieren von Daten aus Azure Blob Storage in Azure SQL-Datenbank]: ../data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[Tutorial: Erstellen der ersten Data Factory (Übersicht)]: ../data-factory/data-factory-build-your-first-pipeline.md

<!--MSDN references-->

<!--Other Web references-->
[Lernpfad für Azure Data Factory]: https://azure.microsoft.com/documentation/learning-paths/data-factory
[Azure-Portal]: https://portal.azure.com
[Laden Sie Beispieldaten herunter]: https://migrhoststorage.blob.core.windows.net/adfsample/FactInternetSales.csv



<!--HONumber=Nov16_HO3-->


