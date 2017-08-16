---
title: "Azure AD Connect: Nahtloses einmaliges Anmelden – Funktionsweise | Microsoft-Dokumentation"
description: In diesem Artikel wird beschrieben, wie die das nahtlose einmalige Anmelden in Azure Active Directory funktioniert.
services: active-directory
keywords: "Was ist Azure AD Connect, Active Directory installieren, erforderliche Komponenten für Azure AD, SSO, Single Sign-On, einmaliges Anmelden"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: billmath
ms.translationtype: HT
ms.sourcegitcommit: 9633e79929329470c2def2b1d06d95994ab66e38
ms.openlocfilehash: f0bcbdb03fbb70ff91ac3a56974a88eb1b26c245
ms.contentlocale: de-de
ms.lasthandoff: 08/04/2017

---

# <a name="azure-active-directory-seamless-single-sign-on-technical-deep-dive"></a>Azure Active Directory: Nahtloses einmaliges Anmelden: Technische Einblicke

In diesem Artikel finden Sie technische Details zur Funktionsweise des nahtlosen einmaligen Anmeldens in Azure Active Directory (Seamless SSO).

>[!IMPORTANT]
>Das nahtlose einmalige Anmelden ist aktuell in der Vorschauversion verfügbar.

## <a name="how-does-seamless-sso-work"></a>Wie funktioniert das nahtlose einmalige Anmelden?

Dieser Abschnitt enthält zwei Teile:
1. Die Einrichtung des nahtlosen einmaligen Anmeldens
2. Funktionsweise einer Anmeldungstransaktion eines einzelnen Benutzers mit dem nahtlosen einmaligen Anmelden

### <a name="how-does-set-up-work"></a>Wie funktioniert die Einrichtung?

Das nahtlose einmalige Anmelden wird mithilfe von Azure AD Connect wie [hier](active-directory-aadconnect-sso-quick-start.md) gezeigt aktiviert. Während der Aktivierung des Features laufen die folgenden Schritte ab:
- Ein Computerkonto namens `AZUREADSSOACCT` (das Azure AD darstellt) wird in Ihrem lokalen Active Directory (AD) erstellt.
- Der Kerberos-Entschlüsselungsschlüssel des Computerkontos wird sicher für Azure AD freigegeben.
- Darüber hinaus werden zwei Kerberos-Dienstprinzipalnamen (SPNs) erstellt, um die zwei URLs darzustellen, die während der Azure AD-Anmeldung verwendet werden.

>[!NOTE]
> In jeder AD-Gesamtstruktur, die Sie mit Azure AD (über Azure AD Connect) synchronisieren und für deren Benutzer Sie das nahtlose einmalige Anmelden aktivieren möchten, werden das Computerkonto und die Kerberos-SPNs erstellt. Verschieben das `AZUREADSSOACCT`-Computerkonto zu einer Organisationseinheit (Organization Unit, OU), in der andere Computerkonten gespeichert sind, um sicherzustellen, dass es auf die gleiche Weise verwaltet und nicht gelöscht wird.

>[!IMPORTANT]
>Es wird dringend empfohlen, den [Rollover des Kerberos-Entschlüsselungsschlüssels](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacct-computer-account) für das Computerkonto `AZUREADSSOACCT` mindestens alle 30 Tage durchzuführen.

### <a name="how-does-sign-in-with-seamless-sso-work"></a>Wie funktioniert die Anmeldung mit dem nahtlosen einmaligen Anmelden?

Nach der Einrichtung funktioniert das nahtlose einmalige Anmelden genauso wie jede andere Anmeldung, die die integrierte Windows-Authentifizierung (IWA) verwendet. Der Ablauf ist wie folgt:

1. Der Benutzer versucht von einem in eine Domäne eingebundenen unternehmenseigenen Gerät innerhalb Ihres Netzwerks auf eine Anwendung zuzugreifen (z.B. auf die Outlook Web App: https://outlook.office365.com/owa/).
2. Wenn der Benutzer nicht bereits angemeldet ist, wird der Benutzer zur Azure AD-Anmeldeseite umgeleitet.

  >[!NOTE]
  >Wenn die Azure AD-Anmeldeseite die Parameter `domain_hint` (zum Ermitteln Ihres Mandanten, z.B. contoso.onmicrosoft.com) oder `login_hint` enthält (zum Ermitteln des Benutzers, z.B. user@contoso.onmicrosoft.com oder user@contoso.com), wird Schritt 2 übersprungen.

3. Der Benutzer gibt auf der Azure AD-Anmeldeseite seinen Benutzernamen ein.
4. Wenn JavaScript im Hintergrund verwendet wird, fordert Azure AD den Browser über eine „401 – Nicht autorisiert“-Antwort auf, ein Kerberos-Ticket bereitzustellen.
5. Der Browser fordert wiederum ein Ticket von Active Directory für das `AZUREADSSOACCT`-Computerkonto an (das Azure AD darstellt).
6. Active Directory sucht das Computerkonto und gibt ein Kerberos-Ticket an den Browser zurück, das mit dem Geheimnis des Computerkontos verschlüsselt ist.
7. Der Browser leitet das Kerberos-Ticket aus Active Directory an Azure AD weiter (zu einer der [Azure AD-URLs, die zuvor den Browsereinstellungen für Intranetzonen hinzugefügt wurden](active-directory-aadconnect-sso-quick-start.md#step-3-roll-out-the-feature)).
8. Azure AD entschlüsselt das Kerberos-Ticket, das die Identität des auf dem unternehmenseigenen Gerät angemeldeten Benutzers enthält, mithilfe des zuvor freigegebenen Schlüssels.
9. Nach der Auswertung gibt Azure AD entweder ein Token an die Anwendung zurück oder fordert den Benutzer auf, zusätzliche Nachweise z.B. per Multi-Factor Authentication bereitzustellen.
10. Wenn die Anmeldung erfolgreich ist, kann der Benutzer auf die Anwendung zugreifen.

Das folgende Diagramm veranschaulicht die dafür notwendigen Schritte und Komponenten.

![Nahtloses einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso2.png)

Das nahtlose einmalige Anmelden ist ein opportunistisches Feature, d.h., wenn ein Fehler auftritt, wird auf die reguläre Benutzeranmeldung zurückgegriffen – die Benutzer müssen für die Anmeldung also ihr Kennwort eingeben.

## <a name="next-steps"></a>Nächste Schritte

- [**Schnellstart:**](active-directory-aadconnect-sso-quick-start.md) Einrichten und Ausführen des nahtlosen einmaligen Anmeldens von Azure AD
- [**Häufig gestellte Fragen:**](active-directory-aadconnect-sso-faq.md) Antworten auf häufig gestellte Fragen
- [**Problembehandlung**](active-directory-aadconnect-troubleshoot-sso.md) – Beheben von häufig auftretenden Problemen mit diesem Feature
- [**UserVoice:**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) Verfassen neuer Feature-Anforderungen

