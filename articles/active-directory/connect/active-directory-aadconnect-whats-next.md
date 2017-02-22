---
title: "Azure AD Connect: Nächste Schritte und Verwalten von Azure AD Connect | Microsoft Docs"
description: "Erfahren Sie, wie die Standardkonfiguration und Betriebsaufgaben für Azure AD Connect erweitert werden."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: c18bee36-aebf-4281-b8fc-3fe14116f1a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: 4fbe7bd802e9cc32d43f019980650c4723b75d5f
ms.openlocfilehash: 9c138ac7031c4cd01542dac137123fb53fcf8c06


---
# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a>Nächste Schritte und Verwalten von Azure AD Connect
Anhand der Anweisungen in den folgenden weiterführenden Themen können Sie Azure Active Directory Connect entsprechend den Bedürfnissen und Anforderungen Ihrer Organisation anpassen.  

## <a name="add-additional-sync-administrators"></a>Hinzufügen weiterer Synchronisierungsadministratoren
Standardmäßig können nur der Benutzer, der die Installation durchgeführt hat, und die lokalen Administratoren das installierte Synchronisierungsmodul verwalten. Damit weitere Personen auf das Synchronisierungsmodul zugreifen und es verwalten können, suchen Sie auf dem lokalen Server die Gruppe „ADSyncAdmins“, und fügen die Personen dieser Gruppe hinzu.

## <a name="assigning-licenses-to-azure-ad-premium-and-enterprise-mobility-users"></a>Zuweisen von Lizenzen für Benutzer von Azure AD Premium und Enterprise Mobility
Nachdem Ihre Benutzer in der Cloud synchronisiert wurden, müssen Sie ihnen nun eine Lizenz zuweisen, sodass sie Cloudanwendungen wie z. B. Office 365 verwenden können.

### <a name="to-assign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a>So weisen Sie eine Azure AD Premium- oder Enterprise Mobility Suite-Lizenz zu
- - -
1. Melden Sie sich beim Azure-Portal als Administrator an.
2. Wählen Sie im linken Bereich **Active Directory**aus.
3. Doppelklicken Sie auf der Seite "Active Directory" auf das Verzeichnis mit den Benutzern, die Sie aktivieren möchten.
4. Wählen Sie oben auf der Seite des Verzeichnisses die Option **Lizenzen**aus.
5. Wählen Sie auf der Seite "Lizenzen" die Option "Active Directory Premium" oder "Enterprise Mobility Suite" aus, und klicken Sie dann auf **Zuweisen**.
6. Wählen Sie im Dialogfeld die Benutzer aus, denen Sie Lizenzen zuweisen möchten, und klicken Sie dann auf das Häkchen, um die Änderungen zu speichern.

## <a name="verifying-the-scheduled-synchronization-task"></a>Überprüfen der geplanten Synchronisierungsaufgabe 
Sie können den Status einer Synchronisierung im Azure-Portal überprüfen.

### <a name="to-verify-the-scheduled-synchronization-task"></a>So überprüfen Sie die geplante Synchronisierungsaufgabe
- - -
1. Melden Sie sich beim Azure-Portal als Administrator an.
2. Wählen Sie im linken Bereich **Active Directory**aus.
3. Doppelklicken Sie auf der Seite "Active Directory" auf das Verzeichnis mit den Benutzern, die Sie aktivieren möchten.
4. Wählen Sie oben auf der Seite des Verzeichnisses die Option **Verzeichnisintegration**aus.
5. Prüfen Sie unter "Integration mit lokalem Active Directory" die letzte Synchronisierungszeit.

<center>![Cloud](./media/active-directory-aadconnect-whats-next/verify.png)</center>

## <a name="starting-a-scheduled-synchronization-task"></a>Starten einer geplanten Synchronisierungsaufgabe
Wenn Sie eine Synchronisierungsaufgabe ausführen möchten, können Sie dazu wieder den Azure AD Connect-Assistenten durchlaufen.  Sie müssen Ihre Azure AD-Anmeldeinformationen angeben.  Wählen Sie im Assistenten die Aufgabe **Synchronisierungsoptionen anpassen** aus, und klicken Sie in den weiteren Schritten des Assistenten auf "Weiter". Stellen Sie am Ende sicher, dass das Kontrollkästchen **Starten Sie den Synchronisierungsvorgang, sobald die Anfangskonfiguration abgeschlossen wurde** aktiviert ist.

<center>![Cloud](./media/active-directory-aadconnect-whats-next/startsynch.png)</center>

Weitere Informationen zum Scheduler für Azure AD Connect finden Sie unter [Azure AD Connect Sync: Scheduler](active-directory-aadconnectsync-feature-scheduler.md)

## <a name="additional-tasks-available-in-azure-ad-connect"></a>Weitere verfügbare Aufgaben in Azure AD Connect
Nach der ersten Installation von Azure AD Connect können Sie den Assistenten über die Azure AD Connect-Startseite oder die Desktopverknüpfung jederzeit erneut starten.  Sie werden feststellen, dass beim erneuten Durchlaufen des Assistenten unter "Zusätzliche Aufgaben" einige neue Optionen zur Verfügung stehen.  

Die folgende Tabelle enthält eine Zusammenfassung und jeweils eine kurze Beschreibung dieser Aufgaben.

![Join Rule](./media/active-directory-aadconnect-whats-next/addtasks.png)

| Weitere Aufgabe | Beschreibung |
| --- | --- |
| Ausgewähltes Szenario anzeigen |Hiermit können Sie Ihre aktuelle Azure AD Connect-Lösung anzeigen.  Dazu gehören allgemeine Einstellungen, synchronisierte Verzeichnisse, Synchronisierungseinstellungen usw. |
| Synchronisierungsoptionen anpassen |Hiermit können Sie die aktuelle Konfiguration ändern, z. B. Hinzufügen weiterer Aktive Directory-Gesamtstrukturen zur Konfiguration oder Aktivieren von Synchronisierungsoptionen wie beispielsweise "Benutzer", "Gruppe", "Gerät" oder "Kennwort zurückschreiben". |
| Stagingmodus aktivieren |Hiermit können Sie Informationen bereitstellen, die später synchronisiert, jedoch nicht in Azure AD oder Active Directory exportiert werden.  Auf diese Weise können Sie die Synchronisierungen in der Vorschau anzeigen, bevor sie ausgeführt werden. |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).




<!--HONumber=Feb17_HO2-->


