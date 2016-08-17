<properties
pageTitle="So wird's gemacht: Verwenden des SFTP-Connectors in Ihren Logik-Apps | Microsoft Azure"
description="Erstellen Sie Logik-Apps mit Azure App Service. Stellen Sie eine Verbindung mit der SFTP-API her, um Dateien zu senden und zu empfangen. Sie können verschiedene Vorgänge ausführen und beispielsweise Dateien erstellen, aktualisieren, abrufen oder löschen."
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
ms.date="07/20/2016"
ms.author="deonhe"/>

# Erste Schritte mit dem SFTP-Connector

Greifen Sie mithilfe des SFTP-Connectors auf ein SFTP-Konto zu, um Dateien zu senden und zu empfangen. Sie können verschiedene Vorgänge ausführen und beispielsweise Dateien erstellen, aktualisieren, abrufen oder löschen.

Wenn Sie einen [Connector](./apis-list.md) verwenden möchten, müssen Sie zuerst eine Logik-App erstellen. Erstellen Sie daher erst einmal eine Logik-App, wie [hier](../app-service-logic/app-service-logic-create-a-logic-app.md) beschrieben.

## Herstellen einer Verbindung mit SFTP

Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, muss zunächst eine *Verbindung* mit dem Dienst hergestellt werden. Eine [Verbindung](./connectors-overview.md) stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her.

### Herstellen einer Verbindung mit SFTP

>[AZURE.INCLUDE [Schritte zum Erstellen einer Verbindung mit SFTP](../../includes/connectors-create-api-sftp.md)]

## Verwenden eines SFTP-Triggers

Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

In diesem Beispiel erfahren Sie, wie Sie mithilfe des Triggers **SFTP - When a file is added or modified** (SFTP – Wenn eine Datei hinzugefügt oder geändert wird) einen Logik-App- Workflow auslösen, wenn einem SFTP-Server eine Datei hinzugefügt oder eine Datei auf einem SFTP-Server geändert wird. Darüber hinaus erfahren Sie in diesem Beispiel, wie Sie eine Bedingung hinzufügen, die den Inhalt der neuen oder geänderten Datei überprüft, und wie Sie entscheiden, dass die Datei extrahiert werden soll, wenn deren Inhalt darauf hindeutet, dass die Datei vor der Verwendung des Inhalts extrahiert werden muss. Und schließlich erfahren Sie, wie Sie eine Aktion hinzufügen, die den Inhalt einer Datei extrahiert und in einem Ordner auf dem SFTP-Server ablegt.

In einem Unternehmensszenario können Sie mit diesem Trigger beispielsweise einen SFTP-Ordner auf neue Dateien überwachen, die jeweils Bestellungen von Kunden darstellen. Anschließend können Sie den Inhalt der Bestellung zur weiteren Verarbeitung und Speicherung in Ihrer Bestelldatenbank mithilfe einer SFTP-Connectoraktion wie **Get file content** (Dateiinhalt abrufen) abrufen.

>[AZURE.INCLUDE [Schritte zum Erstellen eines SFTP-Triggers](../../includes/connectors-create-api-sftp-trigger.md)]

## Bedingung hinzufügen

>[AZURE.INCLUDE [Schritte zum Hinzufügen einer Bedingung](../../includes/connectors-create-api-sftp-condition.md)]

## Verwenden einer SFTP-Aktion

Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

>[AZURE.INCLUDE [Schritte zum Erstellen einer SFTP-Aktion](../../includes/connectors-create-api-sftp-action.md)]


## Technische Details

Im Anschluss finden Sie ausführliche Informationen zu den Triggern, Aktionen und Antworten, die von dieser Verbindung unterstützt werden:

## SFTP-Trigger

Für SFTP stehen folgende Trigger zur Verfügung:

|Trigger | Beschreibung|
|--- | ---|
|[When a file is added or modified](connectors-create-api-sftp.md#when-a-file-is-added-or-modified) (Wenn eine Datei hinzugefügt oder geändert wird)|Dieser Vorgang löst einen Ablauf aus, wenn einem Ordner eine Datei hinzugefügt oder in einem Ordner eine Datei geändert wird.|


## SFTP-Aktionen

Für SFTP stehen folgende Aktionen zur Verfügung:


|Aktion|Beschreibung|
|--- | ---|
|[Dateimetadaten abrufen](connectors-create-api-sftp.md#get-file-metadata)|Dieser Vorgang ruft Dateimetadaten unter Verwendung der Datei-ID ab.|
|[Datei aktualisieren](connectors-create-api-sftp.md#update-file)|Dieser Vorgang aktualisiert Dateiinhalte.|
|[Datei löschen](connectors-create-api-sftp.md#delete-file)|Dieser Vorgang löscht eine Datei.|
|[Dateimetadaten anhand des Pfads abrufen](connectors-create-api-sftp.md#get-file-metadata-using-path)|Dieser Vorgang ruft Dateimetadaten unter Verwendung des Dateipfads ab.|
|[Dateiinhalt anhand des Pfads abrufen](connectors-create-api-sftp.md#get-file-content-using-path)|Dieser Vorgang ruft Dateiinhalte unter Verwendung des Dateipfads ab.|
|[Dateiinhalte abrufen](connectors-create-api-sftp.md#get-file-content)|Dieser Vorgang ruft Dateiinhalte unter Verwendung der Datei-ID ab.|
|[Datei erstellen](connectors-create-api-sftp.md#create-file)|Dieser Vorgang lädt eine Datei an einen SFTP-Server hoch.|
|[Datei kopieren](connectors-create-api-sftp.md#copy-file)|Dieser Vorgang kopiert eine Datei auf einen SFTP-Server.|
|[Dateien im Ordner aufführen](connectors-create-api-sftp.md#list-files-in-folder)|Dieser Vorgang ruft in einem Ordner enthaltene Dateien ab.|
|[Dateien im Stammordner aufführen](connectors-create-api-sftp.md#list-files-in-root-folder)|Dieser Vorgang ruft die Dateien im Stammordner ab.|
|[Ordner extrahieren](connectors-create-api-sftp.md#extract-folder)|Dieser Vorgang extrahiert eine Archivdatei (beispielsweise eine ZIP-Datei) in einen Ordner.|
### Aktionsdetails

Im Anschluss finden Sie ausführliche Informationen zu den Aktionen und Triggern für diesen Connector sowie die jeweiligen Antworten:



### Dateimetadaten abrufen
Dieser Vorgang ruft Dateimetadaten unter Verwendung der Datei-ID ab.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|id*|File|Angeben der Datei|

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
|id*|File|Angeben der Datei|
|body*|Dateiinhalte|Inhalte der zu aktualisierenden Datei|

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
|id*|File|Angeben der Datei|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.




### Dateimetadaten anhand des Pfads abrufen
Dieser Vorgang ruft Dateimetadaten unter Verwendung des Dateipfads ab.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|path*|Dateipfad|Eindeutiger Pfad der Datei|

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
|path*|Dateipfad|Eindeutiger Pfad der Datei|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.




### Dateiinhalte abrufen
Dieser Vorgang ruft Dateiinhalte unter Verwendung der Datei-ID ab.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|id*|File|Angeben der Datei|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.




### Datei erstellen
Dieser Vorgang lädt eine Datei an einen SFTP-Server hoch.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|folderPath*|Ordnerpfad|Eindeutiger Pfad des Ordners|
|name*|Dateiname|Name der Datei|
|body*|Dateiinhalte|Inhalt der zu erstellenden Datei|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### Ausgabedetails

BlobMetadata


|| Eigenschaftenname | Datentyp |
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
Dieser Vorgang kopiert eine Datei auf einen SFTP-Server.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|source*|Quelldateipfad|Pfad zur Quelldatei|
|destination*|Zieldateipfad|Pfad zur Zieldatei, einschließlich des Dateinamens|
|overwrite|Überschreiben?|Überschreibt die Zieldatei, falls auf „True“ festgelegt|

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




### When a file is added or modified (Wenn eine Datei hinzugefügt oder geändert wird)
Dieser Vorgang löst einen Ablauf aus, wenn einem Ordner eine Datei hinzugefügt oder in einem Ordner eine Datei geändert wird.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|folderId*|Ordner|Angeben eines Ordners|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.




### Dateien im Ordner aufführen
Dieser Vorgang ruft in einem Ordner enthaltene Dateien ab.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|id*|Ordner|Angeben des Ordners|

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




### Dateien im Stammordner aufführen
Dieser Vorgang ruft die Dateien im Stammordner ab.


Es gibt keine Parameter für diesen Aufruf

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
Dieser Vorgang extrahiert eine Archivdatei (beispielsweise eine ZIP-Datei) in einen Ordner.


|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|source*|Quellarchiv-Dateipfad|Pfad zur Archivdatei|
|destination*|Zielordnerpfad|Pfad zum Zielordner|
|overwrite|Überschreiben?|Überschreibt die Zieldateien, falls auf „True“ festgelegt|

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

<!---HONumber=AcomDC_0803_2016-->