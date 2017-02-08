---
title: GitHub | Microsoft Docs
description: "Erstellen Sie Logik-Apps mit Azure App Service. GitHub ist ein webbasierter Hostingdienst für Git-Repositorys. Er bietet alle Funktionen von Git zur Kontrolle von verteilten Überarbeitungen und zur Quellcodeverwaltung sowie eigene Features."
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: 8f873e6c-f4c0-4c2e-a5bd-2e953efe5e2b
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: b92f954680603891ced503a1134791312b5214f0
ms.openlocfilehash: 6f336db8c8719b99420b353dca82e36a0d837769


---
# <a name="get-started-with-the-github-connector"></a>Erste Schritte mit dem GitHub-Connector
GitHub ist ein webbasierter Hostingdienst für Git-Repositorys. Er bietet alle Funktionen von Git zur Kontrolle von verteilten Überarbeitungen und zur Quellcodeverwaltung sowie eigene Features.

> [!NOTE]
> Diese Version des Artikels gilt für Logik-Apps mit der Schemaversion „2015-08-01-preview“. 
> 
> 

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md) beschrieben.

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Der GitHub-Connector kann als Aktion verwendet werden. Er verfügt über Trigger. Alle Connectors unterstützen Daten im JSON- und XML-Format. 

 Der GitHub-Connector verfügt über die folgenden Aktionen und/oder Trigger:

### <a name="github-actions"></a>GitHub-Aktionen
Sie können diese Aktionen ausführen:

| Aktion | Beschreibung |
| --- | --- |
| [CreateIssue](connectors-create-api-github.md#createissue) |Erstellt eine Problemmeldung |

### <a name="github-triggers"></a>GitHub-Trigger
Sie können auf diese Ereignisse lauschen:

| Trigger | Beschreibung |
| --- | --- |
| Wenn eine Problemmeldung geöffnet wird |Eine Problemmeldung wird geöffnet. |
| Wenn eine Problemmeldung geschlossen wird |Eine Problemmeldung wird geschlossen. |
| Wenn eine Problemmeldung zugewiesen wird |Eine Problemmeldung wird zugewiesen. |

## <a name="create-a-connection-to-github"></a>Herstellen einer Verbindung mit GitHub
Um Logik-Apps mit GitHub zu erstellen, müssen Sie zuerst eine **Verbindung** erstellen und anschließend die Details für die folgenden Eigenschaften angeben: 

| Eigenschaft | Erforderlich | Beschreibung |
| --- | --- | --- |
| Tokenverschlüsselung |Ja |Angeben der Anmeldeinformationen für GitHub |

Nachdem Sie die Verbindung erstellt haben, können Sie sie zum Ausführen der Aktionen und zum Lauschen auf die in diesem Artikel beschriebenen Trigger verwenden. 

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 
> [!TIP]
> Sie können diese Verbindung in anderen Logik-Apps verwenden.
> 
> 

## <a name="reference-for-github"></a>Referenz für GitHub
Gilt für Version: 1.0.

## <a name="createissue"></a>CreateIssue
Problemmeldung erstellen: Erstellt eine Problemmeldung. 

```POST: /repos/{repositoryOwner}/{repositoryName}/issues``` 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| repositoryOwner |string |Ja |path |(Keine) |Eigentümer des Repositorys |
| repositoryName |string |Ja |path |(Keine) |Name des Repositorys |
| issueBasicDetails | |Ja |body |(Keine) |Details zum Problem |

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

## <a name="issueopened"></a>IssueOpened
Wenn eine Problemmeldung geöffnet wird: Eine Problemmeldung wird geöffnet. 

```GET: /trigger/issueOpened``` 

Es gibt keine Parameter für diesen Aufruf

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

## <a name="issueclosed"></a>IssueClosed
Wenn eine Problemmeldung geschlossen wird: Eine Problemmeldung wird geschlossen. 

```GET: /trigger/issueClosed``` 

Es gibt keine Parameter für diesen Aufruf

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

## <a name="issueassigned"></a>IssueAssigned
Wenn eine Problemmeldung zugewiesen wird: Eine Problemmeldung wird zugewiesen. 

```GET: /trigger/issueAssigned``` 

Es gibt keine Parameter für diesen Aufruf

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
### <a name="issuebasicdetailsmodel"></a>IssueBasicDetailsModel
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| title |string |Ja |
| body |string |Ja |
| assignee |string |Ja |

### <a name="issuedetailsmodel"></a>IssueDetailsModel
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| title |string |Ja |
| body |string |Ja |
| assignee |string |Ja |
| number |string |Nein |
| state |string |Nein |
| created_at |string |Nein |
| repository_url |string |Nein |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md)




<!--HONumber=Jan17_HO3-->


