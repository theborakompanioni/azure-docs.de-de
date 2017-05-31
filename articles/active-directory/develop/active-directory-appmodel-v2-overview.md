---
title: Azure Active Directory v2.0-Endpunkt | Microsoft-Dokumentation
description: "Eine Einführung in die Entwicklung von Apps mit Microsoft-Konto- und Azure Active Directory-Anmeldung."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 2dee579f-fdf6-474b-bc2c-016c931eaa27
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: 17c4dc6a72328b613f31407aff8b6c9eacd70d9a
ms.openlocfilehash: 4a9bf4193017313f532b398880a84966367f0b58
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017


---
# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a>Anmelden von Benutzern mit Microsoft-Konto und aus Azure AD bei einer einzelnen Anwendung
In der Vergangenheit musste ein App-Entwickler, der sowohl Unterstützung für persönliche Microsoft-Konten als auch Geschäftskonten über Azure Active Directory benötigte, die Integration für zwei separate Systeme bereitstellen.  Für den **Azure AD v2.0-Endpunkt** wurde eine neue Authentifizierungs-API-Version eingeführt, über die Sie Anmeldungen mit beiden Arten von Konten durch eine einfache Integration bereitstellen können.  Apps, die den v2.0-Endpunkt verwenden, können auch REST-APIs aus [Microsoft Graph](https://graph.microsoft.io) mit beiden Kontotypen nutzen.

## <a name="getting-started"></a>Erste Schritte
Wählen Sie unten Ihre bevorzugte Plattform aus, um eine App mit unseren Open Source-Bibliotheken und -Frameworks zu erstellen.  Alternativ können Sie unsere OAuth 2.0- und OpenID Connect-Protokolldokumentation verwenden, um Protokollmeldungen direkt ohne Verwendung einer Authentifizierungsbibliothek zu senden und zu empfangen.

<br />

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a>Neuerungen
Anhand der folgenden Informationen lässt sich besser verstehen, was mit dem v2.0-Endpunkt möglich ist und was nicht.

* Erfahren Sie mehr über die [App-Typen, die Sie mit dem v2.0-Endpunkt erstellen können](active-directory-v2-flows.md).
* Informieren Sie sich über die [Einschränkungen](active-directory-v2-limitations.md) des v2.0-Endpunkts.
* Sehen Sie sich das folgende Übersichtsvideo zum v2.0-Endpunkt an:

>[!VIDEO https://channel9.msdn.com/Events/Build/2017/P4031/player]

## <a name="reference"></a>Referenz
Die folgenden Links bieten ausführliche Informationen zur Plattform:

* [v2.0-Protokollreferenz](active-directory-v2-protocols.md)
* [v2.0-Tokenreferenz](active-directory-v2-tokens.md)
* [v2.0-Bibliotheksreferenz](active-directory-v2-libraries.md)
* [Bereiche, Berechtigungen und Zustimmung im v2.0-Endpunkt](active-directory-v2-scopes.md)
* [Microsoft Graph](https://graph.microsoft.io)

## <a name="help--support"></a>Hilfe und Support
Hier sind die besten Hilferessourcen für die Entwicklung mit Azure Active Directory angegeben.

* [Tags `azure-active-directory` und `adal` von Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory+or+adal)
* [Feedback zu Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences)


> [!NOTE]
> Wenn Sie nur Geschäfts- und Schulkonten aus Azure Active Directory anmelden müssen, sollten Sie für den Einstieg unser [Azure AD-Entwicklerhandbuch](active-directory-developers-guide.md) lesen.  Der v2.0-Endpunkt ist für die Verwendung durch Entwickler vorgesehen, die sich explizit bei persönlichen Microsoft-Konten anmelden müssen.


