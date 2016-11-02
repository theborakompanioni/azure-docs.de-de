<properties
    pageTitle="v2.0-Endpunkt – Übersicht | Microsoft Azure"
    description="Eine Einführung in die Entwicklung von Apps mit Microsoft-Konto- und Azure Active Directory-Anmeldung."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="dastrock"/>


# <a name="sign-in-microsoft-account-&-azure-ad-users-in-a-single-app"></a>Anmelden von Benutzern mit Microsoft-Konto und aus Azure AD bei einer einzelnen Anwendung

In der Vergangenheit musste ein App-Entwickler, der sowohl Unterstützung für Microsoft-Konten als auch für Azure Active Directory benötigte, die Integration für zwei separate Systeme bereitstellen.  Wir haben jetzt eine neue Version der Authentifizierungs-API eingeführt, über die Sie Benutzer mit beiden Arten von Konten im Azure AD-System anmelden können.  Dieses zusammengeführte Authentifizierungssystem wird als **v2.0-Endpunkt**bezeichnet.  Mit dem v2.0-Endpunkt können Sie mit einer einzelnen Implementierung eine Zielgruppe erreichen, die Millionen von Benutzern sowohl mit privaten als auch Geschäfts-, Schul- oder Unikonten umfasst.

Apps, die den v2.0-Endpunkt verwenden, können auch REST-APIs aus [Microsoft Graph](https://graph.microsoft.io) und [Office 365](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) mit beiden Kontotypen nutzen.

<!-- For a quick introduction to the v2.0 endpoint, please view the [Getting Started with Microsoft Identities: Enterprise Grade Sign In For Your Apps](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/) video. -->

## <a name="getting-started"></a>Erste Schritte
[AZURE.VIDEO build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps]

Wählen Sie unten Ihre bevorzugte Plattform aus, um eine App mit unseren Open Source-Bibliotheken und -Frameworks zu erstellen.  Alternativ können Sie unsere OAuth 2.0- und OpenID Connect-Protokolldokumentation verwenden, um Protokollmeldungen direkt ohne Verwendung einer Authentifizierungsbibliothek zu senden und zu empfangen.

<!-- TODO: Finalize this table  -->
[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]

## <a name="what's-new"></a>Neuerungen
Anhand der folgenden konzeptuellen Informationen lässt sich besser verstehen, was mit dem v2.0-Endpunkt möglich ist und was nicht.

- Erfahren Sie mehr über die [App-Typen, die Sie mit dem v2.0-Endpunkt erstellen können](active-directory-v2-flows.md).
- Informieren Sie sich über die [Einschränkungen](active-directory-v2-limitations.md) des v2.0-Endpunkts.
- Seit Kurzem werden auf [Administratoren beschränkte Bereiche](active-directory-v2-scopes.md) und die Gewährung von [OAuth2-Clientanmeldeinformationen](active-directory-v2-protocols-oauth-client-creds.md) unterstützt.  Probieren Sie es aus!

## <a name="reference"></a>Referenz
Die folgenden Links bieten ausführliche Informationen zur Plattform:

- Build 2016: [Getting Started with Microsoft Identities: Enterprise Grade Sign In For Your Apps](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/)
- Hilfe zu Stack Overflow mit [azure-active-directory](http://stackoverflow.com/questions/tagged/azure-active-directory)- oder [adal](http://stackoverflow.com/questions/tagged/adal)-Tags.
- [v2.0-Protokollreferenz](active-directory-v2-protocols.md)
- [v2.0-Tokenreferenz](active-directory-v2-tokens.md)
- [v2.0-Bibliotheksreferenz](active-directory-v2-libraries.md)
- [Bereiche, Berechtigungen und Zustimmung im v2.0-Endpunkt](active-directory-v2-scopes.md)
- [Microsoft App-Registrierungsportal](https://apps.dev.microsoft.com)
- [Office 365-REST-API-Referenz](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2)
- [Microsoft Graph](https://graph.microsoft.io)


<!--HONumber=Oct16_HO2-->


