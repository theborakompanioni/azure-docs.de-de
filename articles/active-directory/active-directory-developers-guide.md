---
title: Entwicklerhandbuch zu Azure Active Directory | Microsoft Docs
description: "Dieser Artikel enthält eine umfassende Übersicht über die für Entwickler wichtigen Ressourcen zu Azure Active Directory."
services: active-directory
documentationcenter: dev-center-name
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 5c872c89-ef04-4f4c-98de-bc0c7460c7c2
ms.service: active-directory
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/24/2016
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 3e587c9422e008a7b3c46e34fed4d0f1b2a8189c


---
# <a name="azure-active-directory-developers-guide"></a>Entwicklerhandbuch zu Azure Active Directory
## <a name="overview"></a>Übersicht
Als Plattform vom Typ „Identity Management as a Service (IDMaaS)“ stellt Azure Active Directory (AD) für Entwickler eine effektive Möglichkeit zum Integrieren der Identitätsverwaltung in ihre Anwendungen dar. Die folgenden Artikel enthalten Übersichten über die Implementierung sowie wichtige Features von Azure AD. Wir empfehlen Ihnen, die Artikel in der angegebenen Reihenfolge zu lesen. Wenn Sie möchten, können Sie aber auch direkt mit [Erste Schritte](#getting-started) beginnen.

1. [Vorteile der Integration in Azure AD:](develop/active-directory-how-to-integrate.md)Erfahren Sie, warum die Integration in Azure AD die beste Lösung für eine sichere Anmeldung und Autorisierung ist.
2. [Azure AD-Authentifizierungsszenarien:](active-directory-authentication-scenarios.md)Nutzen Sie die vereinfachte Authentifizierung von Azure AD, um eine Anmeldung für Ihre Anwendung bereitzustellen.
3. [Integrieren von Anwendungen in Azure AD:](active-directory-integrating-applications.md)Erfahren Sie, wie Sie Anwendungen über Azure AD hinzufügen, aktualisieren und entfernen, und informieren Sie sich über die Brandingrichtlinien für integrierte Apps.
4. [Azure AD Graph-API:](active-directory-graph-api.md)Verwenden Sie die Azure AD Graph-API, um über REST-API-Endpunkte programmgesteuert auf Azure AD zuzugreifen. Die Azure AD Graph-API ist auch über [Microsoft Graph](https://graph.microsoft.io/)verfügbar. Microsoft Graph stellt eine einheitliche API bereit, mit deren Hilfe über einen einzelnen REST-API-Endpunkt und mit einem einzelnen Zugriffstoken auf mehrere Microsoft Clouddienst-APIs zugegriffen werden kann.
5. [Azure AD-Authentifizierungsbibliotheken:](active-directory-authentication-libraries.md)Authentifizieren Sie Benutzer auf einfache Weise, indem Sie unter anderem Zugriffstoken über die Azure AD-Authentifizierungsbibliotheken für .NET, JavaScript, Objective-C, Android abrufen.

## <a name="getting-started"></a>Erste Schritte
Diese Tutorials sind auf verschiedene Plattformen ausgelegt und ermöglichen Ihnen einen schnellen Einstieg in die Entwicklung mit Azure Active Directory. Voraussetzung ist, dass Sie über einen [Azure Active Directory-Mandanten](active-directory-howto-tenant.md)verfügen.

### <a name="mobile-and-pc-application-quick-start-guides"></a>Mobile Anwendung und PC-Anwendung – Schnellstartanleitungen
| [![iOS](./media/active-directory-developers-guide/ios.png)](active-directory-devquickstarts-ios.md) | [![Android](./media/active-directory-developers-guide/android.png)](active-directory-devquickstarts-android.md) | [![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-dotnet.md) | [![Windows Universal](./media/active-directory-developers-guide/windows.png)](active-directory-devquickstarts-windowsstore.md) | [![Xamarin](./media/active-directory-developers-guide/xamarin.png)](active-directory-devquickstarts-xamarin.md) | [![Cordova](./media/active-directory-developers-guide/cordova.png)](active-directory-devquickstarts-cordova.md) | [![OAuth 2.0](./media/active-directory-developers-guide/oauth-2.png)](active-directory-protocols-oauth-code.md) |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| [iOS](active-directory-devquickstarts-ios.md) |[Android](active-directory-devquickstarts-android.md) |[.NET](active-directory-devquickstarts-dotnet.md) |[Windows Universal](active-directory-devquickstarts-windowsstore.md) |[Xamarin](active-directory-devquickstarts-xamarin.md) |[Cordova](active-directory-devquickstarts-cordova.md) |[Direktes Integrieren mit OAuth 2.0](active-directory-protocols-oauth-code.md) |

### <a name="web-application-quick-start-guides"></a>Webanwendung – Schnellstartanleitungen
| [![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapp-dotnet.md) | [![Java](./media/active-directory-developers-guide/java.png)](active-directory-devquickstarts-webapp-java.md) | [![AngularJS](./media/active-directory-developers-guide/angularjs.png)](active-directory-devquickstarts-angular.md) | [![JavaScript](./media/active-directory-developers-guide/javascript.png)](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) | [![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-openidconnect-nodejs.md) | [![OpenID Connect](./media/active-directory-developers-guide/openid-connect.png)](active-directory-protocols-openid-connect-code.md) |
|:---:|:---:|:---:|:---:|:---:|:---:|
| [.NET](active-directory-devquickstarts-webapp-dotnet.md) |[Java](active-directory-devquickstarts-webapp-java.md) |[AngularJS](active-directory-devquickstarts-angular.md) |[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |[Node.js](active-directory-devquickstarts-openidconnect-nodejs.md) |[Direktes Integrieren mit OpenID Connect](active-directory-protocols-openid-connect-code.md) |

### <a name="web-api-quick-start-guides"></a>Web-API – Schnellstartanleitungen
| [![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapi-dotnet.md) | [![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-webapi-nodejs.md) |
|:---:|:---:|
| [.NET](active-directory-devquickstarts-webapi-dotnet.md) |[Node.js](active-directory-devquickstarts-webapi-nodejs.md) |

### <a name="querying-the-directory-quickstart-guide"></a>Abfragen des Verzeichnisses – Schnellstartanleitung
| [![.NET](./media/active-directory-developers-guide/graph.png)](active-directory-graph-api-quickstart.md) |
|:---:|
| [Graph-API](active-directory-graph-api-quickstart.md) |

## <a name="how-tos"></a>Vorgehensweisen
In diesen Artikeln wird beschrieben, wie Sie mit Azure Active Directory bestimmte Aufgaben ausführen:

* [Erhalten eines Azure AD-Mandanten](active-directory-howto-tenant.md)
* [How to sign in any Azure Active Directory (AD) user using the multi-tenant application pattern](active-directory-devhowto-multi-tenant-overview.md)
* Aktivieren von App-übergreifendem SSO per ADAL auf [Android](active-directory-sso-android.md)- und [iOS](active-directory-sso-ios.md)-Geräten
* [How to get AppSource Certified for Azure Active Directory (AD)](active-directory-devhowto-appsource-certified.md)
* [Auflisten Ihrer Anwendung im Azure AD-Anwendungskatalog](active-directory-app-gallery-listing.md)
* [Übertragen von Web-Apps für Office 365 an das Verkäuferdashboard (in englischer Sprache)](https://msdn.microsoft.com/office/office365/howto/submit-web-apps-seller-dashboard)
* [Grundlegendes zum Azure Active Directory-Anwendungsmanifest](active-directory-application-manifest.md)
* [Grundlegendes zu den Brandingrichtlinien für die Schaltflächen für Anmeldung und App-Erwerb in Ihrer Clientanwendung](active-directory-branding-guidelines.md)
* [Vorschau: Erstellen von Apps, bei denen sich Benutzer sowohl mit persönlichen als auch mit Geschäfts-, Schul- oder Unikonto anmelden können](active-directory-appmodel-v2-overview.md)
* [Vorschau: Erstellen von Apps, die Kunden registrieren und anmelden](../active-directory-b2c/active-directory-b2c-overview.md)
* [Preview: Configurable Token Lifetimes in Azure Active Directory](active-directory-configurable-token-lifetimes.md) (Vorschau: Konfigurierbare Tokenlebensdauern in Azure Active Directory) mit PowerShell Details zur Konfiguration über die Azure AD Graph-API finden Sie unter [Operations on policy](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) (Richtlinienvorgänge) und [Policy entity](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity) (Richtlinienentität).

## <a name="reference"></a>Referenz
Diese Artikel enthalten grundlegende Referenzen zu REST- und Authentifizierungsbibliothek-APIs, Protokollen, Fehlern, Codebeispielen und Endpunkten.  

### <a name="support"></a>Support
* [Mit Tags markierte Fragen](http://stackoverflow.com/questions/tagged/azure-active-directory): Sie finden Azure Active Directory-Lösungen bei Stack Overflow, indem Sie nach den Tags [azure-active-directory](http://stackoverflow.com/questions/tagged/azure-active-directory) und [adal](http://stackoverflow.com/questions/tagged/adal) suchen.
* Das [Azure Active Directory-Glossar für Entwickler](active-directory-dev-glossary.md) enthält Definitionen für einige Begriffe, die häufig in Verbindung mit der Anwendungsentwicklung und -integration verwendet werden.

### <a name="code"></a>Code
* [Open-Source-Bibliotheken für Azure Active Directory](http://github.com/AzureAD): Am einfachsten finden Sie die Quelle einer Bibliothek in unserer [Bibliotheksliste](active-directory-authentication-libraries.md).
* [Azure Active Directory-Beispiele](https://github.com/azure-samples?query=active-directory): Die einfachste Möglichkeit zum Navigieren in der Liste mit den Beispielen bietet der [Index für die Codebeispiele](active-directory-code-samples.md).
* [Active Directory-Authentifizierungsbibliothek (ADAL) für .NET](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet): Die Referenzdokumentation ist für die [aktuelle Hauptversion](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory) und die [vorherige Hauptversion](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory) verfügbar.

### <a name="graph-api"></a>Graph-API
* [Graph-API-Referenz](https://msdn.microsoft.com/library/azure/hh974476.aspx): Hier finden Sie die REST-Referenz für die Azure Active Directory Graph-API. [Sehen Sie sich die interaktive Umgebung für die Graph-API-Referenz an](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
* [Graph-API-Berechtigungsbereiche](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes): Hier werden die OAuth 2.0-Berechtigungsbereiche beschrieben, mit denen gesteuert wird, welchen Zugriff eine App auf die Verzeichnisdaten für einen Mandanten hat.

### <a name="authentication-and-authorization-protocols"></a>Protokolle für Authentifizierung und Autorisierung
* [Rollover von Signaturschlüsseln in Azure AD](active-directory-signing-key-rollover.md): Erfahren Sie mehr über die Rolloverkadenz von Signaturschlüsseln von Azure AD und über die Aktualisierung des Schlüssels für die häufigsten Anwendungsszenarien.
* [OAuth 2.0-Protokoll: Verwenden der Autorisierungscodegewährung](active-directory-protocols-oauth-code.md): Sie können die Autorisierungscodegewährung des OAuth 2.0-Protokolls verwenden, um den Zugriff auf Webanwendungen und Web-APIs in Ihrem Azure Active Directory-Mandanten zu autorisieren.
* [OAuth 2.0-Protokoll: Grundlagen der impliziten Gewährung](active-directory-dev-understanding-oauth2-implicit-grant.md): Hier erhalten Sie weitere Informationen zur impliziten Autorisierungsgewährung und zur Eignung für Ihre Anwendung.
* [OAuth 2.0-Protokoll: Verwenden von Clientanmeldeinformationen für Dienst-zu-Dienst-Aufrufe](active-directory-protocols-oauth-service-to-service.md): Bei der Erteilung von OAuth 2.0-Clientanmeldeinformationen kann sich ein Webdienst (ein vertraulicher Client), der einen anderen Webdienst aufruft, mit seinen eigenen Anmeldeinformationen authentifizieren, anstatt die Identität eines anderen Benutzers anzunehmen. In diesem Szenario ist der Client normalerweise ein Webdienst der mittleren Ebene, ein Daemondienst oder eine Website.
* [OpenID Connect 1.0-Protokoll: Anmeldung und Authentifizierung](active-directory-protocols-openid-connect-code.md): Durch das OpenID Connect 1.0-Protokoll kann OAuth 2.0 auch als Authentifizierungsprotokoll verwendet werden. Eine Clientanwendung kann ein ID-Token (id_token) erhalten, um den Anmeldeprozess zu verwalten, oder der Autorisierungscodefluss kann erweitert werden, um sowohl ein ID-Token als auch einen Autorisierungscode zu erhalten.
* [SAML 2.0-Protokoll – Referenz](active-directory-saml-protocol-reference.md): Mit dem SAML 2.0-Protokoll können Anwendungen für Benutzer das einmalige Anmelden bereitstellen.
* [WS-Federation 1.2-Protokoll](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html): Azure Active Directory unterstützt WS-Federation 1.2 gemäß der Spezifikation für Web Services Federation (Version 1.2). Weitere Informationen zum Verbundmetadaten-Dokument finden Sie unter [Verbundmetadaten](active-directory-federation-metadata.md).
* [Unterstützte Token und Anspruchstypen](active-directory-token-and-claims.md): Verwenden Sie dieses Handbuch, um die Ansprüche in SAML 2.0- und JSON-Webtoken (JWT) zu verstehen und auszuwerten.

## <a name="videos"></a>Videos
### <a name="build"></a>Entwickeln
In diesen Übersichtspräsentationen zur Entwicklung von Apps mithilfe von Azure Active Directory kommen Mitglieder des Entwicklungsteams zu Wort. In den Präsentationen werden grundlegende Themen wie IDMaaS, Authentifizierung, Identitätsverbund und einmaliges Anmelden behandelt.

* [Microsoft Identity: State of the Union and Future Direction (Microsoft Identity: Aktueller Zustand und zukünftige Entwicklung)](https://azure.microsoft.com/documentation/videos/build-2016-microsoft-identity-state-of-the-union-and-future-direction/)
* [Azure Active Directory: Identitätsverwaltung als Dienst für moderne Anwendungen](https://azure.microsoft.com/documentation/videos/build-2015-azure-active-directory-identity-management-as-a-service-for-modern-applications/)
* [Entwickeln von modernen Webanwendungen mit Azure Active Directory](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-web-applications-with-azure-active-directory/)
* [Entwickeln von modernen systemeigenen Anwendungen mit Azure Active Directory](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-native-applications-with-azure-active-directory/)

### <a name="azure-friday"></a>Azure Friday
[Azure Friday](https://azure.microsoft.com/documentation/videos/azure-friday/) ist eine Videoreihe, bei der jeden Freitag neue kurze Videos mit kurzen Experteninterviews zu unterschiedlichen Azure-Themen veröffentlicht werden. Die Videos dauern jeweils etwa 10 bis 15 Minuten.  Verwenden Sie die Dienstfilterfunktion auf der Seite, um alle Azure Active Directory-Videos anzuzeigen.

* [Azure-Identität 101](https://azure.microsoft.com/documentation/videos/azure-identity-basics/)
* [Azure-Identität 102](https://azure.microsoft.com/documentation/videos/azure-identity-creating-active-directory/)
* [Azure-Identität 103](https://azure.microsoft.com/documentation/videos/azure-identity-application-to-authenticate/)

## <a name="social"></a>Soziale Netzwerke
* [Active Directory-Teamblog](http://blogs.technet.com/b/ad/): Hier können Sie sich über die neuesten Entwicklungen der Azure Active Directory-Welt informieren.
* [Azure Active Directory Graph-Teamblog](http://blogs.msdn.com/b/aadgraphteam): Hier werden Azure Active Directory-Informationen speziell zur Graph-API bereitgestellt.
* [Cloudidentität](http://www.cloudidentity.net): Hier finden Sie Informationen zu IMaaS (Identity Management as a Service) von einem erfahrenen Azure Active Directory-PM.  
* [Azure Active Directory auf Twitter](https://twitter.com/azuread): Azure Active Directory-Ankündigungen in höchstens 140 Zeichen.

## <a name="windows-server-on-premises-development"></a>Windows Server – lokale Entwicklung
Hilfestellung bei der Entwicklung mit Windows Server und Active Directory Federation Services (ADFS) finden Sie unter:

* [AD FS Scenarios for Developers](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/ad-fs-scenarios-for-developers) (AD FS-Szenarios für Entwickler): bietet eine Übersicht über AD FS-Komponenten und deren Funktionsweise sowie Einzelheiten zu den unterstützten Authentifizierungs- und Autorisierungsszenarios.
* [AD FS walkthroughs](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/ad-fs-development) (Exemplarische Vorgehensweisen für AD FS): enthält eine Liste von Artikeln mit exemplarischen Vorgehensweisen, die ausführliche Anleitungen zum Implementieren der zugehörigen Authentifizierungs- und Autorisierungsabläufe bereitstellen.




<!--HONumber=Dec16_HO2-->


