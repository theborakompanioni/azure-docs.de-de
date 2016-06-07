<properties
pageTitle="Verwenden des Office 365-Video-Connectors in Ihren Logik-Apps | Microsoft Azure"
description="Erste Schritte mit dem Office 365-Video-Connector in Ihren Microsoft Azure App Service-Logik-Apps"
services=""    
documentationCenter=""     
authors="msftman"    
manager="erikre"    
editor=""
tags="connectors"/>

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="na"
ms.date="05/18/2016"
ms.author="deonhe"/>

# Erste Schritte mit dem Office 365-Video-Connector
Stellen Sie eine Verbindung mit Office 365 Video her, um Informationen über ein Office 365-Video oder eine Liste von Videos abzurufen und vieles mehr. Der Office 365-Video-Connector kann verwendet werden in:

- Logik-Apps 

>[AZURE.NOTE] Diese Version des Artikels gilt für die Schemaversion 2015-08-01-preview für Logik-Apps. Dieser Connector wird in den vorherigen Schemaversionen nicht unterstützt.

Mit Office 365 Video können Sie folgende Aktionen ausführen:

- Erstellen eines Geschäftsworkflows basierend auf den Daten, die aus Office 365 Video abgerufen werden. 
- Verwenden Sie Aktionen zum Überprüfen des Videoportalstatus, zum Abrufen einer Liste mit allen Videos in einem Kanal und vieles mehr. Diese Aktionen erhalten eine Antwort und stellen anschließend die Ausgabe anderen Aktionen zur Verfügung. Sie können z.B. mit dem Bing-Suche-Connector nach Office 365-Videos suchen und anschließend mithilfe des Office 365-Video-Connectors Informationen zu diesem Video abrufen. Wenn das Video Ihre Anforderungen erfüllt, können Sie es auf Facebook veröffentlichen. 

Informationen zum Hinzufügen eines Vorgangs in Logik-Apps finden Sie unter [Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Trigger und Aktionen

Der Office 365-Video-Connector verfügt über die folgenden Aktionen. Es gibt keine Trigger.

| Trigger | Aktionen|
| --- | --- |
| Keine | <ul><li>Videoportalstatus überprüfen</li><li>Alle anzeigbaren Kanäle abrufen</li><li>Wiedergabe-URL des Azure Media Services-Manifests für ein Video abrufen</li><li>Bearertoken für den Zugriff auf die Videoentschlüsselung abrufen</li><li>Informationen über ein bestimmtes Office 365-Video abrufen</li><li>Alle in einem Kanal vorhandenen Office 365-Videos auflisten</li></ul>

Alle Connectors unterstützen Daten im JSON- und XML-Format.

## Herstellen einer Verbindung mit dem Office 365-Video-Connector
Wenn Sie Ihren Logik-Apps diesen Connector hinzufügen, müssen Sie sich bei Ihrem Office 365-Video-Konto anmelden und den Logik-Apps das Herstellen einer Verbindung mit Ihrem Konto erlauben.

>[AZURE.INCLUDE [Schritte zum Herstellen einer Verbindung mit Office 365-Video](../../includes/connectors-create-api-office365video.md)]

Nachdem Sie eine Verbindung hergestellt haben, geben Sie die Office 365 Video-Eigenschaften ein, z. B. den Mandantennamen oder die Kanal-ID. In der **REST-API-Referenz** in diesem Thema werden diese Eigenschaften beschrieben.

>[AZURE.TIP] Sie können dieselbe Office 365 Video-Verbindung in anderen Logik-Apps verwenden.

## Swagger-REST-API – Referenz
Gilt für Version: 1.0.

### Videoportalstatus überprüfen 
Überprüft den Status des Videoportals, um festzustellen, ob Videodienste aktiviert sind. ```GET: /{tenant}/IsEnabled```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|tenant|string|Ja|path|(Keine)|Der Name des Mandanten für das Verzeichnis, in dem sich der Benutzer befindet|


#### Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|404|Nicht gefunden|
|500|Interner Serverfehler|
|default|Fehler beim Vorgang.|



### Alle anzeigbaren Kanäle abrufen 
Ruft alle Kanäle ab, auf die der Benutzer Anzeigezugriff hat. ```GET: /{tenant}/Channels```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|tenant|string|Ja|path|(Keine)|Der Name des Mandanten für das Verzeichnis, in dem sich der Benutzer befindet|


#### Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|404|Nicht gefunden|
|500|Interner Serverfehler|
|default|Fehler beim Vorgang.|




### Listet alle in einem Kanal vorhandenen Office 365-Videos auf 
Listet alle in einem Kanal vorhandenen Office 365-Videos auf. ```GET: /{tenant}/Channels/{channelId}/Videos```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|tenant|string|Ja|path|(Keine)|Der Name des Mandanten für das Verzeichnis, in dem sich der Benutzer befindet|
|channelId|string|Ja|path|(Keine)|Die Kanal-ID, mit der die Videos abgerufen werden müssen|


#### Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|404|Nicht gefunden|
|500|Interner Serverfehler|
|default|Fehler beim Vorgang.|




### Ruft Informationen über ein bestimmtes Office 365-Video ab 
Ruft Informationen über ein bestimmtes Office 365-Video ab. ```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|tenant|string|Ja|path|(Keine)|Der Name des Mandanten für das Verzeichnis, in dem sich der Benutzer befindet|
|channelId|string|Ja|path|(Keine)|Die Kanal-ID|
|videoId|string|Ja|path|(Keine)|Die Video-ID|


#### Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|404|Nicht gefunden|
|500|Interner Serverfehler|
|default|Fehler beim Vorgang.|




### Ruft die Videowiedergabe-URL des Azure Media Services-Manifests für ein Video ab 
Ruft die Wiedergabe-URL des Azure Media Services-Manifests für ein Video ab. ```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/playbackurl```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|tenant|string|Ja|path|(Keine)|Der Name des Mandanten für das Verzeichnis, in dem sich der Benutzer befindet|
|channelId|string|Ja|path|(Keine)|Die Kanal-ID|
|videoId|string|Ja|path|(Keine)|Die Video-ID|
|streamingFormatType|string|Ja|query|(Keine)|Der Streaming-Formattyp. 1 – Smooth Streaming oder MPEG-DASH. 0 – HLS-Streaming|


#### Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|404|Nicht gefunden|
|500|Interner Serverfehler|
|default|Fehler beim Vorgang.|




### Ruft das Bearertoken für den Zugriff auf die Videoentschlüsselung ab 
Ruft das Bearertoken für den Zugriff auf die Videoentschlüsselung ab. ```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/token```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|tenant|string|Ja|path|(Keine)|Der Name des Mandanten für das Verzeichnis, in dem sich der Benutzer befindet|
|channelId|string|Ja|path|(Keine)|Die Kanal-ID|
|videoId|string|Ja|path|(Keine)|Die Video-ID|


#### Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|404|Nicht gefunden|
|500|Interner Serverfehler|
|default|Fehler beim Vorgang.|


## Objektdefinitionen

#### Kanal: Klasse „Channel“

| Name | Datentyp | Erforderlich|
|---|---|---|
|ID|string|no|
|Titel|string|no|
|Beschreibung|string|no|


#### Video 

| Name | Datentyp |Erforderlich|
|---|---|---|
|ID|string|no|
|Titel|string|no|
|Beschreibung|string|no|
|CreationDate|string|no|
|Besitzer|string|no|
|ThumbnailUrl|string|no|
|VideoUrl|string|no|
|VideoDuration|integer|no|
|VideoProcessingStatus|integer|no|
|ViewCount|integer|no|


## Nächste Schritte
[Erstellen Sie eine Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md).

<!---HONumber=AcomDC_0525_2016-->