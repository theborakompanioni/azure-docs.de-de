---
title: SendGrid | Microsoft Docs
description: Erstellen Sie Logik-Apps mit Azure App Service. Mit dem SendGrid-Verbindungsanbieter können Sie E-Mails senden und Empfängerlisten verwalten.
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: ''
tags: connectors

ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: deonhe

---
# Erste Schritte mit dem SendGrid-Connector
Mit dem SendGrid-Verbindungsanbieter können Sie E-Mails senden und Empfängerlisten verwalten.

> [!NOTE]
> Diese Version des Artikels gilt für die Schemaversion 2015-08-01-preview für Logik-Apps.
> 
> 

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md) beschrieben.

## Trigger und Aktionen
Der SendGrid-Connector kann als Aktion verwendet werden. Er verfügt über Trigger. Alle Connectors unterstützen Daten im JSON- und XML-Format.

 Der SendGrid-Connector verfügt über die folgenden Aktionen: Es gibt keine Trigger.

### SendGrid-Aktionen
Sie können diese Aktionen ausführen:

| Aktion | Beschreibung |
| --- | --- |
| [SendEmail](connectors-create-api-sendgrid.md#sendemail) |Sendet eine E-Mail über die SendGrid-API (maximal 10.000 Empfänger). |
| [AddRecipientToList](connectors-create-api-sendgrid.md#addrecipienttolist) |Fügt einer Empfängerliste einen einzelnen Empfänger hinzu. |

## Herstellen einer Verbindung mit SendGrid
Um Logik-Apps mit SendGrid zu erstellen, müssen Sie zuerst eine **Verbindung** erstellen und anschließend die Details für die folgenden Eigenschaften angeben:

| Eigenschaft | Erforderlich | Beschreibung |
| --- | --- | --- |
| ApiKey |Ja |Angeben Ihres API-Schlüssels für SendGrid |

> [!INCLUDE [Schritte zum Herstellen einer Verbindung mit SendGrid](../../includes/connectors-create-api-sendgrid.md)]
> 
> [!TIP]
> Sie können diese Verbindung in anderen Logik-Apps verwenden.
> 
> 

Nachdem Sie die Verbindung erstellt haben, können Sie sie zum Ausführen der Aktionen und zum Lauschen auf die in diesem Artikel beschriebenen Trigger verwenden.

## Referenz für SendGrid
Gilt für Version 1.0.

## SendEmail
E-Mail senden: Sendet eine E-Mail über die SendGrid-API (maximal 10.000 Empfänger).

```POST: /api/mail.send.json```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| request | |Ja |body |(Keine) |Zu sendende E-Mail-Nachricht |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten |
| 404 |Nicht gefunden |
| 429 |Zu viele Anforderungen |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| default |Fehler beim Vorgang. |

## AddRecipientToList
Empfänger zu Liste hinzufügen: Fügt einer Empfängerliste einen einzelnen Empfänger hinzu.

```POST: /v3/contactdb/lists/{listId}/recipients/{recipientId}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| listId |string |Ja |path |(Keine) |Eindeutige ID der Empfängerliste |
| recipientId |string |Ja |path |(Keine) |Eindeutige ID des Empfängers |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| default |Fehler beim Vorgang. |

## Objektdefinitionen
### EmailRequest
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| from |string |Ja |
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

### EmailResponse
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| message |string |Nein |

### RecipientLists
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| lists |array |Nein |

### RecipientList
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |integer |Nein |
| Name |string |Nein |
| recipient\_count |integer |Nein |

### Recipients
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| recipients |array |Nein |

### Recipient
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| email |string |Nein |
| last\_name |string |Nein |
| first\_name |string |Nein |
| id |string |Nein |

## Nächste Schritte
[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md)

<!---HONumber=AcomDC_0824_2016-->