---
title: "Azure AD Connect: Passthrough-Authentifizierung – Upgrade von Authentifizierungs-Agents der Vorschau | Microsoft-Dokumentation"
description: In diesem Artikel wird beschrieben, wie Sie die Konfiguration Ihrer Azure AD-Passthrough-Authentifizierung (Azure Active Directory) aktualisieren.
services: active-directory
keywords: "Passthrough-Authentifizierung mit Azure AD Connect, Active Directory installieren, erforderliche Komponenten für Azure AD, SSO, einmaliges Anmelden"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: billmath
ms.translationtype: HT
ms.sourcegitcommit: 1dbb1d5aae55a4c926b9d8632b416a740a375684
ms.openlocfilehash: 940cb4466ef5d730c42d04d0107f6901f55eb155
ms.contentlocale: de-de
ms.lasthandoff: 08/07/2017

---

# <a name="azure-active-directory-pass-through-authentication-upgrade-preview-authentication-agents"></a>Azure Active Directory-Passthrough-Authentifizierung – Upgrade von Authentifizierungs-Agents der Vorschau

## <a name="overview"></a>Übersicht

Dieser Artikel richtet sich an Kunden, die die Azure AD-Passthrough-Authentifizierung in der Vorschau verwenden. Die Authentifizierung-Agent-Software wurde kürzlich aktualisiert (und umbenannt). Sie müssen _manuell_ ein Upgrade der auf Ihren lokalen Servern installierten Authentifizierungs-Agents durchführen. Dieses manuelle Upgrade ist eine einmalige Aktion. Alle zukünftigen Updates von Authentifizierungs-Agents erfolgen automatisch. Es gibt folgende Gründe für das Upgrade:

- Die Vorschauversionen der Authentifizierungs-Agents erhalten keine weiteren Sicherheitsupdates oder Fehlerbehebungen.
-   Die Vorschauversionen der Authentifizierungs-Agents können nicht auf weiteren Servern installiert werden, um für Hochverfügbarkeit zu sorgen.

## <a name="check-versions-of-your-authentication-agents"></a>Überprüfen der Versionen Ihrer Authentifizierungs-Agents

### <a name="step-1-check-where-your-authentication-agents-are-installed"></a>Schritt 1: Überprüfen, wo Ihre Authentifizierungs-Agents installiert sind

Führen Sie folgende Schritte aus, um zu überprüfen, wo Ihre Authentifizierungs-Agents installiert sind:

1. Melden Sie sich mit den Anmeldeinformationen des globalen Administrators für Ihren Mandanten beim [Azure Active Directory-Admin Center](https://aad.portal.azure.com) an.
2. Klicken Sie im linken Navigationsmenü auf **Azure Active Directory**.
3. Wählen Sie **Azure AD Connect** aus. 
4. Wählen Sie **Passthrough-Authentifizierung** aus. Auf diesem Blatt werden die Server aufgelistet, auf denen Ihre Authentifizierungs-Agents installiert sind.

![Azure Active Directory-Admin Center – Passthrough-Authentifizierungs-Blatt](./media/active-directory-aadconnect-pass-through-authentication/pta8.png)

### <a name="step-2-check-the-versions-of-your-authentication-agents"></a>Schritt 2: Überprüfen der Versionen Ihrer Authentifizierungs-Agents

Um die Versionen der Authentifizierungs-Agents zu überprüfen, die im vorherigen Schritt auf jedem Server identifiziert wurden, führen Sie folgende Schritte aus:

1. Wechseln Sie auf dem lokalen Server zu **Systemsteuerung -> Programme -> Programme und Funktionen**.
2. Wenn ein Eintrag für **Microsoft Azure AD Connect-Authentifizierungs-Agent** vorhanden ist, müssen Sie auf diesem Server nichts weiter tun.
3. Wenn Sie einen Eintrag für **Microsoft Azure AD-Anwendungsproxyconnector** Version 1.5.132.0 oder früher sehen, müssen Sie auf diesem Server ein manuelles Upgrade durchführen.

![Vorschauversion des Authentifizierungs-Agents](./media/active-directory-aadconnect-pass-through-authentication/pta6.png)

## <a name="best-practices-to-follow-before-starting-the-upgrade"></a>Bewährte Methoden vor dem Start des Upgrades

Stellen Sie vor dem Upgrade sicher, dass folgende Elemente vorhanden sind:

1. **Erstellen Sie ein rein cloudbasiertes globales Administratorkonto**: Führen Sie ein Upgrade erst durch, wenn Sie über ein globales Administratorkonto verfügen, das nur in der Cloud verwendet wird. Dieses benötigen Sie für Notfallsituationen, in denen Ihre Passthrough-Authentifizierungs-Agents nicht ordnungsgemäß funktionieren. Erfahren Sie, wie Sie ein [rein cloudbasiertes Konto für den globalen Administrator hinzufügen](../active-directory-users-create-azure-portal.md). Dieser Schritt ist unerlässlich und stellt sicher, dass Sie sich nicht aus Ihrem Mandanten aussperren.
2.  **Stellen Sie Hochverfügbarkeit sicher**: Sofern noch nicht geschehen, installieren Sie einen zweiten eigenständigen Authentifizierungs-Agent, um Hochverfügbarkeit für Anmeldeanforderungen bereitzustellen. Folgen Sie [diesen Anweisungen](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability).

## <a name="upgrading-the-authentication-agent-on-your-azure-ad-connect-server"></a>Aktualisieren des Authentifizierungs-Agents auf Ihrem Azure AD Connect-Server

Sie müssen Azure AD Connect aktualisieren, bevor Sie ein Upgrade des Authentifizierungs-Agents auf dem gleichen Server durchführen. Führen Sie folgende Schritte sowohl auf dem primären Azure AD Connect-Server als auch auf dem Azure AD Connect-Stagingserver aus:

1. **Aktualisieren Sie Azure AD Connect**: Befolgen Sie die Anweisungen in diesem [Artikel](./active-directory-aadconnect-upgrade-previous-version.md), und führen Sie ein Upgrade auf die neueste Azure AD Connect-Version durch.
2. **Deinstallieren Sie die Vorschauversion der Authentifizierungs-Agents**: Laden Sie [dieses PowerShell-Skript](https://aka.ms/rmpreviewagent) herunter, und führen Sie es als Administrator auf dem Server aus.
3. **Laden Sie die neueste Version des Authentifizierungs-Agents (Versionen 1.5.193.0 oder höher) herunter**: Melden Sie sich mit den Anmeldeinformationen des globalen Administrators für Ihren Mandanten beim [Azure Active Directory-Admin Center](https://aad.portal.azure.com) an. Wählen Sie **Azure Active Directory -> Azure AD Connect -> Passthrough-Authentifizierung -> Agent herunterladen** aus. Akzeptieren Sie die Nutzungsbedingungen des Diensts, und laden Sie die neueste Version des Authentifizierungs-Agents herunter.
4. **Installieren Sie die neueste Version des Authentifizierungs-Agents**: Führen Sie die in Schritt 3 heruntergeladene ausführbare Datei aus. Stellen Sie die Anmeldeinformationen des globalen Administrators Ihres Mandanten bereit, wenn Sie dazu aufgefordert werden.
5. **Überprüfen Sie, ob die neueste Version installiert wurde**: Wie oben gezeigt, wechseln Sie zu **Systemsteuerung -> Programme -> Programme und Funktionen**, und überprüfen Sie, ob ein Eintrag für **Microsoft Azure AD Connect-Authentifizierungs-Agent** vorhanden ist.

>[!NOTE]
>Nach Abschluss der vorherigen Schritte sehen Sie auf dem Blatt „Passthrough-Authentifizierung“ im [Azure Active Directory-Admin Center](https://aad.portal.azure.com) zwei Authentifizierungs-Agent-Einträge pro Server – einen Eintrag, der den Authentifizierungs-Agent als **Aktiv**, und einen, der ihn als **Inaktiv** anzeigt. Dies entspricht dem _erwarteten_ Verhalten. Der **Inaktiv**-Eintrag wird nach einigen Tagen automatisch gelöscht.

## <a name="upgrading-the-authentication-agent-on-other-servers"></a>Aktualisieren der Authentifizierungs-Agents auf anderen Servern

Führen Sie diese Schritte aus, um die Authentifizierungs-Agents auf anderen Servern zu aktualisieren (auf denen Azure AD Connect nicht installiert ist):

1. **Deinstallieren Sie die Vorschauversion der Authentifizierungs-Agents**: Laden Sie [dieses PowerShell-Skript](https://aka.ms/rmpreviewagent) herunter, und führen Sie es als Administrator auf dem Server aus.
2. **Laden Sie die neueste Version des Authentifizierungs-Agents (Versionen 1.5.193.0 oder höher) herunter**: Melden Sie sich mit den Anmeldeinformationen des globalen Administrators für Ihren Mandanten beim [Azure Active Directory-Admin Center](https://aad.portal.azure.com) an. Wählen Sie **Azure Active Directory -> Azure AD Connect -> Passthrough-Authentifizierung -> Agent herunterladen** aus. Akzeptieren Sie die Nutzungsbedingungen des Diensts, und laden Sie die neueste Version herunter.
3. **Installieren Sie die neueste Version des Authentifizierungs-Agents**: Führen Sie die in Schritt 2 heruntergeladene ausführbare Datei aus. Stellen Sie die Anmeldeinformationen des globalen Administrators Ihres Mandanten bereit, wenn Sie dazu aufgefordert werden.
4. **Überprüfen Sie, ob die neueste Version installiert wurde**: Wie oben gezeigt, wechseln Sie zu **Systemsteuerung -> Programme -> Programme und Funktionen**, und überprüfen Sie, ob ein Eintrag für **Microsoft Azure AD Connect-Authentifizierungs-Agent** vorhanden ist.

>[!NOTE]
>Nach Abschluss der vorherigen Schritte sehen Sie auf dem Blatt „Passthrough-Authentifizierung“ im [Azure Active Directory-Admin Center](https://aad.portal.azure.com) zwei Authentifizierungs-Agent-Einträge pro Server – einen Eintrag, der den Authentifizierungs-Agent als **Aktiv**, und einen, der ihn als **Inaktiv** anzeigt. Dies entspricht dem _erwarteten_ Verhalten. Der **Inaktiv**-Eintrag wird nach einigen Tagen automatisch gelöscht.

## <a name="next-steps"></a>Nächste Schritte
- [**Problembehandlung**](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – Beheben von häufig auftretenden Problemen mit diesem Feature

