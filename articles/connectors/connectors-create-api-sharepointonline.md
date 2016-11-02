<properties
pageTitle="So wird's gemacht: Verwenden des SharePoint Online-Connectors in Logik-Apps | Microsoft Azure"
description="Erstellen Sie Logik-Apps mit dem SharePoint Online-Connector, um Listen in SharePoint zu verwalten."
services="logic-apps"	
documentationCenter=".net,nodejs,java" 	
authors="msftman"	
manager="erikre"	
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="07/19/2016"
ms.author="deonhe"/>

# Erste Schritte mit dem SharePoint Online-Connector

Verwenden Sie den SharePoint Online-Connector, um SharePoint-Listen zu verwalten.

Wenn Sie einen [Connector](./apis-list.md) verwenden möchten, müssen Sie zuerst eine Logik-App erstellen. Erstellen Sie daher erst einmal eine Logik-App, wie [hier](../app-service-logic/app-service-logic-create-a-logic-app.md) beschrieben.

## Herstellen einer Verbindung mit SharePoint Online

Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, muss zunächst eine *Verbindung* mit dem Dienst hergestellt werden. Eine [Verbindung](./connectors-overview.md) stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her.

### Erstellen einer Verbindung mit SharePoint Online

>[AZURE.INCLUDE [Schritte zum Erstellen einer Verbindung mit SharePoint](../../includes/connectors-create-api-sharepointonline.md)]

## Verwenden eines SharePoint Online-Triggers

Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

>[AZURE.INCLUDE [Schritte zum Erstellen eines SharePoint Online-Triggers](../../includes/connectors-create-api-sharepointonline-trigger.md)]

## Verwenden einer SharePoint Online-Aktion

Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

>[AZURE.INCLUDE [Schritte zum Erstellen einer SharePoint Online-Aktion](../../includes/connectors-create-api-sharepointonline-action.md)]

## Technische Details

Im Anschluss finden Sie ausführliche Informationen zu den Triggern, Aktionen und Antworten, die von dieser Verbindung unterstützt werden:

## SharePoint Online-Trigger

Für SharePoint stehen folgende Trigger zur Verfügung:

|Trigger | Beschreibung|
|--- | ---|
|[Wenn eine Datei erstellt wird](connectors-create-api-sharepointonline.md#when-a-file-is-created)|Dieser Vorgang löst einen Ablauf aus, wenn in einem SharePoint-Ordner eine neue Datei erstellt wird.|
|[Wenn eine Datei geändert wird](connectors-create-api-sharepointonline.md#when-a-file-is-modified)|Dieser Vorgang löst einen Ablauf aus, wenn in einem SharePoint-Ordner eine Datei geändert wird.|
|[When a new item is created](connectors-create-api-sharepointonline.md#when-a-new-item-is-created) (Wenn ein neues Element erstellt wird)|Dieser Vorgang löst einen Ablauf aus, wenn in einer SharePoint-Liste ein neues Element erstellt wird.|
|[When an existing item is modified](connectors-create-api-sharepointonline.md#when-an-existing-item-is-modified) (Wenn ein vorhandenes Element geändert wird)|Dieser Vorgang löst einen Ablauf aus, wenn in einer SharePoint-Liste ein vorhandenes Element geändert wird.|


## SharePoint Online-Aktionen

Für SharePoint stehen folgende Aktionen zur Verfügung:


|Aktion|Beschreibung|
|--- | ---|
|[Dateimetadaten abrufen](connectors-create-api-sharepointonline.md#get-file-metadata)|Dieser Vorgang ruft Dateimetadaten unter Verwendung der Datei-ID ab.|
|[Datei aktualisieren](connectors-create-api-sharepointonline.md#update-file)|Dieser Vorgang aktualisiert Dateiinhalte.|
|[Datei löschen](connectors-create-api-sharepointonline.md#delete-file)|Dieser Vorgang löscht eine Datei.|
|[Dateimetadaten anhand des Pfads abrufen](connectors-create-api-sharepointonline.md#get-file-metadata-using-path)|Dieser Vorgang ruft Dateimetadaten unter Verwendung des Dateipfads ab.|
|[Dateiinhalt anhand des Pfads abrufen](connectors-create-api-sharepointonline.md#get-file-content-using-path)|Dieser Vorgang ruft Dateiinhalte unter Verwendung des Dateipfads ab.|
|[Dateiinhalte abrufen](connectors-create-api-sharepointonline.md#get-file-content)|Dieser Vorgang ruft Dateiinhalte unter Verwendung der Datei-ID ab.|
|[Datei erstellen](connectors-create-api-sharepointonline.md#create-file)|Dieser Vorgang lädt eine Datei an eine SharePoint-Website hoch.|
|[Datei kopieren](connectors-create-api-sharepointonline.md#copy-file)|Dieser Vorgang kopiert eine Datei an eine SharePoint-Website.|
|[Ordner auflisten](connectors-create-api-sharepointonline.md#list-folder)|Dieser Vorgang ruft in einem SharePoint-Ordner enthaltene Dateien ab.|
|[List root folder](connectors-create-api-sharepointonline.md#list-root-folder) (Stammordner auflisten)|Dieser Vorgang ruft die Dateien im SharePoint-Stammordner ab.|
|[Ordner extrahieren](connectors-create-api-sharepointonline.md#extract-folder)|Dieser Vorgang extrahiert eine Archivdatei (beispielsweise eine ZIP-Datei) in einen SharePoint-Ordner.|
|[Get items](connectors-create-api-sharepointonline.md#get-items) (Elemente abrufen)|Dieser Vorgang ruft Elemente aus einer SharePoint-Liste ab.|
|[Create item](connectors-create-api-sharepointonline.md#create-item) (Element erstellen)|Dieser Vorgang erstellt ein neues Element in einer SharePoint-Liste.|
|[Get item](connectors-create-api-sharepointonline.md#get-item) (Element abrufen)|Dieser Vorgang ruft anhand der ID ein einzelnes Element aus einer SharePoint-Liste ab.|
|[Delete item](connectors-create-api-sharepointonline.md#delete-item) (Element löschen)|Dieser Vorgang löscht ein Element aus einer SharePoint-Liste.|
|[Update item](connectors-create-api-sharepointonline.md#update-item) (Element aktualisieren)|Dieser Vorgang aktualisiert ein Element in einer SharePoint-Liste.|
|[Get entity values](connectors-create-api-sharepointonline.md#get-entity-values) (Entitätswerte abrufen)|Dieser Vorgang ruft mögliche Werte für eine SharePoint-Entität ab.|
|[Get lists](connectors-create-api-sharepointonline.md#get-lists) (Listen abrufen)|Dieser Vorgang ruft SharePoint-Listen von einer Website ab.|
### Aktionsdetails

Im Anschluss finden Sie ausführliche Informationen zu den Aktionen und Triggern für diesen Connector sowie die jeweiligen Antworten:



### Dateimetadaten abrufen
Dieser Vorgang ruft Dateimetadaten unter Verwendung der Datei-ID ab.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|dataset*|Website-URL|SharePoint-Website-URL (Beispiel: http://contoso.sharepoint.com/sites/mysite|)
|id*|Dateibezeichner|Datei auswählen|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### Ausgabedetails

BlobMetadata


| Eigenschaftenname | Datentyp |
|---|---|---|
|ID|string|
|Name|string|
|DisplayName|string|
|Path|string|
|LastModified|string|
|Größe|integer|
|MediaType|string|
|IsFolder|Boolescher Wert|
|ETag|string|
|FileLocator|string|




### Datei aktualisieren
Dieser Vorgang aktualisiert Dateiinhalte.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|dataset*|Website-URL|SharePoint-Website-URL (Beispiel: http://contoso.sharepoint.com/sites/mysite|)
|id*|Dateibezeichner|Datei auswählen|
|body*|Inhalt der Datei|Inhalt der Datei|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### Ausgabedetails

BlobMetadata


| Eigenschaftenname | Datentyp |
|---|---|---|
|ID|string|
|Name|string|
|DisplayName|string|
|Path|string|
|LastModified|string|
|Größe|integer|
|MediaType|string|
|IsFolder|Boolescher Wert|
|ETag|string|
|FileLocator|string|




### Datei löschen
Dieser Vorgang löscht eine Datei.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|dataset*|Website-URL|SharePoint-Website-URL (Beispiel: http://contoso.sharepoint.com/sites/mysite|)
|id*|Dateibezeichner|Datei auswählen|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.




### Dateimetadaten anhand des Pfads abrufen
Dieser Vorgang ruft Dateimetadaten unter Verwendung des Dateipfads ab.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|dataset*|Website-URL|SharePoint-Website-URL (Beispiel: http://contoso.sharepoint.com/sites/mysite|)
|path*|Dateipfad|Datei auswählen|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### Ausgabedetails

BlobMetadata


| Eigenschaftenname | Datentyp |
|---|---|---|
|ID|string|
|Name|string|
|DisplayName|string|
|Path|string|
|LastModified|string|
|Größe|integer|
|MediaType|string|
|IsFolder|Boolescher Wert|
|ETag|string|
|FileLocator|string|




### Dateiinhalt anhand des Pfads abrufen
Dieser Vorgang ruft Dateiinhalte unter Verwendung des Dateipfads ab.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|dataset*|Website-URL|SharePoint-Website-URL (Beispiel: http://contoso.sharepoint.com/sites/mysite|)
|path*|Dateipfad|Datei auswählen|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.




### Dateiinhalte abrufen
Dieser Vorgang ruft Dateiinhalte unter Verwendung der Datei-ID ab.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|dataset*|Website-URL|SharePoint-Website-URL (Beispiel: http://contoso.sharepoint.com/sites/mysite|)
|id*|Dateibezeichner|Datei auswählen|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.




### Datei erstellen
Dieser Vorgang lädt eine Datei an eine SharePoint-Website hoch.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|dataset*|Website-URL|SharePoint-Website-URL (Beispiel: http://contoso.sharepoint.com/sites/mysite|)
|folderPath*|Ordnerpfad|Datei auswählen|
|name*|Dateiname|Name der Datei|
|body*|Inhalt der Datei|Inhalt der Datei|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### Ausgabedetails

BlobMetadata


| Eigenschaftenname | Datentyp |
|---|---|---|
|ID|string|
|Name|string|
|DisplayName|string|
|Path|string|
|LastModified|string|
|Größe|integer|
|MediaType|string|
|IsFolder|Boolescher Wert|
|ETag|string|
|FileLocator|string|




### Datei kopieren
Dieser Vorgang kopiert eine Datei an eine SharePoint-Website.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|dataset*|Website-URL|SharePoint-Website-URL (Beispiel: http://contoso.sharepoint.com/sites/mysite|)
|source*|Quelldateipfad|Pfad zur Quelldatei|
|destination*|Zieldateipfad|Pfad zur Zieldatei|
|overwrite|Überschreibkennzeichen|Gibt an, ob eine ggf. bereits vorhandene Zieldatei überschrieben werden soll.|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### Ausgabedetails

BlobMetadata


| Eigenschaftenname | Datentyp |
|---|---|---|
|ID|string|
|Name|string|
|DisplayName|string|
|Path|string|
|LastModified|string|
|Größe|integer|
|MediaType|string|
|IsFolder|Boolescher Wert|
|ETag|string|
|FileLocator|string|




### Wenn eine Datei erstellt wird
Dieser Vorgang löst einen Ablauf aus, wenn in einem SharePoint-Ordner eine neue Datei erstellt wird.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|dataset*|Website-URL|SharePoint-Website-URL|
|folderId*|Ordner-ID|Ordner auswählen|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.




### Wenn eine Datei geändert wird
Dieser Vorgang löst einen Ablauf aus, wenn in einem SharePoint-Ordner eine Datei geändert wird.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|dataset*|Website-URL|SharePoint-Website-URL|
|folderId*|Ordner-ID|Ordner auswählen|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.




### Ordner auflisten
Dieser Vorgang ruft in einem SharePoint-Ordner enthaltene Dateien ab.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|dataset*|Website-URL|SharePoint-Website-URL (Beispiel: http://contoso.sharepoint.com/sites/mysite|)
|id*|Dateibezeichner|Eindeutiger Bezeichner des Ordners|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.



#### Ausgabedetails

BlobMetadata


| Eigenschaftenname | Datentyp |
|---|---|---|
|ID|string|
|Name|string|
|DisplayName|string|
|Path|string|
|LastModified|string|
|Größe|integer|
|MediaType|string|
|IsFolder|Boolescher Wert|
|ETag|string|
|FileLocator|string|




### List root folder (Stammordner auflisten)
Dieser Vorgang ruft die Dateien im SharePoint-Stammordner ab.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|dataset*|Website-URL|SharePoint-Website-URL (Beispiel: http://contoso.sharepoint.com/sites/mysite|)

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.



#### Ausgabedetails

BlobMetadata


| Eigenschaftenname | Datentyp |
|---|---|---|
|ID|string|
|Name|string|
|DisplayName|string|
|Path|string|
|LastModified|string|
|Größe|integer|
|MediaType|string|
|IsFolder|Boolescher Wert|
|ETag|string|
|FileLocator|string|




### Ordner extrahieren
Dieser Vorgang extrahiert eine Archivdatei (beispielsweise eine ZIP-Datei) in einen SharePoint-Ordner.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|dataset*|Website-URL|SharePoint-Website-URL (Beispiel: http://contoso.sharepoint.com/sites/mysite|)
|source*|Quelldateipfad|Pfad zur Quelldatei|
|destination*|Zielordnerpfad|Pfad zum Zielordner|
|overwrite|Überschreibkennzeichen|Gibt an, ob eine ggf. bereits vorhandene Zieldatei überschrieben werden soll.|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.



#### Ausgabedetails

BlobMetadata


| Eigenschaftenname | Datentyp |
|---|---|---|
|ID|string|
|Name|string|
|DisplayName|string|
|Path|string|
|LastModified|string|
|Größe|integer|
|MediaType|string|
|IsFolder|Boolescher Wert|
|ETag|string|
|FileLocator|string|




### When a new item is created (Wenn ein neues Element erstellt wird)
Dieser Vorgang löst einen Ablauf aus, wenn in einer SharePoint-Liste ein neues Element erstellt wird.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|dataset*|Website-URL|SharePoint-Website-URL (Beispiel: http://contoso.sharepoint.com/sites/mysite)|
|table*|Listenname|Name der SharePoint-Liste|
|$filter|Filterabfrage|Eine ODATA-Filterabfrage zum Einschränken der Anzahl zurückgegebener Einträge|
|$orderby|Sortieren nach|Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen|
|$skip|Auslassungsanzahl|Anzahl der zu überspringenden Einträge (Standardeinstellung = 0)|
|$top|Maximale Anzahl von Get-Anforderungen|Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256)|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### Ausgabedetails

ItemsList


| Eigenschaftenname | Datentyp | 
|---|---|
|value|array|




### When an existing item is modified (Wenn ein vorhandenes Element geändert wird)
Dieser Vorgang löst einen Ablauf aus, wenn in einer SharePoint-Liste ein vorhandenes Element geändert wird.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|dataset*|Website-URL|SharePoint-Website-URL (Beispiel: http://contoso.sharepoint.com/sites/mysite)|
|table*|Listenname|Name der SharePoint-Liste|
|$filter|Filterabfrage|Eine ODATA-Filterabfrage zum Einschränken der Anzahl zurückgegebener Einträge|
|$orderby|Sortieren nach|Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen|
|$skip|Auslassungsanzahl|Anzahl der zu überspringenden Einträge (Standardeinstellung = 0)|
|$top|Maximale Anzahl von Get-Anforderungen|Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256)|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### Ausgabedetails

ItemsList


| Eigenschaftenname | Datentyp |
|---|---|
|value|array|




### Get items (Elemente abrufen)
Dieser Vorgang ruft Elemente aus einer SharePoint-Liste ab.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|dataset*|Website-URL|SharePoint-Website-URL (Beispiel: http://contoso.sharepoint.com/sites/mysite)|
|table*|Listenname|Name der SharePoint-Liste|
|$filter|Filterabfrage|Eine ODATA-Filterabfrage zum Einschränken der Anzahl zurückgegebener Einträge|
|$orderby|Sortieren nach|Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen|
|$skip|Auslassungsanzahl|Anzahl der zu überspringenden Einträge (Standardeinstellung = 0)|
|$top|Maximale Anzahl von Get-Anforderungen|Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256)|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### Ausgabedetails

ItemsList


| Eigenschaftenname | Datentyp |
|---|---|
|value|array|




### Create item (Element erstellen)
Dieser Vorgang erstellt ein neues Element in einer SharePoint-Liste.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|dataset*|Website-URL|SharePoint-Website-URL (Beispiel: http://contoso.sharepoint.com/sites/mysite)|
|table*|Listenname|Name der SharePoint-Liste|
|item*|Item|Zu erstellendes Element|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### Ausgabedetails

Item


| Eigenschaftenname | Datentyp |
|---|---|
|ItemInternalId|string|




### Get item (Element abrufen)
Dieser Vorgang ruft anhand der ID ein einzelnes Element aus einer SharePoint-Liste ab.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|dataset*|Website-URL|SharePoint-Website-URL (Beispiel: http://contoso.sharepoint.com/sites/mysite)|
|table*|Listenname|Name der SharePoint-Liste|
|id*|ID|Eindeutiger Bezeichner des abzurufenden Elements|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### Ausgabedetails

Item


| Eigenschaftenname | Datentyp |
|---|---|
|ItemInternalId|string|




### Delete item (Element löschen)
Dieser Vorgang löscht ein Element aus einer SharePoint-Liste.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|dataset*|Website-URL|SharePoint-Website-URL (Beispiel: http://contoso.sharepoint.com/sites/mysite)|
|table*|Listenname|Name der SharePoint-Liste|
|id*|ID|Eindeutiger Bezeichner des zu löschenden Elements|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.




### Update item (Element aktualisieren)
Dieser Vorgang aktualisiert ein Element in einer SharePoint-Liste.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|dataset*|Website-URL|SharePoint-Website-URL (Beispiel: http://contoso.sharepoint.com/sites/mysite)|
|table*|Listenname|Name der SharePoint-Liste|
|id*|ID|Eindeutiger Bezeichner des zu aktualisierenden Elements|
|item*|Item|Element mit geänderten Eigenschaften|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### Ausgabedetails

Item


| Eigenschaftenname | Datentyp |
|---|---|
|ItemInternalId|string|




### Get entity values (Entitätswerte abrufen)
Dieser Vorgang ruft mögliche Werte für eine SharePoint-Entität ab.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|dataset*|SharePoint-Website-URL|SharePoint-Website-URL|
|table*|Tabellenname|Tabellenname|
|id*|Entitäts-ID|Entitäts-ID|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### Ausgabedetails





### Get lists (Listen abrufen)
Dieser Vorgang ruft SharePoint-Listen von einer Website ab.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|dataset*|Website-URL|SharePoint-Website-URL (Beispiel: http://contoso.sharepoint.com/sites/mysite)|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### Ausgabedetails

TablesList


| Eigenschaftenname | Datentyp |
|---|---|
|value|array|



## HTTP-Antworten

Von den oben angegebenen Aktionen und Triggern können folgende HTTP-Statuscodes zurückgegeben werden:

|Name|Beschreibung|
|---|---|
|200|OK|
|202|Zulässig|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler.|
|die Standardeinstellung|Fehler beim Vorgang.|















## Nächste Schritte
[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md)

<!----HONumber=AcomDC_0803_2016-->
