---
title: Verwenden des Office 365-Video-Connectors in Ihren Logik-Apps | Microsoft Docs
description: Erste Schritte mit dem Office 365-Video-Connector in Ihren Microsoft Azure App Service-Logik-Apps
services: 
documentationcenter: 
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: 738e5aa7-2523-4116-8b65-211b9063852d
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 959eaca7aafd8516cfc6f3b5dd2a44ce21ec4825


---
# <a name="get-started-with-the-office365-video-connector"></a>Erste Schritte mit dem Office 365-Video-Connector
Stellen Sie eine Verbindung mit Office 365 Video her, um Informationen über ein Office 365-Video oder eine Liste von Videos abzurufen und vieles mehr. Der Office 365-Video-Connector kann verwendet werden in:

* Logik-Apps 

> [!NOTE]
> Diese Version des Artikels gilt für die Schemaversion 2015-08-01-preview für Logik-Apps. Dieser Connector wird in den vorherigen Schemaversionen nicht unterstützt.
> 
> 

Mit Office 365 Video können Sie folgende Aktionen ausführen:

* Erstellen eines Geschäftsworkflows basierend auf den Daten, die aus Office 365 Video abgerufen werden. 
* Verwenden Sie Aktionen zum Überprüfen des Videoportalstatus, zum Abrufen einer Liste mit allen Videos in einem Kanal und vieles mehr. Diese Aktionen erhalten eine Antwort und stellen anschließend die Ausgabe anderen Aktionen zur Verfügung. Sie können z.B. mit dem Bing-Suche-Connector nach Office 365-Videos suchen und anschließend mithilfe des Office 365-Video-Connectors Informationen zu diesem Video abrufen. Wenn das Video Ihre Anforderungen erfüllt, können Sie es auf Facebook veröffentlichen. 

Informationen zum Hinzufügen eines Vorgangs in Logik-Apps finden Sie unter [Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Der Office 365-Video-Connector verfügt über die folgenden Aktionen. Es gibt keine Trigger.

| Trigger | Aktionen |
| --- | --- |
| Keine |<ul><li>Videoportalstatus überprüfen</li><li>Alle anzeigbaren Kanäle abrufen</li><li>Ruft die Videowiedergabe-URL des Azure Media Services-Manifests für ein Video ab</li><li>Ruft das Bearertoken für den Zugriff auf die Videoentschlüsselung ab</li><li>Ruft Informationen über ein bestimmtes Office 365-Video ab</li><li>Listet alle in einem Kanal vorhandenen Office 365-Videos auf</li></ul> |

Alle Connectors unterstützen Daten im JSON- und XML-Format. 

## <a name="create-a-connection-to-office365-video-connector"></a>Herstellen einer Verbindung mit dem Office 365-Video-Connector
Wenn Sie Ihren Logik-Apps diesen Connector hinzufügen, müssen Sie sich bei Ihrem Office 365-Video-Konto anmelden und den Logik-Apps das Herstellen einer Verbindung mit Ihrem Konto erlauben.

> [!INCLUDE [Steps to create a connection to Office 365 Video](../../includes/connectors-create-api-office365video.md)]
> 
> 

Nachdem Sie eine Verbindung hergestellt haben, geben Sie die Office 365 Video-Eigenschaften ein, z. B. den Mandantennamen oder die Kanal-ID. In der **REST-API-Referenz** in diesem Thema werden diese Eigenschaften beschrieben.

> [!TIP]
> Sie können dieselbe Office 365 Video-Verbindung in anderen Logik-Apps verwenden.
> 
> 

## <a name="swagger-rest-api-reference"></a>Swagger-REST-API – Referenz
Gilt für Version: 1.0.

### <a name="checks-video-portal-status"></a>Videoportalstatus überprüfen
Überprüft den Status des Videoportals, um festzustellen, ob Videodienste aktiviert sind.  
```GET: /{tenant}/IsEnabled``` 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| tenant |string |Ja |path |Keine |Der Name des Mandanten für das Verzeichnis, in dem sich der Benutzer befindet |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |BadRequest |
| 401 |Nicht autorisiert |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="get-all-viewable-channels"></a>Alle anzeigbaren Kanäle abrufen
Ruft alle Kanäle ab, auf die der Benutzer Anzeigezugriff hat.  
```GET: /{tenant}/Channels``` 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| tenant |string |Ja |path |Keine |Der Name des Mandanten für das Verzeichnis, in dem sich der Benutzer befindet |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |BadRequest |
| 401 |Nicht autorisiert |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="lists-all-the-office365-videos-present-in-a-channel"></a>Listet alle in einem Kanal vorhandenen Office 365-Videos auf
Listet alle in einem Kanal vorhandenen Office 365-Videos auf.  
```GET: /{tenant}/Channels/{channelId}/Videos``` 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| tenant |string |Ja |path |Keine |Der Name des Mandanten für das Verzeichnis, in dem sich der Benutzer befindet |
| channelId |string |Ja |path |Keine |Die Kanal-ID, mit der die Videos abgerufen werden müssen |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |BadRequest |
| 401 |Nicht autorisiert |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="gets-information-about-a-particular-office365-video"></a>Ruft Informationen über ein bestimmtes Office 365-Video ab
Ruft Informationen über ein bestimmtes Office 365-Video ab.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}``` 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| tenant |string |Ja |path |Keine |Der Name des Mandanten für das Verzeichnis, in dem sich der Benutzer befindet |
| channelId |string |Ja |path |Keine |Die Kanal-ID |
| videoId |string |Ja |path |Keine |Die Video-ID |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |BadRequest |
| 401 |Nicht autorisiert |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="get-playback-url-of-the-azure-media-services-manifest-for-a-video"></a>Ruft die Videowiedergabe-URL des Azure Media Services-Manifests für ein Video ab
Ruft die Wiedergabe-URL des Azure Media Services-Manifests für ein Video ab.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/playbackurl``` 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| tenant |string |Ja |path |Keine |Der Name des Mandanten für das Verzeichnis, in dem sich der Benutzer befindet |
| channelId |string |Ja |path |Keine |Die Kanal-ID |
| videoId |string |Ja |path |Keine |Die Video-ID |
| streamingFormatType |string |Ja |query |Keine |Der Streaming-Formattyp. 1 – Smooth Streaming oder MPEG-DASH. 0 – HLS-Streaming |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |BadRequest |
| 401 |Nicht autorisiert |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="get-the-bearer-token-to-get-access-to-decrypt-the-video"></a>Ruft das Bearertoken für den Zugriff auf die Videoentschlüsselung ab
Ruft das Bearertoken für den Zugriff auf die Videoentschlüsselung ab.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/token```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| tenant |string |Ja |path |Keine |Der Name des Mandanten für das Verzeichnis, in dem sich der Benutzer befindet |
| channelId |string |Ja |path |Keine |Die Kanal-ID |
| videoId |string |Ja |path |Keine |Die Video-ID |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |BadRequest |
| 401 |Nicht autorisiert |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="object-definitions"></a>Objektdefinitionen
#### <a name="channel-channel-class"></a>Kanal: Klasse „Channel“
| Name | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |no |
| Titel |string |no |
| Beschreibung |string |no |

#### <a name="video"></a>Video
| Name | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |no |
| Titel |string |no |
| Beschreibung |string |no |
| CreationDate |string |no |
| Besitzer |string |no |
| ThumbnailUrl |string |no |
| VideoUrl |string |no |
| VideoDuration |integer |no |
| VideoProcessingStatus |integer |no |
| ViewCount |integer |no |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md)




<!--HONumber=Nov16_HO3-->


