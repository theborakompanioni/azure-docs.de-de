<properties
pageTitle="OneDrive for Business | Microsoft Azure"
description="Erstellen Sie Logik-Apps mit Azure App Service. Stellen Sie eine Verbindung mit OneDrive for Business her und verwalten Sie dort Ihre Dateien. Sie mit den Dateien verschiedene Aktionen ausführen, z.B. Hochladen, Aktualisieren, Abrufen und Löschen."
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
ms.date="08/18/2016"
ms.author="deonhe"/>

# Erste Schritte mit dem OneDrive for Business-Connector

Stellen Sie eine Verbindung mit OneDrive for Business her und verwalten Sie dort Ihre Dateien. Sie mit den Dateien verschiedene Aktionen ausführen, z.B. Hochladen, Aktualisieren, Abrufen und Löschen.

>[AZURE.NOTE] Diese Version des Artikels gilt für die Schemaversion 2015-08-01-preview für Logik-Apps.

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md) beschrieben.

## Trigger und Aktionen

Der OneDrive for Business-Connector kann als Aktion verwendet werden. Er verfügt über Trigger. Alle Connectors unterstützen Daten im JSON- und XML-Format.

 Der OneDrive for Business-Connector verfügt über die folgenden Aktionen und/oder Trigger:

### Aktionen für OneDrive for Business
Sie können diese Aktionen ausführen:

|Aktion|Beschreibung|
|--- | ---|
|[GetFileMetadata](connectors-create-api-onedriveforbusiness.md#getfilemetadata)|Ruft Metadaten einer Datei in OneDrive for Business anhand der ID ab.|
|[UpdateFile](connectors-create-api-onedriveforbusiness.md#updatefile)|Aktualisiert eine Datei auf OneDrive for Business.|
|[DeleteFile](connectors-create-api-onedriveforbusiness.md#deletefile)|Löscht eine Datei aus OneDrive for Business.|
|[GetFileMetadataByPath](connectors-create-api-onedriveforbusiness.md#getfilemetadatabypath)|Ruft Metadaten einer Datei in OneDrive for Business anhand des Pfads ab.|
|[GetFileContentByPath](connectors-create-api-onedriveforbusiness.md#getfilecontentbypath)|Ruft den Inhalt einer Datei in OneDrive for Business anhand des Pfads ab.|
|[GetFileContent](connectors-create-api-onedriveforbusiness.md#getfilecontent)|Ruft den Inhalt einer Datei in OneDrive for Business anhand der ID ab.|
|[CreateFile](connectors-create-api-onedriveforbusiness.md#createfile)|Lädt eine Datei auf OneDrive for Business hoch.|
|[CopyFile](connectors-create-api-onedriveforbusiness.md#copyfile)|Kopiert eine Datei auf OneDrive for Business.|
|[ListFolder](connectors-create-api-onedriveforbusiness.md#listfolder)|Listet die Dateien eines Ordners auf OneDrive for Business auf.|
|[ListRootFolder](connectors-create-api-onedriveforbusiness.md#listrootfolder)|Listet die Dateien des Stammordners auf OneDrive for Business auf.|
|[ExtractFolderV2](connectors-create-api-onedriveforbusiness.md#extractfolderv2)|Extrahiert einen Ordner zu OneDrive for Business.|
### Trigger für OneDrive for Business
Sie können auf diese Ereignisse lauschen:

|Trigger | Beschreibung|
|--- | ---|
|Wenn eine Datei erstellt wird|Löst einen Workflow aus, wenn in einem OneDrive for Business-Ordner eine neue Datei erstellt wird.|
|Wenn eine Datei geändert wird|Löst einen Workflow aus, wenn in einem OneDrive for Business-Ordner eine Datei geändert wird.|


## Herstellen einer Verbindung mit OneDrive for Business
Um Logik-Apps mit OneDrive for Business zu erstellen, müssen Sie zuerst eine **Verbindung** erstellen und anschließend die Details für die folgenden Eigenschaften angeben:

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|Token|Ja|Angeben der Anmeldeinformationen für OneDrive for Business|
Nachdem Sie die Verbindung erstellt haben, können Sie sie zum Ausführen der Aktionen und zum Lauschen auf die in diesem Artikel beschriebenen Trigger verwenden.

>[AZURE.INCLUDE [Schritte zum Herstellen einer Verbindung mit OneDrive for Business](../../includes/connectors-create-api-onedriveforbusiness.md)]

>[AZURE.TIP] Sie können diese Verbindung in anderen Logik-Apps verwenden.

## Referenz für OneDrive for Business
Gilt für Version 1.0.

## GetFileMetadata
Dateimetadaten anhand der ID abrufen: Ruft Metadaten einer Datei in OneDrive for Business anhand der ID ab.

```GET: /datasets/default/files/{id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|id|string|Ja|path|(Keine)|Angeben der Datei|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|die Standardeinstellung|Fehler beim Vorgang.|


## UpdateFile
Datei aktualisieren: Aktualisiert eine Datei auf OneDrive for Business.

```PUT: /datasets/default/files/{id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|id|string|Ja|path|(Keine)|Angeben der zu aktualisierenden Datei|
|body| |Ja|body|(Keine)|Inhalt der Datei, die in OneDrive for Business aktualisiert werden soll|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|die Standardeinstellung|Fehler beim Vorgang.|


## DeleteFile
Datei löschen: Löscht eine Datei aus OneDrive for Business.

```DELETE: /datasets/default/files/{id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|id|string|Ja|path|(Keine)|Angeben der zu löschenden Datei|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|die Standardeinstellung|Fehler beim Vorgang.|


## GetFileMetadataByPath
Dateimetadaten anhand des Pfads abrufen: Ruft Metadaten einer Datei in OneDrive for Business anhand des Pfads ab.

```GET: /datasets/default/GetFileByPath```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|path|string|Ja|query|(Keine)|Eindeutiger Pfad zur Datei in OneDrive for Business|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|die Standardeinstellung|Fehler beim Vorgang.|


## GetFileContentByPath
Dateiinhalt anhand des Pfads abrufen: Ruft den Inhalt einer Datei in OneDrive for Business anhand des Pfads ab.

```GET: /datasets/default/GetFileContentByPath```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|path|string|Ja|query|(Keine)|Eindeutiger Pfad zur Datei in OneDrive for Business|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|die Standardeinstellung|Fehler beim Vorgang.|


## GetFileContent
Dateiinhalt anhand der ID abrufen: Ruft den Inhalt einer Datei in OneDrive for Business anhand der ID ab.

```GET: /datasets/default/files/{id}/content```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|id|string|Ja|path|(Keine)|Angeben der Datei|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|die Standardeinstellung|Fehler beim Vorgang.|


## CreateFile
Datei erstellen: Lädt eine Datei auf OneDrive for Business hoch.

```POST: /datasets/default/files```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|folderPath|string|Ja|query|(Keine)|Ordnerpfad zum Hochladen der Datei in OneDrive for Business|
|Name|string|Ja|query|(Keine)|Name der Datei, die in OneDrive for Business erstellt werden soll|
|body| |Ja|body|(Keine)|Inhalt der Datei, die in OneDrive for Business hochgeladen werden soll|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|die Standardeinstellung|Fehler beim Vorgang.|


## CopyFile
Datei kopieren: Kopiert eine Datei auf OneDrive for Business.

```POST: /datasets/default/copyFile```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|source|string|Ja|query|(Keine)|URL zur Quelldatei|
|destination|string|Ja|query|(Keine)|Zieldateipfad in OneDrive for Business, einschließlich Zieldateiname|
|overwrite|Boolescher Wert|no|query|false|Überschreibt die Zieldatei, falls auf „True“ festgelegt|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|die Standardeinstellung|Fehler beim Vorgang.|


## OnNewFile
Wenn eine Datei erstellt wird: Löst einen Workflow aus, wenn in einem OneDrive for Business-Ordner eine neue Datei erstellt wird.

```GET: /datasets/default/triggers/onnewfile```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|folderId|string|Ja|query|(Keine)|Angeben eines Ordners|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|die Standardeinstellung|Fehler beim Vorgang.|


## OnUpdatedFile
Wenn eine Datei geändert wird: Löst einen Workflow aus, wenn in einem OneDrive for Business-Ordner eine Datei geändert wird.

```GET: /datasets/default/triggers/onupdatedfile```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|folderId|string|Ja|query|(Keine)|Angeben eines Ordners|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|die Standardeinstellung|Fehler beim Vorgang.|


## ListFolder
Dateien in einem Ordner auflisten: Listet die Dateien eines Ordners auf OneDrive for Business auf.

```GET: /datasets/default/folders/{id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|id|string|Ja|path|(Keine)|Angeben des Ordners|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|die Standardeinstellung|Fehler beim Vorgang.|


## ListRootFolder
Stammordner auflisten: Listet die Dateien des Stammordners auf OneDrive for Business auf.

```GET: /datasets/default/folders```

Es gibt keine Parameter für diesen Aufruf
#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|die Standardeinstellung|Fehler beim Vorgang.|


## ExtractFolderV2
Ordner extrahieren: Extrahiert einen Ordner zu OneDrive for Business.

```POST: /datasets/default/extractFolderV2```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|source|string|Ja|query|(Keine)|Pfad zur Archivdatei|
|destination|string|Ja|query|(Keine)|Pfad in OneDrive for Business, in den der Archivinhalt extrahiert wird|
|overwrite|Boolescher Wert|no|query|false|Überschreibt die Zieldateien, falls auf „True“ festgelegt|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|die Standardeinstellung|Fehler beim Vorgang.|


## Objektdefinitionen 

### DataSetsMetadata


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|tabular|nicht definiert|Nein |
|Blob|nicht definiert|Nein |



### TabularDataSetsMetadata


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|source|string|Nein |
|displayName|string|Nein |
|urlEncoding|string|Nein |
|tableDisplayName|string|Nein |
|tablePluralName|string|Nein |



### BlobDataSetsMetadata


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|source|string|Nein |
|displayName|string|Nein |
|urlEncoding|string|Nein |



### BlobMetadata


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ID|string|Nein |
|Name|string|Nein |
|DisplayName|string|Nein |
|Pfad|string|Nein |
|LastModified|string|Nein |
|Size|integer|Nein |
|MediaType|string|Nein |
|IsFolder|Boolescher Wert|Nein |
|ETag|string|Nein |
|FileLocator|string|Nein |



### Objekt


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|


## Nächste Schritte
[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md)

<!---HONumber=AcomDC_0824_2016-->