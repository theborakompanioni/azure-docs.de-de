---
title: Authentifizierung und Autorisierung in Azure App Service | Microsoft Docs
description: Eine grundlegende Übersicht über das Authentifizierungs-/Autorisierungsfeature für Azure App Service.
services: app-service
documentationcenter: ''
author: mattchenderson
manager: erikre
editor: ''

ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 08/29/2016
ms.author: mahender

---
# Authentifizierung und Autorisierung in Azure App Service
## Was ist App Service-Authentifizierung/Autorisierung?
Mithilfe des Authentifizierungs-/Autorisierungsfeatures von App Service kann Ihre Anwendung Benutzer anmelden, sodass Sie keine Codeänderungen für das Back-End der App vornehmen müssen. Es stellt eine einfache Möglichkeit zum Schutz Ihrer Anwendung und für die Arbeit mit benutzerspezifischen Daten bereit.

App Service nutzt die Verbundidentität. Dabei werden Kontenspeicherung und Benutzerauthentifizierung von einem Drittanbieter übernommen. Die Anwendung verwendet die Identitätsinformationen des Anbieters, sodass die App diese Informationen nicht selbst speichern muss. App Service unterstützt standardmäßig fünf Identitätsanbieter: Azure Active Directory, Facebook, Google, das Microsoft-Konto und Twitter. Ihre App kann eine beliebige Anzahl dieser Identitätsanbieter nutzen. Dadurch können Sie Ihren Benutzern verschiedene Anmeldeoptionen zur Verfügung stellen. Die integrierte Unterstützung kann durch Integrieren eines weiteren Identitätsanbieters oder einer [eigenen benutzerdefinierten Identitätslösung][custom-auth] erweitert werden.

Mit folgenden Tutorials können Sie sofort loslegen:

* [Hinzufügen der Authentifizierung zu Ihrer iOS-App][iOS] \(oder [Android], [Windows], [Xamarin.iOS], [Xamarin.Android], [Xamarin.Forms] oder [Cordova])
* [Benutzerauthentifizierung für API-Apps in Azure App Service][apia-user]
* [Erste Schritte mit Azure App Service – Teil 2][web-getstarted]

## Funktionsweise der Authentifizierung in App Service
Zur Authentifizierung mit einem der Identitätsanbieter müssen Sie zunächst den Identitätsanbieter konfigurieren, damit er Ihre Anwendung kennt. Der Identitätsanbieter stellt anschließend IDs und geheime Schlüssel bereit, die Sie wiederum für App Service bereitstellen. Dadurch wird die Vertrauensstellung komplettiert, und App Service kann Benutzerassertionen (beispielsweise Authentifizierungstoken) des Identitätsanbieters überprüfen.

Für die Anmeldung mit einem dieser Anbieter muss der Benutzer an einen Endpunkt umgeleitet werden, der Benutzer für den entsprechenden Anbieter anmeldet. Wenn Kunden einen Webbrowser verwenden, kann App Service auf Wunsch automatisch alle nicht authentifizierten Benutzer an den Endpunkt für die Benutzeranmeldung weiterleiten. Andernfalls müssen Sie Ihre Kunden an `{your App Service base URL}/.auth/login/<provider>` weiterleiten, wobei `<provider>` einem der folgenden Werte entspricht: „aad“, „facebook“, „google“, „microsoft“ oder „twitter“. Szenarien für mobile Geräte und APIs werden weiter unten in diesem Artikel erläutert.

Für Benutzer, die über einen Webbrowser mit Ihrer Anwendung interagieren, wird ein Cookie festgelegt, damit sie während der Verwendung Ihrer Anwendung authentifiziert bleiben. Bei anderen Clienttypen (etwa bei mobilen Clients) wird an den Client ein JSON-Webtoken (JWT) ausgegeben, das im `X-ZUMO-AUTH`-Header angegeben werden muss. Dies wird von den Mobile Apps-Client-SDKs für Sie übernommen. Alternativ kann ein Azure Active Directory-Identitätstoken oder ein Zugriffstoken direkt als [Bearertoken](https://tools.ietf.org/html/rfc6750) in den `Authorization`-Header eingefügt werden.

App Service überprüft jedes Cookie oder Token, das Ihre Anwendung ausgibt, um Benutzer zu authentifizieren. Informationen zum Einschränken des Zugriffs auf Ihre Anwendung finden Sie weiter unten in diesem Artikel im Abschnitt [Autorisierung](#authorization).

### Mobile Authentifizierung mit einem Anbieter-SDK
Nachdem das Back-End vollständig konfiguriert ist, können Sie mobile Clients für die Anmeldung mit App Service anpassen. Dazu gibt es zwei Ansätze:

* Verwenden Sie ein von einem bestimmten Identitätsanbieter veröffentlichtes SDK, um zuerst die Identität und anschließend den Zugriff auf App Service einzurichten.
* Verwenden Sie eine einzelne Codezeile, um Benutzeranmeldungen über das Mobile Apps-Client-SDK zu ermöglichen.

> [!TIP]
> Bei den meisten Anwendungen empfiehlt sich die Verwendung eines Anbieter-SDKs, um eine möglichst konsistente Benutzeranmeldung zu erhalten. Außerdem profitieren Sie bei dieser Variante von Aktualisierungsunterstützung und weiteren Vorteilen des jeweiligen Anbieters.
> 
> 

Bei Verwendung eines Anbieter-SDKs können sich die Benutzer in einer Umgebung anmelden, die stärker in das Betriebssystem eingebunden ist, unter dem die Anwendung ausgeführt wird. Dadurch erhalten Sie außerdem ein Anbietertoken und bestimmte Benutzerinformationen auf dem Client, was die Nutzung von Graph-APIs und die Anpassung der Benutzerumgebung wesentlich vereinfacht. Dies wird in Blogs und Foren gelegentlich als „Clientfluss“ oder „clientgesteuerter Datenfluss“ bezeichnet, da Benutzer durch auf dem Client befindlichen Code angemeldet werden und der Clientcode Zugriff auf ein Anbietertoken erhält.

Das abgerufene Anbietertoken muss zur Überprüfung an App Service gesendet werden. Nach der Tokenüberprüfung durch App Service wird ein neues App Service-Token erstellt und an den Client zurückgegeben. Im Mobile Apps-Client-SDK stehen Hilfsmethoden zur Verfügung, um diesen Austausch zu verwalten und das Token automatisch an alle Anforderungen anzufügen, die an das Anwendungs-Back-End gerichtet werden. Entwickler können bei Bedarf auch einen Verweis auf das Anbietertoken speichern.

### Mobile Authentifizierung ohne Anbieter-SDK
Wenn Sie kein Anbieter-SDK einrichten möchten, können Sie die Anmeldung dem Mobile Apps-Feature von Azure App Service überlassen. Das Mobile Apps-Client-SDK öffnet eine Webansicht für den Anbieter Ihrer Wahl und meldet den Benutzer an. Dies wird in Blogs und Foren gelegentlich als „Serverfluss“ oder „servergesteuerter Datenfluss“ bezeichnet, da der Server den Prozess für die Benutzeranmeldung verwaltet und das Client-SDK das Anbietertoken nie empfängt.

Den Code zum Starten dieses Datenflusses finden Sie im entsprechenden plattformspezifischen Authentifizierungstutorial. Am Ende des Datenflusses verfügt das Client-SDK über ein App Service-Token, und das Token wird automatisch an alle Anforderungen angefügt, die an das Anwendungs-Back-End gerichtet werden.

### Dienst-zu-Dienst-Authentifizierung
Neben der Gewährung des Benutzerzugriffs auf Ihre Anwendung können Sie auch einer anderen vertrauenswürdigen Anwendung das Aufrufen Ihrer eigenen API ermöglichen. So kann beispielsweise eine Web-App eine API in einer anderen Web-App aufrufen. In diesem Szenario können Sie anstelle von Benutzeranmeldeinformationen Anmeldeinformationen für ein Dienstkonto verwenden, um ein Token zu erhalten. Ein Dienstkonto wird im Kontext von Azure Active Directory auch als *Dienstprinzipal* bezeichnet, und die Authentifizierung mithilfe eines solchen Kontos wird auch „Dienst-zu-Dienst-Szenario“ genannt.

> [!IMPORTANT]
> Mobile Apps werden auf Kundengeräten ausgeführt. Sie gelten daher *nicht* als vertrauenswürdige Anwendungen und dürfen keinen Dienstprinzipalfluss verwenden. Stattdessen müssen diese Apps den zuvor erläuterten Benutzerfluss verwenden.
> 
> 

In Dienst-zu-Dienst-Szenarien kann App Service Ihre Anwendung mithilfe von Azure Active Directory schützen. Die aufrufende Anwendung muss lediglich ein Azure Active Directory-Dienstprinzipal-Autorisierungstoken bereitstellen, das unter Angabe von Client-ID und geheimem Clientschlüssel von Azure Active Directory abgerufen wurde. Ein Beispiel für dieses Szenario unter Verwendung von ASP.NET-API-Apps finden Sie im Tutorial [Dienstprinzipalauthentifizierung für API-Apps in Azure App Service][apia-service].

Wenn Sie ein Dienst-zu-Dienst-Szenario mit App Service-Authentifizierung verwenden möchten, können Sie Clientzertifikate oder die Standardauthentifizierung verwenden. Informationen zu Clientzertifikaten in Azure finden Sie unter [Konfigurieren von gegenseitiger TLS-Authentifizierung für Web-Apps](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Informationen zur Standardauthentifizierung in ASP.NET finden Sie unter [Authentication Filters in ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/authentication-filters) (Authentifizierungsfilter in ASP.NET-Web-API 2).

Die Dienstkontoauthentifizierung zwischen einer App Service-Logik-App und einer API-App ist ein Sonderfall, der in [Verwenden der in App Service gehosteten benutzerdefinierten API mit Logik-Apps](../app-service-logic/app-service-logic-custom-hosted-api.md) erklärt wird.

## <a name="authorization"></a>Funktionsweise der Autorisierung in App Service
Sie haben uneingeschränkte Kontrolle über die Anforderungen, denen der Zugriff auf Ihre Anwendung gestattet werden soll. Die App Service-Authentifizierung/-Autorisierung kann mit folgenden Verhaltensweisen konfiguriert werden:

* Nur zulassen, dass authentifizierte Anforderungen Ihre Anwendung erreichen.
  
    Wenn ein Browser eine anonyme Anforderung empfängt, leitet App Service sie auf eine Seite für den von Ihnen gewählten Identitätsanbieter um, damit sich die Benutzer anmelden können. Stammt die Anforderung von einem mobilen Gerät, wird eine HTTP-Antwort vom Typ *401 - Nicht autorisiert* zurückgegeben.
  
    Mit dieser Option müssen Sie in Ihrer App keinerlei Authentifizierungscode schreiben. Falls Sie eine präzisere Autorisierung benötigen, kann Ihr Code auf Informationen zum Benutzer zurückgreifen.
* Zulassen, dass alle Anforderungen Ihre Anwendung erreichen, aber authentifizierte Anforderungen überprüfen, und Authentifizierungsdaten an die HTTP-Header übergeben.
  
    Diese Option überlässt Autorisierungsentscheidungen Ihrem Anwendungscode. Sie bietet mehr Flexibilität bei der Verarbeitung anonymer Anforderungen, benötigt aber auch entsprechenden Code.
* Zulassen, dass alle Anforderungen Ihre Anwendung erreichen, und keine Aktionen für die Authentifizierungsdaten in den Anforderungen ausführen.
  
    In diesem Fall ist das Authentifizierungs-/Autorisierungsfeature deaktiviert. Authentifizierungs- und Autorisierungsaufgaben bleiben vollständig Ihrem Anwendungscode überlassen.

Die oben genannten Verhaltensweisen werden mithilfe der Option **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** im Azure-Portal gesteuert. Bei Verwendung der Option **Anmelden mit *Anbietername*** müssen alle Anforderungen authentifiziert werden. Bei Verwendung von **Anforderung zulassen (keine Aktion)** bleibt die Autorisierungsentscheidung Ihrem Code überlassen, es werden aber trotzdem Authentifizierungsinformationen angegeben. Falls der Code sämtliche Aufgaben übernehmen soll, können Sie das Authentifizierungs-/Autorisierungsfeature deaktivieren.

## Verwenden von Benutzeridentitäten in Ihrer Anwendung
App Service übergibt einige Benutzerinformationen mithilfe spezieller Header an Ihre Anwendung. Diese Header sind in externen Anforderungen nicht zulässig und nur vorhanden, wenn sie von der App Service-Authentifizierung/-Autorisierung festgelegt werden. Beispiele für solche Header wären etwa:

* X-MS-CLIENT-PRINCIPAL-NAME
* X-MS-CLIENT-PRINCIPAL-ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON

Anhand dieser Header kann jeder Code unabhängig von Programmiersprache oder Framework die benötigten Informationen ermitteln. Bei ASP.NET 4.6-Apps wird **ClaimsPrincipal** automatisch mit entsprechenden Werten festgelegt.

Über eine HTTP GET-Anforderung kann Ihre Anwendung zusätzliche Benutzerdetails vom `/.auth/me`-Endpunkt Ihrer Anwendung abrufen. Wenn die Anforderung ein gültiges Token enthält, wird eine JSON-Nutzlast mit Details zum verwendeten Anbieter, dem zugrunde liegenden Anbietertoken und einigen anderen Benutzerinformationen zurückgegeben. Die Mobile Apps-Server-SDKs enthalten Hilfsmethoden für die Verwendung dieser Daten. Weitere Informationen finden Sie unter [Gewusst wie: Verwenden des Azure Mobile Apps SDK für Node.js](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity) sowie unter [Arbeiten mit dem .NET-Back-End-Server-SDK für Azure Mobile Apps](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## Dokumentation und zusätzliche Ressourcen
### Identitätsanbieter
In den folgenden Tutorials erfahren Sie, wie Sie App Service für die Verwendung verschiedener Authentifizierungsanbieter konfigurieren:

* [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Azure Active Directory-Anmeldung][AAD]
* [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Facebook-Anmeldung][Facebook]
* [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Google-Anmeldung][Google]
* [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Microsoft-Kontoanmeldung][MSA]
* [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Twitter-Anmeldung][Twitter]

Wenn Sie keines der hier beschriebenen Identitätssysteme verwenden möchten, können Sie die [Vorschau der Unterstützung der benutzerdefinierten Authentifizierung im Mobile Apps-.NET-Server-SDK][custom-auth] \(geeignet für Web-Apps, mobile Apps und API-Apps) nutzen.

### Webanwendungen
In den folgenden Tutorials erfahren Sie, wie Sie die Authentifizierung einer Webanwendung hinzufügen:

* [Erste Schritte mit Azure App Service – Teil 2][web-getstarted]

### Mobile Anwendungen
In den folgenden Tutorials erfahren Sie, wie Sie Ihren mobilen Clients die Authentifizierung mit servergesteuertem Datenfluss hinzufügen:

* [Hinzufügen der Authentifizierung zu Ihrer iOS-App][iOS]
* [Hinzufügen der Authentifizierung zu Ihrer Android-App][Android]
* [Hinzufügen der Authentifizierung zu Ihrer Windows-App][Windows]
* [Hinzufügen der Authentifizierung zu Ihrer Xamarin.iOS-App][Xamarin.iOS]
* [Hinzufügen der Authentifizierung zu Ihrer Xamarin.Android-App][Xamarin.Android]
* [Hinzufügen der Authentifizierung zu Ihrer Xamarin.Forms-App][Xamarin.Forms]
* [Hinzufügen der Authentifizierung zu Ihrer Cordova-App][Cordova]

Informationen zur Verwendung des clientgesteuerten Flusses für Azure Active Directory finden Sie hier:

* [Verwenden der Active Directory-Authentifizierungsbibliothek für iOS][ADAL-iOS]
* [Verwenden der Active Directory-Authentifizierungsbibliothek für Android][ADAL-Android]
* [Verwenden der Active Directory-Authentifizierungsbibliothek für Windows und Xamarin][ADAL-dotnet]

Informationen zur Verwendung des clientgesteuerten Flusses für Facebook finden Sie in den folgenden Ressourcen:

* [Verwenden des Facebook-SDK für iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#facebook-sdk)

Informationen zur Verwendung des clientgesteuerten Flusses für Twitter finden Sie in den folgenden Ressourcen:

* [Verwenden von Twitter Fabric für iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#twitter-fabric)

Informationen zur Verwendung des clientgesteuerten Flusses für Google finden Sie in den folgenden Ressourcen:

* [Verwenden des Google-Anmelde-SDK für iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#google-sdk)

### API-Anwendungen
In den folgenden Tutorials erfahren Sie, wie Sie Ihre API-Apps schützen können:

* [Benutzerauthentifizierung für API-Apps in Azure App Service][apia-user]
* [Dienstprinzipalauthentifizierung für API-Apps in Azure App Service][apia-service]

[apia-user]: ../app-service-api/app-service-api-dotnet-user-principal-auth.md
[apia-service]: ../app-service-api/app-service-api-dotnet-service-principal-auth.md

[web-getstarted]: ../app-service-web/app-service-web-get-started-2.md#authenticate-your-users

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[Xamarin.iOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[Xamarin.Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[Xamarin.Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users.md
[Windows]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: ../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: ../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: ../app-service-mobile/app-service-mobile-how-to-configure-google-authentication.md
[MSA]: ../app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: ../app-service-mobile/app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal

<!---HONumber=AcomDC_0831_2016-->