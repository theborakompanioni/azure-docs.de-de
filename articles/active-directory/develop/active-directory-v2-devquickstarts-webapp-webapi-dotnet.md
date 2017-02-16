---
title: Verwenden von Azure AD v2.0 zum Sichern einer .NET MVC-Web-App, die eine Web-API aufruft | Microsoft-Dokumentation
description: "Vorgehensweise beim Erstellen einer .NET-MVC-Web-App, die Webdienste mit persönlichen Microsoft-Konten oder Geschäfts- oder Schulkonten für die Anmeldung aufruft."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 56be906e-71de-469d-9a5c-9fc08aae4223
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 10/27/2016
ms.author: dastrock
translationtype: Human Translation
ms.sourcegitcommit: 47dce83cb4e3e5df92e91f1ca9195326634d6c8b
ms.openlocfilehash: 8c35bfbc75f99fc8558f83667b12a6df1cd3eefd


---
# <a name="calling-a-web-api-from-a-net-web-app"></a>Aufrufen einer Web-API über eine .NET-Web-App
Mit dem v2.0-Endpunkt können Sie schnell eine Authentifizierung zu Ihren Web-Apps und Web-APIs hinzufügen, die sowohl persönliche Microsoft-Konten als auch Geschäfts-, Schul- oder Unikonten unterstützen.  Hier erstellen wir eine MVC-Web-App, über die Benutzer unter Verwendung von OpenID Connect und unterstützt durch die OWIN-Middleware von Microsoft angemeldet werden.  Die Web-App ruft OAuth 2.0-Zugriffstoken für eine durch OAuth 2.0 gesicherte Web-API ab, die das Erstellen, Lesen und Löschen von Daten in der Aufgabenliste eines bestimmten Benutzers ermöglicht.

Dieses Tutorial konzentriert sich hauptsächlich auf das Abrufen und Verwenden von Zugriffstoken mithilfe von MSAL in einer Web-App. Eine vollständige Beschreibung finden Sie [hier](active-directory-v2-flows.md#web-apps).  Vorab möchten Sie vielleicht erfahren, wie eine [grundlegende Anmeldefunktion zu einer Web-App hinzugefügt wird](active-directory-v2-devquickstarts-dotnet-web.md) oder wie [eine Web-API ausreichend geschützt wird](active-directory-v2-devquickstarts-dotnet-api.md).

> [!NOTE]
> Nicht alle Szenarien und Funktionen von Azure Active Directory werden vom v2.0-Endpunkt unterstützt.  Lesen Sie die Informationen zu den [Einschränkungen des v2.0-Endpunkts](active-directory-v2-limitations.md), um herauszufinden, ob Sie den v2.0-Endpunkt verwenden sollten.
> 
> 

## <a name="download-sample-code"></a>Beispielcode herunterladen
Der Code für dieses Tutorial wird [auf GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet)verwaltet.  Um folgen zu können, können Sie [das App-Gerüst als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) oder das Gerüst klonen:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

Alternativ können Sie [die fertige App als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) oder die fertige App klonen:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

## <a name="register-an-app"></a>Registrieren einer App
Erstellen Sie eine neue App unter [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), oder führen Sie die folgenden [ausführlichen Schritte](active-directory-v2-app-registration.md) aus.  Stellen Sie sicher, dass Sie:

* die Ihrer App zugewiesene **Anwendungs-ID** kopieren. Sie benötigen Sie in Kürze.
* einen **geheimen App-Schlüssel** des Typs **Kennwort** erstellen, und den Wert für später kopieren.
* die **Web** -Plattform für Ihre App hinzufügen.
* den richtigen **Umleitungs-URI**eingeben. Der Umleitungs-URI verweist auf Azure AD, wohin Authentifizierungsantworten gesendet werden sollen – der Standardwert in diesem Lernprogramm lautet `https://localhost:44326/`.

## <a name="install-owin"></a>Installieren von OWIN
Fügen Sie dem `TodoList-WebApp` -Projekt über die Paket-Manager-Konsole die NuGet-Pakete der OWIN-Middleware hinzu.  Die OWIN-Middleware wird unter anderem für die Ausgabe von Anmelde- und Abmeldeanforderungen, für die Verwaltung der Benutzersitzungen und für das Abrufen der Benutzerinformationen verwendet.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

## <a name="sign-the-user-in"></a>Anmelden des Benutzers
Konfigurieren Sie nun die OWIN-Middleware für die Verwendung des [Authentifizierungsprotokolls OpenID Connect](active-directory-v2-protocols.md).  

* Öffnen Sie die Datei `web.config` aus dem Stammverzeichnis des `TodoList-WebApp`-Projekts, und geben Sie die Konfigurationswerte Ihrer App im Abschnitt `<appSettings>` ein.
  * `ida:ClientId` ist die **Anwendungs-ID** , die Ihrer App im Registrierungsportal zugewiesen ist.
  * `ida:ClientSecret` ist der **geheime App-Schlüssel** , den Sie im Registrierungsportal erstellt haben.
  * `ida:RedirectUri` ist der **Umleitungs-URI** , den Sie im Portal eingegeben haben.
* Öffnen Sie die Datei `web.config` im Stammverzeichnis des `TodoList-Service`-Projekts, und ersetzen Sie `ida:Audience` durch die gleiche **Anwendungs-ID** wie oben.
* Öffnen Sie die Datei `App_Start\Startup.Auth.cs`, und fügen Sie `using`-Anweisungen für die oben genannten Bibliotheken hinzu.
* Implementieren Sie in der gleichen Datei die `ConfigureAuth(...)` -Methode.  Die Parameter, die Sie in `OpenIDConnectAuthenticationOptions` bereitstellen, dienen als Koordinaten für Ihre Anwendung zur Kommunikation mit Azure AD.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {

                    // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0
                    // The `Scope` describes the permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                    // In a real application you could use issuer validation for additional checks, like making sure the user's organization has signed up for your app, for instance.

                    ClientId = clientId,
                    Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0 "),
                    Scope = "openid email profile offline_access",
                    RedirectUri = redirectUri,
                    PostLogoutRedirectUri = redirectUri,
                    TokenValidationParameters = new TokenValidationParameters
                    {
                        ValidateIssuer = false,
                    },

                    // The `AuthorizationCodeReceived` notification is used to capture and redeem the authorization_code that the v2.0 endpoint returns to your app.

                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed,
                        AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    }

        });
}
// ...
```

## <a name="use-msal-to-get-access-tokens"></a>Abrufen von Zugriffstoken mithilfe von MSAL
In der `AuthorizationCodeReceived`-Benachrichtigung möchten wir [OAuth 2.0 zusammen mit OpenID Connect](active-directory-v2-protocols.md) zum Einlösen des Autorisierungscodes für ein Zugriffstoken auf den To-Do List-Dienst verwenden.  MSAL kann diesen Prozess erleichtern:

* Installieren Sie zunächst die Vorschauversion von MSAL:

```PM> Install-Package Microsoft.Identity.Client -ProjectName TodoList-WebApp -IncludePrerelease```

* Fügen Sie eine weitere `using`-Anweisung zur Datei `App_Start\Startup.Auth.cs` für MSAL hinzu.
* Fügen Sie nun eine neue Methode, den `OnAuthorizationCodeReceived`-Ereignishandler, hinzu.  Dieser Handler nutzt MSAL zum Abrufen eines Zugriffstokens auf die To-Do List-API und speichert das Token im Tokencache von MSAL für spätere Zwecke:

```C#
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
        string userObjectId = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        string tenantID = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenantID, string.Empty);
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        // Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
        app = new ConfidentialClientApplication(Startup.clientId, redirectUri, cred, new NaiveSessionCache(userObjectId, notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase)) {};
        var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { clientId }, notification.Code);
}
// ...
```

* In Web-Apps verfügt MSAL über ein erweiterbares Tokencache, das zum Speichern von Token verwendet werden kann.  In diesem Beispiel wird der `NaiveSessionCache` implementiert, der die HTTP-Sitzungsspeicher zum Zwischenspeichern von Token verwendet.

<!-- TODO: Token Cache article -->


## <a name="call-the-web-api"></a>Aufrufen der Web-API
Jetzt wird das in Schritt 3 abgerufene Zugriffstoken benötigt.  Öffnen Sie die Datei `Controllers\TodoListController.cs` der Web-App, die alle CRUD-Anfragen an die To-Do List-API durchführt.

* Sie können hier wieder MSAL verwenden, um Zugriffstoken aus dem MSAL-Cache abzurufen.  Fügen Sie dieser Datei zunächst eine `using`-Anweisung für MSAL hinzu.
  
    `using Microsoft.Identity.Client;`
* Verwenden Sie in der `Index`-Aktivität die `AcquireTokenSilentAsync`-Methode von MSAL, um ein Zugriffstoken abzurufen, das zum Lesen von Daten aus dem To-Do List-Dienst verwendet werden kann:

```C#
// ...
string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
string tenantID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, tenantID, string.Empty);
ClientCredential credential = new ClientCredential(Startup.clientId, Startup.clientSecret);

// Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
app = new ConfidentialClientApplication(Startup.clientId, redirectUri, credential, new NaiveSessionCache(userObjectID, this.HttpContext)){};
result = await app.AcquireTokenSilentAsync(new string[] { Startup.clientId });
// ...
```

* Im Beispiel wird dann das resultierende Token auf die HTTP GET-Anforderung als `Authorization`-Header hinzugefügt, das der To-Do List-Dienst zum Authentifizieren der Anforderung nimmt.
* Wenn der To-Do List-Dienst eine `401 Unauthorized`-Antwort zurückgibt, sind die Zugriffstoken in MSAL aus irgendeinem Grund ungültig geworden.  In diesem Fall sollten Sie alle Zugriffstoken aus dem MSAL-Cache löschen und dem Benutzer eine Meldung anzeigen, dass er sich möglicherweise erneut anmelden muss, wodurch der Fluss für die Tokenerfassung neu gestartet wird.

```C#
// ...
// If the call failed with access denied, then drop the current access token from the cache,
// and show the user an error indicating they might need to sign-in again.
if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
        app.AppTokenCache.Clear(Startup.clientId);

        return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
}
// ...
```

* Wenn MSAL aus irgendeinem Grund keine Zugriffstoken zurückgeben kann, sollten Sie den Benutzer anweisen, sich erneut anzumelden.  Dies ist so einfach wie das Abfangen jeder `MSALException`:

```C#
// ...
catch (MsalException ee)
{
        // If MSAL could not get a token silently, show the user an error indicating they might need to sign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ee.Message + " You might need to log out and log back in.");
}
// ...
```

* Der genaue identische `AcquireTokenSilentAsync`-Aufruf ist in den Aktionen `Create` und `Delete` implementiert.  In Web-Apps können Sie diese MSAL-Methode nutzen, um Zugriffstoken immer dann abrufen zu können, wenn Sie sie in der App benötigen.  MSAL übernimmt für Sie das Erfassen, Zwischenspeichern und Aktualisieren von Token.

Erstellen und führen Sie Ihre Anwendung zum Schluss aus!  Melden Sie sich entweder mit einem Microsoft-Konto oder einem Azure AD-Konto an, und beachten Sie, wie die Identität des Benutzers in der oberen Navigationsleiste dargestellt wird.  Fügen Sie einige Elemente zur Aufgabenliste (To Do-List) des Benutzers hinzu und löschen Sie sie, um durch die OAuth 2.0-API gesicherte Aufrufe in Aktion zu sehen.  Sie verfügen jetzt über eine mit branchenüblichen Protokollen gesicherte Web-App und Web-API, die Benutzer beide mit ihren persönlichen Konten oder ihren Geschäfts- oder Schulkonten authentifizieren können.

Als Referenz stellen wir [hier](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip)das vollständige Beispiel (ohne Ihre Konfigurationswerte) bereit.  

## <a name="next-steps"></a>Nächste Schritte
Weitere Ressourcen:

* [Das v2.0-Entwicklerhandbuch >>](active-directory-appmodel-v2-overview.md)
* [StackOverflow-Tag „azure-active-directory“ >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Abrufen von Sicherheitsupdates für unsere Produkte
Wir empfehlen Ihnen, den Erhalt von Benachrichtigungen zu Sicherheitsvorfällen einzurichten. Rufen Sie dazu [diese Seite](https://technet.microsoft.com/security/dd252948) auf, und abonnieren Sie Sicherheitsempfehlungen.




<!--HONumber=Jan17_HO4-->


