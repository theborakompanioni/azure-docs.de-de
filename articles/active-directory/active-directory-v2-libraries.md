---
title: Azure Active Directory v2.0-Authentifizierungsbibliotheken | Microsoft Docs
description: "Enthält eine Liste mit den kompatiblen Clientbibliotheken und Bibliotheken der Servermiddleware sowie Links zu verwandten Bibliotheken, Quellen und Beispielen für den Azure Active Directory v2.0-Endpunkt."
services: active-directory
documentationcenter: 
author: skwan
manager: mbaldwin
editor: 
ms.assetid: 19cec615-e51f-4141-9f8c-aaf38ff9f746
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/29/2016
ms.author: skwan;bryanla
translationtype: Human Translation
ms.sourcegitcommit: 5ba85485737a6f1ee8908834a74d34863429aeca
ms.openlocfilehash: e68a83936e6ae0b7349444cb24bafd6908166945


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
| Plattform | Name der Bibliothek | Download | Quellcode | Beispiel |
|:---:|:---:|:---:|:---:|:---:|
| .NET, Windows Store, Xamarin |MSAL (Microsoft Authentication Library) für .NET |[Microsoft.Identity.Client (NuGet)][ClientLib-NET-Lib] |[MSAL für .NET (GitHub)][ClientLib-NET-Repo] |[Beispiel mit einem nativen Windows-Desktopclient][ClientLib-NET-Sample] |
| Node.js |Microsoft Azure Active Directory-Plug-In für Passport.js |[Passport-Azure-AD (npm)][ClientLib-Node-Lib] |[Passport-Azure-AD (GitHub)][ClientLib-Node-Repo] |In Kürze verfügbar |

<!--- COMMENTING OUT UNTIL THEY ARE READY
| iOS, Mac | Microsoft Authentication Library (MSAL) for ObjC | In development | In development | In development |
| Android | Microsoft Authentication Library (MSAL) for Android | In development | In development | In development |
| JavaScript | Microsoft Authentication Library (MSAL) for JavaScript | In development | In development | In development |
 -->

## <a name="microsoft-supported-server-middleware-libraries"></a>Von Microsoft unterstützte Bibliotheken der Servermiddleware
| Plattform | Name der Bibliothek | Download | Quellcode | Beispiel |
|:---:|:---:|:---:|:---:|:---:|
| .NET 4.x |OWIN OpenID Connect Middleware für ASP.NET |[Microsoft.Owin.Security.OpenIdConnect (NuGet)][ServerLib-Net4-Owin-Oidc-Lib] |[Katana Project (CodePlex)][ServerLib-Net4-Owin-Oidc-Repo] |[Web-App-Beispiel][ServerLib-Net4-Owin-Oidc-Sample] |
| .NET 4.x |OWIN OAuth Bearer Middleware für ASP.NET |[Microsoft.Owin.Security.OAuth (NuGet)][ServerLib-Net4-Owin-Oauth-Lib] |[Katana Project (CodePlex)][ServerLib-Net4-Owin-Oauth-Repo] |[Web-API-Beispiel][ServerLib-Net4-Owin-Oauth-Sample] |
| .NET Core |OWIN OpenID Connect Middleware für .NET Core |[Microsoft.AspNetCore.Authentication.OpenIdConnect (NuGet)][ServerLib-NetCore-Owin-Oidc-Lib] |[ASP.NET Security (GitHub)][ServerLib-NetCore-Owin-Oidc-Repo] |[Web-App-Beispiel][ServerLib-NetCore-Owin-Oidc-Sample] |
| .NET Core |OWIN OAuth Bearer Middleware für .NET Core |[Microsoft.AspNetCore.Authentication.OAuth (NuGet)][ServerLib-NetCore-Owin-Oauth-Lib] |[ASP.NET Security (GitHub)][ServerLib-NetCore-Owin-Oauth-Repo] |In Kürze verfügbar |
| Node.js |Microsoft Azure Active Directory-Plug-In für Passport.js |[Passport-Azure-AD (npm)][ServerLib-Node-Lib] |[Passport-Azure-AD (GitHub)][ServerLib-Node-Repo] |[Web-App-Beispiel][ServerLib-Node-Sample] |

<!--- COMMENTING UNTIL SAMPLE IS AVAILABLE
| .NET 4.x, .NET Core | JSON Web Token Handler for .NET | [System.IdentityModel.Tokens.Jwt (NuGet)][ServerLib-Net-Jwt-Lib] | [Azure AD identity model extensions for .NET (GitHub)][ServerLib-Net-Jwt-Repo] | Coming soon |
--->
## <a name="compatible-client-libraries"></a>Kompatible Clientbibliotheken
| Plattform | Name der Bibliothek | Getestete Version | Quellcode | Beispiel |
|:---:|:---:|:---:|:---:|:---:|
| Android |[OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib/wiki) |0.2.1 |[OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib) |[Beispiel mit nativer App](active-directory-v2-devquickstarts-android.md) |
| iOS |[NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) |1.2.8 |[NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) |[Beispiel mit nativer App](active-directory-v2-devquickstarts-ios.md) |
| Java | [Scribe Java scribejava](https://github.com/scribejava/scribejava) | [Version 3.2.0](https://github.com/scribejava/scribejava/releases/tag/scribejava-3.2.0) | [ScribeJava](https://github.com/scribejava/scribejava/archive/scribejava-3.2.0.zip) | In Kürze verfügbar |
| JavaScript |[Hello.js](https://adodson.com/hello.js/) |1.13.5 |[Hello.js](https://github.com/MrSwitch/hello.js) |In Kürze verfügbar |
| PHP | [The PHP League oauth2-Client](https://github.com/thephpleague/oauth2-client) | [Version 1.4.2](https://github.com/thephpleague/oauth2-client/releases/tag/1.4.2) | [oauth2-Client](https://github.com/thephpleague/oauth2-client/archive/1.4.2.zip) | In Kürze verfügbar |
| Python-Flask |[Flask-OAuthlib](https://github.com/lepture/flask-oauthlib) |0.9.3 |[Flask-OAuthlib](https://github.com/lepture/flask-oauthlib) |Bald verfügbar |
| Ruby |[OmniAuth](https://github.com/omniauth/omniauth/wiki) |omniauth:1.3.1</br>omniauth-oauth2:1.4.0 |[OmniAuth](https://github.com/omniauth/omniauth)</br>[OmniAuth OAuth2](https://github.com/intridea/omniauth-oauth2) |Bald verfügbar |

<!--- REMOVING BRANDON'S FOR NOW
|  |  |  |  |  |
| Android | [OAuth2 Client](https://github.com/wuman/android-oauth-client) |   | [OAuth2 Client](https://github.com/wuman/android-oauth-client)  | Coming soon  |
| Java | [WSO2 Identity Server](https://docs.wso2.com/display/IS500/Introducing+the+Identity+Server) | [Version 5.2.0](http://wso2.com/products/identity-server/) | [Source](https://docs.wso2.com/display/IS500/Building+from+Source) | [Samples index](https://docs.wso2.com/display/IS500/Samples)  |
| Java | [Java Gluu Server](https://gluu.org/docs/) |   | [oxAuth](https://github.com/GluuFederation/oxAuth)  | Coming soon |
| Node.js | [NPM passport-openidconnect](https://www.npmjs.com/package/passport-openidconnect) | 0.0.1  | [Passport-OpenID Connect](https://github.com/jaredhanson/passport-openidconnect) | Coming soon  |
| PHP | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP) |   | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP)  | Coming soon  |
-->

## <a name="compatible-server-middleware-libraries"></a>Kompatible Bibliotheken der Servermiddleware
Bald verfügbar

## <a name="related-content"></a>Verwandte Inhalte
Weitere Informationen zum Azure AD v2.0-Endpunkt finden Sie in der [Übersicht über das Azure AD-App-Modell (v2.0)][AAD-App-Model-V2-Overview].

Nutzen Sie die Disqus-Kommentarfunktion am Ende dieses Artikels für Ihr Feedback, damit wir unsere Inhalte optimieren und noch besser anpassen können.

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: active-directory-appmodel-v2-overview.md
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



<!--HONumber=Nov16_HO5-->


