---
title: "Azure AD Connect: Passthrough-Authentifizierung – Funktionsweise | Microsoft-Dokumentation | Microsoft Docs"
description: In diesem Artikel wird die Funktionsweise der Passthrough-Authentifizierung mit Azure Active Directory beschrieben.
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
ms.date: 07/12/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: 294ffde02fdf6b54e03837b2ace77b2940441459
ms.contentlocale: de-de
ms.lasthandoff: 06/16/2017

---

# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a>Passthrough-Authentifizierung mit Azure Active Directory: Technische Einzelheiten

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a>Funktionsweise der Passthrough-Authentifizierung mit Azure Active Directory

Wenn ein Benutzer versucht, sich bei einer durch Azure Active Directory (Azure AD) gesicherten Anwendung anzumelden und die Passthrough-Authentifizierung im Mandanten aktiviert ist, geschieht Folgendes:

1. Der Benutzer versucht, auf eine Anwendung zuzugreifen (z.B. Outlook Web App unter https://outlook.office365.com/owa/).
2. Wenn der Benutzer nicht bereits angemeldet ist, wird er zur Azure AD-Anmeldeseite umgeleitet.
3. Der Benutzer gibt auf der Seite „Azure AD-Anmeldung“ seinen Benutzernamen und sein Kennwort ein und klickt anschließend auf die Schaltfläche „Anmelden“.
4. Nach dem Empfang der Anmeldeanforderung platziert Azure AD den Benutzernamen und das (mit einem öffentlichen Schlüssel verschlüsselte) Kennwort in eine Warteschlange.
5. Ein lokaler Passthrough-Authentifizierungs-Agent führt einen ausgehenden Aufruf an die Warteschlange durch und ruft den Benutzernamen und das verschlüsselte Kennwort ab.
6. Der Agent entschlüsselt das Kennwort mithilfe des privaten Schlüssels.
7. Der Agent überprüft dann mithilfe von Standard-Windows-APIs den Benutzernamen und das Kennwort in Active Directory (ein ähnlicher Mechanismus wie bei Active Directory Federation Services). Beim Benutzernamen kann es sich entweder um den lokalen Standard-Benutzernamen (in der Regel `userPrincipalName`) handeln oder um ein anderes (als `Alternate ID` bezeichnetes) Attribut, das in Azure AD Connect konfiguriert ist.
8. Der lokale Active Directory-Domänencontroller (DC) wertet dann die Anforderung aus und gibt die entsprechende Antwort (Erfolg, Fehler, Kennwort abgelaufen oder Benutzer gesperrt) an den Agenten zurück.
9. Der Agent gibt diese Antwort wiederum an Azure AD zurück.
10. Azure AD wertet die Antwort aus und gibt eine entsprechende Antwort an den Benutzer. So wird z.B. der Benutzer sofort angemeldet, oder es wird eine Multi-Factor Authentication (MFA) angefordert.
11. Wenn die Anmeldung erfolgreich ist, kann der Benutzer auf die Anwendung zugreifen.

Das folgende Diagramm veranschaulicht die dafür notwendigen Schritte und Komponenten.

![Passthrough-Authentifizierung](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

## <a name="next-steps"></a>Nächste Schritte
- [**Aktuelle Einschränkungen**](active-directory-aadconnect-pass-through-authentication-current-limitations.md): Diese Funktion befindet sich derzeit in der Vorschauphase. Erfahren Sie, welche Szenarios unterstützt werden.
- [**Schnellstart**](active-directory-aadconnect-pass-through-authentication-quick-start.md): Einrichten und Ausführen der Passthrough-Authentifizierung mit Azure AD
- [**Häufig gestellte Fragen**](active-directory-aadconnect-pass-through-authentication-faq.md): Antworten auf häufig gestellte Fragen
- [**Problembehandlung**](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): Beheben häufig auftretender Probleme mit dieser Funktion
- [**Nahtlose SSO mit Azure AD**](active-directory-aadconnect-sso.md): Informationen zu dieser Ergänzungsfunktion
- [**UserVoice:**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) Verfassen neuer Feature-Anforderungen

