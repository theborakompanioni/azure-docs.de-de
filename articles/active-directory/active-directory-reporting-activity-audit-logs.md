---
title: "Berichte zu Überwachungsaktivitäten im Azure Active Directory-Portal – Vorschau | Microsoft-Dokumentation"
description: "Enthält eine Einführung in die Berichte zu Überwachungsaktivitäten im Azure Active Directory-Portal (Vorschau)."
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
ms.date: 04/07/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: db7cb109a0131beee9beae4958232e1ec5a1d730
ms.openlocfilehash: d88f8bed0cbd14ee49986d6749396731a810034b
ms.lasthandoff: 04/18/2017


---
# <a name="audit-activity-reports-in-the-azure-active-directory-portal---preview"></a>Berichte zu Überwachungsaktivitäten im Azure Active Directory-Portal – Vorschau

Mit der Berichterstellungsfunktion in Azure Active Directory [(Vorschau)](active-directory-preview-explainer.md) erhalten Sie alle Informationen, die Sie zum Ermitteln des Zustands Ihrer Umgebung benötigen.

Die Architektur für die Berichterstellung in Azure Active Directory umfasst die folgenden Komponenten:

- **Aktivität** 
    - **Anmeldeaktivitäten** : Informationen zur Nutzung von verwalteten Anwendungen und Aktivitäten der Benutzeranmeldung
    - **Überwachungsprotokolle:** Systemaktivitätsinformationen zu Benutzern und zur Gruppenverwaltung sowie zu verwalteten Anwendungen und Verzeichnisaktivitäten.
- **Sicherheit** 
    - **Riskante Anmeldungen:** Eine riskante Anmeldung ist ein Indikator für einen Anmeldeversuch von einem Benutzer, der nicht der rechtmäßige Besitzer eines Benutzerkontos ist. Weitere Informationen finden Sie unter „Riskante Anmeldungen“.
    - **Benutzer mit Risikomarkierung:** Ein Benutzer mit Risikomarkierung ist ein Indikator für ein möglicherweise kompromittiertes Benutzerkonto. Weitere Informationen finden Sie unter „Benutzer mit Risikomarkierung“.

In diesem Thema erhalten Sie einen Überblick über die Überwachungsaktivitäten.
 


## <a name="audit-logs"></a>Überwachungsprotokolle

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


## <a name="filtering-audit-logs"></a>Filtern von Überwachungsprotokollen

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
- Kontobereitstellung
- Automated password rollover (Automatisiertes Kennwortrollover)
- Invited users (Eingeladene Benutzer)
- MIM service (MIM-Dienst)

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

| Aktivitätskategorie| Aktivitätsressourcentyp| Aktivität |
| :-- | :-: | :-- |
| Kernverzeichnis| Group| Delete Group Settings (Gruppeneinstellungen löschen)|
| Kernverzeichnis| Verzeichnis| Domäne aktualisieren|
| Kernverzeichnis| Verzeichnis| Partner aus Unternehmen entfernen|
| Kernverzeichnis| Benutzer| Rolle aktualisieren|
| Kernverzeichnis| Benutzer| Add Role From Template (Rolle aus Vorlage hinzufügen)|
| Kernverzeichnis| Group| Add App Role Assignment To Group (App-Rollenzuweisung zu Gruppe hinzufügen)|
| Kernverzeichnis| Group| Start Applying Group Based License To Users (Starten der gruppenbasierten Lizenzzuweisung zu Benutzern)|
| Kernverzeichnis| Anwendung| Dienstprinzipal hinzufügen|
| Kernverzeichnis| Richtlinie| Richtlinie aktualisieren|
| Kernverzeichnis| Richtlinie| Richtlinie dem Dienstprinzipal hinzufügen|
| Kernverzeichnis| Gerät| Add Registered Owner To Device (Registrierten Besitzer zu Gerät hinzufügen)|
| Kernverzeichnis| Gerät| Add Registered Users To Device (Registrierte Benutzer zu Gerät hinzufügen)|
| Kernverzeichnis| Gerät| Update Device Configuration (Gerätekonfiguration aktualisieren)|
| Self-Service-Kennwortverwaltung| Benutzer| Reset Password (Self-Service) (Kennwort zurücksetzen (Self-Service))|
| Self-Service-Kennwortverwaltung| Benutzer| Unlock User Account (Self-Service) (Benutzerkonto entsperren (Self-Service))|
| Self-Service-Kennwortverwaltung| Benutzer| Reset Password (By Admin) (Kennwort zurücksetzen (durch Administrator))|
| Self-Service-Gruppenverwaltung| Group| Delete A Pending Request To Join A Group (Ausstehende Gruppenbeitrittsanforderung löschen)|
| Kontobereitstellung| Anwendung| Process Escrow (Hinterlegung verarbeiten)|
| Automatisiertes Kennwortrollover| Anwendung| Automatisiertes Kennwortrollover|
| Invited Users (Eingeladene Benutzer)| Andere| Verarbeitete Batch-Einladungen|
| Kernverzeichnis| Verzeichnis| Remove Verified Domain (Überprüfte Domäne entfernen)|
| Kernverzeichnis| Verzeichnis| Add Unverified Domain (Nicht überprüfte Domäne hinzufügen)|
| Kernverzeichnis| Verzeichnis| Add Verified Domain (Überprüfte Domäne hinzufügen)|
| Kernverzeichnis| Verzeichnis| Verzeichnisfunktion für Mandanten festlegen|
| Kernverzeichnis| Verzeichnis| DirSyncEnabled-Flag festlegen|
| Kernverzeichnis| Verzeichnis| Unternehmenseinstellungen erstellen|
| Kernverzeichnis| Verzeichnis| Unternehmenseinstellungen aktualisieren|
| Kernverzeichnis| Verzeichnis| Unternehmenseinstellungen löschen|
| Kernverzeichnis| Verzeichnis| Zulässigen Datenspeicherort für Unternehmen festlegen|
| Kernverzeichnis| Verzeichnis| Funktion für multinationales Unternehmen auf „Aktiviert“ festlegen|
| Kernverzeichnis| Benutzer| Benutzer aktualisieren|
| Kernverzeichnis| Benutzer| Benutzer löschen|
| Kernverzeichnis| Group| Mitglied aus Gruppe entfernen|
| Kernverzeichnis| Group| Set Group License (Gruppenlizenz festlegen)|
| Kernverzeichnis| Group| Create Group Settings (Gruppeneinstellungen erstellen)|
| Kernverzeichnis| Anwendung| Update Service Principal (Dienstprinzipal aktualisieren)|
| Kernverzeichnis| Anwendung| Anwendung löschen|
| Kernverzeichnis| Anwendung| Anwendung aktualisieren|
| Kernverzeichnis| Anwendung| Dienstprinzipal entfernen|
| Kernverzeichnis| Anwendung| Anmeldeinformationen für Dienstprinzipal hinzufügen|
| Kernverzeichnis| Anwendung| Remove App Role Assignment From Service Principal (App-Rollenzuweisung von Dienstprinzipal entfernen)|
| Kernverzeichnis| Anwendung| Besitzer aus Anwendung entfernen|
| Kernverzeichnis| Gerät| Registrierten Besitzer aus Gerät entfernen|
| Self-Service-Kennwortverwaltung| Benutzer| Self-Serve Password Reset Flow Activity Progress (Aktivitätsstatus des Self-Service-Kennwortzurücksetzungsablaufs)|
| Kontobereitstellung| Anwendung| Verwaltung|
| Kontobereitstellung| Anwendung| Directory Operation (Verzeichnisvorgang)|
| MIM Service (MIM-Dienst)| Group| Mitglied entfernen|
| Kernverzeichnis| Richtlinie| Richtlinie löschen|
| Invited Users (Eingeladene Benutzer)| Benutzer| Viral Tenant Creation (Erstellung von viralem Mandanten)|
| Kernverzeichnis| Verzeichnis| Externe geheime Schlüssel aktualisieren|
| Kernverzeichnis| Verzeichnis| Eigenschaften für Rights Management festlegen|
| Kernverzeichnis| Verzeichnis| Update Company (Unternehmen aktualisieren)|
| Kernverzeichnis| Benutzer| Benutzer hinzufügen|
| Kernverzeichnis| Benutzer| Convert Federated User To Managed (Verbundbenutzer in verwalteten Benutzer konvertieren)|
| Kernverzeichnis| Benutzer| Create Application Password For User (Anwendungskennwort für Benutzer erstellen)|
| Kernverzeichnis| Group| Mitglied zur Gruppe hinzufügen|
| Kernverzeichnis| Group| Gruppe hinzufügen|
| Kernverzeichnis| Anwendung| Consent To Application (Anwendung zustimmen)|
| Kernverzeichnis| Anwendung| Anwendung hinzufügen|
| Kernverzeichnis| Anwendung| Add Owner To Service Principal (Besitzer zu Dienstprinzipal hinzufügen)|
| Kernverzeichnis| Anwendung| Remove Oauth2Permissiongrant (Oauth2Permissiongrant entfernen)|
| Kernverzeichnis| Richtlinie| Richtlinien-Anmeldeinformationen entfernen|
| Kernverzeichnis| Gerät| Delete Device Configuration (Gerätekonfiguration löschen)|
| Self-Service-Gruppenverwaltung| Group| Set Dynamic Group Properties (Dynamische Gruppeneigenschaften festlegen)|
| Self-Service-Gruppenverwaltung| Group| Update Lifecycle Management Policy (Richtlinie für Lebenszyklusverwaltung aktualisieren)|
| Kontobereitstellung| Anwendung| Synchronization Rule Action (Synchronisierungsregelaktion)|
| Invited Users (Eingeladene Benutzer)| Andere| Batch-Einladungen hochgeladen|
| MIM Service (MIM-Dienst)| Group| Mitglied hinzufügen|
| Kernverzeichnis| Benutzer| Lizenzeigenschaften festlegen|
| Kernverzeichnis| Benutzer| Benutzer wiederherstellen|
| Kernverzeichnis| Benutzer| Remove Member From Role (Mitglied aus Rolle entfernen)|
| Kernverzeichnis| Benutzer| Remove App Role Assignment From User (App-Rollenzuweisung von Benutzer entfernen)|
| Kernverzeichnis| Benutzer| Remove Scoped Member From Role (Zugeordnetes Mitglied aus Rolle entfernen)|
| Kernverzeichnis| Group| Gruppe aktualisieren|
| Kernverzeichnis| Group| Add Owner To Group (Besitzer zu Gruppe hinzufügen)|
| Kernverzeichnis| Group| Finish Applying Group Based License To Users (Gruppenbasierte Lizenzzuweisung zu Benutzern fertig stellen)|
| Kernverzeichnis| Group| Remove App Role Assignment From Group (App-Rollenzuweisung von Gruppe entfernen)|
| Kernverzeichnis| Group| Set Group To Be Managed By User (Gruppe für Verwaltung durch Benutzer festlegen)|
| Kernverzeichnis| Anwendung| Add Oauth2Permissiongrant (Oauth2Permissiongrant hinzufügen)|
| Kernverzeichnis| Anwendung| Add App Role Assignment To Service Principal (App-Rollenzuweisung zu Dienstprinzipal hinzufügen)|
| Kernverzeichnis| Anwendung| Anmeldeinformationen für Dienstprinzipal entfernen|
| Kernverzeichnis| Richtlinie| Richtlinie aus Dienstprinzipal entfernen|
| Kernverzeichnis| Gerät| Gerät aktualisieren|
| Kernverzeichnis| Gerät| Gerät hinzufügen|
| Kernverzeichnis| Gerät| Add Device Configuration (Gerätekonfiguration hinzufügen)|
| Self-Service-Kennwortverwaltung| Benutzer| Change Password (Self-Service) (Kennwort ändern (Self-Service))|
| Self-Service-Kennwortverwaltung| Benutzer| User Registered For Self-Service Password Reset (Für Self-Service-Kennwortzurücksetzung registrierter Benutzer)|
| Self-Service-Gruppenverwaltung| Group| Approve A Pending Request To Join A Group (Ausstehende Gruppenbeitrittsanforderung genehmigen)|
| Kernverzeichnis| Verzeichnis| Remove Unverified Domain (Nicht überprüfte Domäne entfernen)|
| Kernverzeichnis| Verzeichnis| Domäne überprüfen|
| Kernverzeichnis| Verzeichnis| Domänenauthentifizierung festlegen|
| Kernverzeichnis| Verzeichnis| Kennwortrichtlinie festlegen|
| Kernverzeichnis| Verzeichnis| Partner zu Unternehmen hinzufügen|
| Kernverzeichnis| Verzeichnis| Promote Company To Partner (Unternehmen zu Partner heraufstufen)|
| Kernverzeichnis| Verzeichnis| Set Partnership (Partnerschaft festlegen)|
| Kernverzeichnis| Verzeichnis| Schwellenwert für versehentliches Löschen festlegen|
| Kernverzeichnis| Verzeichnis| Partner tiefer stufen|
| Invited Users (Eingeladene Benutzer)| Benutzer| Externen Benutzer einladen|
| Kontobereitstellung| Anwendung| Import|
| Kernverzeichnis| Anwendung| Remove Owner From Service Principal (Besitzer aus Dienstprinzipal entfernen)|
| Kernverzeichnis| Gerät| Registrierte Benutzer aus Gerät entfernen|
| Kernverzeichnis| Verzeichnis| Unternehmensinformationen festlegen|
| Kernverzeichnis| Verzeichnis| Verbundeinstellungen für Domäne festlegen|
| Kernverzeichnis| Verzeichnis| Unternehmen erstellen|
| Kernverzeichnis| Verzeichnis| Rights Management-Eigenschaften bereinigen|
| Kernverzeichnis| Verzeichnis| DirSync-Funktion festlegen|
| Kernverzeichnis| Verzeichnis| Per E-Mail verifizierte Domäne überprüfen|
| Kernverzeichnis| Benutzer| Benutzerlizenz ändern|
| Kernverzeichnis| Benutzer| Benutzerkennwort ändern|
| Kernverzeichnis| Benutzer| Benutzerkennwort zurücksetzen|
| Kernverzeichnis| Benutzer| Add App Role Assignment Grant To User (App-Rollenzuweisungsgewährung zu Benutzer hinzufügen)|
| Kernverzeichnis| Benutzer| Add Member To Role (Mitglied zu Rolle hinzufügen)|
| Kernverzeichnis| Benutzer| Delete Application Password For User (Anwendungskennwort für Benutzer löschen)|
| Kernverzeichnis| Benutzer| Benutzeranmeldeinformationen aktualisieren|
| Kernverzeichnis| Benutzer| Set User Manager (Benutzer-Manager festlegen)|
| Kernverzeichnis| Benutzer| Add Scoped Member To Role (Zugeordnetes Mitglied zu Rolle hinzufügen)|
| Kernverzeichnis| Group| Gruppe löschen|
| Kernverzeichnis| Group| Remove Owner From Group (Besitzer aus Gruppe entfernen)|
| Kernverzeichnis| Group| Update Group Settings (Gruppeneinstellungen aktualisieren)|
| Kernverzeichnis| Anwendung| Besitzer der Anwendung hinzufügen|
| Kernverzeichnis| Anwendung| Zustimmung widerrufen|
| Kernverzeichnis| Richtlinie| Richtlinie hinzufügen|
| Kernverzeichnis| Gerät| Gerät löschen|
| Self-Service-Kennwortverwaltung| Benutzer| Blocked From Self-Service Password Reset (Von Self-Service-Kennwortzurücksetzung ausgeschlossen)|
| Self-Service-Gruppenverwaltung| Group| Request To Join A Group (Gruppenbeitritt anfordern)|
| Self-Service-Gruppenverwaltung| Group| Create Lifecycle Management Policy (Richtlinie für Lebenszyklusverwaltung erstellen)|
| Self-Service-Gruppenverwaltung| Group| Reject A Pending Request To Join A Group (Ausstehende Gruppenbeitrittsanforderung ablehnen)|
| Self-Service-Gruppenverwaltung| Group| Cancel A Pending Request To Join A Group (Ausstehende Gruppenbeitrittsanforderung abbrechen)|
| Self-Service-Gruppenverwaltung| Group| Renew Group (Gruppe erneuern)|
| Kontobereitstellung| Anwendung| Export|
| Kontobereitstellung| Anwendung| Andere|
| Invited Users (Eingeladene Benutzer)| Benutzer| Redeem External User Invite (Externe Benutzereinladung einlösen)|
| Invited Users (Eingeladene Benutzer)| Benutzer| Viral User Creation (Erstellung von viralem Benutzer)|
| Invited Users (Eingeladene Benutzer)| Benutzer| Externen Benutzer einer Anwendung zuweisen|




## <a name="audit-logs-shortcuts"></a>Verknüpfungen für Überwachungsprotokolle

Neben **Azure Active Directory** bietet das Azure-Portal noch zwei weitere Einstiegspunkte für Überwachungsdaten:

- Benutzer und Gruppen
- Unternehmensanwendungen

Eine vollständige Liste mit den Überwachungsberichtsaktivitäten finden Sie in der [Liste der Überwachungsberichtsereignisse](active-directory-reporting-audit-events.md#list-of-audit-report-events).


### <a name="users-and-groups-audit-logs"></a>Überwachungsprotokolle für Benutzer und Gruppen

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

### <a name="enterprise-applications-audit-logs"></a>Überwachungsprotokolle für Unternehmensanwendungen

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


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter [Anleitung für Azure Active Directory-Berichte](active-directory-reporting-guide.md).


