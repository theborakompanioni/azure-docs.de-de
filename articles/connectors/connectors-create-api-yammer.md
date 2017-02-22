---
title: "Hinzufügen des Yammer-Connectors zu Ihren Logik-Apps | Microsoft Docs"
description: "Übersicht über den Yammer-Connector mit REST-API-Parametern"
services: 
documentationcenter: 
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: b5ae0827-fbb3-45ec-8f45-ad1cc2e7eccc
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: b92f954680603891ced503a1134791312b5214f0
ms.openlocfilehash: c69f7af9f9427eb295481657eaefcdad76b48ccb


---
# <a name="get-started-with-the-yammer-connector"></a>Erste Schritte mit dem Yammer-Connector
Verbinden Sie sich mit Yammer, um auf Konversationen in Ihrem Unternehmensnetzwerk zuzugreifen.

> [!NOTE]
> Diese Version des Artikels gilt für Logik-Apps mit der Schemaversion „2015-08-01-preview“.
> 
> 

Yammer ermöglicht Folgendes:

* Erstellen eines Geschäftsworkflows basierend auf den Daten, die aus Yammer abgerufen werden. 
* Verwenden von Triggern, wenn es eine neue Nachricht in einer Gruppe oder in einem Feed gibt, dem Sie folgen.
* Verwenden Sie Aktionen, um z. : eine Nachricht senden oder alle Nachrichten abzurufen. Diese Aktionen erhalten eine Antwort und stellen anschließend die Ausgabe anderen Aktionen zur Verfügung. Wenn z. B. eine neue Nachricht vorhanden ist, können Sie über Office 365 eine E-Mail senden.

Informationen zum Hinzufügen eines Vorgangs in Logik-Apps finden Sie unter [Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Yammer weist die folgenden Trigger und Aktionen auf. 

| Trigger | Actions |
| --- | --- |
| <ul><li>Wenn es in einer Gruppe eine neue Nachricht gibt</li><li>Wenn es in meinem Feed „Folgen“ eine neue Nachricht gibt</li></ul> |<ul><li>Alle Nachrichten abrufen</li><li>Nachrichten in einer Gruppe abrufen</li><li>Nachrichten aus meinem Feed „Folgen“ abrufen</li><li>Nachricht veröffentlichen</li><li>Wenn es in einer Gruppe eine neue Nachricht gibt</li><li>Wenn es in meinem Feed „Folgen“ eine neue Nachricht gibt</li></ul> |

Alle Connectors unterstützen Daten im JSON- und XML-Format. 

## <a name="create-a-connection-to-yammer"></a>Herstellen einer Verbindung mit Yammer
Stellen Sie zum Verwenden des Yammer-Connectors zunächst eine **Verbindung** her, und geben Sie anschließend die Details für diese Eigenschaften an: 

| Eigenschaft | Erforderlich | Beschreibung |
| --- | --- | --- |
| Tokenverschlüsselung |Ja |Angeben der Yammer-Anmeldeinformationen |

> [!INCLUDE [Steps to create a connection to Yammer](../../includes/connectors-create-api-yammer.md)]
> 
> [!TIP]
> Sie können diese Verbindung in anderen Logik-Apps verwenden.
> 
> 

## <a name="yammer-rest-api-reference"></a>Yammer-REST-API – Referenz
Diese Dokumentation gilt für Version 1.0.

### <a name="get-all-public-messages-in-the-logged-in-users-yammer-network"></a>Alle öffentlichen Nachrichten im Yammer-Netzwerk des angemeldeten Benutzers abrufen
Entspricht „allen“ Konversationen auf der Yammer-Weboberfläche.  
```GET: /messages.json```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| older_than |integer |no |query |(Keine) |Gibt Nachrichten, die älter als die Nachrichten-ID sind, als numerische Zeichenfolge zurück. Dies ist nützlich zum Aufteilen von Nachrichten auf Seiten. Wenn Sie beispielsweise gerade 20 Nachrichten anzeigen und die älteste die Nummer 2912 hat, können Sie „?older_than=2912“ an Ihre Anforderung anfügen, um die 20 Nachrichten abzurufen, die vor den aktuell angezeigten liegen. |
| newer_than |integer |no |query |(Keine) |Gibt Nachrichten, die neuer als die Nachrichten-ID sind, als numerische Zeichenfolge zurück. Dies ist für Abfragen auf neue Nachrichten nützlich. Wenn Sie Nachrichten suchen und die neueste zurückgegebene Nachricht die Nummer 3516 hat, können Sie eine Anforderung mit dem Parameter „newer_than=3516“ stellen. Dadurch stellen Sie sicher, dass Sie keine Duplikate von Nachrichten erhalten, die sich bereits auf der Seite befinden. |
| limit |integer |no |query |(Keine) |Gibt nur die angegebene Anzahl von Nachrichten zurück. |
| page |integer |no |query |(Keine) |Ruft die angegebene Seite ab. Wenn die zurückgegeben Daten größer als dieser Grenzwert sind, können Sie dieses Feld für den Zugriff auf nachfolgende Seiten nutzen. |

### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 408 |Anforderungstimeout |
| 429 |Zu viele Anforderungen |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| 503 |Yammer-Dienst nicht verfügbar |
| 504 |Gatewaytimeout |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="post-a-message-to-a-group-or-all-company-feed"></a>Eine Nachricht an eine Gruppe oder den Unternehmensfeed „Alle“ senden
Wenn eine Gruppen-ID angegeben ist, wird die Nachricht an die angegebene Gruppe, andernfalls an den Unternehmensfeed „Alle“ gesendet.    
```POST: /messages.json``` 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| input | |Ja |body |(Keine) |Nachrichtenanforderung senden |

### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 201 |Erstellt |

## <a name="object-definitions"></a>Objektdefinitionen
#### <a name="message-yammer-message"></a>Message: Yammer-Nachricht
| Name | Datentyp | Erforderlich |
| --- | --- | --- |
| id |integer |no |
| content_excerpt |string |no |
| sender_id |integer |no |
| replied_to_id |integer |no |
| created_at |string |no |
| network_id |integer |no |
| message_type |string |no |
| sender_type |string |no |
| url |string |no |
| web_url |string |no |
| group_id |integer |no |
| body |nicht definiert |no |
| thread_id |integer |no |
| direct_message |Boolescher Wert |no |
| client_type |string |no |
| client_url |string |no |
| Sprache |string |no |
| notified_user_ids |array |no |
| privacy |string |no |
| liked_by |nicht definiert |no |
| system_message |Boolescher Wert |no |

#### <a name="postoperationrequest-represents-a-post-request-for-yammer-connector-to-post-to-yammer"></a>PostOperationRequest: Stellt eine Sendeanforderung für den Yammer-Connector dar, die an Yammer gesendet werden soll.
| Name | Datentyp | Erforderlich |
| --- | --- | --- |
| body |string |Ja |
| group_id |integer |no |
| replied_to_id |integer |no |
| direct_to_id |integer |no |
| broadcast |Boolescher Wert |no |
| topic1 |string |no |
| topic2 |string |no |
| topic3 |string |no |
| topic4 |string |no |
| topic5 |string |no |
| topic6 |string |no |
| topic7 |string |no |
| topic8 |string |no |
| topic9 |string |no |
| topic10 |string |no |
| topic11 |string |no |
| topic12 |string |no |
| topic13 |string |no |
| topic14 |string |no |
| topic15 |string |no |
| topic16 |string |no |
| topic17 |string |no |
| topic18 |string |no |
| topic19 |string |no |
| topic20 |string |no |

#### <a name="messagelist-list-of-messages"></a>MessageList: Liste der Nachrichten
| Name | Datentyp | Erforderlich |
| --- | --- | --- |
| messages |array |no |

#### <a name="messagebody-message-body"></a>MessageBody: Nachrichtentext
| Name | Datentyp | Erforderlich |
| --- | --- | --- |
| parsed |string |no |
| plain |string |no |
| rich |string |no |

#### <a name="likedby-liked-by"></a>LikedBy: Gefällt
| Name | Datentyp | Erforderlich |
| --- | --- | --- |
| count |integer |no |
| names |array |no |

#### <a name="yammmerentity-liked-by"></a>YammmerEntity: Gefällt
| Name | Datentyp | Erforderlich |
| --- | --- | --- |
| type |string |no |
| id |integer |no |
| full_name |string |no |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md)

[1]: ./media/connectors-create-api-yammer/connectionconfig1.png
[2]: ./media/connectors-create-api-yammer/connectionconfig2.png 
[3]: ./media/connectors-create-api-yammer/connectionconfig3.png
[4]: ./media/connectors-create-api-yammer/connectionconfig4.png
[5]: ./media/connectors-create-api-yammer/connectionconfig5.png



<!--HONumber=Jan17_HO3-->


