---
title: Azure Active Directory-Authentifizierungsbibliotheken | Microsoft Docs
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
ms.date: 02/27/2017
ms.author: bryanla
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 2ad287d383f65ea66382d1f449a76db596963f46
ms.contentlocale: de-de
ms.lasthandoff: 03/28/2017


---
# <a name="azure-active-directory-authentication-libraries"></a>Azure Active Directory-Authentifizierungsbibliotheken
Die Azure AD-Authentifizierungsbibliothek (ADAL) ermöglicht es Entwicklern von Clientanwendungen, auf einfache Weise eine Benutzerauthentifizierung mit Active Directory (Cloud oder lokal) bereitzustellen und anschließend Zugriffstoken zur Absicherung von API-Aufrufen abzurufen. ADAL verfügt über zahlreiche Features zur Vereinfachung der Authentifizierung für Entwickler. Dazu gehören beispielsweise die Unterstützung von asynchronen Methoden, ein konfigurierbarer Tokencache zum Speichern von Zugriffstoken und Aktualisierungstoken, eine automatische Tokenaktualisierung bei Ablauf eines Zugriffstokens, sofern ein Aktualisierungstoken verfügbar ist, und vieles mehr. Da durch ADAL die meisten komplexen Aspekte abgedeckt werden, können sich Entwickler auf die Geschäftslogik in ihren Anwendungen konzentrieren und auf einfache Weise Ressourcen sichern, ohne Sicherheitsexperten sein zu müssen.

ADAL steht auf einer Vielzahl von Plattformen zur Verfügung.

### <a name="client-libraries"></a>Clientbibliotheken

| Plattform | Bibliothek | Herunterladen | Quellcode | Beispiel | Referenz
| --- | --- | --- | --- | --- | --- |
| .NET-Client, Windows Store, UWP, Xamarin iOS und Android |ADAL .NET v3 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) | [Desktop-App](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-devquickstarts-dotnet) |[Referenz](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory) | 
| .NET-Client, Windows Store, Windows Phone 8.1 |ADAL .NET v2 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.2) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.2) | [Desktop-App](https://github.com/AzureADQuickStarts/NativeClient-DotNet/releases/tag/v2.X) |[Referenz](https://docs.microsoft.com/en-us/active-directory/adal//v2/microsoft.identitymodel.clients.activedirectory) | 
| JavaScript |ADAL.js |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[Einzelseiten-App](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) | |
| iOS, macOS |ADAL |[CocoaPods](http://cocoadocs.org/docsets/ADAL/) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc) |[iOS-App](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-devquickstarts-ios) | [Referenz](http://cocoadocs.org/docsets/ADAL/)|
| Android |ADAL |[Das zentrale Repository](http://search.maven.org/remotecontent?filepath=com/microsoft/aad/adal/) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android) |[Android-App](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-devquickstarts-android) | [JavaDocs](http://javadoc.io/doc/com.microsoft.aad/adal/)|
| Node.js |ADAL |[npm](https://www.npmjs.com/package/adal-node) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs) | | |
| Java |ADAL4J |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[Java-Web-App](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-devquickstarts-webapp-java) | |

### <a name="server-libraries"></a>Serverbibliotheken 

| Plattform | Bibliothek | Herunterladen | Quellcode | Beispiel | Referenz
| --- | --- | --- | --- | --- | --- |
| .NET |OWIN für Azure AD|[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[CodePlex](http://katanaproject.codeplex.com) |[MVC-App](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-devquickstarts-webapp-dotnet) | |
| .NET |OWIN für OpenIDConnect |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[CodePlex](http://katanaproject.codeplex.com) |[Web App](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet) | |
| Node.js |Azure AD Passport |[npm](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Web-API](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-devquickstarts-webapi-nodejs)| |
| .NET |OWIN für WS-Federation |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) |[CodePlex](http://katanaproject.codeplex.com) |[MVC-Web-App](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) | |
| .NET |Identitätsprotokollerweiterungen für .NET 4.5 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| .NET |JWT-Handler für .NET 4.5 |[NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |



## <a name="scenarios"></a>Szenarios
Nachfolgend werden die drei gängigsten Szenaren vorgestellt, in denen ADAL für die Authentifizierung verwendet werden kann.  

### <a name="authenticating-users-of-a-client-application-to-a-remote-resource"></a>Authentifizieren von Benutzern einer Clientanwendung bei einer Remoteressource
In diesem Szenario verwendet ein Entwickler einen Client (z. B. eine WPF-Anwendung), der auf eine durch Azure AD geschützte Remoteressource (z. B. eine Web-API) zugreifen muss. Er verfügt über ein Azure-Abonnement, weiß, wie die Downstream-Web-API aufgerufen wird, und kennt den Azure AD-Mandanten, den die Web-API verwendet. ADAL kann daher zur Vereinfachung der Authentifizierung bei Azure AD eingesetzt werden – durch vollständiges Delegieren der Authentifizierung an ADAL oder durch explizites Verarbeiten der Benutzeranmeldeinformationen. Dank ADAL ist es einfach, den Benutzer zu authentifizieren, ein Zugriffstoken und ein Aktualisierungstoken aus Azure AD abzurufen und das Zugriffstoken dann zu verwenden, um Anforderungen an die Web-API zu senden.

Ein Codebeispiel, in dem dieses Szenario mithilfe von Authentifizierung für Azure AD veranschaulicht wird, finden Sie unter [Systemeigene Client-WPF-Anwendung für Web-API](https://github.com/azureadsamples/nativeclient-dotnet).

### <a name="authenticating-a-server-application-to-a-remote-resource"></a>Authentifizieren einer Serveranwendung bei einer Remoteressource
In diesem Szenario verwendet ein Entwickler eine Anwendung, die auf einem Server ausgeführt wird und auf eine durch Azure AD geschützte Remoteressource (z. B. eine Web-API) zugreifen muss. Er verfügt über ein Azure-Abonnement, weiß, wie der Downstreamdienst aufgerufen wird, und kennt den Azure AD-Mandanten, den die Web-API verwendet. Er kann daher ADAL zum Ermöglichen der Authentifizierung mit Azure AD verwenden, indem die Anmeldeinformationen der Anwendung explizit verarbeitet werden. ADAL vereinfacht das Abrufen eines Tokens aus Azure AD, indem die Clientanmeldeinformationen der Anwendung verwendet werden und dann das Token verwendet wird, um Anforderungen an die Web-API zu senden. ADAL übernimmt außerdem die Verwaltung der Lebensdauer des Zugriffstokens, indem es zwischengespeichert und bei Bedarf erneuert wird. Ein Codebeispiel, das dieses Szenario veranschaulicht, finden Sie unter [Konsolenanwendung für Web-API](https://github.com/AzureADSamples/Daemon-DotNet).

### <a name="authenticating-a-server-application-on-behalf-of-a-user-to-access-a-remote-resource"></a>Authentifizieren einer Serveranwendung im Auftrag eines Benutzers, um auf eine Remoteressource zuzugreifen
In diesem Szenario verwendet ein Entwickler eine Anwendung, die auf einem Server ausgeführt wird und auf eine durch Azure AD geschützte Remoteressource (z. B. eine Web-API) zugreifen muss. Die Anforderung muss außerdem im Auftrag eines Benutzers in Azure AD vorgenommen werden. Der Entwickler verfügt über ein Azure-Abonnement, weiß, wie die Downstream-Web-API aufgerufen wird, und kennt den Azure AD-Mandanten, den der Dienst verwendet. Sobald der Benutzer für die Webanwendung authentifiziert ist, kann die Anwendung einen Autorisierungscode für den Benutzer aus Azure AD abrufen. Die Webanwendung kann dann ADAL verwenden, um im Auftrag eines Benutzers ein Zugriffstoken und ein Aktualisierungstoken abzurufen, wobei der Autorisierungscode und die Clientanmeldeinformationen verwendet werden, die der Anwendung aus Azure AD zugeordnet sind. Sobald die Webanwendung im Besitz des Zugriffstokens ist, kann sie bis zum Ablauf des Tokens die Web-API aufrufen. Nach Ablauf des Tokens kann die Webanwendung über ADAL mit dem zuvor empfangenen Aktualisierungstoken ein neues Zugriffstoken abrufen.

## <a name="see-also"></a>Weitere Informationen
[Entwicklerhandbuch zu Azure Active Directory](active-directory-developers-guide.md)

[Authentifizierungsszenarien für Azure Active Directory](active-directory-authentication-scenarios.md)

[Azure Active Directory-Codebeispiele](active-directory-code-samples.md)

