---
title: ProjectOnline | Microsoft Docs
description: "Erstellen Sie Logik-Apps mit Azure App Service. Project Online ist eine flexible Onlinelösung von Microsoft für das für Projektportfoliomanagement (PPM) und die tägliche Arbeit. Project Online wird über Office 365 bereitgestellt und verschafft Organisationen einen schnellen Zugang zu leistungsfähigen Projektmanagementfunktionen. So können sie kurzfristig Projekte und Investitionen in Projektportfolios planen, priorisieren und steuern – an fast jedem Ort und auf nahezu jedem Gerät."
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: 40ce621e-4925-4653-93bb-71ab9abcbdf1
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 2e30909f2917fc01f6066d8489961ca94f34f429


---
# <a name="get-started-with-the-projectonline-connector"></a>Erste Schritte mit dem Project Online-Connector
Project Online ist eine flexible Onlinelösung von Microsoft für das für Projektportfoliomanagement (PPM) und die tägliche Arbeit. Project Online wird über Office 365 bereitgestellt und verschafft Organisationen einen schnellen Zugang zu leistungsfähigen Projektmanagementfunktionen. So können sie kurzfristig Projekte und Investitionen in Projektportfolios planen, priorisieren und steuern – an fast jedem Ort und auf nahezu jedem Gerät.

> [!NOTE]
> Diese Version des Artikels gilt für Logik-Apps mit der Schemaversion „2015-08-01-preview“. 
> 
> 

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md) beschrieben.

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Der Project Online-Connector kann als Aktion verwendet werden. Er verfügt über Trigger. Alle Connectors unterstützen Daten im JSON- und XML-Format. 

 Der Project Online-Connector verfügt über die folgenden Aktionen und/oder Trigger:

### <a name="projectonline-actions"></a>Aktionen für Project Online
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

### <a name="projectonline-triggers"></a>Trigger für Project Online
Sie können auf diese Ereignisse lauschen:

| Trigger | Beschreibung |
| --- | --- |
| Wenn ein neues Projekt erstellt wird |Löst einen Workflow aus, sobald ein neues Projekt erstellt wird. |
| Wenn eine neue Ressource erstellt wird |Löst einen neuen Workflow aus, sobald eine neue Ressource erstellt wird. |
| Wenn eine neue Aufgabe erstellt wird |Löst einen Workflow aus, sobald eine neue Aufgabe erstellt wird. |

## <a name="create-a-connection-to-projectonline"></a>Herstellen einer Verbindung mit Project Online
Um Logik-Apps mit Project Online zu erstellen, müssen Sie zuerst eine **Verbindung** erstellen und anschließend die Details für die folgenden Eigenschaften angeben: 

| Eigenschaft | Erforderlich | Beschreibung |
| --- | --- | --- |
| Tokenverschlüsselung |Ja |Angeben der Anmeldeinformationen für Project Online |

> [!INCLUDE [Steps to create a connection to ProjectOnline](../../includes/connectors-create-api-projectonline.md)]
> 
> [!TIP]
> Sie können diese Verbindung in anderen Logik-Apps verwenden.
> 
> 

## <a name="reference-for-projectonline"></a>Referenz für Project Online
Gilt für Version 1.0.

## <a name="onnewproject"></a>OnNewProject
Wenn ein neues Projekt erstellt wird: Löst einen Workflow aus, sobald ein neues Projekt erstellt wird. 

```GET: /trigger/_api/ProjectData/Projects``` 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |Ja |query |(Keine) |Stammwebsite-URL Ihrer Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam) |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="onnewresource"></a>OnNewResource
Wenn eine neue Ressource erstellt wird: Löst einen neuen Workflow aus, sobald eine neue Ressource erstellt wird. 

```GET: /trigger/_api/ProjectData/Resources``` 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |Ja |query |(Keine) |Stammwebsite-URL Ihrer Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam) |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="onnewtask"></a>OnNewTask
Wenn eine neue Aufgabe erstellt wird: Löst einen Workflow aus, sobald eine neue Aufgabe erstellt wird. 

```GET: /trigger/_api/ProjectData/Tasks``` 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |Ja |query |(Keine) |Stammwebsite-URL Ihrer Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam) |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="listprojects"></a>ListProjects
Projekte auflisten: Listet die Projekte auf Ihrer Project Online-Website auf. 

```GET: /_api/ProjectServer/Projects``` 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |Ja |query |(Keine) |Stammwebsite-URL Ihrer Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam) |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="createproject"></a>CreateProject
Neues Projekt erstellen: Erstellt ein neues Projekt auf Ihrer Project Online-Website. 

```POST: /_api/ProjectServer/Projects``` 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |Ja |query |(Keine) |Stammwebsite-URL Ihrer Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam) |
| proj | |Ja |body |(Keine) |Neu zu erstellendes Projekt |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="createtask"></a>CreateTask
Neue Aufgabe erstellen: Erstellt eine neue Aufgabe in Ihrem Projekt. 

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Tasks/Add``` 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |Ja |query |(Keine) |Stammwebsite-URL Ihrer Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam) |
| project_id |string |Ja |path |(Keine) |Eindeutige ID des Projekts, dem die Aufgabe hinzugefügt werden soll |
| task | |Ja |body |(Keine) |Neue Aufgabe, die dem Projekt hinzugefügt werden soll |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="createresource"></a>CreateResource
Neue Ressource erstellen: Erstellt eine Enterprise-Ressource auf Ihrer Project Online-Website. 

```POST: /_api/ProjectServer/EnterpriseResources``` 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |Ja |query |(Keine) |Stammwebsite-URL Ihrer Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam) |
| Ressource | |Ja |body |(Keine) |Neue Enterprise-Ressource, die dem Projekt hinzugefügt werden soll |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="listtasks"></a>ListTasks
Aufgaben auflisten: Listet die veröffentlichten Aufgaben in einem Projekt auf. 

```GET: /_api/ProjectServer/Projects('{project_id}')/Tasks``` 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |Ja |query |(Keine) |Stammwebsite-URL Ihrer Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam) |
| project_id |string |Ja |path |(Keine) |Eindeutige ID des Projekts, aus dem die Aufgaben abgerufen werden sollen |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="checkoutproject"></a>CheckoutProject
Ein Projekt auschecken: Checkt ein Projekt auf Ihrer Website aus. 

```POST: /_api/ProjectServer/Projects('{project_id}')/checkOut``` 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |Ja |query |(Keine) |Stammwebsite-URL Ihrer Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam) |
| project_id |string |Ja |path |(Keine) |Eindeutige ID des Projekts, dem die Aufgabe hinzugefügt werden soll |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="publishproject"></a>PublishProject
Projekt einchecken und veröffentlichen: Checkt ein vorhandenes Projekt auf Ihrer Website ein und veröffentlicht es. 

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Publish(true)``` 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |Ja |query |(Keine) |Stammwebsite-URL Ihrer Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam) |
| project_id |string |Ja |path |(Keine) |Eindeutige ID des Projekts, das eingecheckt werden soll |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="object-definitions"></a>Objektdefinitionen
### <a name="triggerprojectswrapper"></a>TriggerProjectsWrapper
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| value |array |Nein |

### <a name="triggerproject"></a>TriggerProject
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| ProjectStartDate |string |Nein |
| ProjectFinishDate |string |Nein |
| ProjectCreatedDate |string |Nein |
| ProjectId |string |Nein |
| ProjectModifiedDate |string |Nein |
| ProjectType |integer |Nein |
| ProjectName |string |Nein |

### <a name="triggerresourceswrapper"></a>TriggerResourcesWrapper
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| value |array |Nein |

### <a name="triggerresource"></a>TriggerResource
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

### <a name="triggertaskswrapper"></a>TriggerTasksWrapper
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| value |array |Nein |

### <a name="triggertask"></a>TriggerTask
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

### <a name="newproject"></a>NewProject
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Name |string |Ja |
| Description |string |Nein |
| Start |string |Nein |

### <a name="newreource"></a>NewReource
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Name |string |Ja |
| IsBudget |Boolescher Wert |Nein |
| IsGeneric |Boolescher Wert |Nein |
| IsInactive |Boolescher Wert |Nein |

### <a name="project"></a>Project
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

### <a name="projectswrapper"></a>ProjectsWrapper
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| value |array |Nein |

### <a name="newtask"></a>NewTask
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| parameters |nicht definiert |Ja |

### <a name="taskparameters"></a>TaskParameters
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Name |string |Ja |
| Hinweise |string |Nein |
| Start |string |Nein |
| Duration |string |Nein |

### <a name="enterpriseresource"></a>EnterpriseResource
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

### <a name="taskswrapper"></a>TasksWrapper
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| value |array |Nein |

### <a name="task"></a>Task
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

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md)




<!--HONumber=Nov16_HO3-->


