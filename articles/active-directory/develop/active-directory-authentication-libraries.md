---
title: Azure Active Directory-Authentifizierungsbibliotheken | Microsoft-Dokumentation
description: "Die Azure AD-Authentifizierungsbibliothek (ADAL) ermöglicht es Entwicklern von Clientanwendungen, auf einfache Weise eine Benutzerauthentifizierung mit Active Directory (Cloud oder lokal) bereitzustellen und anschließend Zugriffstoken zur Absicherung von API-Aufrufen abzurufen."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: mbaldwin
ms.assetid: 2e4fc79a-0285-40be-8c77-65edee408a22
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/02/2017
ms.author: bryanla
ms.custom: aaddev
ms.translationtype: HT
ms.sourcegitcommit: 79bebd10784ec74b4800e19576cbec253acf1be7
ms.openlocfilehash: 9cf8ca88c9df4082590b18acd5c4a87a91bb1afd
ms.contentlocale: de-de
ms.lasthandoff: 08/03/2017

---
# <a name="azure-active-directory-authentication-libraries"></a>Azure Active Directory-Authentifizierungsbibliotheken
Die Azure Active Directory-Authentifizierungsbibliothek (Azure Active Directory Authentication Library, ADAL) ermöglicht es Entwicklern von Clientanwendungen, auf einfache Weise eine Benutzerauthentifizierung mit Active Directory (Cloud oder lokal) bereitzustellen und Zugriffstoken zur Absicherung von API-Aufrufen abzurufen. ADAL bietet Entwicklern folgende Features, um die Authentifizierung zu vereinfachen:
 - Unterstützung für asynchrone Methodenaufrufe
 - Konfigurierbarer Tokencache zum Speichern von Zugriffs- und Aktualisierungstoken
 - Automatische Tokenaktualisierung, wenn ein Zugriffstoken abläuft und ein Aktualisierungstoken verfügbar ist
 - Viele weitere Funktionen
 
Da ADAL die meisten komplexen Aspekte verarbeitet, können sich Entwickler auf die Geschäftslogik konzentrieren und auf einfache Weise Ressourcen sichern, ohne Sicherheitsexperten sein zu müssen.

ADAL steht auf einer Vielzahl von Plattformen zur Verfügung.

### <a name="client-libraries"></a>Clientbibliotheken

| Plattform | Bibliothek | Herunterladen | Quellcode | Beispiel | Referenz
| --- | --- | --- | --- | --- | --- |
| .NET-Client, Windows Store, UWP, Xamarin iOS und Android |MSAL für .NET (Vorschau) |[NuGet](https://www.nuget.org/packages/Microsoft.Identity.Client/1.1.0-preview) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) | [Desktop-App](~/articles/active-directory/develop/guidedsetups/active-directory-windesktop.md) |[Referenz](https://docs.microsoft.com/dotnet/api/?view=identityclient-1.1.0-preview) | 
| JavaScript |MSAL für JavaScript (Vorschau) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) | [Einzelseiten-App](~/articles/active-directory/develop/GuidedSetups/active-directory-javascriptspa.md) | [Referenz](https://htmlpreview.github.io/?https://raw.githubusercontent.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/docs/classes/_useragentapplication_.msal.useragentapplication.html) | 
| iOS |MSAL für iOS (Vorschau) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) | [iOS-Apps](~/articles/active-directory/develop/GuidedSetups/active-directory-ios.md) | [Referenz](https://azuread.github.io/docs/objc/) |
| Android |MSAL für Android (Vorschau) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android) | [Android-Apps](~/articles/active-directory/develop/GuidedSetups/active-directory-android.md) | [Referenz](http://javadoc.io/doc/com.microsoft.identity.client/msal/0.1.1) |
| .NET-Client, Windows Store, UWP, Xamarin iOS und Android |ADAL .NET v3 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) | [Desktop-App](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-dotnet) |[Referenz](https://docs.microsoft.com/dotnet/api/?view=identitymodelclientsad-3.13.9) | 
| .NET-Client, Windows Store, Windows Phone 8.1 |ADAL .NET v2 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.4) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.4) | [Desktop-App](https://github.com/AzureADQuickStarts/NativeClient-DotNet/releases/tag/v2.X) | | 
| JavaScript |ADAL.js |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[Einzelseiten-App](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) | |
| iOS, macOS |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc/releases) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc) |[iOS-Apps](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-ios) | [Referenz](https://cocoapods.org/pods/ADAL)|
| Android |ADAL |[Das zentrale Repository](http://search.maven.org/remotecontent?filepath=com/microsoft/aad/adal/) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android) |[Android-Apps](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-android) | [JavaDocs](http://javadoc.io/doc/com.microsoft.aad/adal/)|
| Node.js |ADAL |[npm](https://www.npmjs.com/package/adal-node) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs) | | |
| Java |ADAL4J |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[Java-Web-Apps](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapp-java) | |
| Python |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) | | |
### <a name="server-libraries"></a>Serverbibliotheken 

| Plattform | Bibliothek | Herunterladen | Quellcode | Beispiel | Referenz
| --- | --- | --- | --- | --- | --- |
| .NET |OWIN für Azure AD|[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[CodePlex](http://katanaproject.codeplex.com) |[MVC-App](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapp-dotnet) | |
| .NET |OWIN für OpenIDConnect |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[CodePlex](http://katanaproject.codeplex.com) |[Web App](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet) | |
| Node.js |Azure AD Passport |[npm](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Web-API](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapi-nodejs)| |
| .NET |OWIN für WS-Federation |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) |[CodePlex](http://katanaproject.codeplex.com) |[MVC-Web-App](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) | |
| .NET |Identitätsprotokollerweiterungen für .NET 4.5 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| .NET |JWT-Handler für .NET 4.5 |[NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |



## <a name="scenarios"></a>Szenarien

Nachfolgend werden drei gängige Szenarien vorgestellt, in denen ADAL für die Authentifizierung eines Clients verwendet werden kann, der auf eine Remoteressource zugreift:  

### <a name="authenticating-users-of-a-native-client-application-running-on-a-device"></a>Authentifizieren von Benutzern einer auf einem Gerät ausgeführten nativen Clientanwendung 

In diesem Szenario hat ein Entwickler eine WPF-Clientanwendung erstellt, die auf eine durch Azure AD geschützte Remoteressource (z.B. eine Web-API) zugreifen muss. Er verfügt über ein Azure-Abonnement, weiß, wie die Downstream-Web-API aufgerufen wird, und kennt den Azure AD-Mandanten, den die Web-API verwendet. ADAL kann daher zur Vereinfachung der Authentifizierung bei Azure AD eingesetzt werden – durch vollständiges Delegieren der Authentifizierung an ADAL oder durch explizites Verarbeiten der Benutzeranmeldeinformationen. Dank ADAL ist es einfach, den Benutzer zu authentifizieren, ein Zugriffstoken und ein Aktualisierungstoken aus Azure AD abzurufen und das Zugriffstoken dann zu verwenden, um Anforderungen an die Web-API zu senden.

Ein Codebeispiel, in dem dieses Szenario mithilfe von Authentifizierung für Azure AD veranschaulicht wird, finden Sie unter [Systemeigene Client-WPF-Anwendung für Web-API](https://github.com/azureadsamples/nativeclient-dotnet).

### <a name="authenticating-a-confidential-client-application-running-on-a-web-server"></a>Authentifizieren einer auf einem Webserver ausgeführten vertraulichen Clientanwendung

In diesem Szenario verwendet ein Entwickler eine Anwendung, die auf einem Server ausgeführt wird und auf eine durch Azure AD geschützte Remoteressource (z. B. eine Web-API) zugreifen muss. Er verfügt über ein Azure-Abonnement, weiß, wie der Downstreamdienst aufgerufen wird, und kennt den Azure AD-Mandanten, den die Web-API verwendet. Er kann daher ADAL zum Ermöglichen der Authentifizierung mit Azure AD verwenden, indem die Anmeldeinformationen der Anwendung explizit verarbeitet werden. ADAL vereinfacht das Abrufen eines Tokens aus Azure AD, indem die Clientanmeldeinformationen der Anwendung verwendet werden und dann das Token verwendet wird, um Anforderungen an die Web-API zu senden. ADAL übernimmt außerdem die Verwaltung der Lebensdauer des Zugriffstokens, indem es zwischengespeichert und bei Bedarf erneuert wird. Ein Codebeispiel, das dieses Szenario veranschaulicht, finden Sie unter [Daemon console Application to Web API](https://github.com/AzureADSamples/Daemon-DotNet) (Daemon-Konsolenanwendung für Web-API).

### <a name="authenticating-a-confidential-client-application-running-on-a-server-on-behalf-of-a-user"></a>Authentifizieren einer auf einem Server ausgeführten vertraulichen Clientanwendung im Namen eines Benutzers 

In diesem Szenario verwendet ein Entwickler eine Anwendung, die auf einem Server ausgeführt wird und auf eine durch Azure AD geschützte Remoteressource (z. B. eine Web-API) zugreifen muss. Die Anforderung muss außerdem im Namen eines Azure AD-Benutzers ausgeführt werden. Der Entwickler verfügt über ein Azure-Abonnement, weiß, wie die Downstream-Web-API aufgerufen wird, und kennt den Azure AD-Mandanten, den der Dienst verwendet. Sobald der Benutzer für die Webanwendung authentifiziert ist, kann die Anwendung einen Autorisierungscode für den Benutzer aus Azure AD abrufen. Die Webanwendung kann dann ADAL verwenden, um im Auftrag eines Benutzers ein Zugriffstoken und ein Aktualisierungstoken abzurufen, wobei der Autorisierungscode und die Clientanmeldeinformationen verwendet werden, die der Anwendung aus Azure AD zugeordnet sind. Sobald die Webanwendung im Besitz des Zugriffstokens ist, kann sie bis zum Ablauf des Tokens die Web-API aufrufen. Nach Ablauf des Tokens kann die Webanwendung über ADAL mit dem zuvor empfangenen Aktualisierungstoken ein neues Zugriffstoken abrufen. Ein Codebeispiel, das dieses Szenario veranschaulicht, finden Sie unter [Native client to Web API to Web API](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof) (Nativer Clientaufruf zwischen Web-APIs).

## <a name="see-also"></a>Weitere Informationen

- [Entwicklerhandbuch zu Azure Active Directory](active-directory-developers-guide.md)
- [Authentifizierungsszenarien für Azure Active Directory](active-directory-authentication-scenarios.md)
- [Azure Active Directory-Codebeispiele](active-directory-code-samples.md)

