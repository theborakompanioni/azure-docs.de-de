---
title: Dropbox | Microsoft Docs
description: "Erstellen Sie Logik-Apps mit Azure App Service. Stellen Sie eine Verbindung mit Dropbox her, um Dateien zu verwalten. Sie können verschiedene Aktionen ausführen und beispielsweise Dateien hochladen, aktualisieren, abrufen und in Dropbox löschen."
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: cb0ae033-aba7-4ac9-beaa-be561a0f0cac
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/15/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: 9c74b25a2ac5e2088a841d97920035376b7f3f11
ms.openlocfilehash: a8ecad3552f35c02c07fe33aa8531fb462b27002


---
# <a name="get-started-with-the-dropbox-connector"></a>Erste Schritte mit dem Dropbox-Connector
Stellen Sie eine Verbindung mit Dropbox her, um Dateien zu verwalten. Sie können verschiedene Aktionen ausführen und beispielsweise Dateien hochladen, aktualisieren, abrufen und in Dropbox löschen.

Wenn Sie [einen Connector](apis-list.md) verwenden möchten, müssen Sie zuerst eine Logik-App erstellen. [Erstellen Sie daher erst einmal eine Logik-App](../logic-apps/logic-apps-create-a-logic-app.md), wie hier beschrieben.

## <a name="connect-to-dropbox"></a>Herstellen einer Verbindung mit Dropbox
Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, müssen Sie zunächst eine *Verbindung* mit dem Dienst herstellen. Eine Verbindung stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her. Wenn Sie also beispielsweise eine Verbindung mit Dropbox herstellen möchten, müssen Sie zunächst eine entsprechende *Verbindung* erstellen. Geben Sie zum Erstellen einer Verbindung die Anmeldeinformationen an, mit denen Sie normalerweise auf den Dienst zugreifen, mit dem Sie eine Verbindung herstellen möchten. Im Falle des Dropbox-Beispiels benötigen Sie also die Anmeldeinformationen für Ihr Dropbox-Konto, um die Verbindung mit Dropbox zu erstellen. [Weitere Informationen zu Verbindungen]()

### <a name="create-a-connection-to-dropbox"></a>Erstellen einer Verbindung mit Dropbox
> [!INCLUDE [Steps to create a connection to Dropbox](../../includes/connectors-create-api-dropbox.md)]
> 
> 

## <a name="use-a-dropbox-trigger"></a>Verwenden eines Dropbox-Triggers
Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

In diesem Beispiel verwenden wir den Trigger **When a file is created** (Wenn eine Datei erstellt wird). Wenn dieser Trigger aktiviert wird, rufen wir die Dropbox-Aktion **Get file content using path** (Dateiinhalt anhand des Pfads abrufen) auf. 

1. Geben Sie im Suchfeld des Logic Apps-Designers die Zeichenfolge *dropbox* ein, und wählen Sie anschließend den Trigger **Dropbox – When a file is created** (Dropbox – wenn eine Datei erstellt wird) aus.      
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger.PNG)  
2. Wählen Sie den Ordner aus, in dem Sie die Dateierstellung nachverfolgen möchten. Wählen Sie „...“ (rot markiert) aus, und navigieren Sie zum gewünschten Ordner für die Triggereingabe.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger-2.PNG)  

## <a name="use-a-dropbox-action"></a>Verwenden einer Dropbox-Aktion
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

Führen Sie nach dem Hinzufügen des Triggers die folgenden Schritte aus, um eine Aktion hinzuzufügen, die den Inhalt der neuen Datei abruft.

1. Wählen Sie **+ Neuer Schritt** aus, um die Aktion hinzuzufügen, die ausgeführt werden soll, wenn eine neue Datei erstellt wird.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action.PNG)
2. Wählen Sie **Aktion hinzufügen**aus. Daraufhin öffnet sich das Suchfeld, mit dem Sie nach der gewünschten Aktion suchen können.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-2.PNG)
3. Geben Sie *dropbox* ein, um nach Dropbox-spezifischen Aktionen zu suchen.  
4. Wählen Sie **Dropbox - Get file content using path** (Dropbox – Dateiinhalt anhand des Pfads abrufen) als Aktion aus, die ausgeführt werden soll, wenn im ausgewählten Dropbox-Ordner eine neue Datei erstellt wird. Die Aktionskontrollblock wird geöffnet. Sie werden aufgefordert, den Zugriff Ihrer Logik-App auf Ihr Dropbox-Konto zu autorisieren, sofern Sie diesen Schritt noch nicht ausgeführt haben.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-3.PNG)  
5. Wählen Sie „...“ (rechts neben dem Steuerelement **Dateipfad**) aus, und navigieren Sie zum gewünschten Dateipfad. Alternativ können Sie das Token **Dateipfad** verwenden, um die Erstellung Ihrer Logik-App zu beschleunigen.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-4.PNG)  
6. Speichern Sie Ihre Arbeit, und erstellen Sie in Dropbox eine neue Datei, um den Workflow zu aktivieren.  

## <a name="technical-details"></a>Technische Details
Im Anschluss finden Sie ausführliche Informationen zu den Triggern, Aktionen und Antworten, die von dieser Verbindung unterstützt werden:

## <a name="dropbox-triggers"></a>Dropbox-Trigger
Der Dropbox-Connector verfügt über folgende Trigger:  

| Trigger | Beschreibung |
| --- | --- |
| [Wenn eine Datei erstellt wird](connectors-create-api-dropbox.md#when-a-file-is-created) |Dieser Vorgang löst einen Fluss aus, wenn in einem Ordner eine neue Datei erstellt wird. |
| [Wenn eine Datei geändert wird](connectors-create-api-dropbox.md#when-a-file-is-modified) |Dieser Vorgang löst einen Fluss aus, wenn in einem Ordner eine Datei geändert wird. |

## <a name="dropbox-actions"></a>Dropbox-Aktionen
Der Dropbox-Connector verfügt über folgende Aktionen:

| Aktion | Beschreibung |
| --- | --- |
| [Dateimetadaten abrufen](connectors-create-api-dropbox.md#get-file-metadata) |Dieser Vorgang ruft die Metadaten für eine Datei ab. |
| [Datei aktualisieren](connectors-create-api-dropbox.md#update-file) |Dieser Vorgang aktualisiert eine Datei. |
| [Datei löschen](connectors-create-api-dropbox.md#delete-file) |Dieser Vorgang löscht eine Datei. |
| [Dateimetadaten anhand des Pfads abrufen](connectors-create-api-dropbox.md#get-file-metadata-using-path) |Dieser Vorgang ruft die Metadaten einer Datei unter Verwendung des Pfads ab. |
| [Dateiinhalt anhand des Pfads abrufen](connectors-create-api-dropbox.md#get-file-content-using-path) |Dieser Vorgang ruft den Inhalt einer Datei unter Verwendung des Pfads ab. |
| [Dateiinhalte abrufen](connectors-create-api-dropbox.md#get-file-content) |Dieser Vorgang ruft den Inhalt einer Datei ab. |
| [Datei erstellen](connectors-create-api-dropbox.md#create-file) |Dieser Vorgang erstellt eine Datei. |
| [Datei kopieren](connectors-create-api-dropbox.md#copy-file) |Dieser Vorgang kopiert eine Datei in Dropbox. |
| [Dateien im Ordner aufführen](connectors-create-api-dropbox.md#list-files-in-folder) |Dieser Vorgang ruft die Liste mit den in einem Ordner enthaltenen Dateien und Unterordnern ab. |
| [Dateien im Stammordner aufführen](connectors-create-api-dropbox.md#list-files-in-root-folder) |Dieser Vorgang ruft die Liste mit den im Stammordner enthaltenen Dateien und Unterordnern ab. |
| [Archiv in Ordner extrahieren](connectors-create-api-dropbox.md#extract-archive-to-folder) |Dieser Vorgang extrahiert eine Archivdatei (beispielsweise eine ZIP-Datei) in einen Ordner. |

### <a name="action-details"></a>Aktionsdetails
Im Anschluss finden Sie ausführliche Informationen zu den Aktionen und Triggern für diesen Connector sowie die jeweiligen Antworten:

### <a name="get-file-metadata"></a>Dateimetadaten abrufen
Dieser Vorgang ruft die Metadaten für eine Datei ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| id* |Datei |Datei auswählen |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
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

### <a name="update-file"></a>Datei aktualisieren
Dieser Vorgang aktualisiert eine Datei. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| id* |Datei |Datei auswählen |
| body* |Dateiinhalte |Inhalt der Datei |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
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

### <a name="delete-file"></a>Datei löschen
Dieser Vorgang löscht eine Datei. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| id* |Datei |Datei auswählen |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

### <a name="get-file-metadata-using-path"></a>Dateimetadaten anhand des Pfads abrufen
Dieser Vorgang ruft die Metadaten einer Datei unter Verwendung des Pfads ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| path* |Dateipfad |Datei auswählen |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
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

### <a name="get-file-content-using-path"></a>Dateiinhalt anhand des Pfads abrufen
Dieser Vorgang ruft den Inhalt einer Datei unter Verwendung des Pfads ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| path* |Dateipfad |Datei auswählen |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

### <a name="get-file-content"></a>Dateiinhalte abrufen
Dieser Vorgang ruft den Inhalt einer Datei ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| id* |Datei |Datei auswählen |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

### <a name="create-file"></a>Datei erstellen
Dieser Vorgang erstellt eine Datei. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| folderPath* |Ordnerpfad |Ordner auswählen |
| name* |Dateiname |Name der Datei |
| body* |Dateiinhalte |Inhalt der Datei |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
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

### <a name="copy-file"></a>Datei kopieren
Dieser Vorgang kopiert eine Datei in Dropbox. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| source* |Quell-URL |URL zur Quelldatei |
| destination* |Zieldateipfad |Zieldateipfad einschließlich Zieldateiname |
| overwrite |Überschreiben? |Überschreibt die Zieldatei, falls auf „True“ festgelegt |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
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

### <a name="when-a-file-is-created"></a>Wenn eine Datei erstellt wird
Dieser Vorgang löst einen Fluss aus, wenn in einem Ordner eine neue Datei erstellt wird. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| folderId* |Ordner |Ordner auswählen |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

### <a name="when-a-file-is-modified"></a>Wenn eine Datei geändert wird
Dieser Vorgang löst einen Fluss aus, wenn in einem Ordner eine Datei geändert wird. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| folderId* |Ordner |Ordner auswählen |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

### <a name="list-files-in-folder"></a>Dateien im Ordner aufführen
Dieser Vorgang ruft die Liste mit den in einem Ordner enthaltenen Dateien und Unterordnern ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| id* |Ordner |Ordner auswählen |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
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

### <a name="list-files-in-root-folder"></a>Dateien im Stammordner aufführen
Dieser Vorgang ruft die Liste mit den im Stammordner enthaltenen Dateien und Unterordnern ab. 

Es gibt keine Parameter für diesen Aufruf

#### <a name="output-details"></a>Ausgabedetails
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

### <a name="extract-archive-to-folder"></a>Archiv in Ordner extrahieren
Dieser Vorgang extrahiert eine Archivdatei (beispielsweise eine ZIP-Datei) in einen Ordner. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| source* |Quellarchiv-Dateipfad |Pfad zur Archivdatei |
| destination* |Zielordnerpfad |Pfad, an dem der Archivinhalt extrahiert werden soll |
| overwrite |Überschreiben? |Überschreibt die Zieldateien, falls auf „True“ festgelegt |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
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
Von den oben angegebenen Aktionen und Triggern können folgende HTTP-Statuscodes zurückgegeben werden: 

| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 202 |Zulässig |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler. |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md)




<!--HONumber=Feb17_HO2-->


