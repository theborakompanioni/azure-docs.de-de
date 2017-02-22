---
title: MailChimp | Microsoft Docs
description: "Erstellen Sie Logik-Apps mit Azure App Service. MailChimp ist ein SaaS-Dienst, mit dem Unternehmen E-Mail-Marketingaktivitäten verwalten und automatisieren können. Dazu gehört auch das Senden von Marketing-E-Mails und automatisierten Nachrichten sowie das Durchführen zielgruppenspezifischer Kampagnen."
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: 36559de2-94f0-4355-b492-2926dfc56486
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: b92f954680603891ced503a1134791312b5214f0
ms.openlocfilehash: cee2c702c705e1451c0a6d01b140e6291b1e5ce1


---
# <a name="get-started-with-the-mailchimp-connector"></a>Erste Schritte mit dem MailChimp-Connector
MailChimp ist ein SaaS-Dienst, mit dem Unternehmen E-Mail-Marketingaktivitäten verwalten und automatisieren können. Dazu gehört auch das Senden von Marketing-E-Mails und automatisierten Nachrichten sowie das Durchführen zielgruppenspezifischer Kampagnen.

> [!NOTE]
> Diese Version des Artikels gilt für Logik-Apps mit der Schemaversion „2015-08-01-preview“.
> 
> 

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md) beschrieben.

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Der MailChimp-Connector kann als Aktion verwendet werden. Er verfügt über Trigger. Alle Connectors unterstützen Daten im JSON- und XML-Format.

 Der MailChimp-Connector verfügt über die folgenden Aktionen und/oder Trigger:

### <a name="mailchimp-actions"></a>MailChimp-Aktionen
Sie können diese Aktionen ausführen:

| Aktion | Beschreibung |
| --- | --- |
| [newcampaign](connectors-create-api-mailchimp.md#newcampaign) |Erstellen einer neuen Kampagne auf Grundlage eines Kampagnentyps, einer Empfängerliste und der Kampagneneinstellungen (Betreffzeile, Titel, „from_name“ und „reply_to“) |
| [newlist](connectors-create-api-mailchimp.md#newlist) |Erstellen einer neuen Liste in Ihrem MailChimp-Konto |
| [addmember](connectors-create-api-mailchimp.md#addmember) |Hinzufügen eines Mitglieds zu einer Liste oder Aktualisieren eines Mitglieds |
| [removemember](connectors-create-api-mailchimp.md#removemember) |Löschen eines Mitglieds aus einer Liste |
| [updatemember](connectors-create-api-mailchimp.md#updatemember) |Aktualisieren von Informationen für ein bestimmtes Listenmitglied |

### <a name="mailchimp-triggers"></a>MailChimp-Trigger
Sie können auf diese Ereignisse lauschen:

| Trigger | Beschreibung |
| --- | --- |
| Wenn einer Liste ein Mitglied hinzugefügt wurde |Löst einen Workflow aus, wenn einer Liste ein neues Mitglied hinzugefügt wurde. |
| Wenn eine neue Liste erstellt wird |Löst einen Workflow aus, wenn eine neue Liste erstellt wird. |

## <a name="create-a-connection-to-mailchimp"></a>Herstellen einer Verbindung mit MailChimp
Um Logik-Apps mit MailChimp zu erstellen, müssen Sie zuerst eine **Verbindung** erstellen und anschließend die Details für die folgenden Eigenschaften angeben:

| Eigenschaft | Erforderlich | Beschreibung |
| --- | --- | --- |
| Tokenverschlüsselung |Ja |Angeben der Anmeldeinformationen für MailChimp |

> [!INCLUDE [Steps to create a connection to MailChimp](../../includes/connectors-create-api-mailchimp.md)]
> 
> [!TIP]
> Sie können diese Verbindung in anderen Logik-Apps verwenden.
> 
> 

## <a name="reference-for-mailchimp"></a>Referenz für MailChimp
Gilt für Version 1.0.

## <a name="newcampaign"></a>newcampaign
Neue Kampagne: Erstellen einer neuen Kampagne auf Grundlage eines Kampagnentyps, einer Empfängerliste und der Kampagneneinstellungen (Betreffzeile, Titel, „from_name“ und „reply_to“)

```POST: /campaigns```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| newCampaignRequest | |Ja |body |(Keine) |JSON-Objekt mit den Anforderungsparametern für die neue Kampagne, das im Text gesendet wird |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="newlist"></a>newlist
Neue Liste: Erstellen einer neuen Liste in Ihrem MailChimp-Konto

```POST: /lists```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| newListRequest | |Ja |body |(Keine) |JSON-Objekt mit den Anforderungsparametern für die neue Kampagne, das im Text gesendet wird |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="addmember"></a>addmember
Mitglied einer Liste hinzufügen: Hinzufügen eines Mitglieds zu einer Liste oder Aktualisieren eines Mitglieds

```POST: /lists/{list_id}/members```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list_id |string |Ja |path |(Keine) |Die eindeutige ID für die Liste |
| newMemberInList | |Ja |body |(Keine) |JSON-Objekt mit den Informationen zum neuen Mitglied, das im Text gesendet wird |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="removemember"></a>removemember
Mitglied aus der Liste entfernen: Löschen eines Mitglied aus der Liste

```DELETE: /lists/replacemailwithhash/{list_id}/members/{member_email}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list_id |string |Ja |path |(Keine) |Die eindeutige ID für die Liste |
| member_email |string |Ja |path |(Keine) |Die E-Mail-Adresse des zu löschenden Mitglieds |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="updatemember"></a>updatemember
Mitgliedsinformationen aktualisieren: Aktualisieren von Informationen für ein bestimmtes Listenmitglied

```PATCH: /lists/replacemailwithhash/{list_id}/members/{member_email}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list_id |string |Ja |path |(Keine) |Die eindeutige ID für die Liste |
| member_email |string |Ja |path |(Keine) |Die eindeutige E-Mail-Adresse des zu aktualisierenden Mitglieds |
| updateMemberInListRequest | |Ja |body |(Keine) |JSON-Objekt mit den Informationen zum aktualisierten Mitglied, das im Text gesendet wird |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="onmembersubscribed"></a>OnMemberSubscribed
Wenn einer Liste ein Mitglied hinzugefügt wurde: Löst einen Workflow aus, wenn einer Liste ein neues Mitglied hinzugefügt wurde

```GET: /trigger/lists/{list_id}/members```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list_id |string |Ja |path |(Keine) |Die eindeutige ID für die Liste |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 202 |Zulässig |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="oncreatelist"></a>OnCreateList
Wenn eine neue Liste erstellt wird: Löst einen Workflow aus, wenn eine neue Liste erstellt wird.

```GET: /trigger/lists```

Es gibt keine Parameter für diesen Aufruf

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 202 |Zulässig |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="object-definitions"></a>Objektdefinitionen
### <a name="newcampaignrequest"></a>NewCampaignRequest
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| type |string |Ja |
| recipients |nicht definiert |Ja |
| settings |nicht definiert |Ja |
| variate_settings |nicht definiert |Nein |
| tracking |nicht definiert |Nein |
| rss_opts |nicht definiert |Nein |
| social_card |nicht definiert |Nein |

### <a name="recipient"></a>Recipient
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| list_id |string |Ja |
| segment_opts |nicht definiert |Nein |

### <a name="settings"></a>Einstellungen
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| subject_line |string |Ja |
| title |string |Nein |
| from_name |string |Ja |
| reply_to |string |Ja |
| use_conversation |Boolescher Wert |Nein |
| to_name |string |Nein |
| folder_id |integer |Nein |
| authenticate |Boolescher Wert |Nein |
| auto_footer |Boolescher Wert |Nein |
| inline_css |Boolescher Wert |Nein |
| auto_tweet |Boolescher Wert |Nein |
| auto_fb_post |array |Nein |
| fb_comments |Boolescher Wert |Nein |

### <a name="variatesettings"></a>Variate_Settings
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| winner_criteria |string |Nein |
| wait_time |integer |Nein |
| test_size |integer |Nein |
| subject_lines |array |Nein |
| send_times |array |Nein |
| from_names |array |Nein |
| reply_to_addresses |array |Nein |

### <a name="tracking"></a>Tracking
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| opens |Boolescher Wert |Nein |
| html_clicks |Boolescher Wert |Nein |
| text_clicks |Boolescher Wert |Nein |
| goal_tracking |Boolescher Wert |Nein |
| ecomm360 |Boolescher Wert |Nein |
| google_analytics |string |Nein |
| clicktale |string |Nein |
| salesforce |nicht definiert |Nein |
| highrise |nicht definiert |Nein |
| capsule |nicht definiert |Nein |

### <a name="rssopts"></a>RSS_Opts
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| feed_url |string |Nein |
| frequency |string |Nein |
| constrain_rss_img |string |Nein |
| schedule |nicht definiert |Nein |

### <a name="socialcard"></a>Social_Card
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| image_url |string |Nein |
| description |string |Nein |
| title |string |Nein |

### <a name="segmentopts"></a>Segment_Opts
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| saved_segment_id |integer |Nein |
| match |string |Nein |

### <a name="salesforce"></a>Salesforce
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| campaign |Boolescher Wert |Nein |
| notes |Boolescher Wert |Nein |

### <a name="highrise"></a>Highrise
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| campaign |Boolescher Wert |Nein |
| notes |Boolescher Wert |Nein |

### <a name="capsule"></a>Capsule
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| notes |Boolescher Wert |Nein |

### <a name="schedule"></a>Schedule
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| hour |integer |Nein |
| daily_send |nicht definiert |Nein |
| weekly_send_day |string |Nein |
| monthly_send_date |number |Nein |

### <a name="dailysend"></a>Daily_Send
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| sunday |Boolescher Wert |Nein |
| monday |Boolescher Wert |Nein |
| tuesday |Boolescher Wert |Nein |
| wednesday |Boolescher Wert |Nein |
| thursday |Boolescher Wert |Nein |
| friday |Boolescher Wert |Nein |
| saturday |Boolescher Wert |Nein |

### <a name="campaignresponsemodel"></a>CampaignResponseModel
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |Nein |
| Typ |string |Nein |
| create_time |string |Nein |
| archive_url |string |Nein |
| status |string |Nein |
| emails_sent |integer |Nein |
| send_time |string |Nein |
| content_type |string |Nein |
| recipient |array |Nein |
| settings |nicht definiert |Nein |
| variate_settings |nicht definiert |Nein |
| tracking |nicht definiert |Nein |
| rss_opts |nicht definiert |Nein |
| ab_split_opts |nicht definiert |Nein |
| social_card |nicht definiert |Nein |
| report_summary |nicht definiert |Nein |
| delivery_status |nicht definiert |Nein |
| _links |array |Nein |

### <a name="absplitopts"></a>AB_Split_Opts
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| split_test |string |Nein |
| pick_winner |string |Nein |
| wait_units |string |Nein |
| wait_time |integer |Nein |
| split_size |integer |Nein |
| from_name_a |string |Nein |
| from_name_b |string |Nein |
| reply_email_a |string |Nein |
| reply_email_b |string |Nein |
| subject_a |string |Nein |
| subject_b |string |Nein |
| send_time_a |string |Nein |
| send_time_b |string |Nein |
| send_time_winner |string |Nein |

### <a name="reportsummary"></a>Report_Summary
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| opens |integer |Nein |
| unique_opens |integer |Nein |
| open_rate |number |Nein |
| clicks |integer |Nein |
| subscriber_clicks |number |Nein |
| click_rate |number |Nein |

### <a name="deliverystatus"></a>Delivery_Status
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| enabled |Boolescher Wert |Nein |
| can_cancel |Boolescher Wert |Nein |
| status |string |Nein |
| emails_sent |integer |Nein |
| emails_canceled |integer |Nein |

### <a name="link"></a>Link
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| rel |string |Nein |
| href |string |Nein |
| method |string |Nein |
| targetSchema |string |Nein |
| schema |string |Nein |

### <a name="newlistrequest"></a>NewListRequest
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Name |string |Ja |
| contact |nicht definiert |Ja |
| permission_reminder |string |Ja |
| use_archive_bar |Boolescher Wert |Nein |
| campaign_defaults |nicht definiert |Ja |
| notify_on_subscribe |string |Nein |
| notify_on_unsubscribe |string |Nein |
| email_type_option |Boolescher Wert |Ja |
| visibility |string |Nein |

### <a name="contact"></a>Contact
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| company |string |Ja |
| address1 |string |Ja |
| address2 |string |Nein |
| city |string |Ja |
| state |string |Ja |
| zip |string |Ja |
| country |string |Ja |
| phone |string |Ja |

### <a name="campaigndefaults"></a>Campaign_Defaults
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| from_name |string |Ja |
| from_email |string |Ja |
| subject |string |Nein |
| language |string |Ja |

### <a name="createnewlistresponsemodel"></a>CreateNewListResponseModel
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |Ja |
| Name |string |Ja |
| contact |nicht definiert |Ja |
| permission_reminder |string |Ja |
| use_archive_bar |Boolescher Wert |Nein |
| campaign_defaults |nicht definiert |Ja |
| notify_on_subscribe |string |Nein |
| notify_on_unsubscribe |string |Nein |
| date_created |string |Nein |
| list_rating |integer |Nein |
| email_type_option |Boolescher Wert |Ja |
| subscribe_url_short |string |Nein |
| subscribe_url_long |string |Nein |
| beamer_address |string |Nein |
| visibility |string |Nein |
| modules |array |Nein |
| stats |nicht definiert |Nein |
| _links |array |Nein |

### <a name="stats"></a>Stats
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| member_count |integer |Nein |
| unsubscribe_count |integer |Nein |
| cleaned_count |integer |Nein |
| member_count_since_send |integer |Nein |
| unsubscribe_count_since_send |integer |Nein |
| cleaned_count_since_send |integer |Nein |
| campaign_count |integer |Nein |
| campaign_last_sent |integer |Nein |
| merge_field_count |integer |Nein |
| avg_sub_rate |number |Nein |
| avg_unsub_rate |number |Nein |
| target_sub_rate |number |Nein |
| open_rate |number |Nein |
| click_rate |number |Nein |
| last_sub_date |string |Nein |
| last_unsub_date |string |Nein |

### <a name="getlistsresponsemodel"></a>GetListsResponseModel
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| lists |array |Nein |
| total_items |integer |Nein |

### <a name="newmemberinlistrequest"></a>NewMemberInListRequest
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| email_type |string |Nein |
| status |string |Ja |
| merge_fields |nicht definiert |Nein |
| interests |string |Nein |
| language |string |Nein |
| VIP |Boolescher Wert |Nein |
| location |nicht definiert |Nein |
| email_address |string |Ja |

### <a name="firstandlastname"></a>FirstAndLastName
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| FNAME |string |Nein |
| LNAME |string |Nein |

### <a name="location"></a>Location
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| latitude |number |Nein |
| longitude |number |Nein |

### <a name="memberresponsemodel"></a>MemberResponseModel
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |Nein |
| email_address |string |Nein |
| unique_email_id |string |Nein |
| email_type |string |Nein |
| status |string |Nein |
| merge_fields |nicht definiert |Nein |
| interests |string |Nein |
| stats |nicht definiert |Nein |
| ip_signup |string |Nein |
| timestamp_signup |string |Nein |
| ip_opt |string |Nein |
| timestamp_opt |string |Nein |
| member_rating |integer |Nein |
| last_changed |string |Nein |
| language |string |Nein |
| VIP |Boolescher Wert |Nein |
| email_client |string |Nein |
| location |nicht definiert |Nein |
| last_note |nicht definiert |Nein |
| list_id |string |Nein |
| _links |array |Nein |

### <a name="lastnote"></a>Last_Note
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| note_id |integer |Nein |
| created_at |string |Nein |
| created_by |string |Nein |
| note |string |Nein |

### <a name="getallmembersresponsemodel"></a>GetAllMembersResponseModel
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| members |array |Nein |
| list_id |string |Nein |
| total_items |integer |Nein |

### <a name="object"></a>Objekt
### <a name="updatememberinlistrequest"></a>UpdateMemberInListRequest
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| email_address |string |Nein |
| email_type |string |Nein |
| status |string |Ja |
| merge_fields |nicht definiert |Nein |
| interests |string |Nein |
| language |string |Nein |
| VIP |Boolescher Wert |Nein |
| location |nicht definiert |Nein |

### <a name="getmembersresponsemodel"></a>GetMembersResponseModel
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| members |array |Nein |
| list_id |string |Nein |
| total_items |integer |Nein |

### <a name="adduserresponsemodel"></a>AddUserResponseModel
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |Ja |
| email_address |string |Ja |
| unique_email_id |string |Nein |
| email_type |string |Nein |
| status |string |Nein |
| merge_fields |nicht definiert |Ja |
| interests |string |Nein |
| stats |nicht definiert |Nein |
| ip_signup |string |Nein |
| timestamp_signup |string |Nein |
| ip_opt |string |Nein |
| timestamp_opt |string |Nein |
| member_rating |integer |Nein |
| last_changed |string |Nein |
| language |string |Nein |
| VIP |Boolescher Wert |Nein |
| email_client |string |Nein |
| location |nicht definiert |Nein |
| last_note |nicht definiert |Nein |
| list_id |string |Nein |
| _links |array |Nein |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md)




<!--HONumber=Jan17_HO3-->


