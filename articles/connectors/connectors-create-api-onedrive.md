<properties
	pageTitle="Hinzufügen des OneDrive-Connectors zu Ihren Logik-Apps | Microsoft Azure"
	description="Übersicht über den OneDrive-Connector mit REST-API-Parametern"
	services="app-servicelogic"    
	documentationCenter=""     
	authors="MandiOhlinger"    
	manager="erikre"    
	editor=""
	tags="connectors"/>

<tags
   ms.service="app-service-logic"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/19/2016"
   ms.author="mandia"/>

# Erste Schritte mit dem OneDrive-Connector

Stellen Sie eine Verbindung mit OneDrive her, um Ihre Dateien zu verwalten, d. h. diese hochzuladen, abzurufen, zu löschen usw. OneDrive ermöglicht Folgendes:

- Erstellen Ihres Workflows durch Speichern von Dateien in OneDrive oder Aktualisieren vorhandener Dateien in OneDrive.
- Verwenden von Triggern, um den Workflow zu starten, wenn eine Datei in OneDrive erstellt oder aktualisiert wird.
- Verwenden von Aktionen, um z. B. eine Datei zu kopieren oder zu löschen. So kann beispielsweise bei Eingang einer neuen Office 365-E-Mail mit einer Anlage (Trigger) eine neue Datei in OneDrive erstellt werden (Aktion).

Dieses Thema beschreibt, wie Sie den OneDrive-Connector in einer Logik-App verwenden, und enthält eine Liste mit den Triggern und Aktionen.

>[AZURE.NOTE] Diese Version des Artikels gilt für die allgemein verfügbare Version von Logic Apps.

## Herstellen einer Verbindung mit OneDrive

Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, muss zunächst eine *Verbindung* mit dem Dienst hergestellt werden. Eine Verbindung stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her. Wenn Sie also beispielsweise eine Verbindung mit OneDrive herstellen möchten, müssen Sie zunächst eine entsprechende *Verbindung* erstellen. Geben Sie zum Erstellen einer Verbindung die Anmeldeinformationen ein, mit denen Sie normalerweise auf den Dienst zugreifen, mit dem Sie eine Verbindung herstellen möchten. Geben Sie für OneDrive also die Anmeldeinformationen Ihres OneDrive-Kontos ein, um die Verbindung zu erstellen.

### Erstellen der Verbindung

>[AZURE.INCLUDE [Schritte zum Herstellen einer Verbindung mit OneDrive](../../includes/connectors-create-api-onedrive.md)]

## Verwenden eines Triggers

Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Geben Sie in der Logik-App „onedrive“ ein, um eine Liste mit den Triggern abzurufen:

	![](./media/connectors-create-api-onedrive/onedrive-1.png)

2. Wählen Sie **When a file is modified** (Wenn eine Datei geändert wird) aus. Falls bereits eine Verbindung vorhanden ist, wählen Sie die Schaltfläche **...** (Auswahl anzeigen) aus, um einen Ordner auszuwählen.

	![](./media/connectors-create-api-onedrive/sample-folder.png)

	Wenn Sie zur Anmeldung aufgefordert werden, geben Sie die Anmeldeinformationen ein, um die Verbindung zu erstellen. Die erforderlichen Schritte sind in diesem Thema unter [Erstellen der Verbindung](connectors-create-api-onedrive.md#create-the-connection) aufgeführt.

	> [AZURE.NOTE] In diesem Beispiel wird die Logik-App ausgeführt, wenn eine Datei im ausgewählten Ordner aktualisiert wird. Fügen Sie zum Anzeigen der Ergebnisse dieses Triggers eine weitere Aktion hinzu, die Ihnen eine E-Mail sendet. Fügen Sie beispielsweise die Office 365 Outlook-Aktion *E-Mail senden* hinzu, um eine E-Mail zu erhalten, wenn eine Datei aktualisiert wird.

3. Speichern Sie Ihre Änderungen. (Die Option **Speichern** befindet sich links oben auf der Symbolleiste.) Ihre Logik-App wird gespeichert und ggf. automatisch aktiviert.


## Verwenden einer Aktion

Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Wählen Sie das Pluszeichen aus. Es stehen mehrere Auswahlmöglichkeiten zur Verfügung: **Aktion hinzufügen**, **Bedingung hinzufügen** oder eine der Optionen unter **Mehr**.

	![](./media/connectors-create-api-onedrive/add-action.png)

2. Wählen Sie **Aktion hinzufügen** aus.

3. Geben Sie im Textfeld die Zeichenfolge „onedrive“ ein, um eine Liste mit allen verfügbaren Aktionen zu erhalten.

	![](./media/connectors-create-api-onedrive/onedrive-actions.png)

4. In unserem Beispiel wählen wir **OneDrive - Create file** (OneDrive – Datei erstellen) aus. Falls bereits eine Verbindung vorhanden ist, wählen Sie den Pfad des Zielordners für die Datei aus, geben Sie den Dateinamen ein, und wählen Sie den gewünschten Dateiinhalt aus:

	![](./media/connectors-create-api-onedrive/sample-action.png)

	Wenn Sie zur Eingabe der Verbindungsinformationen aufgefordert werden, geben Sie die Details ein, um die Verbindung zu erstellen. Die Eigenschaften werden in diesem Thema unter [Erstellen der Verbindung](connectors-create-api-onedrive.md#create-the-connection) beschrieben.

	> [AZURE.NOTE] In diesem Beispiel erstellen wir eine neue Datei in einem OneDrive-Ordner. Für die Erstellung der OneDrive-Datei kann die Ausgabe eines anderen Triggers verwendet werden. Fügen Sie beispielsweise den Office 365 Outlook-Trigger *When a new email arrives* (Wenn eine neue E-Mail eingeht) hinzu. Fügen Sie dann die OneDrive-Aktion *Datei erstellen* hinzu, die die Felder „Anlagen“ und „Inhaltstyp“ innerhalb einer ForEach-Schleife verwendet, um die neue Datei in OneDrive zu erstellen.
	> 
	> ![](./media/connectors-create-api-onedrive/foreach-action.png)

5. Speichern Sie Ihre Änderungen. (Die Option **Speichern** befindet sich links oben auf der Symbolleiste.) Ihre Logik-App wird gespeichert und ggf. automatisch aktiviert.


## Technische Details

## Trigger

|Trigger | Beschreibung|
|--- | ---|
|[Wenn eine Datei erstellt wird](connectors-create-api-onedrive.md#when-a-file-is-created)|Dieser Vorgang löst einen Fluss aus, wenn in einem Ordner eine neue Datei erstellt wird.|
|[Wenn eine Datei geändert wird](connectors-create-api-onedrive.md#when-a-file-is-modified)|Dieser Vorgang löst einen Fluss aus, wenn in einem Ordner eine Datei geändert wird.|


## Aktionen

|Aktion|Beschreibung|
|--- | ---|
|[Dateimetadaten abrufen](connectors-create-api-onedrive.md#get-file-metadata)|Dieser Vorgang ruft die Metadaten für eine Datei ab.|
|[Datei aktualisieren](connectors-create-api-onedrive.md#update-file)|Dieser Vorgang aktualisiert eine Datei.|
|[Datei löschen](connectors-create-api-onedrive.md#delete-file)|Dieser Vorgang löscht eine Datei.|
|[Dateimetadaten anhand des Pfads abrufen](connectors-create-api-onedrive.md#get-file-metadata-using-path)|Dieser Vorgang ruft die Metadaten einer Datei unter Verwendung des Pfads ab.|
|[Dateiinhalt anhand des Pfads abrufen](connectors-create-api-onedrive.md#get-file-content-using-path)|Dieser Vorgang ruft den Inhalt einer Datei unter Verwendung des Pfads ab.|
|[Dateiinhalte abrufen](connectors-create-api-onedrive.md#get-file-content)|Dieser Vorgang ruft den Inhalt einer Datei ab.|
|[Datei erstellen](connectors-create-api-onedrive.md#create-file)|Dieser Vorgang erstellt eine Datei.|
|[Datei kopieren](connectors-create-api-onedrive.md#copy-file)|Dieser Vorgang kopiert eine Datei in OneDrive.|
|[Dateien im Ordner aufführen](connectors-create-api-onedrive.md#list-files-in-folder)|Dieser Vorgang ruft die Liste mit den in einem Ordner enthaltenen Dateien und Unterordnern ab.|
|[Dateien im Stammordner aufführen](connectors-create-api-onedrive.md#list-files-in-root-folder)|Dieser Vorgang ruft die Liste mit den im Stammordner enthaltenen Dateien und Unterordnern ab.|
|[Archiv in Ordner extrahieren](connectors-create-api-onedrive.md#extract-archive-to-folder)|Dieser Vorgang extrahiert eine Archivdatei (beispielsweise eine ZIP-Datei) in einen Ordner.|

### Aktionsdetails

Dieser Abschnitt enthält spezifische Details zu den einzelnen Aktionen. Hierzu zählen unter anderem erforderliche oder optionale Eingabeeigenschaften sowie entsprechende Ausgaben im Zusammenhang mit dem Connector.


#### Dateimetadaten abrufen
Dieser Vorgang ruft die Metadaten für eine Datei ab.

|Eigenschaftenname| Anzeigename|Beschreibung|
| ---|---|---|
|id*|File|Datei auswählen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails
BlobMetadata

| Eigenschaftenname | Datentyp |
|---|---|
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


#### Datei aktualisieren
Dieser Vorgang aktualisiert eine Datei.

|Eigenschaftenname| Anzeigename|Beschreibung|
| ---|---|---|
|id*|File|Datei auswählen|
|body*|Dateiinhalte|Inhalt der Datei|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails
BlobMetadata

| Eigenschaftenname | Datentyp |
|---|---|
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


#### Datei löschen
Dieser Vorgang löscht eine Datei.

|Eigenschaftenname| Anzeigename|Beschreibung|
| ---|---|---|
|id*|File|Datei auswählen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails
Keine.

#### Dateimetadaten anhand des Pfads abrufen
Dieser Vorgang ruft die Metadaten einer Datei unter Verwendung des Pfads ab.

|Eigenschaftenname| Anzeigename|Beschreibung|
| ---|---|---|
|path*|Dateipfad|Datei auswählen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails
BlobMetadata

| Eigenschaftenname | Datentyp |
|---|---|
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


#### Dateiinhalt anhand des Pfads abrufen
Dieser Vorgang ruft den Inhalt einer Datei unter Verwendung des Pfads ab.

|Eigenschaftenname| Anzeigename|Beschreibung|
| ---|---|---|
|path*|Dateipfad|Datei auswählen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails
Keine.


#### Dateiinhalte abrufen
Dieser Vorgang ruft den Inhalt einer Datei ab.

|Eigenschaftenname| Anzeigename|Beschreibung|
| ---|---|---|
|id*|File|Datei auswählen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails
Keine.

#### Datei erstellen
Dieser Vorgang erstellt eine Datei.

|Eigenschaftenname| Anzeigename|Beschreibung|
| ---|---|---|
|folderPath*|Ordnerpfad|Ordner auswählen|
|name*|Dateiname|Name der Datei|
|body*|Dateiinhalte|Inhalt der Datei|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails
BlobMetadata

| Eigenschaftenname | Datentyp |
|---|---|
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


#### Datei kopieren
Dieser Vorgang kopiert eine Datei in OneDrive.

|Eigenschaftenname| Anzeigename|Beschreibung|
| ---|---|---|
|source*|Quell-URL|URL zur Quelldatei|
|destination*|Zieldateipfad|Zieldateipfad einschließlich Zieldateiname|
|overwrite|Überschreiben?|Überschreibt die Zieldatei, falls auf „True“ festgelegt|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails
BlobMetadata

| Eigenschaftenname | Datentyp |
|---|---|
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


#### Wenn eine Datei erstellt wird
Dieser Vorgang löst einen Fluss aus, wenn in einem Ordner eine neue Datei erstellt wird.

|Eigenschaftenname| Anzeigename|Beschreibung|
| ---|---|---|
|folderId*|Ordner|Ordner auswählen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails
Keine.

#### Wenn eine Datei geändert wird
Dieser Vorgang löst einen Fluss aus, wenn in einem Ordner eine Datei geändert wird.

|Eigenschaftenname| Anzeigename|Beschreibung|
| ---|---|---|
|folderId*|Ordner|Ordner auswählen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails
Keine.

#### Dateien im Ordner aufführen
Dieser Vorgang ruft die Liste mit den in einem Ordner enthaltenen Dateien und Unterordnern ab.

|Eigenschaftenname| Anzeigename|Beschreibung|
| ---|---|---|
|id*|Ordner|Ordner auswählen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails
BlobMetadata

| Eigenschaftenname | Datentyp |
|---|---|
|ID|string|
|Name|string|
|DisplayName|string|
|Path|string|
|LastModified|string|
|Größe|integer|
|MediaType|string||
|IsFolder|Boolescher Wert|
|ETag|string|
|FileLocator|string|


#### Dateien im Stammordner aufführen
Dieser Vorgang ruft die Liste mit den im Stammordner enthaltenen Dateien und Unterordnern ab.

Es gibt keine Parameter für diesen Aufruf.


##### Ausgabedetails
BlobMetadata

| Eigenschaftenname | Datentyp |
|---|---|
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

#### Archiv in Ordner extrahieren
Dieser Vorgang extrahiert eine Archivdatei (beispielsweise eine ZIP-Datei) in einen Ordner.

|Eigenschaftenname| Anzeigename|Beschreibung|
| ---|---|---|
|source*|Quellarchiv-Dateipfad|Pfad zur Archivdatei|
|destination*|Zielordnerpfad|Pfad, an dem der Archivinhalt extrahiert werden soll|
|overwrite|Überschreiben?|Überschreibt die Zieldateien, falls auf „True“ festgelegt|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails
BlobMetadata

| Eigenschaftenname | Datentyp |
|---|---|
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

In der folgenden Tabelle werden die Antworten auf die Aktionen und Trigger aufgeführt und beschrieben:

|Name|Beschreibung|
|---|---|
|200|OK|
|202|Zulässig|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler ist aufgetreten|
|die Standardeinstellung|Fehler beim Vorgang.|


## Nächste Schritte

[Erstellen Sie eine Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md). Informieren Sie sich in unserer [API-Liste](apis-list.md) über die anderen verfügbaren Connectors für Logic Apps.

<!---HONumber=AcomDC_0720_2016-->