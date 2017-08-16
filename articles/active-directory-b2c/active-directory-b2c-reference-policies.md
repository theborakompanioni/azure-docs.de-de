---
title: 'Azure Active Directory B2C: Integrierte Richtlinien | Microsoft-Dokumentation'
description: "Ein Thema über das erweiterbare Richtlinienframework von Azure Active Directory B2C und die Erstellung verschiedener Richtlinientypen"
services: active-directory-b2c
documentationcenter: 
author: sama
manager: mbaldwin
editor: PatAltimore
ms.assetid: 0d453e72-7f70-4aa2-953d-938d2814d5a9
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: sama
ms.translationtype: HT
ms.sourcegitcommit: f5c887487ab74934cb65f9f3fa512baeb5dcaf2f
ms.openlocfilehash: daad3af089afdf76b930053728bb11a5cf4c2a92
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---
# <a name="azure-active-directory-b2c-built-in-policies"></a>Azure Active Directory B2C: Integrierte Richtlinien


Das erweiterbare Richtlinienframework von Azure Active Directory (Azure AD) B2C ist der Hauptvorteil des Diensts. Die Richtlinien beschreiben Benutzeroberflächen im Zusammenhang mit der Kundenidentität, z.B. Registrierung, Anmeldung oder Profilbearbeitung. Eine Registrierungsrichtlinie ermöglicht es z. B., das Verhalten zu steuern, indem die folgenden Einstellungen konfiguriert werden:

* Kontotypen (Konten sozialer Netzwerke wie Facebook oder lokale Konten wie die E-Mail-Adressen), die Kunden zum Registrieren für die Anwendung verwenden können
* Attribute (z.B. Vorname, Postleitzahl und Schuhgröße), die der Kunde während des Anmeldeprozesses angeben muss
* Verwendung der Azure Multi-Factor Authentication
* Das Aussehen und Verhalten aller Registrierungsseiten
* Informationen (als Ansprüche in Token ausgedrückt), die die Anwendung nach Abschluss der Richtlinienausführung erhält

Sie können mehrere Richtlinien verschiedener Typen in Ihrem Mandanten erstellen und bei Bedarf in Ihrer Anwendung verwenden. Richtlinien können anwendungsübergreifend wiederverwendet werden. Durch diese Flexibilität können Entwickler Benutzeroberflächen im Zusammenhang mit der Kundenidentität mit minimalen oder ganz ohne Änderungen an ihrem Code definieren und ändern.

Richtlinien können über eine einfache Entwicklerschnittstelle verwendet werden. Ihre Anwendung löst eine Richtlinie mithilfe einer standardmäßigen HTTP-Authentifizierungsanforderung aus (übergibt einen Richtlinienparameter in der Anforderung) und empfängt ein benutzerdefiniertes Token als Antwort. Der einzige Unterschied zwischen Anforderungen, die eine Registrierungsrichtlinie aufrufen, und Anforderungen, die eine Anmelderichtlinie aufrufen, ist beispielsweise der Richtlinienname, der im Parameter „p“ der Abfragezeichenfolge verwendet wird:

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up policy

```

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in policy

```

Weitere Informationen zum Richtlinienframework finden Sie in diesem [Blogbeitrag zu Azure AD B2C auf dem englischsprachigen Blog zu Unternehmensmobilität und Sicherheit](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx).

## <a name="create-a-sign-up-or-sign-in-policy"></a>Erstellen einer Registrierungs- oder Anmelderichtlinie

Mit dieser Richtlinie werden die Registrierung und Anmeldung von Kunden in ein und derselben Konfiguration verarbeitet. Die Kunden werden je nach Kontext auf den jeweils entsprechenden Pfad geleitet (Registrierung oder Anmeldung). Außerdem wird mit der Richtlinie der Inhalt von Token beschrieben, die die Anwendung nach erfolgreichen Registrierungen oder Abmeldungen erhält.  Ein Codebeispiel für die Richtlinie zur Registrierung oder Anmeldung ist [hier verfügbar](active-directory-b2c-devquickstarts-web-dotnet-susi.md).  Sie sollten diese Richtlinie anstelle einer Registrierungs- und Anmelderichtlinie verwenden.  

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

## <a name="create-a-sign-up-policy"></a>Erstellen einer Registrierungsrichtlinie

[!INCLUDE [active-directory-b2c-create-sign-up-policy](../../includes/active-directory-b2c-create-sign-up-policy.md)]

## <a name="create-a-sign-in-policy"></a>Erstellen einer Anmelderichtlinie

[!INCLUDE [active-directory-b2c-create-sign-in-policy](../../includes/active-directory-b2c-create-sign-in-policy.md)]

## <a name="create-a-profile-editing-policy"></a>Erstellen einer Richtlinie für die Profilbearbeitung

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

## <a name="create-a-password-reset-policy"></a>Erstellen einer Richtlinie zur Kennwortrücksetzung

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="how-do-i-link-a-sign-up-or-sign-in-policy-with-a-password-reset-policy"></a>Wie wird eine Registrierungs- oder Anmelderichtlinie mit einer Richtlinie zum Zurücksetzen von Kennwörtern verknüpft?
Beim Erstellen einer Registrierungs- oder Anmelderichtlinie (für lokale Konten) wird dem Endbenutzer auf der ersten Seite der Oberfläche der Link **Kennwort vergessen?** angezeigt. Durch Klicken auf diesen Link wird nicht automatisch eine Richtlinie zum Zurücksetzen des Kennworts ausgelöst. 

Stattdessen wird der Fehlercode **`AADB2C90118`** an die App zurückgegeben. Ihre App muss mit diesen Fehlercode verarbeiten, indem sie eine bestimmte Richtlinie zur Kennwortzurücksetzung aufruft. Weitere Informationen finden Sie in dem [Beispiel, das das Verknüpfen von Richtlinien veranschaulicht](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI).

### <a name="should-i-use-a-sign-up-or-sign-in-policy-or-a-sign-up-policy-and-a-sign-in-policy"></a>Sollte ich eine Registrierungs- oder Anmelderichtlinie verwenden oder eine Registrierungsrichtlinie und eine Anmelderichtlinie?
Es wird empfohlen, dass Sie statt zwei Richtlinien eine Registrierungs- oder Anmelderichtlinie verwenden.  

Die Registrierungs- oder Anmelderichtlinie weist mehr Funktionen als die Anmelderichtlinie auf. Außerdem ermöglicht sie Ihnen, die Seite für die Benutzeroberflächenanpassung zu nutzen, und sie hat eine bessere Unterstützung für die Lokalisierung. 

Die Anmelderichtlinie wird empfohlen, wenn Sie Ihre Richtlinien nicht lokalisieren müssen, nur geringfügige Anpassungsfunktionen für das Branding benötigen und eine integrierte Kennwortzurücksetzung möchten.

## <a name="next-steps"></a>Nächste Schritte
* [Token, Sitzung und einmaliges Anmelden – Konfiguration](active-directory-b2c-token-session-sso.md)
* [Deaktivieren der E-Mail-Überprüfung während der Registrierung von Endbenutzern](active-directory-b2c-reference-disable-ev.md)


