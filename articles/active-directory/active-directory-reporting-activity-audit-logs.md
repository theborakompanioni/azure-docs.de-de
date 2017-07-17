---
title: "Berichte zu Überwachungsaktivitäten im Azure Active Directory-Portal | Microsoft-Dokumentation"
description: "Enthält eine Einführung in die Berichte zu Überwachungsaktivitäten im Azure Active Directory-Portal."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: d8c49272789e7d33c6f0684875765a1ecea5a2ff
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017

---
# Berichte zu Überwachungsaktivitäten im Azure Active Directory-Portal
<a id="audit-activity-reports-in-the-azure-active-directory-portal" class="xliff"></a> 

Mit der Berichterstellungsfunktion in Azure Active Directory (Azure AD) können Sie alle Informationen abrufen, die Sie zum Ermitteln des Zustands Ihrer Umgebung benötigen.

Die Architektur für die Berichterstellung in Azure AD umfasst die folgenden Komponenten:

- **Aktivität** 
    - **Anmeldeaktivitäten** : Informationen zur Nutzung von verwalteten Anwendungen und Aktivitäten der Benutzeranmeldung
    - **Überwachungsprotokolle:** Systemaktivitätsinformationen zu Benutzern und zur Gruppenverwaltung sowie zu verwalteten Anwendungen und Verzeichnisaktivitäten.
- **Sicherheit** 
    - **Riskante Anmeldungen:** Eine riskante Anmeldung ist ein Indikator für einen Anmeldeversuch von einem Benutzer, der nicht der rechtmäßige Besitzer eines Benutzerkontos ist. Weitere Informationen finden Sie unter „Riskante Anmeldungen“.
    - **Benutzer mit Risikomarkierung:** Ein Benutzer mit Risikomarkierung ist ein Indikator für ein möglicherweise kompromittiertes Benutzerkonto. Weitere Informationen finden Sie unter „Benutzer mit Risikomarkierung“.

In diesem Thema erhalten Sie einen Überblick über die Überwachungsaktivitäten.
 
## Wer kann auf die Daten zugreifen?
<a id="who-can-access-the-data" class="xliff"></a>
* Benutzer mit den Rollen „Sicherheitsadministrator“ oder der Berechtigung „Sicherheit lesen“
* Globale Administratoren
* Einzelne Benutzer (Nicht-Administratoren) können eigene Aktivitäten anzeigen.


## Überwachungsprotokolle
<a id="audit-logs" class="xliff"></a>

Die Überwachungsprotokolle in Azure Active Directory enthalten Datensätze mit Systemaktivitäten, die zum Nachweisen der Konformität verwendet werden können.  
Ihr erster Einstiegspunkt für alle Überwachungsdaten ist die Option **Überwachungsprotokolle** im Abschnitt **Aktivität** von **Azure Active Directory**.

![Überwachungsprotokolle](./media/active-directory-reporting-activity-audit-logs/61.png "Überwachungsprotokolle")

Ein Überwachungsprotokoll enthält eine Standardlistenansicht mit folgenden Informationen:

- Datum und Uhrzeit des Auftretens
- Initiator/Akteur einer Aktivität (*Wer*) 
- Aktivität (*Was*) 
- Ziel

![Überwachungsprotokolle](./media/active-directory-reporting-activity-audit-logs/18.png "Überwachungsprotokolle")

Sie können die Listenansicht anpassen, indem Sie auf der Symbolleiste auf **Spalten** klicken.

![Überwachungsprotokolle](./media/active-directory-reporting-activity-audit-logs/19.png "Überwachungsprotokolle")

So können Sie weitere Felder anzeigen oder bereits angezeigte Felder entfernen.

![Überwachungsprotokolle](./media/active-directory-reporting-activity-audit-logs/21.png "Überwachungsprotokolle")


Wenn Sie in der Listenansicht auf einen Eintrag klicken, werden die dazu verfügbaren Details angezeigt.

![Überwachungsprotokolle](./media/active-directory-reporting-activity-audit-logs/22.png "Überwachungsprotokolle")


## Filtern von Überwachungsprotokollen
<a id="filtering-audit-logs" class="xliff"></a>

Sie können die Überwachungsdaten mit den folgenden Feldern filtern, um die gemeldeten Daten gemäß Ihren Bedürfnissen einzugrenzen:

- Datumsbereich
- Initiiert von (Akteur)
- Kategorie
- Aktivitätsressourcentyp
- Aktivität

![Überwachungsprotokolle](./media/active-directory-reporting-activity-audit-logs/23.png "Überwachungsprotokolle")


Mit dem Filter **Datumsbereich** können Sie einen Zeitrahmen für die zurückgegebenen Daten festlegen.  
Mögliche Werte:

- 1 Monat
- 7 Tage
- 24 Stunden
- Benutzerdefiniert

Wenn Sie einen benutzerdefinierten Zeitraum auswählen, können Sie eine Startzeit und eine Endzeit konfigurieren.

Bei Verwendung des Filters **Initiiert von** können Sie den Namen eines Akteurs oder dessen Benutzerprinzipalnamen (User Principal Name, UPN) definieren.

Bei Verwendung des Filters **Kategorie** können Sie eine der folgenden Filteroptionen auswählen:

- Alle
- Core category (Kernkategorie)
- Core directory (Kernverzeichnis)
- Self-service password management (Self-Service-Kennwortverwaltung)
- Self-Service-Gruppenverwaltung
- Kontobereitstellung: Automated password rollover (Automatisiertes Kennwortrollover)
- Invited Users (Eingeladene Benutzer)
- MIM service (MIM-Dienst)
- Schutz der Identität (Identity Protection)
- B2C

Bei Verwendung des Filters **Aktivitätsressourcentyp** können Sie eine der folgenden Filteroptionen auswählen:

- Alle 
- Gruppe
- Verzeichnis
- Benutzer
- Anwendung
- Richtlinie
- Gerät
- Andere

Wenn Sie als **Aktivitätsressourcentyp** die Option **Gruppe** auswählen, erhalten Sie eine zusätzliche Filterkategorie zur Angabe einer **Quelle**:

- Azure AD
- O365


Der Filter **Aktivität** basiert auf der getroffenen Auswahl für „Kategorie“ und „Aktivitätsressourcentyp“. Sie können entweder eine bestimmte Aktivität verwenden oder alle auswählen. 

Sie können die Liste aller Überwachungsaktivitäten mit der Graph-API https://graph.windows.net/$tenantdomain/activities/auditActivityTypes?api-version=beta abrufen. Dabei ist „$tenantdomain“ Ihr Domänenname. Alternativ können Sie die Informationen im Artikel [Azure Active Directory-Überwachungsberichtsereignisse](active-directory-reporting-audit-events.md#list-of-audit-report-events) lesen.


## Verknüpfungen für Überwachungsprotokolle
<a id="audit-logs-shortcuts" class="xliff"></a>

Neben **Azure Active Directory** bietet das Azure-Portal noch zwei weitere Einstiegspunkte für Überwachungsdaten:

- Benutzer und Gruppen
- Unternehmensanwendungen

### Überwachungsprotokolle für Benutzer und Gruppen
<a id="users-and-groups-audit-logs" class="xliff"></a>

Mit Überwachungsberichten, die auf Benutzern und Gruppen basieren, können Sie beispielsweise Antworten auf folgende Fragen erhalten:

- Welche Arten von Updates wurden von den Benutzern angewendet?

- Wie viele Benutzer wurden geändert?

- Wie viele Kennwörter wurden geändert?

- Welche Schritte hat ein Administrator in einem Verzeichnis ausgeführt?

- Welche Gruppen wurden hinzugefügt?

- Sind Gruppen mit Änderungen der Mitgliedschaft vorhanden?

- Haben sich die Besitzer der Gruppe geändert?

- Welche Lizenzen wurden einer Gruppe oder einem Benutzer zugewiesen?

Wenn Sie nur Überwachungsdaten überprüfen möchten, die sich auf Benutzer und Gruppen beziehen, können Sie die gefilterte Ansicht unter **Überwachungsprotokolle** im Abschnitt **Aktivität** der Option **Benutzer und Gruppen** verwenden. Bei diesem Einstiegspunkt ist für **Aktivitätsressourcentyp** bereits vorab die Option **Benutzer und Gruppen** ausgewählt.

![Überwachungsprotokolle](./media/active-directory-reporting-activity-audit-logs/93.png "Überwachungsprotokolle")

### Überwachungsprotokolle für Unternehmensanwendungen
<a id="enterprise-applications-audit-logs" class="xliff"></a>

Mit Überwachungsberichten, die auf Anwendungen basieren, können Sie beispielsweise Antworten auf folgende Fragen erhalten:

* Welche Anwendungen wurden hinzugefügt oder aktualisiert?
* Welche Anwendungen wurden entfernt?
* Hat sich ein Dienstprinzip für eine Anwendung geändert?
* Haben sich die Namen von Anwendungen geändert?
* Wer hat die Zustimmung zu einer Anwendung erteilt?

Wenn Sie nur Überwachungsdaten überprüfen möchten, die sich auf Ihre Anwendungen beziehen, können Sie die gefilterte Ansicht unter **Überwachungsprotokolle** im Abschnitt **Aktivität** des Blatts **Unternehmensanwendungen** verwenden. Bei diesem Einstiegspunkt ist für **Aktivitätsressourcentyp** bereits vorab die Option **Unternehmensanwendungen** ausgewählt.

![Überwachungsprotokolle](./media/active-directory-reporting-activity-audit-logs/134.png "Überwachungsprotokolle")

Dieser Ansicht kann weiter nach **Gruppen** oder **Benutzer** gefiltert werden.

![Überwachungsprotokolle](./media/active-directory-reporting-activity-audit-logs/25.png "Überwachungsprotokolle")


## Nächste Schritte
<a id="next-steps" class="xliff"></a>
Weitere Informationen finden Sie unter [Anleitung für Azure Active Directory-Berichte](active-directory-reporting-guide.md).


