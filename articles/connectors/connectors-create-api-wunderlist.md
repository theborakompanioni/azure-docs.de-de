---
title: Wunderlist | Microsoft Docs
description: Erstellen Sie Logik-Apps mit Azure App Service. Wunderlist ist gleichzeitig eine Aufgabenliste und ein Aufgabenmanager und erleichtert es Menschen, ihren Alltag zu meistern. Ob Sie eine Einkaufsliste mit Ihrem Partner teilen, an einem Projekt arbeiten oder Ihren Urlaub planen – Wunderlist hilft Ihnen dabei, Ihre Aufgaben zu notieren, sie zu teilen und zu erledigen. Dabei übernimmt Wunderlist die Synchronisierung zwischen Telefon, Tablet und Computer – die Aufgaben sind auf allen Geräten sofort zur Hand.
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
# Erste Schritte mit dem Wunderlist-Connector
Wunderlist ist gleichzeitig eine Aufgabenliste und ein Aufgabenmanager und erleichtert es Menschen, ihren Alltag zu meistern. Ob Sie eine Einkaufsliste mit Ihrem Partner teilen, an einem Projekt arbeiten oder Ihren Urlaub planen – Wunderlist hilft Ihnen dabei, Ihre Aufgaben zu notieren, sie zu teilen und zu erledigen. Dabei übernimmt Wunderlist die Synchronisierung zwischen Telefon, Tablet und Computer – die Aufgaben sind auf allen Geräten sofort zur Hand.

> [!NOTE]
> Diese Version des Artikels gilt für die Schemaversion 2015-08-01-preview für Logik-Apps.
> 
> 

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md) beschrieben.

## Trigger und Aktionen
Der Wunderlist-Connector kann als Aktion verwendet werden. Er verfügt über Trigger. Alle Connectors unterstützen Daten im JSON- und XML-Format.

 Der Wunderlist-Connector verfügt über die folgenden Aktionen und/oder Trigger:

### Wunderlist-Aktionen
Sie können diese Aktionen ausführen:

| Aktion | Beschreibung |
| --- | --- |
| [RetrieveLists](connectors-create-api-wunderlist.md#retrievelists) |Ruft die Ihrem Konto zugeordneten Listen ab. |
| [CreateList](connectors-create-api-wunderlist.md#createlist) |Erstellt eine Liste. |
| [ListTasks](connectors-create-api-wunderlist.md#listtasks) |Ruft Aufgaben aus einer bestimmten Liste ab. |
| [CreateTask](connectors-create-api-wunderlist.md#createtask) |Erstellt eine Aufgabe. |
| [ListSubTasks](connectors-create-api-wunderlist.md#listsubtasks) |Ruft Teilaufgaben aus einer bestimmten Liste oder einer bestimmten Aufgabe ab. |
| [CreateSubTask](connectors-create-api-wunderlist.md#createsubtask) |Erstellt eine Teilaufgabe innerhalb einer bestimmten Aufgabe. |
| [ListNotes](connectors-create-api-wunderlist.md#listnotes) |Ruft Notizen für eine bestimmte Liste oder eine bestimmte Aufgabe ab. |
| [CreateNote](connectors-create-api-wunderlist.md#createnote) |Fügt einer bestimmten Aufgabe eine Notiz hinzu. |
| [ListComments](connectors-create-api-wunderlist.md#listcomments) |Ruft Aufgabenkommentare für eine bestimmte Liste oder eine bestimmte Aufgabe ab. |
| [CreateComment](connectors-create-api-wunderlist.md#createcomment) |Fügt einer bestimmten Aufgabe einen Kommentar hinzu. |
| [RetrieveReminders](connectors-create-api-wunderlist.md#retrievereminders) |Ruft Erinnerungen für eine bestimmte Liste oder eine bestimmte Aufgabe ab. |
| [CreateReminder](connectors-create-api-wunderlist.md#createreminder) |Legt eine Erinnerung fest. |
| [RetrieveFiles](connectors-create-api-wunderlist.md#retrievefiles) |Ruft Dateien für eine bestimmte Liste oder eine bestimmte Aufgabe ab. |
| [GetList](connectors-create-api-wunderlist.md#getlist) |Ruft eine bestimmte Liste ab. |
| [DeleteList](connectors-create-api-wunderlist.md#deletelist) |Löscht eine Liste. |
| [UpdateList](connectors-create-api-wunderlist.md#updatelist) |Aktualisiert eine bestimmte Liste. |
| [GetTask](connectors-create-api-wunderlist.md#gettask) |Ruft eine bestimmte Aufgabe ab. |
| [UpdateTask](connectors-create-api-wunderlist.md#updatetask) |Aktualisiert eine bestimmte Aufgabe. |
| [DeleteTask](connectors-create-api-wunderlist.md#deletetask) |Löscht eine bestimmte Aufgabe. |
| [GetSubTask](connectors-create-api-wunderlist.md#getsubtask) |Ruft eine bestimmte Teilaufgabe ab. |
| [UpdateSubTask](connectors-create-api-wunderlist.md#updatesubtask) |Aktualisiert eine bestimmte Teilaufgabe. |
| [DeleteSubTask](connectors-create-api-wunderlist.md#deletesubtask) |Löscht eine bestimmte Teilaufgabe. |
| [GetNote](connectors-create-api-wunderlist.md#getnote) |Ruft eine bestimmte Notiz ab. |
| [UpdateNote](connectors-create-api-wunderlist.md#updatenote) |Aktualisiert eine bestimmte Notiz. |
| [DeleteNote](connectors-create-api-wunderlist.md#deletenote) |Löscht eine bestimmte Notiz. |
| [GetComment](connectors-create-api-wunderlist.md#getcomment) |Ruft einen bestimmten Aufgabenkommentar ab. |
| [UpdateReminder](connectors-create-api-wunderlist.md#updatereminder) |Aktualisiert eine bestimmte Erinnerung. |
| [DeleteReminder](connectors-create-api-wunderlist.md#deletereminder) |Löscht eine bestimmte Erinnerung. |

### Wunderlist-Trigger
Sie können auf diese Ereignisse lauschen:

| Trigger | Beschreibung |
| --- | --- |
| Wenn eine Aufgabe fällig ist |Löst einen neuen Workflow aus, sobald eine Aufgabe in der Liste fällig ist. |
| Wenn eine neue Aufgabe erstellt wird |Löst einen neuen Workflow aus, sobald eine neue Aufgabe in der Liste erstellt wird. |
| Wenn eine Erinnerung auftritt |Löst einen neuen Workflow aus, sobald eine Erinnerung auftritt. |

## Herstellen einer Verbindung mit Wunderlist
Um Logik-Apps mit Wunderlist zu erstellen, müssen Sie zuerst eine **Verbindung** erstellen und anschließend die Details für die folgenden Eigenschaften angeben:

| Eigenschaft | Erforderlich | Beschreibung |
| --- | --- | --- |
| Token |Ja |Angeben der Anmeldeinformationen für Wunderlist |

Nachdem Sie die Verbindung erstellt haben, können Sie sie zum Ausführen der Aktionen und zum Lauschen auf die in diesem Artikel beschriebenen Trigger verwenden.

> [!INCLUDE [Schritte zum Herstellen einer Verbindung mit Wunderlist](../../includes/connectors-create-api-wunderlist.md)]
> 
> [!TIP]
> Sie können diese Verbindung in anderen Logik-Apps verwenden.
> 
> 

## Referenz für Wunderlist
Gilt für Version 1.0.

## TriggerTaskDue
Wenn eine Aufgabe fällig ist: Löst einen neuen Workflow aus, sobald eine Aufgabe in der Liste fällig ist.

```GET: /trigger/tasksdue```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list\_id |integer |Ja |query |(Keine) |ID der Liste |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |

## TriggerTaskNew
Wenn eine neue Aufgabe erstellt wird: Löst einen neuen Workflow aus, sobald eine neue Aufgabe in der Liste erstellt wird.

```GET: /trigger/tasksnew```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list\_id |integer |Ja |query |(Keine) |ID der Liste |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |

## TriggerReminder
Wenn eine Erinnerung auftritt: Löst einen neuen Workflow aus, sobald eine Erinnerung auftritt.

```GET: /trigger/reminders```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list\_id |integer |Ja |query |(Keine) |ID der Liste |
| task\_id |integer |no |query |(Keine) |ID der Aufgabe |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |

## RetrieveLists
Listen abrufen: Ruft die Ihrem Konto zugeordneten Listen ab.

```GET: /lists```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
|  | | | | | |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| default |Fehler beim Vorgang. |

## CreateList
Liste erstellen: Erstellt eine Liste.

```POST: /lists```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| post | |Ja |body |(Keine) |Neu zu erstellende Liste |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| die Standardeinstellung |Fehler beim Vorgang. |

## ListTasks
Aufgaben abrufen: Ruft Aufgaben aus einer bestimmten Liste ab.

```GET: /tasks```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list\_id |integer |Ja |query |(Keine) |ID der Liste |
| completed |Boolescher Wert |no |query |(Keine) |Abgeschlossen |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| default |Fehler beim Vorgang. |

## CreateTask
Aufgabe erstellen: Erstellt eine Aufgabe.

```POST: /tasks```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| post | |Ja |body |(Keine) |Neu zu erstellende Aufgabe |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 201 |Erstellt |

## ListSubTasks
Teilaufgaben abrufen: Ruft Teilaufgaben aus einer bestimmten Liste oder einer bestimmten Aufgabe ab.

```GET: /subtasks```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list\_id |integer |Ja |query |(Keine) |ID der Liste |
| task\_id |integer |no |query |(Keine) |ID der Aufgabe |
| abgeschlossen |Boolescher Wert |no |query |(Keine) |Abgeschlossen |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| default |Fehler beim Vorgang. |

## CreateSubTask
Teilaufgabe erstellen: Erstellt eine Teilaufgabe innerhalb einer bestimmten Aufgabe.

```POST: /subtasks```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| post | |Ja |body |(Keine) |Neu zu erstellende Teilaufgabe |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 201 |Erstellt |

## ListNotes
Notizen abrufen: Ruft Notizen für eine bestimmte Liste oder eine bestimmte Aufgabe ab.

```GET: /notes```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list\_id |integer |Ja |query |(Keine) |ID der Liste |
| task\_id |integer |no |query |(Keine) |ID der Aufgabe |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| default |Fehler beim Vorgang. |

## CreateNote
Notiz erstellen: Fügt einer bestimmten Aufgabe eine Notiz hinzu.

```POST: /notes```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| post | |Ja |body |(Keine) |Neu zu erstellende Notiz |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 201 |Erstellt |

## ListComments
Aufgabenkommentare abrufen: Ruft Aufgabenkommentare für eine bestimmte Liste oder eine bestimmte Aufgabe ab.

```GET: /task_comments```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list\_id |integer |Ja |query |(Keine) |ID der Liste |
| task\_id |integer |no |query |(Keine) |ID der Aufgabe |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| default |Fehler beim Vorgang. |

## CreateComment
Kommentar zu einer Aufgabe hinzufügen: Fügt einer bestimmten Aufgabe einen Kommentar hinzu.

```POST: /task_comments```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| post | |Ja |body |(Keine) |Neu zu erstellender Aufgabenkommentar |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 201 |Erstellt |

## RetrieveReminders
Erinnerungen abrufen: Ruft Erinnerungen für eine bestimmte Liste oder eine bestimmte Aufgabe ab.

```GET: /reminders```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list\_id |integer |Ja |query |(Keine) |ID der Liste |
| task\_id |integer |no |query |(Keine) |ID der Aufgabe |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| default |Fehler beim Vorgang. |

## CreateReminder
Erinnerung festlegen: Legt eine Erinnerung fest.

```POST: /reminders```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| post | |Ja |body |(Keine) |Neu zu erstellende Erinnerung |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| die Standardeinstellung |Fehler beim Vorgang. |

## RetrieveFiles
Dateien abrufen: Ruft Dateien für eine bestimmte Liste oder eine bestimmte Aufgabe ab.

```GET: /files```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list\_id |integer |Ja |query |(Keine) |ID der Liste |
| task\_id |integer |no |query |(Keine) |ID der Aufgabe |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| default |Fehler beim Vorgang. |

## GetList
Liste abrufen: Ruft eine bestimmte Liste ab.

```GET: /lists/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |string |Ja |path |(Keine) |ID der Liste |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |

## DeleteList
Liste löschen: Löscht eine Liste.

```DELETE: /lists/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |integer |Ja |path |(Keine) |ID der Liste |
| revision |integer |Ja |query |(Keine) |Version |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 204 |Kein Inhalt |

## UpdateList
Liste aktualisieren: Aktualisiert eine bestimmte Liste.

```PATCH: /lists/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |integer |Ja |path |(Keine) |ID der Liste |
| post | |Ja |body |(Keine) |Listendetails |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |

## GetTask
Aufgabe abrufen: Ruft eine bestimmte Aufgabe ab.

```GET: /tasks/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list\_id |integer |Ja |query |(Keine) |ID der Liste |
| id |integer |Ja |path |(Keine) |ID der Aufgabe |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |

## UpdateTask
Aufgabe aktualisieren: Aktualisiert eine bestimmte Aufgabe.

```PATCH: /tasks/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list\_id |integer |Ja |query |(Keine) |ID der Liste |
| id |integer |Ja |path |(Keine) |ID der Aufgabe |
| post | |Ja |body |(Keine) |Aufgabendetails |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |

## DeleteTask
Aufgabe löschen: Löscht eine bestimmte Aufgabe.

```DELETE: /tasks/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list\_id |integer |Ja |query |(Keine) |ID der Liste |
| id |integer |Ja |path |(Keine) |ID der Aufgabe |
| revision |integer |Ja |query |(Keine) |Version |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 204 |Kein Inhalt |

## GetSubTask
Teilaufgabe abrufen: Ruft eine bestimmte Teilaufgabe ab.

```GET: /subtasks/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |string |Ja |path |(Keine) |ID der Teilaufgabe |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |

## UpdateSubTask
Teilaufgabe aktualisieren: Aktualisiert eine bestimmte Teilaufgabe.

```PATCH: /subtasks/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |integer |Ja |path |(Keine) |ID der Teilaufgabe |
| post | |Ja |body |(Keine) |Details der Teilaufgabe |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |

## DeleteSubTask
Teilaufgabe löschen: Löscht eine bestimmte Teilaufgabe.

```DELETE: /subtasks/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |integer |Ja |path |(Keine) |ID der Teilaufgabe |
| revision |integer |Ja |query |(Keine) |Version |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 204 |Kein Inhalt |

## GetNote
Notiz abrufen: Ruft eine bestimmte Notiz ab.

```GET: /notes/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |string |Ja |path |(Keine) |ID der Notiz |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |

## UpdateNote
Notiz aktualisieren: Aktualisiert eine bestimmte Notiz.

```PATCH: /notes/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |integer |Ja |path |(Keine) |ID der Notiz |
| post | |Ja |body |(Keine) |Details der Notiz |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |

## DeleteNote
Notiz löschen: Löscht eine bestimmte Notiz.

```DELETE: /notes/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |integer |Ja |path |(Keine) |ID der Notiz |
| revision |integer |Ja |query |(Keine) |Version |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 204 |Kein Inhalt |

## GetComment
Aufgabenkommentar abrufen: Ruft einen bestimmten Aufgabenkommentar ab.

```GET: /task_comments/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |string |Ja |path |(Keine) |ID des Kommentars |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |

## UpdateReminder
Erinnerung aktualisieren: Aktualisiert eine bestimmte Erinnerung.

```PATCH: /reminders/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |integer |Ja |path |(Keine) |ID der Erinnerung |
| post | |Ja |body |(Keine) |Details der Erinnerung |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |

## DeleteReminder
Erinnerung löschen: Löscht eine bestimmte Erinnerung.

```DELETE: /reminders/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |integer |Ja |path |(Keine) |ID der Erinnerung |
| revision |integer |Ja |query |(Keine) |Version |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 204 |Kein Inhalt |

## Objektdefinitionen
### List
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |integer |Nein |
| created\_at |string |Nein |
| title |string |Nein |
| list\_type |string |Nein |
| Typ |string |Nein |
| revision |integer |Nein |

### CreatedList
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |integer |Nein |
| created\_at |string |Nein |
| title |string |Nein |
| revision |integer |Nein |
| Typ |string |Nein |

### Task
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |integer |Nein |
| assignee\_id |integer |Nein |
| assigner\_id |integer |Nein |
| created\_at |string |Nein |
| created\_by\_id |integer |Nein |
| due\_date |string |Nein |
| list\_id |integer |Nein |
| revision |integer |Nein |
| starred |Boolescher Wert |Nein |
| title |string |Nein |

### Subtask
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |integer |Nein |
| task\_id |integer |Nein |
| created\_at |string |Nein |
| created\_by\_id |integer |Nein |
| revision |string |Nein |
| title |string |Nein |

### Note
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |integer |Nein |
| task\_id |integer |Nein |
| Inhalt |string |Nein |
| created\_at |string |Nein |
| updated\_at |string |Nein |
| revision |integer |Nein |

### Kommentar
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |integer |Nein |
| task\_id |integer |Nein |
| revision |integer |Nein |
| Text |string |Nein |
| Typ |string |Nein |
| created\_at |string |Nein |

### Reminder
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |integer |Nein |
| date |string |Nein |
| task\_id |integer |Nein |
| revision |integer |Nein |
| Typ |string |Nein |
| created\_at |string |Nein |
| updated\_at |string |Nein |

### CreatedReminder
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |integer |Nein |
| date |string |Nein |
| task\_id |integer |Nein |
| revision |integer |Nein |
| created\_at |string |Nein |
| updated\_at |string |Nein |

### File
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |integer |Nein |
| url |string |Nein |
| task\_id |integer |Nein |
| list\_id |integer |Nein |
| user\_id |integer |Nein |
| file\_name |string |Nein |
| content\_type |string |Nein |
| file\_size |integer |Nein |
| local\_created\_at |string |Nein |
| created\_at |string |Nein |
| updated\_at |string |Nein |
| Typ |string |Nein |
| revision |integer |Nein |

### NewTask
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| list\_id |integer |Ja |
| title |string |Ja |
| assignee\_id |integer |Nein |
| abgeschlossen |Boolescher Wert |Nein |
| recurrence\_type |string |Nein |
| recurrence\_count |integer |Nein |
| due\_date |string |Nein |
| starred |Boolescher Wert |Nein |

### NewList
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| title |string |Ja |

### NewSubtask
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| list\_id |integer |Ja |
| task\_id |integer |Ja |
| title |string |Ja |
| abgeschlossen |Boolescher Wert |Nein |

### NewNote
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| list\_id |integer |Ja |
| task\_id |integer |Ja |
| Inhalt |string |Ja |

### NewComment
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| list\_id |integer |Ja |
| task\_id |integer |Ja |
| Text |string |Ja |

### NewReminder
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| list\_id |integer |Ja |
| task\_id |integer |Ja |
| date |string |Ja |

### UpdateTask
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| revision |integer |Nein |
| title |string |Nein |
| assignee\_id |integer |Nein |
| abgeschlossen |Boolescher Wert |Nein |
| recurrence\_type |string |Nein |
| recurrence\_count |integer |Nein |
| due\_date |string |Nein |
| starred |Boolescher Wert |Nein |

### UpdateList
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| revision |integer |Nein |
| title |string |Nein |

### UpdateSubtask
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| revision |integer |Nein |
| title |string |Nein |
| abgeschlossen |Boolescher Wert |Nein |

### UpdateNote
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| revision |integer |Nein |
| Inhalt |string |Nein |

### UpdateReminder
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| date |string |Nein |
| revision |integer |Nein |

## Nächste Schritte
[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md)

<!---HONumber=AcomDC_0824_2016-->