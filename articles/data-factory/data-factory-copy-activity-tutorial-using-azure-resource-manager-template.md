---
title: 'Tutorial: Erstellen einer Pipeline mithilfe einer Resource Manager-Vorlage | Microsoft-Dokumentation'
description: "In diesem Tutorial erstellen Sie mithilfe einer Azure Resource Manager-Vorlage eine Azure Data Factory-Pipeline mit einer Kopieraktivität."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1274e11a-e004-4df5-af07-850b2de7c15e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/10/2016
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 535eb3c22dad35da3c1dbc10be5a7c11c6bb8d00


---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-azure-resource-manager-template"></a>Tutorial: Erstellen einer Pipeline mit Kopieraktivität mithilfe einer Azure Resource Manager-Vorlage
> [!div class="op_single_selector"]
> * [Übersicht und Voraussetzungen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Kopier-Assistent](data-factory-copy-data-wizard-tutorial.md)
> * [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Azure Resource Manager-Vorlage](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST-API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

In diesem Tutorial erfahren Sie, wie Sie mithilfe einer Azure Resource Manager-Vorlage eine Azure Data Factory-Instanz erstellen und überwachen. Die Pipeline in der Data Factory kopiert Daten aus Azure Blob Storage in Azure SQL-Datenbank.

## <a name="prerequisites"></a>Voraussetzungen
* Lesen Sie sich die [Tutorialübersicht und die Voraussetzungen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) durch, und führen Sie die Schritte zur Erfüllung der **Voraussetzungen** aus.
* Befolgen Sie die Anweisungen im Artikel [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) zum Installieren der neuesten Version von Azure PowerShell auf Ihrem Computer. In diesem Tutorial stellen Sie Data Factory-Entitäten mithilfe von PowerShell bereit. 
* Optional: Informationen zu Azure Resource Manager-Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](../resource-group-authoring-templates.md).

## <a name="in-this-tutorial"></a>Dieses Lernprogramm umfasst folgende Punkte
In diesem Tutorial erstellen Sie eine Data Factory mit den folgenden Data Factory-Entitäten:

| Entität | Beschreibung |
| --- | --- |
| Mit Azure Storage verknüpfter Dienst |Verknüpft Ihr Azure Storage-Konto mit der Data Factory. Azure Storage ist der Quelldatenspeicher für die Kopieraktivität des Tutorials, Azure SQL-Datenbank der Senkendatenspeicher. Der Dienst gibt das Speicherkonto an, das die Eingabedaten für die Kopieraktivität enthält. |
| Mit Azure SQL-Datenbank verknüpfter Dienst |Verknüpft Azure SQL-Datenbank mit der Data Factory. Der Dienst gibt die Azure SQL-Datenbank-Instanz zum Speichern der Ausgabedaten für die Kopieraktivität an. |
| Azure-Blob-Eingabedataset |Verweist auf den mit Azure Storage verknüpften Dienst. Der verknüpfte Dienst verweist auf ein Azure Storage-Konto, und das Azure-Blob-Dataset gibt den Container, Ordner und Dateinamen in dem Speicher an, der die Eingabedaten enthält. |
| Azure SQL-Ausgabedataset |Verweist auf den mit Azure SQL verknüpften Dienst. Der mit Azure SQL verknüpfte Dienst verweist auf eine Azure SQL Server-Instanz, und das Azure SQL-Dataset gibt den Namen der Tabelle mit den Ausgabedaten an. |
| Datenpipeline |Die Pipeline verfügt über eine einzelne Kopieraktivität mit dem Azure-Blob-Dataset als Eingabe und dem Azure SQL-Dataset als Ausgabe. Die Kopieraktivität kopiert Daten aus einem Azure-Blob in eine Tabelle in Azure SQL-Datenbank. |

Eine Data Factory kann eine oder mehrere Aktivitäten aufweisen. Eine Pipeline kann eine oder mehrere Aktivitäten aufweisen. Es gibt zwei Arten von Aktivitäten: [Datenverschiebungen](data-factory-data-movement-activities.md) und [Datentransformationen](data-factory-data-transformation-activities.md). In diesem Tutorial erstellen Sie eine Pipeline mit einer einzelnen Kopieraktivität.

![Kopieren von Daten aus dem Azure-Blob in Azure SQL-Datenbank](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/CopyBlob2SqlDiagram.png) 

Der folgende Abschnitt enthält die vollständige Resource Manager-Vorlage zum Definieren von Data Factory-Entitäten, damit Sie das Tutorial schnell durchlaufen und die Vorlage testen können. Informationen zum Definieren der einzelnen Data Factory-Entitäten finden Sie im Abschnitt [Data Factory-Entitäten in der Vorlage](#data-factory-entities-in-the-template).

## <a name="data-factory-json-template"></a>Data Factory-JSON-Vorlage
Das Grundgerüst einer Resource Manager-Vorlage zum Definieren einer Data Factory sieht wie folgt aus: 

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": { ...
        },
        "variables": { ...
        },
        "resources": [
            {
                "name": "[parameters('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "westus",
                "resources": [
                    { ... },
                    { ... },
                    { ... },
                    { ... }
                ]
            }
        ]
    }

Erstellen Sie im Ordner **C:\ADFGetStarted** eine JSON-Datei namens **ADFCopyTutorialARM.json** mit folgendem Inhalt:

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
          "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the data to be copied." } },
          "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
          "sourceBlobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
          "sourceBlobName": { "type": "string", "metadata": { "description": "Name of the blob in the container that has the data to be copied to Azure SQL Database table" } },
          "sqlServerName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Server that will hold the output/copied data." } },
          "databaseName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Database in the Azure SQL server." } },
          "sqlServerUserName": { "type": "string", "metadata": { "description": "Name of the user that has access to the Azure SQL server." } },
          "sqlServerPassword": { "type": "securestring", "metadata": { "description": "Password for the user." } },
          "targetSQLTable": { "type": "string", "metadata": { "description": "Table in the Azure SQL Database that will hold the copied data." } 
          } 
        },
        "variables": {
          "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]",
          "azureSqlLinkedServiceName": "AzureSqlLinkedService",
          "azureStorageLinkedServiceName": "AzureStorageLinkedService",
          "blobInputDatasetName": "BlobInputDataset",
          "sqlOutputDatasetName": "SQLOutputDataset",
          "pipelineName": "Blob2SQLPipeline"
        },
        "resources": [
          {
            "name": "[variables('dataFactoryName')]",
            "apiVersion": "2015-10-01",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "West US",
            "resources": [
              {
                "type": "linkedservices",
                "name": "[variables('azureStorageLinkedServiceName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureStorage",
                  "description": "Azure Storage linked service",
                  "typeProperties": {
                    "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
                  }
                }
              },
              {
                "type": "linkedservices",
                "name": "[variables('azureSqlLinkedServiceName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureSqlDatabase",
                  "description": "Azure SQL linked service",
                  "typeProperties": {
                    "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
                  }
                }
              },
              {
                "type": "datasets",
                "name": "[variables('blobInputDatasetName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureBlob",
                  "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                  "structure": [
                    {
                      "name": "Column0",
                      "type": "String"
                    },
                    {
                      "name": "Column1",
                      "type": "String"
                    }
                  ],
                  "typeProperties": {
                    "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                    "fileName": "[parameters('sourceBlobName')]",
                    "format": {
                      "type": "TextFormat",
                      "columnDelimiter": ","
                    }
                  },
                  "availability": {
                    "frequency": "Day",
                    "interval": 1
                  },
                  "external": true
                }
              },
              {
                "type": "datasets",
                "name": "[variables('sqlOutputDatasetName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureSqlLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureSqlTable",
                  "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
                  "structure": [
                    {
                      "name": "FirstName",
                      "type": "String"
                    },
                    {
                      "name": "LastName",
                      "type": "String"
                    }
                  ],
                  "typeProperties": {
                    "tableName": "[parameters('targetSQLTable')]"
                  },
                  "availability": {
                    "frequency": "Day",
                    "interval": 1
                  }
                }
              },
              {
                "type": "datapipelines",
                "name": "[variables('pipelineName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureStorageLinkedServiceName')]",
                  "[variables('azureSqlLinkedServiceName')]",
                  "[variables('blobInputDatasetName')]",
                  "[variables('sqlOutputDatasetName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "activities": [
                    {
                      "name": "CopyFromAzureBlobToAzureSQL",
                      "description": "Copy data frm Azure blob to Azure SQL",
                      "type": "Copy",
                      "inputs": [
                        {
                          "name": "[variables('blobInputDatasetName')]"
                        }
                      ],
                      "outputs": [
                        {
                          "name": "[variables('sqlOutputDatasetName')]"
                        }
                      ],
                      "typeProperties": {
                        "source": {
                          "type": "BlobSource"
                        },
                        "sink": {
                          "type": "SqlSink",
                          "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                        },
                        "translator": {
                          "type": "TabularTranslator",
                          "columnMappings": "Column0:FirstName,Column1:LastName"
                        }
                      },
                      "Policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 3,
                        "timeout": "01:00:00"
                      }
                    }
                  ],
                  "start": "2016-10-02T00:00:00Z",
                  "end": "2016-10-03T00:00:00Z"
                }
              }
            ]
          }
        ]
      }

## <a name="parameters-json"></a>Parameter (JSON)
Erstellen Sie eine JSON-Datei namens **ADFCopyTutorialARM-Parameters.json** mit Parametern für die Azure Resource Manager-Vorlage. 

> [!IMPORTANT]
> Geben Sie für **storageaccountname** und **accountkey** den Namen und den Schlüssel Ihres Azure Storage-Kontos an.  
> 
> 

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": { 
            "storageAccountName": {    "value": "<Name of the Azure storage account>"    },
            "storageAccountKey": {
                     "value": "<Key for the Azure storage account>"
            },
            "sourceBlobContainer": { "value": "adftutorial" },
            "sourceBlobName": { "value": "emp.txt" },
            "sqlServerName": { "value": "<Name of the Azure SQL server>" },
            "databaseName": { "value": "<Name of the Azure SQL database>" },
            "sqlServerUserName": { "value": "<Name of the user who has access to the Azure SQL database>" },
            "sqlServerPassword": { "value": "<password for the user>" },
            "targetSQLTable": { "value": "emp" }
        }
    }

> [!IMPORTANT]
> Für die Entwicklungs-, die Test- und die Produktionsumgebung können jeweils separate JSON-Parameterdateien mit der gleichen Data Factory-JSON-Vorlage verwendet werden. Mithilfe eines PowerShell-Skripts können Sie die Bereitstellung von Data Factory-Entitäten in diesen Umgebungen automatisieren.  
> 
> 

## <a name="create-data-factory"></a>Erstellen einer Data Factory
1. Starten Sie **Azure PowerShell** , und führen Sie den folgenden Befehl aus:
   * Führen Sie `Login-AzureRmAccount` aus, und geben Sie den Benutzernamen und das Kennwort ein, den bzw. das Sie bei der Anmeldung beim Azure-Portal verwendet haben.  
   * Führen Sie `Get-AzureRmSubscription` aus, um alle Abonnements für dieses Konto anzuzeigen.
   * Führen Sie `Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext` aus, um das Abonnement auszuwählen, das Sie verwenden möchten. 
2. Führen Sie den folgenden Befehl aus, um die Data Factory-Entitäten bereitzustellen, die Sie in Schritt 1 mit der Resource Manager-Vorlage erstellt haben.
   
        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFCopyTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFCopyTutorialARM-Parameters.json

## <a name="monitor-pipeline"></a>Überwachen der Pipeline
1. Melden Sie sich mit Ihrem Azure-Konto beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie im linken Menü auf **Data Factorys**, oder klicken Sie auf **Weitere Dienste** und anschließend unter der Kategorie **INTELLIGENCE + ANALYSE** auf **Data Factorys**.
   
    ![Menü „Data Factorys“](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factories-menu.png)
3. Suchen Sie auf der Seite **Data Factorys** nach Ihrer Data Factory. 
   
    ![Suchen nach der Data Factory](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/search-for-data-factory.png)  
4. Klicken Sie auf Ihre Data Factory. Die Startseite für die Data Factory erscheint.
   
    ![Startseite für die Data Factory](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-home-page.png)  
5. Klicken Sie auf die Kachel **Diagramm**, um die Diagrammansicht Ihrer Data Factory anzuzeigen.
   
    ![Diagrammansicht der Data Factory](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-diagram-view.png)
6. Doppelklicken Sie in der Diagrammansicht auf das Dataset **SQLOutputDataset**. Der Status des Slice wird angezeigt. Nach Abschluss des Kopiervorgangs lautet der Status **Bereit**.
   
    ![Ausgabeslice im Zustand „Bereit“](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/output-slice-ready.png)
7. Wenn sich der Slice im Zustand **Bereit** befindet, vergewissern Sie sich, dass die Daten in die Tabelle **emp** in Azure SQL-Datenbank kopiert wurden.

Unter [Überwachen von Datasets und Pipelines](data-factory-monitor-manage-pipelines.md) finden Sie eine Anleitung zum Überwachen der in diesem Tutorial erstellten Pipeline und Datasets über die Blätter im Azure-Portal.

Sie können auch die App „Überwachung und Verwaltung“ verwenden, um Ihre Datenpipelines zu überwachen. Einzelheiten zur Verwendung der Anwendung finden Sie unter [Überwachen und Verwalten von Azure Data Factory-Pipelines mit der neuen App „Überwachung und Verwaltung“](data-factory-monitor-manage-app.md) .

## <a name="data-factory-entities-in-the-template"></a>Data Factory-Entitäten in der Vorlage
### <a name="define-data-factory"></a>Definieren einer Data Factory
Das folgende Beispiel zeigt, wie Sie in der Resource Manager-Vorlage eine Data Factory definieren:  

    "resources": [
    {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US"
    }

„dataFactoryName“ wird wie folgt definiert: 

    "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]"

Hierbei handelt es sich um eine eindeutige Zeichenfolge auf der Grundlage der Ressourcengruppen-ID.  

### <a name="defining-data-factory-entities"></a>Definieren von Data Factory-Entitäten
In der JSON-Vorlage werden folgende Data Factory-Entitäten definiert: 

1. [Mit Azure Storage verknüpfter Dienst](#azure-storage-linked-service)
2. [Mit Azure SQL verknüpfter Dienst](#azure-sql-database-linked-service)
3. [Azure-Blobdataset](#azure-blob-dataset)
4. [Azure SQL-Dataset](#azure-sql-dataset)
5. [Datenpipeline mit einer Kopieraktivität](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Mit Azure Storage verknüpfter Dienst
In diesem Abschnitt geben Sie Name und Schlüssel Ihres Azure Storage-Kontos an. Informationen zu JSON-Eigenschaften zum Definieren eines mit Azure Storage verknüpften Diensts finden Sie unter [Mit Azure Storage verknüpfter Dienst](data-factory-azure-blob-connector.md#azure-storage-linked-service). 

    {
        "type": "linkedservices",
        "name": "[variables('azureStorageLinkedServiceName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureStorage",
            "description": "Azure Storage linked service",
            "typeProperties": {
                "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
            }
        }
    }

Die Verbindungszeichenfolge (connectionString) enthält die Parameter „storageAccountName“ und „storageAccountKey“. Die Werte für diese Parameter werden mithilfe einer Konfigurationsdatei übergeben. In der Definition werden außerdem die in der Vorlage definierten Variablen „azureStorageLinkedService“ und „dataFactoryName“ verwendet. 

#### <a name="azure-sql-database-linked-service"></a>Mit Azure SQL-Datenbank verknüpfter Dienst
In diesem Abschnitt geben Sie den Namen der Azure SQL Server-Instanz, den Datenbanknamen, den Benutzernamen und das Benutzerkennwort an. Informationen zu JSON-Eigenschaften zum Definieren eines mit Azure SQL verknüpften Diensts finden Sie unter [Mit Azure SQL verknüpfter Dienst](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties).  

    {
        "type": "linkedservices",
        "name": "[variables('azureSqlLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
              "type": "AzureSqlDatabase",
              "description": "Azure SQL linked service",
              "typeProperties": {
                "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
              }
        }
    }

Die Verbindungszeichenfolge (connectionString) enthält die Parameter „sqlServerName“, „databaseName“, „sqlServerUserName“ und „sqlServerPassword“, deren Werte mithilfe einer Konfigurationsdatei übergeben werden. In der Definition werden außerdem folgende Variablen aus der Vorlage verwendet: „azureSqlLinkedServiceName“ und „dataFactoryName“.

#### <a name="azure-blob-dataset"></a>Azure-Blobdataset
In diesem Abschnitt geben Sie den Blobcontainer, den Ordner und die Datei mit den Eingabedaten an. Informationen zu JSON-Eigenschaften zum Definieren eines Azure-Blobdatasets finden Sie unter [Eigenschaften des Dataset-Typs „Azure-Blob“](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 

    {
        "type": "datasets",
        "name": "[variables('blobInputDatasetName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureBlob",
              "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
            "structure": [
            {
                  "name": "Column0",
                  "type": "String"
            },
            {
                  "name": "Column1",
                  "type": "String"
            }
              ],
              "typeProperties": {
                "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                "fileName": "[parameters('sourceBlobName')]",
                "format": {
                      "type": "TextFormat",
                      "columnDelimiter": ","
                }
              },
              "availability": {
                "frequency": "Day",
                "interval": 1
              },
              "external": true
        }
    }

#### <a name="azure-sql-dataset"></a>Azure SQL-Dataset
In diesem Abschnitt geben Sie den Namen der Tabelle in Azure SQL-Datenbank an, die die kopierten Daten aus Azure Blob Storage enthält. Informationen zu JSON-Eigenschaften zum Definieren eines Azure SQL-Datasets finden Sie unter [Eigenschaften des Dataset-Typs „Azure SQL“](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties). 

    {
        "type": "datasets",
        "name": "[variables('sqlOutputDatasetName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]",
              "[variables('azureSqlLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
              "type": "AzureSqlTable",
              "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
              "structure": [
            {
                  "name": "FirstName",
                  "type": "String"
            },
            {
                  "name": "LastName",
                  "type": "String"
            }
              ],
              "typeProperties": {
                "tableName": "[parameters('targetSQLTable')]"
              },
              "availability": {
                "frequency": "Day",
                "interval": 1
              }
        }
    }

#### <a name="data-pipeline"></a>Datenpipeline
In diesem Abschnitt definieren Sie eine Pipeline, die Daten aus dem Azure-Blobdataset in das Azure SQL-Dataset kopiert. Informationen zu JSON-Elementen zum Definieren einer Pipeline für dieses Beispiel finden Sie unter [Pipeline-JSON](data-factory-create-pipelines.md#pipeline-json). 

    {
        "type": "datapipelines",
        "name": "[variables('pipelineName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]",
              "[variables('azureStorageLinkedServiceName')]",
              "[variables('azureSqlLinkedServiceName')]",
              "[variables('blobInputDatasetName')]",
              "[variables('sqlOutputDatasetName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
              "activities": [
            {
                  "name": "CopyFromAzureBlobToAzureSQL",
                  "description": "Copy data frm Azure blob to Azure SQL",
                  "type": "Copy",
                  "inputs": [
                {
                      "name": "[variables('blobInputDatasetName')]"
                }
                  ],
                  "outputs": [
                {
                      "name": "[variables('sqlOutputDatasetName')]"
                }
                  ],
                  "typeProperties": {
                    "source": {
                          "type": "BlobSource"
                    },
                    "sink": {
                          "type": "SqlSink",
                          "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                    },
                    "translator": {
                          "type": "TabularTranslator",
                          "columnMappings": "Column0:FirstName,Column1:LastName"
                    }
                  },
                  "Policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 3,
                    "timeout": "01:00:00"
                  }
            }
              ],
              "start": "2016-10-02T00:00:00Z",
              "end": "2016-10-03T00:00:00Z"
        }
    }

## <a name="reuse-the-template"></a>Wiederverwenden der Vorlage
In diesem Tutorial haben Sie eine Vorlage zum Definieren von Data Factory-Entitäten und eine Vorlage zum Übergeben von Parameterwerten erstellt. Die Pipeline kopiert Daten aus einem Azure Storage-Konto in eine mithilfe von Parametern angegebene Azure SQL-Datenbank-Instanz. Mit der gleichen Vorlage können Sie Data Factory-Entitäten in unterschiedlichen Umgebungen bereitstellen. Hierzu müssen Sie lediglich eine Parameterdatei für die jeweilige Umgebung erstellen und beim Bereitstellen in dieser Umgebung verwenden.     

Beispiel:  

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Dev.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Test.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Production.json

Der erste Befehl verwendet eine Parameterdatei für die Entwicklungsumgebung, der zweite eine Parameterdatei für die Testumgebung und der dritte eine Parameterdatei für die Produktion.  

Mit der Vorlage können auch wiederholte Aufgaben durchgeführt werden. Angenommen, Sie müssen zahlreiche Data Factorys mit einer Pipeline oder mehreren Pipelines erstellen, die die gleiche Logik implementieren, die einzelnen Data Factorys verwenden jedoch jeweils ein anderes Azure Storage- und Azure SQL-Datenbank-Konto. In diesem Szenario können Sie zur Erstellung der Data Factorys die gleiche Vorlage in der gleichen Umgebung (Entwicklungs-, Test- oder Produktionsumgebung) mit unterschiedlichen Parameterdateien Data Factorys verwenden.   




<!--HONumber=Nov16_HO2-->


