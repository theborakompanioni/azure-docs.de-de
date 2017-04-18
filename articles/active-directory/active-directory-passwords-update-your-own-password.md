---
title: "Azure AD: Zurücksetzen Ihres Kennworts | Microsoft-Dokumentation"
description: "Stellen Sie den Zugriff auf Ihr Konto mithilfe der Self-Service-Kennwortzurücksetzung wieder her."
services: active-directory
keywords: "Active Directory-Kennwortverwaltung, Kennwortverwaltung, Self-Service-Kennwortzurücksetzung in Azure AD, SSPR"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 7ba69b18-317a-4a62-afa3-924c4ea8fb49
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/11/2017
ms.author: joflore
ms.custom: end-user
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 9b7735bbe8d3b2ffc78eba8f1489d70a02fe7342
ms.lasthandoff: 04/12/2017


---
# <a name="help-i-forgot-my-password"></a>Ich habe mein Kennwort vergessen. Was nun?

Schritt 1: Keine Panik

Falls eines der folgenden Szenarien auf Sie zutrifft, können wir Ihnen weiterhelfen:

* Sie wissen nicht genau, wie Sie auf Ihr Konto zugreifen sollen, und können sich nicht mehr an Ihr Kennwort erinnern.
* Ihnen wurde kein Kennwort zugewiesen, und Sie wurden von einem Administrator hierher geschickt.

## <a name="unlock-your-account"></a>Entsperren Ihres Kontos

Falls Sie diese Seite aufgerufen haben, um Ihr Konto zu entsperren, gehen Sie wie folgt vor. Wenn weiter unten in Schritt 6 **Neues Kennwort auswählen** angezeigt wird, können Sie Ihr Kennwort entsperren oder ändern und entsperren.

## <a name="reset-my-password"></a>Zurücksetzen des eigenen Kennworts

Gehen Sie wie folgt vor, um den Zugriff auf Ihr Konto wiederherzustellen:
1. Klicken Sie auf einer beliebigen Anmeldeseite für ein Geschäfts-, Schul- oder Unikonto auf den Link **Sie können nicht auf Ihr Konto zugreifen?** und anschließend auf **Geschäfts-, Schul- oder Unikonto**, oder rufen Sie direkt die [Seite für die Kennwortzurücksetzung](https://passwordreset.microsoftonline.com/) auf.

    ![Sie können nicht auf Ihr Konto zugreifen?][Login]

2. Geben Sie die **Benutzer-ID** für Ihr Geschäfts-, Schul- oder Unikonto ein, und beweisen Sie, dass Sie kein Roboter sind, indem Sie den angezeigten CAPTCHA-Text eingeben und auf **Weiter** klicken.
3. Abhängig davon, wie Ihr Administrator das System konfiguriert hat, wird mindestens eine der folgenden Optionen angezeigt:
    * **E-Mail an meine alternative E-Mail-Adresse senden**: Sendet eine E-Mail mit einem sechsstelligen Code an Ihre alternative E-Mail-Adresse oder an Ihre E-Mail-Adresse zur Authentifizierung. (Sie haben die Wahl.)
    * **Textnachricht an mein Mobiltelefon senden**: Sendet eine SMS mit einem sechsstelligen Code an Ihr Mobiltelefon oder an Ihr Authentifizierungstelefon. (Sie haben die Wahl.)
    * **Mein Mobiltelefon anrufen**: Ruft entweder Ihr Mobiltelefon oder Ihr Authentifizierungstelefon an. (Sie haben die Wahl.) Drücken Sie die #-Taste, um den Anruf zu verifizieren.
    * **Meine geschäftliche Rufnummer anrufen**: Ruft Ihre geschäftliche Rufnummer an. Drücken Sie die #-Taste, um den Anruf zu verifizieren.
    * **Meine Sicherheitsfragen beantworten**: Zeigt Ihre zuvor registrierten Sicherheitsfragen zur Beantwortung an.
4. Füllen Sie die erforderlichen Felder mit Antworten auf die Fragen aus, und klicken Sie anschließend auf **Weiter**.

    ![Überprüfen Ihrer Authentifizierungsdaten][Verification]

5. Ihr Administrator kann einen zusätzlichen Überprüfungsschritt einbauen. In diesem Fall müssen Sie unter Umständen Schritt 4 mit einer anderen Option wiederholen.
6. Geben Sie auf der Seite **Neues Kennwort auswählen** ein neues Kennwort ein, das den Anforderungen Ihrer Organisation entspricht, bestätigen Sie Ihr neues Kennwort, und klicken Sie anschließend auf **Fertig stellen**.

    ![Ändern Ihres Kennworts][Change]

7. Wenn **Das Kennwort wurde zurückgesetzt.** erscheint, können Sie sich mit Ihrem neuen Kennwort anmelden.

    ![Das Kennwort wurde zurückgesetzt.][Complete]

> [!NOTE]
> Falls Ihr Administrator diese Funktion nicht aktiviert hat, wird ein Link vom Typ „Wenden Sie sich an den Administrator.“ angezeigt, damit Ihnen der Administrator per E-Mail oder über ein eigenes Webportal weiterhelfen kann.
>

Nachdem Sie Ihr Kennwort mit dieser Methode entsperrt oder zurückgesetzt haben, erhalten Sie unter Umständen eine E-Mail, die den Abschluss dieses Prozesses bestätigt. Die E-Mail stammt von einem Konto wie „Microsoft im Namen von <Ihre Organisation>“. Sollten Sie eine solche E-Mail erhalten, obwohl Sie den Zugriff auf Ihr Konto nicht mithilfe der Self-Service-Kennwortzurücksetzung wiederhergestellt haben, wenden Sie sich an Ihren Administrator.

## <a name="change-my-password"></a>Ändern des eigenen Kennworts

Falls Sie Ihr Kennwort bereits kennen und es ändern möchten, führen Sie die folgenden Schritte aus:

### <a name="change-your-password-from-the-office-365-portal"></a>Ändern des Kennworts über das Office 365-Portal

1. Klicken Sie rechts oben auf Ihr Profil und anschließend auf **Konto anzeigen**.
2. **Sicherheit und Datenschutz**
3. **Kennwort**
4. Geben Sie Ihr altes Kennwort ein, legen Sie Ihr neues Kennwort fest, und bestätigen Sie es.
5. **Absenden**

### <a name="change-your-password-from-the-azure-access-panel"></a>Ändern des Kennworts über den Azure-Zugriffsbereich

1. Melden Sie sich mit Ihrem vorhandenen Kennwort beim [Azure-Zugriffsportal](https://myapps.microsoft.com/) an.
2. Klicken Sie rechts oben auf Ihr Profil und anschließend auf **Profil**.
3. **Kennwort ändern**
4. Geben Sie Ihr altes Kennwort ein, legen Sie Ihr neues Kennwort fest, und bestätigen Sie es.
5. **Absenden**

## <a name="next-steps"></a>Nächste Schritte

* [Register for self-service password reset](active-directory-passwords-reset-register.md) (Registrieren für die Self-Service-Kennwortzurücksetzung)
* [Registrierungsseite für die Kennwortzurücksetzung](http://aka.ms/ssprsetup)
* [Portal für die Kennwortzurücksetzung](https://passwordreset.microsoftonline.com/)

[Login]: ./media/active-directory-passwords-update-your-own-password/reset-1-login.png "Anmeldeseite – Sie können nicht auf Ihr Konto zugreifen?"
[Verification]: ./media/active-directory-passwords-update-your-own-password/reset-2-verification.png "Überprüfen Ihrer Authentifizierungsdaten"
[Change]: ./media/active-directory-passwords-update-your-own-password/reset-3-change.png "Ändern des Kennworts"
[Complete]: ./media/active-directory-passwords-update-your-own-password/reset-4-complete.png "Kennwort wurde zurückgesetzt"

