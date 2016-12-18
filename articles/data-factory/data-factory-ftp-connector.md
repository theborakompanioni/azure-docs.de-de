---
title: Verschieben von Daten vom FTP-Server | Microsoft Docs
description: Erfahren Sie, wie Sie Daten mithilfe von Azure Data Factory von einem FTP-Server verschieben.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: eea3bab0-a6e4-4045-ad44-9ce06229c718
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2016
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: 6ec8ac288a4daf6fddd6d135655e62fad7ae17c2
ms.openlocfilehash: f27d978eab3aba52ee9a51a5f284b592e3edd6af


---
# <a name="move-data-from-an-ftp-server-using-azure-data-factory"></a>Verschieben von Daten mithilfe von Azure Data Factory von einem FTP-Server
Dieser Artikel beschreibt, wie die Kopieraktivität in Azure Data Factory verwendet wird, um Daten von einem FTP-Server zu einem unterstützten Senkendatenspeicher zu verschieben. Dieser Artikel baut auf dem Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) auf, der eine allgemeine Übersicht zur Datenverschiebung mit der Kopieraktivität und der als Quellen/Senken unterstützten Datenspeichern darstellt.

Data Factory unterstützt derzeit nur das Verschieben von Daten aus einem FTP-Server in andere Datenspeicher und nicht das Verschieben aus anderen Datenspeichern auf einen FTP-Server. Es werden jeweils lokale als auch cloudbasierte FTP-Server unterstützt.

Wenn Sie Daten aus einem **lokalen** FTP-Server in einen Clouddatenspeicher (z.B. Azure Blob Storage) verschieben, installieren und verwenden Sie das Datenverwaltungsgateway. Das Datenverwaltungsgateway ist ein Client-Agent, der auf dem lokalen Computer installiert ist, und der ermöglicht, dass die Clouddienste eine Verbindung mit einer lokalen Ressource herstellen. Weitere Informationen zum Gateway finden Sie unter [Gateway zur Datenverwaltung](data-factory-data-management-gateway.md). Im Artikel [Verschieben von Daten zwischen lokalen Standorten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) erhalten Sie eine Schritt-für-Schritt-Anleitung zum Einrichten und Verwenden des Gateways. Sie verwenden das Gateway, um sich mit einem FTP-Server zu verbinden, auch wenn sich der Server auf einem virtuellen Azure-IaaS-Computer befindet.

Sie können das Gateway auf dem gleichen lokalen Computer oder dem virtuelle Azure-IaaS-Computer als FTP-Server installieren. Allerdings sollten Sie das Gateway auf einem separaten Computer oder einer separaten Azure IaaS-VM installieren, um Ressourcenkonflikte zu vermeiden und die Leistung zu steigern. Wenn Sie das Gateway auf einem separaten Computer installieren, muss dieser Computer auf den FTP-Server zugreifen können.

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Die einfachste Möglichkeit zum Erstellen einer Pipeline, die Daten aus einem FTP-Server kopiert, ist die Verwendung des Assistenten zum Kopieren von Daten. Unter [Tutorial: Erstellen einer Pipeline mit dem Assistenten zum Kopieren](data-factory-copy-data-wizard-tutorial.md) finden Sie eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten.

Die folgenden Beispiele zeigen JSON-Beispieldefinitionen, die Sie zum Erstellen einer Pipeline mit dem [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md), mit [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder mit [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) verwenden können.

## <a name="sample-copy-data-from-ftp-server-to-azure-blob"></a>Beispiel: Kopieren von Daten aus dem FTP-Server in Azure-Blob
Dieses Beispiel zeigt, wie Sie Daten aus dem FTP-Server in Azure Blob Storage kopieren. Daten können jedoch mithilfe der Kopieraktivität in Azure Data Factory **direkt** in die [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) aufgeführten Senken kopiert werden.  

Das Beispiel enthält die folgenden Data Factory-Entitäten:

* Einen verknüpften Dienst des Typs [FtpServer](#ftp-linked-service-properties)
* Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service)
* Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [FileShare](#fileshare-dataset-type-properties)
* Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)
* Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [FileSystemSource](#ftp-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) verwendet

Im Beispiel werden stündlich Daten aus einem FTP-Server in ein Azure-Blob kopiert. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

**Verknüpfter FTP-Dienst** Dieses Beispiel verwendet die Standardauthentifizierung mit dem Benutzernamen und Kennwort im Nur-Text-Format. Sie können auch eine der folgenden Methoden verwenden:

* Anonyme Authentifizierung
* Standardauthentifizierung mit verschlüsselten Anmeldeinformationen
* FTP über SSL/TLS (FTPS)

Informationen zu den verschiedenen Authentifizierungstypen finden Sie im Abschnitt [Eigenschaften des verknüpften FTP-Diensts](#ftp-linked-service-properties).

    {
        "name": "FTPLinkedService",
        "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",           
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
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

**FTP-Eingabedataset** Dieses Dataset bezieht sich auf den FTP-Ordner `mysharedfolder` und die Datei `test.csv`. Die Pipeline kopiert die Datei in das Ziel.

Durch Festlegen von „external“ auf „true“ wird dem Data Factory-Dienst mitgeteilt, dass das Dataset für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

    {
      "name": "FTPFileInput",
      "properties": {
        "type": "FileShare",
        "linkedServiceName": "FTPLinkedService",
        "typeProperties": {
          "folderPath": "mysharedfolder",
          "fileName": "test.csv",
          "useBinaryTransfer": true
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }


**Azure-Blob-Ausgabedataset**

Daten werden stündlich in ein neues Blob geschrieben ("frequency": "hour", "interval": 1). Der Ordnerpfad des Blobs wird basierend auf der Startzeit des Slices, der verarbeitet wird, dynamisch ausgewertet. Im Ordnerpfad werden Jahr, Monat, Tag und die Stundenteile der Startzeit verwendet.

    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/ftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Die Pipeline enthält eine Kopieraktivität, die für die Verwendung der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **FileSystemSource** und der Typ **sink** auf **BlobSink** festgelegt.

    {
        "name": "pipeline",
        "properties": {
            "activities": [{
                "name": "FTPToBlobCopy",
                "inputs": [{
                    "name": "FtpFileInput"
                }],
                "outputs": [{
                    "name": "AzureBlobOutput"
                }],
                "type": "Copy",
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
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "00:05:00"
                }
            }],
            "start": "2016-08-24T18:00:00Z",
            "end": "2016-08-24T19:00:00Z"
        }
    }

## <a name="ftp-linked-service-properties"></a>Eigenschaften des verknüpften FTP-Diensts
Die folgende Tabelle enthält eine Beschreibung der JSON-Elemente, die für den verknüpften FTP-Dienst spezifisch sind.

| Eigenschaft | Beschreibung | Erforderlich | Standard |
| --- | --- | --- | --- |
| type |Die type-Eigenschaft muss auf FtpServer festgelegt sein |Ja |&nbsp; |
| host |Name oder IP-Adresse des FTP-Servers |Ja |&nbsp; |
| authenticationType |Angeben des Authentifizierungstyps |Ja |Standard, Anonym |
| username |Der Benutzer, der Zugriff auf den FTP-Server hat |Nein |&nbsp; |
| password |Kennwort für den Benutzer (username) |Nein |&nbsp; |
| encryptedCredential |Verschlüsselte Anmeldeinformation für den Zugriff auf den FTP-Server |Nein |&nbsp; |
| gatewayName |Name des Datenverwaltungsgateway-Gateways zum Herstellen einer Verbindung zu einem lokalen FTP-Server |Nein |&nbsp; |
| port |Port, den der FTP-Server abhört |Nein |21 |
| enableSsl |Angeben, ob FTP über SSL/TSL-Kanal verwendet werden soll |Nein |true |
| enableServerCertificateValidation |Angeben, ob die Überprüfung des SSL-Zertifikats aktiviert werden soll, wenn FTP über SSL/TSL-Kanal verwendet wird |Nein |true |

### <a name="using-anonymous-authentication"></a>Verwenden der anonymen Authentifizierung
    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {        
                "authenticationType": "Anonymous",
                  "host": "myftpserver.com"
            }
        }
    }

### <a name="using-username-and-password-in-plain-text-for-basic-authentication"></a>Verwenden von „username“ und „password“ im Nur-Text-Format für die Standardauthentifizierung
    {
        "name": "FTPLinkedService",
          "properties": {
        "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "username": "Admin",
                "password": "123456"
            }
          }
    }


### <a name="using-port-enablessl-enableservercertificatevalidation"></a>Verwenden von Port, enableSsl, enableServerCertificateValidation
    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",    
                "username": "Admin",
                "password": "123456",
                "port": "21",
                "enableSsl": true,
                "enableServerCertificateValidation": true
            }
        }
    }

### <a name="using-encryptedcredential-for-authentication-and-gateway"></a>Verwenden von encryptedCredential für die Authentifizierung und das Gateway
    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "encryptedCredential": "xxxxxxxxxxxxxxxxx",
                "gatewayName": "mygateway"
            }
          }
    }

Ausführliche Informationen zum Festlegen von Anmeldeinformationen für eine lokale FTP-Datenquelle finden Sie unter [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit dem Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md).

[!INCLUDE [data-factory-file-share-dataset](../../includes/data-factory-file-share-dataset.md)]

[!INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]

[!INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="ftp-copy-activity-type-properties"></a>Eigenschaften des FTP-Kopieraktivitätstyps
Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md). Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabetabellen und Richtlinien sind für alle Arten von Aktivitäten verfügbar.

Eigenschaften im Abschnitt „typeProperties“ der Aktivität können dagegen je nach Aktivitätstyp variieren. Für die Kopieraktivität variieren die Eigenschaften des Typs je nach Art der Quellen und Senken.

[!INCLUDE [data-factory-file-system-source](../../includes/data-factory-file-system-source.md)]

[!INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Leistung und Optimierung
Der Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) beschreibt wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.

## <a name="next-steps"></a>Nächste Schritte
Entsprechende Informationen finden Sie in den folgenden Artikeln:

* [Kopieraktivität-Tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) für schrittweise Anleitungen zum Erstellen einer Pipeline mit einer Kopieraktivität.



<!--HONumber=Nov16_HO3-->


