---
title: 'Azure AD: SSPR-Registrierung | Microsoft-Dokumentation'
description: "Registrieren von Authentifizierungsdaten für die Self-Service-Kennwortzurücksetzung von Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: end-user
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: 485dfa067bd64312f6ae68a533117db1556318ed
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017

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

## <a name="common-problems-and-their-solutions"></a>häufige Probleme und Lösungen

 Hier finden Sie einige häufige Fehler und die passenden Lösungen:

| Fehlerbeschreibung| Welcher Fehler wird angezeigt?| Lösung |
| --- | --- | --- |
| Nach der Eingabe meiner Benutzer-ID wird mir die Seite „Wenden Sie sich an Ihren Administrator“ angezeigt. | Wenden Sie sich an Ihren Administrator <br> <br> Es wurde ermittelt, dass das Kennwort Ihres Benutzerkontos nicht von Microsoft verwaltet wird. Ihr Kennwort kann daher nicht automatisch zurückgesetzt werden. <br> <br> Sie müssen sich für weitere Unterstützung an IT-Mitarbeiter wenden. | Diese Meldung wird angezeigt, weil Ihr IT-Team Ihr Kennwort in Ihrer lokalen Umgebung verwaltet und es nicht gestattet, dass Sie das Kennwort über den Link „Kein Zugriff auf Ihr Konto?“ zurücksetzen. <br> <br> Wenden Sie sich zum Zurücksetzen des Kennworts direkt an Ihr IT-Team, und teilen Sie ihm mit, dass Sie Ihr Kennwort zurücksetzen möchten. Es kann dann dieses Feature für Sie aktivieren.|
| Nach dem Eingeben meiner Benutzer-ID erhalte ich die Fehlermeldung „Ihr Konto ist für ein Zurücksetzen des Kennworts nicht aktiviert“. | Ihr Konto ist nicht für die Kennwortzurücksetzung aktiviert. <br> <br> Ihr IT-Team hat Ihr Konto nicht zur Verwendung mit diesem Dienst eingerichtet. <br> <br> Wenn Sie möchten, können wir einen Administrator in Ihrer Organisation kontaktieren, um Ihr Kennwort für Sie zurückzusetzen. | Diese Meldung wird angezeigt, weil Ihr IT-Team die Kennwortzurücksetzung über den Link „Kein Zugriff auf Ihr Konto?“ für Ihre Organisation nicht aktiviert hat oder weil es Ihnen die Nutzung dieses Features nicht gestattet hat. <br> <br> Klicken Sie zum Zurücksetzen Ihres Kennworts auf den Link zum Kontaktieren eines Administrators, und teilen Sie dem IT-Team Ihres Unternehmens in einer E-Mail mit, dass Sie Ihr Kennwort zurücksetzen möchten, damit es dann dieses Feature für Sie aktivieren kann. |
| Nach dem Eingeben meiner Benutzer-ID erhalte ich die Fehlermeldung „Das Konto konnte nicht überprüft werden“. | Das Konto konnte nicht überprüft werden. <br> <br> Wenn Sie möchten, können wir einen Administrator in Ihrer Organisation kontaktieren, um Ihr Kennwort für Sie zurückzusetzen. | Diese Meldung wird angezeigt, weil die Kennwortzurücksetzung für Sie zwar aktiviert ist, Sie sich aber für die Nutzung dieses Diensts noch nicht registriert haben. Registrieren Sie sich für die Kennwortzurücksetzung unter http://aka.ms/ssprsetup, sobald der Zugriff auf Ihr Konto wieder möglich ist. <br> <br> Klicken Sie zum Zurücksetzen Ihres Kennworts auf den Link zum Kontaktieren eines Administrators, um eine E-Mail an das IT-Team Ihres Unternehmens zu senden. |

## <a name="next-steps"></a>Nächste Schritte

* [Ändern Ihres Kennworts mithilfe der Self-Service-Kennwortzurücksetzung](active-directory-passwords-update-your-own-password.md)
* [Registrierungsseite für die Kennwortzurücksetzung](http://aka.ms/ssprsetup)
* [Portal für die Kennwortzurücksetzung](https://passwordreset.microsoftonline.com/)
* [Wenn Sie sich nicht bei Ihrem Microsoft-Konto anmelden können](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant)

[Register]: ./media/active-directory-passwords-reset-register/register-2-methods.png "Registrierungsseite für die Kennwortzurücksetzung mit registrierten Methoden und der Schaltfläche „Fertig stellen“"


