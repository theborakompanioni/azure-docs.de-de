---
title: "Multi-Factor Authentication für Benutzer der Azure Active Directory B2B-Zusammenarbeit | Microsoft-Dokumentation"
description: "Azure Active Directory B2B-Zusammenarbeit unterstützt die Multi-Factor Authentication (MFA) für selektiven Zugriff auf Ihre Unternehmensanwendungen."
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 02/09/2017
ms.author: sasubram
translationtype: Human Translation
ms.sourcegitcommit: a4e59dfa8a098f63c3173176c4d2675d6a59af00
ms.openlocfilehash: 894f94fdefe081679b1e35183bd4127be35cd33c


---

# <a name="multi-factor-authentication-for-azure-active-directory-b2b-collaboration-users"></a>Multi-Factor Authentication für Benutzer der Azure Active Directory B2B-Zusammenarbeit

Mit dieser aktualisierten öffentlichen Vorschau der Azure AD B2B-Zusammenarbeit führen wir eine Funktion ein, mit der Organisationen die MFA-Richtlinien (Multi-Factor Authentication) auch für B2B-Zusammenarbeitsbenutzer erzwingen können. In dieser aktualisierten Version wird die MFA immer im Ressourcenmandanten erzwungen.

Dies bedeutet Folgendes:
1. Ein Administrator oder Information-Worker in Unternehmen A lädt Benutzer aus Unternehmen B zu einer Anwendung Foo in Unternehmen A ein.
2. Anwendung *Foo* in Unternehmen A ist so konfiguriert, dass für den Zugriff eine MFA erforderlich ist.
3. Wenn ein Benutzer aus Unternehmen B versucht, auf die Anwendung Foo im Mandanten von Unternehmen A zuzugreifen, wird er aufgefordert, ein MFA-Captcha bereitzustellen, wie durch die MFA-Richtlinie von Unternehmen A vorgesehen.
4. Der Benutzer kann die MFA für Unternehmen A einrichten und dessen MFA-Option auswählen.
5. Dies funktioniert für alle Identitäten (z.B. Azure AD oder MSA, wenn Benutzer in Unternehmen B sich mithilfe der ID eines sozialen Mediums authentifizieren).
6. Unternehmen A muss über entsprechende Azure AD Premium-SKUs verfügen, die eine MFA unterstützen. Der Benutzer aus Unternehmen B nutzt diese Lizenz von Unternehmen A.
7. Zusammengefasst: Der einladende Mandant ist *immer* für die MFA der B2B-Zusammenarbeitsbenutzer aus der Partnerorganisation zuständig, nicht die Partnerorganisation (selbst wenn diese über MFA-Funktionen verfügt). In zukünftigen Versionen wird es möglich sein, dass die einladende Organisation den MFA-Funktionen bestimmter Partnerorganisationen vertraut, statt die eigenen MFA-Mechanismen verwenden zu müssen.

## <a name="setting-up-mfa-for-b2b-collaboration-users"></a>Einrichten der MFA für Benutzer der B2B-Zusammenarbeit
Um herauszufinden, wie einfach sich die MFA für B2B-Zusammenarbeitsbenutzer einrichten lässt, sehen Sie sich [dieses Video](https://channel9.msdn.com/Blogs/Azure/b2b-conditional-access-setup) an.

## <a name="b2b-collaboration-users-mfa-experience-for-offer-redemption"></a>MFA-Mechanismen für Benutzer der B2B-Zusammenarbeit beim Einlösen von Angeboten
Sehen Sie sich [dieses Video](https://channel9.msdn.com/Blogs/Azure/MFA-redemption) an, das in einer Animation veranschaulicht, wie die Einlösung von Einladungen funktioniert.

## <a name="mfa-reset-for-b2b-collaboration-users"></a>Zurücksetzen der MFA für B2B-Zusammenarbeitsbenutzer
Zurzeit kann ein Administrator eine erneute Authentifizierung von B2B-Zusammenarbeitsbenutzern nur mithilfe von PowerShell-Cmdlets anfordern. Daher sollten die folgenden PowerShell-Cmdlets verwendet werden, wenn Sie die Authentifizierungsmethode eines B2B-Zusammenarbeitsbenutzers zurücksetzen möchten.

> [!NOTE]
> Um das neue Cmdlet zu verwenden, müssen Sie das Azure AD PowerShell V2-Modul installieren, das Sie hier herunterladen können: https://www.powershellgallery.com/packages/AzureADPreview

1. Stellen Sie eine Verbindung mit Azure AD her.

  ```
  Connect-MsolService and login
  ```
2. Rufen Sie alle Benutzer mit Authentifizierungsmethoden ab.

  ```
  Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```
  Beispiel:

  ```
  PS C:\Users\tjwasser> Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName,
  @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```

3. Setzen Sie die MFA-Methode für einen bestimmten Benutzer zurück. Danach können Sie diesen UserPrincipalName verwenden, um den Zurücksetzungsbefehl auszuführen, mit dem der B2B-Zusammenarbeitsbenutzer aufgefordert wird, seine Authentifizierungsmethoden erneut einzurichten. Beispiel:

  ```
  Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName gsamoogle_gmail.com#EXT#@ WoodGroveAzureAD.onmicrosoft.com
  ```

## <a name="next-steps"></a>Nächste Schritte

Weitere Artikel zur Azure AD B2B-Kollaboration:

* [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Wie fügen Azure Active Directory-Administratoren B2B-Zusammenarbeitsbenutzer hinzu?](active-directory-b2b-admin-add-users.md)
* [Wie fügen Information-Worker B2B-Zusammenarbeitsbenutzer hinzu?](active-directory-b2b-iw-add-users.md)
* [Die Elemente der Einladungs-E-Mail von B2B-Zusammenarbeit](active-directory-b2b-invitation-email.md)
* [B2B-Zusammenarbeit: Einlösen von Einladungen](active-directory-b2b-redemption-experience.md)
* [Lizenzierung von Azure AD B2B-Zusammenarbeit](active-directory-b2b-licensing.md)
* [Problembehandlung für die Azure Active Directory B2B-Zusammenarbeit](active-directory-b2b-troubleshooting.md)
* [Häufig gestellte Fragen zur Azure Active Directory B2B-Zusammenarbeit](active-directory-b2b-faq.md)
* [Azure Active Directory B2B-Zusammenarbeit: API und Anpassung](active-directory-b2b-api.md)
* [Hinzufügen von Benutzern der B2B-Zusammenarbeit ohne Einladung](active-directory-b2b-add-user-without-invite.md)
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)



<!--HONumber=Feb17_HO2-->


