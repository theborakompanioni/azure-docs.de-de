---
title: 'v2.0-Endpunkt: Einschränkungen | Microsoft Docs'
description: Eine Liste der Einschränkungen des v2.0-Endpunkts in Azure AD.
services: active-directory
documentationcenter: ''
author: dstrockis
manager: mbaldwin
editor: ''

ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: dastrock

---
# <a name="should-i-use-the-v2.0-endpoint?"></a>Sollte ich den v2.0-Endpunkt verwenden?
Beim Erstellen von Anwendungen, die in Azure Active Directory integriert werden, müssen Sie entscheiden, ob der v2.0-Endpunkt und die Authentifizierungsprotokolle Ihren Anforderungen entsprechen.  Der ursprüngliche Azure AD-Endpunkt wird weiterhin vollständig unterstützt und umfasst in gewisser Hinsicht mehr Funktionen als v2.0.  Der v2.0-Endpunkt bietet jedoch [entscheidende Vorteile](active-directory-v2-compare.md) für Entwickler, die Sie zur Verwendung des neuen Programmiermodells verleiten könnten.

Zu diesem Zeitpunkt empfehlen wir für die Verwendung des v2.0-Endpunkts Folgendes:

* Wenn Sie persönliche Microsoft-Konten in Ihrer Anwendung unterstützen möchten, sollten Sie den v2.0-Endpunkt verwenden.  Stellen Sie jedoch sicher, dass Sie die in diesem Artikel aufgeführten Einschränkungen kennen, insbesondere diejenigen, die sich auf Geschäfts-, Schul- und Unikonten beziehen.
* Wenn Ihre Anwendung nur Geschäfts-, Schul- oder Unikonten unterstützen soll, verwenden Sie [die ursprünglichen Azure AD-Endpunkte](active-directory-developers-guide.md).

Im Laufe der Zeit wird der v2.0-Endpunkt erweitert, und die hier aufgeführten Einschränkungen werden beseitigt, sodass Sie nur noch den v2.0-Endpunkt verwenden können.  In der Zwischenzeit können Sie anhand dieses Artikels ermitteln, ob der v2.0-Endpunkt für Sie geeignet ist.  Dieser Artikel wird fortlaufend auf den aktuellen Status des v2.0-Endpunkts aktualisiert. Schauen Sie daher regelmäßig vorbei, um Ihre Anforderungen anhand der v2.0-Funktionen neu zu überprüfen.

Wenn Sie eine vorhandene App mit Azure AD einsetzen, die den v2.0-Endpunkt nicht verwendet, müssen Sie nicht bei Null anfangen.  In Zukunft bieten wir eine Möglichkeit, vorhandene Azure AD-Anwendungen auf die Verwendung mit dem v2.0-Endpunkt umzustellen.

## <a name="restrictions-on-apps"></a>Einschränkungen für Apps
Die folgenden App-Typen werden derzeit vom v2.0-Endpunkt nicht unterstützt.  Eine Beschreibung der unterstützten Typen von Apps finden Sie [in diesem Artikel](active-directory-v2-flows.md).

##### <a name="standalone-web-apis"></a>Eigenständige Web-APIs
Mit dem v2.0-Endpunkt können Sie eine [Web-API erstellen, die mit dem OAuth 2.0-Token geschützt wird](active-directory-v2-flows.md#web-apis).  Allerdings kann die Web-API nur Token von einer Anwendung empfangen, die die gleiche Anwendungs-ID aufweist.  Das Erstellen einer Web-API, auf die von einem Client mit einer anderen Anwendungs-ID zugegriffen wird, wird nicht unterstützt.  Dieser Client kann keine Berechtigungen für Ihre Web-API anfordern oder erhalten.

Wenn Sie wissen möchten, wie eine Web-API erstellt wird, die Token von einem Client mit der gleichen App-ID akzeptiert, sehen Sie in den Web-API-Beispielen zum v2.0-Endpunkt unter [Erste Schritte](active-directory-appmodel-v2-overview.md#getting-started).

##### <a name="web-api-on-behalf-of-flow"></a>„Im Auftrag von“-Web-API-Ablauf
Viele Architekturen umfassen eine Web-API, die eine weitere nachgeordnete Web-API aufrufen muss. Beide werden jeweils durch den v2.0-Endpunkt gesichert.  Dieses Szenario kommt häufig bei nativen Clients mit Web-API-Back-End vor, das wiederum einen Microsoft-Onlinedienst oder eine weitere benutzerdefinierte Web-API aufruft, die Azure AD unterstützt.

Dieses Szenario kann mithilfe der Berechtigung für Anmeldeinformationen über den OAuth 2.0-JWT-Bearer unterstützt werden, auch bekannt als „Im Auftrag von“-Ablauf.  Der „Im Auftrag von“-Ablauf wird für den v2.0-Endpunkt derzeit jedoch noch nicht unterstützt.  Um sich die Funktionsweise dieses Vorgangs im allgemein verfügbaren Azure AD-Dienst anzusehen, wechseln Sie zum [Im-Auftrag-von-Codebeispiel auf GitHub](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet).

## <a name="restrictions-on-app-registrations"></a>Einschränkungen für App-Registrierungen
Zu diesem Zeitpunkt müssen alle Apps, die in den v2.0-Endpunkt integriert werden sollen, eine neue App-Registrierung unter [apps.dev.microsoft.com](https://apps.dev.microsoft.com) erstellen.  Keine der vorhandenen Azure AD- oder Microsoft-Konto-Apps sind mit dem v2.0-Endpunkt kompatibel. Dasselbe gilt für Apps, die in einem anderen Portal als dem neuen App-Registrierungsportal registriert werden.  Wir planen eine Möglichkeit, vorhandene Anwendungen für die Verwendung als v2.0-App zu aktivieren. Zu diesem Zeitpunkt gibt es für eine App jedoch keinen Migrationspfad zum v2.0-Endpunkt.

Ebenso können Apps, die im neuen App-Registrierungsportal registriert sind, nicht mit dem ursprünglichen Azure AD-Authentifizierungsendpunkt verwendet werden.  Sie können jedoch im App-Registrierungsportal erstellte Apps erfolgreich in den Microsoft-Konto-Authentifizierungsendpunkt, `https://login.live.com`, integrieren.

Darüber hinaus ist bei App-Registrierungen, die unter [apps.dev.microsoft.com](https://apps.dev.microsoft.com) erstellt wurden, Folgendes zu beachten:

* Die **homepage**-Eigenschaft, auch bekannt als die **Anmelde-URL**, wird nicht unterstützt.  Ohne eine Homepage werden diese Anwendungen nicht im MyApps-Bereich von Office angezeigt.
* Pro Anwendungs-ID sind zu diesem Zeitpunkt nur zwei geheime App-Schlüssel zulässig.
* Eine App-Registrierung kann nur von einem einzigen Entwicklerkonto angezeigt und verwaltet werden.  Sie kann nicht von mehreren Entwicklern gemeinsam genutzt werden.
* Es gibt mehrere Einschränkungen zum zulässigen Format des redirect_uri.  Im folgenden Abschnitt finden Sie weitere Details.

## <a name="restrictions-on-redirect-uris"></a>Einschränkungen für Umleitungs-URIs
Apps, die im neuen Anwendungs-Registrierungsportal registriert sind, sind derzeit auf eine begrenzte Anzahl von Umleitungs-URI-Werten beschränkt.  Der redirect_uri für Web-Apps und Dienste muss mit dem Schema `https` beginnen, und alle redirect-uri-Werte müssen dieselbe DNS-Domäne verwenden.  Beispielsweise ist es nicht möglich, eine Web-App mit folgenden Umleitungs-URIs zu registrieren:

`https://login-east.contoso.com`  
`https://login-west.contoso.com`

Das Registrierungssystem vergleicht den gesamten DNS-Namen des vorhandenen redirect_uri mit dem DNS-Namen des redirect_uri, den Sie hinzufügen. Die Anforderung zum Hinzufügen des DNS-Namens verursacht einen Fehler, wenn eine der folgenden Bedingungen erfüllt ist:  

* Wenn der gesamte DNS-Name des neuen redirect_uri nicht dem DNS-Namen des vorhandenen redirect_uri entspricht.
* Wenn der gesamte DNS-Name des neuen redirect_uri keine Unterdomäne des vorhandenen redirect_uri ist.

Ein Beispiel: Angenommen, die App besitzt aktuell den folgenden Umleitungs-URI:

`https://login.contoso.com`

Dann können Sie Folgendes hinzufügen:

`https://login.contoso.com/new`

(Entspricht exakt dem DNS-Namen.) Oder Folgendes:

`https://new.login.contoso.com`

(Hierbei handelt es sich um eine DNS-Unterdomäne von „login.contoso.com“.)  Wenn Sie eine Anwendung mit den Umleitungs-URIs „login-east.contoso.com“ und login-west.contoso.com“ benötigen, müssen Sie die folgenden Umleitungs-URIs in der angegebenen Reihenfolge hinzufügen:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

Die letzten beiden können hinzugefügt werden, da es sich bei ihnen um Unterdomänen des ersten Umleitungs-URI „contoso.com“ handelt. Diese Einschränkung wird in einer zukünftigen Version aufgehoben.

Um zu erfahren, wie Sie eine Anwendung im neuen Anwendungs-Registrierungsportal registrieren, lesen Sie [diesen Artikel](active-directory-v2-app-registration.md).

## <a name="restrictions-on-services-&-apis"></a>Einschränkungen für Dienste und APIs
Der v2.0-Endpunkt unterstützt derzeit die Anmeldung für jede App, die im neuen Anwendungsregistrierungsportal registriert ist, sofern sie in der Liste der [unterstützten Authentifizierungsabläufe](active-directory-v2-flows.md)enthalten ist.  Diese Apps können OAuth 2.0-Zugriffstoken jedoch nur für einen sehr begrenzten Satz von Ressourcen erhalten.  Der v2.0-Endpunkt gibt Zugriffstoken nur für folgende Apps aus:

* Die App, die das Token angefordert hat.  Eine App kann ein access_token für sich selbst abrufen, wenn die logische App aus mehreren unterschiedlichen Komponenten oder Schichten besteht.  Wenn Sie dieses Szenario in Aktion sehen möchten, besuchen Sie unsere [Erste Schritte](active-directory-appmodel-v2-overview.md#getting-started) -Tutorials.
* Die Outlook-Mail-, -Kalender- und -Kontakte-REST-APIs, die sich alle unter „https://outlook.office.com“ befinden.  Um zu erfahren, wie Sie eine App schreiben können, die auf diese APIs zugreift, gehen Sie zu den [Erste Schritte mit Office](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) -Tutorials.
* Die Microsoft Graph-APIs.  Weitere Informationen zu Microsoft Graph sowie zu den verfügbaren Daten finden Sie unter [https://graph.microsoft.io](https://graph.microsoft.io).

Zu diesem Zeitpunkt werden keine anderen Dienste unterstützt.  In Zukunft werden weitere Microsoft Online-Dienste sowie der Support für Ihre eigenen Web-APIs und Dienste hinzugefügt.

## <a name="restrictions-on-libraries-&-sdks"></a>Einschränkungen für Bibliotheken und SDKs
Bibliotheksunterstützung für den v2.0-Endpunkt ist zu diesem Zeitpunkt relativ begrenzt.  Wenn Sie den v2.0-Endpunkt in einer Produktionsanwendung verwenden möchten, besitzen Sie die folgenden Optionen:

* Wenn Sie eine Webanwendung erstellen, können Sie unsere allgemein verfügbare serverseitige Middleware bedenkenlos für die Anmeldung und die Tokenüberprüfung einsetzen.  Dazu gehören die OWIN Open ID Connect-Middleware für ASP.NET und unser NodeJS Passport-Plug-In.  Codebeispiele, in denen unsere Middleware verwendet wird, stehen in unserem Abschnitt [Erste Schritte](active-directory-appmodel-v2-overview.md#getting-started) ebenfalls zur Verfügung.
* Für andere Plattformen sowie für native und mobile Anwendungen können Sie die Integration mit dem v2.0-Endpunkt auch durchführen, indem Sie Nachrichten direkt in Ihrem Anwendungscode senden und empfangen.  Die v2.0-Protokolle OpenID Connect und OAuth [wurden explizit dokumentiert](active-directory-v2-protocols.md) , um Sie bei einer solchen Integration zu unterstützen.
* Zudem können Sie die Open ID Connect- und OAuth-Open Source-Bibliotheken für die Integration in den v2.0-Endpunkt verwenden.  Das v2.0-Protokoll sollte ohne wesentliche Änderungen mit vielen Open Source-Protokollbibliotheken kompatibel sein.  Die Verfügbarkeit solcher Bibliotheken ist abhängig von der Sprache und der Plattform, und auf den Websites für [Open ID Connect](http://openid.net/connect/) und [OAuth 2.0](http://oauth.net/2/) steht eine Liste mit gängigen Implementierungen zur Verfügung. Weitere Details und die Liste der Open Source-Clientbibliotheken und -Beispiele, die mit dem v2.0-Endpunkt getestet wurden, finden Sie unter [Azure Active Directory (AD) v2.0 und Authentifizierungsbibliotheken](active-directory-v2-libraries.md) .

Wir haben auch eine erste Vorschau der [Microsoft Authentication Library (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) nur für .NET veröffentlicht.  Diese Bibliothek können Sie gerne in .NET-Client- und Serveranwendungen testen. Da es sich um eine Vorschaubibliothek handelt, wird allerdings kein Support von GA-Qualität bereitgestellt.

## <a name="restrictions-on-protocols"></a>Einschränkungen für Protokolle
Der v2.0-Endpunkt unterstützt nur Open ID Connect und OAuth 2.0.  Allerdings wurden nicht alle Features und Funktionen der einzelnen Protokolle in den v2.0-Endpunkt integriert. Dazu gehören:

* Der OpenID Connect-`end_session_endpoint`, der einer Anwendung das Beenden der Benutzersitzung mit dem v2.0-Endpunkt gestattet.
* Vom v2.0-Endpunkt ausgegebene id_tokens enthalten nur einen Paarbezeichner für den Benutzer.  Dies bedeutet, dass zwei verschiedene Anwendungen unterschiedliche IDs für denselben Benutzer erhalten.  Beachten Sie, dass Sie durch Abfragen des Microsoft Graph-`/me`-Endpunkts eine korrelierbare ID für den Benutzer erhalten können, die anwendungsübergreifend verwendet werden kann.
* id_tokens, die vom v2.0-Endpunkt ausgegeben werden, enthalten zurzeit keinen `email`-Anspruch für den Benutzer, selbst wenn Sie vom Benutzer die Berechtigung zum Anzeigen von dessen E-Mail erhalten.
* Der OpenID Connect-Endpunkt mit Benutzerinformationen. Die Endpunkt mit den Benutzerinformationen ist zu diesem Zeitpunkt für den v2.0-Endpunkt nicht implementiert.  Alle Benutzerprofildaten, die Sie möglicherweise an diesem Endpunkt empfangen würden, stehen jedoch über den Microsoft Graph-Endpunkt `/me` zur Verfügung.
* Rollen- und Gruppenansprüche.  Zu diesem Zeitpunkt wird die Ausgabe von Rollen- oder Gruppenansprüchen in id_tokens vom v2.0-Endpunkt nicht unterstützt.

Informationen zum Umfang der vom v2.0-Endpunkt unterstützten Protokollfunktionen finden Sie in unserer [OpenID Connect- und OAuth 2.0-Protokollreferenz](active-directory-v2-protocols.md).

## <a name="restrictions-for-work-&-school-accounts"></a>Einschränkungen für Geschäfts-, Schul- und Unikonten
Es gibt einige spezifische Funktionen für Microsoft-Organisations-/Geschäftskunden, die vom v2.0-Endpunkt noch nicht unterstützt werden.

##### <a name="device-based-conditional-access,-native-and-mobile-apps,-and-the-microsoft-graph"></a>Gerätebasierter bedingter Zugriff, native und mobile Apps und Microsoft Graph
Der v2.0-Endpunkt unterstützt noch keine Geräteauthentifizierung für mobile und native Anwendungen, z.B. native Apps unter iOS oder Android.  Dadurch wird Ihre native Anwendung möglicherweise daran gehindert, Microsoft Graph für bestimmte Organisationen aufzurufen.  Die Geräteauthentifizierung ist erforderlich, wenn ein Administrator eine gerätebasierte bedingte Zugriffsrichtlinie für eine Anwendung festlegt.  Für den v2.0-Endpunkt ist das wahrscheinlichste Szenario für gerätebasierten bedingten Zugriff ein Administrator, der eine Richtlinie für eine Ressource in Microsoft Graph festlegt, z.B. für die Outlook-API.  Wenn ein Administrator diese Richtlinie festlegt und Ihre native Anwendung ein Token für Microsoft Graph anfordert, verursacht die Anforderung letztlich einen Fehler, weil die Geräteauthentifizierung noch nicht unterstützt wird.  Webanwendungen, die Token für Microsoft Graph anfordern, werden jedoch unterstützt, wenn gerätebasierte Richtlinien konfiguriert sind.  Im Web-App-Szenario erfolgt die Geräteauthentifizierung über den Webbrowser des Benutzers.

Als Entwickler haben Sie wahrscheinlich keine Kontrolle darüber, wenn Richtlinien für Microsoft Graph-Ressourcen festgelegt werden, bzw. bemerken es erst gar nicht.  Wenn Sie eine Anwendung für Geschäfts-, Schul- oder Unibenutzer erstellen, verwenden Sie den [ursprünglichen Azure AD-Endpunkt](active-directory-developers-guide.md), bis der v2.0-Endpunkt die Geräteauthentifizierung unterstützt.  Weitere Informationen zum gerätebasierten bedingten Zugriff finden Sie in [diesem Artikel](active-directory-conditional-access.md#device-based-conditional-access).

##### <a name="windows-integrated-authentication-for-federated-tenants"></a>Integrierte Windows-Authentifizierung für Partnermandanten
Wenn Sie ADAL (und somit den ursprünglichen Azure AD-Endpunkt) in Windows-Anwendungen verwendet haben, haben Sie möglicherweise die so genannte SAML-Assertionsgewährung genutzt.  Diese Gewährung erlaubt Benutzern von Azure AD-Partnermandanten, sich im Hintergrund bei ihrer lokalen Active Directory-Instanz zu authentifizieren, ohne ihre Anmeldeinformationen eingeben zu müssen.  Die SAML-Assertionsgewährung wird vom v2.0-Endpunkt zurzeit nicht unterstützt.

<!--HONumber=Oct16_HO2-->


