---
title: "Hinzufügen des Twilio-Connectors zu Ihren Logik-Apps | Microsoft Docs"
description: "Übersicht über den Twilio-Connector mit REST-API-Parametern"
services: 
documentationcenter: 
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: 43116187-4a2f-42e5-9852-a0d62f08c5fc
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/19/2016
ms.author: mandia
translationtype: Human Translation
ms.sourcegitcommit: b92f954680603891ced503a1134791312b5214f0
ms.openlocfilehash: 6770aa8387fe3d381fd9c566ca0c38c88a4b1e55


---
# <a name="get-started-with-the-twilio-connector"></a>Erste Schritte mit dem Twilio-Connector
Stellen Sie eine Verbindung mit Twilio her, um global SMS, MMS und IP-Nachrichten zu senden und zu empfangen.

> [!NOTE]
> Diese Version des Artikels gilt für die Schemaversion 2015-08-01-preview für Logik-Apps.
> 
> 

Mit Twilio können Sie folgende Aktionen ausführen:

* Erstellen eines Geschäftsworkflows basierend auf den Daten, die aus Twilio abgerufen werden. 
* Verwenden von Aktionen zum Abrufen einer Nachricht, von Listennachrichten und mehr. Diese Aktionen erhalten eine Antwort und stellen anschließend die Ausgabe anderen Aktionen zur Verfügung. Wenn Sie eine neue Twilio-Nachricht erhalten, können Sie diese Nachricht B. annehmen und als Service Bus-Workflow verwenden. 

Informationen zum Hinzufügen eines Vorgangs in Logik-Apps finden Sie unter [Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Der Twilio-Connector beinhaltet die folgenden Aktionen. Es gibt keine Trigger. 

| Trigger | Aktionen |
| --- | --- |
| Keine |<ul><li>Get Message</li><li>List Messages</li><li>Nachricht senden</li></ul> |

Alle Connectors unterstützen Daten im JSON- und XML-Format. 

## <a name="create-a-connection-to-twilio"></a>Herstellen einer Verbindung mit Twilio
Wenn Sie diesen Connector Ihren Logik-Apps hinzufügen, geben Sie die folgenden Twilio-Werte ein:

| Eigenschaft | Erforderlich | Beschreibung |
| --- | --- | --- |
| Konto-ID |Ja |Geben Sie Ihre Twilio-Konto-ID ein. |
| Access Token |Ja |Geben Sie Ihr Twilio-Zugriffstoken ein. |

> [!INCLUDE [Steps to create a connection to Twilio](../../includes/connectors-create-api-twilio.md)]
> 
> 

Wenn Sie noch kein Zugriffstoken besitzen, nutzen Sie die Informationen zum Erstellen unter [Twilio](https://www.twilio.com/docs/api/ip-messaging/guides/identity).

> [!TIP]
> Sie können die gleiche Twilio-Verbindung in anderen Logik-Apps verwenden.
> 
> 

## <a name="swagger-rest-api-reference"></a>Swagger-REST-API – Referenz
#### <a name="this-documentation-is-for-version-10"></a>Diese Dokumentation gilt für Version 1.0.
### <a name="get-message"></a>Get Message
Gibt eine einzelne Nachricht zurück, die in der bereitgestellten Nachrichten-ID angegeben ist.  
```GET: /Messages/{MessageId}.json```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| MessageId |string |Ja |path |Keine |Nachrichten-ID |

### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |Ungültige Anforderung |
| 404 |Nachricht nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler ist aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="list-messages"></a>List Messages
Gibt eine Liste von Nachrichten zurück, die Ihrem Konto zugeordnet sind.  
```GET: /Messages.json```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| To |string |no |query |Keine |Angerufene Telefonnummer |
| Aus |string |no |query |Keine |Anrufertelefonnummer |
| DateSent |string |no |query |(Keine) |Nur an diesem Datum (GMT) gesendete Nachrichten anzeigen, Datum im Format JJJJ-MM-TT. Beispiel: DateSent=2009-07-06. Sie können auch Ungleichheit angeben, d. h. DateSent<=JJJJ-MM-TT für Nachrichten, die an oder vor Mitternacht an einem bestimmten Datum gesendet wurden, sowie DateSent>=JJJJ-MM-TT für Nachrichten, die an oder nach Mitternacht an einem bestimmten Datum gesendet wurden. |
| PageSize |integer |no |query |50 |Die Anzahl der Ressourcen, die auf jeder Listenseite zurückgegeben werden. Standard ist 50. |
| Seite |integer |no |query |0 |Seitenzahl. Der Standardwert ist 0. |

### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="send-message"></a>Nachricht senden
Sendet eine neue Nachricht an eine Mobiltelefonnummer.  
```POST: /Messages.json```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| sendMessageRequest | |Ja |body |Keine |Zu sendende Nachricht |

### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler. Unbekannter Fehler ist aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="object-definitions"></a>Objektdefinitionen
#### <a name="sendmessagerequest-request-model-for-send-message-operation"></a>SendMessageRequest: Anforderungsmodell zum Senden von Nachrichten
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Aus |string |Ja |
| To |string |Ja |
| body |string |Ja |
| media_url |array |no |
| status_callback |string |no |
| messaging_service_sid |string |no |
| application_sid |string |no |
| max_price |string |no |

#### <a name="message-model-for-message"></a>Message: Nachrichtenmodell
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| body |string |no |
| Aus |string |no |
| To |string |no |
| status |string |no |
| sid |string |no |
| account_sid |string |no |
| api_version |string |no |
| num_segments |string |no |
| num_media |string |no |
| date_created |string |no |
| date_sent |string |no |
| date_updated |string |no |
| direction |string |no |
| error_code |string |no |
| error_message |string |no |
| price |string |no |
| price_unit |string |no |
| uri |string |no |
| subresource_uris |array |no |
| messaging_service_sid |string |no |

#### <a name="messagelist-response-model-for-list-messages-operation"></a>MessageList: Antwortmodell zum Auflisten von Nachrichten
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| messages |array |no |
| Seite |integer |no |
| page_size |integer |no |
| num_pages |integer |no |
| uri |string |no |
| first_page_uri |string |no |
| next_page_uri |string |no |
| total |integer |no |
| previous_page_uri |string |no |

#### <a name="incomingphonenumberlist-response-model-for-list-messages-operation"></a>IncomingPhoneNumberList: Antwortmodell zum Auflisten von Nachrichten
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| incoming_phone_numbers |array |no |
| Seite |integer |no |
| page_size |integer |no |
| num_pages |integer |no |
| uri |string |no |
| first_page_uri |string |no |
| next_page_uri |string |no |

#### <a name="addincomingphonenumberrequest-request-model-for-add-incoming-number-operation"></a>AddIncomingPhoneNumberRequest: Anforderungsmodell zum Hinzufügen eingehender Telefonnummern
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| PhoneNumber |string |Ja |
| AreaCode |string |no |
| FriendlyName |string |no |

#### <a name="incomingphonenumber-incoming-phone-number"></a>IncomingPhoneNumber: Eingehende Telefonnummer
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| phone_number |string |no |
| friendly_name |string |no |
| sid |string |no |
| account_sid |string |no |
| date_created |string |no |
| date_updated |string |no |
| capabilities |nicht definiert |no |
| status_callback |string |no |
| status_callback_method |string |no |
| api_version |string |no |

#### <a name="capabilities-phone-number-capabilities"></a>Capabilities: Telefonnummernfunktionen
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| mms |Boolescher Wert |no |
| sms |Boolescher Wert |no |
| voice |Boolescher Wert |no |

#### <a name="availablephonenumbers-available-phone-numbers"></a>AvailablePhoneNumbers: Verfügbare Telefonnummern
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| phone_number |string |no |
| friendly_name |string |no |
| lata |string |no |
| latitude |string |no |
| longitude |string |no |
| postal_code |string |no |
| rate_center |string |no |
| region |string |no |
| mms |Boolescher Wert |no |
| sms |Boolescher Wert |no |
| voice |Boolescher Wert |no |

#### <a name="usagerecords-usage-records-class"></a>UsageRecords: Klasse für Nutzungseinträge
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| category |string |no |
| Nutzung |string |no |
| usage_unit |string |no |
| Beschreibung |string |no |
| price |number |no |
| price_unit |string |no |
| count |string |no |
| count_unit |string |no |
| start_date |string |no |
| end_date |string |no |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md)




<!--HONumber=Jan17_HO3-->


