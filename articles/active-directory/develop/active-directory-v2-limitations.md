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
ms.date: 01/07/2017
ms.author: dastrock
translationtype: Human Translation
ms.sourcegitcommit: 3e0bb32a6c60011d71606c896cc506f430bc3c27
ms.openlocfilehash: 5d1ceabeeee8cef0170b928703488845f70656ef


---
# <a name="should-i-use-the-v20-endpoint"></a>Sollte ich den v2.0-Endpunkt verwenden?
Beim Erstellen von Anwendungen, die in Azure Active Directory (Azure AD) integriert werden, müssen Sie entscheiden, ob der v2.0-Endpunkt und die Authentifizierungsprotokolle Ihren Anforderungen entsprechen. Der ursprüngliche Azure AD-Endpunkt wird weiterhin vollständig unterstützt und umfasst in gewisser Hinsicht mehr Funktionen als v2.0. Allerdings werden mit dem v2.0-Endpunkt [bedeutende Vorteile](active-directory-v2-compare.md) für Entwickler eingeführt. Möglicherweise entscheiden Sie sich aufgrund dieser Vorteile von v2.0, das neue Programmiermodell zu verwenden.

Nachfolgend finden Sie unsere Empfehlungen, wenn Sie den v2.0-Endpunkt zum gegenwärtigen Zeitpunkt verwenden möchten:

* Wenn Sie in Ihrer Anwendung persönliche Microsoft-Konten unterstützen möchten, sollten Sie den v2.0-Endpunkt verwenden. Zuvor sollten Sie sich jedoch mit den in diesem Artikel behandelten Einschränkungen vertraut machen, insbesondere denjenigen, die Geschäfts- und Schulkonten betreffen.
* Wenn Ihre Anwendung nur Geschäfts- und Schulkonten unterstützen muss, sollten Sie [die ursprünglichen Azure AD-Endpunkte](active-directory-developers-guide.md) verwenden.

Im Laufe der Zeit wird der v2.0-Endpunkt erweitert, und die hier aufgeführten Einschränkungen werden beseitigt, sodass Sie nur noch den v2.0-Endpunkt verwenden können. In der Zwischenzeit können Sie mithilfe dieses Artikels ermitteln, ob der v2.0-Endpunkt für Sie geeignet ist. Wir werden diesen Artikel immer wieder aktualisieren, damit er den aktuellen Status des v2.0-Endpunkts widerspiegelt. Vergleichen Sie nochmals Ihre Anforderungen mit den v2.0-Funktionen.

Wenn Sie eine vorhandene Azure AD-App einsetzen, die keinen v2.0-Endpunkt verwendet, müssen Sie nicht bei Null anfangen. Wir werden künftig eine Möglichkeit bieten, Ihre vorhandenen Azure AD-Anwendungen mit dem v2.0-Endpunkt zu verwenden.

## <a name="restrictions-on-app-types"></a>Einschränkungen bei App-Typen
Die folgenden App-Typen werden derzeit nicht vom v2.0-Endpunkt unterstützt. Eine Beschreibung der unterstützten App-Typen finden Sie unter [App-Typen für den v2.0-Endpunkt](active-directory-v2-flows.md).

### <a name="standalone-web-apis"></a>Eigenständige Web-APIs
Sie können den v2.0-Endpunkt verwenden, um [eine Web-API zu erstellen, die mit OAuth 2.0 gesichert wird](active-directory-v2-flows.md#web-apis). Allerdings kann die Web-API nur Token von einer Anwendung empfangen, die dieselbe Anwendungs-ID aufweist. Sie können nicht von einem Client, der über eine andere Anwendungs-ID verfügt, auf eine Web-API zugreifen. Diesem Client ist es nicht möglich, Berechtigungen für Ihre Web-API anzufordern oder zu erhalten.

Wenn Sie wissen möchten, wie eine Web-API erstellt wird, die Token von einem Client mit derselben Anwendungs-ID akzeptiert, beachten Sie die Web-API-Beispiele zum v2.0-Endpunkt unter [Erste Schritte](active-directory-appmodel-v2-overview.md#getting-started).

### <a name="web-api-on-behalf-of-flow"></a>„Im Auftrag von“-Web-API-Ablauf
Viele Architekturen umfassen eine Web-API, die eine weitere nachgeordnete Web-API aufrufen muss. Beide werden jeweils durch den v2.0-Endpunkt gesichert. Dieses Szenario kommt häufig bei nativen Clients mit Web-API-Back-End vor, das wiederum eine Instanz der Microsoft Online Services oder eine weitere benutzerdefinierte Web-API aufruft, die Azure AD unterstützt.

Dieses Szenario kann mithilfe der Berechtigung für Anmeldeinformationen über den OAuth 2.0 JSON Web Token (JWT)-Bearer erstellt werden, der auch als „Im Auftrag von“-Ablauf bezeichnet wird. Der „Im Auftrag von“-Ablauf wird für den v2.0-Endpunkt aktuell jedoch noch nicht unterstützt. Beachten Sie das [Im-Auftrag-von-Codebeispiel auf GitHub](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet), um sich die Funktionsweise dieses Ablaufs im allgemein verfügbaren Azure AD-Dienst anzusehen.

## <a name="restrictions-on-app-registrations"></a>Einschränkungen für App-Registrierungen
Derzeit müssen Sie für jede App, die Sie mit dem v2.0-Endpunkt integrieren möchten, eine App-Registrierung im neuen [Microsoft Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) erstellen. Vorhandene Azure AD- oder Microsoft-Konto-Apps sind nicht mit dem v2.0-Endpunkt kompatibel. Apps, die in einem Portal registriert sind, bei dem es sich nicht um das Anwendungsregistrierungsportal handelt, sind nicht mit dem v2.0-Endpunkt kompatibel. Für die Zukunft ist eine Möglichkeit geplant, vorhandene Anwendungen als v2.0-App zu verwenden. Gegenwärtig gibt es für vorhandene Apps jedoch noch keinen Migrationspfad für das Arbeiten mit dem v2.0-Endpunkt.

Apps, die im Anwendungsregistrierungsportal registriert sind, können nicht mit dem ursprünglichen Azure AD-Authentifizierungsendpunkt verwendet werden. Sie können jedoch von Ihnen im Anwendungsregistrierungsportal erstellte Apps erfolgreich in den Microsoft-Konto-Authentifizierungsendpunkt `https://login.live.com` integrieren.

Darüber hinaus gelten für App-Registrierungen, die Sie im [Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) erstellen, die folgenden Einschränkungen:

* Die **homepage**-Eigenschaft (auch bekannt als *Anmelde-URL*) wird nicht unterstützt. Ohne eine Homepage werden diese Anwendungen nicht im MyApps-Bereich von Office angezeigt.
* Pro Anwendungs-ID sind gegenwärtig nur zwei geheime App-Schlüssel zulässig.
* Eine App-Registrierung kann nur über ein einziges Entwicklerkonto angezeigt und verwaltet werden. Sie kann nicht von mehreren Entwicklern gemeinsam genutzt werden.
* Es gibt mehrere Einschränkungen, was das zulässige Format des Umleitungs-URI angeht. Weitere Informationen zu Umleitungs-URIs finden Sie im nächsten Abschnitt.

## <a name="restrictions-on-redirect-uris"></a>Einschränkungen für Umleitungs-URIs
Im Anwendungsregistrierungsportal registrierte Apps sind derzeit auf eine begrenzte Zahl von Umleitungs-URI-Werten beschränkt. Der Umleitungs-URI für Web-Apps und -Dienste muss mit dem Schema `https` beginnen, und alle Umleitungs-URI-Werte müssen dieselbe DNS-Domäne verwenden. Es ist beispielsweise nicht möglich, eine Web-App zu registrieren, die einen der folgenden Direkt-URIs aufweist:

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
* Für andere Plattformen sowie für native und mobile Anwendungen können Sie die Integration mit dem v2.0-Endpunkt auch durchführen, indem Sie Protokollnachrichten direkt in Ihrem Anwendungscode senden und empfangen. Die v2.0-Protokolle OpenID Connect und OAuth [wurden explizit dokumentiert](active-directory-v2-protocols.md), um Sie bei einer solchen Integration zu unterstützen.
* Zudem können Sie die Open ID Connect- und OAuth-Open Source-Bibliotheken für die Integration in den v2.0-Endpunkt verwenden. Das v2.0-Protokoll sollte ohne wesentliche Änderungen mit vielen Open Source-Protokollbibliotheken kompatibel sein. Die Verfügbarkeit dieser Bibliotheksarten variiert je nach Sprache und Plattform. Die [Open ID Connect](http://openid.net/connect/)- und die [OAuth 2.0](http://oauth.net/2/)-Websites enthalten eine Liste gängiger Implementierungen. Weitere Informationen und die Liste der Open Source-Clientbibliotheken und -Beispiele, die mit dem v2.0-Endpunkt getestet wurden, finden Sie unter [Azure Active Directory (AD) v2.0 und Authentifizierungsbibliotheken](active-directory-v2-libraries.md).

Ferner wurde eine erste Vorschau der [Microsoft Authentication Library (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) ausschließlich für .NET veröffentlicht. Diese Bibliothek können Sie in .NET-Client- und -Serveranwendungen testen. Da es sich um eine Vorschaubibliothek handelt, wird allerdings kein Support von GA-Qualität (allgemeine Verfügbarkeit) bereitgestellt.

## <a name="restrictions-on-protocols"></a>Einschränkungen für Protokolle
Der v2.0-Endpunkt unterstützt nur Open ID Connect und OAuth 2.0. Allerdings wurden nicht alle Features und Funktionen der einzelnen Protokolle in den v2.0-Endpunkt integriert.

Die folgenden gängigen Protokollfeatures und -funktionen sind aktuell im v2.&0;-Endpunkt *nicht verfügbar*:

* Der OpenID Connect-Parameter `end_session_endpoint`, der einer Anwendung das Beenden der Benutzersitzung gestattet, ist mit dem v2.0-Endpunkt nicht verfügbar.
* Vom v2.0-Endpunkt ausgegebene ID-Token enthalten nur einen Paarbezeichner für den Benutzer. Dies bedeutet, dass zwei verschiedene Anwendungen unterschiedliche IDs für denselben Benutzer erhalten. Beachten Sie, dass Sie durch Abfragen des Microsoft Graph-Endpunkts `/me` eine korrelierbare ID für den Benutzer erhalten, die anwendungsübergreifend verwendet werden kann.
* ID-Token, die vom v2.0-Endpunkt ausgegeben werden, enthalten keinen `email`-Anspruch für den Benutzer, selbst wenn Sie vom Benutzer die Berechtigung zum Anzeigen seiner E-Mail erhalten.
* Der OpenID Connect UserInfo-Endpunkt ist für den v2.0-Endpunkt nicht implementiert. Alle Benutzerprofildaten, die Sie potenziell an diesem Endpunkt empfangen würden, stehen jedoch über den Microsoft Graph-Endpunkt `/me` zur Verfügung.
* Die Ausgabe von Rollen- oder Gruppenansprüchen in ID-Token wird vom v2.0-Endpunkt nicht unterstützt.

Informationen zum Umfang der vom v2.0-Endpunkt unterstützten Protokollfunktionen finden Sie in der Referenz [v2.0-Protokolle – OAuth 2.0 und OpenID Connect](active-directory-v2-protocols.md).

## <a name="restrictions-for-work-and-school-accounts"></a>Einschränkungen für Geschäfts- und Schulkonten
Es gibt einige spezielle Funktionen für Microsoft-Unternehmensbenutzer, die noch nicht vom v2.0-Endpunkt unterstützt werden. Weitere Informationen hierzu finden Sie in den nachstehenden Abschnitten.

### <a name="device-based-conditional-access-native-and-mobile-apps-and-microsoft-graph"></a>Gerätebasierter bedingter Zugriff, native und mobile Apps und Microsoft Graph
Der v2.0-Endpunkt unterstützt noch keine Geräteauthentifizierung für mobile und native Anwendungen, z.B. native Apps unter iOS oder Android. In einigen Organisationen wird Ihre native Anwendung möglicherweise daran gehindert, Microsoft Graph aufzurufen. Die Geräteauthentifizierung ist erforderlich, wenn ein Administrator eine gerätebasierte bedingte Zugriffsrichtlinie für eine Anwendung festlegt. Für den v2.0-Endpunkt ist das wahrscheinlichste Szenario für einen gerätebasierten bedingten Zugriff ein Administrator, der eine Richtlinie für eine Ressource in Microsoft Graph festlegt, z.B. für die Outlook-API. Wenn ein Administrator diese Richtlinie festlegt und Ihre native Anwendung ein Token für Microsoft Graph anfordert, schlägt die Anforderung letztlich fehl, da die Geräteauthentifizierung noch nicht unterstützt wird. Webanwendungen, die Token für Microsoft Graph anfordern, werden jedoch unterstützt, wenn gerätebasierte Richtlinien konfiguriert wurden. Im Web-App-Szenario erfolgt die Geräteauthentifizierung über den Webbrowser des Benutzers.

Als Entwickler haben Sie wahrscheinlich keine Kontrolle darüber, wann Richtlinien für Microsoft Graph-Ressourcen festgelegt werden, bzw. bemerken es erst gar nicht. Möglicherweise bemerken Sie nicht einmal, wenn dieser Fall eintritt. Wenn Sie eine Anwendung für Geschäfts-, Schul- oder Unibenutzer erstellen, verwenden Sie den [ursprünglichen Azure AD-Endpunkt](active-directory-developers-guide.md), bis der v2.0-Endpunkt die Geräteauthentifizierung unterstützt. Erfahren Sie mehr über den [gerätebasierten bedingten Zugriff in Azure AD](../active-directory-conditional-access.md#device-based-conditional-access).

### <a name="windows-integrated-authentication-for-federated-tenants"></a>Integrierte Windows-Authentifizierung für Partnermandanten
Wenn Sie die Active Directory Authentication Library (ADAL) (mit dem ursprünglichen Azure AD-Endpunkt) in Windows-Anwendungen verwendet haben, haben Sie möglicherweise die Assertionsgewährung für die Security Assertion Markup Language (SAML) genutzt. Diese Gewährung erlaubt Benutzern von Azure AD-Partnermandanten, sich im Hintergrund bei ihrer lokalen Active Directory-Instanz zu authentifizieren, ohne ihre Anmeldeinformationen eingeben zu müssen. Die SAML-Assertionsgewährung wird vom v2.0-Endpunkt derzeit nicht unterstützt.




<!--HONumber=Jan17_HO3-->


