---
title: " Verwenden des Slack-Connectors in Ihren Logik-Apps | Microsoft-Dokumentations"
description: Erste Schritte mit dem Slack-Connector in Ihren Logik-Apps
services: 
documentationcenter: 
author: msftman
manager: anneta
editor: 
tags: connectors
ms.assetid: 234cad64-b13d-4494-ae78-18b17119ba24
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: 9c74b25a2ac5e2088a841d97920035376b7f3f11
ms.openlocfilehash: 404776283ee10aa9410d002597e054757516bcab


---
# <a name="get-started-with-the-slack-connector"></a>Erste Schritte mit dem Slack-Connector
Slack ist ein Tool für die Kommunikation in Teams, das alle Kommunikationen in Ihrem Team zentral zusammenführt und unmittelbar durchsuchbar und überall verfügbar macht.

> [!NOTE]
> Diese Version des Artikels gilt für die Schemaversion 2015-08-01-preview für Logik-Apps.
> 
> 

Der Slack-Connector ermöglicht Folgendes:

* Erstellen von Logik-Apps

Informationen zum Hinzufügen eines Vorgangs in Logik-Apps finden Sie unter [Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="lets-talk-about-triggers-and-actions"></a>Trigger und Aktionen
Der Slack-Connector kann als Aktion verwendet werden. Es gibt keine Trigger. Alle Connectors unterstützen Daten im JSON- und XML-Format. 

 Der Slack-Connector verfügt über die folgenden Aktionen und/oder Trigger:

### <a name="slack-actions"></a>Slack-Aktionen
Sie können diese Aktionen ausführen:

| Aktion | Beschreibung |
| --- | --- |
| PostMessage |Sendet eine Nachricht an einen bestimmten Kanal. |

## <a name="create-a-connection-to-slack"></a>Herstellen einer Verbindung mit Slack
Stellen Sie zum Verwenden des Slack-Connectors zunächst eine **Verbindung** her, und geben Sie anschließend die Details für diese Eigenschaften an: 

| Eigenschaft | Erforderlich | Beschreibung |
| --- | --- | --- |
| Tokenverschlüsselung |Ja |Angeben von Slack-Anmeldeinformationen |

Führen Sie folgende Schritte aus, um sich bei Slack anzumelden und die Konfiguration der Slack- **Verbindung** in Ihrer Logik-App abzuschließen:

1. Wählen Sie **Wiederholung**
2. Wählen Sie eine **Häufigkeit** aus, und geben Sie ein **Intervall** an.
3. Wählen Sie **Aktion hinzufügen** aus.  
   ![Slack konfigurieren][1]  
4. Geben Sie in das Suchfeld „Slack“ ein, und warten Sie, bis die Suche alle Einträge mit Slack im Namen zurückgibt.
5. Wählen Sie **Slack – Nachricht veröffentlichen**
6. Wählen Sie **Bei Slack anmelden**:  
   ![Slack konfigurieren][2]
7. Geben Sie Ihre Slack-Anmeldeinformationen ein, um die Anwendung zu autorisieren.    
   ![Slack konfigurieren][3]  
8. Sie werden zur Anmeldeseite Ihrer Organisation weitergeleitet. **Autorisieren** Sie Slack für die Interaktion mit Ihrer Logik-App:      
   ![Slack konfigurieren][5] 
9. Nach Abschluss der Autorisierung werden Sie zu Ihrer Logik-App umgeleitet, damit Sie diese vervollständigen können. Dazu konfigurieren Sie den Abschnitt **Slack – Alle Nachrichten abrufen**. Fügen Sie bei Bedarf weitere Trigger und Aktionen hinzu.  
   ![Slack konfigurieren][6]
10. Speichern Sie Ihre Arbeit durch Auswählen von **Speichern** in der Menüleiste oben.

> [!TIP]
> Sie können diese Verbindung in anderen Logik-Apps verwenden.
> 
> 

## <a name="slack-rest-api-reference"></a>REST-API-Referenz für Slack
#### <a name="this-documentation-is-for-version-10"></a>Diese Dokumentation gilt für Version 1.0.
### <a name="post-a-message-to-a-specified-channel"></a>Sendet eine Nachricht an einen bestimmten Kanal.
**```POST: /chat.postMessage```** 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| channel |string |Ja |query |(Keine) |Kanal, private Gruppe oder Chatkanal, an den/die die Nachricht gesendet werden soll. Kann ein Name (Beispiel: #general) oder eine codierte ID sein. |
| Text |string |Ja |query |(Keine) |Text der zu sendenden Nachricht. Formatierungsoptionen finden Sie unter https://api.slack.com/docs/formatting. |
| username |string |no |query |(Keine) |Name des Bots |
| as_user |Boolescher Wert |no |query |(Keine) |Übergeben Sie „True“, um die Nachricht als authentifizierter Benutzer statt als Bot zu senden. |
| parse |string |no |query |(Keine) |Ändern, wie Nachrichten behandelt werden. Formatierungsoptionen finden Sie unter https://api.slack.com/docs/formatting. |
| link_names |integer |no |query |(Keine) |Suchen und Verknüpfen von Kanal- und Benutzernamen. |
| unfurl_links |Boolescher Wert |no |query |(Keine) |Übergeben Sie „True“, um das Öffnen von hauptsächlich textbasiertem Inhalt zu aktivieren. |
| unfurl_media |Boolescher Wert |no |query |(Keine) |Übergeben Sie "false", um das Öffnen von Medieninhalten zu deaktivieren. |
| icon_url |string |no |query |(Keine) |URL zu einem Bild, das als Symbol für diese Nachricht verwendet wird |
| icon_emoji |string |no |query |(Keine) |Emoticon, das als Symbol für diese Nachricht verwendet wird |

### <a name="here-are-the-possible-responses"></a>Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 408 |Anforderungstimeout |
| 429 |Zu viele Anforderungen |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| 503 |Slack-Dienst nicht verfügbar |
| 504 |Gatewaytimeout |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
## <a name="object-definitions"></a>Objektdefinitionen:
 **Message**:Slack Message

Erforderliche Eigenschaften für „Message“:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| id |integer |
| content_excerpt |string |
| sender_id |integer |
| replied_to_id |integer |
| created_at |string |
| network_id |integer |
| message_type |string |
| sender_type |string |
| url |string |
| web_url |string |
| group_id |integer |
| body |nicht definiert |
| thread_id |integer |
| direct_message |Boolescher Wert |
| client_type |string |
| client_url |string |
| Sprache |string |
| notified_user_ids |array |
| privacy |string |
| liked_by |nicht definiert |
| system_message |Boolescher Wert |

 **PostOperationRequest**: Stellt eine Sendeanforderung für den Slack-Connector dar, die an Slack gesendet werden soll.

Erforderliche Eigenschaften für PostOperationRequest:

body

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| body |string |
| group_id |integer |
| replied_to_id |integer |
| direct_to_id |integer |
| broadcast |Boolescher Wert |
| topic1 |string |
| topic2 |string |
| topic3 |string |
| topic4 |string |
| topic5 |string |
| topic6 |string |
| topic7 |string |
| topic8 |string |
| topic9 |string |
| topic10 |string |
| topic11 |string |
| topic12 |string |
| topic13 |string |
| topic14 |string |
| topic15 |string |
| topic16 |string |
| topic17 |string |
| topic18 |string |
| topic19 |string |
| topic20 |string |

 **MessageList:**Liste der Nachrichten

Erforderliche Eigenschaften für MessageList:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| messages |array |

 **MessageBody:**Nachrichtentext

Erforderliche Eigenschaften für MessageBody:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| parsed |string |
| plain |string |
| rich |string |

 **LikedBy:**Gefällt

Erforderliche Eigenschaften für LikedBy:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| count |integer |
| names |array |

 **YammmerEntity:**Gefällt

Erforderliche Eigenschaften für YammmerEntity:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| type |string |
| id |integer |
| full_name |string |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md)

## <a name="object-definitions"></a>Objektdefinitionen:
 **WebResultModel:**Bing-Ergebnisse für Websuche

Erforderliche Eigenschaften für WebResultModel:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| Titel |string |
| Beschreibung |string |
| DisplayUrl |string |
| id |string |
| FullUrl |string |

 **VideoResultModel:**Bing-Suchergebnisse für Videos

Erforderliche Eigenschaften für VideoResultModel:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| Titel |string |
| DisplayUrl |string |
| id |string |
| MediaUrl |string |
| Laufzeit |integer |
| Miniaturansicht |nicht definiert |

 **ThumbnailModel:**Miniaturansichteigenschaften des Multimediaelements

Erforderliche Eigenschaften für ThumbnailModel:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| MediaUrl |string |
| ContentType |string |
| Breite |integer |
| Höhe |integer |
| FileSize |integer |

 **ImageResultModel:**Bing-Suchergebnisse für Bilder

Erforderliche Eigenschaften für ImageResultModel:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| Titel |string |
| DisplayUrl |string |
| id |string |
| MediaUrl |string |
| SourceUrl |string |
| Miniaturansicht |nicht definiert |

 **NewsResultModel:**Bing-Suchergebnisse für Nachrichten

Erforderliche Eigenschaften für NewsResultModel:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| Titel |string |
| Beschreibung |string |
| DisplayUrl |string |
| id |string |
| Quelle |string |
| Date |string |

 **SpellResultModel:**Bing-Suchergebnisse für Schreibweisen

Erforderliche Eigenschaften für SpellResultModel:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| id |string |
| Wert |string |

 **RelatedSearchResultModel:**Bing-Suchergebnisse für verwandte Informationen

Erforderliche Eigenschaften für RelatedSearchResultModel:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| Titel |string |
| id |string |
| BingUrl |string |

 **CompositeSearchResultModel:**Zusammengesetzte Bing-Suchergebnisse

Erforderliche Eigenschaften für CompositeSearchResultModel:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| WebResultsTotal |integer |
| ImageResultsTotal |integer |
| VideoResultsTotal |integer |
| NewsResultsTotal |integer |
| SpellSuggestionsTotal |integer |
| WebResults |array |
| ImageResults |array |
| VideoResults |array |
| NewsResults |array |
| SpellSuggestionResults |array |
| RelatedSearchResults |array |

## <a name="object-definitions"></a>Objektdefinitionen:
 **PostOperationResponse:**Stellt die Antwort des Sendevorgangs des Slack-Connectors für das Senden an Slack dar

Erforderliche Eigenschaften für PostOperationResponse:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| OK |Boolescher Wert |
| channel |string |
| ts |string |
| Message: |nicht definiert |
| Fehler |string |

 **MessageItem:**eine Kanalnachricht.

Erforderliche Eigenschaften für MessageItem:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| Text |string |
| id |string |
| user |string |
| created |integer |
| is_user-deleted |Boolescher Wert |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md)

[1]: ./media/connectors-create-api-slack/connectionconfig1.png
[2]: ./media/connectors-create-api-slack/connectionconfig2.png 
[3]: ./media/connectors-create-api-slack/connectionconfig3.png
[4]: ./media/connectors-create-api-slack/connectionconfig4.png
[5]: ./media/connectors-create-api-slack/connectionconfig5.png
[6]: ./media/connectors-create-api-slack/connectionconfig6.png



<!--HONumber=Jan17_HO3-->


