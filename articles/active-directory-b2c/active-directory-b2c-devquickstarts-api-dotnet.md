---
title: Azure AD B2C | Microsoft Docs
description: "Erstellen einer .NET-Web-API mithilfe von Azure Active Directory B2C, die mit OAuth 2.0-Zugriffstoken für die Authentifizierung gesichert ist."
services: active-directory-b2c
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 7146ed7f-2eb5-49e9-8d8b-ea1a895e1966
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 01/07/2017
ms.author: dastrock
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 1062af9abfd167dd251621a43943dea399aed027


---
# <a name="azure-active-directory-b2c-build-a-net-web-api"></a>Azure Active Directory B2C: Erstellen einer .NET-Web-API
<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Mit Azure Active Directory (Azure AD) B2C können Sie eine Web-API mithilfe von OAuth 2.0-Zugriffstoken sichern. Mit diesen Token können Ihre Client-Apps, die Azure AD B2C verwenden, sich bei der API authentifizieren. In diesem Artikel erfahren Sie, wie Sie eine .NET MVC-API (Model-View-Controller) für eine Aufgabenliste erstellen, mit der Benutzer CRUD-Vorgänge für Aufgaben durchführen können. Die Web-API wird mit Azure AD B2C geschützt, und nur authentifizierte Benutzer können ihre Aufgabenliste verwalten.

## <a name="create-an-azure-ad-b2c-directory"></a>Erstellen eines Azure AD B2C-Verzeichnisses
Bevor Sie Azure AD B2C verwenden können, müssen Sie ein Verzeichnis oder einen Mandanten erstellen. Ein Verzeichnis ist ein Container für all Ihre Benutzer, Apps, Gruppen usw. Wenn Sie noch keines verwenden, sollten Sie [ein B2C-Verzeichnis erstellen](active-directory-b2c-get-started.md) , bevor Sie die weiteren Schritte in diesem Leitfaden ausführen.

## <a name="create-an-application"></a>Erstellen einer Anwendung
Als Nächstes müssen Sie in Ihrem B2C-Verzeichnis eine App erstellen. Dadurch werden Azure AD die Informationen bereitgestellt, die für die sichere Kommunikation mit Ihrer App erforderlich sind. Befolgen Sie zum Erstellen einer App [diese Anweisungen](active-directory-b2c-app-registration.md). Führen Sie folgende Schritte aus:

* Fügen Sie der Anwendung eine **Web-App** oder **Web-API** hinzu.
* Verwenden Sie für die Web-App den **Umleitungs-URI** (Uniform Resource Identifier) `https://localhost:44316/`. Dies ist der Standardspeicherort des Web-App-Clients für dieses Codebeispiel.
* Kopieren Sie die **Anwendungs-ID** , die Ihrer App zugewiesen ist. Sie benötigen sie später.

  [!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Erstellen der Richtlinien
In Azure AD B2C wird jede Benutzeroberfläche durch eine [Richtlinie](active-directory-b2c-reference-policies.md)definiert. Der Client in diesem Codebeispiel enthält drei Benutzeroberflächen, für die Identitäten relevant sind: Registrierung, Anmeldung und Profilbearbeitung. Sie müssen für jeden Typ eine Richtlinie erstellen, wie im [Artikel mit Richtlinienreferenzen](active-directory-b2c-reference-policies.md#create-a-sign-up-policy)beschrieben. Beachten Sie beim Erstellen der drei Richtlinien Folgendes:

* Wählen Sie auf dem Blatt für den Identitätsanbieter **User ID sign-up** (Registrierung mit Benutzer-ID) oder **Email sign-up** (Registrierung mit E-Mail-Adresse) aus.
* Wählen Sie den **Anzeigenamen** und andere Registrierungsattribute in der Registrierungsrichtlinie aus.
* Wählen Sie den **Anzeigenamen** und die **Objekt-ID** als Anwendungsansprüche für jede Richtlinie aus. Sie können auch andere Ansprüche auswählen.
* Notieren Sie sich die **Namen** der einzelnen Richtlinien nach ihrer Erstellung. Sie benötigen diese Richtliniennamen später.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nachdem Sie die drei Richtlinien erfolgreich erstellt haben, können Sie Ihre App erstellen.

## <a name="download-the-code"></a>Herunterladen des Codes
Der Code für dieses Tutorial wird [auf GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet)verwaltet. Zum Erstellen des Beispiels können Sie [ein Projektgerüst als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/skeleton.zip). Sie können das Gerüst auch klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet.git
```

Die fertige App ist ebenfalls als [ZIP-Datei](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/complete.zip) oder unter der Verzweigung `complete` im gleichen Repository verfügbar.

Nachdem Sie den Beispielcode heruntergeladen haben, öffnen Sie zum Einstieg die Visual Studio-SLN-Datei. Die Projektmappe enthält jetzt zwei Projekte: `TaskWebApp` und `TaskService`. `TaskWebApp` ist eine MVC-Webanwendung, mit der der Benutzer interagiert. `TaskService` ist die Back-End-Web-API der App, in der die Aufgabenlisten der einzelnen Benutzer gespeichert werden.

## <a name="configure-the-task-web-app"></a>Konfigurieren der Aufgaben-Web-App
Wenn ein Benutzer mit `TaskWebApp` interagiert, sendet der Client Anforderungen an Azure AD und erhält dafür Token, die für den Aufruf der Web-API `TaskService` verwendet werden können. Zum Anmelden des Benutzers und zum Abrufen von Token benötigt `TaskWebApp` einige Informationen über Ihre App. Öffnen Sie im `TaskWebApp`-Projekt die Datei `web.config` im Stammverzeichnis des Projekts, und ersetzen Sie die Werte im Abschnitt `<appSettings>`.  Sie können die Werte `AadInstance`, `RedirectUri` und `TaskServiceUrl` unverändert lassen.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://localhost:44332/" />
  </appSettings>
```

Die Erstellung des `TaskWebApp` -Clients wird in diesem Artikel nicht behandelt.  Informationen zum Erstellen einer Web-App mit Azure AD B2C finden Sie unserem [.NET-Web-App-Tutorial](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="secure-the-api"></a>Sichern der API
Wenn Sie über einen Client verfügen, der die API im Auftrag eines Benutzers aufruft, können Sie `TaskService` mit OAuth 2.0-Bearertoken sichern. Die API kann Token mit der OWIN-Bibliothek von Microsoft (Open Web Interface for .NET) akzeptieren und überprüfen.

### <a name="install-owin"></a>Installieren von OWIN
Installieren Sie zuerst die OWIN-OAuth-Authentifizierungspipeline:

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

### <a name="enter-your-b2c-details"></a>Eingeben der B2C-Details
Öffnen Sie die Datei `web.config` aus dem Stammverzeichnis des `TaskService`-Projekts, und ersetzen Sie die Werte im Abschnitt `<appSettings>`. Diese Werte werden in der API und der OWIN-Bibliothek verwendet.  Sie können den Wert `AadInstance` unverändert lassen.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
  </appSettings>
```

### <a name="add-an-owin-startup-class"></a>Hinzufügen einer OWIN-Startklasse
Fügen Sie dem `TaskService`-Projekt eine OWIN-Startklasse mit dem Namen `Startup.cs` hinzu.  Klicken Sie mit der rechten Maustaste auf das Projekt, wählen Sie **Hinzufügen** und **Neues Element** aus, und suchen Sie dann nach „OWIN“.

```C#
// Startup.cs

// Change the class declaration to "public partial class Startup" - we’ve already implemented part of this class for you in another file.
public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

### <a name="configure-oauth-20-authentication"></a>Konfigurieren der OAuth 2.0-Authentifizierung
Öffnen Sie die Datei `App_Start\Startup.Auth.cs`, und implementieren Sie die Methode `ConfigureAuth(...)`:

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // These values are pulled from web.config
    public static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    public static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    public static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    public static string signUpPolicy = ConfigurationManager.AppSettings["ida:SignUpPolicyId"];
    public static string signInPolicy = ConfigurationManager.AppSettings["ida:SignInPolicyId"];
    public static string editProfilePolicy = ConfigurationManager.AppSettings["ida:UserProfilePolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {   
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signUpPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signInPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(editProfilePolicy));
    }

    public OAuthBearerAuthenticationOptions CreateBearerOptionsFromPolicy(string policy)
    {
        TokenValidationParameters tvps = new TokenValidationParameters
        {
            // This is where you specify that your API only accepts tokens from its own clients
            ValidAudience = clientId,
            AuthenticationType = policy,
        };

        return new OAuthBearerAuthenticationOptions
        {
            // This SecurityTokenProvider fetches the Azure AD B2C metadata & signing keys from the OpenIDConnect metadata endpoint
            AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(aadInstance, tenant, policy))),
        };
    }
}
```

### <a name="secure-the-task-controller"></a>Sichern des Aufgabencontrollers
Nachdem Sie die App für die OAuth 2.0-Authentifizierung konfiguriert haben, können Sie Ihre Web-API sichern, indem Sie dem Aufgabencontroller ein `[Authorize]`-Tag hinzufügen. Dies ist der Controller, in dem die gesamte Bearbeitung der Aufgabenliste durchgeführt wird. Daher wird der gesamte Controller auf Klassenebene gesichert. Zur präziseren Steuerung können Sie das `[Authorize]`-Tag auch einzelnen Aktionen hinzufügen.

```C#
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-the-token"></a>Abrufen von Benutzerinformationen aus dem Token
`TasksController` speichert Aufgaben in einer Datenbank, wobei jeder Aufgabe ein Benutzer zugeordnet ist, der die Aufgabe „besitzt“. Der Besitzer wird durch die **Objekt-ID**des Benutzers identifiziert. (Daher mussten Sie die Objekt-ID als Anwendungsanspruch in all Ihren Richtlinien hinzufügen.)

```C#
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

## <a name="run-the-sample-app"></a>Ausführen der Beispiel-App
Zum Schluss erstellen Sie `TaskWebApp` und `TaskService` und führen diese Elemente aus. Registrieren Sie sich für die App mit einer E-Mail-Adresse oder einem Benutzernamen. Erstellen Sie einige Aufgaben in der Aufgabenliste des Benutzers, und beachten Sie, wie sie in der API beibehalten werden, auch nachdem Sie den Client beendet und neu gestartet haben.

## <a name="edit-your-policies"></a>Bearbeiten der Richtlinien
Nachdem Sie eine API mit Azure AD B2C gesichert haben, können Sie mit den Richtlinien Ihrer App experimentieren und die Auswirkungen (sofern vorhanden) auf die API beobachten. Sie können die Anwendungsansprüche in den Richtlinien bearbeiten und die Benutzerinformationen ändern, die in der Web-API verfügbar sind. Ansprüche, die Sie hinzufügen, stehen Ihrer .NET MVC-Web-API im `ClaimsPrincipal` -Objekt zur Verfügung, wie oben in diesem Artikel beschrieben.

<!--

## Next steps

You can now move onto more advanced B2C topics. You may try:

[Call a web API from a web app]()

[Customize the UX of your B2C app]()

-->



<!--HONumber=Dec16_HO4-->


