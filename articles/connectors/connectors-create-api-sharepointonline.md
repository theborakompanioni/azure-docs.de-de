---
title: Verwenden des SharePoint Online-Connectors in Logik-Apps | Microsoft Docs
description: Erstellen Sie Logik-Apps mit dem SharePoint Online-Connector, um Listen in SharePoint zu verwalten.
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: e0ec3149-507a-409d-8e7b-d5fbded006ce
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/19/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: 66fc8f7e1da55dbe6bb1dd8b8d6a535c498c1cf7
ms.openlocfilehash: 3ecf3e30fe2fcb9d6473d7eda450536cddfa97f4


---
# <a name="get-started-with-the-sharepoint-online-connector"></a>Erste Schritte mit dem SharePoint Online-Connector
Verwenden Sie den SharePoint Online-Connector, um SharePoint-Listen zu verwalten.  

Wenn Sie [einen Connector](apis-list.md) verwenden möchten, müssen Sie zuerst eine Logik-App erstellen. Erstellen Sie daher erst einmal eine Logik-App, wie [hier](../logic-apps/logic-apps-create-a-logic-app.md) beschrieben.

## <a name="connect-to-sharepoint-online"></a>Herstellen einer Verbindung mit SharePoint Online
Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, müssen Sie zunächst eine *Verbindung* mit dem Dienst herstellen. Eine [Verbindung](connectors-overview.md) stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her.  

### <a name="create-a-connection-to-sharepoint-online"></a>Erstellen einer Verbindung mit SharePoint Online
> [!INCLUDE [Steps to create a connection to SharePoint](../../includes/connectors-create-api-sharepointonline.md)]
> 
> 

## <a name="use-a-sharepoint-online-trigger"></a>Verwenden eines SharePoint Online-Triggers
Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

> [!INCLUDE [Steps to create a SharePoint Online trigger](../../includes/connectors-create-api-sharepointonline-trigger.md)]
> 
> 

## <a name="use-a-sharepoint-online-action"></a>Verwenden einer SharePoint Online-Aktion
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

> [!INCLUDE [Steps to create a SharePoint Online action](../../includes/connectors-create-api-sharepointonline-action.md)]
> 
> 

## <a name="technical-details"></a>Technische Details
Im Anschluss finden Sie ausführliche Informationen zu den Triggern, Aktionen und Antworten, die von dieser Verbindung unterstützt werden:

## <a name="sharepoint-online-triggers"></a>SharePoint Online-Trigger
Für SharePoint stehen folgende Trigger zur Verfügung:  

| Trigger | Beschreibung |
| --- | --- |
| [Wenn eine Datei erstellt wird](connectors-create-api-sharepointonline.md#when-a-file-is-created) |Dieser Vorgang löst einen Ablauf aus, wenn in einem SharePoint-Ordner eine neue Datei erstellt wird. |
| [Wenn eine Datei geändert wird](connectors-create-api-sharepointonline.md#when-a-file-is-modified) |Dieser Vorgang löst einen Ablauf aus, wenn in einem SharePoint-Ordner eine Datei geändert wird. |
| [Wenn ein neues Element erstellt wird](connectors-create-api-sharepointonline.md#when-a-new-item-is-created) |Dieser Vorgang löst einen Ablauf aus, wenn in einer SharePoint-Liste ein neues Element erstellt wird. |
| [Wenn ein vorhandenes Element geändert wird](connectors-create-api-sharepointonline.md#when-an-existing-item-is-modified) |Dieser Vorgang löst einen Ablauf aus, wenn in einer SharePoint-Liste ein vorhandenes Element geändert wird. |

## <a name="sharepoint-online-actions"></a>SharePoint Online-Aktionen
Für SharePoint stehen folgende Aktionen zur Verfügung:

| Aktion | Beschreibung |
| --- | --- |
| [Dateimetadaten abrufen](connectors-create-api-sharepointonline.md#get-file-metadata) |Dieser Vorgang ruft Dateimetadaten unter Verwendung der Datei-ID ab. |
| [Datei aktualisieren](connectors-create-api-sharepointonline.md#update-file) |Dieser Vorgang aktualisiert Dateiinhalte. |
| [Datei löschen](connectors-create-api-sharepointonline.md#delete-file) |Dieser Vorgang löscht eine Datei. |
| [Dateimetadaten anhand des Pfads abrufen](connectors-create-api-sharepointonline.md#get-file-metadata-using-path) |Dieser Vorgang ruft Dateimetadaten unter Verwendung des Dateipfads ab. |
| [Dateiinhalt anhand des Pfads abrufen](connectors-create-api-sharepointonline.md#get-file-content-using-path) |Dieser Vorgang ruft Dateiinhalte unter Verwendung des Dateipfads ab. |
| [Dateiinhalte abrufen](connectors-create-api-sharepointonline.md#get-file-content) |Dieser Vorgang ruft Dateiinhalte unter Verwendung der Datei-ID ab. |
| [Datei erstellen](connectors-create-api-sharepointonline.md#create-file) |Dieser Vorgang lädt eine Datei an eine SharePoint-Website hoch. |
| [Datei kopieren](connectors-create-api-sharepointonline.md#copy-file) |Dieser Vorgang kopiert eine Datei an eine SharePoint-Website. |
| [Ordner auflisten](connectors-create-api-sharepointonline.md#list-folder) |Dieser Vorgang ruft in einem SharePoint-Ordner enthaltene Dateien ab. |
| [Stammordner auflisten](connectors-create-api-sharepointonline.md#list-root-folder) |Dieser Vorgang ruft die Dateien im SharePoint-Stammordner ab. |
| [Ordner extrahieren](connectors-create-api-sharepointonline.md#extract-folder) |Dieser Vorgang extrahiert eine Archivdatei (beispielsweise eine ZIP-Datei) in einen SharePoint-Ordner. |
| [Elemente abrufen](connectors-create-api-sharepointonline.md#get-items) |Dieser Vorgang ruft Elemente aus einer SharePoint-Liste ab. |
| [Element erstellen](connectors-create-api-sharepointonline.md#create-item) |Dieser Vorgang erstellt ein neues Element in einer SharePoint-Liste. |
| [Element abrufen](connectors-create-api-sharepointonline.md#get-item) |Dieser Vorgang ruft anhand der ID ein einzelnes Element aus einer SharePoint-Liste ab. |
| [Element löschen](connectors-create-api-sharepointonline.md#delete-item) |Dieser Vorgang löscht ein Element aus einer SharePoint-Liste. |
| [Element aktualisieren](connectors-create-api-sharepointonline.md#update-item) |Dieser Vorgang aktualisiert ein Element in einer SharePoint-Liste. |
| [Entitätswerte abrufen](connectors-create-api-sharepointonline.md#get-entity-values) |Dieser Vorgang ruft mögliche Werte für eine SharePoint-Entität ab. |
| [Listen abrufen](connectors-create-api-sharepointonline.md#get-lists) |Dieser Vorgang ruft SharePoint-Listen von einer Website ab. |

### <a name="action-details"></a>Aktionsdetails
Im Anschluss finden Sie ausführliche Informationen zu den Aktionen und Triggern für diesen Connector sowie die jeweiligen Antworten:

### <a name="get-file-metadata"></a>Dateimetadaten abrufen
Dieser Vorgang ruft Dateimetadaten unter Verwendung der Datei-ID ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| dataset* |Website-URL |Eine SharePoint-Website-URL wie z. B. http://contoso.sharepoint.com/sites/mysite. |
| id* |Dateibezeichner |Datei auswählen |

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
| dataset* |Website-URL |Eine SharePoint-Website-URL wie z. B. http://contoso.sharepoint.com/sites/mysite. |
| id* |Dateibezeichner |Datei auswählen |
| body* |Inhalt der Datei |Inhalt der Datei |

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
| dataset* |Website-URL |Eine SharePoint-Website-URL wie z. B. http://contoso.sharepoint.com/sites/mysite. |
| id* |Dateibezeichner |Datei auswählen |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

### <a name="get-file-metadata-using-path"></a>Dateimetadaten anhand des Pfads abrufen
Dieser Vorgang ruft Dateimetadaten unter Verwendung des Dateipfads ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| dataset* |Website-URL |Eine SharePoint-Website-URL wie z. B. http://contoso.sharepoint.com/sites/mysite. |
| path* |Dateipfad |Datei auswählen |

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
| dataset* |Website-URL |Eine SharePoint-Website-URL wie z. B. http://contoso.sharepoint.com/sites/mysite. |
| path* |Dateipfad |Datei auswählen |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

### <a name="get-file-content"></a>Dateiinhalte abrufen
Dieser Vorgang ruft Dateiinhalte unter Verwendung der Datei-ID ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| dataset* |Website-URL |Eine SharePoint-Website-URL wie z. B. http://contoso.sharepoint.com/sites/mysite. |
| id* |Dateibezeichner |Datei auswählen |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

### <a name="create-file"></a>Datei erstellen
Dieser Vorgang lädt eine Datei an eine SharePoint-Website hoch. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| dataset* |Website-URL |Eine SharePoint-Website-URL wie z. B. http://contoso.sharepoint.com/sites/mysite. |
| folderPath* |Ordnerpfad |Datei auswählen |
| name* |Dateiname |Name der Datei |
| body* |Inhalt der Datei |Inhalt der Datei |

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

### <a name="copy-file"></a>Datei kopieren
Dieser Vorgang kopiert eine Datei an eine SharePoint-Website. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| dataset* |Website-URL |Eine SharePoint-Website-URL wie z. B. http://contoso.sharepoint.com/sites/mysite. |
| source* |Quelldateipfad |Pfad zur Quelldatei |
| destination* |Zieldateipfad |Pfad zur Zieldatei |
| overwrite |Überschreibkennzeichen |Gibt an, ob eine ggf. bereits vorhandene Zieldatei überschrieben werden soll. |

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

### <a name="when-a-file-is-created"></a>Wenn eine Datei erstellt wird
Dieser Vorgang löst einen Ablauf aus, wenn in einem SharePoint-Ordner eine neue Datei erstellt wird. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| dataset* |Website-URL |SharePoint-Website-URL |
| folderId* |Ordner-ID |Ordner auswählen |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

### <a name="when-a-file-is-modified"></a>Wenn eine Datei geändert wird
Dieser Vorgang löst einen Ablauf aus, wenn in einem SharePoint-Ordner eine Datei geändert wird. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| dataset* |Website-URL |SharePoint-Website-URL |
| folderId* |Ordner-ID |Ordner auswählen |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

### <a name="list-folder"></a>Ordner auflisten
Dieser Vorgang ruft in einem SharePoint-Ordner enthaltene Dateien ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| dataset* |Website-URL |Eine SharePoint-Website-URL wie z. B. http://contoso.sharepoint.com/sites/mysite. |
| id* |Dateibezeichner |Eindeutiger Bezeichner des Ordners |

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

### <a name="list-root-folder"></a>List root folder (Stammordner auflisten)
Dieser Vorgang ruft die Dateien im SharePoint-Stammordner ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| dataset* |Website-URL |Eine SharePoint-Website-URL wie z. B. http://contoso.sharepoint.com/sites/mysite. |

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

### <a name="extract-folder"></a>Ordner extrahieren
Dieser Vorgang extrahiert eine Archivdatei (beispielsweise eine ZIP-Datei) in einen SharePoint-Ordner. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| dataset* |Website-URL |Eine SharePoint-Website-URL wie z. B. http://contoso.sharepoint.com/sites/mysite. |
| source* |Quelldateipfad |Pfad zur Quelldatei |
| destination* |Zielordnerpfad |Pfad zum Zielordner |
| overwrite |Überschreibkennzeichen |Gibt an, ob eine ggf. bereits vorhandene Zieldatei überschrieben werden soll. |

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

### <a name="when-a-new-item-is-created"></a>When a new item is created (Wenn ein neues Element erstellt wird)
Dieser Vorgang löst einen Ablauf aus, wenn in einer SharePoint-Liste ein neues Element erstellt wird. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| dataset* |Website-URL |URL der SharePoint-Website (Beispiel: http://contoso.sharepoint.com/sites/mysite) |
| table* |Listenname |Name der SharePoint-Liste |
| $filter |Filterabfrage |Eine ODATA-Filterabfrage zum Einschränken der Anzahl zurückgegebener Einträge |
| $orderby |Sortieren nach |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |
| $skip |Auslassungsanzahl |Anzahl der zu überspringenden Einträge (Standardeinstellung = 0) |
| $top |Maximale Anzahl von Get-Anforderungen |Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256) |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
ItemsList

| Eigenschaftenname | Datentyp |
| --- | --- |
| value |array |

### <a name="when-an-existing-item-is-modified"></a>When an existing item is modified (Wenn ein vorhandenes Element geändert wird)
Dieser Vorgang löst einen Ablauf aus, wenn in einer SharePoint-Liste ein vorhandenes Element geändert wird. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| dataset* |Website-URL |URL der SharePoint-Website (Beispiel: http://contoso.sharepoint.com/sites/mysite) |
| table* |Listenname |Name der SharePoint-Liste |
| $filter |Filterabfrage |Eine ODATA-Filterabfrage zum Einschränken der Anzahl zurückgegebener Einträge |
| $orderby |Sortieren nach |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |
| $skip |Auslassungsanzahl |Anzahl der zu überspringenden Einträge (Standardeinstellung = 0) |
| $top |Maximale Anzahl von Get-Anforderungen |Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256) |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
ItemsList

| Eigenschaftenname | Datentyp |
| --- | --- |
| value |array |

### <a name="get-items"></a>Get items (Elemente abrufen)
Dieser Vorgang ruft Elemente aus einer SharePoint-Liste ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| dataset* |Website-URL |URL der SharePoint-Website (Beispiel: http://contoso.sharepoint.com/sites/mysite) |
| table* |Listenname |Name der SharePoint-Liste |
| $filter |Filterabfrage |Eine ODATA-Filterabfrage zum Einschränken der Anzahl zurückgegebener Einträge |
| $orderby |Sortieren nach |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |
| $skip |Auslassungsanzahl |Anzahl der zu überspringenden Einträge (Standardeinstellung = 0) |
| $top |Maximale Anzahl von Get-Anforderungen |Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256) |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
ItemsList

| Eigenschaftenname | Datentyp |
| --- | --- |
| value |array |

### <a name="create-item"></a>Create item (Element erstellen)
Dieser Vorgang erstellt ein neues Element in einer SharePoint-Liste. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| dataset* |Website-URL |URL der SharePoint-Website (Beispiel: http://contoso.sharepoint.com/sites/mysite) |
| table* |Listenname |Name der SharePoint-Liste |
| item* |Item |Zu erstellendes Element |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
Item

| Eigenschaftenname | Datentyp |
| --- | --- |
| ItemInternalId |string |

### <a name="get-item"></a>Get item (Element abrufen)
Dieser Vorgang ruft anhand der ID ein einzelnes Element aus einer SharePoint-Liste ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| dataset* |Website-URL |URL der SharePoint-Website (Beispiel: http://contoso.sharepoint.com/sites/mysite) |
| table* |Listenname |Name der SharePoint-Liste |
| id* |ID |Eindeutiger Bezeichner des abzurufenden Elements |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
Item

| Eigenschaftenname | Datentyp |
| --- | --- |
| ItemInternalId |string |

### <a name="delete-item"></a>Delete item (Element löschen)
Dieser Vorgang löscht ein Element aus einer SharePoint-Liste. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| dataset* |Website-URL |URL der SharePoint-Website (Beispiel: http://contoso.sharepoint.com/sites/mysite) |
| table* |Listenname |Name der SharePoint-Liste |
| id* |ID |Eindeutiger Bezeichner des zu löschenden Elements |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

### <a name="update-item"></a>Update item (Element aktualisieren)
Dieser Vorgang aktualisiert ein Element in einer SharePoint-Liste. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| dataset* |Website-URL |URL der SharePoint-Website (Beispiel: http://contoso.sharepoint.com/sites/mysite) |
| table* |Listenname |Name der SharePoint-Liste |
| id* |ID |Eindeutiger Bezeichner des zu aktualisierenden Elements |
| item* |Item |Element mit geänderten Eigenschaften |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
Item

| Eigenschaftenname | Datentyp |
| --- | --- |
| ItemInternalId |string |

### <a name="get-entity-values"></a>Get entity values (Entitätswerte abrufen)
Dieser Vorgang ruft mögliche Werte für eine SharePoint-Entität ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| dataset* |SharePoint-Website-URL |SharePoint-Website-URL |
| table* |Tabellenname |Tabellenname |
| id* |Entitäts-ID |Entitäts-ID |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
### <a name="get-lists"></a>Get lists (Listen abrufen)
Dieser Vorgang ruft SharePoint-Listen von einer Website ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| dataset* |Website-URL |URL der SharePoint-Website (Beispiel: http://contoso.sharepoint.com/sites/mysite) |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
TablesList

| Eigenschaftenname | Datentyp |
| --- | --- |
| value |array |

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




<!--HONumber=Jan17_HO3-->


