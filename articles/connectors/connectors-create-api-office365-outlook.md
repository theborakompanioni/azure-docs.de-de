---
title: "Hinzufügen des Office 365 Outlook-Connectors in Ihren Logik-Apps | Microsoft Docs"
description: "Erstellen Sie Logik-Apps mit Office 365-Connector, um die Interaktion mit Office 365 zu ermöglichen. Sie können beispielsweise Kontakte und Kalendereinträge erstellen, bearbeiten und aktualisieren."
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b2f6cc2c-bba2-493a-b0ba-841785462a80
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: mandia
translationtype: Human Translation
ms.sourcegitcommit: 66fc8f7e1da55dbe6bb1dd8b8d6a535c498c1cf7
ms.openlocfilehash: 92c0892128655141f29380890c31451e62ca8853


---
# <a name="get-started-with-the-office-365-outlook-connector"></a>Erste Schritte mit dem Office 365 Outlook-Connector
Der Office 365 Outlook-Connector ermöglicht die Interaktion mit Outlook in Office 365. Verwenden Sie diesen Connector zum Erstellen, Bearbeiten und Aktualisieren von Kontakten und Kalenderelementen sowie zum Empfangen, Senden und Beantworten von E-Mails.

Office 365 Outlook ermöglicht Folgendes:

* Erstellen Ihres Workflows unter Verwendung der E-Mail- und Kalenderfeatures in Office 365. 
* Verwenden von Triggern, um den Workflow zu starten, wenn beispielsweise eine neue E-Mail vorliegt oder ein Kalenderelement aktualisiert wird.
* Verwenden von Aktionen, um beispielsweise eine E-Mail zu senden oder ein neues Kalenderereignis zu erstellen. Wenn also beispielsweise ein neues Objekt in Salesforce vorhanden ist (Trigger), kann eine E-Mail an Office 365 Outlook gesendet werden (Aktion). 

Dieses Thema beschreibt, wie Sie den Office 365 Outlook-Connector in einer Logik-App verwenden, und enthält eine Liste mit den Triggern und Aktionen.

> [!NOTE]
> Diese Version des Artikels gilt für die allgemein verfügbare Version von Logic Apps.
> 
> 

Weitere Informationen zu Logik-Apps finden Sie unter [Was sind Logik-Apps](../logic-apps/logic-apps-what-are-logic-apps.md) sowie unter [Erstellen einer Logik-App zum Verbinden von SaaS-Diensten](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-to-office-365"></a>Herstellen einer Verbindung mit Office 365
Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, muss zunächst eine *Verbindung* mit dem Dienst hergestellt werden. Eine Verbindung stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her. Wenn Sie also etwa eine Verbindung mit Office 365 Outlook herstellen möchten, benötigen Sie zunächst eine *Verbindung* mit Office 365. Geben Sie zum Erstellen einer Verbindung die Anmeldeinformationen ein, mit denen Sie normalerweise auf den Dienst zugreifen, mit dem Sie eine Verbindung herstellen möchten. Geben Sie für Office 365 Outlook also die Anmeldeinformationen Ihres Office 365-Kontos ein, um die Verbindung zu erstellen.

## <a name="create-the-connection"></a>Erstellen der Verbindung
> [!INCLUDE [Steps to create a connection to Office 365](../../includes/connectors-create-api-office365-outlook.md)]
> 
> 

## <a name="use-a-trigger"></a>Verwenden eines Triggers
Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Trigger fragen den Dienst im gewünschten Intervall und mit der gewünschten Häufigkeit ab. Weitere Informationen zu Triggern finden Sie [hier](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. Geben Sie in der Logik-App die Zeichenfolge „office 365“ ein, um eine Liste mit den Triggern abzurufen:  
   
    ![](./media/connectors-create-api-office365-outlook/office365-trigger.png)
2. Wählen Sie **Office 365 Outlook - When an upcoming event is starting soon** (Office 365 Outlook – Wenn ein anstehendes Ereignis in Kürze beginnt) aus. Falls bereits eine Verbindung vorhanden ist, wählen Sie in der Dropdownliste einen Kalender aus.
   
    ![](./media/connectors-create-api-office365-outlook/sample-calendar.png)
   
    Wenn Sie zur Anmeldung aufgefordert werden, geben Sie die Anmeldeinformationen ein, um die Verbindung zu erstellen. Die erforderlichen Schritte sind in diesem Thema unter [Erstellen der Verbindung](connectors-create-api-office365-outlook.md#create-the-connection) aufgeführt. 
   
   > [!NOTE]
   > In diesem Beispiel wird die Logik-App ausgeführt, wenn ein Kalenderereignis aktualisiert wird. Fügen Sie zum Anzeigen der Ergebnisse dieses Triggers eine weitere Aktion hinzu, die Ihnen eine SMS sendet. Fügen Sie beispielsweise die Twilio-Aktion *Nachricht senden* hinzu, um eine SMS zu erhalten, wenn das Ereignis in 15 Minuten beginnt. 
   > 
   > 
3. Wählen Sie die Schaltfläche **Bearbeiten** aus, und legen Sie Werte für **Häufigkeit** und **Intervall** fest. Falls die Abfrage durch den Trigger also beispielsweise alle 15 Minuten erfolgen soll, legen Sie **Häufigkeit** auf **Minute** und **Intervall** auf **15** fest. 
   
    ![](./media/connectors-create-api-office365-outlook/calendar-settings.png)
4. Speichern Sie Ihre Änderungen. (Die Option **Speichern** befindet sich links oben auf der Symbolleiste.) Ihre Logik-App wird gespeichert und ggf. automatisch aktiviert.

## <a name="use-an-action"></a>Verwenden einer Aktion
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. Wählen Sie das Pluszeichen aus. Es stehen mehrere Auswahlmöglichkeiten zur Verfügung: **Aktion hinzufügen**, **Bedingung hinzufügen** oder eine der Optionen unter **Mehr**.
   
    ![](./media/connectors-create-api-office365-outlook/add-action.png)
2. Wählen Sie **Aktion hinzufügen**aus.
3. Geben Sie im Textfeld die Zeichenfolge „office 365“ ein, um eine Liste mit allen verfügbaren Aktionen zu erhalten.
   
    ![](./media/connectors-create-api-office365-outlook/office365-actions.png) 
4. In unserem Beispiel wählen wir **Office 365 Outlook – Kontakt erstellen** aus. Falls bereits eine Verbindung vorhanden ist, wählen Sie **Ordner-ID**, **Vorname** und andere Eigenschaften aus:  
   
    ![](./media/connectors-create-api-office365-outlook/office365-sampleaction.png)
   
    Wenn Sie zur Eingabe der Verbindungsinformationen aufgefordert werden, geben Sie die Details ein, um die Verbindung zu erstellen. Die Eigenschaften werden in diesem Thema unter [Erstellen der Verbindung](connectors-create-api-office365-outlook.md#create-the-connection) beschrieben. 
   
   > [!NOTE]
   > In diesem Beispiel erstellen wir einen neuen Kontakt in Office 365 Outlook. Für die Erstellung des Kontakts kann die Ausgabe eines anderen Triggers verwendet werden. Fügen Sie beispielsweise den Salesforce-Trigger *Wenn ein Objekt erstellt wird* hinzu. Fügen Sie dann die Office 365 Outlook-Aktion *Kontakt erstellen* hinzu, die den neuen Kontakt in Office 365 auf der Grundlage der Salesforce-Felder erstellt. 
   > 
   > 
5. Speichern Sie Ihre Änderungen. (Die Option **Speichern** befindet sich links oben auf der Symbolleiste.) Ihre Logik-App wird gespeichert und ggf. automatisch aktiviert.

## <a name="technical-details"></a>Technische Details
Im Anschluss finden Sie ausführliche Informationen zu den Triggern, Aktionen und Antworten, die von dieser Verbindung unterstützt werden:

## <a name="office-365-triggers"></a>Office 365-Trigger
| Trigger | Beschreibung |
| --- | --- |
| [When an upcoming event is starting soon](connectors-create-api-office365-outlook.md#when-an-upcoming-event-is-starting-soon) (Wenn ein anstehendes Ereignis in Kürze beginnt) |Dieser Vorgang löst einen Ablauf aus, wenn ein anstehendes Kalenderereignis gestartet wird. |
| [When a new email arrives](connectors-create-api-office365-outlook.md#when-a-new-email-arrives) (Wenn eine neue E-Mail eingeht) |Dieser Vorgang löst einen Ablauf aus, wenn eine neue E-Mail eingeht. |
| [When a new event is created](connectors-create-api-office365-outlook.md#when-a-new-event-is-created) (Wenn ein neues Ereignis erstellt wird) |Dieser Vorgang löst einen Ablauf aus, wenn in einem Kalender ein neues Ereignis erstellt wird. |
| [When an event is modified](connectors-create-api-office365-outlook.md#when-an-event-is-modified) (Wenn ein Ereignis geändert wird) |Dieser Vorgang löst einen Ablauf aus, wenn in einem Kalender ein Ereignis geändert wird. |

## <a name="office-365-actions"></a>Office 365-Aktionen
| Aktion | Beschreibung |
| --- | --- |
| [E-Mails abrufen](connectors-create-api-office365-outlook.md#get-emails) |Dieser Vorgang ruft E-Mails aus einem Ordner ab. |
| [E-Mail senden](connectors-create-api-office365-outlook.md#send-an-email) |Dieser Vorgang sendet eine E-Mail. |
| [E-Mail löschen](connectors-create-api-office365-outlook.md#delete-email) |Dieser Vorgang löscht eine E-Mail anhand der ID. |
| [Als gelesen markieren](connectors-create-api-office365-outlook.md#mark-as-read) |Dieser Vorgang markiert eine E-Mail als gelesen. |
| [E-Mail beantworten](connectors-create-api-office365-outlook.md#reply-to-email) |Dieser Vorgang beantwortet eine E-Mail. |
| [Anlagen abrufen](connectors-create-api-office365-outlook.md#get-attachment) |Dieser Vorgang ruft eine E-Mail-Anlage anhand der ID ab. |
| [E-Mails mit Optionen senden](connectors-create-api-office365-outlook.md#send-email-with-options) |Dieser Vorgang sendet eine E-Mail mit mehreren Optionen und wartet, bis der Empfänger mit einer der Optionen antwortet. |
| [Genehmigungs-E-Mail senden](connectors-create-api-office365-outlook.md#send-approval-email) |Dieser Vorgang sendet eine Genehmigungs-E-Mail und wartet auf eine Antwort des Empfängers. |
| [Kalender abrufen](connectors-create-api-office365-outlook.md#get-calendars) |Dieser Vorgang führt die verfügbaren Kalender auf. |
| [Ereignisse abrufen](connectors-create-api-office365-outlook.md#get-events) |Dieser Vorgang ruft Ereignisse aus einem Kalender ab. |
| [Ereignis erstellen](connectors-create-api-office365-outlook.md#create-event) |Dieser Vorgang erstellt ein neues Ereignis in einem Kalender. |
| [Ereignis abrufen](connectors-create-api-office365-outlook.md#get-event) |Dieser Vorgang ruft ein spezifisches Ereignis aus einem Kalender ab. |
| [Ereignis löschen](connectors-create-api-office365-outlook.md#delete-event) |Dieser Vorgang löscht ein Ereignis in einem Kalender. |
| [Ereignis aktualisieren](connectors-create-api-office365-outlook.md#update-event) |Dieser Vorgang aktualisiert ein Ereignis in einem Kalender. |
| [Ordner „Kontakte“ abrufen](connectors-create-api-office365-outlook.md#get-contact-folders) |Dieser Vorgang führt die verfügbaren Ordner vom Typ „Kontakte“ auf. |
| [Kontakte abrufen](connectors-create-api-office365-outlook.md#get-contacts) |Dieser Vorgang ruft Kontakte aus einem Ordner vom Typ „Kontakte“ ab. |
| [Kontakt erstellen](connectors-create-api-office365-outlook.md#create-contact) |Dieser Vorgang erstellt einen neuen Kontakt in einem Ordner vom Typ „Kontakte“. |
| [Kontakt abrufen](connectors-create-api-office365-outlook.md#get-contact) |Dieser Vorgang ruft einen spezifischen Kontakt aus einem Ordner vom Typ „Kontakte“ ab. |
| [Kontakt löschen](connectors-create-api-office365-outlook.md#delete-contact) |Dieser Vorgang löscht einen Kontakt aus einem Ordner vom Typ „Kontakte“. |
| [Kontakt aktualisieren](connectors-create-api-office365-outlook.md#update-contact) |Dieser Vorgang aktualisiert einen Kontakt in einem Ordner vom Typ „Kontakte“. |

### <a name="trigger-and-action-details"></a>Trigger- und Aktionsdetails
Dieser Abschnitt enthält spezifische Details zu den einzelnen Triggern und Aktionen. Hierzu zählen unter anderem erforderliche oder optionale Eingabeeigenschaften sowie entsprechende Ausgaben im Zusammenhang mit dem Connector.

#### <a name="when-an-upcoming-event-is-starting-soon"></a>When an upcoming event is starting soon (Wenn ein anstehendes Ereignis in Kürze beginnt)
Dieser Vorgang löst einen Ablauf aus, wenn ein anstehendes Kalenderereignis gestartet wird. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Kalender-ID |Eindeutiger Bezeichner des Kalenders |
| lookAheadTimeInMinutes |Lookahead-Zeit |Zeit (in Minuten) für Suche nach anstehenden Ereignissen. |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
CalendarItemsList: Die Liste der Kalenderelemente

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| value |array |Liste mit Kalenderelementen |

#### <a name="get-emails"></a>E-Mails abrufen
Dieser Vorgang ruft E-Mails aus einem Ordner ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| folderPath |Ordnerpfad |Pfad des Ordners zum Abrufen von E-Mails (Standard: Posteingang) |
| top |Top |Anzahl der abzurufenden E-Mails (Standardeinstellung: 10) |
| fetchOnlyUnread |Fetch Only Unread Messages (Nur ungelesene Nachrichten abrufen) |Nur ungelesene E-Mails abrufen? |
| includeAttachments |Anlagen einschließen |Falls auf „true“ festgelegt, werden Anlagen zusammen mit der E-Mail abgerufen. |
| searchQuery |Suchabfrage |Suchabfrage zum Filtern von E-Mails |
| skip |Überspringen |Anzahl der zu überspringenden E-Mails (Standardeinstellung: 0) |
| skipToken |Skip Token (Token überspringen) |Token überspringen, um neue Seite abzurufen |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
ReceiveMessage: E-Mail empfangen

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| Aus |string |Aus |
| To
 |string |To
 |
| Betreff |string |Betreff |
| body |string |body |
| Wichtigkeit |string |Wichtigkeit |
| HasAttachment |Boolescher Wert |Mit Anlage |
| ID |string |Nachrichten-ID |
| IsRead |Boolescher Wert |Gelesen |
| DateTimeReceived |string |Empfangsdatum/-uhrzeit |
| Anlagen |array |Anlagen |
| Cc |string |Geben Sie E-Mail-Adressen an, und trennen Sie sie jeweils durch ein Semikolon. (Beispiel: someone@contoso.com) |
| Bcc |string |Geben Sie E-Mail-Adressen an, und trennen Sie sie jeweils durch ein Semikolon. (Beispiel: someone@contoso.com) |
| IsHtml |Boolescher Wert |Ist HTML |

#### <a name="send-an-email"></a>Senden einer E-Mail
Dieser Vorgang sendet eine E-Mail. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| emailMessage* |E-Mail |E-Mail |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine

#### <a name="delete-email"></a>E-Mail löschen
Dieser Vorgang löscht eine E-Mail anhand der ID. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| messageId* |Nachrichten-ID |ID der zu löschenden E-Mail |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine

#### <a name="mark-as-read"></a>Als gelesen markieren
Dieser Vorgang markiert eine E-Mail als gelesen. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| messageId* |Nachrichten-ID |ID der E-Mail, die als gelesen markiert werden soll |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine

#### <a name="reply-to-email"></a>Reply to email (E-Mail beantworten)
Dieser Vorgang beantwortet eine E-Mail. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| messageId* |Nachrichten-ID |ID der zu beantwortenden E-Mail |
| comment* |Kommentar |Antwortkommentar |
| replyAll |Allen antworten |Allen Empfängern antworten |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine

#### <a name="get-attachment"></a>Anlagen abrufen
Dieser Vorgang ruft eine E-Mail-Anlage anhand der ID ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| messageId* |Nachrichten-ID |ID der E-Mail |
| attachmentId* |Anlagen-ID |ID der herunterzuladenden Anlage |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine

#### <a name="when-a-new-email-arrives"></a>When a new email arrives (Wenn eine neue E-Mail eingeht)
Dieser Vorgang löst einen Ablauf aus, wenn eine neue E-Mail eingeht.

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| folderPath |Ordnerpfad |Abzurufender E-Mail-Ordner (Standard: Posteingang) |
| to |To
 |E-Mail-Adressen der Empfänger |
| Aus |Aus |Absenderadresse |
| importance |Wichtigkeit |Wichtigkeit der E-Mail (High, Normal, Low) (Standard: Normal) |
| fetchOnlyWithAttachment |Has Attachments (Mit Anlagen) |Nur E-Mail-Nachrichten mit einer Anlage abrufen |
| includeAttachments |Anlagen einschließen |Anlagen einbeziehen |
| subjectFilter |Subject Filter (Betrefffilter) |Zeichenfolge, die in der Betreffzeile gesucht werden soll. |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
TriggerBatchResponse[ReceiveMessage]

| Eigenschaftenname | Datentyp |
| --- | --- |
| value |array |

#### <a name="send-email-with-options"></a>E-Mails mit Optionen senden
Dieser Vorgang sendet eine E-Mail mit mehreren Optionen und wartet, bis der Empfänger mit einer der Optionen antwortet. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| optionsEmailSubscription* |Abonnementanforderung für E-Mail-Optionen |Abonnementanforderung für E-Mail-Optionen |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
SubscriptionResponse: Modell für das Genehmigungs-E-Mail-Abonnement

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| id |string |ID des Abonnements |
| resource |string |Ressource der Abonnementanforderung |
| notificationType |string |Benachrichtigungstyp |
| notificationUrl |string |Benachrichtigungs-URL |

#### <a name="send-approval-email"></a>Genehmigungs-E-Mail senden
Dieser Vorgang sendet eine Genehmigungs-E-Mail und wartet auf eine Antwort des Empfängers. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| approvalEmailSubscription* |Abonnementanforderung für Genehmigungs-E-Mail. |Abonnementanforderung für Genehmigungs-E-Mail. |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
SubscriptionResponse: Modell für das Genehmigungs-E-Mail-Abonnement

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| id |string |ID des Abonnements |
| resource |string |Ressource der Abonnementanforderung |
| notificationType |string |Benachrichtigungstyp |
| notificationUrl |string |Benachrichtigungs-URL |

#### <a name="get-calendars"></a>Kalender abrufen
Dieser Vorgang führt die verfügbaren Kalender auf. 

Es gibt keine Parameter für diesen Aufruf.

##### <a name="output-details"></a>Ausgabedetails
TablesList

| Eigenschaftenname | Datentyp |
| --- | --- |
| value |array |

#### <a name="get-events"></a>Ereignisse abrufen
Dieser Vorgang ruft Ereignisse aus einem Kalender ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Kalender-ID |Wählen Sie einen Kalender aus. |
| $filter |Filterabfrage |Eine ODATA-Filterabfrage zum Einschränken der Anzahl zurückgegebener Einträge |
| $orderby |Sortieren nach |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |
| $skip |Auslassungsanzahl |Anzahl der zu überspringenden Einträge (Standardeinstellung = 0) |
| $top |Maximale Anzahl von Get-Anforderungen |Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256) |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
CalendarEventList: Die Liste mit den Kalenderelementen

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| value |array |Liste mit Kalenderelementen |

#### <a name="create-event"></a>Ereignis erstellen
Dieser Vorgang erstellt ein neues Ereignis in einem Kalender. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Kalender-ID |Wählen Sie einen Kalender aus. |
| item* |Element |Zu erstellendes Ereignis |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
CalendarEvent: Connector-spezifische Kalenderereignis-Modellklasse.

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| ID |string |Der eindeutige Bezeichner des Ereignisses. |
| Attendees |array |Liste mit den Teilnehmern für das Ereignis. |
| body |nicht definiert |Der Text der dem Ereignis zugeordneten Nachricht. |
| BodyPreview |string |Die Vorschau der dem Ereignis zugeordneten Nachricht. |
| Categories |array |Die dem Ereignis zugeordneten Kategorien. |
| ChangeKey |string |Identifiziert die Version des Ereignisobjekts. ChangeKey ändert sich bei jeder Änderung des Ereignisses. |
| DateTimeCreated |string |Datum und Uhrzeit der Ereigniserstellung. |
| DateTimeLastModified |string |Datum und Uhrzeit der letzten Änderung des Ereignisses. |
| End |string |Die Endzeit des Ereignisses. |
| EndTimeZone |string |Gibt die Zeitzone der Besprechungsendzeit an. Dieser Wert muss der Definition in Windows entsprechen. (Beispiel: Pacific Normalzeit) |
| HasAttachments |Boolescher Wert |„True“, wenn das Ereignis über Anlagen verfügt. |
| Priorität |string |Die Wichtigkeit des Ereignisses: gering, normal oder hoch. |
| IsAllDay |Boolescher Wert |„True“, wenn das Ereignis den ganzen Tag dauert. |
| IsCancelled |Boolescher Wert |„True“, wenn das Ereignis storniert wurde. |
| IsOrganizer |Boolescher Wert |„True“, wenn der Absender der Nachricht auch der Organisator ist. |
| Standort |nicht definiert |Der Ort des Ereignisses. |
| Organizer |nicht definiert |Der Organisator des Ereignisses. |
| Serie |nicht definiert |Das Serienmuster für das Ereignis. |
| Reminder |ganze Zahl |Zeit für die Erinnerung vor Beginn des Ereignisses (in Minuten). |
| ResponseRequested |Boolescher Wert |„True“, falls der Absender eine Antwort erhalten möchte, wenn das Ereignis angenommen oder abgelehnt wird. |
| ResponseStatus |nicht definiert |Gibt die Art der Antwort an, die als Reaktion auf eine Ereignisnachricht gesendet wurde. |
| SeriesMasterId |string |Eindeutiger Bezeichner für den Ereignistyp „Serienmaster“. |
| ShowAs |string |Zeigt den Verfügbarkeitsstatus (verfügbar oder beschäftigt) an. |
| Starten |string |Die Startzeit des Ereignisses. |
| StartTimeZone |string |Gibt die Zeitzone der Besprechungsstartzeit an. Dieser Wert muss der Definition in Windows entsprechen. (Beispiel: Pacific Normalzeit) |
| Betreff |string |Der Betreff des Ereignisses. |
| Typ |string |Der Ereignistyp: einfache Instanz, Vorkommen, Ausnahme oder Serienmaster. |
| WebLink |string |Die Vorschau der dem Ereignis zugeordneten Nachricht. |

#### <a name="get-event"></a>Ereignis abrufen
Dieser Vorgang ruft ein spezifisches Ereignis aus einem Kalender ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Kalender-ID |Wählen Sie einen Kalender aus. |
| id* |Element-ID |Wählen Sie ein Ereignis aus. |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
CalendarEvent: Connector-spezifische Kalenderereignis-Modellklasse.

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| ID |string |Der eindeutige Bezeichner des Ereignisses. |
| Attendees |array |Liste mit den Teilnehmern für das Ereignis. |
| body |nicht definiert |Der Text der dem Ereignis zugeordneten Nachricht. |
| BodyPreview |string |Die Vorschau der dem Ereignis zugeordneten Nachricht. |
| Categories |array |Die dem Ereignis zugeordneten Kategorien. |
| ChangeKey |string |Identifiziert die Version des Ereignisobjekts. ChangeKey ändert sich bei jeder Änderung des Ereignisses. |
| DateTimeCreated |string |Datum und Uhrzeit der Ereigniserstellung. |
| DateTimeLastModified |string |Datum und Uhrzeit der letzten Änderung des Ereignisses. |
| End |string |Die Endzeit des Ereignisses. |
| EndTimeZone |string |Gibt die Zeitzone der Besprechungsendzeit an. Dieser Wert muss der Definition in Windows entsprechen. (Beispiel: Pacific Normalzeit) |
| HasAttachments |Boolescher Wert |„True“, wenn das Ereignis über Anlagen verfügt. |
| Priorität |string |Die Wichtigkeit des Ereignisses: gering, normal oder hoch. |
| IsAllDay |Boolescher Wert |„True“, wenn das Ereignis den ganzen Tag dauert. |
| IsCancelled |Boolescher Wert |„True“, wenn das Ereignis storniert wurde. |
| IsOrganizer |Boolescher Wert |„True“, wenn der Absender der Nachricht auch der Organisator ist. |
| Standort |nicht definiert |Der Ort des Ereignisses. |
| Organizer |nicht definiert |Der Organisator des Ereignisses. |
| Serie |nicht definiert |Das Serienmuster für das Ereignis. |
| Reminder |ganze Zahl |Zeit für die Erinnerung vor Beginn des Ereignisses (in Minuten). |
| ResponseRequested |Boolescher Wert |„True“, falls der Absender eine Antwort erhalten möchte, wenn das Ereignis angenommen oder abgelehnt wird. |
| ResponseStatus |nicht definiert |Gibt die Art der Antwort an, die als Reaktion auf eine Ereignisnachricht gesendet wurde. |
| SeriesMasterId |string |Eindeutiger Bezeichner für den Ereignistyp „Serienmaster“. |
| ShowAs |string |Zeigt den Verfügbarkeitsstatus (verfügbar oder beschäftigt) an. |
| Starten |string |Die Startzeit des Ereignisses. |
| StartTimeZone |string |Gibt die Zeitzone der Besprechungsstartzeit an. Dieser Wert muss der Definition in Windows entsprechen. (Beispiel: Pacific Normalzeit) |
| Betreff |string |Der Betreff des Ereignisses. |
| Typ |string |Der Ereignistyp: einfache Instanz, Vorkommen, Ausnahme oder Serienmaster. |
| WebLink |string |Die Vorschau der dem Ereignis zugeordneten Nachricht. |

#### <a name="delete-event"></a>Ereignis löschen
Dieser Vorgang löscht ein Ereignis in einem Kalender. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Kalender-ID |Wählen Sie einen Kalender aus. |
| id* |ID |Wählen Sie ein Ereignis aus. |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine

#### <a name="update-event"></a>Ereignis aktualisieren
Dieser Vorgang aktualisiert ein Ereignis in einem Kalender. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Kalender-ID |Wählen Sie einen Kalender aus. |
| id* |ID |Wählen Sie ein Ereignis aus. |
| item* |Element |Zu aktualisierendes Ereignis |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
CalendarEvent: Connector-spezifische Kalenderereignis-Modellklasse.

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| ID |string |Der eindeutige Bezeichner des Ereignisses. |
| Attendees |array |Liste mit den Teilnehmern für das Ereignis. |
| body |nicht definiert |Der Text der dem Ereignis zugeordneten Nachricht. |
| BodyPreview |string |Die Vorschau der dem Ereignis zugeordneten Nachricht. |
| Categories |array |Die dem Ereignis zugeordneten Kategorien. |
| ChangeKey |string |Identifiziert die Version des Ereignisobjekts. ChangeKey ändert sich bei jeder Änderung des Ereignisses. |
| DateTimeCreated |string |Datum und Uhrzeit der Ereigniserstellung. |
| DateTimeLastModified |string |Datum und Uhrzeit der letzten Änderung des Ereignisses. |
| End |string |Die Endzeit des Ereignisses. |
| EndTimeZone |string |Gibt die Zeitzone der Besprechungsendzeit an. Dieser Wert muss der Definition in Windows entsprechen. (Beispiel: Pacific Normalzeit) |
| HasAttachments |Boolescher Wert |„True“, wenn das Ereignis über Anlagen verfügt. |
| Priorität |string |Die Wichtigkeit des Ereignisses: gering, normal oder hoch. |
| IsAllDay |Boolescher Wert |„True“, wenn das Ereignis den ganzen Tag dauert. |
| IsCancelled |Boolescher Wert |„True“, wenn das Ereignis storniert wurde. |
| IsOrganizer |Boolescher Wert |„True“, wenn der Absender der Nachricht auch der Organisator ist. |
| Standort |nicht definiert |Der Ort des Ereignisses. |
| Organizer |nicht definiert |Der Organisator des Ereignisses. |
| Serie |nicht definiert |Das Serienmuster für das Ereignis. |
| Reminder |ganze Zahl |Zeit für die Erinnerung vor Beginn des Ereignisses (in Minuten). |
| ResponseRequested |Boolescher Wert |„True“, falls der Absender eine Antwort erhalten möchte, wenn das Ereignis angenommen oder abgelehnt wird. |
| ResponseStatus |nicht definiert |Gibt die Art der Antwort an, die als Reaktion auf eine Ereignisnachricht gesendet wurde. |
| SeriesMasterId |string |Eindeutiger Bezeichner für den Ereignistyp „Serienmaster“. |
| ShowAs |string |Zeigt den Verfügbarkeitsstatus (verfügbar oder beschäftigt) an. |
| Starten |string |Die Startzeit des Ereignisses. |
| StartTimeZone |string |Gibt die Zeitzone der Besprechungsstartzeit an. Dieser Wert muss der Definition in Windows entsprechen. (Beispiel: Pacific Normalzeit) |
| Betreff |string |Der Betreff des Ereignisses. |
| Typ |string |Der Ereignistyp: einfache Instanz, Vorkommen, Ausnahme oder Serienmaster. |
| WebLink |string |Die Vorschau der dem Ereignis zugeordneten Nachricht. |

#### <a name="when-a-new-event-is-created"></a>When a new event is created (Wenn ein neues Ereignis erstellt wird)
Dieser Vorgang löst einen Ablauf aus, wenn in einem Kalender ein neues Ereignis erstellt wird. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Kalender-ID |Wählen Sie einen Kalender aus. |
| $filter |Filterabfrage |Eine ODATA-Filterabfrage zum Einschränken der Anzahl zurückgegebener Einträge |
| $orderby |Sortieren nach |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |
| $skip |Auslassungsanzahl |Anzahl der zu überspringenden Einträge (Standardeinstellung = 0) |
| $top |Maximale Anzahl von Get-Anforderungen |Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256) |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
CalendarItemsList: Die Liste der Kalenderelemente

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| value |array |Liste mit Kalenderelementen |

#### <a name="when-an-event-is-modified"></a>When an event is modified (Wenn ein Ereignis geändert wird)
Dieser Vorgang löst einen Ablauf aus, wenn in einem Kalender ein Ereignis geändert wird. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Kalender-ID |Wählen Sie einen Kalender aus. |
| $filter |Filterabfrage |Eine ODATA-Filterabfrage zum Einschränken der Anzahl zurückgegebener Einträge |
| $orderby |Sortieren nach |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |
| $skip |Auslassungsanzahl |Anzahl der zu überspringenden Einträge (Standardeinstellung = 0) |
| $top |Maximale Anzahl von Get-Anforderungen |Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256) |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
CalendarItemsList: Die Liste der Kalenderelemente

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| value |array |Liste mit Kalenderelementen |

#### <a name="get-contact-folders"></a>Ordner „Kontakte“ abrufen
Dieser Vorgang führt die verfügbaren Ordner vom Typ „Kontakte“ auf. 

Es gibt keine Parameter für diesen Aufruf.

##### <a name="output-details"></a>Ausgabedetails
TablesList

| Eigenschaftenname | Datentyp |
| --- | --- |
| value |array |

#### <a name="get-contacts"></a>Kontakte abrufen
Dieser Vorgang ruft Kontakte aus einem Ordner vom Typ „Kontakte“ ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Ordner-ID |Eindeutiger Bezeichner des abzurufenden Ordners „Kontakte“ |
| $filter |Filterabfrage |Eine ODATA-Filterabfrage zum Einschränken der Anzahl zurückgegebener Einträge |
| $orderby |Sortieren nach |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |
| $skip |Auslassungsanzahl |Anzahl der zu überspringenden Einträge (Standardeinstellung = 0) |
| $top |Maximale Anzahl von Get-Anforderungen |Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256) |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
ContactList: Die Liste mit den Kontakten

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| value |array |Kontaktliste |

#### <a name="create-contact"></a>Kontakt erstellen
Dieser Vorgang erstellt einen neuen Kontakt in einem Ordner vom Typ „Kontakte“. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Ordner-ID |Wählen Sie einen Ordner für Kontakte aus. |
| item* |Element |Zu erstellender Kontakt |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Contact: Kontakt

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| ID |string |Der eindeutige Bezeichner des Kontakts. |
| ParentFolderId |string |Die ID des übergeordneten Ordners des Kontakts. |
| Birthday |string |Das Geburtsdatum des Kontakts. |
| FileAs |string |Der Name, unter dem der Kontakt gespeichert ist. |
| displayName |string |Der Anzeigename des Kontakts. |
| GivenName |string |Der Vorname des Kontakts. |
| Initials |string |Die Initialen des Kontakts. |
| MiddleName |string |Der zweite Vorname des Kontakts. |
| NickName |string |Der Spitzname des Kontakts. |
| Surname |string |Der Nachname des Kontakts. |
| Titel |string |Der Titel des Kontakts. |
| Generation |string |Die Generation des Kontakts. |
| EmailAddresses |array |Die E-Mail-Adressen des Kontakts. |
| ImAddresses |array |Die Sofortnachrichtenadressen des Kontakts. |
| JobTitle |string |Die Position des Kontakts. |
| CompanyName |string |Der Name des Unternehmens des Kontakts. |
| Department |string |Die Abteilung des Kontakts. |
| OfficeLocation |string |Der Standort der Niederlassung des Kontakts. |
| Profession |string |Der Beruf des Kontakts. |
| BusinessHomePage |string |Die geschäftliche Homepage des Kontakts. |
| AssistantName |string |Der Name des Assistenten des Kontakts. |
| Manager |string |Der Name des Vorgesetzten des Kontakts. |
| HomePhones |array |Die privaten Telefonnummern des Kontakts. |
| BusinessPhones |array |Die geschäftlichen Telefonnummern des Kontakts. |
| MobilePhone1 |string |Die Mobiltelefonnummer des Kontakts. |
| HomeAddress |nicht definiert |Die Privatadresse des Kontakts. |
| BusinessAddress |nicht definiert |Die Geschäftsadresse des Kontakts. |
| OtherAddress |nicht definiert |Sonstige Adressen für den Kontakt. |
| YomiCompanyName |string |Der phonetische japanische Unternehmensname des Kontakts. |
| YomiGivenName |string |Der phonetische japanische Vorname des Kontakts. |
| YomiSurname |string |Der phonetische japanische Nachname des Kontakts. |
| Categories |array |Die dem Kontakt zugeordneten Kategorien. |
| ChangeKey |string |Identifiziert die Version des Ereignisobjekts. |
| DateTimeCreated |string |Der Zeitpunkt der Kontakterstellung. |
| DateTimeLastModified |string |Der Zeitpunkt der Kontaktänderung. |

#### <a name="get-contact"></a>Kontakt abrufen
Dieser Vorgang ruft einen spezifischen Kontakt aus einem Ordner vom Typ „Kontakte“ ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Ordner-ID |Wählen Sie einen Ordner für Kontakte aus. |
| id* |Element-ID |Eindeutiger Bezeichner des abzurufenden Kontakts |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Contact: Kontakt

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| ID |string |Der eindeutige Bezeichner des Kontakts. |
| ParentFolderId |string |Die ID des übergeordneten Ordners des Kontakts. |
| Birthday |string |Das Geburtsdatum des Kontakts. |
| FileAs |string |Der Name, unter dem der Kontakt gespeichert ist. |
| displayName |string |Der Anzeigename des Kontakts. |
| GivenName |string |Der Vorname des Kontakts. |
| Initials |string |Die Initialen des Kontakts. |
| MiddleName |string |Der zweite Vorname des Kontakts. |
| NickName |string |Der Spitzname des Kontakts. |
| Surname |string |Der Nachname des Kontakts. |
| Titel |string |Der Titel des Kontakts. |
| Generation |string |Die Generation des Kontakts. |
| EmailAddresses |array |Die E-Mail-Adressen des Kontakts. |
| ImAddresses |array |Die Sofortnachrichtenadressen des Kontakts. |
| JobTitle |string |Die Position des Kontakts. |
| CompanyName |string |Der Name des Unternehmens des Kontakts. |
| Department |string |Die Abteilung des Kontakts. |
| OfficeLocation |string |Der Standort der Niederlassung des Kontakts. |
| Profession |string |Der Beruf des Kontakts. |
| BusinessHomePage |string |Die geschäftliche Homepage des Kontakts. |
| AssistantName |string |Der Name des Assistenten des Kontakts. |
| Manager |string |Der Name des Vorgesetzten des Kontakts. |
| HomePhones |array |Die privaten Telefonnummern des Kontakts. |
| BusinessPhones |array |Die geschäftlichen Telefonnummern des Kontakts. |
| MobilePhone1 |string |Die Mobiltelefonnummer des Kontakts. |
| HomeAddress |nicht definiert |Die Privatadresse des Kontakts. |
| BusinessAddress |nicht definiert |Die Geschäftsadresse des Kontakts. |
| OtherAddress |nicht definiert |Sonstige Adressen für den Kontakt. |
| YomiCompanyName |string |Der phonetische japanische Unternehmensname des Kontakts. |
| YomiGivenName |string |Der phonetische japanische Vorname des Kontakts. |
| YomiSurname |string |Der phonetische japanische Nachname des Kontakts. |
| Categories |array |Die dem Kontakt zugeordneten Kategorien. |
| ChangeKey |string |Identifiziert die Version des Ereignisobjekts. |
| DateTimeCreated |string |Der Zeitpunkt der Kontakterstellung. |
| DateTimeLastModified |string |Der Zeitpunkt der Kontaktänderung. |

#### <a name="delete-contact"></a>Kontakt löschen
Dieser Vorgang löscht einen Kontakt aus einem Ordner vom Typ „Kontakte“. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Ordner-ID |Wählen Sie einen Ordner für Kontakte aus. |
| id* |ID |Eindeutiger Bezeichner des zu löschenden Kontakts |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine

#### <a name="update-contact"></a>Kontakt aktualisieren
Dieser Vorgang aktualisiert einen Kontakt in einem Ordner vom Typ „Kontakte“. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Ordner-ID |Wählen Sie einen Ordner für Kontakte aus. |
| id* |ID |Eindeutiger Bezeichner des zu aktualisierenden Kontakts |
| item* |Element |Zu aktualisierendes Kontaktelement |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Contact: Kontakt

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| ID |string |Der eindeutige Bezeichner des Kontakts. |
| ParentFolderId |string |Die ID des übergeordneten Ordners des Kontakts. |
| Birthday |string |Das Geburtsdatum des Kontakts. |
| FileAs |string |Der Name, unter dem der Kontakt gespeichert ist. |
| displayName |string |Der Anzeigename des Kontakts. |
| GivenName |string |Der Vorname des Kontakts. |
| Initials |string |Die Initialen des Kontakts. |
| MiddleName |string |Der zweite Vorname des Kontakts. |
| NickName |string |Der Spitzname des Kontakts. |
| Surname |string |Der Nachname des Kontakts. |
| Titel |string |Der Titel des Kontakts. |
| Generation |string |Die Generation des Kontakts. |
| EmailAddresses |array |Die E-Mail-Adressen des Kontakts. |
| ImAddresses |array |Die Sofortnachrichtenadressen des Kontakts. |
| JobTitle |string |Die Position des Kontakts. |
| CompanyName |string |Der Name des Unternehmens des Kontakts. |
| Department |string |Die Abteilung des Kontakts. |
| OfficeLocation |string |Der Standort der Niederlassung des Kontakts. |
| Profession |string |Der Beruf des Kontakts. |
| BusinessHomePage |string |Die geschäftliche Homepage des Kontakts. |
| AssistantName |string |Der Name des Assistenten des Kontakts. |
| Manager |string |Der Name des Vorgesetzten des Kontakts. |
| HomePhones |array |Die privaten Telefonnummern des Kontakts. |
| BusinessPhones |array |Die geschäftlichen Telefonnummern des Kontakts. |
| MobilePhone1 |string |Die Mobiltelefonnummer des Kontakts. |
| HomeAddress |nicht definiert |Die Privatadresse des Kontakts. |
| BusinessAddress |nicht definiert |Die Geschäftsadresse des Kontakts. |
| OtherAddress |nicht definiert |Sonstige Adressen für den Kontakt. |
| YomiCompanyName |string |Der phonetische japanische Unternehmensname des Kontakts. |
| YomiGivenName |string |Der phonetische japanische Vorname des Kontakts. |
| YomiSurname |string |Der phonetische japanische Nachname des Kontakts. |
| Categories |array |Die dem Kontakt zugeordneten Kategorien. |
| ChangeKey |string |Identifiziert die Version des Ereignisobjekts. |
| DateTimeCreated |string |Der Zeitpunkt der Kontakterstellung. |
| DateTimeLastModified |string |Der Zeitpunkt der Kontaktänderung. |

## <a name="http-responses"></a>HTTP-Antworten
Von den oben angegebenen Aktionen und Triggern können folgende HTTP-Statuscodes zurückgegeben werden: 

| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 202 |Zulässig |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler ist aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen Sie eine Logik-App](../logic-apps/logic-apps-create-a-logic-app.md). Informieren Sie sich in unserer [API-Liste](apis-list.md)über die anderen verfügbaren Connectors für Logik-Apps.




<!--HONumber=Feb17_HO2-->


