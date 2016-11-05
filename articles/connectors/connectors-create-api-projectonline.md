---
title: ProjectOnline | Microsoft Docs
description: Erstellen Sie Logik-Apps mit Azure App Service. Project Online ist eine flexible Onlinelösung von Microsoft für das für Projektportfoliomanagement (PPM) und die tägliche Arbeit. Project Online wird über Office 365 bereitgestellt und verschafft Organisationen einen schnellen Zugang zu leistungsfähigen Projektmanagementfunktionen. So können sie kurzfristig Projekte und Investitionen in Projektportfolios planen, priorisieren und steuern – an fast jedem Ort und auf nahezu jedem Gerät.
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
# Erste Schritte mit dem Project Online-Connector
Project Online ist eine flexible Onlinelösung von Microsoft für das für Projektportfoliomanagement (PPM) und die tägliche Arbeit. Project Online wird über Office 365 bereitgestellt und verschafft Organisationen einen schnellen Zugang zu leistungsfähigen Projektmanagementfunktionen. So können sie kurzfristig Projekte und Investitionen in Projektportfolios planen, priorisieren und steuern – an fast jedem Ort und auf nahezu jedem Gerät.

> [!NOTE]
> Diese Version des Artikels gilt für die Schemaversion 2015-08-01-preview für Logik-Apps.
> 
> 

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md) beschrieben.

## Trigger und Aktionen
Der Project Online-Connector kann als Aktion verwendet werden. Er verfügt über Trigger. Alle Connectors unterstützen Daten im JSON- und XML-Format.

 Der Project Online-Connector verfügt über die folgenden Aktionen und/oder Trigger:

### Aktionen für Project Online
Sie können diese Aktionen ausführen:

| Aktion | Beschreibung |
| --- | --- |
| [ListProjects](connectors-create-api-projectonline.md#listprojects) |Listet die Projekte auf Ihrer Projekt Online-Website auf. |
| [CreateProject](connectors-create-api-projectonline.md#createproject) |Erstellt ein neues Projekt auf Ihrer Projekt Online-Website. |
| [CreateTask](connectors-create-api-projectonline.md#createtask) |Erstellt eine neue Aufgabe in Ihrem Projekt. |
| [CreateResource](connectors-create-api-projectonline.md#createresource) |Erstellt eine Enterprise-Ressource auf Ihrer Project Online-Website. |
| [ListTasks](connectors-create-api-projectonline.md#listtasks) |Listet die veröffentlichten Aufgaben in einem Projekt auf. |
| [CheckoutProject](connectors-create-api-projectonline.md#checkoutproject) |Checkt ein Projekt auf Ihrer Website aus. |
| [PublishProject](connectors-create-api-projectonline.md#publishproject) |Checkt ein vorhandenes Projekt auf Ihrer Website ein und veröffentlicht es. |

### Trigger für Project Online
Sie können auf diese Ereignisse lauschen:

| Trigger | Beschreibung |
| --- | --- |
| Wenn ein neues Projekt erstellt wird |Löst einen Workflow aus, sobald ein neues Projekt erstellt wird. |
| Wenn eine neue Ressource erstellt wird |Löst einen neuen Workflow aus, sobald eine neue Ressource erstellt wird. |
| Wenn eine neue Aufgabe erstellt wird |Löst einen Workflow aus, sobald eine neue Aufgabe erstellt wird. |

## Herstellen einer Verbindung mit Project Online
Um Logik-Apps mit Project Online zu erstellen, müssen Sie zuerst eine **Verbindung** erstellen und anschließend die Details für die folgenden Eigenschaften angeben:

| Eigenschaft | Erforderlich | Beschreibung |
| --- | --- | --- |
| Token |Ja |Angeben der Anmeldeinformationen für Project Online |

> [!INCLUDE [Schritte zum Herstellen einer Verbindung mit Project Online](../../includes/connectors-create-api-projectonline.md)]
> 
> [!TIP]
> Sie können diese Verbindung in anderen Logik-Apps verwenden.
> 
> 

## Referenz für Project Online
Gilt für Version 1.0.

## OnNewProject
Wenn ein neues Projekt erstellt wird: Löst einen Workflow aus, sobald ein neues Projekt erstellt wird.

```GET: /trigger/_api/ProjectData/Projects```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |Ja |query |(Keine) |Stamm-URL Ihrer Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam) |

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

## OnNewResource
Wenn eine neue Ressource erstellt wird: Löst einen neuen Workflow aus, sobald eine neue Ressource erstellt wird.

```GET: /trigger/_api/ProjectData/Resources```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |Ja |query |(Keine) |Stamm-URL Ihrer Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam) |

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

## OnNewTask
Wenn eine neue Aufgabe erstellt wird: Löst einen Workflow aus, sobald eine neue Aufgabe erstellt wird.

```GET: /trigger/_api/ProjectData/Tasks```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |Ja |query |(Keine) |Stamm-URL Ihrer Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam) |

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

## ListProjects
Projekte auflisten: Listet die Projekte auf Ihrer Project Online-Website auf.

```GET: /_api/ProjectServer/Projects```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |Ja |query |(Keine) |Stamm-URL Ihrer Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam) |

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

## CreateProject
Neues Projekt erstellen: Erstellt ein neues Projekt auf Ihrer Project Online-Website.

```POST: /_api/ProjectServer/Projects```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |Ja |query |(Keine) |Stamm-URL Ihrer Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam) |
| proj | |Ja |body |(Keine) |Neu zu erstellendes Projekt |

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

## CreateTask
Neue Aufgabe erstellen: Erstellt eine neue Aufgabe in Ihrem Projekt.

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Tasks/Add```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |Ja |query |(Keine) |Stamm-URL Ihrer Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam) |
| project\_id |string |Ja |path |(Keine) |Eindeutige ID des Projekts, dem die Aufgabe hinzugefügt werden soll |
| task | |Ja |body |(Keine) |Neue Aufgabe, die dem Projekt hinzugefügt werden soll |

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

## CreateResource
Neue Ressource erstellen: Erstellt eine Enterprise-Ressource auf Ihrer Project Online-Website.

```POST: /_api/ProjectServer/EnterpriseResources```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |Ja |query |(Keine) |Stamm-URL Ihrer Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam) |
| Ressource | |Ja |body |(Keine) |Neue Enterprise-Ressource, die dem Projekt hinzugefügt werden soll |

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

## ListTasks
Aufgaben auflisten: Listet die veröffentlichten Aufgaben in einem Projekt auf.

```GET: /_api/ProjectServer/Projects('{project_id}')/Tasks```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |Ja |query |(Keine) |Stamm-URL Ihrer Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam) |
| project\_id |string |Ja |path |(Keine) |Eindeutige ID des Projekts, aus dem die Aufgaben abgerufen werden sollen |

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

## CheckoutProject
Ein Projekt auschecken: Checkt ein Projekt auf Ihrer Website aus.

```POST: /_api/ProjectServer/Projects('{project_id}')/checkOut```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |Ja |query |(Keine) |Stamm-URL Ihrer Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam) |
| project\_id |string |Ja |path |(Keine) |Eindeutige ID des Projekts, dem die Aufgabe hinzugefügt werden soll |

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

## PublishProject
Projekt einchecken und veröffentlichen: Checkt ein vorhandenes Projekt auf Ihrer Website ein und veröffentlicht es.

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Publish(true)```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |Ja |query |(Keine) |Stamm-URL Ihrer Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam) |
| project\_id |string |Ja |path |(Keine) |Eindeutige ID des Projekts, das eingecheckt werden soll |

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
### TriggerProjectsWrapper
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| value |array |Nein |

### TriggerProject
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| ProjectStartDate |string |Nein |
| ProjectFinishDate |string |Nein |
| ProjectCreatedDate |string |Nein |
| ProjectId |string |Nein |
| ProjectModifiedDate |string |Nein |
| ProjectType |integer |Nein |
| ProjectName |string |Nein |

### TriggerResourcesWrapper
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| value |array |Nein |

### TriggerResource
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| ResourceId |string |Nein |
| ResourceBaseCalendar |string |Nein |
| ResourceBookingType |integer |Nein |
| ResourceCanLevel |Boolescher Wert |Nein |
| ResourceCostPerUse |number |Nein |
| ResourceCreatedDate |string |Nein |
| ResourceEarliestAvailableFrom |string |Nein |
| ResourceEmail |string |Nein |
| ResourceInitials |string |Nein |
| ResourceIsActive |Boolescher Wert |Nein |
| ResourceIsGeneric |Boolescher Wert |Nein |
| ResourceLatestAvailableTo |string |Nein |
| ResourceModifiedDate |string |Nein |
| ResourceName |string |Nein |
| ResourceStatsuName |string |Nein |
| ResourceType |integer |Nein |
| TypeDescription |string |Nein |
| TypeName |string |Nein |

### TriggerTasksWrapper
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| value |array |Nein |

### TriggerTask
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| ProjectId |string |Nein |
| TaskId |string |Nein |
| ProjectName |string |Nein |
| TaskName |string |Nein |
| TaskCreatedDate |string |Nein |
| TaskModifieddate |string |Nein |
| TaskStartDate |string |Nein |
| TaskFinishDate |string |Nein |
| TaskPriority |integer |Nein |
| TaskIsActive |Boolescher Wert |Nein |

### NewProject
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Name |string |Ja |
| Description |string |Nein |
| Start |string |Nein |

### NewReource
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Name |string |Ja |
| IsBudget |Boolescher Wert |Nein |
| IsGeneric |Boolescher Wert |Nein |
| IsInactive |Boolescher Wert |Nein |

### Project
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| ApprovedStart |string |Nein |
| ApprovedEnd |string |Nein |
| CheckedOutDate |string |Nein |
| CheckOutDescription |string |Nein |
| CheckOutId |string |Nein |
| CreatedDate |string |Nein |
| ID |string |Nein |
| IsCheckedOut |Boolescher Wert |Nein |
| LastPublishedDate |string |Nein |
| LastSavedDate |string |Nein |
| OptimizerDecision |integer |Nein |
| PlannerDecision |integer |Nein |
| ProjectType |integer |Nein |
| Name |string |Nein |
| WinprojVersion |string |Nein |

### ProjectsWrapper
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| value |array |Nein |

### NewTask
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| parameters |nicht definiert |Ja |

### TaskParameters
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Name |string |Ja |
| Hinweise |string |Nein |
| Start |string |Nein |
| Duration |string |Nein |

### EnterpriseResource
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| CanLevel |Boolescher Wert |Nein |
| Code |string |Nein |
| CostAccrual |integer |Nein |
| CostCenter |string |Nein |
| Erstellt |string |Nein |
| DefaultBookingType |integer |Nein |
| Email |string |Nein |
| ExternalId |string |Nein |
| Group |string |Nein |
| HireDate |string |Nein |
| ID |string |Nein |
| Initials |string |Nein |
| IsActive |Boolescher Wert |Nein |
| IsBudget |Boolescher Wert |Nein |
| IsCheckedOut |Boolescher Wert |Nein |
| IsGeneric |Boolescher Wert |Nein |
| IsTeam |Boolescher Wert |Nein |
| MaterialLabel |string |Nein |
| Modified |string |Nein |
| Name |string |Nein |
| Phonetics |string |Nein |
| ResourceType |integer |Nein |
| TerminationDate |string |Nein |

### TasksWrapper
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| value |array |Nein |

### Task
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Created |string |Nein |
| Modified |string |Nein |
| Start |string |Nein |
| Finish |string |Nein |
| Name |string |Nein |
| ID |string |Nein |
| Priority |integer |Nein |
| PercentComplete |integer |Nein |
| Notes |string |Nein |
| Contact |string |Nein |

## Nächste Schritte
[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md)

<!---HONumber=AcomDC_0824_2016-->