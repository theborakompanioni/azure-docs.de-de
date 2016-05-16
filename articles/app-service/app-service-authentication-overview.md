<properties
	pageTitle="Authentifizierung und Autorisierung in Azure App Service | Microsoft Azure"
	description="Eine grundlegende Übersicht über das Authentifizierungs-/Autorisierungsfeature für Azure App Service."
	services="app-service"
	documentationCenter=""
	authors="mattchenderson"
	manager="erikre"
	editor=""/>

<tags
	ms.service="app-service"
	ms.workload="mobile"
	ms.tgt_pltfrm="na"
	ms.devlang="multiple"
	ms.topic="article"
	ms.date="04/25/2016"
	ms.author="mahender"/>
    
# Authentifizierung und Autorisierung in Azure App Service

## Was ist App Service-Authentifizierung/Autorisierung?

App Service-Authentifizierung/Autorisierung ist ein Feature, das einer Anwendung die Anmeldung von Benutzern ermöglicht, ohne das Codeänderungen am Back-End der App erforderlich werden. Es stellt eine einfache Möglichkeit zum Schutz Ihrer Anwendung und für die Arbeit mit benutzerspezifischen Daten bereit.

App Service nutzt die Verbundidentität. Dabei werden Kontenspeicherung und Benutzerauthentifizierung von einem Drittanbieter übernommen. Die Anwendung verwendet die Identitätsinformationen des Anbieters und speichert die Informationen nicht selbst. App Service unterstützt standardmäßig fünf Identitätsanbieter: _Azure Active Directory_, _Facebook_, _Google_, das _Microsoft-Konto_ und _Twitter_. Ihre App kann eine beliebige Anzahl dieser Identitätsanbieter nutzen, sodass Sie Ihren Benutzern verschiedene Optionen für die Anmeldung bieten können. Sie können die integrierte Unterstützung auch erweitern, indem Sie einen anderen Identitätsanbieter oder eine [eigene benutzerdefinierte Identitätslösung][custom-auth] integrieren.

Mit folgenden Tutorials können Sie sofort loslegen:

- [Hinzufügen der Authentifizierung zu Ihrer iOS-App][iOS] (oder [Android], [Windows], [Xamarin.iOS], [Xamarin.Android], [Xamarin.Forms] oder [Cordova])
- [Benutzerauthentifizierung für API-Apps in Azure App Service][apia-user]

## Funktionsweise der Authentifizierung in App Service

Zur Authentifizierung mit einem der Identitätsanbieter müssen Sie zunächst den Identitätsanbieter konfigurieren, damit er Ihre Anwendung erkennt. Der Identitätsanbieter stellt anschließend IDs und geheime Schlüssel bereit, die Sie wiederum für App Service bereitstellen. Dadurch wird die Vertrauensstellung komplettiert, und App Service kann Benutzerassertionen des Identitätsanbieters überprüfen (beispielsweise Authentifizierungstoken).

Für die Anmeldung mit einem dieser Anbieter muss der Benutzer an einen Anmeldungsendpunkt für den entsprechenden Anbieter umgeleitet werden. Wenn Kunden einen Webbrowser verwenden, kann App Service auf Wunsch automatisch alle nicht authentifizierten Benutzer an den Anmeldungsendpunkt weiterleiten. Andernfalls müssen Sie Ihre Kunden an `{your App Service base URL}/.auth/login/<provider>` weiterleiten, wobei `<provider>` einer der folgenden Optionen entspricht: _aad_, _facebook_, _google_, _microsoft_ oder _twitter_. Mobile Szenarien und API-Szenarien werden in den folgenden Abschnitten behandelt.

Für Benutzer, die über einen Webbrowser mit Ihrer Anwendung interagieren, wird ein Cookie festgelegt, damit sie während der Navigation in Ihrer Anwendung authentifiziert bleiben. Bei anderen Clienttypen (etwa bei mobilen Clients) wird ein JSON-Webtoken (JWT) an den Client ausgegeben, das im `X-ZUMO-AUTH`-Header angegeben werden muss. Dies wird von den Mobile Apps-Client-SDKs für Sie übernommen. Alternativ kann ein Azure Active Directory-Identitätstoken oder ein Zugriffstoken direkt als [Bearertoken](https://tools.ietf.org/html/rfc6750) in den `Authorization`-Header eingefügt werden.

App Service überprüft jedes von Ihrer Anwendung ausgegebene Cookie oder Token und ermöglicht so die Authentifizierung der Benutzer. Informationen zur Einschränkung des Zugriffs auf Ihre Anwendung finden Sie weiter unten im Abschnitt [Autorisierung](#authorization).

### Mobile Authentifizierung mit einem Anbieter-SDK

Nachdem das Back-End vollständig konfiguriert ist, können Sie mobile Clients für die Anmeldung mit App Service anpassen. Dazu gibt es zwei Ansätze:

- Nutzen Sie ein von einem bestimmten Identitätsanbieter veröffentlichtes SDK zum Einrichten der Identität und für den Zugriff auf App Service.
- Lassen Sie das Mobile Apps-Client-SDK die Anmeldung von Benutzern unter Verwendung einer einzelnen Codezeile durchführen.

>[AZURE.TIP] Die meisten Anwendungen sollten ein Anbieter-SDK verwenden, um eine homogenere Anmeldeumgebung zu bieten und von Aktualisierungsunterstützung sowie anderen anbieterspezifischen Vorteile zu profitieren.

Die Verwendung eines Anbieter-SDKs ermöglicht der Anmeldeumgebung, auf der die App ausgeführt wird, enger mit der Betriebssystemplattform zu interagieren. Dadurch erhalten Sie außerdem ein Anbietertoken und bestimmte Benutzerinformationen auf dem Client, was die Nutzung von Graph-APIs und die Anpassung der Benutzerumgebung wesentlich vereinfacht. Gelegentlich wird dies in Blogs und Foren als „Clientfluss“ oder „clientgesteuerter Datenfluss“ bezeichnet, da Code auf dem Client die Anmeldung verarbeitet und der Clientcode Zugriff auf ein Anbieter-Token erhält.

Sobald ein Anbietertoken abgerufen wurde, muss es zur Überprüfung an App Service gesendet werden. Nach der Tokenüberprüfung durch App Service wird ein neues App Service-Token erstellt und an den Client zurückgegeben. Im Mobile Apps-Client-SDK stehen Hilfsmethoden zur Verfügung, um diesen Austausch zu verwalten und das Token automatisch an alle Anforderungen anzufügen, die an das Anwendungs-Back-End gerichtet werden. Der Entwickler kann bei Bedarf auch einen Verweis auf das Anbietertoken speichern.

### Mobile Authentifizierung ohne Anbieter-SDK

Wenn Sie kein Anbieter-SDK einrichten möchten, können Sie die Anmeldung von Mobile App Service-Apps abwickeln lassen. Das Mobile Apps-Client-SDK öffnet eine Webansicht für den Anbieter Ihrer Wahl und schließt die Anmeldung ab. Dies wird in Blogs und Foren gelegentlich als „Serverfluss“ oder „servergesteuerter Datenfluss“ bezeichnet, da der Server die Anmeldung verwaltet und das Client-SDK das Anbieter-Token nie empfängt.

Der zum Starten dieses Datenflusses benötigte Code wird im Tutorial zur Authentifizierung für die einzelnen Plattformen behandelt. Am Ende des Datenflusses verfügt das Client-SDK über ein App Service-Token, und das Token wird automatisch an alle Anforderungen angefügt, die an das Anwendungs-Back-End gerichtet werden.

### Dienst-zu-Dienst-Authentifizierung

Neben der Gewährung des Benutzerzugriffs auf Ihre Anwendung können Sie auch einer anderen vertrauenswürdigen Anwendung das Aufrufen Ihrer eigenen API ermöglichen. Dadurch kann beispielsweise eine App Service-Anwendung eine API in einer anderen App Service-Anwendung aufrufen. In diesem Szenario können Sie ein Token abrufen, indem Sie Anmeldeinformationen für ein Dienstkonto anstelle der Endbenutzer-Anmeldeinformationen verwenden. Ein Dienstkonto wird im Kontext von Azure Active Directory auch als *Dienstprinzipal* bezeichnet, und die Authentifizierung mithilfe eines solchen Kontos wird auch „Dienst-zu-Dienst-Szenario“ genannt.

>[AZURE.IMPORTANT] Mobile Anwendungen werden auf Kundengeräten ausgeführt. Sie gelten daher _nicht_ als vertrauenswürdige Anwendungen und dürfen keinen Dienstprinzipalablauf verwenden. Stattdessen muss in diesem Fall einer der weiter oben beschriebenen Benutzerabläufe verwendet werden.

In Dienst-zu-Dienst-Szenarien kann App Service Ihre Anwendung mithilfe von Azure Active Directory schützen. Die aufrufende Anwendung muss lediglich ein AAD-Dienstprinzipal-Autorisierungstoken bereitstellen, das unter Angabe von Client-ID und geheimem Clientschlüssel von AAD abgerufen wurde. Ein Beispiel für dieses Szenario mithilfe von ASP.NET-API-Apps finden Sie in dem Tutorial [Dienstprinzipalauthentifizierung für API-Apps in Azure App Service][apia-service].

Wenn Sie ein Dienst-zu-Dienst-Szenario ohne App Service-Authentifizierung behandeln möchten, können Sie Clientzertifikate oder Standardauthentifizierung verwenden. Informationen zu Clientzertifikaten in Azure finden Sie unter [Konfigurieren von gegenseitiger TLS-Authentifizierung für Web-Apps](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Informationen zur grundlegenden Authentifizierung in ASP.NET finden Sie unter [Authentication Filters in ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/authentication-filters) (Authentifizierungsfilter in ASP.NET-Web-API 2).

Die Dienstkontoauthentifizierung zwischen einer App Service-Logik-App und einer API-App ist ein Sonderfall, der in [Verwenden der in App Service gehosteten benutzerdefinierten API mit Logik-Apps](../app-service-logic/app-service-logic-custom-hosted-api.md) erklärt wird.

## <a name="authorization"></a>Funktionsweise der Authentifizierung in App Service

Sie haben uneingeschränkte Kontrolle darüber, welchen Anforderungen der Zugriff auf Ihre Anwendung gestattet werden soll. Die App Service-Authentifizierung/-Autorisierung kann mit folgenden Verhaltensweisen konfiguriert werden:

- Nur zulassen, dass authentifizierte Anforderungen Ihre Anwendung erreichen.

	Wenn von einem Browser eine anonyme Anforderung empfangen wird, leitet App Service sie auf eine Anmeldeseite für den von Ihnen gewählten Identitätsanbieter um. Stammt die Anforderung von einem mobilen Gerät, wird eine HTTP-Antwort vom Typ _401 - Nicht autorisiert_ zurückgegeben.

	Mit dieser Option müssen Sie in Ihrer App keinerlei Authentifizierungscode schreiben. Falls Sie eine präzisere Autorisierung benötigen, kann Ihr Code auf Informationen zum Benutzer zurückgreifen.

- Zulassen, dass alle Anforderungen Ihre Anwendung erreichen, aber authentifizierte Anforderungen überprüfen, und Authentifizierungsdaten an die HTTP-Header übergeben.

	Diese Option überlässt Autorisierungsentscheidungen Ihrem Anwendungscode. Sie bietet mehr Flexibilität bei der Verarbeitung anonymer Anforderungen, benötigt aber auch entsprechenden Code.
	
- Zulassen, dass alle Anforderungen Ihre Anwendung erreichen, und keine Aktionen für die Authentifizierungsdaten in den Anforderungen ausführen.

	In diesem Fall ist das Authentifizierungs-/Autorisierungsfeature deaktiviert. Authentifizierungs- und Autorisierungsaufgaben bleiben vollständig Ihrem Anwendungscode überlassen.

Die oben genannten Verhaltensweisen werden mithilfe der Option **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** im Portal gesteuert. Wenn für einen der Anbieter die Option „Anmelden mit...“ ausgewählt wird, müssen alle Anforderungen authentifiziert werden. Bei Verwendung von „Anforderung zulassen (keine Aktion)“ bleibt die Autorisierungsentscheidung Ihrem Code überlassen, es werden aber trotzdem Authentifizierungsinformationen angegeben. Falls der Code sämtliche Aufgaben übernehmen soll, können Sie das Authentifizierungs-/Autorisierungsfeature deaktivieren.

## Verwenden von Benutzeridentitäten in Ihrer Anwendung

App Service übergibt einige Benutzerinformationen mithilfe spezieller Header an Ihre Anwendung. Diese Header sind in externen Anforderungen nicht zulässig und nur vorhanden, wenn sie von der App Service-Authentifizierung/-Autorisierung festgelegt werden. Beispiele für solche Header wären etwa:

* X-MS-CLIENT-PRINCIPAL-NAME
* X-MS-CLIENT-PRINCIPAL-ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON

Anhand dieser Header kann jeder Code unabhängig von Programmiersprache oder Framework die benötigten Informationen ermitteln. Bei ASP.NET 4.6-Apps wird der ClaimsPrincipal automatisch mit entsprechenden Werten festgelegt.

Über eine HTTP GET-Anforderung kann Ihre Anwendung zusätzliche Benutzerdetails vom `/.auth/me`-Endpunkt Ihrer Anwendung abrufen. Wenn die Anforderung ein gültiges Token enthält, wird eine JSON-Nutzlast mit Details zum verwendeten Anbieter, dem zugrunde liegenden Anbietertoken und einigen anderen Benutzerinformationen zurückgegeben. Die Mobile Apps-Server-SDKs enthalten Hilfsmethoden für die Verwendung dieser Daten ([Node.JS](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md/#howto-tables-getidentity), [.NET](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md/#user-info)).

## Dokumentation und zusätzliche Ressourcen

### Identitätsanbieter
Die folgenden Tutorials zeigen, wie Sie App Service konfigurieren, um verschiedene Authentifizierungsanbieter nutzen zu können:

- [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Azure Active Directory-Anmeldung][AAD]
- [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Facebook-Anmeldung][Facebook]
- [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Google-Anmeldung][Google]
- [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Microsoft-Kontoanmeldung][MSA]
- [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Twitter-Anmeldung][Twitter]

Wenn Sie keines der hier beschriebenen Identitätssysteme verwenden möchten, können Sie die [Vorschau der Unterstützung der benutzerdefinierten Authentifizierung im Mobile Apps-.NET-Server-SDK][custom-auth] (geeignet für Web-Apps, mobile Apps und API-Apps) nutzen.

### Mobile Anwendungen
In den folgenden Tutorials erfahren Sie, wie Sie Ihren mobilen Clients die Authentifizierung mit servergesteuertem Datenfluss hinzufügen:

- [Hinzufügen der Authentifizierung zu Ihrer iOS-App][iOS]
- [Hinzufügen der Authentifizierung zu Ihrer Android-App][Android]
- [Hinzufügen der Authentifizierung zu Ihrer Windows-App][Windows]
- [Hinzufügen der Authentifizierung zu Ihrer Xamarin.iOS-App][Xamarin.iOS]
- [Hinzufügen der Authentifizierung zu Ihrer Xamarin.Android-App][Xamarin.Android]
- [Hinzufügen der Authentifizierung zu Ihrer Xamarin.Forms-App][Xamarin.Forms]
- [Hinzufügen der Authentifizierung zu Ihrer Cordova-App][Cordova]

Informationen zur Verwendung des clientgesteuerten Flusses für AAD finden Sie hier:

- [Verwenden der Active Directory-Authentifizierungsbibliothek für iOS][ADAL-iOS]
- [Verwenden der Active Directory-Authentifizierungsbibliothek für Android][ADAL-Android]
- [Verwenden der Active Directory-Authentifizierungsbibliothek für Windows und Xamarin][ADAL-dotnet]

### API-Anwendungen
In den folgenden Tutorials erfahren Sie, wie Sie Ihre API-Apps schützen können:

- [Benutzerauthentifizierung für API-Apps in Azure App Service][apia-user]
- [Dienstprinzipalauthentifizierung für API-Apps in Azure App Service][apia-service]









[apia-user]: ../app-service-api/app-service-api-dotnet-user-principal-auth.md
[apia-service]: ../app-service-api/app-service-api-dotnet-service-principal-auth.md

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[Xamarin.iOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[Xamarin.Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[Xamarin.Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users
[Windows]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: ../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: ../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: ../app-service-mobile/app-service-mobile-how-to-configure-google-authentication.md
[MSA]: ../app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: ../app-service-mobile/app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md/#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md/#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md/#adal

<!---HONumber=AcomDC_0504_2016-->