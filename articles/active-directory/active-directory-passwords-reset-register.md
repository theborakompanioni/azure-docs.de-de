---
title: "Azure AD: Registrierung für die Self-Service-Kennwortzurücksetzung | Microsoft-Dokumentation"
description: "Registrieren von Authentifizierungsdaten für die Self-Service-Kennwortzurücksetzung"
services: active-directory
keywords: "Active Directory-Kennwortverwaltung, Kennwortverwaltung, Self-Service-Kennwortzurücksetzung in Azure AD, SSPR"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/16/2017
ms.author: joflore
ms.custom: end-user
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 0f10ad4904b10a9554e949e77f0192edcb8f624f
ms.lasthandoff: 04/12/2017


---
# <a name="register-for-self-service-password-reset"></a>Registrieren für die Self-Service-Kennwortzurücksetzung

Sofern der Administrator das Feature aktiviert hat, können Endbenutzer mithilfe der Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) selbstständig ihr Kennwort zurücksetzen oder ihr Konto entsperren. Um diese Funktion verwenden zu können, müssen Sie zunächst Authentifizierungsmethoden registrieren oder die vordefinierten Authentifizierungsmethoden bestätigen, die Ihr Administrator angegeben hat.

## <a name="register-or-confirm-authentication-data-with-sspr"></a>Registrieren oder Bestätigen von Authentifizierungsdaten für die Self-Service-Kennwortzurücksetzung

1. Öffnen Sie den Webbrowser Ihres Geräts, und rufen Sie die [Registrierungsseite für die Kennwortzurücksetzung](http://aka.ms/ssprsetup) auf.
2. Geben Sie Ihren vom Administrator bereitgestellten Benutzernamen und das dazugehörige Kennwort ein.
3. Abhängig von den Optionen, die Ihr Administrator genehmigt hat, wird mindestens eines der folgenden Elemente angezeigt, die Sie konfigurieren oder überprüfen müssen, um bei Bedarf Ihr Kennwort zurücksetzen zu können:
    * Telefon (geschäftlich): Diese Option kann nur vom Administrator festgelegt werden.
    * Authentifizierungstelefon: Diese Option muss auf eine andere Telefonnummer festgelegt werden, die Ihnen zur Verfügung steht – beispielsweise für ein Mobiltelefon, über das Sie eine SMS empfangen oder einen Anruf entgegennehmen können. (Der Systemadministrator kann diese Option für Sie ausfüllen, sofern er bereits Ihr Einverständnis zur Verwendung dieser Information eingeholt hat.)
    * E-Mail-Adresse zur Authentifizierung: Diese Option muss auf eine alternative E-Mail-Adresse festgelegt werden, auf die Sie ohne das zurückzusetzende Kennwort zugreifen können.
    * Sicherheitsfragen: Diese Option stellt eine Liste mit Fragen bereit, die der Administrator zur Beantwortung freigegeben hat. Für die einzelnen Fragen muss jeweils eine andere Antwort angegeben werden.
4. Geben Sie die vom Administrator angeforderten Informationen an, und überprüfen Sie sie. Sollten mehrere Optionen zur Verfügung stehen, empfiehlt es sich, mehrere Methoden zu registrieren, um flexibler zu sein, falls eine andere Methode nicht verfügbar ist – beispielsweise, wenn Sie unterwegs nicht unter Ihrer geschäftlichen Telefonnummer erreichbar sind.

    ![Registrieren von Authentifizierungsmethoden und Klicken auf „Fertig stellen“][Register]

5. Wählen Sie nach Abschluss von Schritt 4 **Fertig stellen** aus. In Zukunft können Sie dann bei Bedarf die Self-Service-Kennwortzurücksetzung verwenden.

Für „Authentifizierungstelefon“ oder „E-Mail-Adresse zur Authentifizierung“ eingegebene Daten werden nicht im globalen Verzeichnis angezeigt. Diese Daten sind nur für Sie selbst und für Ihre Administratoren sichtbar. Die Antworten auf Ihre Sicherheitsfragen sind nur für Sie selbst sichtbar.

Unter Umständen werden Sie von den Administratoren nach einer Weile zur Bestätigung Ihrer Authentifizierungsmethoden aufgefordert, um sicherzustellen, dass die registrierten Methoden immer noch angemessen sind.

## <a name="next-steps"></a>Nächste Schritte

* [Ändern Ihres Kennworts mithilfe der Self-Service-Kennwortzurücksetzung](active-directory-passwords-update-your-own-password.md)
* [Registrierungsseite für die Kennwortzurücksetzung](http://aka.ms/ssprsetup)
* [Kennwortzurücksetzungsportal](https://passwordreset.microsoftonline.com/)

[Register]: ./media/active-directory-passwords-reset-register/register-2-methods.png "Registrierungsseite für die Kennwortzurücksetzung mit registrierten Methoden und der Schaltfläche „Fertig stellen“"

