---
title: Verschieben von Daten in/aus Azure Data Lake-Speicher | Microsoft Docs
description: Erfahren Sie, wie Daten mithilfe von Azure Data Factory in und aus Azure Data Lake-Speicher verschoben werden.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 25b1ff3c-b2fd-48e5-b759-bb2112122e30
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: b2d1a740782a20a7c6b7b8cec8335a41f16231f5
ms.openlocfilehash: 5a6a14e5fc8f6915b34f9667c4294a46c8591633


---
# <a name="move-data-to-and-from-azure-data-lake-store-using-azure-data-factory"></a>Verschieben von Daten in und aus Azure Data Lake-Speicher mithilfe von Azure Data Factory
Dieser Artikel beschreibt, wie Sie die Kopieraktivität in einer Azure Data Factory verwenden können, um Daten aus Azure Data Lake Store in einen anderen Datenspeicher und umgekehrt zu verschieben. Dieser Artikel baut auf dem Artikel zu [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) auf, der eine allgemeine Übersicht zur Datenverschiebung mit der Kopieraktivität und den unterstützten Datenspeicherkombinationen bietet.

> [!NOTE]
> Erstellen Sie ein Azure Data Lake Store-Konto, bevor Sie eine Pipeline mit einer Kopieraktivität zum Verschieben von Daten in einen bzw. aus einem Azure Data Lake Store erstellen können. Weitere Informationen zu Azure Data Lake Store finden Sie unter [Erste Schritte mit Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md).
>

## <a name="supported-authentication-types"></a>Unterstützte Authentifizierungstypen
Der Azure Data Lake Store-Connector unterstützt die Authentifizierung per **Dienstprinzipal** und die Authentifizierung mit **Benutzeranmeldeinformationen**. Wir empfehlen die Verwendung der ersten Option (insbesondere beim geplanten Kopieren von Daten), um das Tokenablaufverhalten der zweiten Option zu vermeiden. Ausführliche Informationen zur Konfiguration finden Sie im Abschnitt [Eigenschaften des mit Azure Data Lake-Speicher verknüpften Diensts](#azure-data-lake-store-linked-service-properties).

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Die einfachste Möglichkeit zum Erstellen einer Pipeline, die Daten in und aus Azure Data Lake Store kopiert, ist die Verwendung des Assistenten zum Kopieren von Daten. Unter [Tutorial: Erstellen einer Pipeline mit dem Assistenten zum Kopieren](data-factory-copy-data-wizard-tutorial.md) finden Sie eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten.

Die folgenden Beispiele zeigen JSON-Beispieldefinitionen, die Sie zum Erstellen einer Pipeline mit dem [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md), mit [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) verwenden können. Sie zeigen, wie Sie Daten in und aus Azure Data Lake Store und Azure-Blobspeicher kopieren. Allerdings können Daten **direkt** aus einer der Quellen in eine der unterstützten Senken kopiert werden. Weitere Informationen finden Sie im Abschnitt „Unterstützte Datenspeicher und Formate“ unter [Verschieben von Daten mit der Kopieraktivität](data-factory-data-movement-activities.md).  

## <a name="sample-copy-data-from-azure-blob-to-azure-data-lake-store"></a>Beispiel: Kopieren von Daten aus einem Azure-Blob in Azure Data Lake-Speicher
Dieses Beispiel zeigt Folgendes:

1. Einen verknüpften Dienst des Typs [AzureStorage](#azure-storage-linked-service-properties)
2. Einen verknüpften Dienst des Typs [AzureDataLakeStore](#azure-data-lake-linked-service-properties)
3. Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](#azure-blob-dataset-type-properties)
4. Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureDataLakeStore](#azure-data-lake-dataset-type-properties)
5. Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [BlobSource](#azure-blob-copy-activity-type-properties) und [AzureDataLakeStoreSink](#azure-data-lake-copy-activity-type-properties) verwendet

Im Beispiel werden Daten einer Zeitreihe aus Azure Blob Storage stündlich in einen Azure Data Lake Store kopiert. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

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

**Mit Azure Data Lake verknüpfter Dienst:**

```JSON
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

> [!NOTE]
> Ausführliche Informationen zur Konfiguration finden Sie in den Schritten des Abschnitts [Eigenschaften des mit Azure Data Lake-Speicher verknüpften Diensts](#azure-data-lake-store-linked-service-properties).
>

**Azure-Blob-Eingabedataset:**

Daten werden stündlich aus einem neuen Blob übernommen ("frequency": "hour", "interval": 1). Ordnerpfad und Dateiname des Blobs werden basierend auf der Startzeit des Slices, der verarbeitet wird, dynamisch ausgewertet. Der Ordnerpfad verwendet die Bestandteile Jahr, Monat und Tag der Startzeit, und der Dateiname verwendet die Stunde der Startzeit. Die Festlegung von „external“ auf „true“ teilt dem Data Factory-Dienst mit, dass diese Tabelle für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

```JSON
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

**Azure Data Lake-Ausgabedataset:**

Im Beispiel werden Daten in einen Azure Data Lake-Speicher kopiert. Neue Daten werden stündlich in den Data Lake-Speicher kopiert.

```JSON
{
    "name": "AzureDataLakeStoreOutput",
      "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/output/"
        },
        "availability": {
              "frequency": "Hour",
              "interval": 1
        }
      }
}
```


**Pipeline mit Kopieraktivität:**

Die Pipeline enthält eine Kopieraktivität, die für die Verwendung der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **BlobSource** und der Typ **sink** auf **AzureDataLakeStoreSink** festgelegt.

```JSON
{  
    "name":"SamplePipeline",
    "properties":
    {  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":
        [  
              {
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "AzureBlobInput"
                  }
                ],
                "outputs": [
                  {
                    "name": "AzureDataLakeStoreOutput"
                  }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                      },
                      "sink": {
                        "type": "AzureDataLakeStoreSink"
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

## <a name="sample-copy-data-from-azure-data-lake-store-to-azure-blob"></a>Beispiel: Kopieren von Daten aus Azure Data Lake-Speicher in ein Azure-Blob
Dieses Beispiel zeigt Folgendes:

1. Einen verknüpften Dienst des Typs [AzureDataLakeStore](#azure-data-lake-linked-service-properties)
2. Einen verknüpften Dienst des Typs [AzureStorage](#azure-storage-linked-service-properties)
3. Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [AzureDataLakeStore](#azure-data-lake-dataset-type-properties)
4. Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](#azure-blob-dataset-type-properties)
5. Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [AzureDataLakeStoreSource](#azure-data-lake-copy-activity-type-properties) und [BlobSink](#azure-blob-copy-activity-type-properties) verwendet

Im Beispiel werden Daten einer Zeitreihe aus einem Azure Data Lake Store stündlich in ein Azure-Blob kopiert. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

**Mit Azure Data Lake-Speicher verknüpfter Dienst:**

```JSON
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        }
    }
}
```

> [!NOTE]
> Ausführliche Informationen zur Konfiguration finden Sie in den Schritten des Abschnitts [Eigenschaften des mit Azure Data Lake-Speicher verknüpften Diensts](#azure-data-lake-store-linked-service-properties).
>

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
**Azure Data Lake-Eingabedataset:**

Durch Festlegen von **„external“ auf „true“** wird dem Data Factory-Dienst mitgeteilt, dass die Tabelle für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

```JSON
{
    "name": "AzureDataLakeStoreInput",
      "properties":
    {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/input/",
            "fileName": "SearchLog.tsv",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
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
**Azure-Blob-Ausgabedataset:**

Daten werden stündlich in ein neues Blob geschrieben ("frequency": "hour", "interval": 1). Der Ordnerpfad des Blobs wird basierend auf der Startzeit des Slices, der verarbeitet wird, dynamisch ausgewertet. Im Ordnerpfad werden Jahr, Monat, Tag und die Stundenteile der Startzeit verwendet.

```JSON
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

Die Pipeline enthält eine Kopieraktivität, die für die Verwendung der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **AzureDataLakeStoreSource** und der Typ **sink** auf **BlobSink** festgelegt.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
              {
                "name": "AzureDakeLaketoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "AzureDataLakeStoreInput"
                  }
                ],
                "outputs": [
                  {
                    "name": "AzureBlobOutput"
                  }
                ],
                "typeProperties": {
                    "source": {
                        "type": "AzureDataLakeStoreSource",
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

## <a name="azure-data-lake-store-linked-service-properties"></a>Eigenschaften des mit Azure Data Lake-Speicher verknüpften Diensts
Die folgende Tabelle beschreibt spezifische JSON-Elemente für den verknüpften Azure Data Lake Store-Dienst. Außerdem können Sie zwischen Authentifizierung per **Dienstprinzipal** und Authentifizierung mit **Benutzeranmeldeinformationen** wählen.

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft muss auf **AzureDataLakeStore** | Ja |
| dataLakeStoreUri | Geben Sie Informationen zum Azure Data Lake-Speicherkonto an. Sie weisen das folgende Format auf: **https://[Kontoname].azuredatalakestore.net/webhdfs/v1** oder **adl://[Kontoname].azuredatalakestore.net/**. | Ja |
| subscriptionId | ID des Azure-Abonnements, dem die Data Lake Store-Instanz angehört. | Erforderlich für Senke |
| ResourceGroupName | Name der Azure-Ressourcengruppe, der die Data Lake Store-Instanz angehört. | Erforderlich für Senke |

### <a name="using-service-principal-authentication-recommended"></a>Verwenden der Dienstprinzipalauthentifizierung (empfohlen)
Wenn Sie die Dienstprinzipalauthentifizierung verwenden möchten, müssen Sie zunächst in Azure Active Directory (AAD) eine Anwendungsentität registrieren und ihr in Data Lake Store Zugriff gewähren. Anschließend können Sie in Azure Data Factory die folgenden Eigenschaften mit entsprechenden Werten für Anwendungs-ID, Anwendungsschlüssel und Mandant angeben, um Daten aus bzw. in Data Lake Store zu kopieren. Informationen zur Einrichtung sowie zum Abrufen der erforderlichen Informationen finden Sie unter [Dienst-zu-Dienst-Authentifizierung](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).

> [!IMPORTANT]
> Wenn Sie den Assistenten zum Kopieren verwenden, müssen Sie dem Dienstprinzipal mindestens Leseberechtigung für den ADLS-Stamm („/“) gewähren oder die Reader-Rolle für das ADLS-Konto zuweisen, damit Sie durch die Ordner navigieren können. Andernfalls wird möglicherweise der Fehler „Die angegebenen Anmeldeinformationen sind ungültig“ angezeigt.
>
> Wenn Sie einen Dienstprinzipal in AAD neu erstellen oder aktualisieren, kann es einige Minuten dauern, bis er tatsächlich wirksam wird. Überprüfen Sie den Dienstprinzipal und die ADLS-ACL-Konfiguration zunächst gründlich, wenn weiterhin die Fehlermeldung „Die angegebenen Anmeldeinformationen sind ungültig“ angezeigt wird. Warten Sie dann einen Moment, und wiederholen Sie den Vorgang.
>

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| servicePrincipalId | Geben Sie die Client-ID der Anwendung an. | Ja |
| servicePrincipalKey | Geben Sie den Schlüssel der Anwendung an. | Ja |
| tenant | Geben Sie die Mandanteninformationen (Domänenname oder Mandanten-ID) für Ihre Anwendung an. Diesen können Sie bestimmen, indem Sie den Mauszeiger über dem rechten oberen Bereich im Azure-Portal bewegen. | Ja |

**Beispiel: Verwenden der Dienstprinzipalauthentifizierung**
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

### <a name="using-user-credential-authentication"></a>Verwenden der Authentifizierung mit Benutzeranmeldeinformationen
Alternativ können Sie die Authentifizierung mit Benutzeranmeldeinformationen verwenden, um Daten aus/in Data Lake Store zu kopieren. Geben Sie hierzu die folgenden Eigenschaften an:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| authorization | Klicken Sie im **Data Factory-Editor** auf die Schaltfläche **Autorisieren**, und geben Sie Ihre Anmeldeinformationen ein, wodurch die automatisch generierte Autorisierungs-URL dieser Eigenschaft zugewiesen wird. | Ja |
| sessionId | OAuth-Sitzungs-ID aus der OAuth-Autorisierungssitzung. Jede Sitzungs-ID ist eindeutig und darf nur einmal verwendet werden. Diese Einstellung wird automatisch generiert, wenn Sie den Data Factory-Editor verwenden. | Ja |

**Beispiel: Verwenden der Authentifizierung mit Benutzeranmeldeinformationen**
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<session ID>",
            "authorization": "<authorization URL>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

#### <a name="token-expiration"></a>Tokenablauf
Der von Ihnen mithilfe der Schaltfläche **Autorisieren** generierte Autorisierungscode läuft nach einer gewissen Zeit ab. Die Zeiten bis zum Ablaufen der Autorisierungscodes für die verschiedenen Benutzerkonten finden Sie in der folgenden Tabelle. Unter Umständen wird Ihnen nach dem **Ablauf des Tokens** folgende Fehlermeldung angezeigt: „Fehler beim Anmeldevorgang: invalid_grant – AADSTS70002: Fehler beim Überprüfen der Anmeldeinformationen. AADSTS70008: Die angegebene Zugriffserteilung ist abgelaufen oder wurde widerrufen. Ablaufverfolgungs-ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 Korrelations-ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 Zeitstempel: 2015-12-15 21-09-31Z“.

| Benutzertyp | Läuft ab nach |
|:--- |:--- |
| Benutzerkonten, die NICHT von Azure Active Directory verwaltet werden (@hotmail.com, @live.com, usw.). |12 Stunden |
| Benutzerkonten, die von Azure Active Directory (AAD) verwaltet werden |14 Tage nach der letzten Sliceausführung. <br/><br/>90 Tage, wenn ein Slice, das auf einem verknüpften OAuth-Dienst basiert, mindestens einmal alle 14 Tage ausgeführt wird. |

Wenn Sie Ihr Kennwort vor Ablauf der Tokengültigkeitsdauer ändern, läuft das Token sofort ab, und Ihnen wird der in diesem Abschnitt aufgeführte Fehler angezeigt.

Um diesen Fehler zu vermeiden oder zu beheben, autorisieren Sie sich durch Klicken auf die Schaltfläche **Autorisieren** erneut, wenn das **Token abläuft**, und stellen den verknüpften Dienst anschließend erneut bereit. Sie können auch programmgesteuert Werte für die Eigenschaften **sessionId** und **authorization** generieren. Verwenden Sie hierzu den im folgenden Abschnitt bereitgestellten Code:

#### <a name="to-programmatically-generate-sessionid-and-authorization-values"></a>So generieren Sie programmgesteuert Werte für „sessionId“ und „authorization“

```csharp
if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
    linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
{
    AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

    WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
    string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

    AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
    if (azureDataLakeStoreProperties != null)
    {
        azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeStoreProperties.Authorization = authorization;
    }

    AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
    if (azureDataLakeAnalyticsProperties != null)
    {
        azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeAnalyticsProperties.Authorization = authorization;
    }
}
```
Nähere Informationen zu den im Code verwendeten Data Factory-Klassen finden Sie in den Themen [AzureDataLakeStoreLinkedService-Klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService-Klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx) und [AuthorizationSessionGetResponse-Klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx). Fügen Sie für die im Code verwendete „WindowsFormsWebAuthenticationDialog“-Klasse einen Verweis auf Version **2.9.10826.1824** von **Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll** hinzu.

## <a name="azure-data-lake-dataset-type-properties"></a>Eigenschaften von Datasets des Typs „Azure Data Lake“
Eine vollständige Liste der JSON-Abschnitte und -Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie „structure“, „availability“ und „policy“ des JSON-Codes eines Datasets sind bei allen Dataset-Typen (Azure SQL, Azure-Blob, Azure-Tabelle usw.) ähnlich.

Der Abschnitt **typeProperties** unterscheidet sich bei jeder Art von Dataset und enthält unter anderem Informationen zum Speicherort und Format der Daten im Datenspeicher. Der Abschnitt „typeProperties“ für ein Dataset des Typs **AzureDataLakeStore** hat die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| folderPath |Der Pfad zum Container und Ordner im Azure Data Lake-Speicher. |Ja |
| fileName |Der Name der Datei im Azure Data Lake-Speicher. fileName ist optional, wobei seine Groß- und Kleinschreibung beachtet werden muss. <br/><br/>Wenn Sie einen Dateinamen angeben, funktioniert die Aktivität (einschließlich Kopieren) für die jeweilige Datei.<br/><br/>Wenn „fileName“ nicht angegeben ist, werden alle Dateien in „folderPath“ für das Eingabedataset kopiert.<br/><br/>Wenn „fileName“ für ein Ausgabedataset nicht angegeben ist, hat der Name der generierten Datei folgendes Format: Data.<Guid>.txt (Beispiel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt). |Nein |
| partitionedBy |"partitionedBy" ist eine optionale Eigenschaft. "partitionedBy" kann genutzt werden, um einen dynamischen Wert für "folderPath" oder "fileName" für Zeitreihendaten anzugeben. Beispiel: "folderPath" kann für jedes stündliche Datenaufkommen parametrisiert werden. Im Abschnitt [Nutzen der partitionedBy-Eigenschaft](#using-partitionedby-property) finden Sie Details und Beispiele. |Nein |
| format | Die folgenden Formattypen werden unterstützt: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Sie müssen die **type** -Eigenschaft unter „format“ auf einen dieser Werte festlegen. Weitere Informationen finden Sie in den Abschnitten [Textformat](#specifying-textformat), [JSON-Format](#specifying-jsonformat), [Avro-Format](#specifying-avroformat), [Orc-Format](#specifying-orcformat) und [Parquet-Format](#specifying-parquetformat). <br><br> Wenn Sie **Dateien unverändert zwischen dateibasierten Speichern kopieren** möchten (binäre Kopie), können Sie den Formatabschnitt bei den Definitionen von Eingabe- und Ausgabedatasets überspringen. |Nein |
| Komprimierung | Geben Sie den Typ und den Grad der Komprimierung für die Daten an. Folgende Typen werden unterstützt: **GZip**, **Deflate**, **BZip2** und **ZipDeflate**. Folgende Komprimierungsstufen werden unterstützt: **Optimal** und **Schnellste**. Weitere Informationen finden Sie im Abschnitt [Angeben der Komprimierung](#specifying-compression). |Nein |

### <a name="using-partitionedby-property"></a>Nutzen der partitionedBy-Eigenschaft
Sie können die dynamischen Werte „folderPath“ und „fileName“ für Zeitreihendaten mit dem Abschnitt **partitionedBy** , mit Data Factory-Makros und mit den Systemvariablen „SliceStart“ und „SliceEnd“ angeben, die die Start- und Endzeit für einen bestimmten Datenslice festlegen.

In den Artikeln [Erstellen von Datasets](data-factory-create-datasets.md) und [Planung und Ausführung](data-factory-scheduling-and-execution.md) finden Sie weitere Details zu Zeitreihendatasets, Planung und Slices.

#### <a name="sample-1"></a>Beispiel 1

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
Im obigen Beispiel wird {Slice} durch den Wert der Data Factory-Systemvariablen SliceStart ersetzt, die im Format (JJJJMMTTHH) angegeben ist. "SliceStart" bezieht sich auf die Startzeit des Slices. "folderPath" unterscheidet sich für jeden Slice. Beispiel: "wikidatagateway/wikisampledataout/2014100103" oder "wikidatagateway/wikisampledataout/2014100104".

#### <a name="sample-2"></a>Beispiel 2
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

## <a name="azure-data-lake-copy-activity-type-properties"></a>Eigenschaften von Azure Data Lake-Kopieraktivitätstypen
Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md). Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabetabellen und Richtlinie sind für alle Arten von Aktivitäten verfügbar.

Eigenschaften im Abschnitt „typeProperties“ der Aktivität können dagegen je nach Aktivitätstyp variieren. Für die Kopieraktivität variieren die Eigenschaften je nach Art der Quellen und Senken.

**AzureDataLakeStoreSource** unterstützt die folgenden Eigenschaften im Abschnitt **typeProperties**:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| --- | --- | --- | --- |
| recursive |Gibt an, ob die Daten rekursiv aus den Unterordnern oder nur aus dem angegebenen Ordner gelesen werden. |True (Standardwert), False |Nein |

**AzureDataLakeStoreSink** unterstützt die folgenden Eigenschaften im Abschnitt **typeProperties**:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| --- | --- | --- | --- |
| copyBehavior |Gibt das Kopierverhalten an. |**PreserveHierarchy:** Behält die Dateihierarchie im Zielordner bei. Der relative Pfad der Quelldatei zum Quellordner entspricht dem relativen Pfad der Zieldatei zum Zielordner.<br/><br/>**FlattenHierarchy** : Alle Dateien aus dem Quellordner werden auf der ersten Ebene des Zielordners erstellt. Die Namen der Zieldateien werden automatisch generiert.<br/><br/>**MergeFiles** : Führt alle Dateien aus dem Quellordner in einer Datei zusammen. Wenn der Datei-/Blob-Name angegeben wurde, entspricht der Name dem angegebenen Namen, andernfalls dem automatisch generierten Dateinamen. |Nein |

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[!INCLUDE [data-factory-type-conversion-sample](../../includes/data-factory-type-conversion-sample.md)]

[!INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>Leistung und Optimierung

Abhängig davon, ob die anfängliche Datenverschiebung mit einer großen Menge von Verlaufsdaten oder mit inkrementeller Produktionsdatenlast geplant ist, verfügt Azure Data Factory über Optionen zur Verbesserung der Leistung dieser Aufgaben. Der Parallelitätsparameter ist ein Teil der **Kopieraktivität** und definiert, wie viele unterschiedliche Aktivitätsfenster parallel verarbeitet werden. Der **parallelCopies**-Parameter definiert die Parallelität für die einzelne Aktivitätsausführung. Zur Erzielung des besten Durchsatzes ist es wichtig, beim Entwerfen von Pipelines für die Datenverschiebung mit Azure Data Factory diese Parameter zu berücksichtigen.

Der Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) beschreibt wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.



<!--HONumber=Feb17_HO2-->


