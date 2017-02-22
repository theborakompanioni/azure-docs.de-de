---
title: Trello | Microsoft Docs
description: "Erstellen Sie Logik-Apps mit Azure App Service. Trello bietet Ihnen eine Übersicht über all Ihre beruflichen und privaten Projekte.  Dies ist eine einfache, kostenlose, flexible und visuelle Möglichkeit, um Ihre Projekte zu verwalten und alles zu organisieren.  Stellen Sie eine Verbindung mit Trello her, um Ihre Boards, Listen und Karten zu verwalten."
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: fe7a4377-5c24-4f72-ab1a-6d9d23e8d895
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: b92f954680603891ced503a1134791312b5214f0
ms.openlocfilehash: 2616db980cb37c1c3759096ec3c05b98d687e047


---
# <a name="get-started-with-the-trello-connector"></a>Erste Schritte mit dem Trello-Connector
Trello bietet Ihnen eine Übersicht über all Ihre beruflichen und privaten Projekte.  Dies ist eine einfache, kostenlose, flexible und visuelle Möglichkeit, um Ihre Projekte zu verwalten und alles zu organisieren.  Stellen Sie eine Verbindung mit Trello her, um Ihre Boards, Listen und Karten zu verwalten.

> [!NOTE]
> Diese Version des Artikels gilt für Logik-Apps mit der Schemaversion „2015-08-01-preview“.
> 
> 

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md) beschrieben.

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Der Trello-Connector kann als Aktion verwendet werden. Er verfügt über Trigger. Alle Connectors unterstützen Daten im JSON- und XML-Format.

 Der Trello-Connector verfügt über die folgenden Aktionen und/oder Trigger:

### <a name="trello-actions"></a>Trello-Aktionen
Sie können diese Aktionen ausführen:

| Aktion | Beschreibung |
| --- | --- |
| [ListCards](connectors-create-api-trello.md#listcards) |Auflisten der Karten im Board |
| [GetCard](connectors-create-api-trello.md#getcard) |Abrufen von Karten nach ID |
| [UpdateCard](connectors-create-api-trello.md#updatecard) |Aktualisieren von Karten |
| [DeleteCard](connectors-create-api-trello.md#deletecard) |Löschen von Karten |
| [CreateCard](connectors-create-api-trello.md#createcard) |Erstellen neuer Karten in Ihrem Trello-Konto |
| [ListBoards](connectors-create-api-trello.md#listboards) |Auflisten von Boards |
| [GetBoard](connectors-create-api-trello.md#getboard) |Abrufen von Boards nach ID |
| [ListLists](connectors-create-api-trello.md#listlists) |Auflisten von Kartenlisten im Board |
| [GetList](connectors-create-api-trello.md#getlist) |Abrufen von Listen nach ID |

### <a name="trello-triggers"></a>Trello-Trigger
Sie können auf diese Ereignisse lauschen:

| Trigger | Beschreibung |
| --- | --- |
| Wenn einem Board eine neue Karte hinzugefügt wird |Löst einen Ablauf aus, wenn einem Board eine neue Karte hinzugefügt wird |
| Wenn einer Liste eine neue Karte hinzugefügt wird |Löst einen Ablauf aus, wenn einer Liste eine neue Karte hinzugefügt wird |

## <a name="create-a-connection-to-trello"></a>Herstellen einer Verbindung mit Trello
Um Logik-Apps mit Trello zu erstellen, müssen Sie zuerst eine **Verbindung** erstellen und anschließend die Details für die folgenden Eigenschaften angeben:

| Eigenschaft | Erforderlich | Beschreibung |
| --- | --- | --- |
| Tokenverschlüsselung |Ja |Angeben von Anmeldeinformationen für Trello |

Nachdem Sie die Verbindung erstellt haben, können Sie sie zum Ausführen der Aktionen und zum Lauschen auf die in diesem Artikel beschriebenen Trigger verwenden.

> [!INCLUDE [Steps to create a connection to Trello](../../includes/connectors-create-api-trello.md)]
> 
> [!TIP]
> Sie können diese Verbindung in anderen Logik-Apps verwenden.
> 
> 

## <a name="reference-for-trello"></a>Referenz für Trello
Gilt für Version 1.0.

## <a name="onnewcardinboard"></a>OnNewCardInBoard
Wenn einem Board eine neue Karte hinzugefügt wird: Löst einen Ablauf aus, wenn einem Board eine neue Karte hinzugefügt wird.

```GET: /trigger/boards/{board_id}/cards```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| board_id |string |Ja |path |(Keine) |Eindeutige ID des Boards, das mit Karten gefüllt wird |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang |

## <a name="onnewcardinlist"></a>OnNewCardInList
Wenn einer Liste eine neue Karte hinzugefügt wird: Löst einen Ablauf aus, wenn einer Liste eine neue Karte hinzugefügt wird.

```GET: /trigger/lists/{list_id}/cards```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| board_id |string |Ja |query |(Keine) |Eindeutige ID des Boards, das mit Karten gefüllt wird |
| list_id |string |Ja |path |(Keine) |Eindeutige ID der Liste, die mit Karten gefüllt wird |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang |

## <a name="listcards"></a>ListCards
Auflisten von Karten im Board: Auflisten von Karten im Board

```GET: /boards/{board_id}/cards```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| board_id |string |Ja |path |(Keine) |ID des Boards zum Abrufen aller Karten |
| actions |string |no |query |(Keine) |Auflisten der zurückzugebenden Aktionen. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| attachments |Boolescher Wert |no |query |(Keine) |Anhänge anzeigen |
| attachment_fields |string |no |query |(Keine) |Auflisten der zurückzugebenden Anhangfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| stickers |Boolescher Wert |no |query |(Keine) |Anzeigen von Stickern |
| members |Boolescher Wert |no |query |(Keine) |Anzeigen von Mitgliedern |
| memeber_fields |string |no |query |(Keine) |Anzeigen der zurückzugebenden Mitgliederfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| CheckItemStates |Boolescher Wert |no |query |(Keine) |Zurückgeben des Kartenstatus. |
| Checklists |string |no |query |(Keine) |Anzeigen von Checklisten |
| limit |integer |no |query |(Keine) |Die maximale Anzahl der zurückzugebenden Ergebnisse, zwischen 1 und 1000 |
| since |string |no |query |(Keine) |Abrufen aller Karten nach diesem Datum |
| Vorbereitung |string |no |query |(Keine) |Abrufen aller Karten vor diesem Datum |
| filter |string |no |query |(Keine) |Filtern der Antwort |
| fields |string |no |query |(Keine) |Anzeigen der zurückzugebenden Kartenfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang |

## <a name="getcard"></a>GetCard
Abrufen von Karten nach ID: Abrufen von Karten nach ID

```GET: /cards/{card_id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| board_id |string |Ja |query |(Keine) |ID des Boards, das mit Karten gefüllt wird |
| card_id |string |Ja |path |(Keine) |ID der abzurufenden Karte |
| actions |string |no |query |(Keine) |Auflisten der zurückzugebenden Aktionen. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| actions_entities |Boolescher Wert |no |query |(Keine) |Zurückgeben von Aktionsentitäten |
| actions_display |Boolescher Wert |no |query |(Keine) |Zurückgeben von Aktionsanzeigen |
| actions_limit |integer |no |query |(Keine) |Max. Anzahl zurückzugebender Aktionen |
| action_fields |string |no |query |(Keine) |Liste der Aktionsfelder, die für jede Aktion zurückzugeben sind. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| action_memberCreator_fields |string |no |query |(Keine) |Liste zurückzugebenden Felder für Aktionsmitgliederersteller. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| attachments |Boolescher Wert |no |query |(Keine) |Zurückgeben von Anhängen |
| attachement_fields |string |no |query |(Keine) |Liste von Anhangfeldern, die für jeden Anhang zurückgegeben werden. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| members |Boolescher Wert |no |query |(Keine) |Zurückgeben von Mitgliedern |
| member_fields |string |no |query |(Keine) |Liste der Mitgliedsfelder, die für jedes Mitglied zurückgegeben werden. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| membersVoted |Boolescher Wert |no |query |(Keine) |Zurückgeben von Mitgliedern, die abgestimmt haben |
| memberVoted_fields |string |no |query |(Keine) |Liste der Felder von Mitglieder, die abgestimmt haben, die für jedes Mitglied zurückgegeben werden. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| checkItemStates |Boolescher Wert |no |query |(Keine) |Zurückgeben des Kartenstatus |
| checkItemState_fields |string |no |query |(Keine) |Liste der Statusfelder, die für jeden Kartenelementstatus zurückgegeben werden. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| checklists |string |no |query |(Keine) |Zurückgeben von Checklisten |
| checklist_fields |string |no |query |(Keine) |Liste der Checklistenfelder, die für jede Checkliste zurückgegeben werden. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| board |Boolescher Wert |no |query |(Keine) |Zurückgeben des Boards, zu dem die Karte gehört |
| board_fields |string |no |query |(Keine) |Auflisten der zurückzugebenden Boardfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| list |Boolescher Wert |no |query |(Keine) |Zurückgeben der Liste, zu der die Karte gehört |
| list_fields |string |no |query |(Keine) |Auflisten der zurückzugebenden Listenfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| stickers |Boolescher Wert |no |query |(Keine) |Zurückgeben von Stickern |
| sticker_fields |string |no |query |(Keine) |Auflisten der Stickerfelder, die für jeden Sticker zurückgegeben werden. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| fields |string |no |query |(Keine) |Anzeigen der zurückzugebenden Kartenfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang |

## <a name="updatecard"></a>UpdateCard
Aktualisieren von Karten: Aktualisieren von Karten

```PUT: /cards/{card_id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| board_id |string |Ja |query |(Keine) |ID des Boards, von dem die Karten abgerufen werden |
| card_id |string |Ja |path |(Keine) |ID der zu aktualisierenden Karte |
| updateCard | |Ja |body |(Keine) |Aktualisierte Kartenwerte |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang |

## <a name="deletecard"></a>DeleteCard
Löschen von Karten: Löschen von Karten

```DELETE: /cards/{card_id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| board_id |string |Ja |query |(Keine) |ID des Boards, von dem die Karten abgerufen werden |
| card_id |string |Ja |path |(Keine) |ID der zu löschenden Karte |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang |

## <a name="createcard"></a>CreateCard
Erstellen von Karten: Erstellt eine neue Karten in Ihrem Trello-Konto

```POST: /cards```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| board_id |string |Ja |query |(Keine) |Eindeutige ID des Boards, in dem Karten erstellt werden |
| newCard | |Ja |body |(Keine) |Neue Karte, die dem Trello-Board hinzugefügt wird |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang |

## <a name="listboards"></a>ListBoards
Auflisten von Boards: Auflisten von Boards

```GET: /member/me/boards```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| filter |string |no |query |(Keine) |Auflisten von Filtern, die auf Board-Ergebnisse angewandt werden. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| fields |string |no |query |(Keine) |Auflisten der zurückzugebenden Boardfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| actions |string |no |query |(Keine) |Auflisten der zurückzugebenden Aktionsfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| actions_entities |Boolescher Wert |no |query |(Keine) |Zurückgeben von Aktionsentitäten |
| actions_limit |integer |no |query |(Keine) |Max. Anzahl zurückzugebender Aktionen |
| actions_format |string |no |query |(Keine) |Festlegen des Formats der zurückzugebenden Aktionen |
| actions_since |string |no |query |(Keine) |Zurückgeben von Aktionen nach dem angegebenen Datum |
| action_fields |string |no |query |(Keine) |Auflisten der Felder der zurückzugebenden Aktion. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| memberships |string |no |query |(Keine) |Festlegen der zurückzugebenden Mitgliedschaftsinformationen. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| organization |Boolescher Wert |no |query |(Keine) |Festlegen der zurückzugebenden Organisationsinformationen. |
| organization_fields |string |no |query |(Keine) |Auflisten der zurückzugebenden Organisationsfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| lists |string |no |query |(Keine) |Festlegen, ob zum Board gehörige Listen zurückgegeben werden |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang |

## <a name="getboard"></a>GetBoard
Abrufen von Boards nach ID: Abrufen von Boards nach ID

```GET: /boards/{board_id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| board_id |string |Ja |path |(Keine) |Eindeutige ID des abzurufenden Boards |
| actions |string |no |query |(Keine) |Auflisten der zurückzugebenden Aktionen. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| action_entities |Boolescher Wert |no |query |(Keine) |Festlegen, ob Aktionsentitäten zurückzugeben sind |
| actions_display |Boolescher Wert |no |query |(Keine) |Festlegen, ob Aktionsanzeigen zurückzugeben sind |
| actions_format |string |no |query |(Keine) |Festlegen des Formats der zurückzugebenden Aktionen |
| actions_since |string |no |query |(Keine) |Nur Aktionen nach diesem Datum zurückgeben |
| actions_limit |integer |no |query |(Keine) |Max. Anzahl zurückzugebender Aktionen |
| action_fields |string |no |query |(Keine) |Auflisten der mit jedem Feld zurückzugebenden Felder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| action_memeber |Boolescher Wert |no |query |(Keine) |Festlegen, ob Aktionsmitglieder zurückzugeben sind |
| action_member_fields |string |no |query |(Keine) |Auflisten der mit jedem Aktionsmitglied zurückzugebenden Mitgliedsfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| action_memberCreator |Boolescher Wert |no |query |(Keine) |Festlegen, ob Aktionsmitgliederersteller zurückzugeben sind |
| action_memberCreator_fields |string |no |query |(Keine) |Auflisten der zurückzugebenden Felder für Aktionsmitgliederersteller. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| cards |string |no |query |(Keine) |Festlegen der zurückzugebenden Karten |
| card_fields |string |no |query |(Keine) |Auflisten der für jede Karte zurückzugebenden Felder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| card_attachments |Boolescher Wert |Ja |query |(Keine) |Festlegen, ob Anhänge zu Karten zurückzugeben sind |
| card_attachment_fields |string |no |query |(Keine) |Auflisten der für jeden Anhang zurückzugebenden Anhangfelder Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| card_checklists |string |no |query |(Keine) |Festlegen der für jede Karte zurückzugebenden Checklisten |
| card_stickers |Boolescher Wert |no |query |(Keine) |Festlegen, ob Kartensticker zurückzugeben sind |
| boardStarts |string |no |query |(Keine) |Festlegen der zurückzugebenden Board-Sterne |
| Bezeichnungen |string |no |query |(Keine) |Festlegen der zurückzugebenden Labels |
| label_fields |string |no |query |(Keine) |Auflisten der für jedes Label zurückzugebenden Labelfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| labels_limits |integer |no |query |(Keine) |Max. Anzahl zurückzugebender Labels |
| lists |string |no |query |(Keine) |Festlegen der zurückzugebenden Listen |
| list_fields |string |no |query |(Keine) |Auflisten der für jede Liste zurückzugebenden Listenfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| memberships |string |no |query |(Keine) |Auflisten der zurückzugebenden Mitgliedschaften. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| memberships_member |Boolescher Wert |no |query |(Keine) |Festlegen, ob Mitgliedschaftsmitglieder zurückzugeben sind |
| memberships_member_fields |string |no |query |(Keine) |Auflisten der Mitgliedsfelder, die für jedes Mitgliedschaftsmitglied zurückgegeben werden. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| members |string |no |query |(Keine) |Auflisten der zurückzugebenden Mitglieder |
| member_fields |string |no |query |(Keine) |Auflisten der Mitgliedsfelder, die für jedes Mitglied zurückgegeben werden. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| membersInvited |string |no |query |(Keine) |Festlegen der zurückzugebenden eingeladenen Mitglieder |
| membersInvited_fields |string |no |query |(Keine) |Auflisten der jeweils zurückzugebenden Felder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| checklists |string |no |query |(Keine) |Festlegen der zurückzugebenden Checklisten |
| checklist_fields |string |no |query |(Keine) |Auflisten der Checklistenfelder, die für jede Checkliste zurückgegeben werden. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| organization |Boolescher Wert |no |query |(Keine) |Festlegen, ob Organisationsinformationen zurückgegeben werden |
| organization_fields |string |no |query |(Keine) |Auflisten der für jede Organisation zurückzugebenden Organisationsfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| organization_memberships |string |no |query |(Keine) |Auflisten der zurückzugebenden Organisationsmitglieder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| myPerfs |Boolescher Wert |no |query |(Keine) |Festlegen, ob eigene Perfs zurückzugeben sind |
| fields |string |no |query |(Keine) |Auflisten der zurückzugebenden Felder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang |

## <a name="listlists"></a>ListLists
Auflisten von Kartenlisten im Board: Auflisten von Kartenlisten im Board

```GET: /boards/{board_id}/lists```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| board_id |string |Ja |path |(Keine) |Eindeutige ID des Boards zum Abrufen von Listen |
| cards |string |no |query |(Keine) |Festlegen der zurückzugebenden Karten |
| card_fields |string |no |query |(Keine) |Auflisten der Kartenfelder für die Rückgabe. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| filter |string |no |query |(Keine) |Festlegen der Filtereigenschaft für Listen |
| fields |string |no |query |(Keine) |Auflisten der zurückzugebenden Felder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang |

## <a name="getlist"></a>GetList
Abrufen von Listen nach ID: Abrufen von Listen nach ID

```GET: /lists/{list_id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| board_id |string |Ja |query |(Keine) |Eindeutige ID des Boards zum Abrufen von Listen |
| list_id |string |Ja |path |(Keine) |Eindeutige ID der abzurufenden Liste |
| cards |string |no |query |(Keine) |Festlegen der zurückzugebenden Karten |
| card_fields |string |no |query |(Keine) |Auflisten der für jede Karte zurückzugebenden Kartenfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| board |Boolescher Wert |no |query |(Keine) |Festlegen, ob Boardinformationen zurückgegeben werden |
| board_fields |string |no |query |(Keine) |Auflisten der zurückzugebenden Boardfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |
| fields |string |no |query |(Keine) |Auflisten der zurückzugebenden Listenfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an. |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang |

## <a name="object-definitions"></a>Objektdefinitionen
### <a name="card"></a>Karte
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |Nein |
| checkItemStates |array |Nein |
| closed |Boolescher Wert |Nein |
| dateLastActivity |string |Nein |
| desc |string |Nein |
| idBoard |string |Nein |
| idList |string |Nein |
| idMembersVoted |array |Nein |
| idShort |integer |Nein |
| idAttachmentCover |string |Nein |
| manualCoverAttachment |Boolescher Wert |Nein |
| idLabels |array |Nein |
| Name |string |Nein |
| pos |integer |Nein |
| shortLink |string |Nein |
| badges |nicht definiert |Nein |
| due |string |Nein |
| email |string |Nein |
| shortUrl |string |Nein |
| subscribed |Boolescher Wert |Nein |
| url |string |Nein |

### <a name="badges"></a>Badges
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Votes |integer |Nein |
| ViewingMemberVoted |Boolescher Wert |Nein |
| Subscribed |Boolescher Wert |Nein |
| Fogbugz |string |Nein |
| CheckItems |integer |Nein |
| CheckItemsChecked |integer |Nein |
| Kommentare |integer |Nein |
| Attachments |integer |Nein |
| Beschreibung |Boolescher Wert |Nein |
| Due |string |Nein |

### <a name="object"></a>Objekt
### <a name="createcard"></a>CreateCard
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| idList |string |Ja |
| Name |string |Ja |
| desc |string |Nein |
| pos |string |Nein |
| idMembers |string |Nein |
| idLabels |string |Nein |
| urlSource |string |Nein |
| fileSource |string |Nein |
| idCardSource |string |Nein |
| keepFromSource |string |Nein |

### <a name="updatecard"></a>UpdateCard
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Name |string |Nein |
| desc |string |Nein |
| closed |Boolescher Wert |Nein |
| idMembers |string |Nein |
| idAttachmentCover |string |Nein |
| idList |string |Nein |
| idBoard |string |Nein |
| pos |string |Nein |
| due |string |Nein |
| subscribed |Boolescher Wert |Nein |

### <a name="board"></a>Board
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |Nein |
| closed |Boolescher Wert |Nein |
| dateLastActivity |string |Nein |
| dateLastView |string |Nein |
| desc |string |Nein |
| idOrganization |string |Nein |
| invitations |array |Nein |
| invited |Boolescher Wert |Nein |
| labelNames |nicht definiert |Nein |
| memberships |array |Nein |
| Name |string |Nein |
| pinned |Boolescher Wert |Nein |
| powerUps |array |Nein |
| perfs |nicht definiert |Nein |
| shortLink |string |Nein |
| shortUrl |string |Nein |
| starred |string |Nein |
| subscribed |string |Nein |
| url |string |Nein |

### <a name="label"></a>Bezeichnung
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| green |string |Nein |
| yellow |string |Nein |
| orange |string |Nein |
| red |string |Nein |
| purple |string |Nein |
| blue |string |Nein |
| sky |string |Nein |
| lime |string |Nein |
| pink |string |Nein |
| black |string |Nein |

### <a name="membership"></a>Mitgliedschaft
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |Nein |
| idMember |string |Nein |
| memberType |string |Nein |
| unconfirmed |Boolescher Wert |Nein |

### <a name="perfs"></a>Perfs
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| permissionLevel |string |Nein |
| voting |string |Nein |
| Kommentare |string |Nein |
| invitations |string |Nein |
| selfJoin |Boolescher Wert |Nein |
| cardCovers |Boolescher Wert |Nein |
| calendarFeedEnabled |Boolescher Wert |Nein |
| background |string |Nein |
| backgroundColor |string |Nein |
| backgroundImage |string |Nein |
| backgroundImageScaled |string |Nein |
| backgroundTile |Boolescher Wert |Nein |
| backgroundBrightness |string |Nein |
| canBePublic |Boolescher Wert |Nein |
| canBeOrg |Boolescher Wert |Nein |
| canBePrivate |Boolescher Wert |Nein |
| canInvite |Boolescher Wert |Nein |

### <a name="list"></a>Auflisten
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |Nein |
| Name |string |Nein |
| closed |Boolescher Wert |Nein |
| idBoard |string |Nein |
| pos |number |Nein |
| subscribed |Boolescher Wert |Nein |
| cards |array |Nein |
| board |nicht definiert |Nein |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md)




<!--HONumber=Jan17_HO3-->


