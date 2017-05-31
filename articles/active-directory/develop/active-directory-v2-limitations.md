---
title: "Azure Active Directory v2.0-Endpunkt – Einschränkungen | Microsoft Docs"
description: "Eine Liste der Einschränkungen des v2.0-Endpunkts von Azure AD."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: a99289c0-e6ce-410c-94f6-c279387b4f66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: bd24c8ba65277b224869351e261e365d699b56e3
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="should-i-use-the-v20-endpoint"></a>Sollte ich den v2.0-Endpunkt verwenden?
Beim Erstellen von Anwendungen, die in Azure Active Directory integriert werden, müssen Sie entscheiden, ob der v2.0-Endpunkt und die Authentifizierungsprotokolle Ihren Anforderungen entsprechen. Der ursprüngliche Azure Active Directory-Endpunkt wird weiterhin vollständig unterstützt und umfasst in gewisser Hinsicht mehr Funktionen als v2.0. Allerdings werden mit dem v2.0-Endpunkt [bedeutende Vorteile](active-directory-v2-compare.md) für Entwickler eingeführt.

So sieht unsere vereinfachte Empfehlung für Entwickler derzeit aus:

* Wenn Sie in Ihrer Anwendung persönliche Microsoft-Konten unterstützen müssen, verwenden Sie den v2.0-Endpunkt. Zuvor sollten Sie sich jedoch mit den in diesem Artikel behandelten Einschränkungen vertraut machen.
* Wenn Ihre Anwendung nur Microsoft-Geschäfts-, Schul- und Unikonten unterstützen muss, verwenden Sie den v2. 0-Endpunkt nicht. Konsultieren Sie stattdessen unser [Azure AD-Entwicklerhandbuch](active-directory-developers-guide.md).

Im Laufe der Zeit wird der v2.0-Endpunkt erweitert, und die hier aufgeführten Einschränkungen werden beseitigt, sodass Sie nur noch den v2.0-Endpunkt verwenden können. In der Zwischenzeit können Sie mithilfe dieses Artikels ermitteln, ob der v2.0-Endpunkt für Sie geeignet ist. Wir werden diesen Artikel immer wieder aktualisieren, damit er den aktuellen Status des v2.0-Endpunkts widerspiegelt. Vergleichen Sie nochmals Ihre Anforderungen mit den v2.0-Funktionen.

Wenn Sie eine vorhandene Azure AD-App einsetzen, die keinen v2.0-Endpunkt verwendet, müssen Sie nicht bei null anfangen. Wir werden künftig eine Möglichkeit bieten, Ihre vorhandenen Azure AD-Anwendungen mit dem v2.0-Endpunkt zu verwenden.

## <a name="restrictions-on-app-types"></a>Einschränkungen bei App-Typen
Die folgenden App-Typen werden derzeit nicht vom v2.0-Endpunkt unterstützt. Eine Beschreibung der unterstützten App-Typen finden Sie unter [App-Typen für den v2.0-Endpunkt](active-directory-v2-flows.md).

### <a name="standalone-web-apis"></a>Eigenständige Web-APIs
Sie können den v2.0-Endpunkt verwenden, um [eine Web-API zu erstellen, die mit OAuth 2.0 gesichert wird](active-directory-v2-flows.md#web-apis). Allerdings kann die Web-API nur Token von einer Anwendung empfangen, die dieselbe Anwendungs-ID aufweist. Sie können nicht von einem Client, der über eine andere Anwendungs-ID verfügt, auf eine Web-API zugreifen. Diesem Client ist es nicht möglich, Berechtigungen für Ihre Web-API anzufordern oder zu erhalten.

Wenn Sie wissen möchten, wie eine Web-API erstellt wird, die Token von einem Client mit derselben Anwendungs-ID akzeptiert, beachten Sie die Web-API-Beispiele zum v2.0-Endpunkt unter [Erste Schritte](active-directory-appmodel-v2-overview.md#getting-started).

## <a name="restrictions-on-app-registrations"></a>Einschränkungen für App-Registrierungen
Derzeit müssen Sie für jede App, die Sie mit dem v2.0-Endpunkt integrieren möchten, eine App-Registrierung im neuen [Microsoft Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) erstellen. Vorhandene Azure AD- oder Microsoft-Konto-Apps sind nicht mit dem v2.0-Endpunkt kompatibel. Apps, die in einem Portal registriert sind, bei dem es sich nicht um das Anwendungsregistrierungsportal handelt, sind nicht mit dem v2.0-Endpunkt kompatibel. Für die Zukunft ist eine Möglichkeit geplant, vorhandene Anwendungen als v2.0-App zu verwenden. Gegenwärtig gibt es für vorhandene Apps jedoch noch keinen Migrationspfad für das Arbeiten mit dem v2.0-Endpunkt.

Darüber hinaus gelten für App-Registrierungen, die Sie im [Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) erstellen, die folgenden Einschränkungen:

* Pro Anwendungs-ID sind nur zwei App-Geheimnisse zulässig.
* Eine von einem Benutzer mit einem persönlichen Microsoft-Konto vorgenommene App-Registrierung kann nur von einem einzelnen Entwicklerkonto angezeigt und verwaltet werden. Sie kann nicht von mehreren Entwicklern gemeinsam genutzt werden.  Wenn Sie Ihre App-Registrierung mit mehreren Entwicklern gemeinsam nutzen möchten, können Sie die Anwendung erstellen, indem Sie sich mit einem Azure AD-Konto beim Registrierungsportal anmelden.
* Es gibt mehrere Einschränkungen, was das zulässige Format des Umleitungs-URI angeht. Weitere Informationen zu Umleitungs-URIs finden Sie im nächsten Abschnitt.

## <a name="restrictions-on-redirect-uris"></a>Einschränkungen für Umleitungs-URIs
Im Anwendungsregistrierungsportal registrierte Apps sind derzeit auf eine begrenzte Zahl von Umleitungs-URI-Werten beschränkt. Der Umleitungs-URI für Web-Apps und -Dienste muss mit dem Schema `https` beginnen, und alle Umleitungs-URI-Werte müssen dieselbe DNS-Domäne verwenden. Es ist beispielsweise nicht möglich, eine Web-App zu registrieren, die einen der folgenden Umleitungs-URIs aufweist:

`https://login-east.contoso.com`  
`https://login-west.contoso.com`

Das Registrierungssystem vergleicht den gesamten DNS-Namen des vorhandenen Umleitungs-URI mit dem DNS-Namen des von Ihnen hinzugefügten Umleitungs-URI. Die Anforderung zum Hinzufügen des DNS-Namens schlägt fehl, wenn eine der folgenden Bedingungen erfüllt ist:  

* Der gesamte DNS-Name des neuen Umleitungs-URI entspricht nicht dem DNS-Namen des vorhandenen Umleitungs-URI.
* Der gesamte DNS-Name des neuen Umleitungs-URI ist keine Unterdomäne des vorhandenen Umleitungs-URI.

Ein Beispiel: Die App hat den folgenden Umleitungs-URI:

`https://login.contoso.com`

Eine Hinzufügung ist wie folgt möglich:

`https://login.contoso.com/new`

In diesem Fall stimmt der DNS-Name exakt überein. Alternativ haben Sie folgende Möglichkeit:

`https://new.login.contoso.com`

In diesem Fall verweisen Sie auf eine DNS-Unterdomäne von login.contoso.com. Wenn Sie eine App mit den Umleitungs-URIs „login-east.contoso.com“ und „login-west.contoso.com“ benötigen, müssen Sie die folgenden Umleitungs-URIs in der angegebenen Reihenfolge hinzufügen:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

Die beiden letztgenannten URIs können hinzugefügt werden, da es sich hierbei um Unterdomänen des ersten Umleitungs-URI „contoso.com“ handelt. Diese Einschränkung wird in einer zukünftigen Version aufgehoben.

Informationen zum Registrieren einer App im Anwendungsregistrierungsportal finden Sie unter [Registrieren einer App mit dem v2.0-Endpunkt](active-directory-v2-app-registration.md).

## <a name="restrictions-on-services-and-apis"></a>Einschränkungen für Dienste und APIs
Der v2.0-Endpunkt unterstützt derzeit die Anmeldung für jede App, die im Anwendungsregistrierungsportal registriert ist, sofern sie in der Liste der [unterstützten Authentifizierungsflüsse](active-directory-v2-flows.md) enthalten ist. Diese Apps können OAuth 2.0-Zugriffstoken jedoch nur für einen sehr begrenzten Satz von Ressourcen erhalten. Der v2.0-Endpunkt gibt Zugriffstoken nur aus für:

* Die App, die das Token angefordert hat. Eine App kann ein Zugriffstoken für sich selbst abrufen, wenn die logische App aus mehreren unterschiedlichen Komponenten oder Schichten besteht. Wenn Sie dieses Szenario in Aktion sehen möchten, besuchen Sie unsere [Erste Schritte](active-directory-appmodel-v2-overview.md#getting-started)-Tutorials.
* Die Outlook-Mail-, -Kalender- und -Kontakte-REST-APIs, die sich alle unter „https://outlook.office.com“ befinden. Um zu erfahren, wie Sie eine App schreiben, die auf diese APIs zugreift, beachten Sie die Tutorials [Erste Schritte mit Office](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2).
* Microsoft Graph-APIs. Informieren Sie sich bei Bedarf über [Microsoft Graph](https://graph.microsoft.io) und die Daten, die Ihnen zur Verfügung stehen.

Zu diesem Zeitpunkt werden keine anderen Dienste unterstützt. In Zukunft werden weitere Microsoft Online Services sowie der Support für Ihre eigenen Web-APIs und Dienste hinzugefügt werden.

## <a name="restrictions-on-libraries-and-sdks"></a>Einschränkungen für Bibliotheken und SDKs
Die Bibliotheksunterstützung für den v2.0-Endpunkt ist gegenwärtig eingeschränkt. Wenn Sie den v2.0-Endpunkt in einer Produktionsanwendung verwenden möchten, stehen Ihnen folgende Möglichkeiten offen:

* Wenn Sie eine Webanwendung erstellen, können Sie die allgemein verfügbare serverseitige Middleware von Microsoft bedenkenlos für die Anmeldung und Tokenüberprüfung einsetzen. Dazu gehören die OWIN Open ID Connect-Middleware für ASP.NET und das NodeJS Passport-Plug-In. Codebeispiele, die Microsoft-Middleware verwenden, finden Sie unter [Erste Schritte](active-directory-appmodel-v2-overview.md#getting-started).
* Wenn Sie eine Desktop- oder mobile Anwendung entwickeln, können Sie eine der Microsoft Authentication Libraries (MSAL, derzeit in der Vorschauphase) verwenden.  Diese Bibliotheken gibt es in der für die Produktion unterstützten Vorschauversion, die Sie gefahrlos in Produktionsanwendungen einsetzen können. In unserer [Referenz zu Authentifizierungsbibliotheken](active-directory-v2-libraries.md) erfahren Sie mehr zu den Nutzungsbedingungen der Vorschauversion und verfügbaren Bibliotheken.
* Für Plattformen, die nicht von den Microsoft-Bibliotheken abgedeckt sind, können Sie die Integration mit dem v2.0-Endpunkt auch erreichen, indem Sie Protokollnachrichten direkt in Ihrem Anwendungscode senden und empfangen. Die v2.0-Protokolle OpenID Connect und OAuth [wurden explizit dokumentiert](active-directory-v2-protocols.md), um Sie bei einer solchen Integration zu unterstützen.
* Zudem können Sie die Open ID Connect- und OAuth-Open Source-Bibliotheken für die Integration in den v2.0-Endpunkt verwenden. Das v2.0-Protokoll sollte ohne wesentliche Änderungen mit vielen Open Source-Protokollbibliotheken kompatibel sein. Die Verfügbarkeit dieser Bibliotheksarten variiert je nach Sprache und Plattform. Die [Open ID Connect](http://openid.net/connect/)- und die [OAuth 2.0](http://oauth.net/2/)-Websites enthalten eine Liste gängiger Implementierungen. Weitere Informationen und die Liste der Open Source-Clientbibliotheken und -Beispiele, die mit dem v2.0-Endpunkt getestet wurden, finden Sie unter [Azure Active Directory (AD) v2.0 und Authentifizierungsbibliotheken](active-directory-v2-libraries.md).

## <a name="restrictions-on-protocols"></a>Einschränkungen für Protokolle
Der v2.0-Endpunkt unterstützt keinen SAML- oder WS-Verbund, sondern nur Open ID Connect und OAuth 2.0.  Nicht alle Features und Funktionen von OAuth-Protokollen wurden in den v2.0-Endpunkt integriert. Die folgenden Protokollfeatures und -funktionen sind aktuell im v2. 0-Endpunkt *nicht verfügbar*:

* ID-Token, die vom v2.0-Endpunkt ausgegeben werden, enthalten keinen `email`-Anspruch für den Benutzer, selbst wenn Sie vom Benutzer die Berechtigung zum Anzeigen seiner E-Mail erhalten.
* Der OpenID Connect UserInfo-Endpunkt ist für den v2.0-Endpunkt nicht implementiert. Alle Benutzerprofildaten, die Sie potenziell an diesem Endpunkt empfangen würden, stehen jedoch über den Microsoft Graph-Endpunkt `/me` zur Verfügung.
* Die Ausgabe von Rollen- oder Gruppenansprüchen in ID-Token wird vom v2.0-Endpunkt nicht unterstützt.
* [OAuth 2.0 Resource Owner Password Credentials Grant](https://tools.ietf.org/html/rfc6749#section-4.3) (Gewährung der Kennwortanmeldeinformationen des Ressourcenbesitzers) wird vom v2.0-Endpunkt nicht unterstützt.

Darüber hinaus unterstützt der v2.0-Endpunkt keine SAML- oder WS-Verbundprotokolle.

Informationen zum Umfang der vom v2.0-Endpunkt unterstützten Protokollfunktionen finden Sie in der Referenz [v2.0-Protokolle – OAuth 2.0 und OpenID Connect](active-directory-v2-protocols.md).

## <a name="restrictions-for-work-and-school-accounts"></a>Einschränkungen für Geschäfts- und Schulkonten
Wenn Sie die Active Directory Authentication Library (ADAL) in Windows-Anwendungen verwendet haben, haben Sie möglicherweise die integrierte Windows-Authentifizierung genutzt, die mit der SAML-Assertionsgewährung (Security Assertion Markup Language) arbeitet. Diese Gewährung erlaubt Benutzern von Azure AD-Partnermandanten, sich im Hintergrund bei ihrer lokalen Active Directory-Instanz zu authentifizieren, ohne ihre Anmeldeinformationen eingeben zu müssen. Die SAML-Assertionsgewährung wird vom v2.0-Endpunkt derzeit nicht unterstützt.
