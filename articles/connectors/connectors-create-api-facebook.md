---
title: Hinzufügen des Facebook-Connectors zu Ihren Logik-Apps | Microsoft Docs
description: Übersicht über den Facebook-Connector mit REST-API-Parametern
services: ''
documentationcenter: ''
author: MandiOhlinger
manager: erikre
editor: ''
tags: connectors

ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: mandia

---
# Erste Schritte mit dem Facebook-Connector
Verbinden Sie sich mit Facebook, um z. B. die Chronik zu ergänzen oder einen Seitenfeed abzurufen.

> [!NOTE]
> Diese Version des Artikels gilt für die Schemaversion 2015-08-01-preview für Logik-Apps.
> 
> 

Facebook ermöglicht Folgendes:

* Erstellen eines Geschäftsworkflows basierend auf den Daten, die aus Facebook abgerufen werden.
* Verwenden eines Triggers, wenn ein neuer Beitrag empfangen wird.
* Verwenden von Aktionen, die z. B. die Chronik ergänzen oder einen Seitenfeed abrufen. Diese Aktionen erhalten eine Antwort und stellen anschließend die Ausgabe anderen Aktionen zur Verfügung. Wenn ein neuer Beitrag in Ihrer Chronik vorhanden ist, können Sie diesen Beitrag per Push an Ihren Twitter-Feed übertragen.

Informationen zum Hinzufügen eines Vorgangs in Logik-Apps finden Sie unter [Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Trigger und Aktionen
Der Facebook-Connector weist die folgenden Trigger und Aktionen auf.

| Trigger | Aktionen |
| --- | --- |
| <ul><li>When there is a new post on my timeline</li></ul> (Wenn ein neuer Beitrag in meiner Chronik vorhanden ist) |<ul><li>Feed aus meiner Chronik abrufen</li><li>Beitrag meiner Chronik hinzufügen</li><li>Wenn ein neuer Beitrag in meiner Chronik vorhanden ist</li><li>Seitenfeed abrufen</li><li>Benutzerchronik abrufen</li><li>Beitrag der Seite hinzufügen</li></ul> |

Alle Connectors unterstützen Daten im JSON- und XML-Format.

## Herstellen einer Verbindung mit Facebook
Wenn Sie diesen Connector Ihren Logik-Apps hinzufügen, müssen Sie ihnen das Herstellen einer Verbindung mit Ihrem Facebook-Konto erlauben.

1. Melden Sie sich bei Ihrem Facebook-Konto an.
2. Wählen Sie **Autorisieren** aus, um zuzulassen, dass Ihre Logik-Apps eine Verbindung mit Ihrem Facebook-Konto herstellen und es nutzen.

> [!INCLUDE [Schritte zum Herstellen einer Verbindung mit Facebook](../../includes/connectors-create-api-facebook.md)]
> 
> [!TIP]
> Sie können dieselbe Facebook-Verbindung in anderen Logik-Apps verwenden.
> 
> 

## Swagger-REST-API – Referenz
Gilt für Version: 1.0.

### Feed aus meiner Chronik abrufen
Ruft die Feeds aus der Chronik des angemeldeten Benutzers ab. ```GET: /me/feed```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| fields |string |no |query |(Keine) |Geben Sie die Felder an, die zurückgegeben werden sollen. Beispiel (id,name,picture). |
| limit |integer |no |query |(Keine) |Maximale Anzahl von Beiträgen, die abgerufen werden sollen |
| durch |string |no |query |(Keine) |Beschränken Sie die Liste der Beiträge auf jene mit angefügtem Speicherort. |
| filter |string |no |query |(Keine) |Rufen Sie nur Beiträge ab, die einen bestimmten Streamfilter entsprechen. |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

### Beitrag meiner Chronik hinzufügen
Sendet eine Statusnachricht an die Chronik des angemeldeten Benutzers. ```POST: /me/feed```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| post |string |Ja |body |(Keine) |Neue zu sendende Nachricht |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

### Wenn ein neuer Beitrag in meiner Chronik vorhanden ist
Löst einen neuen Ablauf aus, wenn ein neuer Beitrag in der Chronik des angemeldeten Benutzers vorhanden ist. ```GET: /trigger/me/feed```

Es gibt keine Parameter.

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

### Seitenfeed abrufen
Ruft Beiträge aus dem Feed einer angegebenen Seite ab. ```GET: /{pageId}/feed```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| pageId |string |Ja |path |(Keine) |ID der Seite, von der Beiträge abgerufen werden sollen. |
| limit |integer |no |query |(Keine) |Maximale Anzahl von Beiträgen, die abgerufen werden sollen |
| include\_hidden |Boolescher Wert |no |query |(Keine) |Angeben, ob Beiträge, die auf der Seite ausgeblendet waren, einbezogen werden oder nicht |
| fields |string |no |query |(Keine) |Geben Sie die Felder an, die zurückgegeben werden sollen. Beispiel (id,name,picture). |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

### Chronik des Benutzers abrufen
Ruft Beiträge aus der Chronik eines Benutzers ab. ```GET: /{userId}/feed```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| userId |string |Ja |path |(Keine) |ID des Benutzers, dessen Chronik abgerufen werden soll. |
| limit |integer |no |query |(Keine) |Maximale Anzahl von Beiträgen, die abgerufen werden sollen |
| durch |string |no |query |(Keine) |Beschränken Sie die Liste der Beiträge auf jene mit angefügtem Speicherort. |
| filter |string |no |query |(Keine) |Rufen Sie nur Beiträge ab, die einen bestimmten Streamfilter entsprechen. |
| fields |string |no |query |(Keine) |Geben Sie die Felder an, die zurückgegeben werden sollen. Beispiel (id,name,picture). |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

### An Seite senden
Veröffentlicht als angemeldeter Benutzer eine Nachricht auf einer Facebook-Seite. ```POST: /{pageId}/feed```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| pageId |string |Ja |path |(Keine) |ID der zu sendenden Seite. |
| post |many |Ja |body |(Keine) |Neue zu sendende Nachricht. |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

## Objektdefinitionen
#### GetFeedResponse
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| data |array |no |

#### TriggerFeedResponse
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| data |array |no |

#### PostItem: Ein einzelner Eintrag im Feed eines Profils.
Das Profil kann ein Benutzer, eine Seite, Anwendung oder Gruppe sein.

| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |no |
| admin\_creator |array |no |
| caption |string |no |
| created\_time |string |no |
| description |string |no |
| feed\_targeting |nicht definiert |no |
| from |nicht definiert |no |
| icon |string |no |
| is\_hidden |Boolescher Wert |no |
| is\_published |Boolescher Wert |no |
| link |string |no |
| message |string |no |
| Name |string |no |
| object\_id |string |no |
| picture |string |no |
| place |nicht definiert |no |
| privacy |nicht definiert |no |
| Eigenschaften |array |no |
| source |string |no |
| status\_type |string |no |
| story |string |no |
| targeting |nicht definiert |no |
| auf fest |array |no |
| Typ |string |no |
| updated\_time |string |no |
| with\_tags |nicht definiert |no |

#### TriggerItem: Ein einzelner Eintrag im Feed eines Profils.
Das Profil kann ein Benutzer, eine Seite, Anwendung oder Gruppe sein.

| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |no |
| created\_time |string |no |
| from |nicht definiert |no |
| message |string |no |
| Typ |string |no |

#### AdminItem
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |no |
| link |string |no |

#### PropertyItem
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Name |string |no |
| Text |string |no |
| href |string |no |

#### UserPostFeedRequest
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| message |string |Ja |
| link |string |no |
| picture |string |no |
| Name |string |no |
| caption |string |no |
| description |string |no |
| place |string |no |
| tags |string |no |
| privacy |nicht definiert |no |
| object\_attachment |string |no |

#### PagePostFeedRequest
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| message |string |Ja |
| link |string |no |
| picture |string |no |
| Name |string |no |
| caption |string |no |
| description |string |no |
| actions |array |no |
| place |string |no |
| tags |string |no |
| object\_attachment |string |no |
| targeting |nicht definiert |no |
| feed\_targeting |nicht definiert |no |
| published |Boolescher Wert |no |
| scheduled\_publish\_time |string |no |
| backdated\_time |string |no |
| backdated\_time\_granularity |string |no |
| child\_attachments |array |no |
| multi\_share\_end\_card |Boolescher Wert |no |

#### PostFeedResponse
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |no |

#### ProfileCollection
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| data |array |no |

#### UserItem
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |no |
| first\_name |string |no |
| last\_name |string |no |
| Name |string |no |
| gender |string |no |
| about |string |no |

#### ActionItem
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Name |string |no |
| link |string |no |

#### TargetItem
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| countries |array |no |
| locales |array |no |
| regions |array |no |
| cities |array |no |

#### FeedTargetItem: Objekt, das Newsfeedziele für diesen Beitrag steuert
Mitglieder dieser Gruppen wird dieser Beitrag wahrscheinlich angezeigt. Bei anderen ist dies weniger wahrscheinlich. Gilt nur für Seiten.

| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| countries |array |no |
| regions |array |no |
| cities |array |no |
| age\_min |integer |no |
| age\_max |integer |no |
| genders |array |no |
| relationship\_statuses |array |no |
| interested\_in |array |no |
| college\_years |array |no |
| interests |array |no |
| relevant\_until |integer |no |
| education\_statuses |array |no |
| locales |array |no |

#### PlaceItem
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |no |
| Name |string |no |
| overall\_rating |number |no |
| location |nicht definiert |no |

#### LocationItem
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| city |string |no |
| country |string |no |
| latitude |number |no |
| located\_in |string |no |
| longitude |number |no |
| Name |string |no |
| region |string |no |
| state |string |no |
| street |string |no |
| zip |string |no |

#### PrivacyItem
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| description |string |no |
| value |string |Ja |
| allow |string |no |
| deny |string |no |
| friends |string |no |

#### ChildAttachmentsItem
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| link |string |no |
| picture |string |no |
| image\_hash |string |no |
| Name |string |no |
| description |string |no |

#### PostPhotoRequest
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| url |string |Ja |
| caption |string |no |

#### PostPhotoResponse
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |Ja |
| post\_id |string |Ja |

#### PostVideoRequest
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| videoData |string |Ja |
| description |string |Ja |
| title |string |Ja |
| uploadedVideoName |string |no |

#### GetPhotoResponse
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| data |nicht definiert |Ja |

#### GetPhotoResponseItem
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| url |string |Ja |
| is\_silhouette |Boolescher Wert |Ja |
| height |string |no |
| width |string |no |

#### GetEventResponse
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| data |array |Ja |

#### GetEventResponseItem
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |Ja |
| Name |string |Ja |
| start\_time |string |no |
| end\_time |string |no |
| timezone |string |no |
| location |string |no |
| description |string |no |
| ticket\_uri |string |no |
| rsvp\_status |string |Ja |

## Nächste Schritte
[Erstellen Sie eine Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md).

<!---HONumber=AcomDC_0824_2016-->