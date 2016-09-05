<properties
pageTitle="GitHub | Microsoft Azure"
description="Erstellen Sie Logik-Apps mit Azure App Service. GitHub ist ein webbasierter Hostingdienst für Git-Repositorys. Er bietet alle Funktionen von Git zur Kontrolle von verteilten Überarbeitungen und zur Quellcodeverwaltung sowie eigene Features."
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
ms.date="08/18/2016"
ms.author="deonhe"/>

# Erste Schritte mit dem GitHub-Connector

GitHub ist ein webbasierter Hostingdienst für Git-Repositorys. Er bietet alle Funktionen von Git zur Kontrolle von verteilten Überarbeitungen und zur Quellcodeverwaltung sowie eigene Features.

>[AZURE.NOTE] Diese Version des Artikels gilt für die Schemaversion 2015-08-01-preview für Logik-Apps.

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md) beschrieben.

## Trigger und Aktionen

Der GitHub-Connector kann als Aktion verwendet werden. Er verfügt über Trigger. Alle Connectors unterstützen Daten im JSON- und XML-Format.

 Der GitHub-Connector verfügt über die folgenden Aktionen und/oder Trigger:

### GitHub-Aktionen
Sie können diese Aktionen ausführen:

|Aktion|Beschreibung|
|--- | ---|
|[CreateIssue](connectors-create-api-github.md#createissue)|Erstellt eine Problemmeldung|
### GitHub-Trigger
Sie können auf diese Ereignisse lauschen:

|Trigger | Beschreibung|
|--- | ---|
|Wenn eine Problemmeldung geöffnet wird|Eine Problemmeldung wird geöffnet.|
|Wenn eine Problemmeldung geschlossen wird|Eine Problemmeldung wird geschlossen.|
|Wenn eine Problemmeldung zugewiesen wird|Eine Problemmeldung wird zugewiesen.|


## Herstellen einer Verbindung mit GitHub
Um Logik-Apps mit GitHub zu erstellen, müssen Sie zuerst eine **Verbindung** erstellen und anschließend die Details für die folgenden Eigenschaften angeben:

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|Token|Ja|Angeben der Anmeldeinformationen für GitHub|
Nachdem Sie die Verbindung erstellt haben, können Sie sie zum Ausführen der Aktionen und zum Lauschen auf die in diesem Artikel beschriebenen Trigger verwenden.

>[AZURE.INCLUDE [Schritte zum Herstellen einer Verbindung mit GitHub](../../includes/connectors-create-api-github.md)]

>[AZURE.TIP] Sie können diese Verbindung in anderen Logik-Apps verwenden.

## Referenz für GitHub
Gilt für Version: 1.0.

## CreateIssue
Problemmeldung erstellen: Erstellt eine Problemmeldung.

```POST: /repos/{repositoryOwner}/{repositoryName}/issues```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|repositoryOwner|string|Ja|path|(Keine)|Eigentümer des Repositorys|
|repositoryName|string|Ja|path|(Keine)|Name des Repositorys|
|issueBasicDetails| |Ja|body|(Keine)|Details zum Problem|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten|
|default|Fehler beim Vorgang.|


## IssueOpened
Wenn eine Problemmeldung geöffnet wird: Eine Problemmeldung wird geöffnet.

```GET: /trigger/issueOpened```

Es gibt keine Parameter für diesen Aufruf
#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten|
|default|Fehler beim Vorgang.|


## IssueClosed
Wenn eine Problemmeldung geschlossen wird: Eine Problemmeldung wird geschlossen.

```GET: /trigger/issueClosed```

Es gibt keine Parameter für diesen Aufruf
#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten|
|default|Fehler beim Vorgang.|


## IssueAssigned
Wenn eine Problemmeldung zugewiesen wird: Eine Problemmeldung wird zugewiesen.

```GET: /trigger/issueAssigned```

Es gibt keine Parameter für diesen Aufruf
#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten|
|default|Fehler beim Vorgang.|


## Objektdefinitionen 

### IssueBasicDetailsModel


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|title|string|Ja |
|body|string|Ja |
|assignee|string|Ja |



### IssueDetailsModel


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|title|string|Ja |
|body|string|Ja |
|assignee|string|Ja |
|number|string|Nein |
|state|string|Nein |
|created\_at|string|Nein |
|repository\_url|string|Nein |


## Nächste Schritte
[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md)

<!---HONumber=AcomDC_0824_2016-->