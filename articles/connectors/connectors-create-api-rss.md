---
title: RSS | Microsoft Docs
description: "Erstellen Sie Logik-Apps mit Azure App Service. Der RSS-Connector ermöglicht Benutzers das Veröffentlichen und Abrufen von Feedelementen. Mit ihm können Benutzer auch Operationen auslösen, wenn ein Element im Feed veröffentlicht wird."
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: a10a6277-ed29-4e68-a881-ccdad6fd0ad8
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: b92f954680603891ced503a1134791312b5214f0
ms.openlocfilehash: 5e13e126fecda66a453b4ced619016121af98b2c


---
# <a name="get-started-with-the-rss-connector"></a>Erste Schritte mit dem RSS-Connector
RSS ist ein beliebtes Webformat zum Veröffentlichen von Inhalten, die häufig aktualisiert werden. Beispiele hierfür wären etwa Blogbeiträge und Schlagzeilen.  Viele Inhaltsherausgeber bieten einen RSS-Feed an, der von Benutzern abonniert werden kann.  Mit dem RSS-Connector können Sie Feedinformationen abrufen und Abläufe auslösen, wenn in einem RSS-Feed neue Elemente veröffentlicht werden.

> [!NOTE]
> Diese Version des Artikels gilt für Logik-Apps mit der Schemaversion „2015-08-01-preview“. 
> 
> 

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md) beschrieben.

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Der RSS-Connector kann als Aktion verwendet werden. Er verfügt über Trigger. Alle Connectors unterstützen Daten im JSON- und XML-Format. 

 Der RSS-Connector verfügt über die folgenden Aktionen und/oder Trigger:

### <a name="rss-actions"></a>RSS-Aktionen
Sie können diese Aktionen ausführen:

| Aktion | Beschreibung |
| --- | --- |
| [ListFeedItems](connectors-create-api-rss.md#listfeeditems) |Ruft alle RSS-Feedelemente ab. |

### <a name="rss-triggers"></a>RSS-Trigger
Sie können auf diese Ereignisse lauschen:

| Trigger | Beschreibung |
| --- | --- |
| Wenn ein neues Feedelement veröffentlicht wird |Löst einen Workflow aus, sobald ein neuer Feed veröffentlicht wird. |

## <a name="create-a-connection-to-rss"></a>Herstellen einer Verbindung mit RSS
> [!INCLUDE [Steps to create a connection to an RSS feed](../../includes/connectors-create-api-rss.md)]
> 
> [!TIP]
> Sie können diese Verbindung in anderen Logik-Apps verwenden.
> 
> 

## <a name="reference-for-rss"></a>Referenz für RSS
Gilt für Version 1.0.

## <a name="onnewfeed"></a>OnNewFeed
Wenn ein neues Feedelement veröffentlicht wird: Löst einen Workflow aus, sobald ein neuer Feed veröffentlicht wird. 

```GET: /OnNewFeed``` 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| feedUrl |string |Ja |query |(Keine) |Feed-URL |

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

## <a name="listfeeditems"></a>ListFeedItems
Alle RSS-Feedelemente auflisten: Ruft alle RSS-Feedelemente ab. 

```GET: /ListFeedItems``` 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| feedUrl |string |Ja |query |(Keine) |Feed-URL |

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
### <a name="triggerbatchresponsefeeditem"></a>TriggerBatchResponse[FeedItem]
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| value |array |Nein |

### <a name="feeditem"></a>FeedItem
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |Ja |
| title |string |Ja |
| Inhalt |string |Ja |
| links |array |Nein |
| updatedOn |string |Nein |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md)




<!--HONumber=Feb17_HO3-->


