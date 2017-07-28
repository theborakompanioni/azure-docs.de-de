---
title: "Azure AD Connect: Nahtloses einmaliges Anmelden – Häufig gestellte Fragen | Microsoft-Dokumentation"
description: "Antworten auf häufig gestellte Fragen zum nahtlosen einmaligen Anmelden mit Azure Active Directory."
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
ms.date: 07/12/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: e363ade43ab9e2b2432c9efdc3ce1b661e278b2a
ms.contentlocale: de-de
ms.lasthandoff: 06/16/2017

---

# <a name="azure-active-directory-seamless-single-sign-on-frequently-asked-questions"></a>Nahtloses einmaliges Anmelden mit Azure Active Directory: Häufig gestellte Fragen (FAQs)

In diesem Artikel werden häufig gestellte Fragen zum nahtlosen einmaligen Anmelden (Seamless Single Sign-On, SSO) mit Azure AD beantwortet. Schauen Sie öfter vorbei, da wir regelmäßig neue Fragen hinzufügen.

## <a name="what-sign-in-methods-do-seamless-sso-work-with"></a>Welche Anmeldemethoden verwendet die nahtlose SSO?

Das nahtlose einmalige Anmelden kann mit den Anmeldemethoden [Kennworthashsynchronisierung](active-directory-aadconnectsync-implement-password-synchronization.md) oder [Passthrough-Authentifizierung](active-directory-aadconnect-pass-through-authentication.md) kombiniert werden, jedoch nicht mit Active Directory-Verbunddiensten (AD FS).

## <a name="is-seamless-sso-a-free-feature"></a>Ist die nahtlose SSO eine kostenlose Funktion?

Die nahtlose SSO ist eine kostenlose Funktion, sodass Sie für deren Verwendung keine kostenpflichtigen Versionen von Azure AD benötigen. Die Funktion bleibt auch kostenlos, wenn sie allgemein verfügbar wird.

## <a name="what-applications-take-advantage-of-domainhint-or-loginhint-parameter-capability-of-seamless-sso"></a>Welche Anwendungen nutzen die `domain_hint`- oder `login_hint`-Parameterfunktion der nahtlosen SSO?

Wir arbeiten zurzeit daran, eine Liste der Anwendungen zu kompilieren, die diese Parameter senden sowie eine Liste derer, die sie nicht senden. Wenn Sie an bestimmten Anwendungen interessiert sind, teilen Sie uns dies bitte im Abschnitt „Kommentare“ mit.

## <a name="does-seamless-sso-support-alternate-id-as-the-username-instead-of-userprincipalname"></a>Unterstützt die nahtlose SSO `Alternate ID` als Benutzername anstelle von `userPrincipalName`?

Ja. Die nahtlose SSO unterstützt `Alternate ID` als Benutzername, wenn dies in Azure AD Connect wie [hier](active-directory-aadconnect-get-started-custom.md) beschrieben entsprechend konfiguriert ist. Nicht alle Office 365-Anwendungen unterstützen `Alternate ID`. Angaben zur Supporterklärung finden Sie in der Dokumentation der jeweiligen Anwendung.

## <a name="i-want-to-register-non-windows-10-devices-with-azure-ad-without-using-ad-fs-can-i-use-seamless-sso-instead"></a>Ich möchte Geräte ohne Windows 10 mit Azure AD registrieren, ohne AD FS zu verwenden. Kann ich stattdessen die nahtlose SSO verwenden?

Ja. Für dieses Szenario benötigen Sie Version 2.1 oder höher des [Clients für die Arbeitsplatzeinbindung](https://www.microsoft.com/download/details.aspx?id=53554).

## <a name="how-can-i-disable-seamless-sso"></a>Wie kann ich die nahtlose SSO deaktivieren?

Die nahtlose SSO kann mit Azure AD Connect deaktiviert werden.

Führen Sie Azure AD Connect aus, wählen Sie „Benutzeranmeldung ändern“, und klicken Sie auf „Weiter“. Deaktivieren Sie dann die Option „Einmaliges Anmelden aktivieren“. Setzen Sie den Assistenten fort. Nach Abschluss des Assistenten ist die nahtlose SSO für Ihren Mandanten deaktiviert.

Allerdings wird auf dem Bildschirm die folgende Meldung angezeigt:

„Das einmalige Anmelden (SSO) ist jetzt deaktiviert, aber zur Bereinigung müssen manuell zusätzliche Schritte ausgeführt werden. Weitere Informationen“

Die folgenden manuellen Schritte müssen ausgeführt werden:

1. Rufen Sie die Liste der AD-Gesamtstrukturen ab, für die das nahtlose einmalige Anmelden aktiviert wurde.
- Laden Sie zuerst den [Microsoft Online Services-Anmeldeassistenten](http://go.microsoft.com/fwlink/?LinkID=286152) herunter, und installieren Sie ihn.
- Laden Sie anschließend das [Azure Active Directory-Modul für Windows PowerShell (64 Bit)](http://go.microsoft.com/fwlink/p/?linkid=236297)herunter, und installieren Sie es.
- Navigieren Sie zum Ordner `%programfiles%\Microsoft Azure Active Directory Connect`.
- Importieren Sie das PowerShell-Modul „Nahtlose SSO“ mit folgendem Befehl: `Import-Module .\AzureADSSO.psd1`.
  - Rufen Sie in PowerShell `New-AzureADSSOAuthenticationContext` auf. Dadurch sollte ein Fenster eingeblendet werden, in dem Sie Ihre Anmeldeinformationen als Azure AD-Mandantenadministrator eingeben.
  - Rufen Sie `Get-AzureADSSOStatus` auf. Dadurch erhalten Sie die Liste der AD-Gesamtstrukturen (siehe die Liste „Domänen“), in denen diese Funktion aktiviert ist.
2. Löschen Sie das Computerkonto `AZUREADSSOACCT` manuell aus jeder AD-Gesamtstruktur, die Sie in der Liste finden.

## <a name="next-steps"></a>Nächste Schritte

- [**Schnellstart**](active-directory-aadconnect-sso-quick-start.md): Einrichten und Ausführen der nahtlosen SSO mit Azure AD
- [**Ausführliche technische Informationen**](active-directory-aadconnect-sso-how-it-works.md): Informationen zur Funktionsweise dieser Funktion
- [**Problembehandlung**](active-directory-aadconnect-troubleshoot-sso.md): Beheben häufig auftretender Probleme mit dieser Funktion
- [**UserVoice:**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) Verfassen neuer Feature-Anforderungen

