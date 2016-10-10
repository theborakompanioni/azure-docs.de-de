<properties
   pageTitle="Azure Active Directory v2.0-Bibliotheken | Microsoft Azure"
   description="Stellt eine Liste aller kompatiblen Clientbibliotheken und Bibliotheken der Servermiddleware bereit, gemeinsam mit Links zu verwandten Bibliotheken/Quellen/Beispielen für den Azure Active Directory v2.0-Endpunkt."
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/26/2016"
   ms.author="skwan;bryanla"/>


# Azure Active Directory (AD) v2.0 und Authentifizierungsbibliotheken
Der Azure AD v2.0-Endpunkt unterstützt die Protokolle OAuth 2.0 und OpenID Connect 1.0 nach Branchenstandard. Mit dem v2.0-Endpunkt können zahlreiche Bibliotheken von Microsoft und anderen Anbietern verwendet werden.

Beim Entwickeln einer Anwendung, die den v2.0-Endpunkt verwendet, wird empfohlen, von Domänenexperten geschriebene Bibliotheken zu verwenden, die einer SDL-Methodologie (Security Development Lifecycle) folgen, [beispielsweise der von Microsoft angewendeten Methodologie][Microsoft-SDL]. Wenn Sie die Unterstützung für die Protokolle selbst schreiben möchten, wird empfohlen, sich nach SDL zu richten und die Sicherheitsaspekte in den Standardspezifikationen für jedes der Protokolle zu beachten.

## Bibliothekstypen
Es gibt zwei Arten von Bibliotheken, die mit v2.0 funktionieren:

- **Clientbibliotheken**: Clientbibliotheken werden auf nativen Clients und auf Servern verwendet, um Zugriffstoken für den Aufruf einer Ressource abzurufen, z.B. Microsoft Graph.
- **Bibliotheken der Servermiddleware**: Bibliotheken der Servermiddleware werden von Webanwendungen verwendet, um einen Benutzer anzumelden. Web-APIs verwenden diese Bibliotheken, um Token zu überprüfen, die von nativen Clients oder anderen Servern gesendet werden.

## Bibliotheksunterstützung
Da Sie sich bei Verwendung des v2.0-Endpunkts für eine beliebige standardkonforme Bibliothek entscheiden können, ist es wichtig zu wissen, wo Sie Hilfe erhalten. Bei Problemen und Featureanfragen in Bezug auf den Bibliothekscode wenden Sie sich an den Bibliotheksbesitzer. Bei Problemen und Featureanfragen in Bezug auf die dienstseitige Protokollimplementierung wenden Sie sich an Microsoft.

Bibliotheken werden in zwei Kategorien unterteilt:

- **Von Microsoft unterstützt**: Microsoft stellt Fehlerbehebungen für diese Bibliotheken bereit. Microsoft hat diese Bibliotheken in Bezug auf die SDL-Anforderungen umfassend geprüft.
- **Kompatibel**: Microsoft hat einen Satz an Bibliotheken in grundlegenden Szenarien getestet und bestätigt, dass sie mit dem v2.0-Endpunkt funktionieren. Microsoft stellt keine Fehlerbehebungen für diese Bibliotheken bereit und hat diese Prüfungen keinerlei Prüfungen unterzogen. Bei Probleme und Featureanfragen sollten Sie sich an die Community für das Open Source-Projekt der Bibliothek wenden.

In den folgenden Abschnitten finden Sie eine Liste der Bibliotheken, die mit dem v2.0-Endpunkt verwendet werden können.

## Von Microsoft unterstützte Clientbibliotheken
| Plattform| Name der Bibliothek| Download | Quellcode | Beispiel |
| :-: | :-: | :-: | :-: | :-: |
| .NET, Windows Store, Xamarin | MSAL (Microsoft Authentication Library) für .NET | [Microsoft.Identity.Client (NuGet)][ClientLib-NET-Lib] | [MSAL für .NET (GitHub)][ClientLib-NET-Repo] | [Beispiel mit einem nativen Windows-Desktopclient][ClientLib-NET-Sample] |
| Node.js | Microsoft Azure Active Directory-Plug-In für Passport.js | [Passport-Azure-AD (npm)][ClientLib-Node-Lib] | [Passport-Azure-AD (GitHub)][ClientLib-Node-Repo] | Bald verfügbar |

<!--- COMMENTING OUT UNTIL THEY ARE READY
| iOS, Mac | Microsoft Authentication Library (MSAL) for ObjC | In development | In development | In development |
| Android | Microsoft Authentication Library (MSAL) for Android | In development | In development | In development |
| JavaScript | Microsoft Authentication Library (MSAL) for JavaScript | In development | In development | In development |
 -->

## Von Microsoft unterstützte Bibliotheken der Servermiddleware
| Plattform| Name der Bibliothek| Download | Quellcode | Beispiel |
| :-: | :-: | :-: | :-: | :-: |
| .NET 4.x | OWIN OpenID Connect Middleware für ASP.NET | [Microsoft.Owin.Security.OpenIdConnect (NuGet)][ServerLib-Net4-Owin-Oidc-Lib] | [Katana Project (CodePlex)][ServerLib-Net4-Owin-Oidc-Repo] | [Web-App-Beispiel][ServerLib-Net4-Owin-Oidc-Sample] |
| .NET 4.x | OWIN OAuth Bearer Middleware für ASP.Net | [Microsoft.Owin.Security.OAuth (NuGet)][ServerLib-Net4-Owin-Oauth-Lib] | [Katana Project (CodePlex)][ServerLib-Net4-Owin-Oauth-Repo] | [Web-API-Beispiel][ServerLib-Net4-Owin-Oauth-Sample] |
| .NET Core | OWIN OpenID Connect Middleware für .NET Core | [Microsoft.AspNetCore.Authentication.OpenIdConnect (NuGet)][ServerLib-NetCore-Owin-Oidc-Lib] | [ASP.NET Security (GitHub)][ServerLib-NetCore-Owin-Oidc-Repo] | [Web-App-Beispiel][ServerLib-NetCore-Owin-Oidc-Sample] |
| .NET Core | OWIN OAuth Bearer Middleware für .NET Core | [Microsoft.AspNetCore.Authentication.OAuth (NuGet)][ServerLib-NetCore-Owin-Oauth-Lib] | [ASP.NET Security (GitHub)][ServerLib-NetCore-Owin-Oauth-Repo] | Bald verfügbar |
| Node.js | Microsoft Azure Active Directory-Plug-In für Passport.js | [Passport-Azure-AD (npm)][ServerLib-Node-Lib] | [Passport-Azure-AD (GitHub)][ServerLib-Node-Repo] | [Web-App-Beispiel][ServerLib-Node-Sample] |
<!--- COMMENTING UNTIL SAMPLE IS AVAILABLE
| .NET 4.x, .NET Core | JSON Web Token Handler for .Net | [System.IdentityModel.Tokens.Jwt (NuGet)][ServerLib-Net-Jwt-Lib] | [Azure AD identity model extensions for .Net (GitHub)][ServerLib-Net-Jwt-Repo] | Coming soon |
--->
## Kompatible Clientbibliotheken
| Plattform| Name | Getestete Version | Quellcode | Beispiel |
| :-: | :-: | :-: | :-: | :-: |
| Android | [OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib/wiki) | 0\.2.1 | [OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib) | [Beispiel mit nativer App](active-directory-v2-devquickstarts-android.md) |
| iOS | [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) | 1\.2.8 | [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) | [Beispiel mit nativer App](active-directory-v2-devquickstarts-ios.md)|
| JavaScript | [Hello.js](https://adodson.com/hello.js/) | 1\.13.5 | [Hello.js](https://github.com/MrSwitch/hello.js) | Bald verfügbar |
| Python – Flask | [Flask-OAuthlib](https://github.com/lepture/flask-oauthlib) | 0\.9.3 | [Flask-OAuthlib](https://github.com/lepture/flask-oauthlib) | In Kürze verfügbar |
| Ruby | [OmniAuth](https://github.com/omniauth/omniauth/wiki) | omniauth:1.3.1</br>omniauth-oauth2:1.4.0 | [OmniAuth](https://github.com/omniauth/omniauth)</br>[OmniAuth OAuth2](https://github.com/intridea/omniauth-oauth2) | Bald verfügbar |
<!--- REMOVING BRANDON'S FOR NOW
|  |  |  |  |  |
| Android | [OAuth2 Client](https://github.com/wuman/android-oauth-client) |   | [OAuth2 Client](https://github.com/wuman/android-oauth-client)  | Coming soon  |
| Java | [WSO2 Identity Server](https://docs.wso2.com/display/IS500/Introducing+the+Identity+Server) | [Version 5.2.0](http://wso2.com/products/identity-server/) | [Source](https://docs.wso2.com/display/IS500/Building+from+Source) | [Samples index](https://docs.wso2.com/display/IS500/Samples)  |
| Java | [Java Gluu Server](https://gluu.org/docs/) |   | [oxAuth](https://github.com/GluuFederation/oxAuth)  | Coming soon |
| Node.js | [NPM passport-openidconnect](https://www.npmjs.com/package/passport-openidconnect) | 0.0.1  | [Passport-OpenID Connect](https://github.com/jaredhanson/passport-openidconnect) | Coming soon  |
| PHP | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP) |   | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP)  | Coming soon  |
-->

## Kompatible Bibliotheken der Servermiddleware 
Bald verfügbar


## Verwandte Inhalte
Weitere Informationen zum Azure AD v2.0-Endpunkt finden Sie in der [Übersicht über das Azure AD-App-Modell (v2)][AAD-App-Model-V2-Overview].

Verwenden Sie den folgenden Disqus-Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: active-directory-appmodel-v2-overview.md
[ClientLib-NET-Lib]: http://www.nuget.org/packages/Microsoft.Identity.Client
[ClientLib-NET-Repo]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[ClientLib-NET-Sample]: active-directory-v2-devquickstarts-wpf.md
[ClientLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ClientLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad
[ClientLib-Node-Sample]: [ClientLib-Iosmac-Lib]:
[ClientLib-Iosmac-Repo]: [ClientLib-Iosmac-Sample]:
[ClientLib-Android-Lib]: [ClientLib-Android-Repo]:
[ClientLib-Android-Sample]: [ClientLib-Js-Lib]:
[ClientLib-Js-Repo]: [ClientLib-Js-Sample]:
[Microsoft-SDL]: http://www.microsoft.com/sdl/default.aspx
[ServerLib-Net4-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/
[ServerLib-Net4-Owin-Oidc-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oidc-Sample]: active-directory-v2-devquickstarts-dotnet-web.md
[ServerLib-Net4-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OAuth/
[ServerLib-Net4-Owin-Oauth-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oauth-Sample]: https://azure.microsoft.com/de-DE/documentation/articles/active-directory-v2-devquickstarts-dotnet-api/
[ServerLib-Net-Jwt-Lib]: https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt
[ServerLib-Net-Jwt-Repo]: https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet
[ServerLib-Net-Jwt-Sample]: /
[ServerLib-NetCore-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OpenIdConnect/
[ServerLib-NetCore-Owin-Oidc-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oidc-Sample]: https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2
[ServerLib-NetCore-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OAuth/
[ServerLib-NetCore-Owin-Oauth-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oauth-Sample]: /
[ServerLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ServerLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad/
[ServerLib-Node-Sample]: https://azure.microsoft.com/de-DE/documentation/articles/active-directory-v2-devquickstarts-node-web/

<!---HONumber=AcomDC_0928_2016-->