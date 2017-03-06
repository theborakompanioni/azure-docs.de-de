---
title: "Hinzufügen des OneDrive-Connectors zu Ihren Logik-Apps | Microsoft Docs"
description: "Übersicht über den OneDrive-Connector mit REST-API-Parametern"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 47a8582a-1b1a-4fc3-beb5-97c60c4306fe
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: mandia
translationtype: Human Translation
ms.sourcegitcommit: 66fc8f7e1da55dbe6bb1dd8b8d6a535c498c1cf7
ms.openlocfilehash: 60c1d307ede5d66f8e92b048581148d6c7112dac
ms.lasthandoff: 02/16/2017


---
# <a name="get-started-with-the-onedrive-connector"></a>Erste Schritte mit dem OneDrive-Connector
Stellen Sie eine Verbindung mit OneDrive her, um Ihre Dateien zu verwalten, d. h. diese hochzuladen, abzurufen, zu löschen usw. 

OneDrive ermöglicht Folgendes: 

* Erstellen Ihres Workflows durch Speichern von Dateien in OneDrive oder Aktualisieren vorhandener Dateien in OneDrive. 
* Verwenden von Triggern, um den Workflow zu starten, wenn eine Datei in OneDrive erstellt oder aktualisiert wird.
* Verwenden von Aktionen, um z. B. eine Datei zu kopieren oder zu löschen. So kann beispielsweise bei Eingang einer neuen Office 365-E-Mail mit einer Anlage (Trigger) eine neue Datei in OneDrive erstellt werden (Aktion).

Dieses Thema beschreibt, wie Sie den OneDrive-Connector in einer Logik-App verwenden, und enthält eine Liste mit den Triggern und Aktionen.

> [!NOTE]
> Diese Version des Artikels gilt für die allgemein verfügbare Version von Logic Apps. 
> 
> 

Weitere Informationen zu Logik-Apps finden Sie unter [Was sind Logik-Apps](../logic-apps/logic-apps-what-are-logic-apps.md) sowie unter [Erstellen einer Logik-App zum Verbinden von SaaS-Diensten](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-to-onedrive"></a>Herstellen einer Verbindung mit OneDrive
Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, muss zunächst eine *Verbindung* mit dem Dienst hergestellt werden. Eine Verbindung stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her. Wenn Sie also beispielsweise eine Verbindung mit OneDrive herstellen möchten, müssen Sie zunächst eine entsprechende *Verbindung* erstellen. Geben Sie zum Erstellen einer Verbindung die Anmeldeinformationen ein, mit denen Sie normalerweise auf den Dienst zugreifen, mit dem Sie eine Verbindung herstellen möchten. Geben Sie für OneDrive also die Anmeldeinformationen Ihres OneDrive-Kontos ein, um die Verbindung zu erstellen.

### <a name="create-the-connection"></a>Erstellen der Verbindung
> [!INCLUDE [Steps to create a connection to OneDrive](../../includes/connectors-create-api-onedrive.md)]
> 
> 

## <a name="use-a-trigger"></a>Verwenden eines Triggers
Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Trigger fragen den Dienst im gewünschten Intervall und mit der gewünschten Häufigkeit ab. Weitere Informationen zu Triggern finden Sie [hier](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. Geben Sie in der Logik-App „onedrive“ ein, um eine Liste mit den Triggern abzurufen:  
   
    ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Wählen Sie **When a file is modified** (Wenn eine Datei geändert wird) aus. Ist bereits eine Verbindung vorhanden, wählen Sie die Schaltfläche „Auswahl anzeigen“ aus, um einen Ordner auszuwählen.
   
    ![](./media/connectors-create-api-onedrive/sample-folder.png)
   
    Wenn Sie zur Anmeldung aufgefordert werden, geben Sie die Anmeldeinformationen ein, um die Verbindung zu erstellen. Die erforderlichen Schritte sind in diesem Thema unter [Erstellen der Verbindung](connectors-create-api-onedrive.md#create-the-connection) aufgeführt. 
   
   > [!NOTE]
   > In diesem Beispiel wird die Logik-App ausgeführt, wenn eine Datei im ausgewählten Ordner aktualisiert wird. Fügen Sie zum Anzeigen der Ergebnisse dieses Triggers eine weitere Aktion hinzu, die Ihnen eine E-Mail sendet. Fügen Sie beispielsweise die Office 365 Outlook-Aktion *E-Mail senden* hinzu, um eine E-Mail zu erhalten, wenn eine Datei aktualisiert wird. 
   > 
   > 
3. Wählen Sie die Schaltfläche **Bearbeiten** aus, und legen Sie Werte für **Häufigkeit** und **Intervall** fest. Falls die Abfrage durch den Trigger also beispielsweise alle 15 Minuten erfolgen soll, legen Sie **Häufigkeit** auf **Minute** und **Intervall** auf **15** fest. 
   
    ![](./media/connectors-create-api-onedrive/trigger-properties.png)
4. Speichern Sie Ihre Änderungen. (Die Option **Speichern** befindet sich links oben auf der Symbolleiste.) Ihre Logik-App wird gespeichert und ggf. automatisch aktiviert.

## <a name="use-an-action"></a>Verwenden einer Aktion
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. Wählen Sie das Pluszeichen aus. Es stehen mehrere Auswahlmöglichkeiten zur Verfügung: **Aktion hinzufügen**, **Bedingung hinzufügen** oder eine der Optionen unter **Mehr**.
   
    ![](./media/connectors-create-api-onedrive/add-action.png)
2. Wählen Sie **Aktion hinzufügen**aus.
3. Geben Sie im Textfeld die Zeichenfolge „onedrive“ ein, um eine Liste mit allen verfügbaren Aktionen zu erhalten.
   
    ![](./media/connectors-create-api-onedrive/onedrive-actions.png) 
4. In unserem Beispiel wählen wir **OneDrive - Create file** (OneDrive – Datei erstellen) aus. Falls bereits eine Verbindung vorhanden ist, wählen Sie den Pfad des **Zielordners** für die Datei aus, geben Sie den **Dateinamen** ein, und wählen Sie den gewünschten **Dateiinhalt** aus:  
   
    ![](./media/connectors-create-api-onedrive/sample-action.png)
   
    Wenn Sie zur Eingabe der Verbindungsinformationen aufgefordert werden, geben Sie die Details ein, um die Verbindung zu erstellen. Die Eigenschaften werden in diesem Thema unter [Erstellen der Verbindung](connectors-create-api-onedrive.md#create-the-connection) beschrieben. 
   
   > [!NOTE]
   > In diesem Beispiel erstellen wir eine neue Datei in einem OneDrive-Ordner. Für die Erstellung der OneDrive-Datei kann die Ausgabe eines anderen Triggers verwendet werden. Fügen Sie beispielsweise den Office 365 Outlook-Trigger *When a new email arrives* (Wenn eine neue E-Mail eingeht) hinzu. Fügen Sie dann die OneDrive-Aktion *Datei erstellen* hinzu, die die Felder „Anlagen“ und „Inhaltstyp“ innerhalb einer ForEach-Schleife verwendet, um die neue Datei in OneDrive zu erstellen. 
   > 
   > ![](./media/connectors-create-api-onedrive/foreach-action.png)
   > 
   > 
5. Speichern Sie Ihre Änderungen. (Die Option **Speichern** befindet sich links oben auf der Symbolleiste.) Ihre Logik-App wird gespeichert und ggf. automatisch aktiviert.

## <a name="technical-details"></a>Technische Details
## <a name="triggers"></a>Trigger
| Trigger | Beschreibung |
| --- | --- |
| [Wenn eine Datei erstellt wird](connectors-create-api-onedrive.md#when-a-file-is-created) |Dieser Vorgang löst einen Fluss aus, wenn in einem Ordner eine neue Datei erstellt wird. |
| [Wenn eine Datei geändert wird](connectors-create-api-onedrive.md#when-a-file-is-modified) |Dieser Vorgang löst einen Fluss aus, wenn in einem Ordner eine Datei geändert wird. |

## <a name="actions"></a>Actions
| Aktion | Beschreibung |
| --- | --- |
| [Dateimetadaten abrufen](connectors-create-api-onedrive.md#get-file-metadata) |Dieser Vorgang ruft die Metadaten für eine Datei ab. |
| [Datei aktualisieren](connectors-create-api-onedrive.md#update-file) |Dieser Vorgang aktualisiert eine Datei. |
| [Datei löschen](connectors-create-api-onedrive.md#delete-file) |Dieser Vorgang löscht eine Datei. |
| [Dateimetadaten anhand des Pfads abrufen](connectors-create-api-onedrive.md#get-file-metadata-using-path) |Dieser Vorgang ruft die Metadaten einer Datei unter Verwendung des Pfads ab. |
| [Dateiinhalt anhand des Pfads abrufen](connectors-create-api-onedrive.md#get-file-content-using-path) |Dieser Vorgang ruft den Inhalt einer Datei unter Verwendung des Pfads ab. |
| [Dateiinhalte abrufen](connectors-create-api-onedrive.md#get-file-content) |Dieser Vorgang ruft den Inhalt einer Datei ab. |
| [Datei erstellen](connectors-create-api-onedrive.md#create-file) |Dieser Vorgang erstellt eine Datei. |
| [Datei kopieren](connectors-create-api-onedrive.md#copy-file) |Dieser Vorgang kopiert eine Datei in OneDrive. |
| [Dateien im Ordner aufführen](connectors-create-api-onedrive.md#list-files-in-folder) |Dieser Vorgang ruft die Liste mit den in einem Ordner enthaltenen Dateien und Unterordnern ab. |
| [Dateien im Stammordner aufführen](connectors-create-api-onedrive.md#list-files-in-root-folder) |Dieser Vorgang ruft die Liste mit den im Stammordner enthaltenen Dateien und Unterordnern ab. |
| [Archiv in Ordner extrahieren](connectors-create-api-onedrive.md#extract-archive-to-folder) |Dieser Vorgang extrahiert eine Archivdatei (beispielsweise eine ZIP-Datei) in einen Ordner. |

### <a name="action-details"></a>Aktionsdetails
Dieser Abschnitt enthält spezifische Details zu den einzelnen Aktionen. Hierzu zählen unter anderem erforderliche oder optionale Eingabeeigenschaften sowie entsprechende Ausgaben im Zusammenhang mit dem Connector.

#### <a name="get-file-metadata"></a>Dateimetadaten abrufen
Dieser Vorgang ruft die Metadaten für eine Datei ab. 

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
| body* |Dateiinhalte |Inhalt der Datei |

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
Dieser Vorgang ruft die Metadaten einer Datei unter Verwendung des Pfads ab. 

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
Dieser Vorgang ruft den Inhalt einer Datei unter Verwendung des Pfads ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| path* |Dateipfad |Datei auswählen |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine

#### <a name="get-file-content"></a>Dateiinhalte abrufen
Dieser Vorgang ruft den Inhalt einer Datei ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| id* |Datei |Datei auswählen |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine

#### <a name="create-file"></a>Datei erstellen
Dieser Vorgang erstellt eine Datei. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| folderPath* |Ordnerpfad |Ordner auswählen |
| name* |Dateiname |Name der Datei |
| body* |Dateiinhalte |Inhalt der Datei |

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
Dieser Vorgang kopiert eine Datei in OneDrive. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| source* |Quell-URL |URL zur Quelldatei |
| destination* |Zieldateipfad |Zieldateipfad einschließlich Zieldateiname |
| overwrite |Überschreiben? |Überschreibt die Zieldatei, falls auf „True“ festgelegt |

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

#### <a name="when-a-file-is-created"></a>Wenn eine Datei erstellt wird
Dieser Vorgang löst einen Fluss aus, wenn in einem Ordner eine neue Datei erstellt wird. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| folderId* |Ordner |Ordner auswählen |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine

#### <a name="when-a-file-is-modified"></a>Wenn eine Datei geändert wird
Dieser Vorgang löst einen Fluss aus, wenn in einem Ordner eine Datei geändert wird. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| folderId* |Ordner |Ordner auswählen |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine

#### <a name="list-files-in-folder"></a>Dateien im Ordner aufführen
Dieser Vorgang ruft die Liste mit den in einem Ordner enthaltenen Dateien und Unterordnern ab.

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| id* |Ordner |Ordner auswählen |

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

#### <a name="list-files-in-root-folder"></a>Dateien im Stammordner aufführen
Dieser Vorgang ruft die Liste mit den im Stammordner enthaltenen Dateien und Unterordnern ab. 

Es gibt keine Parameter für diesen Aufruf.

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
| source* |Quellarchiv-Dateipfad |Pfad zur Archivdatei |
| destination* |Zielordnerpfad |Pfad, an dem der Archivinhalt extrahiert werden soll |
| overwrite |Überschreiben? |Überschreibt die Zieldateien, falls auf „True“ festgelegt |

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
In der folgenden Tabelle werden die Antworten auf die Aktionen und Trigger aufgeführt und beschrieben:  

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


