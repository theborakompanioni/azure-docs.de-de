---
title: "Bedingter Zugriff für Benutzer der Azure Active Directory B2B-Zusammenarbeit | Microsoft-Dokumentation"
description: "Azure Active Directory B2B-Zusammenarbeit unterstützt die Multi-Factor Authentication (MFA) für den selektiven Zugriff auf Ihre Unternehmensanwendungen."
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
ms.date: 04/11/2017
ms.author: sasubram
translationtype: Human Translation
ms.sourcegitcommit: 7f469fb309f92b86dbf289d3a0462ba9042af48a
ms.openlocfilehash: fac4657e9b78e900c052384e212aa9f3915f970d
ms.lasthandoff: 04/13/2017


---

# <a name="conditional-access-for-b2b-collaboration-users"></a>Bedingter Zugriff für Benutzer der B2B-Zusammenarbeit

## <a name="multi-factor-authentication-for-b2b-users"></a>Multi-Factor Authentication für B2B-Benutzer
Azure AD B2B-Zusammenarbeit bietet Organisationen die einzigartige Möglichkeit, Multi-Factor Authentication-Richtlinien für B2B-Benutzer zu erzwingen. Diese Richtlinien können auf Mandantenebene, App-Ebene oder der Ebene einzelner Benutzer auf die gleiche Weise erzwungen werden, wie sie für Vollzeitmitarbeiter und Mitglieder der Organisation aktiviert werden können. MFA-Richtlinien werden in der Ressourcenorganisation erzwungen.

Dies bedeutet Folgendes:
1. Ein Administrator oder Information-Worker in Unternehmen A lädt Benutzer aus Unternehmen B zu einer Anwendung Foo in Unternehmen A ein.
2. Anwendung *Foo* in Unternehmen A ist so konfiguriert, dass für den Zugriff eine MFA erforderlich ist.
3. Wenn ein Benutzer aus Unternehmen B versucht, auf die Anwendung Foo im Mandanten von Unternehmen A zuzugreifen, wird er aufgefordert, ein MFA-Captcha bereitzustellen, wie durch die MFA-Richtlinie von Unternehmen A vorgesehen.
4. Der Benutzer kann die MFA für Unternehmen A einrichten und dessen MFA-Option auswählen.
5. Dies funktioniert für alle Identitäten (z.B. Azure AD oder MSA, wenn Benutzer in Unternehmen B sich mithilfe der ID eines sozialen Mediums authentifizieren).
6. Unternehmen A muss über entsprechende Azure AD Premium-SKUs verfügen, die eine MFA unterstützen. Der Benutzer aus Unternehmen B nutzt diese Lizenz von Unternehmen A.

Zusammengefasst: Der einladende Mandant ist *immer* für die MFA der B2B-Zusammenarbeitsbenutzer aus der Partnerorganisation zuständig, nicht die Partnerorganisation (selbst wenn diese über MFA-Funktionen verfügt).

### <a name="setting-up-mfa-for-b2b-collaboration-users"></a>Einrichten der MFA für Benutzer der B2B-Zusammenarbeit
Um herauszufinden, wie einfach sich die MFA für B2B-Zusammenarbeitsbenutzer einrichten lässt, sehen Sie sich das folgende Video an:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-conditional-access-setup/Player]

### <a name="b2b-users-mfa-experience-for-offer-redemption"></a>MFA-Mechanismen für B2B-Benutzer beim Einlösen von Einladungen
Sehen Sie sich das folgende Video an, das in einer Animation veranschaulicht, wie das Einlösen von Einladungen funktioniert:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/MFA-redemption/Player]

### <a name="mfa-reset-for-b2b-collaboration-users"></a>Zurücksetzen der MFA für B2B-Zusammenarbeitsbenutzer
Zurzeit kann ein Administrator eine erneute Authentifizierung von B2B-Zusammenarbeitsbenutzern nur mithilfe von PowerShell-Cmdlets anfordern. Daher sollten die folgenden PowerShell-Cmdlets verwendet werden, wenn Sie die Authentifizierungsmethode eines B2B-Zusammenarbeitsbenutzers zurücksetzen möchten.

1. Mit Azure AD verbinden

  ```
  $cred = Get-Credential
  Connect-MsolService -Credential $cred
  ```
2. Rufen Sie alle Benutzer mit Authentifizierungsmethoden ab.

  ```
  Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```
  Beispiel:

  ```
  PS C:\Users\tjwasserGet-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```

3. Setzen Sie die MFA-Methode für einen bestimmten Benutzer zurück. Danach können Sie diesen UserPrincipalName verwenden, um den Zurücksetzungsbefehl auszuführen, mit dem der B2B-Zusammenarbeitsbenutzer aufgefordert wird, seine Authentifizierungsmethoden erneut einzurichten. Beispiel:

  ```
  Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName gsamoogle_gmail.com#EXT#@ WoodGroveAzureAD.onmicrosoft.com
  ```

### <a name="why-do-we-perform-mfa-at-the-resource-tenancy"></a>Warum wird MFA im Ressourcenmandanten ausgeführt?

In der aktuellen Version wird MFA immer im Ressourcenmandanten ausgeführt. Dies erfolgt aus Gründen der Vorhersagbarkeit.

Nehmen wir beispielsweise an, dass ein Contoso-Benutzer (Sally) zu Fabrikam eingeladen wird und Fabrikam MFA für B2B-Benutzer aktiviert hat.

Nehmen wir außerdem an, dass Contoso eine MFA-Richtlinie für App1, jedoch nicht für App2 aktiviert hat. Wenn wir dann anhand des Contoso-MFA-Anspruchs im Token bestimmen, ob Sally in Fabrikam MFA ausführen muss, kann das folgende Problem auftreten:

* Tag 1: Sally hat in Contoso MFA ausgeführt, da sie auf App1 zugreift. In Fabrikam wird dann für sie keine MFA-Aufforderung angezeigt.

* Tag 2: Sally hat in Contoso auf App 2 zugegriffen und muss sich deshalb in Fabrikam für MFA registrieren, wenn sie auf Fabrikam zugreift.

Dies kann für den Sally recht verwirrend sein und führt wahrscheinlich zu einer geringeren Anzahl abgeschlossener Anmeldungen.

Außerdem vertraut Fabrikam nicht immer der MFA-Richtlinie von Contoso, selbst wenn Contoso die MFA-Funktionen nutzen kann.

Zudem kann die Ressourcenmandanten-MFA auch für MSAs und IDs für soziale Netzwerke als Identitätsanbieter sowie für Partnerorganisationen, die keine MFA eingerichtet haben, verwendet werden.

Deshalb wird im Hinblick auf MFA für B2B-Benutzer empfohlen, immer Ressourcen-MFA erforderlich zu machen. Dies kann in manchen Fällen zu doppelter MFA führen, jedoch ist bei jedem Zugriff auf einen Ressourcenmandanten die Endbenutzererfahrung vorhersagbar: Sally muss sich beim Ressourcenmandanten für MFA registrieren.

### <a name="device-location-and-risk-based-conditional-access-for-b2b-users"></a>Bedingter Zugriff für B2B-Benutzer auf Grundlage von Gerät, Standort und Risiko

Wenn die Contoso-Organisation gerätebasierte Richtlinien für den bedingten Zugriff auf ihre Unternehmensdaten aktiviert, wird der Zugriff von nicht verwalteten Geräten (d. h. von Geräten, die nicht von der Contoso-Organisation verwaltet werden und nicht die Contoso-Geräterichtlinien erfüllen) verhindert.

Wenn das Gerät des B2B-Benutzers nicht von Contoso verwaltet wird, wird der Zugriff von B2B-Benutzern aus den Partnerorganisationen in jedem Kontext, in dem diese Richtlinien erzwungen werden, blockiert.

Es ist eine hohe Anforderung, von Benutzern aus einer anderen Organisation zu verlangen, dass sie ihr Gerät von der einladenden Organisation verwalten lassen. Aus diesem Grund ermöglichen wir in zukünftigen Updates, dass Contoso auf die Gerätekonformität bestimmter Partner vertrauen kann. Dies ermöglicht Contoso das Erzwingen von Richtlinien, die zulassen, dass ein Benutzer von Fabrikam auf Contoso-Ressourcen zugreifen kann, wenn diese ebenfalls ein von Fabrikam verwaltetes Gerät verwenden.

In der Zwischenzeit kann Contoso Ausschlusslisten erstellen, die bestimmte Partnerbenutzer enthalten, die gemäß der gerätebasierten Richtlinie für bedingten Zugriff ausgeschlossen werden sollen.

#### <a name="location-based-conditional-access-for-b2b"></a>Standortbasierter bedingter Zugriff für B2B

Für B2B-Benutzer können standortbasierte Richtlinien für bedingten Zugriff erzwungen werden, wenn die einladende Organisation (z.B. Contoso) einen vertrauenswürdigen Netzwerkumkreis (d. h. einen IP-Adressbereich) erstellen kann, der ihre Partnerorganisationen (z.B. Fabrikam) definiert.

#### <a name="risk-based-conditional-access-for-b2b"></a>Risikobasierter bedingter Zugriff für B2B

Auf den Risiken bei der Anmeldung basierende Richtlinien können derzeit nicht auf B2B-Benutzer angewendet werden, da die Risikobewertung in der eigenen Organisation des B2B-Benutzers (also im Identitätsmandanten des B2B-Benutzers) erfolgt.

Wir erwägen für zukünftige Updates, die Risikobewertung von Partnern zu bündeln (mit Einwilligung des Partners), damit Contoso seine extern freigegebenen Apps und Daten nicht nur vor bekannten Risiken, sondern auch vor Risiken schützen kann, die Contoso nicht bekannt sind, da sie an anderer Stelle auftreten.

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

