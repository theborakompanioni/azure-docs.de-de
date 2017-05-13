---
title: 'Azure AD: SSPR-Registrierung | Microsoft-Dokumentation'
description: "Registrieren von Authentifizierungsdaten für die Self-Service-Kennwortzurücksetzung von Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: joflore
ms.custom: end-user
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: bd0175558a94a146a365e8c1a32522f1e54c184e
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017


---
# <a name="register-for-self-service-password-reset"></a>Registrieren für die Self-Service-Kennwortzurücksetzung

Endbenutzer können mithilfe der Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) selbstständig ihr Kennwort zurücksetzen oder ihr Konto entsperren. Um diese Funktion verwenden zu können, müssen Sie zunächst Authentifizierungsmethoden registrieren oder die vordefinierten Authentifizierungsmethoden bestätigen, die Ihr Administrator angegeben hat.

## <a name="register-or-confirm-authentication-data-with-sspr"></a>Registrieren oder Bestätigen von Authentifizierungsdaten für die Self-Service-Kennwortzurücksetzung

1. Öffnen Sie den Webbrowser Ihres Geräts, und rufen Sie die [Registrierungsseite für die Kennwortzurücksetzung](http://aka.ms/ssprsetup) auf.
2. Geben Sie Ihren vom Administrator bereitgestellten Benutzernamen und das dazugehörige Kennwort ein.
3. Abhängig von der Konfiguration durch Ihr IT-Team können Sie einige der folgenden Optionen konfigurieren und überprüfen. Ihr Administrator kann einige Informationen für Sie eintragen, sofern er über die Berechtigung zum Verwenden dieser Informationen verfügt.
    * Die Bürotelefonnummer kann nur vom Administrator festgelegt werden.
    * Die Telefonnummer für die Authentifizierung sollte auf eine andere Telefonnummer festgelegt werden, auf die Sie Zugriff haben, z.B. ein Mobiltelefon, auf dem Sie Textnachrichten oder Anrufe empfangen können.
    * Die E-Mail-Adresse zur Authentifizierung muss auf eine alternative E-Mail-Adresse festgelegt werden, auf die Sie ohne das zurückzusetzende Kennwort zugreifen können.
    * Bei den Sicherheitsfragen wird eine Liste mit Fragen bereitgestellt, die der Administrator zur Beantwortung freigegeben hat. Sie dürfen dieselbe Frage oder Antwort nur einmal verwenden.
4. Geben Sie die vom Administrator angeforderten Informationen an, und überprüfen Sie sie. Sollten mehrere Optionen zur Verfügung stehen, empfiehlt es sich, mehrere Methoden zu registrieren, um flexibler zu sein, falls eine andere Methode nicht verfügbar ist – beispielsweise, wenn Sie unterwegs nicht unter Ihrer geschäftlichen Telefonnummer erreichbar sind.

    ![Registrieren von Authentifizierungsmethoden und Klicken auf „Fertig stellen“][Register]

5. Wählen Sie nach Abschluss von Schritt 4 **Fertig stellen** aus. In Zukunft können Sie dann bei Bedarf die Self-Service-Kennwortzurücksetzung verwenden.

Für „Authentifizierungstelefon“ oder „E-Mail-Adresse zur Authentifizierung“ eingegebene Daten werden nicht im globalen Verzeichnis angezeigt. Diese Daten sind nur für Sie selbst und für Ihre Administratoren sichtbar. Die Antworten auf Ihre Sicherheitsfragen sind nur für Sie selbst sichtbar.

Unter Umständen werden Sie von den Administratoren nach einer Weile zur Bestätigung Ihrer Authentifizierungsmethoden aufgefordert, um sicherzustellen, dass die registrierten Methoden immer noch angemessen sind.

## <a name="next-steps"></a>Nächste Schritte

* [Ändern Ihres Kennworts mithilfe der Self-Service-Kennwortzurücksetzung](active-directory-passwords-update-your-own-password.md)
* [Registrierungsseite für die Kennwortzurücksetzung](http://aka.ms/ssprsetup)
* [Portal für die Kennwortzurücksetzung](https://passwordreset.microsoftonline.com/)
* [Wenn Sie sich nicht bei Ihrem Microsoft-Konto anmelden können](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant)

[Register]: ./media/active-directory-passwords-reset-register/register-2-methods.png "Registrierungsseite für die Kennwortzurücksetzung mit registrierten Methoden und der Schaltfläche „Fertig stellen“"


