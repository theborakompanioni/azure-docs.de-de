---
title: "Hinzufügen des Facebook-Connectors zu Ihren Logik-Apps | Microsoft Docs"
description: "Übersicht über den Facebook-Connector mit REST-API-Parametern"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: f4d6f0ed-c09b-488c-be1c-8cf2b5b1d4b8
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/07/2016
ms.author: mandia
translationtype: Human Translation
ms.sourcegitcommit: b92f954680603891ced503a1134791312b5214f0
ms.openlocfilehash: b8a66308c4f4f1df610cdacd092ef133bd605665


---
# <a name="get-started-with-the-facebook-connector"></a>Erste Schritte mit dem Facebook-Connector
Verbinden Sie sich mit Facebook, um z. B. die Chronik zu ergänzen oder einen Seitenfeed abzurufen. 

> [!NOTE]
> Diese Version des Artikels gilt für die Schemaversion 2015-08-01-preview für Logik-Apps.
> 
> 

Facebook ermöglicht Folgendes:

* Erstellen eines Geschäftsworkflows basierend auf den Daten, die aus Facebook abgerufen werden. 
* Verwenden eines Triggers, wenn ein neuer Beitrag empfangen wird.
* Verwenden von Aktionen, die z. B. die Chronik ergänzen oder einen Seitenfeed abrufen. Diese Aktionen erhalten eine Antwort und stellen anschließend die Ausgabe anderen Aktionen zur Verfügung. Wenn ein neuer Beitrag in Ihrer Chronik vorhanden ist, können Sie diesen Beitrag per Push an Ihren Twitter-Feed übertragen. 

Informationen zum Hinzufügen eines Vorgangs in Logik-Apps finden Sie unter [Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Der Facebook-Connector weist die folgenden Trigger und Aktionen auf. 

| Trigger | Aktionen |
| --- | --- |
| <ul><li>Wenn ein neuer Beitrag in meiner Chronik vorhanden ist</li></ul> |<ul><li>Feed aus meiner Chronik abrufen</li><li>Beitrag meiner Chronik hinzufügen</li><li>Wenn ein neuer Beitrag in meiner Chronik vorhanden ist</li><li>Seitenfeed abrufen</li><li>Chronik des Benutzers abrufen</li><li>An Seite senden</li></ul> |

Alle Connectors unterstützen Daten im JSON- und XML-Format.

## <a name="create-a-connection-to-facebook"></a>Herstellen einer Verbindung mit Facebook
Wenn Sie diesen Connector Ihren Logik-Apps hinzufügen, müssen Sie ihnen das Herstellen einer Verbindung mit Ihrem Facebook-Konto erlauben.

1. Melden Sie sich bei Ihrem Facebook-Konto an.
2. Wählen Sie **Autorisieren**aus, um zu erlauben, dass Ihre Logik-Apps sich mit Ihrem Facebook-Konto verbinden und dieses nutzen. 

> [!INCLUDE [Steps to create a connection to Facebook](../../includes/connectors-create-api-facebook.md)]
> 
> [!TIP]
> Sie können dieselbe Facebook-Verbindung in anderen Logik-Apps verwenden.
> 
> 

## <a name="swagger-rest-api-reference"></a>Swagger-REST-API – Referenz
Gilt für Version: 1.0.

### <a name="get-feed-from-my-timeline"></a>Feed aus meiner Chronik abrufen
Ruft die Feeds aus der Chronik des angemeldeten Benutzers ab.  
```GET: /me/feed```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| fields |string |no |query |(Keine) |Geben Sie die Felder an, die zurückgegeben werden sollen. Beispiel (id,name,picture). |
| limit |integer |no |query |(Keine) |Maximale Anzahl von Beiträgen, die abgerufen werden sollen |
| durch |string |no |query |(Keine) |Beschränken Sie die Liste der Beiträge auf jene mit angefügtem Speicherort. |
| filter |string |no |query |(Keine) |Rufen Sie nur Beiträge ab, die einen bestimmten Streamfilter entsprechen. |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="post-to-my-timeline"></a>Beitrag meiner Chronik hinzufügen
Senden einer Statusnachricht an die Chronik des angemeldeten Benutzers.  
```POST: /me/feed```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| post |string |Ja |body |(Keine) |Neue zu sendende Nachricht |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="when-there-is-a-new-post-on-my-timeline"></a>Wenn ein neuer Beitrag in meiner Chronik vorhanden ist
Löst einen neuen Datenfluss aus, wenn ein neuer Beitrag in der Chronik des angemeldeten Benutzers vorhanden ist.  
```GET: /trigger/me/feed```

Es gibt keine Parameter. 

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="get-page-feed"></a>Seitenfeed abrufen
Ruft Beiträge aus dem Feed einer angegebenen Seite ab.  
```GET: /{pageId}/feed```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| pageId |string |Ja |path |(Keine) |ID der Seite, von der Beiträge abgerufen werden sollen. |
| limit |integer |no |query |(Keine) |Maximale Anzahl von Beiträgen, die abgerufen werden sollen |
| include_hidden |Boolescher Wert |no |query |(Keine) |Angeben, ob Beiträge, die auf der Seite ausgeblendet waren, einbezogen werden oder nicht |
| fields |string |no |query |(Keine) |Geben Sie die Felder an, die zurückgegeben werden sollen. Beispiel (id,name,picture). |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="get-user-timeline"></a>Chronik des Benutzers abrufen
Ruft Beiträge aus der Chronik eines Benutzers ab.  
```GET: /{userId}/feed```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| bei der ersten |string |Ja |path |(Keine) |ID des Benutzers, dessen Chronik abgerufen werden soll. |
| limit |integer |no |query |(Keine) |Maximale Anzahl von Beiträgen, die abgerufen werden sollen |
| durch |string |no |query |(Keine) |Beschränken Sie die Liste der Beiträge auf jene mit angefügtem Speicherort. |
| filter |string |no |query |(Keine) |Rufen Sie nur Beiträge ab, die einen bestimmten Streamfilter entsprechen. |
| fields |string |no |query |(Keine) |Geben Sie die Felder an, die zurückgegeben werden sollen. Beispiel (id,name,picture). |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="post-to-page"></a>An Seite senden
Nachricht als angemeldeter Benutzer an eine Facebook-Seite senden.  
```POST: /{pageId}/feed```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| pageId |string |Ja |path |(Keine) |ID der zu sendenden Seite. |
| post |many |Ja |body |(Keine) |Neue zu sendende Nachricht. |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="object-definitions"></a>Objektdefinitionen
#### <a name="getfeedresponse"></a>GetFeedResponse
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| data |array |no |

#### <a name="triggerfeedresponse"></a>TriggerFeedResponse
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| data |array |no |

#### <a name="postitem-a-single-entry-in-a-profiles-feed"></a>PostItem: Ein einzelner Eintrag im Feed eines Profils.
Das Profil kann ein Benutzer, eine Seite, Anwendung oder Gruppe sein. 

| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |no |
| admin_creator |array |no |
| caption |string |no |
| created_time |string |no |
| Beschreibung |string |no |
| feed_targeting |nicht definiert |no |
| from |nicht definiert |no |
| icon |string |no |
| is_hidden |Boolescher Wert |no |
| is_published |Boolescher Wert |no |
| link |string |no |
| message |string |no |
| Name |string |no |
| object_id |string |no |
| picture |string |no |
| place |nicht definiert |no |
| privacy |nicht definiert |no |
| Eigenschaften |array |no |
| Quelle |string |no |
| status_type |string |no |
| story |string |no |
| targeting |nicht definiert |no |
| auf fest |array |no |
| type |string |no |
| updated_time |string |no |
| with_tags |nicht definiert |no |

#### <a name="triggeritem-a-single-entry-in-a-profiles-feed"></a>TriggerItem: Ein einzelner Eintrag im Feed eines Profils.
Das Profil kann ein Benutzer, eine Seite, Anwendung oder Gruppe sein.

| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |no |
| created_time |string |no |
| from |nicht definiert |no |
| message |string |no |
| type |string |no |

#### <a name="adminitem"></a>AdminItem
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |no |
| link |string |no |

#### <a name="propertyitem"></a>PropertyItem
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Name |string |no |
| Text |string |no |
| href |string |no |

#### <a name="userpostfeedrequest"></a>UserPostFeedRequest
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| message |string |Ja |
| link |string |no |
| picture |string |no |
| Name |string |no |
| caption |string |no |
| Beschreibung |string |no |
| place |string |no |
| tags |string |no |
| privacy |nicht definiert |no |
| object_attachment |string |no |

#### <a name="pagepostfeedrequest"></a>PagePostFeedRequest
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| message |string |Ja |
| link |string |no |
| picture |string |no |
| Name |string |no |
| caption |string |no |
| Beschreibung |string |no |
| Aktionen |array |no |
| place |string |no |
| tags |string |no |
| object_attachment |string |no |
| targeting |nicht definiert |no |
| feed_targeting |nicht definiert |no |
| published |Boolescher Wert |no |
| scheduled_publish_time |string |no |
| backdated_time |string |no |
| backdated_time_granularity |string |no |
| child_attachments |array |no |
| multi_share_end_card |Boolescher Wert |no |

#### <a name="postfeedresponse"></a>PostFeedResponse
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |no |

#### <a name="profilecollection"></a>ProfileCollection
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| data |array |no |

#### <a name="useritem"></a>UserItem
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |no |
| first_name |string |no |
| last_name |string |no |
| Name |string |no |
| gender |string |no |
| about |string |no |

#### <a name="actionitem"></a>ActionItem
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Name |string |no |
| link |string |no |

#### <a name="targetitem"></a>TargetItem
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| countries |array |no |
| locales |array |no |
| regions |array |no |
| cities |array |no |

#### <a name="feedtargetitem-object-that-controls-news-feed-targeting-for-this-post"></a>FeedTargetItem: Objekt, das Newsfeedziele für diesen Beitrag steuert
Mitglieder dieser Gruppen wird dieser Beitrag wahrscheinlich angezeigt. Bei anderen ist dies weniger wahrscheinlich. Gilt nur für Seiten.

| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| countries |array |no |
| regions |array |no |
| cities |array |no |
| age_min |integer |no |
| age_max |integer |no |
| genders |array |no |
| relationship_statuses |array |no |
| interested_in |array |no |
| college_years |array |no |
| interests |array |no |
| relevant_until |integer |no |
| education_statuses |array |no |
| locales |array |no |

#### <a name="placeitem"></a>PlaceItem
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |no |
| Name |string |no |
| overall_rating |number |no |
| location |nicht definiert |no |

#### <a name="locationitem"></a>LocationItem
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| city |string |no |
| country |string |no |
| latitude |number |no |
| located_in |string |no |
| longitude |number |no |
| Name |string |no |
| region |string |no |
| state |string |no |
| street |string |no |
| zip |string |no |

#### <a name="privacyitem"></a>PrivacyItem
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Beschreibung |string |no |
| value |string |Ja |
| allow |string |no |
| deny |string |no |
| friends |string |no |

#### <a name="childattachmentsitem"></a>ChildAttachmentsItem
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| link |string |no |
| picture |string |no |
| image_hash |string |no |
| Name |string |no |
| Beschreibung |string |no |

#### <a name="postphotorequest"></a>PostPhotoRequest
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| url |string |Ja |
| caption |string |no |

#### <a name="postphotoresponse"></a>PostPhotoResponse
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |Ja |
| post_id |string |Ja |

#### <a name="postvideorequest"></a>PostVideoRequest
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| videoData |string |Ja |
| Beschreibung |string |Ja |
| title |string |Ja |
| uploadedVideoName |string |no |

#### <a name="getphotoresponse"></a>GetPhotoResponse
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| data |nicht definiert |Ja |

#### <a name="getphotoresponseitem"></a>GetPhotoResponseItem
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| url |string |Ja |
| is_silhouette |Boolescher Wert |Ja |
| height |string |no |
| width |string |no |

#### <a name="geteventresponse"></a>GetEventResponse
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| data |array |Ja |

#### <a name="geteventresponseitem"></a>GetEventResponseItem
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |Ja |
| Name |string |Ja |
| start_time |string |no |
| end_time |string |no |
| timezone |string |no |
| location |string |no |
| Beschreibung |string |no |
| ticket_uri |string |no |
| rsvp_status |string |Ja |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md)




<!--HONumber=Feb17_HO2-->


