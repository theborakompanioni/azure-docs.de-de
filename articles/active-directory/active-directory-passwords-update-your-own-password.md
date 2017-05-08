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
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: c95f52f028cdfaed35821275d816a24035ff02fb
ms.lasthandoff: 04/26/2017


---
# <a name="help-i-forgot-my-azure-ad-password"></a>Ich habe mein Azure AD-Kennwort vergessen. Was nun?

Falls Sie Ihr Kennwort vergessen haben, von der IT-Abteilung kein Kennwort erhalten haben, aus Ihrem Konto ausgesperrt wurden oder Ihr Kennwort ändern möchten, können wir Ihnen weiterhelfen.

## <a name="reset-or-unlock-my-password-for-a-work-or-school-account"></a>Zurücksetzen oder Entsperren des eigenen Kennworts für ein Geschäfts-, Schul- oder Unikonto

Rufen Sie mithilfe der folgenden Schritte die Self-Service-Kennwortzurücksetzung von Azure AD (kurz: SSPR für „Self-Service Password Reset“) auf, um Zugriff auf Ihr Geschäfts-, Schul- oder Unikonto zu erlangen.

1. Klicken Sie auf einer beliebigen Anmeldeseite für ein Geschäfts-, Schul- oder Unikonto auf den Link **Sie können nicht auf Ihr Konto zugreifen?**, und klicken Sie anschließend auf  **Geschäfts-, Schul- oder Unikonto**, oder rufen Sie direkt die [Seite für die Kennwortzurücksetzung](https://passwordreset.microsoftonline.com/) auf.

   > [!NOTE]
   > Wenn Sie versuchen, Zugriff auf ein persönliches Konto wie „hotmail.com“ „outlook.com“ zu erlangen, probieren Sie die [Vorschläge in diesem Artikel](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) aus.
   >
    ![Sie können nicht auf Ihr Konto zugreifen?][Login]

2. Geben Sie die **Benutzer-ID** Ihres Geschäfts-, Schul- oder Unikontos ein, beweisen Sie, dass Sie kein Roboter sind, indem Sie die auf dem Bildschirm angezeigten Zeichen eingeben, und klicken Sie anschließend auf **Weiter**.

   > [!NOTE]
   > Falls Ihr IT-Personal diese Funktion nicht aktiviert hat, wird ein Link angezeigt, über den Sie Ihren Administrator kontaktieren können, um Unterstützung vom IT-Personal zu erhalten –entweder per E-Mail oder über ein eigenes Webportal.
   >

3. Je nachdem, wie das IT-Personal SSPR konfiguriert hat, wird mindestens eine der folgenden Optionen angezeigt. Einige dieser Informationen wurden vom IT-Personal oder von Ihnen selbst bereits angegeben (wie [hier](active-directory-passwords-reset-register.md) beschrieben).
    * **E-Mail an meine alternative E-Mail-Adresse senden**
    * **Textnachricht an mein Mobiltelefon senden**
    * **Mein Mobiltelefon anrufen**
    * **Meine geschäftliche Rufnummer anrufen**
    * **Meine Sicherheitsfragen beantworten**

    Wählen Sie eine Option aus, geben Sie die richtigen Antworten an, und klicken Sie anschließend auf **Weiter**.

    ![Überprüfen Ihrer Authentifizierungsdaten][Verification]

4. Unter Umständen müssen Sie Schritt 3 erneut ausführen, falls das IT-Personal eine weitere Überprüfung erfordert.
5. Geben Sie auf der Seite **Neues Kennwort auswählen** ein neues Kennwort ein, bestätigen Sie Ihr neues Kennwort, und klicken Sie anschließend auf **Fertig stellen**. Wir empfehlen ein Kennwort mit 8 bis 16 Zeichen, Groß- und Kleinbuchstaben, Zahlen und Sonderzeichen.

   > [!NOTE]
   > Falls Sie Ihr Konto entsperren möchten, wählen Sie an diesem Punkt nur die Entsperroption aus, oder ändern Sie Ihr Kennwort, und entsperren Sie Ihr Konto.
   >

6. Wenn **Das Kennwort wurde zurückgesetzt.** erscheint, können Sie sich mit Ihrem neuen Kennwort anmelden.

    ![Das Kennwort wurde zurückgesetzt.][Complete]

Sie sollten nun auf Ihr Konto zugreifen können. Falls nicht, wenden Sie sich an das IT-Personal Ihrer Organisation, um weitere Hilfe zu erhalten.

Möglicherweise erhalten Sie eine Bestätigungs-E-Mail von einem Konto wie „Microsoft im Auftrag von \<Ihre Organisation>\". Sollten Sie eine solche E-Mail erhalten, obwohl Sie den Zugriff auf Ihr Konto nicht mithilfe der Self-Service-Kennwortzurücksetzung wiederhergestellt haben, wenden Sie sich an das IT-Personal Ihrer Organisation.

## <a name="change-my-password"></a>Ändern des eigenen Kennworts

Falls Sie Ihr Kennwort bereits kennen und es ändern möchten, führen Sie die folgenden Schritte aus.

### <a name="change-your-password-from-the-office-365-portal"></a>Ändern des Kennworts über das Office 365-Portal

Verwenden Sie die folgende Methode, falls Sie für gewöhnlich über das Office-Portal auf Ihre Anwendung zugreifen:

1. Melden Sie sich bei Ihrem [Office 365-Konto](https://www.office.com) an.
2. Klicken Sie rechts oben auf Ihr Profil und anschließend auf **Konto anzeigen**.
3. Klicken Sie auf **Sicherheit und Datenschutz** > **Kennwort**.
4. Geben Sie Ihr altes Kennwort ein, legen Sie Ihr neues Kennwort fest, bestätigen Sie es, und klicken Sie anschließend auf **Absenden**.

### <a name="change-your-password-from-the-azure-access-panel"></a>Ändern des Kennworts über den Azure-Zugriffsbereich

Verwenden Sie die folgende Methode, falls Sie für gewöhnlich über das Azure Zugriffsportal auf Ihre Anwendung zugreifen:

1. Melden Sie sich mit Ihrem vorhandenen Kennwort beim [Azure-Zugriffsportal](https://myapps.microsoft.com/) an.
2. Klicken Sie rechts oben auf Ihr Profil und anschließend auf **Profil**.
3. Klicken Sie auf **Kennwort ändern**.
4. Geben Sie Ihr altes Kennwort ein, legen Sie Ihr neues Kennwort fest, bestätigen Sie es, und klicken Sie anschließend auf **Absenden**.

## <a name="next-steps"></a>Nächste Schritte

* [Register for self-service password reset](active-directory-passwords-reset-register.md) (Registrieren für die Self-Service-Kennwortzurücksetzung)
* [Registrierungsseite für die Kennwortzurücksetzung](http://aka.ms/ssprsetup)
* [Portal für die Kennwortzurücksetzung](https://passwordreset.microsoftonline.com/)
* [Wenn Sie sich nicht bei Ihrem Microsoft-Konto anmelden können](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant)

[Login]: ./media/active-directory-passwords-update-your-own-password/reset-1-login.png "Anmeldeseite – Sie können nicht auf Ihr Konto zugreifen?"
[Verification]: ./media/active-directory-passwords-update-your-own-password/reset-2-verification.png "Überprüfen Ihrer Authentifizierungsdaten"
[Change]: ./media/active-directory-passwords-update-your-own-password/reset-3-change.png "Ändern des Kennworts"
[Complete]: ./media/active-directory-passwords-update-your-own-password/reset-4-complete.png "Kennwort wurde zurückgesetzt"

