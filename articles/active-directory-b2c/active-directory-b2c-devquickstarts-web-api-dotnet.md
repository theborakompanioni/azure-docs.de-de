---
title: Azure Active Directory B2C | Microsoft Docs
description: Erstellen einer Webanwendung, die eine Web-API mithilfe von Azure Active Directory B2C aufruft.
services: active-directory-b2c
documentationcenter: .net
author: dstrockis
manager: msmbaldwin
editor: ''

ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/22/2016
ms.author: dastrock

---
# Azure AD B2C: Aufrufen einer Web-API über eine .NET-Web-App
Mit Azure Active Directory (Azure AD) B2C können Sie Ihren Web-Apps und Web-APIs in wenigen Schritten leistungsstarke Self-Service-Features zur Identitätsverwaltung hinzufügen. In diesem Artikel wird erläutert, wie Sie eine .NET-MVC-Web-App (Model-View-Controller) für Aufgabenlisten erstellen, die eine Web-API mithilfe von Bearertoken aufruft.

Dieser Artikel behandelt nicht das Implementieren der Anmeldung, Registrierung und Profilverwaltung mit Azure AD B2C. Er konzentriert sich auf das Aufrufen von Web-APIs aufrufen, nachdem der Benutzer bereits authentifiziert wurde. Beginnen Sie, sofern noch nicht geschehen, mit dem [Tutorial zu den ersten Schritten mit .NET-Web-Apps](active-directory-b2c-devquickstarts-web-dotnet.md), um Informationen zu den Grundlagen von Azure AD B2C zu erhalten.

## Erstellen eines Azure AD B2C-Verzeichnisses
Bevor Sie Azure AD B2C verwenden können, müssen Sie ein Verzeichnis oder einen Mandanten erstellen. Ein Verzeichnis ist ein Container für alle Benutzer, Apps, Gruppen usw. Wenn Sie noch keines verwenden, sollten Sie [ein B2C-Verzeichnis erstellen](active-directory-b2c-get-started.md), bevor Sie die weiteren Schritte in diesem Leitfaden ausführen.

## Erstellen einer Anwendung
Als Nächstes müssen Sie in Ihrem B2C-Verzeichnis eine App erstellen. Dadurch werden Azure AD die Informationen bereitgestellt, die für die sichere Kommunikation mit Ihrer App erforderlich sind. In diesem Fall werden die Web-App und die Web-API durch eine einzige **Anwendungs-ID** dargestellt, da sie zusammen eine logische App bilden. Befolgen Sie zum Erstellen einer App [diese Anweisungen](active-directory-b2c-app-registration.md). Führen Sie folgende Schritte aus:

* Fügen Sie der Anwendung eine **Web-App/Web-API** hinzu.
* Geben Sie `https://localhost:44316/` als **Antwort-URL** ein. Dies ist die Standard-URL für dieses Codebeispiel.
* Kopieren Sie die **Anwendungs-ID**, die Ihrer App zugewiesen ist. Diese benötigen sie später ebenfalls.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## Erstellen der Richtlinien
In Azure AD B2C wird jede Benutzeroberfläche durch eine [Richtlinie](active-directory-b2c-reference-policies.md) definiert. Diese Web-App enthält drei Benutzeroberflächen, für die Identitäten relevant sind: Registrierung, Anmeldung und Profilbearbeitung. Sie müssen eine Richtlinie jedes Typs erstellen, wie im [Artikel zu Richtlinienreferenzen](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy) beschrieben. Beachten Sie beim Erstellen der drei Richtlinien Folgendes:

* Wählen Sie den **Anzeigenamen** und andere Registrierungsattribute in der Registrierungsrichtlinie aus.
* Wählen Sie den **Anzeigenamen** und die **Objekt-ID** als Anwendungsansprüche in jeder Richtlinie aus. Sie können auch andere Ansprüche auswählen.
* Notieren Sie sich die **Namen** der einzelnen Richtlinien nach ihrer Erstellung. Sie müssen das Präfix `b2c_1_` aufweisen. Sie benötigen diese Richtliniennamen später.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nachdem Sie die drei Richtlinien erstellt haben, können Sie Ihre App erstellen.

Beachten Sie, dass in diesem Artikel nicht behandelt wird, wie die gerade erstellten Richtlinien verwendet werden. Grundlegende Informationen zur Funktionsweise von Richtlinien in Azure AD B2C finden Sie im [Tutorial zu den ersten Schritten mit .NET-Web-Apps](active-directory-b2c-devquickstarts-web-dotnet.md).

## Herunterladen des Codes
Der Code für dieses Tutorial wird [auf GitHub](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet) verwaltet. Zum Erstellen des Beispiels können Sie [ein Projektgerüst als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/skeleton.zip). Sie können das Gerüst auch klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git
```

Die fertige App ist ebenfalls [als ZIP-Datei](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip) oder unter der Verzweigung `complete` im gleichen Repository verfügbar.

Nachdem Sie den Beispielcode heruntergeladen haben, öffnen Sie zum Einstieg die Visual Studio-SLN-Datei.

## Konfigurieren der Aufgaben-Web-App
Damit `TaskWebApp` mit Azure AD B2C kommunizieren kann, müssen Sie einige allgemeine Parameter angeben. Öffnen Sie im `TaskWebApp`-Projekt die Datei `web.config` im Stammverzeichnis des Projekts, und ersetzen Sie die Werte im Abschnitt `<appSettings>`. Sie können die Werte `AadInstance`, `RedirectUri` und `TaskServiceUrl` unverändert lassen.

```
  <appSettings>

    ...

    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" />
  </appSettings>
```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

<appSettings> <add key="webpages:Version" value="3.0.0.0" /> <add key="webpages:Enabled" value="false" /> <add key="ClientValidationEnabled" value="true" /> <add key="UnobtrusiveJavaScriptEnabled" value="true" /> <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" /> <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" /> <add key="ida:ClientSecret" value="E:i~5GHYRF$Y7BcM" /> <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" /> <add key="ida:RedirectUri" value="https://localhost:44316/" /> <add key="ida:SignUpPolicyId" value="b2c\_1_sign\_up" /> <add key="ida:SignInPolicyId" value="b2c_1_sign\_in" /> <add key="ida:UserProfilePolicyId" value="b2c_1\_edit\_profile" /> <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" /> </appSettings>

## Abrufen von Zugriffstoken und Aufrufen der Aufgaben-API
In diesem Abschnitt wird beschrieben, wie Sie das während der Anmeldung mit Azure AD B2C erhaltene Token verwenden, um auf eine Web-API zuzugreifen, die ebenfalls mit Azure AD B2C geschützt ist.

In diesem Artikel werden nicht die Details der Sicherung einer API behandelt. Um zu erfahren, wie eine Web-API Anforderungen mithilfe von Azure AD B2C sicher authentifiziert, können Sie sich den [Artikel mit den ersten Schritten für die Web-API](active-directory-b2c-devquickstarts-api-dotnet.md) durchlesen.

### Speichern des Anmeldetokens
Zuerst authentifizieren Sie den Benutzer (mit einer Ihrer Richtlinien) und erhalten ein Anmeldetoken von Azure AD B2C. Wenn Sie nicht sicher sind, wie Sie Richtlinien ausführen sollen, hilft Ihnen das [Tutorial zu den ersten Schritten mit .NET-Web-Apps](active-directory-b2c-devquickstarts-web-dotnet.md) weiter. Darin werden die Grundlagen von Azure AD B2C beschrieben.

Öffnen Sie die Datei `App_Start\Startup.Auth.cs`. Sie müssen eine wichtige Änderung an `OpenIdConnectAuthenticationOptions` vornehmen. Folgendes muss gelten: `SaveSignInToken = true`.

```C#
// App_Start\Startup.Auth.cs

return new OpenIdConnectAuthenticationOptions
{
    // For each policy, give OWIN the policy-specific metadata address, and
    // set the authentication type to the id of the policy
    MetadataAddress = String.Format(aadInstance, tenant, policy),
    AuthenticationType = policy,

    // These are standard OpenID Connect parameters, with values pulled from web.config
    ClientId = clientId,
    RedirectUri = redirectUri,
    PostLogoutRedirectUri = redirectUri,
    Notifications = new OpenIdConnectAuthenticationNotifications
    {
        AuthenticationFailed = OnAuthenticationFailed,
    },
    Scope = "openid",
    ResponseType = "id_token",

    TokenValidationParameters = new TokenValidationParameters
    {
        NameClaimType = "name",

        // Add this line to reserve the sign in token for later use
        SaveSigninToken = true,
    },
};
```

### Abrufen eines Tokens in den Controllern
`TasksController` ist verantwortlich für die Kommunikation mit der Web-API und sendet HTTP-Anforderungen an die API, um Aufgaben zu lesen, zu erstellen und zu löschen. Da die API mit Azure AD B2C geschützt wird, müssen Sie zuerst das Token abrufen, das Sie im obigen Schritt gespeichert haben.

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        var bootstrapContext = ClaimsPrincipal.Current.Identities.First().BootstrapContext as System.IdentityModel.Tokens.BootstrapContext;

    ...
}
```

`BootstrapContext` enthält das Anmeldetoken, das Sie beschafft haben, indem Sie eine Ihrer B2C-Richtlinien ausgeführt haben.

### Lesen von Aufgaben aus der Web-API
Wenn Sie über ein Token verfügen, können Sie es an die HTTP-`GET`-Anforderung im `Authorization`-Header anfügen, um `TaskService` sicher aufzurufen:

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        ...

        HttpClient client = new HttpClient();
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, serviceUrl + "/api/tasks");

        // Add the token acquired from ADAL to the request headers
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", bootstrapContext.Token);
        HttpResponseMessage response = await client.SendAsync(request);

        if (response.IsSuccessStatusCode)
        {
            String responseString = await response.Content.ReadAsStringAsync();
            JArray tasks = JArray.Parse(responseString);
            ViewBag.Tasks = tasks;
            return View();
        }
        else
        {
            // If the call failed with access denied, show the user an error indicating they might need to sign-in again.
            if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
            {
                return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
            }
        }

        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + response.StatusCode);
    }
    catch (Exception ex)
    {
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ex.Message);
    }
}

```

### Erstellen und Löschen von Aufgaben mit der Web-API
Folgen Sie dem gleichen Muster, wenn Sie `POST`- und `DELETE`-Anforderungen an die Web-API senden, indem Sie `BootstrapContext` zum Abrufen des Anmeldetokens verwenden. Wir haben diese Aktion für Sie erstellt. Sie können versuchen, die Löschaktion in `TasksController.cs` zu beenden.

## Ausführen der Beispiel-App
Der letzte Schritt besteht im Erstellen und Ausführen der App. Registrieren Sie sich, und melden Sie sich an. Erstellen Sie anschließend die Aufgaben für den angemeldeten Benutzer. Melden Sie sich ab, und melden Sie sich als ein anderer Benutzer an. Erstellen Sie Aufgaben für diesen Benutzer. Beachten Sie, wie die Aufgaben pro Benutzer in der API gespeichert werden, da die API die Benutzeridentität aus dem empfangenen Token extrahiert.

Als Referenz wird das fertige Beispiel [als ZIP-Datei bereitgestellt](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip). Sie können sie auch aus GitHub klonen:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->

<!---HONumber=AcomDC_0727_2016-->