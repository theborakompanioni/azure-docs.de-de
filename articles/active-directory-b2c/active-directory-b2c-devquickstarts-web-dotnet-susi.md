<properties
	pageTitle="Azure Active Directory B2C-Vorschau | Microsoft Azure"
	description="Erfahren Sie, wie Sie eine Webanwendung mit Registrierung, Anmeldung und Kennwortzurücksetzung mithilfe von Azure Active Directory B2C erstellen."
	services="active-directory-b2c"
	documentationCenter=".net"
	authors="dstrockis"
	manager="msmbaldwin"
	editor=""/>

<tags
	ms.service="active-directory-b2c"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="04/07/2016"
	ms.author="dastrock"/>

# Azure AD B2C-Vorschau: Registrierung und Anmeldung In einer ASP.NET-Web-App

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Mit Azure Active Directory (Azure AD) B2C können Sie Ihren Web-Apps in wenigen Schritten leistungsstarke Self-Service-Features zur Identitätsverwaltung hinzufügen. In diesem Artikel erfahren Sie, wie eine ASP.NET-Web-App erstellt wird, die Benutzerregistrierung und -anmeldung sowie das Zurücksetzen von Kennwörtern umfasst. Die App umfasst Unterstützung für die Registrierung und Anmeldung mit einem Benutzernamen oder einer E-Mail-Adresse sowie über Konten für soziale Netzwerke wie Facebook und Google.

Dieses Tutorial unterscheidet sich von [unserem anderen .NET-Webtutorial](active-directory-b2c-devquickstarts-web-dotnet.md), da es eine [Richtlinie für Registrierung und Anmeldung](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy) verwendet, um Benutzern die Möglichkeit zur Registrierung und Anmeldung über eine Schaltfläche anstelle von zwei (eine für die Registrierung und eine für die Anmeldung) zu geben. Kurz gesagt ermöglicht eine Richtlinie für Registrierung oder Anmeldung Benutzern, sich mit einem vorhandenen Konto anzumelden, sofern ein solches vorhanden ist, oder ein neues zu erstellen, wenn dies die erste Verwendung der App ist.

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## Erstellen eines Azure AD B2C-Verzeichnisses

Bevor Sie Azure AD B2C verwenden können, müssen Sie ein Verzeichnis oder einen Mandanten erstellen. Ein Verzeichnis ist ein Container für all Ihre Benutzer, Apps, Gruppen usw. Wenn Sie noch keines verwenden, [erstellen Sie ein B2C-Verzeichnis](active-directory-b2c-get-started.md), bevor Sie die weiteren Schritte in diesem Leitfaden ausführen.

## Erstellen einer Anwendung

Als Nächstes müssen Sie in Ihrem B2C-Verzeichnis eine App erstellen. Dadurch werden Azure AD die Informationen bereitgestellt, die für die sichere Kommunikation mit Ihrer App erforderlich sind. Befolgen Sie zum Erstellen einer App [diese Anweisungen](active-directory-b2c-app-registration.md). Führen Sie folgende Schritte aus:

- Fügen Sie der Anwendung eine **Web-App/Web-API** hinzu.
- Geben Sie `https://localhost:44316/` als **Umleitungs-URI** ein. Dies ist die Standard-URL für dieses Codebeispiel.
- Notieren Sie die **Anwendungs-ID**, die Ihrer App zugewiesen ist. Sie benötigen sie später.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## Erstellen der Richtlinien

In Azure AD B2C wird jede Benutzeroberfläche durch eine [Richtlinie](active-directory-b2c-reference-policies.md) definiert. Dieses Codebeispiel enthält zwei Benutzeroberflächen, für die Identitäten relevant sind: **Anmelden, Registrieren** sowie **Kennwort zurücksetzen**. Sie müssen eine Richtlinie jedes Typs erstellen, wie im [Artikel zu Richtlinienreferenzen](active-directory-b2c-reference-policies.md) beschrieben. Beachten Sie beim Erstellen der beiden Richtlinien Folgendes:

- Wählen Sie auf dem Blatt für den Identitätsanbieter **User ID sign-up** (Registrierung mit Benutzer-ID) oder **Email sign-up** (E-Mail-Registrierung) aus.
- Wählen Sie den **Anzeigenamen** und andere Registrierungsattribute in der Richtlinie für Registrierung und Anmeldung aus.
- Wählen Sie den Anspruch **Anzeigename** als Anwendungsanspruch in den Richtlinien aus. Sie können auch andere Ansprüche auswählen.
- Notieren Sie sich die **Namen** der einzelnen Richtlinien nach ihrer Erstellung. Sie benötigen diese Richtliniennamen später.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nachdem Sie die beiden Richtlinien erstellt haben, können Sie Ihre App erstellen.

## Herunterladen des Codes und Konfigurieren der Authentifizierung

Der Code für dieses Beispiel wird [auf GitHub](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI) verwaltet. Zum Erstellen des Beispiels können Sie [das Projektgerüst als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/skeleton.zip). Sie können das Gerüst auch klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI.git
```

Das fertige Beispiel ist ebenfalls [als ZIP-Datei verfügbar](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/complete.zip) oder unter der Verzweigung `complete` im gleichen Repository enthalten.

Nachdem Sie den Beispielcode heruntergeladen haben, öffnen Sie zum Einstieg die Visual Studio-SLN-Datei.

Ihre App kommuniziert mit Azure AD B2C durch Senden von HTTP-Authentifizierungsanforderungen, in denen die Richtlinie, die ausgeführt werden soll, als Teil der Anforderung angegeben wird. Für .NET-Webanwendungen können Sie die OWIN-Bibliothek von Microsoft verwenden, um OpenID Connect-Authentifizierungsanfragen zu senden, Richtlinien auszuführen, die Sitzung des Benutzers zu verwalten usw.

Fügen Sie dem Projekt zunächst über die Visual Studio-Paket-Manager-Konsole die NuGet-Pakete der OWIN-Middleware hinzu.

```
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb
Intsall-Package System.IdentityModel.Tokens.Jwt
```

Öffnen Sie dann die Datei `web.config` aus dem Stammverzeichnis des Projekts, und geben Sie die Konfigurationswerte Ihrer App im Abschnitt `<appSettings>` ein. Ersetzen Sie dabei die Werte durch Ihre eigenen. Lassen Sie die Werte `ida:RedirectUri` und `ida:AadInstance` unverändert.

```
<configuration>
  <appSettings>

    ...

    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}{1}{2}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SusiPolicyId" value="b2c_1_susi" />
    <add key="ida:PasswordResetPolicyId" value="b2c_1_reset" />
  </appSettings>
...
```

[AZURE.INCLUDE [active-directory-b2c-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Fügen Sie als Nächstes dem Projekt eine OWIN-Startklasse mit dem Namen `Startup.cs` hinzu. Klicken Sie mit der rechten Maustaste auf das Projekt, wählen Sie **Hinzufügen** und **Neues Element** aus, und suchen Sie dann nach „OWIN“. Ändern Sie die Klassendeklaration in `public partial class Startup`. Wir haben für Sie einen Teil dieser Klasse in einer anderen Datei implementiert. Die OWIN-Middleware ruft beim Starten Ihrer Anwendung die Methode `Configuration(...)` auf. Rufen Sie in dieser Methode `ConfigureAuth(...)` auf, um die Authentifizierung für Ihre App einzurichten.

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

Öffnen Sie die Datei `App_Start\Startup.Auth.cs`, und implementieren Sie die Methode `ConfigureAuth(...)`. Die Parameter, die Sie in `OpenIdConnectAuthenticationOptions` angeben, dienen Ihrer App als Koordinaten für die Kommunikation mit Azure AD. Außerdem müssen Sie die Cookieauthentifizierung einrichten. Die Middleware OpenID Connect verwendet Cookies unter anderem für die Verwaltung von Benutzersitzungen.

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // The ACR claim is used to indicate which policy was executed
    public const string AcrClaimType = "http://schemas.microsoft.com/claims/authnclassreference";
    public const string PolicyKey = "b2cpolicy";
    public const string OIDCMetadataSuffix = "/.well-known/openid-configuration";

    // App config settings
    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    private static string redirectUri = ConfigurationManager.AppSettings["ida:RedirectUri"];

    // B2C policy identifiers
    public static string SusiPolicyId = ConfigurationManager.AppSettings["ida:SusiPolicyId"];
    public static string PasswordResetPolicyId = ConfigurationManager.AppSettings["ida:PasswordResetPolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseCookieAuthentication(new CookieAuthenticationOptions());

        OpenIdConnectAuthenticationOptions options = new OpenIdConnectAuthenticationOptions
        {
            // These are standard OpenID Connect parameters, with values pulled from web.config
            ClientId = clientId,
            RedirectUri = redirectUri,
            PostLogoutRedirectUri = redirectUri,
            Notifications = new OpenIdConnectAuthenticationNotifications
            { 
                AuthenticationFailed = AuthenticationFailed,
                RedirectToIdentityProvider = OnRedirectToIdentityProvider,
                SecurityTokenValidated = OnSecurityTokenValidated,
            },
            Scope = "openid",
            ResponseType = "id_token",

            // The PolicyConfigurationManager takes care of getting the correct Azure AD authentication
            // endpoints from the OpenID Connect metadata endpoint.  It is included in the PolicyAuthHelpers folder.
            ConfigurationManager = new PolicyConfigurationManager(
                String.Format(CultureInfo.InvariantCulture, aadInstance, tenant, "/v2.0", OIDCMetadataSuffix),
                new string[] { SusiPolicyId, PasswordResetPolicyId }),

            // This piece is optional - it is used for displaying the user's name in the navigation bar.
            TokenValidationParameters = new TokenValidationParameters
            {  
                NameClaimType = "name",
            },
        };

        app.UseOpenIdConnectAuthentication(options);
            
    }
    
...
```

## Senden von Authentifizierungsanforderungen an Azure AD
Ihre App ist nun ordnungsgemäß für die Kommunikation mit Azure AD B2C über das Authentifizierungsprotokoll OpenID Connect konfiguriert. OWIN kümmert sich um all die Details der Erstellung von Authentifizierungsnachrichten, der Überprüfung der Azure AD-Token und der Verwaltung der Benutzersitzung. Jetzt müssen nur noch die einzelnen Benutzerabläufe initiiert werden.

Wenn ein Benutzer in der Web-App eine der Schaltflächen **Anmelden** oder **Kennwort vergessen?** auswählt, wird die zugeordnete Aktion in `Controllers\AccountController.cs` aufgerufen. Mit integrierten OWIN-Methoden können Sie in jedem Fall die richtige Richtlinie auslösen:

```C#
// Controllers\AccountController.cs

public void Login()
{
    if (!Request.IsAuthenticated)
    {
        // To execute a policy, you simply need to trigger an OWIN challenge.
        // You can indicate which policy to use by adding it to the AuthenticationProperties using the PolicyKey provided.

        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties (
                new Dictionary<string, string> 
                { 
                    {Startup.PolicyKey, Startup.SusiPolicyId}
                })
            { 
                RedirectUri = "/", 
            }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}

public void ResetPassword()
{
    HttpContext.GetOwinContext().Authentication.Challenge(
        new AuthenticationProperties(
            new Dictionary<string, string>
            {
                {Startup.PolicyKey, Startup.PasswordResetPolicyId}
            })
        {
            RedirectUri = "/",
        }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
}
```

Sie können auch ein benutzerdefiniertes `PolicyAuthorize`-Tag in Ihren Controllern verwenden, um die Ausführung einer bestimmten Richtlinie durchzusetzen, wenn der Benutzer nicht angemeldet ist. Öffnen Sie `Controllers\HomeController.cs`, und fügen Sie dem Anspruchscontroller das `[PolicyAuthorize]`-Tag hinzu. Ersetzen Sie die Beispielrichtlinie durch eine eigene Richtlinie für Registrierung und Anmeldung.

```C#
// Controllers\HomeController.cs

// You can use the PolicyAuthorize decorator to execute a certain policy if the user is not already signed into the app.
[PolicyAuthorize(Policy = "b2c_1_susi")]
public ActionResult Claims()
{
  ...
```

Sie können auch OWIN zum Abmelden des Benutzers von der App verwenden. In `Controllers\AccountController.cs`:

```C#
// Controllers\AccountController.cs

public void Logout()
{
    // To sign out the user, you should issue an OpenIDConnect sign out request using the last policy that the user executed.
    // This is as easy as looking up the current value of the ACR claim, adding it to the AuthenticationProperties, and making an OWIN SignOut call.

    HttpContext.GetOwinContext().Authentication.SignOut(
        new AuthenticationProperties(
            new Dictionary<string, string> 
            { 
                {Startup.PolicyKey, ClaimsPrincipal.Current.FindFirst(Startup.AcrClaimType).Value}
            }), OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
}
```

Standardmäßig sendet OWIN die in `AuthenticationProperties` angegebenen Richtlinien nicht an Azure AD. Sie können jedoch die von OWIN in der `RedirectToIdentityProvider`-Benachrichtigung generierten Anforderungen bearbeiten. Verwenden Sie diese Benachrichtigung in `App_Start\Startup.Auth.cs`, um den richtigen Endpunkt für jede Richtlinie aus den Metadaten der Richtlinie abzurufen. Dadurch wird sichergestellt, dass für jede Richtlinie, die Ihre App ausführen möchte, die richtige Anforderung an Azure AD gesendet wird.

```C#
// App_Start\Startup.Auth.cs

private async Task OnRedirectToIdentityProvider(RedirectToIdentityProviderNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    PolicyConfigurationManager mgr = notification.Options.ConfigurationManager as PolicyConfigurationManager;
    if (notification.ProtocolMessage.RequestType == OpenIdConnectRequestType.LogoutRequest)
    {
        OpenIdConnectConfiguration config = await mgr.GetConfigurationByPolicyAsync(CancellationToken.None, notification.OwinContext.Authentication.AuthenticationResponseRevoke.Properties.Dictionary[Startup.PolicyKey]);
        notification.ProtocolMessage.IssuerAddress = config.EndSessionEndpoint;
    }
    else
    {
        OpenIdConnectConfiguration config = await mgr.GetConfigurationByPolicyAsync(CancellationToken.None, notification.OwinContext.Authentication.AuthenticationResponseChallenge.Properties.Dictionary[Startup.PolicyKey]);
        notification.ProtocolMessage.IssuerAddress = config.AuthorizationEndpoint;
    }
}
```

## Anzeigen der Benutzerinformationen
Bei der Authentifizierung von Benutzern mit OpenID Connect gibt Azure AD ein ID-Token an die App zurück, das **Ansprüche** enthält. Hierbei handelt es sich um Assertionen zum Benutzer. Mithilfe von Ansprüchen können Sie Ihre App personalisieren.

Öffnen Sie die Datei `Controllers\HomeController.cs`. Auf die Benutzeransprüche können Sie in Ihren Controllern über das Sicherheitsprinzipalobjekt `ClaimsPrincipal.Current` zugreifen.

```C#
// Controllers\HomeController.cs

[PolicyAuthorize(Policy = "b2c_1_susi")]
public ActionResult Claims()
{
	Claim displayName = ClaimsPrincipal.Current.FindFirst(ClaimsPrincipal.Current.Identities.First().NameClaimType);
	ViewBag.DisplayName = displayName != null ? displayName.Value : string.Empty;
    return View();
}
```

Der Zugriff auf alle Ansprüche, die Ihre Anwendung erhält, erfolgt auf die gleiche Weise. Eine Liste aller Ansprüche, die bei der App eingehen, steht Ihnen auf der Seite **Ansprüche** zur Verfügung.

## Ausführen der Beispiel-App

Zum Schluss erstellen Sie Ihre App und führen sie aus. Registrieren Sie sich für die App mit einer E-Mail-Adresse oder einem Benutzernamen. Melden Sie sich ab und unter demselben Benutzer wieder an. Bearbeiten Sie das Profil dieses Benutzers. Melden Sie sich ab, und registrieren Sie sich als anderer Benutzer. Beachten Sie, dass die Informationen auf der Registerkarte **Ansprüche** den Informationen entsprechen, die Sie in Ihren Richtlinien konfiguriert haben.

## Soziale Netzwerke als IDPs hinzufügen

Derzeit unterstützt die App nur die Registrierung und Anmeldung von Benutzern über **lokale Konten**. Dies sind in Ihrem B2C-Verzeichnis gespeicherte Konten, die einen Benutzernamen und ein Kennwort verwenden. Mit Azure AD B2C können Sie auch Unterstützung für andere **Identitätsanbieter** (IdPs) hinzufügen, ohne Ihren Code ändern zu müssen.

Um Ihrer App soziale Netzwerke als IdPs hinzuzufügen, befolgen Sie zunächst die detaillierten Anweisungen in diesen Artikeln. Sie müssen für jeden IdP, den Sie unterstützen möchten, eine Anwendung in dessen System registrieren und eine Client-ID abrufen.

- [Einrichten von Facebook als IDP](active-directory-b2c-setup-fb-app.md)
- [Einrichten von Google als IDP](active-directory-b2c-setup-goog-app.md)
- [Einrichten von Amazon als IDP](active-directory-b2c-setup-amzn-app.md)
- [Einrichten von LinkedIn als IDP](active-directory-b2c-setup-li-app.md)

Nachdem Sie Ihrem B2C-Verzeichnis die Identitätsanbieter hinzugefügt haben, müssen Sie jede der drei Richtlinien bearbeiten, um die neuen IdPs wie im [Artikel mit Richtlinienreferenzen](active-directory-b2c-reference-policies.md) beschrieben einzubeziehen. Nachdem Sie Ihre Richtlinien gespeichert haben, führen Sie die App erneut aus. Die neuen IdPs sollten als Registrierungs- und Anmeldeoptionen in allen Benutzeroberflächen für Identitäten angezeigt werden.

Sie können mit Ihren Richtlinien experimentieren und die Auswirkungen auf die Beispiel-App beobachten. Fügen Sie IdPs hinzu, oder entfernen Sie sie, bearbeiten Sie Anwendungsansprüche, oder ändern Sie Registrierungsattribute. Experimentieren Sie so lange, bis Sie die Zusammenhänge zwischen Richtlinien, Authentifizierungsanforderungen und OWIN nachvollziehen können.

Als Referenz stellen wir das vollständige Beispiel (ohne Ihre Konfigurationswerte) [als ZIP-Datei](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/complete.zip) bereit. Sie können sie auch aus GitHub klonen:

```
git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI.git
```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->

<!---HONumber=AcomDC_0525_2016-->