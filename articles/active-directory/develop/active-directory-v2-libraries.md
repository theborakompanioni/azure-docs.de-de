---
title: Azure Active Directory v2.0-Authentifizierungsbibliotheken | Microsoft Docs
description: "Enthält eine Liste mit den kompatiblen Clientbibliotheken und Bibliotheken der Servermiddleware sowie Links zu verwandten Bibliotheken, Quellen und Beispielen für den Azure Active Directory v2.0-Endpunkt."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 19cec615-e51f-4141-9f8c-aaf38ff9f746
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: f0dcb0a9a7c2c9fd362f0ed242471f6c4e495234
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="azure-active-directory-v20-authentication-libraries"></a>Azure Active Directory v2.0-Authentifizierungsbibliotheken
Der Azure Active Directory v2.0-Endpunkt (Azure AD) unterstützt die Protokolle OAuth 2.0 und OpenID Connect 1.0 nach Branchenstandard. Sie können die verschiedenen Bibliotheken von Microsoft und anderen Organisationen mit dem v2.0-Endpunkt verwenden.

Beim Entwickeln einer Anwendung, die den v2.0-Endpunkt verwendet, wird empfohlen, von Protokolldomänenexperten geschriebene Bibliotheken zu verwenden, die einer SDL-Methodik (Security Development Lifecycle) folgen, beispielsweise [der von Microsoft angewendeten Methodik][Microsoft-SDL]. Wenn Sie die Unterstützung für die Protokolle selbst schreiben möchten, wird empfohlen, sich nach der SDL-Methodik zu richten und die Sicherheitsaspekte in den Standardspezifikationen für die einzelnen Protokolle sorgfältig zu beachten.

## <a name="types-of-libraries"></a>Bibliothekstypen
Für Azure AD v2.0 können zwei Arten von Bibliotheken verwendet werden:

* **Clientbibliotheken**. Für native Clients und Server werden Clientbibliotheken verwendet, um den Zugriff auf Token zum Aufrufen einer Ressource zu erhalten, z.B. Microsoft Graph.
* **Bibliotheken der Servermiddleware**. Web-Apps nutzen die Bibliotheken der Servermiddleware für die Benutzeranmeldung. Web-APIs nutzen die Bibliotheken der Servermiddleware zum Überprüfen von Token, die von nativen Clients oder anderen Servern gesendet werden.

## <a name="library-support"></a>Bibliotheksunterstützung
Da Sie sich bei Verwendung des v2.0-Endpunkts für eine beliebige standardkonforme Bibliothek entscheiden können, ist es wichtig zu wissen, wo Sie Hilfe erhalten. Wenden Sie sich bei Problemen und Featureanfragen in Bezug auf den Bibliothekscode an den Bibliotheksbesitzer. Wenden Sie sich bei Problemen und Featureanfragen in Bezug auf die dienstseitige Protokollimplementierung an Microsoft.

Bibliotheken werden in zwei Kategorien unterteilt:

* **Von Microsoft unterstützt**. Microsoft bietet Fixes für diese Bibliotheken an und hat sie in Bezug auf die SDL-Anforderungen umfassend geprüft.
* **Kompatibel**. Microsoft hat diese Bibliotheken in grundlegenden Szenarien getestet und bestätigt, dass sie mit dem v2.0-Endpunkt funktionieren. Microsoft stellt keine Fehlerbehebungen für diese Bibliotheken bereit und hat diese Prüfungen keinerlei Prüfungen unterzogen. Bei Problemen und Featureanfragen sollten Sie sich an die Community für das Open Source-Projekt der Bibliothek wenden.

In den nächsten Abschnitten dieses Artikels finden Sie eine Liste mit den Bibliotheken, die mit dem v2.0-Endpunkt verwendet werden können.


## <a name="microsoft-supported-client-libraries"></a>Von Microsoft unterstützte Clientbibliotheken

> [!IMPORTANT]
> Die MSAL-Vorschaubibliotheken eignen sich für den Einsatz in einer Produktionsumgebung. Wir bieten die gleiche Art von Produktionsunterstützung für diese Bibliotheken wie für unsere aktuellen Produktionsbibliotheken (ADAL). In der Vorschauphase nehmen wir ggf. ohne vorherige Ankündigung Änderungen an der MSAL-API, am internen Cacheformat und an anderen Mechanismen dieser Bibliotheken vor, die neben Fehlerkorrekturen und Verbesserungen von Features erfolgen. Dies kann sich auf Ihre Anwendung auswirken. Eine Änderung am Cacheformat kann sich z.B. bei Ihren Benutzern dadurch bemerkbar machen, dass sie sich erneut anmelden müssen. Eine API-Änderung kann das Aktualisieren Ihres Codes erforderlich machen. Sobald wir die allgemein verfügbare Version veröffentlichen, müssen Sie binnen sechs Monaten ein Update auf diese Version ausführen, da Anwendungen, die mit einer Vorschauversion der Bibliothek geschrieben wurden, ggf. nicht mehr funktionieren.

| Plattform | Bibliothek | Herunterladen | Quellcode | Beispiel | Referenz
| --- | --- | --- | --- | --- | --- |
| .NET-Client, Windows Store, UWP, Xamarin iOS und Android | MSAL .NET (Vorschau) |[NuGet](https://www.nuget.org/packages/Microsoft.Identity.Client) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) | [Desktop-App](guidedsetups/active-directory-mobileanddesktopapp-windowsdesktop-intro.md) |  |
| JavaScript | MSAL.js (Vorschau) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) | [Einzelseiten-App](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2) |  |
| iOS, macOS | MSAL (Vorschau) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) | [iOS-App](https://github.com/Azure-Samples/active-directory-msal-ios-swift) |  |
| Android | MSAL (Vorschau) | [Das zentrale Repository](https://repo1.maven.org/maven2/com/microsoft/identity/client/msal/) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android) | [Android-App](guidedsetups/active-directory-mobileanddesktopapp-android-intro.md) | [JavaDocs](http://javadoc.io/doc/com.microsoft.identity.client/msal) |

## <a name="microsoft-supported-server-middleware-libraries"></a>Von Microsoft unterstützte Bibliotheken der Servermiddleware

| Plattform | Bibliothek | Herunterladen | Quellcode | Beispiel | Referenz
| --- | --- | --- | --- | --- | --- |
| .NET 4.x | OWIN OpenID Connect-Middleware |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[CodePlex](http://katanaproject.codeplex.com) |[MVC-App](guidedsetups/active-directory-serversidewebapp-aspnetwebappowin-intro.md) | |
| .NET 4.x | OWIN OAuth Bearer-Middleware für Azure AD |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[CodePlex](http://katanaproject.codeplex.com) |  | |
| .NET 4.x | JWT-Handler für .NET 4.5 | [NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt/4.0.4.403061554) | [GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| .NET Core | ASP.NET OpenID Connect-Middleware |[Microsoft.AspNetCore.Authentication.OpenIdConnect (NuGet)][ServerLib-NetCore-Owin-Oidc-Lib] |[ASP.NET Security (GitHub)][ServerLib-NetCore-Owin-Oidc-Repo] |[MVC-App](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2) |
| .NET Core | ASP.NET OAuth Bearer-Middleware |[Microsoft.AspNetCore.Authentication.OAuth (NuGet)][ServerLib-NetCore-Owin-Oauth-Lib] |[ASP.NET Security (GitHub)][ServerLib-NetCore-Owin-Oauth-Repo] |  |
| .NET Core | JWT-Handler für .NET Core  |[NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| Node.js |Azure AD Passport |[npm](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Web-App](active-directory-v2-devquickstarts-node-web.md)| |

## <a name="compatible-client-libraries"></a>Kompatible Clientbibliotheken
| Plattform | Name der Bibliothek | Getestete Version | Quellcode | Beispiel |
|:---:|:---:|:---:|:---:|:---:|
| Android |[OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib/wiki) |0.2.1 |[OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib) |[Beispiel mit nativer App](active-directory-v2-devquickstarts-android.md) |
| iOS |[NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) |1.2.8 |[NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) |[Beispiel mit nativer App](active-directory-v2-devquickstarts-ios.md) |
| JavaScript |[Hello.js](https://adodson.com/hello.js/) |1.13.5 |[Hello.js](https://github.com/MrSwitch/hello.js) |[SPA](https://github.com/Azure-Samples/active-directory-javascript-graphapi-web-v2) |

## <a name="compatible-server-middleware-libraries"></a>Kompatible Bibliotheken der Servermiddleware
| Plattform | Name der Bibliothek | Getestete Version | Quellcode | Beispiel |
|:---:|:---:|:---:|:---:|:---:|
| Java | [Scribe Java scribejava](https://github.com/scribejava/scribejava) | [Version 3.2.0](https://github.com/scribejava/scribejava/releases/tag/scribejava-3.2.0) | [ScribeJava](https://github.com/scribejava/scribejava/archive/scribejava-3.2.0.zip) | |
| PHP | [The PHP League oauth2-Client](https://github.com/thephpleague/oauth2-client) | [Version 1.4.2](https://github.com/thephpleague/oauth2-client/releases/tag/1.4.2) | [oauth2-Client](https://github.com/thephpleague/oauth2-client/archive/1.4.2.zip) | |
| Python-Flask |[Flask-OAuthlib](https://github.com/lepture/flask-oauthlib) |0.9.3 |[Flask-OAuthlib](https://github.com/lepture/flask-oauthlib) |[Web App](https://github.com/Azure-Samples/active-directory-python-flask-graphapi-web-v2) |
| Ruby |[OmniAuth](https://github.com/omniauth/omniauth/wiki) |omniauth:1.3.1</br>omniauth-oauth2:1.4.0 |[OmniAuth](https://github.com/omniauth/omniauth)</br>[OmniAuth OAuth2](https://github.com/intridea/omniauth-oauth2) |  |

## <a name="related-content"></a>Verwandte Inhalte
Weitere Informationen zum Azure AD v2.0-Endpunkt finden Sie in der [Übersicht über das Azure AD-App-Modell (v2.0)][AAD-App-Model-V2-Overview].

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: ../active-directory-appmodel-v2-overview.md
[ClientLib-NET-Lib]: http://www.nuget.org/packages/Microsoft.Identity.Client
[ClientLib-NET-Repo]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[ClientLib-NET-Sample]: active-directory-v2-devquickstarts-wpf.md
[ClientLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ClientLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad
[ClientLib-Node-Sample]:/
[ClientLib-Iosmac-Lib]:/
[ClientLib-Iosmac-Repo]:/
[ClientLib-Iosmac-Sample]:/
[ClientLib-Android-Lib]:/
[ClientLib-Android-Repo]:/
[ClientLib-Android-Sample]:/
[ClientLib-Js-Lib]:/
[ClientLib-Js-Repo]:/
[ClientLib-Js-Sample]:/

[Microsoft-SDL]: http://www.microsoft.com/sdl/default.aspx
[ServerLib-Net4-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/
[ServerLib-Net4-Owin-Oidc-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oidc-Sample]: active-directory-v2-devquickstarts-dotnet-web.md
[ServerLib-Net4-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OAuth/
[ServerLib-Net4-Owin-Oauth-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oauth-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-dotnet-api/
[ServerLib-Net-Jwt-Lib]: https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt
[ServerLib-Net-Jwt-Repo]: https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet
[ServerLib-Net-Jwt-Sample]:/
[ServerLib-NetCore-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OpenIdConnect/
[ServerLib-NetCore-Owin-Oidc-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oidc-Sample]: https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2
[ServerLib-NetCore-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OAuth/
[ServerLib-NetCore-Owin-Oauth-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oauth-Sample]:/
[ServerLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ServerLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad/
[ServerLib-Node-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-node-web/

