---
title: Azure Active Directory B2C | Microsoft Docs
description: "Erfahren Sie, wie Sie eine Webanwendung mit Registrierung/Anmeldung, Profilbearbeitung und Kennwortzurücksetzung mithilfe von Azure Active Directory B2C erstellen."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: krassk
editor: 
ms.assetid: 30261336-d7a5-4a6d-8c1a-7943ad76ed25
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/17/2017
ms.author: parakhj
translationtype: Human Translation
ms.sourcegitcommit: f6006d5e83ad74f386ca23fe52879bfbc9394c0f
ms.openlocfilehash: 87b8b91fc5970bd127dfdc47e24d99a19471aa8c
ms.lasthandoff: 05/03/2017


---
# <a name="azure-ad-b2c-sign-up--sign-in-in-a-aspnet-web-app"></a>Azure AD B2C: Registrierung und Anmeldung in einer ASP.NET-Web-App

Mit Azure AD B2C können Sie Ihren Web-Apps leistungsstarke Features zur Identitätsverwaltung hinzufügen. In diesem Artikel erfahren Sie, wie eine ASP.NET-Web-App erstellt wird, die Benutzerregistrierung/-anmeldung, Profilbearbeitung sowie das Zurücksetzen von Kennwörtern umfasst.

## <a name="create-an-azure-ad-b2c-directory"></a>Erstellen eines Azure AD B2C-Verzeichnisses

Bevor Sie Azure AD B2C verwenden können, müssen Sie ein Verzeichnis oder einen Mandanten erstellen. Ein Verzeichnis ist ein Container für all Ihre Benutzer, Apps, Gruppen usw. Wenn Sie noch keines verwenden, sollten Sie [ein B2C-Verzeichnis erstellen](active-directory-b2c-get-started.md) , bevor Sie die weiteren Schritte in diesem Leitfaden ausführen.

## <a name="create-an-application"></a>Erstellen einer Anwendung

Als Nächstes müssen Sie in Ihrem B2C-Verzeichnis eine Web-App erstellen. Dadurch werden Azure AD die Informationen bereitgestellt, die für die sichere Kommunikation mit Ihrer App erforderlich sind. Befolgen Sie zum Erstellen einer App [diese Anweisungen](active-directory-b2c-app-registration.md). Führen Sie folgende Schritte aus:

* Fügen Sie der Anwendung eine **Web-App/Web-API** hinzu.
* Geben Sie `https://localhost:44316/` als **Umleitungs-URI**ein. Dies ist die Standard-URL für dieses Codebeispiel.
* Notieren Sie die **Anwendungs-ID** , die Ihrer App zugewiesen ist.  Sie benötigen sie später.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Erstellen der Richtlinien

In Azure AD B2C wird jede Benutzererfahrung durch eine [Richtlinie](active-directory-b2c-reference-policies.md)definiert. Dieses Codebeispiel enthält drei Benutzeroberflächen, für die Identitäten relevant sind: **Anmelden, Registrieren**, **Profilbearbeitung** sowie **Kennwortzurücksetzung**.  Sie müssen eine Richtlinie jedes Typs erstellen, wie im [Richtlinienreferenzartikel](active-directory-b2c-reference-policies.md)beschrieben. Beachten Sie beim Erstellen der beiden Richtlinien Folgendes:

* Wählen Sie auf dem Blatt für den Identitätsanbieter **Registrierung mit Benutzer-ID** oder **E-Mail-Registrierung** aus.
* Wählen Sie den **Anzeigenamen** und andere Registrierungsattribute in der Richtlinie für Registrierung und Anmeldung aus.
* Wählen Sie den Anspruch **Anzeigename** als Anwendungsanspruch in den Richtlinien aus. Sie können auch andere Ansprüche auswählen.
* Notieren Sie sich die **Namen** der einzelnen Richtlinien nach ihrer Erstellung. Sie benötigen diese Richtliniennamen später.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nachdem Sie die Richtlinien erstellt haben, können Sie Ihre App erstellen.

## <a name="download-the-code"></a>Herunterladen des Codes

Der Code für dieses Tutorial wird auf [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi) verwaltet. Sie können das Beispiel klonen, indem Sie Folgendes ausführen:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Nachdem Sie den Beispielcode heruntergeladen haben, öffnen Sie zum Einstieg die Visual Studio-SLN-Datei. Die Projektmappe enthält jetzt zwei Projekte: `TaskWebApp` und `TaskService`. `TaskWebApp` ist die MVC-Webanwendung, mit der der Benutzer interagiert. `TaskService` ist die Back-End-Web-API der App, in der die Aufgabenlisten der einzelnen Benutzer gespeichert werden. In diesem Artikel wird nur die Anwendung `TaskWebApp` beschrieben. Informationen zum Erstellen einer `TaskService` mit Azure AD B2C finden Sie in unserem [.NET-Web-App-Tutorial](active-directory-b2c-devquickstarts-api-dotnet.md).

### <a name="update-the-azure-ad-b2c-configuration"></a>Aktualisieren der Azure AD B2C-Konfiguration

Unser Beispiel ist so konfiguriert, dass die Richtlinien und die Client-ID unseres Demomandanten verwendet werden. Wenn Sie Ihren eigenen Mandanten verwenden möchten, müssen Sie zum `web.config` im Projekt `TaskWebApp` öffnen und die Werte wie folgt ersetzen:

* `ida:Tenant` durch Ihren Mandantennamen
* `ida:ClientId` durch Ihre Anwendungs-ID der Web-App
* `ida:ClientSecret` durch Ihren geheimen Schlüssel der Web-App
* `ida:SignUpSignInPolicyId` durch Ihren Namen für die Richtlinie zur Registrierung bzw. Anmeldung
* `ida:EditProfilePolicyId` durch den Namen der Richtlinie für „Profil bearbeiten“
* `ida:ResetPasswordPolicyId` durch den Namen der Richtlinie für „Kennwort zurücksetzen“

## <a name="add-authentication-support"></a>Hinzufügen von Authentifizierungssupport

Sie können nun Ihre App zur Verwendung von Azure AD B2C konfigurieren. Ihre App kommuniziert mit Azure AD B2C, indem OpenID Connect-Authentifizierungsanforderungen gesendet werden. Die Anforderungen bestimmen durch Angabe der Richtlinie über die Benutzeroberfläche, die Ihre App ausführen möchte. Sie können die OWIN-Bibliothek von Microsoft verwenden, um derartige Anfragen zu senden, Richtlinien auszuführen, Benutzersitzungen zu verwalten usw.

### <a name="install-owin"></a>Installieren von OWIN

Fügen Sie dem Projekt zunächst über die Visual Studio-Paket-Manager-Konsole die NuGet-Pakete der OWIN-Middleware hinzu.

```Console
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

### <a name="add-an-owin-startup-class"></a>Hinzufügen einer OWIN-Startklasse

Fügen Sie der API eine OWIN-Startklasse mit dem Namen `Startup.cs` hinzu.  Klicken Sie mit der rechten Maustaste auf das Projekt, wählen Sie **Hinzufügen** und **Neues Element** aus, und suchen Sie dann nach „OWIN“. Die OWIN-Middleware ruft beim Starten Ihrer Anwendung die Methode `Configuration(…)` auf.

In unserem Beispiel haben wir die Klassendeklaration in `public partial class Startup` geändert und den anderen Teil der Klasse in `App_Start\Startup.Auth.cs` implementiert. Innerhalb der `Configuration`-Methode haben wir einen Aufruf von `ConfigureAuth` hinzugefügt, der in `Startup.Auth.cs` definiert ist. Nach den Änderungen sieht `Startup.cs` wie folgt aus:

```CSharp
// Startup.cs

public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        // ConfigureAuth defined in other part of the class
        ConfigureAuth(app);
    }
}
```

### <a name="configure-the-authentication-middleware"></a>Konfigurieren der Authentifizierungsmiddleware

Öffnen Sie die Datei `App_Start\Startup.Auth.cs`, und implementieren Sie die Methode `ConfigureAuth(...)`. Die Parameter, die Sie in `OpenIdConnectAuthenticationOptions` angeben, dienen Ihrer App als Koordinaten für die Kommunikation mit Azure AD B2C. Wenn Sie bestimmte Parameter nicht angeben, wird der Standardwert verwendet. Beispielsweise wird der `ResponseType` im Beispiel nicht angegeben, weshalb der Standardwert `code id_token` in jeder ausgehenden Anforderung an Azure AD B2C verwendet wird.

Außerdem müssen Sie die Cookieauthentifizierung einrichten. Die Middleware OpenID Connect verwendet Cookies unter anderem für die Verwaltung von Benutzersitzungen.

```CSharp
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // Initialize variables ...

    // Configure the OWIN middleware
    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseCookieAuthentication(new CookieAuthenticationOptions());
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseOpenIdConnectAuthentication(
            new OpenIdConnectAuthenticationOptions
            {
                // Generate the metadata address using the tenant and policy information
                MetadataAddress = String.Format(AadInstance, Tenant, DefaultPolicy),

                // These are standard OpenID Connect parameters, with values pulled from web.config
                ClientId = ClientId,
                RedirectUri = RedirectUri,
                PostLogoutRedirectUri = RedirectUri,

                // Specify the callbacks for each type of notifications
                Notifications = new OpenIdConnectAuthenticationNotifications
                {
                    RedirectToIdentityProvider = OnRedirectToIdentityProvider,
                    AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    AuthenticationFailed = OnAuthenticationFailed,
                },

                // Specify the claims to validate
                TokenValidationParameters = new TokenValidationParameters
                {
                    NameClaimType = "name"
                },

                // Specify the scope by appending all of the scopes requested into one string (seperated by a blank space)
                Scope = $"{OpenIdConnectScopes.OpenId} {ReadTasksScope} {WriteTasksScope}"
            }
        );
    }

    // Implement the "Notification" methods...
}
```

In `OpenIdConnectAuthenticationOptions` oben wird eine Reihe von Rückruffunktionen für bestimmte Benachrichtigungen definiert, die von der OpenID Connect-Middleware empfangen werden. Diese Verhaltensweisen werden mit einem `OpenIdConnectAuthenticationNotifications`-Objekt definiert und in der `Notifications`-Variable gespeichert. In diesem Beispiel werden drei verschiedene vom Ereignis abhängige Rückrufe definiert.

#### <a name="using-different-policies"></a>Verwenden von unterschiedlichen Richtlinien

Die `RedirectToIdentityProvider`-Benachrichtigung wird ausgelöst, wenn eine Anforderung an Azure AD B2C vorgenommen wird. In der Rückruffunktion `OnRedirectToIdentityProvider` wird im ausgehenden Aufruf überprüft, ob eine andere Richtlinie verwendet werden soll. Um ein Kennwort zurückzusetzen oder ein Profil zu bearbeiten, müssen Sie die entsprechende Richtlinie verwenden, z.B. die Richtlinie für die Kennwortzurücksetzung statt der standardmäßigen Registrierungs- oder Anmelderichtlinie.

Wenn ein Benutzer in diesem Beispiel das Kennwort zurücksetzen oder das Profil bearbeiten möchte, fügen wir die Richtlinie hinzu, die im Zusammenhang mit OWIN verwendet werden soll. Dies kann auf folgende Weise erreicht werden:

```CSharp
    // Let the middleware know you are trying to use the edit profile policy
    HttpContext.GetOwinContext().Set("Policy", EditProfilePolicyId);
```

Zudem kann die Rückruffunktion `OnRedirectToIdentityProvider` auf folgende Weise implementiert werden:

```CSharp
/*
*  On each call to Azure AD B2C, check if a policy (e.g. the profile edit or password reset policy) has been specified in the OWIN context.
*  If so, use that policy when making the call. Also, don't request a code (since it won't be needed).
*/
private Task OnRedirectToIdentityProvider(RedirectToIdentityProviderNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    var policy = notification.OwinContext.Get<string>("Policy");

    if (!string.IsNullOrEmpty(policy) && !policy.Equals(DefaultPolicy))
    {
        notification.ProtocolMessage.Scope = OpenIdConnectScopes.OpenId;
        notification.ProtocolMessage.ResponseType = OpenIdConnectResponseTypes.IdToken;
        notification.ProtocolMessage.IssuerAddress = notification.ProtocolMessage.IssuerAddress.Replace(DefaultPolicy, policy);
    }

    return Task.FromResult(0);
}
```

#### <a name="handling-authorization-codes"></a>Verarbeiten von Autorisierungscodes

Die `AuthorizationCodeReceived`-Benachrichtigung wird ausgelöst, wenn ein Autorisierungscode empfangen wird. Die OpenID Connect-Middleware unterstützt nicht den Austausch von Codes mit Zugriffstokens. Sie können den Code manuell mit dem Token in einer Rückruffunktion austauschen. Weitere Informationen zur Vorgehensweise finden Sie in der [Dokumentation](active-directory-b2c-devquickstarts-web-api-dotnet.md).

#### <a name="handling-errors"></a>Behandeln von Fehlern

Die `AuthenticationFailed`-Benachrichtigung wird ausgelöst, wenn die Authentifizierung fehlschlägt. Bei der Rückrufmethode können Sie Fehler auf beliebige Weise behandeln. Sie sollten jedoch eine Überprüfung für den Fehlercode `AADB2C90118` hinzufügen. Während der Ausführung der Registrierungs- oder Anmelderichtlinie hat der Benutzer die Möglichkeit, auf den Link **Haben Sie Ihr Kennwort vergessen?** zu klicken. In diesem Fall sendet Azure AD B2C Ihrer App den Fehlercode zu, der darauf hinweist, dass Ihre App stattdessen eine Anforderung mithilfe der Richtlinie zur Kennwortzurücksetzung tätigen sollte.

```CSharp
/*
* Catch any failures received by the authentication middleware and handle appropriately
*/
private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    notification.HandleResponse();

    // Handle the error code that Azure AD B2C throws when trying to reset a password from the login page
    // because password reset is not supported by a "sign-up or sign-in policy"
    if (notification.ProtocolMessage.ErrorDescription != null && notification.ProtocolMessage.ErrorDescription.Contains("AADB2C90118"))
    {
        // If the user clicked the reset password link, redirect to the reset password route
        notification.Response.Redirect("/Account/ResetPassword");
    }
    else if (notification.Exception.Message == "access_denied")
    {
        notification.Response.Redirect("/");
    }
    else
    {
        notification.Response.Redirect("/Home/Error?message=" + notification.Exception.Message);
    }

    return Task.FromResult(0);
}
```

## <a name="send-authentication-requests-to-azure-ad"></a>Senden von Authentifizierungsanforderungen an Azure AD

Ihre App ist nun ordnungsgemäß für die Kommunikation mit Azure AD B2C über das Authentifizierungsprotokoll OpenID Connect konfiguriert. OWIN hat alle Details zur Erstellung von Authentifizierungsnachrichten, zur Überprüfung der Azure AD B2C-Tokens und der Verwaltung von Benutzersitzungen übernommen. Jetzt müssen nur noch die einzelnen Benutzerabläufe initiiert werden.

Wenn ein Benutzer in der Web-App eine der Schaltflächen **Anmelden/Registrieren**, **Profil bearbeiten** oder **Kennwort zurücksetzen** auswählt, wird die zugeordnete Aktion in `Controllers\AccountController.cs` aufgerufen:

```CSharp
// Controllers\AccountController.cs

/*
*  Called when requesting to sign up or sign in
*/
public void SignUpSignIn()
{
    // Use the default policy to process the sign up / sign in flow
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge();
        return;
    }

    Response.Redirect("/");
}

/*
*  Called when requesting to edit a profile
*/
public void EditProfile()
{
    if (Request.IsAuthenticated)
    {
        // Let the middleware know you are trying to use the edit profile policy (see OnRedirectToIdentityProvider in Startup.Auth.cs)
        HttpContext.GetOwinContext().Set("Policy", Startup.EditProfilePolicyId);

        // Set the page to redirect to after editing the profile
        var authenticationProperties = new AuthenticationProperties { RedirectUri = "/" };
        HttpContext.GetOwinContext().Authentication.Challenge(authenticationProperties);

        return;
    }

    Response.Redirect("/");

}

/*
*  Called when requesting to reset a password
*/
public void ResetPassword()
{
    // Let the middleware know you are trying to use the reset password policy (see OnRedirectToIdentityProvider in Startup.Auth.cs)
    HttpContext.GetOwinContext().Set("Policy", Startup.ResetPasswordPolicyId);

    // Set the page to redirect to after changing passwords
    var authenticationProperties = new AuthenticationProperties { RedirectUri = "/" };
    HttpContext.GetOwinContext().Authentication.Challenge(authenticationProperties);

    return;
}
```

Sie können auch OWIN zum Abmelden des Benutzers von der App verwenden. `Controllers\AccountController.cs` sieht wie folgt aus:

```CSharp
// Controllers\AccountController.cs

/*
*  Called when requesting to sign out
*/
public void SignOut()
{
    // To sign out the user, you should issue an OpenIDConnect sign out request.
    if (Request.IsAuthenticated)
    {
        IEnumerable<AuthenticationDescription> authTypes = HttpContext.GetOwinContext().Authentication.GetAuthenticationTypes();
        HttpContext.GetOwinContext().Authentication.SignOut(authTypes.Select(t => t.AuthenticationType).ToArray());
        Request.GetOwinContext().Authentication.GetAuthenticationTypes();
    }
}
```

Zusätzlich zum expliziten Aufrufen einer Richtlinie können Sie ein `[Authorize]` -Tag in Ihren Controllern verwenden, mit dem eine Richtlinie ausgeführt wird, wenn der Benutzer nicht angemeldet ist. Öffnen Sie `Controllers\HomeController.cs`, und fügen Sie dem Anspruchscontroller das `[Authorize]`-Tag hinzu.  OWIN wählt bei der Nutzung des `[Authorize]` -Tags die zuletzt konfigurierte Richtlinie aus.

```CSharp
// Controllers\HomeController.cs

// You can use the Authorize decorator to execute a certain policy if the user is not already signed into the app.
[Authorize]
public ActionResult Claims()
{
  ...
```

## <a name="display-user-information"></a>Anzeigen der Benutzerinformationen

Bei der Authentifizierung von Benutzern mit OpenID Connect gibt Azure AD B2C ein ID-Token an die App zurück, das **Ansprüche**enthält. Hierbei handelt es sich um Assertionen zum Benutzer. Mithilfe von Ansprüchen können Sie Ihre App personalisieren.

Öffnen Sie die Datei `Controllers\HomeController.cs` . Auf die Benutzeransprüche können Sie in Ihren Controllern über das Sicherheitsprinzipalobjekt `ClaimsPrincipal.Current` zugreifen.

```CSharp
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

Zum Schluss erstellen Sie Ihre App und führen sie aus. Registrieren Sie sich für die App mit einer E-Mail-Adresse oder einem Benutzernamen. Melden Sie sich ab und unter demselben Benutzer wieder an. Bearbeiten Sie das Profil, oder setzen Sie das Kennwort zurück. Melden Sie sich ab, und registrieren Sie sich als anderer Benutzer. Beachten Sie, dass die Informationen auf der Registerkarte **Ansprüche** den Informationen entsprechen, die Sie in Ihren Richtlinien konfiguriert haben.

## <a name="add-social-idps"></a>Soziale Netzwerke als IDPs hinzufügen

Derzeit unterstützt die App nur die Registrierung und Anmeldung von Benutzern über **lokale Konten**. Dies sind in Ihrem B2C-Verzeichnis gespeicherte Konten, die einen Benutzernamen und ein Kennwort verwenden. Mit Azure AD B2C können Sie auch Unterstützung für andere **Identitätsanbieter** (IdPs) hinzufügen, ohne Ihren Code ändern zu müssen.

Um Ihrer App soziale Netzwerke als IdPs hinzuzufügen, befolgen Sie zunächst die detaillierten Anweisungen in diesen Artikeln. Sie müssen für jeden IdP, den Sie unterstützen möchten, eine Anwendung in dessen System registrieren und eine Client-ID abrufen.

* [Einrichten von Facebook als IDP](active-directory-b2c-setup-fb-app.md)
* [Einrichten von Google als IDP](active-directory-b2c-setup-goog-app.md)
* [Einrichten von Amazon als IDP](active-directory-b2c-setup-amzn-app.md)
* [Einrichten von LinkedIn als IDP](active-directory-b2c-setup-li-app.md)

Nachdem Sie Ihrem B2C-Verzeichnis die Identitätsanbieter hinzugefügt haben, müssen Sie jede der drei Richtlinien bearbeiten, um die neuen IdPs wie im [Artikel mit Richtlinienreferenzen](active-directory-b2c-reference-policies.md)beschrieben einzubeziehen. Nachdem Sie Ihre Richtlinien gespeichert haben, führen Sie die App erneut aus.  Die neuen IdPs sollten als Registrierungs- und Anmeldeoptionen in allen Benutzeroberflächen für Identitäten angezeigt werden.

Sie können mit Ihren Richtlinien experimentieren und die Auswirkungen auf die Beispiel-App beobachten. Fügen Sie IdPs hinzu, oder entfernen Sie sie, bearbeiten Sie Anwendungsansprüche, oder ändern Sie Registrierungsattribute. Experimentieren Sie so lange, bis Sie die Zusammenhänge zwischen Richtlinien, Authentifizierungsanforderungen und OWIN nachvollziehen können.

