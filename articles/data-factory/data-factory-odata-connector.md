---
title: Verschieben von Daten aus OData-Quellen | Microsoft Docs
description: Erfahren Sie, wie Sie Daten aus OData-Quellen mithilfe von Azure Data Factory verschieben.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: de28fa56-3204-4546-a4df-21a21de43ed7
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: 4521a236bfc13e6aca7e13e7400c11d353bc3a66
ms.openlocfilehash: 9c385adfa3da73bef2d05352049d1f71aa5c5847


---
# <a name="move-data-from-a-odata-source-using-azure-data-factory"></a>Verschieben von Daten aus einer OData-Quelle mithilfe von Azure Data Factory
Dieser Artikel beschreibt, wie Sie die Kopieraktivität in einer Azure Data Factory verwenden können, um Daten aus einer OData-Quelle in einen anderen Datenspeicher zu verschieben. Dieser Artikel baut auf dem Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) auf, der eine allgemeine Übersicht zur Datenverschiebung mit Kopieraktivität und unterstützten Datenspeicherkombinationen bietet.

## <a name="supported-versions-and-authentication-types"></a>Unterstützte Versionen und Authentifizierungstypen
Dieser OData-Connector unterstützt OData Version 3.0 and 4.0 und das Kopieren von Daten aus lokalen und cloudbasierten OData-Quellen. In letzterem Fall müssen Sie möglicherweise das Datenverwaltungsgateway installieren. Weitere Informationen zum Datenverwaltungsgateway finden Sie im Artikel [Verschieben von Daten zwischen lokalen Quellen und der Cloud](data-factory-move-data-between-onprem-and-cloud.md) .

Die folgenden Authentifizierungstypen werden unterstützt:

* Um auf den **cloudbasierten** OData-Feed zuzugreifen, können Sie die anonyme, einfache (Benutzername und Kennwort) oder die Azure Active Directory-basierte OAuth-Authentifizierung verwenden.
* Um auf den **lokalen** OData-Feed zuzugreifen, können Sie die anonyme, einfache (Benutzername und Kennwort) oder die Windows-Authentifizierung verwenden.

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Die einfachste Möglichkeit zum Erstellen einer Pipeline, die Daten aus einer OData-Quelle kopiert, ist die Verwendung des Assistenten zum Kopieren von Daten. Unter [Tutorial: Erstellen einer Pipeline mit dem Assistenten zum Kopieren](data-factory-copy-data-wizard-tutorial.md) finden Sie eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten.

Die folgenden Beispiele zeigen JSON-Beispieldefinitionen, die Sie zum Erstellen einer Pipeline mit dem [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md), mit [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) verwenden können. Darin wird veranschaulicht, wie Sie Daten aus einer OData-Quelle in Azure Blob Storage kopieren. Daten können jedoch auch mithilfe der Kopieraktivität in Azure Data Factory in eine beliebige der [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) aufgeführten Senken kopiert werden.

## <a name="sample-copy-data-from-odata-source-to-azure-blob"></a>Beispiel: Kopieren von Daten aus einer OData-Quelle in Azure Blob
In diesem Beispiel wird gezeigt, wie Sie Daten aus einer OData-Quelle in Azure Blob Storage kopieren. Daten können jedoch mithilfe der Kopieraktivität in Azure Data Factory **direkt** in die [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) aufgeführten Senken kopiert werden.  

Das Beispiel enthält die folgenden Data Factory-Entitäten:

1. Einen verknüpften Dienst des Typs [OData](#odata-linked-service-properties)
2. Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service)
3. Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [ODataResource](#odata-dataset-type-properties)
4. Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)
5. Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [RelationalSource](#odata-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) verwendet

Das Beispiel kopiert stündlich Daten durch Abfragen einer OData-Quelle in ein Azure-Blob. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

**Mit OData verknüpfter Dienst**. Bei diesem Beispiel wird die anonyma Authentifizierung verwendet. Informationen zu den verwendbaren Authentifizierungstypen finden Sie im Abschnitt [Mit OData verknüpfter Dienst](#odata-linked-service-properties).

    {
        "name": "ODataLinkedService",
           "properties":
        {
            "type": "OData",
               "typeProperties":
            {
                "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Anonymous"
               }
           }
    }


**Mit Azure Storage verknüpfter Dienst**

    {
          "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
              "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
          }
    }

**OData-Eingabedataset:**

Durch Festlegen von „external“ auf „true“ wird dem Data Factory-Dienst mitgeteilt, dass das Dataset für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

    {
        "name": "ODataDataset",
        "properties":
        {
            "type": "ODataResource",
            "typeProperties":
            {
                 "path": "Products"
            },
            "linkedServiceName": "ODataLinkedService",
            "structure": [],
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3                
            }
        }
    }

Die Angabe von **path** in der Datasetdefinition ist optional.

**Azure-Blob-Ausgabedataset**

Daten werden stündlich in ein neues Blob geschrieben ("frequency": "hour", "interval": 1). Der Ordnerpfad des Blobs wird basierend auf der Startzeit des Slices, der verarbeitet wird, dynamisch ausgewertet. Im Ordnerpfad werden Jahr, Monat, Tag und die Stundenteile der Startzeit verwendet.

    {
        "name": "AzureBlobODataDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/odata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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



**Pipeline mit Kopieraktivität**

Die Pipeline enthält eine Kopieraktivität, die für die Verwendung der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **RelationalSource** und der Typ **sink** auf **BlobSink** festgelegt. Die SQL-Abfrage für die **query** -Eigenschaft wählt die letzten (neuesten) Daten aus der OData-Quelle aus.

    {
        "name": "CopyODataToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "?$select=Name, Description&$top=5",
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "ODataDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobODataDataSet"
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
                    "name": "ODataToBlob"
                }
            ],
            "start": "2016-02-01T18:00:00Z",
            "end": "2016-02-03T19:00:00Z"
        }
    }


Die Angabe von **query** in der Pipelinedefinition ist optional. Die **URL** , die der Data Factory-Dienst zum Abrufen der Daten verwendet, ergibt sich aus: URL, die im verknüpften Dienst (erforderlich) angegeben wurde + im Dataset (optional) angegebener Pfad + Abfrage in der Pipeline (optional).

## <a name="odata-linked-service-properties"></a>Eigenschaften des mit OData verknüpften Diensts
Die folgende Tabelle enthält eine Beschreibung der JSON-Elemente, die für den mit OData verknüpften Dienst spezifisch sind.

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| Typ |Die type-Eigenschaft muss auf **OData** |Ja |
| URL |Die URL des OData-Diensts. |Ja |
| authenticationType |Typ der Authentifizierung für die Verbindung mit der OData-Quelle. <br/><br/> Mögliche Werte für den cloudbasierten OData-Dienst sind „Anonymous“, „Basic“ und „OAuth“ (beachten Sie, dass Azure Data Factory derzeit nur Azure Active Directory-basiertes OAuth unterstützt). <br/><br/> Mögliche Werte für lokales OData sind „Anonymous“, „Basic“ und „Windows“. |Ja |
| username |Geben Sie den Benutzernamen an, wenn Sie die Standardauthentifizierung (Basic) verwenden. |Ja (nur bei Verwendung der Standardauthentifizierung) |
| password |Geben Sie das Kennwort für das Benutzerkonto an, das Sie für den Benutzernamen angegeben haben. |Ja (nur bei Verwendung der Standardauthentifizierung) |
| authorizedCredential |Klicken Sie, wenn Sie OAuth verwenden, im Assistenten zum Kopieren in Data Factory bzw. im Editor auf die Schaltfläche **Autorisieren**, und geben Sie Ihre Anmeldeinformationen ein. Anschließend wird der Wert dieser Eigenschaft automatisch generiert. |Ja (nur bei Verwendung der OAuth-Authentifizierung) |
| gatewayName |Der Name des Gateways, das der Data Factory-Dienst zum Verbinden mit dem lokalen OData-Dienst verwenden soll. Geben Sie diesen nur an, wenn Sie Daten aus einer lokalen OData-Quelle kopieren. |Nein |

### <a name="using-basic-authentication"></a>Verwenden der Standardauthentifizierung
    {
        "name": "inputLinkedService",
        "properties":
        {
            "type": "OData",
               "typeProperties":
            {
               "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Basic",
               "username": "username",
               "password": "password"
           }
       }
    }

### <a name="using-anonymous-authentication"></a>Verwenden der anonymen Authentifizierung
    {
        "name": "ODataLinkedService",
           "properties":
        {
            "type": "OData",
            "typeProperties":
            {
               "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Anonymous"
           }
       }
    }

### <a name="using-windows-authentication-accessing-on-premises-odata-source"></a>Verwenden der Windows-Authentifizierung für den Zugriff auf lokale OData-Quellen
    {
        "name": "inputLinkedService",
        "properties":
        {
            "type": "OData",
               "typeProperties":
            {
               "url": "<endpoint of on-premises OData source e.g. Dynamics CRM>",
               "authenticationType": "Windows",
               "username": "domain\\user",
               "password": "password",
               "gatewayName": "mygateway"
           }
       }
    }

### <a name="using-oauth-authentication-accessing-cloud-odata-source"></a>Verwenden der OAuth-Authentifizierung für den Zugriff auf eine cloudbasierte OData-Quelle
    {
        "name": "inputLinkedService",
        "properties":
        {
            "type": "OData",
               "typeProperties":
            {
               "url": "<endpoint of cloud OData source e.g. https://<tenant>.crm.dynamics.com/XRMServices/2011/OrganizationData.svc">",
               "authenticationType": "OAuth",
               "authorizedCredential": "<auto generated by clicking the Authorize button on UI>"
           }
       }
    }

## <a name="odata-dataset-type-properties"></a>Eigenschaften des Datasettyps „OData“
Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie „structure“, „availability“ und „policy“ des JSON-Codes eines Datasets sind bei allen Dataset-Typen (Azure SQL, Azure-Blob, Azure-Tabelle usw.) ähnlich.

Der Abschnitt **typeProperties** unterscheidet sich bei jedem Typ von Dataset und bietet Informationen zum Speicherort der Daten im Datenspeicher. Der Abschnitt „typeProperties“ für ein Dataset vom Typ **ODataResource** (mit OData-Datasets) hat die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| path |Pfad zu der OData-Ressource |Nein |

## <a name="odata-copy-activity-type-properties"></a>Eigenschaften des OData-Kopieraktivitätstyps
Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md). Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabetabellen und Richtlinie sind für alle Arten von Aktivitäten verfügbar.

Eigenschaften im Abschnitt „typeProperties“ der Aktivität können dagegen je nach Aktivitätstyp variieren. Für die Kopieraktivität variieren die Eigenschaften je nach Art der Quellen und Senken.

Wenn eine Quelle des Typs **RelationalSource** (wozu OData gehört) verwendet wird, sind im Abschnitt „typeProperties“ folgende Eigenschaften verfügbar:

| Eigenschaft | Beschreibung | Beispiel | Erforderlich |
| --- | --- | --- | --- |
| query |Verwendet die benutzerdefinierte Abfrage zum Lesen von Daten. |"?$select=Name, Beschreibung&$top=5" |Nein |

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-odata"></a>Typzuordnung für OData
Wie im Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) beschrieben, führt die Kopieraktivität mithilfe der folgenden beiden Schritte automatische Typkonvertierungen von Quelltypen in Senkentypen durch:

1. Konvertieren von systemeigenen Quelltypen in den .NET-Typ
2. Konvertieren vom .NET-Typ in systemeigenen Senkentyp

Beim Verschieben von Daten aus OData-Datenspeichern werden die OData-Datentypen .NET-Typen zugeordnet.

[!INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[!INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Leistung und Optimierung
Der Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) beschreibt wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.



<!--HONumber=Dec16_HO2-->


