<properties
pageTitle="Trello | Microsoft Azure"
description="Erstellen Sie Logik-Apps mit Azure App Service. Trello bietet Ihnen eine Übersicht über all Ihre beruflichen und privaten Projekte. Dies ist eine einfache, kostenlose, flexible und visuelle Möglichkeit, um Ihre Projekte zu verwalten und alles zu organisieren. Stellen Sie eine Verbindung mit Trello her, um Ihre Boards, Listen und Karten zu verwalten."
services="logic-apps"	
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
ms.date="05/18/2016"
ms.author="deonhe"/>

# Erste Schritte mit dem Trello-Connector



Der Trello-Connector kann verwendet werden in:

- [Logik-Apps](../app-service-logic/app-service-logic-what-are-logic-apps.md)
- [PowerApps](http://powerapps.microsoft.com)
- [Flow](http://flows.microsoft.com)

>[AZURE.NOTE] Diese Version des Artikels gilt für die Schemaversion 2015-08-01-preview für Logik-Apps.

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md) beschrieben.

## Trigger und Aktionen

Der Trello-Connector kann als Aktion verwendet werden. Er verfügt über Trigger. Alle Connectors unterstützen Daten im JSON- und XML-Format.

 Der Trello-Connector verfügt über die folgenden Aktionen und/oder Trigger:

### Trello-Aktionen
Sie können diese Aktionen ausführen:

|Aktion|Beschreibung|
|--- | ---|
|[ListCards](connectors-create-api-trello.md#listcards)|Auflisten der Karten im Board|
|[GetCard](connectors-create-api-trello.md#getcard)|Abrufen von Karten nach ID|
|[UpdateCard](connectors-create-api-trello.md#updatecard)|Aktualisieren von Karten|
|[DeleteCard](connectors-create-api-trello.md#deletecard)|Löschen von Karten|
|[CreateCard](connectors-create-api-trello.md#createcard)|Erstellen neuer Karten in Ihrem Trello-Konto|
|[ListBoards](connectors-create-api-trello.md#listboards)|Auflisten von Boards|
|[GetBoard](connectors-create-api-trello.md#getboard)|Abrufen von Boards nach ID|
|[ListLists](connectors-create-api-trello.md#listlists)|Auflisten von Kartenlisten im Board|
|[GetList](connectors-create-api-trello.md#getlist)|Abrufen von Listen nach ID|
### Trello-Trigger
Sie können auf diese Ereignisse lauschen:

|Trigger | Beschreibung|
|--- | ---|
|Wenn einem Board eine neue Karte hinzugefügt wird|Löst einen Ablauf aus, wenn einem Board eine neue Karte hinzugefügt wird|
|Wenn einer Liste eine neue Karte hinzugefügt wird|Löst einen Ablauf aus, wenn einer Liste eine neue Karte hinzugefügt wird|


## Herstellen einer Verbindung mit Trello
Um Logik-Apps mit Trello zu erstellen, müssen Sie zuerst eine **Verbindung** herstellen und anschließend die Details für die folgenden Eigenschaften angeben:

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|Tokenverschlüsselung|Ja|Angeben von Anmeldeinformationen für Trello|
Nachdem Sie die Verbindung erstellt haben, können Sie sie zum Ausführen der Aktionen und zum Lauschen auf die in diesem Artikel beschriebenen Trigger verwenden.

>[AZURE.INCLUDE [Schritte zum Herstellen einer Verbindung mit Trello](../../includes/connectors-create-api-trello.md)]

>[AZURE.TIP] Sie können diese Verbindung in anderen Logik-Apps verwenden.

## Referenz für Trello
Gilt für Version 1.0.

## OnNewCardInBoard
Wenn einem Board eine neue Karte hinzugefügt wird: Löst einen Ablauf aus, wenn einem Board eine neue Karte hinzugefügt wird.

```GET: /trigger/boards/{board_id}/cards```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board\_id|string|Ja|path|(Keine)|Eindeutige ID des Boards, das mit Karten gefüllt wird|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten|
|die Standardeinstellung|Fehler beim Vorgang|


## OnNewCardInList
Wenn einer Liste eine neue Karte hinzugefügt wird: Löst einen Ablauf aus, wenn einer Liste eine neue Karte hinzugefügt wird.

```GET: /trigger/lists/{list_id}/cards```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board\_id|string|Ja|query|(Keine)|Eindeutige ID des Boards, das mit Karten gefüllt wird|
|list\_id|string|Ja|path|(Keine)|Eindeutige ID der Liste, die mit Karten gefüllt wird|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten|
|die Standardeinstellung|Fehler beim Vorgang|


## ListCards
Auflisten von Karten im Board: Auflisten von Karten im Board

```GET: /boards/{board_id}/cards```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board\_id|string|Ja|path|(Keine)|ID des Boards zum Abrufen aller Karten|
|actions|string|no|query|(Keine)|Auflisten der zurückzugebenden Aktionen. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|attachments|Boolescher Wert|no|query|(Keine)|Anhänge anzeigen|
|attachment\_fields|string|no|query|(Keine)|Auflisten der zurückzugebenden Anhangfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|stickers|Boolescher Wert|no|query|(Keine)|Anzeigen von Stickern|
|members|Boolescher Wert|no|query|(Keine)|Anzeigen von Mitgliedern|
|memeber\_fields|string|no|query|(Keine)|Anzeigen der zurückzugebenden Mitgliederfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|CheckItemStates|Boolescher Wert|no|query|(Keine)|Zurückgeben des Kartenstatus.|
|Checklists|string|no|query|(Keine)|Anzeigen von Checklisten|
|limit|integer|no|query|(Keine)|Die maximale Anzahl der zurückzugebenden Ergebnisse, zwischen 1 und 1000|
|since|string|no|query|(Keine)|Abrufen aller Karten nach diesem Datum|
|Vorbereitung|string|no|query|(Keine)|Abrufen aller Karten vor diesem Datum|
|filter|string|no|query|(Keine)|Filtern der Antwort|
|fields|string|no|query|(Keine)|Anzeigen der zurückzugebenden Kartenfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten|
|die Standardeinstellung|Fehler beim Vorgang|


## GetCard
Abrufen von Karten nach ID: Abrufen von Karten nach ID

```GET: /cards/{card_id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board\_id|string|Ja|query|(Keine)|ID des Boards, das mit Karten gefüllt wird|
|card\_id|string|Ja|path|(Keine)|ID der abzurufenden Karte|
|actions|string|no|query|(Keine)|Auflisten der zurückzugebenden Aktionen. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|actions\_entities|Boolescher Wert|no|query|(Keine)|Zurückgeben von Aktionsentitäten|
|actions\_display|Boolescher Wert|no|query|(Keine)|Zurückgeben von Aktionsanzeigen|
|actions\_limit|integer|no|query|(Keine)|Max. Anzahl zurückzugebender Aktionen|
|action\_fields|string|no|query|(Keine)|Liste der Aktionsfelder, die für jede Aktion zurückzugeben sind. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|action\_memberCreator\_fields|string|no|query|(Keine)|Liste zurückzugebenden Felder für Aktionsmitgliederersteller. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|attachments|Boolescher Wert|no|query|(Keine)|Zurückgeben von Anhängen|
|attachement\_fields|string|no|query|(Keine)|Liste von Anhangfeldern, die für jeden Anhang zurückgegeben werden. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|members|Boolescher Wert|no|query|(Keine)|Zurückgeben von Mitgliedern|
|member\_fields|string|no|query|(Keine)|Liste der Mitgliedsfelder, die für jedes Mitglied zurückgegeben werden. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|membersVoted|Boolescher Wert|no|query|(Keine)|Zurückgeben von Mitgliedern, die abgestimmt haben|
|memberVoted\_fields|string|no|query|(Keine)|Liste der Felder von Mitglieder, die abgestimmt haben, die für jedes Mitglied zurückgegeben werden. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|checkItemStates|Boolescher Wert|no|query|(Keine)|Zurückgeben des Kartenstatus|
|checkItemState\_fields|string|no|query|(Keine)|Liste der Statusfelder, die für jeden Kartenelementstatus zurückgegeben werden. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|checklists|string|no|query|(Keine)|Zurückgeben von Checklisten|
|checklist\_fields|string|no|query|(Keine)|Liste der Checklistenfelder, die für jede Checkliste zurückgegeben werden. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|board|Boolescher Wert|no|query|(Keine)|Zurückgeben des Boards, zu dem die Karte gehört|
|board\_fields|string|no|query|(Keine)|Auflisten der zurückzugebenden Boardfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|list|Boolescher Wert|no|query|(Keine)|Zurückgeben der Liste, zu der die Karte gehört|
|list\_fields|string|no|query|(Keine)|Auflisten der zurückzugebenden Listenfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|stickers|Boolescher Wert|no|query|(Keine)|Zurückgeben von Stickern|
|sticker\_fields|string|no|query|(Keine)|Auflisten der Stickerfelder, die für jeden Sticker zurückgegeben werden. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|fields|string|no|query|(Keine)|Anzeigen der zurückzugebenden Kartenfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten|
|die Standardeinstellung|Fehler beim Vorgang|


## UpdateCard
Aktualisieren von Karten: Aktualisieren von Karten

```PUT: /cards/{card_id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board\_id|string|Ja|query|(Keine)|ID des Boards, von dem die Karten abgerufen werden|
|card\_id|string|Ja|path|(Keine)|ID der zu aktualisierenden Karte|
|updateCard| |Ja|body|(Keine)|Aktualisierte Kartenwerte|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten|
|die Standardeinstellung|Fehler beim Vorgang|


## DeleteCard
Löschen von Karten: Löschen von Karten

```DELETE: /cards/{card_id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board\_id|string|Ja|query|(Keine)|ID des Boards, von dem die Karten abgerufen werden|
|card\_id|string|Ja|path|(Keine)|ID der zu löschenden Karte|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten|
|die Standardeinstellung|Fehler beim Vorgang|


## CreateCard
Erstellen von Karten: Erstellt eine neue Karten in Ihrem Trello-Konto

```POST: /cards```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board\_id|string|Ja|query|(Keine)|Eindeutige ID des Boards, in dem Karten erstellt werden|
|newCard| |Ja|body|(Keine)|Neue Karte, die dem Trello-Board hinzugefügt wird|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten|
|die Standardeinstellung|Fehler beim Vorgang|


## ListBoards
Auflisten von Boards: Auflisten von Boards

```GET: /member/me/boards```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|filter|string|no|query|(Keine)|Auflisten von Filtern, die auf Board-Ergebnisse angewandt werden. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|fields|string|no|query|(Keine)|Auflisten der zurückzugebenden Boardfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|actions|string|no|query|(Keine)|Auflisten der zurückzugebenden Aktionsfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|actions\_entities|Boolescher Wert|no|query|(Keine)|Zurückgeben von Aktionsentitäten|
|actions\_limit|integer|no|query|(Keine)|Max. Anzahl zurückzugebender Aktionen|
|actions\_format|string|no|query|(Keine)|Festlegen des Formats der zurückzugebenden Aktionen|
|actions\_since|string|no|query|(Keine)|Zurückgeben von Aktionen nach dem angegebenen Datum|
|action\_fields|string|no|query|(Keine)|Auflisten der Felder der zurückzugebenden Aktion. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|memberships|string|no|query|(Keine)|Festlegen der zurückzugebenden Mitgliedschaftsinformationen. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|organization|Boolescher Wert|no|query|(Keine)|Festlegen der zurückzugebenden Organisationsinformationen.|
|organization\_fields|string|no|query|(Keine)|Auflisten der zurückzugebenden Organisationsfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|lists|string|no|query|(Keine)|Festlegen, ob zum Board gehörige Listen zurückgegeben werden|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten|
|die Standardeinstellung|Fehler beim Vorgang|


## GetBoard
Abrufen von Boards nach ID: Abrufen von Boards nach ID

```GET: /boards/{board_id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board\_id|string|Ja|path|(Keine)|Eindeutige ID des abzurufenden Boards|
|actions|string|no|query|(Keine)|Auflisten der zurückzugebenden Aktionen. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|action\_entities|Boolescher Wert|no|query|(Keine)|Festlegen, ob Aktionsentitäten zurückzugeben sind|
|actions\_display|Boolescher Wert|no|query|(Keine)|Festlegen, ob Aktionsanzeigen zurückzugeben sind|
|actions\_format|string|no|query|(Keine)|Festlegen des Formats der zurückzugebenden Aktionen|
|actions\_since|string|no|query|(Keine)|Nur Aktionen nach diesem Datum zurückgeben|
|actions\_limit|integer|no|query|(Keine)|Max. Anzahl zurückzugebender Aktionen|
|action\_fields|string|no|query|(Keine)|Auflisten der mit jedem Feld zurückzugebenden Felder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|action\_memeber|Boolescher Wert|no|query|(Keine)|Festlegen, ob Aktionsmitglieder zurückzugeben sind|
|action\_member\_fields|string|no|query|(Keine)|Auflisten der mit jedem Aktionsmitglied zurückzugebenden Mitgliedsfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|action\_memberCreator|Boolescher Wert|no|query|(Keine)|Festlegen, ob Aktionsmitgliederersteller zurückzugeben sind|
|action\_memberCreator\_fields|string|no|query|(Keine)|Auflisten der zurückzugebenden Felder für Aktionsmitgliederersteller. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|cards|string|no|query|(Keine)|Festlegen der zurückzugebenden Karten|
|card\_fields|string|no|query|(Keine)|Auflisten der für jede Karte zurückzugebenden Felder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|card\_attachments|Boolescher Wert|Ja|query|(Keine)|Festlegen, ob Anhänge zu Karten zurückzugeben sind|
|card\_attachment\_fields|string|no|query|(Keine)|Auflisten der für jeden Anhang zurückzugebenden Anhangfelder Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|card\_checklists|string|no|query|(Keine)|Festlegen der für jede Karte zurückzugebenden Checklisten|
|card\_stickers|Boolescher Wert|no|query|(Keine)|Festlegen, ob Kartensticker zurückzugeben sind|
|boardStarts|string|no|query|(Keine)|Festlegen der zurückzugebenden Board-Sterne|
|Bezeichnungen|string|no|query|(Keine)|Festlegen der zurückzugebenden Labels|
|label\_fields|string|no|query|(Keine)|Auflisten der für jedes Label zurückzugebenden Labelfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|labels\_limits|integer|no|query|(Keine)|Max. Anzahl zurückzugebender Labels|
|lists|string|no|query|(Keine)|Festlegen der zurückzugebenden Listen|
|list\_fields|string|no|query|(Keine)|Auflisten der für jede Liste zurückzugebenden Listenfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|memberships|string|no|query|(Keine)|Auflisten der zurückzugebenden Mitgliedschaften. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|memberships\_member|Boolescher Wert|no|query|(Keine)|Festlegen, ob Mitgliedschaftsmitglieder zurückzugeben sind|
|memberships\_member\_fields|string|no|query|(Keine)|Auflisten der Mitgliedsfelder, die für jedes Mitgliedschaftsmitglied zurückgegeben werden. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|members|string|no|query|(Keine)|Auflisten der zurückzugebenden Mitglieder|
|member\_fields|string|no|query|(Keine)|Auflisten der Mitgliedsfelder, die für jedes Mitglied zurückgegeben werden. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|membersInvited|string|no|query|(Keine)|Festlegen der zurückzugebenden eingeladenen Mitglieder|
|membersInvited\_fields|string|no|query|(Keine)|Auflisten der jeweils zurückzugebenden Felder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|checklists|string|no|query|(Keine)|Festlegen der zurückzugebenden Checklisten|
|checklist\_fields|string|no|query|(Keine)|Auflisten der Checklistenfelder, die für jede Checkliste zurückgegeben werden. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|organization|Boolescher Wert|no|query|(Keine)|Festlegen, ob Organisationsinformationen zurückgegeben werden|
|organization\_fields|string|no|query|(Keine)|Auflisten der für jede Organisation zurückzugebenden Organisationsfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|organization\_memberships|string|no|query|(Keine)|Auflisten der zurückzugebenden Organisationsmitglieder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|myPerfs|Boolescher Wert|no|query|(Keine)|Festlegen, ob eigene Perfs zurückzugeben sind|
|fields|string|no|query|(Keine)|Auflisten der zurückzugebenden Felder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten|
|die Standardeinstellung|Fehler beim Vorgang|


## ListLists
Auflisten von Kartenlisten im Board: Auflisten von Kartenlisten im Board

```GET: /boards/{board_id}/lists```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board\_id|string|Ja|path|(Keine)|Eindeutige ID des Boards zum Abrufen von Listen|
|cards|string|no|query|(Keine)|Festlegen der zurückzugebenden Karten|
|card\_fields|string|no|query|(Keine)|Auflisten der Kartenfelder für die Rückgabe. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|filter|string|no|query|(Keine)|Festlegen der Filtereigenschaft für Listen|
|fields|string|no|query|(Keine)|Auflisten der zurückzugebenden Felder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten|
|die Standardeinstellung|Fehler beim Vorgang|


## GetList
Abrufen von Listen nach ID: Abrufen von Listen nach ID

```GET: /lists/{list_id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board\_id|string|Ja|query|(Keine)|Eindeutige ID des Boards zum Abrufen von Listen|
|list\_id|string|Ja|path|(Keine)|Eindeutige ID der abzurufenden Liste|
|cards|string|no|query|(Keine)|Festlegen der zurückzugebenden Karten|
|card\_fields|string|no|query|(Keine)|Auflisten der für jede Karte zurückzugebenden Kartenfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|board|Boolescher Wert|no|query|(Keine)|Festlegen, ob Boardinformationen zurückgegeben werden|
|board\_fields|string|no|query|(Keine)|Auflisten der zurückzugebenden Boardfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|
|fields|string|no|query|(Keine)|Auflisten der zurückzugebenden Listenfelder. Geben Sie „all“ oder eine durch Trennzeichen getrennte Liste gültiger Werte an.|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten|
|die Standardeinstellung|Fehler beim Vorgang|


## Objektdefinitionen 

### Karte


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|id|string|Nein |
|checkItemStates|array|Nein |
|closed|Boolescher Wert|Nein |
|dateLastActivity|string|Nein |
|desc|string|Nein |
|idBoard|string|Nein |
|idList|string|Nein |
|idMembersVoted|array|Nein |
|idShort|integer|Nein |
|idAttachmentCover|string|Nein |
|manualCoverAttachment|Boolescher Wert|Nein |
|idLabels|array|Nein |
|Name|string|Nein |
|pos|integer|Nein |
|shortLink|string|Nein |
|badges|nicht definiert|Nein |
|due|string|Nein |
|E-Mail|string|Nein |
|shortUrl|string|Nein |
|subscribed|Boolescher Wert|Nein |
|url|string|Nein |



### Badges


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Votes|integer|Nein |
|ViewingMemberVoted|Boolescher Wert|Nein |
|Subscribed|Boolescher Wert|Nein |
|Fogbugz|string|Nein |
|CheckItems|integer|Nein |
|CheckItemsChecked|integer|Nein |
|Kommentare|integer|Nein |
|Anlagen|integer|Nein |
|Beschreibung|Boolescher Wert|Nein |
|Due|string|Nein |



### Objekt


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|



### CreateCard


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|idList|string|Ja |
|Name|string|Ja |
|desc|string|Nein |
|pos|string|Nein |
|idMembers|string|Nein |
|idLabels|string|Nein |
|urlSource|string|Nein |
|fileSource|string|Nein |
|idCardSource|string|Nein |
|keepFromSource|string|Nein |



### UpdateCard


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|name|string|Nein |
|desc|string|Nein |
|closed|Boolescher Wert|Nein |
|idMembers|string|Nein |
|idAttachmentCover|string|Nein |
|idList|string|Nein |
|idBoard|string|Nein |
|pos|string|Nein |
|due|string|Nein |
|subscribed|Boolescher Wert|Nein |



### Board


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|id|string|Nein |
|closed|Boolescher Wert|Nein |
|dateLastActivity|string|Nein |
|dateLastView|string|Nein |
|desc|string|Nein |
|idOrganization|string|Nein |
|invitations|array|Nein |
|invited|Boolescher Wert|Nein |
|labelNames|nicht definiert|Nein |
|memberships|array|Nein |
|Name|string|Nein |
|pinned|Boolescher Wert|Nein |
|powerUps|array|Nein |
|perfs|nicht definiert|Nein |
|shortLink|string|Nein |
|shortUrl|string|Nein |
|starred|string|Nein |
|subscribed|string|Nein |
|url|string|Nein |



### Bezeichnung


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|green|string|Nein |
|yellow|string|Nein |
|orange|string|Nein |
|red|string|Nein |
|purple|string|Nein |
|blue|string|Nein |
|sky|string|Nein |
|lime|string|Nein |
|pink|string|Nein |
|black|string|Nein |



### Mitgliedschaft


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|id|string|Nein |
|idMember|string|Nein |
|memberType|string|Nein |
|unconfirmed|Boolescher Wert|Nein |



### Perfs


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|permissionLevel|string|Nein |
|voting|string|Nein |
|Kommentare|string|Nein |
|invitations|string|Nein |
|selfJoin|Boolescher Wert|Nein |
|cardCovers|Boolescher Wert|Nein |
|calendarFeedEnabled|Boolescher Wert|Nein |
|background|string|Nein |
|backgroundColor|string|Nein |
|backgroundImage|string|Nein |
|backgroundImageScaled|string|Nein |
|backgroundTile|Boolescher Wert|Nein |
|backgroundBrightness|string|Nein |
|canBePublic|Boolescher Wert|Nein |
|canBeOrg|Boolescher Wert|Nein |
|canBePrivate|Boolescher Wert|Nein |
|canInvite|Boolescher Wert|Nein |



### Auflisten


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|id|string|Nein |
|Name|string|Nein |
|closed|Boolescher Wert|Nein |
|idBoard|string|Nein |
|pos|number|Nein |
|subscribed|Boolescher Wert|Nein |
|cards|array|Nein |
|board|nicht definiert|Nein |


## Nächste Schritte
[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md)

<!---HONumber=AcomDC_0803_2016-->