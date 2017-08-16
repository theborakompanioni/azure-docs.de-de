---
title: Azure Active Directory B2C | Microsoft Docs
description: "Erfahren Sie, wie Sie eine Webanwendung mit Registrierung/Anmeldung, Profilbearbeitung und Kennwortzurücksetzung mithilfe von Azure Active Directory B2C erstellen."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: krassk
editor: barbaraselden
ms.assetid: 30261336-d7a5-4a6d-8c1a-7943ad76ed25
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/17/2017
ms.author: parakhj
ms.translationtype: HT
ms.sourcegitcommit: f5c887487ab74934cb65f9f3fa512baeb5dcaf2f
ms.openlocfilehash: 14736524bf3c6d299838d8e3dc8b18db117d1041
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---
# <a name="create-an-aspnet-web-app-with-azure-active-directory-b2c-sign-up-sign-in-profile-edit-and-password-reset"></a>Erstellen Sie mithilfe von Azure Active Directory B2C eine ASP.NET-Web-App mit Registrierung, Anmeldung, Profilbearbeitung und Kennwortzurücksetzung.

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

> [!div class="checklist"]
> * Hinzufügen von Azure AD B2C-Identitätsfunktionen zu Ihrer Web-App
> * Registrieren Ihrer Web-App in Ihrem Azure AD B2C-Verzeichnis
> * Erstellen einer Benutzerregistrierung/-anmeldung, Profilbearbeitung und Kennwortzurücksetzungs-Richtlinie für Ihre Web-App

## <a name="prerequisites"></a>Voraussetzungen

- Sie müssen eine Verbindung Ihres B2C-Mandanten mit einem Azure-Konto herstellen. Sie können [hier](https://azure.microsoft.com/en-us/) ein kostenloses Azure-Konto erstellen.
- Sie benötigen [Microsoft Visual Studio](https://www.visualstudio.com/) oder eine ähnliche Anwendung zum Anzeigen und Ändern des Beispielcodes.

## <a name="create-an-azure-ad-b2c-directory"></a>Erstellen eines Azure AD B2C-Verzeichnisses

Bevor Sie Azure AD B2C verwenden können, müssen Sie ein Verzeichnis oder einen Mandanten erstellen. Ein Verzeichnis ist ein Container für alle Benutzer, Apps, Gruppen usw. Wenn Sie noch keines verwenden, sollten Sie ein B2C-Verzeichnis erstellen, bevor Sie die weiteren Schritte in diesem Leitfaden ausführen.

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

> [!NOTE]
> 
> Sie müssen eine Verbindung Ihres B2C-Mandanten mit Ihrem Azure-Abonnement herstellen. Wählen Sie nach **Erstellen** die Option **Vorhandenen Azure AD B2C-Mandanten mit meinem Azure-Abonnement verknüpfen** und dann in der Dropdownliste **Azure AD B2C-Mandant** den Mandanten, den Sie zuordnen möchten.

## <a name="create-and-register-an-application"></a>Erstellen und Registrieren einer Anwendung

Als Nächstes müssen Sie die App in Ihrem B2C-Verzeichnis erstellen und registrieren. So werden Informationen bereitgestellt, die Azure AD B2C für die sichere Kommunikation mit Ihrer App benötigt. 

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

Dann enthalten Ihre Anwendungseinstellungen eine API und eine native Anwendung.

## <a name="create-policies-on-your-b2c-tenant"></a>Erstellen von Richtlinien für Ihren B2C-Mandanten

In Azure AD B2C wird jede Benutzererfahrung durch eine [Richtlinie](active-directory-b2c-reference-policies.md)definiert. Dieses Codebeispiel enthält drei Benutzeroberflächen, für die Identitäten relevant sind: **Anmelden, Registrieren**, **Profilbearbeitung** sowie **Kennwortzurücksetzung**.  Sie müssen eine Richtlinie jedes Typs erstellen, wie im [Richtlinienreferenzartikel](active-directory-b2c-reference-policies.md)beschrieben. Achten Sie darauf, für jede Richtlinie das Attribut „Anzeigename“ oder einen Anspruch auszuwählen und den Namen der Richtlinie für die spätere Verwendung zu kopieren.

### <a name="add-your-identity-providers"></a>Hinzufügen Ihres Identitätsanbieters

Wählen Sie aus Ihren Einstellungen **Identitätsanbieter** und dann die Benutzer-ID- oder E-Mail-Registrierung.

### <a name="create-a-sign-up-and-sign-in-policy"></a>Erstellen einer Registrierungs- und Anmelderichtlinie

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

### <a name="create-a-profile-editing-policy"></a>Erstellen einer Richtlinie für die Profilbearbeitung

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

### <a name="create-a-password-reset-policy"></a>Erstellen einer Richtlinie zur Kennwortrücksetzung

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

Nachdem Sie die Richtlinien erstellt haben, können Sie Ihre App erstellen.

## <a name="download-the-sample-code"></a>Herunterladen des Beispielcodes

Der Code für dieses Tutorial wird auf [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi) verwaltet. Sie können das Beispiel klonen, indem Sie Folgendes ausführen:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Nachdem Sie den Beispielcode heruntergeladen haben, öffnen Sie zum Einstieg die Visual Studio-SLN-Datei. Die Projektmappe enthält jetzt zwei Projekte: `TaskWebApp` und `TaskService`. `TaskWebApp` ist die MVC-Webanwendung, mit der der Benutzer interagiert. `TaskService` ist die Back-End-Web-API der App, in der die Aufgabenlisten der einzelnen Benutzer gespeichert werden. In diesem Artikel wird nur die Anwendung `TaskWebApp` beschrieben. Informationen zum Erstellen einer `TaskService` mit Azure AD B2C finden Sie in unserem [.NET-Web-App-Tutorial](active-directory-b2c-devquickstarts-api-dotnet.md).

## <a name="update-code-to-use-your-tenant-and-policies"></a>Aktualisieren von Code zur Verwendung Ihres Mandanten und Ihrer Richtlinien

Unser Beispiel ist so konfiguriert, dass die Richtlinien und die Client-ID unseres Demomandanten verwendet werden. Um eine Verbindung mit Ihrem eigenen Mandanten herzustellen, müssen Sie `web.config` im `TaskWebApp`-Projekt öffnen und die folgenden Werte ersetzen:

* `ida:Tenant` durch Ihren Mandantennamen
* `ida:ClientId` durch Ihre Anwendungs-ID der Web-App
* `ida:ClientSecret` durch Ihren geheimen Schlüssel der Web-App
* `ida:SignUpSignInPolicyId` durch Ihren Namen für die Richtlinie zur Registrierung bzw. Anmeldung
* `ida:EditProfilePolicyId` durch den Namen der Richtlinie für „Profil bearbeiten“
* `ida:ResetPasswordPolicyId` durch den Namen der Richtlinie für „Kennwort zurücksetzen“

## <a name="launch-the-app"></a>Starten der App
Starten Sie die App aus Visual Studio heraus. Navigieren Sie zur Registerkarte „Aufgabenliste“, und beachten Sie, dass der URl wie folgt lautet: „https://login.microsoftonline.com/*Name_Ihres_Mandanten*/oauth2/v2.0/authorize?p=*Ihre_Registrierungsrichtlinie*&client_id=*Ihre_Client_ID*...“

Registrieren Sie sich mit Ihrer E-Mail-Adresse oder einem Benutzernamen für die App. Melden Sie sich ab und dann erneut an, und bearbeiten Sie das Profil, oder setzen Sie das Kennwort zurück. Melden Sie sich ab, und melden Sie sich als ein anderer Benutzer an. 

## <a name="add-social-idps"></a>Soziale Netzwerke als IDPs hinzufügen

Die App unterstützt derzeit nur Benutzerregistrierung und -anmeldung mit **lokalen Konten**, d.h. in Ihrem B2C-Verzeichnis gespeicherte Konten, die einen Benutzernamen und ein Kennwort verwenden. Mit Azure AD B2C können Sie auch Unterstützung für andere **Identitätsanbieter** (IdPs) hinzufügen, ohne Ihren Code ändern zu müssen.

Um Ihrer App soziale Netzwerke als IdPs hinzuzufügen, befolgen Sie zunächst die detaillierten Anweisungen in diesen Artikeln. Sie müssen für jeden IdP, den Sie unterstützen möchten, eine Anwendung in dessen System registrieren und eine Client-ID abrufen.

* [Einrichten von Facebook als IDP](active-directory-b2c-setup-fb-app.md)
* [Einrichten von Google als IDP](active-directory-b2c-setup-goog-app.md)
* [Einrichten von Amazon als IDP](active-directory-b2c-setup-amzn-app.md)
* [Einrichten von LinkedIn als IDP](active-directory-b2c-setup-li-app.md)

Nachdem Sie Ihrem B2C-Verzeichnis die Identitätsanbieter hinzugefügt haben, bearbeiten Sie jede der drei Richtlinien, um die neuen IDPs wie im [Artikel mit Richtlinienreferenzen](active-directory-b2c-reference-policies.md) beschrieben einzubeziehen. Nachdem Sie Ihre Richtlinien gespeichert haben, führen Sie die App erneut aus.  Die neuen IdPs sollten als Registrierungs- und Anmeldeoptionen in allen Benutzeroberflächen für Identitäten angezeigt werden.

Sie können mit Ihren Richtlinien experimentieren und die Auswirkungen auf die Beispiel-App beobachten. Fügen Sie IdPs hinzu, oder entfernen Sie sie, bearbeiten Sie Anwendungsansprüche, oder ändern Sie Registrierungsattribute. Experimentieren Sie so lange, bis Sie die Zusammenhänge zwischen Richtlinien, Authentifizierungsanforderungen und OWIN nachvollziehen können.

## <a name="sample-code-walkthrough"></a>Exemplarische Vorgehensweise mit Beispielcode
Die folgenden Abschnitte zeigen Ihnen, wie der Beispielcode für die Anwendung konfiguriert wird. Sie können dies in zukünftigen App-Entwicklungen als Leitfaden verwenden.

### <a name="add-authentication-support"></a>Hinzufügen von Authentifizierungssupport

Sie können nun Ihre App zur Verwendung von Azure AD B2C konfigurieren. Ihre App kommuniziert mit Azure AD B2C, indem OpenID Connect-Authentifizierungsanforderungen gesendet werden. Die Anforderungen bestimmen durch Angabe der Richtlinie über die Benutzeroberfläche, die Ihre App ausführen möchte. Sie können die OWIN-Bibliothek von Microsoft verwenden, um derartige Anfragen zu senden, Richtlinien auszuführen, Benutzersitzungen zu verwalten usw.

#### <a name="install-owin"></a>Installieren von OWIN

Fügen Sie dem Projekt zunächst über die Visual Studio-Paket-Manager-Konsole die NuGet-Pakete der OWIN-Middleware hinzu.

```Console
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

#### <a name="add-an-owin-startup-class"></a>Hinzufügen einer OWIN-Startklasse

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

#### <a name="configure-the-authentication-middleware"></a>Konfigurieren der Authentifizierungsmiddleware

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

### <a name="using-different-policies"></a>Verwenden von unterschiedlichen Richtlinien

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

### <a name="handling-authorization-codes"></a>Verarbeiten von Autorisierungscodes

Die `AuthorizationCodeReceived`-Benachrichtigung wird ausgelöst, wenn ein Autorisierungscode empfangen wird. Die OpenID Connect-Middleware unterstützt nicht den Austausch von Codes mit Zugriffstokens. Sie können den Code manuell mit dem Token in einer Rückruffunktion austauschen. Weitere Informationen zur Vorgehensweise finden Sie in der [Dokumentation](active-directory-b2c-devquickstarts-web-api-dotnet.md).

### <a name="handling-errors"></a>Behandeln von Fehlern

Die `AuthenticationFailed`-Benachrichtigung wird ausgelöst, wenn die Authentifizierung fehlschlägt. Bei der Rückrufmethode können Sie Fehler auf beliebige Weise behandeln. Sie sollten jedoch eine Überprüfung für den Fehlercode `AADB2C90118` hinzufügen. Während der Ausführung der Registrierungs- oder Anmelderichtlinie hat der Benutzer die Möglichkeit, den Link **Haben Sie Ihr Kennwort vergessen?** auszuwählen. In diesem Fall sendet Azure AD B2C Ihrer App den Fehlercode zu, der darauf hinweist, dass Ihre App stattdessen eine Anforderung mithilfe der Richtlinie zur Kennwortzurücksetzung tätigen sollte.

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

### <a name="send-authentication-requests-to-azure-ad"></a>Senden von Authentifizierungsanforderungen an Azure AD

Ihre App ist nun ordnungsgemäß für die Kommunikation mit Azure AD B2C über das Authentifizierungsprotokoll OpenID Connect konfiguriert. OWIN verwaltet die Details der Erstellung von Authentifizierungsnachrichten, der Überprüfung der Azure AD B2C-Token und der Verwaltung der Benutzersitzung. Jetzt müssen nur noch die einzelnen Benutzerabläufe initiiert werden.

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

Zusätzlich zum expliziten Aufrufen einer Richtlinie können Sie ein `[Authorize]`-Tag in Ihren Controllern verwenden, mit dem eine Richtlinie ausgeführt wird, wenn der Benutzer nicht angemeldet ist. Öffnen Sie `Controllers\HomeController.cs`, und fügen Sie dem Anspruchscontroller das `[Authorize]`-Tag hinzu.  OWIN wählt bei der Nutzung des `[Authorize]`-Tags die zuletzt konfigurierte Richtlinie aus.

```CSharp
// Controllers\HomeController.cs

// You can use the Authorize decorator to execute a certain policy if the user is not already signed into the app.
[Authorize]
public ActionResult Claims()
{
  ...
```

### <a name="display-user-information"></a>Anzeigen der Benutzerinformationen

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

