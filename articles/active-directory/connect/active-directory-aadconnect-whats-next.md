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
ms.sourcegitcommit: dde6242211ca3d8d7b47c0ec4a01965e1439adeb
ms.openlocfilehash: d3b900b40f683cd446b41696c4f9e617299bf0b1
ms.lasthandoff: 02/22/2017


---
# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a>Nächste Schritte und Verwalten von Azure AD Connect
Befolgen Sie die in diesem Artikel beschriebenen Verfahren zum Anpassen von Azure Active Directory (Azure AD) Connect an die Bedürfnisse und Anforderungen Ihrer Organisation.  

## <a name="add-additional-sync-admins"></a>Hinzufügen weiterer Synchronisierungsadministratoren
Standardmäßig können nur der Benutzer, der die Installation durchgeführt hat, und die lokalen Administratoren das installierte Synchronisierungsmodul verwalten. Damit weitere Personen auf das Synchronisierungsmodul zugreifen und es verwalten können, suchen Sie auf dem lokalen Server die Gruppe „ADSyncAdmins“, und fügen die Personen dieser Gruppe hinzu.

## <a name="assign-licenses-to-azure-ad-premium-and-enterprise-mobility-suite-users"></a>Zuweisen von Lizenzen zu Benutzern von Azure AD Premium und Enterprise Mobility Suite
Nachdem Ihre Benutzer in der Cloud synchronisiert wurden, müssen Sie ihnen nun eine Lizenz zuweisen, damit sie Cloudanwendungen wie Office 365 nutzen können.

### <a name="to-assign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a>So weisen Sie eine Azure AD Premium- oder Enterprise Mobility Suite-Lizenz zu

1. Melden Sie sich beim Azure-Portal als Administrator an.
2. Wählen Sie im linken Bereich **Active Directory** aus.
3. Doppelklicken Sie auf der Seite **Active Directory** auf das Verzeichnis mit den Benutzern, die Sie einrichten möchten.
4. Wählen Sie oben auf der Seite des Verzeichnisses die Option **Lizenzen**aus.
5. Wählen Sie auf der Seite **Lizenzen** die Option **Active Directory Premium** oder **Enterprise Mobility Suite** aus, und klicken Sie dann auf **Zuweisen**.
6. Wählen Sie im Dialogfeld die Benutzer aus, denen Sie Lizenzen zuweisen möchten, und klicken Sie dann auf das Häkchen, um die Änderungen zu speichern.

## <a name="verify-the-scheduled-synchronization-task"></a>Überprüfen der geplanten Synchronisierungsaufgabe
Prüfen Sie im Azure-Portal den Status einer Synchronisierung.

### <a name="to-verify-the-scheduled-synchronization-task"></a>So überprüfen Sie die geplante Synchronisierungsaufgabe
1. Melden Sie sich beim Azure-Portal als Administrator an.
2. Wählen Sie im linken Bereich **Active Directory**aus.
3. Doppelklicken Sie auf der Seite **Active Directory** auf das Verzeichnis mit den Benutzern, die Sie einrichten möchten.
4. Wählen Sie oben auf der Seite des Verzeichnisses die Option **Verzeichnisintegration**aus.
5. Prüfen Sie unter **Integration mit lokalem Active Directory** die letzte Synchronisierungszeit.

<center>![Zeitpunkt der Verzeichnissynchronisierung](./media/active-directory-aadconnect-whats-next/verify.png)</center>

## <a name="start-a-scheduled-synchronization-task"></a>Starten einer geplanten Synchronisierungsaufgabe
Wenn Sie eine Synchronisierungsaufgabe ausführen möchten, können Sie dazu erneut den Azure AD Connect-Assistenten durchlaufen.  Sie müssen Ihre Azure AD-Anmeldeinformationen angeben.  Wählen Sie im Assistenten die Aufgabe **Synchronisierungsoptionen anpassen** aus, und klicken Sie in den weiteren Schritten des Assistenten auf **Weiter**. Stellen Sie am Ende sicher, dass das Kontrollkästchen **Starten Sie den Synchronisierungsvorgang, sobald die Anfangskonfiguration abgeschlossen wurde** aktiviert ist.

<center>![Starten der Synchronisierung](./media/active-directory-aadconnect-whats-next/startsynch.png)</center>

Weitere Informationen zum Scheduler für Azure AD Connect finden Sie unter [Azure AD Connect Sync: Scheduler](active-directory-aadconnectsync-feature-scheduler.md).

## <a name="additional-tasks-available-in-azure-ad-connect"></a>Weitere verfügbare Aufgaben in Azure AD Connect
Nach der ersten Installation von Azure AD Connect können Sie den Assistenten über die Azure AD Connect-Startseite oder die Desktopverknüpfung jederzeit erneut starten.  Sie werden feststellen, dass beim erneuten Durchlaufen des Assistenten unter "Zusätzliche Aufgaben" einige neue Optionen zur Verfügung stehen.  

Die folgende Tabelle enthält eine Zusammenfassung und jeweils eine kurze Beschreibung dieser Aufgaben.

![Liste mit weiteren Aufgaben](./media/active-directory-aadconnect-whats-next/addtasks.png)

| Weitere Aufgabe | Beschreibung |
| --- | --- |
| **Ausgewähltes Szenario anzeigen** |Sie können Ihre aktuelle Azure AD Connect-Lösung anzeigen.  Dazu gehören allgemeine Einstellungen, synchronisierte Verzeichnisse und Synchronisierungseinstellungen. |
| **Synchronisierungsoptionen anpassen** |Sie können die aktuelle Konfiguration ändern, z.B. weitere Aktive Directory-Gesamtstrukturen zur Konfiguration hinzufügen oder Synchronisierungsoptionen wie beispielsweise „Benutzer“, „Gruppe“, „Gerät“ oder „Kennwort zurückschreiben“ aktivieren. |
| **Stagingmodus aktivieren** |Informationen zum Staging, die nicht sofort synchronisiert und nicht in Azure AD oder lokales Active Directory exportiert werden.  Mit diesem Feature können Sie die Synchronisierungen in einer Vorschau anzeigen, ehe sie erfolgen. |

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).

