---
title: Wunderlist | Microsoft Docs
description: "Erstellen Sie Logik-Apps mit Azure App Service. Wunderlist ist gleichzeitig eine Aufgabenliste und ein Aufgabenmanager und erleichtert es Menschen, ihren Alltag zu meistern.  Ob Sie eine Einkaufsliste mit Ihrem Partner teilen, an einem Projekt arbeiten oder Ihren Urlaub planen – Wunderlist hilft Ihnen dabei, Ihre Aufgaben zu notieren, sie zu teilen und zu erledigen. Dabei übernimmt Wunderlist die Synchronisierung zwischen Telefon, Tablet und Computer – die Aufgaben sind auf allen Geräten sofort zur Hand."
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: e4773ecf-3ad3-44b4-a1b5-ee5f58baeadd
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 7a528a47373d7e9ae2cdc7d62328ed8e83112863


---
# <a name="get-started-with-the-wunderlist-connector"></a>Erste Schritte mit dem Wunderlist-Connector
Wunderlist ist gleichzeitig eine Aufgabenliste und ein Aufgabenmanager und erleichtert es Menschen, ihren Alltag zu meistern.  Ob Sie eine Einkaufsliste mit Ihrem Partner teilen, an einem Projekt arbeiten oder Ihren Urlaub planen – Wunderlist hilft Ihnen dabei, Ihre Aufgaben zu notieren, sie zu teilen und zu erledigen. Dabei übernimmt Wunderlist die Synchronisierung zwischen Telefon, Tablet und Computer – die Aufgaben sind auf allen Geräten sofort zur Hand.

> [!NOTE]
> Diese Version des Artikels gilt für Logik-Apps mit der Schemaversion „2015-08-01-preview“.
> 
> 

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md) beschrieben.

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Der Wunderlist-Connector kann als Aktion verwendet werden. Er verfügt über Trigger. Alle Connectors unterstützen Daten im JSON- und XML-Format.

 Der Wunderlist-Connector verfügt über die folgenden Aktionen und/oder Trigger:

### <a name="wunderlist-actions"></a>Wunderlist-Aktionen
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

### <a name="wunderlist-triggers"></a>Wunderlist-Trigger
Sie können auf diese Ereignisse lauschen:

| Trigger | Beschreibung |
| --- | --- |
| Wenn eine Aufgabe fällig ist |Löst einen neuen Workflow aus, sobald eine Aufgabe in der Liste fällig ist. |
| Wenn eine neue Aufgabe erstellt wird |Löst einen neuen Workflow aus, sobald eine neue Aufgabe in der Liste erstellt wird. |
| Wenn eine Erinnerung auftritt |Löst einen neuen Workflow aus, sobald eine Erinnerung auftritt. |

## <a name="create-a-connection-to-wunderlist"></a>Herstellen einer Verbindung mit Wunderlist
Um Logik-Apps mit Wunderlist zu erstellen, müssen Sie zuerst eine **Verbindung** erstellen und anschließend die Details für die folgenden Eigenschaften angeben:

| Eigenschaft | Erforderlich | Beschreibung |
| --- | --- | --- |
| Tokenverschlüsselung |Ja |Angeben der Anmeldeinformationen für Wunderlist |

Nachdem Sie die Verbindung erstellt haben, können Sie sie zum Ausführen der Aktionen und zum Lauschen auf die in diesem Artikel beschriebenen Trigger verwenden.

> [!INCLUDE [Steps to create a connection to Wunderlist](../../includes/connectors-create-api-wunderlist.md)]
> 
> [!TIP]
> Sie können diese Verbindung in anderen Logik-Apps verwenden.
> 
> 

## <a name="reference-for-wunderlist"></a>Referenz für Wunderlist
Gilt für Version 1.0.

## <a name="triggertaskdue"></a>TriggerTaskDue
Wenn eine Aufgabe fällig ist: Löst einen neuen Workflow aus, sobald eine Aufgabe in der Liste fällig ist.

```GET: /trigger/tasksdue```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list_id |integer |Ja |query |(Keine) |ID der Liste |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |

## <a name="triggertasknew"></a>TriggerTaskNew
Wenn eine neue Aufgabe erstellt wird: Löst einen neuen Workflow aus, sobald eine neue Aufgabe in der Liste erstellt wird.

```GET: /trigger/tasksnew```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list_id |integer |Ja |query |(Keine) |ID der Liste |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |

## <a name="triggerreminder"></a>TriggerReminder
Wenn eine Erinnerung auftritt: Löst einen neuen Workflow aus, sobald eine Erinnerung auftritt.

```GET: /trigger/reminders```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list_id |integer |Ja |query |(Keine) |ID der Liste |
| task_id |integer |no |query |(Keine) |ID der Aufgabe |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |

## <a name="retrievelists"></a>RetrieveLists
Listen abrufen: Ruft die Ihrem Konto zugeordneten Listen ab.

```GET: /lists```

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="createlist"></a>CreateList
Liste erstellen: Erstellt eine Liste.

```POST: /lists```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| post | |Ja |body |(Keine) |Neu zu erstellende Liste |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="listtasks"></a>ListTasks
Aufgaben abrufen: Ruft Aufgaben aus einer bestimmten Liste ab.

```GET: /tasks```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list_id |integer |Ja |query |(Keine) |ID der Liste |
| completed |Boolescher Wert |no |query |(Keine) |Abgeschlossen |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="createtask"></a>CreateTask
Aufgabe erstellen: Erstellt eine Aufgabe.

```POST: /tasks```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| post | |Ja |body |(Keine) |Neu zu erstellende Aufgabe |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 201 |Erstellt |

## <a name="listsubtasks"></a>ListSubTasks
Teilaufgaben abrufen: Ruft Teilaufgaben aus einer bestimmten Liste oder einer bestimmten Aufgabe ab.

```GET: /subtasks```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list_id |integer |Ja |query |(Keine) |ID der Liste |
| task_id |integer |no |query |(Keine) |ID der Aufgabe |
| abgeschlossen |Boolescher Wert |no |query |(Keine) |Abgeschlossen |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="createsubtask"></a>CreateSubTask
Teilaufgabe erstellen: Erstellt eine Teilaufgabe innerhalb einer bestimmten Aufgabe.

```POST: /subtasks```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| post | |Ja |body |(Keine) |Neu zu erstellende Teilaufgabe |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 201 |Erstellt |

## <a name="listnotes"></a>ListNotes
Notizen abrufen: Ruft Notizen für eine bestimmte Liste oder eine bestimmte Aufgabe ab.

```GET: /notes```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list_id |integer |Ja |query |(Keine) |ID der Liste |
| task_id |integer |no |query |(Keine) |ID der Aufgabe |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="createnote"></a>CreateNote
Notiz erstellen: Fügt einer bestimmten Aufgabe eine Notiz hinzu.

```POST: /notes```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| post | |Ja |body |(Keine) |Neu zu erstellende Notiz |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 201 |Erstellt |

## <a name="listcomments"></a>ListComments
Aufgabenkommentare abrufen: Ruft Aufgabenkommentare für eine bestimmte Liste oder eine bestimmte Aufgabe ab.

```GET: /task_comments```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list_id |integer |Ja |query |(Keine) |ID der Liste |
| task_id |integer |no |query |(Keine) |ID der Aufgabe |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="createcomment"></a>CreateComment
Kommentar zu einer Aufgabe hinzufügen: Fügt einer bestimmten Aufgabe einen Kommentar hinzu.

```POST: /task_comments```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| post | |Ja |body |(Keine) |Neu zu erstellender Aufgabenkommentar |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 201 |Erstellt |

## <a name="retrievereminders"></a>RetrieveReminders
Erinnerungen abrufen: Ruft Erinnerungen für eine bestimmte Liste oder eine bestimmte Aufgabe ab.

```GET: /reminders```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list_id |integer |Ja |query |(Keine) |ID der Liste |
| task_id |integer |no |query |(Keine) |ID der Aufgabe |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="createreminder"></a>CreateReminder
Erinnerung festlegen: Legt eine Erinnerung fest.

```POST: /reminders```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| post | |Ja |body |(Keine) |Neu zu erstellende Erinnerung |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="retrievefiles"></a>RetrieveFiles
Dateien abrufen: Ruft Dateien für eine bestimmte Liste oder eine bestimmte Aufgabe ab.

```GET: /files```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list_id |integer |Ja |query |(Keine) |ID der Liste |
| task_id |integer |no |query |(Keine) |ID der Aufgabe |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |Vorgang erfolgreich |
| 400 |Ungültige Anforderung |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="getlist"></a>GetList
Liste abrufen: Ruft eine bestimmte Liste ab.

```GET: /lists/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |string |Ja |path |(Keine) |ID der Liste |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |

## <a name="deletelist"></a>DeleteList
Liste löschen: Löscht eine Liste.

```DELETE: /lists/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |integer |Ja |path |(Keine) |ID der Liste |
| revision |integer |Ja |query |(Keine) |Version |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 204 |Kein Inhalt |

## <a name="updatelist"></a>UpdateList
Liste aktualisieren: Aktualisiert eine bestimmte Liste.

```PATCH: /lists/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |integer |Ja |path |(Keine) |ID der Liste |
| post | |Ja |body |(Keine) |Listendetails |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |

## <a name="gettask"></a>GetTask
Aufgabe abrufen: Ruft eine bestimmte Aufgabe ab.

```GET: /tasks/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list_id |integer |Ja |query |(Keine) |ID der Liste |
| id |integer |Ja |path |(Keine) |ID der Aufgabe |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |

## <a name="updatetask"></a>UpdateTask
Aufgabe aktualisieren: Aktualisiert eine bestimmte Aufgabe.

```PATCH: /tasks/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list_id |integer |Ja |query |(Keine) |ID der Liste |
| id |integer |Ja |path |(Keine) |ID der Aufgabe |
| post | |Ja |body |(Keine) |Aufgabendetails |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |

## <a name="deletetask"></a>DeleteTask
Aufgabe löschen: Löscht eine bestimmte Aufgabe.

```DELETE: /tasks/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| list_id |integer |Ja |query |(Keine) |ID der Liste |
| id |integer |Ja |path |(Keine) |ID der Aufgabe |
| revision |integer |Ja |query |(Keine) |Version |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 204 |Kein Inhalt |

## <a name="getsubtask"></a>GetSubTask
Teilaufgabe abrufen: Ruft eine bestimmte Teilaufgabe ab.

```GET: /subtasks/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |string |Ja |path |(Keine) |ID der Teilaufgabe |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |

## <a name="updatesubtask"></a>UpdateSubTask
Teilaufgabe aktualisieren: Aktualisiert eine bestimmte Teilaufgabe.

```PATCH: /subtasks/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |integer |Ja |path |(Keine) |ID der Teilaufgabe |
| post | |Ja |body |(Keine) |Details der Teilaufgabe |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |

## <a name="deletesubtask"></a>DeleteSubTask
Teilaufgabe löschen: Löscht eine bestimmte Teilaufgabe.

```DELETE: /subtasks/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |integer |Ja |path |(Keine) |ID der Teilaufgabe |
| revision |integer |Ja |query |(Keine) |Version |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 204 |Kein Inhalt |

## <a name="getnote"></a>GetNote
Notiz abrufen: Ruft eine bestimmte Notiz ab.

```GET: /notes/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |string |Ja |path |(Keine) |ID der Notiz |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |

## <a name="updatenote"></a>UpdateNote
Notiz aktualisieren: Aktualisiert eine bestimmte Notiz.

```PATCH: /notes/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |integer |Ja |path |(Keine) |ID der Notiz |
| post | |Ja |body |(Keine) |Details der Notiz |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |

## <a name="deletenote"></a>DeleteNote
Notiz löschen: Löscht eine bestimmte Notiz.

```DELETE: /notes/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |integer |Ja |path |(Keine) |ID der Notiz |
| revision |integer |Ja |query |(Keine) |Version |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 204 |Kein Inhalt |

## <a name="getcomment"></a>GetComment
Aufgabenkommentar abrufen: Ruft einen bestimmten Aufgabenkommentar ab.

```GET: /task_comments/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |string |Ja |path |(Keine) |ID des Kommentars |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |

## <a name="updatereminder"></a>UpdateReminder
Erinnerung aktualisieren: Aktualisiert eine bestimmte Erinnerung.

```PATCH: /reminders/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |integer |Ja |path |(Keine) |ID der Erinnerung |
| post | |Ja |body |(Keine) |Details der Erinnerung |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |

## <a name="deletereminder"></a>DeleteReminder
Erinnerung löschen: Löscht eine bestimmte Erinnerung.

```DELETE: /reminders/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |integer |Ja |path |(Keine) |ID der Erinnerung |
| revision |integer |Ja |query |(Keine) |Version |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 204 |Kein Inhalt |

## <a name="object-definitions"></a>Objektdefinitionen
### <a name="list"></a>List
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |integer |Nein |
| created_at |string |Nein |
| title |string |Nein |
| list_type |string |Nein |
| Typ |string |Nein |
| revision |integer |Nein |

### <a name="createdlist"></a>CreatedList
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |integer |Nein |
| created_at |string |Nein |
| title |string |Nein |
| revision |integer |Nein |
| Typ |string |Nein |

### <a name="task"></a>Task
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |integer |Nein |
| assignee_id |integer |Nein |
| assigner_id |integer |Nein |
| created_at |string |Nein |
| created_by_id |integer |Nein |
| due_date |string |Nein |
| list_id |integer |Nein |
| revision |integer |Nein |
| starred |Boolescher Wert |Nein |
| title |string |Nein |

### <a name="subtask"></a>Subtask
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |integer |Nein |
| task_id |integer |Nein |
| created_at |string |Nein |
| created_by_id |integer |Nein |
| revision |string |Nein |
| title |string |Nein |

### <a name="note"></a>Note
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |integer |Nein |
| task_id |integer |Nein |
| Inhalt |string |Nein |
| created_at |string |Nein |
| updated_at |string |Nein |
| revision |integer |Nein |

### <a name="comment"></a>Kommentar
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |integer |Nein |
| task_id |integer |Nein |
| revision |integer |Nein |
| Text |string |Nein |
| Typ |string |Nein |
| created_at |string |Nein |

### <a name="reminder"></a>Reminder
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |integer |Nein |
| date |string |Nein |
| task_id |integer |Nein |
| revision |integer |Nein |
| Typ |string |Nein |
| created_at |string |Nein |
| updated_at |string |Nein |

### <a name="createdreminder"></a>CreatedReminder
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |integer |Nein |
| date |string |Nein |
| task_id |integer |Nein |
| revision |integer |Nein |
| created_at |string |Nein |
| updated_at |string |Nein |

### <a name="file"></a>File
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |integer |Nein |
| url |string |Nein |
| task_id |integer |Nein |
| list_id |integer |Nein |
| user_id |integer |Nein |
| file_name |string |Nein |
| content_type |string |Nein |
| file_size |integer |Nein |
| local_created_at |string |Nein |
| created_at |string |Nein |
| updated_at |string |Nein |
| Typ |string |Nein |
| revision |integer |Nein |

### <a name="newtask"></a>NewTask
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| list_id |integer |Ja |
| title |string |Ja |
| assignee_id |integer |Nein |
| abgeschlossen |Boolescher Wert |Nein |
| recurrence_type |string |Nein |
| recurrence_count |integer |Nein |
| due_date |string |Nein |
| starred |Boolescher Wert |Nein |

### <a name="newlist"></a>NewList
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| title |string |Ja |

### <a name="newsubtask"></a>NewSubtask
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| list_id |integer |Ja |
| task_id |integer |Ja |
| title |string |Ja |
| abgeschlossen |Boolescher Wert |Nein |

### <a name="newnote"></a>NewNote
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| list_id |integer |Ja |
| task_id |integer |Ja |
| Inhalt |string |Ja |

### <a name="newcomment"></a>NewComment
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| list_id |integer |Ja |
| task_id |integer |Ja |
| Text |string |Ja |

### <a name="newreminder"></a>NewReminder
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| list_id |integer |Ja |
| task_id |integer |Ja |
| date |string |Ja |

### <a name="updatetask"></a>UpdateTask
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| revision |integer |Nein |
| title |string |Nein |
| assignee_id |integer |Nein |
| abgeschlossen |Boolescher Wert |Nein |
| recurrence_type |string |Nein |
| recurrence_count |integer |Nein |
| due_date |string |Nein |
| starred |Boolescher Wert |Nein |

### <a name="updatelist"></a>UpdateList
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| revision |integer |Nein |
| title |string |Nein |

### <a name="updatesubtask"></a>UpdateSubtask
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| revision |integer |Nein |
| title |string |Nein |
| abgeschlossen |Boolescher Wert |Nein |

### <a name="updatenote"></a>UpdateNote
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| revision |integer |Nein |
| Inhalt |string |Nein |

### <a name="updatereminder"></a>UpdateReminder
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| date |string |Nein |
| revision |integer |Nein |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md)




<!--HONumber=Nov16_HO3-->


