---
title: "Übersicht – Azure AD B2C | Microsoft-Dokumentation"
description: Entwickeln von kundenorientierten Anwendungen mit Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: c465dbde-f800-4f2e-8814-0ff5f5dae610
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/06/2016
ms.author: saeedakhter-msft
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 44d5d31d49c375c802a67511d1f962df20656559
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="azure-ad-b2c-focus-on-your-app-let-us-worry-about-sign-up-and-sign-in"></a>Azure AD B2C: Konzentrieren Sie sich auf Ihre App, wir kümmern uns um Registrierung und Anmeldung

Azure AD B2C ist eine Lösung zur Cloudidentitätsverwaltung für Ihre Webanwendungen und mobilen Anwendungen. Es ist ein hochverfügbarer globaler Dienst, der für Hunderte Millionen Identitäten skaliert werden kann. Azure AD B2C basiert auf einer sicheren Plattform für Unternehmen und schützt Ihre Anwendungen, Ihr Unternehmen und Ihre Kunden.

Mit minimalem Konfigurationsaufwand ermöglicht Azure AD B2C Ihrer Anwendung die Authentifizierung:

* **Konten sozialer Netzwerke** (wie Facebook, Google, LinkedIn usw.)
* **Unternehmenskonten** (mit offenen Standardprotokollen, OpenID Connect oder SAML)
* **Lokale Konten** (E-Mail-Adresse und Kennwort oder Benutzername und Kennwort)

## <a name="get-started"></a>Erste Schritte

Erstellen Sie zuerst einen eigenen Mandanten mithilfe der unter [Erstellen eines Azure AD B2C-Mandanten](active-directory-b2c-get-started.md) beschriebenen Schritte.

Wählen Sie dann Ihr Szenario für die Anwendungsentwicklung aus:

|  |  |  |  |
| --- | --- | --- | --- |
| <center>![Mobile Apps und Desktop-Apps](../active-directory/develop/media/active-directory-developers-guide/NativeApp_Icon.png)<br />Mobile Apps und Desktop-Apps</center> | [Übersicht](active-directory-b2c-reference-oauth-code.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br /><br />[iOS](https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal)<br /><br />[Android](https://github.com/Azure-Samples/active-directory-b2c-android-native-msal) | [.NET](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop)<br /><br />[Xamarin](https://github.com/Azure-Samples/active-directory-b2c-xamarin-native) |  |
| <center>![Web-Apps](../active-directory/develop/media/active-directory-developers-guide/Web_app.png)<br />Web-Apps</center> | [Übersicht](active-directory-b2c-reference-oidc.md)<br /><br />[ASP.NET](active-directory-b2c-devquickstarts-web-dotnet-susi.md)<br /><br />[ASP.NET Core](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapp) | [Node.js](active-directory-b2c-devquickstarts-web-node.md) |  |
| <center>![Einzelseiten-Apps](../active-directory/develop/media/active-directory-developers-guide/SPA.png)<br />Einzelseiten-Apps</center> | [Übersicht](active-directory-b2c-reference-spa.md)<br /><br />[JavaScript](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp)<br /><br /> |  |  |
| <center>![Web-APIs](../active-directory/develop/media/active-directory-developers-guide/Web_API.png)<br />Web-APIs</center> | [ASP.NET](active-directory-b2c-devquickstarts-api-dotnet.md)<br /><br /> [ASP.NET Core](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi)<br /><br /> [Node.js](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi) | [Aufrufen einer .NET-Web-API](active-directory-b2c-devquickstarts-web-api-dotnet.md) |

## <a name="whats-new"></a>Neuigkeiten

Sie sollten diese Seite häufiger aufrufen, da Informationen über zukünftige Änderungen an Azure Active Directory B2C hier aufgeführt werden. Unter @AzureAD veröffentlichen wir außerdem Tweets zu Updates.

* Zusätzlich zu „Integrierten Richtlinien“ (Allgemeine Verfügbarkeit) sind nun auch die [„Benutzerdefinierten Richtlinien“](active-directory-b2c-overview-custom.md) in der öffentlichen Vorschau verfügbar.  Benutzerdefinierte Richtlinien sind für Identitätsexperten gedacht, die Kontrolle über die Zusammensetzung ihrer Identitätsoberfläche benötigen.
* Die Funktion [Zugriffstoken](https://azure.microsoft.com/en-us/blog/azure-ad-b2c-access-tokens-now-in-public-preview) ist nun in der öffentlichen Vorschau verfügbar.
* Die [allgemeine Verfügbarkeit von Europa-basierten Azure AD B2C-Verzeichnissen](https://azure.microsoft.com/en-us/blog/azuread-b2c-ga-eu/) wurde angekündigt.
* Sehen Sie sich unsere wachsende Bibliothek von [Codebeispielen auf Github](https://github.com/Azure-Samples?q=b2c) an.

## <a name="how-to-articles"></a>Anleitungen

Informieren Sie sich über die Verwendung bestimmter Azure Active Directory B2C-Features:

* Konfigurieren Sie, dass Konten bei [Facebook](active-directory-b2c-setup-fb-app.md), [Google+](active-directory-b2c-setup-goog-app.md), [Microsoft](active-directory-b2c-setup-msa-app.md), [Amazon](active-directory-b2c-setup-amzn-app.md) und [LinkedIn](active-directory-b2c-setup-li-app.md) in Ihren Anwendungen für Endkunden verwendet werden können.
* [Verwenden Sie benutzerdefinierte Attribute zum Erfassen von Informationen über Ihre Kunden](active-directory-b2c-reference-custom-attr.md).
* [Aktivieren Sie Azure Multi-Factor Authentication in Ihren kundenorientierten Anwendungen](active-directory-b2c-reference-mfa.md).
* [Richten Sie die Self-Service-Kennwortzurücksetzung für Kunden ein](active-directory-b2c-reference-sspr.md).
* [Passen Sie das Aussehen und Verhalten der Seiten für die Registrierung und Anmeldung und anderer Seiten für Kunden an](active-directory-b2c-reference-ui-customization.md), die von Azure Active Directory B2C bereitgestellt werden.
* [Verwenden Sie die Azure Active Directory Graph-API, um Kunden im Azure Active Directory B2C-Mandanten programmgesteuert zu erstellen, zu lesen, zu aktualisieren und zu löschen.](active-directory-b2c-devquickstarts-graph-dotnet.md)

## <a name="next-steps"></a>Nächste Schritte

Unter den folgenden Links finden Sie ausführliche Informationen zum Dienst:

* Hier finden Sie [Azure Active Directory B2C-Preisinformationen](https://azure.microsoft.com/pricing/details/active-directory-b2c/).
* Sehen Sie sich unsere [Codebeispiele](https://azure.microsoft.com/en-us/resources/samples/?service=active-directory&term=b2c) für Azure Active Directory B2C an. 
* Verwenden Sie das Tag [azure-ad-b2c](http://stackoverflow.com/questions/tagged/azure-ad-b2c), um Hilfe zu Stack Overflow zu erhalten.
* Teilen Sie uns Ihre Meinung über [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c) mit.
* Lesen Sie die [Referenz zu Azure AD B2C-Protokollen](active-directory-b2c-reference-protocols.md).
* Lesen Sie die [Referenz zu Azure AD B2C-Token](active-directory-b2c-reference-tokens.md).
* Lesen Sie die [häufig gestellten Fragen zu Azure Active Directory B2C](active-directory-b2c-faqs.md).
* [Senden von Supportanfragen für Azure Active Directory B2C](active-directory-b2c-support.md)

## <a name="get-security-updates-for-our-products"></a>Abrufen von Sicherheitsupdates für unsere Produkte

Wir empfehlen Ihnen, den Erhalt von Benachrichtigungen zu Sicherheitsvorfällen einzurichten. Rufen Sie dazu [diese Seite](https://technet.microsoft.com/security/dd252948) auf, und abonnieren Sie Sicherheitsempfehlungen.


