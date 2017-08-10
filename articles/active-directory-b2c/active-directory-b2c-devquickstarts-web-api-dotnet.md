---
title: Azure Active Directory B2C | Microsoft Docs
description: Es wird beschrieben, wie Sie mithilfe von Azure Active Directory B2C und OAuth 2.0-Zugriffstokens eine .NET-Webanwendung erstellen und eine Web-API aufrufen.
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: krassk
editor: 
ms.assetid: d3888556-2647-4a42-b068-027f9374aa61
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/17/2017
ms.author: parakhj
ms.translationtype: HT
ms.sourcegitcommit: 0aae2acfbf30a77f57ddfbaabdb17f51b6938fd6
ms.openlocfilehash: 48452eb68f826d1c7aa61d5e5531f941ac1422b0
ms.contentlocale: de-de
ms.lasthandoff: 08/09/2017

---
# <a name="azure-ad-b2c-call-a-net-web-api-from-a-net-web-app"></a>Azure AD B2C: Aufrufen einer .NET-Web-API über eine .NET-Webanwendung

Mit Azure AD B2C können Sie Ihren Web-Apps und Web-APIs leistungsstarke Features zur Identitätsverwaltung hinzufügen. In diesem Artikel wird beschrieben, wie von einer .NET-Webanwendung mit Aufgabenliste Zugriffstokens von einer .NET-Web-API angefordert und Aufrufe getätigt werden.

Dieser Artikel behandelt nicht das Implementieren der Anmeldung, Registrierung und Profilverwaltung mit Azure AD B2C. Er konzentriert sich auf das Aufrufen von Web-APIs aufrufen, nachdem der Benutzer bereits authentifiziert wurde. Falls noch nicht geschehen, sollten Sie Folgendes durchgehen:

* Erste Schritte mit einer [.NET-Webanwendung](active-directory-b2c-devquickstarts-web-dotnet-susi.md)
* Erste Schritte mit einer [.NET-Web-API](active-directory-b2c-devquickstarts-api-dotnet.md)

## <a name="prerequisite"></a>Voraussetzung

Um eine Webanwendung zu erstellen, die eine Web-API aufruft, sind folgende Schritte erforderlich:

1. [Erstellen eines Azure AD B2C-Mandanten](active-directory-b2c-get-started.md)
2. [Registrieren einer Web-API](active-directory-b2c-app-registration.md#register-a-web-api)
3. [Registrieren einer Web-App](active-directory-b2c-app-registration.md#register-a-web-app)
4. [Einrichten von Richtlinien](active-directory-b2c-reference-policies.md)
5. [Erteilen von Berechtigungen für die Web-App zur Verwendung der Web-API](active-directory-b2c-access-tokens.md#publishing-permissions)

> [!IMPORTANT]
> Die Clientanwendung und die Web-API müssen dasselbe Azure AD B2C-Verzeichnis verwenden.
>

## <a name="download-the-code"></a>Herunterladen des Codes

Der Code für dieses Tutorial wird auf [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi) verwaltet. Sie können das Beispiel klonen, indem Sie Folgendes ausführen:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Nachdem Sie den Beispielcode heruntergeladen haben, öffnen Sie zum Einstieg die Visual Studio-SLN-Datei. Die Projektmappe enthält jetzt zwei Projekte: `TaskWebApp` und `TaskService`. `TaskWebApp` ist eine MVC-Webanwendung, mit der der Benutzer interagiert. `TaskService` ist die Back-End-Web-API der App, in der die Aufgabenlisten der einzelnen Benutzer gespeichert werden. Die Erstellung der `TaskWebApp`-Web-App oder `TaskService`-Web-API wird in diesem Artikel nicht behandelt. Informationen zum Erstellen der .NET-Webanwendung mit Azure AD B2C finden Sie unserem [.NET-Web-App-Tutorial](active-directory-b2c-devquickstarts-web-dotnet-susi.md). Informationen zum Erstellen der .NET-Web-API, die mit Azure AD B2C geschützt ist, finden Sie unserem [.NET-Web-API-Tutorial](active-directory-b2c-devquickstarts-api-dotnet.md).

### <a name="update-the-azure-ad-b2c-configuration"></a>Aktualisieren der Azure AD B2C-Konfiguration

Unser Beispiel ist so konfiguriert, dass die Richtlinien und die Client-ID unseres Demomandanten verwendet werden. Wenn Sie Ihren eigenen Mandanten verwenden möchten, gehen Sie wie folgt vor:

1. Öffnen Sie `web.config` im Projekt `TaskService` und ersetzen Sie die Werte wie folgt:

    * `ida:Tenant` durch Ihren Mandantennamen
    * `ida:ClientId` durch Ihre Anwendungs-ID der Web-API
    * `ida:SignUpSignInPolicyId` durch Ihren Namen für die Richtlinie zur Registrierung bzw. Anmeldung

2. Öffnen Sie `web.config` im Projekt `TaskWebApp` und ersetzen Sie die Werte wie folgt:

    * `ida:Tenant` durch Ihren Mandantennamen
    * `ida:ClientId` durch Ihre Anwendungs-ID der Web-App
    * `ida:ClientSecret` durch Ihren geheimen Schlüssel der Web-App
    * `ida:SignUpSignInPolicyId` durch Ihren Namen für die Richtlinie zur Registrierung bzw. Anmeldung
    * `ida:EditProfilePolicyId` durch den Namen der Richtlinie für „Profil bearbeiten“
    * `ida:ResetPasswordPolicyId` durch den Namen der Richtlinie für „Kennwort zurücksetzen“



## <a name="requesting-and-saving-an-access-token"></a>Anfordern und Speichern eines Zugriffstokens

### <a name="specify-the-permissions"></a>Festlegen von Berechtigungen

Um die Web-API aufzurufen, müssen Sie den Benutzer (mithilfe der Registrierung-/Anmelderichtlinie) authentifizieren und [einen Zugriffstoken von Azure AD B2C erhalten](active-directory-b2c-access-tokens.md). Um ein Zugriffstoken zu erhalten, müssen Sie zunächst die Berechtigungen festlegen, die dem Zugriffstoken erteilt werden sollen. Die Berechtigungen werden im Parameter `scope` festgelegt, wenn Sie die Anforderung beim Endpunkt `/authorize` tätigen. Um ein Zugriffstoken mit Leseberechtigung von der Ressourcenanwendung mit dem App-ID-URI `https://contoso.onmicrosoft.com/tasks` abzurufen, würde der Bereich `https://contoso.onmicrosoft.com/tasks/read` lauten.

Um den Bereich in diesem Beispiel anzugeben, öffnen Sie die Datei `App_Start\Startup.Auth.cs` und definieren Sie die `Scope`-Variable in OpenIdConnectAuthenticationOptions.

```CSharp
// App_Start\Startup.Auth.cs

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            ...

            // Specify the scope by appending all of the scopes requested into one string (seperated by a blank space)
            Scope = $"{OpenIdConnectScopes.OpenId} {ReadTasksScope} {WriteTasksScope}"
        }
    );
}
```

### <a name="exchange-the-authorization-code-for-an-access-token"></a>Austauschen des Autorisierungscodes gegen ein Zugriffstoken

Nachdem ein Benutzer die Registrierung oder Anmeldung durchgeführt hat, erhält Ihre App einen Autorisierungscode von Azure AD B2C. Die OWIN OpenID Connect-Middleware speichert den Code, tauscht diesen jedoch nicht gegen ein Zugriffstoken aus. Sie können die [MSAL-Bibliothek](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) für den Austausch verwenden. In diesem Beispiel wurde stets ein Benachrichtigungsrückruf in der OpenID Connect-Middleware beim Empfang eines Autorisierungscodes konfiguriert. Beim Rückruf wird MSAL verwendet, um den Code gegen ein Token auszutauschen und den Token im Cache zu speichern.

```CSharp
/*
* Callback function when an authorization code is received
*/
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
    // Extract the code from the response notification
    var code = notification.Code;

    var userObjectId = notification.AuthenticationTicket.Identity.FindFirst(ObjectIdElement).Value;
    var authority = String.Format(AadInstance, Tenant, DefaultPolicy);
    var httpContext = notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase;

    // Exchange the code for a token. Make sure to specify the necessary scopes
    ClientCredential cred = new ClientCredential(ClientSecret);
    ConfidentialClientApplication app = new ConfidentialClientApplication(authority, Startup.ClientId,
                                            RedirectUri, cred, new NaiveSessionCache(userObjectId, httpContext));
    var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { ReadTasksScope, WriteTasksScope }, code, DefaultPolicy);
}
```

## <a name="calling-the-web-api"></a>Aufrufen der Web-API

In diesem Abschnitt wird beschrieben, wie Sie das während der Registrierung/Anmeldung bei Azure AD B2C erhaltene Token verwenden, um auf die Web-API zuzugreifen.

### <a name="retrieve-the-saved-token-in-the-controllers"></a>Abrufen des gespeicherten Tokens in den Controllern

`TasksController` ist verantwortlich für die Kommunikation mit der Web-API und das Senden von HTTP-Anforderungen an die API, um Aufgaben zu lesen, zu erstellen und zu löschen. Da die API mit Azure AD B2C geschützt wird, müssen Sie zuerst das Token abrufen, das Sie im obigen Schritt gespeichert haben.

```CSharp
// Controllers\TasksController.cs

/*
* Uses MSAL to retrieve the token from the cache
*/
private async void acquireToken(String[] scope)
{
    string userObjectID = ClaimsPrincipal.Current.FindFirst(Startup.ObjectIdElement).Value;
    string authority = String.Format(Startup.AadInstance, Startup.Tenant, Startup.DefaultPolicy);

    ClientCredential credential = new ClientCredential(Startup.ClientSecret);

    // Retrieve the token using the provided scopes
    ConfidentialClientApplication app = new ConfidentialClientApplication(authority, Startup.ClientId,
                                        Startup.RedirectUri, credential,
                                        new NaiveSessionCache(userObjectID, this.HttpContext));
    AuthenticationResult result = await app.AcquireTokenSilentAsync(scope);

    accessToken = result.Token;
}
```

### <a name="read-tasks-from-the-web-api"></a>Lesen von Aufgaben aus der Web-API

Wenn Sie über ein Token verfügen, können Sie es an die HTTP-`GET`-Anforderung im `Authorization`-Header anfügen, um `TaskService` sicher aufzurufen:

```CSharp
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try {

        // Retrieve the token with the specified scopes
        acquireToken(new string[] { Startup.ReadTasksScope });

        HttpClient client = new HttpClient();
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, apiEndpoint);

        // Add token to the Authorization header and make the request
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
        HttpResponseMessage response = await client.SendAsync(request);

        // Handle response ...
}

```

### <a name="create-and-delete-tasks-on-the-web-api"></a>Erstellen und Löschen von Aufgaben mit der Web-API

Folgen Sie demselben Muster, wenn Sie `POST`- und `DELETE`-Anforderungen an die Web-API senden, indem Sie MSAL zum Abrufen des Zugriffstokens aus dem Cache verwenden.

## <a name="run-the-sample-app"></a>Ausführen der Beispiel-App

Erstellen Sie abschließend beide Apps und führen Sie diese aus. Registrieren Sie sich, und melden Sie sich an. Erstellen Sie anschließend die Aufgaben für den angemeldeten Benutzer. Melden Sie sich ab, und melden Sie sich als ein anderer Benutzer an. Erstellen Sie Aufgaben für diesen Benutzer. Beachten Sie, wie die Aufgaben pro Benutzer in der API gespeichert werden, da die API die Benutzeridentität aus dem empfangenen Token extrahiert. Machen Sie sich auch mit den Bereichen vertraut. Ändern Sie die Berechtigung in eine Schreibberechtigung und versuchen Sie dann, eine Aufgabe hinzuzufügen. Achten Sie lediglich darauf, sich jedes Mal abzumelden, wenn Sie den Bereich ändern.


