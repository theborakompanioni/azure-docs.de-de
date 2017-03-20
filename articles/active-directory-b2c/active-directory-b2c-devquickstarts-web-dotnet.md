---
title: Azure Active Directory B2C | Microsoft Docs
description: Erstellen einer Webanwendung mit Anmeldung, Registrierung und Profilverwaltung mithilfe von Azure Active Directory B2C.
services: active-directory-b2c
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 0b1c6ede-de0a-4c32-92b3-5c813dc26804
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
translationtype: Human Translation
ms.sourcegitcommit: a977cb509fb64d7c986e2e0f7e2b5e4e3e45dec0
ms.openlocfilehash: a0e03bda81d26dbc28706b45ee974359337573f0
ms.lasthandoff: 12/28/2016


---
# <a name="azure-ad-b2c-build-a-net-web-app"></a>Azure AD B2C: Erstellen einer .NET-Web-App
<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Mit Azure Active Directory (Azure AD) B2C können Sie Ihren Web-Apps in wenigen Schritten leistungsstarke Self-Service-Features zur Identitätsverwaltung hinzufügen. In diesem Artikel erfahren Sie, wie eine .NET MVC-Web-App (Model-View-Controller) erstellt wird, die Benutzerregistrierung, -anmeldung und die Verwaltung von Profilen umfasst. Die App umfasst Unterstützung für die Registrierung und Anmeldung mit einem Benutzernamen oder einer E-Mail-Adresse sowie über Konten für soziale Netzwerke wie Facebook und Google.

## <a name="get-an-azure-ad-b2c-directory"></a>Erstellen eines Azure AD B2C-Verzeichnisses
Bevor Sie Azure AD B2C verwenden können, müssen Sie ein Verzeichnis oder einen Mandanten erstellen. Ein Verzeichnis ist ein Container für all Ihre Benutzer, Apps, Gruppen usw.  Wenn Sie noch keines verwenden, sollten Sie [ein B2C-Verzeichnis erstellen](active-directory-b2c-get-started.md) , bevor Sie die weiteren Schritte in diesem Leitfaden ausführen.

## <a name="create-an-application"></a>Erstellen einer Anwendung
Als Nächstes müssen Sie in Ihrem B2C-Verzeichnis eine App erstellen. Dadurch werden Azure AD die Informationen bereitgestellt, die für die sichere Kommunikation mit Ihrer App erforderlich sind. Befolgen Sie zum Erstellen einer App [diese Anweisungen](active-directory-b2c-app-registration.md).  Führen Sie folgende Schritte aus:

* Fügen Sie der Anwendung eine **Web-App/Web-API** hinzu.
* Geben Sie `https://localhost:44316/` als **Umleitungs-URI**ein. Dies ist die Standard-URL für dieses Codebeispiel.
* Notieren Sie die **Anwendungs-ID** , die Ihrer App zugewiesen ist.  Sie benötigen sie später.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Erstellen der Richtlinien
In Azure AD B2C wird jede Benutzeroberfläche durch eine [Richtlinie](active-directory-b2c-reference-policies.md)definiert. Dieses Codebeispiel enthält drei Benutzeroberflächen, für die Identitäten relevant sind: Registrierung, Anmeldung und Profilbearbeitung. Sie müssen eine Richtlinie jedes Typs erstellen, wie im [Artikel zu Richtlinienreferenzen](active-directory-b2c-reference-policies.md#create-a-sign-up-policy)beschrieben.

> [!NOTE]
> Azure AD B2C unterstützt auch eine kombinierte Richtlinie für die Registrierung oder Anmeldung, die in diesem Tutorial jedoch nicht behandelt wird.  Sie finden die Richtlinie für die Registrierung oder Anmeldung in [diesem verwandten Tutorial](active-directory-b2c-devquickstarts-web-dotnet-susi.md).
>
>

Beachten Sie beim Erstellen der drei Richtlinien Folgendes:

* Wählen Sie auf dem Blatt für den Identitätsanbieter **Registrierung mit Benutzer-ID** oder **E-Mail-Registrierung** aus.
* Wählen Sie den **Anzeigenamen** und andere Registrierungsattribute in der Registrierungsrichtlinie aus.
* Wählen Sie den Anspruch **Anzeigename** als Anwendungsanspruch in den Richtlinien aus. Sie können auch andere Ansprüche auswählen.
* Notieren Sie sich die **Namen** der einzelnen Richtlinien nach ihrer Erstellung. Sie benötigen diese Richtliniennamen später.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nachdem Sie die drei Richtlinien erstellt haben, können Sie Ihre App erstellen.  

## <a name="download-the-code-and-configure-authentication"></a>Herunterladen des Codes und Konfigurieren der Authentifizierung
Der Code für dieses Beispiel wird [auf GitHub](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet)verwaltet. Zum Erstellen des Beispiels können Sie [ein Projektgerüst als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip). Sie können das Gerüst auch klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet.git
```

Das fertige Beispiel ist ebenfalls als [ZIP-Datei verfügbar](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet/archive/complete.zip) oder unter der Verzweigung `complete` im gleichen Repository enthalten.

Nachdem Sie den Beispielcode heruntergeladen haben, öffnen Sie zum Einstieg die Visual Studio-SLN-Datei.

Ihre App kommuniziert mit Azure AD B2C durch Senden von Authentifizierungsnachrichten, in denen die Richtlinie, die ausgeführt werden soll, als Teil der HTTP-Anforderung angegeben wird. Für .NET-Webanwendungen können Sie die OWIN-Middleware von Microsoft verwenden, um OpenID Connect-Authentifizierungsanfragen zu senden, Richtlinien auszuführen, die Sitzung des Benutzers zu verwalten usw.

Fügen Sie dem Projekt zunächst über die Visual Studio-Paket-Manager-Konsole die NuGet-Pakete der OWIN-Middleware hinzu.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

Öffnen Sie dann die Datei `web.config` aus dem Stammverzeichnis des Projekts, und geben Sie die Konfigurationswerte Ihrer App im Abschnitt `<appSettings>` ein. Ersetzen Sie dabei die vorhandenen Werte.

```
<configuration>
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
  </appSettings>
...
```

[!INCLUDE [active-directory-b2c-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Fügen Sie als Nächstes dem Projekt eine OWIN-Startklasse mit dem Namen `Startup.cs`hinzu. Klicken Sie mit der rechten Maustaste auf das Projekt, wählen Sie **Hinzufügen** und **Neues Element** aus, und suchen Sie dann nach „OWIN“. **Ändern Sie die Klassendeklaration in `public partial class Startup`**. Wir haben für Sie einen Teil dieser Klasse in einer anderen Datei implementiert. Die OWIN-Middleware ruft beim Starten Ihrer Anwendung die Methode `Configuration(...)` auf. Rufen Sie in dieser Methode `ConfigureAuth(...)`auf, um die Authentifizierung für Ihre App einzurichten.

```C#
// Startup.cs

public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

Öffnen Sie die Datei `App_Start\Startup.Auth.cs`, und implementieren Sie die Methode `ConfigureAuth(...)`.  Die Parameter, die Sie in `OpenIdConnectAuthenticationOptions` angeben, dienen Ihrer App als Koordinaten für die Kommunikation mit Azure AD. Außerdem müssen Sie die Cookieauthentifizierung einrichten. Die Middleware OpenID Connect verwendet Cookies unter anderem für die Verwaltung von Benutzersitzungen.

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // App config settings
    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    private static string redirectUri = ConfigurationManager.AppSettings["ida:RedirectUri"];

    // B2C policy identifiers
    public static string SignUpPolicyId = ConfigurationManager.AppSettings["ida:SignUpPolicyId"];
    public static string SignInPolicyId = ConfigurationManager.AppSettings["ida:SignInPolicyId"];
    public static string ProfilePolicyId = ConfigurationManager.AppSettings["ida:UserProfilePolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseCookieAuthentication(new CookieAuthenticationOptions());

        // Configure OpenID Connect middleware for each policy
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(SignUpPolicyId));
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(ProfilePolicyId));
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(SignInPolicyId));
    }

    // Used for avoiding yellow-screen-of-death
    private Task AuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
    {
        notification.HandleResponse();
        if (notification.Exception.Message == "access_denied")
        {
            notification.Response.Redirect("/");
        }
        else
        {
            notification.Response.Redirect("/Home/Error?message=" + notification.Exception.Message);
        }

        return Task.FromResult(0);
    }

    private OpenIdConnectAuthenticationOptions CreateOptionsFromPolicy(string policy)
    {
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
                AuthenticationFailed = AuthenticationFailed,
            },
            Scope = "openid",
            ResponseType = "id_token",

            // This piece is optional - it is used for displaying the user's name in the navigation bar.
            TokenValidationParameters = new TokenValidationParameters
            {
                NameClaimType = "name",
            },
        };
    }
}
```

## <a name="send-authentication-requests-to-azure-ad"></a>Senden von Authentifizierungsanforderungen an Azure AD
Ihre App ist nun ordnungsgemäß für die Kommunikation mit Azure AD B2C über das Authentifizierungsprotokoll OpenID Connect konfiguriert.  OWIN kümmert sich um all die Details der Erstellung von Authentifizierungsnachrichten, der Überprüfung der Azure AD-Token und der Verwaltung der Benutzersitzung.  Jetzt müssen nur noch die einzelnen Benutzerabläufe initiiert werden.

Wenn ein Benutzer in der Web-App eine der Schaltflächen **Registrieren**, **Anmelden** oder **Profil bearbeiten** auswählt, wird die zugeordnete Aktion in `Controllers\AccountController.cs` aufgerufen. Mit integrierten OWIN-Methoden können Sie in jedem Fall die richtige Richtlinie auslösen:

```C#
// Controllers\AccountController.cs

public void SignIn()
{
    if (!Request.IsAuthenticated)
    {
        // To execute a policy, you simply need to trigger an OWIN challenge.
        // You can indicate which policy to use by specifying the policy id as the AuthenticationType
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties () { RedirectUri = "/" }, Startup.SignInPolicyId);
    }
}

public void SignUp()
{
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties() { RedirectUri = "/" }, Startup.SignUpPolicyId);
    }
}


public void Profile()
{
    if (Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties() { RedirectUri = "/" }, Startup.ProfilePolicyId);
    }
}
```

Sie können auch ein `[Authorize]` -Tag in Ihren Controllern verwenden, um die Ausführung einer bestimmten Richtlinie durchzusetzen, wenn der Benutzer nicht angemeldet ist. Öffnen Sie `Controllers\HomeController.cs`, und fügen Sie dem Anspruchscontroller das `[Authorize]`-Tag hinzu.  OWIN wählt die letzte Richtlinie, die ausgeführt werden soll, wenn das Autorisierungstag aufgerufen wird.

```C#
// Controllers\HomeController.cs

// You can use the Authorize decorator to execute a policy if the user is not already signed in the app.
[Authorize]
public ActionResult Claims()
{
  ...
```

Sie können auch OWIN zum Abmelden des Benutzers von der App verwenden. In `Controllers\AccountController.cs`:  

```C#
// Controllers\AccountController.cs

public void SignOut()
{
    // To sign out the user, you should issue an OpenIDConnect sign out request
    if (Request.IsAuthenticated)
    {
        IEnumerable<AuthenticationDescription> authTypes = HttpContext.GetOwinContext().Authentication.GetAuthenticationTypes();
        HttpContext.GetOwinContext().Authentication.SignOut(authTypes.Select(t => t.AuthenticationType).ToArray());
        Request.GetOwinContext().Authentication.GetAuthenticationTypes();
    }
}
```

## <a name="display-user-information"></a>Anzeigen der Benutzerinformationen
Bei der Authentifizierung von Benutzern mit OpenID Connect gibt Azure AD ein ID-Token an die App zurück, das **Ansprüche**enthält. Hierbei handelt es sich um Assertionen zum Benutzer. Mithilfe von Ansprüchen können Sie Ihre App personalisieren.  

Öffnen Sie die Datei `Controllers\HomeController.cs` . Auf die Benutzeransprüche können Sie in Ihren Controllern über das Sicherheitsprinzipalobjekt `ClaimsPrincipal.Current` zugreifen.

```C#
// Controllers\HomeController.cs

[Authorize]
public ActionResult Claims()
{
    Claim displayName = ClaimsPrincipal.Current.FindFirst(ClaimsPrincipal.Current.Identities.First().NameClaimType);
    ViewBag.DisplayName = displayName != null ? displayName.Value : string.Empty;
    return View();
}
```

Der Zugriff auf alle Ansprüche, die Ihre Anwendung erhält, erfolgt auf die gleiche Weise.  Eine Liste aller Ansprüche, die bei der App eingehen, steht Ihnen auf der Seite **Ansprüche** zur Verfügung.

## <a name="run-the-sample-app"></a>Ausführen der Beispiel-App
Zum Schluss erstellen Sie Ihre App und führen sie aus. Registrieren Sie sich für die App mit einer E-Mail-Adresse oder einem Benutzernamen. Melden Sie sich ab und unter demselben Benutzer wieder an. Bearbeiten Sie das Profil dieses Benutzers. Melden Sie sich ab, und registrieren Sie sich als anderer Benutzer. Beachten Sie, dass die Informationen auf der Registerkarte **Ansprüche** den Informationen entsprechen, die Sie in Ihren Richtlinien konfiguriert haben.

## <a name="add-social-idps"></a>Soziale Netzwerke als IDPs hinzufügen
Derzeit unterstützt die App nur die Registrierung und Anmeldung von Benutzern über **lokale Konten**. Dies sind in Ihrem B2C-Verzeichnis gespeicherte Konten, die einen Benutzernamen und ein Kennwort verwenden. Mit Azure AD B2C können Sie auch Unterstützung für andere **Identitätsanbieter** (IdPs) hinzufügen, ohne Ihren Code ändern zu müssen.

Um Ihrer App soziale Netzwerke als IdPs hinzuzufügen, befolgen Sie zunächst die detaillierten Anweisungen in diesen Artikeln. Sie müssen für jeden IdP, den Sie unterstützen möchten, eine Anwendung in dessen System registrieren und eine Client-ID abrufen.

* [Einrichten von Facebook als IDP](active-directory-b2c-setup-fb-app.md)
* [Einrichten von Google als IDP](active-directory-b2c-setup-goog-app.md)
* [Einrichten von Amazon als IDP](active-directory-b2c-setup-amzn-app.md)
* [Einrichten von LinkedIn als IDP](active-directory-b2c-setup-li-app.md)

Nachdem Sie Ihrem B2C-Verzeichnis die Identitätsanbieter hinzugefügt haben, müssen Sie jede der drei Richtlinien bearbeiten, um die neuen IdPs wie im [Artikel mit Richtlinienreferenzen](active-directory-b2c-reference-policies.md)beschrieben einzubeziehen. Nachdem Sie Ihre Richtlinien gespeichert haben, führen Sie die App erneut aus.  Die neuen IdPs sollten als Registrierungs- und Anmeldeoptionen in allen Benutzeroberflächen für Identitäten angezeigt werden.

Sie können mit Ihren Richtlinien experimentieren und die Auswirkungen auf die Beispiel-App beobachten. Fügen Sie IdPs hinzu, oder entfernen Sie sie, bearbeiten Sie Anwendungsansprüche, oder ändern Sie Registrierungsattribute. Experimentieren Sie so lange, bis Sie die Zusammenhänge zwischen Richtlinien, Authentifizierungsanforderungen und OWIN nachvollziehen können.

Als Referenz stellen wir das vollständige Beispiel (ohne Ihre Konfigurationswerte) [als ZIP-Datei](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet/archive/complete.zip)bereit. Sie können sie auch aus GitHub klonen:

```
git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet.git
```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->

