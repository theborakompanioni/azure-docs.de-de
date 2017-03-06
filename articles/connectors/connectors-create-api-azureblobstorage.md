---
title: "Hinzufügen des Azure Blob Storage-Connectors in Ihren Logik-Apps | Microsoft Docs"
description: "Übersicht über den Azure Blob Storage-Connector mit REST-API-Parametern"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b5dc3f75-6bea-420b-b250-183668d2848d
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: mandia
translationtype: Human Translation
ms.sourcegitcommit: 66fc8f7e1da55dbe6bb1dd8b8d6a535c498c1cf7
ms.openlocfilehash: 6b7c73576e09af3d1b3c886efa88044846e91494
ms.lasthandoff: 02/16/2017


---
# <a name="get-started-with-the-azure-blob-storage-connector"></a>Erste Schritte mit dem Azure Blob Storage-Connector
Azure Blob Storage ist ein Dienst zur Speicherung großer Mengen unstrukturierter Daten. In Azure Blob Storage können Sie verschiedene Aktionen für Blobs ausführen und sie beispielsweise hochladen, aktualisieren, abrufen oder löschen. 

Azure-Blobspeicher ermöglicht Folgendes:

* Gestalten Sie Ihren Workflow, indem Sie neue Projekte hochladen oder kürzlich aktualisierte Dateien abrufen.
* Mithilfe von Aktionen können Sie unter anderem Dateimetadaten abrufen, eine Datei löschen und Dateien kopieren. Wenn also beispielsweise ein Tool auf einer Azure-Website aktualisiert wird (Trigger), kann eine Datei in Blob Storage aktualisiert werden (Aktion). 

Dieses Thema beschreibt, wie Sie den Blob Storage-Connector in einer Logik-App verwenden, und enthält eine Liste mit Aktionen.

> [!NOTE]
> Diese Version des Artikels gilt für die allgemein verfügbare Version von Logic Apps. 
> 
> 

Weitere Informationen zu Logik-Apps finden Sie unter [Was sind Logik-Apps](../logic-apps/logic-apps-what-are-logic-apps.md) sowie unter [Erstellen einer Logik-App zum Verbinden von SaaS-Diensten](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-to-azure-blob-storage"></a>Herstellen einer Verbindung mit Azure Blob Storage
Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, muss zunächst eine *Verbindung* mit dem Dienst hergestellt werden. Eine Verbindung stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her. Um eine Verbindung mit einem Speicherkonto herzustellen, müssen Sie beispielsweise zuerst eine Blob Storage-*Verbindung* erstellen. Hierzu geben Sie die Anmeldeinformationen ein, mit denen Sie normalerweise auf den Dienst zugreifen, mit dem Sie eine Verbindung herstellen möchten. Geben Sie für Azure Storage also die Anmeldeinformationen Ihres Speicherkontos ein, um die Verbindung zu erstellen. 

#### <a name="create-the-connection"></a>Erstellen der Verbindung
> [!INCLUDE [Create a connection to Azure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]
> 
> 

## <a name="use-a-trigger"></a>Verwenden eines Triggers
Dieser Connector verfügt über keine Trigger. Verwenden Sie andere Trigger, um die Logik-App zu starten – beispielsweise einen Wiederholungstrigger, einen HTTP-Webhook-Trigger oder einen Trigger eines anderen Connectors. Unter [Erstellen einer Logik-App zum Verbinden von SaaS-Diensten](../logic-apps/logic-apps-create-a-logic-app.md) finden Sie ein Beispiel.

## <a name="use-an-action"></a>Verwenden einer Aktion
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird.

1. Wählen Sie das Pluszeichen aus. Es stehen mehrere Auswahlmöglichkeiten zur Verfügung: **Aktion hinzufügen**, **Bedingung hinzufügen** oder eine der Optionen unter **Mehr**.
   
    ![](./media/connectors-create-api-azureblobstorage/add-action.png)
2. Wählen Sie **Aktion hinzufügen**aus.
3. Geben Sie im Textfeld die Zeichenfolge „blob“ ein, um eine Liste mit allen verfügbaren Aktionen zu erhalten.
   
    ![](./media/connectors-create-api-azureblobstorage/actions.png) 
4. In unserem Beispiel wählen wir **AzureBlob - Get file metadata using path** (AzureBlob – Dateimetadaten anhand des Pfads abrufen) aus. Falls bereits eine Verbindung vorhanden ist, wählen Sie die Schaltfläche **...** aus. (Auswahl anzeigen) aus, um eine Datei auszuwählen.
   
    ![](./media/connectors-create-api-azureblobstorage/sample-file.png)
   
    Wenn Sie zur Eingabe der Verbindungsinformationen aufgefordert werden, geben Sie die Details ein, um die Verbindung zu erstellen. Die Eigenschaften werden in diesem Thema unter [Erstellen der Verbindung](connectors-create-api-azureblobstorage.md#create-the-connection) beschrieben. 
   
   > [!NOTE]
   > In diesem Beispiel rufen wir die Metadaten einer Datei ab. Fügen Sie zum Anzeigen der Metadaten eine weitere Aktion hinzu, die unter Verwendung eines anderen Connectors eine neue Datei erstellt. Fügen Sie beispielsweise eine OneDrive-Aktion hinzu, die auf der Grundlage der Metadaten eine neue Testdatei erstellt. 
   > 
   > 
5. Speichern Sie Ihre Änderungen. (Die Option **Speichern** befindet sich links oben auf der Symbolleiste.) Ihre Logik-App wird gespeichert und ggf. automatisch aktiviert.

> [!TIP]
> Der [Speicher-Explorer](http://storageexplorer.com/) ist ein praktisches Verwaltungstool für mehrere Speicherkonten.
> 
> 

## <a name="technical-details"></a>Technische Details
## <a name="storage-blob-actions"></a>Storage-Blobaktionen
| Aktion | Beschreibung |
| --- | --- |
| [Dateimetadaten abrufen](connectors-create-api-azureblobstorage.md#get-file-metadata) |Dieser Vorgang ruft Dateimetadaten unter Verwendung der Datei-ID ab. |
| [Datei aktualisieren](connectors-create-api-azureblobstorage.md#update-file) |Dieser Vorgang aktualisiert eine Datei. |
| [Datei löschen](connectors-create-api-azureblobstorage.md#delete-file) |Dieser Vorgang löscht eine Datei. |
| [Dateimetadaten anhand des Pfads abrufen](connectors-create-api-azureblobstorage.md#get-file-metadata-using-path) |Dieser Vorgang ruft Dateimetadaten unter Verwendung des Pfads ab. |
| [Dateiinhalt anhand des Pfads abrufen](connectors-create-api-azureblobstorage.md#get-file-content-using-path) |Dieser Vorgang ruft Dateiinhalte unter Verwendung des Pfads ab. |
| [Dateiinhalte abrufen](connectors-create-api-azureblobstorage.md#get-file-content) |Dieser Vorgang ruft Dateiinhalte unter Verwendung der ID ab. |
| [Datei erstellen](connectors-create-api-azureblobstorage.md#create-file) |Dieser Vorgang lädt eine Datei hoch. |
| [Datei kopieren](connectors-create-api-azureblobstorage.md#copy-file) |Dieser Vorgang kopiert eine Datei in Azure Blob Storage. |
| [Archiv in Ordner extrahieren](connectors-create-api-azureblobstorage.md#extract-archive-to-folder) |Dieser Vorgang extrahiert eine Archivdatei (beispielsweise eine ZIP-Datei) in einen Ordner. |

### <a name="action-details"></a>Aktionsdetails
Dieser Abschnitt enthält spezifische Details zu den einzelnen Aktionen. Hierzu zählen unter anderem erforderliche oder optionale Eingabeeigenschaften sowie entsprechende Ausgaben im Zusammenhang mit dem Connector.

#### <a name="get-file-metadata"></a>Dateimetadaten abrufen
Dieser Vorgang ruft Dateimetadaten unter Verwendung der Datei-ID ab.  

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| id* |Datei |Datei auswählen |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
BlobMetadata

| Eigenschaftenname | Datentyp |
| --- | --- |
| id |string |
| Name |string |
| displayName |string |
| path |string |
| LastModified |string |
| Größe |integer |
| MediaType |string |
| IsFolder |Boolescher Wert |
| ETag |string |
| FileLocator |string |

#### <a name="update-file"></a>Datei aktualisieren
Dieser Vorgang aktualisiert eine Datei.  

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| id* |Datei |Datei auswählen |
| body* |Dateiinhalte |Inhalte der zu aktualisierenden Datei |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
BlobMetadata

| Eigenschaftenname | Datentyp |
| --- | --- |
| id |string |
| Name |string |
| displayName |string |
| path |string |
| LastModified |string |
| Größe |integer |
| MediaType |string |
| IsFolder |Boolescher Wert |
| ETag |string |
| FileLocator |string |

#### <a name="delete-file"></a>Datei löschen
Dieser Vorgang löscht eine Datei.  

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| id* |Datei |Datei auswählen |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine

#### <a name="get-file-metadata-using-path"></a>Dateimetadaten anhand des Pfads abrufen
Dieser Vorgang ruft Dateimetadaten unter Verwendung des Pfads ab.  

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| path* |Dateipfad |Datei auswählen |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
BlobMetadata

| Eigenschaftenname | Datentyp |
| --- | --- |
| id |string |
| Name |string |
| displayName |string |
| path |string |
| LastModified |string |
| Größe |integer |
| MediaType |string |
| IsFolder |Boolescher Wert |
| ETag |string |
| FileLocator |string |

#### <a name="get-file-content-using-path"></a>Dateiinhalt anhand des Pfads abrufen
Dieser Vorgang ruft Dateiinhalte unter Verwendung des Pfads ab.  

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| path* |Dateipfad |Datei auswählen |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine

#### <a name="get-file-content"></a>Dateiinhalte abrufen
Dieser Vorgang ruft Dateiinhalte unter Verwendung der ID ab.  

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| id* |string |Datei auswählen |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine

#### <a name="create-file"></a>Datei erstellen
Dieser Vorgang lädt eine Datei hoch.  

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| folderPath* |Ordnerpfad |Ordner auswählen |
| name* |Dateiname |Name der hochzuladenden Datei |
| body* |Dateiinhalte |Inhalt der hochzuladenden Datei |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
BlobMetadata

| Eigenschaftenname | Datentyp |
| --- | --- |
| id |string |
| Name |string |
| displayName |string |
| path |string |
| LastModified |string |
| Größe |integer |
| MediaType |string |
| IsFolder |Boolescher Wert |
| ETag |string |
| FileLocator |string |

#### <a name="copy-file"></a>Datei kopieren
Dieser Vorgang kopiert eine Datei in Azure Blob Storage.  

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| source* |Quell-URL |URL zur Quelldatei angeben |
| destination* |Zieldateipfad |Zieldateipfad einschließlich Zieldateiname angeben |
| overwrite |Überschreiben? |Soll eine vorhandene Zieldatei überschrieben werden (True/False)? |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
BlobMetadata

| Eigenschaftenname | Datentyp |
| --- | --- |
| id |string |
| Name |string |
| displayName |string |
| path |string |
| LastModified |string |
| Größe |integer |
| MediaType |string |
| IsFolder |Boolescher Wert |
| ETag |string |
| FileLocator |string |

#### <a name="extract-archive-to-folder"></a>Archiv in Ordner extrahieren
Dieser Vorgang extrahiert eine Archivdatei (beispielsweise eine ZIP-Datei) in einen Ordner.  

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| source* |Quellarchiv-Dateipfad |Archivdatei auswählen |
| destination* |Zielordnerpfad |Zu extrahierende Inhalte auswählen |
| overwrite |Überschreiben? |Soll eine vorhandene Zieldatei überschrieben werden (True/False)? |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
BlobMetadata

| Eigenschaftenname | Datentyp |
| --- | --- |
| id |string |
| Name |string |
| displayName |string |
| path |string |
| LastModified |string |
| Größe |integer |
| MediaType |string |
| IsFolder |Boolescher Wert |
| ETag |string |
| FileLocator |string |

## <a name="http-responses"></a>HTTP-Antworten
Das Aufrufen der verschiedenen Aktionen löst unter Umständen bestimmte Antworten aus. Diese Antworten sind in der folgenden Tabelle aufgeführt und beschrieben:  

| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 202 |Zulässig |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler ist aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen Sie eine Logik-App](../logic-apps/logic-apps-create-a-logic-app.md). Informieren Sie sich in unserer [API-Liste](apis-list.md)über die anderen verfügbaren Connectors für Logik-Apps.


