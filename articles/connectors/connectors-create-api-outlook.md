<properties
pageTitle="Outlook.com | Microsoft Azure"
description="Erstellen Sie Logik-Apps mit Azure App Service. Über den Outlook.com-Connector können Sie Ihre E-Mail und Ihre Kalender und Kontakte verwalten. Sie können verschiedene Aktionen ausführen, z.B. E-Mails senden, Besprechungen planen, Kontakte hinzufügen usw."
services="app-servicelogic"	
documentationCenter=".net,nodejs,java" 	
authors="msftman"	
manager="erikre"	
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="04/29/2016"
ms.author="deonhe"/>

# Erste Schritte mit dem Outlook.com-Connector

Über den Outlook.com-Connector können Sie Ihre E-Mail und Ihre Kalender und Kontakte verwalten. Sie können verschiedene Aktionen ausführen, z.B. E-Mails senden, Besprechungen planen, Kontakte hinzufügen usw.

Der Outlook.com-Connector kann verwendet werden in:

- [Logik-Apps](../app-service-logic/app-service-logic-what-are-logic-apps.md)
- [PowerApps](http://powerapps.microsoft.com)
- [Flow](http://flow.microsoft.com)

>[AZURE.NOTE] Diese Version des Artikels gilt für die Schemaversion 2015-08-01-preview für Logik-Apps.

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md) beschrieben.

## Trigger und Aktionen

Der Outlook.com-Connector kann als Aktion verwendet werden. Er verfügt über Trigger. Alle Connectors unterstützen Daten im JSON- und XML-Format.

 Der Outlook.com-Connector verfügt über die folgenden Aktionen und/oder Trigger:

### Aktionen für Outlook.com
Sie können diese Aktionen ausführen:

|Aktion|Beschreibung|
|--- | ---|
|[GetEmails](connectors-create-api-outlook.md#GetEmails)|Ruft E-Mails aus einem Ordner ab.|
|[SendEmail](connectors-create-api-outlook.md#SendEmail)|Sendet eine E-Mail.|
|[DeleteEmail](connectors-create-api-outlook.md#DeleteEmail)|Löscht eine E-Mail nach ID.|
|[MarkAsRead](connectors-create-api-outlook.md#MarkAsRead)|Markiert eine E-Mail als gelesen.|
|[ReplyTo](connectors-create-api-outlook.md#ReplyTo)|Antwortet auf eine E-Mail.|
|[GetAttachment](connectors-create-api-outlook.md#GetAttachment)|Ruft die Anlage zu einer E-Mail anhand der ID ab.|
|[SendMailWithOptions](connectors-create-api-outlook.md#SendMailWithOptions)|Sendet eine E-Mail mit mehreren Optionen und wartet, bis der Empfänger mit einer der Optionen antwortet.|
|[SendApprovalMail](connectors-create-api-outlook.md#SendApprovalMail)|Sendet eine Genehmigungs-E-Mail und wartet auf eine Antwort des Empfängers.|
|[CalendarGetTables](connectors-create-api-outlook.md#CalendarGetTables)|Ruft Kalender ab|
|[CalendarGetItems](connectors-create-api-outlook.md#CalendarGetItems)|Ruft Elemente aus einem Kalender ab|
|[CalendarPostItem](connectors-create-api-outlook.md#CalendarPostItem)|Erstellt ein neues Ereignis|
|[CalendarGetItem](connectors-create-api-outlook.md#CalendarGetItem)|Ruft ein bestimmtes Element aus einem Kalender ab|
|[CalendarDeleteItem](connectors-create-api-outlook.md#CalendarDeleteItem)|Löscht ein Kalenderelement|
|[CalendarPatchItem](connectors-create-api-outlook.md#CalendarPatchItem)|Aktualisiert ein Kalenderelement teilweise|
|[ContactGetTables](connectors-create-api-outlook.md#ContactGetTables)|Ruft Ordner des Typs „Kontakte“ ab|
|[ContactGetItems](connectors-create-api-outlook.md#ContactGetItems)|Ruft Kontakte aus dem Ordner „Kontakte“ ab|
|[ContactPostItem](connectors-create-api-outlook.md#ContactPostItem)|Erstellt einen neuen Kontakt|
|[ContactGetItem](connectors-create-api-outlook.md#ContactGetItem)|Ruft einen bestimmten Kontakt aus einem Ordner „Kontakte“ ab|
|[ContactDeleteItem](connectors-create-api-outlook.md#ContactDeleteItem)|Löscht einen Kontakt|
|[ContactPatchItem](connectors-create-api-outlook.md#ContactPatchItem)|Aktualisiert einen Kontakt teilweise|
### Trigger für Outlook.com
Sie können auf diese Ereignisse lauschen:

|Trigger | Beschreibung|
|--- | ---|
|Bei Start eines anstehenden Ereignisses|Löst einen Workflow aus, wenn ein anstehendes Kalenderereignis gestartet wird|
|Bei neuer E-Mail|Löst einen Workflow aus, wenn eine neue E-Mail eingeht|
|Bei neuen Elementen|Wird ausgelöst, wenn ein neues Kalenderelement erstellt wird|
|Bei aktualisierten Elementen|Wird ausgelöst, wenn ein Kalenderelement geändert wird|


## Herstellen einer Verbindung mit Outlook.com
Um Logik-Apps mit Outlook.com zu erstellen, müssen Sie zuerst eine **Verbindung** herstellen und anschließend die Details für die folgenden Eigenschaften angeben:

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|Tokenverschlüsselung|Ja|Angeben der Anmeldeinformationen für Outlook.com|
Nachdem Sie die Verbindung erstellt haben, können Sie sie zum Ausführen der Aktionen und zum Lauschen auf die in diesem Artikel beschriebenen Trigger verwenden.

>[AZURE.INCLUDE [Schritte zum Herstellen einer Verbindung mit Outlook.com](../../includes/connectors-create-api-outlook.md)]

>[AZURE.TIP] Sie können diese Verbindung in anderen Logik-Apps verwenden.

## Referenz für Outlook.com
Gilt für Version: 1.0.

## OnUpcomingEvents
Bei baldigem Start eines anstehenden Ereignisses: Löst einen Workflow aus, wenn ein anstehendes Kalenderereignis beginnt.

```GET: /Events/OnUpcomingEvents```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|table|string|Ja|query|(Keine)|Eindeutiger Bezeichner des Kalenders|
|lookAheadTimeInMinutes|integer|no|query|15|Zeit (in Minuten) für Suche nach anstehenden Ereignissen.|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|202|Vorgang erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|403|Verboten (403)|
|500|Interner Serverfehler|
|default|Fehler beim Vorgang.|


## GetEmails
E-Mails abrufen: Ruft E-Mails aus einem Ordner ab.

```GET: /Mail```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|folderPath|string|no|query|Posteingang|Pfad des Ordners zum Abrufen von E-Mails (Standard: Posteingang)|
|top|integer|no|query|10|Anzahl der abzurufenden E-Mails (Standardeinstellung: 10)|
|fetchOnlyUnread|Boolescher Wert|no|query|true|Nur ungelesene E-Mails abrufen?|
|includeAttachments|Boolescher Wert|no|query|false|Falls auf „true“ festgelegt, werden Anlagen zusammen mit der E-Mail abgerufen.|
|searchQuery|string|no|query|(Keine)|Suchabfrage zum Filtern von E-Mails|
|skip|integer|no|query|0|Anzahl der zu überspringenden E-Mails (Standardeinstellung: 0)|
|skipToken|string|no|query|(Keine)|Token überspringen, um neue Seite abzurufen|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|403|Verboten (403)|
|500|Interner Serverfehler|
|default|Fehler beim Vorgang.|


## SendEmail
E-Mail senden: Sendet eine E-Mail.

```POST: /Mail```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|emailMessage| |Ja|body|(Keine)|E-Mail|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|403|Verboten (403)|
|500|Interner Serverfehler|
|default|Fehler beim Vorgang.|


## DeleteEmail
E-Mail löschen: Löscht eine E-Mail nach ID.

```DELETE: /Mail/{messageId}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|messageId|string|Ja|path|(Keine)|ID der zu löschenden E-Mail|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|403|Verboten (403)|
|500|Interner Serverfehler|
|default|Fehler beim Vorgang.|


## MarkAsRead
Als gelesen markieren: Markiert eine E-Mail als gelesen.

```POST: /Mail/MarkAsRead/{messageId}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|messageId|string|Ja|path|(Keine)|ID der E-Mail, die als gelesen markiert werden soll|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|403|Verboten (403)|
|500|Interner Serverfehler|
|default|Fehler beim Vorgang.|


## ReplyTo
E-Mail beantworten: Antwortet auf eine E-Mail.

```POST: /Mail/ReplyTo/{messageId}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|messageId|string|Ja|path|(Keine)|ID der zu beantwortenden E-Mail|
|comment|string|Ja|query|(Keine)|Antwortkommentar|
|replyAll|Boolescher Wert|no|query|false|Allen Empfängern antworten|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|403|Verboten (403)|
|500|Interner Serverfehler|
|default|Fehler beim Vorgang.|


## GetAttachment
Anlage abrufen: Ruft die Anlage zu einer E-Mail anhand der ID ab.

```GET: /Mail/{messageId}/Attachments/{attachmentId}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|messageId|string|Ja|path|(Keine)|ID der E-Mail|
|attachmentId|string|Ja|path|(Keine)|ID der herunterzuladenden Anlage|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|403|Verboten (403)|
|500|Interner Serverfehler|
|default|Fehler beim Vorgang.|


## OnNewEmail
Bei neuer E-Mail: Löst einen Workflow aus, wenn eine neue E-Mail eingeht.

```GET: /Mail/OnNewEmail```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|folderPath|string|no|query|Posteingang|Abzurufender E-Mail-Ordner (Standard: Posteingang)|
|to|string|no|query|(Keine)|E-Mail-Adressen der Empfänger|
|from|string|no|query|(Keine)|Absenderadresse|
|importance|string|no|query|Normal|Wichtigkeit der E-Mail (High, Normal, Low) (Standard: Normal)|
|fetchOnlyWithAttachment|Boolescher Wert|no|query|false|Nur E-Mail-Nachrichten mit einer Anlage abrufen|
|includeAttachments|Boolescher Wert|no|query|false|Anlagen einbeziehen|
|subjectFilter|string|no|query|(Keine)|Zeichenfolge, die in der Betreffzeile gesucht werden soll.|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|202|Zulässig|
|400|BadRequest|
|401|Nicht autorisiert|
|403|Verboten (403)|
|500|Interner Serverfehler|
|default|Fehler beim Vorgang.|


## SendMailWithOptions
E-Mail mit Optionen senden: Sendet eine E-Mail mit mehreren Optionen und wartet, bis der Empfänger mit einer der Optionen antwortet.

```POST: /mailwithoptions/$subscriptions```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|optionsEmailSubscription| |Ja|body|(Keine)|Abonnementanforderung für E-Mail-Optionen|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|201|Abonnement erstellt|
|400|BadRequest|
|401|Nicht autorisiert|
|403|Verboten (403)|
|500|Interner Serverfehler|
|default|Fehler beim Vorgang.|


## SendApprovalMail
Genehmigungs-E-Mail senden: Sendet eine Genehmigungs-E-Mail und wartet auf eine Antwort des Empfängers.

```POST: /approvalmail/$subscriptions```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|approvalEmailSubscription| |Ja|body|(Keine)|Abonnementanforderung für Genehmigungs-E-Mail.|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|201|Abonnement erstellt|
|400|BadRequest|
|401|Nicht autorisiert|
|403|Verboten (403)|
|500|Interner Serverfehler|
|default|Fehler beim Vorgang.|


## CalendarGetTables
Kalender abrufen: Ruft Kalender ab.

```GET: /datasets/calendars/tables```

Es gibt keine Parameter für diesen Aufruf
#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


## CalendarGetItems
Ereignisse abrufen: Ruft Elemente aus einem Kalender ab.

```GET: /datasets/calendars/tables/{table}/items```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|table|string|Ja|path|(Keine)|Eindeutiger Bezeichner des abzurufenden Kalenders|
|$filter|string|no|query|(Keine)|Eine ODATA-Filterabfrage zum Einschränken der Anzahl der Einträge|
|$orderby|string|no|query|(Keine)|Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen|
|$skip|integer|no|query|(Keine)|Anzahl der zu überspringenden Einträge (Standardeinstellung = 0)|
|$top|integer|no|query|(Keine)|Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256)|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


## CalendarPostItem
Ereignis erstellen: Erstellt ein neues Ereignis

```POST: /datasets/calendars/tables/{table}/items```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|table|string|Ja|path|(Keine)|Eindeutiger Bezeichner eines Kalenders|
|item| |Ja|body|(Keine)|Zu erstellendes Kalenderelement|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


## CalendarGetItem
Ereignis abrufen: Ruft ein bestimmtes Element aus einem Kalender ab.

```GET: /datasets/calendars/tables/{table}/items/{id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|table|string|Ja|path|(Keine)|Eindeutiger Bezeichner eines Kalenders|
|id|string|Ja|path|(Keine)|Eindeutiger Bezeichner eines abzurufenden Kalenderelements|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


## CalendarDeleteItem
Ereignis löschen: Löscht ein Kalenderelement.

```DELETE: /datasets/calendars/tables/{table}/items/{id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|table|string|Ja|path|(Keine)|Eindeutiger Bezeichner eines Kalenders|
|id|string|Ja|path|(Keine)|Eindeutiger Bezeichner eines zu löschenden Kalenderelements|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


## CalendarPatchItem
Ereignis aktualisieren: Aktualisiert ein Kalenderelement teilweise.

```PATCH: /datasets/calendars/tables/{table}/items/{id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|table|string|Ja|path|(Keine)|Eindeutiger Bezeichner eines Kalenders|
|id|string|Ja|path|(Keine)|Eindeutiger Bezeichner eines zu aktualisierenden Kalenderelements|
|item| |Ja|body|(Keine)|Zu aktualisierendes Kalenderelement|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


## CalendarGetOnNewItems
Bei neuen Elementen: Wird ausgelöst, wenn ein neues Kalenderelement erstellt wird.

```GET: /datasets/calendars/tables/{table}/onnewitems```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|table|string|Ja|path|(Keine)|Eindeutiger Bezeichner eines Kalenders|
|$filter|string|no|query|(Keine)|Eine ODATA-Filterabfrage zum Einschränken der Anzahl der Einträge|
|$orderby|string|no|query|(Keine)|Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen|
|$skip|integer|no|query|(Keine)|Anzahl der zu überspringenden Einträge (Standardeinstellung = 0)|
|$top|integer|no|query|(Keine)|Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256)|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


## CalendarGetOnUpdatedItems
Bei aktualisierten Elementen: Wird ausgelöst, wenn ein Kalenderelement geändert wird.

```GET: /datasets/calendars/tables/{table}/onupdateditems```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|table|string|Ja|path|(Keine)|Eindeutiger Bezeichner eines Kalenders|
|$filter|string|no|query|(Keine)|Eine ODATA-Filterabfrage zum Einschränken der Anzahl der Einträge|
|$orderby|string|no|query|(Keine)|Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen|
|$skip|integer|no|query|(Keine)|Anzahl der zu überspringenden Einträge (Standardeinstellung = 0)|
|$top|integer|no|query|(Keine)|Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256)|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


## ContactGetTables
Ordner „Kontakte“ abrufen: Ruft Ordner des Typs „Kontakte“ ab.

```GET: /datasets/contacts/tables```

Es gibt keine Parameter für diesen Aufruf
#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


## ContactGetItems
Kontakte abrufen: Ruft Kontakte aus einem Ordner des Typs „Kontakte“ ab.

```GET: /datasets/contacts/tables/{table}/items```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|table|string|Ja|path|(Keine)|Eindeutiger Bezeichner des abzurufenden Ordners „Kontakte“|
|$filter|string|no|query|(Keine)|Eine ODATA-Filterabfrage zum Einschränken der Anzahl der Einträge|
|$orderby|string|no|query|(Keine)|Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen|
|$skip|integer|no|query|(Keine)|Anzahl der zu überspringenden Einträge (Standardeinstellung = 0)|
|$top|integer|no|query|(Keine)|Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256)|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


## ContactPostItem
Kontakt erstellen: Erstellt einen neuen Kontakt.

```POST: /datasets/contacts/tables/{table}/items```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|table|string|Ja|path|(Keine)|Eindeutiger Bezeichner eines Ordners des Typs „Kontakte“|
|item| |Ja|body|(Keine)|Zu erstellender Kontakt|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


## ContactGetItem
Kontakt abrufen: Ruft einen bestimmten Kontakt aus einem Ordner „Kontakte“ ab.

```GET: /datasets/contacts/tables/{table}/items/{id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|table|string|Ja|path|(Keine)|Eindeutiger Bezeichner eines Ordners des Typs „Kontakte“|
|id|string|Ja|path|(Keine)|Eindeutiger Bezeichner des abzurufenden Kontakts|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


## ContactDeleteItem
Kontakt löschen: Löscht einen Kontakt.

```DELETE: /datasets/contacts/tables/{table}/items/{id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|table|string|Ja|path|(Keine)|Eindeutiger Bezeichner eines Ordners des Typs „Kontakte“|
|id|string|Ja|path|(Keine)|Eindeutiger Bezeichner des zu löschenden Kontakts|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


## ContactPatchItem
Kontakt aktualisieren: Aktualisiert einen Kontakt teilweise.

```PATCH: /datasets/contacts/tables/{table}/items/{id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|table|string|Ja|path|(Keine)|Eindeutiger Bezeichner eines Ordners des Typs „Kontakte“|
|id|string|Ja|path|(Keine)|Eindeutiger Bezeichner des zu aktualisierenden Kontakts|
|item| |Ja|body|(Keine)|Zu aktualisierendes Kontaktelement|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


## Objektdefinitionen 

### TriggerBatchResponse[IDictionary[String,Object]]


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|value|array|Nein |



### Objekt


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|



### SendMessage


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Attachments|array|Nein |
|From|string|Nein |
|Cc|string|Nein |
|Bcc|string|Nein |
|Subject|string|Ja |
|Body|string|Ja |
|Importance|string|Nein |
|IsHtml|Boolescher Wert|Nein |
|To|string|Ja |



### SendAttachment


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|@odata.type|string|Nein |
|Name|string|Ja |
|ContentBytes|string|Ja |



### ReceiveMessage


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ID|string|Nein |
|IsRead|Boolescher Wert|Nein |
|HasAttachment|Boolescher Wert|Nein |
|DateTimeReceived|string|Nein |
|Anlagen|array|Nein |
|From|string|Nein |
|Cc|string|Nein |
|Bcc|string|Nein |
|Subject|string|Ja |
|Body|string|Ja |
|Importance|string|Nein |
|IsHtml|Boolescher Wert|Nein |
|To|string|Ja |



### ReceiveAttachment


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ID|string|Ja |
|ContentType|string|Ja |
|@odata.type|string|Nein |
|Name|string|Ja |
|ContentBytes|string|Ja |



### DigestMessage


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Subject|string|Ja |
|Body|string|Nein |
|Importance|string|Nein |
|Digest|array|Ja |
|Attachments|array|Nein |
|To|string|Ja |



### TriggerBatchResponse[ReceiveMessage]


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|value|array|Nein |



### DataSetsMetadata


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|tabular|nicht definiert|Nein |
|Blob|nicht definiert|Nein |



### TabularDataSetsMetadata


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|source|string|Nein |
|displayName|string|Nein |
|urlEncoding|string|Nein |
|tableDisplayName|string|Nein |
|tablePluralName|string|Nein |



### BlobDataSetsMetadata


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|source|string|Nein |
|displayName|string|Nein |
|urlEncoding|string|Nein |



### TableMetadata


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|name|string|Nein |
|title|string|Nein |
|x-ms-permission|string|Nein |
|x-ms-capabilities|nicht definiert|Nein |
|schema|nicht definiert|Nein |



### TableCapabilitiesMetadata


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|sortRestrictions|nicht definiert|Nein |
|filterRestrictions|nicht definiert|Nein |
|filterFunctions|array|Nein |



### TableSortRestrictionsMetadata


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|sortable|Boolescher Wert|Nein |
|unsortableProperties|array|Nein |
|ascendingOnlyProperties|array|Nein |



### TableFilterRestrictionsMetadata


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|filterable|Boolescher Wert|Nein |
|nonFilterableProperties|array|Nein |
|requiredProperties|array|Nein |



### OptionsEmailSubscription


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|NotificationUrl|string|Nein |
|Message|nicht definiert|Nein |



### MessageWithOptions


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Subject|string|Ja |
|Options|string|Ja |
|Body|string|Nein |
|Importance|string|Nein |
|Anlagen|array|Nein |
|To|string|Ja |



### SubscriptionResponse


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|id|string|Nein |
|resource|string|Nein |
|notificationType|string|Nein |
|notificationUrl|string|Nein |



### ApprovalEmailSubscription


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|NotificationUrl|string|Nein |
|Message|nicht definiert|Nein |



### ApprovalMessage


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Subject|string|Ja |
|Options|string|Ja |
|Body|string|Nein |
|Importance|string|Nein |
|Anlagen|array|Nein |
|To|string|Ja |



### ApprovalEmailResponse


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|SelectedOption|string|Nein |



### TablesList


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|value|array|Nein |



### Table


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Name|string|Nein |
|DisplayName|string|Nein |



### Item


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ItemInternalId|string|Nein |



### CalendarItemsList


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|value|array|Nein |



### CalendarItem


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ItemInternalId|string|Nein |



### ContactItemsList


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|value|array|Nein |



### ContactItem


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ItemInternalId|string|Nein |



### DataSetsList


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|value|array|Nein |



### DataSet


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Name|string|Nein |
|DisplayName|string|Nein |


## Nächste Schritte
[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md)

<!---HONumber=AcomDC_0727_2016-->