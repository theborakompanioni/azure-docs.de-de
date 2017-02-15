---
title: 'So wird&quot;s gemacht: Verwenden des SFTP-Connectors in Ihren Logik-Apps | Microsoft Docs'
description: "Erstellen Sie Logik-Apps mit Azure App Service. Stellen Sie eine Verbindung mit der SFTP-API her, um Dateien zu senden und zu empfangen. Sie können verschiedene Vorgänge ausführen und beispielsweise Dateien erstellen, aktualisieren, abrufen oder löschen."
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: 697eb8b0-4a66-40c7-be7b-6aa6b131c7ad
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/20/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: 66fc8f7e1da55dbe6bb1dd8b8d6a535c498c1cf7
ms.openlocfilehash: 2977404fb408ea5301c88caa7ce6767a906ca9c7


---
# <a name="get-started-with-the-sftp-connector"></a>Erste Schritte mit dem SFTP-Connector
Greifen Sie mithilfe des SFTP-Connectors auf ein SFTP-Konto zu, um Dateien zu senden und zu empfangen. Sie können verschiedene Vorgänge ausführen und beispielsweise Dateien erstellen, aktualisieren, abrufen oder löschen.  

Wenn Sie [einen Connector](apis-list.md) verwenden möchten, müssen Sie zuerst eine Logik-App erstellen. Erstellen Sie daher erst einmal eine Logik-App, wie [hier](../logic-apps/logic-apps-create-a-logic-app.md) beschrieben.

## <a name="connect-to-sftp"></a>Herstellen einer Verbindung mit SFTP
Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, müssen Sie zunächst eine *Verbindung* mit dem Dienst herstellen. Eine [Verbindung](connectors-overview.md) stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her.  

### <a name="create-a-connection-to-sftp"></a>Herstellen einer Verbindung mit SFTP
> [!INCLUDE [Steps to create a connection to SFTP](../../includes/connectors-create-api-sftp.md)]
> 
> 

## <a name="use-an-sftp-trigger"></a>Verwenden eines SFTP-Triggers
Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

In diesem Beispiel erfahren Sie, wie Sie mithilfe des Triggers **SFTP - When a file is added or modified** (SFTP – Wenn eine Datei hinzugefügt oder geändert wird) einen Logik-App- Workflow auslösen, wenn einem SFTP-Server eine Datei hinzugefügt oder eine Datei auf einem SFTP-Server geändert wird. Darüber hinaus erfahren Sie in diesem Beispiel, wie Sie eine Bedingung hinzufügen, die den Inhalt der neuen oder geänderten Datei überprüft, und wie Sie entscheiden, dass die Datei extrahiert werden soll, wenn deren Inhalt darauf hindeutet, dass die Datei vor der Verwendung des Inhalts extrahiert werden muss. Und schließlich erfahren Sie, wie Sie eine Aktion hinzufügen, die den Inhalt einer Datei extrahiert und in einem Ordner auf dem SFTP-Server ablegt. 

In einem Unternehmensszenario können Sie mit diesem Trigger beispielsweise einen SFTP-Ordner auf neue Dateien überwachen, die jeweils Bestellungen von Kunden darstellen.  Anschließend können Sie den Inhalt der Bestellung zur weiteren Verarbeitung und Speicherung in Ihrer Bestelldatenbank mithilfe einer SFTP-Connectoraktion wie **Get file content** (Dateiinhalt abrufen) abrufen.

> [!INCLUDE [Steps to create an SFTP trigger](../../includes/connectors-create-api-sftp-trigger.md)]
> 
> 

## <a name="add-a-condition"></a>Eine Bedingung hinzufügen
> [!INCLUDE [Steps to add a condition](../../includes/connectors-create-api-sftp-condition.md)]
> 
> 

## <a name="use-an-sftp-action"></a>Verwenden einer SFTP-Aktion
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

> [!INCLUDE [Steps to create an SFTP action](../../includes/connectors-create-api-sftp-action.md)]
> 
> 

## <a name="technical-details"></a>Technische Details
Im Anschluss finden Sie ausführliche Informationen zu den Triggern, Aktionen und Antworten, die von dieser Verbindung unterstützt werden:

## <a name="sftp-triggers"></a>SFTP-Trigger
Für SFTP stehen folgende Trigger zur Verfügung:  

| Trigger | Beschreibung |
| --- | --- |
| [Wenn eine Datei hinzugefügt oder geändert wird](connectors-create-api-sftp.md#when-a-file-is-added-or-modified) |Dieser Vorgang löst einen Ablauf aus, wenn einem Ordner eine Datei hinzugefügt oder in einem Ordner eine Datei geändert wird. |

## <a name="sftp-actions"></a>SFTP-Aktionen
Für SFTP stehen folgende Aktionen zur Verfügung:

| Aktion | Beschreibung |
| --- | --- |
| [Dateimetadaten abrufen](connectors-create-api-sftp.md#get-file-metadata) |Dieser Vorgang ruft Dateimetadaten unter Verwendung der Datei-ID ab. |
| [Datei aktualisieren](connectors-create-api-sftp.md#update-file) |Dieser Vorgang aktualisiert Dateiinhalte. |
| [Datei löschen](connectors-create-api-sftp.md#delete-file) |Dieser Vorgang löscht eine Datei. |
| [Dateimetadaten anhand des Pfads abrufen](connectors-create-api-sftp.md#get-file-metadata-using-path) |Dieser Vorgang ruft Dateimetadaten unter Verwendung des Dateipfads ab. |
| [Dateiinhalt anhand des Pfads abrufen](connectors-create-api-sftp.md#get-file-content-using-path) |Dieser Vorgang ruft Dateiinhalte unter Verwendung des Dateipfads ab. |
| [Dateiinhalte abrufen](connectors-create-api-sftp.md#get-file-content) |Dieser Vorgang ruft Dateiinhalte unter Verwendung der Datei-ID ab. |
| [Datei erstellen](connectors-create-api-sftp.md#create-file) |Dieser Vorgang lädt eine Datei an einen SFTP-Server hoch. |
| [Datei kopieren](connectors-create-api-sftp.md#copy-file) |Dieser Vorgang kopiert eine Datei auf einen SFTP-Server. |
| [Dateien im Ordner aufführen](connectors-create-api-sftp.md#list-files-in-folder) |Dieser Vorgang ruft in einem Ordner enthaltene Dateien ab. |
| [Dateien im Stammordner aufführen](connectors-create-api-sftp.md#list-files-in-root-folder) |Dieser Vorgang ruft die Dateien im Stammordner ab. |
| [Ordner extrahieren](connectors-create-api-sftp.md#extract-folder) |Dieser Vorgang extrahiert eine Archivdatei (beispielsweise eine ZIP-Datei) in einen Ordner. |

### <a name="action-details"></a>Aktionsdetails
Im Anschluss finden Sie ausführliche Informationen zu den Aktionen und Triggern für diesen Connector sowie die jeweiligen Antworten:

### <a name="get-file-metadata"></a>Dateimetadaten abrufen
Dieser Vorgang ruft Dateimetadaten unter Verwendung der Datei-ID ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| id* |Datei |Angeben der Datei |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
BlobMetadata

| Eigenschaftenname | Datentyp |
| --- | --- | --- |
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
Dieser Vorgang aktualisiert Dateiinhalte. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| id* |Datei |Angeben der Datei |
| body* |Dateiinhalte |Inhalte der zu aktualisierenden Datei |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
BlobMetadata

| Eigenschaftenname | Datentyp |
| --- | --- | --- |
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
| id* |Datei |Angeben der Datei |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

### <a name="get-file-metadata-using-path"></a>Dateimetadaten anhand des Pfads abrufen
Dieser Vorgang ruft Dateimetadaten unter Verwendung des Dateipfads ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| path* |Dateipfad |Eindeutiger Pfad der Datei |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
BlobMetadata

| Eigenschaftenname | Datentyp |
| --- | --- | --- |
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
Dieser Vorgang ruft Dateiinhalte unter Verwendung des Dateipfads ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| path* |Dateipfad |Eindeutiger Pfad der Datei |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

### <a name="get-file-content"></a>Dateiinhalte abrufen
Dieser Vorgang ruft Dateiinhalte unter Verwendung der Datei-ID ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| id* |Datei |Angeben der Datei |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

### <a name="create-file"></a>Datei erstellen
Dieser Vorgang lädt eine Datei an einen SFTP-Server hoch. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| folderPath* |Ordnerpfad |Eindeutiger Pfad des Ordners |
| name* |Dateiname |Name der Datei |
| body* |Dateiinhalte |Inhalt der zu erstellenden Datei |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
BlobMetadata

|  | Eigenschaftenname | Datentyp |
| --- | --- | --- |
| id |string | |
| Name |string | |
| displayName |string | |
| path |string | |
| LastModified |string | |
| Größe |integer | |
| MediaType |string | |
| IsFolder |Boolescher Wert | |
| ETag |string | |
| FileLocator |string | |

### <a name="copy-file"></a>Datei kopieren
Dieser Vorgang kopiert eine Datei auf einen SFTP-Server. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| source* |Quelldateipfad |Pfad zur Quelldatei |
| destination* |Zieldateipfad |Pfad zur Zieldatei, einschließlich des Dateinamens |
| overwrite |Überschreiben? |Überschreibt die Zieldatei, falls auf „True“ festgelegt |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
BlobMetadata

| Eigenschaftenname | Datentyp |
| --- | --- | --- |
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

### <a name="when-a-file-is-added-or-modified"></a>When a file is added or modified (Wenn eine Datei hinzugefügt oder geändert wird)
Dieser Vorgang löst einen Ablauf aus, wenn einem Ordner eine Datei hinzugefügt oder in einem Ordner eine Datei geändert wird. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| folderId* |Ordner |Angeben eines Ordners |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

### <a name="list-files-in-folder"></a>Dateien im Ordner aufführen
Dieser Vorgang ruft in einem Ordner enthaltene Dateien ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| id* |Ordner |Angeben des Ordners |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
BlobMetadata

| Eigenschaftenname | Datentyp |
| --- | --- | --- |
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
Dieser Vorgang ruft die Dateien im Stammordner ab. 

Es gibt keine Parameter für diesen Aufruf

#### <a name="output-details"></a>Ausgabedetails
BlobMetadata

| Eigenschaftenname | Datentyp |
| --- | --- | --- |
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

### <a name="extract-folder"></a>Ordner extrahieren
Dieser Vorgang extrahiert eine Archivdatei (beispielsweise eine ZIP-Datei) in einen Ordner. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| source* |Quellarchiv-Dateipfad |Pfad zur Archivdatei |
| destination* |Zielordnerpfad |Pfad zum Zielordner |
| overwrite |Überschreiben? |Überschreibt die Zieldateien, falls auf „True“ festgelegt |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
BlobMetadata

| Eigenschaftenname | Datentyp |
| --- | --- | --- |
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


