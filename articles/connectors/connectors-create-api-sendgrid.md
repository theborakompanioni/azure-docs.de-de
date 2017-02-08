---
title: SendGrid | Microsoft Docs
description: "Erstellen Sie Logik-Apps mit Azure App Service. Mit dem SendGrid-Verbindungsanbieter können Sie E-Mails senden und Empfängerlisten verwalten."
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: bc4f1fc2-824c-4ed7-8de8-e82baff3b746
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: b92f954680603891ced503a1134791312b5214f0
ms.openlocfilehash: e30231bd576436ae69f4fa42d0e2ab312c3938d6


---
# <a name="get-started-with-the-sendgrid-connector"></a>Erste Schritte mit dem SendGrid-Connector
Mit dem SendGrid-Verbindungsanbieter können Sie E-Mails senden und Empfängerlisten verwalten.

> [!NOTE]
> Diese Version des Artikels gilt für Logik-Apps mit der Schemaversion „2015-08-01-preview“. 
> 
> 

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md) beschrieben.

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Der SendGrid-Connector kann als Aktion verwendet werden. Er verfügt über Trigger. Alle Connectors unterstützen Daten im JSON- und XML-Format. 

 Der SendGrid-Connector verfügt über die folgenden Aktionen: Es gibt keine Trigger.

### <a name="sendgrid-actions"></a>SendGrid-Aktionen
Sie können diese Aktionen ausführen:

| Aktion | Beschreibung |
| --- | --- |
| [SendEmail](connectors-create-api-sendgrid.md#sendemail) |Sendet eine E-Mail über die SendGrid-API (maximal 10.000 Empfänger). |
| [AddRecipientToList](connectors-create-api-sendgrid.md#addrecipienttolist) |Fügt einer Empfängerliste einen einzelnen Empfänger hinzu. |

## <a name="create-a-connection-to-sendgrid"></a>Herstellen einer Verbindung mit SendGrid
Um Logik-Apps mit SendGrid zu erstellen, müssen Sie zuerst eine **Verbindung** erstellen und anschließend die Details für die folgenden Eigenschaften angeben: 

| Eigenschaft | Erforderlich | Beschreibung |
| --- | --- | --- |
| ApiKey |Ja |Angeben Ihres API-Schlüssels für SendGrid |

> [!INCLUDE [Steps to create a connection to SendGrid](../../includes/connectors-create-api-sendgrid.md)]
> 
> [!TIP]
> Sie können diese Verbindung in anderen Logik-Apps verwenden.
> 
> 

Nachdem Sie die Verbindung erstellt haben, können Sie sie zum Ausführen der Aktionen und zum Lauschen auf die in diesem Artikel beschriebenen Trigger verwenden.

## <a name="reference-for-sendgrid"></a>Referenz für SendGrid
Gilt für Version 1.0.

## <a name="sendemail"></a>SendEmail
E-Mail senden: Sendet eine E-Mail über die SendGrid-API (maximal 10.000 Empfänger). 

```POST: /api/mail.send.json``` 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| request | |Ja |body |(Keine) |Zu sendende E-Mail-Nachricht |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 429 |Zu viele Anforderungen |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="addrecipienttolist"></a>AddRecipientToList
Empfänger zu Liste hinzufügen: Fügt einer Empfängerliste einen einzelnen Empfänger hinzu. 

```POST: /v3/contactdb/lists/{listId}/recipients/{recipientId}``` 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| listId |string |Ja |path |(Keine) |Eindeutige ID der Empfängerliste |
| recipientId |string |Ja |path |(Keine) |Eindeutige ID des Empfängers |

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

## <a name="object-definitions"></a>Objektdefinitionen
### <a name="emailrequest"></a>EmailRequest
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Aus |string |Ja |
| fromname |string |Nein |
| to |string |Ja |
| toname |string |Nein |
| subject |string |Ja |
| body |string |Ja |
| ishtml |Boolescher Wert |Nein |
| cc |string |Nein |
| ccname |string |Nein |
| bcc |string |Nein |
| bccname |string |Nein |
| replyto |string |Nein |
| date |string |Nein |
| headers |string |Nein |
| files |array |Nein |
| filenames |array |Nein |

### <a name="emailresponse"></a>EmailResponse
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| message |string |Nein |

### <a name="recipientlists"></a>RecipientLists
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| lists |array |Nein |

### <a name="recipientlist"></a>RecipientList
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |integer |Nein |
| Name |string |Nein |
| recipient_count |integer |Nein |

### <a name="recipients"></a>Recipients
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| recipients |array |Nein |

### <a name="recipient"></a>Recipient
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| email |string |Nein |
| last_name |string |Nein |
| first_name |string |Nein |
| id |string |Nein |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md)




<!--HONumber=Jan17_HO3-->


