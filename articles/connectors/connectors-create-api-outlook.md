---
title: Outlook.com | Microsoft Docs
description: "Erstellen Sie Logik-Apps mit Azure App Service. Über den Outlook.com-Connector können Sie Ihre E-Mail und Ihre Kalender und Kontakte verwalten. Sie können verschiedene Aktionen ausführen, z.B. E-Mails senden, Besprechungen planen, Kontakte hinzufügen usw."
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: 87113c85-d158-4dd5-9ed5-5748130003d6
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: b92f954680603891ced503a1134791312b5214f0
ms.openlocfilehash: 409e1a104fa73d911ea508cbff311cb48fc20f9f


---
# <a name="get-started-with-the-outlookcom-connector"></a>Erste Schritte mit dem Outlook.com-Connector
Über den Outlook.com-Connector können Sie Ihre E-Mail und Ihre Kalender und Kontakte verwalten. Sie können verschiedene Aktionen ausführen, z.B. E-Mails senden, Besprechungen planen, Kontakte hinzufügen usw.

> [!NOTE]
> Diese Version des Artikels gilt für Logik-Apps mit der Schemaversion „2015-08-01-preview“.
>
>

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md) beschrieben.

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Der Outlook.com-Connector kann als Aktion verwendet werden. Er verfügt über Trigger. Alle Connectors unterstützen Daten im JSON- und XML-Format.

 Der Outlook.com-Connector verfügt über die folgenden Aktionen und/oder Trigger:

### <a name="outlookcom-actions"></a>Aktionen für Outlook.com
Sie können diese Aktionen ausführen:

| Aktion | Beschreibung |
| --- | --- |
| [GetEmails](connectors-create-api-outlook.md#getemails) |Ruft E-Mails aus einem Ordner ab. |
| [SendEmail](connectors-create-api-outlook.md#sendemail) |Sendet eine E-Mail. |
| [DeleteEmail](connectors-create-api-outlook.md#deleteemail) |Löscht eine E-Mail nach ID. |
| [MarkAsRead](connectors-create-api-outlook.md#markasread) |Markiert eine E-Mail als gelesen. |
| [ReplyTo](connectors-create-api-outlook.md#replyto) |Antwortet auf eine E-Mail. |
| [GetAttachment](connectors-create-api-outlook.md#getattachment) |Ruft die Anlage zu einer E-Mail anhand der ID ab. |
| [SendMailWithOptions](connectors-create-api-outlook.md#sendmailwithoptions) |Sendet eine E-Mail mit mehreren Optionen und wartet, bis der Empfänger mit einer der Optionen antwortet. |
| [SendApprovalMail](connectors-create-api-outlook.md#sendapprovalmail) |Sendet eine Genehmigungs-E-Mail und wartet auf eine Antwort des Empfängers. |
| [CalendarGetTables](connectors-create-api-outlook.md#calendargettables) |Ruft Kalender ab |
| [CalendarGetItems](connectors-create-api-outlook.md#calendargetitems) |Ruft Elemente aus einem Kalender ab |
| [CalendarPostItem](connectors-create-api-outlook.md#calendarpostitem) |Erstellt ein neues Ereignis |
| [CalendarGetItem](connectors-create-api-outlook.md#calendargetitem) |Ruft ein bestimmtes Element aus einem Kalender ab |
| [CalendarDeleteItem](connectors-create-api-outlook.md#calendardeleteitem) |Löscht ein Kalenderelement |
| [CalendarPatchItem](connectors-create-api-outlook.md#calendarpatchitem) |Aktualisiert ein Kalenderelement teilweise |
| [ContactGetTables](connectors-create-api-outlook.md#contactgettables) |Ruft Ordner des Typs „Kontakte“ ab |
| [ContactGetItems](connectors-create-api-outlook.md#contactgetitems) |Ruft Kontakte aus dem Ordner „Kontakte“ ab |
| [ContactPostItem](connectors-create-api-outlook.md#contactpostitem) |Erstellt einen neuen Kontakt |
| [ContactGetItem](connectors-create-api-outlook.md#contactgetitem) |Ruft einen bestimmten Kontakt aus einem Ordner „Kontakte“ ab |
| [ContactDeleteItem](connectors-create-api-outlook.md#contactdeleteitem) |Löscht einen Kontakt |
| [ContactPatchItem](connectors-create-api-outlook.md#contactpatchitem) |Aktualisiert einen Kontakt teilweise |

### <a name="outlookcom-triggers"></a>Trigger für Outlook.com
Sie können auf diese Ereignisse lauschen:

| Trigger | Beschreibung |
| --- | --- |
| Bei Start eines anstehenden Ereignisses |Löst einen Workflow aus, wenn ein anstehendes Kalenderereignis gestartet wird |
| Bei neuer E-Mail |Löst einen Workflow aus, wenn eine neue E-Mail eingeht |
| Bei neuen Elementen |Wird ausgelöst, wenn ein neues Kalenderelement erstellt wird |
| Bei aktualisierten Elementen |Wird ausgelöst, wenn ein Kalenderelement geändert wird |

## <a name="create-a-connection-to-outlookcom"></a>Herstellen einer Verbindung mit Outlook.com
Um Logik-Apps mit Outlook.com zu erstellen, müssen Sie zuerst eine **Verbindung** herstellen und anschließend die Details für die folgenden Eigenschaften angeben:

| Eigenschaft | Erforderlich | Beschreibung |
| --- | --- | --- |
| Tokenverschlüsselung |Ja |Angeben der Anmeldeinformationen für Outlook.com |

Nachdem Sie die Verbindung erstellt haben, können Sie sie zum Ausführen der Aktionen und zum Lauschen auf die in diesem Artikel beschriebenen Trigger verwenden.

> [!INCLUDE [Steps to create a connection to Outlook.com](../../includes/connectors-create-api-outlook.md)]
>
> [!TIP]
> Sie können diese Verbindung in anderen Logik-Apps verwenden.  
>
>

## <a name="reference-for-outlookcom"></a>Referenz für Outlook.com
Gilt für Version 1.0.

## <a name="onupcomingevents"></a>OnUpcomingEvents
Bei baldigem Start eines anstehenden Ereignisses: Löst einen Workflow aus, wenn ein anstehendes Kalenderereignis beginnt.

```GET: /Events/OnUpcomingEvents```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| Tabelle |string |Ja |query |(Keine) |Eindeutiger Bezeichner des Kalenders |
| lookAheadTimeInMinutes |integer |no |query |15 |Zeit (in Minuten) für Suche nach anstehenden Ereignissen. |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 202 |Vorgang erfolgreich |
| 400 |BadRequest |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="getemails"></a>GetEmails
E-Mails abrufen: Ruft E-Mails aus einem Ordner ab.

```GET: /Mail```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| folderPath |string |no |query |Posteingang |Pfad des Ordners zum Abrufen von E-Mails (Standard: Posteingang) |
| top |integer |no |query |10 |Anzahl der abzurufenden E-Mails (Standardeinstellung: 10) |
| fetchOnlyUnread |Boolescher Wert |no |query |true |Nur ungelesene E-Mails abrufen? |
| includeAttachments |Boolescher Wert |no |query |false |Falls auf „true“ festgelegt, werden Anlagen zusammen mit der E-Mail abgerufen. |
| searchQuery |string |no |query |(Keine) |Suchabfrage zum Filtern von E-Mails |
| skip |integer |no |query |0 |Anzahl der zu überspringenden E-Mails (Standardeinstellung: 0) |
| skipToken |string |no |query |(Keine) |Token überspringen, um neue Seite abzurufen |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |BadRequest |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="sendemail"></a>SendEmail
E-Mail senden: Sendet eine E-Mail.

```POST: /Mail```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| emailMessage | |Ja |body |(Keine) |E-Mail |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |BadRequest |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="deleteemail"></a>DeleteEmail
E-Mail löschen: Löscht eine E-Mail nach ID.

```DELETE: /Mail/{messageId}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| messageId |string |Ja |path |(Keine) |ID der zu löschenden E-Mail |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |BadRequest |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="markasread"></a>MarkAsRead
Als gelesen markieren: Markiert eine E-Mail als gelesen.

```POST: /Mail/MarkAsRead/{messageId}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| messageId |string |Ja |path |(Keine) |ID der E-Mail, die als gelesen markiert werden soll |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |BadRequest |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="replyto"></a>ReplyTo
E-Mail beantworten: Antwortet auf eine E-Mail.

```POST: /Mail/ReplyTo/{messageId}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| messageId |string |Ja |path |(Keine) |ID der zu beantwortenden E-Mail |
| comment |string |Ja |query |(Keine) |Antwortkommentar |
| replyAll |Boolescher Wert |no |query |false |Allen Empfängern antworten |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |BadRequest |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="getattachment"></a>GetAttachment
Anlage abrufen: Ruft die Anlage zu einer E-Mail anhand der ID ab.

```GET: /Mail/{messageId}/Attachments/{attachmentId}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| messageId |string |Ja |path |(Keine) |ID der E-Mail |
| attachmentId |string |Ja |path |(Keine) |ID der herunterzuladenden Anlage |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |BadRequest |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="onnewemail"></a>OnNewEmail
Bei neuer E-Mail: Löst einen Workflow aus, wenn eine neue E-Mail eingeht.

```GET: /Mail/OnNewEmail```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| folderPath |string |no |query |Posteingang |Abzurufender E-Mail-Ordner (Standard: Posteingang) |
| auf fest |string |no |query |(Keine) |E-Mail-Adressen der Empfänger |
| from |string |no |query |(Keine) |Absenderadresse |
| importance |string |no |query |Normal |Wichtigkeit der E-Mail (High, Normal, Low) (Standard: Normal) |
| fetchOnlyWithAttachment |Boolescher Wert |no |query |false |Nur E-Mail-Nachrichten mit einer Anlage abrufen |
| includeAttachments |Boolescher Wert |no |query |false |Anlagen einbeziehen |
| subjectFilter |string |no |query |(Keine) |Zeichenfolge, die in der Betreffzeile gesucht werden soll. |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 202 |Zulässig |
| 400 |BadRequest |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="sendmailwithoptions"></a>SendMailWithOptions
E-Mail mit Optionen senden: Sendet eine E-Mail mit mehreren Optionen und wartet, bis der Empfänger mit einer der Optionen antwortet.

```POST: /mailwithoptions/$subscriptions```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| optionsEmailSubscription | |Ja |body |(Keine) |Abonnementanforderung für E-Mail-Optionen |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 201 |Abonnement erstellt |
| 400 |BadRequest |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="sendapprovalmail"></a>SendApprovalMail
Genehmigungs-E-Mail senden: Sendet eine Genehmigungs-E-Mail und wartet auf eine Antwort des Empfängers.

```POST: /approvalmail/$subscriptions```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| approvalEmailSubscription | |Ja |body |(Keine) |Abonnementanforderung für Genehmigungs-E-Mail. |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 201 |Abonnement erstellt |
| 400 |BadRequest |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 500 |Interner Serverfehler |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="calendargettables"></a>CalendarGetTables
Kalender abrufen: Ruft Kalender ab.

```GET: /datasets/calendars/tables```

Es gibt keine Parameter für diesen Aufruf

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="calendargetitems"></a>CalendarGetItems
Ereignisse abrufen: Ruft Elemente aus einem Kalender ab.

```GET: /datasets/calendars/tables/{table}/items```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| Tabelle |string |Ja |path |(Keine) |Eindeutiger Bezeichner des abzurufenden Kalenders |
| $filter |string |no |query |Keine |Eine ODATA-Filterabfrage zum Einschränken der Anzahl der Einträge |
| $orderby |string |no |query |Keine |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |
| $skip |integer |no |query |Keine |Anzahl der zu überspringenden Einträge (Standardeinstellung = 0) |
| $top |integer |no |query |Keine |Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256) |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="calendarpostitem"></a>CalendarPostItem
Ereignis erstellen: Erstellt ein neues Ereignis

```POST: /datasets/calendars/tables/{table}/items```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| Tabelle |string |Ja |path |(Keine) |Eindeutiger Bezeichner eines Kalenders |
| item | |Ja |body |(Keine) |Zu erstellendes Kalenderelement |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="calendargetitem"></a>CalendarGetItem
Ereignis abrufen: Ruft ein bestimmtes Element aus einem Kalender ab.

```GET: /datasets/calendars/tables/{table}/items/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| Tabelle |string |Ja |path |(Keine) |Eindeutiger Bezeichner eines Kalenders |
| id |string |Ja |path |(Keine) |Eindeutiger Bezeichner eines abzurufenden Kalenderelements |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="calendardeleteitem"></a>CalendarDeleteItem
Ereignis löschen: Löscht ein Kalenderelement.

```DELETE: /datasets/calendars/tables/{table}/items/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| Tabelle |string |Ja |path |(Keine) |Eindeutiger Bezeichner eines Kalenders |
| id |string |Ja |path |(Keine) |Eindeutiger Bezeichner eines zu löschenden Kalenderelements |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="calendarpatchitem"></a>CalendarPatchItem
Ereignis aktualisieren: Aktualisiert ein Kalenderelement teilweise.

```PATCH: /datasets/calendars/tables/{table}/items/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| Tabelle |string |Ja |path |(Keine) |Eindeutiger Bezeichner eines Kalenders |
| id |string |Ja |path |(Keine) |Eindeutiger Bezeichner eines zu aktualisierenden Kalenderelements |
| item | |Ja |body |(Keine) |Zu aktualisierendes Kalenderelement |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="calendargetonnewitems"></a>CalendarGetOnNewItems
Bei neuen Elementen: Wird ausgelöst, wenn ein neues Kalenderelement erstellt wird.

```GET: /datasets/calendars/tables/{table}/onnewitems```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| Tabelle |string |Ja |path |(Keine) |Eindeutiger Bezeichner eines Kalenders |
| $filter |string |no |query |Keine |Eine ODATA-Filterabfrage zum Einschränken der Anzahl der Einträge |
| $orderby |string |no |query |Keine |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |
| $skip |integer |no |query |Keine |Anzahl der zu überspringenden Einträge (Standardeinstellung = 0) |
| $top |integer |no |query |Keine |Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256) |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="calendargetonupdateditems"></a>CalendarGetOnUpdatedItems
Bei aktualisierten Elementen: Wird ausgelöst, wenn ein Kalenderelement geändert wird.

```GET: /datasets/calendars/tables/{table}/onupdateditems```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| Tabelle |string |Ja |path |(Keine) |Eindeutiger Bezeichner eines Kalenders |
| $filter |string |no |query |Keine |Eine ODATA-Filterabfrage zum Einschränken der Anzahl der Einträge |
| $orderby |string |no |query |Keine |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |
| $skip |integer |no |query |Keine |Anzahl der zu überspringenden Einträge (Standardeinstellung = 0) |
| $top |integer |no |query |Keine |Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256) |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="contactgettables"></a>ContactGetTables
Ordner „Kontakte“ abrufen: Ruft Ordner des Typs „Kontakte“ ab.

```GET: /datasets/contacts/tables```

Es gibt keine Parameter für diesen Aufruf

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="contactgetitems"></a>ContactGetItems
Kontakte abrufen: Ruft Kontakte aus einem Ordner des Typs „Kontakte“ ab.

```GET: /datasets/contacts/tables/{table}/items```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| Tabelle |string |Ja |path |(Keine) |Eindeutiger Bezeichner des abzurufenden Ordners „Kontakte“ |
| $filter |string |no |query |Keine |Eine ODATA-Filterabfrage zum Einschränken der Anzahl der Einträge |
| $orderby |string |no |query |Keine |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |
| $skip |integer |no |query |Keine |Anzahl der zu überspringenden Einträge (Standardeinstellung = 0) |
| $top |integer |no |query |Keine |Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256) |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="contactpostitem"></a>ContactPostItem
Kontakt erstellen: Erstellt einen neuen Kontakt.

```POST: /datasets/contacts/tables/{table}/items```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| Tabelle |string |Ja |path |(Keine) |Eindeutiger Bezeichner eines Ordners des Typs „Kontakte“ |
| item | |Ja |body |(Keine) |Zu erstellender Kontakt |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="contactgetitem"></a>ContactGetItem
Kontakt abrufen: Ruft einen bestimmten Kontakt aus einem Ordner „Kontakte“ ab.

```GET: /datasets/contacts/tables/{table}/items/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| Tabelle |string |Ja |path |(Keine) |Eindeutiger Bezeichner eines Ordners des Typs „Kontakte“ |
| id |string |Ja |path |(Keine) |Eindeutiger Bezeichner des abzurufenden Kontakts |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="contactdeleteitem"></a>ContactDeleteItem
Kontakt löschen: Löscht einen Kontakt.

```DELETE: /datasets/contacts/tables/{table}/items/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| Tabelle |string |Ja |path |(Keine) |Eindeutiger Bezeichner eines Ordners des Typs „Kontakte“ |
| id |string |Ja |path |(Keine) |Eindeutiger Bezeichner des zu löschenden Kontakts |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="contactpatchitem"></a>ContactPatchItem
Kontakt aktualisieren: Aktualisiert einen Kontakt teilweise.

```PATCH: /datasets/contacts/tables/{table}/items/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| Tabelle |string |Ja |path |(Keine) |Eindeutiger Bezeichner eines Ordners des Typs „Kontakte“ |
| id |string |Ja |path |(Keine) |Eindeutiger Bezeichner des zu aktualisierenden Kontakts |
| item | |Ja |body |(Keine) |Zu aktualisierendes Kontaktelement |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="object-definitions"></a>Objektdefinitionen
### <a name="triggerbatchresponseidictionarystringobject"></a>TriggerBatchResponse[IDictionary[String,Object]]
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| value |array |Nein |

### <a name="object"></a>Objekt
### <a name="sendmessage"></a>SendMessage
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Anlagen |array |Nein |
| From |string |Nein |
| Cc |string |Nein |
| Bcc |string |Nein |
| Subject |string |Ja |
| Body |string |Ja |
| Importance |string |Nein |
| IsHtml |Boolescher Wert |Nein |
| To |string |Ja |

### <a name="sendattachment"></a>SendAttachment
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| @odata.type |string |Nein |
| Name |string |Ja |
| ContentBytes |string |Ja |

### <a name="receivemessage"></a>ReceiveMessage
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |Nein |
| IsRead |Boolescher Wert |Nein |
| HasAttachment |Boolescher Wert |Nein |
| DateTimeReceived |string |Nein |
| Attachments |array |Nein |
| From |string |Nein |
| Cc |string |Nein |
| Bcc |string |Nein |
| Subject |string |Ja |
| Body |string |Ja |
| Importance |string |Nein |
| IsHtml |Boolescher Wert |Nein |
| To |string |Ja |

### <a name="receiveattachment"></a>ReceiveAttachment
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |Ja |
| ContentType |string |Ja |
| @odata.type |string |Nein |
| Name |string |Ja |
| ContentBytes |string |Ja |

### <a name="digestmessage"></a>DigestMessage
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Betreff |string |Ja |
| Body |string |Nein |
| Priorität |string |Nein |
| Digest |array |Ja |
| Attachments |array |Nein |
| To |string |Ja |

### <a name="triggerbatchresponsereceivemessage"></a>TriggerBatchResponse[ReceiveMessage]
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| value |array |Nein |

### <a name="datasetsmetadata"></a>DataSetsMetadata
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| tabular |nicht definiert |Nein |
| Blob |nicht definiert |Nein |

### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Quelle |string |Nein |
| displayName |string |Nein |
| urlEncoding |string |Nein |
| tableDisplayName |string |Nein |
| tablePluralName |string |Nein |

### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Quelle |string |Nein |
| displayName |string |Nein |
| urlEncoding |string |Nein |

### <a name="tablemetadata"></a>TableMetadata
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Name |string |Nein |
| title |string |Nein |
| x-ms-permission |string |Nein |
| x-ms-capabilities |nicht definiert |Nein |
| schema |nicht definiert |Nein |

### <a name="tablecapabilitiesmetadata"></a>TableCapabilitiesMetadata
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| sortRestrictions |nicht definiert |Nein |
| filterRestrictions |nicht definiert |Nein |
| filterFunctions |array |Nein |

### <a name="tablesortrestrictionsmetadata"></a>TableSortRestrictionsMetadata
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| sortable |Boolescher Wert |Nein |
| unsortableProperties |array |Nein |
| ascendingOnlyProperties |array |Nein |

### <a name="tablefilterrestrictionsmetadata"></a>TableFilterRestrictionsMetadata
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| filterable |Boolescher Wert |Nein |
| nonFilterableProperties |array |Nein |
| requiredProperties |array |Nein |

### <a name="optionsemailsubscription"></a>OptionsEmailSubscription
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| NotificationUrl |string |Nein |
| Nachricht |nicht definiert |Nein |

### <a name="messagewithoptions"></a>MessageWithOptions
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Betreff |string |Ja |
| Options |string |Ja |
| Body |string |Nein |
| Priorität |string |Nein |
| Attachments |array |Nein |
| To |string |Ja |

### <a name="subscriptionresponse"></a>SubscriptionResponse
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |Nein |
| resource |string |Nein |
| notificationType |string |Nein |
| notificationUrl |string |Nein |

### <a name="approvalemailsubscription"></a>ApprovalEmailSubscription
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| NotificationUrl |string |Nein |
| Nachricht |nicht definiert |Nein |

### <a name="approvalmessage"></a>ApprovalMessage
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Betreff |string |Ja |
| Options |string |Ja |
| Body |string |Nein |
| Priorität |string |Nein |
| Attachments |array |Nein |
| To |string |Ja |

### <a name="approvalemailresponse"></a>ApprovalEmailResponse
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| SelectedOption |string |Nein |

### <a name="tableslist"></a>TablesList
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| value |array |Nein |

### <a name="table"></a>Tabelle
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Name |string |Nein |
| DisplayName |string |Nein |

### <a name="item"></a>Item
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| ItemInternalId |string |Nein |

### <a name="calendaritemslist"></a>CalendarItemsList
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| value |array |Nein |

### <a name="calendaritem"></a>CalendarItem
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| ItemInternalId |string |Nein |

### <a name="contactitemslist"></a>ContactItemsList
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| value |array |Nein |

### <a name="contactitem"></a>ContactItem
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| ItemInternalId |string |Nein |

### <a name="datasetslist"></a>DataSetsList
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| value |array |Nein |

### <a name="dataset"></a>DataSet
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Name |string |Nein |
| DisplayName |string |Nein |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md)



<!--HONumber=Jan17_HO3-->


