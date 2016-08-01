<properties
    pageTitle="Hinzufügen des Azure Blob Storage-Connectors in Ihren Logik-Apps | Microsoft Azure"
    description="Übersicht über den Azure Blob Storage-Connector mit REST-API-Parametern"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="07/18/2016"
   ms.author="mandia"/>

# Erste Schritte mit dem Azure Blob Storage-Connector
Azure Blob Storage ist ein Dienst zur Speicherung großer Mengen unstrukturierter Daten. In Azure Blob Storage können Sie verschiedene Aktionen für Blobs ausführen und sie beispielsweise hochladen, aktualisieren, abrufen oder löschen.

- Gestalten Sie Ihren Workflow, indem Sie neue Projekte hochladen oder kürzlich aktualisierte Dateien abrufen.
- Mithilfe von Aktionen können Sie unter anderem Dateimetadaten abrufen, eine Datei löschen und Dateien kopieren. Wenn also beispielsweise ein Tool auf einer Azure-Website aktualisiert wird (Trigger), kann eine Datei in Blob Storage aktualisiert werden (Aktion).

Dieses Thema beschreibt, wie Sie den Blob Storage-Connector in einer Logik-App verwenden, und enthält eine Liste mit den Aktionen.

>[AZURE.NOTE] Diese Version des Artikels gilt für die allgemein verfügbare Version von Logic Apps.

[Erstellen Sie zunächst eine Logik-App.](../app-service-logic/app-service-logic-create-a-logic-app.md)

>[AZURE.INCLUDE [Voraussetzungen](../../includes/connectors-create-api-azureblobstorage.md)]


## Herstellen einer Verbindung mit Azure Blob Storage

Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, muss zunächst eine *Verbindung* mit dem Dienst hergestellt werden. Eine Verbindung stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her. Wenn Sie also beispielsweise eine Verbindung mit Dropbox herstellen möchten, müssen Sie zunächst eine entsprechende *Verbindung* erstellen. Zum Erstellen einer Verbindung geben Sie die Anmeldeinformationen ein, mit denen Sie normalerweise auf den Dienst zugreifen, mit dem Sie eine Verbindung herstellen möchten. Im Falle des Dropbox-Beispiels geben Sie also Ihre Dropbox-Anmeldeinformationen ein, um die Verbindung mit Dropbox zu erstellen.

Wenn Sie Ihren Logik-Apps diesen Connector hinzufügen, erstellen Sie eine Verbindung mit dem Blob Storage-Konto. Beim erstmaligen Hinzufügen dieses Connectors werden Sie zur Eingabe der Verbindungsinformationen aufgefordert:

![](./media/connectors-create-api-azureblobstorage/connection-details.png)


#### Erstellen der Verbindung

1. Geben Sie die Speicherkontodetails ein. Mit einem Sternchen gekennzeichnete Eigenschaften müssen angegeben werden.

	| Eigenschaft | Details |
|---|---|
| Verbindungsname* | Geben Sie einen beliebigen Namen für Ihre Verbindung ein. |
| Kontoname für Azure-Speicher* | Geben Sie den Namen des Speicherkontos ein. Der Name des Speicherkontos wird im Azure-Portal in den Speichereigenschaften angezeigt. |
| Zugriffsschlüssel für das Azure-Speicherkonto* | Geben Sie den Schlüssel des Speicherkontos ein. Die Zugriffsschlüssel werden im Azure-Portal in den Speichereigenschaften angezeigt. |

	Mit diesen Anmeldeinformationen kann Ihre Logik-App eine Verbindung herstellen und auf Ihre Daten zugreifen. Nach Abschluss des Vorgangs sehen Ihre Verbindungsdetails in etwa wie folgt aus:

	![Schritt zur Erstellung der Azure Blob Storage-Verbindung](./media/connectors-create-api-azureblobstorage/sample-connection.png)

2. Klicken Sie auf **Erstellen**.

 
## Verwenden eines Triggers

Dieser Connector verfügt über keine Trigger. Verwenden Sie andere Trigger, um die Logik-App zu starten – etwa einen Wiederholungstrigger, einen HTTP-Webhook-Trigger oder einen Trigger eines anderen Connectors. Ein Beispiel finden Sie unter [Erstellen einer Logik-App zum Verbinden von SaaS-Diensten](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Verwenden einer Aktion
	
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird.

1. Wählen Sie das Pluszeichen aus. Es stehen mehrere Auswahlmöglichkeiten zur Verfügung: **Aktion hinzufügen**, **Bedingung hinzufügen** oder eine der Optionen unter **Mehr**.

	![](./media/connectors-create-api-azureblobstorage/add-action.png)

2. Wählen Sie **Aktion hinzufügen** aus.

3. Geben Sie im Textfeld die Zeichenfolge „blob“ ein, um eine Liste mit allen verfügbaren Aktionen zu erhalten.

	![](./media/connectors-create-api-azureblobstorage/actions.png)

4. In unserem Beispiel wählen wir **AzureBlob - Get file metadata using path** (AzureBlob – Dateimetadaten anhand des Pfads abrufen) aus. Falls bereits eine Verbindung vorhanden ist, wählen Sie die Schaltfläche **...** (Auswahl anzeigen) aus, um eine Datei auszuwählen.

	![](./media/connectors-create-api-azureblobstorage/sample-file.png)

	Wenn Sie zur Eingabe der Verbindungsinformationen aufgefordert werden, geben Sie die Details ein, um die Verbindung zu erstellen. Die Eigenschaften werden in diesem Thema unter [Erstellen der Verbindung](connectors-create-api-azureblobstorage.md#create-the-connection) beschrieben.

	> [AZURE.NOTE] In diesem Beispiel rufen wir die Metadaten einer Datei ab. Fügen Sie zum Anzeigen der Metadaten eine weitere Aktion hinzu, die unter Verwendung eines anderen Connectors eine neue Datei erstellt. Fügen Sie beispielsweise eine OneDrive-Aktion hinzu, die auf der Grundlage der Metadaten eine neue Testdatei erstellt.

5. Speichern Sie Ihre Änderungen. (Die Option **Speichern** befindet sich links oben auf der Symbolleiste.) Ihre Logik-App wird gespeichert und ggf. automatisch aktiviert.

> [AZURE.TIP] [Storage Explorer](http://storageexplorer.com/) ist ein praktisches Verwaltungstool für mehrere Speicherkonten.

## Technische Details

## Aktionen

|Aktion|Beschreibung|
|--- | ---|
|[Dateimetadaten abrufen](connectors-create-api-azureblobstorage.md#get-file-metadata)|Dieser Vorgang ruft Dateimetadaten unter Verwendung der Datei-ID ab.|
|[Datei aktualisieren](connectors-create-api-azureblobstorage.md#update-file)|Dieser Vorgang aktualisiert eine Datei.|
|[Datei löschen](connectors-create-api-azureblobstorage.md#delete-file)|Dieser Vorgang löscht eine Datei.|
|[Dateimetadaten anhand des Pfads abrufen](connectors-create-api-azureblobstorage.md#get-file-metadata-using-path)|Dieser Vorgang ruft Dateimetadaten unter Verwendung des Pfads ab.|
|[Dateiinhalt anhand des Pfads abrufen](connectors-create-api-azureblobstorage.md#get-file-content-using-path)|Dieser Vorgang ruft Dateiinhalte unter Verwendung des Pfads ab.|
|[Dateiinhalte abrufen](connectors-create-api-azureblobstorage.md#get-file-content)|Dieser Vorgang ruft Dateiinhalte unter Verwendung der ID ab.|
|[Datei erstellen](connectors-create-api-azureblobstorage.md#create-file)|Dieser Vorgang lädt eine Datei hoch.|
|[Datei kopieren](connectors-create-api-azureblobstorage.md#copy-file)|Dieser Vorgang kopiert eine Datei in Azure Blob Storage.|
|[Archiv in Ordner extrahieren](connectors-create-api-azureblobstorage.md#extract-archive-to-folder)|Dieser Vorgang extrahiert eine Archivdatei (beispielsweise eine ZIP-Datei) in einen Ordner.|

### Aktionsdetails

Dieser Abschnitt enthält spezifische Details zu den einzelnen Aktionen. Hierzu zählen unter anderem erforderliche oder optionale Eingabeeigenschaften sowie entsprechende Ausgaben im Zusammenhang mit dem Connector.

#### Dateimetadaten abrufen
Dieser Vorgang ruft Dateimetadaten unter Verwendung der Datei-ID ab.

|Eigenschaftenname| Display Name|Beschreibung|
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

|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|id*|File|Datei auswählen|
|body*|Dateiinhalte|Inhalte der zu aktualisierenden Datei|

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

|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|id*|File|Datei auswählen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails
Keine.


#### Dateimetadaten anhand des Pfads abrufen
Dieser Vorgang ruft Dateimetadaten unter Verwendung des Pfads ab.

|Eigenschaftenname| Display Name|Beschreibung|
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
Dieser Vorgang ruft Dateiinhalte unter Verwendung des Pfads ab.

|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|path*|Dateipfad|Datei auswählen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails
Keine.


#### Dateiinhalte abrufen
Dieser Vorgang ruft Dateiinhalte unter Verwendung der ID ab.

|Eigenschaftenname| Datentyp|Beschreibung|
| ---|---|---|
|id*|string|Datei auswählen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails
Keine.


#### Datei erstellen
Dieser Vorgang lädt eine Datei hoch.

|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|folderPath*|Ordnerpfad|Ordner auswählen|
|name*|Dateiname|Name der hochzuladenden Datei|
|body*|Dateiinhalte|Inhalt der hochzuladenden Datei|

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
Dieser Vorgang kopiert eine Datei in Azure Blob Storage.

|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|source*|Quell-URL|URL zur Quelldatei angeben|
|destination*|Zieldateipfad|Zieldateipfad einschließlich Zieldateiname angeben|
|overwrite|Überschreiben?|Soll eine vorhandene Zieldatei überschrieben werden (True/False)? |

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

#### Archiv in Ordner extrahieren
Dieser Vorgang extrahiert eine Archivdatei (beispielsweise eine ZIP-Datei) in einen Ordner.

|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|source*|Quellarchiv-Dateipfad|Archivdatei auswählen|
|destination*|Zielordnerpfad|Zu extrahierende Inhalte auswählen|
|overwrite|Überschreiben?|Soll eine vorhandene Zieldatei überschrieben werden (True/False)?|

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

Das Aufrufen der verschiedenen Aktionen löst unter Umständen bestimmte Antworten aus. Diese Antworten sind in der folgenden Tabelle aufgeführt und beschrieben:

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

[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md). Untersuchen der anderen verfügbaren Connectors für Logic Apps in unserer [API-Liste](apis-list.md).

<!---HONumber=AcomDC_0720_2016-->