---
title: "Azure Active Directory für Entwickler | Microsoft-Dokumentation"
description: "Dieser Artikel enthält eine Übersicht über die Anmeldung an Geschäfts-, Schul- oder Unikonten von Microsoft mit Azure Active Directory."
services: active-directory
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 5c872c89-ef04-4f4c-98de-bc0c7460c7c2
ms.service: active-directory
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: a1ba750d2be1969bfcd4085a24b0469f72a357ad
ms.openlocfilehash: 0275bd00850a734c9b721c8348e24c5c40422201
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017

---
# <a name="azure-active-directory-for-developers"></a>Azure Active Directory für Entwickler
Azure Active Directory ist ein Cloudidentitätsdienst, mit dem Entwickler die sichere Anmeldung für alle Benutzer durchführen können, die über ein Geschäfts-, Schul- oder Unikonto von Microsoft verfügen.  Hier wird gezeigt, wie Sie Ihrer Anwendung die Azure AD-Unterstützung hinzufügen, indem Sie branchenübliche Authentifizierungsprotokolle, OAuth und OpenID Connect verwenden.

| | |
| --- | --- |
|[Authentifizierungsgrundlagen](active-directory-authentication-scenarios.md) | Einführung in die Azure AD-Authentifizierung |
|[Anwendungsarten](active-directory-authentication-scenarios.md#application-types-and-scenarios) | Übersicht über die von Azure AD unterstützten Authentifizierungsszenarien |                                
                                                                              
## <a name="get-started"></a>Erste Schritte
In diesen Einrichtungsanleitungen wird Schritt für Schritt beschrieben, wie Sie unsere Authentifizierungsbibliotheken zum Anmelden von Azure Active Directory-Benutzern verwenden.

|  |  |  |  |
| --- | --- | --- | --- |
| <center>![Mobile Apps und Desktop-Apps](./media/active-directory-developers-guide/NativeApp_Icon.png)<br />Mobile Apps und Desktop-Apps</center> | [Übersicht](active-directory-authentication-scenarios.md#native-application-to-web-api)<br /><br />[iOS](active-directory-devquickstarts-ios.md)<br /><br />[Android](active-directory-devquickstarts-android.md) | [.NET](active-directory-devquickstarts-dotnet.md)<br /><br />[Windows](active-directory-devquickstarts-windowsstore.md)<br /><br />[Xamarin](active-directory-devquickstarts-xamarin.md) | [Cordova](active-directory-devquickstarts-cordova.md)<br /><br />[OAuth 2.0](active-directory-protocols-oauth-code.md) |
| <center>![Web-Apps](./media/active-directory-developers-guide/Web_app.png)<br />Web-Apps</center> | [Übersicht](active-directory-authentication-scenarios.md#web-browser-to-web-application)<br /><br />[ASP.NET](active-directory-devquickstarts-webapp-dotnet.md)<br /><br />[Java](active-directory-devquickstarts-webapp-java.md) | [NodeJS](active-directory-devquickstarts-openidconnect-nodejs.md)<br /><br />[OpenID Connect 1.0](active-directory-protocols-openid-connect-code.md) |  |
| <center>![Einzelseiten-Apps](./media/active-directory-developers-guide/SPA.png)<br />Einzelseiten-Apps</center> | [Übersicht](active-directory-authentication-scenarios.md#single-page-application-spa)<br /><br />[AngularJS](active-directory-devquickstarts-angular.md)<br /><br />[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |  |  |
| <center>![Web-APIs](./media/active-directory-developers-guide/Web_API.png)<br />Web-APIs</center> | [Übersicht](active-directory-authentication-scenarios.md#web-application-to-web-api)<br /><br />[ASP.NET](active-directory-devquickstarts-webapi-dotnet.md)<br /><br />[NodeJS](active-directory-devquickstarts-webapi-nodejs.md) | &nbsp; |
| <center>![Dienst-zu-Dienst](./media/active-directory-developers-guide/Service_App.png)<br />Dienst-zu-Dienst</center> | [Übersicht](active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api)<br /><br />[.NET](active-directory-code-samples.md#server-or-daemon-application-to-web-api)<br /><br />[OAuth 2.0-Clientanmeldeinformationen](active-directory-protocols-oauth-service-to-service.md) |  |

## <a name="guides"></a>Anleitungen
Diese Artikel enthalten Informationen dazu, wie Sie mit Azure Active Directory häufig anfallende Aufgaben erledigen.

|                                                                           |  |
|---------------------------------------------------------------------------| --- |
|[App-Registrierung](active-directory-integrating-applications.md)           | Registrieren einer App in Azure AD |
|[Mehrinstanzenfähige Apps](active-directory-devhowto-multi-tenant-overview.md)    | Anmelden bei einem Microsoft-Arbeitskonto |
|[OAuth und OpenID Connect](active-directory-protocols-openid-connect-code.md)| Anmelden von Benutzern und Aufrufen von Web-APIs über unsere modernen Authentifizierungsprotokolle |
|[Weitere Anleitungen...](active-directory-developers-guide-index.md#guides)        |     |

## <a name="reference"></a>Referenz
Diese Artikel enthalten ausführliche Informationen zu APIs, Protokollmeldungen und Ausdrücken, die in Azure Active Directory verwendet werden.

|                                                                                   | |
| ----------------------------------------------------------------------------------| --- |
| [Authentifizierungsbibliotheken (ADAL)](active-directory-authentication-libraries.md)   | Übersicht über die von Azure AD bereitgestellten Bibliotheken und SDKs |
| [Codebeispiele](active-directory-code-samples.md)                                  | Liste mit allen Azure AD-Codebeispielen |
| [Glossar](active-directory-dev-glossary.md)                                      | Begriffe und Definitionen von Wörtern, die in dieser Dokumentation verwendet werden |
| [Weiteres Referenzmaterial...](active-directory-developers-guide-index.md#reference)|     |

## <a name="help--support"></a>Hilfe und Support
Hier sind die besten Hilferessourcen für die Entwicklung mit Azure Active Directory angegeben.

|  |  
|---|
|[Tags `azure-active-directory` und `adal` von Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory+or+adal)      |
|[Feedback zu Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences)|
| [Testen Sie Microsoft Dev Chat (für begrenzte Zeit kostenlos)](http://aka.ms/devchat) |

<br />

> [!NOTE]
> Wenn Sie die Anmeldung für persönliche Microsoft-Konten durchführen möchten, kann es ratsam sein, den [Azure AD v2.0-Endpunkt](active-directory-appmodel-v2-overview.md) zu verwenden.  Der Azure AD v2.0-Endpunkt wird für die Zusammenführung von persönlichen Microsoft-Konten und Microsoft-Geschäftskonten (aus Azure AD) in einem gemeinsamen Authentifizierungssystem genutzt.

